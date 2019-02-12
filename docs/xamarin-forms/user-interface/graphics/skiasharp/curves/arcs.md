---
title: 円弧を描画する 3 つの方法
description: この記事では、SkiaSharp を使用して、次の 3 つの異なる方法で円弧を定義する方法について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2017
ms.openlocfilehash: 020afef6b2eb3743fd17118b2922bac4d4c32239
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233992"
---
# <a name="three-ways-to-draw-an-arc"></a>円弧を描画する 3 つの方法

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_SkiaSharp を使用して、次の 3 つの異なる方法で円弧を定義する方法について説明します_

この無限大記号の角の丸い部分などの楕円の円周上の曲線を円弧には。

![](arcs-images/arcsample.png "無限大記号")

その定義の簡単なコード、円弧の描画をすべてのニーズを満たす関数を定義する方法はありませんし、そのため、円弧を描画する最善の方法のグラフィックス システム間で意見の一致なし。このため、`SKPath`クラスは制限されません自体アプローチの 1 つだけにします。

`SKPath` 定義、 [ `AddArc` ](xref:SkiaSharp.SKPath.AddArc*)メソッドは、5 つの異なる[ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo*)メソッド、および 2 つの相対[ `RArcTo` ](xref:SkiaSharp.SKPath.RArcTo*)メソッド。 これらのメソッドは、3 つのまったく異なるアプローチを表す円弧を指定する 3 つのカテゴリに分類されます。これ 1 つを使用して、円弧とこの円弧を描画している他のグラフィックスを使用して収める方法を定義する情報によって異なります。

## <a name="the-angle-arc"></a>円弧の角度

円弧を描画する角度のアーク アプローチでは、楕円の外接する四角形を指定することが必要です。 この楕円の円周上、円弧は円弧とその長さの先頭を示す、楕円の中心からの角度で示されます。 2 つの方法では、円弧の角度を描画します。 これらは、 [ `AddArc` ](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single))メソッドと[ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKRect,System.Single,System.Single,System.Boolean))メソッド。

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

これらのメソッドは、Android と同じ[ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/)と[ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/)メソッド。 IOS [ `AddArc` ](xref:CoreGraphics.CGPath.AddArc(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.Boolean))メソッドと似ていますが、円の円周を円弧に限定されますではなく楕円に一般化します。

両方の方法が始まり、`SKRect`場所と楕円のサイズを定義する値。

![](arcs-images/anglearcoval.png "円弧の角度を開始する楕円")

円弧は、この楕円の円周の一部です。

`startAngle`引数が右側に楕円の中心から描画された水平線からの角度の時計回りの角度。 `sweepAngle`引数が関連して、`startAngle`します。 ここでは`startAngle`と`sweepAngle`60 度の 100 度をそれぞれ値します。

![](arcs-images/anglearcangles.png "円弧の角度を定義する角度")

開始角度で、円弧が開始されます。 掃引角度の長さが適用されます。 円弧が赤のとおりです。

![](arcs-images/anglearchighlight.png "強調表示されている角度円弧")

曲線のパスに追加された、`AddArc`または`ArcTo`メソッドは、楕円の円周の部分だけです。

![](arcs-images/anglearc.png "単独で角度円弧")

`startAngle`または`sweepAngle`引数を負にすることができます。円弧が時計回りの正の値の`sweepAngle`反時計回りの負の値にします。

ただし、`AddArc`は*いない*クローズドの輪郭を定義します。 呼び出す場合`LineTo`後`AddArc`、内のポイントに円弧の終端から線を描画、`LineTo`メソッドと同じことが当てはまります`ArcTo`します。

`AddArc` 新しい輪郭を開始してへの呼び出しと同等の機能が自動的に`ArcTo`の最後の引数と`true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

最後の引数が呼び出される`forceMoveTo`、効果的にし、`MoveTo`円弧の開始時に呼び出します。新しい輪郭を開始するとします。 つまりケースではなく、最後の引数と`false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

このバージョンの`ArcTo`円弧の始点を現在の位置から線を描画します。これは、円弧が大きい輪郭の途中でどこかにできることを意味します。

