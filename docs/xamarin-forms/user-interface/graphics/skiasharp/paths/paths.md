---
title: SkiaSharp のパスの基礎
description: この記事では、接続されている直線と曲線を結合するための SkiaSharp SKPath オブジェクトについて説明し、サンプル コードを示します。
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: eee338461593ad131f679d32cadf63fe3b1a4c40
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759342"
---
# <a name="path-basics-in-skiasharp"></a>SkiaSharp のパスの基礎

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_接続された直線と曲線を結合するための SkiaSharp SKPath オブジェクトを詳細します。_

グラフィックス パスの最も重要な機能の 1 つは、複数の行を接続するときとすると、接続できませんを定義する機能です。 違いは、これら 2 つの三角形の上部に示すように重要であることができます。

![](paths-images/connectedlinesexample.png "2 つの三角形を接続および切断されている行の間の差異の表示")

グラフィックス パスがカプセル化、 [ `SKPath` ](xref:SkiaSharp.SKPath)オブジェクト。 パスを 1 つまたは複数のコレクションである*輪郭*します。 コレクションである各輪郭*接続*直線と曲線。 輪郭が相互に接続されていないが、視覚的に重複する必要があります。 場合があります単一の輪郭が自体で重複ことができます。

輪郭が通常の次のメソッドの呼び出しで開始`SKPath`:

- [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo*) 新しい輪郭を開始するには

メソッドに引数が 1 つのポイントとして表すことができます、`SKPoint`値または個別 X と Y 座標します。 `MoveTo`の呼び出しによって確立輪郭と、初期の先頭にあるポイント*現在ポイント*します。 行または現在の新しい点となると、メソッドで指定したポイントに現在のポイントから曲線の輪郭を続行するのには、次のメソッドを呼び出すことができます。

- [`LineTo`](xref:SkiaSharp.SKPath.LineTo*) 直線をパスに追加するには
- [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*) 円または楕円の円周上の行には、円弧を追加します
- [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo*) 3 次ベジエ スプラインを追加するには
- [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo*) 2 次ベジエ スプラインを追加するには
- [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo*) 円錐のセクションでは (省略記号、放物線、および hyperbolas) で表示できる正確に合理的な二次ベジエ スプラインを追加するには

これら 5 つのメソッドのいずれには、直線または曲線を記述するために必要なすべての情報が含まれています。 これら 5 つのメソッドのそれぞれは、直前にあるメソッドの呼び出しによって確立された現在の点と組み合わせて動作します。 たとえば、`LineTo`メソッドに基づいて追加します直線輪郭を現在のポイントので、パラメーターを`LineTo`ポイントは 1 つのみです。

`SKPath`クラスもこれら 6 つの方法としては、同じ名前を持つメソッドを定義、`R`先頭。

- [`RMoveTo`](xref:SkiaSharp.SKPath.RMoveTo*)
- [`RLineTo`](xref:SkiaSharp.SKPath.RLineTo*)
- [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*)
- [`RCubicTo`](xref:SkiaSharp.SKPath.RCubicTo*)
- [`RQuadTo`](xref:SkiaSharp.SKPath.RQuadTo*)
- [`RConicTo`](xref:SkiaSharp.SKPath.RConicTo*)

`R`略*相対*します。 これらのメソッドがあることがなく対応するメソッドと同じ構文、`R`は、現在の点を基準とします。 これらは、メソッドを複数回呼び出すことで、パスの類似した部分を描画するのに便利です。

別の呼び出しを終わる輪郭`MoveTo`または`RMoveTo`、新しい輪郭またはへの呼び出しを開始する`Close`輪郭を終了します。 `Close`メソッドに自動的に現在のポイントから直線、曲線の最初のポイントを追加し、パスとしてマークを閉じると、つまり、すべてのストローク キャップせずレンダリングされます。

