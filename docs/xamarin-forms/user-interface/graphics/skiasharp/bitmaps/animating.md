---
title: アニメーション化 (SkiaSharp ビットマップを)
description: 一連のビットマップを順番に表示し、アニメーション GIF ファイルをレンダリングすることにより、ビットマップアニメーションを実行する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD
author: davidbritch
ms.author: dabritch
ms.date: 07/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d9a96de5520a03b2ef51426be2c589c736ca2396
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562393"
---
# <a name="animating-skiasharp-bitmaps"></a>アニメーション化 (SkiaSharp ビットマップを)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

通常、SkiaSharp グラフィックスをアニメーション化するアプリケーション `InvalidateSurface` では、通常、 `SKCanvasView` 16 ミリ秒ごとに固定レートでが呼び出されます。 Surface を無効にすると、ハンドラーへの呼び出しがトリガーされ、 `PaintSurface` 表示が再描画されます。 ビジュアルが1秒間に60回再描画されると、スムーズにアニメーション化されているように見えます。

ただし、グラフィックスが複雑すぎて16ミリ秒でレンダリングできない場合は、アニメーションがちらつく可能性があります。 プログラマは、更新頻度を30倍または15倍に減らすことができますが、それだけでは不十分な場合もあります。 グラフィックが複雑になり、リアルタイムでレンダリングできないことがあります。

1つの解決策は、一連のビットマップでアニメーションの個々のフレームをレンダリングすることによって、アニメーションを事前に準備することです。 アニメーションを表示するために必要なのは、これらのビットマップを1秒間に60回順番に表示することだけです。

もちろん、これは大量のビットマップになる可能性がありますが、それが大きな予算3D アニメーションムービーの作成方法です。 3D グラフィックスは、非常に複雑すぎるため、リアルタイムでレンダリングできません。 各フレームをレンダリングするには、大量の処理時間が必要です。 ムービーを見ると、基本的には一連のビットマップであることがわかります。

SkiaSharp でも同様の操作を行うことができます。 この記事では、2種類のビットマップアニメーションについて説明します。 最初の例は、マンデルブロセットのアニメーションです。

![アニメーション化のサンプル](animating-images/AnimatingSample.png "アニメーション化のサンプル")

2番目の例は、SkiaSharp を使用してアニメーション GIF ファイルをレンダリングする方法を示しています。

## <a name="bitmap-animation"></a>ビットマップアニメーション

マンデルブロセットは視覚的に魅力的ますが、computionally 時間がかかります。 (マンデルブロセットとここで使用されている計算の詳細については、ページ666以降の[ _ Xamarin.Forms Mobile Apps の作成_に関する第20章](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)を参照してください。 次の説明は、背景知識があることを前提としています。)

[**マンデルブロアニメーション**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima)のサンプルでは、ビットマップアニメーションを使用して、マンデルブロセット内の固定ポイントの連続ズームをシミュレートします。 ズームインの後にズームアウトすると、サイクルが永久に繰り返されるか、プログラムが終了するまで繰り返されます。

このプログラムは、アプリケーションローカルストレージに格納される最大50のビットマップを作成することによって、このアニメーションを準備します。 各ビットマップには、前のビットマップとして、複雑な平面の幅と高さが半分含まれています。 (プログラムでは、これらのビットマップは、整数 _ズームレベル_を表すものと呼ばれます)。次に、ビットマップが順に表示されます。 各ビットマップの拡大縮小はアニメーション化され、あるビットマップから別のビットマップに滑らかに進むことができます。

_を使用した Mobile Apps の Xamarin.Forms 作成に関する_第20章で説明した最終的なプログラムと同様に、**マンデルブロアニメーション**でのマンデルブロセットの計算は、8つのパラメーターを持つ非同期メソッドです。 パラメーターには、複雑な中心点、およびその中心点を囲む複合平面の幅と高さが含まれます。 次の3つのパラメーターは、作成するビットマップのピクセル幅と高さ、および再帰計算の反復処理の最大数です。 `progress`パラメーターは、この計算の進行状況を表示するために使用されます。 `cancelToken`このプログラムでは、パラメーターは使用されません。

