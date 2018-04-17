---
title: 型のレジストラー
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 9d4d2e5f3e1c2dc1233b466c00dd60340552fed0
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2018
---
# <a name="type-registrar"></a>型のレジストラー

このドキュメントでは、Xamarin.iOS によって使用される型登録システムについて説明します。

## <a name="registration-of-managed-classes-and-methods"></a>マネージ クラスとメソッドの登録

スタートアップ中に、Xamarin.iOS が登録されます。

  - クラスと、 [[登録]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) Objective C のクラスと属性。
  - クラスと、 [[Category]](https://developer.xamarin.com/api/type/CRuntime.CategoryAttribute) Objective C のカテゴリとして属性。
  - インターフェイスを持つ、 [[プロトコル]](https://developer.xamarin.com/api/type/Foundation.ProtocolAttribute/) Objective C のプロトコルの属性です。

持つ各ケースのメンバーで、 [[エクスポート]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/)目標 C. にエクスポートされ、属性 これにより、マネージ クラスを作成および Objective C から呼び出されるメソッドのマネージ c# world と OBJECTIVE-C いずれかのメソッドとプロパティがリンクされている方法です。

1 つの非常に単純な例は、すべてのアプリケーションがある AppDelegate クラスです。 管理対象の Main メソッドがこのいずれかのような行を持つことを思い出してくださいされます。

    UIApplication.Main (args, null, "AppDelegate");

これは、アプリケーションのデリゲート クラスとして"AppDelegate"と呼ばれる型を作成する Objective C のランタイムを示しています。  C# で記述された"AppDelegate"クラスのインスタンスを作成する方法を知って Objective C ランタイムの登録するこのクラスがあります。

Xamarin.iOS ランタイム登録の注意が、内部的には、(動的登録) を実行時に完全この登録を実行できます。 またはコンパイル時 (静的登録) に実行できます。  動的なアプローチには、起動時にリフレクションを使用して、すべてのクラスとメソッドを登録し、Objective C のランタイムに渡すを検索するが含まれます。  静的なアプローチでは、コンパイル時にアプリケーションによって使用されるアセンブリを検査します。  クラスと OBJECTIVE-C に登録する方法を決定し、これは、バイナリに埋め込むマップを生成します。  次に、起動時に、マップに登録 Objective C ランタイム。

### <a name="categories"></a>カテゴリ

Xamarin.iOS 8.10 で始まることは、c# の構文を使用して OBJECTIVE-C カテゴリを作成することになります。

これは、属性の引数として拡張する型を指定する、[Category] 属性を使用します。
次の例では、NSString が拡張のインスタンスは。

    [Category (typeof (NSString))]

各カテゴリのメソッドは、通常メカニズムを使用して、Objective C の [エクスポート] 属性を使用するメソッドをエクスポートするのには。

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

すべてのマネージ拡張メソッドは静的である必要がありますが、c# での拡張メソッドの標準構文を使用して、OBJECTIVE-C インスタンス メソッドを作成すること。

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

され、拡張メソッドの最初の引数は、メソッドが呼び出されたインスタンスになります。

完全な例:

    [Category (typeof (NSString))]
    public static class MyStringCategory
    {
        [Export ("toUpper")]
        static string ToUpper (this NSString self)
        {
            return self.ToString ().ToUpper ();
        }
    }

この例はネイティブ toUpper インスタンス メソッドに追加されます、NSString クラス、目標 C. から呼び出すことができます。

    [Category (typeof (UIViewController))]
    public static class MyViewControllerCategory
    {
        [Export ("shouldAutoRotate")]
        static bool GlobalRotate ()
        {
            return true;
        }
    }

### <a name="protocols"></a>プロトコル

以降では、Xamarin.iOS 8.10 [プロトコル] 属性を持つインターフェイスにエクスポートされます OBJECTIVE-C プロトコルとして。

例:

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

これは、プロトコル (MyProtocol) とプロトコルを実装するクラス (MyClass) として Objective C にエクスポートされます。

 **動的登録**

ビルドとデバッグのサイクルを高速化と、シミュレーターのビルドに使用されます。  これは、手順を削除するたびに、アプリケーションを起動して、一般的な用途にランチャーが毎回を使用する代わりに、クラスへのマッピングと、アプリケーション起動プログラムにこのマップ テーブルのコンパイルを生成する結果です。  デスクトップ コンピューターに十分なパワー ランタイムを実行するため、パフォーマンスに問題が決してクラスの迅速にスキャンします。

 **静的登録**

デバイスのビルド モバイル デバイスが、デスクトップ コンピューターよりも低速の低速スキャンは、ランタイムを実行するものです。  デバイスのビルドは常に新しいバイナリを作成する必要があります、ビルドとデバッグのサイクルが登録のマッピングを作成して影響はありません。

## <a name="new-registration-system"></a>新しい登録システム

安定した 6.2.6 以降のバージョンと新しい静的レジストラーが追加されました。 ベータ 6.3.4 バージョン。 7.2.1 バージョン行いました新しいレジストラー既定値です。

この新しい登録システムでは、次の新機能を提供します。

- コンパイル時にプログラマのエラーの検出します。
    - 2 つのクラスが登録されると、同じ名前です。
    - 同じセレクターに対応するエクスポートされた 1 つ以上のメソッド



- 未使用のネイティブ コードを削除することができます。
    - 新しい登録システムでは、結果のバイナリから未使用のネイティブ コードを取り除くネイティブ リンカーを許可するスタティック ライブラリで使用されるコードに対する強い参照を追加します。
      Xamarin のサンプルのバインド、ほとんどのアプリケーションになるには、少なくとも 300 k より小さい。

- NSObject の汎用のサブクラスをサポートします。 参照してください[NSObject ジェネリック](~/ios/internals/api-design/nsobject-generics.md)詳細についてはします。 さらに、新しい登録システムでは、原因となる以前実行時にランダムな動作がサポートされていないジェネリック コンストラクトをキャッチします。

新しい registar によってキャッチ、エラーのいくつかの例を次に示します。

同じクラス内に 2 回以上同じセレクターをエクスポートしています。

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo:")]
    void Foo (NSString str);
    [Export ("foo:")]
    void Foo (string str)
}
```

同じ Objective C の名前を持つ 2 つ以上のマネージ クラスをエクスポートしています。

```csharp
[Register ("Class")]
class MyClass : NSObject {}

[Register ("Class")]
class YourClass : NSObject {}
```

ジェネリック メソッドをエクスポートしています。

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo")]
    void Foo<T> () {}
}
```



新しいレジストラーに関する留意事項:
- 一部のサード パーティ ライブラリは、新しい登録システムを使用する更新する必要がありますが、セクションを参照して[以下の変更に必要な](#required_modifications)詳細についてはします。
- 短期的なマイナス面もですアカウント フレームワークを使用する場合に、Clang を使用する必要がある (これは、Apple の accounts.h ヘッダーは Clang によってのみコンパイルできません)。 追加<code>--compiler:clang</code>場合は、Clang を使用する追加 mtouch 引数に、Xcode 4.6 以降を使用している (Xamarin.iOS が自動的に選択 Clang Xcode 5.0 またはそれ以降)。

    <li>Xcode 4.6 (またはそれ以前) の場合使用、GCC/g++ 必要がありますを選択する場合はエクスポートされた型名 (これは Xcode 4.6 に出荷 Clang のバージョンでは、識別子内の非 ascii 文字 Objective C コードでサポートされていません) 非 ascii 文字を含めます。 追加<code>--compiler:gcc</code>GCC を使用する追加 mtouch 引数にします。


## <a name="selecting-a-registrar"></a>レジストラーを選択します。

次のオプションのいずれかのプロジェクトの iOS のビルド オプションの追加 mtouch 引数に追加することで、異なるレジストラーを選択できます。

-  `--registrar:static` : デバイスのビルドの既定値
-  `--registrar:dynamic` : シミュレーターのビルドの既定値
-  `--registrar:legacystatic` : デバイスは、Xamarin.iOS 7.2.1 に準拠するまでビルドの既定値
-  `--registrar:legacydynamic` : シミュレーターは、Xamarin.iOS 7.2.1 に準拠するまでビルドの既定値


## <a name="shortcomings-in-the-old-registration-system"></a>古い登録システムの欠点

古い登録システムでは、次の欠点があります。

-  Objective C のクラスとしたネイティブ リンカー (すべてのものが削除されます) のために使用されなかった実際にサード パーティ製のネイティブ コードを削除するように依頼できませんでした。 意図したもので、サードパーティ ネイティブ ライブラリ内のメソッドへの (ネイティブ) の静的参照がありませんでした。 これは、原因、"--force_load libNative.a"すべてのサードパーティのバインドを行う必要があること (またはときに、等価、ForceLoad [ソースコードファ] 属性で指定)。
-  警告なしで同じの Objective C の名前を持つ 2 つのマネージ型をエクスポートすることができます。 まれなシナリオの 2 つ AppDelegate クラス (別の名前空間) 終了することでした。 実行時になります完全にランダムな (非常に複雑ですしにくいデバッグ機能の作成されませんでした - も再構築するアプリの実行の間にさまざまなファクト) で選択されたものです。
-  同じ OBJECTIVE-C シグネチャを持つ 2 つのメソッドをエクスポートする可能性があります。 もう一度ランダムがどれが Objective C から呼び出されます (ただし実際にこの不具合が発生する唯一の方法が不吉マネージ メソッドをオーバーライドするためにほとんどの場合、この問題が以前のものでは、一般的なされませんでした)。
-  エクスポートされたメソッドのセットが、動的および静的なビルドの間で若干異なります。
-  これが正しく機能しませんジェネリック クラスをエクスポートするときに (実行時に実行される正確などの一般的な実装なりますランダム、実質的に何らかの動作)。


 <a name="required_modifications" />


## <a name="new-registrar-required-changes-to-bindings"></a>バインディングに必要な変更を新しいレジストラーは:


Objective C の既存のバインディングは、新しい registar と連動して更新する必要があります。

実行する必要がある変更の一覧を次に示します。

### <a name="protocols-must-have-the-protocol-attribute"></a>プロトコルは、[プロトコル] 属性を持つ必要があります。

プロトコルの実装は、[プロトコル] 属性を適用を今すぐが必要です。  いないこの場合、このようなネイティブ リンカー エラーがされます。

```csharp
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>セレクターは、有効な数のパラメーターが必要

すべてのセレクターは、正しくパラメーターの数を示す必要があります。  以前は、これらのエラーは無視されたされ、実行時の問題が発生する可能性があります。

要するに、コロンの数は、パラメーターの数と一致する必要があります。

例えば:

-  パラメーターなし:"foo"
-  1 つのパラメーター: ' foo:'
-  2 つのパラメーター: ' foo:parameterName2:'


不適切な使用方法を次に示します。

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>エクスポートで IsVariadic パラメーターを使用します。

可変個引数関数は、[エクスポート] 属性でこれを言う必要があります (これは、セレクターが引数の数は正しくないため、つまり 2 をポイントします。 上記の違反)。

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>既存のシンボルにリンクする必要があります。

ネイティブ ライブラリに存在しないクラスをバインドすることはできません。

存在しないクラスをバインドしようとする場合、ネイティブ リンカー エラーが表示されます。

これは通常、バインディングは、しばらくの間、存在しており、ネイティブ コードが変更されてその期間中にいるため、特定のネイティブ クラスが、削除または名前変更、バインディングが更新されていないときに場合に発生します。