**円弧の角度**ページを使用して、開始日を指定して、角度をスイープする 2 つのスライダーを使用できます。 XAML ファイルでは、2 つのインスタンス化します`Slider`要素と`SKCanvasView`します。 `PaintCanvas`ハンドラーで、 [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs)ファイル楕円と 2 つを使用して、円弧の両方を描画します`SKPaint`フィールドとして定義されているオブジェクト。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
    float startAngle = (float)startAngleSlider.Value;
    float sweepAngle = (float)sweepAngleSlider.Value;

    canvas.DrawOval(rect, outlinePaint);

    using (SKPath path = new SKPath())
    {
        path.AddArc(rect, startAngle, sweepAngle);
        canvas.DrawPath(path, arcPaint);
    }
}
```

ご覧のように、開始角度および掃引角度の両方を負の値で実行できます。

[![](arcs-images/anglearc-small.png "円弧の角度 ページのスクリーン ショットをトリプル")](arcs-images/anglearc-large.png#lightbox "円弧の角度 ページの 3 倍になるスクリーン ショット")

円弧を生成するには、この方法は簡単、アルゴリズムと円弧を記述するパラメーターの式を派生するは簡単です。楕円、および開始および掃引角度の場所とサイズを知ること、開始と円弧の始点と終点できますを計算する単純な三角関数の使用します。

x 楕円を = です。MidX + (楕円。幅/2) * cos(angle)

y 楕円を = です。MidY + (楕円。高さ/2) * sin(angle)

`angle`値は、いずれかの`startAngle`または`startAngle + sweepAngle`します。

円弧を定義する 2 つの角度の使用は、円弧を描画する、円グラフを作成するの angular の長さを認識する場合に最適です。 **分割円グラフ** ページでは、この例を示します。 [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs)クラスでは、内部のクラスを使用して、いくつか用意されたデータと色を定義します。

```csharp
class ChartData
{
    public ChartData(int value, SKColor color)
    {
        Value = value;
        Color = color;
    }

    public int Value { private set; get; }

    public SKColor Color { private set; get; }
}

ChartData[] chartData =
{
    new ChartData(45, SKColors.Red),
    new ChartData(13, SKColors.Green),
    new ChartData(27, SKColors.Blue),
    new ChartData(19, SKColors.Magenta),
    new ChartData(40, SKColors.Cyan),
    new ChartData(22, SKColors.Brown),
    new ChartData(29, SKColors.Gray)
};

```

`PaintSurface`ハンドラーがまずを計算する項目をループ処理、`totalValues`数。 合計の割合として各項目のサイズを決定し、角度に変換することできます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int totalValues = 0;

    foreach (ChartData item in chartData)
    {
        totalValues += item.Value;
    }

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float explodeOffset = 50;
    float radius = Math.Min(info.Width / 2, info.Height / 2) - 2 * explodeOffset;
    SKRect rect = new SKRect(center.X - radius, center.Y - radius,
                             center.X + radius, center.Y + radius);

    float startAngle = 0;

    foreach (ChartData item in chartData)
    {
        float sweepAngle = 360f * item.Value / totalValues;

        using (SKPath path = new SKPath())
        using (SKPaint fillPaint = new SKPaint())
        using (SKPaint outlinePaint = new SKPaint())
        {
            path.MoveTo(center);
            path.ArcTo(rect, startAngle, sweepAngle, false);
            path.Close();

            fillPaint.Style = SKPaintStyle.Fill;
            fillPaint.Color = item.Color;

            outlinePaint.Style = SKPaintStyle.Stroke;
            outlinePaint.StrokeWidth = 5;
            outlinePaint.Color = SKColors.Black;

            // Calculate "explode" transform
            float angle = startAngle + 0.5f * sweepAngle;
            float x = explodeOffset * (float)Math.Cos(Math.PI * angle / 180);
            float y = explodeOffset * (float)Math.Sin(Math.PI * angle / 180);

            canvas.Save();
            canvas.Translate(x, y);

            // Fill and stroke the path
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, outlinePaint);
            canvas.Restore();
        }

        startAngle += sweepAngle;
    }
}
```

新しい`SKPath`各円スライスのオブジェクトを作成します。 パス、センターからの行から成る、`ArcTo`から中央の結果に戻る、円弧と別の行を描画するために、`Close`を呼び出します。 このプログラムに移動してすべて中央から 50 ピクセルで「分割」円のスライスを表示します。 そのタスクには、各スライスの掃引角度の中心の方向ベクトルが必要です。

