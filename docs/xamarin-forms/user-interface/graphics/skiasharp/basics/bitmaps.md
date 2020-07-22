---
title: SkiaSharp でのビットマップの基礎
description: この記事では、さまざまなソースから SkiaSharp のビットマップを読み込んでアプリケーションに表示する方法について説明 Xamarin.Forms し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1e4c170f818dc62640b1cd72ec3b70f48d227d93
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137737"
---
# <a name="bitmap-basics-in-skiasharp"></a>SkiaSharp でのビットマップの基礎

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_さまざまなソースからビットマップを読み込んで表示します。_

SkiaSharp でのビットマップのサポートは非常に広範囲です。 この記事では、 &mdash; ビットマップの読み込み方法とその表示方法について説明します。

![](bitmaps-images/basicbitmaps-small.png "The display of two bitmaps")

ビットマップの詳細な探索については、「 [SkiaSharp ビットマップ](../bitmaps/index.md)」セクションを参照してください。

SkiaSharp bitmap は、型のオブジェクトです [`SKBitmap`](xref:SkiaSharp.SKBitmap) 。 ビットマップを作成するにはさまざまな方法がありますが、この記事では、 [`SKBitmap.Decode`](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream)) .net オブジェクトからビットマップを読み込むメソッドに限定してい `Stream` ます。

**SkiaSharpFormsDemos**プログラムの**基本的なビットマップ**ページは、3つの異なるソースからビットマップを読み込む方法を示しています。

- インターネット経由
- 実行可能ファイルに埋め込まれたリソースから
- ユーザーのフォトライブラリから

`SKBitmap`これら3つのソースの3つのオブジェクトは、クラスのフィールドとして定義されてい [`BasicBitmapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) ます。

```csharp
public class BasicBitmapsPage : ContentPage
{
    SKCanvasView canvasView;
    SKBitmap webBitmap;
    SKBitmap resourceBitmap;
    SKBitmap libraryBitmap;

    public BasicBitmapsPage()
    {
        Title = "Basic Bitmaps";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ...
    }
    ...
}
```

## <a name="loading-a-bitmap-from-the-web"></a>Web からビットマップを読み込んでいます

URL に基づいてビットマップを読み込むには、クラスを使用し [`HttpClient`](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) ます。 インスタンスを1つだけインスタンス化して再利用する必要がある `HttpClient` ため、フィールドとして格納します。

```csharp
HttpClient httpClient = new HttpClient();
```

IOS および Android アプリケーションでを使用する場合は、 `HttpClient` **[トランスポート層セキュリティ (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)** のドキュメントで説明されているように、プロジェクトのプロパティを設定する必要があります。

で演算子を使用すると最も便利 `await` なので `HttpClient` 、コンストラクターでコードを実行することはできません `BasicBitmapsPage` 。 代わりに、オーバーライドの一部です `OnAppearing` 。 ここでの URL は、いくつかのサンプルビットマップを含む Xamarin web サイトの領域を指しています。 Web サイトのパッケージでは、ビットマップのサイズを特定の幅に変更するための仕様を追加できます。

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    // Load web bitmap.
    string url = "https://developer.xamarin.com/demo/IMG_3256.JPG?width=480";

    try
    {
        using (Stream stream = await httpClient.GetStreamAsync(url))
        using (MemoryStream memStream = new MemoryStream())
        {
            await stream.CopyToAsync(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            webBitmap = SKBitmap.Decode(memStream);
            canvasView.InvalidateSurface();
        };
    }
    catch
    {
    }
}
```

Android オペレーティングシステムは、 `Stream` `GetStreamAsync` メインスレッドで時間のかかる操作を実行しているため、メソッドでから返されたを使用するときに例外を発生させ `SKBitmap.Decode` ます。 このため、ビットマップファイルの内容は、 `MemoryStream` を使用してオブジェクトにコピーされ `CopyToAsync` ます。

静的 `SKBitmap.Decode` メソッドは、ビットマップファイルのデコードを行います。 JPEG、PNG、および GIF ビットマップ形式で動作し、結果を内部 SkiaSharp 形式で格納します。 この時点で、 `SKCanvasView` ハンドラーがディスプレイを更新できるようにするには、を無効にする必要があり `PaintSurface` ます。

## <a name="loading-a-bitmap-resource"></a>ビットマップリソースを読み込んでいます

コードに関して、ビットマップを読み込む最も簡単な方法は、ビットマップリソースをアプリケーションに直接含めることです。 **SkiaSharpFormsDemos**プログラムには、 **monkey.png**という名前のビットマップファイルを含む、**メディア**という名前のフォルダーが含まれています。 プログラムリソースとして格納されているビットマップの場合、[**プロパティ**] ダイアログボックスを使用して、**埋め込みリソース**の**ビルドアクション**をファイルに指定する必要があります。

各埋め込みリソースには、プロジェクト名、フォルダー、およびファイル名で構成される*リソース ID*が含まれています。これらはすべて、ピリオド: **SkiaSharpFormsDemos.Media.monkey.png**で結ばれています。 このリソースにアクセスするには、そのリソース ID をクラスのメソッドの引数として指定し [`GetManifestResourceStream`](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) [`Assembly`](xref:System.Reflection.Assembly) ます。

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    resourceBitmap = SKBitmap.Decode(stream);
}
```

この `Stream` オブジェクトは、メソッドに直接渡すことができ `SKBitmap.Decode` ます。

## <a name="loading-a-bitmap-from-the-photo-library"></a>フォトライブラリからビットマップを読み込む

ユーザーは、デバイスの画像ライブラリから写真を読み込むこともできます。 この機能は、単独では提供されません Xamarin.Forms 。 このジョブには、「[画像ライブラリから写真を選択](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)する」で説明されているような依存関係サービスが必要です。

**SkiaSharpFormsDemos**プロジェクトの**IPhotoLibrary.cs**ファイルと、プラットフォームプロジェクト内の3つの**PhotoLibrary.cs**ファイルは、その記事から応用されています。 さらに、この記事で説明されているように Android **MainActivity.cs**ファイルが変更されています。 iOS プロジェクトには、**情報 plist**ファイルの下部に2行の写真ライブラリへのアクセス許可が付与されています。

コンストラクターは、を `BasicBitmapsPage` に追加し `TapGestureRecognizer` て、 `SKCanvasView` タップが通知されるようにします。 タップを受け取ると、ハンドラーは `Tapped` picture picker dependency service にアクセスしてを呼び出し `PickPhotoAsync` ます。 `Stream`オブジェクトが返された場合は、メソッドに渡され `SKBitmap.Decode` ます。

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();

    using (Stream stream = await photoLibrary.PickPhotoAsync())
    {
        if (stream != null)
        {
            libraryBitmap = SKBitmap.Decode(stream);
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

`Tapped`ハンドラーもオブジェクトのメソッドを呼び出すことに注意して `InvalidateSurface` `SKCanvasView` ください。 これにより、ハンドラーへの新しい呼び出しが生成さ `PaintSurface` れます。

## <a name="displaying-the-bitmaps"></a>ビットマップを表示する

このハンドラーでは、 `PaintSurface` 3 つのビットマップを表示する必要があります。 このハンドラーは、電話が縦モードであることを前提としており、キャンバスを垂直方向に3つの等しい部分に分割します。

最初のビットマップが最も簡単な方法で表示され [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) ます。 指定する必要があるのは、ビットマップの左上隅が配置される X 座標と Y 座標です。

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

`SKPaint`パラメーターは定義されていますが、既定値はであり、無視して `null` かまいません。 ビットマップのピクセルは、単純に1対1のマッピングで表示サーフェイスのピクセルに転送されます。 この引数のアプリケーションは、 `SKPaint` [**SkiaSharp の透過性**](transparency.md)に関する次のセクションで確認できます。

プログラムは、およびプロパティを使用してビットマップのピクセルディメンションを取得でき [`Width`](xref:SkiaSharp.SKBitmap.Width) [`Height`](xref:SkiaSharp.SKBitmap.Height) ます。 これらのプロパティを使用すると、プログラムは、キャンバスの左上の中央にビットマップを配置する座標を計算できます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    if (webBitmap != null)
    {
        float x = (info.Width - webBitmap.Width) / 2;
        float y = (info.Height / 3 - webBitmap.Height) / 2;
        canvas.DrawBitmap(webBitmap, x, y);
    }
    ...
}
```

