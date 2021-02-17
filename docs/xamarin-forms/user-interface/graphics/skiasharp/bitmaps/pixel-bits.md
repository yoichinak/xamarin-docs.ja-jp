---
title: SkiaSharp ビットマップピクセルビットへのアクセス
description: SkiaSharp ビットマップのピクセルビットにアクセスして変更するためのさまざまな手法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: DBB58522-F816-4A8C-96A5-E0236F16A5C6
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0797829a566ddd71311cb701dbc4d6b1f7bb1c2e
ms.sourcegitcommit: a0de974875f8fa1a29f7abc990137246789ad85a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "100630242"
---
# <a name="accessing-skiasharp-bitmap-pixel-bits"></a>SkiaSharp ビットマップピクセルビットへのアクセス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

「 [**SkiaSharp ビットマップをファイルに保存する**](saving.md)」の記事で説明したように、通常、ビットマップは JPEG や PNG などの圧縮形式でファイルに保存されます。 とは異なりでは、メモリに格納されている SkiaSharp ビットマップは圧縮されません。 連続する一連のピクセルとして格納されます。 この圧縮されていない形式を指定すると、ビットマップを画面に簡単に転送できます。

SkiaSharp ビットマップによって占有されているメモリブロックは、非常に簡単な方法で編成されます。最初の行が左から右に、次に2番目の行が続きます。 フルカラービットマップの場合、各ピクセルは4バイトで構成されます。つまり、ビットマップに必要なメモリ領域の合計は、幅と高さの4倍になります。

この記事では、アプリケーションが、ビットマップのピクセルメモリブロックにアクセスすることによって直接、または間接的に、これらのピクセルにアクセスできるようにする方法について説明します。 場合によっては、プログラムでイメージのピクセルを分析し、何らかの種類のヒストグラムを作成することが必要になることがあります。 多くの場合、アプリケーションは、ビットマップを構成するピクセルをアルゴリズムて作成することで、一意のイメージを作成できます。

![ピクセルビットのサンプル](pixel-bits-images/PixelBitsSample.png "ピクセルビットのサンプル")

## <a name="the-techniques"></a>技法

SkiaSharp は、ビットマップのピクセルビットにアクセスするためのいくつかの手法を提供します。 どちらを選択すればよいかは、通常、コーディングの便宜 (メンテナンスやデバッグの容易さに関連します) とパフォーマンスの妥協です。 ほとんどの場合、 `SKBitmap` ビットマップのピクセルにアクセスするには、の次のいずれかのメソッドとプロパティを使用します。

- `GetPixel`メソッドとメソッドを使用すると、 `SetPixel` 1 つのピクセルの色を取得または設定できます。
- プロパティは、 `Pixels` ビットマップ全体のピクセル色の配列を取得するか、色の配列を設定します。
- `GetPixels` ビットマップによって使用されるピクセルメモリのアドレスを返します。
- `SetPixels` ビットマップによって使用されるピクセルメモリのアドレスを置き換えます。

最初の2つの手法は "高レベル" で、2つ目は "低レベル" と考えることができます。 使用できるメソッドとプロパティは他にもいくつかありますが、これらは最も重要なものです。

これらの手法のパフォーマンスの違いを確認できるように、 [**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) アプリケーションには、グラデーションを作成するために赤と青の網掛けを組み合わせたピクセルのビットマップを作成する、 **グラデーションビットマップ** という名前のページが含まれています。 このプログラムは、ビットマップピクセルを設定するためのさまざまな手法を使用して、このビットマップの8つの異なるコピーを作成します。 これらの8つのビットマップはそれぞれ、個別のメソッドで作成されます。この方法では、技術の簡単な説明が設定され、すべてのピクセルを設定するために必要な時間が計算されます。 各メソッドは、ピクセル設定ロジック100をループ処理して、パフォーマンスをより正確に推定します。

### <a name="the-setpixel-method"></a>SetPixel メソッド

複数の個別のピクセルを設定または取得する必要があるだけの場合は、 [`SetPixel`](xref:SkiaSharp.SKBitmap.SetPixel(System.Int32,System.Int32,SkiaSharp.SKColor)) [`GetPixel`](xref:SkiaSharp.SKBitmap.GetPixel(System.Int32,System.Int32)) メソッドとメソッドが理想的です。 これらの2つの方法では、整数の列と行を指定します。 ピクセル形式に関係なく、これらの2つのメソッドを使用して、ピクセルを値として取得または設定でき `SKColor` ます。

