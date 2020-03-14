---
title: SkiaSharp のビットマップをアニメーション化
description: ビットマップのアニメーションを実行するには、順番に一連のビットマップを表示し、アニメーション GIF ファイルを表示する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD
author: davidbritch
ms.author: dabritch
ms.date: 07/12/2018
ms.openlocfilehash: 33e17a01d8a13fcdaee27e5857c554a4a232c534
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305673"
---
# <a name="animating-skiasharp-bitmaps"></a>SkiaSharp のビットマップをアニメーション化

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

通常、SkiaSharp グラフィックスをアニメーション化するアプリケーションでは、通常は16ミリ秒ごとに固定レートで `SKCanvasView` に `InvalidateSurface` を呼び出します。 Surface を無効にすると、表示を再描画するために `PaintSurface` ハンドラーへの呼び出しがトリガーされます。 ビジュアルは、1 秒あたり 60 回再描画される、スムーズにアニメーション化に表示されます。

ただし、グラフィックスが 16 ミリ秒で表示するには複雑すぎる場合は、アニメーションはちらつきのようになります。 プログラマが 30 倍、または 15 分、秒、リフレッシュ レートを削減することもできますが、さえでは不十分です。 グラフィックスことが単にレンダリングできませんをリアルタイムで複雑になる場合もあります。

1 つのソリューションでは、一連のビットマップのアニメーションの個別のフレームをレンダリングすることにより、アニメーションの事前準備します。 アニメーションを表示するには、順番に 1 秒あたり 60 回でこれらのビットマップを表示する必要はのみです。

もちろん、ビットマップの多くは可能性がありますが大きな-予算の 3D をアニメーション化されたムービーを作成します。 3D グラフィックはリアルタイムで表示するより複雑です。 各フレームを表示するために多くの処理時間が必要です。 一連のビットマップは基本的には、ビデオを視聴するときに表示します。

SkiaSharp と似たことを行うことができます。 この記事では、ビットマップのアニメーションの 2 つの種類を示します。 最初の例では、マンデルブロ集合のアニメーションを示します。

![アニメーション化のサンプル](animating-images/AnimatingSample.png "アニメーション化のサンプル")

2 番目の例では、アニメーション GIF ファイルを表示するために SkiaSharp を使用する方法を示します。

## <a name="bitmap-animation"></a>ビットマップのアニメーション

マンデルブロ集合とは、魅力的な視覚的が時間のかかる computionally です。 (マンデルブロセットと、ここで使用されている計算の詳細については、ページ666以降の[ _Xamarin の Mobile Apps の作成_に関する第20章](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)を参照してください。 次の説明を想定その背景知識。)

[**マンデルブロアニメーション**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima)のサンプルでは、ビットマップアニメーションを使用して、マンデルブロセット内の固定ポイントの連続ズームをシミュレートします。 ズーム アウト、後で拡大され、状態のままか、プログラムを終了するまで、サイクルが繰り返されます。

プログラムは、このアニメーションのまでアプリケーションのローカル ストレージに格納された 50 のビットマップを作成して準備します。 各ビットマップには、前のビットマップとしての半分の幅と複素平面の高さが含まれます。 (プログラムでは、これらのビットマップは、整数_ズームレベル_を表すものと呼ばれます)。次に、ビットマップが順に表示されます。 ビットマップごとのスケーリングは別に 1 つのビットマップからスムーズな進行を提供するアニメーション化します。

「 _Xamarin. フォームで Mobile Apps を作成する_」の第20章で説明した最終的なプログラムと同様に、**マンデルブロアニメーション**でのマンデルブロセットの計算は、8つのパラメーターを持つ非同期メソッドです。 パラメーターには、複雑な中心および幅とその center 点を囲む複素平面の高さが含まれます。 次の 3 つのパラメーターは、ピクセル幅と、作成するビットマップの高さと再帰的な計算のイテレーションの最大数です。 この計算の進行状況を表示するには、`progress` パラメーターを使用します。 `cancelToken` パラメーターは、このプログラムでは使用されません。

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

メソッドは、ビットマップを作成するための情報を提供する `BitmapInfo` 型のオブジェクトを返します。

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

**マンデルブロアニメーション**の XAML ファイルには、2つの `Label` ビュー、`ProgressBar`、`Button` および `SKCanvasView`が含まれています。

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

