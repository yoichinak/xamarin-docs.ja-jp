---
title: "ドットとダッシュ"
description: "マスターの SkiaSharp で点線および破線を描画の複雑さ"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 32326eb472b1bb8b4fbaf4066edc36255f5948ca
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="dots-and-dashes"></a>ドットとダッシュ

_マスターの SkiaSharp で点線および破線を描画の複雑さ_

SkiaSharp を使用して、純色ではない代わりに、ドットとダッシュで構成されている線を描画できます。

![](dots-images/dottedlinesample.png "点線")

これを行う、*パス効果*のインスタンスは、 [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/)を設定するクラス、 [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/)プロパティの`SKPaint`します。 パスを作成することができます (または結合パス複数のエフェクト) 静的`Create`によって定義されたメソッド`SKPathEffect`です。

使用する点線または破線を描画する、 [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/)静的メソッドです。 2 つの引数がある: 最初の配列は、この`float`ドットとダッシュの長さとそれらの間のスペースの長さを示す値。 この配列は、要素の偶数をいる必要がありには、少なくとも 2 つの要素が存在する必要があります。 (がありますが、配列の要素がゼロ、実線の結果である。)2 つの要素がある場合は、1 つは、ピリオドまたはダッシュの長さ、2 番目の間隔の長さ [次へ] のピリオドまたはダッシュの前にします。 3 つ以上の要素があるかどうか、この順序で、: ダッシュと長さ、ギャップの長さ、ダッシュの長さ、ギャップの長さ。

一般に、ダッシュと間隔の長さをストロークの幅の倍数にします。 ストロークの幅が 10 ピクセルの場合は、たとえば、し、配列 {10, 10} を描画点線、ドットとのギャップが同じ長さのストロークの太さとします。

ただし、`StrokeCap`の設定、`SKPaint`オブジェクトは、これらのドットとダッシュにも影響します。 後ほどと、影響を与えるをこの配列の要素を持ちます。

ドットで区切られたおよびで破線が示されている、**点線および破線**ページ。 [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml)ファイルでは、2 つをインスタンス化`Picker`線幅および dash 配列を選択する、2 つ目を選択することのいずれかを表示します。

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

内の最初の 3 つの項目、`dashArrayPicker`ストロークの幅が 10 ピクセルであると仮定します。 {10, 10}、点線の場合は、配列 {30, 10} は破線の行と {10、10、30, 10} 点鎖線には、します。 (他の 3 つ、後ほど。)

[ `DotsAndDashesPage`分離コード ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs)が含まれています、`PaintSurface`イベント ハンドラーと、いくつかヘルパー ルーチンにアクセスするための`Picker`ビュー。

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

次のスクリーン ショットでは、左端の iOS の画面には、点線が表示されます。

[![](dots-images/dotsanddashes-small.png "ドットとダッシュのページのスクリーン ショットをトリプル")](dots-images/dotsanddashes-large.png#lightbox "ドットとダッシュのページのトリプル スクリーン ショット")

ただし、Android の画面は {10, 10} の配列を使用して、点線を表示する必要がありますも行は純色です。 代わりにします。 どうなっているのでしょうか。 問題は、Android 画面も、ストローク cap の設定は`Square`します。 これは、線幅の半分だけ、発生すると、ギャップを埋めるすべてのダッシュを拡張します。

線幅を使用する場合、この問題を回避する`Square`または`Round`、(場合によってその結果、dash 長さ 0 の)、ストロークの長さで、配列でダッシュの長さを小さくして、ストロークの長さによって間隔の長さを増やす必要があります。 これは、最後の 3 つが内の配列をダッシュ方法、`Picker`計算された XAML ファイルで。

- {10, 10} が {0, 20} 点線の場合
- {30, 10} なります {20、20} 破線の
- {10、10、30, 10} {0、20、20、20} が点線および破線の行の

ドットで区切られた、ストロークの破線 Windows 画面表示のキャップの`Round`します。 `Round`多くの場合、キャップのストロークは太線でドットとダッシュの最適な外観を与えます。

これまで説明が行われないに 2 番目のパラメーターの`SKPathEffect.CreateDash`メソッドです。 このパラメーターの名前は`phase`あり、行の先頭のドットとダッシュのパターン内のオフセットを意味します。 たとえば、dash 配列がある場合 {10, 10} と`phase`は 10 ですが、行が、ドットではなく、ギャップが開始されます。

1 つの興味深いアプリケーション、`phase`パラメーターは、アニメーションでします。 **アニメーションなる**ページがに似ていますが、 **Archimedean なる** ページの点を除いて、 [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs)クラスをアニメーション化、`phase`パラメーター。 ページには、アニメーションを別の方法も示しています。 前の例、 [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs)使用、`Task.Delay`アニメーションを制御します。 この例では代わりに、Xamarin.Forms`Device.Timer`メソッド。


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

もちろん、実際には、アニメーションを表示するプログラムを実行する必要があります。

[![](dots-images/animatedspiral-small.png "アニメーションなるページのスクリーン ショットをトリプル")](dots-images/animatedspiral-large.png#lightbox "アニメーションなるページのトリプル スクリーン ショット")

パラメーター型方程式を使用して曲線を定義する線を描画する方法を確認したようになりました。 後で発行するためのセクションが含まれる曲線のさまざまな種類をカバーする`SKPath`をサポートしています。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
