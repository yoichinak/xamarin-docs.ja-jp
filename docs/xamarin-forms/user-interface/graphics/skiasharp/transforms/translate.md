---
title: "平行移動の変換"
description: "SkiaSharp グラフィックスをシフトする平行移動の変換を使用する方法をについてください。"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 491c82406dafceb876ddbb4a0a7204447b95f57d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="the-translate-transform"></a>平行移動の変換

_SkiaSharp グラフィックスをシフトする平行移動の変換を使用する方法をについてください。_

SkiaSharp の変換などの最も単純な型が、*変換*または*翻訳*を変換します。 この変換は、水平方向および垂直方向にグラフィカル オブジェクトを移動します。 描画の関数で使用している座標を変更するだけで、同じ効果が得られます通常ために、意味では、翻訳、最も不要な変換です。 パスを表示する際にただし、すべての座標はカプセル化、パスのため、パス全体をシフトする変換の変換を適用する方が簡単です。

翻訳がアニメーションを実行し、単純なテキスト効果に便利ですが。

![](translate-images/translateexample.png "Engraving、および翻訳エンボスのテキスト シャドウ")

[ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/)メソッド`SKCanvas`によって描画された後でグラフィックス オブジェクトを水平方向および垂直方向にシフトする 2 つのパラメーターします。

```csharp
public void Translate (Single dx, Single dy)
```

これらの引数を負の値にすることがあります。 1 秒あたり[ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/SkiaSharp.SKPoint/)メソッドが 1 つの 2 つの変換の値を結合`SKPoint`値。

```csharp
public void Translate (SKPoint point)
```

**変換蓄積**のページ、 [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)サンプル プログラムでは、その何度も呼び出すを示しています、`Translate`メソッドは累積されます。 [ `AccumulatedTranslate` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs)クラスが同じ四角形の 20 のバージョンを表示、1 つずつオフセット前の四角形から十分な量対角線に沿ったをストレッチするようにします。 ここでは、`PaintSurface`イベントのハンドラー。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Color = SKColors.Black;
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = 3;

        int rectangleCount = 20;
        SKRect rect = new SKRect(0, 0, 250, 250);
        float xTranslate = (info.Width - rect.Width) / (rectangleCount - 1);
        float yTranslate = (info.Height - rect.Height) / (rectangleCount - 1);

        for (int i = 0; i < rectangleCount; i++)
        {
            canvas.DrawRect(rect, strokePaint);
            canvas.Translate(xTranslate, yTranslate);
        }
    }
}
```

一連の四角形は、ページの下方トリクルします。

[![](translate-images/accumulatedtranslate-small.png "トリプル ページのスクリーン ショット、翻訳蓄積")](translate-images/accumulatedtranslate-large.png "トリプル ページのスクリーン ショット、蓄積された変換")

場合、蓄積された翻訳要素は、`dx`と`dy`、描画関数で指定した点と (`x`、 `y`)、ポイントでグラフィック オブジェクトを表示し、(`x'`、 `y'`) ここで。

x' = x + dx

y' = y + dy

これらと呼ばれますが、*数式の変換*変換用。 既定値は、`dx`と`dy`、新しい`SKCanvas`は 0 です。

シャドウ効果と同様の手法として平行移動の変換を使用するが一般的、**テキスト効果の翻訳**ページを示しています。 ここで、関連の一部である、`PaintSurface`ハンドラー、 [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs)クラス。

```csharp
float textSize = 150;

using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = textSize;
    textPaint.FakeBoldText = true;

    float x = 10;
    float y = textSize;

    // Shadow
    canvas.Translate(10, 10);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("SHADOW", x, y, textPaint);
    canvas.Translate(-10, -10);
    textPaint.Color = SKColors.Pink;
    canvas.DrawText("SHADOW", x, y, textPaint);

    y += 2 * textSize;

    // Engrave
    canvas.Translate(-5, -5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("ENGRAVE", x, y, textPaint);
    canvas.ResetMatrix();
    textPaint.Color = SKColors.White;
    canvas.DrawText("ENGRAVE", x, y, textPaint);

    y += 2 * textSize;

    // Emboss
    canvas.Save();
    canvas.Translate(5, 5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("EMBOSS", x, y, textPaint);
    canvas.Restore();
    textPaint.Color = SKColors.White;
    canvas.DrawText("EMBOSS", x, y, textPaint);
}
```

次の 3 つの例については、の各`Translate`で指定した位置からオフセットするテキストを表示するために呼び出されますが、`x`と`y`変数。 テキストを翻訳の効果がない別の色でもう一度表示されます。

[![](translate-images/translatetexteffects-small.png "テキスト効果の変換 ページのスクリーン ショットをトリプル")](translate-images/translatetexteffects-large.png "テキスト効果の変換 ページのトリプル スクリーン ショット")

役に立たなくさまざまな方法を示していますの 3 つの例では、それぞれ、`Translate`呼び出し。

最初の例を呼び出すだけ`Translate`負の値がもう一度です。 `Translate`累積的な呼び出しでは、この一連の呼び出しがゼロの既定値を単に合計の翻訳を復元します。

2 番目の例では、 [ `ResetMatrix`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix()/)です。 これにより、すべての変換は既定の状態に戻ります。

3 番目の例の状態を保存するの`SKCanvas`オブジェクトへの呼び出しを[ `Save` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Save()/)への呼び出しでの状態を復元および[ `Restore`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Restore/)です。 これは、描画操作の一連の変換を操作するための最も用途の広い方法です。 これら`Save`と`Restore`スタックのように関数を呼び出します: 呼び出すことができます`Save`複数の時刻、およびを呼び出します`Restore`で前の状態に戻るにはシーケンスを反転します。 `Save`メソッドは、整数を返し、その整数を渡すことができます[ `RestoreToCount` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RestoreToCount/)を効果的に呼び出す`Restore`複数回です。 [ `SaveCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.SaveCount/)プロパティをスタックに現在保存されている状態の数を返します。

