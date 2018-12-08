---
title: 平行移動変換
description: この記事では、平行移動変換を使用して、Xamarin.Forms アプリケーションで SkiaSharp グラフィックスをシフトする方法を調べ、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: a51a441aeacf265093b82ddb65237887b0a30719
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53053835"
---
# <a name="the-translate-transform"></a>平行移動変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_SkiaSharp のグラフィックスをシフトする平行移動変換を使用する方法について説明します_

SkiaSharp の変換の最も単純な型が、*変換*または*翻訳*変換します。 この変換では、水平および垂直方向のグラフィカル オブジェクトを移動します。 描画関数で使用している座標を変更するだけで通常と同じ効果を実現できますので、ある意味は、翻訳、最も不要な変換です。 パスを表示するときにただし、すべての座標はカプセル化パスにあるため、パス全体をシフトする平行移動変換を適用する方が簡単です。

翻訳もアニメーションを実行し、単純なテキスト効果に役立ちます。

![](translate-images/translateexample.png "テキストの影、彫刻、翻訳とエンボス")

[ `Translate` ](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single))メソッド`SKCanvas`は水平および垂直方向にシフトするときにオブジェクトが描画された後でグラフィックスを 2 つのパラメーターがあります。

```csharp
public void Translate (Single dx, Single dy)
```

これらの引数は負の値にすることはできます。 1 秒あたり[ `Translate` ](xref:SkiaSharp.SKCanvas.Translate(SkiaSharp.SKPoint))メソッドは、1 つの 2 つの変換の値を組み合わせて`SKPoint`値。

```csharp
public void Translate (SKPoint point)
```

**変換累積**のページ、 [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)サンプル プログラムがその何度も呼び出すを示して、`Translate`メソッドは累積されます。 [ `AccumulatedTranslatePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs)クラスには、同じの四角形の 20 個のバージョンが表示されます、1 つずつオフセット前の四角形からは十分な対角線に沿って伸縮するようにします。 ここでは、`PaintSurface`イベント ハンドラー。

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

連続する四角形は少量のページの下。

[![](translate-images/accumulatedtranslate-small.png "累積変換ページのスクリーン ショットをトリプル")](translate-images/accumulatedtranslate-large.png#lightbox "累積変換ページの 3 倍になるスクリーン ショット")

累積翻訳要素は、する場合`dx`と`dy`、描画関数で指定したポイントが (`x`、 `y`)、グラフィカル オブジェクトは、ポイントにレンダリングし、(`x'`、 `y'`) ここで。

x' = x + dx

y' = y + dy

これらと呼ばれますが、*数式の変換*変換します。 既定値`dx`と`dy`新しい`SKCanvas`は 0。

シャドウ効果と同様の手法として、平行移動変換を使用するが一般的、**翻訳テキスト効果**ページを示します。 関連部分を次に示します、`PaintSurface`ハンドラーで、 [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs)クラス。

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

3 つの例では、各`Translate`で指定した位置からオフセットするテキストを表示するために呼び出されますが、`x`と`y`変数。 テキストを翻訳影響を別の色でもう一度表示されます。

[![](translate-images/translatetexteffects-small.png "テキスト効果の変換 ページのスクリーン ショットをトリプル")](translate-images/translatetexteffects-large.png#lightbox "テキスト効果の変換 ページの 3 倍になるスクリーン ショット")

3 つの例の各役に立たなくなる別の方法を示しています、`Translate`呼び出し。

最初の例では単に`Translate`が負の値。 `Translate`累積的な呼び出しでは、この一連の呼び出しは、0 の既定値を合計の翻訳を復元するだけです。

2 番目の例では、 [ `ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix)します。 これにより、すべての変換の既定の状態に戻ります。

