---
title: SkiaSharp ビットマップの作成と描画
description: SkiaSharp ビットマップを作成し、それらに基づいてキャンバスを作成することにより、ビットマップに描画する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79BD3266-D457-4E50-BDDF-33450035FA0F
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3ca546f69dd8c4995747ad352c54e9ba184b2425
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373498"
---
# <a name="creating-and-drawing-on-skiasharp-bitmaps"></a>SkiaSharp ビットマップの作成と描画

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

アプリケーションで、Web からのビットマップ、アプリケーションリソース、およびユーザーの写真ライブラリを読み込む方法を説明しました。 また、アプリケーション内に新しいビットマップを作成することもできます。 最も簡単な方法は、のコンストラクターの1つです [`SKBitmap`](xref:SkiaSharp.SKBitmap.%23ctor(System.Int32,System.Int32,System.Boolean)) 。

```csharp
SKBitmap bitmap = new SKBitmap(width, height);
```

`width`パラメーターと `height` パラメーターは整数で、ビットマップのピクセルディメンションを指定します。 このコンストラクターは、ピクセルあたり4バイトのフルカラービットマップを作成します。各要素には、赤、緑、青、およびアルファ (不透明度) の各コンポーネントの1バイトが含まれます。

新しいビットマップを作成したら、ビットマップの表面で何かを取得する必要があります。 通常は、次の2つの方法のいずれかで行います。

- 標準の描画メソッドを使用してビットマップに描画し `Canvas` ます。
- ピクセルビットに直接アクセスします。

この記事では、最初の方法について説明します。

![描画のサンプル](drawing-images/DrawingSample.png "描画のサンプル")

2番目の方法については、記事「 [**SkiaSharp Bitmap Pixels にアクセス**](pixel-bits.md)する」をご覧ください。

## <a name="drawing-on-the-bitmap"></a>ビットマップでの描画

ビットマップの表面の描画は、ビデオディスプレイの描画と同じです。 ビデオディスプレイに描画するには、 `SKCanvas` イベント引数からオブジェクトを取得し `PaintSurface` ます。 ビットマップに描画するには、 `SKCanvas` コンストラクターを使用してオブジェクトを作成し [`SKCanvas`](xref:SkiaSharp.SKCanvas.%23ctor(SkiaSharp.SKBitmap)) ます。

```csharp
SKCanvas canvas = new SKCanvas(bitmap);
```

ビットマップでの描画が完了したら、オブジェクトを破棄でき `SKCanvas` ます。 このため、コンストラクターは `SKCanvas` 通常、ステートメントで呼び出され `using` ます。

```csharp
using (SKCanvas canvas = new SKCanvas(bitmap))
{
    ··· // call drawing function
}
```

ビットマップが表示されます。 後で、プログラムは `SKCanvas` 同じビットマップに基づいて新しいオブジェクトを作成し、それをさらに描画できます。

