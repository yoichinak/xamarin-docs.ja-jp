---
title: 傾斜変換
description: この記事では、傾斜変換が SkiaSharp に傾いているグラフィカル オブジェクトを作成する方法について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2017
ms.openlocfilehash: a0c65ab2d319e39b236799cd453874142206b467
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53058887"
---
# <a name="the-skew-transform"></a>傾斜変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_傾斜変換が SkiaSharp に傾いているグラフィカル オブジェクトを作成する方法を参照してください。_

SkiaSharp、傾斜変換は、このイメージ内のシャドウなどのグラフィカル オブジェクトを傾けます。

![](skew-images/skewexample.png "シャドウのテキストを傾斜プログラムから傾斜の例")

スキュー、平行四辺形に四角形をオンにしますが、傾斜楕円が楕円。

平行移動、拡大縮小、および回転のプロパティを定義しますが、Xamarin.Forms は対応するプロパティ Xamarin.Forms でスキューを

[ `Skew` ](xref:SkiaSharp.SKCanvas.Skew(System.Single,System.Single))メソッドの`SKCanvas`スキューの水平方向および垂直の 2 つの引数の傾斜を受け入れます。

```csharp
public void Skew (Single xSkew, Single ySkew)
```

1 秒あたり[ `Skew` ](xref:SkiaSharp.SKCanvas.Skew(SkiaSharp.SKPoint))メソッドは、1 つの引数を組み合わせた`SKPoint`値。

```csharp
public void Skew (SKPoint skew)
```

ただしを使用するこれら 2 つのメソッドのいずれかの分離可能性が高いです。

**実験傾斜**値 – 10 と 10 の範囲をページの傾斜を試すことができます。 テキスト文字列が 2 から取得した傾きの値をページの左上隅に配置されている`Slider`要素。 ここでは、`PaintSurface`ハンドラーで、 [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs)クラス。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        canvas.Skew((float)xSkewSlider.Value, (float)ySkewSlider.Value);
        canvas.DrawText(text, 0, -textBounds.Top, textPaint);
    }
}
```

値、`xSkew`引数が正の値のテキストまたは負の値の左側の下部にあるをシフトします。 値`ySkew`または負の値に正の値の下、テキストの右側にシフトします。

[![](skew-images/skewexperiment-small.png "傾斜の実験のページのスクリーン ショットをトリプル")](skew-images/skewexperiment-large.png#lightbox "傾斜実験ページの 3 倍になるスクリーン ショット")

場合、`xSkew`値が負の値、`ySkew`値、結果は、回転が、多少として拡張することも、UWP の表示を示します。

変換式は次のとおりです。

x' = x + xSkew · y

y' ySkew 押しを =x + y

たとえば、正`xSkew`値に変換された`x'`値が増えるにつれて`y`が増加します。 傾きの原因は何です。

三角形 200 ピクセル、高さ 100 ピクセルが (0, 0) の時点で、左上隅に配置されているし、でレンダリングされる場合、 `xSkew` 1.5 では、平行四辺形の結果は次の値。

![](skew-images/skeweffect.png "四角形の傾斜変換の効果")

下端の座標が`y`できるようになります、100 の値が右側に 150 ピクセルをシフトします。

値が 0 以外の`xSkew`または`ySkew`、のみ、ポイント (0, 0) は変わりません。 そのポイントは、傾斜の中心を見なすことができます。 必要がある場合にその他の傾斜の中心 (これは通常の場合)、あるありません`Skew`メソッドを提供します。 明示的に結合する必要があります`Translate`を呼び出し、`Skew`呼び出します。 中央にある傾斜`px`と`py`、次の呼び出しを行います。

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

複合変換式は、次のとおりです。

x' = x + xSkew 押し(y – py)

y' ySkew 押しを =(x – px) + y

場合`ySkew`0 の場合は、次に、`px`値は使用されません。 値が、関連して同様に`ySkew`と`py`します。

この図では、角度 α などの傾きの角度の傾斜を指定する使いやすいを感じることがあります。

![](skew-images/skewangleeffect.png "四角形の傾斜の角度で傾斜変換の効果が示されます")

100 ピクセル垂直に 150 ピクセルのシフトの比率は、この例では、その角度のタンジェントは、56.3 度です。

