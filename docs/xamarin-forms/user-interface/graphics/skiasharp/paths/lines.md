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

_SkiaSharp を使用して異なるストロークキャップの線を描画する方法について説明します。_

SkiaSharp、1 行のレンダリングは、一連の接続された直線のレンダリングと大きく異なります。 1 つの線を描画するには場合でも、ことが多い行は、特定のストロークの幅を提供するために必要です。 これらの行の幅になると、行の最後の外観も重要になります。 線の端の外観は、*ストロークキャップ*と呼ばれます。

![](lines-images/strokecapsexample.png "The three stroke caps options")

単一行を描画する場合、`SKCanvas` は、`SKPaint` オブジェクトを使用して行の開始座標と終了座標を示す引数を持つ単純な[`DrawLine`](xref:SkiaSharp.SKCanvas.DrawLine(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint))メソッドを定義します。

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

既定では、新しくインスタンス化された `SKPaint` オブジェクトの [ [`StrokeWidth`](xref:SkiaSharp.SKPaint.StrokeWidth) ] プロパティは0です。これは、1つのピクセルの線を太さで描画する場合の値1と同じ効果があります。 これは携帯電話などの高解像度のデバイスでは非常に薄く見えます。そのため、`StrokeWidth` を大きな値に設定することをお勧めします。 別の問題が発生しますがかなり大きな太さの線を描画を開始すると: を開始し、これら太い線の両端表示する方法でしょうか。

線の始点と終点の外観は、*直線キャップ*、つまり skia では*ストロークキャップ*と呼ばれます。 このコンテキストの "cap" という単語は、行の末尾にあるもの &mdash; hat の一種を指します。 `SKPaint` オブジェクトの[`StrokeCap`](xref:SkiaSharp.SKPaint.StrokeCap)プロパティを、 [`SKStrokeCap`](xref:SkiaSharp.SKStrokeCap)列挙体の次のいずれかのメンバーに設定します。

- `Butt` (既定値)
- `Square`
- `Round`

サンプル プログラムでこれらを最適に示します。 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)プログラムの**SkiaSharp Lines and Paths**セクションは、 [`StrokeCapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeCapsPage.cs)クラスに基づく**Stroke Caps**という名前のページから始まります。 このページでは、`SKStrokeCap` 列挙体の3つのメンバーをループ処理し、列挙体のメンバーの名前とそのストロークキャップを使用して線を描画する `PaintSurface` イベントハンドラーを定義します。

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

`SKStrokeCap` 列挙体の各メンバーに対して、ハンドラーは2つの線を描画します。1つは50ピクセルのストロークの太さで、もう1つはストロークの太さが2ピクセルの上に位置します。 この 2 行目は、幾何学の開始と線の太さとストローク キャップの独立した行の末尾を示すために対象としています。

[![](lines-images/strokecaps-small.png "Triple screenshot of the Stroke Caps page")](lines-images/strokecaps-large.png#lightbox "Triple screenshot of the Stroke Caps page")

ご覧のように、`Square` と `Round` ストロークキャップを使用すると、行の先頭と末尾にあるストロークの幅の半分によって線の長さを効果的に拡張できます。 この拡張機能は、レンダリングされたグラフィック オブジェクトのサイズを決定する必要がある場合に重要になります。

`SKCanvas` クラスには、やや独特な複数の行を描画するための別のメソッドも含まれています。

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points` パラメーターは `SKPoint` 値の配列で、`mode` は、次の3つのメンバーを持つ[`SKPointMode`](xref:SkiaSharp.SKPointMode)列挙型のメンバーです。

- 個々の点を表示する `Points`
- ポイントの各ペアを接続するための `Lines`
- すべての連続するポイントを接続するための `Polygon`

**[複数行]** ページでは、このメソッドを示しています。 [**乗数 Elinespage .xaml**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/MultipleLinesPage.xaml)ファイルは、`SKPointMode` 列挙体のメンバーと `SKStrokeCap` 列挙体のメンバーを選択できる2つの `Picker` ビューをインスタンス化します。

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

SkiaSharp 名前空間宣言は、`SKPointMode` と `SKStrokeCap` 列挙型のメンバーを参照するために `SkiaSharp` 名前空間が必要であるため、少し異なることに注意してください。 両方の `Picker` ビューの `SelectedIndexChanged` ハンドラーは、`SKCanvasView` オブジェクトを無効にするだけです。

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

このハンドラーは、`SKCanvasView` オブジェクトが存在するかどうかを確認する必要があります。これは、XAML ファイルで `Picker` の `SelectedIndex` プロパティが0に設定されていて、`SKCanvasView` がインスタンス化される前に発生する場合に、イベントハンドラーが最初に呼び出されるためです。

`PaintSurface` ハンドラーは、`Picker` ビューから2つの列挙値を取得します。

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

スクリーンショットには、さまざまな `Picker` 選択が示されています。

[![](lines-images/multiplelines-small.png "Triple screenshot of the Multiple Lines page")](lines-images/multiplelines-large.png#lightbox "Triple screenshot of the Multiple Lines page")

左側の iPhone は、`SKPointMode.Points` 列挙型の `DrawPoints` メンバーが、ラインキャップが `Butt` または `Square`の場合に `SKPoint` 配列内の各点を正方形としてレンダリングする方法を示しています。 線のキャップが `Round`場合、円が表示されます。

Android のスクリーンショットは、`SKPointMode.Lines`の結果を示しています。 `DrawPoints` メソッドは、指定されたラインキャップ (この場合は `Round`) を使用して `SKPoint` 値の各ペアの間に線を描画します。

代わりに `SKPointMode.Polygon`を使用すると、配列内の連続する点の間に線が描画されますが、非常によく似ている場合は、これらの行が接続されていないことがわかります。 これらの個別の行の各は、開始し、指定のライン キャップで終了します。 `Round` キャップを選択すると、線が接続されているように見えますが、実際には接続されていません。

線を接続または接続されていないかどうかは、グラフィックス パスの作業の重要な側面です。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