**[SkiaSharpFormsDemos](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** アプリケーションの **hello ビットマップ** ページには、"hello, Bitmap!" というテキストが書き込まれます。 ビットマップでは、そのビットマップが複数回表示されます。

のコンストラクターは、 `HelloBitmapPage` `SKPaint` テキストを表示するためのオブジェクトを作成することによって開始されます。 ここでは、テキスト文字列の大きさを決定し、それらの寸法を使用してビットマップを作成します。 次に、その `SKCanvas` ビットマップに基づいてオブジェクトを作成し、 `Clear` を呼び出して、を呼び出し `DrawText` ます。 `Clear`新しく作成されたビットマップにはランダムなデータが含まれている可能性があるため、必ず新しいビットマップを使用してを呼び出すことをお勧めします。

コンストラクターは、 `SKCanvasView` ビットマップを表示するオブジェクトを作成します。

```csharp
public partial class HelloBitmapPage : ContentPage
{
    const string TEXT = "Hello, Bitmap!";
    SKBitmap helloBitmap;

    public HelloBitmapPage()
    {
        Title = TEXT;

        // Create bitmap and draw on it
        using (SKPaint textPaint = new SKPaint { TextSize = 48 })
        {
            SKRect bounds = new SKRect();
            textPaint.MeasureText(TEXT, ref bounds);

            helloBitmap = new SKBitmap((int)bounds.Right,
                                       (int)bounds.Height);

            using (SKCanvas bitmapCanvas = new SKCanvas(helloBitmap))
            {
                bitmapCanvas.Clear();
                bitmapCanvas.DrawText(TEXT, 0, -bounds.Top, textPaint);
            }
        }

        // Create SKCanvasView to view result
        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Aqua);

        for (float y = 0; y < info.Height; y += helloBitmap.Height)
            for (float x = 0; x < info.Width; x += helloBitmap.Width)
            {
                canvas.DrawBitmap(helloBitmap, x, y);
            }
    }
}
```

ハンドラーは、 `PaintSurface` 表示の行と列に複数回ビットマップをレンダリングします。 `Clear`ハンドラーのメソッドに `PaintSurface` はの引数があり `SKColors.Aqua` 、表示サーフェイスの背景色が決まります。

[![Hello, Bitmap!](drawing-images/HelloBitmap.png "Hello, Bitmap!")](drawing-images/HelloBitmap-Large.png#lightbox)

水色の背景の外観は、テキストを除き、ビットマップが透明であることを示しています。

## <a name="clearing-and-transparency"></a>クリアと透明度

[ **Hello bitmap** ] ページの表示では、作成されたプログラムが黒のテキスト以外に透明であることがわかります。 そのため、ディスプレイ画面の水色の色はから表示されます。

のメソッドのドキュメントでは、 `Clear` `SKCanvas` "キャンバス ' 現在のクリップ内のすべてのピクセルを置き換える" というステートメントを使用してそれらを記述しています。 "置換" という単語を使用すると、これらのメソッドの重要な特性が明らかになります。つまり、 `SKCanvas` 既存の表示サーフェイスに何かを追加するすべての描画メソッドです。 メソッドは、 `Clear` 既に存在するものを _置き換え_ ます。

`Clear` 2つの異なるバージョンに存在します。

- [`Clear`](xref:SkiaSharp.SKCanvas.Clear(SkiaSharp.SKColor))パラメーターを持つメソッドは、 `SKColor` 画面表面のピクセルをその色のピクセルで置き換えます。

- [`Clear`](xref:SkiaSharp.SKCanvas.Clear)パラメーターを指定せずにメソッドを使用すると、ピクセルが色に置き換えられます。この色 [`SKColors.Empty`](xref:SkiaSharp.SKColors.Empty) は、すべてのコンポーネント (赤、緑、青、およびアルファ) が0に設定されています。 この色は、"透明な黒" と呼ばれることもあります。

`Clear`新しいビットマップで引数を指定せずにを呼び出すと、完全に透明になるようにビットマップ全体が初期化されます。 その後ビットマップに描画されたものは、通常、不透明または部分的に不透明になります。

次のようにします。 [ **Hello Bitmap** ] ページで、 `Clear` に適用されたメソッドを次のように置き換えます `bitmapCanvas` 。

```csharp
bitmapCanvas.Clear(new SKColor(255, 0, 0, 128));
```

コンストラクターのパラメーターの順序 `SKColor` は、赤、緑、青、およびアルファです。各値の範囲は 0 ~ 255 です。 アルファ値0は透明であるのに対し、アルファ値255は不透明であることに注意してください。

値 (255、0、0、128) は、ビットマップのピクセルを、50% の不透明度で赤ピクセルにクリアします。 これは、ビットマップの背景が半透明であることを意味します。 ビットマップの半透明の赤の背景には、画面の明るい背景色が表示されます。

初期化子に次の割り当てを指定して、テキストの色を透明な黒に設定してみ `SKPaint` ます。

```csharp
Color = new SKColor(0, 0, 0, 0)
```

この透明なテキストはビットマップの完全に透明な領域を作成し、ディスプレイ画面の水色の背景が表示されると考えられます。 しかし、それはそれほどではありません。 テキストは、ビットマップに既に存在するものの上に描画されます。 透明なテキストは表示されません。

どの `Draw` 方法を使用しても、ビットマップをより透明にすることはできません。 この `Clear` 操作を実行できるのは、だけです。

## <a name="bitmap-color-types"></a>ビットマップの色の種類

最も単純 `SKBitmap` なコンストラクターでは、ビットマップのピクセル幅と高さを整数で指定できます。 その他の `SKBitmap` コンストラクターはより複雑です。 これらのコンストラクターには、との2つの列挙型の引数が必要です [`SKColorType`](xref:SkiaSharp.SKColorType) [`SKAlphaType`](xref:SkiaSharp.SKAlphaType) 。 その他のコンストラクターは、 [`SKImageInfo`](xref:SkiaSharp.SKImageInfo) この構造体を使用して、この情報を統合します。

`SKColorType`列挙体には、9個のメンバーがあります。 これらの各メンバーは、ビットマップピクセルを格納する特定の方法を記述します。

- `Unknown`
- `Alpha8`&mdash;各ピクセルは8ビットで、完全に透明から完全に不透明なアルファ値を表します。
- `Rgb565`&mdash;各ピクセルは16ビット、赤と青は5ビット、緑の場合は6です。
- `Argb4444`&mdash;各ピクセルは16ビット、それぞれアルファ、赤、緑、および青になります。
- `Rgba8888`&mdash;各ピクセルは32ビット、赤、緑、青、およびアルファ用です。
- `Bgra8888`&mdash;各ピクセルは32ビット、それぞれ青、緑、赤、およびアルファになります。
- `Index8`&mdash;各ピクセルは8ビットであり、のインデックスを表します。[`SKColorTable`](xref:SkiaSharp.SKColorTable)
- `Gray8`&mdash;各ピクセルは、黒色から白までの灰色の網掛けを表す8ビットです。
- `RgbaF16`&mdash;各ピクセルは64ビットで、赤、緑、青、アルファは16ビットの浮動小数点の形式になります

各ピクセルが32ピクセル (4 バイト) である2つの形式は、多くの場合、 _フルカラー_ 形式と呼ばれます。 その他の書式設定の多くは、ビデオが自分で表示されたときから、完全な色には対応していませんでした。 色が制限されているビットマップは、これらの表示に対しては十分であり、使用可能なビットマップではメモリ内の領域が少なくなります。

これらの日には、ほとんどの場合、プログラマはフルカラービットマップを使用し、他の形式は使用しません。 この例外は `RgbaF16` 形式です。これにより、フルカラー形式であっても、より多くの色解像度が可能になります。 ただし、この形式は、医療イメージングなどの特別な目的で使用され、標準のフルカラー表示ではあまり意味がありません。

この一連の記事では、 `SKBitmap` メンバーが指定されていない場合に、既定で使用される色の形式に制限され `SKColorType` ます。 この既定の形式は、基になるプラットフォームに基づいています。 でサポートされているプラットフォームでは、 Xamarin.Forms 既定の色の種類は次のとおりです。

- `Rgba8888` iOS と Android の場合
- `Bgra8888` UWP の場合

唯一の違いは、メモリ内の4バイトの順序です。これは、ピクセルビットに直接アクセスした場合にのみ問題になります。 これは、 [**SkiaSharp ビットマップピクセルにアクセス**](pixel-bits.md)する記事に到達するまで重要になりません。

`SKAlphaType`列挙体には、次の4つのメンバーがあります。

- `Unknown`
- `Opaque`&mdash;ビットマップに透明度がありません
- `Premul`&mdash;カラーコンポーネントは、アルファコンポーネントによって事前に乗算されます。
- `Unpremul`&mdash;カラーコンポーネントは、アルファコンポーネントによって事前乗算されていません

次に、50% の透明度を持つ4バイトの赤のビットマップピクセルを示します。バイトは、赤、緑、青、アルファの順に表示されます。

0xFF 0x00 0x00 0x80

半透明ピクセルを含むビットマップを表示サーフェイスにレンダリングする場合は、各ビットマップピクセルのカラーコンポーネントにそのピクセルのアルファ値を乗算する必要があります。また、ディスプレイサーフェイスの対応するピクセルのカラーコンポーネントには、255を引いた値を、アルファ値から乗算する必要があります。 2つのピクセルを組み合わせることができます。 ビットマップピクセルのカラーコンポーネントが既にアルファ値によって事前に mulitplied されている場合は、ビットマップを速くレンダリングできます。 同じ赤色のピクセルは、次のように、前乗算された形式で格納されます。

0x80 0x00 0x00 0x80

このパフォーマンスの向上は `SkiaSharp` 、既定ではビットマップが形式で作成されるためです `Premul` 。 ここでも、ピクセルビットにアクセスして操作する場合にのみ、これを知る必要があります。

## <a name="drawing-on-existing-bitmaps"></a>描画 (既存のビットマップを)

新しいビットマップを作成して描画する必要はありません。 また、既存のビットマップを使用して描画することもできます。

**サル Moustache** ページは、そのコンストラクターを使用して **MonkeyFace.png** イメージを読み込みます。 次に、その `SKCanvas` ビットマップに基づいてオブジェクトを作成し、オブジェクトとオブジェクトを使用し `SKPaint` `SKPath` て moustache を描画します。

```csharp
public partial class MonkeyMoustachePage : ContentPage
{
    SKBitmap monkeyBitmap;

    public MonkeyMoustachePage()
    {
        Title = "Monkey Moustache";

        monkeyBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MonkeyFace.png");

        // Create canvas based on bitmap
        using (SKCanvas canvas = new SKCanvas(monkeyBitmap))
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                paint.StrokeWidth = 24;
                paint.StrokeCap = SKStrokeCap.Round;

                using (SKPath path = new SKPath())
                {
                    path.MoveTo(380, 390);
                    path.CubicTo(560, 390, 560, 280, 500, 280);

                    path.MoveTo(320, 390);
                    path.CubicTo(140, 390, 140, 280, 200, 280);

                    canvas.DrawPath(path, paint);
                }
            }
        }

        // Create SKCanvasView to view result
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
        canvas.DrawBitmap(monkeyBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

コンストラクターは、 `SKCanvasView` `PaintSurface` 最終的に結果を表示するだけのハンドラーを持つを作成します。

[![サル Moustache](drawing-images/MonkeyMoustache.png "サル Moustache")](drawing-images/MonkeyMoustache-Large.png#lightbox)

## <a name="copying-and-modifying-bitmaps"></a>ビットマップのコピーと変更

`SKCanvas`ビットマップで描画するために使用できるのメソッドには、があり `DrawBitmap` ます。 つまり、1つのビットマップを別のビットマップで描画できます。通常は、何らかの方法で変更します。

ビットマップを変更する最も汎用性の高い方法は、「 **[SkiaSharp ビットマップピクセルにアクセス](pixel-bits.md)** する」の記事で説明されている実際のピクセルビットにアクセスすることです。 ただし、ピクセルビットへのアクセスを必要としないビットマップを変更する方法は他にも多数あります。

**[SkiaSharpFormsDemos](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** アプリケーションに含まれる次のビットマップは、幅が360ピクセル、高さが480ピクセルです。

![マウンテン Climbers](drawing-images/MountainClimbers.jpg "マウンテン Climbers")

この写真を公開するために、左側にあるサルからアクセス許可を受け取っていないとします。 1つの解決策として、 _pixelization_ という手法を使用して、サルの顔を隠すことができます。 面のピクセルは色のブロックで置き換えられるため、機能を使用できません。 通常、色のブロックは、これらのブロックに対応するピクセルの色を平均して元のイメージから派生します。 しかし、この平均化を自分で実行する必要はありません。 これは、ビットマップを小さいピクセルディメンションにコピーするときに自動的に発生します。

左側のサルの顔は、約72ピクセルの四角形領域で、ポイント (112、238) の左上隅になります。 72ピクセルの四角形領域を、カラーブロックの 9 x 9 の配列 (それぞれ8×8ピクセルの正方形) に置き換えてみましょう。

**Pixelize Image** ページがそのビットマップに読み込まれ、最初にという小さな9ピクセルの正方形のビットマップが作成され `faceBitmap` ます。 これは、サルの顔だけをコピーするための場所です。 コピー先の四角形は9ピクセルの正方形ですが、基になる四角形は72ピクセルの四角形です。 ソースピクセルの8×8ブロックはそれぞれ、色を平均して1ピクセルに統合されます。

次の手順では、元のビットマップをと同じサイズの新しいビットマップにコピーし `pixelizedBitmap` ます。 次に、 `faceBitmap` の各ピクセルの `faceBitmap` サイズが8倍になるように、72ピクセルの四角形のコピー先の四角形を使用して、小さい方をコピーします。

```csharp
public class PixelizedImagePage : ContentPage
{
    SKBitmap pixelizedBitmap;

    public PixelizedImagePage ()
    {
        Title = "Pixelize Image";

        SKBitmap originalBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        // Create tiny bitmap for pixelized face
        SKBitmap faceBitmap = new SKBitmap(9, 9);

        // Copy subset of original bitmap to that
        using (SKCanvas canvas = new SKCanvas(faceBitmap))
        {
            canvas.Clear();
            canvas.DrawBitmap(originalBitmap,
                              new SKRect(112, 238, 184, 310),   // source
                              new SKRect(0, 0, 9, 9));          // destination

        }

        // Create full-sized bitmap for copy
        pixelizedBitmap = new SKBitmap(originalBitmap.Width, originalBitmap.Height);

        using (SKCanvas canvas = new SKCanvas(pixelizedBitmap))
        {
            canvas.Clear();

            // Draw original in full size
            canvas.DrawBitmap(originalBitmap, new SKPoint());

            // Draw tiny bitmap to cover face
            canvas.DrawBitmap(faceBitmap,
                              new SKRect(112, 238, 184, 310));  // destination
        }

        // Create SKCanvasView to view result
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
        canvas.DrawBitmap(pixelizedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

コンストラクターは、を作成して `SKCanvasView` 結果を表示します。

[![Pixelize イメージ](drawing-images/PixelizeImage.png "Pixelize イメージ")](drawing-images/PixelizeImage-Large.png#lightbox)

## <a name="rotating-bitmaps"></a>回転 (ビットマップを)

もう1つの一般的なタスクは、ビットマップの回転です。 これは、iPhone または iPad フォトライブラリからビットマップを取得するときに特に便利です。 写真が撮影されたときにデバイスが特定の向きで保持されていない限り、画像は上下左右に配置される可能性が高くなります。

ビットマップを上下に反転させるには、最初のビットマップと同じサイズで別のビットマップを作成し、最初のを2番目のにコピーするときに、変換を180度ずつ回転するように設定する必要があります。 このセクションのすべての例で、 `bitmap` は、 `SKBitmap` 回転する必要のあるオブジェクトです。

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(180, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

90度回転する場合は、高さと幅をスワップすることによって、元のサイズとは異なるサイズのビットマップを作成する必要があります。 たとえば、元のビットマップの幅が1200ピクセル、高さが800ピクセルの場合、回転したビットマップの幅は 800 1200 ピクセル、幅はピクセルになります。 ビットマップが左上隅を中心に回転してから表示されるように、平行移動と回転を設定します。 ( `Translate` `RotateDegrees` メソッドとメソッドは、適用される順序とは逆の順序で呼び出されることに注意してください)。次に、90°を時計回りに回転させるコードを示します。

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(bitmap.Height, 0);
    canvas.RotateDegrees(90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

ここでは、90°の反時計回りの回転に似た関数を示しています。

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(0, bitmap.Width);
    canvas.RotateDegrees(-90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

この2つの方法は、「 [**SkiaSharp ビットマップのトリミング**](cropping.md#cropping-skiasharp-bitmaps)」で説明されている **写真パズル** ページで使用されます。

ユーザーが90度の増分でビットマップを回転できるようにするプログラムでは、90°で回転するために1つの関数のみを実装する必要があります。 ユーザーは、この1つの関数を繰り返し実行することで、90度の任意のインクリメントでローテーションできます。

また、プログラムはビットマップを任意の大きさで回転させることもできます。 単純な方法の1つは、180を一般化された変数に置き換えることによって、180度回転する関数を変更する方法です `angle` 。

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(angle, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

ただし、一般的なケースでは、このロジックは回転したビットマップのコーナーをトリミングします。 より良い方法は、三角を使用して回転したビットマップのサイズを計算し、その角を含めることです。

この三角三角は、 **ビットマップの回転** ページに表示されます。 XAML ファイルは、とをインスタンス化します。これは、 `SKCanvasView` 現在の値を `Slider` 示すを使用して、0 ~ 360 度の範囲で指定でき `Label` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapRotatorPage"
             Title="Bitmap Rotator">
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="slider"
                Maximum="360"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Rotate by {0:F0}&#x00B0;'}"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

分離コードファイルは、ビットマップリソースを読み込み、という名前の静的な読み取り専用フィールドとして保存し `originalBitmap` ます。 ハンドラーに表示されるビットマップ `PaintSurface` は `rotatedBitmap` で、初期設定は次のように設定されてい `originalBitmap` ます。

```csharp
public partial class BitmapRotatorPage : ContentPage
{
    static readonly SKBitmap originalBitmap =
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.Banana.jpg");

    SKBitmap rotatedBitmap = originalBitmap;

    public BitmapRotatorPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(rotatedBitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double angle = args.NewValue;
        double radians = Math.PI * angle / 180;
        float sine = (float)Math.Abs(Math.Sin(radians));
        float cosine = (float)Math.Abs(Math.Cos(radians));
        int originalWidth = originalBitmap.Width;
        int originalHeight = originalBitmap.Height;
        int rotatedWidth = (int)(cosine * originalWidth + sine * originalHeight);
        int rotatedHeight = (int)(cosine * originalHeight + sine * originalWidth);

        rotatedBitmap = new SKBitmap(rotatedWidth, rotatedHeight);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear(SKColors.LightPink);
            canvas.Translate(rotatedWidth / 2, rotatedHeight / 2);
            canvas.RotateDegrees((float)angle);
            canvas.Translate(-originalWidth / 2, -originalHeight / 2);
            canvas.DrawBitmap(originalBitmap, new SKPoint());
        }

        canvasView.InvalidateSurface();
    }
}
```

`ValueChanged`のハンドラーは、 `Slider` 回転角度に基づいて新しいを作成する操作を実行し `rotatedBitmap` ます。 新しい幅と高さは、サインの絶対値と、元の幅と高さをコサインした値に基づいています。 回転したビットマップの元のビットマップを描画するために使用される変換では、元のビットマップの中心を原点に移動した後、指定した度数だけ回転してから、その中心を回転したビットマップの中央に変換します。 ( `Translate` メソッドと `RotateDegrees` メソッドは、適用方法とは逆の順序で呼び出されます)。

メソッドを使用して `Clear` 薄いピンクの背景を設定することに注意して `rotatedBitmap` ください。 これは、ディスプレイののサイズを示すためだけに表示され `rotatedBitmap` ます。

[![ビットマップの回転](drawing-images/BitmapRotator.png "ビットマップの回転")](drawing-images/BitmapRotator-Large.png#lightbox)

回転したビットマップは、元のビットマップ全体を格納するのに十分な大きさですが、それ以上のサイズではありません。

## <a name="flipping-bitmaps"></a>ビットマップの反転

通常、ビットマップに対して実行されるもう1つの操作は、 _反転_ と呼ばれます。 概念的には、ビットマップは、垂直軸または水平軸を中心に、ビットマップの中心を通る3次元で回転します。 垂直方向の反転では、ミラーイメージが作成されます。

**[SkiaSharpFormsDemos](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** アプリケーションの **ビットマップフリッパー** ページは、これらのプロセスを示しています。 XAML ファイルには、 `SKCanvasView` 垂直方向および水平方向に反転するためのボタンと2つのボタンが含まれています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapFlipperPage"
             Title="Bitmap Flipper">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Button Text="Flip Vertical"
                Grid.Row="1" Grid.Column="0"
                Margin="0, 10"
                Clicked="OnFlipVerticalClicked" />

        <Button Text="Flip Horizontal"
                Grid.Row="1" Grid.Column="1"
                Margin="0, 10"
                Clicked="OnFlipHorizontalClicked" />
    </Grid>
</ContentPage>
```

分離コードファイルは、これらの2つの操作 `Clicked` をボタンのハンドラーに実装します。

```csharp
public partial class BitmapFlipperPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public BitmapFlipperPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnFlipVerticalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(-1, 1, bitmap.Width / 2, 0);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnFlipHorizontalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(1, -1, 0, bitmap.Height / 2);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }
}
```

垂直方向の反転は、水平方向のスケールファクターが1であるスケーリング変換によって実現され &ndash; ます。 スケーリングセンターは、ビットマップの垂直方向の中心です。 水平方向の反転は、垂直方向のスケールファクターが1に設定されたスケーリング変換です &ndash; 。

サルのシャツの反転された文字からわかるように、反転は回転と同じではありません。 しかし、右側の UWP スクリーンショットに示すように、水平方向と垂直方向の反転は、180度の回転と同じです。

[![ビットマップフリッパー](drawing-images/BitmapFlipper.png "ビットマップフリッパー")](drawing-images/BitmapFlipper-Large.png#lightbox)

同様の手法を使用して処理できるもう1つの一般的なタスクは、ビットマップを四角形のサブセットにトリミングすることです。 これについては、次の記事「 [**SkiaSharp ビットマップのトリミング**](cropping.md)」で説明されています。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)