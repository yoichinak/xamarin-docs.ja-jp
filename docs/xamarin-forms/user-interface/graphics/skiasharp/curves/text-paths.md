---
title: SkiaSharp のパスとテキスト
description: この記事では、SkiaSharp のパスとテキストの共通部分について説明し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 08/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3da24f803c6d41564c2c4d556a108a324aa78dc4
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93369975"
---
# <a name="paths-and-text-in-skiasharp"></a>SkiaSharp のパスとテキスト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_パスとテキストの共通部分を調べる_

最新のグラフィックスシステムでは、テキストフォントは、通常、2次ベジエ曲線で定義される文字アウトラインのコレクションです。 そのため、最新のグラフィックスシステムの多くには、テキスト文字をグラフィックスパスに変換する機能が含まれています。

テキスト文字のアウトラインを描画するだけでなく、テキストを塗りつぶすこともできます。 これにより、 [**パス効果**](effects.md) の記事で説明されているように、特定のストロークの幅とパスの効果を使用して、これらの文字アウトラインを表示できます。 ただし、文字列をオブジェクトに変換することもでき `SKPath` ます。 つまり、テキストのアウトラインを使用して、 [**パスと領域のクリッピング**](clipping.md) に関する記事で説明されている手法とのクリッピングを行うことができます。

パス効果を使用して文字アウトラインを描くだけでなく、文字文字列から派生したパスに基づいたパス効果を作成することもできます。また、次の2つの効果を組み合わせることもできます。

![テキストパスの効果](text-paths-images/pathsandtextsample.png)

[**パスの効果**](effects.md)に関する前の記事では、 [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single)) のメソッドが `SKPaint` ストロークパスの輪郭を取得する方法について説明しました。 このメソッドは、文字アウトラインから派生したパスでも使用できます。

最後に、この記事では、パスとテキストのもう1つの交差部分を示します。 [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) のメソッドを使用すると、テキスト `SKCanvas` 文字列を表示して、テキストのベースラインが曲線のパスに続くようにすることができます。

## <a name="text-to-path-conversion"></a>テキストからパスへの変換

[`GetTextPath`](xref:SkiaSharp.SKPaint.GetTextPath(System.String,System.Single,System.Single))のメソッドは、 `SKPaint` 文字列をオブジェクトに変換し `SKPath` ます。

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x`引数と `y` 引数は、テキストの左辺のベースラインの開始点を示します。 ここでは、のメソッドと同じ役割を果たし `DrawText` `SKCanvas` ます。 パス内では、テキストの左側のベースラインに座標 (x, y) が設定されます。

メソッドは、 `GetTextPath` 結果のパスの塗りつぶしまたはストロークを行うだけの場合は過剰です。 通常の方法では、これを行うことができ `DrawText` ます。 メソッドは、 `GetTextPath` パスに関連する他のタスクにとって便利です。

これらのタスクの1つはクリッピングです。 **クリッピングテキスト** ページは、"CODE" という単語の文字アウトラインに基づいてクリッピングパスを作成します。 このパスは、ページのサイズに拡張され、 **クリッピングテキスト** のソースコードのイメージを含むビットマップをクリップします。

[![クリッピングテキストページのトリプルスクリーンショット](text-paths-images/clippingtext-small.png)](text-paths-images/clippingtext-large.png#lightbox "クリッピングテキストページのトリプルスクリーンショット")

[`ClippingTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs)クラスコンストラクターは、埋め込みリソースとして格納されているビットマップをソリューションの **メディア** フォルダーに読み込みます。

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

ハンドラーは、 `PaintSurface` まず、 `SKPaint` テキストに適したオブジェクトを作成します。 プロパティはと同様に `Typeface` 設定され `TextSize` ますが、この特定のアプリケーションで `TextSize` は、プロパティは純粋に任意です。 また、設定がないことにも注意 `Style` してください。

`TextSize` `Style` この `SKPaint` オブジェクトは、 `GetTextPath` テキスト文字列 "CODE" を使用した呼び出しにのみ使用されるため、プロパティとプロパティの設定は必要ありません。 次に、結果のオブジェクトを測定し、 `SKPath` 3 つの変換を適用して中央に配置し、ページのサイズにスケーリングします。 その後、パスをクリッピングパスとして設定できます。

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

クリッピングパスを設定すると、ビットマップが表示され、文字のアウトラインにクリップされます。 [`AspectFill`](xref:SkiaSharp.SKRect.AspectFill(SkiaSharp.SKSize)) `SKRect` 縦横比を維持したままページに入力するための四角形を計算するのメソッドを使用することに注意してください。

[ **テキストパスの効果** ] ページでは、1つのアンパサンド文字をパスに変換して、1d パス効果を作成します。 このパス効果を持つ描画オブジェクトは、同じ文字のより大きなバージョンのアウトラインを描画するために使用されます。

