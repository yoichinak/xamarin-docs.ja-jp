---
title: Blend のない分離モード
description: Blend のない分離モードを使用すると、色相、彩度、または明るさを変更できます。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97FA2730-87C0-4914-8C9F-C64A02CF9EEF
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 9054539b08da89c0f7d8a93150866fb1b41e63f1
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68642782"
---
# <a name="the-non-separable-blend-modes"></a>Blend のない分離モード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

この記事で示した[**分離 SkiaSharp 描画モード**](separable.md)blend の分離モードは、赤、緑、および青のチャネルでの操作を個別に実行します。 Blend のない分離モードは必要ありません。 色の色合い、鮮やかさ、および明るさのレベルに運用する、blend のない分離モードは興味深い方法で色を変更できます。

![非分離サンプル](non-separable-images/NonSeparableSample.png "の分離を可能なサンプル")

## <a name="the-hue-saturation-luminosity-model"></a>色合い-鮮やかさ-明るさモデル

Blend のない分離モードを理解するのには、送信先と送信元のピクセルを色の色合い-鮮やかさ-明るさモデルとして扱う必要があります。 (光度もと呼びます輝度。)

HSL カラー モデルは、情報の記事で説明した[ **Xamarin.Forms との統合**](../../basics/integration.md)でき実験 HSL の色をその記事ではサンプル プログラムです。 作成することができます、`SKColor`色合い、鮮やかさ、および明るさの値を使用して、静的値[ `SKColor.FromHsl` ](xref:SkiaSharp.SKColor.FromHsl*)メソッド。

色合いは、色のドミナント波長を表します。 色相値の範囲は 0 ~ 360 で、加法および減法混色を循環しています。赤は値0、黄色は60、緑は120、シアンは180、青は240、マゼンタは300、循環は360で赤に戻ります。

主調色がない場合は&mdash;色は白または黒、灰色の網かけ、たとえば、&mdash;色合いは未定義であり、通常は 0 に設定します。 

彩度の値では、0 から 100 まででき、色の純度を指定することができます。 100 の鮮やかさの値は、値が 100 になるより grayish 色よりもを下げるときに、純粋な色です。 鮮やかさの値が 0 の灰色の網かけになります。

光度 (明るさ) の値は、どのように明るい色を示します。 明るさの値は 0 では、その他の設定に関係なく黒です。 同様に、100 の明るさの値は白です。 

HSL 値 (0, 100, 50) は、RGB 値 (FF、00, 00)、これは純粋な赤色です。 HSL 値 (180, 100, 50) は、純粋なシアン RGB 値 (00、FF FF) です。 彩度が減少すると、主調色コンポーネントの減少し、その他のコンポーネントを増加させます。 0 の彩度のレベルにあるすべてのコンポーネントが同じとは灰色の網かけの色。 黒; に移動する明るさを減らす白に進むには、輝度を増やします。

## <a name="the-blend-modes-in-detail"></a>詳細にブレンド モード

(これは、ビットマップ イメージでは多くの場合) 先と、ソースでは、1 つの色またはグラデーションを多くの場合は、他の blend モードのように 4 つの非分離 blend モードが含まれます。 ブレンド モードでは、コピー先と、ソースから色合い、鮮やかさ、および明るさの値を組み合わせてください。

| ブレンド モード   | ソースからのコンポーネント | 変換先コンポーネント |
| ------------ | ---------------------- | --------------------------- |
| `Hue`        | [色合い]                    | 彩度と輝度   |
| `Saturation` | [彩度]             | Hue と輝度          |
| `Color`      | 色相と彩度     | 光度                  | 
| `Luminosity` | 光度             | 色相と彩度          | 

