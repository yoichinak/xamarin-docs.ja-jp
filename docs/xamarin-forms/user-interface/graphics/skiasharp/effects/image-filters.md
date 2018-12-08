---
title: SkiaSharp の画像のフィルター
description: イメージ フィルター作成ぼかしやドロップ シャドウを使用する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 173E7B22-AEC8-4F12-B657-1C0CEE01AD63
author: davidbritch
ms.author: dabritch
ms.date: 08/27/2018
ms.openlocfilehash: 517ebfb529dd26236ba157d40168fa7c75288d27
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53050375"
---
# <a name="skiasharp-image-filters"></a>SkiaSharp の画像のフィルター

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

イメージ フィルターは、イメージを構成するピクセルの色のすべてのビットで動作する効果です。 記事の説明に従って、アルファ チャネルに対してのみ作用マスク フィルターより汎用性の高い[ **SkiaSharp マスク フィルター**](mask-filters.md)します。 イメージ フィルターを使用する設定、 [ `ImageFilter` ](xref:SkiaSharp.SKPaint.ImageFilter)プロパティの`SKPaint`型のオブジェクトに[ `SKImageFilter` ](xref:SkiaSharp.SKImageFilter)クラスの静的メソッドのいずれかを呼び出すことで作成したことです。

フィルターのマスクを理解する最善の方法では、これらの静的メソッドを使って試してみるです。 ぼかしビットマップ全体をマスク フィルターを使用することができます。

![例のぼかし](image-filters-images/ImageFilterExample.png "ぼかしの例")

この記事では、ドロップ シャドウ、および作成エンボスの効果は彫刻にイメージ フィルターを使用しても示します。

## <a name="blurring-vector-graphics-and-bitmaps"></a>ベクター グラフィックスとビットマップにぼかし効果

によって作成されたぼかし効果、 [ `SKImageFilter.CreateBlur` ](xref:SkiaSharp.SKImageFilter.CreateBlur*)静的メソッドでぼかしメソッドに優る大きな利点が、 [ `SKMaskFilter` ](xref:SkiaSharp.SKMaskFilter)クラス: イメージ フィルターは、ビットマップ全体をぼかすことができます。 メソッドでは、次の構文があります。

```csharp
public static SkiaSharp.SKImageFilter CreateBlur (float sigmaX, float sigmaY,
                                                  SKImageFilter input = null,
                                                  SKImageFilter.CropRect cropRect = null);
```

メソッドには 2 つのシグマ値&mdash;水平方向と垂直方向の 2 つ目のぼかしエクステントの最初。 省略可能な 3 番目の引数として別のイメージ フィルターを指定することでイメージ フィルターを連鎖させることができます。 トリミングの四角形を指定することもできます。

**イメージをぼかす実験**ページで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 2 つ`Slider`できるビューを試してさまざまなレベルのぼかし。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.ImageBlurExperimentPage"
             Title="Image Blur Experiment">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="sigmaXSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaXSlider},
                              Path=Value,
                              StringFormat='Sigma X = {0:F1}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="sigmaYSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaYSlider},
                              Path=Value,
                              StringFormat='Sigma Y = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

分離コード ファイルで使用する 2 つ`Slider`を呼び出す値`SKImageFilter.CreateBlur`の`SKPaint`テキストとビットマップの両方を表示するために使用するオブジェクト。


