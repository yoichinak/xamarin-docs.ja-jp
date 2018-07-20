---
title: SkiaSharp のビットマップの基礎
description: この記事では、さまざまなソースから SkiaSharp のビットマップを読み込むし、Xamarin.Forms アプリケーションで表示する方法について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 5a535d60dd01e32dc1d888d3372db13312cc069a
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156965"
---
# <a name="bitmap-basics-in-skiasharp"></a>SkiaSharp のビットマップの基礎

_さまざまなソースからビットマップを読み込むし、それらを表示します。_

SkiaSharp のビットマップのサポートは非常に広範です。 この記事では、のみの基本を説明します&mdash;ビットマップを読み込む方法と、それらを表示する方法。

![](bitmaps-images/bitmapssample.png "2 つのビットマップの表示")

ビットマップの量により詳細な検証は、セクションで見つかる[SkiaSharp ビットマップ](../bitmaps/index.md)します。

SkiaSharp ビットマップの種類のオブジェクトである[ `SKBitmap`](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/)します。 ビットマップを作成する方法はたくさんありますが、この記事に制限する、 [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/System.IO.Stream/)メソッドは、.NET からビットマップを読み込み`Stream`オブジェクト。

**基本的なビットマップ**ページで、 **SkiaSharpFormsDemos**プログラムは、次の 3 つの異なるソースからビットマップを読み込む方法を示します。

- インターネットを介した
- 実行可能ファイルに埋め込まれたリソースから
- ユーザーのフォト ライブラリから

次の 3 つ`SKBitmap`これら 3 つのソースがフィールドとして定義されているオブジェクト、 [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs)クラス。

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

## <a name="loading-a-bitmap-from-the-web"></a>Web からビットマップを読み込む

URL に基づくビットマップを読み込むには、使用することができます、 [ `HttpClient` ](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0)クラス。 インスタンスを 1 つだけをインスタンス化する必要があります`HttpClient`と再利用であるため、そのフィールドとして。

```csharp
HttpClient httpClient = new HttpClient();
```

使用する場合`HttpClient`iOS と Android アプリケーションでは、に関するドキュメントで説明したように、プロジェクトのプロパティを設定する必要あります**[トランスポート層セキュリティ (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)** します。

使用する最も便利なので、`await`演算子`HttpClient`では、コードを実行できません、`BasicBitmapsPage`コンス トラクター。 代わりの一部では、`OnAppearing`をオーバーライドします。 ここで、URL は、いくつかのサンプルのビットマップの Xamarin の web サイト上の領域を指します。 Web サイト上のパッケージでは特定の幅にビットマップのサイズを変更するための仕様を追加できます。


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

            webBitmap = SKBitmap.Decode(stream);
            canvasView.InvalidateSurface();
        };
    }
    catch
    {
    }
}
```

使用する場合、android の例外が発生する、`Stream`から返された`GetStreamAsync`で、`SKBitmap.Decode`メソッド メイン スレッドで時間のかかる操作を実行することがあるためです。 このためにビットマップ ファイルの内容をコピー、`MemoryStream`オブジェクトを使用して`CopyToAsync`します。

静的な`SKBitmap.Decode`メソッドはビットマップ ファイルをデコードします。 JPEG、PNG、GIF、およびその他のいくつかの一般的なビットマップ形式と連携し、内部 SkiaSharp 形式で結果を格納します。 この時点で、`SKCanvasView`許可を無効にする必要があります、`PaintSurface`ハンドラーの表示を更新します。 

## <a name="loading-a-bitmap-resource"></a>ビットマップ リソースの読み込み

コードの観点からビットマップを読み込む最も簡単な方法は、アプリケーションで直接ビットマップ リソースを含むです。 **SkiaSharpFormsDemos**プログラムには、という名前のフォルダーが含まれています。**メディア**という名前のビットマップ ファイルを含む**monkey.png**します。 **プロパティ**ダイアログ、ファイルに対しては、このようなファイルを付ける必要があります、**ビルド アクション**の**埋め込まれたリソース**!

各埋め込みリソースには、*リソース ID*プロジェクト名、フォルダー、およびファイル名、ピリオドで接続されているすべてから構成される: **SkiaSharpFormsDemos.Media.monkey.png**します。 そのリソースを指定することでこのリソースへのアクセスを取得する ID を引数として、 [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String))のメソッド、 [ `Assembly` ](xref:System.Reflection.Assembly)クラス。

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    resourceBitmap = SKBitmap.Decode(stream);
}
```

