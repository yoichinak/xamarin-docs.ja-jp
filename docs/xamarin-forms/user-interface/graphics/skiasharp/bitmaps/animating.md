---
title: SkiaSharp のビットマップをアニメーション化
description: ビットマップのアニメーションを実行するには、順番に一連のビットマップを表示し、アニメーション GIF ファイルを表示する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD
author: davidbritch
ms.author: dabritch
ms.date: 07/12/2018
ms.openlocfilehash: 604067ac853bd53707e059b7db4abf2cfade21ce
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668960"
---
# <a name="animating-skiasharp-bitmaps"></a>SkiaSharp のビットマップをアニメーション化

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

SkiaSharp のグラフィックスを一般にアニメーション化するアプリケーションを呼び出す`InvalidateSurface`上、 `SKCanvasView` 16 ミリ秒ごとに多くの場合、固定の率でします。 呼び出しをトリガーする、画面を無効化、`PaintSurface`画面を再描画するハンドラー。 ビジュアルは、1 秒あたり 60 回再描画される、スムーズにアニメーション化に表示されます。

ただし、グラフィックスが 16 ミリ秒で表示するには複雑すぎる場合は、アニメーションはちらつきのようになります。 プログラマが 30 倍、または 15 分、秒、リフレッシュ レートを削減することもできますが、さえでは不十分です。 グラフィックスことが単にレンダリングできませんをリアルタイムで複雑になる場合もあります。

1 つのソリューションでは、一連のビットマップのアニメーションの個別のフレームをレンダリングすることにより、アニメーションの事前準備します。 アニメーションを表示するには、順番に 1 秒あたり 60 回でこれらのビットマップを表示する必要はのみです。

もちろん、ビットマップの多くは可能性がありますが大きな-予算の 3D をアニメーション化されたムービーを作成します。 3D グラフィックはリアルタイムで表示するより複雑です。 各フレームを表示するために多くの処理時間が必要です。 一連のビットマップは基本的には、ビデオを視聴するときに表示します。

SkiaSharp と似たことを行うことができます。 この記事では、ビットマップのアニメーションの 2 つの種類を示します。 最初の例では、マンデルブロ集合のアニメーションを示します。

![サンプルをアニメーション化](animating-images/AnimatingSample.png "サンプルをアニメーション化")

2 番目の例では、アニメーション GIF ファイルを表示するために SkiaSharp を使用する方法を示します。

## <a name="bitmap-animation"></a>ビットマップのアニメーション

マンデルブロ集合とは、魅力的な視覚的が時間のかかる computionally です。 (マンデルブロ集合とここで使用する数学の詳細については、[の第 20 章_を Xamarin.Forms での Mobile Apps の作成_](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf) 666 ページで開始を参照してください。 次の説明を想定その背景知識。)

[**マンデルブロ アニメーション**](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/MandelAnima/)サンプルでは、ビットマップのアニメーションを使用して、マンデルブロ集合に固定小数点の継続的な拡大をシミュレートします。 ズーム アウト、後で拡大され、状態のままか、プログラムを終了するまで、サイクルが繰り返されます。

プログラムは、このアニメーションのまでアプリケーションのローカル ストレージに格納された 50 のビットマップを作成して準備します。 各ビットマップには、前のビットマップとしての半分の幅と複素平面の高さが含まれます。 (プログラムでは、これらのビットマップが整数型を表すと言います_ズーム レベル_。)。シーケンス内のビットマップが表示されます。 ビットマップごとのスケーリングは別に 1 つのビットマップからスムーズな進行を提供するアニメーション化します。

などの第 20 章で説明されているプログラムの最終_を Xamarin.Forms での Mobile Apps の作成_、マンデルブロ集合内の計算**マンデルブロ アニメーション**が 8 個の非同期メソッドパラメーター。 パラメーターには、複雑な中心および幅とその center 点を囲む複素平面の高さが含まれます。 次の 3 つのパラメーターは、ピクセル幅と、作成するビットマップの高さと再帰的な計算のイテレーションの最大数です。 `progress`パラメーターがこの計算の進行状況を表示するために使用します。 `cancelToken`パラメーターは、このプログラムでは使用されません。

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

型のオブジェクトを返します`BitmapInfo`ビットマップを作成するための情報を提供します。

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

**マンデルブロ アニメーション**XAML ファイルでは、2 つが含まれる`Label`ビュー、 `ProgressBar`、および`Button`だけでなく`SKCanvasView`:

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

分離コード ファイルは、3 つの重要な定数とビットマップの配列を定義することで始まります。

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