W3C を参照してください。 [**合成とレベル 1 のブレンド**](https://www.w3.org/TR/compositing-1/)アルゴリズムの仕様。

**非分離 Blend モード**ページが含まれています、`Picker`次のいずれかを選択するモードと 3 つの blend `Slider` HSL の色を選択するビュー。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.NonSeparableBlendModesPage"
             Title="Non-Separable Blend Modes">

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
                    <x:Static Member="skia:SKBlendMode.Hue" />
                    <x:Static Member="skia:SKBlendMode.Saturation" />
                    <x:Static Member="skia:SKBlendMode.Color" />
                    <x:Static Member="skia:SKBlendMode.Luminosity" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="satSlider"
                Maximum="100"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="lumSlider"
                Maximum="100"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <StackLayout Orientation="Horizontal">
            <Label x:Name="hslLabel"
                   HorizontalOptions="CenterAndExpand" />

            <Label x:Name="rgbLabel"
                   HorizontalOptions="CenterAndExpand" />

        </StackLayout>
    </StackLayout>
</ContentPage>
```

3 つの領域を節約する`Slider`ビューは、プログラムのユーザー インターフェイスでは識別されません。 順序は、色合い、鮮やかさ、および明るさを記憶する必要があります。 2 つ`Label`ページの下部にあるビューは、HSL および RGB カラー値を表示します。

分離コード ファイル ビットマップ リソースの 1 つを読み込み、表示する、キャンバス上で可能な限り大きくし、四角形をキャンバスを説明しています。 四角形の色は、3 つに基づいて`Slider`はで選択されている 1 つのビューと blend モード、 `Picker`:

```csharp
public partial class NonSeparableBlendModesPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(NonSeparableBlendModesPage),
                        "SkiaSharpFormsDemos.Media.Banana.jpg");
    SKColor color;

    public NonSeparableBlendModesPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs e)
    {
        // Calculate new color based on sliders
        color = SKColor.FromHsl((float)hueSlider.Value,
                                (float)satSlider.Value,
                                (float)lumSlider.Value);

        // Use labels to display HSL and RGB color values
        color.ToHsl(out float hue, out float sat, out float lum);

        hslLabel.Text = String.Format("HSL = {0:F0} {1:F0} {2:F0}",
                                      hue, sat, lum);

        rgbLabel.Text = String.Format("RGB = {0:X2} {1:X2} {2:X2}",
                                      color.Red, color.Green, color.Blue);

        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);

        // Get blend mode from Picker
        SKBlendMode blendMode =
            (SKBlendMode)(blendModePicker.SelectedIndex == -1 ?
                                        0 : blendModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Color = color;
            paint.BlendMode = blendMode;
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

プログラムによって、3 つのスライダーで選択されているの HSL の色の値が表示されないことに注意してください。 代わりに、これらのスライダーから色の値を作成しを使用して、 [ `ToHsl` ](xref:SkiaSharp.SKColor.ToHsl*)色合い、鮮やかさ、および明るさの値を取得するメソッド。 これは、ため、`FromHsl`メソッドで内部的に格納されている、RGB 色を変換して、HSL カラー、`SKColor`構造体。 `ToHsl`メソッドが RGB から HSL に変換しますが、結果常には、元の値。 

たとえば、 `FromHsl` HSL 値に変換します (180, 50, 0)、RGB 色 (0, 0, 0) ため、`Luminosity`は 0 です。 `ToHsl`メソッドは、色相と彩度値は関連がないため、HSL の色 (0, 0, 0) に RGB 色 (0, 0, 0) を変換します。 お勧めこのプログラムを使用する場合、スライダーで指定した 1 つではなく、プログラムを使用している HSL の色の表現を参照してください。

`SKBlendModes.Hue` Blend モードは、変換先の彩度と輝度レベルを維持しながら、ソースの Hue レベルを使用します。 この blend モードをテストするときに、彩度、光度スライダーする必要がありますに設定する 0 および 100 以外ような場合、色合いは一意に定義されていません。

[![非分離ブレンド モード - Hue](non-separable-images/NonSeparableBlendModes-Hue.png "非分離ブレンド モードの Hue")](non-separable-images/NonSeparableBlendModes-Hue-Large.png#lightbox)

ときに使用して (左側にある iOS のスクリーン ショット) と同様に、スライダーを 0 に設定する、すべてのものが赤になります。 イメージがまったく存在しないことはありませんが、緑、青の。 当然ながら、結果内に存在しているが灰色の網かけがあります。 RGB 色である (40, 40, C0) たとえば、HSL カラー (240, 50, 50) に相当します。 色合いは青ですが、50 の鮮やかさの値は、赤、緑のコンポーネントもあることを示します。 0 に設定されている、Hue 場合`SKBlendModes.Hue`HSL の色です (0, 50, 50)、RGB 色 (C0、40, 40)。 まだ青と緑のコンポーネントがありますが、主要なコンポーネントが赤のようになりました。

`SKBlendModes.Saturation` Blend モードでは、ソースの彩度のレベルを結合 Hue と変換先の明るさを使用します。 Hue など、輝度が 0 または 100 の場合、鮮やかさはも定義されません。 理論上は、これら 2 つの両極端の間、明るさの設定が動作する必要があります。 ただし、明るさの設定は、必要以上に、結果に影響を与えるようです。 明るさは 50 に設定し、画像の彩度のレベルを設定する方法を参照してください。

[![非分離ブレンド モード - 彩度](non-separable-images/NonSeparableBlendModes-Saturation.png "非分離ブレンド モードの彩度")](non-separable-images/NonSeparableBlendModes-Saturation-Large.png#lightbox)

この blend モードを使用するには退屈のイメージの彩度の色を向上させることもの灰色の網かけのみで構成される結果の画像の (左側にある iOS のスクリーン ショット) のようにゼロまでの彩度を減らすことができます。

`SKBlendModes.Color` Blend モードは、変換先の輝度が保持されますが、色相と彩度のソースを使用します。 ここでも、光度スライダー、両極端の間にどこかの設定が機能するを意味します。 

[![非分離 Blend モード - 色](non-separable-images/NonSeparableBlendModes-Color.png "非分離ブレンド モードの色")](non-separable-images/NonSeparableBlendModes-Color-Large.png#lightbox)

この blend モードのアプリケーションは、間もなく表示されます。

最後に、 `SKBlendModes.Luminosity` blend モードは、逆の`SKBlendModes.Color`します。 Hue と変換先の飽和状態が保持されますが、ソースの明るさを使用します。 `Luminosity` Blend モードは、バッチの最も不可解な部分です。色合いと鮮やかさのスライダーは画像に影響しますが、中程度の明るさでも画像は区別されません。

[![非分離ブレンド モード - 光度](non-separable-images/NonSeparableBlendModes-Luminosity.png "非分離ブレンド モードの明るさ")](non-separable-images/NonSeparableBlendModes-Luminosity-Large.png#lightbox)

理論上は、画像の明るさを増減させる簡単に明るくしたり暗くしたりします。 興味深いことに、[光度の例で、Skia **SkBlendMode 参照**](https://skia.org/user/api/SkBlendMode_Reference#Luminosity)はよく似ています。

通常、全体のコピー先の画像に適用される 1 つの色で構成される、ソースと非分離 blend モードのいずれかを使用し場合。 効果が多すぎるだけです。 イメージの 1 つの部分に影響を制限します。 その場合は、ソースが transparancy、組み込まれる可能性があります。 または、おそらく、ソースは小さなグラフィックに制限されます。

## <a name="a-matte-for-a-separable-mode"></a>分離モードのマット

1 つのリソースとして含まれているビットマップ、 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプル。 ファイル名は**Banana.jpg**:

![バナナ Monkey](non-separable-images/Banana.jpg "Banana Monkey")

バナナだけを含むマットを作成することになります。 これは内のリソースにも、 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプル。 ファイル名は**BananaMatte.png**:

![バナナ マット](non-separable-images/BananaMatte.png "Banana マット")

別に、黒のバナナ図形、ビットマップの残りの部分は透過的です。

**青い Banana**色相と彩度、monkey が保持しているバナナの変更しますが、イメージの他に何も変更するページがそのマットを使用します。 

次に`BlueBananaPage`クラス、 **Banana.jpg**ビットマップがフィールドとして読み込まれます。 コンス トラクターの読み込み、 **BananaMatte.png**としてビットマップ、`matteBitmap`オブジェクトがコンス トラクターは、そのオブジェクトを保持しません。 代わりに、3 つ目のビットマップの名前付き`blueBananaBitmap`が作成されます。 `matteBitmap`に描画される`blueBananaBitmap`続けて、`SKPaint`でその`Color`青に設定し、その`BlendMode`に設定`SKBlendMode.SrcIn`。 `blueBananaBitmap`透過的ですが、バナナの solid 純粋な青いイメージ、ほとんどの場合は残ります。

```csharp
public class BlueBananaPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BlueBananaPage),
        "SkiaSharpFormsDemos.Media.Banana.jpg");

    SKBitmap blueBananaBitmap;

    public BlueBananaPage()
    {
        Title = "Blue Banana";

        // Load banana matte bitmap (black on transparent)
        SKBitmap matteBitmap = BitmapExtensions.LoadBitmapResource(
            typeof(BlueBananaPage),
            "SkiaSharpFormsDemos.Media.BananaMatte.png");

        // Create a bitmap with a solid blue banana and transparent otherwise
        blueBananaBitmap = new SKBitmap(matteBitmap.Width, matteBitmap.Height);

        using (SKCanvas canvas = new SKCanvas(blueBananaBitmap))
        {
            canvas.Clear();
            canvas.DrawBitmap(matteBitmap, new SKPoint(0, 0));

            using (SKPaint paint = new SKPaint())
            {
                paint.Color = SKColors.Blue;
                paint.BlendMode = SKBlendMode.SrcIn;
                canvas.DrawPaint(paint);
            }
        }

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);

        using (SKPaint paint = new SKPaint())
        {
            paint.BlendMode = SKBlendMode.Color;
            canvas.DrawBitmap(blueBananaBitmap, 
                              info.Rect, 
                              BitmapStretch.Uniform, 
                              paint: paint);
        }
    }
}
```

`PaintSurface`ハンドラー、monkey、banana を保持するいると、ビットマップを描画します。 このコードの表示の後に`blueBananaBitmap`で`SKBlendMode.Color`します。 Banana の上、各ピクセルの色相と彩度が 240 の色合いの値と 100 の鮮やかさの値に対応する青色の実線に置換されます。 明るさ、ただし、変わらない、banana は引き続きその新しい色に関係なく現実的なテクスチャには。

[![青のバナナ](non-separable-images/BlueBanana.png "バナナの青")](non-separable-images/BlueBanana-Large.png#lightbox)

ブレンド モードを変更してみてください`SKBlendMode.Saturation`します。 Banana は黄色のままですより濃い黄色です。 実際のアプリケーションで、banana を青にすることよりも望ましい効果があります。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