```csharp
static class Mandelbrot
{
    public static Task<BitmapInfo> CalculateAsync(Complex center,
                                                  double width, double height,
                                                  int pixelWidth, int pixelHeight,
                                                  int iterations,
                                                  IProgress<double> progress,
                                                  CancellationToken cancelToken)
    {
        return Task.Run(() =>
        {
            int[] iterationCounts = new int[pixelWidth * pixelHeight];
            int index = 0;

            for (int row = 0; row < pixelHeight; row++)
            {
                progress.Report((double)row / pixelHeight);
                cancelToken.ThrowIfCancellationRequested();

                double y = center.Imaginary + height / 2 - row * height / pixelHeight;

                for (int col = 0; col < pixelWidth; col++)
                {
                    double x = center.Real - width / 2 + col * width / pixelWidth;
                    Complex c = new Complex(x, y);

                    if ((c - new Complex(-1, 0)).Magnitude < 1.0 / 4)
                    {
                        iterationCounts[index++] = -1;
                    }
                    // http://www.reenigne.org/blog/algorithm-for-mandelbrot-cardioid/
                    else if (c.Magnitude * c.Magnitude * (8 * c.Magnitude * c.Magnitude - 3) < 3.0 / 32 - c.Real)
                    {
                        iterationCounts[index++] = -1;
                    }
                    else
                    {
                        Complex z = 0;
                        int iteration = 0;

                        do
                        {
                            z = z * z + c;
                            iteration++;
                        }
                        while (iteration < iterations && z.Magnitude < 2);

                        if (iteration == iterations)
                        {
                            iterationCounts[index++] = -1;
                        }
                        else
                        {
                            iterationCounts[index++] = iteration;
                        }
                    }
                }
            }
            return new BitmapInfo(pixelWidth, pixelHeight, iterationCounts);
        }, cancelToken);
    }
}
```

メソッドは、 `BitmapInfo` ビットマップを作成するための情報を提供する型のオブジェクトを返します。

```csharp
class BitmapInfo
{
    public BitmapInfo(int pixelWidth, int pixelHeight, int[] iterationCounts)
    {
        PixelWidth = pixelWidth;
        PixelHeight = pixelHeight;
        IterationCounts = iterationCounts;
    }

    public int PixelWidth { private set; get; }

    public int PixelHeight { private set; get; }

    public int[] IterationCounts { private set; get; }
}
```

**マンデルブロアニメーション**の XAML ファイルには、、、およびという2つのビューが含まれてい `Label` `ProgressBar` `Button` `SKCanvasView` ます。

```csharp
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="MandelAnima.MainPage"
             Title="Mandelbrot Animation">

    <StackLayout>
        <Label x:Name="statusLabel"
               HorizontalTextAlignment="Center" />
        <ProgressBar x:Name="progressBar" />

        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     Padding="5">
            <Label x:Name="storageLabel"
                   VerticalOptions="Center" />

            <Button x:Name="deleteButton"
                    Text="Delete All"
                    HorizontalOptions="EndAndExpand"
                    Clicked="OnDeleteButtonClicked" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

分離コードファイルは、次の3つの重要な定数とビットマップの配列を定義することから始まります。

```csharp
public partial class MainPage : ContentPage
{
    const int COUNT = 10;           // The number of bitmaps in the animation.
                                    // This can go up to 50!

    const int BITMAP_SIZE = 1000;   // Program uses square bitmaps exclusively

    // Uncomment just one of these, or define your own
    static readonly Complex center = new Complex(-1.17651152924355, 0.298520986549558);
    //   static readonly Complex center = new Complex(-0.774693089457127, 0.124226621261617);
    //   static readonly Complex center = new Complex(-0.556624880053304, 0.634696788141351);

