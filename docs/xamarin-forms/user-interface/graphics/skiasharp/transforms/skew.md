---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 207b16f062a5c2137ac5fc3c21775d2486fda57d
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135864"
---
# <a name="the-skew-transform"></a>傾斜変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_傾斜変換による SkiaSharp での傾いたグラフオブジェクトの作成方法を確認する_

SkiaSharp では、傾斜変換は、次の図の影などのグラフィカルオブジェクトを傾けます。

![](skew-images/skewexample.png "An example of skewing from the Skew Shadow Text program")

傾斜によって四角形が平行四辺形に変わりますが、傾斜した楕円は楕円のままです。

Xamarin.Formsでは、平行移動、拡大縮小、および回転のプロパティが定義されていますが、傾斜に対応するプロパティはありません Xamarin.Forms 。

[`Skew`](xref:SkiaSharp.SKCanvas.Skew(System.Single,System.Single))のメソッドは、 `SKCanvas` 水平方向の傾斜と垂直方向の傾斜の2つの引数を受け取ります。

```csharp
public void Skew (Single xSkew, Single ySkew)
```

2番目のメソッドは、 [`Skew`](xref:SkiaSharp.SKCanvas.Skew(SkiaSharp.SKPoint)) これらの引数を1つの値に結合し `SKPoint` ます。

```csharp
public void Skew (SKPoint skew)
```

ただし、これら2つの方法のいずれかを単独で使用することはほとんどありません。

[**傾斜実験**] ページでは、-10 ~ 10 の範囲の傾斜値を試すことができます。 テキスト文字列は、ページの左上隅に配置されます。傾斜値は2つの要素から取得され `Slider` ます。 クラスのハンドラーを次に示し `PaintSurface` [`SkewExperimentPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) ます。

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

引数の値 `xSkew` は、正の値の場合はテキストの右側、負の値の場合は左にシフトします。 正の `ySkew` 値の場合はテキストの右側にシフトし、負の値の場合は [上] の値を指定します。

[![](skew-images/skewexperiment-small.png "Triple screenshot of the Skew Experiment page")](skew-images/skewexperiment-large.png#lightbox "Triple screenshot of the Skew Experiment page")

値が負の値の場合、 `xSkew` `ySkew` 結果はローテーションになりますが、少しだけ拡大縮小されます。

変換式は次のとおりです。

x ' = x + xSkew \前年

y ' = ySkew ·x + y

たとえば、正の値の場合、 `xSkew` 変換後の `x'` 値は `y` 増加します。 これで、傾きが発生します。

三角形200ピクセルと高さ100ピクセルがポイント (0, 0) の左上隅に配置され、値1.5 でレンダリングされる場合 `xSkew` 、次の平行四辺形が生成されます。

![](skew-images/skeweffect.png "The effect of the skew transform on a rectangle")

下端の座標には `y` 100 の値があるため、150ピクセルを右にシフトします。

またはの0以外の値について `xSkew` は、 `ySkew` ポイント (0, 0) だけが変わりません。 この点は、傾斜の中心と考えることができます。 傾斜の中心を別のものにする必要がある場合 (通常はこれが該当します)、それを提供するメソッドはありません `Skew` 。 呼び出しで呼び出しを明示的に結合する必要があり `Translate` `Skew` ます。 とで傾斜を中央に配置するには、 `px` `py` 次の呼び出しを行います。

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

複合変換の数式は次のとおりです。

x ' = x + xSkew \(y – .py)

y ' = ySkew ·(x – px) + y

`ySkew`が0の場合、 `px` 値は使用されません。 値は無関係であり、とでも同様です `ySkew` `py` 。

傾斜の角度として傾斜を指定する方が、次の図のような角度をαます。

![](skew-images/skewangleeffect.png "The effect of the skew transform on a rectangle with a skewing angle indicated")

100ピクセルの垂直方向への150ピクセルシフトの比率は、その角度のタンジェント (この例では56.3 度) です。

**傾斜角度実験**ページの XAML ファイルは、[**傾斜角度**] ページと似ていますが、 `Slider` 要素の範囲が-90 °から90度になっている点が異なります。 [`SkewAngleExperiment`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs)分離コードファイルは、ページ上にテキストを配置し、を使用して、 `Translate` ページの中央に傾斜の中心を設定します。 `SkewDegrees`コードの一番下にある短いメソッドは、角度を傾斜値に変換します。

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

角度が正または負の90度に近づくにつれて、タンジェントは無限に近づいていますが、約80度までの角度を使用できます。

[![](skew-images/skewangleexperiment-small.png "Triple screenshot of the Skew Angle Experiment page")](skew-images/skewangleexperiment-large.png#lightbox "Triple screenshot of the Skew Angle Experiment page")

小さい負の水平方向の傾斜は、斜投影の**テキスト**ページで示すように、斜投影または斜体のテキストを模倣することができます。 クラスは、 [`ObliqueTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) その方法を示しています。

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

`TextAlign`のプロパティ `SKPaint` がに設定されて `Center` います。 変換を行わない場合、 `DrawText` 座標 (0, 0) を指定した呼び出しでは、ベースラインの水平方向の中心が左上隅に配置されます。 は、 `SkewDegrees` ベースラインに対して相対的にテキストを20度水平に傾斜させます。 この呼び出しにより、 `Translate` テキストのベースラインの水平方向の中心がキャンバスの中央に移動します。

[![](skew-images/obliquetext-small.png "Triple screenshot of the Oblique Text page")](skew-images/obliquetext-large.png#lightbox "Triple screenshot of the Oblique Text page")

[**傾斜の影のテキスト**] ページでは、45度の傾斜と垂直方向のスケールの組み合わせを使用して、テキストから離れたテキストの影を作成する方法を示します。 ハンドラーの適切な部分を次に示し `PaintSurface` ます。

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

最初に影が表示され、次にテキストが表示されます。

[![](skew-images/skewshadowtext1-small.png "Triple screenshot of the Skew Shadow Text page")](skew-images/skewshadowtext1-large.png#lightbox "Triple screenshot of the Skew Shadow Text page")

メソッドに渡される垂直座標は、ベースラインを基準とした `DrawText` テキストの位置を示します。 これは、傾斜の中心に使用される垂直方向の座標と同じです。 テキスト文字列にディセンダーが含まれている場合、この手法は機能しません。 たとえば、"Shadow" に対して "奇妙" という語を使用すると、結果が次のようになります。

[![](skew-images/skewshadowtext2-small.png "Triple screenshot of the Skew Shadow Text page with an alternative word with descenders")](skew-images/skewshadowtext2-large.png#lightbox "Triple screenshot of the Skew Shadow Text page with an alternative word with descenders")

シャドウとテキストはベースラインでまだアラインされていますが、効果が正しくないようです。 この問題を解決するには、テキストの境界を取得する必要があります。

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

呼び出しは、 `Translate` ディセンダーの高さによって調整する必要があります。

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

これで、シャドウは、これらのディセンダーの下部から拡張されます。

[![](skew-images/skewshadowtext3-small.png "Triple screenshot of the Skew Shadow Text page with adjustments for descenders")](skew-images/skewshadowtext3-large.png#lightbox "Triple screenshot of the Skew Shadow Text page with adjustments for descenders")

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
