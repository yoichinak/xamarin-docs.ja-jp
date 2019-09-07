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
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759512"
---
# <a name="displaying-skiasharp-bitmaps"></a>SkiaSharp のビットマップの表示

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp のビットマップのサブジェクトが、情報の記事で導入された **[SkiaSharp のビットマップ基本](../basics/bitmaps.md)** します。 その記事では、負荷のビットマップに 3 つの方法とビットマップを表示する 3 つの方法を示しました。 この記事では、ビットマップを読み込む方法を確認しより深いがの使用になる、`DrawBitmap`メソッドの`SKCanvas`します。

![サンプルを表示する](displaying-images/DisplayingSample.png "サンプルを表示します。")

`DrawBitmapLattice`と`DrawBitmapNinePatch`メソッドが、情報の記事で説明した **[SkiaSharp ビットマップの表示をセグメント化された](segmented.md)** します。

このページのサンプルは、 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** アプリケーション。 そのアプリケーションのホーム ページから次のように選択します。 **SkiaSharp ビットマップ**、順に移動し、**を表示するビットマップ**セクション。

## <a name="loading-a-bitmap"></a>ビットマップの読み込み

SkiaSharp のアプリケーションで一般的に使用されるビットマップでは、次の 3 つの異なるソースのいずれかのです。

- インターネットを介した
- 実行可能ファイルに埋め込まれたリソースから
- ユーザーのフォト ライブラリから

SkiaSharp アプリケーションで、新しいビットマップを作成しとに描画またはアルゴリズム ビットマップのビットを設定することもできます。 これらの方法については、記事で説明 **[SkiaSharp ビットマップの描画の作成と](drawing.md)** と **[SkiaSharp ビットマップのピクセルへのアクセス](pixel-bits.md)** .

ビットマップの読み込みの次の 3 つのコード例では、クラスは型のフィールドを格納するものと`SKBitmap`:

```csharp
SKBitmap bitmap;
```

アーティクルとして **[SkiaSharp のビットマップ基本](../basics/bitmaps.md)** 言えば、最善の方法をインターネット経由でビットマップを読み込むとでは、 [`HttpClient`](xref:System.Net.Http.HttpClient)クラス。 クラスの 1 つのインスタンスは、フィールドとして定義できます。

```csharp
HttpClient httpClient = new HttpClient();
```

使用する場合`HttpClient`iOS と Android アプリケーションでは、に関するドキュメントで説明したように、プロジェクトのプロパティを設定する必要あります **[トランスポート層セキュリティ (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)** します。

使用するコード`HttpClient`多くの場合、`await`に存在する必要があるため、演算子、`async`メソッド。

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

注意、`Stream`オブジェクトから取得`GetStreamAsync`にコピーされます、`MemoryStream`します。 Android ではできません、`Stream`から`HttpClient`を非同期メソッドでは、メイン スレッド以外で処理できます。 

で[`SKBitmap.Decode`](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream))は、さまざまな作業が行われます。渡さ`Stream`れたオブジェクトは、一般的なビットマップファイル形式 (通常は JPEG、PNG、GIF) のいずれかでビットマップ全体を含むメモリブロックを参照します。 `Decode`メソッドが、形式が決定し、SkiaSharp の内部ビットマップ形式にビットマップ ファイルをデコードする必要があります。

コードの呼び出し後`SKBitmap.Decode`、おそらくが無効になります、`CanvasView`ように、`PaintSurface`ハンドラーが新しく読み込まれたビットマップを表示できます。

ビットマップを読み込む 2 つ目の方法は、.NET Standard ライブラリ内の埋め込みリソースとして、ビットマップを含めることによってによって参照される個別のプラットフォーム プロジェクト。 渡された ID、リソース、 [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String))メソッド。 このリソース ID は、アセンブリ名、フォルダー名、およびピリオドで区切られたリソースのファイル名で構成されます。

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

ビットマップを取得するための 3 番目のアプローチは、ユーザーの画像ライブラリです。 次のコードに含まれている依存関係サービスを使用して、 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** アプリケーション。 **SkiaSharpFormsDemo** .NET 標準ライブラリが含まれています、`IPhotoLibrary`の各プラットフォーム プロジェクトに含まれていますが、インターフェイス、`PhotoLibrary`そのインターフェイスを実装するクラス。

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

一般に、このようなコードも無効になります、`CanvasView`ように、`PaintSurface`ハンドラーは、新しいビットマップを表示できます。

`SKBitmap`クラスなど、いくつかの便利なプロパティを定義します[ `Width` ](xref:SkiaSharp.SKBitmap.Width)と[ `Height`](xref:SkiaSharp.SKBitmap.Height)を含む、多くのメソッドと同様に、ビットマップのピクセル寸法を表示します。ビットマップ、ピクセル ビットを公開して、それらをコピーを作成するメソッド。 

## <a name="displaying-in-pixel-dimensions"></a>ピクセル数で表示します。

