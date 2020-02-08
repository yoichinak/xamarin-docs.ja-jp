---
title: パス情報と列挙
description: この記事では、SkiaSharp パスに関する情報を取得し、コンテンツを列挙する方法を説明し、サンプル コードを示します。
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2017
ms.openlocfilehash: 6f4f4e6253c14d86e2057f13d6232a07a83b4d26
ms.sourcegitcommit: ae5557c5024d4b7bd52b2f33cb96114ce2b8e086
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2020
ms.locfileid: "77045076"
---
# <a name="path-information-and-enumeration"></a>パス情報と列挙

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_パスに関する情報を取得して内容を列挙する_

[`SKPath`](xref:SkiaSharp.SKPath)クラスは、パスに関する情報を取得できるようにするいくつかのプロパティとメソッドを定義します。 [`Bounds`](xref:SkiaSharp.SKPath.Bounds)プロパティと[`TightBounds`](xref:SkiaSharp.SKPath.TightBounds)プロパティ (および関連するメソッド) は、パスの metrical ディメンションを取得します。 [`Contains`](xref:SkiaSharp.SKPath.Contains(System.Single,System.Single))メソッドを使用すると、特定のポイントがパス内にあるかどうかを判断できます。

すべての行とパスを構成する曲線の長さの合計を確認すると役立つ場合があります。 この長さの計算はアルゴリズム単純なタスクではないため、 [`PathMeasure`](xref:SkiaSharp.SKPathMeasure)という名前のクラス全体が対象になります。

描画操作とパスを構成する点を取得する便利なことでがあります。 最初は、この機能は不要なかもしれません: プログラムが既に内容を認識している場合は、プログラムのパスが作成されます。 ただし、[パスの効果](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)や[テキスト文字列をパスに](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)変換することによって、パスを作成することもできます。 描画操作とこれらのパスを構成するポイントを取得することもできます。 1 つの方法は、半球の周囲のテキストを折り返す、たとえば、すべてのポイントにアルゴリズムの変換を適用するには。

![](information-images/pathenumerationsample.png "Text wrapped on a hemisphere")

## <a name="getting-the-path-length"></a>パスの長さを取得します。

記事の「[**パスとテキスト**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)」では、 [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint))メソッドを使用して、パスのコースの後にベースラインを持つテキスト文字列を描画する方法を説明しました。 しかし、テキストのサイズのパスを正確に収まるようにする場合でしょうか。 円の円周を計算する単純なので、テキストの描画を円の周囲は簡単です。 楕円の円周やベジエ曲線の長さはそう簡単ではありません。

[`SKPathMeasure`](xref:SkiaSharp.SKPathMeasure)クラスは役に立ちます。 [コンストラクター](xref:SkiaSharp.SKPathMeasure.%23ctor(SkiaSharp.SKPath,System.Boolean,System.Single))は `SKPath` 引数を受け取り、 [`Length`](xref:SkiaSharp.SKPathMeasure.Length)プロパティはその長さを表します。

このクラスは、 **[ベジエ曲線]** ページに基づく**Path Length**サンプルに示されています。 [**Pathlength ページ .xaml**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml)ファイルは `InteractivePage` から派生し、タッチインターフェイスを含みます。

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Curves.PathLengthPage"
                       Title="Path Length">
    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</local:InteractivePage>
```

[**PathLengthPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs)分離コードファイルを使用すると、4つのタッチポイントを移動して、3次ベジエ曲線の終点と制御点を定義できます。 3つのフィールドは、テキスト文字列、`SKPaint` オブジェクト、およびテキストの計算された幅を定義します。

```csharp
public partial class PathLengthPage : InteractivePage
{
    const string text = "Compute length of path";

