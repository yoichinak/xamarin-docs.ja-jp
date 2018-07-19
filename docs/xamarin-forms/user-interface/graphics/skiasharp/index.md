---
title: Xamarin.Forms で SkiaSharp を使用します。
description: SkiaSharp は、.NET と c# は Google の製品で広く使用されるオープン ソース Skia グラフィック エンジンを利用した 2D グラフィックス システムです。 このガイドでは、Xamarin.Forms アプリケーションの 2D グラフィックスを SkiaSharp を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: bdc0603c6ae406adac42533e251106ccccf2cb7c
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130947"
---
# <a name="using-skiasharp-in-xamarinforms"></a>Xamarin.Forms で SkiaSharp を使用します。

_SkiaSharp を使用して、Xamarin.Forms アプリケーションでの 2D グラフィック_

SkiaSharp は、.NET と c# は Google の製品で広く使用されるオープン ソース Skia グラフィック エンジンを利用した 2D グラフィックス システムです。 2 次元ベクター グラフィックス、ビットマップ、およびテキストを描画するために、Xamarin.Forms アプリケーションで SkiaSharp を使用できます。 参照してください、 [2D 描画](~/graphics-games/skiasharp/index.md)SkiaSharp ライブラリに関する一般的な情報とその他のいくつかのチュートリアル ガイド。

このガイドでは、Xamarin.Forms のプログラミングに精通していることを前提としています。

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Xamarin.Forms のウェビナー: SkiaSharp**

## <a name="skiasharp-preliminaries"></a>SkiaSharp の準備作業

SkiaSharp の Xamarin.Forms は NuGet パッケージとしてパッケージ化されます。 For Mac、Visual Studio または Visual Studio で Xamarin.Forms ソリューションを作成したら、検索する NuGet パッケージ マネージャーを使用することができます、 **SkiaSharp.Views.Forms**パッケージ化し、ソリューションに追加します。 チェックする場合、**参照**セクション SkiaSharp を追加した後、各プロジェクトのことがわかりますさまざまな**SkiaSharp**ライブラリが各ソリューションのプロジェクトに追加されました。

Xamarin.Forms アプリケーションが iOS を対象と場合は、プロジェクトのプロパティ ページを使用して、iOS 8.0 を最小展開ターゲットを変更します。

SkiaSharp を使用する (C#) ページに含める必要あります、`using`ディレクティブを[ `SkiaSharp` ](https://developer.xamarin.com/api/namespace/SkiaSharp/)すべて SkiaSharp クラス、構造体、および、グラフィックスで使用する列挙体を含む名前空間プログラミングします。 必要、`using`ディレクティブを[ `SkiaSharp.Views.Forms` ](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) Xamarin.Forms に固有のクラスの名前空間。 これは、くらい小さい名前空間、最も重要なクラスが[ `SKCanvasView`](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/)します。 このクラスは、Xamarin.Forms から派生`View`クラスし、SkiaSharp グラフィックスの出力をホストします。

> [!IMPORTANT]
> `SkiaSharp.Views.Forms`名前空間にも含まれています、`SKGLView`クラスから派生した`View`が、グラフィックのレンダリングの OpenGL を使用します。 このガイドでは簡潔さのために、制限自体に`SKCanvasView`を使用していますが`SKGLView`は代わりによく似ています。

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[SkiaSharp 描画の基礎](basics/index.md)

SkiaSharp で描画できる最も簡単なグラフィック図形には、円、楕円、四角形などがあります。 これらの数値を表示するためには、SkiaSharp 座標、サイズ、および色について学びます。

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[SkiaSharp の線とパス](paths/index.md)

グラフィック パスは、一連の接続された直線と曲線です。 データの格納、パスを指定できますかまたは両方。 このトピックには、線の描画ストロークの端との結合、破線などと、点線がジオメトリのカーブできない場合は停止の多くの側面が含まれます。

## <a name="skiasharp-transformstransformsindexmd"></a>[SkiaSharp の変換](transforms/index.md)

変換では、グラフィックス オブジェクトを一様に変換、拡大縮小、回転、または傾斜を許可します。 この記事では、非アフィン変換を作成すると、パスに変換を適用する標準的な 3-3 での変換行列を使用する方法も示します。

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[SkiaSharp の曲線とパス](curves/index.md)

パスの探索は、パス オブジェクトに曲線を追加し、その他のパスの強力な機能を利用してで続行されます。 簡潔な文字列のパス全体を指定する方法、パスの効果を使用する方法、およびパスの内部を詳しく調査する方法を確認します。

## <a name="skiasharp-bitmapsbitmapsindexmd"></a>[SkiaSharp のビットマップ](bitmaps/index.md)

ビットマップは、ディスプレイ デバイスのピクセルに対応するビットの四角形の配列です。 この一連の記事では、ロード、保存、表示、作成、上に描画をアニメーション化する、SkiaSharp ビットマップのビットにアクセスする方法を示します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [SkiaSharp 付き Xamarin.Forms ウェビナー (ビデオ)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
