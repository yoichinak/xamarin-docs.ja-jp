---
title: 平行移動変換
description: この記事では、変換変換を使用してアプリケーションで SkiaSharp グラフィックスをシフトする方法につい Xamarin.Forms て説明し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fdb59cf8b40c62bc4375a12368ed871898497adf
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368592"
---
# <a name="the-translate-transform"></a>平行移動変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_変換変換を使用して SkiaSharp グラフィックスをシフトする方法について説明します。_

SkiaSharp の変換の最も単純な種類は *、変換変換または**変換* 変換です。 この変換は、グラフィカルオブジェクトを水平方向および垂直方向にシフトします。 これは、通常、描画関数で使用している座標を変更するだけで同じ効果が得られるため、変換は最も不要な変換です。 ただし、パスをレンダリングする場合は、すべての座標がパスにカプセル化されるため、パス全体をシフトするために変換変換を適用する方がはるかに簡単です。

翻訳は、アニメーションや単純なテキスト効果にも役立ちます。

![変換によるテキストの影、engraving、およびエンボス](translate-images/translateexample.png)

[`Translate`](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single))のメソッドに `SKCanvas` は2つのパラメーターがあり、その後、描画されたグラフィックスオブジェクトが水平方向および垂直方向にシフトされます。

```csharp
public void Translate (Single dx, Single dy)
```

これらの引数には負の値を指定できます。 2番目のメソッドは、 [`Translate`](xref:SkiaSharp.SKCanvas.Translate(SkiaSharp.SKPoint)) 次の2つの変換値を1つの値に結合し `SKPoint` ます。

```csharp
public void Translate (SKPoint point)
```

