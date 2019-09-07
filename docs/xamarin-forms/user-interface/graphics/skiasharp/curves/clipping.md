---
title: パスおよび領域でのクリッピング
description: この記事では、特定の領域にクリップのグラフィックスを SkiaSharp パスを使用して、領域を作成する方法について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 1daf4822dd7debe98aabd58d42cb6ed29f95b90d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759353"
---
# <a name="clipping-with-paths-and-regions"></a>パスおよび領域でのクリッピング

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_クリップ グラフィックスへのパスを使用して、特定の領域を領域を作成するには_

特定の領域へのグラフィックスのレンダリングを制限する必要があります。 これと呼ばれます*クリッピング*します。 このイメージを keyhole で表示 monkey のなどの特殊効果のクリッピングを使用できます。

![Keyhole を介したサル](clipping-images/clippingsample.png)

*のクリッピング領域*はグラフィックスのレンダリングを画面の領域です。 クリッピング領域の外側に表示されるものは表示されません。 クリッピング領域は通常、四角形によって定義または[ `SKPath` ](xref:SkiaSharp.SKPath)オブジェクトが別の方法として定義できますを使用してクリッピング領域を[ `SKRegion` ](xref:SkiaSharp.SKRegion)オブジェクト。 これら 2 種類のオブジェクトは、パスからの領域を作成するために最初に、関連ようです。 ただし、リージョンからパスを作成することはできません。また、内部的には次のような違いがあります。パスは一連の線と曲線で構成され、領域は一連の水平スキャン行によって定義されます。

上の図は、によって作成された、 **Keyhole を通じて Monkey**ページ。 [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs)クラスは、SVG のデータを使用して、パスを定義し、コンス トラクターを使ってプログラム リソースからビットマップを読み込みます。

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

ただし、`keyholePath`オブジェクトについて、keyhole のアウトラインを説明します、座標は完全に任意と内容は、パスのデータが考案されたときに便利でしたが反映されます。 このため、`PaintSurface`ハンドラーは、このパスと呼び出しの境界を取得します。`Translate`と`Scale`画面とほぼ同じ高さには、画面の中央にパスを移動します。

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

パスは表示されません。 代わりに、次の変換、パスはこのステートメントを使用してクリッピング領域を設定するため。

```csharp
canvas.ClipPath(keyholePath);
```

`PaintSurface`ハンドラーへの呼び出しを使用して、変換をリセットし、`ResetMatrix`し、画面の高さに拡張するビットマップを描画します。 このコードでは、ビットマップが正方形が、この特定のビットマップがあると仮定します。 クリッピング パスで定義される領域内でのみ、ビットマップにレンダリングされます。