[![](arcs-images/explodedpiechart-small.png "分割円グラフの選択 ページのスクリーン ショットをトリプル")](arcs-images/explodedpiechart-large.png#lightbox "分割円グラフの選択 ページの 3 倍になるスクリーン ショット")

どのように見えるなし「展開」を表示するには、単には、コメント アウト、`Translate`呼び出し。

[![](arcs-images/explodedpiechartunexploded-small.png "展開なしの分割円グラフ ページのスクリーン ショットをトリプル")](arcs-images/explodedpiechartunexploded-large.png#lightbox "展開なしの分割円グラフ ページの 3 倍になるスクリーン ショット")

## <a name="the-tangent-arc"></a>接線の円弧

2 番目の型でサポートされている円弧の`SKPath`は、*アーク タンジェント*円弧が接続されている 2 つの直線をタンジェントである円の円周であるためにと呼ばれます。

接線の円弧がへの呼び出しで、パスに追加、 [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single))メソッドの 2 つ`SKPoint`パラメーター、または[ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,System.Single,System.Single))個別にオーバー ロード`Single`パラメーターをポイント:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

これは、`ArcTo`メソッドは、PostScript [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) (ページ 532) 関数および iOS は、 [ `AddArcToPoint` ](xref:CoreGraphics.CGPath.AddArcToPoint(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat))メソッド。

`ArcTo`メソッドが 3 つのポイントが含まれます。

- 現在のポイント、輪郭または点 (0, 0) の場合`MoveTo`が呼び出されていません。
- 最初のポイント引数、`ArcTo`というメソッドを*ポイント上にあります。*
- 2 番目のポイント引数`ArcTo`という、*終点*:

![](arcs-images/tangentarcthreepoints.png "接線の円弧の開始を 3 つのポイント")

これら 3 つのポイントは、接続された 2 つの行を定義します。

![](arcs-images/tangentarcconnectinglines.png "接線の円弧の 3 つの点を結ぶ線")

3 つのポイントが同一線上にある場合&mdash;は、同一直線上に存在する場合&mdash;円弧は描画されません。

`ArcTo`メソッドも含まれています、`radius`パラメーター。 これには、円の半径を定義します。

![](arcs-images/tangentarccircle.png "接線の円弧の円")

楕円の接線の円弧が汎用化されていません。

2 つの行を満たす任意の角度で場合、両方の行に接するにある円はそれらの行の間で挿入できます。

![](arcs-images/tangentarctangentcircle.png "2 行の間のアーク タンジェント円")

指定されたポイントのいずれかに追加される曲線に接触しない、`ArcTo`メソッド。 現在のポイントから最初の接点と赤で示される 2 番目接点で終わる円弧を直線で構成されます。

![](arcs-images/tangentarchighlight.png "2 行の間の強調表示されている接線円弧")

最終的な直線および曲線に追加される円弧を次に示します。

![](arcs-images/tangentarc.png "2 行の間の強調表示されている接線円弧")

輪郭は、2 つ目の接点から続行することができます。

**タンジェント円弧** ページでは、正接の円弧をテストすることができます。これは、最初から派生するいくつかのページの[ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs)、便利ないくつかを定義する`SKPaint`オブジェクトし、実行`TouchPoint`処理します。

```csharp
public class InteractivePage : ContentPage
{
    protected SKCanvasView baseCanvasView;
    protected TouchPoint[] touchPoints;

    protected SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3
    };

    protected SKPaint redStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 15
    };

    protected SKPaint dottedStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    };

    protected void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = baseCanvasView.CanvasSize.Width / (float)baseCanvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            baseCanvasView.InvalidateSurface();
        }
    }
}
```

`TangentArcPage` クラスは `InteractivePage` から派生したものです。 コンス トラクター、 [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs)ファイルがインスタンス化と初期化を担当、`touchPoints`配列、および設定`baseCanvasView`(で`InteractivePage`) に、`SKCanvasView`でオブジェクトをインスタンス化、 [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml)ファイル。

