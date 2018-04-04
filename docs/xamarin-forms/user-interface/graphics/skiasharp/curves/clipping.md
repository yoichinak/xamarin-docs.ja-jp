---
title: パスおよび領域でクリッピング
description: クリップの画像へのパスを使用して、領域を作成して特定の領域
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: charlespetzold
ms.author: chape
ms.date: 06/16/2017
ms.openlocfilehash: 0451653b4ee5c85b9bcf884b6b5609a251cf577c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="clipping-with-paths-and-regions"></a>パスおよび領域でクリッピング

_クリップの画像へのパスを使用して、領域を作成して特定の領域_

特定の領域へのグラフィックのレンダリングを制限する必要があります。 これは呼ば*クリッピング*です。 この猿、keyhole 使用して表示されるイメージなどの特殊効果のクリッピングを使用できます。

![](clipping-images/clippingsample.png "を介して、keyhole サル")

*のクリッピング領域*グラフィックスが表示される画面の領域です。 クリッピング領域の外側に表示されるものは表示されません。 クリッピング領域が通常で定義されている、 [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/)オブジェクトが別の方法として定義できますクリッピング領域を使用して、 [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/)オブジェクト。 これら 2 種類のオブジェクトは、パスから領域を作成するために最初に、関連する思えるかもしれません。 ただし、領域からパスを作成することはできませんし、それらが内部的に非常に異なる: パスは、一連の水平方向のスキャン ライン、地域が定義されている一連の直線と曲線、構成されています。

上記の図は、によって作成された、 **Keyhole を通じてサル**ページ。 [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs)クラス SVG データを使用してパスを定義し、コンス トラクターを使用してプログラム リソースからビットマップを読み込めません。

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

ただし、 `keyholePath` keyhole の輪郭を記述するオブジェクト、座標が完全に任意とパスのデータが考案されたときに何が便利な反映します。 このため、`PaintSurface`ハンドラーは、このパスおよび呼び出しの境界を取得`Translate`と`Scale`ほぼ高さを画面には、画面の中央にパスを移動します。


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

パスはレンダリングされません。 代わりに、次の変換は、次のステートメントでのクリッピング領域を設定するパスが使用します。

```csharp
canvas.ClipPath(keyholePath);
```

`PaintSurface`ハンドラーを呼び出して変換をリセットし、`ResetMatrix`し、画面の高さに拡張するビットマップを描画します。 このコードでは、ビットマップがある正方形では、この特定のビットマップを前提としています。 クリッピング パスを使用して定義される領域内でのみ、ビットマップを描画します。

