---
title: SkiaSharp のパスの基礎
description: この記事では、接続されている直線と曲線を結合するための SkiaSharp SKPath オブジェクトについて説明し、サンプル コードを示します。
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: c892adf2f75ec00c4a9ee171ded78f79bb8227e9
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725195"
---
# <a name="path-basics-in-skiasharp"></a>SkiaSharp のパスの基礎

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_接続された直線と曲線を組み合わせるための SkiaSharp SKPath オブジェクトの探索_

グラフィックス パスの最も重要な機能の 1 つは、複数の行を接続するときとすると、接続できませんを定義する機能です。 違いは、これら 2 つの三角形の上部に示すように重要であることができます。

![](paths-images/connectedlinesexample.png "Two triangles showing the difference between connected and disconnected lines")

グラフィックスパスは、 [`SKPath`](xref:SkiaSharp.SKPath)オブジェクトによってカプセル化されます。 パスは、1つまたは複数の*輪郭*のコレクションです。 各輪郭は、*接続さ*れた直線と曲線のコレクションです。 輪郭が相互に接続されていないが、視覚的に重複する必要があります。 場合があります単一の輪郭が自体で重複ことができます。

通常、輪郭は、次の `SKPath`のメソッドの呼び出しから始まります。

- 新しい輪郭を開始する[`MoveTo`](xref:SkiaSharp.SKPath.MoveTo*)

このメソッドの引数は単一点であり、`SKPoint` 値として表すことも、個別の X および Y 座標として表現することもできます。 `MoveTo` の呼び出しは、輪郭の開始位置と初期の*現在のポイント*を確立します。 行または現在の新しい点となると、メソッドで指定したポイントに現在のポイントから曲線の輪郭を続行するのには、次のメソッドを呼び出すことができます。

- 直線をパスに追加するための[`LineTo`](xref:SkiaSharp.SKPath.LineTo*)
- 円または楕円の円周の線である円弧を追加する[`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*)
- 3次ベジエスプラインを追加する[`CubicTo`](xref:SkiaSharp.SKPath.CubicTo*)
- 2次ベジエスプラインを追加する[`QuadTo`](xref:SkiaSharp.SKPath.QuadTo*)
- 円錐のセクション (省略記号、放物線、および hyperbold) を正確にレンダリングできる、有理数の2次ベジエスプラインを追加する[`ConicTo`](xref:SkiaSharp.SKPath.ConicTo*)ます。

これら 5 つのメソッドのいずれには、直線または曲線を記述するために必要なすべての情報が含まれています。 これら 5 つのメソッドのそれぞれは、直前にあるメソッドの呼び出しによって確立された現在の点と組み合わせて動作します。 たとえば、`LineTo` メソッドは、現在の点に基づいて輪郭に直線を追加します。そのため、`LineTo` のパラメーターは単なる点です。

`SKPath` クラスでは、これらの6つのメソッドと同じ名前で、先頭に `R` を持つメソッドも定義されています。

- [`RMoveTo`](xref:SkiaSharp.SKPath.RMoveTo*)
- [`RLineTo`](xref:SkiaSharp.SKPath.RLineTo*)
- [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*)
- [`RCubicTo`](xref:SkiaSharp.SKPath.RCubicTo*)
- [`RQuadTo`](xref:SkiaSharp.SKPath.RQuadTo*)
- [`RConicTo`](xref:SkiaSharp.SKPath.RConicTo*)

`R` は、*相対*を意味します。 これらのメソッドは、`R` のない対応するメソッドと同じ構文を持ちますが、現在のポイントを基準としています。 これらは、メソッドを複数回呼び出すことで、パスの類似した部分を描画するのに便利です。

輪郭は、新しい輪郭を開始する `MoveTo` または `RMoveTo`への別の呼び出し、または輪郭を閉じる `Close`の呼び出しで終了します。 `Close` メソッドは、現在の点から輪郭の最初の点までの直線を自動的に追加し、そのパスを closed としてマークします。これは、ストロークキャップなしで描画されることを意味します。

開いている輪郭と閉じた輪郭の違いは、2つの輪郭を持つ `SKPath` オブジェクトを使用して2つの三角形をレンダリングする**2 つの三角形の輪郭**ページに示されています。 最初の輪郭が開いており、2 つ目が終了します。 [`TwoTriangleContoursPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/TwoTriangleContoursPage.cs)クラスを次に示します。

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