```csharp
bitmap.SetPixel(col, row, color);

SKColor color = bitmap.GetPixel(col, row);
```

引数は、 `col` 0 からビットマップのプロパティより1小さい値までの範囲で指定する必要があり `Width` ます。また、0 ~ 1 の範囲のプロパティよりも小さい値を指定する必要があり `row` `Height` ます。

次に、メソッドを使用してビットマップの内容を設定する **グラデーションビットマップ** 内のメソッドを示し `SetPixel` ます。 ビットマップは 256 x 256 ピクセルで、ループは `for` 値の範囲でハードコーディングされます。

```csharp
public class GradientBitmapPage : ContentPage
{
    const int REPS = 100;

    Stopwatch stopwatch = new Stopwatch();
    ···
    SKBitmap FillBitmapSetPixel(out string description, out int milliseconds)
    {
        description = "SetPixel";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        for (int rep = 0; rep < REPS; rep++)
            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    bitmap.SetPixel(col, row, new SKColor((byte)col, 0, (byte)row));
                }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
}
```

各ピクセルの色セットには、ビットマップ列と同じ赤のコンポーネントと、その行に等しい青のコンポーネントがあります。 結果として得られるビットマップは、左上、赤は左、左下には青、右下の場合は青、他の場所にはグラデーションがあります。

`SetPixel`このメソッドは65536回呼び出されますが、この方法がどの程度効率的であるかにかかわらず、別の方法が使用可能な場合は、多くの API 呼び出しを行うことをお勧めしません。 幸い、いくつかの代替手段があります。

### <a name="the-pixels-property"></a>Pixels プロパティ

`SKBitmap`[`Pixels`](xref:SkiaSharp.SKBitmap.Pixels)ビットマップ全体の値の配列を返すプロパティを定義し `SKColor` ます。 また、を使用して `Pixels` 、ビットマップの色の値の配列を設定することもできます。

```csharp
SKColor[] pixels = bitmap.Pixels;

bitmap.Pixels = pixels;
```

ピクセルは、最初の行、左から右、2番目の行の順に配列に配置されます。 配列内の色の合計数は、ビットマップの幅と高さの積と同じです。

このプロパティは効率的であると考えられますが、ピクセルがビットマップから配列にコピーされ、配列からビットマップに戻され、ピクセルがとの値に変換されることに注意してください `SKColor` 。

`GradientBitmapPage`プロパティを使用してビットマップを設定するクラスのメソッドを次に示し `Pixels` ます。 メソッドは、 `SKColor` 必要なサイズの配列を割り当てますが、プロパティを使用して `Pixels` その配列を作成することもできます。

```csharp
SKBitmap FillBitmapPixelsProp(out string description, out int milliseconds)
{
    description = "Pixels property";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    SKColor[] pixels = new SKColor[256 * 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                pixels[256 * row + col] = new SKColor((byte)col, 0, (byte)row);
            }

    bitmap.Pixels = pixels;

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

配列のインデックスは、 `pixels` 変数と変数から計算する必要があることに注意して `row` `col` ください。 行に各行のピクセル数を乗算し (この場合は 256)、列が追加されます。

`SKBitmap` は同様のプロパティも定義します `Bytes` 。これにより、ビットマップ全体のバイト配列が返されますが、フルカラービットマップの方が煩雑になります。

### <a name="the-getpixels-pointer"></a>GetPixels ポインター

ビットマップピクセルにアクセスする最も強力な手法は [`GetPixels`](xref:SkiaSharp.SKBitmap.GetPixels) 、 `GetPixel` メソッドまたはプロパティと混同しないことがあり `Pixels` ます。 `GetPixels`C# プログラミングではあまり一般的ではないものを返すという点で、との違いについてすぐに確認します。

```csharp
IntPtr pixelsAddr = bitmap.GetPixels();
```

.NET [`IntPtr`](xref:System.IntPtr) 型はポインターを表します。 これは、 `IntPtr` プログラムが実行されるコンピューターのネイティブプロセッサ上の整数の長さであるために呼び出されます。通常は、32ビットまたは64ビットの長さになります。 が `IntPtr` 返すは、 `GetPixels` ビットマップオブジェクトがピクセルを格納するために使用している実際のメモリブロックのアドレスです。

を `IntPtr` C# ポインター型に変換するには、メソッドを使用し [`ToPointer`](xref:System.IntPtr.ToPointer) ます。 C# のポインター構文は、C および C++ と同じです。

```csharp
byte* ptr = (byte*)pixelsAddr.ToPointer();
```

`ptr`変数は _byte ポインター_ 型です。 この `ptr` 変数を使用すると、ビットマップのピクセルを格納するために使用されるメモリの個々のバイトにアクセスできます。 次のようなコードを使用して、このメモリからバイトを読み取り、またはメモリにバイトを書き込むことができます。

```csharp
byte pixelComponent = *ptr;

