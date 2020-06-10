---
title: Xamarin の型レジストラー。 iOS
description: このドキュメントでは、C# クラスを目的の C ランタイムで使用できるようにする、Xamarin の種類のレジストラーについて説明します。
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 1a664b197b9e1a28f19ef9fd90a6f1dd26c159b4
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84565058"
---
# <a name="type-registrar-for-xamarinios"></a>Xamarin の型レジストラー。 iOS

このドキュメントでは、Xamarin. iOS によって使用されるタイプ登録システムについて説明します。

## <a name="registration-of-managed-classes-and-methods"></a>マネージクラスとマネージメソッドの登録

起動時に、Xamarin. iOS によって登録されます。

- [[Register]](xref:Foundation.RegisterAttribute)属性を持つクラスは、目的 C クラスとして使用されます。
- [[Category]](xref:ObjCRuntime.CategoryAttribute)属性を持つクラスは、目標 C カテゴリとして使用されます。
- " [Protocol](xref:Foundation.ProtocolAttribute) " 属性を持つインターフェイスを目的とする C プロトコルとして使用します。
- [[Export]](xref:Foundation.ExportAttribute)が設定されたメンバーは、目的の C にアクセスすることができます。

たとえば、 `Main` Xamarin. iOS アプリケーションで共通するマネージメソッドを考えてみます。

```csharp
UIApplication.Main (args, null, "AppDelegate");
```

このコードは、 `AppDelegate` アプリケーションのデリゲートクラスと呼ばれる型を使用するように、C ランタイムに指示します。 目的 C ランタイムで C# クラスのインスタンスを作成できるようにするには `AppDelegate` 、そのクラスを登録する必要があります。

Xamarin iOS は、実行時 (動的登録) またはコンパイル時 (静的登録) で、自動的に登録を実行します。

動的登録では、起動時にリフレクションを使用して、登録するすべてのクラスとメソッドを検索し、それらを目的の C ランタイムに渡します。 既定では、シミュレータービルドに動的登録が使用されます。

静的登録では、コンパイル時に、アプリケーションによって使用されるアセンブリが検査されます。 これは、目的の C に登録するクラスとメソッドを決定し、バイナリに埋め込まれているマップを生成します。
次に、スタートアップ時に、マップを目的の C ランタイムに登録します。 デバイスのビルドには、静的登録が使用されます。

### <a name="categories"></a>Categories

Xamarin. iOS 8.10 以降では、C# 構文を使用して目的 C カテゴリを作成できます。

カテゴリを作成するには、 `[Category]` 属性を使用して、拡張する型を指定します。 たとえば、次のコードはを拡張し `NSString` ます。

```csharp
[Category (typeof (NSString))]
```

各カテゴリのメソッドには属性があるため `[Export]` 、目的 C ランタイムで使用できるようになります。

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

すべてのマネージ拡張メソッドは静的である必要がありますが、拡張メソッドの標準の C# 構文を使用して、目的の C インスタンスメソッドを作成することができます。

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

この例では、ネイティブ `toUpper` インスタンスメソッドをクラスに追加 `NSString` します。 このメソッドは、次のように、目的の C から呼び出すことができます。

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

Xamarin. iOS 8.10 以降では、属性を持つインターフェイス `[Protocol]` が、プロトコルとして目的の C にエクスポートされます。

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

このコードは、というプロトコルとして、プロトコルを `IMyProtocol` 実装するという名前のクラスを目的の C にエクスポートし `MyProtocol` `MyClass` ます。

## <a name="new-registration-system"></a>新しい登録システム

安定した6.2.6 バージョンとベータ版6.3.4 バージョンから、新しい静的レジストラーが追加されました。 7.2.1 バージョンでは、新しいレジストラーが既定値として作成されました。

この新しい登録システムには、次の新機能が用意されています。

- コンパイル時のプログラマエラーの検出:
  - 2つのクラスが同じ名前で登録されています。
  - 同じセレクターに応答するために複数のメソッドがエクスポートされています
- 未使用のネイティブコードの削除:
  - 新しい登録システムは、スタティックライブラリで使用されるコードへの強い参照を追加します。これにより、ネイティブリンカーは、結果として得られるバイナリから未使用のネイティブコードを取り除くことができます。 Xamarin のサンプルバインドでは、ほとんどのアプリケーションが少なくとも300k より小さくなります。

