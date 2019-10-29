---
title: .NET 埋め込みエラー
description: このドキュメントでは、.NET 埋め込みによって生成されるエラーについて説明します。 エラーはコードによって一覧表示され、トラブルシューティングに役立つ説明が示されています。
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: b7cdf56ac4d917c8692f33d388adc003fabd9cac
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73007241"
---
# <a name="net-embedding-errors"></a>.NET 埋め込みエラー

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: エラーメッセージのバインド

たとえば、 パラメーター、環境

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000" />

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: 予期しないエラー- https://github.com/mono/Embeddinator-4000/issues にバグレポートを入力してください

予期しないエラー状態が発生しました。 以下を含む、できるだけ多くの情報を含む[問題](https://github.com/mono/Embeddinator-4000/issues)を報告してください。

* 完全なビルドログ、最大の詳細度
* エラーを再現する最小限のテストケース
* すべてのバージョンの解説

正確なバージョン情報を取得する最も簡単な方法は、 **[Xamarin Studio]** メニューを使用し**て、Xamarin Studio 項目について**、 **[詳細の表示]** ボタンをクリックし、バージョン情報をコピー/貼り付けすることです ( **[情報のコピー]** ボタンを使用できます)。

<a name="EM0001" />

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: 出力ディレクトリを作成できませんでした `X`

`-o=DIR` によって指定されたディレクトリ名は存在しないため、作成できませんでした。 ファイルシステムの名前が無効である可能性があります。

<a name="EM0002" />

### <a name="em0002-option-x-is-not-supported"></a>EM0002: オプション `X` はサポートされていません

このツールでは、オプション `X`がサポートされていません。 ツールの別のバージョンがサポートしているか、この環境に適用されていない可能性があります。

<a name="EM0003" />

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: プラットフォーム `X` が無効です。

このツールでは、プラットフォーム `X`がサポートされていません。 ツールの別のバージョンがサポートしているか、この環境に適用されていない可能性があります。

<a name="EM0004" />

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: ターゲット `X` が無効です。

このツールでは、ターゲット `X`がサポートされていません。 ツールの別のバージョンがサポートしているか、この環境に適用されていない可能性があります。

<a name="EM0005" />

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: コンパイルターゲット `X` が無効です。

このツールでは、コンパイルターゲット `X`はサポートされていません。 ツールの別のバージョンがサポートしているか、この環境に適用されていない可能性があります。

<a name="EM0006" />

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: Xcode の場所が見つかりませんでした。

`xcode-select -p` コマンドを使用して、現在選択されている Xcode の場所を見つけることができませんでした。 このコマンドが成功したことを確認し、正しい Xcode の場所を返してください。

<a name="EM0007" />

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: ' {sdk} ' の sdk バージョンを取得できませんでした。

ツールでは、`xcrun --show-sdk-version --sdk {sdk}` コマンドを使用して SDK のバージョンを取得できませんでした。 このコマンドが成功したことを確認し、SDK のバージョンを返してください。

<a name="EM0008" />

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: アーキテクチャ ' {arch} ' は {platform} では無効です。 {Platform} の有効なアーキテクチャは ' {アーキテクチャ} ' です。

エラーメッセージのアーキテクチャは、対象となるプラットフォームでは無効です。 --Abi オプションに有効なアーキテクチャが渡されていることを確認してください。

<a name="EM0009" />

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: 機能 `X` は、現在ジェネレーターによって実装されていません

これは、ジェネレーターの将来のリリースで修正が予定されている既知の問題です。 投稿は歓迎します。

<a name="EM0010" />

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: フレームワーク ' {simulatorFramework} ' と ' {deviceFramework} ' をマージできません。ファイル ' {file} ' が両方に存在します。

このツールでは、エラーメッセージに示されているフレームワークをマージできませんでした。これらの間に共通のファイルが存在します。

これは、.NET 埋め込みのバグを示している可能性があります。テストケースがある[https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues)でバグレポートをファイルに登録してください。

<a name="EM0011" />

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011: アセンブリ `X` が存在しません。

引数で指定された `X` アセンブリが見つかりませんでした。

<a name="EM0012" />

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: アセンブリ名 `X` が一意ではありません

複数のアセンブリが指定されている場合、内部名は同じであり、実行時にそれらを区別することはできません。

場合によっては、アセンブリがコマンドライン引数で複数回指定されていることが考えられます。 ただし、名前が変更されたアセンブリは引き続き元の名前を保持し、複数のコピーを共存することはできません。

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

コマンドラインオプションに指定された構文をツールで解析できませんでした `A`。 正しくない可能性があります。正しい構文については、ドキュメントまたはヘルプを参照してください。

<a name="EM0099" />

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099: 内部エラー *。 テストケース (https://github.com/mono/Embeddinator-4000/issues) を含むバグレポートをファイルに登録してください。

このエラーメッセージは、.NET の埋め込みでの内部整合性チェックが失敗した場合に報告されます。

これは、.NET 埋め込みのバグを示します。テストケースがある[https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues)でバグレポートをファイルに登録してください。

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: コード処理

<a name="EM1010" />

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: `X` はサポートされていないため、型 `T` は生成されません。

これは、サポートされていない機能である `X`が使用されているため、型 `T` が無視されることを示す**警告**です (つまり、何も生成されません)。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1011" />

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: 型 `T` が生成されません。この型には、対応するネイティブのマーシャリングコードが不足しています。

これは、追加のマーシャリングを必要とする .NET framework の内容を公開するため、型 `T` が無視されることを示す**警告**です (つまり、何も生成されません)。

注: これは、将来のバージョンのツールでサポートされることがありますが、いくつかの制限があります。

<a name="EM1020" />

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: パラメーターの型 `T` がサポートされていないため、コンストラクター `C` が生成されません。

これは、型 `T` のパラメーターがサポートされていないため、コンストラクター `C` が無視される (つまり、何も生成されない) ことを示す**警告**です。

型 `T` がサポートされていない理由の詳細については、前の警告が発生しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1021" />

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: コンストラクター `C` には、ラッパーが生成されない既定値があります。

これは、コンストラクター `C` の既定のパラメーターが追加のコードを生成していないことを示す**警告**です。 最も一般的な原因は、既存のメソッドに同じシグネチャが既に存在することです。 たとえば、 .net では、次のことが可能です。

```csharp
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

このような場合は、2つの生成された `init` セレクターのみが Mono を呼び出しますが、それ以降のラッパーは存在しません。

<a name="EM1030" />

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: `T` 戻り値の型がサポートされていないため、メソッド `M` は生成されません。

これは、戻り値の型 `T` がサポートされていないため、メソッド `M` が無視される (つまり、何も生成されない) ことを示す**警告**です。

型 `T` がサポートされていない理由の詳細については、前の警告が発生しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1031" />

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: パラメーターの型 `T` がサポートされていないため、メソッド `M` は生成されません。

これは、`T` 型のパラメーターがサポートされていないため、メソッド `M` が無視される (つまり、何も生成されない) ことを示す**警告**です。

型 `T` がサポートされていない理由の詳細については、前の警告が発生しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1032" />

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: メソッド `M` には、ラッパーが生成されない既定値があります。

これは、メソッド `M` の既定のパラメーターが追加のコードを生成していないことを示す**警告**です。 最も一般的な原因は、既存のメソッドに同じシグネチャが既に存在することです。 たとえば、 .net では、次のことが可能です。

```csharp
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

このような場合は、2つの生成された `increment` セレクターのみが Mono を呼び出しますが、それ以降のラッパーは存在しません。

<a name="EM1033" />

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: 別のメソッドがフレンドリ名で演算子を公開しているため、メソッド `M` は生成されません。

これは、別のメソッドがフレンドリ名を使用して演算子を公開するため、メソッド `M` が生成されないことを示す**警告**です。 (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034" />

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: 拡張メソッド `M` は、プリミティブ型 `T`で作成できないため、カテゴリの内部では生成されません。 通常の静的メソッドが生成されました。

これは、の拡張メソッド (例: `System.Int32`) が見つかったことを示す**警告**です。 目的 C では、プリミティブ型にカテゴリを作成することはできません。 代わりに、ジェネレーターは通常の静的メソッドを生成します。

<a name="EM1040" />

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: パラメーターの型 `T` がサポートされていないため、プロパティ `P` は生成されません。

これは、公開される型 `T` がサポートされていないため、プロパティ `P` が無視される (つまり、何も生成されない) ことを示す**警告**です。

型 `T` がサポートされていない理由の詳細については、前の警告が発生しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1041" />

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041: 複数のインデックス付きプロパティがサポートされていないため、`T` のインデックス付きプロパティは生成されません。

これは、複数のインデックス付きプロパティがサポートされていないため、`T` のインデックス付きプロパティが無視される (つまり、何も生成されない) ことを示す**警告**です。

<a name="EM1050" />

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: フィールドの種類 `T` がサポートされていないため、フィールド `F` が生成されません。

これは、公開される型 `T` がサポートされていないため、フィールド `F` が無視される (つまり、何も生成されない) ことを示す**警告**です。

型 `T` がサポートされていない理由の詳細については、前の警告が発生しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1051" />

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: 要素 `E` が `F` として生成されます。これは、名前が重要な目標 c セレクターと競合するためです。

これは、名前が重要な目標 c セレクターと競合しているために、要素 `E` が `F` として生成されることを示す**警告**です。

[NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)のセレクターは、目的 c では重要な意味を持ち、慎重にオーバーライドする必要があります。

注: 予約されたセレクターの一覧は、新しいバージョンのツールで進化します。

<a name="EM1052" />

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: 要素 `E` が生成されません。名前が同じクラスの他の要素と競合しています。

これは、名前が同じクラスの他の要素と競合するため、要素 `E` が生成されないことを示す**警告**です。

<a name="EM1053" />

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: ターゲット `E` は、Xamarin および Xamarin. Mac ではサポートされていません。 `framework` オプションのみがサポートされていると見なされ、使用する必要があります。

これは、ターゲット `E` が、Xamarin および Xamarin のユースケースではサポートされていないと見なされることを示す**警告**です。 

.NET 埋め込みライブラリを静的または動的に使用する場合は、追加の作業手順や調整が必要になることがあります。ほとんどのユースケースでは避ける必要があります。

`--target` パラメーターを削除するか、代わりに `--target=framework` を渡すことを検討してください。

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: コード生成

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
