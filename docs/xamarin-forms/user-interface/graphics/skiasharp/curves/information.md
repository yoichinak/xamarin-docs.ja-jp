---
title: パス情報と列挙
description: この記事では、SkiaSharp パスに関する情報を取得し、その内容を列挙する方法について説明し、サンプルコードを使用してその方法を示します。
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 931b8d0946f1af5e697e581a04c0feefb31ba2d3
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84131925"
---
# <a name="path-information-and-enumeration"></a>パス情報と列挙

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_パスに関する情報を取得して内容を列挙する_

クラスは、 [`SKPath`](xref:SkiaSharp.SKPath) パスに関する情報を取得できるようにするいくつかのプロパティとメソッドを定義します。 [`Bounds`](xref:SkiaSharp.SKPath.Bounds)プロパティと [`TightBounds`](xref:SkiaSharp.SKPath.TightBounds) プロパティ (および関連するメソッド) は、パスの metrical ディメンションを取得します。 [`Contains`](xref:SkiaSharp.SKPath.Contains(System.Single,System.Single))メソッドを使用すると、特定のポイントがパス内にあるかどうかを判断できます。

パスを構成するすべての直線と曲線の合計の長さを判断すると便利な場合があります。 この長さの計算はアルゴリズム単純なタスクではないため、という名前のクラス全体が対象となり [`PathMeasure`](xref:SkiaSharp.SKPathMeasure) ます。

また、パスを構成するすべての描画操作とポイントを取得すると便利な場合もあります。 最初に、この機能は不要に思えるかもしれません。プログラムがパスを作成した場合、プログラムは既にその内容を認識しています。 ただし、[パスの効果](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)や[テキスト文字列をパスに](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)変換することによって、パスを作成することもできます。 これらのパスを構成するすべての描画操作とポイントを取得することもできます。 1つの方法として、すべてのポイントにアルゴリズム変換を適用して、たとえば、次のように、すべてのポイントにテキストをラップすることができます。

![](information-images/pathenumerationsample.png "Text wrapped on a hemisphere")

## <a name="getting-the-path-length"></a>パスの長さを取得する

記事の「[**パスとテキスト**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)」では、メソッドを使用して、 [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) パスのコースに従ってベースラインを持つテキスト文字列を描画する方法を説明しました。 では、パスに正確に収まるようにテキストのサイズを変更するにはどうすればよいでしょうか。 円の円周は簡単に計算できるので、円の周りにテキストを描画するのは簡単です。 しかし、楕円の円周やベジエ曲線の長さは単純ではありません。

クラスは、 [`SKPathMeasure`](xref:SkiaSharp.SKPathMeasure) 役に立ちます。 [コンストラクター](xref:SkiaSharp.SKPathMeasure.%23ctor(SkiaSharp.SKPath,System.Boolean,System.Single))は `SKPath` 引数を受け取り、プロパティは [`Length`](xref:SkiaSharp.SKPathMeasure.Length) その長さを表します。

このクラスは、[**ベジエ曲線**] ページに基づく**Path Length**サンプルに示されています。 [**Pathlength ページ .xaml**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml)ファイルはから派生 `InteractivePage` し、touch インターフェイスを含みます。

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

[**PathLengthPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs)分離コードファイルを使用すると、4つのタッチポイントを移動して、3次ベジエ曲線の終点と制御点を定義できます。 3つのフィールドは、テキスト文字列、 `SKPaint` オブジェクト、および計算されたテキストの幅を定義します。

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

フィールドは、 `baseTextWidth` `TextSize` 10 の設定に基づくテキストの幅です。

`PaintSurface`ハンドラーはベジエ曲線を描画し、完全な長さに合わせてテキストのサイズを調整します。

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

`Length`新しく作成されたオブジェクトのプロパティは、 `SKPathMeasure` パスの長さを取得します。 パスの長さは、 `baseTextWidth` 値 (10 のテキストサイズに基づいたテキストの幅) で除算され、基本テキストのサイズ (10) で乗算されます。 結果は、そのパスに沿ってテキストを表示するための新しいテキストサイズになります。

