---
title: テキストとグラフィックスを統合します。
description: SkiaSharp グラフィックスとテキストを統合するレンダリングされたテキスト文字列のサイズを決定する方法を参照してください。
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 1607fe31785b6793175dfb61e1e12e34429aa089
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="integrating-text-and-graphics"></a>テキストとグラフィックスを統合します。

_SkiaSharp グラフィックスとテキストを統合するレンダリングされたテキスト文字列のサイズを決定する方法を参照してください。_

この記事では、テキストを測定、可能性のある拡張、テキストの特定のサイズ、およびその他のグラフィックスとテキストを統合する方法を示します。

![](text-images/textandgraphicsexample.png "四角形で囲まれたテキスト")

SkiaSharp`Canvas`クラスでは、四角形を描画するメソッドも含まれます ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) と角の丸い四角形 ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/))。 これらのメソッドとして定義する四角形の必要な`SKRect`値。

**フレーム化テキスト**ページ センター ページと角の丸い四角形のペアから成るフレームでブロックの挿入に短いテキスト文字列。 [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs)クラスは、その方法を示しています。

SkiaSharp でを使用して、`SKPaint`セットのテキストとフォントの属性をクラスも使用できますをレンダリングされるテキストのサイズを取得します。 次の先頭`PaintSurface`イベント ハンドラーを呼び出す 2 つの異なる`MeasureText`メソッドです。 最初の[ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/)呼び出しでは、単純な`string`引数と現在のフォント属性に基づいて、テキストの幅をピクセル単位を返します。 プログラムは、新しい計算`TextSize`のプロパティ、`SKPaint`オブジェクトが現在、レンダリングされた幅に基づく`TextSize`プロパティ、および表示領域の幅。 設定するためのものでは、この`TextSize`文字列が画面の幅の 90% に表示されるようにします。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string str = "Hello SkiaSharp!";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Chocolate
    };

    // Adjust TextSize property so text is 90% of screen width
    float textWidth = textPaint.MeasureText(str);
    textPaint.TextSize = 0.9f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds;
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

2 番目[ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/)呼び出しでは、`SKRect`引数、幅と表示されるテキストの高さを取得するようにします。 `Height`プロパティこの`SKRect`値は、大文字、アセンダー、およびテキスト文字列にディセンダーの有無によって異なります。 異なる`Height`の値がテキスト文字列"mom"、"cat"および"dog"、たとえば報告されます。

`Left`と`Top`のプロパティ、`SKRect`によってテキストが表示されている場合、構造体をレンダリングするテキストの左上隅の座標を示すため、`DrawText`を呼び出す 0 X と Y の位置。 このプログラムが 7 の iPhone シミュレーターで実行されているときになど`TextSize`90.6254 最初の呼び出しの後、計算の結果としての値が割り当てられている`MeasureText`です。 `SKRect` 2 番目の呼び出しから得られた値`MeasureText`は次のプロパティ値があります。

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

留意すること、X 座標と Y 座標を渡す、`DrawText`メソッドで指定されたベースラインのテキストの左側にあります。 `Top`値は、テキストが 68 ピクセルを基準と (68 it 88 から差し引くこと) の上に拡張されていることを示しますベースラインの下の 20 ピクセルです。 `Left` 6 の値が、テキストの先頭の X 値の右側に 6 のピクセルを示す、`DrawText`呼び出します。 これにより、通常の文字間スペースのためです。 ディスプレイ画面の左上隅でしっかりテキストを表示する場合は、これらの符号を反転を渡す`Left`と`Top`と、X 座標と Y 座標の値の`DrawText`、この例では&ndash;6 および 68。

