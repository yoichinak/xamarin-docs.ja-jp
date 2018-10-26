---
title: Android 呼び出し可能ラッパー
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: 7edbdaa5a690a641523cb5baad7909ed01992aa5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121816"
---
# <a name="android-callable-wrappers"></a>Android 呼び出し可能ラッパー

Android ランタイムがマネージ コードを呼び出すたびに、android 呼び出し可能ラッパー (ACWs) が必要です。 アート (Android ランタイム) で実行時にクラスを登録する方法がないために、これらのラッパーが必要です。 (具体的には、 [JNI DefineClass() 関数](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986)Android ランタイムではサポートされていません}。 Android 呼び出し可能ラッパーは、そのため、ランタイム型の登録のサポートの不足を構成します。 

*毎回*Android コードを実行する必要があります、`virtual`インターフェイスは、メソッドまたは`overridden`またはマネージ コードで実装すると、Xamarin.Android する必要があります Java のプロキシを提供するため、このメソッドは、適切なマネージ型にディスパッチされます。 これらの Java プロキシ型は、同じコンス トラクターを実装して、オーバーライドされた基底クラスとインターフェイスのメソッドの宣言は、マネージ型として、「同じ」の基底クラスと Java インターフェイスの一覧を含む Java コードです。 

Android 呼び出し可能ラッパーがによって生成された、 **monodroid.exe**中のプログラム、[ビルド プロセス](~/android/deploy-test/building-apps/build-process.md): (直接または間接的に) を継承するすべての種類が生成される[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)します。 



## <a name="android-callable-wrapper-naming"></a>Android 呼び出し可能ラッパーの名前を付ける

Android 呼び出し可能ラッパーのパッケージ名は、エクスポートされる型のアセンブリ修飾名の md5 チェックサムに基づいています。 この名前付けの方法では、使用可能にする別のアセンブリをパッケージ化エラーを導入することがなく同じ完全修飾型名を使用します。 

この md5 チェックサムの名前付けスキームにより、型を名前で直接アクセスできません。 たとえば、次`adb`ため、コマンドが機能しません、型名`my.ActivityType`は既定では生成されません。 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

また、名前で型を参照しようとした場合は、次のようなエラーを参照してください可能性があります。

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

場合する*は*へのアクセスを必要と名前で、型属性の宣言では、その型の名前を宣言できます。 たとえば、完全修飾名を持つアクティビティを宣言するコードをここでは`My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

`ActivityAttribute.Name`このアクティビティの名前を明示的に宣言するプロパティを設定することができます。 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

このプロパティの設定が追加されると、`my.ActivityType`名と外部コードからアクセスできる`adb`スクリプト。 `Name`など、さまざまな種類の属性を設定できます`Activity`、 `Application`、 `Service`、 `BroadcastReceiver`、および`ContentProvider`: 

-   [ActivityAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/)
-   [ApplicationAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Name/)
-   [ServiceAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ServiceAttribute.Name/)
-   [BroadcastReceiverAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.BroadcastReceiverAttribute.Name/)
-   [ContentProviderAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.ContentProviderAttribute.Name/)

Md5 チェックサムに基づくについての名前付けは、Xamarin.Android 5.0 で導入されました。 属性の名前付けの詳細については、次を参照してください。 [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)します。 



## <a name="implementing-interfaces"></a>インターフェイスの実装

Android のインターフェイスを実装します。 必要がある場合があります[Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/)します。 すべての Android のクラスとインターフェイスを拡張するため、 [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/)インターフェイス、疑問が浮かびます。 の実装方法`IJavaObject`でしょうか。 

上に、質問の答えが: 理由を実装する必要があるすべての Android の種類`IJavaObject`は Xamarin.Android がある Android、つまり指定した型の Java プロキシに提供する Android 呼び出し可能ラッパーをできるようにします。 **Monodroid.exe**だけを検索`Java.Lang.Object`サブクラスと`Java.Lang.Object`実装`IJavaObject,`、答えは明らかな: サブクラス`Java.Lang.Object`: 

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

*このページの残りの部分は、予告なく変更される可能性の実装の詳細を提供します。* (および、ここでは何が起こってについて調べることがあるためのみ)。 

たとえば、次を指定C#ソース。

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

**Mandroid.exe**プログラムは、次の Android 呼び出し可能ラッパーを生成します。 

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

基底クラスが保持されることに注意してくださいと`native`各メソッドは、マネージ コード内でオーバーライドされるメソッドの宣言が提供されます。 
