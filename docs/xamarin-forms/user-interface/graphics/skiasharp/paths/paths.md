---
title: パスの基礎
description: 接続されている直線と曲線を組み合わせる場合の SkiaSharp SKPath オブジェクトを探索します。
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: b2881148631435c9082b42cad0e784100b010b46
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/05/2018
---
# <a name="path-basics"></a>パスの基礎

_接続されている直線と曲線を組み合わせる場合の SkiaSharp SKPath オブジェクトを探索します。_

グラフィックス パスの最も重要な機能の 1 つは、複数の行を接続するときと、ときに、接続されていないを定義する機能です。 これら 2 つの三角形の最上部が示すように、違いは非常に表示できます。

![](paths-images/connectedlinesexample.png "接続および切断されている行の差異を示す 2 つの三角形")

グラフィックス パスがによってカプセル化、 [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/)オブジェクト。 1 つまたは複数のコレクションが、パスが*輪郭*です。 コレクションは、各輪郭*接続*直線と曲線。 輪郭が相互に接続されていないが、視覚的に重複する可能性があります。 場合があります単一輪郭は、自体に重複できます。

輪郭が通常の次のメソッドの呼び出しで始まる`SKPath`:

- `MoveTo` 新しい輪郭を開始するには

そのメソッドに引数がいずれかを示すことができます、単一のポイントでは、`SKPoint`値または個別の X と Y 座標します。 `MoveTo`呼び出しが、先頭の輪郭と最初のポイントを確立*現在ポイント*です。 行または現在のポイントから新しい点となると、メソッドで指定したポイントに曲線の輪郭を続行する次のメソッドを呼び出すことができます。

- `LineTo` 直線をパスに追加するには
- `ArcTo` 円または楕円の円周上の行には、円弧を追加します
- `CubicTo` 3 次ベジエ スプラインを追加するには
- `QuadTo` 2 次ベジエ スプラインを追加するには
- `ConicTo` 円錐曲線のセクションでは (省略記号ボタン、parabolas、および hyperbolas) を正確に表示できる合理性のある 2 次ベジエ スプラインを追加するには

これら 5 つのメソッドのいずれには、直線または曲線を記述するために必要なすべての情報が含まれています。 これらの 5 つメソッドと連携します直前にあるメソッドの呼び出しによって確立された現在のポイント。 たとえば、`LineTo`メソッドは曲線を直線に基づいて、現在のポイントので、パラメーターを追加`LineTo`ポイントは 1 つだけです。

`SKPath`クラスもが、これら 6 つのメソッドとして同じ名前を持つメソッドを定義、`R`先頭に。

- `RMoveTo`
- `RLineTo`
- `RArcTo`
- `RCubicTo`
- `RQuadTo`
- `RConicTo`

`R`の略*相対*です。 対応することがなくメソッドと同じ構文がある、`R`が現在のポイントに対してはします。 これらは、パスを複数回呼び出すメソッド内の類似した部分を描画するために便利です。

輪郭が別の呼び出しで終わる`MoveTo`または`RMoveTo`、新しい輪郭またはへの呼び出しを開始する`Close`輪郭を終了します。 `Close`メソッドに自動的に、輪郭の最初のポイントに現在の位置から、直線を追加し、パスとしてマークを閉じると、任意ストローク線端なしに描画されることを意味します。

オープンおよびクローズされた輪郭の違いを示す、 **2 つの三角形輪郭**ページを使用して、 `SKPath` 2 つの三角形を表示するために 2 つの輪郭を使用したオブジェクト。 最初の輪郭が開いていて、2 つ目が閉じられます。 ここでは、 [ `TwoTriangleContours` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs)クラス。

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

最初の輪郭への呼び出しから成る[ `MoveTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.MoveTo/p/System.Single/System.Single/) X と Y 座標を使用してではなく、`SKPoint`値の 3 つの呼び出し後に[ `LineTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.LineTo/p/System.Single/System.Single/)の 3 つの辺を描画する、三角形です。 2 番目の輪郭への 2 つの呼び出しがある`LineTo`線への呼び出しを完了するが、 [ `Close`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Close()/)輪郭を終了します。 違いは重要では。