`SKRect`構造体は、いくつかの便利なプロパティおよびの残りの部分で使用されるこれらのいくつかのメソッドを定義、`PaintSurface`ハンドラー。 `MidX`と`MidY`値は、四角形の中心の座標を示します。 (例では、iPhone 7、それらの値が 338.4107 と&ndash;24 です)。次のコードでは、座標の最も簡単な計算のこれらの値を使用して、ディスプレイ上のテキストを中央に。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(str, xText, yText, textPaint);
    ...
}
```

`PaintSurface`ハンドラーの 2 つの呼び出しを終了`DrawRoundRect`の引数を必要とする`SKRect`です。 これは、`SKRect`値に確実に似ています、`SKRect`から得られた値、`MeasureText`メソッドは同じであることはできません。 最初に、必要なもう少し大きくできるように、テキストのエッジ上角の丸い四角形を描画しないし、次に、空間に移動する必要がありますできるように、`Left`と`Top`値である四角形がここでは左上隅に対応配置されます。 行われますこれら 2 つのジョブ`Offset`と`Inflate`によって定義されたメソッド`SKRect`:。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Create a new SKRect object for the frame around the text
    SKRect frameRect = textBounds;
    frameRect.Offset(xText, yText);
    frameRect.Inflate(10, 10);

    // Create an SKPaint object to display the frame
    SKPaint framePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5,
        Color = SKColors.Blue
    };

    // Draw one frame
    canvas.DrawRoundRect(frameRect, 20, 20, framePaint);

    // Inflate the frameRect and draw another
    frameRect.Inflate(10, 10);
    framePaint.Color = SKColors.DarkBlue;
    canvas.DrawRoundRect(frameRect, 30, 30, framePaint);
}
```

次は、メソッドの残りの部分は、単純なです。 別の作成`SKPaint`枠線と呼び出しのオブジェクト`DrawRoundRect`2 回クリックします。 2 番目の呼び出しでは、別の 10 ピクセルずつ拡大長方形を使用します。 最初の呼び出しが 20 ピクセルの角の半径を指定します2 つ目はよう平行に見えるように 30 (ピクセル単位) の角の半径があります。

 [![](text-images/framedtext-small.png "テキストのフレームのページのスクリーン ショットをトリプル")](text-images/framedtext-large.png#lightbox "フレーム化テキスト ページのトリプル スクリーン ショット")

テキストとフレームのサイズの増加を表示するには、携帯電話やシミュレーターを横方向を有効にすることができます。

画面上のいくつかのテキストを中央揃えする場合は、行うことができますを設定して、テキストを測定することがなく約、`TextAlign`プロパティ`SKPaint`に`SKTextAlign.Center`です。 指定した X 座標、`DrawText`メソッドは、テキストの水平方向の中央の位置を示します。 画面の中間点を渡した場合、`DrawText`メソッド、テキストが水平方向に中央揃えと*ほぼ*垂直方向にベースラインが垂直方向に中央揃え、中央に配置します。

テキスト自体は、グラフィカル オプションを使用するようにより処理できます。 1 つの単純なオプションでは、通常の塗りつぶしの表示ではなく、テキスト文字のアウトラインを表示します。

[![](text-images/outlinedtext-small.png "3 つのテキストの説明 ページのスクリーン ショット")](text-images/outlinedtext-large.png#lightbox "3 つのテキストの説明 ページのスクリーン ショット")

これには、標準の変更を単純に`Style`のプロパティ、`SKPaint`オブジェクトの既定の設定から`SKPaintStyle.Fill`に`SKPaintStyle.Stroke`ストロークの幅を指定することで。 `PaintSurface`のハンドラー、**記載されているテキスト**ページは、その方法を示しています。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "OUTLINE";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 1,
        FakeBoldText = true,
        Color = SKColors.Blue
    };

    // Adjust TextSize property so text is 95% of screen width
    float textWidth = textPaint.MeasureText(text);
    textPaint.TextSize = 0.95f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds;
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

 過去いくつかの記事でが SkiaSharp を使用して、テキストを描画する方法を説明円、楕円、および角の丸い四角形。 次の手順は[SkiaSharp 線およびパス](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)でグラフィックス パスに接続されている直線を描画する方法が説明されます。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