```csharp
public partial class ImageBlurExperimentPage : ContentPage
{
    const string TEXT = "Blur My Text";

    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                            typeof(MaskBlurExperimentPage),
                            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public ImageBlurExperimentPage ()
    {
        InitializeComponent ();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Pink);

        // Get values from sliders
        float sigmaX = (float)sigmaXSlider.Value;
        float sigmaY = (float)sigmaYSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = (info.Width - 100) / (TEXT.Length / 2);
            paint.ImageFilter = SKImageFilter.CreateBlur(sigmaX, sigmaY);

            // Get text bounds and calculate display rectangle
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);
            SKRect textRect = new SKRect(0, 0, info.Width, textBounds.Height + 50);

            // Center the text in the display rectangle
            float xText = textRect.Width / 2 - textBounds.MidX;
            float yText = textRect.Height / 2 - textBounds.MidY;

            canvas.DrawText(TEXT, xText, yText, paint);

            // Calculate rectangle for bitmap
            SKRect bitmapRect = new SKRect(0, textRect.Bottom, info.Width, info.Height);
            bitmapRect.Inflate(-50, -50);

            canvas.DrawBitmap(bitmap, bitmapRect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

次の 3 つのスクリーン ショットのさまざまな設定を表示する、`sigmaX`と`sigmaY`設定。

[![イメージのぼかし実験](image-filters-images/ImageBlurExperiment.png "ぼかし実験の画像")](image-filters-images/ImageBlurExperiment-Large.png#lightbox)

ぼかしを異なる画面サイズと解像度の間で一貫性のあるさせるには、次のように設定します。`sigmaX`と`sigmaY`ぼかしが適用されるイメージのレンダリングされたピクセル サイズに比例した値にします。

## <a name="drop-shadow"></a>ドロップ シャドウ

[ `SKImageFilter.CreateDropShadow` ](xref:SkiaSharp.SKImageFilter.CreateDropShadow*)静的メソッドを作成、`SKImageFilter`ドロップ シャドウのオブジェクト。

```csharp
public static SKImageFilter CreateDropShadow (float dx, float dy,
                                              float sigmaX, float sigmaY,
                                              SKColor color,
                                              SKDropShadowImageFilterShadowMode shadowMode,
                                              SKImageFilter input = null,
                                              SKImageFilter.CropRect cropRect = null);
```

このオブジェクトを設定、`ImageFilter`のプロパティ、`SKPaint`は、その背後にあるドロップ シャドウがあるオブジェクト、およびそのオブジェクトを使用して描画したものです。

`dx`と`dy`パラメーターは、グラフィカル オブジェクトからのピクセル単位でシャドウの水平および垂直オフセットを示します。 2D グラフィックスの形式では、これら両方の引数が正の値以下とグラフィカル オブジェクトの右側に影を配置するがあることを意味します。 左上から光源を前提としています。

`sigmaX`と`sigmaY`パラメーターは要素がドロップ シャドウのぼかし効果が。

`color`パラメーターがドロップ シャドウの色。 これは、`SKColor`値は、透明度を含めることができます。 色の値の 1 つの方法としては`SKColors.Black.WithAlpha(0x80)`を任意の背景の色を暗くします。

最後の 2 つのパラメーターは省略可能です。

**ドロップ シャドウの実験**の値を試すことができますをプログラム`dx`、 `dy`、 `sigmaX`、および`sigmaY`影付きのテキスト文字列を表示します。 XAML ファイルには、4 つがインスタンス化します`Slider`これらの値を設定するビュー。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.DropShadowExperimentPage"
             Title="Drop Shadow Experiment">
    <ContentPage.Resources>
        <Style TargetType="Slider">
            <Setter Property="Margin" Value="10, 0" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="HorizontalTextAlignment" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="dxSlider"
                Minimum="-20"
                Maximum="20"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference dxSlider},
                              Path=Value,
                              StringFormat='Horizontal offset = {0:F1}'}" />

        <Slider x:Name="dySlider"
                Minimum="-20"
                Maximum="20"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference dySlider},
                              Path=Value,
                              StringFormat='Vertical offset = {0:F1}'}" />

        <Slider x:Name="sigmaXSlider"
                Maximum="10"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaXSlider},
                              Path=Value,
                              StringFormat='Sigma X = {0:F1}'}" />

        <Slider x:Name="sigmaYSlider"
                Maximum="10"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaYSlider},
                              Path=Value,
                              StringFormat='Sigma Y = {0:F1}'}" />
    </StackLayout>
</ContentPage>
```

