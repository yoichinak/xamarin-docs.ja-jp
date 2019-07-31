---
title: パス情報と列挙
description: この記事では、SkiaSharp パスに関する情報を取得し、コンテンツを列挙する方法を説明し、サンプル コードを示します。
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2017
ms.openlocfilehash: 05d5003d349ae11a1ec6a1b6f3d66b2f68ffad8a
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652877"
---
# <a name="path-information-and-enumeration"></a>パス情報と列挙

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_パスに関する情報を取得し、内容を列挙_

[ `SKPath` ](xref:SkiaSharp.SKPath)クラスは、いくつかのプロパティとパスに関する情報を取得するためのメソッドを定義します。 [ `Bounds` ](xref:SkiaSharp.SKPath.Bounds)と[ `TightBounds` ](xref:SkiaSharp.SKPath.TightBounds)プロパティ (と関連するメソッド) は、パスの metrical ディメンションを取得します。 [ `Contains` ](xref:SkiaSharp.SKPath.Contains(System.Single,System.Single))メソッドを使用して、パス内の特定のポイントが判断できます。

すべての行とパスを構成する曲線の長さの合計を確認すると役立つ場合があります。 この長さを計算するタスクではありません、単純なアルゴリズム、という名前のクラス全体ように[ `PathMeasure` ](xref:SkiaSharp.SKPathMeasure)を使用します。

描画操作とパスを構成する点を取得する便利なことでがあります。 最初に、この機能は不要に思えるかもしれません。プログラムがパスを作成した場合、プログラムは既にその内容を認識しています。 パスが作成することも、見た[パスの効果](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)と変換することで[パスへの文字列のテキスト](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)します。 描画操作とこれらのパスを構成するポイントを取得することもできます。 1 つの方法は、半球の周囲のテキストを折り返す、たとえば、すべてのポイントにアルゴリズムの変換を適用するには。

![](information-images/pathenumerationsample.png "半球でラップされたテキスト")

## <a name="getting-the-path-length"></a>パスの長さを取得します。

この記事で[**パスとテキスト**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)を使用する方法を説明しました、 [ `DrawTextOnPath` ](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint))がベースラインに依存してパスのテキスト文字列を描画するメソッド。 しかし、テキストのサイズのパスを正確に収まるようにする場合でしょうか。 円の円周を計算する単純なので、テキストの描画を円の周囲は簡単です。 楕円の円周やベジエ曲線の長さはそう簡単ではありません。

[ `SKPathMeasure` ](xref:SkiaSharp.SKPathMeasure)クラスが役立つことができます。 [コンストラクター](xref:SkiaSharp.SKPathMeasure.%23ctor(SkiaSharp.SKPath,System.Boolean,System.Single))を受け入れる、`SKPath`引数、および[ `Length` ](xref:SkiaSharp.SKPathMeasure.Length)プロパティが、長さが表示されます。

このクラスの説明については、**パスの長さ**、サンプルに基づいています、**ベジエ曲線**ページ。 [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml)から派生したファイル`InteractivePage`タッチ インターフェイスが含まれています。

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

[ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs)分離コード ファイルでは、エンドポイントを定義および 3 次ベジエ曲線のポイントを制御する 4 つのタッチ ポイントを移動することができます。 3 つのフィールド定義のテキスト文字列を`SKPaint`オブジェクト、およびテキストの計算された幅。

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

`baseTextWidth`フィールドに基づいて、テキストの幅では、 `TextSize` 10 の設定。

`PaintSurface`ハンドラーは、ベジエ曲線を描画し、端から端に合わせてテキストのサイズを設定します。

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

`Length`プロパティの新しく作成した`SKPathMeasure`オブジェクトは、パスの長さを取得します。 パスの長さで割ります、`baseTextWidth`値 (10 のテキストのサイズに基づいて、テキストの幅は) と基本のテキストのサイズが 10 を乗算します。 結果は、そのパスに沿ってテキストを表示するための新しいテキスト サイズです。

