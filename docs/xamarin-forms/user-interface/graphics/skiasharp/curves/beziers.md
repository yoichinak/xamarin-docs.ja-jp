---
title: 3 つの種類のベジエ曲線
description: この記事では、SkiaSharp を使用して、Xamarin.Forms アプリケーションでの立方、二次方程式、および円錐のベジエ曲線をレンダリングする方法を説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: davidbritch
ms.author: dabritch
ms.date: 05/25/2017
ms.openlocfilehash: 60024be0c39bd215a828acfd8a4ac6294eeac9d8
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52172354"
---
# <a name="three-types-of-bzier-curves"></a>3 つの種類のベジエ曲線

_SkiaSharp を使用して、円錐、二次方程式、3 次ベジエ曲線をレンダリングする方法を詳細します。_

サンピエール ベジエ (1910 – 1999 年)、フランス語のエンジニア、自動車会社 Renault、車の本文のコンピューター支援型のデザインの曲線を使用した後、ベジエ曲線と呼びます。

ベジエ曲線が対話型の設計に最適であることがわかって: は正常に動作&mdash;が無限または扱いにくくする曲線の特異点がない、つまり&mdash;見栄えの良いは一般に、:

![](beziers-images/beziersample.png "サンプルのベジエ曲線")

コンピューターのフォントの文字アウトラインは通常、ベジエ曲線で定義されます。

Wikipedia の記事で[**ベジエ曲線**](https://en.wikipedia.org/wiki/B%C3%A9zier_curve)便利な背景情報が含まれています。 用語*ベジエ曲線*のような曲線のファミリを実際に参照します。 SkiaSharp のベジエ曲線、という 3 種類をサポートしています、*立方*、*二次*、および*円錐*します。 円錐とも呼ばれますが、*合理的な二次方程式*します。

## <a name="the-cubic-bzier-curve"></a>3 次ベジエ曲線

3 次ベジエ曲線の件名表示されたら、ほとんどの開発者が考えるベジエ曲線の種類です。

3 次ベジエ曲線を追加することができます、`SKPath`オブジェクトを使用して、 [ `CubicTo` ](xref:SkiaSharp.SKPath.CubicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKPoint))メソッドで 3 つ`SKPoint`パラメーター、または[ `CubicTo` ](xref:SkiaSharp.SKPath.CubicTo(System.Single,System.Single,System.Single,System.Single,System.Single,System.Single)) でオーバーロード`x`と`y`パラメーター。

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

輪郭の現在の点で曲線が開始されます。 完了に 3 次ベジエ曲線は、4 つの点によって定義されます。

- 開始点現在、輪郭または (0, 0) でポイントする場合`MoveTo`が呼び出されていない。
- 最初のコントロール ポイント:`point1`で、`CubicTo`を呼び出す
- 2 つ目のコントロール ポイント:`point2`で、`CubicTo`を呼び出す
- エンドポイント:`point3`で、`CubicTo`を呼び出す

結果の曲線が開始時点で始まり終了時点で終了します。 曲線は、一般的にはパススルーされず、2 つの制御点。代わりに、コントロールは、それらに曲線をプルする磁石と同様に機能を指します。

実験は 3 次ベジエ曲線をおおまかに把握することをお勧めします。 目的は、これは、**ベジエ曲線** ページから派生した`InteractivePage`します。 [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml)ファイルをインスタンス化、`SKCanvasView`と`TouchEffect`します。 [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs)分離コード ファイルには、4 つ作成します`TouchPoint`コンス トラクター内のオブジェクト。 `PaintSurface`イベント ハンドラーを作成、`SKPath`ベースの 4 つのベジエ曲線を表示するために`TouchPoint`オブジェクトし、も管理ポイントから終了ポイントにピリオドで区切られた接線を描画します。

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

