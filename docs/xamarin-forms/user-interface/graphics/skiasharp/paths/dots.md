---
title: SkiaSharp のドットとダッシュ
description: この記事では、SkiaSharp で点線と破線を描画する複雑さを習得する方法について説明し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5064a53b140c26acdc5149f5495cc002e657a9b0
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91564005"
---
# <a name="dots-and-dashes-in-skiasharp"></a>SkiaSharp のドットとダッシュ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp に点線や破線を描画した場合の複雑さをマスタにします。_

SkiaSharp を使用すると、実線ではなく、ドットとダッシュで構成される直線を描画できます。

![点線](dots-images/dottedlinesample.png)

これを行うには、 *パス効果*を使用します。これは、 [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) のプロパティに設定するクラスのインスタンスです [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) `SKPaint` 。 で定義されているいずれかの静的作成方法を使用して、パス効果を作成する (またはパス効果を結合する) ことができ `SKPathEffect` ます。 ( `SKPathEffect` は、SkiaSharp でサポートされている6つの効果の1つです。その他は、 [**SkiaSharp の効果**](../effects/index.md)に関するセクションで説明されています)。

点線または破線を描画するには、 [`SKPathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) 静的メソッドを使用します。 2つの引数があります。これは、 `float` ドットとダッシュの長さ、およびそれらの間のスペースの長さを示す値の配列です。 この配列には、偶数個の要素を含める必要があります。また、少なくとも2つの要素が必要です。 (配列には0個の要素がありますが、その結果、実線になります)。要素が2つある場合、1つ目はドットまたはダッシュの長さ、2番目は次のドットまたはダッシュの前のギャップの長さです。 3つ以上の要素がある場合は、ダッシュの長さ、ギャップの長さ、ダッシュの長さ、ギャップの長さなどの順序になります。

通常は、ダッシュとギャップの長さをストロークの幅の倍数にする必要があります。 たとえば、ストロークの幅が10ピクセルの場合、配列 {10, 10} は、ドットとギャップがストロークの太さと同じ長さになる点線を描画します。

ただし、 `StrokeCap` オブジェクトの設定は、 `SKPaint` これらのドットとダッシュにも影響します。 すぐにわかるように、この配列の要素には影響があります。

点線と破線は、 **ドットとダッシュ** のページで示されています。 [**DotsAndDashesPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml)ファイルは2つのビューをインスタンス化します。 `Picker` 1 つはストロークキャップを選択し、2番目はダッシュ配列を選択します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.DotsAndDashesPage"
             Title="Dots and Dashes">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKStrokeCap}">
                    <x:Static Member="skia:SKStrokeCap.Butt" />
                    <x:Static Member="skia:SKStrokeCap.Round" />
                    <x:Static Member="skia:SKStrokeCap.Square" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>10, 10</x:String>
                    <x:String>30, 10</x:String>
                    <x:String>10, 10, 30, 10</x:String>
                    <x:String>0, 20</x:String>
                    <x:String>20, 20</x:String>
                    <x:String>0, 20, 20, 20</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                PaintSurface="OnCanvasViewPaintSurface"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

 の最初の3つの項目は、 `dashArrayPicker` ストロークの幅が10ピクセルであることを前提としています。 {10, 10} 配列は点線、{30, 10} は破線に使用され、{10, 10, 30, 10} はドットとダッシュの線に対して使用されます。 (他の3つについては、すぐに説明します)。

[`DotsAndDashesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml.cs)分離コードファイルには、 `PaintSurface` イベントハンドラーと、ビューにアクセスするためのいくつかのヘルパールーチンが含まれてい `Picker` ます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem,
        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string str = (string)picker.SelectedItem;
    string[] strs = str.Split(new char[] { ' ', ',' }, StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

次のスクリーンショットでは、左側の iOS 画面に点線が表示されています。

[![[ドットとダッシュ] ページのトリプルスクリーンショット](dots-images/dotsanddashes-small.png)](dots-images/dotsanddashes-large.png#lightbox "[ドットとダッシュ] ページのトリプルスクリーンショット")

ただし、Android の画面では、配列 {10, 10} を使用して点線を表示することも想定されていますが、線は実線です。 何が起きましたか? 問題は、Android の画面にも、のストロークキャップが設定されていることです `Square` 。 これにより、すべてのダッシュがストローク幅の半分まで拡張され、ギャップを埋めることができます。

またはのストロークキャップを使用するときにこの問題を回避するに `Square` は `Round` 、配列内のダッシュの長さをストロークの長さで減らし (場合によってはダッシュ長が0になることもあります)、境界の長さをストロークの長さで増やします。 XAML ファイル内の最後の3つのダッシュ配列は、次のように `Picker` 計算されます。

- 点線の場合は {10, 10} が {0, 20} になります
- 破線では {30, 10} が {20, 20} になります
- ' 10, 10, 30, 10} は、点線と破線の {0, 20, 20, 20} になります

UWP 画面に、のストロークキャップの点線と破線が表示され `Round` ます。 `Round`ストロークキャップは、多くの場合、太い線でのドットとダッシュの最適な外観を提供します。

ここまでは、メソッドの2番目のパラメーターについて言及していませんでした `SKPathEffect.CreateDash` 。 このパラメーターはという名前 `phase` で、行の先頭のドットとダッシュのパターン内のオフセットを参照します。 たとえば、ダッシュ配列が {10, 10} で、が10の場合、 `phase` 行はドットではなくギャップで始まります。

パラメーターの興味深いアプリケーションの1つ `phase` は、アニメーションです。 アニメーション化された**らせん**のページは、クラスが**Archimedean Spiral** [`AnimatedSpiralPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/AnimatedSpiralPage.cs) `phase` メソッドを使用してパラメーターをアニメーション化する点を除いて、[アーカイブ] と同じようにらせんされたページに似てい Xamarin.Forms `Device.Timer` ます。

```csharp
public class AnimatedSpiralPage : ContentPage
{
    const double cycleTime = 250;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float dashPhase;

    public AnimatedSpiralPage()
    {
        Title = "Animated Spiral";

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
            dashPhase = (float)(10 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }
    ···  
}
```

もちろん、アニメーションを表示するには、プログラムを実際に実行する必要があります。

[![アニメーション化されたらせんページのトリプルスクリーンショット](dots-images/animatedspiral-small.png)](dots-images/animatedspiral-large.png#lightbox "アニメーション化されたらせんページのトリプルスクリーンショット")

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)