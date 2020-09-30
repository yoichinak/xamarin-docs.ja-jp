---
title: 表示 (SkiaSharp ビットマップを)
description: 縦横比を維持したまま、ピクセルサイズで SkiaSharp ビットマップを表示し、四角形を塗りつぶすために展開する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8E074F8D-4715-4146-8CC0-FD7A8290EDE9
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1427b6f8461c74ded933fe562a7d17221790383a
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562315"
---
# <a name="displaying-skiasharp-bitmaps"></a>表示 (SkiaSharp ビットマップを)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp ビットマップの件名は、 **[SkiaSharp の「ビットマップの基礎](../basics/bitmaps.md)**」で導入されました。 この記事では、ビットマップを読み込む3つの方法と、ビットマップを表示する3つの方法について説明しました。 この記事では、ビットマップを読み込み、のメソッドの使用方法について詳しく説明し `DrawBitmap` `SKCanvas` ます。

![サンプルの表示](displaying-images/DisplayingSample.png "サンプルの表示")

`DrawBitmapLattice`メソッドと `DrawBitmapNinePatch` メソッドについては、 **[SkiaSharp ビットマップのセグメント](segmented.md)** 化された表示に関する記事で説明されています。

このページのサンプルは、 **[SkiaSharpFormsDemos](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** アプリケーションからのものです。 そのアプリケーションのホームページで、[ **SkiaSharp ビットマップ**] を選択し、[ **ビットマップの表示** ] セクションにアクセスします。

## <a name="loading-a-bitmap"></a>ビットマップの読み込み

SkiaSharp アプリケーションによって使用されるビットマップは、通常、次の3種類のソースのいずれかから取得されます。

- インターネット経由
- 実行可能ファイルに埋め込まれたリソースから
- ユーザーのフォトライブラリから

また、SkiaSharp アプリケーションで新しいビットマップを作成し、それを描画したり、ビットマップビットアルゴリズムを設定したりすることもできます。 これらの手法については、 **[SkiaSharp ビットマップの作成と描画](drawing.md)** 、および **[SkiaSharp ビットマップピクセルへのアクセス](pixel-bits.md)** に関する記事で説明されています。

次の3つのコード例では、ビットマップを読み込みます。クラスには、型のフィールドが含まれていると見なされ `SKBitmap` ます。

```csharp
SKBitmap bitmap;
```

**[SkiaSharp の記事ビットマップの基礎](../basics/bitmaps.md)** として、インターネット経由でビットマップを読み込む最適な方法は、クラスを使用することです [`HttpClient`](xref:System.Net.Http.HttpClient) 。 クラスの1つのインスタンスをフィールドとして定義できます。

```csharp
HttpClient httpClient = new HttpClient();
```

IOS および Android アプリケーションでを使用する場合は、 `HttpClient` **[トランスポート層セキュリティ (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)** のドキュメントで説明されているように、プロジェクトのプロパティを設定する必要があります。

を使用するコードは、 `HttpClient` 多くの場合、 `await` 演算子を必要とするため、メソッドに存在する必要があり `async` ます。

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

`Stream`から取得したオブジェクトがにコピーされていることに注意 `GetStreamAsync` `MemoryStream` してください。 Android では、 `Stream` `HttpClient` 非同期メソッドを除き、メインスレッドでのの処理が許可されていません。 

は [`SKBitmap.Decode`](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream)) 多くの作業を行います。 `Stream` これに渡されるオブジェクトは、一般的なビットマップファイル形式 (通常は JPEG、PNG、GIF) のいずれかでビットマップ全体を含むメモリブロックを参照します。 メソッドは、 `Decode` 形式を決定し、ビットマップファイルを SkiaSharp の内部ビットマップ形式にデコードする必要があります。

コードを呼び出すと、 `SKBitmap.Decode` `CanvasView` ハンドラーが新しく読み込まれたビットマップを表示できるように、が無効になる可能性があり `PaintSurface` ます。

ビットマップを読み込む2番目の方法は、個別のプラットフォームプロジェクトによって参照される .NET Standard ライブラリに、埋め込みリソースとしてビットマップを含めることです。 リソース ID がメソッドに渡され [`GetManifestResourceStream`](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) ます。 このリソース ID は、ピリオドで区切られたリソースのアセンブリ名、フォルダー名、およびファイル名で構成されます。

```csharp
string resourceID = "assemblyName.folderName.fileName";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    bitmap = SKBitmap.Decode(stream);
    ···
}
```

ビットマップファイルは、iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) の個別のプラットフォームプロジェクトにリソースとして保存することもできます。 ただし、これらのビットマップを読み込むには、プラットフォームプロジェクトに配置されているコードが必要です。