```csharp
public partial class TangentArcPage : InteractivePage
{
    public TangentArcPage()
    {
        touchPoints = new TouchPoint[3];

        for (int i = 0; i < 3; i++)
        {
            TouchPoint touchPoint = new TouchPoint
            {
                Center = new SKPoint(i == 0 ? 100 : 500,
                                     i != 2 ? 100 : 500)
            };
            touchPoints[i] = touchPoint;
        }

        InitializeComponent();

        baseCanvasView = canvasView;
        radiusSlider.Value = 100;
    }

    void sliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

`PaintSurface`ハンドラーを使用して、`ArcTo`タッチ ポイントに基づいて、円弧を描画するメソッドと`Slider`もアルゴリズムに角度が基づいている円を計算しますが。

```csharp
public partial class TangentArcPage : InteractivePage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw the two lines that meet at an angle
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.LineTo(touchPoints[1].Center);
            path.LineTo(touchPoints[2].Center);
            canvas.DrawPath(path, dottedStrokePaint);
        }

        // Draw the circle that the arc wraps around
        float radius = (float)radiusSlider.Value;

        SKPoint v1 = Normalize(touchPoints[0].Center - touchPoints[1].Center);
        SKPoint v2 = Normalize(touchPoints[2].Center - touchPoints[1].Center);

        double dotProduct = v1.X * v2.X + v1.Y * v2.Y;
        double angleBetween = Math.Acos(dotProduct);
        float hypotenuse = radius / (float)Math.Sin(angleBetween / 2);
        SKPoint vMid = Normalize(new SKPoint((v1.X + v2.X) / 2, (v1.Y + v2.Y) / 2));
        SKPoint center = new SKPoint(touchPoints[1].Center.X + vMid.X * hypotenuse,
                                     touchPoints[1].Center.Y + vMid.Y * hypotenuse);

        canvas.DrawCircle(center.X, center.Y, radius, this.strokePaint);

        // Draw the tangent arc
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.ArcTo(touchPoints[1].Center, touchPoints[2].Center, radius);
            canvas.DrawPath(path, redStrokePaint);
        }

        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }

    // Vector methods
    SKPoint Normalize(SKPoint v)
    {
        float magnitude = Magnitude(v);
        return new SKPoint(v.X / magnitude, v.Y / magnitude);
    }

    float Magnitude(SKPoint v)
    {
        return (float)Math.Sqrt(v.X * v.X + v.Y * v.Y);
    }
}
```

ここでは、**タンジェント円弧**実行されているページ。

[![](arcs-images/tangentarc-small.png "タンジェント円弧ページのスクリーン ショットをトリプル")](arcs-images/tangentarc-large.png#lightbox "タンジェント円弧ページの 3 倍になるスクリーン ショット")

接線の円弧は、角が丸い角の丸い四角形などの作成に適しています。 `SKPath`が既に含まれて、`AddRoundedRect`メソッド、**丸め七角形**ページが使用する方法を示します`ArcTo`7 辺の多角形の角を丸める場合。 (コードは、通常の多角形の一般化。)

`PaintSurface`のハンドラー、 [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs)クラスには、1 つ含まれる`for`ループ、七角形およびこれらから 7 つの辺の中間点を計算する 2 つ目の 7 つの頂点の座標を計算するには頂点。 これらの中間点を使用して、パスを作成します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float cornerRadius = 100;
    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPoint[] vertices = new SKPoint[numVertices];
    SKPoint[] midPoints = new SKPoint[numVertices];

    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    // Coordinates of the midpoints of the sides connecting the vertices
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        int prevVertex = (vertex + numVertices - 1) % numVertices;
        midPoints[vertex] = new SKPoint((vertices[prevVertex].X + vertices[vertex].X) / 2,
                                        (vertices[prevVertex].Y + vertices[vertex].Y) / 2);
    }

    // Create the path
    using (SKPath path = new SKPath())
    {
        // Begin at the first midpoint
        path.MoveTo(midPoints[0]);

        for (int vertex = 0; vertex < numVertices; vertex++)
        {
            SKPoint nextMidPoint = midPoints[(vertex + 1) % numVertices];

            // Draws a line from the current point, and then the arc
            path.ArcTo(vertices[vertex], nextMidPoint, cornerRadius);

            // Connect the arc with the next midpoint
            path.LineTo(nextMidPoint);
        }
        path.Close();

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);
        }
    }
}

```

実行中のプログラムを次に示します。

