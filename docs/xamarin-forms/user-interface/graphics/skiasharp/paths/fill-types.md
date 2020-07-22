---
title: パスの塗りつぶしの種類
description: この記事では、SkiaSharp path fill 型で可能なさまざまな効果について説明し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c8c54f3d3815e418d2f71960dc7733711cb40ae2
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139049"
---
# <a name="the-path-fill-types"></a>パスの塗りつぶしの種類

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp path fill 型で可能なさまざまな効果を発見する_

パスの2つの輪郭が重なり、1つの輪郭を構成する線が重なり合うことがあります。 囲まれた領域はすべて塗りつぶされる可能性がありますが、囲まれたすべての領域を塗りつぶす必要はありません。 次に例を示します。

![](fill-types-images/filltypeexample.png "Five-pointed star partially filles")

これについては、簡単に制御できます。 入力アルゴリズムは、 [`SKFillType`](xref:SkiaSharp.SKPath.FillType) `SKPath` 列挙体のメンバーに設定したのプロパティによって管理され [`SKPathFillType`](xref:SkiaSharp.SKPathFillType) ます。

- `Winding` (既定値)
- `EvenOdd`
- `InverseWinding`
- `InverseEvenOdd`

ワインディングアルゴリズムと偶数奇数アルゴリズムは、囲まれた領域が塗りつぶされるか、またはその領域から無限大に描画された架空の線に基づいて塗りつぶされるかを決定します。 この線は、パスを構成する1つ以上の境界線と交差します。 ワインディングモードでは、ある方向に描画される境界線の数が、もう一方の方向に描画される直線の数とのバランスを取る場合、領域は塗りつぶされません。 それ以外の場合、領域は塗りつぶされます。 境界線の数が奇数の場合、偶数のアルゴリズムは領域を塗りつぶします。

多くのルーチンパスでは、ワインディングアルゴリズムによって、パスのすべての囲まれた領域がいっぱいになることがよくあります。 偶数の奇数のアルゴリズムでは、通常、より興味深い結果が生成されます。

クラシックの例は、5つの**星**のページで説明されているように、星5つの星です。 [**Fivepointedstarpage .xaml**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FivePointedStarPage.xaml)ファイルは、2つのビューをインスタンス化して `Picker` パスの塗りつぶしの種類を選択し、パスがストロークされるか、または塗りつぶされるか、またはその両方を順番に指定します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.FivePointedStarPage"
             Title="Five-Pointed Star">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="fillTypePicker"
                Title="Path Fill Type"
                Grid.Row="0"
                Grid.Column="0"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPathFillType}">
                    <x:Static Member="skia:SKPathFillType.Winding" />
                    <x:Static Member="skia:SKPathFillType.EvenOdd" />
                    <x:Static Member="skia:SKPathFillType.InverseWinding" />
                    <x:Static Member="skia:SKPathFillType.InverseEvenOdd" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="drawingModePicker"
                Title="Drawing Mode"
                Grid.Row="0"
                Grid.Column="1"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Fill only</x:String>
                    <x:String>Stroke only</x:String>
                    <x:String>Stroke then Fill</x:String>
                    <x:String>Fill then Stroke</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2"
                                PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

分離コードファイルは、両方の値を使用して、 `Picker` 次の5つの星を描画します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPath path = new SKPath
    {
        FillType = (SKPathFillType)fillTypePicker.SelectedItem
    };
    path.MoveTo(info.Width / 2, info.Height / 2 - radius);

    for (int i = 1; i < 5; i++)
    {
        // angle from vertical
        double angle = i * 4 * Math.PI / 5;
        path.LineTo(center + new SKPoint(radius * (float)Math.Sin(angle),
                                        -radius * (float)Math.Cos(angle)));
    }
    path.Close();

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 50,
        StrokeJoin = SKStrokeJoin.Round
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    switch ((string)drawingModePicker.SelectedItem)
    {
        case "Fill only":
            canvas.DrawPath(path, fillPaint);
            break;

        case "Stroke only":
            canvas.DrawPath(path, strokePaint);
            break;

        case "Stroke then Fill":
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case "Fill then Stroke":
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

通常、パスの塗りつぶしの種類は塗りつぶしにのみ影響し、ストロークは影響しませんが、2つの `Inverse` モードは塗りつぶしとストロークの両方に影響します。 塗りつぶしの場合、2つの型は、 `Inverse` 星の外側の領域が塗りつぶされるように、塗りつぶし領域を oppositely ます。 ストロークの場合、この 2 `Inverse` 種類の色はストロークを除くすべての色になります。 これらの逆フィルの種類を使用すると、iOS のスクリーンショットに示すように、いくつかの奇妙な効果が生じる可能性があります。

[![](fill-types-images/fivepointedstar-small.png "Triple screenshot of the Five-Pointed Star page")](fill-types-images/fivepointedstar-large.png#lightbox "Triple screenshot of the Five-Pointed Star page")

Android のスクリーンショットには、通常の奇数とワインディングの効果が示されていますが、ストロークと塗りつぶしの順序も結果に影響します。

ワインディングアルゴリズムは、線が描画される方向に依存します。 通常、パスを作成するときは、線がある点から別の点へと描画されるように指定するときに、その方向を制御できます。 ただし、クラスは、 `SKPath` `AddRect` 輪郭全体を描画するやなどのメソッドも定義し `AddCircle` ます。 これらのオブジェクトを描画する方法を制御するために、メソッドには、2つのメンバーを持つ型のパラメーターが含まれて [`SKPathDirection`](xref:SkiaSharp.SKPathDirection) います。

- `Clockwise`
- `CounterClockwise`

パラメーターを含むのメソッドでは、 `SKPath` `SKPathDirection` 既定値のが指定さ `Clockwise` れます。

**重なった円**ページにより、偶数の円で始まるパスが作成されます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(info.Width, info.Height) / 4;

    SKPath path = new SKPath
    {
        FillType = SKPathFillType.EvenOdd
    };

    path.AddCircle(center.X - radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X - radius / 2, center.Y + radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y + radius / 2, radius);

    SKPaint paint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    canvas.DrawPath(path, paint);

    paint.Style = SKPaintStyle.Stroke;
    paint.StrokeWidth = 10;
    paint.Color = SKColors.Magenta;

    canvas.DrawPath(path, paint);
}
```

これは、最小限のコードで作成された興味深いイメージです。

[![](fill-types-images/overlappingcircles-small.png "Triple screenshot of the Overlapping Circles page")](fill-types-images/overlappingcircles-large.png#lightbox "Triple screenshot of the Overlapping Circles page")

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
