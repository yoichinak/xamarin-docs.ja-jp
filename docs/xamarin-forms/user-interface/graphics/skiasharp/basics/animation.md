---
title: SkiaSharp の基本的なアニメーション
description: この記事では、Xamarin.Forms アプリケーションで、SkiaSharp グラフィックスをアニメーション化する方法を説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 80de16a0cf9b601ac3795085b638b9d62812f4d9
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725551"
---
# <a name="basic-animation-in-skiasharp"></a>SkiaSharp の基本的なアニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp グラフィックスをアニメーション化する方法を見つける_

SkiaSharp のグラフィックスをアニメーション化するには、`PaintSurface` メソッドを定期的に呼び出す必要があります。これにより、グラフィックスの描画が少しずつ異なります。 一見センターから展開同心円でこの記事の後半で示すようにアニメーションを次に示します。

![](animation-images/animationexample.png "Several concentric circles seemingly expanding from the center")

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)プログラムの2つの軸をアニメーション化して、その楕円の2つの軸をアニメーション**化します**。これにより、この pulsation の速度を制御することもできます。 PulsatingEllipsePage ファイルは、 [**PulsatingEllipsePage.xaml**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) `Slider` と、スライダーの現在の値を表示するための `Label` をインスタンス化します。 これは、`SKCanvasView` を他の Xamarin 形式のビューと統合する一般的な方法です。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.PulsatingEllipsePage"
             Title="Pulsating Ellipse">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Slider x:Name="slider"
                Grid.Row="0"
                Maximum="10"
                Minimum="0.1"
                Value="5"
                Margin="20, 0" />

        <Label Grid.Row="1"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Cycle time = {0:F1} seconds'}"
               HorizontalTextAlignment="Center" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

分離コードファイルは、高精度のクロックとして機能する `Stopwatch` オブジェクトをインスタンス化します。 `OnAppearing` override は、`pageIsActive` フィールドを `true` に設定し、`AnimationLoop`という名前のメソッドを呼び出します。 `OnDisappearing` のオーバーライドでは、フィールドを `false`に `pageIsActive` します。

```csharp
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float scale;            // ranges from 0 to 1 to 0

public PulsatingEllipsePage()
{
    InitializeComponent();
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    AnimationLoop();
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    pageIsActive = false;
}
```

`AnimationLoop` メソッドは、`Stopwatch` を開始し、`pageIsActive` が `true`されている間ループします。 これは、実質的には、ページがアクティブな間は "無限ループ" になりますが、プログラムがハングする原因となるのは、ループが `await` 演算子を使用して `Task.Delay` の呼び出しで終了し、プログラムの他の部分を可能にするためです。 `Task.Delay` の引数を指定すると、1/30 秒後に完了します。 これには、アニメーションのフレーム レートを定義します。

```csharp
async Task AnimationLoop()
{
    stopwatch.Start();

    while (pageIsActive)
    {
        double cycleTime = slider.Value;
        double t = stopwatch.Elapsed.TotalSeconds % cycleTime / cycleTime;
        scale = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;
        canvasView.InvalidateSurface();
        await Task.Delay(TimeSpan.FromSeconds(1.0 / 30));
    }

    stopwatch.Stop();
}

```

`while` ループは、`Slider`からサイクル時間を取得することによって開始されます。 たとえば、5 秒単位での時間です。 2番目のステートメントは、 *time*に対して `t` の値を計算します。 `cycleTime` 5 の場合、`t` は5秒ごとに0から1に増加します。 2番目のステートメントの `Math.Sin` 関数の引数は、0 ~ 2 πの範囲で5秒ごとになります。 `Math.Sin` 関数は、0 ~ 1 の範囲の値を0に戻してから5秒ごとに &ndash;1 と0に戻りますが、値が1または-1 のいずれかに近づくと、値が遅くなります。 正の値は常に、値であり、値の範囲は ½ 1 1/2 に値が約 1 と 0 の場合は、お勧めの 1/2 に 0 にするため、2 で分割されますし、値 1 が追加されます。 これは `scale` フィールドに格納され、`SKCanvasView` は無効になります。

