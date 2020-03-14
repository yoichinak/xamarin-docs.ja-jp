---
title: 3 つの種類のベジエ曲線
description: この記事では、SkiaSharp を使用して、Xamarin.Forms アプリケーションでの立方、二次方程式、および円錐のベジエ曲線をレンダリングする方法を説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: davidbritch
ms.author: dabritch
ms.date: 05/25/2017
ms.openlocfilehash: 1cf061f2ff27720ad78567bc26f00d99c5456f04
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306489"
---
# <a name="three-types-of-bzier-curves"></a>3 つの種類のベジエ曲線

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp を使用して、3次ベジエ曲線を描画する方法について説明します。_

サンピエール ベジエ (1910 – 1999 年)、フランス語のエンジニア、自動車会社 Renault、車の本文のコンピューター支援型のデザインの曲線を使用した後、ベジエ曲線と呼びます。

ベジエ曲線は、対話的な設計に適していることがわかっています。つまり、見た目は非常に優れて &mdash; います。つまり、曲線が無限または扱いにくい &mdash; になるような特異性はなく、通常はのようになります。

![サンプルのベジエ曲線](beziers-images/beziersample.png)

コンピューターのフォントの文字アウトラインは通常、ベジエ曲線で定義されます。

[**ベジエ曲線**](https://en.wikipedia.org/wiki/B%C3%A9zier_curve)の Wikipedia の記事には、いくつかの便利な背景情報が含まれています。 *ベジエ曲線*という用語は、実際には同様の曲線のファミリを指します。 SkiaSharp*は、* 3 種類のベジエ曲線 (3*種類のベジエ*曲線) をサポート*しています。* 円錐は、*有理数*とも呼ばれます。

## <a name="the-cubic-bzier-curve"></a>3 次ベジエ曲線

3 次ベジエ曲線の件名表示されたら、ほとんどの開発者が考えるベジエ曲線の種類です。

3つの `SKPoint` パラメーターを持つ[`CubicTo`](xref:SkiaSharp.SKPath.CubicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKPoint))メソッドを使用して `SKPath` オブジェクトに3次ベジエ曲線を追加することも、 [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo(System.Single,System.Single,System.Single,System.Single,System.Single,System.Single))オーバーロードに個別の `x` と `y` パラメーターを追加することもできます。

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

輪郭の現在の点で曲線が開始されます。 完了に 3 次ベジエ曲線は、4 つの点によって定義されます。

- 開始点: 輪郭の現在のポイント。 `MoveTo` が呼び出されていない場合は (0, 0)
- 最初の制御点: `CubicTo` 呼び出しの `point1`
- 2番目の制御点: `CubicTo` 呼び出しの `point2`
- エンドポイント: `CubicTo` 呼び出しの `point3`

結果の曲線が開始時点で始まり終了時点で終了します。 曲線は、一般的にはパススルーされず、2 つの制御点。代わりに、コントロールは、それらに曲線をプルする磁石と同様に機能を指します。

