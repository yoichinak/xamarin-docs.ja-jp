---
title: 線とストローク キャップ
description: この記事では、SkiaSharp を使用して、アプリケーションでさまざまなストロークキャップを持つ線を描画する方法について説明 Xamarin.Forms し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 644e6ab4acffa7acf2d86733d68fed8db07a752a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86932367"
---
# <a name="lines-and-stroke-caps"></a>線とストローク キャップ

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp を使用して異なるストロークキャップの線を描画する方法について説明します。_

SkiaSharp では、単一行のレンダリングは、接続された一連の直線を描画することとは大きく異なります。 ただし、1つの線を描画する場合でも、多くの場合、線に特定のストロークの幅を付ける必要があります。 これらの行の幅が広くなるにつれて、線の端の外観も重要になります。 線の端の外観は、*ストロークキャップ*と呼ばれます。

![3つのストロークキャップオプション](lines-images/strokecapsexample.png)

単一行を描画するために、は、 `SKCanvas` [`DrawLine`](xref:SkiaSharp.SKCanvas.DrawLine(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) オブジェクトを使用して、直線の開始座標と終了座標を示す引数を持つ単純なメソッドを定義し `SKPaint` ます。

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

既定では、 [`StrokeWidth`](xref:SkiaSharp.SKPaint.StrokeWidth) 新しくインスタンス化されたオブジェクトのプロパティ `SKPaint` は0であり、1ピクセルの線を太さで描画する場合の値1と同じ効果があります。 これは携帯電話などの高解像度のデバイスでは非常に薄く見えます。そのため、をより大きな値に設定することをお勧めし `StrokeWidth` ます。 しかし、サイズが非常に大きい線の描画を開始すると、別の問題が発生します。このような太い線の開始と終了はどのようにレンダリングされますか。

線の始点と終点の外観は、*直線キャップ*、つまり skia では*ストロークキャップ*と呼ばれます。 このコンテキストの "cap" という単語は、 &mdash; 行の末尾にある一種の hat を指します。 [`StrokeCap`](xref:SkiaSharp.SKPaint.StrokeCap)オブジェクトのプロパティは、 `SKPaint` 列挙体の次のいずれかのメンバーに設定し [`SKStrokeCap`](xref:SkiaSharp.SKStrokeCap) ます。

- `Butt`(既定値)
- `Square`
- `Round`

これらの例は、サンプルプログラムを使用して説明します。 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)プログラムの**SkiaSharp Lines and Paths**セクションは、クラスに基づく**Stroke Caps**というタイトルのページから始まり [`StrokeCapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeCapsPage.cs) ます。 このページでは、列挙 `PaintSurface` 体の3つのメンバーをループ処理 `SKStrokeCap` し、列挙体のメンバーの名前とそのストロークキャップを使用して線を描画するイベントハンドラーを定義します。

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

ハンドラーは、列挙体のメンバーごとに2つの `SKStrokeCap` 線を描画します。1つは50ピクセルのストロークの太さで、もう1つはストロークの太さが2ピクセルの上に位置します。 この2番目の行は、線の太さとストロークキャップに依存しない、線の始点と終点を示すことを目的としています。

[![[ストロークキャップ] ページのトリプルスクリーンショット](lines-images/strokecaps-small.png)](lines-images/strokecaps-large.png#lightbox "[ストロークキャップ] ページのトリプルスクリーンショット")

ご覧のように、とストロークキャップは、行の `Square` `Round` 先頭と末尾にあるストロークの幅の半分を使用して、線の長さを効果的に拡張します。 この拡張機能は、レンダリングされたグラフィックスオブジェクトの大きさを判断する必要がある場合に重要になります。

`SKCanvas`クラスには、やや独特な複数の行を描画するための別のメソッドも含まれています。

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points`パラメーターは値の配列であり、次の `SKPoint` `mode` [`SKPointMode`](xref:SkiaSharp.SKPointMode) 3 つのメンバーを持つ列挙体のメンバーです。

- `Points`個々の点を表示するには
- `Lines`ポイントの各ペアを接続するには
- `Polygon`すべての連続するポイントを接続するには

[**複数行**] ページでは、このメソッドを示しています。 [**乗数 Elinespage .xaml**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/MultipleLinesPage.xaml)ファイルは、 `Picker` `SKPointMode` 列挙体のメンバーと列挙体のメンバーを選択できる2つのビューをインスタンス化し `SKStrokeCap` ます。

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

SkiaSharp 名前空間宣言は、 `SkiaSharp` および列挙型のメンバーを参照するために名前空間が必要であるため、少し異なることに注意して `SKPointMode` `SKStrokeCap` ください。 `SelectedIndexChanged`両方のビューのハンドラーは、 `Picker` 単純にオブジェクトを無効にし `SKCanvasView` ます。

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

このハンドラーは、オブジェクトの存在を確認する必要があり `SKCanvasView` `SelectedIndex` ます。これは、のプロパティ `Picker` が XAML ファイルで0に設定され、がインスタンス化される前に発生した場合に、イベントハンドラーが最初に呼び出されるためです `SKCanvasView` 。

この `PaintSurface` ハンドラーは、ビューから2つの列挙値を取得し `Picker` ます。

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

スクリーンショットには、次のようなさまざまな選択項目が表示され `Picker` ます。

[![[複数行] ページのトリプルスクリーンショット](lines-images/multiplelines-small.png)](lines-images/multiplelines-large.png#lightbox "[複数行] ページのトリプルスクリーンショット")

左側の iPhone は、 `SKPointMode.Points` `DrawPoints` 行キャップがまたはの場合に、列挙体のメンバーが配列内の各点を四角形としてレンダリングする方法を示して `SKPoint` `Butt` `Square` います。 線のキャップがの場合は、円がレンダリングされ `Round` ます。

Android のスクリーンショットは、の結果を示して `SKPointMode.Lines` います。 メソッドは、 `DrawPoints` `SKPoint` 指定されたラインキャップ (この場合は) を使用して、値の各ペアの間に線を描画し `Round` ます。

代わりにを使用する場合、 `SKPointMode.Polygon` 配列内の連続する点の間に線が描画されますが、非常によく似ている場合は、これらの行が接続されていないことがわかります。 これらのそれぞれの行は、指定された行キャップで開始および終了します。 キャップを選択した場合、 `Round` 線は接続されているように見えますが、実際には接続されていません。

線が接続されているかどうかは、グラフィックスパスを操作する上で重要な要素です。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
