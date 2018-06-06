---
title: Xamarin での言語サポートのプログラミング
description: このドキュメントでは、Xamarin でサポートされているさまざまなプログラミング言語について説明します。 これは、c#、f#、ポータブル Visual Basic.NET、および Razor テンプレートについて説明します。
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 715f63a0be54ba3342bd63c1c76d89656313359a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781677"
---
# <a name="programming-language-support-in-xamarin"></a>Xamarin での言語サポートのプログラミング

## <a name="c"></a>C# 

###  <a name="async-support-overviewcross-platformplatformasyncmd"></a>[非同期サポートの概要](~/cross-platform/platform/async.md)

バージョン 5 の C# の場合に、非同期操作を表すための 2 つの新しいキーワードが導入されました: async と await です。 これらのキーワードでは、別のスレッドで実行時間の長い操作 (ネットワーク アクセスなど) を実行するには、タスク並列ライブラリを使用する単純なコードを記述し、完了時に結果を簡単にアクセスできます。 Xamarin.iOS および Xamarin.Android の最新バージョン サポート async および await - 説明、および Xamarin を使用した新しい構文の使用例を説明します。

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[C# 6 の言語機能](~/cross-platform/platform/csharp-six.md)

C# 言語 – version 6 – の最新バージョンは、小さい定型、強化されたわかりやすくするため、および一貫性に言語を発展させるが続行されます。 クリーナーの初期化の構文を使用する機能などが`await`で`catch/finally`ブロック、および null 条件`?`演算子は特に便利です。

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

F# および Xamarin でモバイル アプリケーションを構築します。

##  <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[ポータブル Visual Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Visual Studio では、Visual Basic.NET Xamarin アプリケーションに組み込むことができますし、これを使用して、ポータブル クラス ライブラリの作成をサポートします。 この記事では、新しい Visual Basic PCL を作成し、サンプル Xamarin.iOS、Xamarin.Android、および Windows Phone アプリケーションで使用する方法を示します。

##  <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Razor テンプレートを使用して構築 HTML ビュー](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin と c# コードでの HTML 文字列を手動で構築の手間をかけず、HTML、Javascript および CSS のデータを簡単に組み合わせて、ASP.NET MVC で最初に導入された、Razor テンプレート エンジンを活用することができます。
この記事では、Xamarin を使用した Android と iOS の Razor テンプレートを使用する方法を示します。