実験は 3 次ベジエ曲線をおおまかに把握することをお勧めします。 これは、`InteractivePage`から派生した **[ベジエ曲線]** ページの目的です。 [**BezierCurvePage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml)ファイルは、`SKCanvasView` と `TouchEffect`をインスタンス化します。 [**BezierCurvePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs)分離コードファイルは、コンストラクターに4つの `TouchPoint` オブジェクトを作成します。 `PaintSurface` イベントハンドラーは、4つの `TouchPoint` オブジェクトに基づいてベジエ曲線を描画するための `SKPath` を作成します。また、コントロールポイントからエンドポイントまで点線の接線を描画します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with cubic Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.CubicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     touchPoints[3].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[2].Center.X,
                    touchPoints[2].Center.Y,
                    touchPoints[3].Center.X,
                    touchPoints[3].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
       touchPoint.Paint(canvas);
    }
}
```

ここでは、実行します。

[[ベジエ曲線] ページのトリプルスクリーンショット ![](beziers-images/beziercurve-small.png)](beziers-images/beziercurve-large.png#lightbox)

数学的、曲線は、3 次多項式近似が。 曲線では、次の 3 つのポイントで直線が多くてと交差します。 開始時点では、曲線は常に、接線のと同じ方向に、直線の開始からは最初の制御点をポイントします。 曲線は常に、終了時点では、接線のと同じ方向に、直線 2 つ目のコントロールからは終了点をポイントします。

3 次ベジエ曲線が凸四角形の 4 つの点を結ぶによって常に制限されます。 これは、*凸ハル*と呼ばれます。 制御点は、起点と終点の間の直線上にあるを場合は、直線としてベジエ曲線が表示されます。 曲線できますも伴う自体には、3 番目のスクリーン ショットに示すようにします。

パスの輪郭が複数の 3 次の接続されたベジエ曲線を含めることができますが、次の 3 つのポイントが同一線上に場合にのみ、スムーズ 3 次ベジエ曲線を 2 つの間の接続になります (つまりは直線上にある)。

- 最初の曲線の 2 つ目の制御点
- これは、2 番目の曲線の開始点でも最初の曲線の終点
- 2 番目の曲線の最初の制御点

[**SVG パスデータ**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)に関する次の記事では、スムーズに接続されたベジエ曲線の定義を容易にする機能について説明します。

3 次ベジエ曲線をレンダリングする、基になるパラメーターの式を把握する便利な場合があります。 0から1までの*t*の場合、パラメーター式は次のようになります。

x(t) = (t) 1 日 ~ ³x₀ 3t + (1 – t) ²x₁ 3t² + (1 – t) x₂ + t³x₃。

y(t) = (t) 1 日 ~ ³y₀ 3t + (1 – t) ²y₁ 3t² + (1 – t) y₂ + t³y₃。

これらは三次 polynomials 3 の最大指数を確認します。 `t` が0の場合、ポイント (x ₀, y ₀) が始点であり、`t` が1の場合は、終点が (x ₃, y ₃) であることを確認するのが簡単です。 始点の近く (`t`の値が小さい場合)、1つ目の制御点 (x ₁, y ₁) は強い効果を持ち、エンドポイント (x ₂, y ₂) が強い効果を持っていることを意味します。

## <a name="bezier-curve-approximation-to-circular-arcs"></a>円弧を近似をベジエ曲線

ベジエ曲線を使用して円弧を描画すると便利な場合があります。3次ベジエ曲線では、円の弧が非常に大きくなる可能性があります。そのため、4つの接続されたベジエ曲線で円全体を定義できます。 この推定は、25 年以上前に公開する 2 つの記事でについて説明します。

> Tor Dokken は、"*コンピューター支援ジオメトリ設計 7* (1990), 33-41" という、"曲率連続のベジエ曲線による丸の良い近似値" です。

> Michael Goldapp、"3 次的 Polynomials による円弧の近似値"、"*コンピューター支援型のジオメトリック設計 8* (1991)、227-238。

次の図は `pto`、`pt1`、`pt2`というラベルが付いた4つの点を示しています。ベジエ曲線 (赤で示されています) を定義 `pt3` して円弧に近似します。

![ベジエ曲線を持つ円弧の近似値](beziers-images/bezierarc45.png)

開始位置から制御点までの線は、円とベジエ曲線に接し、長さは*L*になっています。上記の最初の記事では、この長さの*L*が次のように計算される場合に、ベジエ曲線が円弧に近似されることを示しています。

L = 4 × tan(α/4)/3

L 0.265 ために、図には、45 度の角度を示します。 コードでは、その値は、必要な円の半径が乗算は。

**[ベジエ円弧]** ページを使用すると、ベジエ曲線を定義して実験し、最大180°の角度を円弧に近似できます。 [**BezierCircularArcPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml)ファイルは、角度を選択するための `SKCanvasView` と `Slider` をインスタンス化します。 [**BezierCircularArgPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs)分離コードファイル内の `PaintSurface` イベントハンドラーは、変換を使用して、ポイント (0, 0) をキャンバスの中央に設定します。 比較については、点を中心に、円を描画しをベジエ曲線の 2 つの制御点を計算します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 3;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate length of control point line
    float length = radius * 4 * (float)Math.Tan(Math.PI * angle / 180 / 4) / 3;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the end points
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point3 = new SKPoint(radius * sin, radius * cos);

    // Find the control points
    SKPoint point0Normalized = Normalize(point0);
    SKPoint point1 = point0 + new SKPoint(length * point0Normalized.Y,
                                          -length * point0Normalized.X);

    SKPoint point3Normalized = Normalize(point3);
    SKPoint point2 = point3 + new SKPoint(-length * point3Normalized.Y,
                                          length * point3Normalized.X);

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);
    canvas.DrawCircle(point3.X, point3.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point3.X, point3.Y, point2.X, point2.Y, dottedStroke);

    // Draw the Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.CubicTo(point1, point2, point3);
        canvas.DrawPath(path, redStroke);
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

```

