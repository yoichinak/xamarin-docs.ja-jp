---
title: .NET 埋め込みエラー
description: このドキュメントでは、.NET 埋め込みによって生成されるエラーについて説明します。 エラーはコードによって一覧表示され、トラブルシューティングに役立つ説明が示されています。
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 9d3dff0de912455a49e9f7a66aae2640a99db746
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573912"
---
# <a name="net-embedding-errors"></a>.NET 埋め込みエラー

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: エラーメッセージのバインド

例: パラメーター、環境

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000"></a>

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: 予期しないエラー-バグレポートをに入力してくださいhttps://github.com/mono/Embeddinator-4000/issues

予期しないエラー状態が発生しました。 以下を含む、できるだけ多くの情報を含む[問題](https://github.com/mono/Embeddinator-4000/issues)を報告してください。

* 最大冗長性を持つ完全ビルドログ
* エラーを再現する最小限のテストケース
* すべてのバージョン情報

正確なバージョン情報を取得する最も簡単な方法は、[ **Xamarin Studio** ] メニューを使用し**て Xamarin Studio 項目について**、[**詳細の表示**] ボタンをクリックし、バージョン情報をコピーして貼り付けることです ([**情報のコピー** ] ボタンを使用できます)。

<a name="EM0001"></a>

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: 出力ディレクトリを作成できませんでした`X`

によって指定されたディレクトリ名は存在しないため、作成できません `-o=DIR` でした。 ファイルシステムの名前が無効である可能性があります。

<a name="EM0002"></a>

### <a name="em0002-option-x-is-not-supported"></a>EM0002: オプション `X` はサポートされていません

このツールでは、オプションはサポートされていません `X` 。 別のバージョンのツールでサポートされているか、 `X` この環境でオプションが適用されていない可能性があります。

<a name="EM0003"></a>

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: プラットフォーム `X` が無効です。

このツールでは、プラットフォームはサポートされていません `X` 。 別のバージョンのツールでサポートされているか、 `X` この環境にプラットフォームが適用されていない可能性があります。

<a name="EM0004"></a>

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: ターゲット `X` が無効です。

このツールでは、ターゲットはサポートされていません `X` 。 別のバージョンのツールでサポートされているか、 `X` この環境でターゲットが適用されていない可能性があります。

<a name="EM0005"></a>

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: コンパイルターゲット `X` が無効です。

このツールでは、コンパイルターゲットはサポートされていません `X` 。 別のバージョンのツールでサポートされているか、 `X` この環境にコンパイルターゲットが適用されていない可能性があります。

<a name="EM0006"></a>

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: Xcode の場所が見つかりませんでした。

このツールでは、コマンドを使用して現在選択されている Xcode の場所を見つけることができませんでした `xcode-select -p` 。 このコマンドを正常に実行できることを確認してから、正しい Xcode の場所を返してください。

<a name="EM0007"></a>

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: ' {sdk} ' の sdk バージョンを取得できませんでした。

ツールでは、コマンドを使用して SDK のバージョンを取得できませんでした `xcrun --show-sdk-version --sdk {sdk}` 。 このコマンドを正常に実行できることを確認し、SDK のバージョンを返します。

<a name="EM0008"></a>

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: アーキテクチャ ' {arch} ' は ' {platform} ' に対して無効です。 ' {Platform} ' の有効なアーキテクチャは次のとおりです: ' {アーキテクチャ} '。

エラーメッセージのアーキテクチャは、対象となるプラットフォームでは無効です。 オプションに有効なアーキテクチャが渡されていることを確認してください `--abi` 。

<a name="EM0009"></a>

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: この機能 `X` は、現在ジェネレーターによって実装されていません

これは、ジェネレーターの将来のリリースで修正が予定されている既知の問題です。 投稿は歓迎します。

<a name="EM0010"></a>

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: フレームワーク ' {simulatorFramework} ' と ' {deviceFramework} ' をマージできません。ファイル ' {file} ' が両方に存在します。

エラーメッセージに示されているフレームワークをマージできませんでした。これらの間に共通のファイルがあります。

これは、.NET 埋め込みのバグを示している可能性があります。テストケースでバグレポートをにファイルしてください [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues) 。

<a name="EM0011"></a>

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011: アセンブリ `X` が存在しません。

引数で指定されたアセンブリが見つかりませんでした `X` 。

<a name="EM0012"></a>

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: アセンブリ名 `X` が一意ではありません

指定された複数のアセンブリには同じ内部名があり、実行時にそれらを区別することはできません。

多くの場合、アセンブリがコマンドライン引数で複数回指定されていることが原因です。 ただし、名前が変更されたアセンブリは元の名前を保持し、複数のコピーを共存させることはできません。

<a name="EM0013"></a>

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013: ' Y ' によって参照されているアセンブリ ' X ' が見つかりません。

アセンブリ ' Y ' によって参照されているアセンブリ ' X ' が見つかりませんでした。 参照されているすべてのアセンブリが、バインドされるアセンブリと同じディレクトリにあることを確認してください。

<a name="EM0014"></a>

### <a name="em0014-could-not-find-product-product-min_version-is-required"></a>EM0014: {product} ({product} {min_version} が必要です) が見つかりませんでした。

エラーメッセージに示されている依存関係がシステムで見つかりませんでした。

<a name="EM0015"></a>

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-min_version-is-required"></a>EM0015: 有効なバージョンの {product} が見つかりませんでした (検出されたのは {version} ですが、少なくとも {min_version} が必要です)。

エラーメッセージに示されている依存関係がシステムに見つかりましたが、古すぎます。 新しいバージョンに更新してください。

<a name="EM0016"></a>

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016: シンボリックリンク ' {file} ' を作成できませんでした-> ' {target} ': エラー {number}

エラーメッセージに示されているシンボリックリンクを作成できませんでした。

<a name="EM0026"></a>

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>EM0026 はコマンドライン引数 ' A ' を解析できませんでした: *

コマンドラインオプションに指定された構文を `A` ツールで解析できませんでした。 正しい構文については、ドキュメントを確認してください。

<a name="EM0099"></a>

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099: 内部エラー *。 テストケース () でバグレポートをファイルに登録してください https://github.com/mono/Embeddinator-4000/issues) 。

このエラーメッセージは、.NET の埋め込みでの内部整合性チェックが失敗した場合に報告されます。

これは、.NET 埋め込みのバグを示します。テストケースでバグレポートをにファイルしてください [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues) 。

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: コード処理

<a name="EM1010"></a>

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: `T` はサポートされていないため、型は生成されません `X` 。

これは、 **warning**サポートされ `T` `X` ていない機能を使用しているため、型が無視される (つまり、何も生成されない) ことを示す警告です。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1011"></a>

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: 型 `T` が生成されません。この型には、対応するネイティブのマーシャリングコードが不足しています。

これは、追加のマーシャリングを必要とする .NET framework の内容を公開するため、型が無視されることを示す**警告**です `T` (つまり、何も生成されません)。

注: これは、将来のバージョンのツールでサポートされることがありますが、いくつかの制限があります。

<a name="EM1020"></a>

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: `C` パラメーターの型がサポートされていないため、コンストラクターが生成されません `T` 。

これは、 **warning** `C` 型のパラメーター `T` がサポートされていないため、コンストラクターが無視される (つまり、何も生成されない) ことを示す警告です。

型がサポートされていない理由の詳細については、以前に警告が発生 `T` しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1021"></a>

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: コンストラクターに `C` は、ラッパーが生成されない既定値があります。

これは、コンストラクターの既定のパラメーターが追加のコードを生成していないことを示す**警告**です `C` 。 最も一般的な原因は、既存のメソッドに同じシグネチャが既に存在することです。 たとえば、.NET では次のことが可能です。

```csharp
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

このような場合は、2つの生成されたセレクターだけが `init` Mono を呼び出しますが、後者のラッパーは存在しません。

<a name="EM1030"></a>

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: `M` 戻り値の型がサポートされていないため、メソッドは生成されません `T` 。

これは、 **warning** `M` 戻り値の型 `T` がサポートされていないため、メソッドが無視される (つまり、何も生成されない) ことを示す警告です。

型がサポートされていない理由の詳細については、以前に警告が発生 `T` しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1031"></a>

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: `M` パラメーターの型がサポートされていないため、メソッドは生成されません `T` 。

これは、 **warning** `M` 型のパラメーター `T` がサポートされていないために、メソッドが無視される (つまり、何も生成されない) ことを示す警告です。

型がサポートされていない理由の詳細については、以前に警告が発生 `T` しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1032"></a>

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: メソッド `M` には、ラッパーが生成されない既定値があります。

これは、メソッドの既定のパラメーターが追加のコードを生成していないことを示す**警告**です `M` 。 最も一般的な原因は、既存のメソッドに同じシグネチャが既に存在することです。 たとえば、.NET では次のことが可能です。

```csharp
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

このような場合は、2つの生成されたセレクターだけが `Increment` Mono を呼び出しますが、後者のラッパーは存在しません。

<a name="EM1033"></a>

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: `M` 別のメソッドがフレンドリ名で演算子を公開しているため、メソッドは生成されません。

これは、 **warning** `M` 別のメソッドがフレンドリ名を使用して演算子を公開するため、メソッドが生成されないことを示す警告です。 (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034"></a>

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: 拡張メソッド `M` は、プリミティブ型では作成できないため、カテゴリの内部では生成されません `T` 。 通常の静的メソッドが生成されました。

これは、の拡張メソッド (例:) が見つかったことを示す**警告**です `System.Int32` 。 目的 C では、プリミティブ型にカテゴリを作成することはできません。 代わりに、ジェネレーターによって通常の静的メソッドが生成されます。

<a name="EM1040"></a>

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: `P` パラメーターの型がサポートされていないため、プロパティは生成されません `T` 。

これは、 **warning**公開された `P` 型 `T` がサポートされていないため、プロパティが無視される (つまり、何も生成されない) ことを示す警告です。

型がサポートされていない理由の詳細については、以前に警告が発生 `T` しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1041"></a>

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041: `T` 複数のインデックス付きプロパティがサポートされていないため、のインデックス付きプロパティは生成されません。

これは、 **warning** `T` 複数のインデックス付きプロパティがサポートされていないため、のインデックス付きプロパティが無視される (つまり、何も生成されない) ことを示す警告です。

<a name="EM1050"></a>

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: フィールド `F` の種類がサポートされていないため、フィールドは生成されません `T` 。

これは、 **warning**公開された `F` 型 `T` がサポートされていないため、フィールドが無視される (つまり、何も生成されない) ことを示す警告です。

型がサポートされていない理由の詳細については、以前に警告が発生 `T` しています。

注: サポートされている機能は、新しいバージョンのツールで進化します。

<a name="EM1051"></a>

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: 要素 `E` の `F` 名前が重要な目標 c セレクターと競合するため、代わりに要素が生成されます。

これは、 **warning**要素の名前が `E` `F` 重要な目標 c セレクターと競合するため、要素が生成されることを示す警告です。

[NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)のセレクターは、目的 c では重要な意味を持ち、慎重にオーバーライドする必要があります。

注: 予約されたセレクターの一覧は、新しいバージョンのツールで進化します。

<a name="EM1052"></a>

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: 要素の `E` 名前が、同じクラスの他の要素と競合しています。

これは、 **warning** `E` 同じクラスの他の要素と名前が競合しているため、要素が生成されないことを示す警告です。

<a name="EM1053"></a>

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: Target `E` は、xamarin および xamarin. Mac ではサポートされていません。 オプションのみ `framework` がサポートされていると見なされ、使用する必要があります。

これは、 **warning**ターゲット `E` が xamarin および xamarin のユースケースでサポートされていないと見なされることを示す警告です。 

.NET 埋め込みライブラリを静的または動的に使用する場合は、追加の作業手順または調整が必要であり、ほとんどのユースケースでは回避する必要があります。

`--target`代わりに、パラメーターを削除するか、を渡すことを検討してください `--target=framework` 。

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: コード生成

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
