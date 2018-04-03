---
title: パス情報は、列挙型
description: パスに関する情報を取得し、コンテンツの列挙
ms.topic: article
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/12/2017
ms.openlocfilehash: 9c7e8a44a9bb4ee8cb131ffa25e4a595b4225ed6
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="path-information-and-enumeration"></a>パス情報は、列挙型

_パスに関する情報を取得し、コンテンツの列挙_

[ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/)クラスは、いくつかのプロパティとパスに関する情報を取得できるようにするメソッドを定義します。 [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/)と[ `TightBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.TightBounds/)プロパティ (および関連するメソッド) は、パスの metrical 次元を取得します。 [ `Contains` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Contains/p/System.Single/System.Single/)メソッドでは、パス内の特定のポイントがかどうかを判断することができます。

すべての直線と曲線のパスを構成するの合計の長さを決定に役立つ場合があります。 これは、作業ではありません、アルゴリズムによって単純なため、クラス全体がという名前[ `PathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/)割り当てられます。

描画操作とパスを形成する点を取得するには役立ちます。 これでがあります。 最初は、この機能は不要なかもしれません: プログラムがコンテンツを既に知っている場合は、プログラムのパスが作成されます。 ただし、見たことのパスがによって作成することも[パス効果](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)および変換することで[パスへの文字列のテキスト](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)です。 描画操作とこれらのパスを形成する点を取得することもできます。 1 つの方法では、すべてのポイントに、アルゴリズムの変換を適用します。 これによりの半球に収まらないテキストの折り返しなどの手法です。

![](information-images/pathenumerationsample.png "半球でラップされたテキスト")

## <a name="getting-the-path-length"></a>パスの長さを取得します。

アーティクルで[**パスとテキスト**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)を使用する方法を説明する、 [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/)を基準に依存して、パスのテキスト文字列を描画するメソッド。 しかし、テキストのサイズのパスを正確に収まるようにする場合でしょうか。 円の周囲のテキストの描画、これは簡単な円の円周を計算する単純なためです。 楕円の円周またはベジエ曲線の長さがそれほど単純ではありません。 