[![](clipping-images/monkeythroughkeyhole-small.png "Keyhole ページを通じてサル用のトリプル スクリーン ショット")](clipping-images/monkeythroughkeyhole-large.png#lightbox "Keyhole ページを通じてサル用のトリプル スクリーン ショット")

クリッピング パスが有効でとき、変換の対象、`ClipPath`メソッドが呼び出されない、変換に有効で、グラフィカル オブジェクト (ビットマップ) などが表示される場合。 クリッピング パスはと共に保存されるキャンバス状態の一部である、`Save`メソッドで復元されると、`Restore`メソッドです。

## <a name="combining-clipping-paths"></a>クリッピング パスの結合

厳密には、クリッピング領域がありません"set"、`ClipPath`メソッドです。 代わりに、既存のクリッピングのパスは、画面サイズと等しい四角形として開始と組み合わされます。 クリッピング領域を使用して、四角形の境界を取得することができます、 [ `ClipBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipBounds/)プロパティまたは[ `ClipDeviceBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipDeviceBounds/)プロパティです。 `ClipBounds`プロパティから返される、`SKRect`をいずれかの変換を表す値が有効にすることがあります。 `ClipDeviceBounds`プロパティから返される、`RectI`値。 これは整数のディメンションを含む四角形であり、実際のピクセル数でのクリッピング領域について説明します。

すべての呼び出しに`ClipPath`を新しい領域のクリッピング領域を組み合わせることにより、クリッピング領域を削減します。 完全な構文、 [ `ClipPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipPath/p/SkiaSharp.SKPath/SkiaSharp.SKClipOperation/System.Boolean/)メソッドは。

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

[ `ClipRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRect/p/SkiaSharp.SKRect/SkiaSharp.SKClipOperation/System.Boolean/)四角形のクリッピング領域を結合する方法。

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

既定では、結果のクリッピング領域が、既存のクリッピング領域の積集合と`SKPath`または`SKRect`で指定されている、`ClipPath`または`ClipRect`メソッドです。 これに示されている、 **4 つの円が交差するクリップ**ページ。 `PaintSurface`ハンドラー、 [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs)クラスを再利用同じ`SKPath`連続呼び出しでのクリッピング領域を削減それぞれ 4 つの重なり合う円を作成するオブジェクト`ClipPath`:

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

これら 4 つの円の交差部分。

[![](clipping-images//fourcircleintersectclip-small.png "次の 4 つの円が交差するクリップのページのスクリーン ショットをトリプル")](clipping-images/fourcircleintersectclip-large.png#lightbox "円が交差するクリップの 4 つのページのトリプル スクリーン ショット")

[ `SKClipOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKClipOperation/)列挙体には 2 つのメンバー。

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Difference/) 指定されたパスまたは四角形を既存のクリッピング領域から削除します。

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Intersect/) 指定されたパスまたは既存のクリッピング領域を含む四角形と交差しています。

4 つを置換する場合`SKClipOperation.Intersect`の引数、`FourCircleIntersectClipPage`クラス`SKClipOperation.Difference`次が表示されます。

[![](clipping-images//fourcircledifferenceclip-small.png "トリプル ページのスクリーン ショット、4 つの円が交差するクリップ差分演算と")](clipping-images/fourcircledifferenceclip-large.png#lightbox "トリプル ページのスクリーン ショット、4 つの円が交差するクリップ差分演算で")

4 つの重なり合う円は、クリッピング領域から削除されています。

**クリップ操作**ページは、これら 2 つの操作で、円のペアだけの違いを示します。 左の最初の円が既定値のクリップ操作をクリッピング領域に追加`Intersect`テキスト ラベルで示されるクリップ操作と共に、右側に 2 番目の円がクリッピング領域に追加中に、します。

[![](clipping-images//clipoperations-small.png "クリップ オペレーション ページのスクリーン ショットをトリプル")](clipping-images/clipoperations-large.png#lightbox "クリップ オペレーション ページのトリプル スクリーン ショット")

[ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs)クラスは、2 つ定義`SKPaint`フィールドとしてオブジェクトし、し、画面に 2 つの四角形領域分割します。 これらの領域は、電話が縦または横モードではかどうかによって異なります。 `DisplayClipOp`クラスを表示し、テキストと呼び出し`ClipPath`各クリップ操作を説明するために 2 つの円のパス。

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

呼び出す`DrawPaint`通常キャンバス全体を格納すると、`SKPaint`オブジェクトが、この場合は、メソッドだけ描画クリッピング領域内で。

## <a name="exploring-regions"></a>領域の表示

API ドキュメントを調査した場合`SKCanvas`のオーバー ロードお気付きかも、`ClipPath`と`ClipRect`メソッドは、上記の方法に似ていますが、代わりをという名前のパラメーターがある[ `SKRegionOperation`](https://developer.xamarin.com/api/type/SkiaSharp.SKRegionOperation/)なく`SKClipOperation`です。 `SKRegionOperation` フォームのクリッピング領域へのパスを組み合わせることで柔軟性が多少向上、6 つのメンバーがあります。

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Difference/)

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Intersect/)

- [`Union`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Union/)

- [`XOR`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.XOR/)

- [`ReverseDifference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.ReverseDifference/)

- [`Replace`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Replace/)

ただしのオーバー ロード`ClipPath`と`ClipRect`で`SKRegionOperation`パラメーターは廃止され、使用できません。

使用することもできます、`SKRegionOperation`が列挙体の観点でのクリッピング領域を定義することが必要です、 [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/)オブジェクト。

新しく作成した`SKRegion`オブジェクトには、空の領域がについて説明します。 オブジェクトの最初の呼び出しは、通常[ `SetRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetRect/p/SkiaSharp.SKRectI/)領域に四角形の領域がについて説明するようにします。 パラメーターを`SetRect`、`SKRectI`値&mdash;整数のプロパティを持つ四角形の値。 呼び出してできます[ `SetPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetPath/p/SkiaSharp.SKPath/SkiaSharp.SKRegion/)で、`SKPath`オブジェクト。 これは、パスの内部と同じですが、初期の四角形領域のクリップ先する領域を作成します。

`SKRegionOperation`列挙型のみが関係の 1 つを呼び出すときに、 [ `Op` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.Op/p/SkiaSharp.SKRegion/SkiaSharp.SKRegionOperation/)このなどのメソッドのオーバー ロードします。

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

加えようとするリージョンを`Op`上での呼び出しに基づくパラメーターとして指定された領域と組み合わせて、`SKRegionOperation`メンバー。 取得すると最後に、領域の領域に適したを使用して、キャンバスのクリッピング領域として設定できます、 [ `ClipRegion` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRegion/p/SkiaSharp.SKRegion/SkiaSharp.SKClipOperation/)メソッドの`SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

次のスクリーン ショットは、6 つの地域の操作に基づくクリッピング領域を示しています。 左側の円は、領域を`Op`で、メソッドが呼び出され、右側の円は、地域に渡される、`Op`メソッド。

[![](clipping-images//regionoperations-small.png "領域の管理 ページのスクリーン ショットをトリプル")](clipping-images/regionoperations-large.png#lightbox "領域の管理 ページのトリプル スクリーン ショット")

これらすべての可能性をこれら 2 つの円を組み合わせてとは 単独でに表示される 3 つのコンポーネントの組み合わせとして、結果のイメージを検討してください、 `Difference`、 `Intersect`、および`ReverseDifference`操作します。 組み合わせの総数は、3 つ目の電源を 2 つまたは 8 です。 不足している 2 つは、元の地域 (いない呼び出し元からの結果を`Op`まったく) と完全に空の領域。

これは困難領域を使用して、そのパスから、パスと、地域をまず作成し、複数の領域を結合する必要があるためにクリップするためです。 全体的な構造、**地域の操作**によく似ていますページ**クリップ操作**ですが、 [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs)クラスは、画面 6 つの領域に分割し、このジョブに領域を使用するために必要な追加の作業を示しています。

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

ここでは、大きな違い、`ClipPath`メソッドおよび`ClipRegion`メソッド。

> [!IMPORTANT]
> 異なり、 `ClipPath` 、メソッド、`ClipRegion`変換メソッドは影響しません。

この違いの論理的根拠を理解するのには理解しておいては、どのような領域。 クリップ操作または地域の操作が実装する方法内部的に考えましたとしている場合は、おそらくよう非常に複雑です。 いくつかの可能性のある非常に複雑なパスを結合されている、および結果のパスのアウトラインがアルゴリズムに大変な作業では可能性があります。

ただし、このジョブは大幅に簡略化各パスが、一連の昔ながら掃除 tube Tv などの水平方向のスキャン ラインに縮小された場合。 各スキャン ラインは、始点と終点で水平方向だけです。 たとえば、10 の半径の円は、20 の水平方向のスキャン ライン、それぞれの円の左端から開始し、右側の部分で終了に分解することができます。 2 つの円の領域の操作を組み合わせること対応のスキャン ラインの各ペアの始点と終点の座標を確認するだけで簡単になっているため非常に単純になります。

これは、どのような領域: 領域を定義する一連の線の水平方向のスキャンします。

ただし、一連のスキャンに領域を縮小するときに行、特定のピクセルのディメンションに基づいた行これらスキャンします。 厳密には、領域はベクター グラフィックス オブジェクトではありません。 パスにより圧縮モノクロ ビットマップには、本質的に近いです。 そのため、領域を拡大縮小または信頼性のあるを失うことがなく、このため、これらは変換されませんクリッピング領域を使用する場合を回転することはできません。

ただし、描画用の領域に変換を適用できます。 **地域ペイント**プログラムが鮮明に領域の内側の性質を示します。 [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs)クラスを作成、`SKRegion`オブジェクトに基づいて、 `SKPath` 10 単位の半径の円のです。 トランス フォームでは、円のページを埋めるし展開します。

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

`DrawRegion`呼び出しがオレンジ色で領域を塗りつぶします中に、`DrawPath`呼び出しの比較は青で元のパスの線します。

[![](clipping-images//regionpaint-small.png "領域 [ペイント] ページのスクリーン ショットをトリプル")](clipping-images/regionpaint-large.png#lightbox "地域ペイント ページのトリプル スクリーン ショット")

地域が一連の個別の座標では明確にします。

場合は、クリッピング領域に関連して変換を使用する必要はありません、として使用できます領域、クリッピング、 **4 つ-リーフ クローバー**ページを示しています。 [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs)クラスの 4 つの循環領域から複合領域を作成、クリッピング領域としてその複合地域を設定、および一連のページの中央から 360 の直線を描画します。

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

これが 4 つ-リーフ クローバーようになりますが実際には、クリッピングなしで表示するためにハードされる可能性があるイメージ。

[![](clipping-images//fourleafclover-small.png "4 つ-リーフ クローバー ページのスクリーン ショットをトリプル")](clipping-images/fourleafclover-large.png#lightbox "-リーフ クローバーの 4 つのページのトリプル スクリーン ショット")


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
