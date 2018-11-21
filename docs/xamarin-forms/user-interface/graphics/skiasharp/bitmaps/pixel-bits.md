---
title: SkiaSharp のビットマップのピクセル ビットへのアクセス
description: アクセスおよび SkiaSharp ビットマップのピクセル ビットを変更するためのさまざまな手法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: DBB58522-F816-4A8C-96A5-E0236F16A5C6
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2018
ms.openlocfilehash: eebfe40bca6db92bae1f2fdcc9cbff3173dc4e51
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52172016"
---
# <a name="accessing-skiasharp-bitmap-pixel-bits"></a>SkiaSharp のビットマップのピクセル ビットへのアクセス

この記事で示した[**保存 SkiaSharp ビットマップ ファイルを**](saving.md)ビットマップは一般に JPEG または PNG などの圧縮形式でファイルに格納されています。 異なり、メモリに格納されている SkiaSharp ビットマップは圧縮されません。 ピクセルの連番として格納されます。 この非圧縮形式には、ビットマップの表示サーフェイスへの転送が容易になります。

SkiaSharp ビットマップによって占有されていたメモリ ブロックは非常に簡単な方法で構成されています。 左から右、ピクセル単位の最初の行で開始し、2 番目の行を続行します。 完全なカラー ビットマップの場合は、各ピクセルで構成されます 4 バイトのビットマップが必要なメモリの合計容量が 4 回、幅と高さの製品であることを意味します。

この記事では、アプリケーションがそれらのピクセルがビットマップのピクセルのメモリ ブロックにアクセスするには、直接かへのアクセスを取得する方法について説明します直接的または間接的にします。 場合によっては、プログラム イメージのピクセルを分析し、何らかのヒストグラムを作成する可能性があります。 一般的には、アプリケーションでは、アルゴリズム、ビットマップを構成するピクセルを作成して一意のイメージを構築できます。

![ピクセル ビット サンプル](pixel-bits-images/PixelBitsSample.png "ピクセル ビット サンプル")

## <a name="the-techniques"></a>手法

SkiaSharp は、ビットマップのピクセル ビットにアクセスするためのいくつかの手法を提供します。 どちらを選択は、通常、コーディング (これは、メンテナンスとデバッグの容易さに関連) 利便性とパフォーマンスのバランスです。 ほとんどの場合は、次のメソッドのいずれかとのプロパティを使用します`SKBitmap`ビットマップのピクセルにアクセスするため。

- `GetPixel`と`SetPixel`メソッドでは、取得または 1 つのピクセルの色を設定することができます。
- `Pixels`プロパティは、全体のビットマップのピクセルの色の配列を取得または色の配列を設定します。
- `GetPixels` ビットマップで使用されるピクセルのメモリのアドレスを返します。
- `SetPixels` ビットマップで使用されるピクセルのメモリのアドレスを置き換えます。

最初の 2 つの手法として「概要」と「低レベル」として 2 つ目の 2 つを考えることができます。 その他のいくつかのメソッドとプロパティを使用することができますが、これらは最も重要です。

パフォーマンスの違い、これらの手法を確認できるように、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)アプリケーションには、という名前のページが含まれている**グラデーション ビットマップ**をグラデーションを作成する赤、青の網掛けを結合するピクセルのビットマップを作成します。 プログラムは 8 つの異なるコピーを作成、このビットマップのすべてを使用してさまざまな手法のビットマップのピクセルを設定します。 この手法の簡単な説明を設定し、すべてのピクセルを設定するために必要な時間を計算する個別のメソッドで各これら 8 つのビットマップが作成されます。 各メソッドをループ処理ピクセル設定ロジック 100 倍のパフォーマンスをより正確に予測を取得します。

### <a name="the-setpixel-method"></a>SetPixel メソッド

いくつかの個々 のピクセルを取得または設定するだけの場合、 [ `SetPixel` ](xref:SkiaSharp.SKBitmap.SetPixel(System.Int32,System.Int32,SkiaSharp.SKColor))と[ `GetPixel` ](xref:SkiaSharp.SKBitmap.GetPixel(System.Int32,System.Int32))メソッドは最適です。 これら 2 つのメソッドごとに、整数型の列と行を指定します。 ピクセル形式に関係なくこれら 2 つのメソッドを使用するを取得したり設定としてピクセル、`SKColor`値。