最初の輪郭は、`SKPoint` 値ではなく X 座標と Y 座標を使用して[`MoveTo`](xref:SkiaSharp.SKPath.MoveTo(System.Single,System.Single))への呼び出しで構成され、次[`LineTo`](xref:SkiaSharp.SKPath.LineTo(System.Single,System.Single))に3つの呼び出しによって三角形の3つの辺が描画されます。 2番目の輪郭には `LineTo` の呼び出しが2つだけありますが、 [`Close`](xref:SkiaSharp.SKPath.Close)の呼び出しによって輪郭が終了し、輪郭が閉じられます。 違いは重要では。

[![](paths-images/twotrianglecontours-small.png "Triple screenshot of the Two Triangle Contours page")](paths-images/twotrianglecontours-large.png#lightbox "Triple screenshot of the Two Triangle Contours page")

ご覧のとおり、最初の輪郭は明らかに一連の 3 つの接続線ですが先頭と末尾が接続されません。 上部にある 2 つの行が重複します。 2番目の輪郭は明らかに閉じられており、`Close` メソッドによって、輪郭を閉じるための最終行が自動的に追加されるため、1つ `LineTo` の呼び出しが1回だけ行われます。

`SKCanvas` で定義される[`DrawPath`](xref:SkiaSharp.SKCanvas.DrawPath(SkiaSharp.SKPath,SkiaSharp.SKPaint))メソッドは1つだけです。この例では、パスを塗りつぶしてストロークを描画するために2回呼び出されます。 すべての輪郭がいっぱいが閉じている場合でもです。 閉じられていないパスの入力のために、直線が開始し、輪郭の始点と終点の間に存在すると見なされます。 最初の輪郭から最後の `LineTo` を削除した場合、または2番目の輪郭からの `Close` 呼び出しを削除した場合、各輪郭は2辺のみになりますが、三角形であるかのように入力されます。

`SKPath` は、他の多くのメソッドとプロパティを定義します。 次のメソッドは、閉じているか、メソッドによって終了していない可能性がありますパスに全体の輪郭を追加します。

- [`AddRect`](xref:SkiaSharp.SKPath.AddRect*)
- [`AddRoundedRect`](xref:SkiaSharp.SKPath.AddRoundedRect(SkiaSharp.SKRect,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddCircle`](xref:SkiaSharp.SKPath.AddCircle(System.Single,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddOval`](xref:SkiaSharp.SKPath.AddOval(SkiaSharp.SKRect,SkiaSharp.SKPathDirection))
- 楕円の円周に曲線を追加する[`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single))
- 現在のパスに別のパスを追加する[`AddPath`](xref:SkiaSharp.SKPath.AddPath*)
- 別のパスを逆に追加する[`AddPathReverse`](xref:SkiaSharp.SKPath.AddPathReverse(SkiaSharp.SKPath))

`SKPath` オブジェクトは、一連のポイントと接続 &mdash; ジオメトリのみを定義することに注意してください。 `SKPath` が `SKPaint` オブジェクトと結合されている場合にのみ、特定の色、ストロークの幅などでレンダリングされるパスです。 また、`DrawPath` メソッドに渡される `SKPaint` オブジェクトは、パス全体の特性を定義することに注意してください。 いくつかの色を必要とする何かを描画する場合は、各色の別のパスを使用する必要があります。

線の始点と終点の外観がストロークキャップによって定義されているのと同じように、2つの線間の接続の外観は*ストローク結合*によって定義されます。 これを指定するには、`SKPaint` の[`StrokeJoin`](xref:SkiaSharp.SKPaint.StrokeJoin)プロパティを[`SKStrokeJoin`](xref:SkiaSharp.SKStrokeJoin)列挙体のメンバーに設定します。

- まし join の `Miter`
- 丸い結合の `Round`
- 切り取った結合の `Bevel`

**[ストローク結合]** ページには、これら3つのストローク結合が **[ストロークキャップ]** ページに似たコードと共に表示されます。 これは、 [`StrokeJoinsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeJoinsPage.cs)クラスの `PaintSurface` イベントハンドラーです。

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

[![](paths-images/strokejoins-small.png "Triple screenshot of the Stroke Joins page")](paths-images/strokejoins-large.png#lightbox "Triple screenshot of the Stroke Joins page")

マイター結合は、行の接続先シャープなポイントで構成されます。 2 つの行は、小さい角度に参加させる、マイター結合は非常に長くなります。 マイタ結合が極端に長くならないようにするため、マイター結合の長さは、`SKPaint`の[`StrokeMiter`](xref:SkiaSharp.SKPaint.StrokeMiter)プロパティの値によって制限されます。 この長さを超えるマイター結合はベベル結合に切り取ったします。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
