---
title: SkiaSharp のグラフィック Xamarin.Forms
description: SkiaSharp は、Google 製品で幅広く使用されているオープンソース Skia グラフィックスエンジンを搭載した .NET および C# 用の2D グラフィックスシステムです。 このガイドでは、アプリケーションで SkiaSharp を2D グラフィックスに使用する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b91d8f459992d33f417e99f5d92ece63f887f691
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373433"
---
# <a name="skiasharp-graphics-in-no-locxamarinforms"></a>SkiaSharp のグラフィック Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_アプリケーションで2D グラフィックスに SkiaSharp を使用する Xamarin.Forms_

SkiaSharp は、Google 製品で幅広く使用されているオープンソース Skia グラフィックスエンジンを搭載した .NET および C# 用の2D グラフィックスシステムです。 アプリケーションで SkiaSharp を使用して Xamarin.Forms 、2d ベクターグラフィックス、ビットマップ、テキストを描画することができます。

このガイドでは、プログラミングについて理解していることを前提としてい Xamarin.Forms ます。

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**ウェビナー: SkiaSharp Xamarin.Forms**

## <a name="skiasharp-preliminaries"></a>SkiaSharp 準備作業

の SkiaSharp Xamarin.Forms は、NuGet パッケージとしてパッケージ化されています。 Xamarin.FormsVisual Studio または Visual Studio for Mac でソリューションを作成したら、NuGet パッケージマネージャーを使用して **SkiaSharp** パッケージを検索し、ソリューションに追加できます。 SkiaSharp を追加した後に各プロジェクトの [ **参照** ] セクションを確認すると、ソリューション内の各プロジェクトにさまざまな **SkiaSharp** ライブラリが追加されていることがわかります。

アプリケーションが Xamarin.Forms ios を対象としている場合は、その **情報の plist** ファイルを編集して、最小配置ターゲットを ios 8.0 に変更します。

SkiaSharp を使用する任意の C# ページで、名前空間のディレクティブを含める必要があります。これには、 `using` [`SkiaSharp`](xref:SkiaSharp) グラフィックスプログラミングで使用するすべての SkiaSharp クラス、構造体、および列挙体が含まれます。 また、 `using` [`SkiaSharp.Views.Forms`](xref:SkiaSharp.Views.Forms) に固有のクラスの名前空間のディレクティブも必要になり Xamarin.Forms ます。 これは、非常に小さな名前空間であり、最も重要なクラス [`SKCanvasView`](xref:SkiaSharp.Views.Forms.SKCanvasView) です。 このクラスは、クラスから派生 Xamarin.Forms `View` し、SkiaSharp グラフィックス出力をホストします。

> [!IMPORTANT]
> `SkiaSharp.Views.Forms`名前空間には、 `SKGLView` から派生したクラスも含まれ `View` ますが、グラフィックスのレンダリングに OpenGL を使用します。 わかりやすくするために、このガイド自体はに限定 `SKCanvasView` されていますが、代わりにを使用すること `SKGLView` は非常に似ています。

## <a name="skiasharp-drawing-basics"></a>[SkiaSharp 描画の基礎](basics/index.md)

SkiaSharp を使用して描画できる最も単純なグラフィックスの例として、円、楕円、四角形があります。 これらの図を表示する際には、SkiaSharp 座標、サイズ、および色について学習します。 テキストやビットマップの表示はより複雑ですが、これらの記事でもこれらの手法を紹介しています。

## <a name="skiasharp-lines-and-paths"></a>[SkiaSharp の線とパス](paths/index.md)

グラフィックスパスは、接続された直線と曲線の集まりです。 パスのストローク、塗りつぶし、またはその両方を行うことができます。 この記事では、ストロークの端と結合、破線と点線を含む直線描画のさまざまな側面について説明しますが、曲線のジオメトリは停止しません。

## <a name="skiasharp-transforms"></a>[SkiaSharp の変換](transforms/index.md)

変換を使用すると、グラフィックスオブジェクトを一様に平行移動、拡大縮小、回転、または傾斜させることができます。 この記事では、標準の 3-3 変換行列を使用して、非アフィン変換を作成し、パスに変換を適用する方法についても説明します。

## <a name="skiasharp-curves-and-paths"></a>[SkiaSharp の曲線とパス](curves/index.md)

パスの探索は、パスオブジェクトへの曲線の追加や、その他の強力なパス機能の活用によって継続されます。 簡潔なテキスト文字列でパス全体を指定する方法、パスの効果を使用する方法、およびパスの内部を掘り下げる方法について説明します。

## <a name="skiasharp-bitmaps"></a>[SkiaSharp のビットマップ](bitmaps/index.md)

ビットマップは、ディスプレイデバイスのピクセルに対応する四角形のビット配列です。 この一連の記事では、SkiaSharp ビットマップの読み込み、保存、表示、作成、描画、アニメーション化、およびビットへのアクセスを行う方法について説明します。

## <a name="skiasharp-effects"></a>[SkiaSharp の効果](effects/index.md)

効果は、線状および円形のグラデーション、ビットマップタイル、ブレンドモード、ぼかしなど、グラフィックスの通常の表示を変更するプロパティです。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [SkiaSharp with Xamarin.Forms ウェビナー (ビデオ)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)