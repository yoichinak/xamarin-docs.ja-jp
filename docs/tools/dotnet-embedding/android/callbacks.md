---
title: Android でのコールバック
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 2d1d5b8985d132e5a5839e3cd23aaec32fc3815a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725114"
---
# <a name="callbacks-on-android"></a>Android でのコールバック

からC# Java への呼び出しは、やや*危険なビジネス*です。 これは、からC# Java にコールバックするための*パターン*があるとします。ただし、これは必要以上に複雑です。

ここでは、Java に最適なコールバックを実行するための3つのオプションについて説明します。

- 抽象クラス
- インターフェイス
- 仮想メソッド

## <a name="abstract-classes"></a>抽象クラス

これはコールバックの最も簡単なルートであるため、最も単純な形式で動作するコールバックを取得しようとしている場合は、`abstract` を使用することをお勧めします。

Java を実装するC#クラスから始めましょう。

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

この作業を行うための詳細は次のとおりです。

- `[Register]` は Java で適切なパッケージ名を生成します。自動生成されたパッケージ名を取得します。
- .NET 埋め込みに `Java.Lang.Object` 信号をサブクラス化して、Xamarin Android の Java ジェネレーターを通じてクラスを実行します。
- 空のコンストラクター: Java コードから使用するものです。
- `(IntPtr, JniHandleOwnership)` コンストラクター: Java オブジェクトと同等のものを作成C#するために使用する Xamarin。
- `[Export]` は、Java にメソッドを公開するように Xamarin Android に通知します。 また、Java ワールドでは小文字のメソッドを使用することが気に入っているため、メソッド名を変更することもできます。

次に、シナリオをC#テストするメソッドを作成してみましょう。

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

`JavaCallbacks` は、`Java.Lang.Object`である限り、これをテストする任意のクラスにすることができます。

次に、.net アセンブリで .NET 埋め込みを実行して、AAR を生成します。 詳細については、[はじめにガイド](~/tools/dotnet-embedding/get-started/java/android.md)を参照してください。

AAR ファイルを Android Studio にインポートしたら、単体テストを記述しましょう。

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

では、次のようにします。

- 匿名型を使用して Java で `AbstractClass` を実装します。
- インスタンスが Java から `"Java"` を返すことを確認しました
- インスタンスがから `"Java"` を返すようにしましたC#
- コンストラクターが現在でC#マークされているため、`throws Throwable`が追加されました `throws`

この単体テストをそのように実行した場合、次のようなエラーが発生して失敗します。

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

ここには、`Invoker` の種類があります。 これは、Java への呼び出しをC#転送する `AbstractClass` のサブクラスです。 Java オブジェクトC#が世界に入り、それに対応C#する型が Abstract の場合、Xamarin Android は、コードC#内でC#使用するためにサフィックス `Invoker` を持つ型を自動的に検索します。

Xamarin Android では、このような `Invoker` パターンを Java バインドプロジェクトに使用します。

`AbstractClassInvoker`の実装は次のとおりです。

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

ここにはかなりのことがあります。

- サブクラスを `Invoker` サフィックスを持つクラスを追加しました `AbstractClass`
- C#クラスをサブクラス化する Java クラスへの JNI 参照を保持するために `class_ref` を追加しました
- Java `getText` メソッドへの JNI 参照を保持するために `id_gettext` を追加しました
- `(IntPtr, JniHandleOwnership)` コンストラクターを含む
- `ThresholdType` と `ThresholdClass` を Xamarin Android の要件として実装し、`Invoker` についての詳細を把握します。
- 適切な JNI signature を使用して Java `getText` メソッドを参照して呼び出す必要が `GetText`
- への参照をクリアするだけで `Dispose` が必要 `class_ref`

このクラスを追加して新しい AAR を生成すると、単体テストは成功します。 コールバックのこのパターンは*理想的*ではありませんが、取り上げです。

Java 相互運用機能の詳細については、このテーマに関するすばらしい[Xamarin. Android のドキュメント](~/android/platform/java-integration/working-with-jni.md)を参照してください。

## <a name="interfaces"></a>インターフェイス

インターフェイスは抽象クラスとほぼ同じですが、1つ詳しく説明します。 Xamarin では、Java が生成されません。 これは、.NET を埋め込む前に、Java がインターフェイスをC#実装するシナリオが多くないためです。

たとえば、次C#のようなインターフェイスがあるとします。

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```

`IJavaObject` は、これが Xamarin. Android インターフェイスであることを .NET に通知しますが、それ以外の場合は `abstract` クラスとまったく同じです。

現在、Xamarin Android ではこのインターフェイスの Java コードが生成されないため、次の Java C#をプロジェクトに追加します。

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```

ファイルは任意の場所に配置できますが、必ずビルドアクションを `AndroidJavaSource`に設定してください。 これにより、.NET 埋め込みが、AAR ファイルにコンパイルされるために適切なディレクトリにコピーされます。

次に、`Invoker` の実装はまったく同じになります。

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

AAR ファイルを生成した後、Android Studio で、次の渡す単体テストを記述できます。

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

Java で `virtual` をオーバーライドすることは可能ですが、優れたエクスペリエンスではありません。

次C#のクラスを使用しているとします。

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

上記の `abstract` クラスの例に従っている場合は、1つの詳細を除いて機能します。 _Xamarin は、`Invoker`を参照しません_。

この問題を解決するにC#は、`abstract`するようにクラスを変更します。

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

これは理想的ではありませんが、このシナリオは動作します。 Xamarin Android は `VirtualClassInvoker` を取得し、Java はメソッドで `@Override` を使用できます。

## <a name="callbacks-in-the-future"></a>コールバック (将来)

これらのシナリオを改善するには、いくつかの点があります。

1. この [PR](https://github.com/xamarin/java.interop/pull/170) C#では、コンストラクターの`throws Throwable` が修正されています。
1. Java ジェネレーターを Xamarin. Android サポートインターフェイスで作成します。
    - これにより、`AndroidJavaSource`のビルドアクションを含む Java ソースファイルを追加する必要がなくなります。
1. Xamarin Android で仮想クラスの `Invoker` を読み込む方法を作成します。
    - これにより、`virtual` の例 `abstract`でクラスをマークする必要がなくなります。
1. .NET 埋め込み用の `Invoker` クラスを自動的に生成する
    - これは複雑になりますが、取り上げです。 Xamarin Android では、Java バインドプロジェクトの場合と同様の処理が既に実行されています。

ここで実行する作業はたくさんありますが、.NET の埋め込みに対するこれらの機能強化が可能です。

## <a name="further-reading"></a>関連項目

- [Android でのはじめに](~/tools/dotnet-embedding/get-started/java/android.md)
- [Android の暫定版の研究](~/tools/dotnet-embedding/android/index.md)
- [.NET 埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
- [エラーコードと説明](~/tools/dotnet-embedding/errors.md)
