---
title: Blend の分離モード
description: Blend の分離モードを使用すると、赤、緑、および青の色を変更できます。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 66D1A537-A247-484E-B5B9-FBCB7838FBE9
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 594e98230d4f4bd8aca27f92f4544f8c59b5f0a2
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061456"
---
# <a name="the-separable-blend-modes"></a>Blend の分離モード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

この記事で示した[ **SkiaSharp の Porter Duff ブレンド モード**](porter-duff.md)、Porter Duff blend モードは、一般にクリッピングの操作を実行します。 Blend の分離モードは異なります。 分離モードでは、イメージの個々 の赤、緑、および青の色のコンポーネントを変更します。 Blend の分離モードでは、赤、緑、青の組み合わせが本当に白いことを示すための色を混在させることが。

![色の原色](separable-images/SeparableSample.png "色の原色")

## <a name="lighten-and-darken-two-ways"></a>明るくして、2 つの方法を暗くします。 

通常、ビットマップがいくらかを暗すぎる、または明るすぎます。 Blend の分離モードは、明るく/暗く、imabe を使用できます。  実際には、2 つの分離の blend モードの[ `SKBlendMode` ](xref:SkiaSharp.SKBlendMode)列挙体の名前は`Lighten`と`Darken`します。 

これら 2 つのモードがで示されています、**明るくと暗く**ページ。 XAML ファイルでは、2 つのインスタンス化します`SKCanvasView`オブジェクトと 2 つ`Slider`ビュー。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.LightenAndDarkenPage"
             Title="Lighten and Darken">
    <StackLayout>
        <skia:SKCanvasView x:Name="lightenCanvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="lightenSlider"
                Margin="10"
                ValueChanged="OnSliderValueChanged" />

        <skia:SKCanvasView x:Name="darkenCanvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="darkenSlider"
                Margin="10"
                ValueChanged="OnSliderValueChanged" />
    </StackLayout>
