---
title: SkiaSharp の透過性
description: 透明度を使用すると、1 つのシーンで複数のオブジェクトを結合します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B62F9487-C30E-4C63-BAB1-4C091FF50378
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 577eb19106ffa0ebd19c54aeeb155a9c6c85feac
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53062044"
---
# <a name="skiasharp-transparency"></a>SkiaSharp の透過性

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

で説明したように、 [ `SKPaint` ](xref:SkiaSharp.SKPaint)クラスが含まれています、 [ `Color` ](xref:SkiaSharp.SKPaint.Color)型のプロパティ[ `SKColor`](xref:SkiaSharp.SKColor)します。 `SKColor` アルファ チャネルにはがカラーされるものが含まれています、`SKColor`値を部分的に透明にすることができます。 

いくつかの透明度で示されています、 [ **SkiaSharp で基本アニメーション**](animation.md)記事。 透過性とも呼ばれる手法、1 つのシーンで複数のオブジェクトを結合するに、この記事では少し深く_ブレンド_します。 記事では、描画手法を高度な複数記載されて、 [ **SkiaSharp シェーダー** ](../effects/shaders/index.md)セクション。

透過性レベルを設定するには、最初に 4 つのパラメーターを使用して色を作成するときに[ `SKColor` ](xref:SkiaSharp.SKColor.%23ctor(System.Byte,System.Byte,System.Byte,System.Byte))コンス トラクター。

```csharp
SKColor (byte red, byte green, byte blue, byte alpha);
```

アルファ値は 0 が完全に透明と 0 xff のアルファ値が完全に不透明です。 これら 2 つの両極端の間の値は、部分的に透明な色を作成します。

さらに、`SKColor`便利な定義[ `WithAlpha` ](xref:SkiaSharp.SKColor.WithAlpha*)既存の色からがアルファ レベルを指定して、新しい色を作成するメソッド。

```csharp
SKColor halfTransparentBlue = SKColors.Blue.WithAlpha(0x80);
```

部分的に透明なテキストの使用方法については、**コードよりコード**ページで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)サンプル。 このページ 2 つのテキスト文字列でフェードインおよびフェードアウトでの透過性を組み込むことにより、`SKColor`値。

```csharp
public class CodeMoreCodePage : ContentPage
{
    SKCanvasView canvasView;
    bool isAnimating;
    Stopwatch stopwatch = new Stopwatch();
    double transparency;

    public CodeMoreCodePage ()
    {
        Title = "Code More Code";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 5;     // seconds
        double progress = stopwatch.Elapsed.TotalSeconds % duration / duration;
        transparency = 0.5 * (1 + Math.Sin(progress * 2 * Math.PI));
        canvasView.InvalidateSurface();

        return isAnimating;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        const string TEXT1 = "CODE";
        const string TEXT2 = "MORE";

        using (SKPaint paint = new SKPaint())
        {
            // Set text width to fit in width of canvas
            paint.TextSize = 100;
            float textWidth = paint.MeasureText(TEXT1);
            paint.TextSize *= 0.9f * info.Width / textWidth;

            // Center first text string
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT1, ref textBounds);

            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * (1 - transparency)));
            canvas.DrawText(TEXT1, xText, yText, paint);

            // Center second text string
            textBounds = new SKRect();
            paint.MeasureText(TEXT2, ref textBounds);

            xText = info.Width / 2 - textBounds.MidX;
            yText = info.Height / 2 - textBounds.MidY;

            paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * transparency));
            canvas.DrawText(TEXT2, xText, yText, paint);
        }
    }
}
```

`transparency`フィールドが 0 から 1 までさまざまの振幅のリズムに再びをアニメーション化されます。 引いて計算アルファ値を持つ最初のテキスト文字列が表示されます、 `transparency` 1 の値。

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * (1 - transparency)));
```

[ `WithAlpha` ](xref:SkiaSharp.SKColor.WithAlpha*)メソッドは、ここでは、既存の色のアルファ コンポーネントを設定`SKColors.Blue`します。 2 番目のテキスト文字列から計算される、アルファ値を使用して、`transparency`値です。

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * transparency));
```

