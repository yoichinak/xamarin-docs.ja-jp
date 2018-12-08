---
title: パスと SkiaSharp 内のテキスト
description: この記事では、SkiaSharp パスとテキストの交差部分について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 08/01/2017
ms.openlocfilehash: 366a6e9585817c5a47ba5bec14fb2f238ab23a6b
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53050201"
---
# <a name="paths-and-text-in-skiasharp"></a>パスと SkiaSharp 内のテキスト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_パスとテキストの交差部分を調べる_

最新のグラフィックス システムでは、テキストのフォントは、文字アウトラインは、通常は 2 次ベジエ曲線で定義のコレクションです。 その結果、多くの最新のグラフィックス システムには、テキストの文字をグラフィックス パスに変換する機能が含まれます。

できますテキスト文字の輪郭をストロークだけでなくは既に説明しました。 」の説明に従って、特定のストロークの幅とパスの効果も文字アウトラインを表示することができます、 [**パスの効果**](effects.md)記事。 文字の文字列に変換することも、`SKPath`オブジェクト。 つまり、テキスト アウトラインで説明した手法に精通するクリッピングに使用できること、 [**パスおよび領域でクリッピング**](clipping.md)記事。

だけでなく、文字の輪郭をストロークを描画するパスの効果を使用して、文字の文字列から派生したパスに基づくパスの効果を作成することもでき、2 つの効果を組み合わせることもできます。

![](text-paths-images/pathsandtextsample.png "パスのテキスト効果")

前の記事で[**パスの効果**](effects.md)、見た方法、 [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single))メソッドの`SKPaint`ストロークのパスのアウトラインを取得できます。 文字アウトラインから派生したパスにこのメソッドを使用することもできます。

最後に、この記事ではパスとテキストのもう 1 つの交差部分を示します。 [ `DrawTextOnPath` ](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint))メソッドの`SKCanvas`テキストのベースラインに依存して、曲線のパスをテキスト文字列を表示することができます。

## <a name="text-to-path-conversion"></a>テキスト パスへの変換から

[ `GetTextPath` ](xref:SkiaSharp.SKPaint.GetTextPath(System.String,System.Single,System.Single))メソッドの`SKPaint`文字の文字列に変換する`SKPath`オブジェクト。

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x`と`y`引数が、テキストの左側にあるのベースラインの開始点を示します。 同じロールは、ここでとしてを果たす、`DrawText`メソッドの`SKCanvas`します。 このパス内の左側にあるテキストのベースラインが (x, y) 座標があります。

`GetTextPath`単に入力するか、または結果のパスの境界線の描画する場合、メソッドは大げさすぎます。 通常の`DrawText`メソッドは、そのことができます。 `GetTextPath`メソッドは、パスに関連するその他のタスクの方が実用的です。

これらのタスクのいずれかをクリッピングするとします。 **クリッピング テキスト**ページでは、「コードです。」という単語の文字アウトラインに基づいてクリッピング パスを作成します。 このパスは、のイメージを含むビットマップをクリップするページのサイズに拡大されます、**テキストをクリッピング**ソース コード。

[![](text-paths-images/clippingtext-small.png "クリッピングのテキスト ページのスクリーン ショットをトリプル")](text-paths-images/clippingtext-large.png#lightbox "クリッピング テキスト ページの 3 倍になるスクリーン ショット")

[ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs)クラスのコンス トラクター内の埋め込みリソースとして格納されているビットマップを読み込み、**メディア**ソリューションのフォルダー。

```csharp
public class ClippingTextPage : ContentPage
{
    SKBitmap bitmap;