オープンおよびクローズされた輪郭の違いを示します、 **2 つの三角形の輪郭**ページの使用、 `SKPath` 2 つの三角形を表示するために 2 つの輪郭を持つオブジェクト。 最初の輪郭が開いており、2 つ目が終了します。 ここでは、 [ `TwoTriangleContoursPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs)クラス。

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

最初の輪郭の呼び出しから成る[ `MoveTo` ](xref:SkiaSharp.SKPath.MoveTo(System.Single,System.Single)) X と Y 座標を使用してなく`SKPoint`値の 3 つの呼び出し後に[ `LineTo` ](xref:SkiaSharp.SKPath.LineTo(System.Single,System.Single))の 3 つの辺を描画するために、三角形です。 2 番目の輪郭が 2 つしか呼び出し`LineTo`への呼び出しで輪郭を終了するが、 [ `Close`](xref:SkiaSharp.SKPath.Close)輪郭を終了します。 違いは重要では。

[![](paths-images/twotrianglecontours-small.png "2 つの三角形の輪郭のページのスクリーン ショットをトリプル")](paths-images/twotrianglecontours-large.png#lightbox "2 つの三角形の輪郭のページの 3 倍になるスクリーン ショット")

ご覧のとおり、最初の輪郭は明らかに一連の 3 つの接続線ですが先頭と末尾が接続されません。 上部にある 2 つの行が重複します。 2 番目の輪郭が明らかに閉じられるし、少ないのいずれかで完了しました`LineTo`ためにを呼び出し、`Close`メソッドが自動的に輪郭を閉じる最後の行を追加します。

`SKCanvas` 1 つだけ定義[ `DrawPath` ](xref:SkiaSharp.SKCanvas.DrawPath(SkiaSharp.SKPath,SkiaSharp.SKPaint))メソッド、このデモで 2 回呼び出されるを入力し、パスのストロークを描画します。 すべての輪郭がいっぱいが閉じている場合でもです。 閉じられていないパスの入力のために、直線が開始し、輪郭の始点と終点の間に存在すると見なされます。 最後を削除する場合`LineTo`最初の輪郭または削除から、`Close`のみ両側が三角形の場合と同様に入力する 2 つ目の輪郭の各輪郭からの呼び出しが必要があります。

`SKPath` その他の多くのメソッドとプロパティを定義します。 次のメソッドは、閉じているか、メソッドによって終了していない可能性がありますパスに全体の輪郭を追加します。

- [`AddRect`](xref:SkiaSharp.SKPath.AddRect*)
- [`AddRoundedRect`](xref:SkiaSharp.SKPath.AddRoundedRect(SkiaSharp.SKRect,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddCircle`](xref:SkiaSharp.SKPath.AddCircle(System.Single,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddOval`](xref:SkiaSharp.SKPath.AddOval(SkiaSharp.SKRect,SkiaSharp.SKPathDirection))
- [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single)) 楕円の円周上、曲線を追加するには
- [`AddPath`](xref:SkiaSharp.SKPath.AddPath*) 現在のパスに別のパスを追加するには
- [`AddPathReverse`](xref:SkiaSharp.SKPath.AddPathReverse(SkiaSharp.SKPath)) 逆の順序で別のパスを追加するには

注意を`SKPath`オブジェクト定義のジオメトリを&mdash;一連のポイントとの接続。 場合にのみ、`SKPath`を組み合わせて、`SKPaint`オブジェクトが特定の色、線の幅などのレンダリング パス。 また、ことに注意、`SKPaint`オブジェクトに渡される、`DrawPath`メソッドは、パス全体の特性を定義します。 いくつかの色を必要とする何かを描画する場合は、各色の別のパスを使用する必要があります。

2 つの行の間の接続の外観が定義されている先頭と末尾の行の外観を定義している線の幅と同様、*ストローク結合*します。 これを設定して指定する、 [ `StrokeJoin` ](xref:SkiaSharp.SKPaint.StrokeJoin)プロパティの`SKPaint`のメンバーに、 [ `SKStrokeJoin` ](xref:SkiaSharp.SKStrokeJoin)列挙型。

- `Miter` 会議の参加
- `Round` 丸みのある結合
- `Bevel` 細分化オフ結合

**ストローク結合**ページこれら 3 つのストロークのようなコードとの結合を示しています、**ストローク キャップ**ページ。 これは、`PaintSurface`内のイベント ハンドラー、 [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs)クラス。

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

[![](paths-images/strokejoins-small.png "ストロークの結合 ページのスクリーン ショットをトリプル")](paths-images/strokejoins-large.png#lightbox "3 倍になるページのスクリーン ショット、ストロークの結合")

マイター結合は、行の接続先シャープなポイントで構成されます。 2 つの行は、小さい角度に参加させる、マイター結合は非常に長くなります。 極端に長いマイター結合を防ぐためには、マイター結合の長さがの値によって制限されます、 [ `StrokeMiter` ](xref:SkiaSharp.SKPaint.StrokeMiter)プロパティの`SKPaint`します。 この長さを超えるマイター結合はベベル結合に切り取ったします。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
