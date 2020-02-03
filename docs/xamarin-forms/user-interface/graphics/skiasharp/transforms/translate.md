---
title: 平行移動変換
description: この記事では、平行移動変換を使用して、Xamarin.Forms アプリケーションで SkiaSharp グラフィックスをシフトする方法を調べ、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: f1efd7610b32e6a3903d34fc2f8b5a6e20c9da8a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723604"
---
# <a name="the-translate-transform"></a>平行移動変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_変換変換を使用して SkiaSharp グラフィックスをシフトする方法について説明します。_

SkiaSharp の変換の最も単純な種類は *、変換変換または* *変換*変換です。 この変換では、水平および垂直方向のグラフィカル オブジェクトを移動します。 描画関数で使用している座標を変更するだけで通常と同じ効果を実現できますので、ある意味は、翻訳、最も不要な変換です。 パスを表示するときにただし、すべての座標はカプセル化パスにあるため、パス全体をシフトする平行移動変換を適用する方が簡単です。

翻訳もアニメーションを実行し、単純なテキスト効果に役立ちます。

![](translate-images/translateexample.png "Text shadow, engraving, and embossing with translation")

`SKCanvas` の[`Translate`](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single))メソッドには2つのパラメーターがあり、その後、描画されたグラフィックスオブジェクトが水平方向および垂直方向にシフトされます。

```csharp
public void Translate (Single dx, Single dy)
```

これらの引数は負の値にすることはできます。 2つ目の[`Translate`](xref:SkiaSharp.SKCanvas.Translate(SkiaSharp.SKPoint))メソッドは、1つの `SKPoint` 値に2つの変換値を結合します。

```csharp
public void Translate (SKPoint point)
```

