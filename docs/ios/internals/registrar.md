---
title: Xamarin の型レジストラー。 iOS
description: このドキュメントでは、Xamarin. iOS 型レジストラーについC#て説明します。これにより、目的の C ランタイムでクラスを使用できるようになります。
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 0d8e16c2a651df293b13e7f7586d5a643caa1c9c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291842"
---
# <a name="type-registrar-for-xamarinios"></a>Xamarin の型レジストラー。 iOS

このドキュメントでは、Xamarin. iOS によって使用されるタイプ登録システムについて説明します。

## <a name="registration-of-managed-classes-and-methods"></a>マネージクラスとマネージメソッドの登録

起動時に、Xamarin.iOS は以下を登録します。

- [[Register]](xref:Foundation.RegisterAttribute)属性を持つクラスを、Objective-C のクラスとして登録します。
- [[Category]](xref:ObjCRuntime.CategoryAttribute)属性を持つクラスを、Objective-C のカテゴリとして登録します。
- [[Protocol]](xref:Foundation.ProtocolAttribute)属性を持つインターフェースを、Objective-C のプロトコルとして登録します。
- [[Export]](xref:Foundation.ExportAttribute)属性を持つメンバーを、Objective-C がそれらにアクセスできるようにします。

たとえば、Xamarin.iOS アプリケーションで一般的なマネージ`Main`メソッドを考えてみます。

```csharp
UIApplication.Main (args, null, "AppDelegate");
```

このコードは Objective-C ランタイムに`AppDelegate`と呼ばれる型をアプリケーションのデリゲート クラスとして使用するように指示します。 Objective-C ランタイムが C# `AppDelegate`クラスのインスタンスを作成できるようにするためには、そのクラスが登録されている必要があります。

Xamarin.iOS は実行時 (動的な登録) またはコンパイル時 (静的登録) に自動的に登録を実行します。

動的登録ではリフレクションを使用して、起動時に登録するすべてのクラスとメソッドを検索し、それらを Objective-C ランタイムに渡します。 既定では、シミュレータービルドに動的登録が使用されます。

静的登録では、アプリケーションによって使用されるアセンブリをコンパイル時に検査します。 Objective-C に登録するクラスおよびメソッドを決定し、マップを生成します。マップはバイナリに埋め込まれます。
次に、起動時に、Objective-C ランタイムにマップを登録します。 静的登録は、デバイスのビルドで使用されます。

### <a name="categories"></a>カテゴリ

Xamarin. iOS 8.10 以降では、構文を使用してC# 、目的の C のカテゴリを作成することができます。

カテゴリを作成するには、 `[Category]`属性を使用して、拡張する型を指定します。 たとえば、次のコードはを`NSString`拡張します。

```csharp
[Category (typeof (NSString))]
```

各カテゴリのメソッドには`[Export]`属性があるため、目的 C ランタイムで使用できるようになります。

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

すべてのマネージ拡張メソッドは静的である必要がありますが、拡張メソッドの標準C#構文を使用して、目的の C インスタンスメソッドを作成することができます。

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

拡張メソッドの最初の引数は、メソッドが呼び出されたインスタンスです。

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

この例では、 `toUpper` `NSString`ネイティブインスタンスメソッドをクラスに追加します。 このメソッドは、次のように、目的の C から呼び出すことができます。

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

Xamarin. iOS 8.10 以降では、 `[Protocol]`属性を持つインターフェイスが、プロトコルとして目的の C にエクスポートされます。

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

このコードは`IMyProtocol` 、という`MyProtocol`プロトコルとして、プロトコルを実装する`MyClass`という名前のクラスを目的の C にエクスポートします。

## <a name="new-registration-system"></a>新しい登録システム

安定した6.2.6 バージョンとベータ版6.3.4 バージョンから、新しい静的レジストラーが追加されました。 7\.2.1 バージョンでは、新しいレジストラーが既定値として作成されました。

この新しい登録システムには、次の新機能が用意されています。

- コンパイル時のプログラマエラーの検出:
  - 2つのクラスが同じ名前で登録されています。
  - 同じセレクターに応答するために複数のメソッドがエクスポートされています
- 未使用のネイティブコードの削除:
  - 新しい登録システムは、スタティックライブラリで使用されるコードへの強い参照を追加します。これにより、ネイティブリンカーは、結果として得られるバイナリから未使用のネイティブコードを取り除くことができます。 Xamarin のサンプルバインドでは、ほとんどのアプリケーションが少なくとも300k より小さくなります。

- の`NSObject`ジェネリックサブクラスのサポート。詳細については、「 [NSObject ジェネリック](~/ios/internals/api-design/nsobject-generics.md)」を参照してください。 さらに、新しい登録システムは、以前は実行時にランダムな動作を引き起こした、サポートされていないジェネリックコンストラクトをキャッチします。

### <a name="errors-caught-by-the-new-registrar"></a>新しいレジストラーでキャッチされたエラー

次に、新しいレジストラーでキャッチされるエラーの例をいくつか示します。

- 同じクラス内で同じセレクターを複数回エクスポートする場合:

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