場合によっては、`COUNT` 値を50に変更して、アニメーションの全範囲を表示することをお勧めします。 50 を超える値は便利です。 48 などのズーム レベルに倍精度浮動小数点数の解像度になります、マンデルブロ集合の計算が不足しています。 この問題については _、Xamarin. Forms を使用した Mobile Apps の作成_に関するページ684で説明されています。

`center` 値は非常に重要です。 これは、アニメーションのズームのフォーカスです。 このファイルの3つの値は、684ページに_Mobile Apps を作成する方法_の3番目のスクリーンショットで使用されているものですが、この章のプログラムを使用して、独自の値のいずれかを取得することもできます。

**マンデルブロアニメーション**のサンプルでは、これらの `COUNT` ビットマップをローカルアプリケーションストレージに格納します。 50 のビットマップをこれらのビットマップが占有しているストレージの量を知りたい場合がありますのでお使いのデバイス上の記憶域の 20 のメガバイト数以上を必要とし、ある時点でそれらすべてを削除する可能性があります。 `MainPage` クラスの下部には、次の2つのメソッドの目的があります。

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

ローカルアプリケーションストレージに格納されているビットマップには、`center` の値がファイル名に組み込まれています。したがって、`center` 設定を変更しても、既存のビットマップはストレージで置き換えられず、領域を占有し続けます。

ここでは、ファイル名を作成するために `MainPage` 使用するメソッドと、カラーコンポーネントに基づいてピクセル値を定義するための `MakePixel` 方法について説明します。

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

`FilePath` の `zoomLevel` パラメーターは、0から `COUNT` 定数-1 までの範囲です。

`MainPage` コンストラクターは、`LoadAndStartAnimation` メソッドを呼び出します。

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

`LoadAndStartAnimation` メソッドは、アプリケーションのローカルストレージにアクセスして、プログラムが以前に実行されたときに作成された可能性のあるビットマップを読み込む役割を担います。 0から `COUNT`まで `zoomLevel` 値をループ処理します。 ファイルが存在する場合は、`bitmaps` 配列に読み込まれます。 それ以外の場合は、`Mandelbrot.CalculateAsync`を呼び出すことによって、特定の `center` と `zoomLevel` 値のビットマップを作成する必要があります。 そのメソッドは、このメソッドの色に変換し、各ピクセルのイテレーション数を取得します。

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

アプリケーションのローカル記憶域は、デバイスのフォト ライブラリではなく、プログラムでこれらのビットマップを格納することを確認します。 .NET Standard 2.0 ライブラリでは、このタスクに使い慣れた `File.OpenRead` と `File.WriteAllBytes` の方法を使用できます。

すべてのビットマップが作成されるか、メモリに読み込まれると、メソッドは `Stopwatch` オブジェクトを開始し、`Device.StartTimer`を呼び出します。 `OnTimerTick` メソッドは、16ミリ秒ごとに呼び出されます。

`OnTimerTick` は、0から6000の `COUNT`の範囲の `time` 値をミリ秒単位で計算します。これは、各ビットマップの表示について6秒のうち6秒の部分です。 `progress` 値は、`Math.Sin` の値を使用して、決まる正弦波アニメーションを作成します。このアニメーションは、循環の開始時に遅くなり、方向を反転させると終了時に遅くなります。

`progress` 値の範囲は 0 ~ `COUNT`です。 これは、`progress` の整数部分が `bitmaps` 配列のインデックスであり、`progress` の小数部が特定のビットマップのズームレベルを示すことを意味します。 これらの値は `bitmapIndex` フィールドと `bitmapProgress` フィールドに格納され、`Label` と `Slider` によって XAML ファイルに表示されます。 `SKCanvasView` は、ビットマップディスプレイを更新するために無効になっています。

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

最後に、`SKCanvasView` の `PaintSurface` ハンドラーは、縦横比を維持したままビットマップを可能な限り大きく表示する目的の四角形を計算します。 ソースの四角形は、`bitmapProgress` 値に基づいています。 ここで計算される `fraction` 値の範囲は0です `bitmapProgress` 0 は0.25 ビットマップ全体を表示し、`bitmapProgress` が1の場合はビットマップの幅と高さの半分を表示し、実際にはズームインします。

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