他の2つのビットマップは、のバージョンのと共にパラメーターと共に表示され [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) `SKRect` ます。

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

3番目のバージョンのに [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) は、 `SKRect` 表示するビットマップの四角形のサブセットを指定するための2つの引数がありますが、そのバージョンはこの記事では使用されていません。

埋め込みリソースビットマップから読み込まれたビットマップを表示するコードを次に示します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (resourceBitmap != null)
    {
        canvas.DrawBitmap(resourceBitmap,
            new SKRect(0, info.Height / 3, info.Width, 2 * info.Height / 3));
    }
    ...
}
```

ビットマップは、四角形の大きさに拡大されます。そのため、次のスクリーンショットでは、サルが水平方向に拡大されています。

[![](bitmaps-images/basicbitmaps-small.png "A triple screenshot of the Basic Bitmaps page")](bitmaps-images/basicbitmaps-large.png#lightbox "A triple screenshot of the Basic Bitmaps page")

3番目のイメージは、 &mdash; プログラムを実行して独自の画像ライブラリから写真を読み込む場合にのみ表示されますが、 &mdash; 四角形内にも表示されますが、四角形の縦横比を維持するために四角形の位置とサイズが調整されます。 この計算は、ビットマップと移動先の四角形のサイズに基づいてスケールファクターを計算し、その領域で四角形を中央揃えにする必要があるため、さらに複雑になります。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (libraryBitmap != null)
    {
        float scale = Math.Min((float)info.Width / libraryBitmap.Width,
                               info.Height / 3f / libraryBitmap.Height);

        float left = (info.Width - scale * libraryBitmap.Width) / 2;
        float top = (info.Height / 3 - scale * libraryBitmap.Height) / 2;
        float right = left + scale * libraryBitmap.Width;
        float bottom = top + scale * libraryBitmap.Height;
        SKRect rect = new SKRect(left, top, right, bottom);
        rect.Offset(0, 2 * info.Height / 3);

        canvas.DrawBitmap(libraryBitmap, rect);
    }
    else
    {
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Blue;
            paint.TextAlign = SKTextAlign.Center;
            paint.TextSize = 48;

            canvas.DrawText("Tap to load bitmap",
                info.Width / 2, 5 * info.Height / 6, paint);
        }
    }
}
```

画像ライブラリからまだビットマップが読み込まれていない場合は、 `else` ユーザーに画面をタップするように求めるテキストがブロックに表示されます。

さまざまな透明度でビットマップを表示できます。また、SkiaSharp の[**透明度**](transparency.md)に関する次の記事では、その方法について説明します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [画像ライブラリから写真を選択する](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