ある時点でおそらくたい変更、`COUNT`値を 50 に、アニメーションの完全な範囲を参照してください。 50 を超える値は便利です。 48 などのズーム レベルに倍精度浮動小数点数の解像度になります、マンデルブロ集合の計算が不足しています。 この問題の 684 ページで説明した_を Xamarin.Forms での Mobile Apps の作成_です。

`center`値が非常に重要です。 これは、アニメーションのズームのフォーカスです。 ファイルの 3 つの値を使用しての第 20 章の 3 つの最終的なスクリーン ショットで使用されている_を Xamarin.Forms での Mobile Apps の作成_ ページで、684 が独自の値のいずれかを考案するには、その章でプログラムを試すことができます。

**マンデルブロ アニメーション**サンプルでは、これらを格納`COUNT`ローカル アプリケーション記憶域のビットマップ。 50 のビットマップをこれらのビットマップが占有しているストレージの量を知りたい場合がありますのでお使いのデバイス上の記憶域の 20 のメガバイト数以上を必要とし、ある時点でそれらすべてを削除する可能性があります。 これら 2 つのメソッドの下部の目的は、`MainPage`クラス。

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

プログラムがそれらをメモリ内に保持するため、プログラムは、同じビットマップをアニメーション化する中には、ローカル ストレージ内のビットマップを削除することができます。 ビットマップを再作成する必要が次に、プログラムを実行したとき。

アプリケーションのローカル ストレージに格納されているビットマップを組み込む、 `center` 、ファイル名に値を変更する場合、`center`設定すると、既存のビットマップ、ストレージには置き換えられません、領域を占有し続けます。

ここでは、方法を`MainPage`、ファイル名を構築するために使用すると同様に、`MakePixel`色成分に基づいてピクセル値を定義するためのメソッド。

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

`zoomLevel`パラメーターを`FilePath`範囲は 0 を`COUNT`から 1 を引いた定数。

`MainPage`コンス トラクターの呼び出し、`LoadAndStartAnimation`メソッド。

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

`LoadAndStartAnimation`メソッドは、プログラムが以前に実行されたときに作成された可能性があります、ビットマップを読み込むアプリケーションのローカル ストレージにアクセスする責任を負います。 ループ`zoomLevel`値は 0 ~`COUNT`します。 それを読み込んだこと、ファイルが存在する場合、`bitmaps`配列。 特定のビットマップを作成する必要がありますそれ以外の場合、`center`と`zoomLevel`呼び出すことによって値`Mandelbrot.CalculateAsync`します。 そのメソッドは、このメソッドの色に変換し、各ピクセルのイテレーション数を取得します。

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

アプリケーションのローカル記憶域は、デバイスのフォト ライブラリではなく、プログラムでこれらのビットマップを格納することを確認します。 .NET Standard 2.0 ライブラリにより、使い慣れたを使用して`File.OpenRead`と`File.WriteAllBytes`このタスクのメソッド。

すべてのビットマップを作成またはメモリに読み込まれた後、メソッドは、開始、`Stopwatch`オブジェクトと呼び出し`Device.StartTimer`します。 `OnTimerTick`メソッドは 16 ミリ秒ごとに呼び出されます。

`OnTimerTick` 計算を`time`0 ~ 6000 の時間範囲の値 (ミリ秒単位) `COUNT`、6 秒の各ビットマップの表示を apportions をします。 `progress`値の使用、`Math.Sin`として、最後に低速の方向が反転プロパティと値は、サイクルの先頭に遅くなります振幅のアニメーションを作成します。

`progress`値の範囲は 0 ~`COUNT`します。 こうすることの整数部`progress`へのインデックス、`bitmaps`の小数部の中に、配列`progress`が特定のビットマップのズーム レベルを示します。 これらの値が格納されている、`bitmapIndex`と`bitmapProgress`フィールド、およびによって表示される、`Label`と`Slider`XAML ファイルにします。 `SKCanvasView`ビットマップの表示を更新するのには無効になります。

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

最後に、`PaintSurface`のハンドラー、`SKCanvasView`縦横比を維持しながら、可能な限り大きくのビットマップを表示する先の四角形を計算します。 元の四角形がに基づいて、`bitmapProgress`値。 `fraction`値はここで計算される場合は 0 から範囲`bitmapProgress`0 0.25 のときに、ビットマップ全体を表示するには`bitmapProgress`1 と、効果的にズーム イン、ビットマップの高さの半分の幅を表示するには。

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

