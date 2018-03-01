---
title: "パスとテキスト"
description: "パスとテキストの交差部分を調べる"
ms.topic: article
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/01/2017
ms.openlocfilehash: bf382f380876e85db46226fb3586382f20d630f2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="paths-and-text"></a>パスとテキスト

_パスとテキストの交差部分を調べる_

最新のグラフィックス システムでは、テキストのフォントは、2 次ベジエ曲線によって定義された通常の文字のアウトラインのコレクションです。 そのため、多くの最新のグラフィックス システムには、テキストの文字をグラフィックス パスに変換する機能が含まれます。 

既に見たようことするテキストの文字の輪郭を描画したりできるように入力します。 」の説明に従って、特定のストロークの幅とパスの効果も文字アウトラインを表示できます、 [**パス効果**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)資料です。 文字列に変換することも、`SKPath`オブジェクト。 つまりで説明した手法でクリッピングのテキストのアウトラインを使用できること、 [**パスおよび領域でクリッピング**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)資料です。 

文字のアウトラインを描画するパスの効果を使用して、だけでなく、パスに基づいている効果は、文字の文字列から派生するパスを作成することもできます。 と 2 つの効果を組み合わせることもできます。

![](text-paths-images/pathsandtextsample.png "パスのテキスト効果")

[前の記事](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)という方法、 [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/)メソッドの`SKPaint`ストロークのパスの概要を取得できます。 文字アウトラインから派生したパスが、このメソッドを使用することもできます。

この記事の内容が最後に、パスとテキストのもう 1 つの交差部分を示します。 [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/)メソッドの`SKCanvas`テキストのベースライン曲線のパスに依存するように、テキスト文字列を表示することができます。

## <a name="text-to-path-conversion"></a>テキストをパスに変換する

