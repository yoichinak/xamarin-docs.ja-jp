---
title: ドットとダッシュで SkiaSharp
description: この記事で SkiaSharp、点線および破線の線を描画の複雑さをマスターする方法について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 7c336e6b5224f61ff84eb39652788b23f52b806e
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615419"
---
# <a name="dots-and-dashes-in-skiasharp"></a>ドットとダッシュで SkiaSharp

_点線および破線の SkiaSharp 描画の複雑さをマスターします。_

SkiaSharp では、solid はありませんが、代わりに、ドットとダッシュで構成されている直線を描画することができます。

![](dots-images/dottedlinesample.png "点線")

これには、*パス効果*のインスタンスである、 [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/)に設定するクラス、 [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/)プロパティの`SKPaint`します。 パスを作成する静的なを使用して、効果 (または結合パスの効果)`Create`によって定義されたメソッド`SKPathEffect`します。

点線または破線を描画するために使用する、 [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/)静的メソッド。 2 つの引数: 最初の配列は、この`float`ドットとダッシュの長さとそれらの間の空白文字の長さを示す値。 この配列は、要素の偶数をいる必要があり、少なくとも 2 つの要素があります。 (がありますが、配列の要素が 0、実線では、その結果です。)2 つの要素がある場合は、1 つはドットまたはダッシュの長さ、2 番目の間隔の長さ [次へ] のドットまたは dash の前にします。 3 つ以上の要素があるかどうか、この順序で、: ダッシュの長さ、ギャップの長さ、ダッシュの長さ、時間の差、およびなど。

一般的には、dash と間隔の長さのストロークの幅の倍数を作成するします。 ストロークの幅が 10 ピクセルの場合は、たとえば、し {10, 10} 配列描画点線、ドットとのギャップが、同じ長さのストロークの太さとします。

ただし、`StrokeCap`の設定、`SKPaint`オブジェクトは、これらのドットとダッシュにも影響します。 後ほど、としては、この配列の要素に対する影響を与えるを持っている必要があります。

点線および破線がで示されています、**ドットし、ダッシュ**ページ。 [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml)ファイルでは、2 つのインスタンス化します`Picker`ストローク キャップおよび dash アレイを選択する 2 番目を選択することの 1 つのビューします。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.DotsAndDashesPage"
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
            <Picker.Items>
                <x:String>Butt</x:String>
                <x:String>Round</x:String>
                <x:String>Square</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>10, 10</x:String>
                <x:String>30, 10</x:String>
                <x:String>10, 10, 30, 10</x:String>
                <x:String>0, 20</x:String>
                <x:String>20, 20</x:String>
                <x:String>0, 20, 20, 20</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

最初の 3 つの項目、`dashArrayPicker`ストロークの幅が 10 ピクセルであると仮定します。 {10, 10} アレイが点線では、{30, 10} 破線、および {10, 10、30, 10} には点鎖線です。 (他の 3 つ、後ほど。)

[ `DotsAndDashesPage`分離コード ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs)が含まれています、`PaintSurface`イベント ハンドラーとヘルパー ルーチンにアクセスするための 2 つ、`Picker`ビュー。

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
        StrokeCap = (SKStrokeCap)Enum.Parse(typeof(SKStrokeCap),
                        strokeCapPicker.Items[strokeCapPicker.SelectedIndex]),

        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }

    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string[] strs = picker.Items[picker.SelectedIndex].Split(new char[] { ' ', ',' },
                                                             StringSplitOptions.RemoveEmptyEntries);
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

1 つの興味深いアプリケーション、`phase`パラメーターは、アニメーションにできます。 **アニメーションなる**ページがに似ていますが、 **Archimedean なる** ページの点を除いて、 [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs)クラスをアニメーション化、`phase`パラメーター。 ページには、アニメーションに別の方法も示しています。 前の例、 [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs)使用、`Task.Delay`アニメーションを制御するメソッド。 この例では代わりに、Xamarin.Forms`Device.Timer`メソッド。


```csharp
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
```

もちろん、実際にアニメーションを表示するプログラムを実行する必要があります。

[![](dots-images/animatedspiral-small.png "アニメーションのスパイラル ページのスクリーン ショットをトリプル")](dots-images/animatedspiral-large.png#lightbox "らせんをアニメーション化されるページの 3 倍になるスクリーン ショット")

これで線を描画して、パラメーターの式を使用して曲線を定義する方法を説明しました。 後で発行する」セクションが含まれる曲線のさまざまな種類をについて説明する`SKPath`をサポートしています。


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
