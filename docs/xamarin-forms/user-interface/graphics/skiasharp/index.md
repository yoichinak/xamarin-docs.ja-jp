---
title: "Xamarin.Forms で SkiaSharp の使用"
description: "Xamarin.Forms アプリケーションで 2 D グラフィックスの SkiaSharp を使用します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: eaebd8cbae996e9a5792d0a4898fafb72bdded47
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="using-skiasharp-in-xamarinforms"></a>Xamarin.Forms で SkiaSharp の使用

_Xamarin.Forms アプリケーションで 2 D グラフィックスの SkiaSharp を使用します。_

SkiaSharp は、.NET と c# Google 製品で広範囲に使用されるオープン ソース Skia グラフィックス engine によって 2 次元グラフィックス システムです。 Xamarin.Forms アプリケーションで SkiaSharp を使用すると、2 次元ベクター グラフィックス、ビットマップ、およびテキストを描画します。 参照してください、 [2D 図面](~/graphics-games/skiasharp/index.md)SkiaSharp ライブラリに関する一般的な情報およびその他のいくつかのチュートリアルについてガイドします。

このガイドでは、Xamarin.Forms プログラミングに慣れていることを前提としています。

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Xamarin.Forms のウェビナー: SkiaSharp**

## <a name="skiasharp-preliminaries"></a>SkiaSharp 準備

Xamarin.Forms の SkiaSharp は、NuGet パッケージとしてパッケージされています。 検索する NuGet パッケージ マネージャーを使用するには Mac を Visual Studio または Visual Studio で、まず Xamarin.Forms ソリューションを作成した後、 **SkiaSharp.Views.Forms**パッケージ化し、ソリューションに追加します。 チェックする場合、**参照**セクション SkiaSharp を追加した後の各プロジェクトのことがわかりますさまざまな**SkiaSharp**ライブラリが各ソリューションのプロジェクトに追加されました。

場合は、Xamarin.Forms アプリケーションは、iOS をターゲット、iOS 8.0 に最小の配置ターゲットを変更するプロジェクトのプロパティ ページを使用します。

SkiaSharp を使用する (C#) ページを追加する、`using`ディレクティブを[ `SkiaSharp` ](https://developer.xamarin.com/api/namespace/SkiaSharp/)名前空間は、すべて SkiaSharp クラス、構造体、および、グラフィックスで使用する列挙体が含まれますプログラミングします。 必要もあります、`using`ディレクティブを[ `SkiaSharp.Views.Forms` ](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) Xamarin.Forms に固有のクラスの名前空間。 これは、容量より小さい名前空間で最も重要なクラスは[ `SKCanvasView`](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/)です。 このクラスは、Xamarin.Forms から派生`View`クラスし、SkiaSharp グラフィック出力をホストします。

> [!IMPORTANT]
> `SkiaSharp.Views.Forms`名前空間も含まれています、`SKGLView`から派生したクラス`View`が、グラフィックのレンダリングの OpenGL を使用します。 わかりやすくするための目的では、このガイドに制限する`SKCanvasView`を使用して`SKGLView`が代わりに、非常に似ています。

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[SkiaSharp 描画の基礎](basics/index.md)

SkiaSharp で描画できる最も単純なグラフィックス図形には、円、楕円、および四角形などがあります。 これらの数値を表示するのには、SkiaSharp 座標、サイズ、および色について学習します。

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[SkiaSharp の線とパス](paths/index.md)

グラフィックス パスは、接続されている直線と曲線の系列です。 データの格納、パスを描画できますまたはその両方です。 このトピックにはと、点線は曲線ジオメトリ不足して停止する線の描画、ストロークの端との結合、点線などの多くの側面が含まれます。

## <a name="skiasharp-transformstransformsindexmd"></a>[SkiaSharp の変換](transforms/index.md)

変換を一様に翻訳、拡大/縮小、回転、または傾斜グラフィック オブジェクトを許可します。 この記事では、非アフィン変換を作成して、パスへの変換の適用の標準的な 3-3 での変換行列を使用する方法も示しています。

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[SkiaSharp の曲線とパス](curves/index.md)

パスの探索は、パス オブジェクトでは、曲線を追加すると、その他のパスの強力な機能を利用して続行します。 簡潔な文字列のパス全体を指定する方法、パスの効果を使用する方法、およびパスの内部構造を調べる方法が表示されます。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [Xamarin.Forms ウェビナー (ビデオ) と SkiaSharp](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
