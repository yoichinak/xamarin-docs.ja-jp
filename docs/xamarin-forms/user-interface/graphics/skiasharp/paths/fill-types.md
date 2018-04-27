---
title: パスの塗りつぶしの種類
description: SkiaSharp パスの塗りつぶし型で実行できるさまざまな特殊効果を検出します。
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: d22ebf0e150c064835fa73765a65025f10ef4c2a
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
---
# <a name="the-path-fill-types"></a>パスの塗りつぶしの種類

_SkiaSharp パスの塗りつぶし型で実行できるさまざまな特殊効果を検出します。_

パス内の 2 つの輪郭が重なることができます、1 つの輪郭を構成する行が重複する可能性がします。 かっこで囲んだ任意領域は塗りつぶさ可能性があることができます、あっても取り込まれているすべての領域を塗りつぶすする可能性があります。 次に例を示します。

![](fill-types-images/filltypeexample.png "5 ポイント filles を部分的に星")

これを少しの制御があります。 いっぱいになるアルゴリズムを受ける、 [ `SKFillType` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.FillType/)プロパティ`SKPath`のメンバーに設定する、 [ `SKPathFillType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathFillType/)列挙。

- [`Winding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.Winding/)、既定値
- [`EvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.EvenOdd/)
- [`InverseWinding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseWinding/)
- [`InverseEvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseEvenOdd/)

ワインディングと偶の両方のアルゴリズムでは、任意の領域で囲まれたが入力または無限にその領域から描画される線で仮定に基づいて入力されていないかどうかを決定します。 その行は、パスを 1 つまたは複数の境界線を越えます。 モードでは、ワインディング、逆方向の領域に描画された直線の数を 1 つの方向のバランスに描画される境界線の数がない場合が入力されます。 それ以外の場合、領域は塗りつぶされます。 偶アルゴリズムは、境界の行の数が奇数の場合、領域を塗りつぶします。

多くの日常的なパスを持つワインディング アルゴリズムは多くの場合、パスの含まれているすべての領域を塗りつぶします。 一般に、偶アルゴリズムには、さらに興味深い結果が生成されます。

典型的な例は、5 ポイントの星で示したように、 **Five-Pointed スター**ページ。 [FivePointedStarPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FivePointedStarPage.xaml)ファイルでは、2 つをインスタンス化`Picker`パスを選択するビューは入力型とパスを描画または入力するかどうかまたは両方とどのような順序で。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.FivePointedStarPage"
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
            <Picker.Items>
                <x:String>Winding</x:String>
                <x:String>EvenOdd</x:String>
                <x:String>InverseWinding</x:String>
                <x:String>InverseEvenOdd</x:String>
            </Picker.Items>
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
            <Picker.Items>
                <x:String>Fill only</x:String>
                <x:String>Stroke only</x:String>
                <x:String>Stroke then Fill</x:String>
                <x:String>Fill then Stroke</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

分離コード ファイルを使用して両方`Picker`5 ポイントの星を描画する値。

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
        FillType = (SKPathFillType)Enum.Parse(typeof(SKPathFillType),
                        fillTypePicker.Items[fillTypePicker.SelectedIndex])
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

    switch (drawingModePicker.SelectedIndex)
    {
        case 0:
            canvas.DrawPath(path, fillPaint);
            break;

        case 1:
            canvas.DrawPath(path, strokePaint);
            break;

        case 2:
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case 3:
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

通常、パスの塗りつぶしの種類には影響はのみですがいっぱいになったと線ではありませんが、2 つ`Inverse`モードは、塗りつぶしとストロークの両方に影響します。 いっぱいになった 2 つの`Inverse`型領域を塗りつぶす oppositely 星の外側の領域が入力されるようにします。 2 つのストロークの`Inverse`色の線を除くすべての種類。 これら逆フィルの型を使用すると、iOS スクリーン ショットに示すようにいくつか奇数効果を生成できます。

[![](fill-types-images/fivepointedstar-small.png "Five-Pointed スター ページのスクリーン ショットをトリプル")](fill-types-images/fivepointedstar-large.png#lightbox "Five-Pointed スター ページのトリプル スクリーン ショット")

Android および UWP スクリーン ショットでは、一般的な偶ワインディングとの効果を表示するには塗りつぶし、ストロークの順序は、結果も影響します。

ワインディング アルゴリズムは、線を描画する方向に左右されます。 通常、パスを作成している場合、制御できますその方向別に行が 1 つのポイントから描画されることを指定します。 ただし、`SKPath`クラスもなどのメソッドを定義`AddRect`と`AddCircle`全体の輪郭を描画します。 これらのオブジェクトの描画方法を制御するには、メソッドは型のパラメーターを含めます。 [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/)、2 つのメンバーを持ちます。

- [`Clockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.Clockwise/)
- [`CounterClockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.CounterClockwise/)

メソッドは、`SKPath`が含まれ、`SKPathDirection`パラメーター付けます、既定値は`Clockwise`します。

**重なり合う円**ページ偶パスの塗りつぶし型の 4 つの重なり合う円にパスを作成します。

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

最小限のコードで作成した興味深いイメージです。

[![](fill-types-images/overlappingcircles-small.png "円の重複するページのスクリーン ショットをトリプル")](fill-types-images/overlappingcircles-large.png#lightbox "円の重複するページのトリプル スクリーン ショット")


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
