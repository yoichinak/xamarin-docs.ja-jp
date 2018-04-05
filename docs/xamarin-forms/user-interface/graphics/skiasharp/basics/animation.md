---
title: 基本的なアニメーション
description: SkiaSharp グラフィックスをアニメーション化する方法を説明します。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 859677a3dcfcddd0b333c9ddf60c01e2093b6a5b
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/05/2018
---
# <a name="basic-animation"></a>基本的なアニメーション

_SkiaSharp グラフィックスをアニメーション化する方法を説明します。_

発生させて xamarin.forms SkiaSharp グラフィックスをアニメーション化することができます、`PaintSurface`に非常に頻繁に呼び出されるメソッドごとに少し異なる方法でグラフィックスを描画します。 同心円一見センターから展開すると、この記事の後半に示すように、アニメーションを次に示します。

![](animation-images/animationexample.png "いくつかの同心円センターから展開するように見える")

**いた楕円** ページで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)プログラムいたでに表示されないように、楕円の 2 つの軸をアニメーション化して、制御することも、この pulsation の比率。


[ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml)ファイルは、Xamarin.Forms をインスタンス化`Slider`と`Label`スライダーの現在の値を表示します。 これは、一般的な方法で、統合、 `SKCanvasView` Xamarin.Forms 他のビューと。

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

分離コード ファイルをインスタンス化、`Stopwatch`高精度クロックとして機能するオブジェクト。 `OnAppearing`セットを上書き、`pageIsActive`フィールドを`true`という名前のメソッドを呼び出すと`AnimationLoop`です。 `OnDisappearing`上書きを設定する`pageIsActive`フィールドを`false`:

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

`AnimationLoop`メソッドの開始、`Stopwatch`と中にループし、`pageIsActive`は`true`します。 ページ アクティブですが、ループの最後の呼び出しにハングするプログラムが原因で、これは、本質的には、「無限ループ」`Task.Delay`で、`await`演算子で、プログラムの関数の他の部分をことができます。 引数に`Task.Delay`1/30 秒後に完了するとします。 これには、アニメーションのフレーム レートを定義します。

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

`while`ループを開始からのサイクル時間を取得することによって、`Slider`です。 たとえば、5 秒単位で時間です。 2 番目のステートメントの値を計算する`t`の*時間*です。 `cycleTime` 5、 `t` 1 に 5 秒ごとに 0 から増加します。 引数、`Math.Sin`関数の 2 番目のステートメントの範囲が 0 から 2 π 5 秒ごとにします。 `Math.Sin`関数は、0 から 0 と 1 の背面まで値を返します&ndash;1 と 0、5 秒ごと、値が 1 または – 1 の近くに遅く変更値しますが、します。 値 1 が加算されるは、正の値は常に、値とし、これが 2 で割った値、値が約 1 および 0 の場合にお勧めの 1/2 に 0 に 1/2 に 1 ~ ½ 値の範囲、ためです。 設定されて、`scale`フィールド、および`SKCanvasView`は無効になります。

`PaintSurface`メソッドを使用してこの`scale`楕円の 2 つの軸を計算する値。

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

メソッドは、表示領域のサイズに基づく最大 radius および最大半径に基づく最小 radius を計算します。 `scale`を計算するメソッドを使用するため、値を 0 ~ 1 の間、および 0 にはアニメーション、`xRadius`と`yRadius`間の範囲を`minRadius`と`maxRadius`です。 塗りつぶし楕円を描画し、これらの値が使用されます。

[![](animation-images/pulsatingellipse-small.png "点滅するフロッピー楕円ページのスクリーン ショットをトリプル")](animation-images/pulsatingellipse-large.png#lightbox "点滅するフロッピー楕円ページのトリプル スクリーン ショット")

注意して、`SKPaint`でオブジェクトを作成、`using`ブロックします。 などの多くの SkiaSharp クラス`SKPaint`から派生した`SKObject`から派生した`SKNativeObject`を実装する、 [ `IDisposable` ](https://developer.xamarin.com/api/type/System.IDisposable/)インターフェイスです。 `SKPaint` 上書き、`Dispose`アンマネージ リソースを解放します。

 配置`SKPaint`で、`using`ブロックにより`Dispose`はこれらのアンマネージ リソースを解放する、ブロックの最後に呼び出されます。 いる場合、このままでメモリが使用される、`SKPaint`オブジェクトの解放を適切な方法でメモリの解放で若干積極的に取り組むことをお勧め .NET ガベージ コレクターによっては、アニメーションのコードでします。

 2 つを作成することでこの特定のケースのより良いソリューション`SKPaint`フィールドとしてオブジェクトの 1 回、および保存します。

その目的、**展開円**アニメーションはします。 [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs)クラスの開始など、いくつかのフィールドを定義することによって、`SKPaint`オブジェクト。

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

このプログラムは、Xamarin.Forms に基づいてアニメーションを別のアプローチを使用して`Device.StartTimer`です。 `t`フィールドが 0 から 1 をアニメーション化すべて`cycleTime`(ミリ秒)。

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

`PaintSurface`ハンドラーはアニメーション radii して 5 つの同心円を描画します。 場合、`baseRadius`変数として計算されます、100、として`t`0 1、5 つの円の増加を 0 から 100、100 ~ 200、200 ~ 300、300 ~ 400、400 ~ 500 の半径をアニメーション化します。 ほとんどの円の`strokeWidth`は 50 ですが、最初の円、 `strokeWidth` 0 から 50 までアニメーション化します。 円のほとんどは、色が青が、最後の円には、青と色をアニメーションを透明に。

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

結果は同じときに、画質`t`場合に 0 に等しい`t`1 と等しいし、円が永久に展開を続行しましょう。

[![](animation-images/expandingcircles-small.png "円の展開 ページのスクリーン ショットをトリプル")](animation-images/expandingcircles-large.png#lightbox "円の展開 ページのトリプル スクリーン ショット")


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
