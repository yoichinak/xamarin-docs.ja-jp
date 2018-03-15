---
title: "次の 3 つの種類のベジエ曲線"
description: "SkiaSharp を円錐、二次方程式、3 次ベジエ曲線を表示するために使用する方法について解説します。"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: charlespetzold
ms.author: chape
ms.date: 05/25/2017
ms.openlocfilehash: dcfcf43c89f26b4e721c9752b9cbad1f4a30cfc2
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="three-types-of-bzier-curves"></a>次の 3 つの種類のベジエ曲線

_SkiaSharp を円錐、二次方程式、3 次ベジエ曲線を表示するために使用する方法について解説します。_

ベジエ曲線がサンピエール ベジエ (1910: 1999)、車の本文のコンピューター支援型のデザインの曲線を使用した Renault 自動車企業のフランス語のエンジニア リング後という名前です。

ベジエ曲線が対話型のデザインに最適であるため呼ばれる: はうまく動作するように&mdash;言い換えれば、無限であるか、または長すぎて扱いに曲線が発生する特異がない&mdash;され、通常、見栄えの良い. 通常、コンピューター ベースのフォントの文字のアウトラインはベジエ曲線を含む定義します。

![](beziers-images/beziersample.png "サンプルのベジエ曲線")

に関する Wikipedia の記事[ベジエ曲線](https://en.wikipedia.org/wiki/B%C3%A9zier_curve)有用な情報が含まれています。 用語*ベジエ曲線*のような曲線のファミリを実際に参照します。 SkiaSharp と呼ばれるベジエ曲線の 3 種類のサポート、*立方*、*二次*、および*円錐*です。 円錐とも呼ばれます、*有理数二次方程式*です。

## <a name="the-cubic-bzier-curve"></a>3 次ベジエ曲線

3 次は、ベジエ曲線をベジエ曲線のサブジェクトが表示されたらのほとんどの開発者と思われる型です。

3 次ベジエ曲線を追加することができます、`SKPath`オブジェクトを使用して、 [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/SkiaSharp.SKPoint/)を 3 つのメソッド`SKPoint`パラメーター、または[ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/System.Single/) でオーバーロード`x`と`y`パラメーター。

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

曲線を輪郭の現在の時点で開始します。 完全な 3 次ベジエ曲線は、4 つの点によって定義されます。

- 始点: 現在のポイント、線、または (0, 0) で場合`MoveTo`が呼び出されていません。
- 最初のコントロール ポイント:`point1`で、`CubicTo`を呼び出す
- 2 つ目のコントロール ポイント:`point2`で、`CubicTo`を呼び出す
- エンド ポイント:`point3`で、`CubicTo`を呼び出す

結果の曲線は、開始した時点、終点で終わります。 曲線通常を通過しません、2 つの制御ポイントです。代わりにそれらの方向に曲線をプルする程度 like 磁石機能します。

3 次ベジエ曲線の把握する最善の方法は、実験でです。 目的は、これは、**ベジエ曲線** ページから派生する`InteractivePage`です。 [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml)ファイルをインスタンス化、`SKCanvasView`と`TouchEffect`です。 [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs)分離コード ファイルは、4 つ作成`TouchPoint`コンス トラクター内のオブジェクト。 `PaintSurface`イベント ハンドラーを作成、`SKPath`基に、4 つのベジエ曲線を表示するために`TouchPoint`オブジェクトし、も管理ポイントから終了ポイントにドット区切りの接線を描画します。

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

ここで 3 つすべてのプラットフォームで実行されています。

[![](beziers-images/beziercurve-small.png "ベジエ曲線のページのスクリーン ショットをトリプル")](beziers-images/beziercurve-large.png#lightbox "ベジエ曲線のページのトリプル スクリーン ショット")

数学的に、曲線では、3 次多項式近似です。 曲線では、3 つの点に直線が多くてと交差します。 開始時点では、曲線は常に最初の制御点を正接しと方向が同じでは、開始から直線ポイントです。 曲線を常には終了時点では、終了点に接線のと同じ方向に、2 つ目のコントロールから直線ポイントです。

3 次ベジエ曲線に凸四角形の 4 つの点を結ぶ常に制限されます。 これと呼ばれる、*凸包*です。 コントロール ポイントは、始点と終点の直線上にあるを場合は、直線としてベジエ曲線が表示されます。 曲線できますも伴う自体には、Windows Mobile デバイスのスクリーン ショットに示すようです。

パス輪郭は複数の立方の接続されたベジエ曲線を含めることができますが、3 次ベジエ曲線を 2 つの間の接続は次の 3 つのポイントが同一線上に場合にのみ、スムーズになります (つまり、上に存在する直線)。

- 最初の曲線の 2 つ目のコントロール ポイント
- これは、2 番目の曲線の開始点でも最初の曲線の終了点
- 2 番目の曲線の最初の制御点

次の記事で[ **SVG パス データ**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)滑らかな接続のベジエ曲線の定義を簡単にする機能を紹介します。

3 次ベジエ曲線をレンダリングする基になるパラメーター式を知っておくと便利場合があります。 *T* 0 ~ 1、パラメーター型の式は、次のようにします。

x(t) = (1 – t)³x₀ + 3t(1 – t)²x₁ + 3t²(1 – t)x₂ + t³x₃

y(t) = (1 – t)³y₀ + 3t(1 – t)²y₁ + 3t²(1 – t)y₂ + t³y₃

3 の最大指数は、これらが立方 polynomials であることを確認します。 簡単にすることを確認してください`t`0 に等しいポイントは、(x₀、y₀)、これは、始点とタイミング`t`は 1 に等しいポイントは、(x₃、y₃)、終点であります。 始点を付近 (の値が低`t`)、最初のコントロール ポイント (x₁、y₁) が、強力な効果し、終了点をほぼ (' t の値が高い ') (x₂、y₂)、2 番目の制御点が大きな影響を与える。

## <a name="bzier-curve-approximation-to-circular-arcs"></a>円弧をベジエ曲線近似

ベジエ曲線を円弧を表示するために使用すると便利な場合があります。3 次ベジエ曲線を概算できる円弧よく四半期円までため、接続された 4 つのベジエ曲線に完全な円を定義できます。 この概算値が 2 つのアーティクルを 25 年以上前のパブリッシュについて説明します。

> Tor Dokken、et al「曲率連続のベジエ曲線を円の適切に近似」*コンピューター支援幾何学的デザイン 7* (1990)、33 41。

> Michael Goldapp、「立方 Polynomials、によって円弧の近似」*コンピューター支援幾何学的デザイン 8* 227 238 (1991)。

次の図に、4 つのポイント ラベルの付いた`pto`、 `pt1`、 `pt2`、および`pt3`概算するための円弧をベジエ曲線 (赤で表示) を定義します。

![](beziers-images/bezierarc45.png "円弧をベジエ曲線に近似")

始点と終点のコントロール ポイントへの行は、タンジェント円にしをベジエ曲線にある、および長さの*L*です。前述の最初の記事が最良のベジエ曲線が円弧を近似を示すときにその長さ*L*次のように計算されます。

L = 4 × tan(α/4)/3

L ハイパーボリック 0.265、45 度の角度を示します。 コードでは、必要な円の半径によってその値を乗算するは。

**円弧をベジエ** ページでは、おおよその最大 180 ° の範囲の角度の円弧をベジエ曲線を定義するをテストすることができます。 [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml)ファイルをインスタンス化、`SKCanvasView`と`Slider`の角度を選択します。 `PaintSurface`内のイベント ハンドラー、 [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs)分離コード ファイルでは、変換を使用して、キャンバスの中央に点 (0, 0) を設定します。 比較については、そのポイントを中心と円を描画し、ベジエ曲線の 2 つの制御点を計算します。

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

始点と終点 (`point0`と`point3`) は、円の通常のパラメーター型数式に基づいて計算されます。 円の中心があるため (0, 0)、これらのポイント扱うことも放射状のベクトルとして、円の中心から円周にします。 コントロール ポイントは、直角これら放射状ベクトルには、円の接線の行には。 直角に単に、元ベクトルを X 座標と Y 座標をスワップと負の値で行われたこれらのいずれかを使用します。

次の 3 つの異なる角度の 3 つのプラットフォームで実行されているプログラムを次に示します。

[![](beziers-images/beziercirculararc-small.png "円弧をベジエ ページのスクリーン ショットをトリプル")](beziers-images/beziercirculararc-large.png#lightbox "円弧をベジエ ページのトリプル スクリーン ショット")

Windows Mobile 画面について詳しく見てし、角度が 180 度は、その場合場合に合わせて正しく四半期円角度が 90 度思えます iOS の画面を示しています、半円からにベジエ曲線を外れて顕著なことを確認します。

四半期の円が次のように指向の場合は、この 2 つの制御点の座標を計算することは非常に簡単です。

![](beziers-images/bezierarc90.png "ベジエ曲線に四半期円の近似")

100 では、円の半径*L* 55 とは、簡単な数に注意してください。

**円を 2 乗**ページには、円、四角形の間の図がアニメーション化します。 円がこの配列内の定義の最初の列の座標が示すように 4 つのベジエ曲線によって近似された形状、 [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs)クラス。

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

2 番目の列には、領域を持つは、円の領域と同じでは約正方形を定義する 4 つのベジエ曲線の座標が含まれています。 (に付いた四角形の描画、*正確な*として指定した円の領域は、典型的解決不可能な幾何学的な問題の[円を 2 乗](https://en.wikipedia.org/wiki/Squaring_the_circle))。ベジエ曲線に付いた四角形を表示するため各曲線の 2 つの制御ポイントは、同じ、され、同一線上に始点と終点をベジエ曲線が直線として表示されるようにします。

配列の 3 番目の列は、アニメーションの補間値です。 ページは、16 のミリ秒のタイマーを設定し、`PaintSurface`ペースがハンドラーが呼び出されます。

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

Sinusoidally 不安定な値に基づいて、ポイントが補間`t`です。 補間のポイントは、一連の接続された 4 つのベジエ曲線を構築するために使用されます。 正方形に円から進行状況を示す 3 つのプラットフォームで実行されているアニメーションを次に示します。

[![](beziers-images/squaringthecircle-small.png "トリプル スクリーン ショット、Squaring 円ページ")](beziers-images/squaringthecircle-large.png#lightbox "Squaring のトリプル スクリーン ショット円 ページ")

このようなアニメーションはアルゴリズムによっては柔軟なので円弧と直線の両方としてレンダリングされる曲線なしで実行できなくなります。

**ベジエ無限大**ページのおおよその円弧をベジエ曲線の機能を使用してもかかります。ここでは、`PaintSurface`からハンドラー、 [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs)クラス。

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

関連性を表示するグラフ用紙にこれらの座標をプロットする適切な手順があります。 無限大記号が (0, 0) のポイントを中心としたいて、2 つのループの中央 (–150, 0) と (150, 0) および 100 の半径。 一連の`CubicTo`コマンドを表示できます X –95 と –205 の値に基づいて実行の制御点の座標 (それらの値は –150 プラスとマイナス 55)、205 および 95 (150 プラスとマイナス 55) だけでなく 250 と右と左の辺の –250 します。 唯一の例外は、無限大記号は中央に交差する場合です。 その場合は、コントロール ポイントを中心に近い曲線まっすぐには、50 および –50 の組み合わせで座標にあります。

次の 3 つすべてのプラットフォームで、無限大記号を次に示します。

[![](beziers-images/bezierinfinity-small.png "ベジエ無限大ページのスクリーン ショットをトリプル")](beziers-images/bezierinfinity-large.png#lightbox "ベジエ無限大ページのトリプル スクリーン ショット")

によって表示される、無限大記号よりも中央方向にある程度スムーズには、**円弧無限大**からページ、 [**円弧を描画する方法は 3 つ**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)資料です。

## <a name="the-quadratic-bzier-curve"></a>2 次ベジエ曲線

2 次ベジエ曲線の 1 つだけの制御点があり、曲線が 3 つの点によって定義されます: 始点、コントロール ポイントと終了点です。 パラメーターの式は、最高の指数部は 2、曲線は 2 次多項式近似する点を除いて、3 次ベジエ曲線とよく似ています。

x(t) = (1 – t)²x₀ + 2t(1 – t)x₁ + t²x₂

y(t) = (1 – t)²y₀ + 2t(1 – t)y₁ + t²y₂

パスに 2 次ベジエ曲線を追加するには、使用、 [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/)メソッドまたは[ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/System.Single/System.Single/System.Single/System.Single/)独立したオーバー ロードを`x`と`y`座標。

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

メソッドでは、曲線を追加する現在の位置から`point2`で`point1`コントロール ポイントとして。

2 次ベジエ曲線を試すことができます、**二次曲線**ページとよく似ていますが、**ベジエ曲線**ページのみの 3 つのタッチ ポイントがある点が異なります。 ここでは、`PaintSurface`ハンドラー、 [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs)分離コード ファイル。

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

ここで 3 つすべてのプラットフォームで実行されています。

[![](beziers-images/quadraticcurve-small.png "二次曲線 ページのスクリーン ショットをトリプル")](beziers-images/quadraticcurve-large.png#lightbox "二次曲線 ページのトリプル スクリーン ショット")

点線は、正接を開始点と終了ポイントを曲線にし、コントロールの時点を満たします。

全般的な形式では、曲線を作成する必要がありますが、2 つではなく 1 つのコントロール ポイントの利便性したい場合は、二次方程式のベジエをお勧めします。 2 次ベジエで使用されている内部 Skia 楕円の円弧をレンダリングするための他の曲線より効率的に表示します。

ただし、2 次ベジエ曲線の形状が楕円、これが複数の 2 次ベジエ スプラインが楕円の円弧を近似に必要な理由2 次ベジエは、代わりに、放物線のセグメントです。

## <a name="the-conic-bzier-curve"></a>円錐のベジエ曲線

円錐のベジエ曲線&mdash;合理的に 2 次ベジエ曲線とも呼ばれる&mdash;は比較的最近のベジエ曲線のファミリに追加します。 2 次のベジエ曲線のような合理性のある 2 次ベジエ曲線には、始点、終点、および 1 つのコントロール ポイントが含まれます。 有理数 2 次ベジエ曲線も必要ですが、*重み*値。 呼び出された、*有理数*パラメーター型数式には、比率が含まれまるため二次です。

パラメーターの式の X と Y が、同じ分母を共有する比率。 ここでは、式の分母*t* 0 から 1 の重みの値を*w*:

d(t) = (1 – t)² + 2wt(1 – t) + t²

理論上は、有理数二次方程式に使用できる 3 つの用語ごとに 1 つ、3 つの個別の重み付け値が中間語に重みの値を 1 つだけにこれらを簡略化できます。

X と Y 座標のパラメーター型の式は、中間という用語は、重みの値も含まれています。 と式を除数で除算する点を除いて 2 次ベジエのパラメーター型の式に似ています。

x(t) = ((1 – t)²x₀ + 2wt(1 – t)x₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t)²y₀ + 2wt(1 – t)y₁ + t²y₂)) ÷ d(t)

有理数の 2 次ベジエ曲線とも呼ばれる*conics*円錐セクションのセグメントを正確に表すことができるため&mdash;hyperbolas、parabolas、省略記号、および円です。

有理数 2 次ベジエ曲線をパスに追加するには、使用、 [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/)メソッドまたは[ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/)独立したオーバー ロードを`x`と`y`座標。

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

最後に注意してください`weight`パラメーター。

**円錐曲線** ページでは、これらの曲線をテストすることができます。 `ConicCurvePage` クラスは `InteractivePage` から派生したものです。 [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml)ファイルをインスタンス化、 `Slider` – 2 と 2 の重み値を選択します。 [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs)分離コード ファイルでは、3 つが作成されます`TouchPoint`オブジェクト、および`PaintSurface`ハンドラーは、コントロールに接線で結果の曲線を単にレンダリングポイント:

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

ここで 3 つすべてのプラットフォームで実行されています。

[![](beziers-images/coniccurve-small.png "円錐曲線 ページのスクリーン ショットをトリプル")](beziers-images/coniccurve-large.png#lightbox "円錐曲線 ページのトリプル スクリーン ショット")

ご覧のように、コントロール ポイントは、重みが大きい場合に、さらにこれに対する曲線をプルするようです。 重みが 0 の場合は、始点から終点に直線が曲線になります。

理論上は、負の重みが許可され、曲げる曲線が発生する*離れた*コントロール ポイントから。 ただし、重みの重みが-1 の以下の原因になる特定の値の負の値パラメーターの式の分母*t*です。 おそらくこのため、負の重みのでは無視されます、`ConicTo`メソッドです。 **円錐曲線**プログラムでは、負の重みを設定できます。 のものですがわかるように試すことによって、負の重みのゼロの重みと同じ効果がある直線を表示します。

非常に簡単コントロール ポイントを使用する重みを派生させるのには、`ConicTo`最大円弧を描画するメソッド (は含まれません) 半円します。 次の図では、始点と終点から接線出会ったは、コントロール ポイントでします。

![](beziers-images/conicarc.png "円弧の円錐円弧レンダリング")

円の中心からのコントロール ポイントの距離を調べるに三角関数を使用することができます: α する半分の角度のコサインの値で割った値円の半径はします。 始点と終点の間の円弧を描画するには、半分の角度の同じそのコサインを重みを設定します。 角度が 180 度の場合は、接線を満たすことはありませんし、重みが 0 に注意してください。 より小さい角度 180 ° の計算は問題なく動作します。

**円錐円弧** ページでは、この動作について説明します。 [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml)ファイルをインスタンス化、`Slider`の角度を選択します。 `PaintSurface`ハンドラー、 [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs)分離コード ファイルは、管理ポイントと重みを計算します。

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

ビジュアルの違いはありませんが表示されるよう、`ConicTo`パスが赤で表示し、基になる円を表示して参照します。

[![](beziers-images/coniccirculararc-small.png "円錐円弧ページのスクリーン ショットをトリプル")](beziers-images/coniccirculararc-large.png#lightbox "円錐円弧ページのトリプル スクリーン ショット")

180 度、および数学失敗に角度を設定します。

にない残念ながら、ここでを`ConicTo`理論上は (パラメーターの式に基づく)、円を別の呼び出しで完了できるため、負の重みをサポートしません`ConicTo`で同じポイントが、重量の負の値。 これは、2 つの完全な円を作成するようにすると`ConicTo`曲線が任意の角度との間 (含みません) に基づく度、180 度は 0 です。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