グラフィックス インターチェンジ形式 (GIF) 仕様には、ループ内で多くの場合、連続して表示できるシーンの複数の連続フレームを格納する 1 つの GIF ファイルを許可する機能が含まれています。 これらのファイルは、_アニメーション gif_と呼ばれます。 Web ブラウザーは、Gif、アニメーションを再生でき、SkiaSharp により、アプリケーションはアニメーション GIF ファイルからのフレームを抽出して、それらを順番に表示します。

[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルには、DemonDeLuxe によって作成された**Newtons_cradle_animation_book_2**という名前のアニメーション Gif リソースと、Wikipedia の[ニュートンのクレードル](https://en.wikipedia.org/wiki/Newton%27s_cradle)ページからダウンロードされたものが含まれています。 **アニメーション gif**ページには、その情報を提供し、`SKCanvasView`をインスタンス化する XAML ファイルが含まれています。

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

アニメーション GIF ファイルのデコードは、ページのコンストラクター内で行われます。この場合、ビットマップを参照する `Stream` オブジェクトを使用して、`SKManagedStream` オブジェクトを作成し、 [`SKCodec`](xref:SkiaSharp.SKCodec)オブジェクトを作成する必要があります。 [`FrameCount`](xref:SkiaSharp.SKCodec.FrameCount)プロパティは、アニメーションを構成するフレームの数を示します。

これらのフレームは最終的に個々のビットマップとして保存されるため、コンストラクターは `FrameCount` を使用して `SKBitmap` 型の配列と、各フレームの継続時間に対して2つの `int` 配列を割り当て、累積された期間を (アニメーションのロジックを簡単に) 割り当てます。

`SKCodec` クラスの[`FrameInfo`](xref:SkiaSharp.SKCodec.FrameInfo)プロパティは、フレームごとに1つずつ[`SKCodecFrameInfo`](xref:SkiaSharp.SKCodecFrameInfo)値の配列ですが、このプログラムがこの構造から取得するのは、フレームの[`Duration`](xref:SkiaSharp.SKCodecFrameInfo.Duration) (ミリ秒単位) だけです。

`SKCodec` は[`SKImageInfo`](xref:SkiaSharp.SKImageInfo)型の[`Info`](xref:SkiaSharp.SKCodec.Info)という名前のプロパティを定義しますが、`SKImageInfo` 値は色の種類が `SKColorType.Index8`である (少なくともこのイメージの) ことを示します。つまり、各ピクセルは色の種類のインデックスです。 かけるを使用してカラーテーブルを作成しないようにするために、プログラムはその構造の[`Width`](xref:SkiaSharp.SKImageInfo.Width)と[`Height`](xref:SkiaSharp.SKImageInfo.Height)情報を使用して、独自のフルカラー `ImageInfo` 値を構築します。 各 `SKBitmap` は、そのから作成されます。

`SKBitmap` の `GetPixels` メソッドは、そのビットマップのピクセルビットを参照する `IntPtr` を返します。 これらのピクセル ビットはまだ設定されていません。 この `IntPtr` は `SKCodec`の[`GetPixels`](xref:SkiaSharp.SKCodec.GetPixels(SkiaSharp.SKImageInfo,System.IntPtr,SkiaSharp.SKCodecOptions))メソッドのいずれかに渡されます。 このメソッドは、GIF ファイルから、`IntPtr`によって参照されるメモリ空間にフレームをコピーします。 [`SKCodecOptions`](xref:SkiaSharp.SKCodecOptions)コンストラクターは、フレーム番号を示します。

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

`IntPtr` 値にもかかわらず、`IntPtr` はC#ポインター値に変換されないため、`unsafe` コードは必要ありません。

各フレームを抽出すると、コンス トラクターはすべてのフレームの期間を合計し、累積期間別の配列を初期化します。

分離コード ファイルの残りの部分には、アニメーションは専用です。 `Device.StartTimer` メソッドは、タイマーを開始するために使用され、`OnTimerTick` コールバックは `Stopwatch` オブジェクトを使用して経過時間をミリ秒単位で決定します。 累積期間配列をループ処理は、現在のフレームを検索するための十分なは。

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

`currentframe` 変数が変更されるたびに、`SKCanvasView` が無効になり、新しいフレームが表示されます。

[![アニメーション Gif](animating-images/AnimatedGif.png "アニメーション GIF")](animating-images/AnimatedGif-Large.png#lightbox)

プログラムを実行したいは言うまでもなく、アニメーションを確認します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [マンデルブロアニメーション (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima)
