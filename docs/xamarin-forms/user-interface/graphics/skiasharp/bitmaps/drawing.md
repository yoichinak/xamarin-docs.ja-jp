---
title: 作成して、SkiaSharp ビットマップの描画
description: SkiaSharp のビットマップを作成し、それらに基づくキャンバスを作成してこれらのビットマップ上で描画する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79BD3266-D457-4E50-BDDF-33450035FA0F
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: acdee7d34f913b125887f021dab39220c9560191
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109238"
---
# <a name="creating-and-drawing-on-skiasharp-bitmaps"></a>作成して、SkiaSharp ビットマップの描画

どのアプリケーション ビットマップをロードできます、Web、アプリケーション リソース、およびユーザーのフォト ライブラリから見てきました。 アプリケーション内で新しいビットマップを作成することもできます。 最も簡単な方法では、1 つのコンス トラクターの[ `SKBitmap` ](xref:SkiaSharp.SKBitmap.%23ctor(System.Int32,System.Int32,System.Boolean)):

```csharp
SKBitmap bitmap = new SKBitmap(width, height);
```

`width`と`height`パラメーターが整数であり、ビットマップのピクセル寸法を指定します。 このコンス トラクターは、1 ピクセルあたり 4 バイトの完全なカラー ビットマップを作成します。 赤、緑、青、およびアルファ (透明度) のコンポーネントの 1 バイトごと。 

新しいビットマップを作成した後は、ビットマップの画面上の何かを取得する必要があります。 2 つの方法のいずれかで一般に行います。

- 標準を使用してビットマップ上で描画`Canvas`メソッドを描画します。
- ピクセル ビットに直接アクセスします。

この記事では、最初の方法を示しています。

![サンプルの描画](drawing-images/DrawingSample.png "サンプルの描画")

2 番目の方法が、記事で説明した[ **SkiaSharp ビットマップのピクセルへのアクセス**](pixel-bits.md)します。

## <a name="drawing-on-the-bitmap"></a>ビットマップの描画

ビットマップの表面に描画すると、ビデオ ディスプレイ上に描画と同じです。 取得するビデオ ディスプレイ上で描画する、`SKCanvas`オブジェクトから、`PaintSurface`イベント引数。 作成するビットマップの描画、するために、`SKCanvas`オブジェクトを使用して、 [ `SKCanvas` ](xref:SkiaSharp.SKCanvas.%23ctor(SkiaSharp.SKBitmap))コンス トラクター。

```csharp
SKCanvas canvas = new SKCanvas(bitmap);
```

ビットマップの描画が完了したらを破棄することができます、`SKCanvas`オブジェクト。 このため、`SKCanvas`コンス トラクターの呼び出し一般に、`using`ステートメント。

```csharp
using (SKCanvas canvas = new SKCanvas(bitmap))
{
    ··· // call drawing function
}
```

ビットマップは、表示されます。 後では、プログラムを新規に作成できます`SKCanvas`に依存するオブジェクトが同じ、ビットマップ、さらに描画します。

**こんにちはビットマップ**ページで、 **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** アプリケーションが書き込むテキスト「こんにちは, ビットマップ!」 ビットマップと複数回のビットマップを表示します。  

コンス トラクター、`HelloBitmapPage`作成では、まず、`SKPaint`テキストを表示するオブジェクト。 テキスト文字列の大きさを決定し、その寸法のビットマップを作成します。 これは、後、作成、`SKCanvas`オブジェクトを呼び出し、そのビットマップに基づいて`Clear`、号餧ェヒェマル`DrawText`。 常を呼び出すことをお勧め`Clear`新しいビットマップで新しく作成されたビットマップにランダムなデータが含まれる場合がありますので。

コンス トラクターの最後に作成、`SKCanvasView`ビットマップを表示するオブジェクト。

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

`PaintSurface`ハンドラーが複数回の表示の行と列のビットマップをレンダリングします。 注意、`Clear`メソッドで、`PaintSurface`ハンドラーの引数には、`SKColors.Aqua`画面の背景の色します。

