---
title: SkiaSharp の線とパス
description: この記事では、SkiaSharp を使用して、Xamarin.Forms アプリケーションでの線とグラフィックス パスを描画する方法を説明し、サンプル コードを示します。
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: f9964d68e33e84dff789a4ad34443782f22ea821
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759162"
---
# <a name="skiasharp-lines-and-paths"></a>SkiaSharp の線とパス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp を使用して、行とグラフィックスのパスを描画するには_

[前のセクション](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)を示す、SkiaSharp`SKCanvas`クラスには、円、楕円、四角形、角の丸い四角形、テキスト、およびビットマップを描画するためにいくつかのメソッドが含まれています。 このセクションと以降のセクションの作成とレンダリングに接続されているさまざまなクラスをカバー*グラフィックス パス*します。

グラフィック パスは、行と SkiaSharp の曲線を描画する最も一般的なアプローチです。 ここを使用して、 [ `SKPath` ](xref:SkiaSharp.SKPath)オブジェクトの小さな直線のコレクションを使用して、直線を描画するために (と呼ばれる、*ポリライン*) アルゴリズムを定義できます曲線を描画するためにします。 以降のセクション[ **SkiaSharp の曲線とパス**](../curves/index.md)でサポートされている曲線のさまざまな種類について説明します`SKPath`します。

このセクションでは、すべてのサンプル プログラムが、見出しの下に表示**の線とパス**のホーム ページで、 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)プログラム、および、 [**パス**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths)をソリューションのフォルダー。

## <a name="lines-and-stroke-capslinesmd"></a>[線とストローク キャップ](lines.md)

SkiaSharp を使用して異なるストローク キャップを持つ行を描画する方法について説明します。

## <a name="path-basicspathsmd"></a>[パスの基礎](paths.md)

探索、SkiaSharp`SKPath`直線と曲線を結合するためのオブジェクト。

## <a name="the-path-fill-typesfill-typesmd"></a>[パスの塗りつぶしの種類](fill-types.md)

SkiaSharp のパスの塗りつぶしの種類で可能なさまざまな効果を検出します。

## <a name="polylines-and-parametric-equationspolylinesmd"></a>[多角形やパラメーターの式](polylines.md)

SkiaSharp を使用して、パラメーターの式を定義できる任意の行を表示します。

## <a name="dots-and-dashesdotsmd"></a>[ドットとダッシュ](dots.md)

点線および破線の SkiaSharp 描画の複雑さをマスターします。

## <a name="finger-paintingfinger-paintmd"></a>[指による描画](finger-paint.md)

指を使用して、キャンバスに描画します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
