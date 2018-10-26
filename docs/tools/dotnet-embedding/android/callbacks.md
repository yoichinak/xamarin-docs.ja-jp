---
title: Android コールバック
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: c6eaf4dd90b172053b4b87e3427cfe35213c6727
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117162"
---
# <a name="callbacks-on-android"></a>Android コールバック

Java から呼び出すC#がいくらか、*リスクの高いビジネス*します。 つまりがある、*パターン*からのコールバックのC#java; が、今回はよりも複雑です。

Java のほとんどの意味のあるコールバックを行うための 3 つのオプションについて説明します。

- 抽象クラス
- インターフェイス
- 仮想メソッド

## <a name="abstract-classes"></a>抽象クラス

使用をお勧めしますが、これは、コールバック関数の最も簡単なルート`abstract`場合、最も単純な形式での作業のコールバックを取得しようとするだけです。

まず、C#クラスを実装するために Java を今回は。

```csharp
[Register("mono.embeddinator.android.AbstractClass")]
public abstract class AbstractClass : Java.Lang.Object
{
    public AbstractClass() { }

    public AbstractClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public abstract string GetText();
}
```

この作業を行うに詳細を次に示します。

- `[Register]` 便利なパッケージ名が生成されます - Java では、なしで自動生成されたパッケージ名を取得します。
- サブクラス化`Java.Lang.Object`クラスを Xamarin.Android の Java のジェネレーターを実行する .NET の埋め込みに通知します。
- 空のコンス トラクター: Java コードから使用する、です。
- `(IntPtr, JniHandleOwnership)` コンス トラクター: は Xamarin.Android の使用を作成するためには、 C#-Java オブジェクトに相当します。
- `[Export]` java メソッドを公開する Xamarin.Android を通知します。 小文字メソッドを使用する Java の世界が好きなので、メソッド名を変更もできます。

[次へ] を行います、C#シナリオをテストするメソッド。

```csharp
[Register("mono.embeddinator.android.JavaCallbacks")]
public class JavaCallbacks : Java.Lang.Object
{
    [Export("abstractCallback")]
    public static string AbstractCallback(AbstractClass callback)
    {
        return callback.GetText();
    }
}
```
`JavaCallbacks` あれば、任意のクラスが、これをテストする可能性があります、`Java.Lang.Object`します。

ここで、AAR を生成する .NET アセンブリの .NET の埋め込みを実行します。 参照してください、[ファースト ステップ ガイド](~/tools/dotnet-embedding/get-started/java/android.md)詳細についてはします。

AAR ファイルを Android Studio にインポートした後は、単体テストを記述してみましょう。

```java
@Test
public void abstractCallback() throws Throwable {
    AbstractClass callback = new AbstractClass() {
        @Override
        public String getText() {
            return "Java";
        }
    };

    assertEquals("Java", callback.getText());
    assertEquals("Java", JavaCallbacks.abstractCallback(callback));
}
```
したがって。

- 実装されている、`AbstractClass`匿名型の java
- インスタンスを返すことを確認した`"Java"`Java から
- インスタンスを返すことを確認した`"Java"`からC#
- 追加`throws Throwable`、ためC#コンス トラクターとマークされています。 `throws`

としてこの単体テストを実行した場合に、エラーなど、失敗します。

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

不足しているここでは、`Invoker`型。 これは、サブクラス`AbstractClass`を転送するC#Java への呼び出し。 Java オブジェクトを入力した場合、C#世界と、対応C#型が抽象クラスで、Xamarin.Android を自動的に検索、C#サフィックスを持つ型`Invoker`内で使用するC#コード。

Xamarin.Android でこれを使用して`Invoker`特に Java バインド プロジェクトのパターン。