SkiaSharp、 [ `Canvas` ](xref:SkiaSharp.SKCanvas)クラスは、4 つ定義`DrawBitmap`メソッド。 これらのメソッドは、根本的に異なる 2 つの方法で表示されるビットマップを許可します。 

- 指定する、`SKPoint`値 (または個別`x`と`y`値) のピクセル寸法のビットマップを表示します。 ビットマップのピクセルは、ビデオ ディスプレイのピクセルに直接マップされます。
- 四角形を指定すると、四角形の形状とサイズに拡大するビットマップとします。 

使用してそのピクセル寸法のビットマップを表示する[ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKPoint,SkiaSharp.SKPaint))で、`SKPoint`パラメーターまたは[ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint))で`x`と`y`パラメーター。

```csharp
DrawBitmap(SKBitmap bitmap, SKPoint pt, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, float x, float y, SKPaint paint = null)
```

これら 2 つの方法では、機能的に同じです。 指定したポイントでは、ビットマップをキャンバスの左上隅の場所を示します。 モバイル デバイスのピクセルの解像度は非常に高いため、小さいビットマップは通常、これらのデバイスでは非常に狭かった表示されます。

省略可能な`SKPaint`パラメーターでは、透明度を使用してビットマップを表示することができます。 これを行うには、作成、`SKPaint`オブジェクトし、設定、`Color`プロパティにいずれか`SKColor`値、アルファ チャネルの 1 より小さい。 例:

```csharp
paint.Color = new SKColor(0, 0, 0, 0x80);
```

最後の引数として渡される 0x80 では、透明度が 50% を示します。 定義済みの色のいずれかでアルファ チャネルを設定することもできます。

```csharp
paint.Color = SKColors.Red.WithAlpha(0x80);
```

ただし、色自体では、関係ありません。 使用するときに、アルファ チャネルだけが調べられる、`SKPaint`オブジェクト、`DrawBitmap`呼び出します。

`SKPaint`オブジェクトは、表示するビットマップを使用して、blend のモードまたは効果をフィルター処理とも、役割を果たします。 これらの記事で説明されています[SkiaSharp 合成と blend モード](../effects/blend-modes/index.md)と[SkiaSharp イメージ フィルター](../effects/image-filters.md)します。

**ピクセル寸法**ページで、 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** サンプル プログラムは、320 ピクセル 240 ピクセル、高さのビットマップ リソースを表示します。

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

`PaintSurface`ハンドラー、ビットマップを計算することで、中央`x`と`y`表示サーフェイスのピクセル寸法とビットマップのピクセルのサイズに基づいて、値。

[![ピクセル寸法](displaying-images/PixelDimensions.png "ピクセル寸法")](displaying-images/PixelDimensions-Large.png#lightbox)

座標を単に渡す場合、アプリケーションの左上隅にあるビットマップを表示する場合、(0, 0)。 

## <a name="a-method-for-loading-resource-bitmaps"></a>リソース ビットマップを読み込むためのメソッド

次のサンプルの多くは、ビットマップ リソースを読み込む必要があります。 静的な`BitmapExtensions`クラス、 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** ソリューションに役立てるためにメソッドが含まれています。

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

通知、`Type`パラメーター。 これは、`Type`ビットマップ リソースを格納するアセンブリ内の任意の型に関連付けられているオブジェクト。

これは、`LoadBitmapResource`メソッドはビットマップ リソースを必要とする後続のすべてのサンプルで使用されます。

## <a name="stretching-to-fill-a-rectangle"></a>四角形の塗りつぶしへの拡張

`SKCanvas`クラスも定義、 [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint))別の四角形をビットマップをレンダリングするメソッド[ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint))するビットマップの四角形のサブセットを表示するメソッド、四角形。

```
DrawBitmap(SKBitmap bitmap, SKRect dest, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, SKRect source, SKRect dest, SKPaint paint = null)
```

ビットマップを拡大するという名前の四角形の塗りつぶし両方の場合、`dest`します。 2 番目のメソッドで、`source`四角形を使用すると、ビットマップのサブセットを選択します。 `dest`四角形を出力デバイス; の基準としたが、`source`ビットマップに対する相対パスの四角形です。

**四角形の塗りつぶし**ページは、同じビットマップを表示することによってこれら 2 つのメソッドの最初の四角形内の前の例では同じサイズとして使用、キャンバスを示します。 

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

新しい使用`BitmapExtensions.LoadBitmapResource`を設定するメソッド、`SKBitmap`フィールド。 先の四角形がから取得した、 [ `Rect` ](xref:SkiaSharp.SKImageInfo.Rect)プロパティの`SKImageInfo`を画面のサイズについて説明します。

