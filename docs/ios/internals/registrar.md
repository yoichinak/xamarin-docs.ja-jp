---
title: Xamarin.iOS の種類のレジストラー
description: このドキュメントには、これにより、Xamarin.iOS 型レジストラーがについて説明しますC#OBJECTIVE-C ランタイムに使用できるクラス。
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/29/2018
ms.openlocfilehash: 83340ce2d5db145c29166d90d3a5180b1767d7ca
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036400"
---
# <a name="type-registrar-for-xamarinios"></a>Xamarin.iOS の種類のレジストラー

このドキュメントでは、Xamarin.iOS で使用される型の登録システムについて説明します。

## <a name="registration-of-managed-classes-and-methods"></a>マネージ クラスとメソッドの登録

起動時に、Xamarin.iOS は以下を登録します。

- [[Register]](xref:Foundation.RegisterAttribute)属性を持つクラスを、Objective-C のクラスとして登録します。
- [[Category]](xref:ObjCRuntime.CategoryAttribute)属性を持つクラスを、Objective-C のカテゴリとして登録します。
- [[Protocol]](xref:Foundation.ProtocolAttribute)属性を持つインターフェースを、Objective-C のプロトコルとして登録します。
- [[Export]](xref:Foundation.ExportAttribute)属性を持つメンバーを、Objective-C がそれらにアクセスできるようにします。

たとえば、マネージ`Main`Xamarin.iOS アプリケーションで一般的なメソッド。

```csharp
UIApplication.Main (args, null, "AppDelegate");
```

このコードと呼ばれる型を使用する OBJECTIVE-C ランタイムに指示`AppDelegate`として、アプリケーションのデリゲート クラス。 Objective C ランタイムのインスタンスを作成できるように、 C# `AppDelegate`クラス、クラスを登録する必要があります。

Xamarin.iOS は、(動的な登録) の実行時またはコンパイル時 (静的登録) に自動的に登録を実行します。

動的登録リフレクションを使用して起動時にすべてのクラスとメソッドを登録するには、検索、OBJECTIVE-C ランタイムに渡すことです。 動的登録は、シミュレーターのビルドの場合、既定で使用されます。

静的登録には、アプリケーションによって使用されるアセンブリがコンパイル時に検査します。 クラスおよび OBJECTIVE-C を登録する方法を決定しは、バイナリに埋め込まれているマップを生成します。
次に、起動時に、登録、マップ、OBJECTIVE-C ランタイムでします。 静的登録は、デバイスのビルドに使用されます。

### <a name="categories"></a>カテゴリ

以降で Xamarin.iOS 8.10 を使用して、OBJECTIVE-C でカテゴリを作成することC#構文。

カテゴリを作成するには、使用、`[Category]`属性し、拡張する型を指定します。 たとえば、次のコードを拡張`NSString`:

```csharp
[Category (typeof (NSString))]
```

各カテゴリのメソッドは、`[Export]`属性、OBJECTIVE-C ランタイムで使用できるようにします。

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

すべてのマネージ拡張メソッドは静的である必要がありますが、標準を使用して、OBJECTIVE-C でインスタンス メソッドを作成することはC#拡張メソッドの構文。

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

拡張メソッドの最初の引数は、メソッドが呼び出されたインスタンスを示します。

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
 }
 ```

この例は、ネイティブの追加`toUpper`インスタンス メソッドを`NSString`クラス。 C: の目的からこのメソッドを呼び出すことができます。

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAutoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

### <a name="protocols"></a>プロトコル

Xamarin.iOS 8.10 から始めて、インターフェイス、`[Protocol]`属性は、プロトコルとして OBJECTIVE-C にエクスポートされます。

```csharp
[Protocol ("MyProtocol")]
interface IMyProtocol
{
    [Export ("method")]
    void Method ();
}