    static SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Black,
        TextSize = 10,
    };

    static readonly float baseTextWidth = textPaint.MeasureText(text);
    ...
}
```

`baseTextWidth` フィールドは、`TextSize` 設定の10に基づいてテキストの幅を示します。

`PaintSurface` ハンドラーはベジエ曲線を描画し、完全な長さに合わせてテキストのサイズを調整します。

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

        // Get path length
        SKPathMeasure pathMeasure = new SKPathMeasure(path, false, 1);

        // Find new text size
        textPaint.TextSize = pathMeasure.Length / baseTextWidth * 10;

        // Draw text on path
        canvas.DrawTextOnPath(text, path, 0, 0, textPaint);
    }
    ...
}
```

新しく作成された `SKPathMeasure` オブジェクトの `Length` プロパティは、パスの長さを取得します。 パスの長さは `baseTextWidth` 値 (10 のテキストサイズに基づいたテキストの幅) で除算され、基本テキストのサイズ (10) で乗算されます。 結果は、そのパスに沿ってテキストを表示するための新しいテキスト サイズです。

[![](information-images/pathlength-small.png "Triple screenshot of the Path Length page")](information-images/pathlength-large.png#lightbox "Triple screenshot of the Path Length page")

長いまたは短い、ベジエ曲線を取得、変更、テキストのサイズを確認できます。

## <a name="traversing-the-path"></a>パスの走査

`SKPathMeasure` は、パスの長さを測定するだけではありません。 0からパスの長さまでの任意の値について、`SKPathMeasure` オブジェクトは、パス上の位置と、そのポイントのパス曲線への接線を取得できます。 タンジェントは、`SKPoint` オブジェクトの形式でベクターとして使用することも、`SKMatrix` オブジェクトにカプセル化された回転として使用することもできます。 さまざまで柔軟な方法でこの情報を取得する `SKPathMeasure` の方法を次に示します。

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

[`SKPathMeasureMatrixFlags`](xref:SkiaSharp.SKPathMeasureMatrixFlags)列挙体のメンバーは次のとおりです。

- `GetPosition`
- `GetTangent`
- `GetPositionAndTangent`

**Unicycle ハーフパイプ**ページでは、3次ベジエ曲線に沿って逆方向になるように見える Unicycle で、スティック図形をアニメーション化します。

[![](information-images/unicyclehalfpipe-small.png "Triple screenshot of the Unicycle Half-Pipe page")](information-images/unicyclehalfpipe-large.png#lightbox "Triple screenshot of the Unicycle Half-Pipe page")

ハーフパイプと unicycle の両方を描画するために使用される `SKPaint` オブジェクトは、`UnicycleHalfPipePage` クラスのフィールドとして定義されます。 Unicycle の `SKPath` オブジェクトも定義されています。

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };

    SKPath unicyclePath = SKPath.ParseSvgPathData(
        "M 0 0" +
        "A 25 25 0 0 0 0 -50" +
        "A 25 25 0 0 0 0 0 Z" +
        "M 0 -25 L 0 -100" +
        "A 15 15 0 0 0 0 -130" +
        "A 15 15 0 0 0 0 -100 Z" +
        "M -25 -85 L 25 -85");
    ...
}
```

クラスには、アニメーションの `OnAppearing` および `OnDisappearing` メソッドの標準オーバーライドが含まれています。 `PaintSurface` ハンドラーは、ハーフパイプのパスを作成して描画します。 その後、次のパスに基づいて `SKPathMeasure` オブジェクトが作成されます。

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath pipePath = new SKPath())
        {
            pipePath.MoveTo(50, 50);
            pipePath.CubicTo(0, 1.25f * info.Height,
                             info.Width - 0, 1.25f * info.Height,
                             info.Width - 50, 50);

            canvas.DrawPath(pipePath, strokePaint);

            using (SKPathMeasure pathMeasure = new SKPathMeasure(pipePath))
            {
                float length = pathMeasure.Length;

                // Animate t from 0 to 1 every three seconds
                TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
                float t = (float)(timeSpan.TotalSeconds % 5 / 5);

                // t from 0 to 1 to 0 but slower at beginning and end
                t = (float)((1 - Math.Cos(t * 2 * Math.PI)) / 2);

                SKMatrix matrix;
                pathMeasure.GetMatrix(t * length, out matrix,
                                      SKPathMeasureMatrixFlags.GetPositionAndTangent);

                canvas.SetMatrix(matrix);
                canvas.DrawPath(unicyclePath, strokePaint);
            }
        }
    }
}
```