[**SkiaSharpForms**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルプログラムの**累積翻訳**ページは、`Translate` メソッドの複数の呼び出しが累積されていることを示しています。 [`AccumulatedTranslatePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs)クラスでは、同じ四角形の20個のバージョンが表示されます。これは、前の四角形からの各オフセットが対角線に沿って引き伸ばされるためです。 `PaintSurface` イベントハンドラーを次に示します。

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

[![](translate-images/accumulatedtranslate-small.png "Triple screenshot of the Accumulated Translate page")](translate-images/accumulatedtranslate-large.png#lightbox "Triple screenshot of the Accumulated Translate page")

累積された変換係数が `dx` および `dy`で、描画関数で指定するポイントが (`x`、`y`) の場合、そのグラフィックオブジェクトはポイント (`x'`、`y'`) に表示されます。

x' = x + dx

y' = y + dy

これらは、変換の*変換式*として知られています。 新しい `SKCanvas` の `dx` と `dy` の既定値は0です。

**[テキスト効果の変換]** ページで示すように、影効果や同様の手法には変換変換を使用するのが一般的です。 [`TranslateTextEffectsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs)クラスの `PaintSurface` ハンドラーの関連する部分を次に示します。

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

3つの各例では、`Translate` を呼び出して、`x` と `y` 変数によって指定された場所からテキストをオフセットするようにします。 テキストを翻訳影響を別の色でもう一度表示されます。

[![](translate-images/translatetexteffects-small.png "Triple screenshot of the Translate Text Effects page")](translate-images/translatetexteffects-large.png#lightbox "Triple screenshot of the Translate Text Effects page")

次の3つの例は、`Translate` 呼び出しを否定する別の方法を示しています。

最初の例では、単に `Translate` を呼び出しますが、負の値を使用します。 `Translate` の呼び出しは累積的であるため、この一連の呼び出しは、変換の合計を既定値の0に単純に復元します。

2番目の例では、 [`ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix)を呼び出します。 これにより、すべての変換の既定の状態に戻ります。

3番目の例では、 [`Save`](xref:SkiaSharp.SKCanvas.Save)への呼び出しを使用して `SKCanvas` オブジェクトの状態を保存し、 [`Restore`](xref:SkiaSharp.SKCanvas.Restore)の呼び出しを使用して状態を復元します。 これは、最も汎用的な描画操作の一連の変換を操作する方法です。 これらの `Save` と `Restore` は、スタックのような関数を呼び出します。 `Save` を複数回呼び出し、逆の順序で `Restore` を呼び出して以前の状態に戻すことができます。 `Save` メソッドは整数を返します。この整数を[`RestoreToCount`](xref:SkiaSharp.SKCanvas.RestoreToCount*)に渡すことで、効果的に `Restore` を複数回呼び出すことができます。 [`SaveCount`](xref:SkiaSharp.SKCanvas.SaveCount)プロパティは、スタックに現在保存されている状態の数を返します。

[`SKAutoCanvasRestore`](xref:SkiaSharp.SKAutoCanvasRestore)クラスを使用して、キャンバスの状態を復元することもできます。 このクラスのコンストラクターは、`using` ステートメントで呼び出すことを意図しています。キャンバスの状態は、`using` ブロックの最後に自動的に復元されます。

ただし、`PaintSurface` ハンドラーの1回の呼び出しから次の呼び出しへの変換について心配する必要はありません。 `PaintSurface` の新しい呼び出しごとに、既定の変換を使用して新しい `SKCanvas` オブジェクトが配信されます。

`Translate` 変換のもう1つの一般的な用途は、描画に便利な座標を使用して最初に作成されたビジュアルオブジェクトをレンダリングすることです。 たとえば、中心点 (0, 0) で、アナログ時計の座標を指定する可能性があります。 できますし、変換を使用できます、時計を表示するのに目的の場所。 この手法については、 **[Hendecagram Array]** ページで説明します。 [`HendecagramArrayPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramArrayPage.cs)クラスは、まず、11ポイントの星の `SKPath` オブジェクトを作成します。 `HendecagramPath` オブジェクトは、他のデモンストレーションプログラムからアクセスできるように、パブリック、静的、および読み取り専用として定義されます。 静的コンス トラクター内に作成されます。

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

星のすべてのポイントが点を囲む円に星形の中心点 (0, 0) がある場合。 各ポイントは、5/11ths の 360 度ずつ増加する角度の正弦と余弦の値の組み合わせです。 (山の 2/11、3/11、または 4/11 の角度を増やすことで、11ポイントの星印を作成することもできます)。その円の半径は100として設定されます。

このパスを変換せずに表示すると、中央は `SKCanvas`の左上隅に配置され、そのうちの四半期だけが表示されます。 代わりに、`HendecagramPage` の `PaintSurface` ハンドラは `Translate` を使用して、キャンバスに星の複数のコピーを並べて表示します。各コピーはランダムに色分けされています。

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

結果は次のようになります。

[![](translate-images/hendecagramarray-small.png "Triple screenshot of the Hendecagram Array page")](translate-images/hendecagramarray-large.png#lightbox "Triple screenshot of the Hendecagram Array page")

多くの場合、アニメーションには、変換が含まれます。 **Hendecagram アニメーション**ページでは、円の周りにある11個の星が移動します。 [`HendecagramAnimationPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs)クラスは、次のいくつかのフィールドと、`OnAppearing` および `OnDisappearing` メソッドのオーバーライドを開始および停止して、Xamarin. フォームタイマーを開始および停止します。

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

`angle` フィールドは、5秒ごとに0°から360度にアニメーション化されます。 `PaintSurface` ハンドラーは、次の2つの方法で `angle` プロパティを使用します。 `SKColor.FromHsl` メソッドで色の色合いを指定する方法と、`Math.Sin` の引数として、星の場所を管理するための `Math.Cos` メソッドです。

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

`PaintSurface` ハンドラーは、`Translate` メソッドを2回呼び出します。まず、キャンバスの中央に平行移動し、次に (0, 0) を中心とする円の円周に変換します。 円の半径は、ページの範囲内で星を保持しながら可能な限り大きくに設定されます。

[![](translate-images/hendecagramanimation-small.png "Triple screenshot of the Hendecagram Animation page")](translate-images/hendecagramanimation-large.png#lightbox "Triple screenshot of the Hendecagram Animation page")

ページの中央を中心に、星が同じ方向を維持することを確認します。 これはまったく回転しません。 回転の変換用のジョブです。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