分離コード ファイルでは、これらの値を使用して、青色のテキスト文字列で赤のドロップ シャドウの作成します。

```csharp
public partial class DropShadowExperimentPage : ContentPage
{
    const string TEXT = "Drop Shadow";

    public DropShadowExperimentPage ()
    {
        InitializeComponent ();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get values from sliders
        float dx = (float)dxSlider.Value;
        float dy = (float)dySlider.Value;
        float sigmaX = (float)sigmaXSlider.Value;
        float sigmaY = (float)sigmaYSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = info.Width / 7;
            paint.Color = SKColors.Blue;
            paint.ImageFilter = SKImageFilter.CreateDropShadow(
                                    dx,
                                    dy,
                                    sigmaX,
                                    sigmaY,
                                    SKColors.Red,
                                    SKDropShadowImageFilterShadowMode.DrawShadowAndForeground);

            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Center the text in the display rectangle
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

実行中のプログラムを次に示します。

[![ドロップ シャドウ実験](image-filters-images/DropShadowExperiment.png "ドロップ シャドウの実験")](image-filters-images/DropShadowExperiment-Large.png#lightbox)

右端のユニバーサル Windows プラットフォームのスクリーン ショットに負のオフセット値の上部およびテキストの左側に表示するシャドウが発生します。 これは、コンピューター_グラフィックスの規則が右下で光源を示しています。 ないよう、何らかの方法で問題がおそらくため影が非常に画像がぼやけるも行われます、ほとんどのドロップ シャドウよりもより装飾ようです。

## <a name="lighting-effects"></a>光源の効果

`SKImageFilter`クラスと同様の名前およびここの複雑さを昇順でパラメーターを持つ 6 つのメソッドを定義します。

- [`CreateDistantLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateDistantLitDiffuse*)
- [`CreateDistantLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateDistantLitSpecular*)
- [`CreatePointLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreatePointLitDiffuse*)
- [`CreatePointLitSpecular`](xref:SkiaSharp.SKImageFilter.CreatePointLitSpecular*)
- [`CreateSpotLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateSpotLitDiffuse*)
- [`CreateSpotLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateSpotLitSpecular*)

これらのメソッドは、3 次元サーフェスにさまざまな種類の光の効果を模倣するイメージのフィルターを作成します。 結果のイメージのフィルターでは、または 3D 空間を管理者特権またはくぼんだを表示するこれらのオブジェクトが発生することができます、鏡面ハイライトが存在している場合、2 次元のオブジェクトが点灯します。

`Distant`ライト メソッドでは、光が遠距離から取得されると仮定します。 わかりやすいオブジェクトは、するために、ライトが地球の小さい領域での Sun と同様に、3 D 空間で一貫性のある 1 つの方向と見なされます。 `Point`ライト メソッドは、すべての方向に光を放射 3 D 空間で電球を模倣します。 `Spot`ライトの位置と方向を懐中電灯と同様の両方。

場所と 3 次元空間で指示の両方の値を指定、 [ `SKPoint3` ](xref:SkiaSharp.SKPoint3)のような構造体は、`SKPoint`がという名前の 3 つのプロパティを持つ`X`、`Y`と`Z`します。

これらのメソッドのパラメーターの複雑さと数ことに実験を困難です。 を開始するため、**離れた光実験**ページへのパラメーターを試すことができます、`CreateDistantLightDiffuse`メソッド。

```csharp
public static SKImageFilter CreateDistantLitDiffuse (SKPoint3 direction,
                                                     SKColor lightColor,
                                                     float surfaceScale,
                                                     float kd,
                                                     SKImageFilter input = null,
                                                     SKImageFilter.CropRect cropRect = null);
```

ページには、最後の 2 つの省略可能なパラメーターを使用しません。

