---
title: パスおよび領域でのクリッピング
description: この記事では、SkiaSharp パスを使用して、特定の領域にグラフィックスをクリップしたり、領域を作成したりする方法について説明し、サンプルコードを使用してその例を示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 79c32595053cc80ffae91b5434e18690ca2dce4a
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370716"
---
# <a name="clipping-with-paths-and-regions"></a>パスおよび領域でのクリッピング

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_パスを使用して、特定の領域にグラフィックスをクリップし、領域を作成する_

グラフィックのレンダリングを特定の領域に制限することが必要になる場合があります。 これを *クリッピング* と呼びます。 次のような特殊な効果にはクリッピングを使用できます。たとえば、キー穴に見られるサルのイメージです。

![Keyhole を介したサル](clipping-images/clippingsample.png)

*クリッピング領域* は、グラフィックスがレンダリングされる画面の領域です。 クリッピング領域の外に表示されるものはレンダリングされません。 クリッピング領域は通常、四角形またはオブジェクトによって定義され [`SKPath`](xref:SkiaSharp.SKPath) ますが、オブジェクトを使用してクリッピング領域を定義することもでき [`SKRegion`](xref:SkiaSharp.SKRegion) ます。 最初の2種類のオブジェクトは、パスから領域を作成できるため、関連したように見えます。 ただし、地域からパスを作成することはできません。内部では、パスは一連の線と曲線で構成され、一連の水平スキャン行によって領域が定義されます。