`PaintSurface` メソッドは、この `scale` 値を使用して、楕円の2つの軸を計算します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float maxRadius = 0.75f * Math.Min(info.Width, info.Height) / 2;
    float minRadius = 0.25f * maxRadius;

    float xRadius = minRadius * scale + maxRadius * (1 - scale);
    float yRadius = maxRadius * scale + minRadius * (1 - scale);

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.Color = SKColors.Blue;
        paint.StrokeWidth = 50;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);

        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.SkyBlue;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
    }
}
```

メソッドは、表示領域のサイズに基づく最大半径と最大の半径に基づく最小半径を計算します。 `scale` 値は、0 ~ 1 の間で0に戻るようにアニメーション化されます。そのため、このメソッドは、を使用して `xRadius` を計算し、`minRadius` と `maxRadius`の範囲を `yRadius` します。 これらの値は、描画し、楕円の塗りつぶしに使用されます。

[![](animation-images/pulsatingellipse-small.png "Triple screenshot of the Pulsating Ellipse page")](animation-images/pulsatingellipse-large.png#lightbox "Triple screenshot of the Pulsating Ellipse page")

`SKPaint` オブジェクトが `using` ブロックで作成されていることに注意してください。 `SKPaint` は、多くの SkiaSharp クラスと同様に、 [`IDisposable`](xref:System.IDisposable)インターフェイスを実装する `SKNativeObject`から派生する `SKObject`から派生します。 `SKPaint` は、アンマネージリソースを解放するために `Dispose` メソッドをオーバーライドします。

 `using` ブロックに `SKPaint` を配置すると、ブロックの最後に `Dispose` が呼び出され、これらのアンマネージリソースが解放されます。 これは、`SKPaint` オブジェクトによって使用されるメモリが .NET ガベージコレクターによって解放される場合に発生しますが、アニメーションコードでは、より適切な方法でメモリを解放することをお勧めします。

 このような場合には、2つの `SKPaint` オブジェクトを1回作成し、フィールドとして保存する方が適しています。

これが、**拡大する円**のアニメーションによって行われます。 [`ExpandingCirclesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs)クラスは、まず、`SKPaint` オブジェクトを含むいくつかのフィールドを定義します。

```csharp
public class ExpandingCirclesPage : ContentPage
{
    const double cycleTime = 1000;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float t;
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke
    };

    public ExpandingCirclesPage()
    {
        Title = "Expanding Circles";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ...
}
```

このプログラムでは、Xamarin. Forms `Device.StartTimer` メソッドに基づいて、アニメーションに別のアプローチを使用します。 `t` フィールドは、`cycleTime` ミリ秒ごとに 0 ~ 1 のアニメーション化されます。

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            t = (float)(stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime);
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

`PaintSurface` ハンドラーは、アニメーション化された半径を持つ5つの同心円円を描画します。 `baseRadius` 変数が100として計算された場合、`t` が0から1にアニメーション化されると、5つの円の半径が0から100、100から200、200から300、300から400、および400から500に増加します。 ほとんどの円では、`strokeWidth` は50ですが、最初の円の場合、`strokeWidth` は0から50にアニメーション化されます。 円のほとんどは、色が青が、最後の円は、青から色をアニメーションを透明にします。 不透明度を指定する `SKColor` コンストラクターの4番目の引数に注意してください。

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
        float baseRadius = Math.Min(info.Width, info.Height) / 12;

        for (int circle = 0; circle < 5; circle++)
        {
            float radius = baseRadius * (circle + t);

            paint.StrokeWidth = baseRadius / 2 * (circle == 0 ? t : 1);
            paint.Color = new SKColor(0, 0, 255,
                (byte)(255 * (circle == 4 ? (1 - t) : 1)));

            canvas.DrawCircle(center.X, center.Y, radius, paint);
        }
    }
}
```

結果として、`t` が1に等しい場合は `t` が0の場合と同じように見え、円は継続して展開されているように見えます。

[![](animation-images/expandingcircles-small.png "Triple screenshot of the Expanding Circles page")](animation-images/expandingcircles-large.png#lightbox "Triple screenshot of the Expanding Circles page")

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
