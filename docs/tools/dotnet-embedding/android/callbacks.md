---
title: Android でのコールバック
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 3f18516643c00dc67fe533ecab00e1f415eb5c46
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
ms.locfileid: "33922823"
---
# <a name="callbacks-on-android"></a>Android でのコールバック

Java を c# からの呼び出しは、*危険性の高いビジネス*です。 つまりがある、*パターン*コールバックの c# から Java; が、ようよりも複雑です。

Java のほとんど意味のあるコールバックを行うための 3 つのオプションを説明します。

- 抽象クラス
- インターフェイス
- 仮想メソッド

## <a name="abstract-classes"></a>抽象クラス

これは最も簡単なルート、コールバックの使用をお勧め`abstract`場合は、コールバックが最も簡単なフォームで作業を取得しようとするだけです。

Java を実装するよう、c# クラスから始めましょう。

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

この作業を作成するための詳細を次に示します。

- `[Register]` nice パッケージ名が生成される Java--で指定されていない場合、パッケージの自動生成された名前が表示されます。
- サブクラス化`Java.Lang.Object`Xamarin.Android の Java ジェネレーターで、クラスを実行する .NET の埋め込みを通知します。
- 空のコンス トラクター: は、Java コードから使用する場合は。
- `(IntPtr, JniHandleOwnership)` コンス トラクター: Xamarin.Android が c# の作成に使用されますが、Java オブジェクトの等価です。
- `[Export]` Java にメソッドを公開する Xamarin.Android を通知します。 小文字メソッドを使用して、Java 世界が好きなので、メソッド名を変更もできます。

次のシナリオをテストする c# のメソッドを加えてみましょう。

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
`JavaCallbacks` ある限り、任意のクラス、これをテストする可能性があります、`Java.Lang.Object`です。

ここで、[aar] を生成する .NET アセンブリで .NET の埋め込みを実行します。 参照してください、[ファースト ステップ ガイド](~/tools/dotnet-embedding/get-started/java/android.md)詳細についてはします。

Android Studio に AAR ファイルをインポートした後に、単体テストを記述してみましょう。

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
これは。

- 実装されている、`AbstractClass`匿名型を Java で
- インスタンスが返されますことを確認した`"Java"`Java から
- インスタンスが返されますことを確認した`"Java"`c# から
- 追加`throws Throwable`c# コンス トラクターとマークされているため、 `throws`

としてこの単体テストを実行しました-などがエラーで失敗します。

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

何が足りないここでは、`Invoker`型です。 これは、サブクラス`AbstractClass`c# Java への呼び出しを転送します。 かどうか Java オブジェクトを入力 c# world と等価の c# 型は abstract、し、Xamarin.Android は c# のサフィックスを持つ型を自動的に検索`Invoker`c# コード内で使用します。

Xamarin.Android がこれを使用して`Invoker`Java などの他のプロジェクトのバインド パターン。

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

ここでは、起こって相当量があるお。

- サフィックスを持つクラスを追加`Invoker`をサブクラス化します。 `AbstractClass`
- 追加`class_ref`保持するために、JNI への参照、Java クラスをサブクラスとして、c# クラス
- 追加`id_gettext`、java JNI 参照を保持する`getText`メソッド
- 含まれている、`(IntPtr, JniHandleOwnership)`コンス トラクター
- 実装`ThresholdType`と`ThresholdClass`Xamarin.Android に関する詳細を把握するための要件として、 `Invoker`
- `GetText` Java を照合するために必要な`getText`適切な JNI シグネチャを持つメソッドおよびの呼び出し
- `Dispose` 参照をクリアするために必要なだけ `class_ref`

このクラスを追加して新しい AAR を生成した後は、当社の単位は、パスをテストします。 コールバックのパターンではありませんがわかるように*理想的な*が元に戻せるです。

Java の相互運用機能の詳細については、「、優れた[Xamarin.Android ドキュメント](~/android/platform/java-integration/working-with-jni.md)このサブジェクトにします。

## <a name="interfaces"></a>インターフェイス

インターフェイスは、1 つの詳細を除く、抽象クラスとほぼ同じ: Xamarin.Android には、それらの Java は生成されません。 .NET の埋め込みをする前にあるシナリオは多くありません Java が c# のインターフェイスを実装はためにです。

次の c# のインターフェイスがあるとします。

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```

`IJavaObject` これは、Xamarin.Android インターフェイスが、それ以外の場合これはまったく同じに .NET の埋め込みに通知する`abstract`クラスです。

Xamarin.Android はこのインターフェイスの Java コードが現在生成しないので、次の Java を c# プロジェクトに追加します。

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```

ファイルをどこにでも配置できますが、そのビルド アクションに設定することを確認`AndroidJavaSource`です。 これは、.NET が AAR ファイルにコンパイルされる適切なディレクトリにコピーする埋め込みを通知します。

次に、`Invoker`実装はまったく同じになります。

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

AAR ファイルを生成した後次の引き渡し単位を記述することも Android Studio でをテストします。

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

オーバーライドする、 `virtual` Java では可能ですが優れたエクスペリエンスではありません。

次の c# クラスがあると仮定します。

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

実行した場合、`abstract`クラスの例は上、それが動作を除く 1 つの詳細: _Xamarin.Android を検索しません、 `Invoker`_ です。

これを解決するには、変更を行うには、c# クラス`abstract`:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

これは理想的ではありませんが、このシナリオの作業を取得します。 Xamarin.Android にピックアップされます、`VirtualClassInvoker`および Java を使用して`@Override`メソッドにします。

## <a name="callbacks-in-the-future"></a>将来的にコールバック

2 つ物は、これらのシナリオを向上させるためにできます。

1. `throws Throwable` このコンス トラクターは固定で c# [PR](https://github.com/xamarin/java.interop/pull/170)です。
1. インターフェイスをサポートする Xamarin.Android で Java ジェネレーターを確認します。
    - Java ソース ファイルのビルド アクションを追加する必要がなくなりますこの`AndroidJavaSource`です。
1. Xamarin.Android を読み込むための手段を行う、`Invoker`仮想クラスです。
    - これにより、削除、内のクラスをマークする必要が、`virtual`例`abstract`です。
1. 生成`Invoker`.NET を埋め込むために自動的にクラス
    - これは、複雑が元に戻せるすることになります。 Xamarin.Android は、Java プロジェクトのバインドは、次のようなものは、既に行っています。

ここでは、実行する作業の多くがあるが、.NET の埋め込みにこれらの機能強化が可能です。

## <a name="further-reading"></a>関連項目

* [Android で作業の開始](~/tools/dotnet-embedding/get-started/java/android.md)
* [Android の事前調査](~/tools/dotnet-embedding/android/index.md)
* [.NET の埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献しています。](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)
