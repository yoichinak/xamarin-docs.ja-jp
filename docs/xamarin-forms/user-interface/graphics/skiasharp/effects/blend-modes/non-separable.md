---
title: 分離不可能な blend モード
description: 分離不可能な blend モードを使用して、色合い、鮮やかさ、または輝度を変更します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97FA2730-87C0-4914-8C9F-C64A02CF9EEF
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a77ebb07a09c1bbd2df482c81040f271cdf8f56e
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556348"
---
# <a name="the-non-separable-blend-modes"></a>分離不可能な blend モード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

この記事で説明したように、分離可能な blend モードでは、赤、緑、および青のチャネルに対して個別に [**操作を実行**](separable.md)します。 分離不可能な blend モードには対応していません。 色の色合い、鮮やかさ、および輝度のレベルに基づいて操作することにより、分離不可能な blend モードで色を変更することができます。

![分離不可能なサンプル](non-separable-images/NonSeparableSample.png "分離不可能なサンプル")

## <a name="the-hue-saturation-luminosity-model"></a>色合いと鮮やかさの輝度モデル

分離不可能な blend モードについて理解するためには、色合いと光源の輝度モデルで、変換先とソースのピクセルを色として扱う必要があります。 (明度は輝度とも呼ばれます)。

HSL カラーモデルは、との統合に[**関する Xamarin.Forms **](../../basics/integration.md)記事で説明されています。また、この記事のサンプルプログラムでは、hsl 色を使用した実験を行うことができます。 `SKColor`静的な方法では、色合い、鮮やかさ、および明るさの値を使用して値を作成でき [`SKColor.FromHsl`](xref:SkiaSharp.SKColor.FromHsl*) ます。

色合いは、色の主要な波長を表します。 色合いの値は、0 ~ 360 の範囲で、加法と減法を循環しています。赤は0、黄色は60、緑は120、シアンは180、blue は240、マゼンタは300、そのサイクルは360で赤に戻ります。

&mdash;たとえば、色が白、黒、灰色の網かけの場合、色合いは未定義で、 &mdash; 通常は0に設定されます。 

鮮やかさの値は 0 ~ 100 の範囲で、色の純度を示します。 鮮やかさの値100は purest の色であり、100より小さい値を指定すると、色がグレーになります。 彩度の値が0の場合、灰色の網掛けが生成されます。

明るさ (または明るさ) の値は、色がどれくらい明るいかを示します。 明るさの値0は、他の設定に関係なく黒です。 同様に、輝度値100は白です。 

HSL 値 (0、100、50) は RGB 値 (FF、00、00) です。これは純粋な赤です。 HSL 値 (180、100、50) は、RGB 値 (00、FF、FF)、および純粋シアンです。 鮮やかさが減少すると、主要な色コンポーネントが減少し、その他のコンポーネントも増えます。 鮮やかさレベルが0の場合、すべてのコンポーネントが同じで、色は灰色の網掛けになります。 明るさを下げて、黒に移行します。白になるように明るさを上げます。

## <a name="the-blend-modes-in-detail"></a>Blend モードの詳細

他の blend モードと同様、分離不可能な4つのブレンドモードには、変換先 (多くの場合ビットマップイメージ) とソース (多くの場合、単一の色またはグラデーション) が関係します。 Blend モードでは、変換先とソースから、色合い、鮮やかさ、および明るさの値が結合されます。

| Blend モード   | ソースからのコンポーネント | 変換先からのコンポーネント |
| ------------ | ---------------------- | --------------------------- |
| `Hue`        | [色合い]                    | 鮮やかさと明るさ   |
| `Saturation` | [鮮やかさ]             | 色合いと輝度          |
| `Color`      | 色合いと鮮やかさ     | 明度                  | 
| `Luminosity` | 明度             | 色合いと鮮やかさ          | 

