---
title: 行とストローク キャップ
description: 異なるストローク キャップを持つ線を描画する SkiaSharp を使用する方法をについてください。
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 24bf7bd7fb2aa51968a96bdbf808030604665c26
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
---
# <a name="lines-and-stroke-caps"></a>行とストローク キャップ

_異なるストローク キャップを持つ線を描画する SkiaSharp を使用する方法をについてください。_

SkiaSharp で 1 行の表示とは大きく異なります、一連の接続の直線を描画します。 でも単一の線を描画するときに、特定ストローク、および幅が広い、行、行を付与する必要がありますと呼ばれる、直線の終点の外観をより重要になります、*キャップのストローク*:

![](lines-images/strokecapsexample.png "次の 3 つのストローク cap オプション")

1 つの線を描画するため`SKCanvas`定義、単純な[ `DrawLine` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawLine/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/)メソッドの引数は、開始と終了値では、行の座標を示すため、`SKPaint`オブジェクト。

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

既定では、`StrokeWidth`新しくインスタンス化のプロパティ`SKPaint`オブジェクトが 0 で、太さで 1 ピクセルの線の表示では 1 の値と同じ効果がします。 これが表示される、電話などの高解像度のデバイスで薄すぎてのでを設定する可能性がありますが、`StrokeWidth`より大きい値にします。 別の問題を発生させるかなりの数の太さの線を描画を開始すると、: する必要がありますが開始されるとシック (thick) これらの線の端点の表示方法ですか?

開始および線の端の外観が呼び出された、*ライン キャップ*または Skia、*キャップのストローク*です。 このコンテキストでは、"cap"という単語は hat の種類を参照&mdash;行の末尾に位置するものです。 設定する、 [ `StrokeCap` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeCap/)のプロパティ、`SKPaint`オブジェクトの次のメンバーのいずれかを[ `SKStrokeCap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeCap/)列挙型。

- [`Butt`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Butt/) (既定)
- [`Square`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)

サンプル プログラムを使用してこれらは、最適な説明します。 ホーム ページの 2 番目のセクション、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)というタイトルのページでプログラムが始まる**ストローク Cap**に基づいて、 [ `StrokeCapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs)クラスです。 このページを定義、`PaintSurface`の 3 つのメンバーをループ処理するイベント ハンドラー、`SKStrokeCap`列挙体、列挙体メンバーの両方に名前を表示して、その線幅を使用して線を描画します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Center
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width / 2;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = textPaint.FontSpacing;

    foreach (SKStrokeCap strokeCap in Enum.GetValues(typeof(SKStrokeCap)))
    {
        // Display text
        canvas.DrawText(strokeCap.ToString(), xText, y, textPaint);
        y += textPaint.FontSpacing;

        // Display thick line
        thickLinePaint.StrokeCap = strokeCap;
        canvas.DrawLine(xLine1, y, xLine2, y, thickLinePaint);

        // Display thin line
        canvas.DrawLine(xLine1, y, xLine2, y, thinLinePaint);
        y += 2 * textPaint.FontSpacing;
    }
}
```

各メンバーに対して、`SKStrokeCap`列挙型、ハンドラーが 50 ピクセルと 2 ピクセルのストロークの太さの一番上に配置されている別の行のストロークの太さを持つ 2 つの行を描画します。 この 2 行目は、幾何学模様の開始と終了の線の太さと線の幅からの行を示すために対象としています。

[![](lines-images/strokecaps-small.png "キャップのストロークのページのスクリーン ショットをトリプル")](lines-images/strokecaps-large.png#lightbox "ストローク線端 ページのトリプル スクリーン ショット")

ご覧のように、`Square`と`Round`ストローク cap がストローク幅の半分だけ行の先頭に、最後にもう一度、行の長さを効果的に拡張します。 この拡張機能は、レンダリングされたグラフィック オブジェクトのサイズを決定する必要がある場合に重要になります。

`SKCanvas`クラスには少し変わった複数行を描画するための別のメソッドも含まれています。

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points`パラメーターは配列の`SKPoint`値および`mode`のメンバーである、 [ `SKPointMode` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPointMode/)列挙体は、次の 3 つのメンバーがあります。

- [`Points`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Points/) 個々 のポイントを表示するには
- [`Lines`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Lines/) ポイントの各ペアを接続するには
- [`Polygon`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Polygon/) すべての連続する点を接続するには

**複数行**ページは、このメソッドを示しています。 [ `MultipleLinesPage` XAML ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml)2 つのインスタンスを作成`Picker`できるビューのメンバーを選択して、`SKPointMode`列挙型でありのメンバー、`SKStrokeCap`列挙。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.MultipleLinesPage"
             Title="Multiple Lines">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="pointModePicker"
                Title="Point Mode"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Points</x:String>
                <x:String>Lines</x:String>
                <x:String>Polygon</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
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

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

`SelectedIndexChanged`両方のハンドラー`Picker`ビューを単に無効化、`SKCanvasView`オブジェクト。

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

このハンドラーは、の有無を確認する必要があります、`SKCanvasView`オブジェクト イベント ハンドラーは最初のためときに呼び出されます、`SelectedIndex`のプロパティ、 `Picker` XAML ファイルで 0 に設定されている前に発生して、`SKCanvasView`がインスタンス化されています。

`PaintSurface`ハンドラーから 2 つの選択した項目を取得するためのジェネリック メソッドにアクセスする、`Picker`ビューおよび列挙値に変換します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create an array of points scattered through the page
    SKPoint[] points = new SKPoint[10];

    for (int i = 0; i < 2; i++)
    {
        float x = (0.1f + 0.8f * i) * info.Width;

        for (int j = 0; j < 5; j++)
        {
            float y = (0.1f + 0.2f * j) * info.Height;
            points[2 * j + i] = new SKPoint(x, y);
        }
    }

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.DarkOrchid,
        StrokeWidth = 50,
        StrokeCap = GetPickerItem<SKStrokeCap>(strokeCapPicker)
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = GetPickerItem<SKPointMode>(pointModePicker);
    canvas.DrawPoints(pointMode, points, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }
    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}
```

このスクリーン ショットに、さまざまな`Picker`3 つのプラットフォームを選択します。

[![](lines-images/multiplelines-small.png "複数行のページのスクリーン ショットをトリプル")](lines-images/multiplelines-large.png#lightbox "複数行のページのトリプル スクリーン ショット")

左側には iPhone 方法、`SKPointMode.Points`列挙体メンバーにより`DrawPoints`内の地点のそれぞれを表示するために、`SKPoint`ライン キャップの場合、四角形配列`Butt`または`Square`です。 ライン キャップの場合、円が表示される`Round`です。

代わりに使用する場合`SKPointMode.Lines`センターでは、Android の画面に示すように、`DrawPoints`メソッドでは、直線を描画の各ペア間`SKPoint`値は、ここでは、指定した行 cap を使用して`Round`です。

UWP スクリーン ショットがの結果を表示、`SKPointMode.Polygon`値。 線を描画して、配列内の連続する点の間が非常に密接に確認する場合は、これらの行が接続されていないことが表示されます。 これらの個別の行のそれぞれで終わります指定ライン キャップ。 選択した場合、 `Round` cap を接続する線が表示されるが、実際に接続していません。

線を接続または接続されていないかどうかは、グラフィックス パスの作業の重要な側面です。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