[![](information-images/pathlength-small.png "Triple screenshot of the Path Length page")](information-images/pathlength-large.png#lightbox "Triple screenshot of the Path Length page")

ベジエ曲線が長くなるか、短くなるにつれて、テキストサイズの変更を確認できます。

## <a name="traversing-the-path"></a>パスの走査

`SKPathMeasure`は、パスの長さを測定するだけではありません。 0からパスの長さまでの任意の値について、 `SKPathMeasure` オブジェクトはパス上の位置と、そのポイントのパス曲線への接線を取得できます。 タンジェントは、オブジェクトの形式でベクターとして、 `SKPoint` またはオブジェクトにカプセル化された回転として使用でき `SKMatrix` ます。 `SKPathMeasure`さまざまで柔軟な方法でこの情報を取得するのメソッドを次に示します。

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

列挙体のメンバー [`SKPathMeasureMatrixFlags`](xref:SkiaSharp.SKPathMeasureMatrixFlags) は次のとおりです。

- `GetPosition`
- `GetTangent`
- `GetPositionAndTangent`

**Unicycle ハーフパイプ**ページでは、3次ベジエ曲線に沿って逆方向になるように見える Unicycle で、スティック図形をアニメーション化します。

[![](information-images/unicyclehalfpipe-small.png "Triple screenshot of the Unicycle Half-Pipe page")](information-images/unicyclehalfpipe-large.png#lightbox "Triple screenshot of the Unicycle Half-Pipe page")

`SKPaint`ハーフパイプと unicycle の両方を描画するために使用されるオブジェクトは、クラスのフィールドとして定義され `UnicycleHalfPipePage` ます。 また、unicycle のオブジェクトも定義されてい `SKPath` ます。

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

クラスには、 `OnAppearing` アニメーションのメソッドとメソッドの標準オーバーライドが含まれてい `OnDisappearing` ます。 ハンドラーは、 `PaintSurface` ハーフパイプのパスを作成してから描画します。 次 `SKPathMeasure` に、このパスに基づいてオブジェクトが作成されます。

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

この `PaintSurface` ハンドラーは、 `t` 5 秒ごとに 0 ~ 1 の値を計算します。 次に、関数を使用して、 `Math.Cos` 0 ~ 1 の範囲の値に変換し `t` 、0に戻ります。0は、左上の先頭の unicycle に相当し、1は右上の unicycle に相当します。 余弦関数を使用すると、パイプの一番上で速度が遅くなり、一番下に速くなります。

のこの値には、の `t` 最初の引数のパスの長さを乗算する必要があることに注意 `GetMatrix` してください。 次に、unicycle パスを描画するために、マトリックスがオブジェクトに適用され `SKCanvas` ます。

## <a name="enumerating-the-path"></a>パスの列挙

の2つの埋め込みクラス `SKPath` を使用すると、パスの内容を列挙できます。 これらのクラスは [`SKPath.Iterator`](xref:SkiaSharp.SKPath.Iterator) と [`SKPath.RawIterator`](xref:SkiaSharp.SKPath.RawIterator) です。 2つのクラスは非常に似ていますが、 `SKPath.Iterator` パス内の要素を長さ0で削除することも、長さ0にすることもできます。 `RawIterator`次の例では、が使用されています。

型のオブジェクトを取得する `SKPath.RawIterator` には、 [`CreateRawIterator`](xref:SkiaSharp.SKPath.CreateRawIterator) のメソッドを呼び出し `SKPath` ます。 パスの列挙は、メソッドを繰り返し呼び出すことによって行われ [`Next`](xref:SkiaSharp.SKPath.RawIterator.Next*) ます。 次の4つの値の配列を渡し `SKPoint` ます。

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

メソッドは、 `Next` 列挙型のメンバーを返し [`SKPathVerb`](xref:SkiaSharp.SKPathVerb) ます。 これらの値は、パス内の特定の描画コマンドを示します。 配列に挿入される有効なポイントの数は、次の動詞によって異なります。

- `Move`1つのポイントで
- `Line`2つの点を持つ
- `Cubic`4つのポイント
- `Quad`3点で
- `Conic`3つの点を持つ (また、 [`ConicWeight`](xref:SkiaSharp.SKPath.RawIterator.ConicWeight*) 重みに対してメソッドを呼び出す)
- `Close`1つのポイントで
- `Done`

動詞は、 `Done` パスの列挙が完了したことを示します。

動詞がないことに注意 `Arc` してください。 これは、パスに追加されると、すべての円弧がベジエ曲線に変換されることを示します。

配列内の情報の一部 `SKPoint` が冗長です。 たとえば、動詞の `Move` 後に動詞が続く場合、 `Line` に付随する2つ目の点のうち、1番目の点 `Line` が点と同じになり `Move` ます。 実際には、この冗長性は非常に便利です。 動詞を取得すると `Cubic` 、3次ベジエ曲線を定義する4つのすべてのポイントが付随します。 前の動詞によって確立された現在位置を保持する必要はありません。

ただし、問題のある動詞は `Close` です。 このコマンドは、現在の位置から、コマンドによって前に確立された輪郭の先頭までの直線を描画 `Move` します。 動詞は、 `Close` 1 つのポイントではなく、これらの2つの点を提供するのが理想的です。 最悪の点は、動詞に付随するポイント `Close` は常に (0, 0) です。 パスを列挙する場合は、ポイントと現在位置を保持する必要がある場合があり `Move` ます。

## <a name="enumerating-flattening-and-malforming"></a>列挙、フラット化、およびマルウェアの形成

何らかの方法で攻撃を行うために、アルゴリズムの変換をパスに適用することが望ましい場合があります。

![](information-images/pathenumerationsample.png "Text wrapped on a hemisphere")

これらの文字のほとんどは直線で構成されていますが、これらの直線は曲線にツイストされています。 どうすればよいでしょうか。

キーとは、元の直線が一連の小さな直線に分割されることです。 これらの個々の小さい直線は、曲線を形成するためにさまざまな方法で操作できます。

このプロセスを支援するために、 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルには静的クラスが含まれてい [`PathExtensions`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) ます。このメソッドを使用すると、 `Interpolate` 1 つの長さの単位だけを持つ多数の短い行に直線を分割できます。 さらに、クラスには、3種類のベジエ曲線を、曲線を近似する一連の小さな直線に変換するメソッドがいくつか含まれています。 (パラメーター式については、 [**3 種類のベジエ曲線**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)に記載されています)。このプロセスを、曲線の_フラット_化と呼びます。

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

これらのメソッドはすべて、このクラスに含まれる拡張メソッドから参照され `CloneWithTransform` 、次のようになります。 このメソッドは、パスコマンドを列挙し、データに基づいて新しいパスを構築することによって、パスを複製します。 ただし、新しいパスはとの呼び出しのみで構成さ `MoveTo` `LineTo` れます。 すべての曲線と直線は、一連の小さな線に縮小されます。

を呼び出すときに `CloneWithTransform` 、メソッド a にを渡し `Func<SKPoint, SKPoint>` ます。これは、 `SKPaint` 値を返すパラメーターを持つ関数です `SKPoint` 。 この関数は、カスタムのアルゴリズム変換を適用するすべてのポイントに対して呼び出されます。

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

複製されたパスは小さい直線に縮小されるため、変換関数には直線を曲線に変換する機能があります。

メソッドは、という変数内の各輪郭の最初の点 `firstPoint` と、変数内の各描画コマンドの後の現在位置を保持することに注意して `lastPoint` ください。 これらの変数は、 `Close` 動詞が検出されたときに最後の終了行を構築するために必要です。

**GlobularText**サンプルでは、この拡張メソッドを使用して、次のように3d 効果で、この拡張メソッドを使用します。

[![](information-images/globulartext-small.png "Triple screenshot of the Globular Text page")](information-images/globulartext-large.png#lightbox "Triple screenshot of the Globular Text page")

[`GlobularTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs)クラスコンストラクターは、この変換を実行します。 この `SKPaint` メソッドは、テキストのオブジェクトを作成し、 `SKPath` メソッドからオブジェクトを取得し `GetTextPath` ます。 これは、 `CloneWithTransform` 変換関数と共に拡張メソッドに渡されるパスです。

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

変換関数は、まず、という名前の2つの値を計算します。この値は、テキスト `longitude` `latitude` の左上からπ/2 までの範囲で、テキストの右と下でπ/2 になります。 これらの値の範囲は視覚的には不十分であるため、0.75 によって乗算されます。 (調整されていないコードを試してみてください。 このテキストは、北と南の電柱では見えにくくなりすぎます。これら3次元球座標は2次元に変換され `x` 、 `y` 標準の数式によって調整されます。

新しいパスはフィールドとして格納されます。 `PaintSurface`次に、ハンドラーは、画面に表示するためのパスを中央に配置し、スケーリングするだけです。

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

これは、非常に汎用性のある手法です。 [**パスの効果**](effects.md)の記事で説明されているパスの効果の配列が、含まれているはずのものに含まれていない場合は、ギャップを埋める方法です。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