XAML ファイル、**傾斜角度実験**に似ている、**傾斜の角度**ページ点を除いて、`Slider`要素-90 から 90 ° の範囲します。 [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs)分離コード ファイルは、ページ上のテキストを中央揃え、使用して`Translate`ページの中央に傾斜の中心を設定します。 短い`SkewDegrees`コードの下部にあるメソッドは、角度の傾斜の値を変換します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;

        canvas.Translate(xCenter, yCenter);
        SkewDegrees(canvas, xSkewSlider.Value, ySkewSlider.Value);
        canvas.Translate(-xCenter, -yCenter);
        canvas.DrawText(text, xText, yText, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

角度が 90 度を正または負の値に近づく、タンジェント、無限大のアプローチが最大で約 80 度などの角度は、使用可能です。

[![](skew-images/skewangleexperiment-small.png "傾斜の角度実験ページのスクリーン ショットをトリプル")](skew-images/skewangleexperiment-large.png#lightbox "傾斜角度実験ページの 3 倍になるスクリーン ショット")

小さい負水平方向の傾斜として斜投影体または斜体のテキストを模倣、**斜体テキスト**ページを示します。 [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs)クラスを行う方法を示しています。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Maroon,
        TextAlign = SKTextAlign.Center,
        TextSize = info.Width / 8       // empirically determined
    })
    {
        canvas.Translate(info.Width / 2, info.Height / 2);
        SkewDegrees(canvas, -20, 0);
        canvas.DrawText(Title, 0, 0, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

`TextAlign`プロパティの`SKPaint`に設定されている`Center`します。 任意の変換を行わず、`DrawText`の座標を使用して呼び出す (0, 0) 左上隅にあるテキスト ベースラインの水平方向の中心の位置は。 `SkewDegrees`水平方向にベースラインを基準と 20 度のテキストを傾けます。 `Translate`呼び出しは、キャンバスの中央にテキストのベースラインの水平方向の中心を移動します。

[![](skew-images/obliquetext-small.png "斜体のテキスト ページのスクリーン ショットをトリプル")](skew-images/obliquetext-large.png#lightbox "斜体のテキスト ページの 3 倍になるスクリーン ショット")

**シャドウのテキストを傾斜**ページが 45 度の傾斜、垂直方向のスケールの組み合わせを使用して、テキストから傾けますテキストの影を作成する方法を示します。 関連する部分を次に示します、`PaintSurface`ハンドラー。

```csharp
using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = info.Width / 6;   // empirically determined

    // Common to shadow and text
    string text = "Shadow";
    float xText = 20;
    float yText = info.Height / 2;

    // Shadow
    textPaint.Color = SKColors.LightGray;
    canvas.Save();
    canvas.Translate(xText, yText);
    canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
    canvas.Scale(1, 3);
    canvas.Translate(-xText, -yText);
    canvas.DrawText(text, xText, yText, textPaint);
    canvas.Restore();

    // Text
    textPaint.Color = SKColors.Blue;
    canvas.DrawText(text, xText, yText, textPaint);
}
```

シャドウは、表示されている 1 つ目とし、テキストには。

[![](skew-images/skewshadowtext1-small.png "傾斜シャドウ テキスト ページのスクリーン ショットをトリプル")](skew-images/skewshadowtext1-large.png#lightbox "傾斜シャドウ テキスト ページの 3 倍になるスクリーン ショット")

渡される垂直座標、`DrawText`メソッド ベースラインを基準とテキストの位置を示します。 傾斜のセンターに使用される同じ垂直座標です。 テキスト文字列にディセンダーが含まれている場合、この手法は機能しません。 たとえば、「シャドウ」の「奇妙」という単語を置き換えるし、結果を次に示します。

[![](skew-images/skewshadowtext2-small.png "ディセンダーと代替の word、傾斜シャドウ テキスト ページの 3 倍になるスクリーン ショット")](skew-images/skewshadowtext2-large.png#lightbox "ディセンダーと代替の word、傾斜シャドウ テキスト ページの 3 倍になるスクリーン ショット")

シャドウとテキストは基本的には、配置もが、効果はだけが正しく表示します。 これを修正するには、テキストの境界を取得する必要があります。

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

`Translate`呼び出しが、ディセンダーの高さに調整する必要があります。

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

今すぐそれらディセンダーの下部にあるシャドウを拡張します。

[![](skew-images/skewshadowtext3-small.png "ディセンダーの調整、傾斜シャドウ テキスト ページの 3 倍になるスクリーン ショット")](skew-images/skewshadowtext3-large.png#lightbox "ディセンダーの調整、傾斜シャドウ テキスト ページの 3 倍になるスクリーン ショット")


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