3 番目の例の状態を保存する、`SKCanvas`オブジェクトへの呼び出しで[ `Save` ](xref:SkiaSharp.SKCanvas.Save)への呼び出しで状態を復元[ `Restore`](xref:SkiaSharp.SKCanvas.Restore)します。 これは、最も汎用的な描画操作の一連の変換を操作する方法です。 これら`Save`と`Restore`スタックのように関数を呼び出します: 呼び出すことができます`Save`複数の時刻、および 呼び出し`Restore`で以前の状態を返すシーケンスを反転します。 `Save`メソッドは、整数を返し、その整数を渡すことができます[ `RestoreToCount` ](xref:SkiaSharp.SKCanvas.RestoreToCount*)を効果的に呼び出す`Restore`複数回です。 [ `SaveCount` ](xref:SkiaSharp.SKCanvas.SaveCount)プロパティがスタックに現在保存されている状態の数を返します。

使用することも、 [ `SKAutoCanvasRestore` ](xref:SkiaSharp.SKAutoCanvasRestore)キャンバスの状態を復元するためのクラス。 このクラスのコンス トラクターで呼び出されるものでは、`using`ステートメントでは、キャンバスの最後に状態が自動的に復元、`using`ブロックします。 

ただし、変換の 1 回の呼び出しから引き継がについて心配することも、`PaintSurface`へハンドラー。 新しい各呼び出し`PaintSurface`新規の配信`SKCanvas`既定の変換を持つオブジェクト。

もう 1 つの一般的な用途、`Translate`変換はビジュアル オブジェクトのレンダリングがもともと作成された描画は便利である座標を使用しています。 たとえば、中心点 (0, 0) で、アナログ時計の座標を指定する可能性があります。 できますし、変換を使用できます、時計を表示するのに目的の場所。 示します。 この手法は、**[Hendecagram 配列]** ページ。 [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs)クラスを作成して開始、 `SKPath` 11 - 星のオブジェクト。 `HendecagramPath`オブジェクトは、その他のデモ プログラムからアクセスできるように、パブリック、static、および読み取り専用で定義されます。 静的コンス トラクター内に作成されます。

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

星のすべてのポイントが点を囲む円に星形の中心点 (0, 0) がある場合。 各ポイントは、5/11ths の 360 度ずつ増加する角度の正弦と余弦の値の組み合わせです。 (2 の角度を増やすことで 11 の星を作成することも、11/3/11ths、または 4 の円の 11/)。円の半径は 100 として設定されます。

中央の左上隅に配置されますこのパスが、変換を行わずに表示される場合、`SKCanvas`とに四半期のみが表示されます。 `PaintSurface`のハンドラー`HendecagramPage`代わりに使用`Translate`色付きそれぞれランダムの星の複数のコピーでキャンバスを並べて表示します。

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

[![](translate-images/hendecagramarray-small.png "Hendecagram 配列ページのスクリーン ショットをトリプル")](translate-images/hendecagramarray-large.png#lightbox "Hendecagram 配列ページの 3 倍になるスクリーン ショット")

多くの場合、アニメーションには、変換が含まれます。 **Hendecagram アニメーション**ページは、11 星をで円状に移動します。 [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs)クラスがいくつかのフィールドで始まりのオーバーライド、`OnAppearing`と`OnDisappearing`開始および Xamarin.Forms タイマーを停止するメソッド。

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

`angle`フィールドがアニメーション化 0 度から 360 度を 5 秒ごとです。 `PaintSurface`ハンドラーを使用して、`angle`プロパティを 2 つの方法で: で色の色合いを指定する、`SKColor.FromHsl`メソッドをおよびへの引数として、`Math.Sin`と`Math.Cos`星の場所を制御する方法。

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

`PaintSurface`ハンドラーの呼び出し、`Translate`キャンバスの中央に変換するには、最初にメソッドを 2 回、およびその中心の円の円周を変換するために (0, 0)。 円の半径は、ページの範囲内で星を保持しながら可能な限り大きくに設定されます。

[![](translate-images/hendecagramanimation-small.png "Hendecagram アニメーション ページのスクリーン ショットをトリプル")](translate-images/hendecagramanimation-large.png#lightbox "Hendecagram アニメーション ページの 3 倍になるスクリーン ショット")

ページの中央を中心に、星が同じ方向を維持することを確認します。 これはまったく回転しません。 回転の変換用のジョブです。


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