```csharp
bitmap.SetPixel(col, row, color);

SKColor color = bitmap.GetPixel(col, row);
```

`col`引数は、0 から 1 つにまで必要がありますより小さい`Width`ビットマップのプロパティと`row`範囲は 1 つに 0 より小さい`Height`プロパティ。

ここ、メソッドは、**グラデーション ビットマップ**を使用して、ビットマップの内容を設定する、`SetPixel`メソッド。 ビットマップは 256 ピクセルでは、256、および`for`ループは、値の範囲でハード コードします。

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

各ピクセルの色が、ビットマップの列に赤のコンポーネントがあり、行、青のコンポーネントと等しい。 結果のビットマップは、左、右上に、マゼンタ、右下に、グラデーションを別の場所で、左下にある青いに赤で黒です。

`SetPixel`メソッドが呼び出されて 65,536 の時間に関係なくこのメソッドは、効率的な方法、一般的に別の方法が使用可能な場合は、多くの API 呼び出しを行うことをお勧めします。 さいわい、いくつかの選択肢があります。

### <a name="the-pixels-property"></a>ピクセル プロパティ

`SKBitmap` 定義、 [ `Pixels` ](xref:SkiaSharp.SKBitmap.Pixels)プロパティの配列を返す`SKColor`ビットマップ全体の値。 使用することも`Pixels`ビットマップの色の値の配列を設定します。

```csharp
SKColor[] pixels = bitmap.Pixels;

bitmap.Pixels = pixels;
```

ピクセルは、左から 2 番目の行、最初の行で始まる配列に配置されなど。 配列内の色の合計数は、ビットマップの幅と高さの製品です。

このプロパティを効率的に表示されますが、留意およびビットマップに配列を配列にビットマップからピクセルがコピーされると、ピクセルとの間に変換されます`SKColor`値。

メソッドを次に示します、`GradientBitmapPage`クラスを使用して、ビットマップを設定する、`Pixels`プロパティ。 メソッドが割り当てる、`SKColor`が、必要なサイズの配列を使用しても、`Pixels`その配列を作成するプロパティ。

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

注意のインデックス、`pixels`配列から計算する必要があります、`row`と`col`変数。 行が (この場合の 256) を行ごとのピクセル数が乗算され、列が追加されます。

`SKBitmap` 定義も、類似した`Bytes`プロパティは、ビットマップ全体のバイト配列が返されますが、フルカラー ビットマップの煩雑です。

### <a name="the-getpixels-pointer"></a>GetPixels ポインター

ビットマップのピクセルにアクセスする最も強力な手法は、可能性のある[ `GetPixels`](xref:SkiaSharp.SKBitmap.GetPixels)と混同しないように、`GetPixel`メソッドまたは`Pixels`プロパティ。 違いにすぐ気付く`GetPixels`ことで、c# プログラミングで非常に一般的な結果が返されます。

```csharp
IntPtr pixelsAddr = bitmap.GetPixels();
```

.NET [ `IntPtr` ](xref:System.IntPtr)型のポインターを表します。 呼び出された`IntPtr`ネイティブ プログラムを実行する、通常、32 ビットまたは 64 ビットのいずれかの長さ、マシンのプロセッサの整数の長さがあるためです。 `IntPtr`を`GetPixels`がビットマップ オブジェクトを使用してそのピクセルを格納するメモリの実際のブロックのアドレスを返します。

変換することができます、 `IntPtr` c# ポインター型を使用して、、 [ `ToPointer` ](xref:System.IntPtr.ToPointer)メソッド。 C# ポインターの構文は、C および C++ として同じです。

```csharp
byte* ptr = (byte*)pixelsAddr.ToPointer();
```

`ptr`型の変数は、_バイト ポインター_します。 これは、`ptr`変数では、ビットマップのピクセルの格納に使用される個々 のバイトのメモリにアクセスできます。 このメモリからバイトを読み取りまたはメモリにバイトを書き込むには、このようなコードを使用します。

