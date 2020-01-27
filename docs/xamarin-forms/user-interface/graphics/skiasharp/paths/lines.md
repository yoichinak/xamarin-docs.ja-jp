---
title: 線とストローク キャップ
description: この記事では、SkiaSharp を使用して、Xamarin.Forms アプリケーションで異なるストローク キャップを持つ行を描画する方法を説明し、サンプル コードを示します。
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 9aaecb8c63ff28111097dce81954f523b4c7731b
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725213"
---
# <a name="lines-and-stroke-caps"></a>線とストローク キャップ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp を使用して異なるストローク キャップを持つ行を描画する方法について説明します_

SkiaSharp、1 行のレンダリングは、一連の接続された直線のレンダリングと大きく異なります。 1 つの線を描画するには場合でも、ことが多い行は、特定のストロークの幅を提供するために必要です。 これらの行の幅になると、行の最後の外観も重要になります。 行の末尾の外観と呼ばれる、*ストローク キャップ*:

![](lines-images/strokecapsexample.png "The three stroke caps options")

1 つの行を描画するため`SKCanvas`定義、単純な[ `DrawLine` ](xref:SkiaSharp.SKCanvas.DrawLine(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint))メソッドの引数は、開始日と終了では、行の座標を指定、`SKPaint`オブジェクト。

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

既定で、 [ `StrokeWidth` ](xref:SkiaSharp.SKPaint.StrokeWidth)新しくインスタンス化のプロパティ`SKPaint`オブジェクトが 0 で、太さで 1 ピクセルの行の表示では 1 の値と同じ効果があります。 これは表示を設定する可能性がありますので、携帯電話などの高解像度のデバイスで非常に軽量、`StrokeWidth`より大きい値にします。 別の問題が発生しますがかなり大きな太さの線を描画を開始すると: を開始し、これら太い線の両端表示する方法でしょうか。

開始と終了行の外観と呼ばれる、*ライン キャップ*または Skia、*ストローク キャップ*します。 このコンテキストでは、「上限」という単語がある種の hat を指す&mdash;行の末尾に位置するものです。 設定する、 [ `StrokeCap` ](xref:SkiaSharp.SKPaint.StrokeCap)のプロパティ、`SKPaint`オブジェクトの次のメンバーのいずれかに、 [ `SKStrokeCap` ](xref:SkiaSharp.SKStrokeCap)列挙体。

- `Butt` (既定値)
- `Square`
- `Round`

サンプル プログラムでこれらを最適に示します。 **SkiaSharp の線とパス**のセクション、 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)というタイトルでページとプログラムの開始**ストローク キャップ**に基づいて、[ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeCapsPage.cs)クラス。 このページを定義、`PaintSurface`の 3 つのメンバーをループ処理するイベント ハンドラー、`SKStrokeCap`列挙体、列挙型メンバーの両方の名前を表示して、そのストローク キャップを使用して線を描画します。

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

各メンバーに対して、`SKStrokeCap`列挙型で、ハンドラーはストロークの太さを 50 ピクセルと 2 つのピクセルのストロークの太さで一番上に配置されている別の行のいずれか 2 つの行を描画します。 この 2 行目は、幾何学の開始と線の太さとストローク キャップの独立した行の末尾を示すために対象としています。

[![](lines-images/strokecaps-small.png "Triple screenshot of the Stroke Caps page")](lines-images/strokecaps-large.png#lightbox "Triple screenshot of the Stroke Caps page")

ご覧のとおり、`Square`と`Round`ストローク キャップはストローク幅の半分だけ行の先頭に、最後にもう一度、行の長さを効果的に拡張します。 この拡張機能は、レンダリングされたグラフィック オブジェクトのサイズを決定する必要がある場合に重要になります。

`SKCanvas`クラスにはやや特殊な複数の行を描画するための別のメソッドも含まれています。

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points`パラメーターが配列の`SKPoint`値と`mode`のメンバーである、 [ `SKPointMode` ](xref:SkiaSharp.SKPointMode)を 3 つのメンバーを持つ列挙型。

- `Points` 個々 のポイントを表示するには
- `Lines` ポイントの各ペアを接続するには
- `Polygon` すべての連続するポイントを接続するには

**複数行**ページは、この方法を示します。 [ **MultipleLinesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/MultipleLinesPage.xaml)ファイルでは、2 つのインスタンス化します`Picker`できるビューのメンバーの選択、`SKPointMode`列挙体のメンバーと、`SKStrokeCap`列挙体。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.MultipleLinesPage"
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
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPointMode}">
                    <x:Static Member="skia:SKPointMode.Points" />
                    <x:Static Member="skia:SKPointMode.Lines" />
                    <x:Static Member="skia:SKPointMode.Polygon" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
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

        <skiaforms:SKCanvasView x:Name="canvasView"
                                PaintSurface="OnCanvasViewPaintSurface"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

SkiaSharp 名前空間の宣言が少し異なることに注意してください、`SkiaSharp`名前空間がのメンバーを参照するために必要な`SKPointMode`と`SKStrokeCap`列挙体。 `SelectedIndexChanged`両方のハンドラー`Picker`ビューを単に無効化、`SKCanvasView`オブジェクト。

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

このハンドラーは、の有無を確認する必要があります、`SKCanvasView`オブジェクトはイベント ハンドラーは最初のためというときに、`SelectedIndex`のプロパティ、 `Picker` 、XAML ファイルで 0 に設定されて前に発生して、`SKCanvasView`がインスタンス化されました。

`PaintSurface`ハンドラーから 2 つの列挙値の取得、`Picker`ビュー。

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
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = (SKPointMode)pointModePicker.SelectedItem;
    canvas.DrawPoints(pointMode, points, paint);
}
```

スクリーン ショットは、表示のさまざまな`Picker`の選択。

[![](lines-images/multiplelines-small.png "Triple screenshot of the Multiple Lines page")](lines-images/multiplelines-large.png#lightbox "Triple screenshot of the Multiple Lines page")

左側には iPhone 方法、`SKPointMode.Points`列挙体メンバーにより`DrawPoints`の各ポイントで表示するために、`SKPoint`ライン キャップがある場合は、四角形として配列`Butt`または`Square`します。 ライン キャップがある場合、円が表示される`Round`します。

Android のスクリーンショットは、`SKPointMode.Lines`の結果を示しています。 `DrawPoints` メソッドは、指定されたラインキャップ (この場合は `Round`) を使用して `SKPoint` 値の各ペアの間に線を描画します。

代わりに `SKPointMode.Polygon`を使用すると、配列内の連続する点の間に線が描画されますが、非常によく似ている場合は、これらの行が接続されていないことがわかります。 これらの個別の行の各は、開始し、指定のライン キャップで終了します。 選択した場合、 `Round` cap、接続されている行が表示されるが、実際に接続していません。

線を接続または接続されていないかどうかは、グラフィックス パスの作業の重要な側面です。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
