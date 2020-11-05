---
title: SkiaSharp の透明度
description: 透明度を使用して、複数のオブジェクトを1つのシーンに結合します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B62F9487-C30E-4C63-BAB1-4C091FF50378
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 837d58508c3f5b14c4c36a867a2aa974a5bf397c
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374317"
---
# <a name="skiasharp-transparency"></a>SkiaSharp の透明度

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

既に説明したように、クラスには [`SKPaint`](xref:SkiaSharp.SKPaint) 型のプロパティが含まれてい [`Color`](xref:SkiaSharp.SKPaint.Color) [`SKColor`](xref:SkiaSharp.SKColor) ます。 `SKColor` にはアルファチャネルが含まれているため、値を使用して色を指定すると、 `SKColor` 部分的に透明になることがあります。 

いくつかの透過性は、 [**SkiaSharp 記事の基本的なアニメーション**](animation.md) に示されています。 この記事では、複数のオブジェクトを1つのシーン ( _ブレンド_ とも呼ばれます) で組み合わせることにより、透過性について少し掘り下げていきます。 より高度なブレンド手法については、「 [**SkiaSharp シェーダー**](../effects/shaders/index.md) 」セクションの記事で説明されています。

4つのパラメーターを持つコンストラクターを使用して最初に色を作成するときに、透明度レベルを設定でき [`SKColor`](xref:SkiaSharp.SKColor.%23ctor(System.Byte,System.Byte,System.Byte,System.Byte)) ます。

```csharp
SKColor (byte red, byte green, byte blue, byte alpha);
```

アルファ値0は完全に透明で、アルファ値0xFF は完全に不透明です。 これらの2つの値の間の値は、部分的に透明な色を作成します。

さらに、には、 `SKColor` [`WithAlpha`](xref:SkiaSharp.SKColor.WithAlpha*) 既存の色から、指定したアルファレベルで新しい色を作成する便利なメソッドが定義されています。

```csharp
SKColor halfTransparentBlue = SKColors.Blue.WithAlpha(0x80);
```

部分的に透明なテキストの使用方法については、 [**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルのコードページを **参照** してください。 このページでは、値に透明度を組み込むことによって、2つのテキスト文字列をフェードアウトし `SKColor` ます。

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

フィールドは、 `transparency` 0 から1に変化し、決まる正弦波のリズムで再び戻るようにアニメーション化されます。 最初のテキスト文字列は、1からの値を減算することによって計算されたアルファ値と共に表示され `transparency` ます。

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * (1 - transparency)));
```

メソッドは、 [`WithAlpha`](xref:SkiaSharp.SKColor.WithAlpha*) 既存の色にアルファコンポーネントを設定 `SKColors.Blue` します。ここではです。 2番目のテキスト文字列は、値自体から計算されたアルファ値を使用し `transparency` ます。

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * transparency));
```

アニメーションでは、2つの単語の間で代替が実行され、ユーザーが "コードをさらに" (または "より多くのコードを要求する") に追加されます。

[![コードの追加コード](transparency-images/CodeMoreCode.png "コードの追加コード")](transparency-images/CodeMoreCode-Large.png#lightbox)

[**SkiaSharp のビットマップの基礎**](bitmaps.md)に関する前の記事では、のいずれかのメソッドを使用してビットマップを表示する方法を説明しました [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap*) `SKCanvas` 。 すべてのメソッドには、 `DrawBitmap` `SKPaint` 最後のパラメーターとしてオブジェクトが含まれています。 既定では、このパラメーターはに設定されて `null` おり、無視することができます。 

または、 `Color` このオブジェクトのプロパティを設定し `SKPaint` て、ある程度透明度のビットマップを表示することもできます。 のプロパティで透明度のレベルを設定すると、 `Color` `SKPaint` ビットマップをフェードインまたはフェードアウトしたり、ビットマップを別のビットマップに分割したりすることができます。 

ビットマップの透明度は、[ **ビットマップのディゾルブ** ] ページで示されています。 XAML ファイルは、とをインスタンス化し `SKCanvasView` `Slider` ます。

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

分離コードファイルは、2つのビットマップリソースを読み込みます。 これらのビットマップのサイズは同じではありませんが、縦横比は同じです。

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

`Color`オブジェクトのプロパティ `SKPaint` は、2つのビットマップに対して2つの補完的なアルファレベルに設定されます。 ビットマップでを使用する場合 `SKPaint` 、値の残りの部分がどのようなものであるかは関係ありません `Color` 。 重要なのは、アルファチャネルだけです。 このコードは、単 `WithAlpha` にプロパティの既定値に対してメソッドを呼び出し `Color` ます。

`Slider`1 つのビットマップ間でのディゾルブの移動:

[![ビットマップディゾルブ](transparency-images/BitmapDissolve.png "ビットマップディゾルブ")](transparency-images/BitmapDissolve-Large.png#lightbox)

これまでのいくつかの記事では、SkiaSharp を使用して、テキスト、円、楕円、角丸四角形、ビットマップを描画する方法を説明しました。 次の手順では、 [SkiaSharp の行とパス](../paths/index.md) を使用して、接続された線をグラフィックパスに描画する方法を学習します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)