これは、`Stream`オブジェクトに直接渡すことができます、`SKBitmap.Decode`メソッド。

## <a name="loading-a-bitmap-from-the-photo-library"></a>フォト ライブラリからビットマップを読み込む

ユーザーがデバイスの画像ライブラリから写真を読み込むこともできます。 この機能は、Xamarin.Forms 自体によって提供されていません。 ジョブには、資料に記載されているものなどの依存関係サービスが必要です。[画像ライブラリから写真を選択](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)します。

**IPhotoLibrary.cs**ファイル、 **SkiaSharpFormsDemos**プロジェクトと、3 つ**PhotoLibrary.cs**プラットフォーム プロジェクト ファイルをその記事から引用したものにされています。 さらに、Android **MainActivity.cs**ように、この記事で説明されているファイルが変更されていて、iOS プロジェクトに 2 行の下部に、フォト ライブラリへのアクセス許可が与えられて、 **info.plist**ファイル。

`BasicBitmapsPage`コンス トラクターを追加、`TapGestureRecognizer`を`SKCanvasView`タップの通知を受け取る。 タップの受信時に、`Tapped`ハンドラーは、依存関係サービスの画像の選択と呼び出しへのアクセスを取得します。`GetImageStreamAsync`します。 場合、`Stream`オブジェクトが返された後に内容がコピーされます、`MemoryStream`一部のプラットフォームで必要があります。 コードの残りの部分は、その他の 2 つの手法と同様です。

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

注意、`Tapped`ハンドラーの呼び出し、`InvalidateSurface`のメソッド、`SKCanvasView`オブジェクト。 新しい呼び出しが生成されます、`PaintSurface`ハンドラー。

## <a name="displaying-the-bitmaps"></a>ビットマップを表示します。

`PaintSurface`ハンドラーは、3 つのビットマップを表示する必要があります。 ハンドラーは、携帯電話が縦向きモードがあり、同等の 3 つの部分に、キャンバスを垂直方向に分割ことを想定しています。

最初のビットマップを表示すると、最も簡単な[ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/)メソッド。 指定する必要があるすべては、ビットマップの左上隅が配置される X および Y 座標。

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

ですが、`SKPaint`パラメーターが定義されている場合の既定値を持つ、`null`し、これを無視することができます。 ビットマップのピクセルは、一対一のマッピングを表示画面のピクセルに単純に転送されます。

プログラムでのビットマップのピクセル寸法を取得できます、 [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/)と[ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/)プロパティ。 これらのプロパティは、ビットマップをキャンバスの上に 3 番目の中央に位置座標を計算するプログラムを許可します。

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

その他の 2 つのビットマップがのバージョンで表示される[ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/)で、`SKRect`パラメーター。

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

3 番目のバージョン[ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/)が 2 つあります`SKRect`に表示されるのに、そのバージョンのビットマップの四角形のサブセットを指定する引数は、この記事で使用されていません。

次に、埋め込みリソース ビットマップから読み込まれたビットマップを表示するコードを示します。

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

ビットマップは、monkey はこれらのスクリーン ショットで水平方向に広がります理由は、四角形の大きさに拡大されます。

[![](bitmaps-images/basicbitmaps-small.png "基本的なビットマップ ページのスクリーン ショットが 3 倍になる")](bitmaps-images/basicbitmaps-large.png#lightbox "3 倍になる基本的なビットマップ ページのスクリーン ショット")

3 番目のイメージ&mdash;プログラムを実行し、画像ライブラリから写真を読み込むかどうかにのみ表示されています&mdash;四角形が四角形内に表示されることも、ビットマップの縦横比を維持するために、位置とサイズが調整されます。 この計算は、スケール ファクターのビットマップと先の四角形のサイズに基づいて、その領域内の四角形の中心を計算する必要があるため少し複雑です。

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

ビットマップがまだされて読み込まれていない場合、画像ライブラリから、`else`ブロックは、画面をタップしてユーザー入力を求めるテキストを表示します。


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [画像ライブラリから写真を選択](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
