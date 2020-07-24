---
title: スケール変換
description: この記事では、オブジェクトをさまざまなサイズにスケーリングするための SkiaSharp scale 変換について説明し、サンプルコードを使用してその方法を示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5cb43bfe572b98a6530dfeb8d923ac71b5b633a7
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86932043"
---
# <a name="the-scale-transform"></a>スケール変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_オブジェクトをさまざまなサイズにスケーリングするための SkiaSharp scale 変換について説明します。_

変換[**変換に関する記事で**](translate.md)説明したように、変換変換では、グラフィカルオブジェクトをある場所から別の場所に移動できます。 一方、スケール変換は、グラフィカルオブジェクトのサイズを変更します。

![高さの長い単語のサイズ調整](scale-images/scaleexample.png)

また、多くの場合、スケール変換によってグラフィック座標が大きくなるため、移動します。

前に、との変換要素の効果を示す2つの変換式について説明しました `dx` `dy` 。

x ' = x + dx

y ' = y + dy

とのスケール `sx` ファクター `sy` は加法ではなく乗算です。

x ' = sx '閉じる

y ' = sy ·前年

変換係数の既定値は0です。スケールファクターの既定値は1です。

クラスは、 `SKCanvas` 4 つの `Scale` メソッドを定義します。 最初の [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single)) 方法は、同じ水平方向および垂直方向のスケールファクターが必要な場合です。

```csharp
public void Scale (Single s)
```

これは*isotropic* &mdash; 、両方向で同一の等幅スケーリングスケーリングと呼ばれます。 等幅スケーリングでは、オブジェクトの縦横比が維持されます。

2番目の方法では、 [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) 水平方向および垂直方向のスケーリングに異なる値を指定できます。

```csharp
public void Scale (Single sx, Single sy)
```

これにより、*異方性*のスケーリングが発生します。
3番目の [`Scale`](xref:SkiaSharp.SKCanvas.Scale(SkiaSharp.SKPoint)) 方法では、2つのスケールファクターを1つの値に結合し `SKPoint` ます。

```csharp
public void Scale (SKPoint size)
```

4番目の方法は、 `Scale` すぐに説明します。

[**基本スケール**] ページでは、メソッドを示し `Scale` ます。 [**Basicscalepage .xaml**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml)ファイルには、 `Slider` 0 ~ 10 の水平方向および垂直方向のスケールファクターを選択できる2つの要素が含まれています。 [**BasicScalePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs)分離コードファイルでは、これらの値を使用して、 `Scale` 破線で線が付けられた角丸四角形を表示し、キャンバスの左上隅にあるテキストに合わせてサイズを調整します。

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

スケールファクターは、のメソッドから返される値にどのように影響するかということを不思議に思うかもしれません。 `MeasureText` `SKPaint` 答えは「いいえ」です。 `Scale`はのメソッドです `SKCanvas` 。 オブジェクトを使用すると、 `SKPaint` そのオブジェクトを使用してキャンバスに何かが表示されなくなるまで、どのような操作にも影響しません。

ご覧のように、呼び出し後に描画されたものはすべて比例して `Scale` 増加します。

