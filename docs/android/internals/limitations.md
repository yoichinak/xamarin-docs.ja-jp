---
title: Xamarin.Android とします。デスクトップの Mono ランタイムの違い
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 115d715214d7af3174c41d9d82e894ce429dab42
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120906"
---
# <a name="limitations"></a>制限事項

Android でのアプリケーションでは、ビルド プロセス中に Java プロキシ型を生成する必要があるために、実行時にすべてのコードを生成することはできません。

デスクトップ Mono と比較して、Xamarin.Android の制限事項を次に示します。


## <a name="limited-dynamic-language-support"></a>制限付きの動的言語のサポート

 [Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) Android ランタイムがマネージ コードを呼び出す必要がある任意の時間が必要です。 Android 呼び出し可能ラッパーは、IL のスタティック分析に基づいて、コンパイル時に生成されます。 これの最終的な結果: する*できません*を使用して、動的言語 (IronPython、IronRuby など) のシナリオで Java 型のサブクラスを (間接的なサブクラス化) を含む必要があるようにこれらの動的な型を抽出する方法はありませんコンパイル時に必要な Android 呼び出し可能ラッパーを生成します。


## <a name="limited-java-generation-support"></a>生成の制限の Java サポート

[Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md) Java コードをマネージ コードを呼び出すために、生成する必要があります。 *既定で*、Android 呼び出し可能ラッパーには (一部) の宣言されたコンス トラクターと仮想の Java メソッドをオーバーライドするメソッドのみが (つまりが[ `RegisterAttribute` ](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)) または Java インターフェイスのメソッド (の実装インターフェイスには同様に`Attribute`)。
  
4.1 のリリースの前に追加のメソッドを宣言できるありません。 4.1 のリリースでは、 [、`Export`と`ExportField`Java のメソッドと、Android 呼び出し可能ラッパー内のフィールドを宣言するカスタム属性を使用できます。](~/android/platform/java-integration/working-with-jni.md)します。

### <a name="missing-constructors"></a>不足しているコンス トラクター

コンス トラクターのまま、厄介な場合を除き、 [ `ExportAttribute` ](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute)使用されます。 Android 呼び出し可能ラッパー コンス トラクターを生成するためのアルゴリズムでは、場合に、Java のコンス トラクターを出力することを示します。

1. すべてのパラメーターの型の Java マッピングがあります。
2. 基本クラスは、同じコンス トラクターを宣言します&ndash;があるため、Android 呼び出し可能ラッパー*する必要があります*(する簡単な方法が存在しないため、既定の引数を使用するありません対応する基本クラス コンス トラクターを呼び出すことは。値を決定 Java 内で使用)。

たとえば、次のクラスを考えてみます。

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

結果として得られる Android 呼び出し可能ラッパーこの検索を論理的に完ぺきを while*リリース ビルドで*は既定のコンス トラクターが含まれていません。 そのため、このサービスを開始しようとした場合 (例: [ `Context.StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/))、失敗になります。

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

既定のコンス トラクターを宣言し、それを装飾するを回避するには、 `ExportAttribute`、設定、 [ `ExportAttribute.SuperStringArgument` ](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/): 

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


### <a name="generic-c-classes"></a>ジェネリックC#クラス

ジェネリックC#クラスが部分的にしかサポートされています。 次の制限事項があります。


-   ジェネリック型を使用していない可能性があります`[Export]`または`[ExportField`]。 操作が生成されます、`XA4207`エラー。

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

-   ジェネリック メソッドを使用していない`[Export]`または`[ExportField]`:

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

-   `[ExportField]` 返すメソッドを使用することはできません`void`:

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

-   ジェネリック型のインスタンス_しないで_Java コードから作成します。
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

Java のバインドのジェネリックのサポートは制限されます。 (非インスタンス化の) 別のジェネリック クラスから派生した汎用のインスタンス クラスのメンバーのままにする、特に Java.Lang.Object として公開します。 たとえば、 [Android.Content.Intent.GetParcelableExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.GetParcelableExtra/p/System.String/) Java.Lang.Object を返します。 これは消去された Java ジェネリックのためにです。
このような制限が適用されない一部のクラスが手動で調整します。


## <a name="related-links"></a>関連リンク

- [Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md)
- [JNI の使用](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)
- [SuperString](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/)
- [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)
