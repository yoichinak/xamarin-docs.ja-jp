---
title: "作業の開始"
ms.topic: article
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 01c390af08e59f3b10888a183df7fa6758c2609c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started"></a>作業の開始

<style type="text/css"> .terminal-blue { color: rgb(10,96,254); } .terminal-green { color: rgb(12,156,26); } .terminal-magenta { color: rgb(152,12,103); } </style>


> [!IMPORTANT]
> **警告:**目標ペンを使わず、Objective C の (および C の拡張機能によって) 高度な知識と経験を積んだ開発者向けツールです。 Objective C ライブラリをバインドする前に、コマンドライン (およびのネイティブ ライブラリのしくみをよく理解) ネイティブ ライブラリを構築する方法を十分に理解が必要です。

<a name="installing" />

# <a name="installing-objective-sharpie"></a>目標ペンを使わずにインストールします。

目標ペンを使わず Mac OS X 10.10 を以降のバージョンではスタンドアロンのコマンド ライン ツールでは現在、_完全にサポートされている Xamarin 製品ではない_です。 する必要がありますしか使えません上級開発者によって、サード パーティの Objective C のライブラリにバインド プロジェクトの作成を支援します。

目標ペンを使わずに標準の OS X パッケージ インストーラーとしてダウンロードできます。
インストーラーを実行し、次のすべてのインストール ウィザードから画面の指示します。

- **現在のバージョン: 3.4**
  - [最新のリリースをダウンロードします。](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [フォーラムのお知らせ](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> 💡 **ヒント:**を使用して、`sharpie update`コマンドを最新バージョンに更新します。

# <a name="basic-walkthrough"></a>基本的なチュートリアル

目標ペンを使わずはコマンド ライン ツールで作成するために役立ちます Xamarin 定義には必要サード パーティ製 Objective C のライブラリを c# にバインドします。
目標ペンを使わず、開発者を使用する場合でも*は*目標ペンを使わずがツールによって自動的に処理できなかったすべての問題に対処が完了したら、生成されたファイルを変更する必要があります。

目標ペンを使わずにいるが疑問に適切にバインドする方法の Api は注釈を付ける可能であれば、(ネイティブ コードに多くの構成要素は、あいまいです)。
これらの注釈は、として表示されます。 [ `[Verify]`属性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)です。

目標ペンを使わずの出力は次のファイルのペア[`ApiDefinition.cs`と`StructsAndEnums.cs`](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md)の Xamarin アプリで使用できるどのコンパイルをライブラリにバインド プロジェクトの作成に使用できます。

> [!IMPORTANT]
> 目標ペンを使わずが付属して 1 つ**メジャー**正しい使用法についての規則: 絶対に渡す必要がありますが、正しい clang コンパイラ コマンドライン引数適切に解析することを確認するためにします。 これは、解析フェーズの目標ペンを使わずだけのツールは、ため[clang libtooling API に対して実装される](http://clang.llvm.org/docs/LibTooling.html)です。

これは、目標ペンを使わずに Clang (実際にはバインドのネイティブ ライブラリをコンパイルする C/目標-C と C++ コンパイラ) とすべてのバインディングのヘッダー ファイルの内部情報のすべての電源が入っていることを意味します。
解析済みの変換ではなく[AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree)オブジェクト コードに目標ペンを使わずに、c# バインディング「スキャフォールディング」の入力に適したを AST の変換、`bmac`と`btouch`Xamarin バインド ツールです。

目標ペンを使わずのエラーが発生の解析中に、場合その clang 中にエラーを AST を構築しようとしています。 フェーズの解析と理由を解明する必要があります。

**新機能！** バージョン 3.0 の試行でアドレスをこの複雑さの一部の Xcode プロジェクトを直接サポートします。 ネイティブ ライブラリに、有効な Xcode プロジェクトがある場合は、目標ペンを使わずは指定されたターゲット、および構成は、必要な入力ヘッダー ファイルとコンパイラ フラグを推測するためのプロジェクトを評価できます。

Xcode プロジェクトが使用できない場合は、正しい入力ヘッダー ファイル、ヘッダー ファイルの検索パス、およびその他の必要なコンパイラ フラグをリスンしてプロジェクトに慣れている必要があります。 ネイティブ ライブラリをビルドするために使用するコンパイラ フラグが同じである目標ペンを使わずに渡す必要があることに注意してに重要です。 これはより手動のプロセスとは少し Clang ツール チェーンでのコマンド ラインでネイティブ コードのコンパイルに関する知識を必要と 1 つです。

**新機能！** バージョン 3.0 は、簡単にバインドするためのツールも導入されています。 [CocoaPods](https://cocoapods.org)を介して、`sharpie pod`コマンド。
CocoaPod として、ライブラリに関心がある場合 (ソースに対して直接バインドしようとしています) と目標ペンを使わずに CocoaPod をバインドしようとして起動するをお勧めします。