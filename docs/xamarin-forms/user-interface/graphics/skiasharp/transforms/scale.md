---
title: スケール変換
description: Thhis 記事では、オブジェクトをさまざまな規模を拡張するための SkiaSharp スケール変換について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
ms.openlocfilehash: 7dc7a2faabef86aa63b52739edda696efcb54aba
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724245"
---
# <a name="the-scale-transform"></a>スケール変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_オブジェクトをさまざまなサイズにスケーリングするための SkiaSharp scale 変換について説明します。_

変換[**変換に関する記事で**](translate.md)説明したように、変換変換では、グラフィカルオブジェクトをある場所から別の場所に移動できます。 これに対し、スケール変換は、グラフィカル オブジェクトのサイズを変更します。

![](scale-images/scaleexample.png "A tall word scaled in size")

スケール変換には、大規模な実行時に移動するグラフィックス座標も多くの場合とします。

前に、`dx` と `dy`の変換要因の効果を示す2つの変換式を示しました。

x' = x + dx

y' = y + dy

`sx` と `sy` のスケールファクターは加法ではなく乗算です。

x' sx 押しを =x

y' sy 押しを =y

翻訳の要素の既定値は 0 です。スケール ファクターの既定値は、1 です。

`SKCanvas` クラスは、4つの `Scale` メソッドを定義します。 最初の[`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single))方法は、同じ水平方向および垂直方向のスケールファクターが必要な場合に適しています。

```csharp
public void Scale (Single s)
```

これは、両方向で同じように、*等*幅スケーリング &mdash; スケーリングと呼ばれます。 アイソトロ ピック スケーリングには、オブジェクトの縦横比が保持されます。

2番目の[`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single))メソッドを使用すると、水平方向および垂直方向のスケーリングに異なる値を指定できます。

```csharp
public void Scale (Single sx, Single sy)
```

これにより、*異方性*のスケーリングが発生します。
3番目の[`Scale`](xref:SkiaSharp.SKCanvas.Scale(SkiaSharp.SKPoint))方法では、2つのスケールファクターを1つの `SKPoint` 値に結合します。

```csharp
public void Scale (SKPoint size)
```

4つ目の `Scale` 方法については、すぐに説明します。

**[基本スケール]** ページでは、`Scale` メソッドについて説明します。 [**Basicscalepage .xaml**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml)ファイルには、0 ~ 10 の水平方向および垂直方向のスケールファクターを選択できる2つの `Slider` 要素が含まれています。 [**BasicScalePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs)分離コードファイルでは、これらの値を使用して `Scale` を呼び出してから、点線で線が付けられ、キャンバスの左上隅にあるテキストに合わせてサイズが調整されます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] {  7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        canvas.Scale((float)xScaleSlider.Value,
                     (float)yScaleSlider.Value);

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);

        float margin = 10;
        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

スケールファクターは、`SKPaint`の `MeasureText` メソッドから返される値にどのように影響するかということを不思議に思うかもしれません。 答え: いいえ、違います。 `Scale` は `SKCanvas`のメソッドです。 オブジェクトを使用してキャンバスに何かを表示するまで、`SKPaint` オブジェクトで行った操作には影響しません。

ご覧のように、`Scale` 呼び出しの後に描画されたすべてのものは、次のように比例して増加します。