[![Keyhole を通じたサルのトリプルスクリーンショット](clipping-images/monkeythroughkeyhole-small.png)](clipping-images/monkeythroughkeyhole-large.png#lightbox)

クリッピング パスが有効でとき、変換の対象、`ClipPath`メソッドが呼び出され、いない変換に有効な (ビットマップ) などのグラフィカル オブジェクトが表示される場合。 クリッピング パスに保存されているキャンバスの状態の一部である、`Save`メソッドで復元されると、`Restore`メソッド。

## <a name="combining-clipping-paths"></a>クリッピング パスの結合

厳密に言えば、クリッピング領域「は設定されていません」、`ClipPath`メソッド。 代わりに、四角形をキャンバスにサイズと同じように始めます。 既存のクリッピング パスと組み合わされます。 クリッピング領域を使用して、四角形の境界を取得することができます、 [ `ClipBounds` ](xref:SkiaSharp.SKCanvas.ClipBounds)プロパティまたは[ `ClipDeviceBounds` ](xref:SkiaSharp.SKCanvas.ClipDeviceBounds)プロパティ。 `ClipBounds`プロパティが返す、`SKRect`変換いずれかを反映した値が有効にすることがあります。 `ClipDeviceBounds`プロパティが返す、`RectI`値。 これは整数のディメンションを含む四角形であり、実際のピクセル寸法のクリッピング領域について説明します。

すべての呼び出しに`ClipPath`新しい領域でクリッピング領域を組み合わせることで、クリッピング領域を削減します。 完全な構文、 [ `ClipPath` ](xref:SkiaSharp.SKCanvas.ClipPath(SkiaSharp.SKPath,SkiaSharp.SKClipOperation,System.Boolean))メソッドは。

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

[ `ClipRect` ](xref:SkiaSharp.SKCanvas.ClipRect(SkiaSharp.SKRect,SkiaSharp.SKClipOperation,System.Boolean))クリッピング領域を四角形と組み合わせたメソッド。

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

既定では、結果のクリッピング領域には、既存のクリッピング領域の積集合、および`SKPath`または`SKRect`が指定されている、`ClipPath`または`ClipRect`メソッド。 これは、方法については、 **4 つの円が交差するクリップ**ページ。 `PaintSurface`ハンドラーで、 [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs)クラスを再利用、同じ`SKPath`連続呼び出しで、クリッピング領域を削減の 4 つの重なり合う円を作成するオブジェクト`ClipPath`:

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

これら 4 つの円の積集合を残骸は残っています。

[![4つの円の交差部分のクリップページのトリプルスクリーンショット](clipping-images//fourcircleintersectclip-small.png)](clipping-images/fourcircleintersectclip-large.png#lightbox)

[ `SKClipOperation` ](xref:SkiaSharp.SKClipOperation)列挙体が 2 つのみのメンバーには。

- `Difference` 既存のクリッピング領域から、指定されたパスまたは四角形を削除します

- `Intersect` 指定されたパスまたは既存のクリッピング領域を含む四角形と交差しています。

4 つに置き換える場合`SKClipOperation.Intersect`の引数、`FourCircleIntersectClipPage`クラス`SKClipOperation.Difference`次が表示されます。

[![差演算を含む4つの円の交差部分の3番目のスクリーンショット](clipping-images//fourcircledifferenceclip-small.png)](clipping-images/fourcircledifferenceclip-large.png#lightbox)

クリッピング領域から削除された 4 つの重なり合う円。

**クリップ操作**ページはこれら 2 つの操作で、円のペアだけの違いを示しています。 左側の最初の円は、既定値のクリップ操作をクリッピング領域に追加されます`Intersect`右側の 2 つ目の円は、テキスト ラベルで示されるクリップ操作と共に、クリッピング領域に追加されます。

[![[クリップ操作] ページのトリプルスクリーンショット](clipping-images//clipoperations-small.png)](clipping-images/clipoperations-large.png#lightbox)

[ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs)クラスは、2 つ定義`SKPaint`フィールドとしてオブジェクトし、し、画面に 2 つの四角形領域分割します。 これらの領域は、電話を縦または横モードではかどうかによって異なります。 `DisplayClipOp`クラスを表示し、テキストと呼び出し`ClipPath`各クリップ操作を説明するために 2 つの円のパス。

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

呼び出す`DrawPaint`通常キャンバス全体を格納すると、`SKPaint`オブジェクトが、今回は、メソッドだけを描画、クリッピング領域内で。

## <a name="exploring-regions"></a>リージョンの調査

クリッピング領域を定義することも、 [ `SKRegion` ](xref:SkiaSharp.SKRegion)オブジェクト。

新しく作成された`SKRegion`オブジェクトが空の領域について説明します。 通常、オブジェクトの最初の呼び出しは[ `SetRect` ](xref:SkiaSharp.SKRegion.SetRect(SkiaSharp.SKRectI))を領域に四角形の領域がについて説明します。 パラメーターを`SetRect`は、`SKRectI`値&mdash;整数値で四角形座標をピクセル単位で四角形が指定されているためです。 呼び出して[ `SetPath` ](xref:SkiaSharp.SKRegion.SetPath(SkiaSharp.SKPath,SkiaSharp.SKRegion))で、`SKPath`オブジェクト。 これは、パスの内部の場合と同じですが、初期の四角形の領域にクリップされる領域を作成します。

いずれかを呼び出すことによって、リージョンを変更することも、 [ `Op` ](xref:SkiaSharp.SKRegion.Op*)ようなメソッドのオーバー ロードします。

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

[ `SKRegionOperation` ](xref:SkiaSharp.SKRegionOperation)列挙型はのような`SKClipOperation`がさらにメンバーがあります。

- `Difference`

- `Intersect`

- `Union`

- `XOR`

- `ReverseDifference`

- `Replace`

作成するリージョン、`Op`に基づくパラメーターとして指定したリージョンと組み合わせて使用で呼び出し、`SKRegionOperation`メンバー。 最後に取得する領域の領域に適した、ときにを使用して、キャンバスのクリッピング領域として設定することができます、 [ `ClipRegion` ](xref:SkiaSharp.SKCanvas.ClipRegion(SkiaSharp.SKRegion,SkiaSharp.SKClipOperation))メソッドの`SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

次のスクリーン ショットは、6 つの地域の操作に基づいて、クリッピング領域を示しています。 左側の円は、リージョンを`Op`でメソッドが呼び出され、右側の円は、リージョンに渡される、`Op`メソッド。

[![[領域の操作] ページのトリプルスクリーンショット](clipping-images//regionoperations-small.png)](clipping-images/regionoperations-large.png#lightbox)

これらすべての可能性をこれら 2 つの円を組み合わせてか? これ自体に表示される 3 つのコンポーネントの組み合わせとして、結果のイメージを検討してください、 `Difference`、 `Intersect`、および`ReverseDifference`操作。 組み合わせの総数は、3 つ目の電源を 2 つまたは 8 です。 不足している 2 つは、元のリージョン (この結果から呼び出さない`Op`まったく) と完全に空の領域。

困難に最初に、そのパスからパスをし、リージョンを作成し、複数のリージョンを結合する必要があるため、クリッピング領域を使用します。 全体的な構造、**リージョン操作**に非常に似ている**クリップ操作**が、 [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs)クラス 6 つの領域に画面を分割してこのジョブにリージョンを使用するために必要な追加の作業を示しています。

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

ここでは、大きな違い、`ClipPath`メソッドと`ClipRegion`メソッド。

> [!IMPORTANT]
> 異なり、`ClipPath`メソッド、`ClipRegion`変換メソッドは影響されません。

この違いの"the rationale"を理解するのには理解するどのリージョンです。 方法クリップ操作または地域の操作が内部的に実装されるを見た場合おそらくよう非常に複雑です。 いくつかの可能性のある非常に複雑なパスが結合されているし、結果のパスのアウトラインは、アルゴリズムの悪夢では可能性があります。

一連の昔ながらの掃除機 tube Tv などの水平方向のスキャン ラインの各パスが短縮された場合、このジョブがかなり簡素化します。 各スキャン ラインは、始点と終点で水平な線だけです。 たとえば、10 ピクセルの半径の円は、20 の水平方向のスキャン ラインをそれぞれの円の左端から始まり、右側の部分で終わりますに分解することができます。 リージョンのいずれかの操作を 2 つの円を組み合わせること、単に対応するスキャン ラインの各ペアの開始と終了座標を調べることである非常に簡単になります。

リージョンは次のようになります。領域を定義する一連の水平スキャン行。

ただし、一連のスキャンに領域を縮小するときに行、行は、特定のピクセル寸法に基づいて、これらのスキャンします。 厳密に言えば、領域は、ベクター グラフィックス オブジェクトではありません。 パスよりも圧縮モノクロ ビットマップと本質的に似ています。 そのため、リージョンを拡大または忠実度を失うことがなく、このような理由のクリッピング領域を使用すると、変換されませんを回転することはできません。

ただし、描画のためのリージョンへの変換を適用できます。 **リージョン ペイント**プログラム端的領域の内部の性質を示します。 [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs)クラスを作成、`SKRegion`オブジェクトに基づいて、 `SKPath` 10 単位の半径の円の。 変換では、円のページを埋めるし展開します。

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

`DrawRegion`呼び出しは、オレンジ色で領域を塗りつぶすときに、`DrawPath`呼び出し青色の比較で元のパスの線します。

[![領域のペイントページのトリプルスクリーンショット](clipping-images//regionpaint-small.png)](clipping-images/regionpaint-large.png#lightbox)

リージョンは、一連の不連続の座標でが明確にします。

クリッピング領域に関連して変換を使用しない場合は、クリッピング用のリージョンを使用として、 **4 つ-リーフ クローバー**ページを示します。 [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs)クラス循環の 4 つのリージョンから複合領域を作成、クリッピング領域としてその複合リージョンを設定して、一連のページの中央から 360 の直線を描画します。

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

4 つ-リーフ クローバー、ように実際には見えませんが、クリッピングなしで表示するためにハードされるイメージ。

[![4つのリーフページのトリプルスクリーンショット](clipping-images//fourleafclover-small.png)](clipping-images/fourleafclover-large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
