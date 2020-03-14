---
title: SkiaSharp のビットマップの表示
description: SkiaSharp のピクセルのビットマップのサイズし、縦横比を維持しながら、四角形を入力するように拡張を表示する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8E074F8D-4715-4146-8CC0-FD7A8290EDE9
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 9955b68346c74435a3a141c69d02e1bec5856bd3
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305655"
---
# <a name="displaying-skiasharp-bitmaps"></a>SkiaSharp のビットマップの表示

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp ビットマップの件名は、 **[SkiaSharp の「ビットマップの基礎](../basics/bitmaps.md)** 」で導入されました。 その記事では、負荷のビットマップに 3 つの方法とビットマップを表示する 3 つの方法を示しました。 この記事では、ビットマップを読み込み、`SKCanvas`の `DrawBitmap` メソッドの使用方法について詳しく説明します。

![サンプルの表示](displaying-images/DisplayingSample.png "サンプルの表示")

`DrawBitmapLattice` と `DrawBitmapNinePatch` の方法については、 **[SkiaSharp ビットマップのセグメント](segmented.md)** 化された表示に関する記事をご覧ください。

このページのサンプルは、 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** アプリケーションからのものです。 そのアプリケーションのホームページで、 **[SkiaSharp ビットマップ]** を選択し、 **[ビットマップの表示]** セクションにアクセスします。

## <a name="loading-a-bitmap"></a>ビットマップの読み込み

SkiaSharp のアプリケーションで一般的に使用されるビットマップでは、次の 3 つの異なるソースのいずれかのです。

- インターネットを介した
- 実行可能ファイルに埋め込まれたリソースから
- ユーザーのフォト ライブラリから

SkiaSharp アプリケーションで、新しいビットマップを作成しとに描画またはアルゴリズム ビットマップのビットを設定することもできます。 これらの手法については、 **[SkiaSharp ビットマップの作成と描画](drawing.md)** 、および **[SkiaSharp ビットマップピクセルへのアクセス](pixel-bits.md)** に関する記事で説明されています。

ビットマップを読み込むための次の3つのコード例では、クラスに `SKBitmap`型のフィールドが含まれていると想定されます。

```csharp
SKBitmap bitmap;
```

**[SkiaSharp の記事ビットマップの基礎](../basics/bitmaps.md)** として、インターネット上にビットマップを読み込む最適な方法は、 [`HttpClient`](xref:System.Net.Http.HttpClient)クラスを使用することです。 クラスの 1 つのインスタンスは、フィールドとして定義できます。

```csharp
HttpClient httpClient = new HttpClient();
```

IOS アプリケーションと Android アプリケーションで `HttpClient` を使用する場合は、 **[トランスポート層セキュリティ (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)** のドキュメントで説明されているように、プロジェクトのプロパティを設定することをお勧めします。

`HttpClient` を使用するコードは、多くの場合、`await` 演算子を伴うため、`async` メソッドに存在する必要があります。

```csharp
try
{
    using (Stream stream = await httpClient.GetStreamAsync("https:// ··· "))
    using (MemoryStream memStream = new MemoryStream())
    {
        await stream.CopyToAsync(memStream);
        memStream.Seek(0, SeekOrigin.Begin);

        bitmap = SKBitmap.Decode(stream);
        ···
    };
}
catch
{
    ···
}
```

`GetStreamAsync` から取得した `Stream` オブジェクトが `MemoryStream`にコピーされていることに注意してください。 Android では、非同期メソッドを除き、メインスレッドで `HttpClient` の `Stream` を処理することはできません。 

[`SKBitmap.Decode`](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream))は多くの作業を行います。これに渡される `Stream` オブジェクトは、一般的なビットマップファイル形式 (通常は JPEG、PNG、GIF) のいずれかでビットマップ全体を含むメモリブロックを参照します。 `Decode` メソッドは、形式を決定し、ビットマップファイルを SkiaSharp の内部ビットマップ形式にデコードする必要があります。

コードが `SKBitmap.Decode`を呼び出すと、`PaintSurface` ハンドラーが新しく読み込まれたビットマップを表示できるように、`CanvasView` が無効になる可能性があります。