[ `SKPathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/)クラスに役立つことができます。 [コンス トラクター](https://developer.xamarin.com/api/constructor/SkiaSharp.SKPathMeasure.SKPathMeasure/p/SkiaSharp.SKPath/System.Boolean/System.Single/)を受け入れる、`SKPath`引数、および[ `Length` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPathMeasure.Length/)プロパティは、その長さを表示します。

これに示されている、**パスの長さ**に基づくサンプル、**ベジエ曲線**ページ。 [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml)から派生したファイル`InteractivePage`タッチ インターフェイスが含まれています。

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

[ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs)分離コード ファイルでは、端点を定義および 3 次ベジエ曲線の点を制御する 4 つのタッチ ポイントを移動することができます。 3 つのフィールドをテキスト文字列を定義する、`SKPaint`オブジェクト、およびテキストの計算された幅。

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

`baseTextWidth`フィールドは、基にテキストの幅、 `TextSize` 10 に設定します。

`PaintSurface`ハンドラー ベジエ曲線を描画し、端から端に合わせてテキストのサイズを設定します。

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

`Length`プロパティの新しく作成した`SKPathMeasure`オブジェクトは、パスの長さを取得します。 割った値がこの、 `baseTextWidth` (これは、基にテキストを 10 文字のテキスト サイズの幅) を値し、し、基本のテキストのサイズが 10 を乗算します。 結果は、そのパスに沿ってテキストを表示するための新しいテキスト サイズです。

[![](information-images/pathlength-small.png "パスの長さ ページのスクリーン ショットをトリプル")](information-images/pathlength-large.png#lightbox "パスの長さ ページのトリプル スクリーン ショット")

長いまたは短い、ベジエ曲線を取得、テキストのサイズ変更を確認できます。

## <a name="traversing-the-path"></a>パスを走査します。

`SKPathMeasure` 以上のメジャーのパスの長さを実行できます。 任意の値 0 と、パスの長さの間、`SKPathMeasure`オブジェクトがその時点で、パス、およびパス曲線に正接での位置を取得できます。 タンジェントがの形式でのベクトルとして利用可能な`SKPoint`オブジェクト、または回転としてにカプセル化されて、`SKMatrix`オブジェクト。 以下の方法を示します`SKPathMeasure`可変で柔軟な方法でこの情報を取得します。

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

[ `SKPathMeasureMatrixFlags` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasureMatrixFlags/)は。

- [`GetPosition`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPosition/)
- [`GetTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)
- [`GetPositionAndTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)

**一輪車半分パイプ**ページ 3 次ベジエ曲線に沿った前後にオーバーライドすると思われる一輪車上にスティック図形をアニメーション化します。

[![](information-images/unicyclehalfpipe-small.png "一輪車半分パイプ ページのスクリーン ショットをトリプル")](information-images/unicyclehalfpipe-large.png#lightbox "一輪車半分パイプ ページのトリプル スクリーン ショット")

`SKPaint`半分パイプと、一輪車線の描画に使用されるオブジェクトがフィールドとして定義されている、 [ `UnicycleHalfPipePage` ]()クラスです。 定義されても、`SKPath`一輪車のオブジェクト。

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

クラスには標準の上書きが含まれています、`OnAppearing`と`OnDisappearing`アニメーションのメソッドです。 `PaintSurface`ハンドラーが半分パイプのパスを作成し、描画します。 `SKPathMeasure`オブジェクトはこのパスのベースが作成されます。

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

`PaintSurface`ハンドラーの値を計算する`t`数式の 0 から 1 を 5 秒ごとです。 次を使用して、`Math.Cos`関数の値を変換する`t`の範囲は 0 から 1 および 0 へが 0 に対応する左、上の先頭に一輪車 1 は右上の一輪車に対応中にします。 コサイン関数は、パイプの上部にある最も低速と下部にある最も高速にする速度をによりします。

注意してこの値の`t`1 番目の引数のパスの長さで乗算する必要があります`GetMatrix`です。 マトリックスに適用されます、`SKCanvas`一輪車パスを描画するためのオブジェクト。

## <a name="enumerating-the-path"></a>パスを列挙します。

2 つのクラスの埋め込み`SKPath`パスの内容を列挙することです。 これらのクラスは[ `SKPath.Iterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+Iterator/)と[ `SKPath.RawIterator`](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+RawIterator/)です。 2 つのクラスは、よく似ていますが、`SKPath.Iterator`長さが 0 で、または長さがゼロに近いへのパス内の要素を除去できます。 `RawIterator`は、次の例で使用します。

