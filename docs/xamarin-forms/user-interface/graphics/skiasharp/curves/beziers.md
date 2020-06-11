---
title: "3 種類のベジエ曲線" の説明: "この記事では、SkiaSharp を使用して、アプリケーションで3次ベジエ曲線および円錐ベジエ曲線を描画する方法について説明 Xamarin.Forms し、サンプルコードを使用してこれを示します。"
ms. 製品: xamarin ms テクノロジ: skiasharp: 8FE0F6DC-16BC-435F-9626-DD1790C0145A author: davidbritch ms. author: dabritch ms. date: 05/25/2017 no loc: [, & # 3. Xamarin.Forms Xamarin.Essentials
---

# <a name="three-types-of-bzier-curves"></a>3 つの種類のベジエ曲線

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp を使用して、3次ベジエ曲線を描画する方法について説明します。_

ベジエ曲線には、ピエール・ Renault のフランスのエンジニアであるピエールベジェ (1910 – 1999) の名前が付けられています。これは、自動車メーカーのコンピューター支援型の設計に曲線を使用したものです。

ベジエ曲線は、対話的な設計に適していることがわかっています。つまり、相互に正常に動作しています。つまり &mdash; 、曲線が無限または扱いにくくなり、通常は見た目になり &mdash; ます。

![サンプルのベジエ曲線](beziers-images/beziersample.png)

コンピューターベースのフォントの文字アウトラインは、通常、ベジエ曲線で定義されます。

[**ベジエ曲線**](https://en.wikipedia.org/wiki/B%C3%A9zier_curve)の Wikipedia の記事には、いくつかの便利な背景情報が含まれています。 *ベジエ曲線*という用語は、実際には同様の曲線のファミリを指します。 SkiaSharp*は、* 3 種類のベジエ曲線 (3*種類のベジエ*曲線) をサポート*しています。* 円錐は、*有理数*とも呼ばれます。

## <a name="the-cubic-bzier-curve"></a>3次ベジエ曲線

3種類はベジエ曲線の種類であり、ほとんどの開発者がベジエ曲線のサブジェクトが登場したときに考えられます。

`SKPath`3 つのパラメーターを持つメソッドを使用して、 [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKPoint)) `SKPoint` または [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo(System.Single,System.Single,System.Single,System.Single,System.Single,System.Single)) 個別のパラメーターとパラメーターを持つオーバーロードを使用して、3次ベジエ曲線をオブジェクトに追加でき `x` `y` ます。

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

曲線は、輪郭の現在の点から開始します。 3番目のベジエ曲線は、次の4つの点によって定義されます。

- 開始点: 輪郭の現在のポイント。 `MoveTo` が呼び出されていない場合は (0, 0)
- 最初の制御点: `point1` `CubicTo` 呼び出しで
- 2番目の制御点: `point2` `CubicTo`
- エンドポイント: `point3` 通話中 `CubicTo`

結果の曲線は始点から開始し、終点で終了します。 通常、曲線は2つの制御点を通過しません。代わりに、コントロールポイントはマグネットとよく似ていて、曲線を引きます。

3次ベジエ曲線の感覚を得る最良の方法は実験です。 これは、から派生する [**ベジエ曲線**] ページの目的です `InteractivePage` 。 [**BezierCurvePage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml)ファイルは、とをインスタンス化します `SKCanvasView` `TouchEffect` 。 [**BezierCurvePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs)分離コードファイルは、コンストラクターに4つのオブジェクトを作成し `TouchPoint` ます。 `PaintSurface`イベントハンドラーは、 `SKPath` 4 つのオブジェクトに基づいてベジエ曲線を描画するを作成し、さらに、 `TouchPoint` コントロールポイントからエンドポイントまでの点線のタンジェント線を描画します。

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

次のように実行されています。

[![[ベジエ曲線] ページのトリプルスクリーンショット](beziers-images/beziercurve-small.png)](beziers-images/beziercurve-large.png#lightbox)

数学的には、曲線は3次多項式になります。 曲線は、最大3点で直線と交差します。 開始点では、曲線は常に始点から最初の制御点までの直線と同じ方向に接しています。 エンドポイントでは、曲線は常に、2番目の制御点から終点までの直線と同じ方向に接しています。

3次ベジエ曲線は、常に4つの点を結ぶ凸状の四角形によって制限されます。 これは、*凸ハル*と呼ばれます。 コントロールポイントが始点と終点の間の直線上にある場合、ベジエ曲線は直線としてレンダリングされます。 3番目のスクリーンショットに示すように、曲線もそれ自体を越えることができます。

パスの輪郭には、複数の接続された3次ベジエ曲線を含めることができますが、2つの三次ベジエ曲線間の接続は、次の3つの点が同じである (つまり、直線上にある) 場合にのみ、スムーズになります。

- 最初の曲線の2番目の制御点。
- 最初の曲線の終点。2番目の曲線の始点でもあります。
- 2番目の曲線の最初の制御点

[**SVG パスデータ**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)に関する次の記事では、スムーズに接続されたベジエ曲線の定義を容易にする機能について説明します。

3次ベジエ曲線を描画する、基になるパラメーター式を理解しておくと便利な場合があります。 0から1までの*t*の場合、パラメーター式は次のようになります。

x (t) = (1 – t) ³ x ₀ + 3t (1 – t) ² x ₁ + 3t ² (1 ~ t) x ₂ + t ³ x ₃

y (t) = (1 – t) ³ y ₀ + 3t (1 – t) ² y ₁ + 3t ² (1 ~ t) y ₂ + t ³ y ₃

3の最大指数は、3乗されていることを確認します。3つの polynomials です。 が `t` 0 の場合、ポイント (x ₀, y ₀) が始点であり、が1の場合は、 `t` 終点が (x ₃, y ₃) であることを確認するのが簡単です。 始点の近く (の値が小さい場合 `t` ) は、最初の制御点 (x ₁、y ₁) が強い効果を持ち、エンドポイント (値が ' t ' の値が大きい) の近くで2番目の制御点 (x ₂, y ₂) が強い効果を持ちます。

## <a name="bezier-curve-approximation-to-circular-arcs"></a>円弧から円弧までの近似曲線

ベジエ曲線を使用して円弧を描画すると便利な場合があります。3次ベジエ曲線では、円の弧が非常に大きくなる可能性があります。そのため、4つの接続されたベジエ曲線で円全体を定義できます。 この概数については、25年前に公開された2つの記事で説明されています。

> Tor Dokken は、"*コンピューター支援ジオメトリ設計 7* (1990), 33-41" という、"曲率連続のベジエ曲線による丸の良い近似値" です。

> Michael Goldapp、"3 次的 Polynomials による円弧の近似値"、"*コンピューター支援型のジオメトリック設計 8* (1991)、227-238。

次の図は、、、およびというラベルが付いた4つの点を示してい `pto` `pt1` `pt2` `pt3` ます。ベジエ曲線 (赤で示されています) を定義します。

![ベジエ曲線を持つ円弧の近似値](beziers-images/bezierarc45.png)

開始位置から制御点までの線は、円とベジエ曲線に接し、長さは*L*になっています。上記の最初の記事では、この長さの*L*が次のように計算される場合に、ベジエ曲線が円弧に近似されることを示しています。

L = 4 × tan (α/4)/3

この図は45度の角度を示しているため、L は0.265 になります。 コードでは、その値に円の必要な半径が乗算されます。

[**ベジエ円弧**] ページを使用すると、ベジエ曲線を定義して実験し、最大180°の角度を円弧に近似できます。 [**BezierCircularArcPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml)ファイルは、 `SKCanvasView` 角度を選択する `Slider` ためにとをインスタンス化します。 `PaintSurface` [**BezierCircularArgPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs)分離コードファイルのイベントハンドラーは、変換を使用して、ポイント (0, 0) をキャンバスの中央に設定します。 比較のためにそのポイントの中央に円を描画し、次にベジエ曲線の2つの制御点を計算します。

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

始点と終点 ( `point0` および `point3` ) は、円の通常のパラメーター式に基づいて計算されます。 円の中心が (0, 0) であるため、これらの点は円の中心から円周までの放射状ベクトルとして処理することもできます。 コントロールポイントは、円に接している線の上にあるため、これらの放射状ベクトルに対しては直角に位置します。 逆方向のベクターは、単に X 座標と Y 座標が交換された元のベクトルであり、そのうちの1つが負の値になります。

さまざまな角度で実行されるプログラムは次のようになります。

[![[ベジエ円弧] ページのトリプルスクリーンショット](beziers-images/beziercirculararc-small.png)](beziers-images/beziercirculararc-large.png#lightbox)

3番目のスクリーンショットをよく見てください。ベジエ曲線は、角度が180度のときに半円とは特に異なることがわかりますが、iOS の画面では、角度が90度の場合には、円の丸のように表示されているように見えます。

次のように、2つの制御点の座標を計算するのは、四半期の円が次のようになると非常に簡単です。

![ベジエ曲線を使用した四半期の円の近似値](beziers-images/bezierarc90.png)

円の半径が100の場合、 *L*は55であり、覚えやすい数値です。

**2 乗**は、円と正方形の間の図形をアニメーション化します。 円は、次のように、クラスのこの配列定義の最初の列に座標が表示される4つのベジエ曲線によって近似され [`SquaringTheCirclePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) ます。

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

2番目の列には、4つのベジエ曲線の座標が含まれています。これは、円の面積とほぼ同じ領域を持つ正方形を定義します。 (特定の円として*正確*な領域を持つ正方形を描画することは、[円を2乗](https://en.wikipedia.org/wiki/Squaring_the_circle)とする従来の解決不可能なジオメトリックの問題です)。ベジエ曲線で正方形をレンダリングする場合、各曲線の2つの制御点は同じであり、始点と終点が線形であるため、ベジエ曲線は直線としてレンダリングされます。

配列の3番目の列は、アニメーションの補間値用です。 このページでは、タイマーを16ミリ秒に設定し、 `PaintSurface` その速度でハンドラーが呼び出されます。

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

点は、sinusoidally の不安定な値に基づいて補間され `t` ます。 次に、補間されたポイントを使用して、4つの接続されたベジエ曲線を作成します。 次に、実行中のアニメーションを示します。

[![2乗の [円] ページのトリプルスクリーンショット](beziers-images/squaringthecircle-small.png)](beziers-images/squaringthecircle-large.png#lightbox)

このようなアニメーションは、円弧と直線の両方としてレンダリングするのに十分なアルゴリズムの曲線を使用することはできません。

また、[**ベジエ無限大**] ページでは、ベジエ曲線の機能を利用して円弧を概数化できます。クラスのハンドラーを次に示し `PaintSurface` [`BezierInfinityPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) ます。

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

これらの座標をグラフの紙にプロットして、それらがどのように関連しているかを確認することをお勧めします。 無限大記号はポイント (0, 0) の中心にあり、2つのループの中心は (– 150, 0) と (150, 0)、および100の半径です。 一連のコマンドでは、 `CubicTo` -95 と–205の値 (これらの値は、–150プラスおよびマイナス 55)、205と 95 (150 およびマイナス 55)、および右側と左側の250および–250を使用して、制御点の X 座標を確認できます。 唯一の例外は、無限大記号が中央で交差する場合です。 その場合、コントロールポイントには、中心の近くの曲線を回転させるために、50と–50を組み合わせた座標があります。

無限大記号は次のようになります。

[![ベジエ無限大ページのトリプルスクリーンショット](beziers-images/bezierinfinity-small.png)](beziers-images/bezierinfinity-large.png#lightbox)

Arc 記事[**を描画する3つの方法**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)から、**アーク無限大**ページによってレンダリングされる無限大記号よりも、中心方向の方がややスムーズです。

## <a name="the-quadratic-bzier-curve"></a>2次ベジエ曲線

2次ベジエ曲線には制御点が1つだけあり、曲線は、開始点、制御点、および終点という3つの点によって定義されています。 パラメーター式は3次ベジエ曲線とよく似ていますが、最大指数は2であるため、曲線は2次多項式になります。

x (t) = (1 – t) ² x ₀ + 2t (1 – t) x ₁ + t ² x ₂

y (t) = (1 – t) ² y ₀ + 2t (1 – t) y ₁ + t ² y ₂

2次ベジエ曲線をパスに追加するには、 [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint)) メソッドまたはオーバーロードを使用して、 [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo(System.Single,System.Single,System.Single,System.Single)) `x` と座標を分け `y` ます。

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

メソッドは、現在の位置からへの曲線を、 `point2` `point1` コントロールポイントとしてに追加します。

2次ベジエ曲線は**2 次曲線**ページで試してみることができます。これは、3つのタッチポイントだけがある点を除いて、[**ベジエ曲線**] ページとよく似ています。 `PaintSurface` [**QuadraticCurve.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs)分離コードファイルのハンドラーを次に示します。

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

[![2次曲線ページのトリプルスクリーンショット](beziers-images/quadraticcurve-small.png)](beziers-images/quadraticcurve-large.png#lightbox)

点線は、開始点と終了点の曲線に接し、制御ポイントで満たされます。

2次ベジエは、一般的な形の曲線が必要な場合に適していますが、2つではなく1つの制御ポイントを使用する方が便利です。 2次ベジエは、他の曲線よりも効率的にレンダリングされます。これは、楕円の円弧を描画するために Skia で内部的に使用されるためです。

ただし、2次ベジエ曲線の形状は楕円ではないため、楕円弧を模倣するために複数の2次 Béziers が必要になります。2次ベジエは、代わりに放物線のセグメントです。

## <a name="the-conic-bzier-curve"></a>円錐ベジエ曲線

円錐ベジエ曲線は、 &mdash; 2 次ベジエ曲線とも呼ばれて &mdash; いますが、ベジエ曲線のファミリには比較的最近の追加があります。 2次ベジエ曲線と同様、有理数の2次ベジエ曲線には、始点、終点、および1つのコントロールポイントが含まれます。 ただし、有理数の2次ベジエ曲線にも*重み*値が必要です。 パラメーター式には比率が含まれているため、これは*有理数*の2次式と呼ばれます。

X と Y のパラメーター式は、同じ分母を共有する比率です。 次に示すのは、0から1までの*t*の分母と*w*の重み値の式です。

d (t) = (1 – t) ² + 2wt (1 ~ t) + t ²

理論的には、有理数2項には3つの異なる重み値が関係しています。3つの用語のそれぞれに1つずつありますが、中間用語では1つの重み値に単純化できます。

X 座標と Y 座標のパラメーター式は、2次ベジエのパラメーター式に似ています。ただし、中間の用語には weight 値も含まれ、式は分母で除算されます。

x (t) = ((1 – t) ² x ₀ + 2wt (1 – t) x ₁ + t ² x ₂) ÷ d (t)

y (t) = ((1 – t) ² y ₀ + 2wt (1 – t) y ₁ + t ² y ₂)) ÷ d (t)

合理的な2次ベジエ曲線は*conics*とも呼ばれ &mdash; ます。これは、放物線、楕円、および円として、すべての円錐セクションのセグメントを正確に表すことができるためです。

有理数の2次ベジエ曲線をパスに追加するには、メソッドまたはオーバーロードを使用して、 [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo(System.Single,System.Single,System.Single,System.Single,System.Single)) と座標を分け `x` `y` ます。

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

最後のパラメーターに注意して `weight` ください。

[**円錐曲線**] ページでは、これらの曲線を試すことができます。 `ConicCurvePage` クラスは、`InteractivePage` から派生したものです。 [**Coniccurvepage. .xaml**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml)ファイルは、をインスタンス化して、 `Slider` -2 ~ 2 の重み値を選択します。 [**ConicCurvePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs)分離コードファイルは3つの `TouchPoint` オブジェクトを作成し `PaintSurface` ます。このハンドラーは、結果の曲線を制御ポイントに接し線でレンダリングします。

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

次のように実行されています。

[![[円錐曲線] ページのトリプルスクリーンショット](beziers-images/coniccurve-small.png)](beziers-images/coniccurve-large.png#lightbox)

ご覧のように、コントロールポイントは、重みが高くなると、曲線をさらに大きくするように見えます。 重みが0の場合、曲線は始点から終点までの直線になります。

理論的には、負の重みが許容され、曲線が制御ポイントから*離れる*ようになります。 ただし、-1 以下の重みを指定すると、パラメーター式の分母が*t*の特定の値に対して負になります。 このような理由から、メソッドで負の重みが無視される場合があり `ConicTo` ます。 **円錐曲線**プログラムを使用すると、負の重みを設定できます。ただし、実験によって表示されるように、負の重みは重み付けが0の場合と同じ効果を持ち、直線がレンダリングされます。

メソッドを使用して、 `ConicTo` 半円を最大 (またはそれを除く) まで円弧を描画するために、制御点とウェイトを簡単に派生させることができます。 次の図では、始点と終点の接線線が制御ポイントで一致しています。

![円弧の円錐の円弧レンダリング](beziers-images/conicarc.png)

三角を使用すると、円の中心から制御ポイントまでの距離を調べることができます。これは、円の半径を角度αのコサインで割った値です。 始点と終点の間に円弧を描画するには、その重みを角度の半分の同じコサインに設定します。 角度が180°の場合、接線は満たされず、weight はゼロになります。 しかし、180°未満の角度では、数値演算は正常に機能します。

この例では、**円錐の円弧**ページが示されています。 [**ConicCircularArc**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml)ファイルは、 `Slider` 角度を選択するためのをインスタンス化します。 `PaintSurface` [**ConicCircularArc.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs)分離コードファイルのハンドラーは、制御点と重みを計算します。

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

ご覧のように、 `ConicTo` 赤で示されたパスと、参照用に表示される基になる円の間には視覚的な違いはありません。

[![円錐の円弧ページのトリプルスクリーンショット](beziers-images/coniccirculararc-small.png)](beziers-images/coniccirculararc-large.png#lightbox)

ただし、角度を180度に設定すると、算術演算は失敗します。

このケースでは、 `ConicTo` (パラメーター式に基づく) 理論的には、 `ConicTo` 同じ点で重みが負の値であるの別の呼び出しを使用して円を完了できます。 これにより、 `ConicTo` ゼロ度と180度の間の角度に基づいて、2つの曲線だけを含む円全体を作成できます。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