- 同じ目的 C 名で複数のマネージクラスをエクスポートする場合:

  ```csharp
  [Register ("Class")]
  class MyClass : NSObject {}

  [Register ("Class")]
  class YourClass : NSObject {}
  ```

- ジェネリックメソッドのエクスポート:

  ```csharp
  [Register]
  class MyDemo : NSObject
  {
      [Export ("foo")]
      void Foo<T> () {}
  }
  ```

### <a name="limitations-of-the-new-registrar"></a>新しいレジストラーの制限事項

新しいレジストラーについては、次の点に注意してください。

- 新しい登録システムで動作するように、一部のサードパーティライブラリを更新する必要があります。 詳細については、以下の「[必要な変更](#required-modifications)」を参照してください。

- また、アカウントフレームワークが使用されている場合、Clang を使用する必要があるという点もあります (これは、Apple の**Accounts. h**ヘッダーは clang でのみコンパイルできるためです)。 Xcode `--compiler:clang` 4.6 以前を使用している場合は、追加の mtouch 引数に clang を使用するように追加します (Xamarin は Xcode 5.0 以降で clang を自動的に選択します)。

- エクスポートされた型名に ASCII 以外の文字が含まれている場合、Xcode 4.6 (またはそれ以前) が使用されている場合は、GCC/G + + を選択する必要があります (これは、Xcode 4.6 に付属する Clang のバージョンでは、目的の C コードでは、識別子に含まれる ASCII 以外の文字をサポート 追加`--compiler:gcc`の mtouch 引数にを追加して、GCC を使用します。

## <a name="selecting-a-registrar"></a>レジストラーの選択

次のいずれかのオプションをプロジェクトの**IOS ビルド**設定の追加の mtouch 引数に追加することで、別のレジストラーを選択できます。

- `--registrar:static`–デバイスビルドの既定値
- `--registrar:dynamic`-シミュレータービルドの既定値

> [!NOTE]
> Xamarin の Classic API `--registrar:legacystatic`では、や`--registrar:legacydynamic`などの他のオプションがサポートされています。 ただし、これらのオプションは、Unified API ではサポートされていません。

## <a name="shortcomings-in-the-old-registration-system"></a>古い登録システムの欠点

以前の登録システムには、次のような欠点があります。

- サードパーティのネイティブライブラリでは、目的 C のクラスおよびメソッドへの (ネイティブな) 静的参照がありませんでした。これは、実際には使用されていなかったサードパーティのネイティブコード (すべてが削除されるため) を削除するようネイティブリンカーに要求できなかったことを意味します。 これは、 `-force_load libNative.a`すべてのサードパーティバインドが実行する必要があった (または`[LinkWith]`属性`ForceLoad=true`の同等の) ためです。
- 同じ目的 C 名を持つ2つのマネージ型を、警告なしでエクスポートできます。 まれに、異なる名前空間に2つ`AppDelegate`のクラスがあるというシナリオがあります。 実行時には、選択されたものが完全にランダムになります (実際には、非常に不可解でストレスのあるデバッグエクスペリエンスを実現するために再構築されていないアプリの実行間で異なります)。
- 同じ目的 C シグネチャを使用して2つのメソッドをエクスポートできます。 ここでも、前の例と同じように、この問題はランダムに呼び出されていましたが、この問題は前の例ほど一般的ではありませんでしたが、ほとんどの場合、このバグを実際に体験する唯一の方法は unlucky マネージメソッドをオーバーライドすることでした。
- エクスポートされたメソッドのセットは、動的ビルドと静的ビルドで若干異なりました。
- ジェネリッククラスのエクスポート時には正しく機能しません (実行時に実行される厳密なジェネリック実装はランダムで、実質的には動作が不定になります)。

<a name="required-modifications" />

## <a name="new-registrar-required-changes-to-bindings"></a>新しいレジストラー: バインドに必要な変更

このセクションでは、新しいレジストラーを使用するために行う必要があるバインドの変更について説明します。

### <a name="protocols-must-have-the-protocol-attribute"></a>プロトコルには [Protocol] 属性が必要です

プロトコルに`[Protocol]`属性が設定されている必要があります。 これを行わないと、次のようなネイティブリンカーエラーが発生します。

```console
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>セレクターには、有効な数のパラメーターが必要です

すべてのセレクターは、パラメーターの数を正確に示す必要があります。 以前は、これらのエラーは無視され、実行時の問題が発生する可能性がありました。

つまり、コロンの数はパラメーターの数と一致している必要があります。

- パラメーターなし:`foo`
- 1つのパラメーター:`foo:`
- 2つのパラメーター:`foo:parameterName2:`

不適切な使用方法を次に示します。

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>エクスポートで IsVariadic パラメーターを使用する

可変個引数関数で`IsVariadic`は、属性の`[Export]`引数を使用する必要があります。

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>既存のシンボルにリンクする必要があります

ネイティブライブラリに存在しないクラスをバインドすることはできません。
ネイティブライブラリでクラスを削除または名前変更した場合は、一致するようにバインドを更新してください。