    SKBitmap[] bitmaps = new SKBitmap[COUNT];   // array of bitmaps
    ···
}
```

場合によっては、 `COUNT` 値を50に変更して、アニメーションの全範囲を表示することをお勧めします。 50を超える値は役に立ちません。 ズームレベルが48の場合、または、倍精度浮動小数点数の解像度が、マンデルブロ集合の計算では不十分になります。 この問題については _、を使用した Xamarin.Forms Mobile Apps の作成_に関するページ684で説明されています。

この `center` 値は非常に重要です。 これは、アニメーションズームの焦点です。 このファイルの3つの値は、ページ684での_Mobile Apps の作成に関する Xamarin.Forms _ 20 章の3番目のスクリーンショットで使用されていますが、その章のプログラムを試して、独自の値のいずれかを取得することができます。

**マンデルブロアニメーション**のサンプルでは、これらの `COUNT` ビットマップをローカルアプリケーションストレージに格納します。 50ビットマップではデバイス上に 20 mb 以上のストレージが必要であるため、これらのビットマップが使用されているストレージの量を把握し、すべてを削除することが必要な場合があります。 クラスの下部には、次の2つのメソッドの目的が `MainPage` あります。

```csharp
public partial class MainPage : ContentPage
{
    ···
    void TallyBitmapSizes()
    {
        long fileSize = 0;

        foreach (string filename in Directory.EnumerateFiles(FolderPath()))
        {
            fileSize += new FileInfo(filename).Length;
        }

        storageLabel.Text = $"Total storage: {fileSize:N0} bytes";
    }