実装を次に示します`AbstractClassInvoker`:
```csharp
class AbstractClassInvoker : AbstractClass
{
    IntPtr class_ref, id_gettext;

    public AbstractClassInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(AbstractClassInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public override string GetText()
    {
        if (id_gettext == IntPtr.Zero)
            id_gettext = JNIEnv.GetMethodID(class_ref, "getText", "()Ljava/lang/String;");
        IntPtr lref = JNIEnv.CallObjectMethod(Handle, id_gettext);
        return GetObject<Java.Lang.String>(lref, JniHandleOwnership.TransferLocalRef)?.ToString();
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

ここでかなりしました。

- サフィックスを持つクラスを追加`Invoker`をサブクラス化します。 `AbstractClass`
- 追加`class_ref`をサブクラス化する Java クラスへの JNI の参照を保持するために、C#クラス
- 追加`id_gettext`、java JNI の参照を保持する`getText`メソッド
- 含まれる、`(IntPtr, JniHandleOwnership)`コンス トラクター
- 実装`ThresholdType`と`ThresholdClass`Xamarin.Android について詳細を把握するための要件として、 `Invoker`
- `GetText` Java を検索するために必要な`getText`適切な JNI のシグネチャを持つメソッド呼び出す
- `Dispose` 参照をクリアするために必要なだけです。 `class_ref`

このクラスを追加して新しい AAR を生成した後、単体テストには合格します。 このパターンのコールバックではありませんがわかるように*理想的な*が元に戻せるします。

Java の相互運用機能について詳しくは、参照、優れた[Xamarin.Android ドキュメント](~/android/platform/java-integration/working-with-jni.md)について。

## <a name="interfaces"></a>インターフェイス

インターフェイスは、1 つの詳細を除く、抽象クラスとほぼ同じ: Xamarin.Android には、それらの Java は生成されません。 これは、前に、.NET の埋め込みが含まれていない Java が実装は多くのシナリオのため、C#インターフェイス。

たとえば、次C#インターフェイス。

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```

`IJavaObject` これは、Xamarin.Android インターフェイスですが、それ以外の場合これはまったく同じとして、.NET の埋め込みに通知を`abstract`クラス。

Xamarin.Android はこのインターフェイスの Java コードが現在生成しないので、次の Java を追加、C#プロジェクト。

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```

ファイルをどこにでも配置できますが、そのビルド アクションに設定することを確認`AndroidJavaSource`します。 .NET の埋め込み、AAR ファイルにコンパイルされるを適切なディレクトリにコピーして、これは通知します。

次に、`Invoker`実装には、まったく同じになります。

```csharp
class IJavaCallbackInvoker : Java.Lang.Object, IJavaCallback
{
    IntPtr class_ref, id_send;

    public IJavaCallbackInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(IJavaCallbackInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public void Send(string text)
    {
        if (id_send == IntPtr.Zero)
            id_send = JNIEnv.GetMethodID(class_ref, "send", "(Ljava/lang/String;)V");
        JNIEnv.CallVoidMethod(Handle, id_send, new JValue(new Java.Lang.String(text)));
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

AAR ファイルを生成するには後で、次の引き渡し単体作成 Android Studio をテストします。

```java
class ConcreteCallback implements IJavaCallback {
    public String text;
    @Override
    public void send(String text) {
        this.text = text;
    }
}

@Test
public void interfaceCallback() {
    ConcreteCallback callback = new ConcreteCallback();
    JavaCallbacks.interfaceCallback(callback, "Java");
    assertEquals("Java", callback.text);
}
```

## <a name="virtual-methods"></a>仮想メソッド

オーバーライドを`virtual`Java では可能ですが、優れたエクスペリエンスではありません。

次のものと仮定C#クラス。

```csharp
[Register("mono.embeddinator.android.VirtualClass")]
public class VirtualClass : Java.Lang.Object
{
    public VirtualClass() { }

    public VirtualClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public virtual string GetText() { return "C#"; }
}
```

実行した場合、 `abstract` 1 つの詳細を除く上記クラスの例で使用: _Xamarin.Android を検索しません、 `Invoker`_ します。

これを解決するには、変更、C#クラスを作成できる`abstract`:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

これは理想的ではありませんが、このシナリオの動作を取得します。 Xamarin.Android を引き継ぎます、`VirtualClassInvoker`および Java を使用して`@Override`メソッドにします。

## <a name="callbacks-in-the-future"></a>将来のコールバック

いくつかは、これらのシナリオを向上させるためがモ ノの。

1. `throws Throwable` C#コンス トラクターはこれで固定されています。 [PR](https://github.com/xamarin/java.interop/pull/170)します。
1. インターフェイスをサポートする Xamarin.Android で Java ジェネレーターを確認します。
    - Java ソース ファイルのビルド アクションを追加する必要がなくなりましたこの`AndroidJavaSource`します。
1. Xamarin.Android を読み込むための手段を行う、`Invoker`仮想クラスの。
    - これは、削除内のクラスをマークする必要がある、`virtual`例`abstract`します。
1. 生成`Invoker`自動的に .NET を埋め込むためクラス
    - これが非常に複雑で、元に戻せるをします。 Xamarin.Android は、バインド プロジェクトの Java のようなものは既に行っています。

ここでは、実行する作業の多くがあるが、.NET 向けの埋め込みにこれらの機能強化が可能です。

## <a name="further-reading"></a>関連項目

* [Android の概要](~/tools/dotnet-embedding/get-started/java/android.md)
* [Android の予備調査](~/tools/dotnet-embedding/android/index.md)
* [.NET の埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献します。](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)