[![](scale-images/basicscale-small.png "Triple screenshot of the Basic Scale page")](scale-images/basicscale-large.png#lightbox "Triple screenshot of the Basic Scale page")

テキスト、破線、隅と、キャンバスの左と上の端と角丸四角形の間の 10 ピクセルの余白の丸め、その行にダッシュの長さの幅はすべて同じスケール ファクター。

> [!IMPORTANT]
> ユニバーサル Windows プラットフォームは anisotropicly 拡大縮小されたテキストを正しく表示されません。

水平および垂直の軸に揃えになる別の線のストロークの幅をエラーの原因をスケーリング異方性。 (これは、このページの最初のイメージからも明らかです)。ストロークの幅がスケールファクターの影響を受けないようにするには、この値を0に設定します。 `Scale` 設定に関係なく、常に1ピクセル幅になります。

スケーリングは、キャンバスの左上隅に対して相対的です。 これは厳密にあります目的の動作ができない可能性があります。 キャンバスで、テキストと四角形の別の場所に移動して、その中心を基準としたサイズを変更するとします。 その場合は、4番目のバージョンの[`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single))メソッドを使用できます。これには、スケーリングの中心を指定する2つの追加パラメーターがあります。

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px` パラメーターと `py` パラメーターは、*スケールセンター*と呼ばれることもありますが、SkiaSharp のドキュメントでは、*ピボットポイント*と呼ばれる点を定義します。 これは、スケーリングして影響はありませんが、キャンバスの左上隅に対する相対ポイントです。 すべてのスケーリングは、その中心を基準に発生します。

[**中央揃え**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs)のページには、そのしくみが示されています。 `PaintSurface` ハンドラーは、**基本的なスケール**プログラムに似ています。ただし、`margin` 値は、テキストを水平方向に中央揃えにすることを意味します。これは、プログラムが縦モードで最適に動作することを意味します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);
        float margin = (info.Width - textBounds.Width) / 2;

        float sx = (float)xScaleSlider.Value;
        float sy = (float)yScaleSlider.Value;
        float px = margin + textBounds.Width / 2;
        float py = margin + textBounds.Height / 2;

        canvas.Scale(sx, sy, px, py);

        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

角丸四角形の左上隅は、キャンバスの左側から `margin` ピクセルに配置され、上端からピクセル `margin` ます。 `Scale` メソッドの最後の2つの引数は、これらの値に、テキストの幅と高さに加えて、丸みのある四角形の幅と高さにも設定されます。 つまり、すべてのスケーリングが四角形の中央を基準。

[![](scale-images/centeredscale-small.png "Triple screenshot of the Centered Scale page")](scale-images/centeredscale-large.png#lightbox "Triple screenshot of the Centered Scale page")

このプログラムの `Slider` 要素には、&ndash;10 ~ 10 の範囲があります。 ご覧のように、負の値の垂直スケール (画面の中央で、android など) が発生するスケーリングの中心を通過する水平方向の軸を中心に反転するオブジェクト。 水平スケーリング (右側の UWP 画面など) の負の値では、スケーリングの中心を通過する垂直軸を中心に反転するオブジェクトが発生します。

ピボットポイントを使用した[`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single))メソッドのバージョンは、3つの `Translate` と `Scale` の呼び出しの一連のショートカットです。 この動作を確認するには、**中央のスケール**ページの `Scale` 方法を次のように置き換えます。

```csharp
canvas.Translate(-px, -py);
```

これらは、ピボット ポイントの座標の符号を反転します。

そして、再びプログラムを実行します。 中央がキャンバスの左上隅になるよう、四角形やテキストがシフトしたことを確認します。 ほんの少し確認できます。 スライダーは、プログラムがすべての拡張できないようになりましたので、もちろん機能しません。

次に、`Translate` を呼び出す*前に*、(スケーリングセンターのない) 基本的な `Scale` 呼び出しを追加します。

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

慣れている場合にその他のグラフィックスのシステムをプログラミングで、問題のあるを考える場合がありますが、この演習はありません。 Skia 連続する変換の呼び出し、少し異なる方法で処理からどのような場合があります理解してでしょう。

連続した `Scale` および `Translate` 呼び出しでは、角丸四角形の中心は左上隅にありますが、キャンバスの左上隅 (丸みのある四角形の中心) を基準にして拡大縮小できるようになりました。

次に、`Scale` を呼び出す前に、センタリング値を持つ別の `Translate` 呼び出しを追加します。

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

これは、スケールの結果に移動元の位置。 これら 3 つの呼び出しと同等のとおりです。

```csharp
canvas.Scale(sx, sy, px, py);
```

合計変換式が実行されるよう、個々 の変換が複雑化します。

 x' = sx ・ (x ? px) + px

 y' = sy ・ (y ? py) + py

`sx` と `sy` の既定値は1であることに注意してください。 これらの数式、ピボット ポイント (px、py) が変換されませんをたどりやすいになります。 これは、キャンバスの基準と同じ場所に残ります。

`Translate` と `Scale` の呼び出しを組み合わせると、順序が重要になります。 `Translate` が `Scale`の後にある場合、変換係数はスケールファクターによって効果的にスケーリングされます。 `Translate` が `Scale`の前にある場合、翻訳要素はスケーリングされません。 このプロセスが少し明確になります (ただしより数学的な) の変換行列のサブジェクトを導入したとき。

`SKPath` クラスは、パス内の座標の範囲を定義する `SKRect` を返す読み取り専用の[`Bounds`](xref:SkiaSharp.SKPath.Bounds)プロパティを定義します。 たとえば、前に作成した hendecagram パスから `Bounds` プロパティを取得した場合、四角形の `Left` プロパティと `Top` プロパティは約–100、`Right` プロパティと `Bottom` プロパティは約100で、`Width` プロパティと `Height` プロパティは約200です。 (ほとんどの実際の値はほとんどない星のポイントが 100 の半径の円で定義されているが、上のポイントのみが水平または垂直の軸を持つ並列ため)。

この情報の可用性は、スケールを派生し、キャンバスのサイズへのパスを拡張するための適切な要素を変換することができます、ことを意味します。 [[**異方性の拡大/縮小**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs)] ページでは、このことを示しています。 *異*形スケールは、水平方向と垂直方向では等しくないことを意味します。つまり、星は元の縦横比を保持しません。 `PaintSurface` ハンドラーの関連するコードを次に示します。

```csharp
SKPath path = HendecagramPage.HendecagramPath;
SKRect pathBounds = path.Bounds;

using (SKPaint fillPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Pink
})
using (SKPaint strokePaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 3,
    StrokeJoin = SKStrokeJoin.Round
})
{
    canvas.Scale(info.Width / pathBounds.Width,
                 info.Height / pathBounds.Height);
    canvas.Translate(-pathBounds.Left, -pathBounds.Top);

    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

`pathBounds` 四角形は、このコードの先頭付近に取得され、後で `Scale` 呼び出しのキャンバスの幅と高さで使用されます。 この呼び出しでは、`DrawPath` の呼び出しによってレンダリングされるときに、パスの座標が拡大縮小されますが、星はキャンバスの右上隅に中央揃えで表示されます。 下と左にシフトが必要です。 これは `Translate` 呼び出しのジョブです。 `pathBounds` のこれら2つのプロパティは約100であるため、翻訳要素は約100です。 `Translate` の呼び出しは `Scale` 呼び出しの後に行われるため、これらの値はスケールファクターによって実質的にスケーリングされるので、星の中央をキャンバスの中央に移動します。

[![](scale-images/anisotropicscaling-small.png "Triple screenshot of the Anisotropic Scaling page")](scale-images/anisotropicscaling-large.png#lightbox "Triple screenshot of the Anisotropic Scaling page")

`Scale` と `Translate` の呼び出しについて検討するもう1つの方法は、逆の順序で効果を判断することです。 `Translate` 呼び出しは、パスを完全に表示されますが、キャンバスの左上隅に配置されるように移動します。 次に、`Scale` メソッドによって、左上隅を基準としてその星が大きくなります。

実際には、星がキャンバスより少し大きいでことが表示されます。 問題は、ストロークの幅です。 `SKPath` の [`Bounds`] プロパティは、パスでエンコードされた座標の大きさを示します。これは、プログラムがスケーリングに使用するものです。 特定のストロークの幅と、パスが表示されると、表示されたパスは、キャンバスを超えています。

この問題を解決するには、補正する必要があります。 このプログラムの簡単な方法の1つとして、`Scale` の呼び出しの直前に次のステートメントを追加します。

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

これにより、4辺すべてに1.5 単位の `pathBounds` 四角形が増加します。 これは、ストロークの結合が丸められた場合にのみ、妥当なソリューションです。 マイター結合は長くなり、計算することは困難です。

また、**異方性テキスト**ページで示すように、テキストと同様の手法を使用することもできます。 [`AnisotropicTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs)クラスの `PaintSurface` ハンドラーの関連する部分を次に示します。

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 0.1f,
    StrokeJoin = SKStrokeJoin.Round
})
{
    SKRect textBounds = new SKRect();
    textPaint.MeasureText("HELLO", ref textBounds);

    // Inflate bounds by the stroke width
    textBounds.Inflate(textPaint.StrokeWidth / 2,
                       textPaint.StrokeWidth / 2);

    canvas.Scale(info.Width / textBounds.Width,
                 info.Height / textBounds.Height);
    canvas.Translate(-textBounds.Left, -textBounds.Top);

    canvas.DrawText("HELLO", 0, 0, textPaint);
}
```

これは同様のロジックであり、テキストは `MeasureText` から返されたテキスト境界の四角形に基づいてページのサイズに拡張されます (これは実際のテキストよりも少し大きくなります)。

[![](scale-images/anisotropictext-small.png "Triple screenshot of the Anisotropic Test page")](scale-images/anisotropictext-large.png#lightbox "Triple screenshot of the Anisotropic Test page")

グラフィカル オブジェクトの縦横比を保持する必要がある場合にアイソトロ ピック スケーリングを使用します。 **[等幅スケーリング]** ページでは、このことを示しています。 概念的には、グラフィカル オブジェクトをアイソトロ ピック スケーリングとページの中央に表示するための手順です。

- 左上隅にグラフィカル オブジェクトの中心に変換します。
- 水平および垂直方向のページの寸法のグラフィカル オブジェクトのサイズで割った値の最小値に基づくオブジェクトを拡張します。
- ページの中央にスケーリングされたオブジェクトの中心に変換します。

[`IsotropicScalingPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs)は、星を表示する前に、これらの手順を逆の順序で実行します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPath path = HendecagramArrayPage.HendecagramPath;
    SKRect pathBounds = path.Bounds;

    using (SKPaint fillPaint = new SKPaint())
    {
        fillPaint.Style = SKPaintStyle.Fill;

        float scale = Math.Min(info.Width / pathBounds.Width,
                               info.Height / pathBounds.Height);

        for (int i = 0; i <= 10; i++)
        {
            fillPaint.Color = new SKColor((byte)(255 * (10 - i) / 10),
                                          0,
                                          (byte)(255 * i / 10));
            canvas.Save();
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(scale);
            canvas.Translate(-pathBounds.MidX, -pathBounds.MidY);
            canvas.DrawPath(path, fillPaint);
            canvas.Restore();

            scale *= 0.9f;
        }
    }
}
```

によって 10% と赤から青に徐々 に色を変更するたびに、スケーリングの減少を考慮、コードが他にもスター 10 に表示されます。

[![](scale-images/isotropicscaling-small.png "Triple screenshot of the Isotropic Scaling page")](scale-images/isotropicscaling-large.png#lightbox "Triple screenshot of the Isotropic Scaling page")

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