型のオブジェクトを取得する`SKPath.RawIterator`を呼び出して、 [ `CreateRawIterator` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CreateRawIterator()/)メソッドの`SKPath`します。 繰り返し呼び出すことにより実現は、パスの列挙、 [ `Next` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.Next/p/SkiaSharp.SKPoint[]/)メソッドです。 4 つの配列を渡す`SKPoint`値。

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next`のメンバーを返します、 [ `SKPathVerb` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathVerb/)列挙します。 これらの値は、パス内の特定の描画コマンドを示します。 有効なポイントが、配列で挿入の数は、この動詞によって決まります。

- [`Move`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Move/) 1 つのポイントと
- [`Line`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Line/) 2 つの点で
- [`Cubic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Cubic/) 次の 4 つのポイントを持つ
- [`Quad`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Quad/) 次の 3 つのポイントを持つ
- [`Conic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Conic/) 次の 3 つのポイントを持つ (も呼び出すと、 [ `ConicWeight` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.ConicWeight/)の重みメソッド)
- [`Close`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Close/) 1 つのポイントと
- [`Done`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Done/)

`Done`動詞は、列挙が完了したことを示します。

あることに注意してくださいありません`Arc`動詞です。 これは、すべての円弧をベジエ曲線のパスに追加されたときに変換されることを示します。

一部の情報は、の`SKPoint`配列が重複します。 たとえば場合、`Move`動詞が続く、`Line`動詞を最初に付随する 2 つのポイントの`Line`と同じ、`Move`ポイントします。 実際には、このような冗長性は非常に便利です。 取得するときに、`Cubic`動詞、3 次ベジエ曲線を定義する 4 つのすべてのポイントが付属します。 以前の動詞によって確立された現在の位置を保持する必要はありません。

ただしは、問題のある動詞`Close`です。 このコマンドでは、現在の位置から直線を描画によって以前に確立された輪郭の先頭に、`Move`コマンド。 理想的には、`Close`動詞は、1 つのポイントではなく、これら 2 つのポイントを提供する必要があります。 さらに悪い付随するポイントは、`Close`動詞は、常に (0, 0) です。 これは、必要があるパスを列挙するときにあります可能性を保持する、`Move`ポイントと、現在の位置。

静的な[ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathExtensions.cs)クラスには、小さな直線、曲線のおおよその系列に次の 3 つの種類のベジエ曲線に変換するいくつかのメソッドが含まれています。 (パラメーターの数式は、アーティクルで表示された[ **3 つの種類のベジエ曲線**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md))。`Interpolate`直線長さ単位の 1 つだけである多数の短い行に分割する方法。

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

これらすべてのメソッドが拡張メソッドから参照される`CloneWithTransform`の下に表示します。 このメソッドは、パスのコマンドを列挙して、データに基づく新しいパスを作成するパスを複製します。 ただし、新しいパスだけで構成されて`MoveTo`と`LineTo`呼び出しです。 一連の小さな線には、すべての直線と曲線が縮小されます。

呼び出すときに`CloneWithTransform`、メソッドに渡す、`Func<SKPoint, SKPoint>`を持つ関数は、`SKPaint`パラメーターを返す、`SKPoint`値。 この関数は、カスタム アルゴリズムの変換を適用するポイントごとに呼び出されます。

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

小さな直線、複製のパスを短縮するため、変換関数の直線を曲線に変換する機能はします。

メソッドが各輪郭と呼ばれる、変数内の最初のポイントを保持することに注意してください`firstPoint`とそれぞれの後に現在の位置が、変数にコマンドを描画`lastPoint`です。 これらは、最後の閉じを構築するために必要なときに行、`Close`動詞が発生しました。

**GlobularText**サンプルでは、見かけ上のテキストを折り返す半球の周囲に 3D 効果この拡張メソッドを使用します。

[![](information-images/globulartext-small.png "Globular テキスト ページのスクリーン ショットをトリプル")](information-images/globulartext-large.png#lightbox "Globular テキスト ページのトリプル スクリーン ショット")

[ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs)クラスのコンス トラクターは、この変換を実行します。 作成、 `SKPaint` 、テキストのオブジェクトし、を取得し、`SKPath`オブジェクトから、`GetTextPath`メソッドです。 渡されるパスは、`CloneWithTransform`変換関数と共に拡張メソッド。 

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

変換関数は、まずという 2 つの値を計算`longitude`と`latitude`上部と、テキストの左には、– π/2 ~ π/2 の権限と、テキストの下での範囲です。 これらの値の範囲は、0.75 を掛けることで、縮小効果が、視覚的に満足できればはありません。 (これらの調整なしのコードを実行してください。 テキストになり、北と南の極地の漠然辺に薄すぎる。)これらの 3 次元球座標は、2 次元に変換されます`x`と`y`標準の数式での座標。

新しいパスは、フィールドとして保存されます。 `PaintSurface`ハンドラーしだけを実行する必要があります、画面に表示へのパスを拡大/縮小センターします。

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

これは、非常に多様な手法です。 パスの効果の配列が記述されている場合、 [**パス効果**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)資料は、含める必要があるとするものを含む非常これは、ギャップを埋めるための方法です。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
