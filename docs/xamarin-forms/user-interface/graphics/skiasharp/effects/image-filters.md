---
title: SkiaSharp イメージフィルター
description: 画像フィルターを使用して、ぼかしとドロップシャドウを作成する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 173E7B22-AEC8-4F12-B657-1C0CEE01AD63
author: davidbritch
ms.author: dabritch
ms.date: 08/27/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4f10d39ff9fb08897f12cf1991ddcd2d7793b695
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91564031"
---
# <a name="skiasharp-image-filters"></a>SkiaSharp イメージフィルター

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

イメージフィルターは、イメージを構成するすべてのカラービットのピクセルに作用する効果です。 これはマスクフィルターよりも汎用性があり、 [**SkiaSharp mask フィルター**](mask-filters.md)に関する記事で説明されているように、アルファチャネル上でのみ動作します。 イメージフィルターを使用するには、 [`ImageFilter`](xref:SkiaSharp.SKPaint.ImageFilter) クラスの `SKPaint` [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) 静的メソッドの1つを呼び出すことによって、のプロパティを、作成した型のオブジェクトに設定します。

マスクフィルターを理解する最善の方法は、これらの静的メソッドを試してみることです。 マスクフィルターを使用して、ビットマップ全体をぼかすことができます。

![ぼかしの例](image-filters-images/ImageFilterExample.png "ぼかしの例")

この記事では、イメージフィルターを使用してドロップシャドウを作成する方法と、エンボスと engraving の効果についても示します。

## <a name="blurring-vector-graphics-and-bitmaps"></a>ぼかしベクターグラフィックスとビットマップ

静的メソッドによって作成されたぼかし効果は、 [`SKImageFilter.CreateBlur`](xref:SkiaSharp.SKImageFilter.CreateBlur*) クラスのぼかしメソッドよりも大きな利点があり [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) ます。イメージフィルターでは、ビットマップ全体をぼかすことができます。 メソッドの構文は次のとおりです。

```csharp
public static SkiaSharp.SKImageFilter CreateBlur (float sigmaX, float sigmaY,
                                                  SKImageFilter input = null,
                                                  SKImageFilter.CropRect cropRect = null);
```

メソッドには、ぼかしの範囲の最初の方向が1つ、垂直方向が2番目のシグマ値が2つあり &mdash; ます。 別のイメージフィルターを省略可能な3番目の引数として指定することで、イメージフィルターを連鎖させることができます。 トリミング四角形を指定することもできます。

[**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)の**Image ぼかす実験**ページには、 `Slider` さまざまなぼかしレベルの設定を試すことができる2つのビューが含まれています。

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

分離コードファイルは、2つの値を使用して `Slider` `SKImageFilter.CreateBlur` 、 `SKPaint` テキストとビットマップの両方を表示するために使用されるオブジェクトを呼び出します。

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

次の3つのスクリーンショットは、設定と設定のさまざまな設定を示してい `sigmaX` `sigmaY` ます。

