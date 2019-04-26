---
title: ドットとダッシュで SkiaSharp
description: この記事で SkiaSharp、点線および破線の線を描画の複雑さをマスターする方法について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: f59aa92f5f4f013a2d14b1667f4d0679a7ba82b3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384831"
---
# <a name="dots-and-dashes-in-skiasharp"></a>ドットとダッシュで SkiaSharp

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_点線および破線の SkiaSharp 描画の複雑さをマスターします。_

SkiaSharp では、solid はありませんが、代わりに、ドットとダッシュで構成されている直線を描画することができます。

![](dots-images/dottedlinesample.png "点線")

これには、*パス効果*のインスタンスである、 [ `SKPathEffect` ](xref:SkiaSharp.SKPathEffect)に設定するクラス、 [ `PathEffect` ](xref:SkiaSharp.SKPaint.PathEffect)プロパティの`SKPaint`します。 パスを作成することによって定義された静的な作成方法のいずれかを使用して、効果 (または結合パスの効果)`SKPathEffect`します。 (`SKPathEffect` SkiaSharp でサポートされる 6 つの効果の 1 つは、他のユーザーは、セクションで説明されている[ **SkiaSharp 効果**](../effects/index.md))。

点線または破線を描画するために使用する、 [ `SKPathEffect.CreateDash` ](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single))静的メソッド。 これには 2 つの引数があります。これは最初の配列、`float`ドットとダッシュの長さとそれらの間の空白文字の長さを示す値。 この配列は、要素の偶数をいる必要があり、少なくとも 2 つの要素があります。 (がありますが、配列の要素が 0、実線では、その結果です。)2 つの要素がある場合は、1 つはドットまたはダッシュの長さ、2 番目の間隔の長さ [次へ] のドットまたは dash の前にします。 3 つ以上の要素があるかどうか、この順序で、: ダッシュの長さ、ギャップの長さ、ダッシュの長さ、時間の差、およびなど。

一般的には、dash と間隔の長さのストロークの幅の倍数を作成するします。 ストロークの幅が 10 ピクセルの場合は、たとえば、し {10, 10} 配列描画点線、ドットとのギャップが、同じ長さのストロークの太さとします。

ただし、`StrokeCap`の設定、`SKPaint`オブジェクトは、これらのドットとダッシュにも影響します。 後ほど、としては、この配列の要素に対する影響を与えるを持っている必要があります。

点線および破線がで示されています、**ドットし、ダッシュ**ページ。 [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml)ファイルでは、2 つのインスタンス化します`Picker`ストローク キャップおよび dash アレイを選択する 2 番目を選択することの 1 つのビューします。

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

 最初の 3 つの項目、`dashArrayPicker`ストロークの幅が 10 ピクセルであると仮定します。 {10, 10} アレイが点線では、{30, 10} 破線、および {10, 10、30, 10} には点鎖線です。 (他の 3 つ、後ほど。)

[ `DotsAndDashesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs)分離コード ファイルが含まれています、`PaintSurface`イベント ハンドラーとヘルパー ルーチンにアクセスするための 2 つ、`Picker`ビュー。

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

[![](dots-images/dotsanddashes-small.png "ドットとダッシュのページのスクリーン ショットをトリプル")](dots-images/dotsanddashes-large.png#lightbox "ドットとダッシュのページの 3 倍になるスクリーン ショット")

ただし、Android の画面は {10, 10} の配列を使用して点線を表示することにもなってが代わりに、行が堅牢です。 どうなっているのでしょうか。 問題は、Android の画面は、のストローク キャップの設定もが`Square`します。 これは、ストローク幅の半分だけ、ギャップがいっぱいになり、すべてのダッシュを拡張します。

ストローク キャップを使用する場合、この問題を回避する`Square`または`Round`、(場合によってその結果、ダッシュの長さは 0)、ストロークの長さによって配列のダッシュの長さを短縮し、ストロークの長さによって間隔の長さを増やす必要があります。 これは、方法、最後の 3 つダッシュで配列、 `Picker` XAML ファイルが計算されます。

- {10, 10} が {0, 20} 点線の場合
- {30, 10} が {20、20} 破線の
- {10, 10、30, 10} {0、20、20、20} を点線および破線の線になります

点線および破線の線を UWP 画面表示のキャップの`Round`します。 `Round`ストローク キャップが太い線で多くの場合、ドットとダッシュの最良の外観を提供します。

これまでにメンションが加えられていない 2 番目のパラメーターの`SKPathEffect.CreateDash`メソッド。 このパラメータの名前は`phase`と行の先頭のドットの破線パターン内のオフセットを指しています。 たとえば、dash 配列 {10, 10} と`phase`10 では、ドットではなく、ギャップで始まる行。

1 つの興味深いアプリケーション、`phase`パラメーターは、アニメーションにできます。 **アニメーション スパイラル**に似ている、 **Archimedean スパイラル**ページのことを除いて、 [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/AnimatedSpiralPage.cs)クラスがアニメーション化、`phase`パラメーターを使用して、Xamarin.Forms`Device.Timer`メソッド。


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

[![](dots-images/animatedspiral-small.png "アニメーションのスパイラル ページのスクリーン ショットをトリプル")](dots-images/animatedspiral-large.png#lightbox "らせんをアニメーション化されるページの 3 倍になるスクリーン ショット")

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