始点と終点 (`point0` と `point3`) は、円の通常のパラメーター式に基づいて計算されます。 円の中央にあるため (0, 0)、これらのポイントもとして処理できる放射状のベクトルの円の中心から円周をします。 コントロール ポイントは、これらの放射状ベクトルに直角に交わっているので、円のタンジェントである行には。 右の山を別のベクトルとは、元のベクター スワップ X および Y 座標と負の値に加えられたうち 1 つだけです。

さまざまな角度の実行中のプログラムを次に示します。

[[ベジエ円弧] ページのトリプルスクリーンショット ![](beziers-images/beziercirculararc-small.png)](beziers-images/beziercirculararc-large.png#lightbox)

3 番目のスクリーン ショットをよく見るし、角度が 180 度が四半期円うまくを角度が 90 度に合わせて見えます iOS の画面を示しています、半円からにベジエ曲線を外れて顕著なことを確認します。

四半期の円がこのような方向は、この 2 つの制御点の座標を計算することは非常に簡単です。

![ベジエ曲線を使用した四半期の円の近似値](beziers-images/bezierarc90.png)

円の半径が100の場合、 *L*は55であり、覚えやすい数値です。

**2 乗**は、円と正方形の間の図形をアニメーション化します。 円は、 [`SquaringTheCirclePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs)クラスのこの配列定義の最初の列に座標が表示される4つのベジエ曲線によって近似されます。

```csharp
public class SquaringTheCirclePage : ContentPage
{
    SKPoint[,] points =
    {
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() },
        { new SKPoint(  55,  100), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,   55), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,    0), new SKPoint(   125,      0), new SKPoint() },
        { new SKPoint( 100,  -55), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(  55, -100), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(   0, -100), new SKPoint(     0,   -125), new SKPoint() },
        { new SKPoint( -55, -100), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,  -55), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,    0), new SKPoint(  -125,      0), new SKPoint() },
        { new SKPoint(-100,   55), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint( -55,  100), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() }
    };
    ...
}
```

2 番目の列には、領域を持つが、円の面積と同じでは約正方形を定義する 4 つのベジエ曲線の座標が含まれています。 (特定の円として*正確*な領域を持つ正方形を描画することは、[円を2乗](https://en.wikipedia.org/wiki/Squaring_the_circle)とする従来の解決不可能なジオメトリックの問題です)。ベジエ曲線で正方形をレンダリングする場合、各曲線の2つの制御点は同じであり、始点と終点が線形であるため、ベジエ曲線は直線としてレンダリングされます。

配列の 3 番目の列は、アニメーションの補間値です。 このページでは、タイマーが16ミリ秒に設定され、`PaintSurface` ハンドラーがその速度で呼び出されます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    canvas.Translate(info.Width / 2, info.Height / 2);
    canvas.Scale(Math.Min(info.Width / 300, info.Height / 300));

    // Interpolate
    TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
    float t = (float)(timeSpan.TotalSeconds % 3 / 3);   // 0 to 1 every 3 seconds
    t = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;     // 0 to 1 to 0 sinusoidally

    for (int i = 0; i < 13; i++)
    {
        points[i, 2] = new SKPoint(
            (1 - t) * points[i, 0].X + t * points[i, 1].X,
            (1 - t) * points[i, 0].Y + t * points[i, 1].Y);
    }

    // Create the path and draw it
    using (SKPath path = new SKPath())
    {
        path.MoveTo(points[0, 2]);

        for (int i = 1; i < 13; i += 3)
        {
            path.CubicTo(points[i, 2], points[i + 1, 2], points[i + 2, 2]);
        }
        path.Close();

        canvas.DrawPath(path, cyanFill);
        canvas.DrawPath(path, blueStroke);
    }
}
```

ポイントは、`t`の不安定な sinusoidally 値に基づいて補間されます。 挿入ポイントは、一連の 4 つの接続されたベジエ曲線を作成し、使用されます。 アニメーションの実行を次に示します。