アニメーションは、2 つの単語、またはの間に「コードの詳細は」ユーザーのアクション (おそらく「コード」を要求する) 代替します。

[![さらにコードをコード](transparency-images/CodeMoreCode.png "より多くのコードのコード")](transparency-images/CodeMoreCode-Large.png#lightbox)


に関する前回のコラムで[ **SkiaSharp のビットマップ基本**](bitmaps.md)のいずれかを使用してビットマップを表示する方法を説明する、 [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap*)メソッドの`SKCanvas`します。 すべての`DrawBitmap`メソッドには、`SKPaint`最後のパラメーターとしてオブジェクト。 既定では、このパラメーターに設定`null`し、これを無視することができます。 

または、設定、`Color`プロパティのこの`SKPaint`ビットマップを透過性の程度を表示するオブジェクト。 透過性のレベルの設定、`Color`プロパティの`SKPaint`ビットマップのフェードインとフェードアウトをしたり、別に 1 つのビットマップを削除することができます。 

ビットマップの透過性の説明については、**ビットマップ ディゾルブ**ページ。 XAML ファイルのインスタンスを作成、`SKCanvasView`と`Slider`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BitmapDissolvePage"
             Title="Bitmap Dissolve">
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="progressSlider"
                Margin="10"
                ValueChanged="OnSliderValueChanged" />
    </StackLayout>
</ContentPage>
```

分離コード ファイルは、2 つのビットマップ リソースを読み込みます。 これらのビットマップは同じサイズではありませんが、同じの縦横比は。

```csharp
public partial class BitmapDissolvePage : ContentPage
{
    SKBitmap bitmap1;
    SKBitmap bitmap2;

    public BitmapDissolvePage()
    {
        InitializeComponent();

        // Load two bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(
                                "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg"))
        {
            bitmap1 = SKBitmap.Decode(stream);
        }
        using (Stream stream = assembly.GetManifestResourceStream(
                                "SkiaSharpFormsDemos.Media.FacePalm.jpg"))
        {
            bitmap2 = SKBitmap.Decode(stream);
        }
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

        // Find rectangle to fit bitmap
        float scale = Math.Min((float)info.Width / bitmap1.Width,
                                (float)info.Height / bitmap1.Height);
        SKRect rect = SKRect.Create(scale * bitmap1.Width,
                                    scale * bitmap1.Height);
        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Get progress value from Slider
        float progress = (float)progressSlider.Value;

        // Display two bitmaps with transparency
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = paint.Color.WithAlpha((byte)(0xFF * (1 - progress)));
            canvas.DrawBitmap(bitmap1, rect, paint);

            paint.Color = paint.Color.WithAlpha((byte)(0xFF * progress));
            canvas.DrawBitmap(bitmap2, rect, paint);
        }
    }
}
```

`Color`のプロパティ、`SKPaint`オブジェクトが 2 つのビットマップの 2 つの補完的なアルファ レベルに設定します。 使用する場合`SKPaint`ビットマップが、どのような他の関係ありません、`Color`値します。 重要なは、アルファ チャネルです。 次のコードを呼び出すだけです、`WithAlpha`メソッドの既定値を`Color`プロパティ。

移動、`Slider`ディゾルブ 1 つのビットマップと、その他の間。

[![ディザー ビットマップ](transparency-images/BitmapDissolve.png "ディザー ビットマップ")](transparency-images/BitmapDissolve-Large.png#lightbox)

過去のいくつかの記事では、SkiaSharp を使用して、テキスト、円、楕円、角の丸い四角形、およびビットマップを描画する方法を説明しました。 次の手順は[SkiaSharp の線とパス](../paths/index.md)でグラフィック パスに接続されている直線を描画する方法が説明されます。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
