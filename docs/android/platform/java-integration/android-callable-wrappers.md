---
title: Android の呼び出し可能ラッパー
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: bb15c7f3a36cc7f1ed235d92e343bbae67a718b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764561"
---
# <a name="android-callable-wrappers"></a>Android の呼び出し可能ラッパー

Android のランタイムがマネージ コードを呼び出すたびに、android 呼び出し可能ラッパー (ACWs) する必要があります。 アート (Android のランタイム) で実行時にクラスを登録する方法がないために、これらのラッパーが必要です。 (具体的には、 [JNI DefineClass() 関数](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986)は Android のランタイムによってサポートされていません}。 したがって、android の呼び出し可能ラッパーはランタイム型の登録のサポートの不足を構成します。 

*たびに*Android のコードを実行する必要があります、`virtual`インターフェイスは、メソッドまたは`overridden`、マネージ コードに実装された Xamarin.Android 必要があります Java のプロキシを提供できるように、このメソッドは、適切なマネージ型にディスパッチまたはします。 これらの Java プロキシ型は、Java コードは、同じコンス トラクターを実装して、オーバーライドされた基底クラスとインターフェイスのメソッドの宣言は、マネージ型と「同じ」基本クラスおよび Java インターフェイスの一覧です。 

Android の呼び出し可能ラッパーがによって生成される、 **monodroid.exe**中のプログラム、[ビルド プロセス](~/android/deploy-test/building-apps/build-process.md): が生成されるすべての種類 (直接または間接的に) を継承する[Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)です。 



## <a name="android-callable-wrapper-naming"></a>Android の呼び出し可能ラッパーの名前を付ける

呼び出し可能ラッパーの Android 用のパッケージ名は、エクスポートされる型のアセンブリ修飾名の md5 チェックサムに基づきます。 この名前付けの方法により、同じ完全修飾型名を使用できる異なるアセンブリによって、パッケージのエラーを導入することがなくです。 

この MD5SUM 名前付けスキームのため、型を名前で直接アクセスできません。 たとえば、次`adb`ためのコマンドは動作しません型名`my.ActivityType`既定では生成されません。 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

また、名前で型を参照しようとする場合、次のようなエラーが発生する可能性があります。

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

場合する*しないで*アクセスを必要と名前で、型属性の宣言では、その型の名前を宣言することができます。 たとえば、完全修飾名を持つアクティビティを宣言するコードをここでは`My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

`ActivityAttribute.Name`このアクティビティの名前を明示的に宣言するプロパティを設定できます。 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

このプロパティの設定が追加されると、`my.ActivityType`名および外部コードからアクセスできる`adb`スクリプト。 `Name`など、さまざまな種類の属性を設定できます`Activity`、 `Application`、 `Service`、 `BroadcastReceiver`、および`ContentProvider`: 

-   [ActivityAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/)
-   [ApplicationAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Name/)
-   [ServiceAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ServiceAttribute.Name/)
-   [BroadcastReceiverAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.BroadcastReceiverAttribute.Name/)
-   [ContentProviderAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.ContentProviderAttribute.Name/)

MD5SUM ベースについての命名は、Xamarin.Android 5.0 で導入されました。 属性の名前付けに関する詳細については、次を参照してください。 [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)です。 



## <a name="implementing-interfaces"></a>インターフェイスの実装

など、Android のインターフェイスを実装する必要がある場合があります[Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/)です。 すべての Android のクラスおよびインターフェイスを拡張するため、 [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/)インターフェイス、問題が発生: お実装方法`IJavaObject`しますか? 

上の質問に回答されました: 理由を実装する必要があるすべての Android の種類`IJavaObject`Xamarin.Android がある Android、つまり Java のプロキシを指定された型を提供する Android の呼び出し可能ラッパーを持つように、します。 **Monodroid.exe**だけを検索`Java.Lang.Object`サブクラスと`Java.Lang.Object`実装`IJavaObject,`答えは明らかな: サブクラス`Java.Lang.Object`: 

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

*このページの残りの部分は、将来予告なしに変更される可能性が実装の詳細を提供*(およびここでは開発者に何が起こってについて調べるなるためにのみ)。 

たとえば、次の c# のソースを指定します。

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

基本クラスが維持されることに注意してくださいと`native`メソッド宣言はマネージ コード内でオーバーライドされるメソッドごとに提供されます。 