[**SkiaSharpForms**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルプログラムの **累積翻訳** ページは、メソッドの複数の呼び出しが累積されていることを示して `Translate` います。 クラスには、 [`AccumulatedTranslatePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) 同じ四角形の20個のバージョンが表示されます。これは、前の四角形からのオフセットを1つだけ、対角線に沿って伸縮します。 `PaintSurface`イベントハンドラーを次に示します。

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

連続した四角形は、ページの下にトリクルます。

[![累積翻訳ページのトリプルスクリーンショット](translate-images/accumulatedtranslate-small.png)](translate-images/accumulatedtranslate-large.png#lightbox "累積翻訳ページのトリプルスクリーンショット")

累積された変換係数が `dx` およびで `dy` 、描画関数で指定するポイントが (,) である場合、 `x` `y` グラフィックオブジェクトはポイント (、) にレンダリングされます。この場合、 `x'` 次のように `y'` なります。

x ' = x + dx

y ' = y + dy

これらは、変換の *変換式* として知られています。 新しいのの既定 `dx` 値 `dy` `SKCanvas` は0です。

[ **テキスト効果の変換** ] ページで示すように、影効果や同様の手法には変換変換を使用するのが一般的です。 クラスのハンドラーに関連する部分を次 `PaintSurface` に示し [`TranslateTextEffectsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) ます。

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

3つの各例で `Translate` は、変数および変数で指定された場所からオフセットするテキストを表示するために、が呼び出され `x` `y` ます。 次に、テキストは、変換効果のない別の色で再び表示されます。

[![[テキストの翻訳] ページのトリプルスクリーンショット](translate-images/translatetexteffects-small.png)](translate-images/translatetexteffects-large.png#lightbox "[テキストの翻訳] ページのトリプルスクリーンショット")

次の3つの例は、呼び出しを否定する別の方法を示してい `Translate` ます。

最初の例では、単 `Translate` にを呼び出しますが、負の値を使用します。 `Translate`呼び出しは累積されるため、この一連の呼び出しは、合計変換を既定値の0に単純に復元します。

2番目の例はを呼び出し [`ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix) ます。 これにより、すべての変換が既定の状態に戻ります。

3番目の例では、への呼び出しを使用してオブジェクトの状態を保存し、 `SKCanvas` [`Save`](xref:SkiaSharp.SKCanvas.Save) を呼び出して状態を復元し [`Restore`](xref:SkiaSharp.SKCanvas.Restore) ます。 これは、一連の描画操作の変換を操作するための最も汎用性の高い方法です。 これら `Save` の `Restore` 関数と関数は、スタックのように関数を呼び出します。 `Save` 複数回を呼び出してから逆順でを呼び出して `Restore` 前の状態に戻すことができます。 `Save`メソッドは整数を返します。この整数をに渡して、 [`RestoreToCount`](xref:SkiaSharp.SKCanvas.RestoreToCount*) 効率的に複数回呼び出すことができ `Restore` ます。 プロパティは、 [`SaveCount`](xref:SkiaSharp.SKCanvas.SaveCount) スタックに現在保存されている状態の数を返します。

また、クラスを使用して、キャンバスの状態を復元することもでき [`SKAutoCanvasRestore`](xref:SkiaSharp.SKAutoCanvasRestore) ます。 このクラスのコンストラクターは、ステートメントで呼び出されることを意図しています `using` 。キャンバスの状態は、ブロックの最後に自動的に復元され `using` ます。

ただし、ハンドラーの1回の呼び出しから次の呼び出しへの変換について心配する必要はありません `PaintSurface` 。 の新しい呼び出しごとに `PaintSurface` 、既定の変換で新しいオブジェクトが提供さ `SKCanvas` れます。

変換のもう1つの一般的な用途 `Translate` は、描画に便利な座標を使用して最初に作成されたビジュアルオブジェクトをレンダリングすることです。 たとえば、ポイント (0, 0) の中心を持つアナログクロックの座標を指定することができます。 変換を使用して、必要な場所に時計を表示できます。 この手法については、[ **Hendecagram Array** ] ページで説明します。 クラスは、 [`HendecagramArrayPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramArrayPage.cs) 最初に、 `SKPath` 11 ポイントの星のオブジェクトを作成します。 `HendecagramPath`オブジェクトは、他のデモンストレーションプログラムからアクセスできるように、パブリック、静的、および読み取り専用として定義されます。 静的コンストラクターに作成されます。

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

星の中心がポイント (0, 0) の場合、星のすべてのポイントはその点を囲む円上にあります。 各点は、360度の 5/11 分の1になる角度の正弦とコサインの値の組み合わせです。 (山の 2/11、3/11、または 4/11 の角度を増やすことで、11ポイントの星印を作成することもできます)。その円の半径は100として設定されます。

このパスを変換せずに表示すると、中央はの左上隅に配置され、 `SKCanvas` そのうち四半期だけが表示されます。 のハンドラーは、を使用して、それぞれがランダムに色付けされた `PaintSurface` `HendecagramPage` `Translate` 星の複数のコピーでキャンバスを並べて表示します。

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

結果は次のとおりです。

[![Hendecagram 配列ページのトリプルスクリーンショット](translate-images/hendecagramarray-small.png)](translate-images/hendecagramarray-large.png#lightbox "Hendecagram 配列ページのトリプルスクリーンショット")

多くの場合、アニメーションには変換が含まれます。 **Hendecagram アニメーション** ページでは、円の周りにある11個の星が移動します。 クラスは、 [`HendecagramAnimationPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) `OnAppearing` `OnDisappearing` タイマーを開始および停止するためのメソッドとメソッドのいくつかのフィールドとオーバーライドで始まり Xamarin.Forms ます。

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

この `angle` フィールドは、5秒ごとに0°から360度にアニメーション化されます。 この `PaintSurface` ハンドラーでは、 `angle` 次の2つの方法でプロパティを使用します。メソッドの色の色合いを指定する方法 `SKColor.FromHsl` と、 `Math.Sin` `Math.Cos` 星の位置を制御するメソッドとメソッドの引数として使用する方法です。

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

`PaintSurface`ハンドラーはメソッドを `Translate` 2 回呼び出し、まずキャンバスの中央に平行移動し、次に (0, 0) を中心とする円の円周に変換します。 円の半径は可能な限り大きくなるように設定されていますが、ページの境界内で星を維持しています。

[![Hendecagram アニメーションページのトリプルスクリーンショット](translate-images/hendecagramanimation-small.png)](translate-images/hendecagramanimation-large.png#lightbox "Hendecagram アニメーションページのトリプルスクリーンショット")

星の向きは、ページの中央を中心として同じ向きになります。 まったく回転しません。 これは、回転変換のジョブです。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)