[![](beziers-images/beziercurve-small.png "ベジエ曲線のページのスクリーン ショットをトリプル")](beziers-images/beziercurve-large.png#lightbox "ベジエ曲線のページの 3 倍になるスクリーン ショット")

数学的、曲線は、3 次多項式近似が。 曲線では、次の 3 つのポイントで直線が多くてと交差します。 開始時点では、曲線は常に、接線のと同じ方向に、直線の開始からは最初の制御点をポイントします。 曲線は常に、終了時点では、接線のと同じ方向に、直線 2 つ目のコントロールからは終了点をポイントします。

3 次ベジエ曲線が凸四角形の 4 つの点を結ぶによって常に制限されます。 これと呼ばれますが、*凸包*します。 制御点は、起点と終点の間の直線上にあるを場合は、直線としてベジエ曲線が表示されます。 曲線できますも伴う自体には、3 番目のスクリーン ショットに示すようにします。

パスの輪郭が複数の 3 次の接続されたベジエ曲線を含めることができますが、次の 3 つのポイントが同一線上に場合にのみ、スムーズ 3 次ベジエ曲線を 2 つの間の接続になります (つまりは直線上にある)。

- 最初の曲線の 2 つ目の制御点
- これは、2 番目の曲線の開始点でも最初の曲線の終点
- 2 番目の曲線の最初の制御点

次の記事で[ **SVG パス データ**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)、スムーズな接続されたベジエ曲線の定義を容易にする機能がわかるでしょう。

3 次ベジエ曲線をレンダリングする、基になるパラメーターの式を把握する便利な場合があります。 *T* 0 ~ 1、パラメーターの式は次のようにします。

x(t) = (t) 1 日 ~ ³x₀ 3t + (1 – t) ²x₁ 3t² + (1 – t) x₂ + t³x₃。

y(t) = (t) 1 日 ~ ³y₀ 3t + (1 – t) ²y₁ 3t² + (1 – t) y₂ + t³y₃。

これらは三次 polynomials 3 の最大指数を確認します。 簡単にすることを確認します。`t`が 0 と等しいポイントは、(x₀ y₀)、これは、開始点とタイミング`t`が 1 に等しいポイントは、(x₃ y₃)、終点であります。 始点の近く (の値が低い`t`)、最初の制御点 (x₁、y₁) は強力な効果し、終了点の方が (' t の値が高い ')、2 つ目のコントロール ポイント (x₂、y₂) が大きな影響を与える。

## <a name="bezier-curve-approximation-to-circular-arcs"></a>円弧を近似をベジエ曲線

円弧のレンダリングにベジエ曲線を使用する便利な場合があります。3 次ベジエ曲線概算の円弧も非常に最大で 4 分の 1 円 4 つの接続されたベジエ曲線は、完全な円を定義できるようします。 この推定は、25 年以上前に公開する 2 つの記事でについて説明します。

> Tor Dokken、「十分な概算の曲率連続のベジエ曲線、によって円の」*コンピューター支援幾何学的デザイン 7* 33 41 (1990)。

> Michael Goldapp、「3 次の Polynomials によって円弧の近似」*コンピューター支援幾何学的デザイン 8* 227 238 (1991)。

次の図は、4 つのポイント ラベル`pto`、 `pt1`、 `pt2`、および`pt3`の円弧を近似するベジエ曲線 (赤で表示) を定義します。

![](beziers-images/bezierarc45.png "ベジエ曲線を円弧の近似")

制御点を始点と終点から行がタンジェントの円におよび、ベジエ曲線の長さがある*L*します。前述の最初の記事では、最善のベジエ曲線が円弧を近似を示すときにその長さ*L*は次のように計算されます。

L = 4 × tan(α/4)/3

L 0.265 ために、図には、45 度の角度を示します。 コードでは、その値は、必要な円の半径が乗算は。

**円弧をベジエ**ページでは、円弧の角度を 180 度までの範囲を概算するベジエ曲線を定義するみたりすることができます。 [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml)ファイルをインスタンス化、`SKCanvasView`と`Slider`角度を選択するためです。 `PaintSurface`内のイベント ハンドラー、 [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs)分離コード ファイルでは、変換を使用して、キャンバスの中央に点 (0, 0) を設定します。 比較については、点を中心に、円を描画しをベジエ曲線の 2 つの制御点を計算します。

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

始点と終点 (`point0`と`point3`)、円の通常のパラメーターの式に基づいて計算されます。 円の中央にあるため (0, 0)、これらのポイントもとして処理できる放射状のベクトルの円の中心から円周をします。 コントロール ポイントは、これらの放射状ベクトルに直角に交わっているので、円のタンジェントである行には。 右の山を別のベクトルとは、元のベクター スワップ X および Y 座標と負の値に加えられたうち 1 つだけです。

さまざまな角度の実行中のプログラムを次に示します。

[![](beziers-images/beziercirculararc-small.png "円弧をベジエ ページのスクリーン ショットをトリプル")](beziers-images/beziercirculararc-large.png#lightbox "円弧をベジエ ページの 3 倍になるスクリーン ショット")

3 番目のスクリーン ショットをよく見るし、角度が 180 度が四半期円うまくを角度が 90 度に合わせて見えます iOS の画面を示しています、半円からにベジエ曲線を外れて顕著なことを確認します。

四半期の円がこのような方向は、この 2 つの制御点の座標を計算することは非常に簡単です。

![](beziers-images/bezierarc90.png "ベジエ曲線で 4 分の 1 円の近似")

100 では、円の半径*L* 55 とを覚えておくの簡単な数では。

**円を 2 乗**ページは、円、四角形の間の図をアニメーション化します。 円がの場合は、この配列定義の最初の列の座標が示すように 4 つのベジエ曲線を近似、 [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs)クラス。

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

2 番目の列には、領域を持つが、円の面積と同じでは約正方形を定義する 4 つのベジエ曲線の座標が含まれています。 (に付いた四角形を描画、*正確な*領域に指定した円のクラシック解決不可能な幾何学的問題は、[円を 2 乗](https://en.wikipedia.org/wiki/Squaring_the_circle))。ベジエ曲線に付いた四角形をレンダリングするためには、各曲線の 2 つの制御点は同じですとベジエ曲線は直線として表示されるためでは、始点と終点を同一線上には。

配列の 3 番目の列は、アニメーションの補間値です。 ページは、16 のミリ秒のタイマーを設定、`PaintSurface`ペースがハンドラーが呼び出されます。

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

Sinusoidally 不安定な値に基づいて、ポイントが補間`t`します。 挿入ポイントは、一連の 4 つの接続されたベジエ曲線を作成し、使用されます。 アニメーションの実行を次に示します。

[![](beziers-images/squaringthecircle-small.png "Squaring の 3 倍になるスクリーン ショット円ページ")](beziers-images/squaringthecircle-large.png#lightbox "Squaring の 3 倍になるスクリーン ショット、[円] ページ")

このようなアニメーションがなければ曲線は弧と直線の両方としてレンダリングされる柔軟性アルゴリズムになります。

**ベジエ無限大**ページのもかかるおおよその円弧をベジエ曲線の機能を利用します。ここでは、`PaintSurface`からハンドラー、 [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs)クラス。

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

関係を表示するグラフ用紙にこれらの座標をプロットする良い練習が考えられます。 無限大記号の中心点 (0, 0) と 2 つのループのセンターがある (–150, 0) と (150, 0) と 100 の半径。 一連の`CubicTo`コマンドで確認できます –95 と –205 の値の制御点の座標 X (これらの値はプラス記号とマイナス 55 –150)、205 と 95 (150 プラス記号とマイナス 55) だけでなく 250 –250 右辺と左辺をします。 無限大記号はセンターが超えたときに、例外です。 その場合は、コントロール ポイントでは、中心に近い曲線をまっすぐに 50 と –50 の組み合わせで座標があります。

無限大記号を次に示します。

[![](beziers-images/bezierinfinity-small.png "ベジエ無限大ページのスクリーン ショットをトリプル")](beziers-images/bezierinfinity-large.png#lightbox "ベジエ無限大ページの 3 倍になるスクリーン ショット")

によって表示される、無限大記号よりも中央方向に円滑になります、**円弧無限大**ページから、 [**円弧を描画する方法は 3 つ**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)記事。

## <a name="the-quadratic-bzier-curve"></a>2 次ベジエ曲線

2 次ベジエ曲線が 1 つだけの制御点と曲線が 3 つの点によって定義されます。 開始点、コントロール ポイントと終了点。 パラメーターの式は 3 次ベジエ曲線と非常に似ていますが、最高の指数部は 2、曲線は 2 次多項式。

x(t) = (1 – t)²x₀ + 2t(1 – t)x₁ + t²x₂

y(t) = (1 – t)²y₀ + 2t(1 – t)y₁ + t²y₂

パスには、2 次ベジエ曲線を追加するには、使用、 [ `QuadTo` ](xref:SkiaSharp.SKPath.QuadTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint))メソッドまたは[ `QuadTo` ](xref:SkiaSharp.SKPath.QuadTo(System.Single,System.Single,System.Single,System.Single))個別にオーバー ロード`x`と`y`座標。

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

メソッドは、現在位置から曲線を追加`point2`で`point1`コントロール ポイントとして。

2 次ベジエ曲線を試すことができます、**二次曲線**ページとよく似ていますが、**ベジエ曲線**ページのみの 3 つのタッチ点がある点が異なります。 ここでは、`PaintSurface`ハンドラーで、 [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs)分離コード ファイル。

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

[![](beziers-images/quadraticcurve-small.png "二次曲線ページのスクリーン ショットをトリプル")](beziers-images/quadraticcurve-large.png#lightbox "二次曲線ページの 3 倍になるスクリーン ショット")

点線では、始点と終点の曲線の接線は、コントロール ポイントで交差します。

全般的な形式では、曲線を作成する必要がありますが、2 つではなく 1 つのコントロール ポイントの利便性を使用する場合は、2 次ベジエをお勧めします。 2 次ベジエを他の曲線、楕円の円弧を表示するために Skia で内部的に使用される理由より効率的に表示します。

ただし、2 次ベジエ曲線の形状は楕円、これが複数の 2 次ベジエ スプラインは楕円の円弧を概算するために必要な理由です。2 次ベジエは、代わりに、放物線のセグメントです。

## <a name="the-conic-bzier-curve"></a>円錐のベジエ曲線

円錐のベジエ曲線&mdash;合理的に 2 次ベジエ曲線とも呼ばれます&mdash;は比較的最近のベジエ曲線のファミリに追加します。 、2 次ベジエ曲線のように、有理数の 2 次ベジエ曲線には開始点、終点、および 1 つのコントロール ポイントが含まれます。 有理数の 2 次ベジエ曲線も必要ですが、*重み*値。 呼び出された、*合理的な*パラメトリック数式が比率が関与するために 2 次です。

パラメーターの式 X と Y は、同じ分母を共有する比率。 ここでは、式の分母を*t* 0 から 1 を重み値*w*:

d(t) = (1 – t)² + 2wt(1 – t) + t²

理論上は、合理的な二次方程式は 3 つの用語ごとに 1 つ、3 つの個別の重み付け値を伴うことができますが、これらを 1 つの重み値の中間期間に簡略化できます。

中間という用語は、重みの値も含まれています。 および、式は、分母で除算する点を除いて、X と Y 座標のパラメーターの式は 2 次ベジエのパラメーターの式に似ています。

x(t) = ((1 – t)²x₀ + 2wt(1 – t)x₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t)²y₀ + 2wt(1 – t)y₁ + t²y₂)) ÷ d(t)

有理数の 2 次ベジエ曲線とも呼ばれます*conics*円錐セクションのセグメントを正確に表現するため、 &mdash; hyperbolas、放物線、省略記号、および円。

パスには、有理数の 2 次ベジエ曲線を追加するには、使用、 [ `ConicTo` ](xref:SkiaSharp.SKPath.ConicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single))メソッドまたは[ `ConicTo` ](xref:SkiaSharp.SKPath.ConicTo(System.Single,System.Single,System.Single,System.Single,System.Single))個別にオーバー ロード`x`と`y`座標。

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

最後に注意してください`weight`パラメーター。

**円錐曲線** ページでは、これらの曲線を実験できます。 `ConicCurvePage` クラスは `InteractivePage` から派生したものです。 [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml)ファイルをインスタンス化、 `Slider` – 2 と 2 の重み値を選択します。 [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) 3 つの分離コード ファイルが作成`TouchPoint`オブジェクト、および`PaintSurface`ハンドラーは単にコントロールに接線で結果の曲線をレンダリングしますポイント:

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

[![](beziers-images/coniccurve-small.png "曲線の円錐のページのスクリーン ショットをトリプル")](beziers-images/coniccurve-large.png#lightbox "円錐曲線ページの 3 倍になるスクリーン ショット")

ご覧のように、コントロール ポイントは、重みが大きいときに詳細が曲線をプルするようです。 重みが 0 の場合、曲線は始点から終点を直線になります。

理論上は、負の重みを許可、および修正する曲線が発生する*離れた*コントロール ポイントから。 ただし、重み付け – 1 の場合、または以下の原因の特定の値の場合は負になるには、パラメーターの式の分母*t*します。 おそらくこのため、負の重みでは無視されます、`ConicTo`メソッド。 **円錐曲線**プログラムでは、負の重みを設定することができますが、負の重みは重みを 0 と同じ効果があるし、表示するには、直線が発生するで試してみるご覧のとおりです。

コントロール ポイントを使用する重みを派生させるのには非常に簡単、`ConicTo`最大の円弧を描画するメソッド (ただしを除く)、半円形。 次の図では、始点と終点の接線は、コントロール ポイントに満たしています。

![](beziers-images/conicarc.png "円弧の円錐円弧レンダリング")

制御点。 円の中心からの距離を調べるに三角関数を使用することができます: が半分の角度 α のコサインの値で除算する円の半径。 始点と終点の間の円弧を描画するには、その同じコサイン半分の角度の重みを設定します。 角度が 180 度の場合は、接線を満たすことはありませんし、重みは 0 に注意してください。 角度が 180 度未満、数学で正しく動作します。

**円錐の円弧** ページでは、この例を示します。 [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml)ファイルをインスタンス化、`Slider`角度を選択するためです。 `PaintSurface`ハンドラーで、 [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs)分離コード ファイルの制御点と重量を計算します。

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

ビジュアルの違いはありません、ご覧のとおり、`ConicTo`赤で表示されているパスと基になる円参照用として表示します。

[![](beziers-images/coniccirculararc-small.png "円錐の円弧のページのスクリーン ショットをトリプル")](beziers-images/coniccirculararc-large.png#lightbox "円錐の円弧のページの 3 倍になるスクリーン ショット")

角度を 180 度、および数学失敗に設定します。

残念ながら、ここでを`ConicTo`理論上は (パラメーターの式に基づく)、円を別の呼び出しをで実行できるため、負の重みをサポートしません`ConicTo`で同じポイントが、重みの負の値。 これは、2 つだけで完全な円を作成するようにすると`ConicTo`間 (が含まれていない) すべての角度に基づいて曲線度、180 度は 0。


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