    void OnDeleteButtonClicked(object sender, EventArgs args)
    {
        foreach (string filepath in Directory.EnumerateFiles(FolderPath()))
        {
            File.Delete(filepath);
        }

        TallyBitmapSizes();
    }
}
```

プログラムが同じビットマップをメモリに保持しているため、ローカルストレージのビットマップを削除することができます。 ただし、次回プログラムを実行するときに、ビットマップを再作成する必要があります。

ローカルアプリケーションストレージに格納されているビットマップには、そのファイル名に値が組み込まれています。その `center` ため、設定を変更する `center` と、既存のビットマップはストレージで置き換えられず、領域を占有し続けます。

次に、 `MainPage` ファイル名の作成に使用するメソッドと、 `MakePixel` カラーコンポーネントに基づいてピクセル値を定義する方法を示します。

```csharp
public partial class MainPage : ContentPage
{
    ···
    // File path for storing each bitmap in local storage
    string FolderPath() =>
        Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);

    string FilePath(int zoomLevel) =>
        Path.Combine(FolderPath(),
                     String.Format("R{0}I{1}Z{2:D2}.png", center.Real, center.Imaginary, zoomLevel));

    // Form bitmap pixel for Rgba8888 format
    uint MakePixel(byte alpha, byte red, byte green, byte blue) =>
        (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

`zoomLevel` `FilePath` 0 から `COUNT` 定数に1を引いた値までの範囲のパラメーター。

`MainPage`コンストラクターは、メソッドを呼び出し `LoadAndStartAnimation` ます。

```csharp
public partial class MainPage : ContentPage
{
    ···
    public MainPage()
    {
        InitializeComponent();

        LoadAndStartAnimation();
    }
    ···
}
```

メソッドは、アプリケーションのローカルストレージにアクセスして、 `LoadAndStartAnimation` プログラムが以前に実行されたときに作成された可能性のあるビットマップを読み込む役割を担います。 `zoomLevel`0 からまでの値をループ処理 `COUNT` します。 ファイルが存在する場合は、そのファイルが `bitmaps` 配列に読み込まれます。 それ以外の場合は、を呼び出して、特定の値と値のビットマップを作成する必要があり `center` `zoomLevel` `Mandelbrot.CalculateAsync` ます。 このメソッドは、各ピクセルのイテレーション数を取得します。このメソッドは、次のように色に変換します。

```csharp
public partial class MainPage : ContentPage
{
    ···
    async void LoadAndStartAnimation()
    {
        // Show total bitmap storage
        TallyBitmapSizes();

        // Create progressReporter for async operation
        Progress<double> progressReporter =
            new Progress<double>((double progress) => progressBar.Progress = progress);

        // Create (unused) CancellationTokenSource for async operation
        CancellationTokenSource cancelTokenSource = new CancellationTokenSource();

        // Loop through all the zoom levels
        for (int zoomLevel = 0; zoomLevel < COUNT; zoomLevel++)
        {
            // If the file exists, load it
            if (File.Exists(FilePath(zoomLevel)))
            {
                statusLabel.Text = $"Loading bitmap for zoom level {zoomLevel}";

                using (Stream stream = File.OpenRead(FilePath(zoomLevel)))
                {
                    bitmaps[zoomLevel] = SKBitmap.Decode(stream);
                }
            }
            // Otherwise, create a new bitmap
            else
            {
                statusLabel.Text = $"Creating bitmap for zoom level {zoomLevel}";

                CancellationToken cancelToken = cancelTokenSource.Token;

                // Do the (generally lengthy) Mandelbrot calculation
                BitmapInfo bitmapInfo =
                    await Mandelbrot.CalculateAsync(center,
                                                    4 / Math.Pow(2, zoomLevel),
                                                    4 / Math.Pow(2, zoomLevel),
                                                    BITMAP_SIZE, BITMAP_SIZE,
                                                    (int)Math.Pow(2, 10), progressReporter, cancelToken);

                // Create bitmap & get pointer to the pixel bits
                SKBitmap bitmap = new SKBitmap(BITMAP_SIZE, BITMAP_SIZE, SKColorType.Rgba8888, SKAlphaType.Opaque);
                IntPtr basePtr = bitmap.GetPixels();

                // Set pixel bits to color based on iteration count
                for (int row = 0; row < bitmap.Width; row++)
                    for (int col = 0; col < bitmap.Height; col++)
                    {
                        int iterationCount = bitmapInfo.IterationCounts[row * bitmap.Width + col];
                        uint pixel = 0xFF000000;            // black

                        if (iterationCount != -1)
                        {
                            double proportion = (iterationCount / 32.0) % 1;
                            byte red = 0, green = 0, blue = 0;

                            if (proportion < 0.5)
                            {
                                red = (byte)(255 * (1 - 2 * proportion));
                                blue = (byte)(255 * 2 * proportion);
                            }
                            else
                            {
                                proportion = 2 * (proportion - 0.5);
                                green = (byte)(255 * proportion);
                                blue = (byte)(255 * (1 - proportion));
                            }

                            pixel = MakePixel(0xFF, red, green, blue);
                        }

                        // Calculate pointer to pixel
                        IntPtr pixelPtr = basePtr + 4 * (row * bitmap.Width + col);

                        unsafe     // requires compiling with unsafe flag
                        {
                            *(uint*)pixelPtr.ToPointer() = pixel;
                        }
                    }

                // Save as PNG file
                SKData data = SKImage.FromBitmap(bitmap).Encode();

                try
                {
                    File.WriteAllBytes(FilePath(zoomLevel), data.ToArray());
                }
                catch
                {
                    // Probably out of space, but just ignore
                }

                // Store in array
                bitmaps[zoomLevel] = bitmap;

                // Show new bitmap sizes
                TallyBitmapSizes();
            }

            // Display the bitmap
            bitmapIndex = zoomLevel;
            canvasView.InvalidateSurface();
        }

        // Now start the animation
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }
    ···
}
```

このプログラムでは、これらのビットマップはデバイスのフォトライブラリではなく、ローカルアプリケーションストレージに格納されていることに注意してください。 .NET Standard 2.0 ライブラリでは、 `File.OpenRead` このタスクに使い慣れたメソッドと方法を使用でき `File.WriteAllBytes` ます。

すべてのビットマップが作成されるか、メモリに読み込まれると、メソッドはオブジェクトを起動してを `Stopwatch` 呼び出し `Device.StartTimer` ます。 この `OnTimerTick` メソッドは、16ミリ秒ごとに呼び出されます。

`OnTimerTick` 0 ~ 6000 回の範囲の値をミリ秒単位で計算します。この値は、 `time` `COUNT` 各ビットマップの表示に対して6秒間になります。 値は `progress` 、値を使用して `Math.Sin` 決まる正弦波アニメーションを作成します。このアニメーションは、循環の開始時に遅くなり、方向を反転させると終了時に遅くなります。

`progress`値の範囲は 0 ~ `COUNT` です。 つまり、の整数部分 `progress` は配列のインデックスであり、 `bitmaps` の小数部は `progress` 特定のビットマップのズームレベルを示します。 これらの値はフィールドとフィールドに格納され、 `bitmapIndex` `bitmapProgress` `Label` および `Slider` XAML ファイルに表示されます。 `SKCanvasView`ビットマップディスプレイを更新するために、が無効になります。

```csharp
public partial class MainPage : ContentPage
{
    ···
    Stopwatch stopwatch = new Stopwatch();      // for the animation
    int bitmapIndex;
    double bitmapProgress = 0;
    ···
    bool OnTimerTick()
    {
        int cycle = 6000 * COUNT;       // total cycle length in milliseconds

        // Time in milliseconds from 0 to cycle
        int time = (int)(stopwatch.ElapsedMilliseconds % cycle);

        // Make it sinusoidal, including bitmap index and gradation between bitmaps
        double progress = COUNT * 0.5 * (1 + Math.Sin(2 * Math.PI * time / cycle - Math.PI / 2));

        // These are the field values that the PaintSurface handler uses
        bitmapIndex = (int)progress;
        bitmapProgress = progress - bitmapIndex;

        // It doesn't often happen that we get up to COUNT, but an exception would be raised
        if (bitmapIndex < COUNT)
        {
            // Show progress in UI
            statusLabel.Text = $"Displaying bitmap for zoom level {bitmapIndex}";
            progressBar.Progress = bitmapProgress;

            // Update the canvas
            canvasView.InvalidateSurface();
        }

        return true;
    }
    ···
}
```

最後に、 `PaintSurface` のハンドラーは、 `SKCanvasView` 縦横比を維持したままビットマップを可能な限り大きく表示する目的の四角形を計算します。 ソースの四角形は、値に基づいてい `bitmapProgress` ます。 `fraction`ここで計算される値は、が0の場合、ビットマップ全体を表示する場合は0、 `bitmapProgress` `bitmapProgress` ビットマップの幅と高さの半分を表示する場合は、効果的にズームインする場合は0.25 になります。

```csharp
public partial class MainPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        if (bitmaps[bitmapIndex] != null)
        {
            // Determine destination rect as square in canvas
            int dimension = Math.Min(info.Width, info.Height);
            float x = (info.Width - dimension) / 2;
            float y = (info.Height - dimension) / 2;
            SKRect destRect = new SKRect(x, y, x + dimension, y + dimension);

            // Calculate source rectangle based on fraction:
            //  bitmapProgress == 0: full bitmap
            //  bitmapProgress == 1: half of length and width of bitmap
            float fraction = 0.5f * (1 - (float)Math.Pow(2, -bitmapProgress));
            SKBitmap bitmap = bitmaps[bitmapIndex];
            int width = bitmap.Width;
            int height = bitmap.Height;
            SKRect sourceRect = new SKRect(fraction * width, fraction * height,
                                           (1 - fraction) * width, (1 - fraction) * height);

            // Display the bitmap
            canvas.DrawBitmap(bitmap, sourceRect, destRect);
        }
    }
    ···
}
```

実行中のプログラムを次に示します。

[![マンデルブロアニメーション](animating-images/MandelbrotAnimation.png "マンデルブロアニメーション")](animating-images/MandelbrotAnimation-Large.png#lightbox)

## <a name="gif-animation"></a>GIF アニメーション

グラフィックスインターチェンジ形式 (GIF) 仕様には、1つの GIF ファイルに連続して表示できるシーンの複数の連続フレームを含めることができる機能が含まれています (多くの場合ループで)。 これらのファイルは、 _アニメーション gif_と呼ばれます。 Web ブラウザーでは、アニメーション gif を再生できます。また、SkiaSharp を使用すると、アプリケーションはアニメーション GIF ファイルからフレームを抽出し、順番に表示することができます。

[SkiaSharpFormsDemos](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルには、DemonDeLuxe によって作成された**Newtons_cradle_animation_book_2.gif**という名前のアニメーション gif リソースが含まれており、Wikipedia の[ニュートンのクレードル](https://en.wikipedia.org/wiki/Newton%27s_cradle)ページからダウンロードされます。 **アニメーション gif**ページには、その情報を提供し、をインスタンス化する XAML ファイルが含まれてい `SKCanvasView` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.AnimatedGifPage"
             Title="Animated GIF">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="0"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="GIF file by DemonDeLuxe from Wikipedia Newton's Cradle page"
               Grid.Row="1"
               Margin="0, 5"
               HorizontalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

分離コードファイルは、アニメーション GIF ファイルを再生するために一般化されていません。 使用可能な情報の一部 (特に繰り返し回数) を無視し、ループ内でアニメーション GIF を再生します。

SkisSharp を使用してアニメーション GIF ファイルのフレームを抽出しても、どこにも記述されていないように見えるので、次のコードの説明は通常よりも詳細です。

アニメーション GIF ファイルのデコードはページのコンストラクター内で行われ、 `Stream` ビットマップを参照するオブジェクトを使用してオブジェクトとオブジェクトを作成する必要があり `SKManagedStream` [`SKCodec`](xref:SkiaSharp.SKCodec) ます。 プロパティは、 [`FrameCount`](xref:SkiaSharp.SKCodec.FrameCount) アニメーションを構成するフレームの数を示します。

これらのフレームは最終的に個々のビットマップとして保存されるため、コンストラクターはを使用して、 `FrameCount` `SKBitmap` `int` 各フレームの継続時間に対して2つの配列と、累積された期間を (アニメーションのロジックを容易にするために) 2 つの配列を割り当てます。

[`FrameInfo`](xref:SkiaSharp.SKCodec.FrameInfo)クラスのプロパティは、 `SKCodec` [`SKCodecFrameInfo`](xref:SkiaSharp.SKCodecFrameInfo) 各フレームに1つずつの値の配列ですが、このプログラムがその構造から取得するのは、フレームのがミリ秒単位であることだけです [`Duration`](xref:SkiaSharp.SKCodecFrameInfo.Duration) 。

`SKCodec` 型のという名前のプロパティを定義します [`Info`](xref:SkiaSharp.SKCodec.Info) [`SKImageInfo`](xref:SkiaSharp.SKImageInfo) が、その `SKImageInfo` 値は色の種類がであること (少なくともこのイメージでは) を示し `SKColorType.Index8` ます。これは、各ピクセルが色の種類のインデックスであることを意味します。 色テーブルを使用したかけるを避けるために、プログラムは [`Width`](xref:SkiaSharp.SKImageInfo.Width) その構造のおよび情報を使用し [`Height`](xref:SkiaSharp.SKImageInfo.Height) て、独自のフルカラー値を構築し `ImageInfo` ます。 それぞれ `SKBitmap` がから作成されます。

`GetPixels`のメソッドは、 `SKBitmap` `IntPtr` そのビットマップのピクセルビットを参照するを返します。 これらのピクセルビットはまだ設定されていません。 これ `IntPtr` は、のいずれかのメソッドに渡され [`GetPixels`](xref:SkiaSharp.SKCodec.GetPixels(SkiaSharp.SKImageInfo,System.IntPtr,SkiaSharp.SKCodecOptions)) `SKCodec` ます。 このメソッドは、GIF ファイルから、によって参照されるメモリ空間にフレームをコピーし `IntPtr` ます。 コンストラクターは、 [`SKCodecOptions`](xref:SkiaSharp.SKCodecOptions) フレーム番号を示します。

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;
    ···

    public AnimatedGifPage ()
    {
        InitializeComponent ();

        string resourceID = "SkiaSharpFormsDemos.Media.Newtons_cradle_animation_book_2.gif";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        using (SKCodec codec = SKCodec.Create(skStream))
        {
            // Get frame count and allocate bitmaps
            int frameCount = codec.FrameCount;
            bitmaps = new SKBitmap[frameCount];
            durations = new int[frameCount];
            accumulatedDurations = new int[frameCount];

            // Note: There's also a RepetitionCount property of SKCodec not used here

            // Loop through the frames
            for (int frame = 0; frame < frameCount; frame++)
            {
                // From the FrameInfo collection, get the duration of each frame
                durations[frame] = codec.FrameInfo[frame].Duration;

                // Create a full-color bitmap for each frame
                SKImageInfo imageInfo = code.new SKImageInfo(codec.Info.Width, codec.Info.Height);
                bitmaps[frame] = new SKBitmap(imageInfo);

                // Get the address of the pixels in that bitmap
                IntPtr pointer = bitmaps[frame].GetPixels();

                // Create an SKCodecOptions value to specify the frame
                SKCodecOptions codecOptions = new SKCodecOptions(frame, false);

                // Copy pixels from the frame into the bitmap
                codec.GetPixels(imageInfo, pointer, codecOptions);
            }

            // Sum up the total duration
            for (int frame = 0; frame < durations.Length; frame++)
            {
                totalDuration += durations[frame];
            }

            // Calculate the accumulated durations
            for (int frame = 0; frame < durations.Length; frame++)
            {
                accumulatedDurations[frame] = durations[frame] +
                    (frame == 0 ? 0 : accumulatedDurations[frame - 1]);
            }
        }
    }
    ···
}
```

値にかかわらず、は `IntPtr` `unsafe` `IntPtr` C# ポインター値に変換されないため、コードは必要ありません。

各フレームが抽出された後、コンストラクターはすべてのフレームの継続時間を合計し、累積期間を使用して別の配列を初期化します。

分離コードファイルの残りの部分は、アニメーション専用です。 `Device.StartTimer`メソッドは、タイマーを開始するために使用され `OnTimerTick` ます。コールバックはオブジェクトを使用して、 `Stopwatch` 経過時間をミリ秒単位で決定します。 累積された期間の配列をループ処理するだけで、現在のフレームを見つけることができます。

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;

    Stopwatch stopwatch = new Stopwatch();
    bool isAnimating;

    int currentFrame;
    ···
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
        int msec = (int)(stopwatch.ElapsedMilliseconds % totalDuration);
        int frame = 0;

        // Find the frame based on the elapsed time
        for (frame = 0; frame < accumulatedDurations.Length; frame++)
        {
            if (msec < accumulatedDurations[frame])
            {
                break;
            }
        }

        // Save in a field and invalidate the SKCanvasView.
        if (currentFrame != frame)
        {
            currentFrame = frame;
            canvasView.InvalidateSurface();
        }

        return isAnimating;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        // Get the bitmap and center it
        SKBitmap bitmap = bitmaps[currentFrame];
        canvas.DrawBitmap(bitmap,info.Rect, BitmapStretch.Uniform);
    }
}
```

変数が変更されるたびに、が無効になり、 `currentframe` `SKCanvasView` 新しいフレームが表示されます。

[![アニメーション GIF](animating-images/AnimatedGif.png "アニメーション GIF")](animating-images/AnimatedGif-Large.png#lightbox)

もちろん、アニメーションを表示するには、自分でプログラムを実行します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [マンデルブロアニメーション (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima)