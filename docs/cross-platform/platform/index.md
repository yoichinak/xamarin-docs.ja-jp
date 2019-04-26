---
title: Xamarin での言語サポートのプログラミング
description: このドキュメントでは、Xamarin でサポートされている、さまざまなプログラミング言語について説明します。 説明C#、 F#、移植可能な Visual Basic.NET、および Razor テンプレート。
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 715f63a0be54ba3342bd63c1c76d89656313359a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035912"
---
# <a name="programming-language-support-in-xamarin"></a>Xamarin での言語サポートのプログラミング

## <a name="c"></a>C# 

###  <a name="async-support-overviewcross-platformplatformasyncmd"></a>[非同期サポートの概要](~/cross-platform/platform/async.md)

バージョン 5 のC#非同期操作を表現する 2 つの新しいキーワードの導入: async と await します。 これらのキーワードでは、別のスレッドで (ネットワーク アクセス) などの実行時間の長い操作を実行するには、タスク並列ライブラリを使用する単純なコードを記述し、完了時に結果を簡単にアクセスできます。 最新のバージョンの Xamarin.iOS と Xamarin.Android が非同期のサポートし、await - 説明、および Xamarin を使用した新しい構文を使用する例を説明します。

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[C# 6 の言語機能](~/cross-platform/platform/csharp-six.md)

最新バージョンのC#言語 – バージョン 6 – は少ない定型、明確さの向上、および一貫性のある言語の進化を続けています。 使用する機能などがより明確な初期化構文`await`で`catch/finally`ブロック、および null 条件`?`演算子は特に便利です。

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

モバイル アプリを構築F#と Xamarin。

##  <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[移植可能な Visual Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Visual Studio では、Xamarin アプリケーションに組み込むことができますが、Visual basic.net を使用して、ポータブル クラス ライブラリの作成をサポートします。 この記事では、サンプル Xamarin.iOS、Xamarin.Android、および Windows Phone アプリケーションで使用して新しい Visual Basic の PCL を作成する方法を示します。

##  <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Razor テンプレートを使用して構築 HTML ビュー](~/cross-platform/platform/razor-html-templates/index.md)

と共に ASP.NET MVC で初めて導入、Razor テンプレート エンジンを利用する開発者は XamarinC#コード内の HTML 文字列を手動で構築するための負担をかけず、HTML、Javascript、CSS でのデータを簡単に統合します。
この記事では、Android および iOS 用の Xamarin を使用した Razor テンプレートを使用する方法を示します。