ビットマップを読み込む 2 つ目の方法は、.NET Standard ライブラリ内の埋め込みリソースとして、ビットマップを含めることによってによって参照される個別のプラットフォーム プロジェクト。 [`GetManifestResourceStream`](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String))メソッドにリソース ID が渡されます。 このリソース ID は、アセンブリ名、フォルダー名、およびピリオドで区切られたリソースのファイル名で構成されます。

```csharp
string resourceID = "assemblyName.folderName.fileName";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    bitmap = SKBitmap.Decode(stream);
    ···
}
```

ビットマップ ファイルは、iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) 用の個別のプラットフォーム プロジェクトにリソースとしても格納できます。 ただし、これらのビットマップを読み込むには、プラットフォーム プロジェクトに配置されているコードが必要です。

ビットマップを取得するための 3 番目のアプローチは、ユーザーの画像ライブラリです。 次のコードでは、 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** アプリケーションに含まれている依存関係サービスを使用しています。 **SkiaSharpFormsDemo** .NET Standard ライブラリには `IPhotoLibrary` インターフェイスが含まれていますが、各プラットフォームプロジェクトには、そのインターフェイスを実装する `PhotoLibrary` クラスが含まれています。

```csharp
IPhotoicturePicker picturePicker = DependencyService.Get<IPhotoLibrary>();

using (Stream stream = await picturePicker.GetImageStreamAsync())
{
    if (stream != null)
    {
        bitmap = SKBitmap.Decode(stream);
        ···
    }
}
```

通常、このようなコードは、`PaintSurface` ハンドラーが新しいビットマップを表示できるように、`CanvasView` も無効にします。

`SKBitmap` クラスは、ビットマップのピクセルディメンションを表示する、 [`Width`](xref:SkiaSharp.SKBitmap.Width)や[`Height`](xref:SkiaSharp.SKBitmap.Height)などの便利なプロパティをいくつか定義します。また、ビットマップを作成し、それらをコピーして、ピクセルビットを公開するメソッドなど、多くのメソッドを定義します。 

## <a name="displaying-in-pixel-dimensions"></a>ピクセル数で表示します。

SkiaSharp [`Canvas`](xref:SkiaSharp.SKCanvas)クラスは、4つの `DrawBitmap` メソッドを定義します。 これらのメソッドは、根本的に異なる 2 つの方法で表示されるビットマップを許可します。 

- `SKPoint` 値 (または個別の `x` と `y` 値) を指定すると、ビットマップがピクセルディメンションで表示されます。 ビットマップのピクセルは、ビデオ ディスプレイのピクセルに直接マップされます。
- 四角形を指定すると、四角形の形状とサイズに拡大するビットマップとします。 

