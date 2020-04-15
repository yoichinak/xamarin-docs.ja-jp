---
title: Xamarin.Android の Android 呼び出し可能ラッパー
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 7278fd624bb3147c2e1a1a1a79adde68813a9888
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020154"
---
# <a name="android-callable-wrappers-for-xamarinandroid"></a>Xamarin.Android の Android 呼び出し可能ラッパー

Android ランタイムでマネージド コードを呼び出すときは、常に Android 呼び出し可能ラッパー (ACW) が必要です。 これらのラッパーが必要なのは、実行時に ART (Android ランタイム) にクラスを登録する方法がないためです (具体的には、Android ランタイムでは [JNI DefineClass() 関数](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986)はサポートされていません)。 そのため、Android 呼び出し可能ラッパーによって、ランタイム型登録のサポートの欠如が補われます。 

マネージド コードで `overridden` または実装される `virtual` メソッドまたは interface メソッドを Android コードで使用する必要があるときは、Xamarin.Android で Java プロキシを "*毎回*" 提供して、このメソッドが適切なマネージド型にディスパッチされるようにする必要があります。 これらの Java プロキシ型は、マネージド型と "同じ" 基底クラスと Java インターフェイス リストを持つ Java コードであり、同じコンストラクターを実装し、オーバーライドされた基底クラスとインターフェイス メソッドを宣言します。 

Android 呼び出し可能ラッパーは、[ビルド プロセス](~/android/deploy-test/building-apps/build-process.md)の間に **monodroid.exe** プログラムによって生成され、(直接的または間接的に) [Java.Lang.Object](xref:Java.Lang.Object) を継承するすべての型に対して生成されます。 

## <a name="android-callable-wrapper-naming"></a>Android 呼び出し可能ラッパーの名前付け規則

Android 呼び出し可能ラッパーのパッケージ名は、エクスポートされる型のアセンブリ修飾名の MD5SUM に基づきます。 この名前付け方法により、パッケージ化エラーを発生させることなく、異なるアセンブリで同じ完全修飾名の使用が可能になります。 

この MD5SUM 名前付けスキームのため、お使いの型に名前で直接アクセスすることはできません。 たとえば、次の `adb` コマンドでは型名 `my.ActivityType` が既定で生成されないため、このコマンドは機能しません。 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

また、型を名前で参照しようとすると、次のようなエラーが表示される可能性があります。

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

型に名前でアクセスする必要が "*ある*" 場合は、その型の名前を属性宣言内に宣言できます。 たとえば、次に示すのは、完全修飾名が `My.ActivityType` であるアクティビティを宣言するコードです。

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

このアクティビティの名前を明示的に宣言するために、`ActivityAttribute.Name` プロパティを設定できます。 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

このプロパティの設定を追加すると、外部コードと `adb` スクリプトから名前を使用して `my.ActivityType` にアクセスできます。 `Activity`、`Application`、`Service`、`BroadcastReceiver`、`ContentProvider` を含むさまざまな型に対して `Name` 属性を設定できます。 

- [ActivityAttribute.Name](xref:Android.App.ActivityAttribute.Name)
- [ApplicationAttribute.Name](xref:Android.App.ApplicationAttribute.Name)
- [ServiceAttribute.Name](xref:Android.App.ServiceAttribute.Name)
- [BroadcastReceiverAttribute.Name](xref:Android.Content.BroadcastReceiverAttribute.Name)
- [ContentProviderAttribute.Name](xref:Android.Content.ContentProviderAttribute.Name)

MD5SUM ベースの ACW の名前付け規則は、Xamarin. Android 5.0 で導入されました。 属性の名前付け規則の詳細については、「[RegisterAttribute](xref:Android.Runtime.RegisterAttribute)」を参照してください。 

## <a name="implementing-interfaces"></a>インターフェイスの実装

Android インターフェイス ([Android.Content.IComponentCallbacks](xref:Android.Content.IComponentCallbacks) など) の実装が必要になる場合があります。 Android のすべてのクラスとインターフェイスでは、[Android.Runtime.IJavaObject](xref:Android.Runtime.IJavaObject) インターフェイスが拡張されるため、`IJavaObject` をどのように実装するかという疑問が生じます。 

この疑問には上記で答えています。つまり、Android のすべての型で `IJavaObject` を実装する必要がある理由は、Android に提供される Android 呼び出し可能ラッパー (つまり、特定の種類の Java プロキシ) を Xamarin.Android に提供できるようにすることです。 **monodroid.exe** では `Java.Lang.Object` サブクラスだけが検索され、`Java.Lang.Object` によって `IJavaObject,` が実装されるため、答えは明らかにサブクラス `Java.Lang.Object` になります。 

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {

    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
    {
        // implementation goes here...
    } 

    public void OnLowMemory ()
    {
        // implementation goes here...
    }
}
```

## <a name="implementation-details"></a>実装の詳細

"*この記事の残りの部分で説明する実装の詳細は、予告なしに変更される可能性があります*" (開発者が内部の処理に関心があるという理由でのみ、ここに記載されています)。 

たとえば、次のような C# ソースについて考えます。

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

**mandroid.exe** プログラムでは、次のような Android 呼び出し可能ラッパーが生成されます。 

```java
package mono.samples.helloWorld;

public class HelloAndroid
    extends android.app.Activity
{
    static final String __md_methods;
    static {
        __md_methods = "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" + "";
        mono.android.Runtime.register (
            "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
            Culture=neutral, PublicKeyToken=null", HelloAndroid.class, __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
                Culture=neutral, PublicKeyToken=null", "", this, new java.lang.Object[] {  });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

基底クラスが保持されており、マネージド コード内でオーバーライドされるメソッドごとに `native` メソッド宣言が提供されていることに注意してください。 