[![こんにちは, ビットマップ!](drawing-images/HelloBitmap.png "こんにちは, ビットマップ。")](drawing-images/HelloBitmap-Large.png#lightbox)

水色の背景の外観は、ビットマップが透明のテキストを除くことがわかります。

## <a name="clearing-and-transparency"></a>消去と透明度

表示、**こんにちはビットマップ**ページは、ビットマップを作成するプログラムが透明こと黒色の文字を除くを示します。 理由を画面の色を水色を示しています。

ドキュメント、`Clear`メソッドの`SKCanvas`ステートメントを使用してそれらをについて説明します"キャンバスの現在のクリップ内のすべてのピクセルを置き換えます。"。 「置換」という単語の使用がこれらのメソッドの重要な特性が表示されます: すべての描画メソッドの`SKCanvas`何か、既存の画面に追加します。 `Clear`メソッド_置換_は既に存在します。

`Clear` 2 つのバージョンに存在します。 

- [ `Clear` ](xref:SkiaSharp.SKCanvas.Clear(SkiaSharp.SKColor))メソッドを`SKColor`パラメーターは、その色のピクセルを使用して、表示画面のピクセルを置き換えます。

- [ `Clear` ](xref:SkiaSharp.SKCanvas.Clear)パラメーターなしのメソッドは、ピクセルを置き換える、 [ `SKColors.Empty` ](xref:SkiaSharp.SKColors.Empty)は 0 に設定されます (赤、緑、青、およびアルファ) コンポーネントのすべての色の色。 この色は、「透明な黒」と呼ばれることがあります。

呼び出す`Clear`新しいビットマップを引数なしで完全に透過的にするビットマップ全体を初期化します。 非透過または部分的に非透過的なビットマップの描画後何も通常なります。

試すものを次に示します: で、**こんにちはビットマップ** ページで、置換、`Clear`メソッドに適用される、`bitmapCanvas`をこのデータセット。

```csharp
bitmapCanvas.Clear(new SKColor(255, 0, 0, 128));
```

順序、`SKColor`コンス トラクターのパラメーターは、赤、緑、青、およびアルファ、各値は 0 から 255 までの場所。 アルファ値 255 は不透明アルファ値は 0 が透明であることに留意してください。

(255、0, 0, 128) 値を 50% の不透明度の赤いピクセルのビットマップのピクセルをクリアします。 これは、ビットマップの背景が半透明であることを意味します。 ビットマップの半透明の赤の背景は灰色の背景を作成する画面の水色の背景と組み合わせます。

次の割り当てを配置することで、テキストの色を透明な黒色に設定してください、`SKPaint`初期化子。

```csharp
Color = new SKColor(0, 0, 0, 0)
```

この透過的なテキストが、画面の水色の背景が表示されるビットマップの完全に透明な領域を作成と思うかもしれません。 そんなことはありません。 ビットマップに既にあるものの上にテキストを描画します。 透過的なテキストにはまったくで表示されません。

いいえ`Draw`メソッドこれまで、ビットマップは透明にします。 のみ`Clear`ことができます。

## <a name="bitmap-color-types"></a>ビットマップの色の種類

最も単純な`SKBitmap`コンス トラクターでは、ビットマップの高さとピクセル幅の整数を指定できます。 その他の`SKBitmap`コンス トラクターは複雑になります。 これらのコンス トラクターの 2 つの列挙型の引数が必要: [ `SKColorType` ](xref:SkiaSharp.SKColorType)と[ `SKAlphaType`](xref:SkiaSharp.SKAlphaType)します。 他のコンス トラクターを使用して、 [ `SKImageInfo` ](xref:SkiaSharp.SKImageInfo)構造体は、この情報を統合します。

`SKColorType`列挙体には 9 のメンバー。 各メンバーには、特定のビットマップのピクセルを格納する方法について説明します。

- `Unknown`
- `Alpha8` &mdash; 各ピクセルは 8 ビット、完全に透明から完全に不透明にアルファ値を表す
- `Rgb565` &mdash; 各ピクセルが 16 ビット、5 ビットの赤と青と緑の 6
- `Argb4444` &mdash; 各ピクセルはアルファ、赤、緑、および青の 4 つの 16 ビットです。
- `Rgba8888` &mdash; 各ピクセルが 32 ビット、赤、緑、青、およびアルファの 8 各
- `Bgra8888` &mdash; 各ピクセルが 32 ビット、青、緑、赤、およびアルファの 8 各
- `Index8` &mdash; 各ピクセルは 8 ビットでありへのインデックスを表す、 [`SKColorTable`](xref:SkiaSharp.SKColorTable)
- `Gray8` &mdash; 各ピクセルは白、黒から灰色の網かけを表す 8 ビットです。
- `RgbaF16` &mdash; 各ピクセルは赤、緑、青、および 16 ビットの浮動小数点形式でアルファとの 64 ビットです。

各ピクセルが 32 ピクセル (4 バイト) を 2 つの形式とも呼ばれます_フルカラー_形式。 ビデオが自身を表示するときに時間から他の形式の日付の多くは完全な色の対応でした。 制限付きの色のビットマップは、これらの表示に適してし、占有するメモリの領域を節約するビットマップを許可します。 

最近は、プログラマほぼ常にフルカラー ビットマップを使用して、およびその他の形式を使用する必要はありません。 例外は、`RgbaF16`形式で、色の解像度でもフルカラー形式よりも大きいを許可します。 ただし、この形式は医用画像などの特殊な目的で使用され、標準の完全なカラー ディスプレイを使用すると、あまり意味します。

自体には、制限するこの一連の記事、`SKBitmap`色の既定ではない場合に使用される形式`SKColorType`メンバーが指定されました。 この既定の形式は、基になるプラットフォームに基づきます。 Xamarin.Forms でサポートされるプラットフォーム、既定の色の種類は次のとおりです。

- `Rgba8888` iOS および Android 用
- `Bgra8888` UWP の

唯一の違いは、メモリ内の 4 バイトの順序とピクセル ビットを直接アクセスするときにのみ問題になるこれです。 これはありませんが重要になります、情報の記事が表示されるまで[ **SkiaSharp ビットマップのピクセルへのアクセス**](pixel-bits.md)します。

`SKAlphaType`列挙体には 4 つのメンバー。

- `Unknown`
- `Opaque` &mdash; ビットマップに透過性がありません。
- `Premul` &mdash; アルファ値によって左側から乗算は色のコンポーネント
- `Unpremul` &mdash; アルファ コンポーネントでの色要素が事前乗算されていません。

4 バイトの赤いビットマップ ピクセルで、バイトの順序の赤、緑、青、アルファに示すように、50% の透明度を次に示します。

0xFF 0x00 0x00 0x80

表示画面で半透明ピクセルを含むビットマップがレンダリングされると、ピクセルのアルファ値が各ビットマップのピクセルの色要素を乗算する必要があり。、画面の対応するピクセルの色要素を乗算する必要があります。で 255 のアルファ値を減算します。 2 つのピクセルに結合できます。 ビットマップ場合に表示できる高速ビットマップのピクセルの色要素の前 mulitplied 済みアルファ値。 その同じ赤いピクセル前乗算された形式で次のように格納されます。

0x80 0x00 0x00 0x80

このパフォーマンス向上のため`SkiaSharp`で既定のビットマップが作成された、`Premul`形式。 アクセスして、ピクセル ビットを操作する場合にのみ、これを把握する必要がもう一度。

## <a name="drawing-on-existing-bitmaps"></a>既存のビットマップの描画

描画するために新しいビットマップを作成する必要はありません。 既存のビットマップを描画することもできます。 

**Monkey あります**ページでは、そのコンス トラクターを使用して、読み込み、 **MonkeyFace.png**イメージ。 これは、後、作成、`SKCanvas`オブジェクトがそのビットマップに基づいて、使用して`SKPaint`と`SKPath`ありますを描画するオブジェクト。

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

コンス トラクターの最後に作成、`SKCanvasView`が`PaintSurface`ハンドラーは単に結果を表示します。

[![モンキーあります](drawing-images/MonkeyMoustache.png "モンキーあります")](drawing-images/MonkeyMoustache-Large.png#lightbox)

## <a name="copying-and-modifying-bitmaps"></a>コピーしてビットマップを変更します。

メソッド`SKCanvas`描画するために使用できるビットマップを含める`DrawBitmap`します。 これは、描画できるビットマップが 1 つ、別の通常、何らかの方法で変更することを意味します。

ビットマップを変更する最も汎用的な方法は、実際のピクセル ビットへのアクセスを使用、サブジェクトが、記事で取り上げる **[にアクセスする SkiaSharp ビットマップ ピクセル](pixel-bits.md)** します。 ビットマップのピクセル ビットへのアクセスを必要としないを変更するその他の多くの手法があります。

次に含まれているビットマップ、 **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** アプリケーションは 360 ピクセル、高さが 480 ピクセル。

![登山愛好者](drawing-images/MountainClimbers.jpg "登山愛好者")

たとえば、アクセス許可は、この写真を公開する左側の monkey から受信していません。 1 つのソリューションと呼ばれる手法を使用して、monkey の顔が見えにくく_pixelization_します。 面のピクセルは、機能することはできませんので、色のブロックに置き換えられます。 色のブロックは、通常、これらのブロックに対応するピクセルの色の平均を計算して、元のイメージから派生します。 ただし、自分でこの平均を実行する必要はありません。 小さいピクセル寸法にビットマップをコピーするときに自動的に発生します。 

左 monkey の面では、約 72 ピクセルの正方形の領域 (112、238) 時点で、左上隅を占有します。 8-8 でピクセルの正方形は、それぞれの色付きのブロックの 9-9 で配列を 72 ピクセルの四角形の領域で置換しましょう。

**Pixelize イメージ**ページでそのビットマップが表示されと呼ばれる小さな 9 ピクセルの正方形ビットマップをまず作成`faceBitmap`です。 これは、変換先、monkey の面だけをコピーするためです。 先の四角形は単なる 9 ピクセルの正方形が元の四角形は 72 ピクセルの四角形。 ソース ピクセルの各 8-8 によってブロックは、色を平均して 1 つだけのピクセルに統合されます。

次の手順と呼ばれる同じサイズの新しいビットマップに、元のビットマップをコピーするは`pixelizedBitmap`します。 小さな`faceBitmap`72 ピクセルの正方形の変換先の四角形とそこにコピーし、ように各ピクセルの`faceBitmap`8 倍のサイズに拡張されます。

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

コンス トラクターの最後に作成、`SKCanvasView`結果を表示します。

[![イメージを pixelize](drawing-images/PixelizeImage.png "Pixelize イメージ")](drawing-images/PixelizeImage-Large.png#lightbox)

<a name="rotating-bitmaps" />

## <a name="rotating-bitmaps"></a>ビットマップの回転

もう 1 つの一般的なタスクには、ビットマップは回転させます。 これは、iPhone または iPad のフォト ライブラリからビットマップを取得するときに特に便利です。 デバイスが保持されている特定の方向で、写真の撮影としない限り、画像は、上下または横にある可能性があります。

ビットマップの上下を有効にするには、1 番目と同じサイズのもう 1 つのビットマップを作成し、最初の 2 番目のコピー中に 180 度回転する変換を設定する必要があります。 このセクションの例のすべての`bitmap`は、`SKBitmap`オブジェクトを回転する必要があります。

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(180, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

90 度回転の場合は、高さと幅をスワップして、元のサイズと異なるはビットマップを作成する必要があります。 たとえば、元のビットマップが 1200 ピクセル、高さ 800 ピクセルの場合は、回転したビットマップは 800 ピクセル、幅が 1200 ピクセルは。 平行移動と回転を設定して、ビットマップがその左上隅を中心に回転し、ビューに移動し、ようにします。 (することに注意、`Translate`と`RotateDegrees`メソッドが適用されている方法のために逆の順序で呼び出されます)。時計回りに 90 度回転させるコードを次に示します。

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

同様の関数を反時計回りに 90 度回転を次に示します。

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

これら 2 つのメソッドで使用される、**パズルの写真**、情報の記事で説明したページ[ **SkiaSharp ビットマップをトリミング**](cropping.md#tile-division)します。

ビットマップが 90 度ずつ回転できるプログラムは、90 度回転させる 1 つの関数を実装のみ必要があります。 ユーザーは、この 1 つの関数の実行を繰り返すして 90 度のすべての増分で回転できます。

プログラムでは、任意の量によってビットマップを回転することもできます。 1 つの簡単な方法は、一般化されたを 180 に置き換えて、180 度回転する関数を変更する、`angle`変数。

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(angle, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

ただし、一般的なケースでは、このロジックは回転したビットマップの角に丸みをトリミングします。 三角関数を使用して、これらの角を含める回転したビットマップのサイズを計算することをお勧めします。 

この三角法が示した、**ビットマップ ローテータ**ページ。 XAML ファイルのインスタンスを作成、`SKCanvasView`と`Slider`する範囲は 0 ~ 360 度、`Label`現在の値を表示します。

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

分離コード ファイルは、ビットマップ リソースを読み込むし、という名前の静的な読み取り専用フィールドとして保存します`originalBitmap`します。 表示されるビットマップ、`PaintSurface`ハンドラーが`rotatedBitmap`に最初に設定されています`originalBitmap`:

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

`ValueChanged`のハンドラー、`Slider`を新規作成操作を実行します`rotatedBitmap`回転角度に基づいて。 新しい幅と高さは、サインの absolute values と元の幅と高さの余弦に基づいています。 回転のビットマップに、元のビットマップを描画するために使用される変換は、配信元に、元のビットマップ センターに移動によって指定された数の角度の回転し、その center ビットマップを回転の中心に変換します。 (、`Translate`と`RotateDegrees`を適用する方法よりも、その逆の順序でメソッドが呼び出されます)。

使用に注意してください、`Clear`メソッドの背景を`rotatedBitmap`ライト pink」と入力します。 サイズを示すためにのみこれは`rotatedBitmap`表示にします。

[![ビットマップの回転](drawing-images/BitmapRotator.png "ビットマップ ローテータ")](drawing-images/BitmapRotator-Large.png#lightbox)

回転したビットマップが十分な大きさが、元のビットマップ全体を含めるです。

## <a name="flipping-bitmaps"></a>ビットマップを反転

ビットマップに一般的に実行される別の操作が呼び出された_反転_します。 概念的には、ビットマップが 3 次元垂直軸またはビットマップの中心を通るの水平軸の周りで回転します。 垂直方向の反転ミラー イメージを作成します。

**ビットマップ フリッパー**ページで、 **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** アプリケーション demonstates これらのプロセス。 XAML ファイルが含まれています、`SKCanvasView`と垂直方向および水平方向に反転の 2 つのボタン。

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

分離コード ファイルでこれら 2 つの操作を実装する、`Clicked`ボタンのハンドラー。 

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

水平方向のスケーリング ファクターでスケーリングの変換によって、垂直方向に反転が実現されます&ndash;1。 スケーリングの中心は、ビットマップの垂直方向の中央です。 水平方向に反転の垂直方向のスケーリング ファクターでスケーリングの変換は、 &ndash;1。 

Monkey のシャツで反転レタリングからわかる、反転の回転と同じではありません。 右側の UWP スクリーン ショットに示すようは 180 度回転と同じでは両方の水平および垂直方向に反転します。

[![ビットマップのフリッパー](drawing-images/BitmapFlipper.png "フリッパーのビットマップ")](drawing-images/BitmapFlipper-Large.png#lightbox)

同様の手法を使用して処理できる別の一般的なタスクには、四角形のサブセットにビットマップをトリミングは。 次の記事「 [ **SkiaSharp ビットマップをトリミング**](cropping.md)します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