ただし、変換の呼び出しは 1 つから引き継がについて心配することも、`PaintSurface`へハンドラー。 新しい呼び出しごとに`PaintSurface`新規の配信`SKCanvas`の既定の変換を持つオブジェクト。

一般的な用途の別、`Translate`ビジュアル オブジェクトをレンダリングがもともと作成された描画都合の良い座標を使用して変換がします。 たとえば、中央の位置 (0, 0) を持つ、アナログ時計の座標を指定することができます。 できますし、変換を使用できます表示目的の位置。 示されているこれは、**[Hendecagram 配列]** ページ。 [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs)クラスが作成した後、 `SKPath` 11 - 星のオブジェクト。 `HendecagramPath`オブジェクトが定義されている、公開、static、および読み取り専用他のデモ プログラムからアクセスできるようにします。 静的コンス トラクターで作成されます。

```csharp
public class HendecagramArrayPage : ContentPage
{
    ...
    public static readonly SKPath HendecagramPath;

    static HendecagramArrayPage()
    {
        // Create 11-pointed star
        HendecagramPath = new SKPath();
        for (int i = 0; i < 11; i++)
        {
            double angle = 5 * i * 2 * Math.PI / 11;
            SKPoint pt = new SKPoint(100 * (float)Math.Sin(angle),
                                    -100 * (float)Math.Cos(angle));
            if (i == 0)
            {
                HendecagramPath.MoveTo(pt);
            }
            else
            {
                HendecagramPath.LineTo(pt);
            }
        }
        HendecagramPath.Close();
    }
}
```

星の中央に点 (0, 0) がある場合、星のすべてのポイントはそのポイントを囲む、円周上です。 各ポイントは、5/11ths の 360 度ずつ増加する角度の正弦と余弦の値の組み合わせです。 (2 によって角度を増やすことで、11 星を作成することも、11/3/11ths、または 4 の円の 11/です)。円の半径は 100 に設定されます。

このパスは、変換なしでレンダリングされますが、左上隅にあるの中心に配置されます、`SKCanvas`とに四半期のみが表示されます。 `PaintSurface`のハンドラー`HendecagramPage`代わりに使用して`Translate`色付きそれぞれランダムの星のコピーを複数持つキャンバスを並べて表示します。

```csharp
public class HendecagramArrayPage : ContentPage
{
    Random random = new Random();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            for (int x = 100; x < info.Width + 100; x += 200)
                for (int y = 100; y < info.Height + 100; y += 200)
                {
                    // Set random color
                    byte[] bytes = new byte[3];
                    random.NextBytes(bytes);
                    paint.Color = new SKColor(bytes[0], bytes[1], bytes[2]);

                    // Display the hendecagram
                    canvas.Save();
                    canvas.Translate(x, y);
                    canvas.DrawPath(HendecagramPath, paint);
                    canvas.Restore();
                }
        }
    }
}

```

結果を次に示します。

[![](translate-images/hendecagramarray-small.png "Hendecagram アレイのページのスクリーン ショットをトリプル")](translate-images/hendecagramarray-large.png "Hendecagram アレイのページのトリプル スクリーン ショット")

アニメーションには、多くの場合、変換が含まれます。 **Hendecagram アニメーション**ページ内を移動 11 星円です。 [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs)クラスがいくつかのフィールドで始まり、かつの上書きが、`OnAppearing`と`OnDisappearing`開始および Xamarin.Forms タイマーを停止する方法。

```csharp
public class HendecagramAnimationPage : ContentPage
{
    const double cycleTime = 5000;      // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float angle;

    public HendecagramAnimationPage()
    {
        Title = "Hedecagram Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            angle = (float)(360 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`angle`フィールドはアニメーション 0 360 度を 5 秒ごとです。 `PaintSurface`ハンドラーを使用して、`angle`プロパティを 2 つの方法で: で色の色合いを指定する、`SKColor.FromHsl`メソッド、およびへの引数として、`Math.Sin`と`Math.Cos`星の場所を制御するためにメソッド。

```csharp
public class HendecagramAnimationPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.Translate(info.Width / 2, info.Height / 2);
        float radius = (float)Math.Min(info.Width, info.Height) / 2 - 100;

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColor.FromHsl(angle, 100, 50);

            float x = radius * (float)Math.Sin(Math.PI * angle / 180);
            float y = -radius * (float)Math.Cos(Math.PI * angle / 180);
            canvas.Translate(x, y);
            canvas.DrawPath(HendecagramPage.HendecagramPath, paint);
        }
    }
}
```

`PaintSurface`ハンドラーの呼び出し、`Translate`キャンバスの中央に変換するには、最初にメソッドを 2 回、しを中心とした円の円周に変換する (0, 0) です。 ページの境界内にスターしたまま、可能な限り大きくする円の半径が設定されます。

[![](translate-images/hendecagramanimation-small.png "Hendecagram アニメーション ページのスクリーン ショットをトリプル")](translate-images/hendecagramanimation-large.png "Hendecagram アニメーション ページのトリプル スクリーン ショット")

ページの中央を中心として、星が同じ方向が維持されることを確認します。 すべての回転しません。 回転変換用のジョブです。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
