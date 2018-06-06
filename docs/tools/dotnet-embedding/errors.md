---
title: .NET の埋め込みエラー
description: このドキュメントでは、.NET を埋め込むことによって生成されたエラーについて説明します。 エラーはコードによって一覧表示され、トラブルシューティングに役立つ説明を指定します。
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
author: topgenorth
ms.author: toopge
ms.date: 04/11/2018
ms.openlocfilehash: 5aa95379b18fb9412e7888c79a2cd19997f7258f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793719"
---
# <a name="net-embedding-errors"></a>.NET の埋め込みエラー

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: エラー メッセージのバインド

たとえば、 パラメーター、環境

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000" />

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: 予期しないエラーでバグ報告を入力してください。 https://github.com/mono/Embeddinator-4000/issues

予期しないエラーが発生しました。 ください[問題を報告](https://github.com/mono/Embeddinator-4000/issues)できるだけ多くの情報を含みます。

* 冗長性を最大でログをフル ビルドします。
* エラーを再現する最小限のテスト_ケース
* すべてのバージョン情報

正確なバージョン情報を取得する最も簡単な方法を使用する、 **Xamarin Studio** ] メニューの [ **Xamarin Studio に関する**項目、**詳細の表示**ボタンをクリックし、バージョンのコピー/貼り付け情報 (使用することができます、**コピー情報**ボタン)。

<a name="EM0001" />

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: 出力ディレクトリを作成できませんでした。 `X`

指定されたディレクトリ名`-o=DIR`が存在しないと、作成できませんでした。 ファイル システムに対して無効な名前があります。

<a name="EM0002" />

### <a name="em0002-option-x-is-not-supported"></a>EM0002: オプション`X`はサポートされていません

このツールは、オプションをサポートしていない`X`です。 別のバージョンのツールをサポートしているか、この環境では当てはまりません。

<a name="EM0003" />

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: プラットフォーム`X`が無効です。

このツールは、プラットフォームをサポートしていません`X`です。 別のバージョンのツールをサポートしているか、この環境では当てはまりません。

<a name="EM0004" />

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: ターゲット`X`が無効です。

このツールは、ターゲットをサポートしていません`X`です。 別のバージョンのツールをサポートしているか、この環境では当てはまりません。

<a name="EM0005" />

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: コンパイル対象`X`が無効です。

ツールは、コンパイル ターゲットをサポートしていません`X`です。 別のバージョンのツールをサポートしているか、この環境では当てはまりません。

<a name="EM0006" />

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: Xcode の場所を見つけられませんでした。

ツールが見つかりませんでした、現在選択されている Xcode の場所を使用して、`xcode-select -p`コマンド。 このコマンドが成功し、Xcode の正しい場所を返すことを確認してください。

<a name="EM0007" />

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: は、'{sdk}' の sdk バージョンを取得できませんでした。

このツールは、SDK のバージョンを使用してを取得できませんでした、`xcrun --show-sdk-version --sdk {sdk}`コマンド。 このコマンドが成功すると、し、SDK のバージョンを返すことを確認してください。

<a name="EM0008" />

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: '{arch}' のアーキテクチャでは、{プラットフォーム} は無効です。 {プラットフォーム} の有効なアーキテクチャが: '{アーキテクチャ}'。

対象のプラットフォームのアーキテクチャ、エラー メッセージが正しくありません。 --Abi オプションが有効なアーキテクチャを渡されることを確認してください。

<a name="EM0009" />

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: 機能`X`ジェネレーターによって現在実装されていません

これは、ジェネレーターの将来のリリースで修正しようとする既知の問題です。 コントリビューションは歓迎します。

<a name="EM0010" />

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: は、両方で、ファイル ' {}' が存在するためフレームワーク '{simulatorFramework}' と '{デバイス}' をマージすることはできません。

ツールは、それらの間に共通のファイルがあるために、エラー メッセージに記載されているフレームワークをマージできませんでした。

これは .NET 埋め込みのバグを示している可能性があります。バグ報告を送信してください[ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues)とテスト_ケースをします。

<a name="EM0011" />

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011: アセンブリ`X`存在しません。

ツールには、アセンブリが見つかりません`X`引数で指定します。

<a name="EM0012" />

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: アセンブリ名`X`一意ではありません

指定された 1 つ以上のアセンブリは同じ、内部の名前を持ち、実行時にファイルを区別することはできません。

最も一般的な原因は、コマンドライン引数にアセンブリが複数回指定されています。 ただしの共存元の名前と複数のコピーであるアセンブリの名前を変更したまま保持できません。

<a name="EM0013" />

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013: が 'X'、'Y' によって参照されるアセンブリを見つけることができません。

このツールは、'X'、'Y' アセンブリによって参照されるアセンブリを見つけられませんでした。 すべての参照アセンブリがバインドするアセンブリと同じディレクトリにあることを確認してください。

<a name="EM0014" />

### <a name="em0014-could-not-find-product-product-minversion-is-required"></a>EM0014: {製品} が見つかりませんでした ({製品} {min_version} が必要です)。

エラー メッセージに示されている依存関係がシステムに見つかりませんでした。

<a name="EM0015" />

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-minversion-is-required"></a>EM0015: {製品} の有効なバージョンが見つかりませんでした ({バージョン} が検出されましたが、少なくとも {min_version} が必要)。

依存関係は、システムでは、メッセージが見つかりましたが古すぎるため、エラーに示されています。 新しいバージョンに更新してください。

<a name="EM0016" />

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016: シンボリック リンクを作成できませんでした '{file}' は、'{ターゲット}']-> [: エラー {番号}。

エラー メッセージに記載されているシンボリック リンクを作成できませんでした。

<a name="EM0026" />

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>コマンドライン引数 'A' を解析できません EM0026 でした *。

コマンド ライン オプションに指定された構文`A`ツールで解析できませんでした。 可能性があります、正しくないドキュメントか、正しい構文のヘルプを参照してください。

<a name="EM0099" />

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099: 内部エラー * です。 テスト_ケースとバグのレポートを送信してください (https://github.com/mono/Embeddinator-4000/issues)です。

.NET の埋め込みに内部整合性チェックが失敗したときに、このエラー メッセージが報告されます。

これは .NET 埋め込みのバグを示しますバグ報告を送信してください[ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues)とテスト_ケースをします。

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: コードの処理

<a name="EM1010" />

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: 入力`T`ためには生成されません`X`はサポートされていません。

これは、**警告**を種類`T`は無視されます (つまり何が生成されます) を使用するため`X`、サポートされていない機能です。

注: サポートされる機能も進化を新しいバージョンのツールです。

<a name="EM1011" />

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: 入力`T`ネイティブの対応するマーシャ リング コードがないためには生成されません。

これは、**警告**を種類`T`は無視されます (つまり何が生成されます) 余分なマーシャ リングを必要とする .NET framework から何かを公開していること。

注: これは、将来のバージョンのツールのいくつかの制限と、サポートの取得可能性があるものです。

<a name="EM1020" />

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: コンス トラクター`C`ためパラメーターの型は生成されません`T`はサポートされていません。

これは、**警告**をコンス トラクターは、`C`は無視されます (つまり何が生成されます) ため型のパラメーター`T`はサポートされていません。

存在する必要があります理由の詳細を提供以前の警告タイプ`T`はサポートされていません。

注: サポートされる機能も進化を新しいバージョンのツールです。

<a name="EM1021" />

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: コンス トラクター`C`が既定値のラッパーが生成されません。

これは、**警告**をコンス トラクターの既定のパラメーター`C`余分なコードを生成するされません。 最も一般的な原因は、既存のメソッドが既に同じシグネチャを持つことです。 たとえば、 .net は、あります。

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

このような場合のみ 2 つ生成`init`セレクターが作成されます、モノラルに両方の呼び出し元が、それ以降のラッパーは必要ありません。

<a name="EM1030" />

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: メソッド`M`ために生成されない型を返す`T`はサポートされていません。

これは、**警告**をメソッド`M`は無視されます (つまり何が生成されます)、戻り値の型である`T`はサポートされていません。

存在する必要があります理由の詳細を提供以前の警告タイプ`T`はサポートされていません。

注: サポートされる機能も進化を新しいバージョンのツールです。

<a name="EM1031" />

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: メソッド`M`ためパラメーターの型は生成されません`T`はサポートされていません。

これは、**警告**をメソッド`M`は無視されます (つまり何が生成されます) ため型のパラメーター`T`はサポートされていません。

存在する必要があります理由の詳細を提供以前の警告タイプ`T`はサポートされていません。

注: サポートされる機能も進化を新しいバージョンのツールです。

<a name="EM1032" />

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: メソッド`M`が既定値のラッパーが生成されません。

これは、**警告**をメソッドの既定のパラメーター`M`余分なコードを生成するされません。 最も一般的な原因は、既存のメソッドが既に同じシグネチャを持つことです。 たとえば、 .net は、あります。

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

このような場合のみ 2 つ生成`increment`セレクターが作成されます、モノラルに両方の呼び出し元が、それ以降のラッパーは必要ありません。

<a name="EM1033" />

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: メソッド`M`別の方法はわかりやすい名前を持つ演算子を公開するためには生成されません。

これは、**警告**をメソッド`M`別の方法はわかりやすい名前を持つ演算子を公開するためには生成されません。 (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034" />

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: 拡張メソッド`M`プリミティブ型を作成できないため、カテゴリ内は生成されません`T`です。 通常、静的メソッドが生成されました。

これは、**警告**、primivite の拡張メソッドが入力 (例: `System.Int32`) が見つかりませんでした。 Objective C のプリミティブ型のカテゴリを作成することはできません。 代わりに、ジェネレーターには、通常、静的メソッドが生成されます。

<a name="EM1040" />

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: プロパティ`P`ためパラメーターの型は生成されません`T`はサポートされていません。

これは、**警告**をプロパティ`P`は無視されます (つまり何が生成されます) ため、公開されている型`T`はサポートされていません。

存在する必要があります理由の詳細を提供以前の警告タイプ`T`はサポートされていません。

注: サポートされる機能も進化を新しいバージョンのツールです。

<a name="EM1041" />

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041: インデックス付きプロパティを`T`複数のインデックス付きプロパティはサポートされていないためには生成されません。

これは、**警告**をインデックス付きのプロパティを`T`は無視されます (つまり何が生成されます) 複数のインデックス付きプロパティはサポートされていません。

<a name="EM1050" />

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: フィールド`F`ためフィールドの型は生成されません`T`はサポートされていません。

これは、**警告**をフィールド`F`は無視されます (つまり何が生成されます) ため、公開されている型`T`はサポートされていません。

存在する必要があります理由の詳細を提供以前の警告タイプ`T`はサポートされていません。

注: サポートされる機能も進化を新しいバージョンのツールです。

<a name="EM1051" />

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: 要素`E`代わりに生成されるとして`F`重要な目標 c セレクターとその名前が競合するためです。

これは、**警告**を要素`E`が生成されますとして`F`重要な目標 c セレクターとその名前が競合するためです。

セレクター、 [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) objective c 重要な意味を持ち、慎重にオーバーライドする必要があります。

注: 予約済みのセレクターの一覧も進化を新しいバージョンのツールです。

<a name="EM1052" />

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: 要素`E`は生成されず、名前が同じクラスの他の要素と競合しています。

これは、**警告**要素`E`同じクラスの他の要素とその名前が競合とは生成されません。

<a name="EM1053" />

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: 対象`E`Xamarin.iOS および Xamarin.Mac はサポートされていません。 のみ、`framework`オプションはサポートされており、使用すると見なされます。

これは、**警告**をターゲットとする`E`Xamarin.iOS および Xamarin.Mac ユース ケースに対してサポートされていないと見なされます。 

静的または動的な .NET の埋め込みのライブラリの使用量は、追加の作業の手順や手を加えればを必要可能性があり、ほとんどの用途に避ける必要があります。

削除を検討してください、`--target`パラメーターまたは pass`--target=framework`代わりにします。

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: コードの生成

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