[2乗ののトリプルスクリーンショット ![](beziers-images/squaringthecircle-small.png)](beziers-images/squaringthecircle-large.png#lightbox)

このようなアニメーションがなければ曲線は弧と直線の両方としてレンダリングされる柔軟性アルゴリズムになります。

また、 **[ベジエ無限大]** ページでは、ベジエ曲線の機能を利用して円弧を概数化できます。[`BezierInfinityPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs)クラスの `PaintSurface` ハンドラーを次に示します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.MoveTo(0, 0);                                // Center
        path.CubicTo(  50,  -50,   95, -100,  150, -100); // To top of right loop
        path.CubicTo( 205, -100,  250,  -55,  250,    0); // To far right of right loop
        path.CubicTo( 250,   55,  205,  100,  150,  100); // To bottom of right loop
        path.CubicTo(  95,  100,   50,   50,    0,    0); // Back to center  
        path.CubicTo( -50,  -50,  -95, -100, -150, -100); // To top of left loop
        path.CubicTo(-205, -100, -250,  -55, -250,    0); // To far left of left loop
        path.CubicTo(-250,   55, -205,  100, -150,  100); // To bottom of left loop
        path.CubicTo( -95,  100,  -50,   50,    0,    0); // Back to center
        path.Close();

        SKRect pathBounds = path.Bounds;
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.9f * Math.Min(info.Width / pathBounds.Width,
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

関係を表示するグラフ用紙にこれらの座標をプロットする良い練習が考えられます。 無限大記号の中心点 (0, 0) と 2 つのループのセンターがある (–150, 0) と (150, 0) と 100 の半径。 一連の `CubicTo` コマンドでは、-95 と–205の値 (これらの値は、–150プラスおよびマイナス 55)、205と 95 (150 とマイナス 55)、および右と左側の250および–250を使用して、制御点の X 座標を確認できます。 無限大記号はセンターが超えたときに、例外です。 その場合は、コントロール ポイントでは、中心に近い曲線をまっすぐに 50 と –50 の組み合わせで座標があります。

無限大記号を次に示します。

[ベジエ無限大ページのトリプルスクリーンショット ![](beziers-images/bezierinfinity-small.png)](beziers-images/bezierinfinity-large.png#lightbox)

Arc 記事[**を描画する3つの方法**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)から、**アーク無限大**ページによってレンダリングされる無限大記号よりも、中心方向の方がややスムーズです。

## <a name="the-quadratic-bzier-curve"></a>2 次ベジエ曲線

2 次ベジエ曲線が 1 つだけの制御点と曲線が 3 つの点によって定義されます。 開始点、コントロール ポイントと終了点。 パラメーターの式は 3 次ベジエ曲線と非常に似ていますが、最高の指数部は 2、曲線は 2 次多項式。

x(t) = (1 – t)²x₀ + 2t(1 – t)x₁ + t²x₂

y(t) = (1 – t)²y₀ + 2t(1 – t)y₁ + t²y₂

パスに2次ベジエ曲線を追加するには、 [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint))メソッドまたは別の `x` および `y` 座標で[`QuadTo`](xref:SkiaSharp.SKPath.QuadTo(System.Single,System.Single,System.Single,System.Single))オーバーロードを使用します。

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

メソッドは、現在の位置から、`point1` を制御ポイントとして `point2` に曲線を追加します。

2次ベジエ曲線は**2 次曲線**ページで試してみることができます。これは、3つのタッチポイントだけがある点を除いて、 **[ベジエ曲線]** ページとよく似ています。 [**QuadraticCurve.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs)分離コードファイルの `PaintSurface` ハンドラーは次のようになります。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with quadratic Bezier
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.QuadTo(touchPoints[1].Center,
                    touchPoints[2].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

ここで実行しています。

[[2 次曲線] ページのトリプルスクリーンショット ![](beziers-images/quadraticcurve-small.png)](beziers-images/quadraticcurve-large.png#lightbox)

点線では、始点と終点の曲線の接線は、コントロール ポイントで交差します。

全般的な形式では、曲線を作成する必要がありますが、2 つではなく 1 つのコントロール ポイントの利便性を使用する場合は、2 次ベジエをお勧めします。 2 次ベジエを他の曲線、楕円の円弧を表示するために Skia で内部的に使用される理由より効率的に表示します。

ただし、2次ベジエ曲線の形状は楕円ではないため、楕円弧を模倣するために複数の2次 Béziers が必要になります。2次ベジエは、代わりに放物線のセグメントです。

## <a name="the-conic-bzier-curve"></a>円錐のベジエ曲線

円錐ベジエ曲線 &mdash;、有理数曲線のファミリに比較的最近追加された &mdash;、有理数の2次ベジエ曲線とも呼ばれます。 、2 次ベジエ曲線のように、有理数の 2 次ベジエ曲線には開始点、終点、および 1 つのコントロール ポイントが含まれます。 ただし、有理数の2次ベジエ曲線にも*重み*値が必要です。 パラメーター式には比率が含まれているため、これは*有理数*の2次式と呼ばれます。

パラメーターの式 X と Y は、同じ分母を共有する比率。 次に示すのは、0から1までの*t*の分母と*w*の重み値の式です。

d(t) = (1 – t)² + 2wt(1 – t) + t²

理論上は、合理的な二次方程式は 3 つの用語ごとに 1 つ、3 つの個別の重み付け値を伴うことができますが、これらを 1 つの重み値の中間期間に簡略化できます。

中間という用語は、重みの値も含まれています。 および、式は、分母で除算する点を除いて、X と Y 座標のパラメーターの式は 2 次ベジエのパラメーターの式に似ています。

x(t) = ((1 – t)²x₀ + 2wt(1 – t)x₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t)²y₀ + 2wt(1 – t)y₁ + t²y₂)) ÷ d(t)

合理的な2次ベジエ曲線は*conics*とも呼ばれます。これは、放物線、楕円、および円の &mdash;、すべての円錐セクションのセグメントを正確に表すことができるためです。

有理数の2次ベジエ曲線をパスに追加するには、 [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single))メソッドを使用するか、別の `x` および `y` 座標で[`ConicTo`](xref:SkiaSharp.SKPath.ConicTo(System.Single,System.Single,System.Single,System.Single,System.Single))オーバーロードを使用します。

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

最後の `weight` パラメーターに注意してください。

**[円錐曲線]** ページでは、これらの曲線を試すことができます。 `ConicCurvePage` クラスは `InteractivePage` から派生したものです。 [**Coniccurvepage .xaml**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml)ファイルは、-2 ~ 2 の重み値を選択するために `Slider` をインスタンス化します。 [**ConicCurvePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs)分離コードファイルは3つの `TouchPoint` オブジェクトを作成し、`PaintSurface` ハンドラーは単に、結果の曲線をコントロールポイントに接線と共に描画します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with conic curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.ConicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     (float)weightSlider.Value);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

ここでは、実行します。

[[円錐曲線] ページのトリプルスクリーンショット ![](beziers-images/coniccurve-small.png)](beziers-images/coniccurve-large.png#lightbox)

ご覧のように、コントロール ポイントは、重みが大きいときに詳細が曲線をプルするようです。 重みが 0 の場合、曲線は始点から終点を直線になります。

理論的には、負の重みが許容され、曲線が制御ポイントから*離れる*ようになります。 ただし、-1 以下の重みを指定すると、パラメーター式の分母が*t*の特定の値に対して負になります。 この理由により、`ConicTo` メソッドで負の重みが無視されることがあります。 **円錐曲線**プログラムを使用すると、負の重みを設定できます。ただし、実験によって表示されるように、負の重みは重み付けが0の場合と同じ効果を持ち、直線がレンダリングされます。

`ConicTo` メソッドを使用して、半円の上に (ただし、含まない) 円弧を描画するために、制御点とウェイトを簡単に派生させることができます。 次の図では、始点と終点の接線は、コントロール ポイントに満たしています。

![円弧の円錐の円弧レンダリング](beziers-images/conicarc.png)

制御点。 円の中心からの距離を調べるに三角関数を使用することができます: が半分の角度 α のコサインの値で除算する円の半径。 始点と終点の間の円弧を描画するには、その同じコサイン半分の角度の重みを設定します。 角度が 180 度の場合は、接線を満たすことはありませんし、重みは 0 に注意してください。 角度が 180 度未満、数学で正しく動作します。

この例では、**円錐の円弧**ページが示されています。 [**ConicCircularArc**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml)ファイルは、角度を選択するための `Slider` をインスタンス化します。 [**ConicCircularArc.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs)分離コードファイルの `PaintSurface` ハンドラーは、制御点と重みを計算します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 4;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the points and weight
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point1 = new SKPoint(0, radius / cos);
    SKPoint point2 = new SKPoint(radius * sin, radius * cos);
    float weight = cos;

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point2.X, point2.Y, point1.X, point1.Y, dottedStroke);

    // Draw the conic
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.ConicTo(point1, point2, weight);
        canvas.DrawPath(path, redStroke);
    }
}
```

ご覧のように、赤で示されている `ConicTo` パスと、参照用に表示される基になる円の間には視覚的な違いはありません。

[円錐の円弧ページのトリプルスクリーンショット ![](beziers-images/coniccirculararc-small.png)](beziers-images/coniccirculararc-large.png#lightbox)

角度を 180 度、および数学失敗に設定します。

この場合、`ConicTo` は負の重みをサポートしていません。理論的には (パラメーター式に基づく)、同じ点を持つが、重みが負の値である `ConicTo` の別の呼び出しで円を完了できます。 これにより、ゼロ°と180度の間の任意の角度に基づいて、2つの `ConicTo` 曲線だけを含む円全体を作成できます。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
