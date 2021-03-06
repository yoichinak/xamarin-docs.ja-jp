---
title: 目標マジックペンを使用したはじめに
description: このドキュメントでは、目標マジックペンの概要を説明します。これは、目的の C コードへの C# バインディングの作成を自動化するために使用されるツールです。
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: davidortinau
ms.author: daortin
ms.date: 10/11/2017
ms.openlocfilehash: 5bee8df72290cab3ed6d5c23fef6c2ae2f1a2559
ms.sourcegitcommit: d1980b2251999224e71c1289e4b4097595b7e261
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2020
ms.locfileid: "92928517"
---
# <a name="getting-started-with-objective-sharpie"></a>目標マジックペンを使用したはじめに

> [!IMPORTANT]
> 目標マジックペンは、経験豊富な Xamarin 開発者向けのツールです (拡張機能、C)。 目的の C ライブラリをバインドする前に、コマンドラインでネイティブライブラリを構築する方法についての十分な知識を持っている必要があります (また、ネイティブライブラリのしくみについてよく理解している必要があります)。

<a name="installing"></a>

## <a name="installing-objective-sharpie"></a>目標マジックペンのインストール

目標マジックペンは、現在 Mac OS X 10.10 以降のスタンドアロンコマンドラインツールであり、 _完全にサポートされている Xamarin 製品ではありません_ 。 この機能は、サードパーティの目的 C ライブラリへのバインディングプロジェクトの作成を支援するために、上級開発者によってのみ使用されます。

目標マジックペンは、標準の OS X パッケージインストーラーとしてダウンロードできます。
インストーラーを実行し、インストールウィザードの画面に表示されるすべてのプロンプトに従います。

- **現在のバージョン: 3.4**
  - [最新のリリースのダウンロード](https://aka.ms/objective-sharpie)
  - [フォーラムのお知らせ](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> コマンドを使用して `sharpie update` 、最新バージョンに更新します。

## <a name="basic-walkthrough"></a>基本的なチュートリアル

目標マジックペンは、Xamarin によって提供されるコマンドラインツールであり、サードパーティの目的 C ライブラリを C# にバインドするために必要な定義を作成するのに役立ちます。
目標マジックペンを使用している場合でも、ツールによって自動的に処理されなかった問題に対処するために、開発者は、目標マジックペンの完了後に生成されたファイルを変更する *必要があり* ます。

可能であれば、目標マジックペンは、適切にバインドする方法に関して不明な Api に注釈を付けます (ネイティブコード内の多くのコンストラクターはあいまいです)。
これらの注釈は[ `[Verify]` 属性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)として表示されます。

目標マジックペンの出力は、ファイルとのペアです。これを使用する[ `ApiDefinition.cs` と `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) 、Xamarin アプリで使用できるライブラリにコンパイルされるバインドプロジェクトを作成できます。

> [!IMPORTANT]
> 目標マジックペンには、 **major** 適切な解析を確実に行うために、正しい clang コンパイラコマンドライン引数を渡す必要があります。 これは、目標のマジックペン解析フェーズは、 [clang libtooling API に対して実装](https://clang.llvm.org/docs/LibTooling.html)されたツールであるためです。

これは、目標マジックペンが Clang (バインドするネイティブライブラリを実際にコンパイルする C/c/c/c + + コンパイラ) の全機能と、ヘッダーファイルに関するすべての内部ナレッジをバインドすることを意味します。
解析された [ast](https://en.wikipedia.org/wiki/Abstract_syntax_tree) をオブジェクトコードに変換するのではなく、目標マジックペンは ast を、 `bmac` および Xamarin バインドツールへの入力に適した C# バインド "スキャフォールディング" に変換し `btouch` ます。

マジックペンの解析中にエラーが発生した場合は、その解析中に AST を構築しようとしたときに clang でエラーが発生したことを意味し、その理由を解明する必要があります。

**新機能！** バージョン3.0 では、Xcode プロジェクトを直接サポートすることで、このような複雑さの一部に対処します。 ネイティブライブラリに有効な Xcode プロジェクトがある場合、目標マジックペンは、指定されたターゲットと構成のプロジェクトを評価して、必要な入力ヘッダーファイルとコンパイラフラグを推測できます。

使用できる Xcode プロジェクトがない場合は、正しい入力ヘッダーファイル、ヘッダーファイルの検索パス、およびその他の必要なコンパイラフラグをと判断ことで、プロジェクトについて理解しておく必要があります。 ネイティブライブラリの構築に使用されるコンパイラフラグは、目標マジックペンに渡す必要があるものと同じであることに注意してください。 これは、より手動のプロセスです。また、Clang ツールチェーンを使用してコマンドラインでネイティブコードをコンパイルする場合は、多少の知識が必要です。

**新機能！** また、バージョン3.0 では、コマンドを使用して [Cocoアポストロフィ](https://cocoapods.org) を簡単にバインドするためのツールも導入さ `sharpie pod` れています。
興味のあるライブラリが Cocoアポストロフィ d として使用可能な場合は、最初に Cocoアポストロフィ d と目標マジックペンをバインドすることをお勧めします (ソースに直接バインドすることはできません)。