[![イメージのぼかし実験](image-filters-images/ImageBlurExperiment.png "イメージのぼかし実験")](image-filters-images/ImageBlurExperiment-Large.png#lightbox)

さまざまな表示サイズと解像度の間でぼかしを統一するには、 `sigmaX` と `sigmaY` を、ぼかしが適用されるイメージの描画ピクセルサイズに比例する値に設定します。

## <a name="drop-shadow"></a>ドロップ シャドウ

[`SKImageFilter.CreateDropShadow`](xref:SkiaSharp.SKImageFilter.CreateDropShadow*)静的メソッドは、 `SKImageFilter` ドロップシャドウのオブジェクトを作成します。

```csharp
public static SKImageFilter CreateDropShadow (float dx, float dy,
                                              float sigmaX, float sigmaY,
                                              SKColor color,
                                              SKDropShadowImageFilterShadowMode shadowMode,
                                              SKImageFilter input = null,
                                              SKImageFilter.CropRect cropRect = null);
```

このオブジェクトを `ImageFilter` オブジェクトのプロパティに設定する `SKPaint` と、そのオブジェクトを使用して描画したすべてのものには、背後に影が付きます。

`dx`パラメーターと `dy` パラメーターは、グラフィカルオブジェクトの影の水平方向と垂直方向のオフセットをピクセル単位で示します。 2D グラフィックスの規則は、左上から光源を使用することを前提としています。これは、これらの両方の引数が、グラフィックオブジェクトの下および右側に影を配置するために正である必要があることを意味します。

`sigmaX`およびパラメーターは、 `sigmaY` ドロップシャドウのぼかし要因です。

パラメーターは、 `color` ドロップシャドウの色です。 この `SKColor` 値には、透明度を含めることができます。 1つの可能性として、色の `SKColors.Black.WithAlpha(0x80)` 背景を暗くする色の値があります。

最後の2つのパラメーターは省略可能です。

**ドロップシャドウ実験**プログラムを使用すると、、、、およびの値を試して `dx` 、 `dy` `sigmaX` `sigmaY` テキスト文字列とドロップシャドウを表示できます。 XAML ファイルは、 `Slider` これらの値を設定するために4つのビューをインスタンス化します。

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

分離コードファイルでは、これらの値を使用して、青いテキスト文字列に赤色のドロップシャドウを作成します。

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

[![シャドウ実験のドロップ](image-filters-images/DropShadowExperiment.png "シャドウ実験のドロップ")](image-filters-images/DropShadowExperiment-Large.png#lightbox)

右端にあるユニバーサル Windows プラットフォームスクリーンショットの負のオフセット値により、影がテキストの上と左に表示されます。 これにより右下に光源が提示されますが、これはコンピューターグラフィックスの規則ではありません。 しかし、影も非常にぼやけており、ほとんどのドロップシャドウよりも装飾が多くなるため、何らかの問題が発生することはありません。

## <a name="lighting-effects"></a>照明効果

クラスは、 `SKImageFilter` 類似した名前とパラメーターを持つ6つのメソッドを定義します。これについては、複雑さが増します。

- [`CreateDistantLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateDistantLitDiffuse*)
- [`CreateDistantLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateDistantLitSpecular*)
- [`CreatePointLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreatePointLitDiffuse*)
- [`CreatePointLitSpecular`](xref:SkiaSharp.SKImageFilter.CreatePointLitSpecular*)
- [`CreateSpotLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateSpotLitDiffuse*)
- [`CreateSpotLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateSpotLitSpecular*)

これらのメソッドは、3次元サーフェイス上のさまざまな種類のライトの効果を模倣するイメージフィルターを作成します。 結果の画像フィルターは、2次元オブジェクトが3D 空間に存在しているかのように、これらのオブジェクトが昇格または埋め込みになるか、反射の強調表示を使用して表示されることがあります。

`Distant`ライトメソッドは、光が遠く離れていることを前提としています。 オブジェクトを照明するために、光は地球の小さな部分の太陽と同じように、3D 空間で1つの一貫した方向を指していると見なされます。 ライト `Point` メソッドは、すべての方向に光を発する3d 空間に配置された電球を模倣します。 `Spot`光の位置と方向は、懐中電灯のようになります。

3D 空間内の位置と方向は、どちらも構造の値と共に指定されます [`SKPoint3`](xref:SkiaSharp.SKPoint3) 。これはと似ていますが、、、 `SKPoint` およびという3つのプロパティが `X` `Y` `Z` あります。

これらのメソッドのパラメーターの数と複雑さにより、実験が困難になります。 作業を開始するために、 **遠くの薄い実験** ページでメソッドのパラメーターを試してみることができ `CreateDistantLightDiffuse` ます。

```csharp
public static SKImageFilter CreateDistantLitDiffuse (SKPoint3 direction,
                                                     SKColor lightColor,
                                                     float surfaceScale,
                                                     float kd,
                                                     SKImageFilter input = null,
                                                     SKImageFilter.CropRect cropRect = null);
```

このページでは、最後の2つの省略可能なパラメーターは使用されません。

`Slider`XAML ファイル内の3つのビューを使用すると、 `Z` `SKPoint3` `surfaceScale` `kd` API ドキュメントで "拡散光定数" として定義されている値、パラメーター、およびパラメーターの座標を選択できます。

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

分離コードファイルは、これら3つの値を取得し、それらを使用して、テキスト文字列を表示するイメージフィルターを作成します。

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

の最初の引数 `SKImageFilter.CreateDistantLitDiffuse` はライトの方向です。 正の X 座標と Y 座標は、ライトが右と下を指していることを示します。 正の Z 座標は画面をポイントします。 XAML ファイルでは負の Z 値を選択することができますが、これは実際には何が起こるかを確認できるようにするためだけです。概念的には、負の Z 座標によって、画面から光が出ます。 それ以外の負の値については、光源効果は動作を停止します。

引数は、 `surfaceScale` -1 ~ 1 の範囲で指定できます。 (値が大きいか小さい場合は、それ以上の効果はありません)。これらは、キャンバス画面からのグラフィカルオブジェクト (この場合はテキスト文字列) の移動を示す、Z 軸の相対値です。 負の値を使用して、キャンバスの表面の上にテキスト文字列を上げ、正の値を使用してキャンバスに挿入します。

値は正の値である `lightConstant` 必要があります。 (プログラムによって負の値が許可されるため、効果が動作しなくなる可能性があります)。値が大きいほど、より強い光が発生します。

これらの要素 `surfaceScale` は、が負の場合 (iOS や Android のスクリーンショットと同様) にエンボス効果を得るためにバランスをとることができ `surfaceScale` ます。また、が正の場合は、右側の UWP スクリーンショットのように、浮き彫り効果が得られます。

[![遠くの軽実験](image-filters-images/DistantLightExperiment.png "遠くの軽実験")](image-filters-images/DistantLightExperiment-Large.png#lightbox)

Android のスクリーンショットの Z 値は0です。これは、ライトが下向きと右を指していることを意味します。 背景は照明されず、テキスト文字列の表面は点灯しません。 ライトは、非常に微妙な効果を実現するためにテキストの端にのみ影響します。

エンボスおよび浮き彫りのテキストに対する別の方法として、「 [変換変換](../transforms/translate.md)」の記事で説明されているように、テキスト文字列は異なる色で2回表示されます。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)