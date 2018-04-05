---
title: 円弧を描画する 3 つの方法
description: SkiaSharp を使用して、次の 3 つの異なる方法で円弧を定義する方法をについてください。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: charlespetzold
ms.author: chape
ms.date: 05/10/2017
ms.openlocfilehash: c6fd0f905aceb9dddc4047abc6ad2722adf2d8e9
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/05/2018
---
# <a name="three-ways-to-draw-an-arc"></a>円弧を描画する 3 つの方法

_SkiaSharp を使用して、次の 3 つの異なる方法で円弧を定義する方法をについてください。_

この無限大記号の角の丸い部分などの楕円の円周の曲線を円弧には。

![](arcs-images/arcsample.png "無限大記号")

定義のわかりやすいようにに関係なくすべてのニーズを満たす円弧を描画関数を定義する方法はありませんし、そのため、円弧を描画する最善の方法のグラフィックス システム間で合意しません。このため、`SKPath`クラスは、制限されません自体方法の 1 つだけにします。

`SKPath` 定義、`AddArc`メソッドは、5 つの異なる`ArcTo`メソッド、および 2 つの相対`RArcTo`メソッドです。 これらのメソッドは、次の 3 つの大きく異なるアプローチを円弧を指定することを表す 3 つのカテゴリに分類されます。どちらを使用は、円弧と描画している他のグラフィックスを使用してこの円弧の動作の定義に使用できる情報とは異なります。

## <a name="the-angle-arc"></a>角度円弧

円弧を描画するための角度円弧アプローチでは、楕円の外接する四角形を指定することが必要です。 楕円の円弧とその長さの開始を行うの中心からの角度で、この楕円の円周の円弧が示されます。 2 つの方法では、角度の円弧を描画します。 これらは、 [ `AddArc` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/)メソッドおよび[ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKRect/System.Single/System.Single/System.Boolean/)メソッド。

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

これらのメソッドは、Android と同じ[ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/)と[ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/)メソッドです。 IOS [ `AddArc` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArc/p/System.Boolean/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/)メソッドと似ていますが、円の円周を円弧に制限されてではなく楕円を一般化します。

どちらの方法が始まる、`SKRect`場所と楕円のサイズの両方を定義する値。

![](arcs-images/anglearcoval.png "開始角度円弧楕円")

弧は、この楕円の円周の一部です。

`startAngle`引数は、楕円の中心から右に描画される線で水平方向の基準とした角度の時計回りの角度。 `sweepAngle`を基準には、引数、`startAngle`です。 ここでは、`startAngle`と`sweepAngle`と 100 の 60 度の値それぞれ。

![](arcs-images/anglearcangles.png "円弧を角度を定義する角度")

弧は、開始角度から開始されます。 長さは、掃引角度によって拘束されます。

![](arcs-images/anglearchighlight.png "強調表示されている角度円弧")

使用してパスに追加された、曲線、`AddArc`または`ArcTo`メソッドは赤で示される、楕円の円周の部分だけで。

![](arcs-images/anglearc.png "自体によって角度円弧")

`startAngle`または`sweepAngle`引数を負の値にすることができます: 円弧は正の値の時計回り`sweepAngle`反時計回りの負の値にします。

ただし、`AddArc`は*いない*閉じた輪郭を定義します。 呼び出す場合`LineTo`後`AddArc`、内の点を円弧の終点から線を描画、`LineTo`メソッド、および同じことが当てはまります`ArcTo`です。

`AddArc` 新しい輪郭を開始してへの呼び出しに相当する機能が自動的に`ArcTo`の最終的な引数を持つ`true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

最後の引数が呼び出される`forceMoveTo`、効果的になります、`MoveTo`円弧の始点で呼び出します。新しい輪郭を開始するとします。 Case ではなくの最後の引数を持つ`false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

このバージョンの`ArcTo`円弧の始点までの現在位置から線を描画します。つまり円弧をより大きな輪郭の途中でどこかにあることができます。

**角度円弧**ページを使用して、2 つのスライダーを使用して、開始時刻を指定し、掃引角度にできます。 XAML ファイルでは、2 つをインスタンス化`Slider`要素および`SKCanvasView`です。 `PaintCanvas`ハンドラー、 [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs)ファイルは、楕円と円弧を使用して 2 つの両方を描画`SKPaint`フィールドとして定義されているオブジェクト。

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