[![マンデルブロ アニメーション](animating-images/MandelbrotAnimation.png "マンデルブロ アニメーション")](animating-images/MandelbrotAnimation-Large.png#lightbox)

## <a name="gif-animation"></a>GIF アニメーション

グラフィックス インターチェンジ形式 (GIF) 仕様には、ループ内で多くの場合、連続して表示できるシーンの複数の連続フレームを格納する 1 つの GIF ファイルを許可する機能が含まれています。 これらのファイルと呼ばれる_アニメーション Gif_します。 Web ブラウザーは、Gif、アニメーションを再生でき、SkiaSharp により、アプリケーションはアニメーション GIF ファイルからのフレームを抽出して、それらを順番に表示します。

[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)サンプルには、という名前のアニメーション GIF リソースが含まれて**Newtons_cradle_animation_book_2.gif** DemonDeLuxe によって作成されからダウンロード、[ニュートンのクレードルに接続](https://en.wikipedia.org/wiki/Newton%27s_cradle) wikipedia ページ。 **アニメーション GIF**ページには、その情報を提供し、インスタンス化する XAML ファイルが含まれています、 `SKCanvasView`:

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

分離コード ファイルは、アニメーション GIF ファイルを再生する汎用化されていません。 具体的には使用可能な情報の一部は無視されますが繰り返し数と再生をループ内でアニメーション GIF だけです。

アニメーション GIF ファイルのフレームを抽出する SkisSharp の使用はしていない、任意の場所を文書化するため、次のコードの記述が通常よりも詳細。

アニメーション GIF ファイルのデコードがページのコンス トラクターで発生しする必要があります、`Stream`ビットマップを参照するオブジェクトを作成するために使用する`SKManagedStream`オブジェクトをクリックし、 [ `SKCodec` ](xref:SkiaSharp.SKCodec)オブジェクト。 [ `FrameCount` ](xref:SkiaSharp.SKCodec.FrameCount)プロパティは、アニメーションを構成するフレームの数を示します。

コンス トラクターを使用して、これらのフレームは個々 のビットマップとして保存最終的には`FrameCount`型の配列を割り当てる`SKBitmap`と 2 つ`int`累積 (アニメーションのロジックが容易になります) を各フレームの期間の配列期間。

[ `FrameInfo` ](xref:SkiaSharp.SKCodec.FrameInfo)プロパティの`SKCodec`クラスの配列は、 [ `SKCodecFrameInfo` ](xref:SkiaSharp.SKCodecFrameInfo)値、1 つは、各フレームがこのプログラムではその構造からですだけが、 [ `Duration` ](xref:SkiaSharp.SKCodecFrameInfo.Duration) (ミリ秒) 内のフレームの。

`SKCodec` という名前のプロパティを定義します[ `Info` ](xref:SkiaSharp.SKCodec.Info)型の[ `SKImageInfo`](xref:SkiaSharp.SKImageInfo)が、その`SKImageInfo`値を示します (少なくともこのイメージ) の色の種類がある`SKColorType.Index8`、です。 つまり。各ピクセルが、色の種類へのインデックス。 カラー テーブルを怠ってを避けるためには、プログラムを使用して、 [ `Width` ](xref:SkiaSharp.SKImageInfo.Width)と[ `Height` ](xref:SkiaSharp.SKImageInfo.Height)フルカラーを所有していることを構築するには、その構造から情報`ImageInfo`値。 各`SKBitmap`をから作成されます。

`GetPixels`メソッドの`SKBitmap`を返します、`IntPtr`そのビットマップのピクセル ビットを参照します。 これらのピクセル ビットはまだ設定されていません。 ある`IntPtr`のいずれかに渡される、 [ `GetPixels` ](xref:SkiaSharp.SKCodec.GetPixels(SkiaSharp.SKImageInfo,System.IntPtr,SkiaSharp.SKCodecOptions))メソッドの`SKCodec`します。 そのメソッドが GIF ファイルからによって参照されるメモリ領域にフレームをコピー、`IntPtr`します。 [ `SKCodecOptions` ](xref:SkiaSharp.SKCodecOptions)コンス トラクターは、フレームの数を示します。

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

関係なく、`IntPtr`値、いいえ`unsafe`コードは、必要なため、 `IntPtr` c# ポインター値には変換されません。

各フレームを抽出すると、コンス トラクターはすべてのフレームの期間を合計し、累積期間別の配列を初期化します。

分離コード ファイルの残りの部分には、アニメーションは専用です。 `Device.StartTimer`メソッドを使用して、タイマーを開始し、`OnTimerTick`コールバックを使用して、`Stopwatch`経過時間 (ミリ秒単位) を決定するオブジェクト。 累積期間配列をループ処理は、現在のフレームを検索するための十分なは。

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

毎回、 `currentframe` 、変数の変更、`SKCanvasView`は無効にし、新しいフレームが表示されます。

[![アニメーション GIF](animating-images/AnimatedGif.png "アニメーション GIF")](animating-images/AnimatedGif-Large.png#lightbox)

プログラムを実行したいは言うまでもなく、アニメーションを確認します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [マンデルブロ アニメーション (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/MandelAnima/)
