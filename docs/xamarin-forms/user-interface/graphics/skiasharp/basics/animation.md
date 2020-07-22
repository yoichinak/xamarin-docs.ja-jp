---
title: SkiaSharp の基本的なアニメーション
description: この記事では、アプリケーションで SkiaSharp グラフィックスをアニメーション化する方法について説明 Xamarin.Forms し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9a59f65655772768860ce29128f14a48641abc26
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84134278"
---
# <a name="basic-animation-in-skiasharp"></a>SkiaSharp の基本的なアニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp グラフィックスをアニメーション化する方法を見つける_

で SkiaSharp グラフィックスをアニメーション化するには、 Xamarin.Forms `PaintSurface` メソッドを定期的に呼び出す必要があります。これにより、グラフィックスの描画が少しずつ異なります。 この記事の後半では、中央から拡張されているように見える同心円を含むアニメーションを示します。

![](animation-images/animationexample.png "Several concentric circles seemingly expanding from the center")

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)プログラムの2つの軸をアニメーション化して、その楕円の2つの軸をアニメーション**化します**。これにより、この pulsation の速度を制御することもできます。 [**PulsatingEllipsePage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml)ファイルは、 Xamarin.Forms `Slider` `Label` スライダーの現在の値を表示するためにとをインスタンス化します。 これは、を `SKCanvasView` 他のビューと統合する一般的な方法です Xamarin.Forms 。

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

分離コードファイルは、 `Stopwatch` 高精度クロックとして機能するオブジェクトをインスタンス化します。 オーバーライドは、 `OnAppearing` `pageIsActive` フィールドをに設定し、 `true` という名前のメソッドを呼び出し `AnimationLoop` ます。 オーバーライドは、 `OnDisappearing` その `pageIsActive` フィールドを次のように設定し `false` ます。

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

メソッドはを開始し、 `AnimationLoop` `Stopwatch` がの間はループし `pageIsActive` `true` ます。 これは、実質的には、ページがアクティブな間は "無限ループ" になりますが、プログラムが動作しなくなるのは、ループが演算子を使用したの呼び出しで終了し `Task.Delay` `await` 、プログラムの他の部分を使用できるためです。 の引数を指定すると、 `Task.Delay` 1/30 秒後に完了します。 これにより、アニメーションのフレームレートが定義されます。

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

`while`ループは、からサイクル時間を取得することによって開始され `Slider` ます。 これは、たとえば5のように、秒単位の時間です。 2番目のステートメントは、 `t` *time*の値を計算します。 が `cycleTime` 5 の場合は、 `t` 5 秒ごとに0から1に増加します。 2番目のステートメントの関数の引数は、 `Math.Sin` 0 ~ 2 πの範囲で5秒ごとになります。 関数は、 `Math.Sin` 0 ~ 1 の範囲の値を0に戻し、 &ndash; 5 秒ごとに1と0に戻りますが、値が1または-1 のいずれかに近づくと、値がより遅く変化します。 値が常に正の値になるように値1が追加され、2で除算されているため、値は1/2 から1、1/2 から0、1/2 になりますが、値が 1 ~ 0 の場合は遅くなります。 これはフィールドに格納され、は `scale` `SKCanvasView` 無効になります。

`PaintSurface`このメソッドは、この値を使用して、 `scale` 楕円の2つの軸を計算します。

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

メソッドは、表示領域のサイズに基づいて最大半径を計算し、最大半径に基づく最小半径を計算します。 この `scale` 値は 0 ~ 1 の間でアニメーション化され、0に戻るため、メソッドはを使用して、との間の範囲を計算し `xRadius` `yRadius` `minRadius` `maxRadius` ます。 これらの値は、楕円を描画して塗りつぶすために使用されます。

[![](animation-images/pulsatingellipse-small.png "Triple screenshot of the Pulsating Ellipse page")](animation-images/pulsatingellipse-large.png#lightbox "Triple screenshot of the Pulsating Ellipse page")

`SKPaint`オブジェクトがブロック内に作成されていることに注意 `using` してください。 と同様に、多くの SkiaSharp クラスはから派生します `SKPaint` `SKObject` 。これは、 `SKNativeObject` インターフェイスを実装するから派生し [`IDisposable`](xref:System.IDisposable) ます。 `SKPaint`メソッドをオーバーライドして `Dispose` アンマネージリソースを解放します。

 ブロックに配置すると、 `SKPaint` `using` `Dispose` がブロックの最後に呼び出され、これらのアンマネージリソースが解放されます。 これは、オブジェクトによって使用されるメモリ `SKPaint` が .net ガベージコレクターによって解放される場合に発生しますが、アニメーションコードでは、より適切な方法でメモリを解放することをお勧めします。

 このような場合には、2つのオブジェクトを `SKPaint` 1 回作成し、それらをフィールドとして保存する方が適しています。

これが、**拡大する円**のアニメーションによって行われます。 クラスは、 [`ExpandingCirclesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) オブジェクトなど、いくつかのフィールドを定義することから始まり `SKPaint` ます。

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

このプログラムでは、メソッドに基づいて、アニメーションに別のアプローチを使用 Xamarin.Forms `Device.StartTimer` します。 フィールドは、 `t` 0 ~ 1 (ミリ秒ごと) にアニメーション化され `cycleTime` ます。

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

このハンドラーは、アニメーション化さ `PaintSurface` れた半径を持つ5つの同心円円を描画します。 `baseRadius`変数が100として計算された場合、 `t` 0 から1にアニメーション化されると、5つの円の半径は0から100、100から200、200から300、300から400、および400から500に増加します。 のほとんどの円では、は50ですが、最初の円の場合、は `strokeWidth` `strokeWidth` 0 から50までアニメーション化されます。 ほとんどの円では、色は青ですが、最後の円では、色は青から透明にアニメーション化されます。 不透明度を指定するコンストラクターの4番目の引数に注意して `SKColor` ください。

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

結果として、が0の場合、画像は同じように見えますが、円は継続して展開されたように `t` `t` 見えます。

[![](animation-images/expandingcircles-small.png "Triple screenshot of the Expanding Circles page")](animation-images/expandingcircles-large.png#lightbox "Triple screenshot of the Expanding Circles page")

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