ビットマップを取得するための3番目の方法は、ユーザーのピクチャライブラリからです。 次のコードでは、 **[SkiaSharpFormsDemos](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** アプリケーションに含まれている依存関係サービスを使用しています。 **SkiaSharpFormsDemo** .NET Standard ライブラリにはインターフェイスが含まれていますが、 `IPhotoLibrary` 各プラットフォームプロジェクトには、そのインターフェイスを実装するクラスが含まれてい `PhotoLibrary` ます。

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

通常、このようなコードは、 `CanvasView` ハンドラーが新しいビットマップを表示できるように、も無効にし `PaintSurface` ます。

クラスは、 `SKBitmap` ビットマップのピクセルの大きさを示すやなど、いくつかの便利なプロパティを定義し [`Width`](xref:SkiaSharp.SKBitmap.Width) ます。また、ビットマップを作成したり、ビットマップをコピーしたり、 [`Height`](xref:SkiaSharp.SKBitmap.Height) ピクセルビットを公開したりするメソッドなど、多くのメソッドを定義します。 

## <a name="displaying-in-pixel-dimensions"></a>表示 (ピクセルディメンションで)

SkiaSharp クラスは、 [`Canvas`](xref:SkiaSharp.SKCanvas) 4 つの `DrawBitmap` メソッドを定義します。 これらのメソッドを使用すると、次の2つの基本的な方法でビットマップを表示できます。 

- `SKPoint`値 (または個別の値) を指定する `x` と `y` 、ビットマップがピクセルディメンションで表示されます。 ビットマップのピクセルは、ビデオディスプレイのピクセルに直接マップされます。
- 四角形を指定すると、ビットマップが四角形のサイズと形状に拡大されます。 

ピクセルディメンションにビットマップを表示するには、パラメーターを指定する [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKPoint,SkiaSharp.SKPaint)) `SKPoint` か [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) 、およびパラメーターを個別に使用し `x` `y` ます。

```csharp
DrawBitmap(SKBitmap bitmap, SKPoint pt, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, float x, float y, SKPaint paint = null)
```

これらの2つの方法は機能的には同じです。 指定したポイントは、キャンバスを基準とするビットマップの左上隅の位置を示します。 モバイルデバイスのピクセル解像度は非常に高いため、これらのデバイスでは通常、小さいビットマップが非常に小さく表示されます。

省略可能な `SKPaint` パラメーターを使用すると、透明度を使用してビットマップを表示できます。 これを行うには、オブジェクトを作成し、プロパティを、 `SKPaint` `Color` アルファチャネルが1未満の任意の値に設定し `SKColor` ます。 次に例を示します。

```csharp
paint.Color = new SKColor(0, 0, 0, 0x80);
```

最後の引数として渡された0x80 は、50% の透明度を示します。 定義済みの色のいずれかでアルファチャネルを設定することもできます。

```csharp
paint.Color = SKColors.Red.WithAlpha(0x80);
```

ただし、色自体は無関係です。 呼び出しでオブジェクトを使用すると、アルファチャネルのみが検査され `SKPaint` `DrawBitmap` ます。

また、オブジェクトは、 `SKPaint` blend モードまたはフィルター効果を使用してビットマップを表示するときにも役割を果たします。 これらの情報については、「 [SkiaSharp 合成」と「blend モード](../effects/blend-modes/index.md) と [SkiaSharp イメージフィルター](../effects/image-filters.md)」の記事で説明されています。

