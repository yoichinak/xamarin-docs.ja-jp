---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c1939c30cbefdbf8d6546761a8c6ac7199bfff62
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139686"
---
# <a name="the-separable-blend-modes"></a>分離可能 blend モード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

記事「 [**SkiaSharp Porter-Duff blend**](porter-duff.md)mode」に示されているように、Porter-duff blend モードでは一般にクリッピング操作が実行されます。 分離可能な blend モードは異なります。 分離可能モードでは、イメージの赤、緑、および青の各色要素が変更されます。 分離可能な blend モードでは、色を混ぜて、赤、緑、および青の組み合わせが実際に白であることを示すことができます。

![原色](separable-images/SeparableSample.png "原色")

## <a name="lighten-and-darken-two-ways"></a>2つの方法を比較して暗くする 

ビットマップは、暗すぎるか、または小さすぎることがよくあります。 分離可能な blend モードを使用すると、imabe を明るくしたり暗くしたりできます。  実際には、列挙型の分離可能な blend モードのうちの2つ [`SKBlendMode`](xref:SkiaSharp.SKBlendMode) はとという名前です `Lighten` `Darken` 。 

この2つのモードについては、「**明るくと暗く**」ページで説明しています。 XAML ファイルは、2つの `SKCanvasView` オブジェクトと2つのビューをインスタンス化し `Slider` ます。

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

最初のとを示し、 `SKCanvasView` `Slider` `SKBlendMode.Lighten` 2 番目のペアを示し `SKBlendMode.Darken` ます。 2つの `Slider` ビューは同じ `ValueChanged` ハンドラーを共有し、2つのビューは同じハンドラーを共有し `SKCanvasView` `PaintSurface` ます。 どちらのイベントハンドラーも、どのオブジェクトがイベントを発生させているかを確認します。

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

ハンドラーは、 `PaintSurface` ビットマップに適した四角形を計算します。 ハンドラーはビットマップを表示し、 `SKPaint` `BlendMode` プロパティがまたはに設定されたオブジェクトを使用して、ビットマップの上に四角形を表示し `SKBlendMode.Lighten` `SKBlendMode.Darken` ます。 `Color`プロパティは、に基づく灰色の網掛けです `Slider` 。 モードでは、 `Lighten` 色は黒から白までの範囲になりますが、モードの場合は `Darken` 白から黒までの範囲になります。

左から右へのスクリーンショットは、上位の画像が明るくなり、下の画像が暗くなるほど、より大きな値を示し `Slider` ています。