</ContentPage>
```

最初の`SKCanvasView`と`Slider`デモンストレーション`SKBlendMode.Lighten`し、2 番目のペアを示します`SKBlendMode.Darken`します。 2 つ`Slider`ビューでは、同じ共有`ValueChanged`ハンドラー、および 2 つ`SKCanvasView`同じ共有`PaintSurface`ハンドラー。 オブジェクトが両方のイベント ハンドラーのチェックがイベントを発生させます。

```csharp
public partial class LightenAndDarkenPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                typeof(SeparableBlendModesPage),
                "SkiaSharpFormsDemos.Media.Banana.jpg");

    public LightenAndDarkenPage ()
    {
        InitializeComponent ();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if ((Slider)sender == lightenSlider)
        {
            lightenCanvasView.InvalidateSurface();
        }
        else
        {
            darkenCanvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find largest size rectangle in canvas
        float scale = Math.Min((float)info.Width / bitmap.Width,
                               (float)info.Height / bitmap.Height);
        SKRect rect = SKRect.Create(scale * bitmap.Width, scale * bitmap.Height);
        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Display bitmap
        canvas.DrawBitmap(bitmap, rect);

        // Display gray rectangle with blend mode
        using (SKPaint paint = new SKPaint())
        {
            if ((SKCanvasView)sender == lightenCanvasView)
            {
                byte value = (byte)(255 * lightenSlider.Value);
                paint.Color = new SKColor(value, value, value);
                paint.BlendMode = SKBlendMode.Lighten;
            }
            else
            {
                byte value = (byte)(255 * (1 - darkenSlider.Value));
                paint.Color = new SKColor(value, value, value);
                paint.BlendMode = SKBlendMode.Darken;
            }

            canvas.DrawRect(rect, paint);
        }
    }
}
```

`PaintSurface`ハンドラーは、ビットマップに適した四角形を計算します。 ハンドラーは、そのビットマップを表示し、ビットマップを使用して、上の四角形を表示、`SKPaint`オブジェクトをその`BlendMode`プロパティに設定`SKBlendMode.Lighten`または`SKBlendMode.Darken`します。 `Color`プロパティは、灰色の網かけに基づく、`Slider`します。 `Lighten`モードでは、色の範囲黒から白には、`Darken`白から黒に範囲がモード。

左から右にスクリーン ショットはますます大きく表示`Slider`値薄色一番上のイメージを取得し、暗い、下の画像を取得します。

[![明るくして、画面の](separable-images/LightenAndDarken.png "明るくし、暗くします。")](separable-images/LightenAndDarken-Large.png#lightbox)

このプログラムは、blend の分離モードが使用される標準の方法を示します。 コピー先ビットマップでは非常に多くの場合、何らかのイメージであります。 使用して表示される四角形は、ソースは、`SKPaint`オブジェクトをその`BlendMode`プロパティを分離できる blend モードに設定します。 四角形は、純色を指定できます (ここでは、)、またはグラデーションをします。 透過性は_いない_通常、blend の分離モードで使用します。

このプログラムで実験するときこれら 2 つの blend のモードのされませんは明るくし、一様にイメージが暗く説明します。 代わりに、`Slider`に何らかのしきい値を設定します。 たとえば、するとして次のように向上します。、`Slider`の、`Lighten`モードでは、イメージの暗い部分の取得光最初明るい部分は同じままです。

`Lighten`モード、対象のピクセルは RGB カラー値 (Dr, Dg, Db) と、出力は、ソース ピクセルが色 (Sr、Sg、Sb) の場合 (Or, Og, Ob) として計算されます。

 Or = max(Dr, Sr) Og = max(Dg, Sg) Ob = max(Db, Sb)

赤、緑、青のとは別に、結果が大きいの送信先と送信元です。 これには、最初に、変換先の暗い領域を明るくの効果が生成されます。

`Darken`モードは、結果の送信先と送信元のうちの小さい方がある点が似ています。

 Or = min(Dr, Sr) Og = min(Dg, Sg) Ob = min(Db, Sb)

赤、緑、青のコンポーネントは、個別に処理がこれらの描画モードと呼ばれます、_分離_ブレンド モード。 このため、省略形**Dc**と**Sc**宛先と元の色に使用でき、計算が個別に適用する赤、緑、青のコンポーネントの各が認識されます。

次の表では、その実行内容について簡単に説明とすべての blend の分離モードを示します。 2 番目の列には、変更が生成されない元の色が表示されます。

| ブレンド モード   | 変更なし | 操作 |
| ------------ | --------- | --------- |
| `Plus`       | 黒     | 色を追加することで明るく: Sc + Dc |
| `Modulate`   | 白     | 色を乗算して暗く: Sc·Dc | 
| `Screen`     | 黒     | 補完製品を補完するもの: Sc + Dc &ndash; Sc·Dc |
| `Overlay`    | 灰色      | 逆関数 `HardLight` |
| `Darken`     | 白     | 色の最小: min (Sc, Dc) |
| `Lighten`    | 黒     | 色の最大: max (Sc、Dc) |
| `ColorDodge` | 黒     | ソースに基づいて変換先を明るきます。 |
| `ColorBurn`  | 白     | ソースに基づいて変換先を暗くなります。 | 
| `HardLight`  | 灰色      | 同様に、過酷なスポット ライトの効果 |
| `SoftLight`  | 灰色      | 論理的なスポット ライトの効果に似ています | 
| `Difference` | 黒     | 濃い薄いから減算します Abs (Dc &ndash; Sc)。 | 
| `Exclusion`  | 黒     | ような`Difference`がコントラストを下げる |
| `Multiply`   | 白     | 色を乗算して暗く: Sc·Dc |

詳細なアルゴリズムは、W3C で見つかる[**合成とレベル 1 のブレンド**](https://www.w3.org/TR/compositing-1/)仕様と、Skia [ **SkBlendMode 参照**](https://skia.org/user/api/SkBlendMode_Reference)これら 2 つのソースでの表記は同じはなりません。 注意`Plus`Porter Duff blend モードと見なされることがよくと`Modulate`W3C 仕様の一部ではありません。

ソースが透過的な場合は、し、すべての分離の描画モードを除く`Modulate`、blend モードが影響を与えません。 前に説明したように、 `Modulate` blend モードには、乗算でアルファ チャネルが組み込まれています。 それ以外の場合、`Modulate`と同じ効果`Multiply`します。 

という名前の 2 つのモードに注意してください`ColorDodge`と`ColorBurn`します。 単語_覆い_と_書き込む_photographic 暗室プラクティスで開始されます。 ツールでは、負の値を光が当たって写真印刷。 ライトが点灯しない、印刷は白です。 時間の長い期間の印刷に多くの光が当たる、暗い、印刷を取得します。 印刷者では、一部が、印刷、薄色領域の特定部分の光のブロックを手動または小さなオブジェクトをよく使用されます。 これと呼ばれます_を回避する_します。 逆に、(または手光のほとんどをブロックしている) に穴が不透明なマテリアルを暗くと呼ばれる特定の位置より多くの光に出力するため使用でした_書き込み_します。

**覆いし、Burn**プログラムはほぼ**明るくと暗く**。 XAML ファイルは同じですが、別の要素の名前を持つ構造化し分離コード ファイルはよく似ていますが、同様に、これら 2 つのブレンド モードの影響は大きく異なります。

[![覆いし、Burn](separable-images/DodgeAndBurn.png "覆いし、Burn")](separable-images/DodgeAndBurn-Large.png#lightbox)

小規模`Slider`値、`Lighten`暗い領域を明るくモード中に最初に、`ColorDodge`一様により明るくなります。

多くの場合、イメージ処理アプリケーションのプログラムを回避して、暗室のと同じように、特定の領域に制限するへの書き込みを許可します。 これは、グラデーション、またはさまざまな種類の灰色のビットマップに実現できます。

## <a name="exploring-the-separable-blend-modes"></a>Blend の分離モードの調査

**Blend の分離モード** ページでは、すべての blend の分離モードを確認できます。 ビットマップの保存先と blend モードのいずれかを使用して、色付きの四角形のソースが表示されます。 

XAML ファイルの定義、 `Picker` (blend モードを選択) して 4 つのスライダー。 最初の 3 つのスライダーを使用して、ソースの赤、緑、および青のコンポーネントを設定できます。 4 番目のスライダーは、灰色の網かけに設定してこれらの値をオーバーライドするものです。 個々 のスライダーは識別されませんが、色はそれぞれの機能を示します。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.SeparableBlendModesPage"
             Title="Separable Blend Modes">

    <StackLayout>
        <skiaviews:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blendModePicker"
                Title="Blend Mode"
                Margin="10, 0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlendMode}">
                    <x:Static Member="skia:SKBlendMode.Plus" />
                    <x:Static Member="skia:SKBlendMode.Modulate" />
                    <x:Static Member="skia:SKBlendMode.Screen" />
                    <x:Static Member="skia:SKBlendMode.Overlay" />
                    <x:Static Member="skia:SKBlendMode.Darken" />
                    <x:Static Member="skia:SKBlendMode.Lighten" />
                    <x:Static Member="skia:SKBlendMode.ColorDodge" />
                    <x:Static Member="skia:SKBlendMode.ColorBurn" />
                    <x:Static Member="skia:SKBlendMode.HardLight" />
                    <x:Static Member="skia:SKBlendMode.SoftLight" />
                    <x:Static Member="skia:SKBlendMode.Difference" />
                    <x:Static Member="skia:SKBlendMode.Exclusion" />
                    <x:Static Member="skia:SKBlendMode.Multiply" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Slider x:Name="redSlider"
                MinimumTrackColor="Red"
                MaximumTrackColor="Red"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="greenSlider"
                MinimumTrackColor="Green"
                MaximumTrackColor="Green"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="blueSlider"
                MinimumTrackColor="Blue"
                MaximumTrackColor="Blue"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="graySlider"
                MinimumTrackColor="Gray"
                MaximumTrackColor="Gray"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="colorLabel"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

分離コード ファイルを選択し、ビットマップ リソースの 1 つを読み込み、キャンバスの上部に 1 回と、もう一度下には、2 回、描画キャンバスの半分。

```csharp
public partial class SeparableBlendModesPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(SeparableBlendModesPage),
                        "SkiaSharpFormsDemos.Media.Banana.jpg"); 

    public SeparableBlendModesPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs e)
    {
        if (sender == graySlider)
        {
            redSlider.Value = greenSlider.Value = blueSlider.Value = graySlider.Value;
        }

        colorLabel.Text = String.Format("Color = {0:X2} {1:X2} {2:X2}",
                                        (byte)(255 * redSlider.Value),
                                        (byte)(255 * greenSlider.Value),
                                        (byte)(255 * blueSlider.Value));

        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw bitmap in top half
        SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
        canvas.DrawBitmap(bitmap, rect, BitmapStretch.Uniform);

        // Draw bitmap in bottom halr
        rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
        canvas.DrawBitmap(bitmap, rect, BitmapStretch.Uniform);

        // Get values from XAML controls
        SKBlendMode blendMode =
            (SKBlendMode)(blendModePicker.SelectedIndex == -1 ?
                                        0 : blendModePicker.SelectedItem);

        SKColor color = new SKColor((byte)(255 * redSlider.Value),
                                    (byte)(255 * greenSlider.Value),
                                    (byte)(255 * blueSlider.Value));

        // Draw rectangle with blend mode in bottom half
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = color;
            paint.BlendMode = blendMode;
            canvas.DrawRect(rect, paint);
        }
    }
}
```

下部の`PaintSurface`ハンドラー、四角形は、選択した blend モードと選択した色で 2 つ目のビットマップを描画します。 上部にある元のビットマップの下部にある変更されたビットマップを比較することができます。

[![Blend の分離モード](separable-images/SeparableBlendModes.png "分離可能なブレンド モード")](separable-images/SeparableBlendModes-Large.png#lightbox)

## <a name="additive-and-subtractive-primary-colors"></a>加算と減算の色の原色

**色の原色**ページは、赤、緑、青の 3 つの重なり合う円を描画します。

[![付加的な色の原色](separable-images/PrimaryColors-Additive.png "加法色の原色")](separable-images/PrimaryColors-Additive.png#lightbox)

これらは、付加的な色の原色です。 任意の 2 つの組み合わせによって生成されるシアン、マゼンタ、黄、3 つすべての組み合わせは白です。

これら 3 つの円を描画、`SKBlendMode.Plus`がモードを使用できますも`Screen`、 `Lighten`、または`Difference`同じ効果です。 プログラムを次に示します。

```csharp
public class PrimaryColorsPage : ContentPage
{
    bool isSubtractive;

