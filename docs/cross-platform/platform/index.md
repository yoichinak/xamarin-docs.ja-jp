---
title: Xamarin でのプログラミング言語のサポート
description: このドキュメントでは、Xamarin でサポートされるさまざまなプログラミング言語について説明します。 、、 C#ポータブルF#ビジュアル Basic.NET、および Razor テンプレートについて説明します。
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 2ec934b2747f89e959d659615629489e86449660
ms.sourcegitcommit: 3d21bb1a6d9b78b65aa49917b545c39d44aa3e3c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70065156"
---
# <a name="programming-language-support-in-xamarin"></a>Xamarin でのプログラミング言語のサポート

## <a name="c"></a>C\#

### <a name="async-support-overviewcross-platformplatformasyncmd"></a>[非同期サポートの概要](~/cross-platform/platform/async.md)

Version 5 でC#は、非同期操作を表すために、async と await の2つの新しいキーワードが導入されました。 これらのキーワードを使用すると、タスク並列ライブラリを利用して、別のスレッドで長時間実行される操作 (ネットワークアクセスなど) を実行し、完了時に結果に簡単にアクセスできる単純なコードを記述できます。 最新バージョンの Xamarin. iOS および Xamarin. Android サポート async と await-このドキュメントでは、Xamarin で新しい構文を使用する方法について説明し、例を示します。

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[C# 6 の言語機能](~/cross-platform/platform/csharp-six.md)

C#言語の最新バージョン (バージョン 6) では、言語を進化させて、定型句を小さくし、わかりやすくし、一貫性を高めることができます。 クリーンな初期化構文、ブロック内で`await` `catch/finally`使用する機能、および null 条件`?`演算子は特に便利です。

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

と Xamarin を使用F#したモバイルアプリの構築。

## <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[ポータブルビジュアル Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Visual Studio では、Visual Basic.NET を使用したポータブルクラスライブラリの作成がサポートされており、これを Xamarin アプリケーションに組み込むことができます。 この記事では、新しい Visual Basic PCL を作成し、サンプルの Xamarin、Xamarin、Android、Windows Phone アプリケーションで使用する方法について説明します。

## <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Razor テンプレートを使用した HTML ビューの作成](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin を使用すると、開発者は ASP.NET MVC で初めて導入されたC# Razor テンプレートエンジンを利用して、コードで html 文字列を手動で作成する手間をかけることなく、データを Html、JAVASCRIPT、CSS と簡単に組み合わせることができます。
この記事では、Android および iOS 用の Xamarin で Razor テンプレートを使用する方法について説明します。