- のジェネリックサブクラスのサポート `NSObject` 。詳細については、「 [NSObject ジェネリック](~/ios/internals/api-design/nsobject-generics.md)」を参照してください。 さらに、新しい登録システムは、以前は実行時にランダムな動作を引き起こした、サポートされていないジェネリックコンストラクトをキャッチします。

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

- また、アカウントフレームワークが使用されている場合、Clang を使用する必要があるという点もあります (これは、Apple の**Accounts. h**ヘッダーは clang でのみコンパイルできるためです)。 `--compiler:clang`Xcode 4.6 以前を使用している場合は、追加の mtouch 引数に Clang を使用するように追加します (Xamarin は Xcode 5.0 以降で Clang を自動的に選択します)。

- エクスポートされた型名に ASCII 以外の文字が含まれている場合、Xcode 4.6 (またはそれ以前) が使用されている場合は、GCC/G + + を選択する必要があります (これは、Xcode 4.6 に付属する Clang のバージョンでは、目的の C コードでは、識別子に含まれる ASCII 以外の文字をサポート 追加 `--compiler:gcc` の mtouch 引数にを追加して、GCC を使用します。

## <a name="selecting-a-registrar"></a>レジストラーの選択

次のいずれかのオプションをプロジェクトの**IOS ビルド**設定の追加の mtouch 引数に追加することで、別のレジストラーを選択できます。

- `--registrar:static`–デバイスビルドの既定値
- `--registrar:dynamic`-シミュレータービルドの既定値

> [!NOTE]
> Xamarin の Classic API では、やなどの他のオプションがサポートされてい `--registrar:legacystatic` `--registrar:legacydynamic` ます。 ただし、これらのオプションは、Unified API ではサポートされていません。

## <a name="shortcomings-in-the-old-registration-system"></a>古い登録システムの欠点

以前の登録システムには、次のような欠点があります。

- サードパーティのネイティブライブラリでは、目的 C のクラスおよびメソッドへの (ネイティブな) 静的参照がありませんでした。これは、実際には使用されていなかったサードパーティのネイティブコード (すべてが削除されるため) を削除するようネイティブリンカーに要求できなかったことを意味します。 これは、 `-force_load libNative.a` すべてのサードパーティバインドが実行する必要があった (または属性の同等の) ためです `ForceLoad=true` `[LinkWith]` 。
- 同じ目的 C 名を持つ2つのマネージ型を、警告なしでエクスポートできます。 まれに、異なる名前空間に2つのクラスがあるというシナリオが `AppDelegate` あります。 実行時には、選択されたものが完全にランダムになります (実際には、非常に不可解でストレスのあるデバッグエクスペリエンスを実現するために再構築されていないアプリの実行間で異なります)。
- 同じ目的 C シグネチャを使用して2つのメソッドをエクスポートできます。 ここでも、前の例と同じように、この問題はランダムに呼び出されていましたが、この問題は前の例ほど一般的ではありませんでしたが、ほとんどの場合、このバグを実際に体験する唯一の方法は unlucky マネージメソッドをオーバーライドすることでした。
- エクスポートされたメソッドのセットは、動的ビルドと静的ビルドで若干異なりました。
- ジェネリッククラスのエクスポート時には正しく機能しません (実行時に実行される厳密なジェネリック実装はランダムで、実質的には動作が不定になります)。

<a name="required-modifications"></a>

## <a name="new-registrar-required-changes-to-bindings"></a>新しいレジストラー: バインドに必要な変更

このセクションでは、新しいレジストラーを使用するために行う必要があるバインドの変更について説明します。

### <a name="protocols-must-have-the-protocol-attribute"></a>プロトコルには [Protocol] 属性が必要です

プロトコルに属性が設定されている必要があり `[Protocol]` ます。 これを行わないと、次のようなネイティブリンカーエラーが発生します。

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

可変個引数関数では、 `IsVariadic` 属性の引数を使用する必要があり `[Export]` ます。

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>既存のシンボルにリンクする必要があります

ネイティブライブラリに存在しないクラスをバインドすることはできません。
ネイティブライブラリでクラスを削除または名前変更した場合は、一致するようにバインドを更新してください。