    public PrimaryColorsPage ()
    {
        Title = "Primary Colors";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;

        // Switch between additive and subtractive primaries at tap
        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            isSubtractive ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);

        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
        float radius = Math.Min(info.Width, info.Height) / 4;
        float distance = 0.8f * radius;     // from canvas center to circle center
        SKPoint center1 = center + 
            new SKPoint(distance * (float)Math.Cos(9 * Math.PI / 6),
                        distance * (float)Math.Sin(9 * Math.PI / 6));
        SKPoint center2 = center +
            new SKPoint(distance * (float)Math.Cos(1 * Math.PI / 6),
                        distance * (float)Math.Sin(1 * Math.PI / 6));
        SKPoint center3 = center +
            new SKPoint(distance * (float)Math.Cos(5 * Math.PI / 6),
                        distance * (float)Math.Sin(5 * Math.PI / 6));

        using (SKPaint paint = new SKPaint())
        {
            if (!isSubtractive)
            {
                paint.BlendMode = SKBlendMode.Plus; 
                System.Diagnostics.Debug.WriteLine(paint.BlendMode);

                paint.Color = SKColors.Red;
                canvas.DrawCircle(center1, radius, paint);

                paint.Color = SKColors.Lime;    // == (00, FF, 00)
                canvas.DrawCircle(center2, radius, paint);

                paint.Color = SKColors.Blue;
                canvas.DrawCircle(center3, radius, paint);
            }
            else
            {
                paint.BlendMode = SKBlendMode.Multiply
                System.Diagnostics.Debug.WriteLine(paint.BlendMode);

                paint.Color = SKColors.Cyan;
                canvas.DrawCircle(center1, radius, paint);

                paint.Color = SKColors.Magenta;
                canvas.DrawCircle(center2, radius, paint);

                paint.Color = SKColors.Yellow;
                canvas.DrawCircle(center3, radius, paint);
            }
        }
    }
}
```

プログラムが含まれています、`TabGestureRecognizer`します。 タップまたは画面をクリックすると、プログラムを使用して`SKBlendMode.Multiply`減算の 3 つのプライマリを表示します。

[![減算の色の原色](separable-images/PrimaryColors-Subtractive.png "減算の色の原色")](separable-images/PrimaryColors-Subtractive-Large.png#lightbox)

`Darken`モードは、これと同じ効果に対しても機能します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