[![[基本スケール] ページのトリプルスクリーンショット](scale-images/basicscale-small.png)](scale-images/basicscale-large.png#lightbox "[基本スケール] ページのトリプルスクリーンショット")

テキスト、破線の幅、その行のダッシュの長さ、角の丸み、および四角形の左端と上端の間の10ピクセルの余白は、すべて同じスケールファクターになりますが、この場合はそのままです。

> [!IMPORTANT]
> ユニバーサル Windows プラットフォームは、anisotropicly に拡大縮小されたテキストを正しくレンダリングしません。

異方性の拡大/縮小では、水平軸と垂直軸に沿って整列された線の太さが異なることになります。 (これは、このページの最初のイメージからも明らかです)。ストロークの幅が拡大率の影響を受けないようにするには、この値を0に設定します。設定に関係なく、常に1ピクセル幅になり `Scale` ます。

拡大縮小は、キャンバスの左上隅を基準としています。 これは必要なものですが、そうでない場合もあります。 テキストと四角形をキャンバス上の他の場所に配置し、その中心を基準として拡大縮小するとします。 その場合は、4番目のバージョンのメソッドを使用できます [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) 。これには、スケーリングの中心を指定する2つの追加のパラメーターが含まれています。

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px`パラメーターと `py` パラメーターは、*スケールセンター*と呼ばれることもありますが、SkiaSharp のドキュメントでは*ピボットポイント*と呼ばれる点を定義します。 これは、スケーリングの影響を受けないキャンバスの左上隅を基準としたポイントです。 すべてのスケールは、その中心を基準として行われます。

[**中央揃え**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs)のページには、そのしくみが示されています。 この `PaintSurface` ハンドラーは**基本的なスケール**プログラムに似ています `margin` 。ただし、値はテキストを水平方向に中央揃えにすることを意味します。これは、プログラムが縦モードで最適に動作することを意味します。

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

角丸四角形の左上隅は、キャンバスの左側からピクセルに位置し、 `margin` ピクセルは上端からピクセルに配置され `margin` ます。 メソッドの最後の2つの引数は、 `Scale` これらの値に、テキストの幅と高さに加えて、丸みのある四角形の幅と高さにも設定されます。 これは、すべてのスケーリングが、その四角形の中心を基準としていることを意味します。

[![中央のスケールページのトリプルスクリーンショット](scale-images/centeredscale-small.png)](scale-images/centeredscale-large.png#lightbox "中央のスケールページのトリプルスクリーンショット")

`Slider`このプログラムの要素の範囲は 10 ~ &ndash; 10 です。 ご覧のように、垂直方向のスケーリングの負の値 (たとえば、中央の Android 画面) では、オブジェクトがスケールの中心を通過する水平軸を中心に反転します。 水平方向のスケーリングの負の値 (右側の UWP 画面のなど) では、オブジェクトが垂直軸を中心に反転して、スケーリングの中心を通過します。

[`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single))ピボットポイントを使用したメソッドのバージョンは、一連の3つの呼び出しのショートカットです `Translate` `Scale` 。 この動作を確認するには `Scale` 、**中央のスケール**ページのメソッドを次のように置き換えます。

```csharp
canvas.Translate(-px, -py);
```

これらは、ピボットポイント座標の負の値です。

そして、再びプログラムを実行します。 四角形とテキストが移動され、中央がキャンバスの左上隅に表示されます。 このように表示されることはほとんどありません。 ここでは、プログラムがまったく拡張されないため、スライダーは機能しません。

次に、 `Scale` その呼び出しの*前に*基本的な呼び出し (スケールセンターなし) を追加し `Translate` ます。

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

他のグラフィックスプログラミングシステムでこの演習を使い慣れている場合は、間違っていると思われるかもしれませんが、そうではありません。 Skia は、連続した変換呼び出しを、使い慣れているものとは少し異なる方法で処理します。

連続した `Scale` およびを `Translate` 呼び出すと、角丸四角形の中心は左上隅になりますが、キャンバスの左上隅 (丸みのある四角形の中央) に合わせてスケールできできるようになりました。

次に、その呼び出しの前に、 `Scale` `Translate` センタリング値を持つ別の呼び出しを追加します。

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

これにより、スケールされた結果が元の位置に戻ります。 これら3つの呼び出しは、次の場合と同じです。

```csharp
canvas.Scale(sx, sy, px, py);
```

個々の変換は、合計変換式が次のように複雑になります。

 x ' = sx '(x – px) + px

 y ' = sy ·(y – .py) + .py

およびの既定値は1であることに注意してください `sx` `sy` 。 ピボットポイント (px, .py) は、これらの数式によって変換されないことを簡単に納得できます。 キャンバスと同じ場所に保持されます。

とを組み合わせる `Translate` と `Scale` 、順序が重要になります。 がの後に来た場合、 `Translate` `Scale` 変換係数はスケーリングファクターによって効果的にスケーリングされます。 が `Translate` の前にある場合 `Scale` 、翻訳要素はスケーリングされません。 変換マトリックスのサブジェクトが導入されると、このプロセスはやや明確になります (数学的ではありません)。

クラスは、 `SKPath` [`Bounds`](xref:SkiaSharp.SKPath.Bounds) パス内の座標の範囲を定義するを返す読み取り専用プロパティを定義し `SKRect` ます。 たとえば、 `Bounds` 前に作成した hendecagram パスからプロパティを取得した場合、四角形のプロパティとプロパティは約100で、プロパティとプロパティは約 `Left` 100 で、プロパティ `Top` `Right` `Bottom` `Width` とプロパティは `Height` 約200です。 (実際の値のほとんどは、星の点は半径が100の円で定義されていますが、上の点だけが水平軸と垂直軸で平行しています)。