[![](arcs-images/roundedheptagon-small.png "丸め七角形ページのスクリーン ショットをトリプル")](arcs-images/roundedheptagon-large.png#lightbox "丸め七角形ページの 3 倍になるスクリーン ショット")

## <a name="the-elliptical-arc"></a>楕円の円弧

楕円の円弧がへの呼び出しで、パスに追加、 [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,SkiaSharp.SKPoint))メソッドを持つ 2 つ`SKPoint`パラメーター、または[ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,System.Single,System.Single))個別 X と Y 座標をオーバー ロードします。

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

楕円の円弧が矛盾、[楕円の円弧](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands)スケーラブル ベクター グラフィックス (SVG) とユニバーサル Windows プラットフォームに含まれる[ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/)クラス。

これら`ArcTo`メソッドは、輪郭の現在のポイントは、2 つのポイント間の円弧を描画し、最後のパラメーターを`ArcTo`メソッド (、`xy`パラメーターまたは個別の`x`と`y`パラメーター)。

![](arcs-images/ellipticalarcpoints.png "楕円の円弧が定義されている 2 つのポイント")

最初のポイント パラメーター、`ArcTo`メソッド (`r`、または`rx`と`ry`) ポイントではありませんが、代わりに、楕円の水平および垂直方向の半径を指定します

![](arcs-images/ellipticalarcellipse.png "楕円の円弧を定義する楕円")

`xAxisRotate`パラメーターはこの楕円を回転する時計回りの角度の数値。

![](arcs-images/ellipticalarctiltedellipse.png "楕円の円弧を定義する楕円を傾斜しています。")

傾斜しているこの楕円は、2 つの点と接触する配置されているし、ポイントが異なる 2 つの円弧に接続されます。

![](arcs-images/ellipticalarcellipse1.png "楕円の円弧の最初のセット")

2 つの方法でこれら 2 つの円弧を識別できます。最上位の円弧が下部の円弧を超えると、下部円弧が反時計回りの方向に描画中に時計回りの方向に最上位の円弧が描画されるように左から右に円弧の描画。

別の方法で 2 つのポイント間の楕円の適合することです。

![](arcs-images/ellipticalarcellipse2.png "楕円の円弧の 2 番目のセット")

ようになりましたが、時計回りに描画される一番上に小さい円弧あり描画される下部にある大きい円弧反時計回りにあります。

これら 2 つのポイントは、合計で 4 つの方法で傾いている楕円で定義された円弧によってそのため接続できます。

![](arcs-images/ellipticalarccolors.png "すべての 4 つの楕円の円弧")

これら 4 つの円弧が 4 つの組み合わせによって識別されます、 [ `SKPathArcSize` ](xref:SkiaSharp.SKPathArcSize)と[ `SKPathDirection` ](xref:SkiaSharp.SKPathDirection)列挙体の型引数、`ArcTo`メソッド。

- 赤:SKPathArcSize.Large と SKPathDirection.Clockwise
- 緑:SKPathArcSize.Small と SKPathDirection.Clockwise
- 青。SKPathArcSize.Small と SKPathDirection.CounterClockwise
- マゼンタ:SKPathArcSize.Large と SKPathDirection.CounterClockwise

傾斜している省略記号がない場合、2 つの点の間に合わせて十分な大きさが十分な大きさになるまで一様拡大縮小し。 2 つの一意な円弧は、その場合は 2 つのポイントを接続します。 これらを区別することができます、`SKPathDirection`パラメーター。

円弧を定義するには、この方法は、初めて直面するときに複雑なサウンドが回転の楕円の円弧を定義できます。 唯一の方法であり、多くの場合、最も簡単な方法輪郭の他の部分と円弧を統合する必要がある場合。

**楕円の円弧** ページでは、2 つの点と、サイズと楕円の回転を対話的に設定できます。 `EllipticalArcPage`クラスから派生`InteractivePage`と`PaintSurface`ハンドラーで、 [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs)分離コード ファイルが 4 つの円弧を描画します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        int colorIndex = 0;
        SKPoint ellipseSize = new SKPoint((float)xRadiusSlider.Value,
                                          (float)yRadiusSlider.Value);
        float rotation = (float)rotationSlider.Value;

        foreach (SKPathArcSize arcSize in Enum.GetValues(typeof(SKPathArcSize)))
            foreach (SKPathDirection direction in Enum.GetValues(typeof(SKPathDirection)))
            {
                path.MoveTo(touchPoints[0].Center);
                path.ArcTo(ellipseSize, rotation,
                           arcSize, direction,
                           touchPoints[1].Center);

                strokePaint.Color = colors[colorIndex++];
                canvas.DrawPath(path, strokePaint);
                path.Reset();
            }
    }

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}