[![[テキストパスの効果] ページのトリプルスクリーンショット](text-paths-images/textpatheffect-small.png)](text-paths-images/textpatheffect-large.png#lightbox "[テキストパスの効果] ページのトリプルスクリーンショット")

クラスの作業の多くは、 [`TextPathEffectPath`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) フィールドとコンストラクターで発生します。 フィールドとして定義されている2つのオブジェクトは、 `SKPaint` 2 つの異なる目的で使用されます。1つ目の (名前付き) を使用して、が `textPathPaint` 50 のアンパサンドを、 `TextSize` 1d パス効果のパスに変換します。 2番目の ( `textPaint` ) は、そのパスの効果を持つアンパサンドの大きなバージョンを表示するために使用されます。 このため、 `Style` この2番目の描画オブジェクトのはに設定され `Stroke` ますが、 `StrokeWidth` プロパティは設定されません。これは、1d パス効果を使用する場合に、このプロパティが必要ないためです。

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

コンストラクターは、最初にオブジェクトを使用して `textPathPaint` 、が50のアンパサンドを計測し `TextSize` ます。 その四角形の中心座標の負の値がメソッドに渡され、 `GetTextPath` テキストがパスに変換されます。 結果のパスは、文字の中央に (0, 0) のポイントがあります。これは、1D パスの効果に最適です。

`SKPathEffect`コンストラクターの最後に作成されたオブジェクトが、 `PathEffect` `textPaint` フィールドとして保存されるのではなく、のプロパティに設定されている可能性があります。 しかし、これはハンドラーでの呼び出しの結果をゆがめてしまうため、非常にうまく機能しませんでした `MeasureText` `PaintSurface` 。

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

この `MeasureText` 呼び出しは、ページの文字を中央揃えにするために使用されます。 問題を回避するために、 `PathEffect` テキストが測定された後、表示される前に、プロパティが paint オブジェクトに設定されます。

## <a name="outlines-of-character-outlines"></a>文字アウトラインの概要

通常、 [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single)) のメソッドは、 `SKPaint` ペイントプロパティを適用することによってパスを別のパスに変換します。これは、特にストロークの幅とパスの効果です。 パスの効果なしで使用すると、に `GetFillPath` よって、別のパスを示すパスが効果的に作成されます。 これについては、 [**パス効果**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)の記事の「タップして **パスページをアウトラインする** 」で説明しました。

`GetFillPath`から返されたパスに対してを呼び出すこともできますが、最初はのように `GetTextPath` 見えます。

「 **文字アウトラインの概要** 」ページでは、その方法を示しています。 関連するすべてのコードは、クラスのハンドラーに含まれてい `PaintSurface` [`CharacterOutlineOutlinesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) ます。

コンストラクターは、 `SKPaint` `textPaint` `TextSize` ページのサイズに基づいてプロパティを持つという名前のオブジェクトを作成することから始めます。 これは、メソッドを使用してパスに変換され `GetTextPath` ます。 `GetTextPath`画面上のパスを効果的に中央揃えにするための座標引数。

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

ハンドラーは、 `PaintSurface` という名前の新しいパスを作成し `outlinePath` ます。 これは、の呼び出しの宛先パスになり `GetFillPath` ます。 `StrokeWidth`25 のプロパティを使うと、は、 `outlinePath` テキスト文字を描画する25ピクセル幅のパスのアウトラインを記述します。 このパスは赤で表示され、ストロークの幅は5になります。

[![文字アウトラインの概要ページのトリプルスクリーンショット](text-paths-images/characteroutlineoutlines-small.png)](text-paths-images/characteroutlineoutlines-large.png#lightbox "文字アウトラインの概要ページのトリプルスクリーンショット")

詳しく見てみましょう。パスの輪郭が鋭い角になっている場所が重なっていることがわかります。 これらは、このプロセスの通常の成果物です。

## <a name="text-along-a-path"></a>パスに沿ったテキスト

通常、テキストは水平方向のベースラインに表示されます。 テキストを回転して垂直方向または斜めに実行できますが、ベースラインは直線のままです。

ただし、曲線に沿ってテキストを実行する場合には、時間がかかることがあります。 これは、 [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) のメソッドの目的です `SKCanvas` 。

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

最初の引数で指定されたテキストは、2番目の引数として指定されたパスに沿って実行されます。 パスの先頭からのオフセットで、引数を使用してテキストを開始でき `hOffset` ます。 通常、パスはテキストのベースラインを形成します。テキストのアセンダーはパスの片側にあり、テキストのディセンダーはもう一方の側にあります。 ただし、引数を使用してパスからテキストベースラインをオフセットでき `vOffset` ます。

このメソッドには、のプロパティを設定して、 `TextSize` `SKPaint` パスの先頭から最後までテキストを完全に実行できるようにするためのガイダンスを提供する機能はありません。 場合によっては、独自のテキストサイズを把握できます。 また、パス測定関数を使用する必要があります。 [**パス情報と列挙**](information.md)については、次の記事で説明します。

**円形テキスト** プログラムは、円の周りのテキストをラップします。 円の円周を簡単に判断できるため、テキストのサイズを正確に調整するのは簡単です。 `PaintSurface`クラスのハンドラーは、 [`CircularTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) ページのサイズに基づいて円の半径を計算します。 この円は次のようになり `circularPath` ます。

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

次に、のプロパティを調整して、 `TextSize` `textPaint` テキストの幅が円の円周と一致するようにします。

[![円形テキストページのトリプルスクリーンショット](text-paths-images/circulartext-small.png)](text-paths-images/circulartext-large.png#lightbox "円形テキストページのトリプルスクリーンショット")

テキスト自体も少し円形になるように選択されています。単語 "circle" は、文の件名と事前位置フレーズのオブジェクトの両方です。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)