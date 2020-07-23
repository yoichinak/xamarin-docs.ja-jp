---
title: SkiaSharp のパスの基礎
description: この記事では、接続された直線と曲線を組み合わせるための SkiaSharp SKPath オブジェクトについて説明し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a7a4e8c4467438d1f732508a15bee7045310109b
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931223"
---
# <a name="path-basics-in-skiasharp"></a>SkiaSharp のパスの基礎

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_接続された直線と曲線を組み合わせるための SkiaSharp SKPath オブジェクトの探索_

グラフィックスパスの最も重要な特徴の1つは、複数の行が接続されるタイミングと、接続されないようにする必要があるタイミングを定義する機能です。 次の2つの三角形の先頭に示すように、違いは大きくなる可能性があります。

![接続されている行と未接続の線の違いを示す2つの三角形](paths-images/connectedlinesexample.png)

グラフィックスパスは、オブジェクトによってカプセル化され [`SKPath`](xref:SkiaSharp.SKPath) ます。 パスは、1つまたは複数の*輪郭*のコレクションです。 各輪郭は、*接続さ*れた直線と曲線のコレクションです。 輪郭は相互に接続されていませんが、視覚的に重なり合う可能性があります。 場合によっては、1つの輪郭を重ねることができます。

通常、輪郭は、の次のメソッドの呼び出しによって開始され `SKPath` ます。

- [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo*)新しい輪郭を開始するには

このメソッドの引数は単一点で、値として表現することも、 `SKPoint` 個別の X および Y 座標として表すこともできます。 この呼び出しにより、 `MoveTo` 輪郭の先頭と最初の*現在のポイント*にポイントが確立されます。 次のメソッドを呼び出して、現在の点からメソッドに指定された点までの線または曲線で輪郭を続行できます。これは、新しい現在の点になります。

- [`LineTo`](xref:SkiaSharp.SKPath.LineTo*)直線をパスに追加するには
- [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*)円または楕円の円周の線である円弧を追加するには
- [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo*)3次ベジエスプラインを追加するには
- [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo*)2次ベジエスプラインを追加するには
- [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo*)2次ベジエスプラインを追加して、円錐のセクション (楕円、放物線、および hyperbold) を正確にレンダリングできるようにするには

これらの5つの方法には、直線または曲線を説明するために必要なすべての情報が含まれていません。 これらの5つのメソッドは、その直前のメソッド呼び出しによって確立された現在のポイントと組み合わせて動作します。 たとえば、メソッドは、 `LineTo` 現在の点に基づいて輪郭に直線を追加します。そのため、のパラメーター `LineTo` は単なる点です。

また、クラスは、 `SKPath` これらの6つのメソッドと同じ名前を持ち、先頭にを持つメソッドも定義し `R` ます。

- [`RMoveTo`](xref:SkiaSharp.SKPath.RMoveTo*)
- [`RLineTo`](xref:SkiaSharp.SKPath.RLineTo*)
- [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*)
- [`RCubicTo`](xref:SkiaSharp.SKPath.RCubicTo*)
- [`RQuadTo`](xref:SkiaSharp.SKPath.RQuadTo*)
- [`RConicTo`](xref:SkiaSharp.SKPath.RConicTo*)

は、 `R` *相対*を表します。 これらのメソッドは、を含まない対応するメソッドと同じ構文を持ち `R` ますが、現在のポイントに対する相対パスです。 これらは、複数回呼び出すメソッドで、パスの類似部分を描画する場合に便利です。

輪郭は、新しい輪郭を開始するまたはへの別の呼び出しで終了します `MoveTo` `RMoveTo` 。または、の呼び出しによって、 `Close` 輪郭を閉じます。 メソッドは、 `Close` 現在の点から輪郭の最初の点までの直線を自動的に追加し、そのパスを closed としてマークします。これは、ストロークキャップがなくてもレンダリングされることを意味します。