```csharp
byte pixelComponent = *ptr;

*ptr = pixelComponent;
```

このコンテキストでは、アスタリスクは、c#_間接演算子_によって示されるメモリの内容を参照するために使用して`ptr`します。 最初に、 `ptr` 、ビットマップの最初の行の最初のピクセルの最初のバイトへのポインターが算術演算を実行できる、`ptr`ビットマップ内の他の場所に移動する変数。

1 つの欠点は、これを使用できる`ptr`でマークされたコード ブロックでのみ、変数、`unsafe`キーワード。 さらに、アセンブリの場合は、unsafe ブロックを許容するようフラグが設定する必要があります。 これは、プロジェクトのプロパティです。

ポインターを使用して c# では非常に強力でも非常に危険です。 ポインターが参照するはずを超えるメモリにアクセスしないことに注意する必要があります。 これは、ポインターの使用は、「アンセーフ」という単語に関連付けられているためにです。

メソッドを次に示します、`GradientBitmapPage`クラスを使用する、`GetPixels`メソッド。 通知、`unsafe`バイト ポインターを使用してすべてのコードを含むブロック。

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

ときに、`ptr`から変数が最初に取得した、`ToPointer`ビットマップの最初の行の左端のピクセルの最初のバイトを指すメソッド。 `for`のループ`row`と`col`設定ように`ptr`をインクリメントすることができます、`++`演算子の各ピクセルの各バイトを設定した後。 For (ピクセル単位) を他の 99 ループ、`ptr`ビットマップの先頭に戻るに設定する必要があります。

各ピクセルは 4 バイトのメモリは、各バイトは個別に設定する必要があります。 次のコードで注文の赤、緑、青では、バイトとアルファは矛盾、`SKColorType.Rgba8888`型の色します。 これは iOS と Android、ユニバーサル Windows プラットフォームではなく既定の色の種類であることを思い出してください。 既定では、UWP がのビットマップを作成、`SKColorType.Bgra8888`型の色します。 このため、予想されるプラットフォームでいくつかの異なる結果を参照してください。

返される値をキャストすることは`ToPointer`を`uint`ポインターではなく`byte`ポインター。 これにより、1 つのステートメント内でアクセス全体のピクセルができます。 適用、`++`そのポインターを演算子は、[次へ] のピクセルを指す 4 バイトでインクリメントします。

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

使用して、ピクセルを設定、`MakePixel`メソッドで、赤、緑、青、およびアルファのコンポーネントからの整数ピクセルを構築します。 注意、`SKColorType.Rgba8888`形式では、このような順序付けピクセルのバイト。

RR GG BB AA

それらのバイトに対応する整数値です。

AABBGGRR

整数の下位バイトは、リトル エンディアン アーキテクチャに従って最初に格納されます。 これは、`MakePixel`のビットマップ メソッドは正しく機能しません、`Bgra8888`型の色します。

`MakePixel`メソッド フラグが付き、 [ `MethodImplOptions.AggressiveInlining` ](xref:System.Runtime.CompilerServices.MethodImplOptions)を促すため、別のメソッドが代わりに、コードをコンパイルするメソッドが呼び出された場合を回避するためにコンパイラ オプション。 これにより、パフォーマンスが向上する必要があります。

興味深いことに、`SKColor`構造体からの明示的な変換を定義する`SKColor`を符号なし整数つまり、`SKColor`値を作成することができます、およびへの変換`uint`の代わりに使用できる`MakePixel`:。

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

これの質問は、: の整数形式です、`SKColor`値の順序で、`SKColorType.Rgba8888`色の種類、または`SKColorType.Bgra8888`色の種類、または、これはまったく別ですか? その質問に対する回答をすぐに明らかには。

### <a name="the-setpixels-method"></a>SetPixels メソッド

`SKBitmap` という名前のメソッドも定義[ `SetPixels` ](xref:SkiaSharp.SKBitmap.SetPixels(System.IntPtr))、次のように呼び出すもの。

```csharp
bitmap.SetPixels(intPtr);
```