[ `GetTextPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetTextPath/p/System.String/System.Single/System.Single/)メソッドの`SKPaint`文字の文字列に変換する`SKPath`オブジェクト。

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x`と`y`引数は、テキストの左側のベースラインの開始時点を指定します。 役割を果たします、同じここで、`DrawText`メソッドの`SKCanvas`します。 パス内でテキストの左側のベースラインは座標 (x, y) があります。 

`GetTextPath`を入力するか、または結果のパスの境界線の描画だけを実行する場合、メソッドが過剰です。 法線`DrawText`メソッドを実行することができます。 `GetTextPath`メソッドは、パスに関連するその他のタスクの方が実用的です。

これらのタスクのいずれかがクリッピングされます。 **クリッピング テキスト**ページは、単語"CODE"の文字のアウトラインに基づいてクリッピング パスを作成します。 このパスのイメージを含むビットマップをクリップするページのサイズに合わせて引き伸ばされます、**クリッピング テキスト**ソース コード。

[![](text-paths-images/clippingtext-small.png "領域のテキスト ページのスクリーン ショットをトリプル")](text-paths-images/clippingtext-large.png "クリッピング テキスト ページのトリプル スクリーン ショット")

[ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs)クラスのコンス トラクターが内の埋め込みリソースとして格納されているビットマップを読み込み、**メディア**ソリューションのフォルダー。

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

`PaintSurface`ハンドラーが作成した後、`SKPaint`オブジェクトのテキストに適してします。 `Typeface`プロパティの設定だけでなく、 `TextSize`、特定のアプリケーションについては、`TextSize`プロパティは、純粋な arbirtrary です。 あることを確認もない`Style`設定します。

`TextSize`と`Style`プロパティ設定が必要ないためこれ`SKPaint`専用のオブジェクトを使用して、`GetTextPath`テキスト文字列「コード」を使用して呼び出します。 ハンドラーが、結果を測定し、`SKPath`オブジェクトを中央に配置し、ページのサイズに拡大する 3 つの変換を適用します。 パスは、クリッピング パスとして設定できます。

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

クリッピング パスを設定すると、ビットマップを表示することができます、および文字のアウトラインをクリップされます。 使用に注意してください、 [ `AspectFill` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRect.AspectFill/p/SkiaSharp.SKSize/)メソッドの`SKRect`縦横比を維持しながら、ページを塗りつぶすときの四角形を計算します。

**パスのテキスト効果**ページ 1 D パス効果を作成するパスを単一のアンパサンド文字を変換します。 大きなバージョンの同じ文字のアウトラインを描画する効果がこのパスの描画オブジェクトを使用しています。

[![](text-paths-images/textpatheffect-small.png "パスのテキスト効果 ページのスクリーン ショットをトリプル")](text-paths-images/textpatheffect-large.png "パスのテキスト効果 ページのトリプル スクリーン ショット")

含まれる作業の量、 [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs)クラスは、フィールドとコンス トラクターで発生します。 2 つ`SKPaint`2 つの異なる目的のために使用されるフィールドが定義されているオブジェクト: 最初の (という名前`textPathPaint`) でアンパサンドを変換するために使用、`TextSize`の 1 D パス効果のパスを 50 です。 2 番目 (`textPaint`)、アンパサンドとそのパスの効果の大きいバージョンを表示するために使用します。 そのため、`Style`に設定されているオブジェクトのこの 2 つ目のペイント`Stroke`、ですが、`StrokeWidth`プロパティが設定されていないため、1 D パス効果を使用したときにそのプロパティが必要はありません。

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
        SKRect textPathPaintBounds;
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

最初のコンス トラクターを使用して、`textPathPaint`でアンパサンドを計測するオブジェクト、 `TextSize` 50 です。 渡されている四角形の中心の座標の符号を反転、`GetTextPath`テキストをパスに変換します。 結果のパスが、(0, 0) の 1 D パス効果に最適であり、文字の中央にポイントします。

考えることも、`SKPathEffect`コンス トラクターの最後に作成されたオブジェクトに設定できる、`PathEffect`プロパティ`textPaint`フィールドとして保存するのではなくです。 非常を適切に機能の結果が音がひずむためこのをオンにできません、`MeasureText`で呼び出し、`PaintSurface`ハンドラー。

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
        SKRect textBounds;
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

ある`MeasureText`呼び出しが、ページ上の文字を中央に使用します。 問題を回避する、`PathEffect`が表示されるまでは、テキストが測定された後、ペイント オブジェクトにプロパティが設定されます。

## <a name="outlines-of-character-outlines"></a>文字の枠のアウトライン

通常、 [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/)のメソッド`SKPaint`ペイントのプロパティを適用することで別に 1 つのパスを変換します。 最も顕著な、境界線の幅とパス効果。 パス効果なしで使わ場合`GetFillPath`実質的に別のパスが説明されているパスを作成します。 示されているこれが、**パスをタップすると、アウトライン** ページで、 [**パス効果**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)資料です。

呼び出すこともできます`GetFillPath`から返されるパスで`GetTextPath`なる場合が最初にすることですが、そのを次に示します。

**文字アウトライン アウトライン**ページは、方法を示します。 関連するすべてのコードが、`PaintSurface`のハンドラー、 [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs)クラスです。

コンス トラクターは、作成した後、`SKPaint`という名前のオブジェクト`textPaint`で、`TextSize`プロパティ ページのサイズに基づきます。 これを使用してパスに変換されます、`GetTextPath`メソッドです。 座標の引数`GetTextPath`画面上のパスを効果的にセンターします。

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
        SKRect textBounds;
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

`PaintSurface`ハンドラーという名前の新しいパスを作成し`outlinePath`です。 これは、呼び出しでは、移行先パスになります`GetFillPath`です。 `StrokeWidth` 25 原因のプロパティ`outlinePath`テキスト文字の描画、ピクセル幅の 25 パスのアウトラインを記述します。 このパスが 5 のストロークの幅が赤で表示されます。

[![](text-paths-images/characteroutlineoutlines-small.png "文字のアウトライン アウトライン ページのスクリーン ショットをトリプル")](text-paths-images/characteroutlineoutlines-large.png "文字アウトライン アウトライン ページのトリプル スクリーン ショット")

詳しく調べるし、わかります overlaps パス アウトラインが鋭い角をによりします。 これらは、このプロセスの標準の成果物です。

## <a name="text-along-a-path"></a>パスに沿ったテキスト

テキストは通常、水平方向のベースラインに表示されます。 垂直方向にまたは斜めを実行するテキストを回転することができますが、ベースラインは、直線のままです。

ただし、テキストを曲線に沿った実行をする場合があります。 目的は、これは、 [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/)メソッドの`SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

最初の引数で指定したテキストが 2 番目の引数として指定されたパスに沿ったを実行しようとしています。 テキストを使用してパスの先頭からのオフセットを開始することができます、`hOffset`引数。 パスが、テキストのベースラインを形成する通常: テキスト アセンダーは、パスの 1 つの側で、テキスト ディセンダー互いにします。 使用してパスからテキストのベースラインをオフセットできますが、`vOffset`引数。

このメソッドの設定に関するガイダンスを提供する機能はありません、`TextSize`プロパティ`SKPaint`テキストを完璧にサイズ設定すると、パスの先頭から末尾を実行します。 場合によって、自分でそのテキストのサイズを理解できます。 それ以外の時間は、今後の記事で説明するパスの測定機能を使用する必要があります。

**循環テキスト**プログラムは、円記号のテキストをラップします。 これは、円の円周を判断するので、簡単に正確に合わせてテキストのサイズに簡単です。 `PaintSurface`のハンドラー、 [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs)クラスは、ページのサイズに基づく円の半径を計算します。 その円になります`circularPath`:

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

`TextSize`プロパティ`textPaint`テキストの幅が、円の円周と一致するようには、調整。

[![](text-paths-images/circulartext-small.png "循環テキスト ページのスクリーン ショットをトリプル")](text-paths-images/circulartext-large.png "循環テキスト ページのトリプル スクリーン ショット")

ある程度循環させることに、テキスト自体が選択されました:"circle"という単語が両方の文の件名と前置詞句のオブジェクト。 

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