[![四角形の塗りつぶし](displaying-images/FillRectangle.png "四角形の塗りつぶし")](displaying-images/FillRectangle-Large.png#lightbox)

これは、通常_いない_対象します。 水平および垂直方向に異なる方法で拡張されたによってイメージがゆがめられます。 ピクセル サイズ以外の方法で、ビットマップを表示するときに通常するビットマップの元の縦横比を保持します。

## <a name="stretching-while-preserving-the-aspect-ratio"></a>縦横比を維持しながら拡張

プロセスとも呼ばれますが、縦横比を維持しながら、ビットマップを拡大_統一されたスケーリング_します。 この用語は、アルゴリズムのアプローチを提案します。 ソリューションの 1 つを示した、**統一されたスケーリング**ページ。

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

`PaintSurface`ハンドラーを計算する`scale`表示幅と高さのビットマップの幅と高さの比率の最小の要素。 `x`と`y`表示幅と高さにスケーリングされたビットマップを中央揃えの値を計算することができます。 先の四角形が、左上隅の`x`と`y`し、それらの値とスケールの幅の右下隅とビットマップの高さ。

[![統一されたスケーリング](displaying-images/UniformScaling.png "統一されたスケーリング")](displaying-images/UniformScaling-Large.png#lightbox)

その領域を拡大するビットマップを表示する電話を横向きにします。

[![スケーリングのランドス ケープを uniform](displaying-images/UniformScaling-Landscape.png "ランドス ケープの統一されたスケーリング")](displaying-images/UniformScaling-Landscape-Large.png#lightbox)

これを使用する利点`scale`若干異なるアルゴリズムを実装する場合に要素が明らかになります。 ビットマップの縦横比が維持されますも先の四角形を入力するとします。 これは、可能な唯一の方法は、イメージの一部をトリミングしてが、そのアルゴリズムを実装するには変更するだけで`Math.Min`に`Math.Max`上記のコード。 結果を次に示します。 

[![代わりに統一されたスケーリング](displaying-images/UniformScaling-Alternative.png "代わりに統一されたスケーリング")](displaying-images/UniformScaling-Alternative-Large.png#lightbox)

ビットマップの縦横比は保持されますが、ビットマップの左側の領域がトリミングされます。

## <a name="a-versatile-bitmap-display-function"></a>汎用的なビットマップの表示関数

UWP および Xamarin.Forms) などの XAML ベースのプログラミング環境では、展開したり、縦横比を維持しながら、ビットマップのサイズを縮小する機能があります。 SkiaSharp にこの機能が含まれていませんを自分でその実装できます。 `BitmapExtensions`クラスに含まれる、 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)アプリケーションの表示方法。 このクラスを定義する 2 つの新しい`DrawBitmap`縦横比の計算を実行するメソッド。 これらの新しいメソッドが拡張メソッドの`SKCanvas`します。

新しい`DrawBitmap`メソッドは、型のパラメーターを含める`BitmapStretch`、列挙体で定義されている、 **BitmapExtensions.cs**ファイル。

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

`None`、 `Fill`、 `Uniform`、および`UniformToFill`メンバーは、UWP 内のものと同じ[ `Stretch` ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx)列挙体。 ような Xamarin.Forms [ `Aspect` ](xref:Xamarin.Forms.Aspect)列挙型メンバーを定義する`Fill`、 `AspectFit`、および`AspectFill`します。

**統一されたスケーリング**上に示すページ中央に四角形内でビットマップが左または四角形、または上部または下部の右側にあるビットマップの配置など、他のオプションをする可能性があります。 目的は、`BitmapAlignment`列挙体。

```csharp
public enum BitmapAlignment
{
    Start,
    Center,
    End
}
```

配置の設定がある影響しないと併用すると`BitmapStretch.Fill`します。

最初の`DrawBitmap`拡張関数には、先の四角形がないソースの四角形が含まれています。 既定値が定義され、ビットマップの中央に配置する場合を指定するだけ、`BitmapStretch`メンバー。

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

このメソッドの主な目的は、という名前のスケール ファクターを計算することです。`scale`を呼び出すときに、ビットマップの幅と高さに適用されるし、`CalculateDisplayRect`メソッド。 これは、水平方向および垂直方向の配置に基づいて、ビットマップを表示するための四角形を計算するメソッドです。

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

`BitmapExtensions`クラスが含まれていますが、追加`DrawBitmap`ソース四角形を持つビットマップのサブセットを指定するためのメソッド。 スケール ファクターが基に計算する点を除いて、このメソッドは 1 つ目のような`source`四角形に適用し、`source`への呼び出し内の四角形`CalculateDisplayRect`:

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

これら 2 つの新しいの最初の`DrawBitmap`メソッドの説明については、**スケーリング モード**ページ。 XAML ファイルには、3 つが含まれている`Picker`できる要素のメンバーの選択、`BitmapStretch`と`BitmapAlignment`列挙体。

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

分離コード ファイルを単に無効化、`CanvasView`いずれかが`Picker`項目が変更されました。 `PaintSurface`ハンドラーにアクセスする、3 つ`Picker`呼び出し元のビュー、`DrawBitmap`拡張メソッド。

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

[![スケーリング モード](displaying-images/ScalingModes.png "スケーリング モード")](displaying-images/ScalingModes-Large.png#lightbox)

**四角形のサブセット**ページと同じ XAML ファイルには事実上**スケーリング モード**、分離コード ファイルで指定されたビットマップの四角形のサブセットを定義しますが、`SOURCE`フィールド。 

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

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