class MyClass : IMyProtocol
{
    void Method ()
    {
    }
}
```

このコードがエクスポート`IMyProtocol`という名前のプロトコルとしては、OBJECTIVE-C に`MyProtocol`という名前のクラスと`MyClass`プロトコルを実装します。

## <a name="new-registration-system"></a>新しい登録システム

安定した 6.2.6 以降のバージョンとベータ 6.3.4 バージョンでは、新しい静的レジストラーが追加されました。 7\.2.1 バージョンについては、行った新しいレジストラー既定値。

この新しい登録システムでは、次の新機能を提供します。

- コンパイル時にプログラマ エラーの検出:
    - 同じ名前で登録されている 2 つのクラス。
    - 1 つ以上のメソッドは、同じセレクターに対応するエクスポート
- 未使用のネイティブ コードの削除:
    - 新しい登録システムでは、結果のバイナリから未使用のネイティブ コードを除去するネイティブ リンカーの動作を許可するスタティック ライブラリで使用するコードへの強参照を追加します。 Xamarin のサンプルのバインディングのほとんどのアプリケーションは小さい少なくとも 300 k になります。

- サポートの汎用サブクラス`NSObject`; を参照してください[NSObject ジェネリック](~/ios/internals/api-design/nsobject-generics.md)詳細についてはします。 さらに、新しい登録システムでは、可能性があった以前実行時にランダムな動作なジェネリック コンストラクトがサポートされていないをキャッチします。

### <a name="errors-caught-by-the-new-registrar"></a>新しいレジストラーによってキャッチ エラー

次に、新しいレジストラーによってキャッチ、エラーの例をいくつか示します。

- 同じクラスで複数回、同じセレクターをエクスポートします。

    ```csharp
    [Register]
    class MyDemo : NSObject
    {
        [Export ("foo:")]
        void Foo (NSString str);
        [Export ("foo:")]
        void Foo (string str)
    }
    ```

- OBJECTIVE-C で同じ名前の 1 つ以上のマネージ クラスをエクスポートするには。

    ```csharp
    [Register ("Class")]
    class MyClass : NSObject {}

    [Register ("Class")]
    class YourClass : NSObject {}
    ```

- ジェネリック メソッドをエクスポートするには。

    ```csharp
    [Register]
    class MyDemo : NSObject
    {
        [Export ("foo")]
        void Foo<T> () {}
    }
    ```

### <a name="limitations-of-the-new-registrar"></a>新しい登録者の制限事項

新しい登録者に関する留意点:

- 新しい登録システムを使用するいくつかのサード パーティ製ライブラリを更新する必要があります。 参照してください[変更に必要な](#required-modifications)以下の詳細。

- 短期的な欠点は、アカウントのフレームワークを使用する場合に Clang を使用する必要がありますも (これは Apple の**accounts.h**ヘッダーは Clang でのみコンパイルできます)。 追加`--compiler:clang`Xcode 4.6 以降を使用している場合は、Clang を使用するその他の mtouch 引数に (Xamarin.iOS は、Clang Xcode 5.0 以降を選択が自動的に)。

- 場合、Xcode 4.6 (またはそれ以前) は、使用、GCC、g++ 型をエクスポートする場合は選択する必要があります/名 (これは Xcode 4.6 出荷 Clang のバージョンでは、識別子内の非 ASCII 文字 Objective C コードでサポートされていません)、非 ASCII 文字を含めます。 追加`--compiler:gcc`GCC を使用するその他の mtouch 引数にします。

## <a name="selecting-a-registrar"></a>レジストラーの選択

異なるレジストラーを選択するには、次のオプションのいずれかのプロジェクトの追加 mtouch 引数に追加することで**iOS ビルド**設定。

- `--registrar:static` – デバイスのビルドの既定値
- `--registrar:dynamic` – シミュレーターのビルドの既定値

> [!NOTE]
> サポートされているその他のオプションをなど、Xamarin のクラシック API`--registrar:legacystatic`と`--registrar:legacydynamic`します。 ただし、これらのオプションは、Unified API でサポートされていません。

## <a name="shortcomings-in-the-old-registration-system"></a>古い登録システムの欠点

古い登録システムでは、次の欠点があります。

- OBJECTIVE-C のクラスおよびメソッド (ため、すべてが削除されます) が実際に使用されるサード パーティのネイティブ コードを削除するには、ネイティブ リンカーの動作を願いできなかったもので、サードパーティ ネイティブ ライブラリにする (ネイティブ) の静的参照がありませんでした。 これは、理由、`-force_load libNative.a`行う必要があるすべてのサード パーティのバインドを (または同等の`ForceLoad=true`で、`[LinkWith]`属性)。
- 警告なしで同じ Objective C の名前を持つ 2 つのマネージ型をエクスポートすることができます。 まれなシナリオが 2 つを終了するが`AppDelegate`異なる名前空間のクラス。 実行時に完全にランダムな (非常に複雑ですしにくいデバッグ エクスペリエンスの作成されませんでした - も再構築するアプリの実行の間さまざまなファクト、) で取得されたもの。
- OBJECTIVE-C で同じシグネチャを持つ 2 つのメソッドをエクスポートする可能性があります。 さらにもう一度ランダムが OBJECTIVE-C からどれが呼び出されます (ただし実際にこのバグが発生する唯一の方法が不吉マネージ メソッドをオーバーライドするためにほとんどの場合は、前と一般的でこの問題がありませんでした)。
- エクスポートされたメソッドのセットが、動的および静的なビルドの間で若干異なります。
- 正しく機能しませんジェネリック クラスをエクスポートするときに (実行時に実行される正確などの一般的な実装がランダムになります、効果的に未定義の動作になります)。

<a name="required-modifications" />

## <a name="new-registrar-required-changes-to-bindings"></a>新しいレジストラー: バインドへの変更が必要

このセクションでは、新しいレジストラーを使用するために行う必要があるバインディングの変更について説明します。

### <a name="protocols-must-have-the-protocol-attribute"></a>プロトコルは、[プロトコル] 属性を持つ必要があります。

プロトコルが必要になります、`[Protocol]`属性。 これを行わない場合、ネイティブ リンカー エラーがなどは。

```console
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>セレクターは、パラメーターの有効な数値をいる必要があります。

すべてのセレクターでは、正しくパラメーターの数を指定する必要があります。 以前は、これらのエラーが無視された、実行時の問題が発生する可能性があります。

簡単に言えば、コロンの数は、パラメーターの数と一致する必要があります。

- パラメーターのない: `foo`
- 1 つのパラメーター: `foo:`
- 2 つのパラメーター: `foo:parameterName2:`

次に、不適切な使用方法を示します。

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>エクスポートで IsVariadic パラメーターを使用します。

可変個引数関数を使用する必要があります、`IsVariadic`への引数、`[Export]`属性。

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>既存のシンボルにリンクする必要があります。

ネイティブ ライブラリに存在しないクラスをバインドすることはできません。
クラスがから削除またはネイティブ ライブラリの名前が変更された場合は、一致するようにバインドを更新することを確認してください。