ピクセルディメンションにビットマップを表示するには、 [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKPoint,SkiaSharp.SKPaint))を `SKPoint` パラメーターと共に使用するか、個別の `x` と `y` パラメーターを使用して[`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint))します。

```csharp
DrawBitmap(SKBitmap bitmap, SKPoint pt, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, float x, float y, SKPaint paint = null)
```

これら 2 つの方法では、機能的に同じです。 指定したポイントでは、ビットマップをキャンバスの左上隅の場所を示します。 モバイル デバイスのピクセルの解像度は非常に高いため、小さいビットマップは通常、これらのデバイスでは非常に狭かった表示されます。

省略可能な `SKPaint` パラメーターを使用すると、透明度を使用してビットマップを表示できます。 これを行うには、`SKPaint` オブジェクトを作成し、`Color` プロパティに、アルファチャネルが1未満の任意の `SKColor` 値を設定します。 例 :

```csharp
paint.Color = new SKColor(0, 0, 0, 0x80);
```

最後の引数として渡される 0x80 では、透明度が 50% を示します。 定義済みの色のいずれかでアルファ チャネルを設定することもできます。

```csharp
paint.Color = SKColors.Red.WithAlpha(0x80);
```

ただし、色自体では、関係ありません。 `DrawBitmap` 呼び出しで `SKPaint` オブジェクトを使用すると、アルファチャネルのみが検査されます。

`SKPaint` オブジェクトは、blend モードまたはフィルター効果を使用してビットマップを表示するときにも役割を果たします。 これらの情報については、「 [SkiaSharp 合成」と「blend モード](../effects/blend-modes/index.md)と[SkiaSharp イメージフィルター](../effects/image-filters.md)」の記事で説明されています。

**[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** サンプルプログラムの **[ピクセルディメンション]** ページには、320ピクセル、高さが240ピクセルのビットマップリソースが表示されます。

```csharp
public class PixelDimensionsPage : ContentPage
{
    SKBitmap bitmap;

    public PixelDimensionsPage()
    {
        Title = "Pixel Dimensions";

        // Load the bitmap from a resource
        string resourceID = "SkiaSharpFormsDemos.Media.Banana.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }

        // Create the SKCanvasView and set the PaintSurface handler
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

        float x = (info.Width - bitmap.Width) / 2;
        float y = (info.Height - bitmap.Height) / 2;

        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

`PaintSurface` ハンドラーは、ディスプレイ画面のピクセル寸法とビットマップのピクセルサイズに基づいて `x` と `y` 値を計算することによって、ビットマップを中央揃えにします。

[![ピクセルディメンション](displaying-images/PixelDimensions.png "ピクセルディメンション")](displaying-images/PixelDimensions-Large.png#lightbox)

座標を単に渡す場合、アプリケーションの左上隅にあるビットマップを表示する場合、(0, 0)。 

## <a name="a-method-for-loading-resource-bitmaps"></a>リソース ビットマップを読み込むためのメソッド

次のサンプルの多くは、ビットマップ リソースを読み込む必要があります。 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** ソリューションの静的 `BitmapExtensions` クラスには、次のことを支援するメソッドが含まれています。

```csharp
static class BitmapExtensions
{
    public static SKBitmap LoadBitmapResource(Type type, string resourceID)
    {
        Assembly assembly = type.GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            return SKBitmap.Decode(stream);
        }
    }
    ···
}
```

`Type` パラメーターに注意してください。 これには、ビットマップリソースを格納するアセンブリ内の任意の型に関連付けられた `Type` オブジェクトを指定できます。

この `LoadBitmapResource` メソッドは、ビットマップリソースを必要とする後続のすべてのサンプルで使用されます。

## <a name="stretching-to-fill-a-rectangle"></a>四角形の塗りつぶしへの拡張

`SKCanvas` クラスは、ビットマップを四角形にレンダリングする[`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint))メソッドと、ビットマップの四角形のサブセットを四角形にレンダリングする別の[`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint))メソッドも定義します。

```
DrawBitmap(SKBitmap bitmap, SKRect dest, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, SKRect source, SKRect dest, SKPaint paint = null)
```

どちらの場合も、ビットマップは `dest`という名前の四角形を埋めるように拡大されます。 2番目の方法では、`source` 四角形を使用して、ビットマップのサブセットを選択できます。 `dest` の四角形は、出力デバイスを基準にしています。`source` 四角形は、ビットマップに対して相対的です。

**[四角形の塗りつぶし]** ページでは、前の例で使用したものと同じビットマップを四角形に表示することで、最初の2つの方法を示しています。 

```csharp
public class FillRectanglePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public FillRectanglePage ()
    {
        Title = "Fill Rectangle";

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

新しい `BitmapExtensions.LoadBitmapResource` メソッドを使用して、`SKBitmap` フィールドを設定することに注意してください。 コピー先の四角形は `SKImageInfo`の [ [`Rect`](xref:SkiaSharp.SKImageInfo.Rect) ] プロパティから取得されます。これにより、ディスプレイ画面のサイズが決まります。

[![塗りつぶし四角形](displaying-images/FillRectangle.png "塗りつぶし四角形")](displaying-images/FillRectangle-Large.png#lightbox)

通常、この方法は必要_ありません_。 水平および垂直方向に異なる方法で拡張されたによってイメージがゆがめられます。 ピクセル サイズ以外の方法で、ビットマップを表示するときに通常するビットマップの元の縦横比を保持します。

## <a name="stretching-while-preserving-the-aspect-ratio"></a>縦横比を維持しながら拡張

縦横比を維持したままビットマップを拡大するプロセスは、_一様なスケーリング_とも呼ばれます。 この用語は、アルゴリズムのアプローチを提案します。 1つの考えられる解決方法を、**一様なスケーリング**のページに示します。

```csharp
public class UniformScalingPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(UniformScalingPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public UniformScalingPage()
    {
        Title = "Uniform Scaling";

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

        float scale = Math.Min((float)info.Width / bitmap.Width, 
                               (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect destRect = new SKRect(x, y, x + scale * bitmap.Width, 
                                           y + scale * bitmap.Height);

        canvas.DrawBitmap(bitmap, destRect);
    }
}
```

`PaintSurface` ハンドラーは、ビットマップの幅と高さに対する表示幅と高さの最小値である `scale` 係数を計算します。 次に、`x` と `y` の値を計算して、拡大縮小されたビットマップを表示幅と高さの中央に配置することができます。 コピー先の四角形には、`x` と `y` の左上隅があり、これらの値の右下隅に、ビットマップの幅と高さが拡大縮小されています。

[![一様スケーリング](displaying-images/UniformScaling.png "一様スケーリング")](displaying-images/UniformScaling-Large.png#lightbox)

その領域を拡大するビットマップを表示する電話を横向きにします。

[![一様スケーリングの横](displaying-images/UniformScaling-Landscape.png "一様スケーリングの横")](displaying-images/UniformScaling-Landscape-Large.png#lightbox)

この `scale` 要因を使用する利点は、わずかに異なるアルゴリズムを実装する場合に明らかになります。 ビットマップの縦横比が維持されますも先の四角形を入力するとします。 これが可能な唯一の方法は、イメージの一部をトリミングすることですが、上記のコードで `Math.Min` を `Math.Max` に変更するだけで、そのアルゴリズムを実装できます。 結果は次のとおりです。 

[![一様スケーリングの代替](displaying-images/UniformScaling-Alternative.png "一様スケーリングの代替")](displaying-images/UniformScaling-Alternative-Large.png#lightbox)

ビットマップの縦横比は保持されますが、ビットマップの左側の領域がトリミングされます。

## <a name="a-versatile-bitmap-display-function"></a>汎用的なビットマップの表示関数

UWP および Xamarin.Forms) などの XAML ベースのプログラミング環境では、展開したり、縦横比を維持しながら、ビットマップのサイズを縮小する機能があります。 SkiaSharp にこの機能が含まれていませんを自分でその実装できます。 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)アプリケーションに含まれている `BitmapExtensions` クラスは、その方法を示しています。 クラスは、縦横比の計算を実行する2つの新しい `DrawBitmap` メソッドを定義します。 これらの新しいメソッドは、`SKCanvas`の拡張メソッドです。

新しい `DrawBitmap` メソッドには、型 `BitmapStretch`のパラメーター、 **BitmapExtensions.cs**ファイルで定義されている列挙体が含まれています。

```csharp
public enum BitmapStretch
{
    None,
    Fill,
    Uniform,
    UniformToFill,
    AspectFit = Uniform,
    AspectFill = UniformToFill
}
```

`None`、`Fill`、`Uniform`、および `UniformToFill` のメンバーは、UWP [`Stretch`](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx)列挙体のメンバーと同じです。 同様の Xamarin Forms [`Aspect`](xref:Xamarin.Forms.Aspect)列挙体は、`Fill`、`AspectFit`、および `AspectFill`のメンバーを定義します。

上に示した**均一なスケーリング**のページでは、四角形の中にビットマップが配置されますが、四角形の左側または右側にビットマップを配置するなど、他のオプションを使用することもできます。 `BitmapAlignment` 列挙型の目的は次のとおりです。

```csharp
public enum BitmapAlignment
{
    Start,
    Center,
    End
}
```

配置設定は、`BitmapStretch.Fill`で使用した場合には効果がありません。

最初の `DrawBitmap` 拡張関数には変換先の四角形が含まれますが、変換元の四角形は含まれません。 ビットマップを中央揃えにする場合は、`BitmapStretch` のメンバーのみを指定する必要があるため、既定値が定義されています。

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect dest, 
                                  BitmapStretch stretch, 
                                  BitmapAlignment horizontal = BitmapAlignment.Center, 
                                  BitmapAlignment vertical = BitmapAlignment.Center, 
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * bitmap.Width, scale * bitmap.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, display, paint);
        }
    }
    ···
}
```

このメソッドの主な目的は、`CalculateDisplayRect` メソッドを呼び出すときにビットマップの幅と高さに適用される `scale` という名前のスケールファクターを計算することです。 これは、水平方向および垂直方向の配置に基づいて、ビットマップを表示するための四角形を計算するメソッドです。

```csharp
static class BitmapExtensions
{
    ···
    static SKRect CalculateDisplayRect(SKRect dest, float bmpWidth, float bmpHeight, 
                                       BitmapAlignment horizontal, BitmapAlignment vertical)
    {
        float x = 0;
        float y = 0;

        switch (horizontal)
        {
            case BitmapAlignment.Center:
                x = (dest.Width - bmpWidth) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                x = dest.Width - bmpWidth;
                break;
        }

        switch (vertical)
        {
            case BitmapAlignment.Center:
                y = (dest.Height - bmpHeight) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                y = dest.Height - bmpHeight;
                break;
        }

        x += dest.Left;
        y += dest.Top;

        return new SKRect(x, y, x + bmpWidth, y + bmpHeight);
    }
}
```

`BitmapExtensions` クラスには、ビットマップのサブセットを指定するためのソース四角形を持つ追加の `DrawBitmap` メソッドが含まれています。 このメソッドは最初の方法と似ていますが、スケールファクターは `source` の四角形に基づいて計算され、`CalculateDisplayRect`の呼び出しの `source` 四角形に適用される点が異なります。

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect source, SKRect dest,
                                  BitmapStretch stretch,
                                  BitmapAlignment horizontal = BitmapAlignment.Center,
                                  BitmapAlignment vertical = BitmapAlignment.Center,
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, source, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / source.Width, dest.Height / source.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / source.Width, dest.Height / source.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * source.Width, scale * source.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, source, display, paint);
        }
    }
    ···
}
```

これら2つの新しい `DrawBitmap` メソッドの最初の部分は、 **[スケーリングモード]** ページで説明されています。 XAML ファイルには、`BitmapStretch` と `BitmapAlignment` 列挙型のメンバーを選択できる3つの `Picker` 要素が含まれています。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.ScalingModesPage"
             Title="Scaling Modes">

    <Grid Padding="10">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <skia:SKCanvasView x:Name="canvasView" 
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="Stretch:"
               Grid.Row="1" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="stretchPicker"
                Grid.Row="1" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapStretch}">
                    <x:Static Member="local:BitmapStretch.None" />
                    <x:Static Member="local:BitmapStretch.Fill" />
                    <x:Static Member="local:BitmapStretch.Uniform" />
                    <x:Static Member="local:BitmapStretch.UniformToFill" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Horizontal Alignment:"
               Grid.Row="2" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="horizontalPicker" 
                Grid.Row="2" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Vertical Alignment:"
               Grid.Row="3" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="verticalPicker" 
                Grid.Row="3" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</ContentPage>
```

分離コードファイルは、`Picker` 項目が変更されたときに、`CanvasView` を無効にするだけです。 `PaintSurface` ハンドラーは、`DrawBitmap` 拡張メソッドを呼び出すために、次の3つの `Picker` ビューにアクセスします。

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public ScalingModesPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, dest, stretch, horizontal, vertical);
    }
}
```

いくつかのオプションの組み合わせを次に示します。

[![スケーリングモード](displaying-images/ScalingModes.png "スケーリングモード")](displaying-images/ScalingModes-Large.png#lightbox)

**四角形のサブセット**ページには**スケーリングモード**とほぼ同じ XAML ファイルがありますが、分離コードファイルでは、`SOURCE` フィールドによって指定されたビットマップの四角形のサブセットが定義されています。 

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");

    static readonly SKRect SOURCE = new SKRect(94, 12, 212, 118);

    public RectangleSubsetPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, SOURCE, dest, stretch, horizontal, vertical);
    }
}
```

この四角形のソースは、これらのスクリーン ショットに示すように、monkey のヘッドを分離します。

[![四角形のサブセット](displaying-images/RectangleSubset.png "四角形のサブセット")](displaying-images/RectangleSubset-Large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