[![](arcs-images/anglearc-small.png "角度円弧ページのスクリーン ショットをトリプル")](arcs-images/anglearc-large.png#lightbox "角度円弧ページのトリプル スクリーン ショット")

円弧を生成するには、このアプローチはアルゴリズムによって最も単純で、かつ簡単に弧を記述するパラメーターの式を取得します。楕円、および開始および掃引角度の位置とサイズを理解していれば、始点と円弧の終点計算できます単純な三角関数の使用します。

x 楕円を = です。MidX + (oval です。幅/2) * cos(angle)

y 楕円を = です。MidY + (oval です。高さ/2) * sin(angle)

`angle`いずれかの値は`startAngle`または`startAngle + sweepAngle`です。

円弧を定義する 2 つの角度の使用は、円弧を描くには、たとえば、円グラフを作成するの角度の長さがわかっている場合に最適です。 **分割円グラフ** ページでは、この動作について説明します。 [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs)クラスは、内部クラスを使用していくつか用意されたデータと色を定義します。

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

`PaintSurface`ハンドラーはまずを計算する項目をループ処理、`totalValues`数。 その一覧から、全体に占める割合として各項目のサイズを決定して角度に変換します。

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

新しい`SKPath`各円スライスのオブジェクトを作成します。 パスは、センターから行を`ArcTo`から中央の結果を戻すに向けて、円弧と別の行を描画する、`Close`呼び出します。 このプログラムに移動してすべてセンターから 50 ピクセル「分割」円スライスを表示します。 そのタスクには、各スライスの掃引角度の中心の方向にベクターが必要です。

[![](arcs-images/explodedpiechart-small.png "分割円グラフ ページのスクリーン ショットをトリプル")](arcs-images/explodedpiechart-large.png#lightbox "分割円グラフ ページのトリプル スクリーン ショット")

見たなし「急増」を表示するには、単には、コメント アウト、`Translate`呼び出し。

[![](arcs-images/explodedpiechartunexploded-small.png "トリプル ページのスクリーン ショット、分割円グラフ、爆発せず")](arcs-images/explodedpiechartunexploded-large.png#lightbox "トリプル ページのスクリーン ショット、分割円グラフ、爆発なし")

## <a name="the-tangent-arc"></a>正接の円弧

サポートされている弧の 2 番目の型`SKPath`は、*正接円弧*円弧が接続されている 2 つの直線に正接を円の円周であるためです。

呼び出しと共に正接円弧がパスに追加、 [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) 、2 つのメソッド`SKPoint`パラメーター、または[ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/)独立したオーバー ロードを`Single`パラメーターをポイント:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

これは、`ArcTo`メソッドは、PostScript [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) (PDF ドキュメントで 532 ページ) の関数と、iOS [ `AddArcToPoint` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArcToPoint/p/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/)メソッドです。

`ArcTo`方法では、3 つのポイント。

- 現在のポイント、線、またはポイント (0, 0) の場合`MoveTo`が呼び出されていません。
- 最初のポイント引数、`ArcTo`というメソッドを*コーナーのポイント*
- 2 番目のポイント引数`ArcTo`という、*終点*:

![](arcs-images/tangentarcthreepoints.png "正接の円弧を開始する 3 つのポイント")

これら 3 つのポイントは、接続されている 2 つの直線を定義します。

![](arcs-images/tangentarcconnectinglines.png "正接の円弧の 3 つの点を結ぶ線")

3 つのポイントが同一線上にある場合&mdash;は、同一直線上に存在する場合&mdash;円弧は描画されません。

`ArcTo`メソッドも含まれています、`radius`パラメーター。 これには、円の半径を定義します。

![](arcs-images/tangentarccircle.png "正接の円弧の円")

正接の円弧は汎用化されていないの楕円を描画します。

両方の行に正接にされる、満たす場合は、2 つの行では、これらの行の間で円を挿入することができます。

![](arcs-images/tangentarctangentcircle.png "2 つの線の間、正接の円弧円")

輪郭に追加される、曲線でタッチで指定されたポイントのいずれかのない、`ArcTo`メソッドです。 最初の接点と 2 番目の接点で終わる円弧を直線現在のポイントからで構成されます。

![](arcs-images/tangentarchighlight.png "2 つの行の強調表示された正接円弧")

最終的な直線および曲線に追加される円弧を次に示します。

![](arcs-images/tangentarc.png "2 つの行の強調表示された正接円弧")

2 番目の接点から輪郭を続行することができます。

**タンジェント円弧** ページでは、正接の円弧をテストすることができます。これは、最初のいくつかのページから派生する[ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs)、便利ないくつかを定義する`SKPaint`オブジェクトし、実行`TouchPoint`処理します。

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

`TangentArcPage` クラスは `InteractivePage` から派生したものです。 コンス トラクター、 [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs)ファイルがインスタンス化と初期化を担当する、`touchPoints`配列、および設定`baseCanvasView`(で`InteractivePage`) に、`SKCanvasView`でオブジェクトがインスタンス化、 [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml)ファイル。

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

`PaintSurface`ハンドラーを使用して、`ArcTo`タッチ ポイントに基づいて円弧を描画する方法、および`Slider`、アルゴリズムによっても、角度の基になっている円を計算しますが。

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

ここでは、**タンジェント円弧**3 つすべてのプラットフォームで実行されているページ。

[![](arcs-images/tangentarc-small.png "タンジェント円弧ページのスクリーン ショットをトリプル")](arcs-images/tangentarc-large.png#lightbox "タンジェント円弧ページのトリプル スクリーン ショット")

Windows Mobile デバイスで、3 つのポイントは、ほぼ同一線上と円弧が非常に小さいです。

正接弧は、角が丸い角の丸い四角形などの作成に最適です。 `SKPath`が既に含まれる、`AddRoundedRect`メソッド、**丸め七角形**ページが使用する方法を示します`ArcTo`7 つの辺の多角形の角を丸める場合。 (コードは、正規の多角形の汎用的です。)

`PaintSurface`のハンドラー、 [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs)クラスでは、1 つ含まれています`for`ループ、七角形とこれらの値から 7 つの辺の中間点を計算する 2 番目の 7 つの頂点の座標を計算するには頂点。 これらの中間点が、パスを構築するために使用されます。

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

3 つのプラットフォームで実行されているプログラムを次に示します。

[![](arcs-images/roundedheptagon-small.png "丸め七角形ページのスクリーン ショットをトリプル")](arcs-images/roundedheptagon-large.png#lightbox "丸め七角形ページのトリプル スクリーン ショット")

## <a name="the-elliptical-arc"></a>楕円の円弧

呼び出しと共に楕円の円弧がパスに追加、 [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/SkiaSharp.SKPoint/)を持つ 2 つのメソッド`SKPoint`パラメーター、または[ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/System.Single/System.Single/)個別 X と Y 座標をオーバー ロードします。

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

楕円の円弧が整合、[楕円の円弧](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands)スケーラブル ベクター グラフィックス (SVG) とユニバーサル Windows プラットフォームに含まれる[ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/)クラスです。

これら`ArcTo`メソッドが、輪郭の現在のポイントが 2 つのポイントの間の円弧を描画し、最後のパラメーターを`ArcTo`メソッド (、`xy`パラメーターまたは個別の`x`と`y`パラメーター)。

![](arcs-images/ellipticalarcpoints.png "楕円の円弧が定義されている 2 つのポイント")

最初のポイント パラメーター、`ArcTo`メソッド (`r`、または`rx`と`ry`) ポイントではありませんが、代わりに ; 楕円の水平方向および垂直方向の半径を指定します

![](arcs-images/ellipticalarcellipse.png "楕円の円弧が定義される楕円")

`xAxisRotate`パラメーターはこの楕円を回転する時計回りの角度の数。

![](arcs-images/ellipticalarctiltedellipse.png "楕円の円弧が定義されているいた傾いた楕円")

この傾斜している省略記号が 2 つのポイントと接触する配置されているし場合、によって異なる 2 つの円弧の点が接続されています。

![](arcs-images/ellipticalarcellipse1.png "楕円の円弧の最初のセット")

これら 2 つの円弧を 2 つの方法で識別できる: 最上位の円弧が下部にある円弧よりも大きいと反時計回りの下部にある円弧を描画中に時計回りに最上位の円弧が描画されるように左から右に円弧が描画されします。

別の方法で 2 つのポイント間の楕円の適合することも。

![](arcs-images/ellipticalarcellipse2.png "楕円の円弧の 2 番目のセット")

今すぐが小さい円弧が描画されるまで、時計回り、上部にありで描画される下部にある大きな円弧反時計回りにあります。

これら 2 つのポイントは、合計で 4 つの方法で割引楕円で定義された円弧によってため接続できます。

![](arcs-images/ellipticalarccolors.png "すべての 4 つの楕円の円弧")

これら 4 つの円弧が 4 つの組み合わせによって識別されます、 [ `SKPathArcSize` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathArcSize/)と[ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/)列挙体の型引数、`ArcTo`メソッド。

- 赤: SKPathArcSize.Large と SKPathDirection.Clockwise
- 緑: SKPathArcSize.Small と SKPathDirection.Clockwise
- 青い: SKPathArcSize.Small と SKPathDirection.CounterClockwise
- マゼンタ: SKPathArcSize.Large と SKPathDirection.CounterClockwise

傾斜している省略記号がない場合、2 つのポイント間に収まるように十分な大きさが十分な大きさになるまで一様拡大縮小します。 2 つの一意な円弧は、その場合は、2 つのポイントを接続します。 これらを識別できますが、`SKPathDirection`パラメーター。

円弧を定義するには、この方法は、最初に遭遇したで複雑な音、回転の楕円と円弧を定義できます。 唯一の方法、ことがある多くの場合、最も簡単なアプローチ輪郭の他の部分と円弧を統合する必要がある場合。

**楕円の円弧** ページでは、2 つのポイント サイズと楕円の回転を対話的に設定することができます。 `EllipticalArcPage`クラスから派生`InteractivePage`、および`PaintSurface`ハンドラーに、 [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs)分離コード ファイルは、次の 4 つの円弧を描画します。

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

ここで、次の 3 つのプラットフォームで実行されています。

[![](arcs-images/ellipticalarc-small.png "楕円の円弧のページのスクリーン ショットをトリプル")](arcs-images/ellipticalarc-large.png#lightbox "楕円の円弧のページのトリプル スクリーン ショット")

**円弧無限大**ページでは、楕円の円弧を使用して、無限大記号を描画します。 無限大記号は、100 単位で区切られた 100 単位の半径のある 2 つの円に基づいています。

![](arcs-images/infinitycircles.png "2 つの円")

両方の円グラフに接線を互いを横断する 2 つの行には。

![](arcs-images/infinitycircleslines.png "接線 2 つの円")

無限大記号は、これらの円、次の 2 つの行の部分の組み合わせです。 楕円の円弧を使用して、無限大記号を描画する、2 つの行が、円グラフに接線は座標を決定する必要があります。

円のいずれかで正しい四角形を作成します。

![](arcs-images/infinitytriangle.png "接線と埋め込みの円の 2 つの円")

円の半径は 100 単位、および三角形の斜辺は 150 単位は、角度 α は 150 でまたは 41.8 度分割 100 のアークサイン (逆正弦)。 三角形の相手側の長さは、150 回 41.8 角度のコサイン 112 で、ピタゴラスの定理で計算することもできます。

接線の点の座標にはこの情報を使用して計算できます。

x = 112·cos(41.8) = 83

y = 112·sin(41.8) = 75

次の 4 つの接点では、100 の円の半径のあるポイント (0, 0) で中央揃え無限大記号を描画するために必要なすべて。

![](arcs-images/infinitycoordinates.png "接線と座標の 2 つの円")

`PaintSurface`ハンドラー、 [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs)クラスは、無限大記号を配置できるように、(0, 0) ポイントが、ページの中央に配置されているし、画面のサイズへのパスのスケールを設定。

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

コードを使用して、`Bounds`プロパティ`SKPath`キャンバスのサイズに拡大する無限大サインの寸法を決定します。

[![](arcs-images/arcinfinity-small.png "円弧の無限大のページのスクリーン ショットをトリプル")](arcs-images/arcinfinity-large.png#lightbox "円弧無限大ページのトリプル スクリーン ショット")

示します結果とは少し小さな、`Bounds`プロパティ`SKPath`パスよりも大きいサイズを報告します。

内部的には、複数の 2 次ベジエ曲線を使用して、円弧が Skia の近似値を求めます。 これらの曲線 (次のセクションで表示されます) を含める曲線の描画方法を制御が表示された曲線の一部ではないの制御点。 `Bounds`プロパティには、これらのコントロール ポイントが含まれています。

厳密な調整を取得する、`TightBounds`プロパティで、コントロール ポイントは含まれません。 ここでは、横モードで実行されておりを使用して、プログラム、`TightBounds`境界のパスを取得するプロパティ。

[![](arcs-images/arcinfinitytightbounds-small.png "トリプル ページのスクリーン ショット、円弧無限大緊密な境界を持つ")](arcs-images/arcinfinitytightbounds-large.png#lightbox "緊密な境界を持つ弧無限大ページのトリプル スクリーン ショット")

円弧と直線間の接続は、スムーズな数学的には、直線を弧からの変更が少し突然思われる場合します。 次のページより優れた無限大記号が表示されます。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