[![](paths-images/twotrianglecontours-small.png "2 つの三角形の輪郭のページのスクリーン ショットをトリプル")](paths-images/twotrianglecontours-large.png#lightbox "2 つの三角形輪郭ページのトリプル スクリーン ショット")

ご覧のように、最初の輪郭明らかに一連の 3 つの接続された線は先頭と末尾にアクセスできなかった。 上部にある 2 つの行が重複します。 2 番目の輪郭が明らかに閉じられ、以下のいずれかで完了しました`LineTo`ためにを呼び出し、`Close`メソッドが自動的に輪郭を閉じるには最後の行を追加します。

`SKCanvas` 1 つだけ定義[ `DrawPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawPath/p/SkiaSharp.SKPath/SkiaSharp.SKPaint/)メソッド、このデモで呼び出される 2 回入力し、パスの境界線の描画にします。 すべての輪郭が入力を終了していないものもされます。 閉じられたパスの入力のために、直線は、始点と終点の輪郭の間に存在すると見なされます。 最後に削除する場合`LineTo`から最初の輪郭、または削除する、`Close`はのみに 2 つの辺を三角形の場合と同様に格納する、2 番目の輪郭各輪郭からの呼び出し。

`SKPath` その他の多くのメソッドやプロパティを定義します。 次の方法では、終了または方法によっては、終了していない可能性がありますパスに全体の輪郭を追加します。

- `AddRect`
- [`AddRoundedRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddRoundedRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddCircle`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddCircle/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddOval`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddOval/p/SkiaSharp.SKRect/SkiaSharp.SKPathDirection/)
- [`AddArc`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) 楕円の円周上、曲線を追加するには
- `AddPath` 現在のパスに別のパスを追加するには
- [`AddPathReverse`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddPathReverse/p/SkiaSharp.SKPath/) 逆の順序で別のパスを追加するには

注意してください、`SKPath`オブジェクト定義のみ geometry&mdash;一連のポイントとの接続。 場合にのみ、`SKPath`と組み合わせて、`SKPaint`オブジェクトが特定の色や線幅などのレンダリングにパスします。 またを注意してください、`SKPaint`オブジェクトに渡される、`DrawPath`メソッドは、パス全体の特性を定義します。 いくつかの色を必要とするものを描画する場合は、それぞれの色の独立したパスを使用する必要があります。

2 本の線の間の接続の外観がによって定義された線幅を開始および行の末尾の外観が定義されていると同様、*ストローク結合*です。 これの設定を指定する、 [ `StrokeJoin` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeJoin/)のプロパティ`SKPaint`のメンバーに、 [ `SKStrokeJoin` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeJoin/)列挙します。

- [`Miter`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Miter/) いや結合
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Round/) 角丸の結合
- [`Bevel`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Bevel/) 切り詰めるオフ結合

**ストローク結合**番組のようなコードとの結合を描くこれら 3 つのページ、**ストローク Cap**ページ。 これは、`PaintSurface`内のイベント ハンドラー、 [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs)クラス。

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

3 つのプラットフォームで実行されているプログラムを次に示します。

[![](paths-images/strokejoins-small.png "トリプル ページのスクリーン ショット、ストロークの結合")](paths-images/strokejoins-large.png#lightbox "トリプル ページのスクリーン ショット、ストロークの結合")

マイター結合は、行の接続先シャープなポイントで構成されます。 2 つの行は、小さな角度で参加するときにマイター結合がかなり長くなることができます。 極端に長くマイター結合を回避するのには、マイター結合の長さの値によって制限されます、 [ `StrokeMiter` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeMiter/)プロパティ`SKPaint`です。 この長さを超えるマイター結合が切り取られるベベルにします。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