*ptr = pixelComponent;
```

このコンテキストでは、アスタリスクは C# _間接演算子_ で、が指すメモリの内容を参照するために使用され `ptr` ます。 最初のは、 `ptr` ビットマップの最初の行の最初のピクセルの最初のバイトを指しますが、変数に対して算術演算を実行し `ptr` て、ビットマップ内の他の場所に移動することができます。

欠点の1つは `ptr` 、この変数をキーワードでマークされたコードブロックでのみ使用できることです `unsafe` 。 また、アセンブリには安全でないブロックを許可するようにフラグを設定する必要があります。 これは、プロジェクトのプロパティで行います。

C# でポインターを使用することは非常に強力ですが、非常に危険です。 ポインターが参照するものを超えてメモリにアクセスしないように注意する必要があります。 このため、ポインターの使用は "unsafe" という語に関連付けられています。

メソッドを使用するクラスのメソッドを次に示し `GradientBitmapPage` `GetPixels` ます。 `unsafe`バイトポインターを使用して、すべてのコードを含むブロックに注目します。

```csharp
SKBitmap FillBitmapBytePtr(out string description, out int milliseconds)
{
    description = "GetPixels byte ptr";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            byte* ptr = (byte*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (byte)(col);   // red
                    *ptr++ = 0;             // green
                    *ptr++ = (byte)(row);   // blue
                    *ptr++ = 0xFF;          // alpha
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

`ptr`変数が最初にメソッドから取得されると `ToPointer` 、ビットマップの最初の行の左端のピクセルの最初のバイトを指します。 `for`とのループは、 `row` `col` `ptr` `++` 各ピクセルの各バイトを設定した後に演算子を使用してインクリメントできるように設定されています。 他の99ループでピクセルを使用する場合は、を `ptr` ビットマップの先頭に戻す必要があります。

各ピクセルは4バイトのメモリであるため、各バイトを個別に設定する必要があります。 ここでのコードでは、バイトが赤、緑、青、アルファという順序になっていることを前提としています。これは色の種類と一致してい `SKColorType.Rgba8888` ます。 これは iOS と Android の既定の色の種類であり、ユニバーサル Windows プラットフォームではないことを思い出してください。 既定では、UWP は色の種類を使用してビットマップを作成し `SKColorType.Bgra8888` ます。 このため、そのプラットフォームで異なる結果が表示されることが予想されます。

から返された値を `ToPointer` ポインターではなくポインターにキャストすることができ `uint` `byte` ます。 これにより、1つのステートメントでピクセル全体にアクセスできるようになります。 そのポインターに演算子を適用すると、 `++` 次のピクセルを指すように4バイトずつインクリメントされます。

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    SKBitmap FillBitmapUintPtr(out string description, out int milliseconds)
    {
        description = "GetPixels uint ptr";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        IntPtr pixelsAddr = bitmap.GetPixels();

        unsafe
        {
            for (int rep = 0; rep < REPS; rep++)
            {
                uint* ptr = (uint*)pixelsAddr.ToPointer();

                for (int row = 0; row < 256; row++)
                    for (int col = 0; col < 256; col++)
                    {
                        *ptr++ = MakePixel((byte)col, 0, (byte)row, 0xFF);
                    }
            }
        }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    uint MakePixel(byte red, byte green, byte blue, byte alpha) =>
            (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

ピクセルはメソッドを使用して設定され `MakePixel` ます。このメソッドは、赤、緑、青、およびアルファの各コンポーネントの整数ピクセルを構築します。 この形式は、次の `SKColorType.Rgba8888` ようにピクセルのバイト順序を持つことに注意してください。

RR GG BB AA

ただし、これらのバイトに対応する整数は次のとおりです。

AABBGGRR

整数の最下位バイトは、リトルエンディアンアーキテクチャに従って最初に格納されます。 `MakePixel`色の種類のビットマップでは、このメソッドは正しく機能しません `Bgra8888` 。

メソッドには、このメソッドを別のメソッドにすることを回避するためのオプションが設定 `MakePixel` されてい [`MethodImplOptions.AggressiveInlining`](xref:System.Runtime.CompilerServices.MethodImplOptions) ますが、メソッドが呼び出されたコードをコンパイルすることをお勧めします。 これにより、パフォーマンスが向上します。

興味深いことに、 `SKColor` 構造体はから符号なし整数への明示的な変換を定義します `SKColor` 。これは、 `SKColor` 値を作成でき、の代わりにへの変換を使用できることを意味します `uint` `MakePixel` 。

```csharp
SKBitmap FillBitmapUintPtrColor(out string description, out int milliseconds)
{
    description = "GetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            uint* ptr = (uint*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (uint)new SKColor((byte)col, 0, (byte)row);
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

唯一の質問は、 `SKColor` 色の種類の順序における値の整数形式、または色の種類ですか。それとも、 `SKColorType.Rgba8888` `SKColorType.Bgra8888` まったく別のものですか。 その質問に対する答えは、すぐに明らかになります。

### <a name="the-setpixels-method"></a>SetPixels メソッド

`SKBitmap` また、はという名前のメソッドを定義 [`SetPixels`](xref:SkiaSharp.SKBitmap.SetPixels(System.IntPtr)) します。このメソッドは次のように呼ばれます。

```csharp
bitmap.SetPixels(intPtr);
```

ピクセルを `GetPixels` `IntPtr` 格納するためにビットマップによって使用されるメモリブロックを参照するを取得することを思い出してください。 この `SetPixels` 呼び出しは、そのメモリブロックを引数として指定されたによって参照されるメモリブロックに _置き換え_ `IntPtr` `SetPixels` ます。 ビットマップは、以前に使用されていたメモリブロックを解放します。 次にが呼び出されたときに、 `GetPixels` によって設定されたメモリブロックを取得し `SetPixels` ます。

一見すると、の方が便利ではないかのように、より `SetPixels` 強力でパフォーマンスが得られないように思え `GetPixels` ます。 で `GetPixels` は、ビットマップメモリブロックを取得し、それにアクセスします。 で `SetPixels` は、いくつかのメモリを割り当ててアクセスし、それをビットマップメモリブロックとして設定します。

ただし、を使用すると、構文上の利点が明確になり `SetPixels` ます。これにより、配列を使用してビットマップピクセルビットにアクセスできます。 この手法を示すのメソッドを次に `GradientBitmapPage` 示します。 メソッドは、まず、ビットマップのピクセルのバイトに対応する多次元バイト配列を定義します。 最初のディメンションは行、2番目のディメンションは列、3番目のディメンションは各ピクセルの4つのコンポーネントにはます。

```csharp
SKBitmap FillBitmapByteBuffer(out string description, out int milliseconds)
{
    description = "SetPixels byte buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    byte[,,] buffer = new byte[256, 256, 4];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col, 0] = (byte)col;   // red
                buffer[row, col, 1] = 0;           // green
                buffer[row, col, 2] = (byte)row;   // blue
                buffer[row, col, 3] = 0xFF;        // alpha
            }

    unsafe
    {
        fixed (byte* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

次に、配列にピクセルが格納された後、 `unsafe` ブロックとステートメントを使用して、 `fixed` この配列を指すバイトポインターを取得します。 そのバイトポインターをにキャストして、 `IntPtr` に渡すことができ `SetPixels` ます。

作成する配列は、バイト配列である必要はありません。 この値は、行と列に対して2つの次元のみを持つ整数配列にすることができます。

```csharp
SKBitmap FillBitmapUintBuffer(out string description, out int milliseconds)
{
    description = "SetPixels uint buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = MakePixel((byte)col, 0, (byte)row, 0xFF);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

この `MakePixel` メソッドは、カラーコンポーネントを32ビットピクセルに結合するために再び使用されます。

完全を期すために、次に示すのは同じコードですが、 `SKColor` 符号なし整数にキャストされた値を使用します。

```csharp
SKBitmap FillBitmapUintBufferColor(out string description, out int milliseconds)
{
    description = "SetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = (uint)new SKColor((byte)col, 0, (byte)row);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

### <a name="comparing-the-techniques"></a>手法の比較

[ **グラデーションの色** ] ページのコンストラクターは、上に示した8つのメソッドすべてを呼び出し、結果を保存します。

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    string[] descriptions = new string[8];
    SKBitmap[] bitmaps = new SKBitmap[8];
    int[] elapsedTimes = new int[8];

    SKCanvasView canvasView;

    public GradientBitmapPage ()
    {
        Title = "Gradient Bitmap";

        bitmaps[0] = FillBitmapSetPixel(out descriptions[0], out elapsedTimes[0]);
        bitmaps[1] = FillBitmapPixelsProp(out descriptions[1], out elapsedTimes[1]);
        bitmaps[2] = FillBitmapBytePtr(out descriptions[2], out elapsedTimes[2]);
        bitmaps[4] = FillBitmapUintPtr(out descriptions[4], out elapsedTimes[4]);
        bitmaps[6] = FillBitmapUintPtrColor(out descriptions[6], out elapsedTimes[6]);
        bitmaps[3] = FillBitmapByteBuffer(out descriptions[3], out elapsedTimes[3]);
        bitmaps[5] = FillBitmapUintBuffer(out descriptions[5], out elapsedTimes[5]);
        bitmaps[7] = FillBitmapUintBufferColor(out descriptions[7], out elapsedTimes[7]);

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

コンストラクターは、結果のビットマップを表示するを作成することによって終了し `SKCanvasView` ます。 この `PaintSurface` ハンドラーは、そのサーフェイスを8つの四角形に分割し、を呼び出して `Display` それぞれを表示します。

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        int width = info.Width;
        int height = info.Height;

        canvas.Clear();

        Display(canvas, 0, new SKRect(0, 0, width / 2, height / 4));
        Display(canvas, 1, new SKRect(width / 2, 0, width, height / 4));
        Display(canvas, 2, new SKRect(0, height / 4, width / 2, 2 * height / 4));
        Display(canvas, 3, new SKRect(width / 2, height / 4, width, 2 * height / 4));
        Display(canvas, 4, new SKRect(0, 2 * height / 4, width / 2, 3 * height / 4));
        Display(canvas, 5, new SKRect(width / 2, 2 * height / 4, width, 3 * height / 4));
        Display(canvas, 6, new SKRect(0, 3 * height / 4, width / 2, height));
        Display(canvas, 7, new SKRect(width / 2, 3 * height / 4, width, height));
    }

    void Display(SKCanvas canvas, int index, SKRect rect)
    {
        string text = String.Format("{0}: {1:F1} msec", descriptions[index],
                                    (double)elapsedTimes[index] / REPS);

        SKRect bounds = new SKRect();

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.TextSize = (float)(12 * canvasView.CanvasSize.Width / canvasView.Width);
            textPaint.TextAlign = SKTextAlign.Center;
            textPaint.MeasureText("Tly", ref bounds);

            canvas.DrawText(text, new SKPoint(rect.MidX, rect.Bottom - bounds.Bottom), textPaint);
            rect.Bottom -= bounds.Height;
            canvas.DrawBitmap(bitmaps[index], rect, BitmapStretch.Uniform);
        }
    }
}
```

コンパイラがコードを最適化できるようにするため、このページは **リリース** モードで実行されました。 このページは、MacBook Pro の iPhone 8 シミュレーターで実行されているページ、または Windows 10 を実行している Microsoft の Android フォンと Surface Pro 3 です。 ハードウェアの違いにより、デバイス間のパフォーマンス時間を比較するのではなく、各デバイスの相対的な時間を確認します。

[![グラデーションビットマップ](pixel-bits-images/GradientBitmap.png "グラデーションビットマップ")](pixel-bits-images/GradientBitmap-Large.png#lightbox)

実行時間をミリ秒単位で統合するテーブルを次に示します。

| API       | データ型 | iOS  | Android | UWP  |
| --------- | --------- | ----:| -------:| ----:|
| SetPixel  |           | 3.17 |   10.77 | 3.49 |
| ピクセル    |           | 0.32 |    1.23 | 0.07 |
| GetPixels | byte      | 0.09 |    0.24 | 0.10 |
|           | uint      | 0.06 |    0.26 | 0.05 |
|           | SKColor   | 0.29 |    0.99 | 0.07 |
| SetPixels | byte      | 1.33 |    6.78 | 0.11 |
|           | uint      | 0.14 |    0.69 | 0.06 |
|           | SKColor   | 0.35 |    1.93 | 0.10 |

必要に応じて、65536回を呼び出すと `SetPixel` 、ビットマップのピクセルを設定する effeicient 方法が最も少なくなります。 配列に `SKColor` 値を入力してプロパティを設定する `Pixels` 方がはるかに優れています。また、相性をいくつかの手法と比較することも `GetPixels` `SetPixels` できます。 `uint`ピクセル値の使用は、通常、個別のコンポーネントを設定するより高速です `byte` `SKColor` 。値を符号なし整数に変換することで、プロセスにオーバーヘッドを追加できます。

また、さまざまなグラデーションを比較しても興味深い点があります。各プラットフォームの上位行は同じであり、目的に応じてグラデーションが表示されます。 これは、 `SetPixel` メソッドとプロパティが、 `Pixels` 基になるピクセル形式に関係なく、色からピクセルを正しく作成することを意味します。

IOS と Android のスクリーンショットの次の2つの行も同じであり、これらの `MakePixel` プラットフォームの既定のピクセル形式に対して小さなメソッドが正しく定義されていることを確認し `Rgba8888` ます。

IOS と Android のスクリーンショットの一番下の行は後方にあり、値をキャストすることによって取得される符号なし整数は `SKColor` 次の形式で示されます。

AARRGGBB

バイトの順序は次のとおりです。

BB GG RR AA

これは順序付けでは `Bgra8888` なく順序 `Rgba8888` です。 この `Brga8888` 形式は、ユニバーサル Windows プラットフォームの既定値です。そのため、スクリーンショットの最後の行のグラデーションが1行目と同じになります。 しかし、中間の2行は正しくありません。これらのビットマップを作成するコードでは順序が想定されているため `Rgba8888` です。

各プラットフォームのピクセルビットにアクセスするために同じコードを使用する場合 `SKBitmap` は、形式または形式を使用してを明示的に作成でき `Rgba8888` `Bgra8888` ます。 ビットマップピクセルに値をキャストする場合は `SKColor` 、を使用し `Bgra8888` ます。

## <a name="random-access-of-pixels"></a>ピクセルのランダムアクセス

`FillBitmapBytePtr`[ `FillBitmapUintPtr` **グラデーションビットマップ**] ページのメソッドとメソッドは、ビットマップを順番に、 `for` 先頭行から末尾の行に、各行を左から右に設定するように設計されたループから効果を持ちます。 ピクセルは、ポインターをインクリメントしたのと同じステートメントを使用して設定できます。

場合によっては、順番にではなく、ランダムにピクセルにアクセスする必要があります。 この方法を使用している場合は、 `GetPixels` 行と列に基づいてポインターを計算する必要があります。 これについては、「 **レインボーサイン** 」ページで説明されています。これは、サイン曲線の1サイクルの形でレインボーを示すビットマップを作成します。

レインボーの色は、HSL (色合い、鮮やかさ、明るさ) の色モデルを使用して簡単に作成できます。 メソッドは、 `SKColor.FromHsl` `SKColor` 0 ~ 360 (円の角度、赤から、緑から赤、赤に戻る)、および 0 ~ 100 の範囲の鮮やかさと輝度の値を使用して値を作成します。 レインボーの色については、鮮やかさを最大100、輝度を50の中間点に設定する必要があります。

**レインボーサイン** は、ビットマップの行をループしてから、360の色相値をループすることで、このイメージを作成します。 各色合いの値から、次のように正弦値に基づくビットマップ列が計算されます。

```csharp
public class RainbowSinePage : ContentPage
{
    SKBitmap bitmap;

    public RainbowSinePage()
    {
        Title = "Rainbow Sine";

        bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);

        unsafe
        {
            // Pointer to first pixel of bitmap
            uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();

            // Loop through the rows
            for (int row = 0; row < bitmap.Height; row++)
            {
                // Calculate the sine curve angle and the sine value
                double angle = 2 * Math.PI * row / bitmap.Height;
                double sine = Math.Sin(angle);

                // Loop through the hues
                for (int hue = 0; hue < 360; hue++)
                {
                    // Calculate the column
                    int col = (int)(360 + 360 * sine + hue);

                    // Calculate the address
                    uint* ptr = basePtr + bitmap.Width * row + col;

                    // Store the color value
                    *ptr = (uint)SKColor.FromHsl(hue, 100, 50);
                }
            }
        }

        // Create the SKCanvasView
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
        canvas.DrawBitmap(bitmap, info.Rect);
    }
}
```

コンストラクターによって、形式に基づいてビットマップが作成されることに注意して `SKColorType.Bgra8888` ください。

```csharp
bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);
```

これにより、プログラムでは、値の変換を使用して、 `SKColor` 気にせずに値をピクセルに変換でき `uint` ます。 この特定のプログラムでは役割を果たしませんが、変換を使用してピクセルを設定する場合は、が `SKColor` `SKAlphaType.Unpremul` `SKColor` カラーコンポーネントをアルファ値でからないように、も指定する必要があります。

次に、コンストラクターはメソッドを使用して、 `GetPixels` ビットマップの最初のピクセルへのポインターを取得します。

```csharp
uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();
```

特定の行と列については、オフセット値をに追加する必要があり `basePtr` ます。 このオフセットは、ビットマップの幅の行と列を組み合わせたものです。

```csharp
uint* ptr = basePtr + bitmap.Width * row + col;
```

`SKColor`値は、次のポインターを使用してメモリに格納されます。

```csharp
*ptr = (uint)SKColor.FromHsl(hue, 100, 50);
```

`PaintSurface`のハンドラーでは、 `SKCanvasView` 表示領域に合わせてビットマップが拡大されます。

[![レインボーサイン](pixel-bits-images/RainbowSine.png "レインボーサイン")](pixel-bits-images/RainbowSine-Large.png#lightbox)

## <a name="from-one-bitmap-to-another"></a>1つのビットマップから別のビットマップに

多くのイメージ処理タスクでは、1つのビットマップから別のビットマップに転送されるときに、ピクセルの変更が行われます。 この手法は、[ **カラー調整** ] ページで説明されています。 ページはビットマップリソースの1つを読み込み、次の3つのビューを使用してイメージを変更でき `Slider` ます。

[![[ドライバーによる色補正]](pixel-bits-images/ColorAdjustment.png "[ドライバーによる色補正]")](pixel-bits-images/ColorAdjustment-Large.png#lightbox)

ピクセルの色ごとに、最初のは `Slider` 0 ~ 360 の値を色相に加算しますが、剰余演算子を使用して 0 ~ 360 の結果を保持し、(UWP スクリーンショットのように) スペクトルに沿って色を効果的にシフトします。 2番目の方法では、 `Slider` 0.5 ~ 2 の範囲の中から鮮やかさに適用する係数を選択できます。3番目は、 `Slider` Android のスクリーンショットに示すように、明度に対しても同じです。

このプログラムは、という名前の元のソースビットマップとという名前の調整されたターゲットビットマップの2つのビットマップを保持し `srcBitmap` `dstBitmap` ます。 が移動されるたびに、 `Slider` プログラムはのすべての新しいピクセルを計算し `dstBitmap` ます。 もちろん、ユーザーはビューを非常に短時間で移動することで実験を `Slider` 行います。そのため、管理可能なパフォーマンスを最大限に高めることができます。 これには、 `GetPixels` コピー元とコピー先の両方のビットマップのメソッドが含まれます。

[ **カラー調整** ] ページでは、コピー元とコピー先のビットマップの色形式は制御されません。 代わりに、形式と形式のロジックが若干異なり `SKColorType.Rgba8888` `SKColorType.Bgra8888` ます。 変換元と変換先は異なる形式にすることができ、プログラムは引き続き機能します。

次に示すのは、変換 `TransferPixels` 元から変換先にピクセルを転送する重要なメソッドを除いたプログラムです。 コンストラクターは `dstBitmap` と等しいを設定 `srcBitmap` します。 `PaintSurface`次のハンドラーが表示され `dstBitmap` ます。

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    SKBitmap srcBitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    SKBitmap dstBitmap;

    public ColorAdjustmentPage()
    {
        InitializeComponent();

        dstBitmap = new SKBitmap(srcBitmap.Width, srcBitmap.Height);
        OnSliderValueChanged(null, null);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        float hueAdjust = (float)hueSlider.Value;
        hueLabel.Text = $"Hue Adjustment: {hueAdjust:F0}";

        float saturationAdjust = (float)Math.Pow(2, saturationSlider.Value);
        saturationLabel.Text = $"Saturation Adjustment: {saturationAdjust:F2}";

        float luminosityAdjust = (float)Math.Pow(2, luminositySlider.Value);
        luminosityLabel.Text = $"Luminosity Adjustment: {luminosityAdjust:F2}";

        TransferPixels(hueAdjust, saturationAdjust, luminosityAdjust);
        canvasView.InvalidateSurface();
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(dstBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

`ValueChanged`ビューのハンドラーは、 `Slider` 調整値を計算し、を呼び出し `TransferPixels` ます。

メソッド全体 `TransferPixels` がとしてマークされてい `unsafe` ます。 まず、両方のビットマップのピクセルビットへのバイトポインターを取得し、次にすべての行と列をループ処理します。 ソースビットマップから、メソッドはピクセルごとに4バイトを取得します。 またはの順序で指定 `Rgba8888` でき `Bgra8888` ます。 色の種類を確認すると、 `SKColor` 値を作成できます。 その後、HSL コンポーネントが抽出、調整されて、値の再作成に使用され `SKColor` ます。 コピー先のビットマップがまたはのどちらであるかに応じて、 `Rgba8888` `Bgra8888` バイトは宛先の bitmp に格納されます。

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    ···
    unsafe void TransferPixels(float hueAdjust, float saturationAdjust, float luminosityAdjust)
    {
        byte* srcPtr = (byte*)srcBitmap.GetPixels().ToPointer();
        byte* dstPtr = (byte*)dstBitmap.GetPixels().ToPointer();

        int width = srcBitmap.Width;       // same for both bitmaps
        int height = srcBitmap.Height;

        SKColorType typeOrg = srcBitmap.ColorType;
        SKColorType typeAdj = dstBitmap.ColorType;

        for (int row = 0; row < height; row++)
        {
            for (int col = 0; col < width; col++)
            {
                // Get color from original bitmap
                byte byte1 = *srcPtr++;         // red or blue
                byte byte2 = *srcPtr++;         // green
                byte byte3 = *srcPtr++;         // blue or red
                byte byte4 = *srcPtr++;         // alpha

                SKColor color = new SKColor();

                if (typeOrg == SKColorType.Rgba8888)
                {
                    color = new SKColor(byte1, byte2, byte3, byte4);
                }
                else if (typeOrg == SKColorType.Bgra8888)
                {
                    color = new SKColor(byte3, byte2, byte1, byte4);
                }

                // Get HSL components
                color.ToHsl(out float hue, out float saturation, out float luminosity);

                // Adjust HSL components based on adjustments
                hue = (hue + hueAdjust) % 360;
                saturation = Math.Max(0, Math.Min(100, saturationAdjust * saturation));
                luminosity = Math.Max(0, Math.Min(100, luminosityAdjust * luminosity));

                // Recreate color from HSL components
                color = SKColor.FromHsl(hue, saturation, luminosity);

                // Store the bytes in the adjusted bitmap
                if (typeAdj == SKColorType.Rgba8888)
                {
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Alpha;
                }
                else if (typeAdj == SKColorType.Bgra8888)
                {
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Alpha;
                }
            }
        }
    }
    ···
}
```

変換元と変換先のビットマップの色の種類のさまざまな組み合わせに対して個別のメソッドを作成して、このメソッドのパフォーマンスをさらに向上させることができる可能性があります。また、各ピクセルの種類をチェックしないようにしてください。 別の方法として、 `for` 色の種類に基づいて変数に複数のループを設定することも `col` できます。

## <a name="posterization"></a>Posterization

ピクセルビットへのアクセスを伴うもう1つの一般的なジョブは、 _posterization_ です。 ビットマップのピクセルでエンコードされた色が縮小され、カラーパレットが制限されている場合に、結果が手書きのポスターに似ている場合は、数値。

**ポスタリゼーション** ページでは、次のいずれかのサルイメージでこのプロセスを実行します。

```csharp
public class PosterizePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public PosterizePage()
    {
        Title = "Posterize";

        unsafe
        {
            uint* ptr = (uint*)bitmap.GetPixels().ToPointer();
            int pixelCount = bitmap.Width * bitmap.Height;

            for (int i = 0; i < pixelCount; i++)
            {
                *ptr++ &= 0xE0E0E0FF;
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
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform;
    }
}
```

コンストラクター内のコードは、各ピクセルにアクセスし、値が0xE0E0E0FF のビットごとの AND 演算を実行してから、結果をビットマップに格納します。 値0xE0E0E0FF は、各カラーコンポーネントの上位3ビットを保持し、下位5ビットを0に設定します。 2 ~ 16777216 色<sup>ではなく</sup> 、ビットマップが 2<sup>9</sup> または512色に縮小されます。

[![スクリーンショットは、2つのモバイルデバイスとデスクトップウィンドウにあるおもちゃのサルの画像を示しています。](pixel-bits-images/Posterize.png "ポスタリゼーション")](pixel-bits-images/Posterize-Large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)