    public ClippingTextPage()
    {
        Title = "Clipping Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.PageOfCode.png";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

`PaintSurface`ハンドラーを作成して開始、`SKPaint`テキストに適したオブジェクト。 `Typeface`プロパティの設定だけでなく`TextSize`、特定のアプリケーションについては、`TextSize`プロパティは、任意の純粋な。 また、あるありません`Style`設定。

`TextSize`と`Style`プロパティの設定は必要ありませんので、この`SKPaint`専用のオブジェクトを使用して、`GetTextPath`テキスト文字列「コード」を使用して呼び出します。 ハンドラーが、結果を測定し、`SKPath`オブジェクトし、中央に配置し、ページのサイズに拡大する 3 つの変換を適用します。 パスは、クリッピング パスとして設定できます。

```csharp
public class ClippingTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Blue);

        using (SKPaint paint = new SKPaint())
        {
            paint.Typeface = SKTypeface.FromFamilyName(null, SKTypefaceStyle.Bold);
            paint.TextSize = 10;

            using (SKPath textPath = paint.GetTextPath("CODE", 0, 0))
            {
                // Set transform to center and enlarge clip path to window height
                SKRect bounds;
                textPath.GetTightBounds(out bounds);

                canvas.Translate(info.Width / 2, info.Height / 2);
                canvas.Scale(info.Width / bounds.Width, info.Height / bounds.Height);
                canvas.Translate(-bounds.MidX, -bounds.MidY);

                // Set the clip path
                canvas.ClipPath(textPath);
            }
        }

        // Reset transforms
        canvas.ResetMatrix();

        // Display bitmap to fill window but maintain aspect ratio
        SKRect rect = new SKRect(0, 0, info.Width, info.Height);
        canvas.DrawBitmap(bitmap,
            rect.AspectFill(new SKSize(bitmap.Width, bitmap.Height)));
    }
}
```

クリッピング パスを設定するとビットマップを表示でき、文字アウトラインを揃えることができます。 使用に注意してください、 [ `AspectFill` ](xref:SkiaSharp.SKRect.AspectFill(SkiaSharp.SKSize))メソッドの`SKRect`縦横比を維持しながら、ページの塗りつぶしの四角形を計算します。

**パスのテキスト効果**ページは、単一のアンパサンド文字を 1 D パスの効果を作成するパスに変換します。 その同じ文字の拡大版の輪郭をストロークを描画するでこの現象がパスの描画オブジェクトを使用しています。

[![](text-paths-images/textpatheffect-small.png "パスのテキスト効果のページのスクリーン ショットをトリプル")](text-paths-images/textpatheffect-large.png#lightbox "パスのテキスト効果のページの 3 倍になるスクリーン ショット")

作業の量、 [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs)クラスは、フィールドとコンス トラクターで発生します。 2 つ`SKPaint`フィールドは、2 つの異なる目的で使用するように定義されたオブジェクト: 最初の (という名前`textPathPaint`) でアンパサンドを変換するために使用、 `TextSize` 1d パス効果のパスに 50 の。 2 番目 (`textPaint`) パスの影響はそのアンパサンドの拡大版を表示するために使用します。 そのため、`Style`この 2 つ目のペイントのオブジェクトに設定`Stroke`が、 `StrokeWidth` 1d パスの効果を使用する場合、そのプロパティが必要はありませんので、プロパティが設定されていません。

```csharp
public class TextPathEffectPage : ContentPage
{
    const string character = "@";
    const float littleSize = 50;

    SKPathEffect pathEffect;

    SKPaint textPathPaint = new SKPaint
    {
        TextSize = littleSize
    };

    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black
    };

    public TextPathEffectPage()
    {
        Title = "Text Path Effect";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Get the bounds of textPathPaint
        SKRect textPathPaintBounds = new SKRect();
        textPathPaint.MeasureText(character, ref textPathPaintBounds);

        // Create textPath centered around (0, 0)
        SKPath textPath = textPathPaint.GetTextPath(character,
                                                    -textPathPaintBounds.MidX,
                                                    -textPathPaintBounds.MidY);
        // Create the path effect
        pathEffect = SKPathEffect.Create1DPath(textPath, littleSize, 0,
                                               SKPath1DPathEffectStyle.Translate);
    }
    ...
}
```

コンス トラクターが最初に使用して、`textPathPaint`でアンパサンドを測定するオブジェクト、 `TextSize` 50。 渡されますが、四角形の中心の座標の符号を反転、`GetTextPath`テキストをパスに変換します。 結果のパスが、(0, 0) 1 D パスの効果に最適であり、文字の中央にポイントします。

あると考えることがあります、`SKPathEffect`コンス トラクターの末尾に作成されたオブジェクトに設定できる、`PathEffect`プロパティの`textPaint`フィールドとして保存するのではなく。 結果を変形ため非常にうまく機能するこのをオンになっていませんが、`MeasureText`呼び出し、`PaintSurface`ハンドラー。

```csharp
public class TextPathEffectPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set textPaint TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Do not measure the text with PathEffect set!
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(character, ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Set the PathEffect property and display text
        textPaint.PathEffect = pathEffect;
        canvas.DrawText(character, xText, yText, textPaint);
    }
}
```

ある`MeasureText`ページ上の文字を中央に呼び出しを使用します。 問題を回避するために、`PathEffect`プロパティは、テキストが測定された後、表示されるまで、ペイント オブジェクトに設定されています。

## <a name="outlines-of-character-outlines"></a>文字アウトラインのアウトライン

通常、 [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single))メソッドの`SKPaint`ペイントのプロパティを適用して、別に 1 つのパスを変換します線の幅とパス効果最も顕著な。 パスの効果を指定せずに使用時に`GetFillPath`実質的に別のパスが説明されているパスを作成します。 これで示されています、**パスをタップすると、アウトライン**ページで、 [**パスの効果**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)記事。

呼び出すこともできます`GetFillPath`から返されるパスに`GetTextPath`最初必要しますが、ありますいないか、完全をどのようなを次に示します。

**文字アウトライン アウトライン**ページは、この手法を示します。 関連するすべてのコードは、`PaintSurface`のハンドラー、 [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs)クラス。

コンス トラクターを作成して開始、`SKPaint`という名前のオブジェクト`textPaint`で、`TextSize`プロパティ ページのサイズに基づきます。 これを使用してパスに変換、`GetTextPath`メソッド。 座標の引数`GetTextPath`画面上のパスを効果的に center:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint())
    {
        // Set Style for the character outlines
        textPaint.Style = SKPaintStyle.Stroke;

        // Set TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Measure the text
        SKRect textBounds = new SKRect();
        textPaint.MeasureText("@", ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Get the path for the character outlines
        using (SKPath textPath = textPaint.GetTextPath("@", xText, yText))
        {
            // Create a new path for the outlines of the path
            using (SKPath outlinePath = new SKPath())
            {
                // Convert the path to the outlines of the stroked path
                textPaint.StrokeWidth = 25;
                textPaint.GetFillPath(textPath, outlinePath);

                // Stroke that new path
                using (SKPaint outlinePaint = new SKPaint())
                {
                    outlinePaint.Style = SKPaintStyle.Stroke;
                    outlinePaint.StrokeWidth = 5;
                    outlinePaint.Color = SKColors.Red;

                    canvas.DrawPath(outlinePath, outlinePaint);
                }
            }
        }
    }
}
```

`PaintSurface`ハンドラーという名前の新しいパスを作成し、`outlinePath`します。 呼び出しで宛先パスになるこの`GetFillPath`します。 `StrokeWidth` 25 の原因のプロパティ`outlinePath`テキスト文字の描画ピクセル幅の 25 パスのアウトラインを記述します。 このパスが 5 のストロークの幅が赤色で表示されます。

[![](text-paths-images/characteroutlineoutlines-small.png "文字アウトライン アウトライン ページのスクリーン ショットをトリプル")](text-paths-images/characteroutlineoutlines-large.png#lightbox "文字アウトライン アウトライン ページの 3 倍になるスクリーン ショット")

よく見るし、パスのアウトラインがシャープな角をにより、重複が表示されます。 これらは、このプロセスの通常の成果物です。

## <a name="text-along-a-path"></a>テキストをパスに沿って

テキストは通常、水平方向のベースラインを表示します。 垂直方向にまたは斜めを実行するテキストを回転することができますが、ベースラインが直線では引き続き。

ただし、テキストを曲線に沿って実行する場合があります。 目的は、これは、 [ `DrawTextOnPath` ](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint))メソッドの`SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

最初の引数で指定したテキストが 2 番目の引数として指定されたパスに沿って実行しようとしています。 テキストを使用してパスの先頭からのオフセットを開始することができます、`hOffset`引数。 通常、テキストのベースラインのフォームのパス: テキスト アセンダーが、パスの 1 つの側があり、もう一方がテキスト ディセンダーします。 使用してパスからテキストのベースラインを補うことができますが、`vOffset`引数。

このメソッドの設定に関するガイダンスを提供する機能はありません、`TextSize`プロパティの`SKPaint`テキストをパスの先頭から最後まで実行を完全にサイズします。 自分でそのテキストのサイズを確認できる場合があります。 それ以外の場合に、次の記事で説明するパス測定関数を使用する必要があります[**パス情報と列挙**](information.md)します。

**循環テキスト**プログラムは、円の周囲のテキストをラップします。 円の円周を判断するので、簡単にちょうど収まるようにテキストのサイズを簡単になります。 `PaintSurface`のハンドラー、 [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs)クラスは、ページのサイズに基づいて円の半径を計算します。 円になります`circularPath`:

```csharp
public class CircularTextPage : ContentPage
{
    const string text = "xt in a circle that shapes the te";
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circularPath = new SKPath())
        {
            float radius = 0.35f * Math.Min(info.Width, info.Height);
            circularPath.AddCircle(info.Width / 2, info.Height / 2, radius);

            using (SKPaint textPaint = new SKPaint())
            {
                textPaint.TextSize = 100;
                float textWidth = textPaint.MeasureText(text);
                textPaint.TextSize *= 2 * 3.14f * radius / textWidth;

                canvas.DrawTextOnPath(text, circularPath, 0, 0, textPaint);
            }
        }
    }
}
```

`TextSize`プロパティの`textPaint`テキストの幅は、円の円周を一致するように、調整します。

[![](text-paths-images/circulartext-small.png "循環テキスト ページのスクリーン ショットをトリプル")](text-paths-images/circulartext-large.png#lightbox "循環テキスト ページの 3 倍になるスクリーン ショット")

多少循環テキスト自体が選択されました:"circle"という単語が両方の文の件名と前置詞句のオブジェクト。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
