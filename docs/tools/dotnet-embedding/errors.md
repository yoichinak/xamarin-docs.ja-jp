---
title: .NET 埋め込みエラー
description: このドキュメントでは、.NET 埋め込みによって生成されるエラーについて説明します。 エラーはコードによって一覧表示され、トラブルシューティングに役立つ説明が示されています。
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 12db92d8522e9ec1ceddd9a41361f3b600991eeb
ms.sourcegitcommit: 7a55f096c17a20cfaa17c64d87490b63bbb6816e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/01/2020
ms.locfileid: "82630517"
---
# <a name="net-embedding-errors"></a>.NET 埋め込みエラー

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: エラーメッセージのバインド

例: パラメーター、環境

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000" />

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: 予期しないエラー-バグレポートをに入力してくださいhttps://github.com/mono/Embeddinator-4000/issues

予期しないエラー状態が発生しました。 以下を含む、できるだけ多くの情報を含む[問題](https://github.com/mono/Embeddinator-4000/issues)を報告してください。

* 最大冗長性を持つ完全ビルドログ
* エラーを再現する最小限のテストケース
* すべてのバージョン情報

正確なバージョン情報を取得する最も簡単な方法は、[ **Xamarin Studio** ] メニューを使用し**て Xamarin Studio 項目について**、[**詳細の表示**] ボタンをクリックし、バージョン情報をコピーして貼り付けることです ([**情報のコピー** ] ボタンを使用できます)。

<a name="EM0001" />

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: 出力ディレクトリを作成できませんでした`X`

によって`-o=DIR`指定されたディレクトリ名は存在しないため、作成できませんでした。 ファイルシステムの名前が無効である可能性があります。

<a name="EM0002" />

### <a name="em0002-option-x-is-not-supported"></a>EM0002: オプション`X`はサポートされていません

このツールでは、オプション`X`はサポートされていません。 別のバージョンのツールでサポートされているか、 `X`この環境でオプションが適用されていない可能性があります。

<a name="EM0003" />

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: プラットフォーム`X`が無効です。

このツールでは、プラットフォーム`X`はサポートされていません。 別のバージョンのツールでサポートされているか、 `X`この環境にプラットフォームが適用されていない可能性があります。

<a name="EM0004" />

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: ターゲット`X`が無効です。

このツールでは、ターゲット`X`はサポートされていません。 別のバージョンのツールでサポートされているか、 `X`この環境でターゲットが適用されていない可能性があります。

<a name="EM0005" />

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: コンパイルターゲット`X`が無効です。

このツールでは、コンパイルターゲット`X`はサポートされていません。 別のバージョンのツールでサポートされているか、 `X`この環境にコンパイルターゲットが適用されていない可能性があります。

<a name="EM0006" />

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: Xcode の場所が見つかりませんでした。

このツールでは、 `xcode-select -p`コマンドを使用して現在選択されている Xcode の場所を見つけることができませんでした。 このコマンドを正常に実行できることを確認してから、正しい Xcode の場所を返してください。

<a name="EM0007" />

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: ' {sdk} ' の sdk バージョンを取得できませんでした。

ツールでは、 `xcrun --show-sdk-version --sdk {sdk}`コマンドを使用して SDK のバージョンを取得できませんでした。 このコマンドを正常に実行できることを確認し、SDK のバージョンを返します。

<a name="EM0008" />

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: アーキテクチャ ' {arch} ' は ' {platform} ' に対して無効です。 ' {Platform} ' の有効なアーキテクチャは次のとおりです: ' {アーキテクチャ} '。

エラーメッセージのアーキテクチャは、対象となるプラットフォームでは無効です。 `--abi`オプションに有効なアーキテクチャが渡されていることを確認してください。

<a name="EM0009" />

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: この機能`X`は、現在ジェネレーターによって実装されていません

これは、ジェネレーターの将来のリリースで修正が予定されている既知の問題です。 投稿は歓迎します。

<a name="EM0010" />

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: フレームワーク ' {simulatorFramework} ' と ' {deviceFramework} ' をマージできません。ファイル ' {file} ' が両方に存在します。

エラーメッセージに示されているフレームワークをマージできませんでした。これらの間に共通のファイルがあります。

これは、.NET 埋め込みのバグを示している可能性があります。テストケースでバグレポートを[https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues)にファイルしてください。

<a name="EM0011" />

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011: アセンブリ`X`が存在しません。

引数で指定されたアセンブリ`X`が見つかりませんでした。

<a name="EM0012" />

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: アセンブリ名`X`が一意ではありません

指定された複数のアセンブリには同じ内部名があり、実行時にそれらを区別することはできません。

多くの場合、アセンブリがコマンドライン引数で複数回指定されていることが原因です。 ただし、名前が変更されたアセンブリは元の名前を保持し、複数のコピーを共存させることはできません。

<a name="EM0013" />

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013: ' Y ' によって参照されているアセンブリ ' X ' が見つかりません。

アセンブリ ' Y ' によって参照されているアセンブリ ' X ' が見つかりませんでした。 参照されているすべてのアセンブリが、バインドされるアセンブリと同じディレクトリにあることを確認してください。

<a name="EM0014" />

### <a name="em0014-could-not-find-product-product-min_version-is-required"></a>EM0014: {product} ({product} {min_version} が必要です) が見つかりませんでした。

エラーメッセージに示されている依存関係がシステムで見つかりませんでした。

<a name="EM0015" />

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-min_version-is-required"></a>EM0015: 有効なバージョンの {product} が見つかりませんでした (検出されたのは {version} ですが、少なくとも {min_version} が必要です)。

エラーメッセージに示されている依存関係がシステムに見つかりましたが、古すぎます。 新しいバージョンに更新してください。

<a name="EM0016" />

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016: シンボリックリンク ' {file} ' を作成できませんでした-> ' {target} ': エラー {number}

エラーメッセージに示されているシンボリックリンクを作成できませんでした。

<a name="EM0026" />

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>EM0026 はコマンドライン引数 ' A ' を解析できませんでした: *

コマンドラインオプション`A`に指定された構文をツールで解析できませんでした。 正しい構文については、ドキュメントを確認してください。

<a name="EM0099" />

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099: 内部エラー *。 テストケース (https://github.com/mono/Embeddinator-4000/issues)) でバグレポートをファイルに登録してください。

このエラーメッセージは、.NET の埋め込みでの内部整合性チェックが失敗した場合に報告されます。

これは、.NET 埋め込みのバグを示します。テストケースでバグレポートを[https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues)にファイルしてください。

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: コード処理

<a name="EM1010" />

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: は`T`サポートされて`X`いないため、型は生成されません。

これは、 **warning**サポートされて`T`いない機能を使用`X`しているため、型が無視される (つまり、何も生成されない) ことを示す警告です。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1011" />

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: 型`T`が生成されません。この型には、対応するネイティブのマーシャリングコードが不足しています。

これは、 **warning**追加のマーシャリングを`T`必要とする .net framework の内容を公開するため、型が無視されることを示す警告です (つまり、何も生成されません)。

注: これは、将来のバージョンのツールでサポートされることがありますが、いくつかの制限があります。

<a name="EM1020" />

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: パラメーター `C`の型`T`がサポートされていないため、コンストラクターが生成されません。

これは、型`T`のパラメーターが`C`サポートされていないため、コンストラクターが無視される (つまり、何も生成されない) ことを示す**警告**です。

型`T`がサポートされていない理由の詳細については、以前に警告が発生しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1021" />

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: コンストラクター `C`には、ラッパーが生成されない既定値があります。

これは、 **warning**コンストラクター `C`の既定のパラメーターが追加のコードを生成していないことを示す警告です。 最も一般的な原因は、既存のメソッドに同じシグネチャが既に存在することです。 たとえば、.NET では次のことが可能です。

```csharp
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

このような場合は、 `init` 2 つの生成されたセレクターだけが Mono を呼び出しますが、後者のラッパーは存在しません。

<a name="EM1030" />

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: 戻り`M`値の型`T`がサポートされていないため、メソッドは生成されません。

これは、 **warning**戻り値の型`M` `T`がサポートされていないため、メソッドが無視される (つまり、何も生成されない) ことを示す警告です。

型`T`がサポートされていない理由の詳細については、以前に警告が発生しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1031" />

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: パラメーター `M`の型`T`がサポートされていないため、メソッドは生成されません。

これは、型`T`のパラメーターが`M`サポートされていないために、メソッドが無視される (つまり、何も生成されない) ことを示す**警告**です。

型`T`がサポートされていない理由の詳細については、以前に警告が発生しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1032" />

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: メソッド`M`には、ラッパーが生成されない既定値があります。

これは、 **warning**メソッド`M`の既定のパラメーターが追加のコードを生成していないことを示す警告です。 最も一般的な原因は、既存のメソッドに同じシグネチャが既に存在することです。 たとえば、.NET では次のことが可能です。

```csharp
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

このような場合は、 `Increment` 2 つの生成されたセレクターだけが Mono を呼び出しますが、後者のラッパーは存在しません。

<a name="EM1033" />

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: 別`M`のメソッドがフレンドリ名で演算子を公開しているため、メソッドは生成されません。

これは、 **warning**別のメソッドが`M`フレンドリ名を使用して演算子を公開するため、メソッドが生成されないことを示す警告です。 (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034" />

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: 拡張メソッド`M`は、プリミティブ型`T`では作成できないため、カテゴリの内部では生成されません。 通常の静的メソッドが生成されました。

これは、 **warning**の拡張メソッド (例: `System.Int32`) が見つかったことを示す警告です。 目的 C では、プリミティブ型にカテゴリを作成することはできません。 代わりに、ジェネレーターによって通常の静的メソッドが生成されます。

<a name="EM1040" />

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: パラメーター `P`の型`T`がサポートされていないため、プロパティは生成されません。

これは、 **warning**公開された`P`型`T`がサポートされていないため、プロパティが無視される (つまり、何も生成されない) ことを示す警告です。

型`T`がサポートされていない理由の詳細については、以前に警告が発生しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1041" />

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041: 複数のインデックス`T`付きプロパティがサポートされていないため、のインデックス付きプロパティは生成されません。

これは、 **warning**複数のインデックス付きプロパティが`T`サポートされていないため、のインデックス付きプロパティが無視される (つまり、何も生成されない) ことを示す警告です。

<a name="EM1050" />

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: フィールド`F`の種類`T`がサポートされていないため、フィールドは生成されません。

これは、 **warning**公開された`F`型`T`がサポートされていないため、フィールドが無視される (つまり、何も生成されない) ことを示す警告です。

型`T`がサポートされていない理由の詳細については、以前に警告が発生しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1051" />

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: 要素`E`の名前が重要`F`な目標 c セレクターと競合するため、代わりに要素が生成されます。

これは、要素の名前が`E`重要な目標 c `F`セレクターと競合するため、要素が生成されることを示す**警告**です。

[NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)のセレクターは、目的 c では重要な意味を持ち、慎重にオーバーライドする必要があります。

注: 予約されたセレクターの一覧は、新しいバージョンのツールで進化します。

<a name="EM1052" />

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: 要素`E`の名前が、同じクラスの他の要素と競合しています。

これは、 **warning**同じクラスの`E`他の要素と名前が競合しているため、要素が生成されないことを示す警告です。

<a name="EM1053" />

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: Target `E`は、Xamarin および Xamarin. Mac ではサポートされていません。 `framework`オプションのみがサポートされていると見なされ、使用する必要があります。

これは、 **warning**ターゲット`E`が xamarin および xamarin のユースケースでサポートされていないと見なされることを示す警告です。 

.NET 埋め込みライブラリを静的または動的に使用する場合は、追加の作業手順または調整が必要であり、ほとんどのユースケースでは回避する必要があります。

代わりに、パラメーター `--target`を削除する`--target=framework`か、を渡すことを検討してください。

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: コード生成

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