上の画像は、サルの **穴** のページを通じて作成されています。 クラスは、  [`MonkeyThroughKeyholePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) SVG データを使用してパスを定義し、コンストラクターを使用してプログラムリソースからビットマップを読み込みます。

```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    SKBitmap bitmap;
    SKPath keyholePath = SKPath.ParseSvgPathData(
        "M 300 130 L 250 350 L 450 350 L 400 130 A 70 70 0 1 0 300 130 Z");

    public MonkeyThroughKeyholePage()
    {
        Title = "Monkey through Keyhole";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

オブジェクトでは、 `keyholePath` keyhole のアウトラインが記述されていますが、座標は完全に任意で、パスデータが考案されたときの便利なものを反映しています。 このため、ハンドラーは `PaintSurface` このパスの境界を取得し、を呼び出して、 `Translate` `Scale` パスを画面の中央に移動し、画面とほぼ同じ高さにするようにします。

```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transform to center and enlarge clip path to window height
        SKRect bounds;
        keyholePath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.98f * info.Height / bounds.Height);
        canvas.Translate(-bounds.MidX, -bounds.MidY);

        // Set the clip path
        canvas.ClipPath(keyholePath);

        // Reset transforms
        canvas.ResetMatrix();

        // Display monkey to fill height of window but maintain aspect ratio
        canvas.DrawBitmap(bitmap,
            new SKRect((info.Width - info.Height) / 2, 0,
                       (info.Width + info.Height) / 2, info.Height));
    }
}
```

ただし、パスはレンダリングされません。 代わりに、変換の後に、次のステートメントを使用してパスを使用してクリッピング領域を設定します。

```csharp
canvas.ClipPath(keyholePath);
```

次に、ハンドラーは、への呼び出しを使用して `PaintSurface` 変換をリセットし、 `ResetMatrix` 画面の高さ全体に拡張するビットマップを描画します。 このコードでは、ビットマップが四角形であることを前提としています。このビットマップは、この特定のビットマップです。 ビットマップは、クリッピングパスで定義されている領域内にのみ表示されます。

[![Keyhole を通じたサルのトリプルスクリーンショット](clipping-images/monkeythroughkeyhole-small.png)](clipping-images/monkeythroughkeyhole-large.png#lightbox)

クリッピングパスは、メソッドが呼び出されたときに有効な変換の影響を受け `ClipPath` ます。また、グラフィカルオブジェクト (ビットマップなど) が表示されたときに有効な変換には適用されません。 クリッピングパスは、メソッドと共に保存され、メソッドを使用して復元されるキャンバスの状態の一部です `Save` `Restore` 。

## <a name="combining-clipping-paths"></a>クリッピングパスの結合

厳密に言えば、クリッピング領域はメソッドによって "設定" されるわけではありません `ClipPath` 。 代わりに、キャンバスと同じサイズの四角形として開始される既存のクリッピングパスと組み合わせて使用します。 クリッピング領域の四角形の境界を取得するには、 [`ClipBounds`](xref:SkiaSharp.SKCanvas.ClipBounds) プロパティまたはプロパティを使用し [`ClipDeviceBounds`](xref:SkiaSharp.SKCanvas.ClipDeviceBounds) ます。 プロパティは、 `ClipBounds` `SKRect` 有効な変換を反映する値を返します。 `ClipDeviceBounds`プロパティは値を返し `RectI` ます。 これは、整数の次元を持つ四角形で、実際のピクセルディメンションのクリッピング領域について説明します。

を呼び出す `ClipPath` と、クリッピング領域が新しい領域と結合され、クリッピング領域が縮小されます。 メソッドの完全な構文 [`ClipPath`](xref:SkiaSharp.SKCanvas.ClipPath(SkiaSharp.SKPath,SkiaSharp.SKClipOperation,System.Boolean)) は次のとおりです。

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

[`ClipRect`](xref:SkiaSharp.SKCanvas.ClipRect(SkiaSharp.SKRect,SkiaSharp.SKClipOperation,System.Boolean))クリッピング領域と四角形を結合するメソッドもあります。

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

既定では、結果のクリッピング領域は、既存のクリッピング領域と、 `SKPath` `SKRect` またはメソッドで指定されたまたはの交差部分になり `ClipPath` `ClipRect` ます。 これについては、 **4 つの円の交差** 部分のクリップページで説明されています。 クラスのハンドラーは、同じオブジェクトを再利用して `PaintSurface`  [`FourCircleInteresectClipPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) `SKPath` 4 つの重なり合う円を作成します。各円は、の連続した呼び出しによってクリッピング領域を縮小し `ClipPath` ます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float size = Math.Min(info.Width, info.Height);
    float radius = 0.4f * size;
    float offset = size / 2 - radius;

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    using (SKPath path = new SKPath())
    {
        path.AddCircle(-offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(-offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColors.Blue;
            canvas.DrawPaint(paint);
        }
    }
}
```

残されているのは、次の4つの円の交点です。

[![4つの円の交差部分のクリップページのトリプルスクリーンショット](clipping-images//fourcircleintersectclip-small.png)](clipping-images/fourcircleintersectclip-large.png#lightbox)

[`SKClipOperation`](xref:SkiaSharp.SKClipOperation)列挙体には、次の2つのメンバーのみが含まれます。

- `Difference` 指定したパスまたは四角形を既存のクリッピング領域から削除します。

- `Intersect` 指定したパスまたは四角形を既存のクリッピング領域と交差させる

クラスの4つの `SKClipOperation.Intersect` 引数を `FourCircleIntersectClipPage` に置き換えると `SKClipOperation.Difference` 、次のように表示されます。

[![差演算を含む4つの円の交差部分の3番目のスクリーンショット](clipping-images//fourcircledifferenceclip-small.png)](clipping-images/fourcircledifferenceclip-large.png#lightbox)

4つの重なり合う円がクリッピング領域から削除されました。

[ **クリップ操作** ] ページでは、これら2つの操作の違いを円のペアだけで示しています。 左側の最初の円は、の既定のクリップ操作でクリッピング領域に追加され `Intersect` ます。一方、右側の2つ目の円は、テキストラベルで示されているクリップ操作でクリッピング領域に追加されます。

[![[クリップ操作] ページのトリプルスクリーンショット](clipping-images//clipoperations-small.png)](clipping-images/clipoperations-large.png#lightbox)

[`ClipOperationsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs)クラスは2つのオブジェクトをフィールドとして定義し、 `SKPaint` 画面を四角形の2つの領域に分割します。 これらの領域は、電話が縦モードと横モードのどちらであるかによって異なります。 `DisplayClipOp`次に、クラスはテキストを表示し、 `ClipPath` 各クリップ操作を示す2つの円パスを使用してを呼び出します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;

    foreach (SKClipOperation clipOp in Enum.GetValues(typeof(SKClipOperation)))
    {
        // Portrait mode
        if (info.Height > info.Width)
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width, y + info.Height / 2), clipOp);
            y += info.Height / 2;
        }
        // Landscape mode
        else
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width / 2, y + info.Height), clipOp);
            x += info.Width / 2;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKClipOperation clipOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(clipOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    canvas.Save();

    using (SKPath path1 = new SKPath())
    {
        path1.AddCircle(xCenter - radius / 2, yCenter, radius);
        canvas.ClipPath(path1);

        using (SKPath path2 = new SKPath())
        {
            path2.AddCircle(xCenter + radius / 2, yCenter, radius);
            canvas.ClipPath(path2, clipOp);

            canvas.DrawPaint(fillPaint);
        }
    }

    canvas.Restore();
}
```

を呼び出す `DrawPaint` と、キャンバス全体にそのオブジェクトが格納され `SKPaint` ますが、この場合、メソッドはクリッピング領域内で描画するだけです。

## <a name="exploring-regions"></a>探索 (リージョンを)

また、オブジェクトの観点からクリッピング領域を定義することもでき [`SKRegion`](xref:SkiaSharp.SKRegion) ます。

新しく作成されたオブジェクトは、 `SKRegion` 空の領域を表します。 通常、オブジェクトの最初の呼び出しは、 [`SetRect`](xref:SkiaSharp.SKRegion.SetRect(SkiaSharp.SKRectI)) 領域が四角形の領域を表すようにするためのものです。 のパラメーターは、 `SetRect` `SKRectI` &mdash; ピクセル単位で四角形を指定するため、整数座標を持つ四角形の値です。 その後、オブジェクトを使用してを呼び出すことができ [`SetPath`](xref:SkiaSharp.SKRegion.SetPath(SkiaSharp.SKPath,SkiaSharp.SKRegion)) `SKPath` ます。 これにより、パスの内部と同じ領域が作成されますが、最初の四角形領域にクリップされます。

この領域は、次のようにメソッドオーバーロードのいずれかを呼び出すことによって変更することもでき [`Op`](xref:SkiaSharp.SKRegion.Op*) ます。

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

[`SKRegionOperation`](xref:SkiaSharp.SKRegionOperation)列挙はに似ていますが、 `SKClipOperation` さらに多くのメンバーがあります。

- `Difference`

- `Intersect`

- `Union`

- `XOR`

- `ReverseDifference`

- `Replace`

呼び出しを行っている領域 `Op` は、メンバーに基づいてパラメーターとして指定された領域と結合され `SKRegionOperation` ます。 最終的には、クリッピングに適した領域を取得するときに、のメソッドを使用して、キャンバスのクリッピング領域として設定でき [`ClipRegion`](xref:SkiaSharp.SKCanvas.ClipRegion(SkiaSharp.SKRegion,SkiaSharp.SKClipOperation)) `SKCanvas` ます。

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

次のスクリーンショットは、6つの領域の操作に基づくクリッピング領域を示しています。 左の円はメソッドが呼び出される領域で、 `Op` 右の円はメソッドに渡される領域です `Op` 。

[![[領域の操作] ページのトリプルスクリーンショット](clipping-images//regionoperations-small.png)](clipping-images/regionoperations-large.png#lightbox)

これらの2つの円を組み合わせることができるかどうか。 結果のイメージは3つのコンポーネントの組み合わせとして考えてみます。これらのコンポーネント自体は、、、およびの各操作で見られ `Difference` `Intersect` `ReverseDifference` ます。 組み合わせの合計数は、2 ~ 3 (8) になります。 不足している2つの領域は、元の領域 (これはまったくを呼び出していません `Op` ) と完全に空の領域です。

最初にパスを作成し、次にそのパスから領域を作成してから、複数の領域を結合する必要があるため、領域をクリッピングに使用するのは困難です。 **領域の操作** ページの全体的な構造は **クリップ操作** によく似ていますが、 [`RegionOperationsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) クラスは画面を6つの領域に分割し、このジョブのリージョンを使用するために必要な追加作業を示しています。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;
    float width = info.Height > info.Width ? info.Width / 2 : info.Width / 3;
    float height = info.Height > info.Width ? info.Height / 3 : info.Height / 2;

    foreach (SKRegionOperation regionOp in Enum.GetValues(typeof(SKRegionOperation)))
    {
        DisplayClipOp(canvas, new SKRect(x, y, x + width, y + height), regionOp);

        if ((x += width) >= info.Width)
        {
            x = 0;
            y += height;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKRegionOperation regionOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(regionOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    SKRectI recti = new SKRectI((int)rect.Left, (int)rect.Top,
                                (int)rect.Right, (int)rect.Bottom);

    using (SKRegion wholeRectRegion = new SKRegion())
    {
        wholeRectRegion.SetRect(recti);

        using (SKRegion region1 = new SKRegion(wholeRectRegion))
        using (SKRegion region2 = new SKRegion(wholeRectRegion))
        {
            using (SKPath path1 = new SKPath())
            {
                path1.AddCircle(xCenter - radius / 2, yCenter, radius);
                region1.SetPath(path1);
            }

            using (SKPath path2 = new SKPath())
            {
                path2.AddCircle(xCenter + radius / 2, yCenter, radius);
                region2.SetPath(path2);
            }

            region1.Op(region2, regionOp);

            canvas.Save();
            canvas.ClipRegion(region1);
            canvas.DrawPaint(fillPaint);
            canvas.Restore();
        }
    }
}
```

メソッドとメソッドの大きな違いは `ClipPath` `ClipRegion` 次のとおりです。

> [!IMPORTANT]
> メソッドとは異なり `ClipPath` 、 `ClipRegion` メソッドは変換の影響を受けません。

この違いを理解するには、リージョンの概要を理解することをお勧めします。 クリップの操作や領域の操作が内部でどのように実装されているかを考えている場合は、非常に複雑に思えるかもしれません。 非常に複雑な可能性のあるいくつかのパスが結合されており、結果のパスの概略はアルゴリズムの悪夢である可能性があります。

このジョブは、各パスが連続した真空管テレビなどの一連の水平スキャン行に縮小された場合に、大幅に簡素化されます。 各スキャンラインは、開始点と終点を持つ水平線にすぎません。 たとえば、半径が10ピクセルの円は、20個の水平スキャン線に分解できます。各行は、円の左端から右の部分に向かって開始されます。 2つの円を任意の領域操作と結合することは非常に単純です。これは、対応するスキャン行の各ペアの始点と終点の座標を調べるだけであるためです。

領域とは、領域を定義する一連の水平スキャン行のことです。

ただし、1つの領域が一連のスキャンラインに縮小されると、これらのスキャン行は特定のピクセルディメンションに基づきます。 厳密に言えば、この領域はベクターグラフィックスオブジェクトではありません。 これは、パスに比べて、圧縮されたモノクロビットマップに近い性質を持ちます。 その結果、地域の調整や回転は、再現性を損なうことなく行うことができます。このため、領域のクリッピングに使用しても、変換されません。

ただし、描画用に領域に変換を適用することができます。 **Region Paint** program 端的は、地域の内部的な性質を示しています。 クラスは、 [`RegionPaintPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) `SKRegion` `SKPath` 10 単位の半径円のに基づいてオブジェクトを作成します。 次に、変換によってその円が拡大され、ページがいっぱいになります。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int radius = 10;

    // Create circular path
    using (SKPath circlePath = new SKPath())
    {
        circlePath.AddCircle(0, 0, radius);

        // Create circular region
        using (SKRegion circleRegion = new SKRegion())
        {
            circleRegion.SetRect(new SKRectI(-radius, -radius, radius, radius));
            circleRegion.SetPath(circlePath);

            // Set transform to move it to center and scale up
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(Math.Min(info.Width / 2, info.Height / 2) / radius);

            // Fill region
            using (SKPaint fillPaint = new SKPaint())
            {
                fillPaint.Style = SKPaintStyle.Fill;
                fillPaint.Color = SKColors.Orange;

                canvas.DrawRegion(circleRegion, fillPaint);
            }

            // Stroke path for comparison
            using (SKPaint strokePaint = new SKPaint())
            {
                strokePaint.Style = SKPaintStyle.Stroke;
                strokePaint.Color = SKColors.Blue;
                strokePaint.StrokeWidth = 0.1f;

                canvas.DrawPath(circlePath, strokePaint);
            }
        }
    }
}
```

この `DrawRegion` 呼び出しは領域をオレンジ色で塗りつぶしますが、呼び出しによって `DrawPath` 元のパスが青色で塗りつぶされ、比較されます。

[![領域のペイントページのトリプルスクリーンショット](clipping-images//regionpaint-small.png)](clipping-images/regionpaint-large.png#lightbox)

この領域は、明確に一連の不連続な座標です。

クリッピング領域に関連付けられた変換を使用する必要がない場合は、 **4 つのリーフの Clover** ページで示すように、領域を使用してクリッピングを行うことができます。 クラスは、 [`FourLeafCloverPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) 4 つの円形領域から複合領域を構築し、その複合領域をクリッピング領域として設定してから、ページの中央から一連の360直線 emanating を描画します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float xCenter = info.Width / 2;
    float yCenter = info.Height / 2;
    float radius = 0.24f * Math.Min(info.Width, info.Height);

    using (SKRegion wholeScreenRegion = new SKRegion())
    {
        wholeScreenRegion.SetRect(new SKRectI(0, 0, info.Width, info.Height));

        using (SKRegion leftRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion rightRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion topRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion bottomRegion = new SKRegion(wholeScreenRegion))
        {
            using (SKPath circlePath = new SKPath())
            {
                // Make basic circle path
                circlePath.AddCircle(xCenter, yCenter, radius);

                // Left leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, 0));
                leftRegion.SetPath(circlePath);

                // Right leaf
                circlePath.Transform(SKMatrix.MakeTranslation(2 * radius, 0));
                rightRegion.SetPath(circlePath);

                // Make union of right with left
                leftRegion.Op(rightRegion, SKRegionOperation.Union);

                // Top leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, -radius));
                topRegion.SetPath(circlePath);

                // Combine with bottom leaf
                circlePath.Transform(SKMatrix.MakeTranslation(0, 2 * radius));
                bottomRegion.SetPath(circlePath);

                // Make union of top with bottom
                bottomRegion.Op(topRegion, SKRegionOperation.Union);

                // Exclusive-OR left and right with top and bottom
                leftRegion.Op(bottomRegion, SKRegionOperation.XOR);

                // Set that as clip region
                canvas.ClipRegion(leftRegion);

                // Set transform for drawing lines from center
                canvas.Translate(xCenter, yCenter);

                // Draw 360 lines
                for (double angle = 0; angle < 360; angle++)
                {
                    float x = 2 * radius * (float)Math.Cos(Math.PI * angle / 180);
                    float y = 2 * radius * (float)Math.Sin(Math.PI * angle / 180);

                    using (SKPaint strokePaint = new SKPaint())
                    {
                        strokePaint.Color = SKColors.Green;
                        strokePaint.StrokeWidth = 2;

                        canvas.DrawLine(0, 0, x, y, strokePaint);
                    }
                }
            }
        }
    }
}
```

これは、4リーフの clover のようには見えませんが、クリッピングなしでレンダリングするのが難しいイメージです。

[![Four-Leaf Clover ページのトリプルスクリーンショット](clipping-images//fourleafclover-small.png)](clipping-images/fourleafclover-large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)