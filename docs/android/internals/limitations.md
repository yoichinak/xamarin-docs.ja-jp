---
title: Xamarin Android とデスクトップ-Mono ランタイムの違い
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 8fe0e3a9adedb161c527ccdf6d6c3a7cd06a1d86
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027838"
---
# <a name="limitations"></a>制限事項

Android のアプリケーションはビルドプロセス中に Java プロキシ型を生成する必要があるため、実行時にすべてのコードを生成することはできません。

デスクトップ Mono と比較した Xamarin Android の制限事項は次のとおりです。

## <a name="limited-dynamic-language-support"></a>制限付きの動的言語サポート

 Android ランタイムがマネージコードを呼び出す必要がある場合は常に、 [android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md)が必要です。 Android 呼び出し可能ラッパーは、IL の静的分析に基づいて、コンパイル時に生成されます。 この結果、次のような結果が得られます: 動的言語 (IronPython、IronRuby など) を使用する*ことはできません*。 Java 型のサブクラスが必要なシナリオ (間接サブクラス化を含む) では、コンパイル時にこれらの動的な型を抽出する方法がないためです。必要な Android 呼び出し可能ラッパーを生成します。

## <a name="limited-java-generation-support"></a>Java の生成サポートの制限

Java コードからマネージコードを呼び出すためには、 [Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md)を生成する必要があります。 *既定で*は、Android 呼び出し可能ラッパーは、( [`RegisterAttribute`](xref:Android.Runtime.RegisterAttribute)を持つ) 仮想 java メソッドをオーバーライドしたり、java インターフェイスメソッドを実装したりする (特定の) 宣言されたコンストラクターとメソッドだけを格納します (インターフェイスも同様に `Attribute`ます)。
  
4\.1 リリースより前のバージョンでは、追加のメソッドを宣言することはできませんでした。 4\.1 リリースでは、 [`Export` および `ExportField` カスタム属性を使用して、Android 呼び出し可能ラッパー内の Java メソッドとフィールドを宣言でき](~/android/platform/java-integration/working-with-jni.md)ます。

### <a name="missing-constructors"></a>見つからないコンストラクター

[`ExportAttribute`](xref:Java.Interop.ExportAttribute)を使用しない限り、コンストラクターは注意してください。 Android 呼び出し可能ラッパーコンストラクターを生成するためのアルゴリズムは、次の場合に Java コンストラクターが生成されることを示します。

1. すべてのパラメーターの型に対して Java マッピングが存在します
2. この基本クラスでは、Android 呼び出し可能ラッパーが対応する基底クラスコンストラクターを呼び出す*必要*があるため、このコンストラクターが必要 &ndash; 同じコンストラクターが宣言されています。既定の引数は使用できません (Java 内で使用する必要がある値を簡単に判断する方法はありません)。

たとえば、次のクラスを考えてみます。

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

これは完全に論理的に見えますが、*リリースビルドで*の Android 呼び出し可能ラッパーには既定のコンストラクターが含まれません。 そのため、このサービスを開始しようとすると (たとえば、 [`Context.StartService`](xref:Android.Content.Context.StartService*)しようとすると失敗します。

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

この回避策は、既定のコンストラクターを宣言し、`ExportAttribute`を使用してそれを装飾し、 [`ExportAttribute.SuperStringArgument`](xref:Java.Interop.ExportAttribute.SuperArgumentsString)を設定することです。 

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

ジェネリックC#クラスは部分的にのみサポートされています。 次の制限事項があります。

- ジェネリック型で `[Export]` または `[ExportField`] を使用することはできません。 これを行おうとすると、`XA4207` エラーが発生します。

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

- ジェネリックメソッドで `[Export]` または `[ExportField]`を使用することはできません。

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

- `[ExportField]` は、`void`を返すメソッドでは使用できません。

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

- ジェネリック型のインスタンスを Java コードから作成することはでき_ません_。
    これらのコードは、マネージコードからのみ安全に作成できます。

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

Java ジェネリックのバインディングサポートは制限されています。 特に、別のジェネリック (インスタンス化されていない) クラスから派生したジェネリックインスタンスクラスのメンバーは、Java. Lang. オブジェクトとして公開されたままになります。 たとえば、 [GetParcelableExtra](xref:Android.Content.Intent.GetParcelableExtra*)メソッドは、Java. Lang. オブジェクトを返します。 これは、Java ジェネリックが消去されたことが原因です。
この制限を適用しないクラスがいくつかありますが、手動で調整します。

## <a name="related-links"></a>関連リンク

- [Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md)
- [JNI の操作](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](xref:Java.Interop.ExportAttribute)
- [SuperString](xref:Java.Interop.ExportAttribute.SuperArgumentsString)
- [RegisterAttribute](xref:Android.Runtime.RegisterAttribute)
