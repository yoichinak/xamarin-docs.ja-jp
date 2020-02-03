---
title: ドットとダッシュで SkiaSharp
description: この記事で SkiaSharp、点線および破線の線を描画の複雑さをマスターする方法について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 229f60cbb96058454a1c634e53a7bb00ec725bcf
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723755"
---
# <a name="dots-and-dashes-in-skiasharp"></a>ドットとダッシュで SkiaSharp

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp に点線や破線を描画した場合の複雑さをマスタにします。_

SkiaSharp では、solid はありませんが、代わりに、ドットとダッシュで構成されている直線を描画することができます。

![](dots-images/dottedlinesample.png "Dotted line")

これを行うには、*パス効果*を使用します。これは、`SKPaint`の[`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect)プロパティに設定する[`SKPathEffect`](xref:SkiaSharp.SKPathEffect)クラスのインスタンスです。 `SKPathEffect`によって定義された静的な作成方法の1つを使用して、パス効果 (またはパス効果の組み合わせ) を作成できます。 (`SKPathEffect` は、SkiaSharp でサポートされている6つの効果の1つです。その他は[**SkiaSharp Effect**](../effects/index.md)セクションで説明されています)。

点線または破線を描画するには、 [`SKPathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single))の静的メソッドを使用します。 2つの引数があります。これは、ドットとダッシュの長さ、およびそれらの間のスペースの長さを示す `float` 値の配列です。 この配列は、要素の偶数をいる必要があり、少なくとも 2 つの要素があります。 (配列には0個の要素がありますが、その結果、実線になります)。要素が2つある場合、1つ目はドットまたはダッシュの長さ、2番目は次のドットまたはダッシュの前のギャップの長さです。 3 つ以上の要素があるかどうか、この順序で、: ダッシュの長さ、ギャップの長さ、ダッシュの長さ、時間の差、およびなど。

一般的には、dash と間隔の長さのストロークの幅の倍数を作成するします。 ストロークの幅が 10 ピクセルの場合は、たとえば、し {10, 10} 配列描画点線、ドットとのギャップが、同じ長さのストロークの太さとします。

ただし、`SKPaint` オブジェクトの `StrokeCap` 設定は、これらのドットとダッシュにも影響します。 後ほど、としては、この配列の要素に対する影響を与えるを持っている必要があります。

点線と破線は、**ドットとダッシュ**のページで示されています。 [**DotsAndDashesPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml)ファイルは2つの `Picker` ビューをインスタンス化します。1つはストロークキャップを選択し、2番目はダッシュ配列を選択します。

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

 `dashArrayPicker` の最初の3つの項目は、ストロークの幅が10ピクセルであることを前提としています。 {10, 10} アレイが点線では、{30, 10} 破線、および {10, 10、30, 10} には点鎖線です。 (他の 3 つ、後ほど。)

[`DotsAndDashesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml.cs)分離コードファイルには、`PaintSurface` イベントハンドラーと、`Picker` ビューにアクセスするためのいくつかのヘルパールーチンが含まれています。

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

次のスクリーン ショットの一番左にある iOS の画面には、点線が示されています。

[![](dots-images/dotsanddashes-small.png "Triple screenshot of the Dots and Dashes page")](dots-images/dotsanddashes-large.png#lightbox "Triple screenshot of the Dots and Dashes page")

ただし、Android の画面は {10, 10} の配列を使用して点線を表示することにもなってが代わりに、行が堅牢です。 どうなっているのでしょうか。 問題は、Android の画面に `Square`のストロークキャップが設定されていることです。 これは、ストローク幅の半分だけ、ギャップがいっぱいになり、すべてのダッシュを拡張します。

`Square` または `Round`のストロークキャップを使用するときにこの問題を回避するには、配列内のダッシュの長さをストロークの長さで減らし (場合によってはダッシュの長さが0になることがあります)、境界の長さをストロークの長さで増やします。 XAML ファイル内の `Picker` の最後の3つのダッシュ配列は、次のように計算されます。

- {10, 10} が {0, 20} 点線の場合
- {30, 10} が {20、20} 破線の
- {10, 10、30, 10} {0、20、20、20} を点線および破線の線になります

UWP 画面に、`Round`のストロークキャップの点線と破線が表示されます。 `Round` ストロークキャップは、多くの場合、太い線でのドットとダッシュの最適な外観を提供します。

ここまでは、`SKPathEffect.CreateDash` メソッドの2番目のパラメーターについては説明していませんでした。 このパラメーターには `phase` という名前が付けられ、行の先頭のドットとダッシュのパターン内のオフセットを参照します。 たとえば、ダッシュ配列が {10, 10} で、`phase` が10の場合、行はドットではなくギャップで始まります。

`phase` パラメーターの興味深いアプリケーションの1つは、アニメーションです。 アニメーション化された**らせん**状のページは、 [`AnimatedSpiralPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/AnimatedSpiralPage.cs)クラスが Xamarin. Forms `Device.Timer` メソッドを使用して `phase` パラメーターをアニメーション化する点を除いて、アーカイブのための**らせん状**のページに似ています。

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

もちろん、実際にアニメーションを表示するプログラムを実行する必要があります。

[![](dots-images/animatedspiral-small.png "Triple screenshot of the Animated Spiral page")](dots-images/animatedspiral-large.png#lightbox "Triple screenshot of the Animated Spiral page")

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