開いている輪郭と閉じた輪郭の違いは、2つの輪郭を持つオブジェクトを使用して2つの三角形をレンダリングする**2 つの三角形の輪郭**ページに示されてい `SKPath` ます。 最初の輪郭が開いており、2番目の輪郭が閉じています。 クラスを次に示し [`TwoTriangleContoursPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/TwoTriangleContoursPage.cs) ます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create the path
    SKPath path = new SKPath();

    // Define the first contour
    path.MoveTo(0.5f * info.Width, 0.1f * info.Height);
    path.LineTo(0.2f * info.Width, 0.4f * info.Height);
    path.LineTo(0.8f * info.Width, 0.4f * info.Height);
    path.LineTo(0.5f * info.Width, 0.1f * info.Height);

    // Define the second contour
    path.MoveTo(0.5f * info.Width, 0.6f * info.Height);
    path.LineTo(0.2f * info.Width, 0.9f * info.Height);
    path.LineTo(0.8f * info.Width, 0.9f * info.Height);
    path.Close();

    // Create two SKPaint objects
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Magenta,
        StrokeWidth = 50
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    // Fill and stroke the path
    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

最初の輪郭は、 [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo(System.Single,System.Single)) 値ではなく X 座標と Y 座標を使用するの呼び出しで構成され、の `SKPoint` 3 つの呼び出しによって [`LineTo`](xref:SkiaSharp.SKPath.LineTo(System.Single,System.Single)) 三角形の3つの辺が描画されます。 2番目の輪郭はの呼び出しは2つだけ `LineTo` ですが、の呼び出しで輪郭を終了します。これにより、 [`Close`](xref:SkiaSharp.SKPath.Close) 輪郭が閉じられます。 この違いは重要です。

[![2つの三角形の輪郭ページのトリプルスクリーンショット](paths-images/twotrianglecontours-small.png)](paths-images/twotrianglecontours-large.png#lightbox "2つの三角形の輪郭ページのトリプルスクリーンショット")

ご覧のように、最初の輪郭は、3つの接続された直線であることが明らかですが、末尾には接続されていません。 2行は上部に重なっています。 2番目の輪郭は明らかに閉じられており、1つより小さい呼び出しを使用して実行されました `LineTo` 。これは、メソッドが輪郭を閉じるための `Close` 最終行を自動的に追加するためです。

`SKCanvas`は1つの [`DrawPath`](xref:SkiaSharp.SKCanvas.DrawPath(SkiaSharp.SKPath,SkiaSharp.SKPaint)) メソッドのみを定義します。このデモでは、パスを塗りつぶしてストロークを描画するために2回呼び出されます。 閉じていないものも含め、すべての輪郭が塗りつぶされます。 閉じていないパスを入力するために、輪郭の始点と終点の間に直線を配置することを想定しています。 最初の輪郭から最後の輪郭を削除した場合、 `LineTo` または2番目の輪郭から呼び出しを削除した場合 `Close` 、各輪郭は2辺のみになりますが、三角形であるかのように塗りつぶされます。

`SKPath`他の多くのメソッドとプロパティを定義します。 次のメソッドは、メソッドに応じて、閉じられているか閉じられていない可能性があるパスに、すべての輪郭を追加します。

- [`AddRect`](xref:SkiaSharp.SKPath.AddRect*)
- [`AddRoundedRect`](xref:SkiaSharp.SKPath.AddRoundedRect(SkiaSharp.SKRect,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddCircle`](xref:SkiaSharp.SKPath.AddCircle(System.Single,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddOval`](xref:SkiaSharp.SKPath.AddOval(SkiaSharp.SKRect,SkiaSharp.SKPathDirection))
- [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single))楕円の円周に曲線を追加するには
- [`AddPath`](xref:SkiaSharp.SKPath.AddPath*)現在のパスに別のパスを追加するには
- [`AddPathReverse`](xref:SkiaSharp.SKPath.AddPathReverse(SkiaSharp.SKPath))逆に別のパスを追加するには

オブジェクトは、 `SKPath` ジオメトリのみを定義する &mdash; 点と、一連のポイントと接続を定義することに注意してください。 がオブジェクトと結合されている場合にのみ、 `SKPath` `SKPaint` 特定の色、ストローク幅などでレンダリングされるパスです。 また、メソッドに渡されたオブジェクトによって、 `SKPaint` `DrawPath` パス全体の特性が定義されていることに注意してください。 複数の色を必要とするものを描画する場合は、色ごとに個別のパスを使用する必要があります。

線の始点と終点の外観がストロークキャップによって定義されているのと同じように、2つの線間の接続の外観は*ストローク結合*によって定義されます。 これを指定するには、 [`StrokeJoin`](xref:SkiaSharp.SKPaint.StrokeJoin) のプロパティを `SKPaint` 列挙体のメンバーに設定し [`SKStrokeJoin`](xref:SkiaSharp.SKStrokeJoin) ます。

- `Miter`まし join の場合
- `Round`丸い結合の場合
- `Bevel`切り取らない結合の場合

[**ストローク結合**] ページには、これら3つのストローク結合が [**ストロークキャップ**] ページに似たコードと共に表示されます。 `PaintSurface`クラスのイベントハンドラーは次の [`StrokeJoinsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeJoinsPage.cs) ようになります。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Right
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width - 100;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = 2 * textPaint.FontSpacing;
    string[] strStrokeJoins = { "Miter", "Round", "Bevel" };

    foreach (string strStrokeJoin in strStrokeJoins)
    {
        // Display text
        canvas.DrawText(strStrokeJoin, xText, y, textPaint);

        // Get stroke-join value
        SKStrokeJoin strokeJoin;
        Enum.TryParse(strStrokeJoin, out strokeJoin);

        // Create path
        SKPath path = new SKPath();
        path.MoveTo(xLine1, y - 80);
        path.LineTo(xLine1, y + 80);
        path.LineTo(xLine2, y + 80);

        // Display thick line
        thickLinePaint.StrokeJoin = strokeJoin;
        canvas.DrawPath(path, thickLinePaint);

        // Display thin line
        canvas.DrawPath(path, thinLinePaint);
        y += 3 * textPaint.FontSpacing;
    }
}
```

実行中のプログラムを次に示します。

[![ストローク結合ページのトリプルスクリーンショット](paths-images/strokejoins-small.png)](paths-images/strokejoins-large.png#lightbox "ストローク結合ページのトリプルスクリーンショット")

マイター結合は、直線が接続する鋭いポイントで構成されます。 2本の線が少しずつ結合している場合、マイター結合が非常に長くなる可能性があります。 マイタ結合が極端に長くならないようにするため、マイター結合の長さは、のプロパティの値によって制限され [`StrokeMiter`](xref:SkiaSharp.SKPaint.StrokeMiter) `SKPaint` ます。 この長さを超えるマイタ結合は、ベベル結合になるように傾斜します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
