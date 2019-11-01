---
title: 円弧を描画する 3 つの方法
description: この記事では、SkiaSharp を使用して、3つの異なる方法で円弧を定義する方法について説明し、サンプルコードを使用してその方法を示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2017
ms.openlocfilehash: 56c2da48c596fa35dd1a7658a38eee23c939e2fd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029544"
---
# <a name="three-ways-to-draw-an-arc"></a>円弧を描画する 3 つの方法

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp を使用して、3つの異なる方法で円弧を定義する方法について説明します。_

弧は、この無限大符号の丸められた部分など、楕円の円周に対する曲線です。

![無限大記号](arcs-images/arcsample.png)

その定義の単純さにもかかわらず、すべてのニーズを満たす円弧描画関数を定義する方法はありません。したがって、円弧を描画する最適な方法のグラフィックスシステム間に同意する必要はありません。このため、`SKPath` クラスは、それ自体が1つのアプローチに限定されるわけではありません。

`SKPath` は、 [`AddArc`](xref:SkiaSharp.SKPath.AddArc*)メソッド、5つの異なる[`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*)メソッド、および2つの相対[`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*)メソッドを定義します。 これらのメソッドは3つのカテゴリに分類され、円弧を指定するための3つの非常に異なるアプローチを表します。どの方法を使用するかは、円弧を定義するために使用できる情報によって異なります。また、この弧が描画する他のグラフィックスとどのように適合するかによって決まります。

## <a name="the-angle-arc"></a>角度の円弧

円弧を描画するための山の弧アプローチでは、楕円の境界となる四角形を指定する必要があります。 この楕円の円周の弧は、円弧の始点と長さを示す楕円の中心からの角度によって示されます。 2つの異なるメソッドは、山の弧を描画します。 [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single))メソッドと[`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKRect,System.Single,System.Single,System.Boolean))メソッドを次に示します。

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

これらのメソッドは、Android [`AddArc`](xref:Android.Graphics.Path.AddArc*) 、[`ArcTo`]、Xref: Android. arcto *) の各メソッドと同じです。 IOS の[`AddArc`](xref:CoreGraphics.CGPath.AddArc(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.Boolean))方法は似ていますが、楕円に一般化されるのではなく、円の円周の弧に限定されています。

どちらのメソッドも、楕円の位置とサイズの両方を定義する `SKRect` 値で始まります。

![山の円弧を開始する楕円](arcs-images/anglearcoval.png)

円弧は、この楕円の円周の一部です。

`startAngle` 引数は、楕円の中心から右に描画される水平線に対する時計回りの角度です。 `sweepAngle` 引数は、`startAngle`に対する相対パスです。 次に示すのは、60度と100度の値の `startAngle` と `sweepAngle` です。

![角度の円弧を定義する角度](arcs-images/anglearcangles.png)

円弧は開始角度から開始されます。 長さはスイープ角度によって管理されます。 ここでは、弧を赤で示しています。

![強調表示された山の円弧](arcs-images/anglearchighlight.png)

`AddArc` または `ArcTo` メソッドを使用してパスに追加された曲線は、単に楕円の円周の一部です。

![弧の角度](arcs-images/anglearc.png)

`startAngle` 引数または `sweepAngle` 引数は負の値にすることができます。円弧は `sweepAngle` の正の値に対して時計回りで、負の値の場合は反時計回りに回転します。

ただし、`AddArc` では、閉じた輪郭は定義*されません*。 `AddArc`後に `LineTo` を呼び出すと、円弧の末尾から `LineTo` メソッドの点までの線が描画されます。また、`ArcTo`の場合も同様です。

`AddArc` によって新しい輪郭が自動的に開始され、`true`の最後の引数を使用して `ArcTo` の呼び出しと機能的に同等になります。

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

最後の引数は `forceMoveTo`と呼ばれ、実質的には、円弧の先頭で `MoveTo` 呼び出しを行います。新しい輪郭が開始されます。 これは、`false`の最後の引数には当てはまりません。

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

このバージョンの `ArcTo` は、現在の位置から円弧の先頭までの線を描画します。これは、円弧が大きな輪郭の途中にある可能性があることを意味します。

**[山弧]** ページでは、2つのスライダーを使用して開始角度とスイープ角度を指定できます。 XAML ファイルは、2つの `Slider` 要素と `SKCanvasView`をインスタンス化します。 [**AngleArcPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs)ファイルの `PaintCanvas` ハンドラーは、フィールドとして定義されている2つの `SKPaint` オブジェクトを使用して、oval と弧の両方を描画します。

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

ご覧のように、開始角度とスイープ角度の両方で負の値を受け取ることができます。

[[山弧] ページのトリプルスクリーンショット![](arcs-images/anglearc-small.png)](arcs-images/anglearc-large.png#lightbox)

このように円弧を生成する方法は最も単純なアルゴリズムであり、円弧を記述するパラメーター式を簡単に導き出すことができます。楕円のサイズと位置、および開始とスイープの角度は、単純な三角式を使用して、円弧の始点と終点を計算することができます。

`x = oval.MidX + (oval.Width / 2) * cos(angle)`

`y = oval.MidY + (oval.Height / 2) * sin(angle)`

`angle` 値は `startAngle` または `startAngle + sweepAngle`のいずれかです。

2つの角度を使用して弧を定義するのは、描画する弧の長さがわかっている場合に最適です。たとえば、円グラフを作成する場合に適しています。 **分割円グラフ**のページには、この例が示されています。 [`ExplodedPieChartPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs)クラスは、内部クラスを使用して、いくつかの製造されたデータと色を定義します。

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

