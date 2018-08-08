---
title: SkiaSharp の基本的なアニメーション
description: この記事では、Xamarin.Forms アプリケーションで、SkiaSharp グラフィックスをアニメーション化する方法を説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 0ba3d86f52d2e6907f32450d87f30280ade95d3f
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615640"
---
# <a name="basic-animation-in-skiasharp"></a>SkiaSharp の基本的なアニメーション

_SkiaSharp、グラフィックスをアニメーション化する方法を検出します。_

それによって Xamarin.Forms で SkiaSharp グラフィックスをアニメーション化することができます、`PaintSurface`メソッドが非常に頻繁に呼び出されるたびに少し異なる方法でグラフィックスを描画します。 一見センターから展開同心円でこの記事の後半で示すようにアニメーションを次に示します。

![](animation-images/animationexample.png "一見、センターから展開するいくつかの同心円")

**いた楕円** ページで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)プログラムいたでに表示されないように、楕円の 2 つの軸をアニメーション化して、制御することも、この pulsation の比率。


[ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml)ファイルには、Xamarin.Forms がインスタンス化`Slider`と`Label`スライダーの現在の値を表示します。 これは、一般的な方法を統合する、`SKCanvasView`を他の Xamarin.Forms のビューと。

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

分離コード ファイルをインスタンス化、`Stopwatch`と精度の高いクロックとして機能するオブジェクト。 `OnAppearing`セットを上書き、`pageIsActive`フィールドを`true`という名前のメソッドを呼び出すと`AnimationLoop`します。 `OnDisappearing`上書きを設定する`pageIsActive`フィールドを`false`:

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

`AnimationLoop`メソッドの開始、`Stopwatch`と中にループし`pageIsActive`は`true`します。 ページがアクティブですが、ループの最後の呼び出しにハングするプログラムが原因で、これは、基本的に、「無限ループ」`Task.Delay`で、`await`演算子で、プログラムの関数の他の部分のことができます。 引数に`Task.Delay`1/30 秒後に完了すると、そのします。 これには、アニメーションのフレーム レートを定義します。

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

`while`ループの開始からのサイクル時間を取得することによって、`Slider`します。 たとえば、5 秒単位での時間です。 2 番目のステートメントの値を計算する`t`の*時間*します。 `cycleTime` 5 の`t`1 を 5 秒ごとに 0 から増加します。 引数、`Math.Sin`関数の 2 つ目のステートメントの範囲が 0 から 2 π 5 秒ごとにします。 `Math.Sin`関数は、0 ~ 1 のバックアップ、0 に値を返します&ndash;1 と 0 5 秒ごとに、値が 1 または – 1 近く遅く変更値。 値は正の値は常に、値が約 1 と 0 の場合は、お勧めの 1/2 に 0 に 1/2 に 1 ~ ½ 値の範囲であるために、2 で割ったは、値 1 は追加します。 保存されて、`scale`フィールド、および`SKCanvasView`は無効になります。

`PaintSurface`メソッドがこれを使用して`scale`楕円の 2 つの軸を計算する値。

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

メソッドは、表示領域のサイズに基づく最大半径と最大の半径に基づく最小半径を計算します。 `scale`値は、0 ~ 1 の間、および 0 にアニメーション化を計算するメソッドを使用するよう、`xRadius`と`yRadius`間の範囲を`minRadius`と`maxRadius`します。 これらの値は、描画し、楕円の塗りつぶしに使用されます。

[![](animation-images/pulsatingellipse-small.png "点滅するフロッピー楕円ページのスクリーン ショットをトリプル")](animation-images/pulsatingellipse-large.png#lightbox "点滅するフロッピー楕円ページの 3 倍になるスクリーン ショット")

注意、`SKPaint`でオブジェクトを作成、`using`ブロックします。 などの多くの SkiaSharp クラス`SKPaint`から派生した`SKObject`から派生した`SKNativeObject`、実装、 [ `IDisposable` ](xref:System.IDisposable)インターフェイス。 `SKPaint` 上書き、`Dispose`アンマネージ リソースを解放します。

 配置する`SKPaint`で、`using`ブロックにより`Dispose`はこれらのアンマネージ リソースを解放するブロックの最後に呼び出されます。 これは起こりますでメモリが使用するとき、`SKPaint`オブジェクトが解放されるより規則正しく方法でメモリを解放することで、ある程度積極的にすることをお勧め、.NET ガベージ コレクターによってがアニメーションのコードでは、します。

 このケースでより良いソリューションは 2 つ作成すること`SKPaint`フィールドとして保存し、1 回のオブジェクトします。

これが、**展開円**アニメーションは。 [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs)クラスの開始など、いくつかのフィールドを定義することで、`SKPaint`オブジェクト。

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

このプログラムは、Xamarin.Forms に基づくアニメーションを別のアプローチを使用して`Device.StartTimer`します。 `t`フィールドが 0 から 1 をアニメーション化すべて`cycleTime`(ミリ秒)。

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

`PaintSurface`ハンドラーして、半径をアニメーション化された 5 つの同心円を描画します。 場合、`baseRadius`変数として計算されます、100、として`t`が 0 から 1、0 から 100、100 ~ 200、200 ~ 300、300 ~ 400、400 ~ 500 に 5 つの円増加の半径をアニメーション化します。 ほとんどの円の`strokeWidth`50 ですが 1 つ目の円は、 `strokeWidth` 0 から 50 までアニメーション化します。 円のほとんどは、色が青ですが最後の円は、青から色をアニメーション透過的になります。

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

結果が同じ場合に画質`t`場合と同様の 0 に等しい`t`1 と等しいし、円を永久に展開を続行すると思われます。

[![](animation-images/expandingcircles-small.png "円の展開 ページのスクリーン ショットをトリプル")](animation-images/expandingcircles-large.png#lightbox "円の展開 ページの 3 倍になるスクリーン ショット")


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