いることを思い出してください`GetPixels`取得、`IntPtr`ビットマップのピクセルを格納するために使用するメモリ ブロックを参照します。 `SetPixels`呼び出す_置き換えます_によって参照されるメモリ ブロックを使用してそのメモリ ブロック、`IntPtr`として指定された、`SetPixels`引数。 ビットマップは、以前を使用していたメモリ ブロックを解放します。 次回`GetPixels`が呼び出されると、メモリ ブロックを使用して設定を取得`SetPixels`します。

最初、見えますまるで`SetPixels`以上の能力とよりもパフォーマンスは、`GetPixels`が簡単。 `GetPixels`ビットマップ メモリ ブロックを取得し、アクセスします。 `SetPixels`を割り当てると、メモリの一部にアクセスし、ビットマップ メモリ ブロックとして設定しを実行します。

使用していますが`SetPixels`明確な構文上の利点を提供しています。 配列を使用してビットマップのピクセル ビットにアクセスすることができます。 ここ、メソッドは、`GradientBitmapPage`この手法を示しています。 メソッドは、まず、ビットマップのピクセルのバイト数に対応する多次元のバイト配列を定義します。 最初の次元は、行、2 番目の次元は、列と各ピクセルの 4 つのコンポーネントの 3 番目の次元は。

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

そして、配列のデータが (ピクセル単位) に格納された後、`unsafe`ブロックと`fixed`ステートメントを使用して、この配列を指すバイトのポインターを取得します。 そのバイト ポインターにキャストできる、`IntPtr`に渡す`SetPixels`します。

バイト配列を作成する配列がありません。 行および列に対する 2 つだけディメンションを含む整数の配列を指定できます。

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

`MakePixel`メソッドは、32 ビットのピクセルに色の要素を結合するもう一度使用します。

同じコードを示します完全を期すのためだけですが、`SKColor`値が符号なし整数にキャストします。

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

コンス トラクター、**グラデーション カラー**ページが 8 個すべての上に示したメソッドを呼び出し、結果を保存します。

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

コンス トラクターの最後に作成、`SKCanvasView`結果のビットマップを表示します。 `PaintSurface`ハンドラーは、8 つの四角形と呼び出しに画面を除算`Display`それぞれを表示します。

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

コンパイラがコードを最適化できるように、このページを実行した**リリース**モード。 MacBook Pro、Nexus 5 Android phone、および Surface Pro 3 の Windows 10 を実行する iPhone 8 シミュレーターで実行されているそのページを次に示します。 ハードウェアの違いにより、デバイス間でパフォーマンスの時間を比較することを回避が各デバイスでの相対的なタイミングで代わりになります。