```

ここでは、実行します。

[![](arcs-images/ellipticalarc-small.png "楕円の円弧のページのスクリーン ショットをトリプル")](arcs-images/ellipticalarc-large.png#lightbox "楕円の円弧のページの 3 倍になるスクリーン ショット")

**円弧無限大** ページでは、楕円の円弧を使用して、無限大記号に描画します。 無限大記号は、2 つの 100 単位で区切られた 100 単位の半径の円に基づいています。

![](arcs-images/infinitycircles.png "2 つの円")

他の何らかの関わり 2 つの行では、両方の円の接線です。

![](arcs-images/infinitycircleslines.png "接線の 2 つの円")

無限大記号は、これらのサークルと 2 つの行の部分の組み合わせです。 楕円の円弧を無限大記号を描画するために使用するには、2 つの行の円に正接がある座標を決定する必要があります。

円のいずれかで適切な四角形を作成します。

![](arcs-images/infinitytriangle.png "2 つの円の接線と埋め込まれた円")

円の半径が 100 の単位と三角形の斜辺は 150 ユニットは、角度 α は 150、または 41.8 度を分割する 100 のアークサイン (逆正弦)。 三角形の他の辺の長さは、150 回 41.8 の角度のコサインまたは 112 で、ピタゴラスの定理を計算することもできます。

接線の点の座標にはこの情報を使用して計算できます。

x = 112・cos(41.8) = 83

y = 112・sin(41.8) = 75

4 つの接点は 100 の円の半径のあるポイント (0, 0) で中央揃え、無限大記号を描画するために必要なことです。

![](arcs-images/infinitycoordinates.png "接線と座標の 2 つの円")

`PaintSurface`ハンドラーで、 [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs)クラスは、無限大記号を配置できるように、(0, 0) ポイントが、ページの中央に配置されているし、画面サイズへのパスをスケールします。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.LineTo(83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.CounterClockwise, 83, -75);
        path.LineTo(-83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.Clockwise, -83, -75);
        path.Close();

        // Use path.TightBounds for coordinates without control points
        SKRect pathBounds = path.Bounds;

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / pathBounds.Width,
                              info.Height / pathBounds.Height));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 5;

            canvas.DrawPath(path, paint);
        }
    }
}
```

コードを使用して、`Bounds`プロパティの`SKPath`キャンバスのサイズに拡大する無限大サインの大きさを決定します。

[![](arcs-images/arcinfinity-small.png "円弧の無限大のページのスクリーン ショットをトリプル")](arcs-images/arcinfinity-large.png#lightbox "円弧無限大ページの 3 倍になるスクリーン ショット")

結果を提案していますが、少し小さいよう、`Bounds`プロパティの`SKPath`パスよりも大きいサイズを報告します。

内部的には、複数の 2 次ベジエ曲線を使用して、円弧が Skia の近似値を求めます。 これらの曲線 (次のセクションで表示されます) が含まれている曲線の描画方法を制御がレンダリングされた曲線の一部ではないの制御点。 `Bounds`プロパティには、それらのコントロール ポイントが含まれています。

厳密な適合を取得する、`TightBounds`プロパティで、管理ポイントは含まれません。 使用して、横向きモードで実行されているプログラムを次に示します、`TightBounds`パスの範囲を取得するプロパティ。

[![](arcs-images/arcinfinitytightbounds-small.png "緊密な境界を持つ弧無限大ページのスクリーン ショットをトリプル")](arcs-images/arcinfinitytightbounds-large.png#lightbox "緊密な境界を持つ弧無限大ページの 3 倍になるスクリーン ショット")

弧と直線間の接続は、滑らかな数学的には、直線を円弧から変更を少し突然に見えるかもしれません。 次の記事でより良い無限大記号が表示される[**ベジエ曲線の型を次の 3 つ**](beziers.md)します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