**[SkiaSharpFormsDemos](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** サンプルプログラムの [**ピクセルディメンション**] ページには、320ピクセル、高さが240ピクセルのビットマップリソースが表示されます。

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

この `PaintSurface` ハンドラーは、 `x` `y` ディスプレイ画面のピクセル寸法とビットマップのピクセルサイズに基づいて、との値を計算することによってビットマップを中央揃えにします。

[![ピクセルディメンション](displaying-images/PixelDimensions.png "ピクセルディメンション")](displaying-images/PixelDimensions-Large.png#lightbox)

アプリケーションで左上隅にビットマップを表示する場合は、単に (0, 0) の座標を渡します。 

## <a name="a-method-for-loading-resource-bitmaps"></a>リソースビットマップを読み込む方法

多くのサンプルでは、ビットマップリソースを読み込む必要があります。 SkiaSharpFormsDemos ソリューションの静的クラスには、 `BitmapExtensions` 次のことを支援するメソッドが含まれています。 **[SkiaSharpFormsDemos](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)**

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

パラメーターに注意 `Type` してください。 これは、 `Type` ビットマップリソースを格納するアセンブリ内の任意の型に関連付けられたオブジェクトにすることができます。

この `LoadBitmapResource` メソッドは、ビットマップリソースを必要とする後続のすべてのサンプルで使用されます。

## <a name="stretching-to-fill-a-rectangle"></a>拡大して四角形を塗りつぶす

`SKCanvas`また、クラスは、 [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) ビットマップを四角形にレンダリングするメソッドと、 [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) ビットマップの四角形のサブセットを四角形にレンダリングする別のメソッドも定義します。

```
DrawBitmap(SKBitmap bitmap, SKRect dest, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, SKRect source, SKRect dest, SKPaint paint = null)
```

どちらの場合も、ビットマップは、という名前の四角形を埋めるように拡大され `dest` ます。 2番目の方法では、四角形を使用して `source` ビットマップのサブセットを選択できます。 四角形は、出力デバイスを基準にしています `dest` 。四角形は、ビットマップを基準としてい `source` ます。

[ **四角形の塗りつぶし** ] ページでは、前の例で使用したものと同じビットマップを四角形に表示することで、最初の2つの方法を示しています。 

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

新しいメソッドを使用してフィールドを設定することに注意して `BitmapExtensions.LoadBitmapResource` `SKBitmap` ください。 コピー先の四角形は、のプロパティから取得され [`Rect`](xref:SkiaSharp.SKImageInfo.Rect) `SKImageInfo` ます。これにより、ディスプレイ画面のサイズが決まります。

[![塗りつぶし四角形](displaying-images/FillRectangle.png "塗りつぶし四角形")](displaying-images/FillRectangle-Large.png#lightbox)

通常、この方法は必要 _ありません_ 。 画像は、水平方向と垂直方向で異なる方法で拡大されているため、歪んでいます。 ピクセルサイズ以外でビットマップを表示する場合は、通常、ビットマップの元の縦横比を維持する必要があります。

## <a name="stretching-while-preserving-the-aspect-ratio"></a>縦横比を維持しながら拡大する

縦横比を維持したままビットマップを拡大するプロセスは、 _一様なスケーリング_とも呼ばれます。 この用語は、アルゴリズムのアプローチを提案します。 1つの考えられる解決方法を、 **一様なスケーリング** のページに示します。

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

この `PaintSurface` ハンドラーは、 `scale` 表示幅と高さの最小値をビットマップの幅と高さに対する最小値とする係数を計算します。 次に、との値を計算して、 `x` `y` 拡大縮小されたビットマップを表示幅と高さの中央に配置することができます。 コピー先の四角形には、との左上 `x` 隅 `y` と、これらの値の右下隅に、ビットマップの幅と高さを拡大した値が含まれます。

[![一様スケーリング](displaying-images/UniformScaling.png "一様スケーリング")](displaying-images/UniformScaling-Large.png#lightbox)

スマートフォンを横向きにして、ビットマップがその領域に拡大されていることを確認します。

[![一様スケーリングの横](displaying-images/UniformScaling-Landscape.png "一様スケーリングの横")](displaying-images/UniformScaling-Landscape-Large.png#lightbox)

この要因を使用する利点は、 `scale` 若干異なるアルゴリズムを実装する場合に明らかになります。 ビットマップの縦横比を維持しながら、コピー先の四角形も塗りつぶす場合を考えてみます。 これが可能な唯一の方法は、イメージの一部をトリミングすることですが、上記のコードのに変更するだけで、そのアルゴリズムを実装できます `Math.Min` `Math.Max` 。 結果は次のとおりです。 

[![一様スケーリングの代替](displaying-images/UniformScaling-Alternative.png "一様スケーリングの代替")](displaying-images/UniformScaling-Alternative-Large.png#lightbox)

ビットマップの縦横比は維持されますが、ビットマップの左右の領域はトリミングされます。

## <a name="a-versatile-bitmap-display-function"></a>汎用性のあるビットマップ表示関数

XAML ベースのプログラミング環境 (UWP やなど Xamarin.Forms ) には、縦横比を維持したままビットマップのサイズを拡大または縮小する機能があります。 SkiaSharp にはこの機能が含まれていませんが、独自に実装することができます。 `BitmapExtensions` [**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)アプリケーションに含まれるクラスは、その方法を示しています。 クラスは、 `DrawBitmap` 縦横比の計算を実行する2つの新しいメソッドを定義します。 これらの新しいメソッドは、の拡張メソッドです `SKCanvas` 。

新しいメソッドには、 `DrawBitmap` `BitmapStretch` **BitmapExtensions.cs** ファイルで定義されている列挙型のパラメーターが含まれています。

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

`None`、、 `Fill` `Uniform` 、およびの `UniformToFill` 各メンバーは、UWP 列挙体のメンバーと同じ [`Stretch`](/uwp/api/Windows.UI.Xaml.Media.Stretch) です。 同様の Xamarin.Forms [`Aspect`](xref:Xamarin.Forms.Aspect) 列挙体は、メンバー `Fill` 、、およびを定義し `AspectFit` `AspectFill` ます。

上に示した **均一なスケーリング** のページでは、四角形の中にビットマップが配置されますが、四角形の左側または右側にビットマップを配置するなど、他のオプションを使用することもできます。 列挙型の目的は `BitmapAlignment` 次のとおりです。

```csharp
public enum BitmapAlignment
{
    Start,
    Center,
    End
}
```

配置設定は、で使用しても効果がありません `BitmapStretch.Fill` 。

最初の `DrawBitmap` 拡張関数には変換先の四角形が含まれますが、変換元の四角形は含まれません。 ビットマップを中央揃えにする場合は、メンバーを指定するだけで済み `BitmapStretch` ます。

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

このメソッドの主な目的は、 `scale` メソッドの呼び出し時にビットマップの幅と高さに適用されるという名前のスケールファクターを計算することです `CalculateDisplayRect` 。 次に示すのは、水平方向と垂直方向の配置に基づいてビットマップを表示する四角形を計算するメソッドです。

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

クラスには、 `BitmapExtensions` `DrawBitmap` ビットマップのサブセットを指定するためのソース四角形を含む追加のメソッドが含まれています。 このメソッドは、最初の方法と似ていますが、スケールファクターが四角形に基づいて計算され、の `source` 呼び出しの四角形に適用される点が異なり `source` `CalculateDisplayRect` ます。

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

これら2つの新しいメソッドの最初の部分 `DrawBitmap` は、[ **スケーリングモード** ] ページに示されています。 XAML ファイルに `Picker` は、および列挙型のメンバーを選択できる3つの要素が含まれてい `BitmapStretch` `BitmapAlignment` ます。

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

分離コードファイルは、 `CanvasView` `Picker` 項目が変更されたときにを無効にするだけです。 この `PaintSurface` ハンドラーは、 `Picker` 拡張メソッドを呼び出すための3つのビューにアクセスし `DrawBitmap` ます。

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

オプションの組み合わせを次に示します。

[![スケーリングモード](displaying-images/ScalingModes.png "スケーリングモード")](displaying-images/ScalingModes-Large.png#lightbox)

**四角形のサブセット**ページには**スケーリングモード**とほぼ同じ XAML ファイルがありますが、分離コードファイルでは、フィールドによって指定されたビットマップの四角形のサブセットが定義されてい `SOURCE` ます。 

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

この四角形のソースは、次のスクリーンショットに示すように、サルの頭を分離します。

[![四角形のサブセット](displaying-images/RectangleSubset.png "四角形のサブセット")](displaying-images/RectangleSubset-Large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)