`PaintSurface` ハンドラーは、5秒ごとに 0 ~ 1 の `t` の値を計算します。 次に、`Math.Cos` 関数を使用して、0 ~ 1 の範囲の `t` の値に変換し、0に戻ります。0は左上の先頭の unicycle に、1は右上の unicycle に相当します。 コサイン関数は、パイプの上部にある最も低速なと下部にある最も高速にする速度とします。

この `t` の値には、`GetMatrix`する1番目の引数のパスの長さを乗算する必要があることに注意してください。 次に、unicycle パスを描画するために、マトリックスが `SKCanvas` オブジェクトに適用されます。

## <a name="enumerating-the-path"></a>パスを列挙します。

`SKPath` の2つの埋め込みクラスを使用すると、パスの内容を列挙できます。 これらのクラスは[`SKPath.Iterator`](xref:SkiaSharp.SKPath.Iterator)および[`SKPath.RawIterator`](xref:SkiaSharp.SKPath.RawIterator)です。 2つのクラスはよく似ていますが、`SKPath.Iterator` では、長さが0のパス内の要素や長さが0の要素を削除できます。 次の例では、`RawIterator` を使用します。

`SKPath`の[`CreateRawIterator`](xref:SkiaSharp.SKPath.CreateRawIterator)メソッドを呼び出すことによって `SKPath.RawIterator` 型のオブジェクトを取得できます。 パスの列挙は、 [`Next`](xref:SkiaSharp.SKPath.RawIterator.Next*)メソッドを繰り返し呼び出すことによって行われます。 4つの `SKPoint` 値の配列を渡します。

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next` メソッドは、 [`SKPathVerb`](xref:SkiaSharp.SKPathVerb)列挙型のメンバーを返します。 これらの値は、パス内の特定の描画コマンドを示します。 配列に挿入された有効なポイントの数は、この動作によって異なります。

- 1つのポイントでの `Move`
- 2つの点を持つ `Line`
- 4つのポイントを持つ `Cubic`
- 3つのポイントを含む `Quad`
- 3つのポイントを使用して `Conic` します (また、重みの[`ConicWeight`](xref:SkiaSharp.SKPath.RawIterator.ConicWeight*)メソッドも呼び出します)。
- 1つのポイントでの `Close`
- `Done`

`Done` 動詞は、パスの列挙が完了したことを示します。

`Arc` 動詞がないことに注意してください。 これは、すべての円弧をベジエ曲線のパスに追加されたときに変換されることを示します。

`SKPoint` 配列内の情報の一部が冗長です。 たとえば、`Move` 動詞の後に `Line` 動詞が続く場合、`Line` に付随する2つの点のうち、最初の点は `Move` ポイントと同じになります。 実際には、この冗長性は、非常に便利です。 `Cubic` 動詞を取得すると、3次ベジエ曲線を定義する4つの点がすべてになります。 前の動詞で確立されている現在の位置を保持する必要はありません。

ただし、問題のある動詞は `Close`です。 このコマンドは、現在の位置から `Move` コマンドによって前に確立された輪郭の先頭まで直線を描画します。 `Close` 動詞は、1つのポイントではなく、これらの2つのポイントを提供するのが理想的です。 さらに悪いのは、`Close` 動詞に付随するポイントは常に (0, 0) です。 パスを列挙する場合は、`Move` ポイントと現在の位置を保持する必要がある場合があります。

## <a name="enumerating-flattening-and-malforming"></a>列挙する、フラット化、および Malforming

アルゴリズムを適用することが望ましい場合があります何らかの方法で malform へのパスに変換します。

![](information-images/pathenumerationsample.png "Text wrapped on a hemisphere")

これらの文字のほとんどは、直線で構成されているまだこれらの直線を曲線にツイストが明らかされています。 設定できないのでしょうか。

元の直線が一連の小さな直線に分類されていることが重要です。 これら個々 の小さな直線行は、曲線を形成するさまざまな方法で操作できます。

このプロセスを支援するために、 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルには静的な[`PathExtensions`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs)クラスが含まれています。このクラスは、直線を、1つの長さの単位だけを持つ多数の短い行に分割する `Interpolate` メソッドを含んでいます。 さらに、クラスには、一連の小さな直線、曲線を近似する 3 つの種類のベジエ曲線に変換するいくつかのメソッドが含まれています。 (パラメーター式については、 [**3 種類のベジエ曲線**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)に記載されています)。このプロセスを、曲線の_フラット_化と呼びます。

```csharp
static class PathExtensions
{
    ...
    static SKPoint[] Interpolate(SKPoint pt0, SKPoint pt1)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * pt0.X + t * pt1.X;
            float y = (1 - t) * pt0.Y + t * pt1.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenCubic(SKPoint pt0, SKPoint pt1, SKPoint pt2, SKPoint pt3)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2) + Length(pt2, pt3));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * (1 - t) * pt0.X +
                        3 * t * (1 - t) * (1 - t) * pt1.X +
                        3 * t * t * (1 - t) * pt2.X +
                        t * t * t * pt3.X;
            float y = (1 - t) * (1 - t) * (1 - t) * pt0.Y +
                        3 * t * (1 - t) * (1 - t) * pt1.Y +
                        3 * t * t * (1 - t) * pt2.Y +
                        t * t * t * pt3.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenQuadratic(SKPoint pt0, SKPoint pt1, SKPoint pt2)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenConic(SKPoint pt0, SKPoint pt1, SKPoint pt2, float weight)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float denominator = (1 - t) * (1 - t) + 2 * weight * t * (1 - t) + t * t;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * weight * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * weight * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            x /= denominator;
            y /= denominator;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static double Length(SKPoint pt0, SKPoint pt1)
    {
        return Math.Sqrt(Math.Pow(pt1.X - pt0.X, 2) + Math.Pow(pt1.Y - pt0.Y, 2));
    }
}
```

これらのメソッドはすべて、このクラスに含まれている `CloneWithTransform` 拡張メソッドから参照され、次のようになります。 このメソッドは、パスのコマンドを列挙して、データに基づく新しいパスを作成して、パスを複製します。 ただし、新しいパスは `MoveTo` と `LineTo` の呼び出しのみで構成されます。 すべての直線と曲線は、一連の小さな線に縮小されます。

`CloneWithTransform`を呼び出すときに、メソッドに `Func<SKPoint, SKPoint>`を渡します。これは、`SKPoint` 値を返す `SKPaint` パラメーターを持つ関数です。 この関数は、カスタム アルゴリズムの変換を適用するすべてのポイントの呼び出されます。

```csharp
static class PathExtensions
{
    public static SKPath CloneWithTransform(this SKPath pathIn, Func<SKPoint, SKPoint> transform)
    {
        SKPath pathOut = new SKPath();

        using (SKPath.RawIterator iterator = pathIn.CreateRawIterator())
        {
            SKPoint[] points = new SKPoint[4];
            SKPathVerb pathVerb = SKPathVerb.Move;
            SKPoint firstPoint = new SKPoint();
            SKPoint lastPoint = new SKPoint();

            while ((pathVerb = iterator.Next(points)) != SKPathVerb.Done)
            {
                switch (pathVerb)
                {
                    case SKPathVerb.Move:
                        pathOut.MoveTo(transform(points[0]));
                        firstPoint = lastPoint = points[0];
                        break;

                    case SKPathVerb.Line:
                        SKPoint[] linePoints = Interpolate(points[0], points[1]);

                        foreach (SKPoint pt in linePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[1];
                        break;

                    case SKPathVerb.Cubic:
                        SKPoint[] cubicPoints = FlattenCubic(points[0], points[1], points[2], points[3]);

                        foreach (SKPoint pt in cubicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[3];
                        break;

                    case SKPathVerb.Quad:
                        SKPoint[] quadPoints = FlattenQuadratic(points[0], points[1], points[2]);

                        foreach (SKPoint pt in quadPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Conic:
                        SKPoint[] conicPoints = FlattenConic(points[0], points[1], points[2], iterator.ConicWeight());

                        foreach (SKPoint pt in conicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Close:
                        SKPoint[] closePoints = Interpolate(lastPoint, firstPoint);

                        foreach (SKPoint pt in closePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        firstPoint = lastPoint = new SKPoint(0, 0);
                        pathOut.Close();
                        break;
                }
            }
        }
        return pathOut;
    }
    ...
}
```

複製されたパスは、小さな直線を削減のため、変換関数には、直線を曲線に変換する機能があります。

メソッドは、各輪郭の最初の点を `firstPoint` という変数に保持し、`lastPoint`変数内の各描画コマンドの後の現在位置を保持することに注意してください。 これらの変数は、`Close` 動詞が検出されたときに最後の終了行を構築するために必要です。

**GlobularText**サンプルでは、この拡張メソッドを使用して、次のように3d 効果で、この拡張メソッドを使用します。

[![](information-images/globulartext-small.png "Triple screenshot of the Globular Text page")](information-images/globulartext-large.png#lightbox "Triple screenshot of the Globular Text page")

[`GlobularTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs)クラスコンストラクターは、この変換を実行します。 このメソッドは、テキストの `SKPaint` オブジェクトを作成し、`GetTextPath` メソッドから `SKPath` オブジェクトを取得します。 これは、変換関数と共に `CloneWithTransform` 拡張メソッドに渡されるパスです。

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;

    public GlobularTextPage()
    {
        Title = "Globular Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.Typeface = SKTypeface.FromFamilyName("Times New Roman");
            textPaint.TextSize = 100;

            using (SKPath textPath = textPaint.GetTextPath("HELLO", 0, 0))
            {
                SKRect textPathBounds;
                textPath.GetBounds(out textPathBounds);

                globePath = textPath.CloneWithTransform((SKPoint pt) =>
                {
                    double longitude = (Math.PI / textPathBounds.Width) *
                                            (pt.X - textPathBounds.Left) - Math.PI / 2;
                    double latitude = (Math.PI / textPathBounds.Height) *
                                            (pt.Y - textPathBounds.Top) - Math.PI / 2;

                    longitude *= 0.75;
                    latitude *= 0.75;

                    float x = (float)(Math.Cos(latitude) * Math.Sin(longitude));
                    float y = (float)Math.Sin(latitude);

                    return new SKPoint(x, y);
                });
            }
        }
    }
    ...
}
```

変換関数は、最初に `longitude` という名前の2つの値を計算し、テキストの左上にある–π/2 から、テキストの右と下のπ/2 の範囲を `latitude` します。 これらの値の範囲は、0.75 を掛けることによって削減されますが、視覚的に満足はありません。 (これらの調整なしのコードを実行してください。 このテキストは、北と南の電柱では見えにくくなりすぎます。これら3次元の球体座標は、標準の数式によって2次元の `x` と `y` 座標に変換されます。

新しいパスは、フィールドとして保存されます。 `PaintSurface` ハンドラーは、画面に表示するためにパスを中央に配置し、スケーリングするだけです。

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint pathPaint = new SKPaint())
        {
            pathPaint.Style = SKPaintStyle.Fill;
            pathPaint.Color = SKColors.Blue;
            pathPaint.StrokeWidth = 3;
            pathPaint.IsAntialias = true;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(0.45f * Math.Min(info.Width, info.Height));     // radius
            canvas.DrawPath(globePath, pathPaint);
        }
    }
}
```

これは、非常に汎用的な手法です。 [**パスの効果**](effects.md)の記事で説明されているパスの効果の配列が、含まれているはずのものに含まれていない場合は、ギャップを埋める方法です。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