詳細については、「W3C [**合成」と「レベル 1**](https://www.w3.org/TR/compositing-1/) のアルゴリズムの合成」を参照してください。

[ **分離不可能な Blend モード** ] ページには、 `Picker` 次のいずれかの blend モードを選択するためのが含まれており、3つの `Slider` ビューで HSL 色を選択します。

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

領域を節約するために、3つの `Slider` ビューはプログラムのユーザーインターフェイスでは識別されません。 この順序は、色合い、鮮やかさ、および輝度であることを覚えておく必要があります。 `Label`ページの下部にある2つのビューには、HSL と RGB の色の値が表示されます。

分離コードファイルは、ビットマップリソースの1つを読み込み、キャンバスで可能な限り大きいものとして表示した後、キャンバスを四角形で覆います。 四角形の色は、3つの `Slider` ビューに基づいています。 blend モードは、で選択されてい `Picker` ます。

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

このプログラムでは、3つのスライダーによって選択された HSL カラー値が表示されないことに注意してください。 代わりに、これらのスライダーから色の値を作成し、メソッドを使用して、 [`ToHsl`](xref:SkiaSharp.SKColor.ToHsl*) 色合い、鮮やかさ、および輝度の値を取得します。 これは、 `FromHsl` メソッドが HSL の色を、内部的に構造体に格納されている RGB 色に変換するためです `SKColor` 。 メソッドは、 `ToHsl` RGB から HSL に変換しますが、結果は常に元の値になるとは限りません。 

たとえば、 `FromHsl` は、が0であるため、HSL 値 (180、50、0) を RGB 色 (0, 0, 0) に変換し `Luminosity` ます。 この `ToHsl` メソッドは、RGB 色 (0, 0, 0) を HSL 色 (0, 0, 0) に変換します。これは、色合いと鮮やかさの値が無関係であるためです。 このプログラムを使用する場合は、スライダーで指定したものではなく、プログラムが使用している HSL の色の表現を確認することをお勧めします。

Blend モードでは、 `SKBlendModes.Hue` 変換先の鮮やかさと輝度のレベルを維持しながら、ソースの色合いレベルを使用します。 この blend モードをテストする場合は、[鮮やかさ] および [明るさ] スライダーを0または100以外の値に設定する必要があります。この場合、色合いは一意に定義されていません。

[![分離不可能な Blend モード-色相](non-separable-images/NonSeparableBlendModes-Hue.png "分離不可能な Blend モード-色相")](non-separable-images/NonSeparableBlendModes-Hue-Large.png#lightbox)

を使用すると、スライダーを 0 (左側の iOS スクリーンショットのように) に設定すると、すべて reddish が切り替わります。 ただし、これは、イメージが完全に緑と青ではないことを意味するわけではありません。 結果には、まだグレーの網掛けがあることが明らかです。 たとえば、RGB 色 (40、40、C0) は、HSL 色 (240、50、50) に相当します。 色合いは青ですが、鮮やかさの値50は、赤と緑のコンポーネントもあることを示します。 色合いがで0に設定されている場合、 `SKBlendModes.Hue` HSL の色は (0, 50, 50) になります。これは RGB 色 (c0、40、40) です。 まだ青と緑のコンポーネントがありますが、ここでは主要なコンポーネントが赤になっています。

Blend モードでは、 `SKBlendModes.Saturation` ソースの鮮やかさレベルと変換先の色合いと輝度が結合されます。 色合いと同様に、輝度が0または100の場合、鮮やかさは適切に定義されていません。 理論的には、これらの2つの値の間に明るさの設定があれば、それが機能します。 ただし、明るさの設定は、結果によっては必要以上に影響するように見えます。 輝度を50に設定すると、画像の鮮やかさレベルを設定する方法を確認できます。

[![分離不可能な Blend モード-鮮やかさ](non-separable-images/NonSeparableBlendModes-Saturation.png "分離不可能な Blend モード-鮮やかさ")](non-separable-images/NonSeparableBlendModes-Saturation-Large.png#lightbox)

この blend モードを使用すると、退屈イメージの色の鮮やかさを増やすことができます。または、灰色の網掛けだけで構成される結果のイメージについて、鮮やかさを 0 (左側の iOS スクリーンショットのように) に下げることができます。

Blend モードでは、 `SKBlendModes.Color` 変換先の輝度が保持されますが、ソースの色合いと鮮やかさが使用されます。 ここでも、これは、すべての明るさスライダーのいずれかの設定が極端に機能することを意味します。 

[![分離不可能な Blend モード-色](non-separable-images/NonSeparableBlendModes-Color.png "分離不可能な Blend モード-色")](non-separable-images/NonSeparableBlendModes-Color-Large.png#lightbox)

この blend モードのアプリケーションはすぐに表示されます。

最後に、 `SKBlendModes.Luminosity` blend モードはの逆です `SKBlendModes.Color` 。 これは、変換先の色合いと鮮やかさを保持しますが、ソースの輝度を使用します。 `Luminosity`Blend モードは、バッチの最も不可解な部分です。色合いと鮮やかさのスライダーは画像に影響しますが、中程度の輝度でも画像は区別されません。

[![分離不可能な Blend モード-輝度](non-separable-images/NonSeparableBlendModes-Luminosity.png "分離不可能な Blend モード-輝度")](non-separable-images/NonSeparableBlendModes-Luminosity-Large.png#lightbox)

理論的には、画像の明るさを増減すると、明るくしたり暗くしたりすることができます。 興味深いことに、 [Skia **Skblendmode リファレンス**の輝度の例](https://skia.org/user/api/SkBlendMode_Reference#Luminosity)は非常に似ています。

通常は、変換先イメージ全体に適用される単一の色で構成されるソースで、分離不可能な blend モードの1つを使用することはできません。 効果は非常に優れています。 イメージの1つの部分に効果を制限する必要があります。 この場合、ソースには transparancy が組み込まれているかもしれませんが、ソースは小さいグラフィックに制限されます。

## <a name="a-matte-for-a-separable-mode"></a>分離可能モードのマット

[**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルのリソースとして含まれているビットマップの1つを次に示します。 ファイル名は **Banana.jpg**:

![バナナサル](non-separable-images/Banana.jpg "バナナサル")

バナナだけを含むマットを作成することができます。 これは、 [**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) サンプルのリソースでもあります。 ファイル名は **BananaMatte.png**:

![バナナマット](non-separable-images/BananaMatte.png "バナナマット")

黒のバナナ図形とは別に、ビットマップの残りの部分は透明になります。

**Blue バナナ**ページでは、そのマットを使用して、サルが保持しているバナナの色合いと鮮やかさを変更しますが、イメージ内の他は何も変更しません。 

次の `BlueBananaPage` クラスでは、 **Banana.jpg** ビットマップがフィールドとして読み込まれます。 コンストラクターは、オブジェクトとして **BananaMatte.png** ビットマップを読み込み `matteBitmap` ますが、コンストラクター以外のオブジェクトは保持しません。 代わりに、という名前の3番目のビットマップ `blueBananaBitmap` が作成されます。 はに `matteBitmap` 描画され、その後には `blueBananaBitmap` blue に設定され、が `SKPaint` `Color` に設定され `BlendMode` `SKBlendMode.SrcIn` ます。 は `blueBananaBitmap` ほとんど透明なままですが、バナナの完全に青い青い画像があります。

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

この `PaintSurface` ハンドラーは、バナナを保持しているサルでビットマップを描画します。 このコードの後に、と共にが表示され `blueBananaBitmap` `SKBlendMode.Color` ます。 バナナの表面では、各ピクセルの色相と鮮やかさは水色に置き換えられます。これは、色合いの値240と、鮮やかさの値100に対応しています。 ただし、明るさは変わりません。つまり、バナナは新しい色に関係なく、現実的なテクスチャを引き続き使用できます。

[![Blue バナナ](non-separable-images/BlueBanana.png "Blue バナナ")](non-separable-images/BlueBanana-Large.png#lightbox)

Blend モードをに変更してみてください `SKBlendMode.Saturation` 。 バナナは黄色のままですが、より強い黄色です。 実際のアプリケーションでは、バナナ blue をオンにするよりも望ましい効果があります。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)