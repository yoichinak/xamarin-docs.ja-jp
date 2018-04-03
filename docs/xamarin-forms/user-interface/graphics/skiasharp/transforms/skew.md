---
title: 傾斜変換
description: 傾斜変換が SkiaSharp で割引グラフィカル オブジェクトを作成する方法を参照してください。
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: charlespetzold
ms.author: chape
ms.date: 03/20/2017
ms.openlocfilehash: fd5601f380e51ec3fb3a6fdcaa491b218390b7f3
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="the-skew-transform"></a>傾斜変換

_傾斜変換が SkiaSharp で割引グラフィカル オブジェクトを作成する方法を参照してください。_

SkiaSharp、傾斜変換は、この図で影などのグラフィカル オブジェクトを傾けます。

![](skew-images/skewexample.png "シャドウ テキストのずれのプログラムから傾斜の例")

傾斜 parallelograms、四角形に変換が歪曲される楕円まだ楕円を描画します。

Xamarin.Forms では、翻訳、スケール、および回転のプロパティを定義はない対応するプロパティ Xamarin.Forms でスキューを。

[ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/System.Single/System.Single/)メソッドの`SKCanvas`ずれの水平方向および垂直方向の 2 つの引数のずれを受け入れます。

```csharp
public void Skew (Single xSkew, Single ySkew)
```

1 秒あたり[ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/SkiaSharp.SKPoint/)メソッドが 1 つの引数を組み合わせる`SKPoint`値。

```csharp
public void Skew (SKPoint skew)
```

ただし、使用することをこれら 2 つのメソッドのいずれかの分離の可能性はありません。

**実験傾斜**– 10 ~ 10 の間には、その範囲の値 ページでの傾斜を試してみることです。 テキスト文字列が 2 から取得した傾きの値と、ページの左上隅に配置されている`Slider`要素。 ここでは、`PaintSurface`ハンドラー、 [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs)クラス。

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

値、`xSkew`引数は正の値のテキストまたは負の値は、左の下をシフトします。 値`ySkew`または負の値に正の値の下のテキストの右側にシフトします。

[![](skew-images/skewexperiment-small.png "傾斜実験 ページのスクリーン ショットをトリプル")](skew-images/skewexperiment-large.png#lightbox "傾斜実験 ページのトリプル スクリーン ショット")

場合`xSkew`の負の値は、`ySkew`結果の回転が拡張することも多少として、Windows の表示を示します。

変換の数式は次のとおりです。

x' = x + xSkew ·y

y' ySkew · を =x + y

たとえば、正`xSkew`値、変換された`x'`値が増えるにつれて`y`が増加します。 傾きの原因です。

ポイント (0, 0) で、左上隅に配置されているし、レンダリングには 100 ピクセル、高さ、幅の三角形 200 ピクセル、 `xSkew` 1.5 では、平行四辺形結果は、次の値。

![](skew-images/skeweffect.png "傾斜変換の四角形への影響")

下端の座標が`y`であるため、100 の値が右に 150 ピクセルをシフトします。

値が 0 以外の`xSkew`または`ySkew`、だけで、ポイント (0, 0) は変わりません。 そのポイントは、傾斜の中心を見なすことができます。 必要がある場合に何がゆがめられるの中心 (これは通常の場合)、あるありません`Skew`メソッドを提供します。 明示的に結合する必要があります`Translate`を指定した呼び出し、`Skew`呼び出します。 傾斜を中央に`px`と`py`、次の呼び出しを行います。

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

複合変換の数式は次のとおりです。

x' = x + xSkew ·(y – py)

y' ySkew · を =(x – px) + y

場合`ySkew`0、0 以外の値の中のみを指定して`xSkew`、し`px`値は使用されません。 値は使用されませんと同様に`ySkew`と`py`です。

快適なこのダイアグラムに角度 α などの傾きの角度として傾斜を指定すると思われる場合があります。

![](skew-images/skewangleeffect.png "傾斜の角度を含む四角形の傾斜変換の結果が示されます")

100 ピクセル垂直方向に 150 ピクセルのシフトの比率は、この例では、その角度のタンジェントの値は、56.3 度です。

XAML ファイル、**傾斜角度実験**ページがに似ていますが、**傾斜の角度**ページの点を除いて、`Slider`範囲に要素を –90 から 90 度。 [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs)分離コード ファイルが、ページ上のテキストを中央揃え、使用して`Translate`をページの中央に傾斜の中心を設定します。 短い`SkewDegrees`コードの下部にあるメソッドが傾けるには値の角度に変換します。

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

角度に近づくと正または負の値の 90 °、タンジェント近づく無限大の場合は、約 80 度またはこれまでの角度は、使用可能です。

[![](skew-images/skewangleexperiment-small.png "傾斜角度実験 ページのスクリーン ショットをトリプル")](skew-images/skewangleexperiment-large.png#lightbox "傾斜角度実験 ページのトリプル スクリーン ショット")

水平方向の負の値の小さいゆがみとして斜投影または斜体のテキストを模倣、**斜投影テキスト**ページを示しています。 [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs)クラスは、その方法を示しています。

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

`TextAlign`プロパティ`SKPaint`に設定されている`Center`です。 任意の変換を行わず、`DrawText`の座標を使用して呼び出す (0, 0) 左上隅にあるベースラインの水平方向に中央揃えのテキストを配置するとします。 `SkewDegrees`水平方向に 20 度ベースラインの基準とした、テキストのずれ。 `Translate`呼び出し、キャンバスの中央にテキストのベースラインの水平方向の中央に移動します。

[![](skew-images/obliquetext-small.png "斜体のテキスト ページのスクリーン ショットをトリプル")](skew-images/obliquetext-large.png#lightbox "斜体のテキスト ページのトリプル スクリーン ショット")

**シャドウ テキストの傾斜**ページが 45 度のずれおよび垂直方向のスケールの組み合わせを使用して、テキストから離れて傾けますテキストの影を作成する方法を示します。 ここで、関連の一部である、`PaintSurface`ハンドラー。

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

影は、表示されている最初、テキスト。

[![](skew-images/skewshadowtext1-small.png "傾斜シャドウ テキスト ページのスクリーン ショットをトリプル")](skew-images/skewshadowtext1-large.png#lightbox "傾斜シャドウ テキスト ページのトリプル スクリーン ショット")

渡される、垂直座標、`DrawText`メソッド ベースラインの基準としたテキストの位置を示します。 傾斜のセンターに使用される同じ垂直方向の座標です。 テキスト文字列にディセンダーが含まれている場合、この手法は使用できません。 たとえば、代用および「影」を「特異な」という単語は、結果の。

[![](skew-images/skewshadowtext2-small.png "トリプル ページのスクリーン ショット、傾斜シャドウ テキスト ディセンダーと代替の言葉で始まる")](skew-images/skewshadowtext2-large.png#lightbox "トリプル ページのスクリーン ショット、傾斜シャドウ テキスト ディセンダーと代替の言葉で始まる")

シャドウとテキストも、ベースラインに揃えられますが、効果はだけが正しく表示します。 これを修正するには、テキストの境界を取得する必要があります。

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

`Translate`呼び出しが、ディセンダーの高さを調整する必要があります。

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

今すぐこれらディセンダーの下端から影を拡張します。

[![](skew-images/skewshadowtext3-small.png "トリプル ページのスクリーン ショット、傾斜シャドウ テキストが、ディセンダー用の調整")](skew-images/skewshadowtext3-large.png#lightbox "トリプル ページのスクリーン ショット、傾斜シャドウ テキストが、ディセンダー用の調整")


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