`PaintSurface` ハンドラーはまず項目をループ処理して `totalValues` 番号を計算します。 これにより、各項目のサイズを合計の割合として特定し、それを角度に変換することができます。

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

円スライスごとに新しい `SKPath` オブジェクトが作成されます。 パスは、中心からの1行で構成され、次に円弧を描画するための `ArcTo` と、`Close` 呼び出しの結果として、中央の行に戻ります。 このプログラムは、すべてを中央から50ピクセルずつ移動して、"分割" された円のスライスを表示します。 このタスクでは、各スライスのスイープ角度の中間点の方向にベクターが必要です。

[[分割円グラフ] ページのトリプルスクリーンショット![](arcs-images/explodedpiechart-small.png)](arcs-images/explodedpiechart-large.png#lightbox)

"爆発" がない状態を確認するには、単に `Translate` の呼び出しをコメントアウトします。

[分割されていない分割円グラフのページのトリプルスクリーンショット![](arcs-images/explodedpiechartunexploded-small.png)](arcs-images/explodedpiechartunexploded-large.png#lightbox)

## <a name="the-tangent-arc"></a>アークタンジェント

`SKPath` によってサポートされる2番目の型は*タンジェント円弧*です。そのため、弧は2つの接続された直線に接する円の円周であるため、が呼び出されます。

接線円弧は、2つの `SKPoint` パラメーターを持つ[`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single))メソッドを呼び出すことによってパスに追加されます。また、 [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,System.Single,System.Single))オーバーロードは、ポイントに対して個別の `Single` パラメーターを使用します。

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

この `ArcTo` 方法は、PostScript [`arct`](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) (ページ 532) 関数と iOS [`AddArcToPoint`](xref:CoreGraphics.CGPath.AddArcToPoint(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat))メソッドに似ています。

`ArcTo` メソッドには、次の3つの点が含まれます。

- 輪郭の現在の点。 `MoveTo` が呼び出されていない場合はポイント (0, 0)。
- `ArcTo` メソッドへの最初のポイント引数。*コーナーポイント*と呼ばれます。
- *コピー先のポイント*と呼ばれる、`ArcTo`する2番目の point 引数。

![アークタンジェントを開始する3つの点](arcs-images/tangentarcthreepoints.png)

これら3つのポイントは、2つの接続された直線を定義します。

![アークタンジェントの3つの点を結ぶ線](arcs-images/tangentarcconnectinglines.png)

3つの &mdash; 点が同じである場合、同じ直線上に存在する場合 &mdash; 弧は描画されません。

`ArcTo` メソッドには、`radius` パラメーターも含まれています。 これにより、円の半径が定義されます。

![アークタンジェントの円](arcs-images/tangentarccircle.png)

アークタンジェントは、楕円のために一般化されていません。

2本の線が任意の角度で交わる場合、その円は、両方の線に接するように線の間に挿入できます。

![2本の線の間の接線の円弧](arcs-images/tangentarctangentcircle.png)

輪郭に追加される曲線は、`ArcTo` メソッドに指定された点のいずれにも触れません。 これは、現在のポイントから最初の接点までの直線と、2番目の接点で終わる弧で構成されます。ここでは、赤で示されています。

![2本の線の間の強調表示されたタンジェントの円弧](arcs-images/tangentarchighlight.png)

次に、輪郭に追加される最後の直線と円弧を示します。

![2本の線の間の強調表示されたタンジェントの円弧](arcs-images/tangentarc.png)

輪郭は、2番目のタンジェントポイントから継続できます。

**[タンジェント弧]** ページを使用すると、アークタンジェントを試すことができます。これは、 [`InteractivePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs)から派生したいくつかのページのうち、いくつかの便利な `SKPaint` オブジェクトを定義し `TouchPoint` 処理を実行するものです。

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

`TangentArcPage` クラスは `InteractivePage` から派生したものです。 [**TangentArcPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs)ファイル内のコンストラクターは、`touchPoints` 配列をインスタンス化して初期化し、 [**TangentArcPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml)ファイルでインスタンス化された `SKCanvasView` オブジェクトに `baseCanvasView` (`InteractivePage`) を設定します。

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

`PaintSurface` ハンドラーは、`ArcTo` メソッドを使用して、タッチポイントと `Slider`に基づいて円弧を描画しますが、さらにアルゴリズムは角度の基になる円を計算します。

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

次の例で**は、を実行してい**ます。

[[接線の弧] ページのトリプルスクリーンショット![](arcs-images/tangentarc-small.png)](arcs-images/tangentarc-large.png#lightbox)

アークタンジェントは、丸みのある四角形など、角を丸くする場合に最適です。 `SKPath` には `AddRoundedRect` メソッドが既に含まれているため、**丸い Heptagon**ページでは、`ArcTo` を使用して、両側の多角形の角を丸める方法を示しています。 (コードは通常の多角形に対して一般化されています)。

[`RoundedHeptagonPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs)クラスの `PaintSurface` ハンドラーには、heptagon の7つの頂点の座標を計算するための `for` ループが1つ含まれています。2番目のループは、これらの頂点から7辺の中間点を計算します。 次に、これらの中間点を使用してパスを作成します。

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

[丸い Heptagon ページのトリプルスクリーンショット![](arcs-images/roundedheptagon-small.png)](arcs-images/roundedheptagon-large.png#lightbox)

## <a name="the-elliptical-arc"></a>楕円の円弧

楕円の円弧は、2つの `SKPoint` パラメーターを持つ[`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,SkiaSharp.SKPoint))メソッドを呼び出すことによってパスに追加されます。または、別の X 座標と Y 座標を持つ[`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,System.Single,System.Single))オーバーロードがあります。

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

楕円弧は、スケーラブルベクターグラフィックス (SVG) およびユニバーサル Windows プラットフォーム[`ArcSegment`](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/)クラスに含まれる[楕円弧](https://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands)と一致します。

これらの `ArcTo` メソッドは、輪郭の現在のポイントである2つの点の間に円弧を描画し、`ArcTo` メソッドの最後のパラメーター (`xy` パラメーターまたは個別の `x` と `y` パラメーター) を描画します。

![楕円の円弧を定義した2つの点](arcs-images/ellipticalarcpoints.png)

`ArcTo` メソッド (`r`、`rx` および `ry`) への最初のポイントパラメーターは、すべての点ではなく、楕円の水平方向と垂直方向の半径を指定します。

![楕円の円弧を定義した楕円](arcs-images/ellipticalarcellipse.png)

`xAxisRotate` パラメーターは、この楕円を回転させる時計回りの角度を指定します。

![楕円の円弧を定義した傾い楕円](arcs-images/ellipticalarctiltedellipse.png)

この傾斜楕円が2つの点に接するように配置されている場合、ポイントは2つの異なる円弧によって接続されます。

![楕円弧の最初のセット](arcs-images/ellipticalarcellipse1.png)

これら2つの円弧は、2つの方法で区別できます。一番上の弧は、下側の弧よりも大きくなります。また、円弧が左から右に描画されると、最上位の弧は時計回りの方向に描画され、下円弧は反時計回りに描画されます。

また、次のように、2つの点の間に楕円を収めることもできます。

![2番目の楕円弧のセット。](arcs-images/ellipticalarcellipse2.png)

これで、上には時計回りに描画される小さい円弧があり、一番下の方が反時計回りに描画されています。

したがって、これら2つの点は、合計4つの方法で、傾い楕円によって定義された弧によって接続できます。

![4つの楕円の円弧](arcs-images/ellipticalarccolors.png)

これらの4つの円弧は、 [`SKPathArcSize`](xref:SkiaSharp.SKPathArcSize)の4つの組み合わせと、`ArcTo` メソッドへの[`SKPathDirection`](xref:SkiaSharp.SKPathDirection)列挙型引数によって区別されます。

- 赤: SKPathArcSize. Large と SKPathDirection. 時計回り
- 緑: SKPathArcSize. Small と SKPathDirection. 時計回り
- blue: SKPathArcSize. s と SKPathDirection. 反時計回り
- マゼンタ: SKPathArcSize. Large および SKPathDirection. 反時計回り

傾斜楕円が2つの点の間に収めるのに十分な大きさでない場合は、十分な大きさになるまで均一にスケーリングされます。 2つの一意の弧だけが、その場合は2つのポイントを接続します。 これらは、`SKPathDirection` パラメーターと区別できます。

最初の発生時にはこのような円弧を定義する方法は複雑ですが、回転された楕円で円弧を定義できる唯一のアプローチであり、多くの場合、弧を他の部分と統合する必要がある場合に最も簡単な方法です。

**[楕円の円弧]** ページでは、2つの点、および楕円のサイズと回転を対話形式で設定できます。 `EllipticalArcPage` クラスは `InteractivePage`から派生し、 [**EllipticalArcPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs)分離コードファイルの `PaintSurface` ハンドラーは4つの円弧を描画します。

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

次のように実行されています。

[楕円の円弧ページのトリプルスクリーンショット![](arcs-images/ellipticalarc-small.png)](arcs-images/ellipticalarc-large.png#lightbox)

**[弧無限大]** ページでは、楕円弧を使用して無限大記号を描画します。 無限大記号は、100単位で区切られた100ユニットの半径を持つ2つの円に基づいています。

![2つの円](arcs-images/infinitycircles.png)

相互に交差する2つの線が両方の円に接しています。

![接線を持つ2つの円](arcs-images/infinitycircleslines.png)

無限大記号は、これらの円の一部と2行の組み合わせです。 楕円の円弧を使用して無限大記号を描画するには、円に接する2本の直線の座標を決定する必要があります。

次のいずれかの円で右の四角形を構築します。

![接線と埋め込み円を含む2つの円](arcs-images/infinitytriangle.png)

円の半径は100単位で、三角形の斜辺は150単位であるため、αは100のアークサイン (逆正弦)、150、または41.8 度で除算されます。 三角形のもう一方の辺の長さは、41.8 度のコサインの150倍、つまりピタゴラス定理によって計算される112です。

次の情報を使用して、タンジェントポイントの座標を計算できます。

`x = 112·cos(41.8) = 83`

`y = 112·sin(41.8) = 75`

4つの接点は、円の半径100を使用して、ポイント (0, 0) の中央に無限大記号を描画するために必要です。

![接線と座標がある2つの円](arcs-images/infinitycoordinates.png)

[`ArcInfinityPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs)クラスの `PaintSurface` ハンドラーは、(0, 0) ポイントがページの中央に配置されるように無限大記号を配置し、パスを画面サイズにスケーリングします。

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

このコードでは、`SKPath` の `Bounds` プロパティを使用して無限正弦の大きさを判断し、キャンバスのサイズにスケーリングします。

[[弧無限大] ページのトリプルスクリーンショット![](arcs-images/arcinfinity-small.png)](arcs-images/arcinfinity-large.png#lightbox)

結果は少し小さいように見えますが、`SKPath` の `Bounds` プロパティが、パスより大きいサイズを報告していることがわかります。

内部的には、Skia は複数の2次ベジエ曲線を使用して円弧を近似します。 これらの曲線 (次のセクションで説明します) には、曲線が描画される方法を制御する制御ポイントが含まれていますが、描画される曲線の一部ではありません。 `Bounds` プロパティには、これらのコントロールポイントが含まれます。

厳密に適合させるには、`TightBounds` プロパティを使用します。これにより、コントロールポイントが除外されます。 横モードで実行されているプログラムを次に示します。また、`TightBounds` プロパティを使用してパスの範囲を取得します。

[[円弧無限大] ページのトリプルスクリーンショットを![します。](arcs-images/arcinfinitytightbounds-small.png)](arcs-images/arcinfinitytightbounds-large.png#lightbox)

弧と直線の間の接続は数学的に smooth ていますが、弧から直線への変化は少し変わっているように思えるかもしれません。 次の記事では、 [**3 種類のベジエ曲線**](beziers.md)について、より優れた無限大記号が示されています。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
