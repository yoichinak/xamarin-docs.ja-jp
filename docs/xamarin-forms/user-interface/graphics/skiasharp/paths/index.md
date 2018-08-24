---
title: SkiaSharp の線とパス
description: この記事では、SkiaSharp を使用して、Xamarin.Forms アプリケーションでの線とグラフィックス パスを描画する方法を説明し、サンプル コードを示します。
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 9febfabb7b44b1ec09abda4b352691b37565cb48
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615136"
---
# <a name="skiasharp-lines-and-paths"></a>SkiaSharp の線とパス

_SkiaSharp を使用して、行とグラフィックスのパスを描画するには_

[前のセクション](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)を示す、SkiaSharp`SKCanvas`クラスには、四角形、楕円、および角の丸い四角形を描画するためにいくつかのメソッドが含まれています。 このセクションと以降のセクションの作成とレンダリングに接続されているさまざまなクラスをカバー*グラフィックス パス*します。

グラフィック パスは、行と SkiaSharp の曲線を描画する最も一般的なアプローチです。 ここを使用して、`SKPath`オブジェクトの小さな直線のコレクションを使用して、直線を描画するために (と呼ばれる、*ポリライン*) 数学的に定義できる曲線を描画するためにします。 以降のセクションには、さまざまな種類でサポートされている曲線がについて説明しますが`SKPath`します。

という見出しの下に表示されるこのセクションのすべてのサンプル プログラム**線およびパス**のホーム ページで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)プログラム、し、 [**パス**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths)ソリューションのフォルダーです。

## <a name="lines-and-stroke-capslinesmd"></a>[線とストローク キャップ](lines.md)

SkiaSharp を使用して異なるストローク キャップを持つ行を描画する方法について説明します。

## <a name="path-basicspathsmd"></a>[パスの基礎](paths.md)

直線と曲線を結合するための SkiaSharp SKPath オブジェクトについて説明します。

## <a name="the-path-fill-typesfill-typesmd"></a>[パスの塗りつぶしの種類](fill-types.md)

SkiaSharp のパスの塗りつぶしの種類で可能なさまざまな効果を検出します。

## <a name="polylines-and-parametric-equationspolylinesmd"></a>[多角形やパラメーターの式](polylines.md)

SkiaSharp を使用して、パラメーターの式を定義できる任意の行を表示します。

## <a name="dots-and-dashesdotsmd"></a>[ドットとダッシュ](dots.md)

点線および破線の SkiaSharp 描画の複雑さをマスターします。

## <a name="finger-paintingfinger-paintmd"></a>[指による描画](finger-paint.md)

指を使用して、キャンバスに描画します。


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