[![](information-images/pathlength-small.png "パスの長さのページのスクリーン ショットをトリプル")](information-images/pathlength-large.png#lightbox "パスの長さ ページの 3 倍になるスクリーン ショット")

長いまたは短い、ベジエ曲線を取得、変更、テキストのサイズを確認できます。

## <a name="traversing-the-path"></a>パスの走査

`SKPathMeasure` 以上のメジャーのパスの長さを実行できます。 0 から、パスの長さまでの値を`SKPathMeasure`オブジェクトでは、パス、およびパスの曲線の接線の位置をその時点で取得できます。 形式でのベクトルとして使用できますが、タンジェント、`SKPoint`オブジェクト、または回転としてにカプセル化されて、`SKMatrix`オブジェクト。 以下の方法を示します`SKPathMeasure`多様かつ柔軟な方法でこの情報を取得します。

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

メンバー、 [ `SKPathMeasureMatrixFlags` ](xref:SkiaSharp.SKPathMeasureMatrixFlags)列挙されます。

- `GetPosition`
- `GetTangent`
- `GetPositionAndTangent`

**一輪車半分パイプ**ページ 3 次ベジエ曲線に沿った前後をオーバーライドすると思われる一輪車上にスティック図形をアニメーション化します。

[![](information-images/unicyclehalfpipe-small.png "一輪車半分パイプ ページのスクリーン ショットをトリプル")](information-images/unicyclehalfpipe-large.png#lightbox "一輪車半分パイプ ページの 3 倍になるスクリーン ショット")

ハーフパイプと unicycle の両方を描画するために使用される`UnicycleHalfPipePage` オブジェクトは、クラスのフィールドとして定義されます。`SKPaint` 定義されていることも、`SKPath`一輪車のオブジェクト。

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

クラスには標準的な上書きが含まれています、`OnAppearing`と`OnDisappearing`メソッド アニメーションを実行します。 `PaintSurface`ハンドラーが半分パイプのパスを作成し、描画します。 `SKPathMeasure`オブジェクトがこのパスに基づいて、作成します。

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

`PaintSurface`ハンドラーの値を計算する`t`が 0 から 1 を 5 秒ごとです。 次を使用して、`Math.Cos`関数の値を変換する`t`範囲 0 から 1 および 0 へ 1 は右上にある一輪車に対応中に、左、上の先頭にある一輪車に 0 が対応する場所。 コサイン関数は、パイプの上部にある最も低速なと下部にある最も高速にする速度とします。

注意この値の`t`最初の引数のパスの長さで乗算する必要があります`GetMatrix`します。 マトリックスに適用されます、`SKCanvas`一輪車パスを描画するためのオブジェクト。

## <a name="enumerating-the-path"></a>パスを列挙します。

2 つのクラスの埋め込み`SKPath`パスの内容を列挙できます。 これらのクラスは[ `SKPath.Iterator` ](xref:SkiaSharp.SKPath.Iterator)と[ `SKPath.RawIterator`](xref:SkiaSharp.SKPath.RawIterator)します。 2 つのクラスは非常に似ていますが、`SKPath.Iterator`パスの長さが 0 で、または長さが 0 に近い要素を削除することができます。 `RawIterator`は次の例で使用します。

型のオブジェクトを取得する`SKPath.RawIterator`呼び出すことによって、 [ `CreateRawIterator` ](xref:SkiaSharp.SKPath.CreateRawIterator)メソッドの`SKPath`します。 繰り返し呼び出すことにより実現は、パスを列挙、 [ `Next` ](xref:SkiaSharp.SKPath.RawIterator.Next*)メソッド。 4 つの配列を渡す`SKPoint`値。

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next`メソッドのメンバーを返します、 [ `SKPathVerb` ](xref:SkiaSharp.SKPathVerb)列挙型。 これらの値は、パス内の特定の描画コマンドを示します。 配列に挿入された有効なポイントの数は、この動作によって異なります。

- `Move` 1 つのポイント
- `Line` 2 つの点を持つ
- `Cubic` 次の 4 つの点を持つ
- `Quad` 次の 3 つの点を持つ
- `Conic` 次の 3 つの点を持つ (も呼び出すと、 [ `ConicWeight` ](xref:SkiaSharp.SKPath.RawIterator.ConicWeight*)メソッドの重み)
- `Close` 1 つのポイント
- `Done`

`Done`動詞は、パスの列挙が完了したことを示します。

あることに注意してくださいありません`Arc`動詞。 これは、すべての円弧をベジエ曲線のパスに追加されたときに変換されることを示します。

一部の情報の`SKPoint`配列が冗長です。 たとえば場合、`Move`動詞の後に、`Line`動詞、最初に付随する 2 つのポイントの`Line`と同じです、`Move`ポイントします。 実際には、この冗長性は、非常に便利です。 取得するときに、`Cubic`動詞、3 次ベジエ曲線を定義する 4 つのすべてのポイントが付属します。 前の動詞で確立されている現在の位置を保持する必要はありません。

ただし、問題の動詞は、`Close`します。 このコマンドでは、現在の位置から直線を描画によって以前に確立された輪郭の先頭に、`Move`コマンド。 理想的には、`Close`動詞は、1 つのポイントではなく、これら 2 つのポイントを提供する必要があります。 さらに悪いことは、関連するポイント、`Close`動詞は常に (0, 0)。 パスを列挙する場合はおそらく必要がありますを保持する、`Move`ポイントと、現在の位置。

## <a name="enumerating-flattening-and-malforming"></a>列挙する、フラット化、および Malforming

アルゴリズムを適用することが望ましい場合があります何らかの方法で malform へのパスに変換します。

![](information-images/pathenumerationsample.png "半球でラップされたテキスト")

これらの文字のほとんどは、直線で構成されているまだこれらの直線を曲線にツイストが明らかされています。 設定できないのでしょうか。

元の直線が一連の小さな直線に分類されていることが重要です。 これら個々 の小さな直線行は、曲線を形成するさまざまな方法で操作できます。

このプロセスで利用できる、 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルには、静的な[ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs)クラス、`Interpolate`を細分化するメソッド、直線の長さ単位の 1 つだけである多数の短い行にします。 さらに、クラスには、一連の小さな直線、曲線を近似する 3 つの種類のベジエ曲線に変換するいくつかのメソッドが含まれています。 (パラメーターの数式は、情報の記事で表示された[**ベジエ曲線の型を次の 3 つ**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md))。このプロセスが呼び出されます_フラット化_曲線。

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
        }

        return points;
    }

    static double Length(SKPoint pt0, SKPoint pt1)
    {
        return Math.Sqrt(Math.Pow(pt1.X - pt0.X, 2) + Math.Pow(pt1.Y - pt0.Y, 2));
    }
}
```

これらすべてのメソッドは、拡張メソッドから参照される`CloneWithTransform`もこのクラスに含まれており、次に示します。 このメソッドは、パスのコマンドを列挙して、データに基づく新しいパスを作成して、パスを複製します。 ただし、新しいパスだけで構成されて`MoveTo`と`LineTo`呼び出し。 すべての直線と曲線は、一連の小さな線に縮小されます。

呼び出すときに`CloneWithTransform`、メソッドに渡す、`Func<SKPoint, SKPoint>`での関数は、`SKPaint`パラメーターを返す、`SKPoint`値。 この関数は、カスタム アルゴリズムの変換を適用するすべてのポイントの呼び出されます。

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

メソッドはという変数には、各輪郭の最初のポイントを保持することに注意してください`firstPoint`とそれぞれの後の現在の位置が変数にコマンドを描画`lastPoint`します。 これらの変数は、最終的な終了を構築するために必要なときに行を`Close`動詞が発生しました。

**GlobularText**サンプルでは、この拡張メソッドを使用して、3 D 効果で一見半球の周りでテキストをラップします。

[![](information-images/globulartext-small.png "Globular テキスト ページのスクリーン ショットをトリプル")](information-images/globulartext-large.png#lightbox "Globular テキスト ページの 3 倍になるスクリーン ショット")

[ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs)クラスのコンストラクターは、この変換を実行します。 作成、`SKPaint`オブジェクトのテキスト、および取得し、`SKPath`オブジェクトから、`GetTextPath`メソッド。 これは、パスに渡される、`CloneWithTransform`変換関数と共に拡張メソッド。

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

変換関数は、まずという名前の 2 つの値を計算する`longitude`と`latitude`上部と、テキストの左側には、– π/2 ~ π/2 右とテキストの下での範囲。 これらの値の範囲は、0.75 を掛けることによって削減されますが、視覚的に満足はありません。 (これらの調整なしのコードを実行してください。 テキストになります、南北、北米にある漠然と辺にシンすぎます。)これらの 3 次元球座標は、2 次元に変換されます`x`と`y`標準の数式で座標。

新しいパスは、フィールドとして保存されます。 `PaintSurface`ハンドラー、単なる必要がありますの中心や画面上に表示するパスを拡張します。

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

これは、非常に汎用的な手法です。 パスの効果の配列で説明されている場合、 [**パスの効果**](effects.md)記事で非常に encompass 動いた、含める必要があるものはこれは、ギャップを埋めるための方法です。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
