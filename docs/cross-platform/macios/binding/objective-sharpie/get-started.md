---
title: 目標油性の概要
description: このドキュメントでは、目標油性、c# OBJECTIVE-C コードへのバインドの作成を自動化するために使用するツールの概要を提供します。
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: da8c51c4ba4df74afac950bbff867221e7307d6e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854780"
---
# <a name="getting-started-with-objective-sharpie"></a>目標油性の概要

> [!IMPORTANT]
> 油性の目標は、OBJECTIVE-C の (および C の拡張機能によって) 高度な知識と経験豊富な Xamarin 開発者向けツールです。 Objective C ライブラリをバインドしようとする前に、コマンドライン (およびネイティブ ライブラリの動作をよく理解) でネイティブ ライブラリを構築する方法を十分に理解が必要です。

<a name="installing" />

## <a name="installing-objective-sharpie"></a>目標油性をインストールします。

目標油性 Mac OS X 10.10 の以降のバージョン、スタンドアロン コマンド ライン ツールでは現在、_完全にサポートされている Xamarin 製品ではない_します。 のみ使用してください高度な開発者がサード パーティの Objective C ライブラリにバインド プロジェクトの作成を支援します。

油性の目標は、標準 OS X パッケージ インストーラーとしてダウンロードできます。
インストーラーを実行し、次のいずれも、インストール ウィザードから画面の指示します。

- **現在のバージョン: 3.4**
  - [最新リリースをダウンロードします。](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [フォーラムのお知らせ](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> 使用して、`sharpie update`最新バージョンに更新するコマンド。

## <a name="basic-walkthrough"></a>基本的なチュートリアル

油性の目標は、サード パーティ製の Objective C ライブラリを c# にバインドする xamarin の作成に役立ちます定義が必要な指定のコマンド ライン ツールです。
目的の油性、開発者が使用する場合でも*は*目標油性がツールによって自動的に処理できなかったすべての問題に対処が完了したら、生成されたファイルを変更する必要があります。

目的の油性が Api の使用に疑問に正しくバインドする方法の注釈を設定が可能であれば、(あいまいなは、ネイティブ コードに多くの構成要素です)。
これらの注釈は、として表示されます。 [ `[Verify]`属性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)します。

油性の目的の出力が 1 組のファイル - [ `ApiDefinition.cs`と`StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) -ライブラリにコンパイルする Xamarin アプリで使用できるバインド プロジェクトを作成できます。

> [!IMPORTANT]
> 目標油性に含まれています**メジャー**適切な使用方法のルール: を実行する必要があります絶対に渡す正しい clang コンパイラのコマンドライン引数適切な解析できるようにするためにします。 これは単にツールが解析フェーズの目標油性[clang libtooling API に対して実装される](http://clang.llvm.org/docs/LibTooling.html)します。

これは、目標油性に Clang (実際にはバインドのネイティブ ライブラリをコンパイルする C/目的-C と C++ コンパイラ) とそのすべてのバインドのヘッダー ファイルの内部のナレッジの完全な電源が入っていることを意味します。
解析済みの変換ではなく[AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree)オブジェクトのコードに目標油性 c# バインディング「スキャフォールディング」の入力に適したに AST を変換する、`bmac`と`btouch`Xamarin バインディング ツール。

解析中にエラーを目標油性、場合その clang アウト中にエラーが発生した AST を構築しようとしています。 その解析フェーズと理由を解明する必要があります。

**新機能！** バージョン 3.0 の試行は、この複雑さの一部を Xcode プロジェクトを直接サポートして対処します。 ネイティブ ライブラリに、有効な Xcode プロジェクトがある場合は、目的の油性は指定されたターゲットと構成に必要な入力のヘッダー ファイルとコンパイラ フラグを推定するためのプロジェクトを評価できます。

Xcode プロジェクトが使用できない場合は、正しい入力ヘッダー ファイル、ヘッダー ファイルの検索パス、およびその他の必要なコンパイラ フラグをリスンしてプロジェクトをより理解する必要があります。 ネイティブ ライブラリをビルドするために使用するコンパイラ フラグ目標油性に渡す必要があると一致していることを実現する重要です。 これより手動のプロセスとは少し Clang ツール チェーンとコマンド ラインでネイティブ コードのコンパイルに関する知識を必要とする 1 つです。

**新機能！** バージョン 3.0 は、簡単にバインドするためのツールも導入されています。 [CocoaPods](https://cocoapods.org)を使用して、`sharpie pod`コマンド。
CocoaPod としてライブラリに関心がある場合は、(従来は、ソースに対して直接バインドしようとしています) と目標油性 CocoaPod をバインドしようとしてを起動するをお勧めします。

## <a name="related-links"></a>関連リンク

- [Xamarin University のコース: OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University のコース: 目標油性、OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)