次の 3 つ`Slider`、XAML でビュー ファイルを選択するように、`Z`の座標、`SKPoint3`値、`surfaceScale`パラメーター、および`kd`「拡散光定数」として、API のドキュメントで定義されているパラメーター。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaLightExperiment.MainPage"
             Title="Distant Light Experiment">

    <StackLayout>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           VerticalOptions="FillAndExpand" />

        <Slider x:Name="zSlider"
                Minimum="-10"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zSlider},
                              Path=Value,
                              StringFormat='Z = {0:F0}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="surfaceScaleSlider"
                Minimum="-1"
                Maximum="1"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference surfaceScaleSlider},
                              Path=Value,
                              StringFormat='Surface Scale = {0:F1}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="lightConstantSlider"
                Minimum="-1"
                Maximum="1"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference lightConstantSlider},
                              Path=Value,
                              StringFormat='Light Constant = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

分離コード ファイルでは、これら 3 つの値を取得し、それらを使用してテキスト文字列を表示するイメージ フィルターを作成します。

```csharp
public partial class DistantLightExperimentPage : ContentPage
{
    const string TEXT = "Lighting";

    public DistantLightExperimentPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float z = (float)zSlider.Value;
        float surfaceScale = (float)surfaceScaleSlider.Value;
        float lightConstant = (float)lightConstantSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.IsAntialias = true;

            // Size text to 90% of canvas width
            paint.TextSize = 100;
            float textWidth = paint.MeasureText(TEXT);
            paint.TextSize *= 0.9f * info.Width / textWidth;

            // Find coordinates to center text
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            float xText = info.Rect.MidX - textBounds.MidX;
            float yText = info.Rect.MidY - textBounds.MidY;

            // Create distant light image filter
            paint.ImageFilter = SKImageFilter.CreateDistantLitDiffuse(
                                    new SKPoint3(2, 3, z),
                                    SKColors.White,
                                    surfaceScale,
                                    lightConstant);

            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

最初の引数`SKImageFilter.CreateDistantLitDiffuse`光の方向です。 正の X と Y 座標は、光が、右、下に鋭くであることを示します。 正の Z 座標が画面に位置します。 XAML ファイルでは、負の Z 値を選択できますが、確認するためにのみの動作: 概念的には、負の Z 座標が、画面の手前ポイント ライトを発生します。 何かの他、小さい負の値では、照明効果の動作が停止します。

`surfaceScale`引数の 1 に範囲は – 1。 (高いまたは低い値にはそれ以上影響をあるありません)。これらは、キャンバスの画面から (この例では、テキスト文字列) では、グラフィカル オブジェクトの移動距離を示す Z 軸方向の相対値です。 キャンバスの画面上のテキスト文字列を生成する負の値とそれをキャンバスにオブジェクトに正の値を使用します。

`lightConstant`値は正でなければなりません。 (プログラムにより、負の値が動作を停止する効果が発生することを確認できるようにします。)値が大きいほど濃いほど光が発生します。

これらの要因のバランスを取る、浮き出し効果を取得する必要されるときに有効`surfaceScale`(と同様に、iOS と Android のスクリーン ショット) が負の値を浮き彫りとされるときに有効`surfaceScale`が正の値として、右にある UWP スクリーン ショット。

[![離れた場所にある明るい実験](image-filters-images/DistantLightExperiment.png "離れたライト実験")](image-filters-images/DistantLightExperiment-Large.png#lightbox)

Android のスクリーン ショットでは、0 で、し、右下にのみ、光が指していることを意味の Z 値があります。 バック グラウンドが点灯はありませんし、画面のテキスト文字列のない点灯しているか。 光は、テキストの端を非常に微妙な効果にのみ影響します。

エンボスと浮き彫りテキストを別のアプローチは、情報の記事で示した[、変換の変換](../transforms/translate.md): 互いに若干異なる色のオフセットを 2 回で、テキスト文字列が表示されます。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