この情報が利用可能であることは、パスをキャンバスのサイズにスケーリングするのに適したスケールと変換係数を導き出すことができることを意味します。 [[**異方性の拡大/縮小**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs)] ページでは、このことを示しています。 *異*形スケールは、水平方向と垂直方向では等しくないことを意味します。つまり、星は元の縦横比を保持しません。 ハンドラー内の関連するコードを `PaintSurface` 次に示します。

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

`pathBounds`このコードの先頭付近に四角形が取得され、その後、呼び出しでキャンバスの幅と高さが使用され `Scale` ます。 この呼び出しでは、呼び出しによってレンダリングされるときに、パスの座標が拡大縮小され `DrawPath` ますが、星はキャンバスの右上隅に中央揃えで表示されます。 左に移動する必要があります。 これは呼び出しのジョブです `Translate` 。 の2つのプロパティ `pathBounds` は、約100であるため、翻訳要素は約100です。 この呼び出しは呼び出しの後に行われるため、 `Translate` `Scale` これらの値はスケールファクターによって実質的にスケーリングされるので、星の中央をキャンバスの中央に移動します。

[![[異方性の拡大/縮小] ページのトリプルスクリーンショット](scale-images/anisotropicscaling-small.png)](scale-images/anisotropicscaling-large.png#lightbox "[異方性の拡大/縮小] ページのトリプルスクリーンショット")

との呼び出しについて考えることができるもう1つの方法 `Scale` `Translate` は、逆の順序で効果を判断することです。 `Translate` 呼び出しはパスを移動するため、完全に見えるようになりますが、キャンバスの左上隅に配置されます。 次に、メソッドによって、 `Scale` 左上隅を基準としてその星が大きくなります。

実際には、星がキャンバスよりも少し大きいことが示されています。 ストロークの幅は問題です。 `Bounds`のプロパティは、 `SKPath` パスでエンコードされた座標の大きさを示します。これは、プログラムがスケーリングに使用するものです。 パスが特定のストローク幅でレンダリングされる場合、描画されるパスはキャンバスよりも大きくなります。

この問題を解決するには、修正する必要があります。 このプログラムの簡単な方法の1つは、呼び出しの直前に次のステートメントを追加することです `Scale` 。

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

これ `pathBounds` により、4辺すべてで四角形が1.5 単位で増加します。 これは、ストローク結合が丸められた場合にのみ、妥当なソリューションです。 マイタ結合が長くなり、計算が困難になる場合があります。

また、**異方性テキスト**ページで示すように、テキストと同様の手法を使用することもできます。 クラスからのハンドラーの関連部分を次に示し `PaintSurface` [`AnisotropicTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) ます。

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

これは同様のロジックであり、テキストはから返されたテキスト境界四角形に基づいてページのサイズに拡張 `MeasureText` されます (これは実際のテキストよりも少し大きくなります)。

[![[異方性テスト] ページのトリプルスクリーンショット](scale-images/anisotropictext-small.png)](scale-images/anisotropictext-large.png#lightbox "[異方性テスト] ページのトリプルスクリーンショット")

グラフィカルオブジェクトの縦横比を維持する必要がある場合は、等幅スケーリングを使用します。 [**等幅スケーリング**] ページでは、このことを示しています。 概念的には、等幅スケーリングを使用して、ページの中央にグラフィカルオブジェクトを表示する手順は次のとおりです。

- グラフィックオブジェクトの中心を左上隅に変換します。
- 水平および垂直のページ寸法の最小値を、グラフィカルオブジェクトディメンションで割った値に基づいてオブジェクトを拡大します。
- スケーリングされたオブジェクトの中心をページの中央に変換します。

は、 [`IsotropicScalingPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) 星を表示する前に、次の手順を逆の順序で実行します。

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

また、このコードでは、次のように星10も表示されます。スケールファクターが10% 減少し、色が赤から青に徐々に変化します。

[![[等幅スケール] ページのトリプルスクリーンショット](scale-images/isotropicscaling-small.png)](scale-images/isotropicscaling-large.png#lightbox "[等幅スケール] ページのトリプルスクリーンショット")

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