[![明るくして暗くする](separable-images/LightenAndDarken.png "明るくして暗くする")](separable-images/LightenAndDarken-Large.png#lightbox)

このプログラムは、分離可能な blend モードを使用する通常の方法を示しています。変換先は、何らかの種類の画像であり、非常に多くの場合、ビットマップです。 ソースは、 `SKPaint` `BlendMode` プロパティが分離可能な blend モードに設定されたオブジェクトを使用して表示される四角形です。 四角形は、(ここに示すように) 純色でもグラデーションでもかまいません。 透明度は、分離可能な blend モードでは通常使用され_ません_。

このプログラムを試してみると、これら2つの blend モードではイメージの明るさと暗くが統一されていないことがわかります。 代わりに、に `Slider` よって、ある種のしきい値が設定されているように見えます。 たとえば、モードのを大きくすると、 `Slider` `Lighten` 明るい領域が同じままでも、画像の暗い領域が最初に明るくなります。

モードでは、 `Lighten` 変換先のピクセルが RGB カラー値 (Dr、Dg、Db) で、ソースピクセルが色 (Sr、Sg、Sb) の場合、出力は次のように計算されます (または、Og、Ob)。

 `Or = max(Dr, Sr)` `Og = max(Dg, Sg)`
 `Ob = max(Db, Sb)`

赤、緑、および青の場合は、出力先とコピー元の値が大きくなります。 これにより、最初に変換先の暗い領域を明るくする効果が得られます。

`Darken`モードは似ていますが、結果はコピー先とコピー元の方が小さい点が異なります。

 `Or = min(Dr, Sr)` `Og = min(Dg, Sg)`
 `Ob = min(Db, Sb)`

赤、緑、および青のコンポーネントはそれぞれ個別に処理されるため、これらの blend モードは_分離_可能な blend モードと呼ばれます。 このため、ターゲットとソースの色には、省略形**Dc**と**Sc**を使用できます。また、計算は、赤、緑、および青の各コンポーネントに個別に適用されることがわかります。

次の表に、すべての分離可能な blend モードの概要とその説明を示します。 2番目の列には、変更されていないソースの色が表示されます。

| Blend モード   | 変更なし | 操作 |
| ---
タイトル: 説明: ms。製品: ms。テクノロジ: ms. assetid: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
タイトル: 説明: ms。製品: ms。テクノロジ: ms. assetid: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
タイトル: 説明: ms。製品: ms。テクノロジ: ms. assetid: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
タイトル: 説明: ms。製品: ms。テクノロジ: ms. assetid: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

------ |---title: description: ms. 製品: ms. テクノロジ: ms. assetid: author: ms. author: ms. date: no loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
タイトル: 説明: ms。製品: ms。テクノロジ: ms. assetid: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

----- |---title: description: ms. 製品: ms. テクノロジ: ms. assetid: author: ms. author: ms. date: no loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
タイトル: 説明: ms。製品: ms。テクノロジ: ms. assetid: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

----- | |`Plus`       |黒 |色の追加による明るく: Sc + Dc | |`Modulate`   |白 |色を乗算して暗くする: Sc ·Dc | |`Screen`     |黒 |補完の製品を補完する: Sc + Dc &ndash; Sc ·Dc | |`Overlay`    |灰色 |`HardLight`| | `Darken` の逆関数    |白 |最小色: 最小値 (Sc、Dc) | |`Lighten`    |黒 |最大色: 最大値 (Sc、Dc) | |`ColorDodge` |黒 |ソースに基づいてターゲットを明るくする | |`ColorBurn`  |白 |ソースに基づいて変換先を暗くする | |`HardLight`  |灰色 |厳格なスポットライトの効果と似ています | |`SoftLight`  |灰色 |ソフトスポットライトスポットの場合と同様の効果 | |`Difference` |黒 |淡い: Abs (Dc &ndash; Sc) `Exclusion` から濃い色を減算します。 |黒 |と似て `Difference` いますが、コントラストが低い | | `Multiply`  |白 |色を乗算して暗くする: Sc ·Dc |

より詳細なアルゴリズムについては、W3C[**合成とブレンドレベル 1**](https://www.w3.org/TR/compositing-1/)仕様と Skia [**Skblendmode リファレンスを参照**](https://skia.org/user/api/SkBlendMode_Reference)してください。ただし、これらの2つのソースの表記は同じではありません。 は、通常、 `Plus` Porter-Duff blend モードと見なされ、W3C 仕様には含まれていないことに注意してください `Modulate` 。

ソースが透明である場合、を除くすべての分離可能な blend モードでは、 `Modulate` blend モードでは効果がありません。 前に見たように、 `Modulate` blend モードでは、乗算にアルファチャネルが組み込まれています。 それ以外の場合、 `Modulate` はと同じ効果を持ち `Multiply` ます。 

とという2つのモードに注意して `ColorDodge` `ColorBurn` ください。 「_覆い焼き_」と「焼き_込み_」という言葉は、写真の暗室プラクティスに由来しています。 たまにを使用すると、光が負の値になるように写真を印刷できます。 ライトを使用しない場合、印刷は白になります。 印刷により長い時間がかかるほど、印刷が暗くなります。 多くの場合、紙または小さいオブジェクトを使用して、ある種の光が印刷の特定の部分に落下するのをブロックし、その領域を明るくしています。 これは_dodging_と呼ばれています。 反対に、穴がある不透明なマテリアル (または、ほとんどの光を手でブロックする) を使用して、特定のスポットに光を追加して、_書き込み_と呼ばれる暗くすることができます。

**覆い焼きと書き込み**プログラムは、**明るく、暗く**するのとよく似ています。 XAML ファイルは同じように構成されていますが、要素名が異なるため、分離コードファイルも同様に似ていますが、これら2つの blend モードの効果は大きく異なります。

[![覆い焼きと書き込み](separable-images/DodgeAndBurn.png "覆い焼きと書き込み")](separable-images/DodgeAndBurn-Large.png#lightbox)

小さい値の場合 `Slider` 、このモードでは最初に `Lighten` 暗い領域が明るく `ColorDodge` なり、より均一に明るくなります。

多くの場合、イメージ処理アプリケーションプログラムでは、暗室の場合と同様に、dodging と書き込みを特定の領域に制限することができます。 これを行うには、グラデーション、または灰色の網掛けを持つビットマップを使用します。

## <a name="exploring-the-separable-blend-modes"></a>分離可能 blend モードの調査

[**分離可能な Blend モード**] ページでは、すべての分離可能な blend モードを調べることができます。 ブレンドモードの1つを使用して、ビットマップの変換先と色付きの四角形のソースが表示されます。 

XAML ファイルでは、 `Picker` (blend モードを選択するための) と4つのスライダーが定義されています。 最初の3つのスライダーを使用すると、ソースの赤、緑、および青のコンポーネントを設定できます。 4番目のスライダーは、灰色の網掛けを設定してこれらの値をオーバーライドすることを目的としています。 個々のスライダーは識別されませんが、色はその機能を示しています。

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

分離コードファイルは、ビットマップリソースの1つを読み込み、キャンバスの上半分に1回、キャンバスの下半分にもう一度描画します。

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

ハンドラーの下部に向かって `PaintSurface` 、選択した blend モードと選択した色で2番目のビットマップに四角形が描画されます。 一番下にある変更されたビットマップを、一番上にある元のビットマップと比較できます。

[![分離可能なブレンド モード](separable-images/SeparableBlendModes.png "分離可能なブレンド モード")](separable-images/SeparableBlendModes-Large.png#lightbox)

## <a name="additive-and-subtractive-primary-colors"></a>加法および減法混色の原色

[**原色**] ページでは、赤、緑、および青の3つの重なり合う円が描画されます。

[![加法主色](separable-images/PrimaryColors-Additive.png "加法主色")](separable-images/PrimaryColors-Additive.png#lightbox)

これらは加算主色です。 任意の2つの組み合わせ (シアン、マゼンタ、黄) と、3つすべての組み合わせが白です。

この3つの円は、モードで描画され `SKBlendMode.Plus` ますが `Screen` 、 `Lighten` 同じ効果を得るために、、またはを使用することもでき `Difference` ます。 プログラムは次のようになります。

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

プログラムにはが含まれてい `TabGestureRecognizer` ます。 画面をタップまたはクリックすると、プログラムは `SKBlendMode.Multiply` を使用して3つの減法三原色を表示します。

[![主色を減法ます](separable-images/PrimaryColors-Subtractive.png "主色を減法ます")](separable-images/PrimaryColors-Subtractive-Large.png#lightbox)

モードは、 `Darken` この同じ効果にも有効です。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
