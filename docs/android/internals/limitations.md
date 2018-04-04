---
title: 制限事項
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 07353cfa3ce9bc20688b9a59c09a6e602f16dd60
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="limitations"></a>制限事項

Android でのアプリケーションでは、ビルド処理中に Java プロキシ型を生成する必要があるために、実行時にすべてのコードを生成することはできません。

デスクトップ モノラルと比較して Xamarin.Android 制限事項を次に示します。


## <a name="limited-dynamic-language-support"></a>制限付きの動的言語サポート

 [Android の呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) Android ランタイムがマネージ コードを呼び出すために必要な任意の時間が必要です。 Android の呼び出し可能ラッパーは、IL のスタティック分析に基づいて、コンパイル時に生成されます。 この最終的な結果: する*できません*言語を使用して動的 (IronPython、IronRuby など) どのような状況で Java 型のサブクラス化を (間接的なサブクラス化) を含む必要があるこれらの動的な型を抽出するための手段がないです。コンパイル時に必要な Android 呼び出し可能ラッパーを生成します。


## <a name="limited-java-generation-support"></a>限られた Java 生成のサポート

[Android の呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) Java コードをマネージ コードを呼び出すために、生成する必要があります。 *既定では*、(特定) 宣言されたコンス トラクターと Java の仮想メソッドをオーバーライドするメソッド、Android の呼び出し可能ラッパーにはのみが含まれます (つまりが[ `RegisterAttribute` ](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)) または Java インターフェイスのメソッド (の実装インターフェイスには同様に`Attribute`)。
  
4.1 のリリースでは、前にその他の方法でしたとして宣言します。 4.1 のリリースでは、 [、`Export`と`ExportField`Java と Android の呼び出し可能ラッパー内のフィールドを宣言するカスタム属性を使用できる](~/android/platform/java-integration/working-with-jni.md)です。

### <a name="missing-constructors"></a>必要なコンス トラクター

コンス トラクターは、わかりにくい場合を除き、 [ `ExportAttribute` ](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute)を使用します。 Android の呼び出し可能ラッパー コンス トラクターを生成するためのアルゴリズムでは、場合に、Java コンス トラクターを生成することを示します。

1. すべてのパラメーター型の Java マッピングがあります。
2. 基本クラスが同じコンス トラクターを宣言して&ndash;これが必要な Android 呼び出し可能ラッパー*必要があります*対応する基本クラス コンス トラクターを呼び出す以外の既定の引数は使用できません (ようにする簡単な方法はありません値を決定 Java 内で使用)。

たとえば、次のクラスを考えてみます。

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

一方、結果として得られる Android 呼び出し可能ラッパーこの可能性を完全に論理*リリース ビルドで*既定のコンス トラクターは含まれません。 そのため、このサービスを開始しようとする場合 (例: [ `Context.StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)) は失敗します。

```shell
E/AndroidRuntime(31766): FATAL EXCEPTION: main
E/AndroidRuntime(31766): java.lang.RuntimeException: Unable to instantiate service example.MyIntentService: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2347)
E/AndroidRuntime(31766):        at android.app.ActivityThread.access$1600(ActivityThread.java:130)
E/AndroidRuntime(31766):        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1277)
E/AndroidRuntime(31766):        at android.os.Handler.dispatchMessage(Handler.java:99)
E/AndroidRuntime(31766):        at android.os.Looper.loop(Looper.java:137)
E/AndroidRuntime(31766):        at android.app.ActivityThread.main(ActivityThread.java:4745)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invokeNative(Native Method)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invoke(Method.java:511)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:786)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:553)
E/AndroidRuntime(31766):        at dalvik.system.NativeStart.main(Native Method)
E/AndroidRuntime(31766): Caused by: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at java.lang.Class.newInstanceImpl(Native Method)
E/AndroidRuntime(31766):        at java.lang.Class.newInstance(Class.java:1319)
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2344)
E/AndroidRuntime(31766):        ... 10 more
```

回避策は、既定のコンス トラクターを宣言、装飾では、 `ExportAttribute`、設定と、 [ `ExportAttribute.SuperStringArgument` ](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/): 

```csharp
[Service]
class MyIntentService : IntentService {
    [Export (SuperArgumentsString = "\"value\"")]
    public MyIntentService (): base("value")
    {
    }

    // ...
}
```


### <a name="generic-c-classes"></a>ジェネリック クラス (C#)

一般的な c# クラスは部分的にサポートします。 次の制限事項します。


-   ジェネリック型を使用することはできません`[Export]`または`[ExportField`] です。 操作が生成されます、`XA4207`エラーです。

    ```csharp
    public abstract class Parcelable<T> : Java.Lang.Object, IParcelable
    {
        // Invalid; generates XA4207
        [ExportField ("CREATOR")]
        public static IParcelableCreator CreateCreator ()
        {
            ...
    }
    ```

-   ジェネリック メソッドを使用することはできません`[Export]`または`[ExportField]`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        
        // Invalid; generates XA4207
        [Export]
        public static void Method<T>(T value)
        {
            ...
        }
    }
    ```

-   `[ExportField]` 返すメソッドでは使用できません`void`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        // Invalid; generates XA4208
        [ExportField ("CREATOR")]
        public static void CreateSomething ()
        {
        }
    }
    ```

-   ジェネリック型のインスタンス_許容できない_の Java コードから作成します。
    また、マネージ コードから安全にのみ作成できます。

    ```csharp
    [Activity (Label="Die!", MainLauncher=true)]
    public class BadGenericActivity<T> : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);
        }
    }
    ```


## <a name="partial-java-generics-support"></a>部分的な Java ジェネリックのサポート

ジェネリックのバインドをサポートし、Java は、制限されています。 (非インスタンス化された) 別のジェネリック クラスから派生するクラスをジェネリックのインスタンス内のメンバーのままにする、特に Java.Lang.Object として公開します。 たとえば、 [Android.Content.Intent.GetParcelableExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.GetParcelableExtra/p/System.String/) Java.Lang.Object を返します。 これは、消去された Java ジェネリックのためです。
このような制限は適用されない一部のクラスが、手動で調整されます。


## <a name="related-links"></a>関連リンク

- [Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md)
- [JNI の操作](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)
- [SuperString](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/)
- [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)