[![グラデーションのビットマップ](pixel-bits-images/GradientBitmap.png "グラデーション ビットマップ")](pixel-bits-images/GradientBitmap-Large.png#lightbox)

ここでは、実行時間 (ミリ秒単位) を統合するテーブル。

| API       | データの種類 | iOS  | Android | UWP  |
| --------- | --------- | ----:| -------:| ----:|
| SetPixel  |           | 3.17 |   10.77 | 3.49 |
| ピクセル    |           | 0.32 |    1.23 | [0.07] |
| GetPixels | byte      | 0.09 |    $0.24 | 0.10 |
|           | uint      | 0.06 |    0.26 | 0.05 |
|           | SKColor   | 0.29 |    0.99 | [0.07] |
| SetPixels | byte      | 1.33 |    6.78 | 0.11 |
|           | uint      | 0.14 |    0.69 | 0.06 |
|           | SKColor   | 0.35 |    1.93 | 0.10 |

予想どおり、呼び出す`SetPixel`65,536 回はビットマップのピクセルを設定する最小限の effeicient 方法。 入力、`SKColor`配列と設定、`Pixels`プロパティの一部のよりも有利な比較でも、`GetPixels`と`SetPixels`手法です。 操作`uint`ピクセル値は、一般に、個別の設定よりも高速`byte`コンポーネント、および変換する、`SKColor`を符号なし整数値をプロセスにいくつかのオーバーヘッドを追加します。

さまざまなグラデーションを比較する興味深いはも: プラットフォームごとの上位の行が同じで、グラデーションが表示されています。 つまり、`SetPixel`メソッドと`Pixels`プロパティが基になるピクセル形式に関係なく色からピクセルを正しく作成します。

IOS と Android のスクリーン ショットの次の 2 つの行も同じことを確認する、小さな`MakePixel`、既定のメソッドが正しく定義されている`Rgba8888`これらのプラットフォームのピクセル形式。

IOS と Android のスクリーン ショットの一番下の行をキャストすることによって取得した符号なし整数であることを示します、`SKColor`値がフォームには。

AARRGGBB

順序には、バイト数です。

BB GG RR AA

これは、`Bgra8888`順序ではなく、`Rgba8888`順序付けします。 `Brga8888`形式はそのスクリーン ショットの最後の行にグラデーションは、最初の行と同じ理由であるユニバーサル Windows プラットフォームの既定値。 中間の 2 つの行が正しくないため、これらのビットマップを作成するコードと見なされますが、`Rgba8888`順序付けします。

明示的に作成できますを各プラットフォームでのピクセル ビットにアクセスするため、同じコードを使用する場合、`SKBitmap`いずれかを使用して、`Rgba8888`または`Bgra8888`形式。 キャストする場合`SKColor`にビットマップのピクセル値を使用して、`Bgra8888`します。

## <a name="random-access-of-pixels"></a>ピクセルのランダム アクセス

`FillBitmapBytePtr`と`FillBitmapUintPtr`メソッド、**グラデーション ビットマップ**ページの使用例を確認`for`ループの一番下の行と左から右への行はごとの上位行から順番にビットマップを入力するように設計します。 ポインターのインクリメント、同じステートメントでは、ピクセルを設定できます。

ピクセルを順番にではなくランダムにアクセスするために必要な場合があります。 使用している場合、`GetPixels`アプローチでは、行と列に基づくポインターを計算する必要があります。 これは、方法については、**虹サイン**ページで、正弦曲線の 1 つのサイクルの形式で、レインボーを示すビットマップを作成します。

背景色は、HSL (色相、彩度、光度) の色のモデルを使用して作成する最も簡単です。 `SKColor.FromHsl`メソッドを作成、`SKColor`値の範囲は 0 ~ 360 (などが、円、赤、緑、青、赤にしてからの角度)、色相値を使用して、彩度、光度の値が 0 から 100 までです。 虹の色、鮮やかさを最大 100、および明るさを 50 の中間点に設定する必要があります。

**レインボー サイン**ビットマップの行をループして 360 の色相値をループ処理し、このイメージを作成します。 各色合いの値からは、サインの値にも基づいているビットマップ列を計算します。

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

コンス トラクターがに基づいて、ビットマップを作成することに注意してください、`SKColorType.Bgra8888`形式。

```csharp
bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);
```

これにより、プログラムへの変換を使用する`SKColor`に値を`uint`心配をせず (ピクセル)。 この特定のプログラムでは、ロールに再生しない、ときに使用する、`SKColor`も指定する必要があります (ピクセル単位) を設定する変換`SKAlphaType.Unpremul`ため`SKColor`アルファ値によって、その色のコンポーネントを合成しません。

コンス トラクターを使用し、`GetPixels`最初のピクセルのビットマップへのポインターを取得します。

```csharp
uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();
```

特定の行と列にオフセット値を追加する必要があります`basePtr`します。 このオフセットは、ビットマップの幅と列を乗算します。

```csharp
uint* ptr = basePtr + bitmap.Width * row + col;
```

`SKColor`値は、このポインターを使用してメモリに格納されます。

```csharp
*ptr = (uint)SKColor.FromHsl(hue, 100, 50);
```

`PaintSurface`のハンドラー、`SKCanvasView`ビットマップが表示領域をいっぱいに拡大されます。

[![レインボー サイン](pixel-bits-images/RainbowSine.png "虹サイン")](pixel-bits-images/RainbowSine-Large.png#lightbox)

## <a name="from-one-bitmap-to-another"></a>別の 1 つのビットマップから

非常に多くのイメージ処理タスクには、別に 1 つのビットマップから転送されるピクセルの変更が含まれます。 この手法の説明については、**ドライバーによる色補正**ページ。 ページがビットマップ リソースの 1 つ読み込まれ、3 つを使用してイメージを変更することができますし、`Slider`ビュー。

[![色の調整](pixel-bits-images/ColorAdjustment.png "色の調整")](pixel-bits-images/ColorAdjustment-Large.png#lightbox)

各ピクセルの色の最初の`Slider`値を 0 から 360、hue に追加しますを使用して、モジュロ 0 ~ 360 の間の結果を保持する演算子、効果的にシフト スペクトルに沿って色など、UWP のスクリーン ショットを示します)。 2 番目の`Slider`0.5 になり、鮮やかさ、および 3 番目に適用する 2 の間の乗算要素を選択することができます`Slider`Android スクリーン ショットに示すようには、光度のと同じです。

プログラムは、元のソース ビットマップという名前の 2 つのビットマップを保持`srcBitmap`という名前の調整済みのコピー先ビットマップと`dstBitmap`します。 毎回、`Slider`移動すると、プログラムのすべての新しいピクセルが計算`dstBitmap`します。 もちろん、ユーザーが移動することによって実験は、`Slider`ビューの非常に高速、最適なパフォーマンスを管理できるようにします。 これは、ためには、`GetPixels`ビットマップのソースと宛先の両方のメソッド。

**ドライバーによる色補正**ページがソースと変換先のビットマップの色の書式を制御します。 代わりに、若干異なるロジックを含まれている`SKColorType.Rgba8888`と`SKColorType.Bgra8888`形式。 ソースと変換先は、さまざまな形式を指定でき、プログラムは引き続き動作します。

を除き、重要なプログラムを次に示します`TransferPixels`ピクセル データを転送するメソッドが、先にソースを形成します。 コンス トラクターのセット`dstBitmap`等しく`srcBitmap`します。 `PaintSurface`ハンドラー `dstBitmap`:

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

`ValueChanged`のハンドラー、`Slider`ビュー調整値と呼び出しの計算`TransferPixels`します。

全体`TransferPixels`メソッドをマーク`unsafe`します。 両方のビットマップのピクセル ビットへのバイト ポインターを取得することで開始し、すべての行と列をループ処理します。 元のビットマップからは、メソッドは、各ピクセルの 4 バイトを取得します。 これらには、いずれかで、`Rgba8888`または`Bgra8888`順序。 により、色の種類のチェック、`SKColor`を作成する値。 HSL コンポーネントは、抽出、調整され、再作成するために使用、`SKColor`値。 コピー先のビットマップがかどうかに応じて`Rgba8888`または`Bgra8888`バイトが変換先 bitmp に格納されます。

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

このメソッドのパフォーマンスが元とコピー先のビットマップの色の種類のさまざまな組み合わせについての個別のメソッドを作成してさらに改善可能性があるし、すべてのピクセルに対して型チェックを避けるために可能性があります。 別のオプションは、複数`for`のループ、`col`変数色の種類に基づいています。

## <a name="posterization"></a>ポスタリゼーション

ピクセル ビットへのアクセスを必要とする別の一般的なジョブは_ポスタリゼーション_します。 制限付きのカラー パレットを使用して、手書きのポスターのように、結果をビットマップのピクセルの色がエンコードされている場合の数が減少します。

**「ポスタリゼーション」** ページ monkey イメージのいずれかでこのプロセスを実行します。

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

コンス トラクターでコードは、各ピクセルにアクセスする、0xE0E0E0FF、値のビットごとの AND 演算を実行します。 および、ビットマップに戻り、結果を格納します。 0xE0E0E0FF 値は、各色成分の上位 3 ビットを保持し、下位 5 ビットを 0 に設定します。 2 ではなく<sup>24</sup> 16,777, 216 色、ビットマップが 2 に減少または<sup>9</sup>または 512 色。

[![ポスタリゼーション](pixel-bits-images/Posterize.png "ポスタリゼーション")](pixel-bits-images/ポスタリゼーション-Large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
