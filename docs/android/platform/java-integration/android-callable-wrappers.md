---
title: Xamarin android の呼び出し可能ラッパー
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: a8bd3f11698260b944bd29fcd9551825cb76e506
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511190"
---
# <a name="android-callable-wrappers-for-xamarinandroid"></a>Xamarin android の呼び出し可能ラッパー

Android ランタイムがマネージコードを呼び出すときは常に、android 呼び出し可能ラッパー (ACWs) が必要です。 これらのラッパーは、実行時にアート (Android ランタイム) にクラスを登録する方法がないために必要です。 (具体的には、 [JNI のていぎ eclass () 関数](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986)は、Android ランタイムではサポートされていません。 そのため、Android 呼び出し可能ラッパーは、ランタイム型の登録がサポートされないようにします。 

*毎回*Android コードは、マネージコード`virtual`で実装または実装`overridden`されているまたはインターフェイスメソッドを実行する必要があります。 Xamarin は、このメソッドが適切なマネージ型にディスパッチされるように Java プロキシを提供する必要があります。 これらの Java プロキシ型は、マネージ型として "同じ" 基底クラスと Java インターフェイスリストを持つ Java コードです。同じコンストラクターを実装し、オーバーライドされた基底クラスとインターフェイスメソッドを宣言します。 

Android 呼び出し可能ラッパーは、[ビルド処理](~/android/deploy-test/building-apps/build-process.md)中**に、(** 直接または間接的に) [Java](xref:Java.Lang.Object)を継承するすべての型に対して生成されます。 



## <a name="android-callable-wrapper-naming"></a>Android 呼び出し可能ラッパーの名前付け

Android 呼び出し可能ラッパーのパッケージ名は、エクスポートされる型のアセンブリ修飾名の MD5SUM に基づいています。 この名前付け方法を使用すると、パッケージ化エラーを発生させることなく、同じ完全修飾型名を異なるアセンブリから使用できるようになります。 

この MD5SUM 名前付けスキームにより、型に名前で直接アクセスすることはできません。 たとえば、次`adb`のコマンドは、既定では型名`my.ActivityType`が生成されないため、機能しません。 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

また、名前によって型を参照しようとすると、次のようなエラーが表示されることがあります。

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

名前で型にアクセス*する必要が*ある場合は、属性宣言でその型の名前を宣言できます。 たとえば、次に示すのは、完全修飾名`My.ActivityType`を持つアクティビティを宣言するコードです。

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

プロパティ`ActivityAttribute.Name`は、このアクティビティの名前を明示的に宣言するように設定できます。 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

このプロパティの設定が追加さ`my.ActivityType`れた後、には、外部コード`adb`およびスクリプトから名前を使用してアクセスできます。 属性`Name`は、、、 `BroadcastReceiver`、、および`Activity` `Application` `Service`など、さまざまな型に対して設定できます`ContentProvider`。 

-   [ActivityAttribute.Name](xref:Android.App.ActivityAttribute.Name)
-   [ApplicationAttribute.Name](xref:Android.App.ApplicationAttribute.Name)
-   [ServiceAttribute.Name](xref:Android.App.ServiceAttribute.Name)
-   [BroadcastReceiverAttribute.Name](xref:Android.Content.BroadcastReceiverAttribute.Name)
-   [ContentProviderAttribute.Name](xref:Android.Content.ContentProviderAttribute.Name)

MD5SUM ベースの ACW の名前付けは、Xamarin. Android 5.0 で導入されました。 属性の名前付けの詳細については、「 [Registerattribute](xref:Android.Runtime.RegisterAttribute)」を参照してください。 



## <a name="implementing-interfaces"></a>インターフェイスの実装

Android インターフェイス ( [IComponentCallbacks](xref:Android.Content.IComponentCallbacks)など) の実装が必要になる場合があります。 すべての Android のクラスとインターフェイスは[IJavaObject](xref:Android.Runtime.IJavaObject)インターフェイスを拡張するので、次のよう`IJavaObject`な疑問が生じます。 

上記の質問に回答しました。 android のすべての型`IJavaObject`を実装する必要がある理由は、android に提供する android 呼び出し可能ラッパー (つまり、特定の種類の Java プロキシ) を使用できるようにするためです。 **モノ**の`Java.Lang.Object` id はサブクラス`Java.Lang.Object`のみを検索するため、答え`IJavaObject,`を実装することは明らか`Java.Lang.Object`です。サブクラスは次のとおりです。 

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

*このページの残りの部分では、通知なしに変更される可能性のある実装の詳細について説明します*。(ここでは、開発者が何が起こっているかを知りたいので、ここでのみ説明します)。 

たとえば、次C#のソースがあるとします。

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

**Manid .exe**プログラムは、次の Android 呼び出し可能ラッパーを生成します。 

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

基本クラスが保持され、マネージコード`native`内でオーバーライドされるメソッドごとにメソッド宣言が提供されることに注意してください。 
