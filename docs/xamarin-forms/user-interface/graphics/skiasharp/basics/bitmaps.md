---
title: SkiaSharp でビットマップの基本事項
description: この記事では、さまざまなソースから SkiaSharp のビットマップを読み込んで、Xamarin.Forms のアプリケーションでそれらを表示する方法について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 04/03/2017
ms.openlocfilehash: 291f08afb95c70e9f9fccc02e1fd7353cf107213
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244387"
---
# <a name="bitmap-basics-in-skiasharp"></a>SkiaSharp でビットマップの基本事項

_さまざまなソースからビットマップを読み込み、それらを表示します。_

SkiaSharp のビットマップのサポートは非常に広範です。 基本だけを取り上げて&mdash;ビットマップを読み込む方法と、それらを表示する方法。

![](bitmaps-images/bitmapssample.png "2 つのビットマップの表示")

SkiaSharp ビットマップは、型のオブジェクトを[ `SKBitmap`](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/)です。 ビットマップを作成する方法はたくさんありますが、この記事に制限する、 [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/SkiaSharp.SKStream/)からビットマップをロードするメソッド、 [ `SKStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStream/)ビットマップ ファイルを参照するオブジェクト。 使用すると便利です、 [ `SKManagedStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKManagedStream/)から派生したクラス`SKStream`.NET を受け取るコンス トラクターがあるため[ `Stream` ](https://developer.xamarin.com/api/type/System.IO.Stream/)オブジェクト。

**基本的なビットマップ** ページで、 **SkiaSharpFormsDemos**プログラムは、次の 3 つの異なるソースからビットマップを読み込む方法を示します。

- インターネット経由
- 実行可能ファイルに埋め込まれたリソースから
- ユーザーの写真のライブラリから

次の 3 つ`SKBitmap`これら 3 つのソースがフィールドのフィールドとして定義されているオブジェクト、 [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs)クラス。

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

## <a name="loading-a-bitmap-from-the-web"></a>Web からのビットマップの読み込み

URL に基づいているビットマップを読み込むには、使用することができます、 [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/)で実行される次のコードに示すように、クラス、`BasicBitmapsPage`コンス トラクターです。 ここで、URL は、いくつかのサンプルのビットマップと、Xamarin の web サイト上の領域を指します。 Web サイト上のパッケージは、特定の幅にビットマップのサイズを変更するための仕様を追加することを使用できます。

```csharp
Uri uri = new Uri("http://developer.xamarin.com/demo/IMG_3256.JPG?width=480");
WebRequest request = WebRequest.Create(uri);
request.BeginGetResponse((IAsyncResult arg) =>
{
    try
    {
        using (Stream stream = request.EndGetResponse(arg).GetResponseStream())
        using (MemoryStream memStream = new MemoryStream())
        {
            stream.CopyTo(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            using (SKManagedStream skStream = new SKManagedStream(memStream))
            {
                webBitmap = SKBitmap.Decode(skStream);
            }
        }
    }
    catch
    {
    }

    Device.BeginInvokeOnMainThread(() => canvasView.InvalidateSurface());

}, null);
```

ビットマップは正常にダウンロードされると、コールバック メソッドに渡す、`BeginGetResponse`メソッドが実行されます。 `EndGetResponse`呼び出しである必要があります、`try`ブロックする場合に、エラーが発生しました。 `Stream`オブジェクトから取得`GetResponseStream`が十分でない一部のプラットフォームにビットマップのコンテンツがコピーされますので、`MemoryStream`オブジェクト。 この時点で、`SKManagedStream`オブジェクトを作成することができます。 これは、ビットマップ ファイル、おそらくは JPEG または PNG ファイルを参照します。 `SKBitmap.Decode`メソッドは、ビットマップ ファイルをデコードし、内部 SkiaSharp 形式で結果を格納します。

コールバック メソッドに渡されます`BeginGetResponse`つまり実行コンス トラクターの実行が完了した後、`SKCanvasView`する許可を無効にする必要があります、`PaintSurface`表示を更新するハンドラー。 ただし、`BeginGetResponse`を使用する必要があるため、実行のセカンダリ スレッドでコールバックを実行`Device.BeginInvokeOnMainThread`を実行する、`InvalidateSurface`ユーザー インターフェイス スレッド内のメソッドです。

## <a name="loading-a-bitmap-resource"></a>ビットマップ リソースの読み込み

コードの観点からビットマップを読み込みに最も簡単な方法は、アプリケーションに直接ビットマップ リソースをも含みます。 **SkiaSharpFormsDemos**プログラムには、という名前のフォルダーが含まれています。**メディア**という名前のビットマップ ファイルを含む**monkey.png**です。 **プロパティ**ダイアログこのファイルには、このようなファイルに付与、**ビルド アクション**の**埋め込まれたリソース**!

各埋め込みリソースには、*リソース ID*プロジェクト名、フォルダー、およびファイル名、ピリオドで接続されているすべてから構成される: **SkiaSharpFormsDemos.Media.monkey.png**です。 このリソースへのアクセスは、そのリソースを指定することによって取得できます ID に渡す引数として、 [ `GetManifestResourceStream` ](https://developer.xamarin.com/api/member/System.Reflection.Assembly.GetManifestResourceStream/p/System.String/)のメソッド、 [ `Assembly` ](https://developer.xamarin.com/api/type/System.Reflection.Assembly/)クラス。

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
using (SKManagedStream skStream = new SKManagedStream(stream))
{
    resourceBitmap = SKBitmap.Decode(skStream);
}
```

これは、`Stream`に直接オブジェクトを変換することができます、`SKManagedStream`オブジェクト。

## <a name="loading-a-bitmap-from-the-photo-library"></a>フォト ライブラリからのビットマップの読み込み

ユーザー、デバイスの画像ライブラリから写真をロードすることもできます。 Xamarin.Forms 自体では、この機能は提供されていません。 ジョブには、記事に記載されているものなどの依存関係サービスが必要です。[画像ライブラリから写真をピッキング](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)です。

**IPicturePicker.cs**ファイルと、3 つ**PicturePickerImplementation.cs**のさまざまなプロジェクトをそのアーティクルのファイルがコピーされて、 **SkiaSharpFormsDemos**ソリューションでは、新しい名前空間の名前を指定します。 さらに、Android **MainActivity.cs**記事ではの説明に従って、ファイルは変更されており、iOS プロジェクトに 2 行の下部でフォト ライブラリへのアクセス権限が与えられて、 **info.plist**ファイル。

`BasicBitmapsPage`コンス トラクターを追加、`TapGestureRecognizer`を`SKCanvasView`タップの通知を受信します。 タップの受信時に、`Tapped`ハンドラーを呼び出し、画像の選択の依存関係サービスへのアクセスを取得`GetImageStreamAsync`です。 場合、`Stream`オブジェクトを取得しに、内容がコピー、`MemoryStream`プラットフォームの一部で必要があります。 コードの残りの部分は、その他の 2 つの手法と同様です。

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPicturePicker picturePicker = DependencyService.Get<IPicturePicker>();

    using (Stream stream = await picturePicker.GetImageStreamAsync())
    {
        if (stream != null)
        {
            using (MemoryStream memStream = new MemoryStream())
            {
                stream.CopyTo(memStream);
                memStream.Seek(0, SeekOrigin.Begin);

                using (SKManagedStream skStream = new SKManagedStream(memStream))
                {
                    libraryBitmap = SKBitmap.Decode(skStream);
                }
            }
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

注意して、`Tapped`ハンドラーの呼び出し、`InvalidateSurface`のメソッド、`SKCanvasView`オブジェクト。 これは、新しい呼び出しで生成される、`PaintSurface`ハンドラー。

## <a name="displaying-the-bitmaps"></a>ビットマップを表示します。

`PaintSurface`ハンドラーは、3 つのビットマップを表示する必要があります。 ハンドラーには、電話が縦向きモードがあり、同等の 3 つの部分にキャンバスを垂直方向に分割ことが前提とします。

最初のビットマップが表示され、最も簡単な[ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/)メソッドです。 指定する必要がありますすべてを配置するビットマップの左上隅がここでは、X 座標と Y 座標。

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

ただし、`SKPaint`パラメーターが定義されているが、既定値は、`null`およびそれを無視することができます。 ビットマップのピクセルは、一対一のマッピングをディスプレイ画面のピクセルに単に転送されます。

プログラムのビットマップのピクセル数を取得できます、 [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/)と[ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/)プロパティです。 これらのプロパティは、ビットマップを上の 3 分のキャンバスの中央に位置する座標を計算するプログラムを許可します。

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

バージョンとその他の 2 つのビットマップが表示される[ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/)で、`SKRect`パラメーター。

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

3 番目のバージョンの[ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) 2 つの`SKRect`に表示されるのに、そのバージョンのビットマップの四角形のサブセットを指定する引数は、この記事で使用されていません。

埋め込まれたリソース ビットマップから読み込むビットマップを表示するコードを次に示します。

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

ビットマップが変わっても、サルはこれらのスクリーン ショットで水平方向に広がります四角形の寸法に合わせて引き伸ばされます。

[![](bitmaps-images/basicbitmaps-small.png "基本的なビットマップ ページのスクリーン ショット トリプル")](bitmaps-images/basicbitmaps-large.png#lightbox "トリプル基本的なビットマップ ページのスクリーン ショット")

3 番目のイメージ&mdash;のみ表示されるプログラムを実行し、独自の画像ライブラリから写真を読み込むかどうか&mdash;四角形は四角形内に表示されるもビットマップの縦横比を維持するために位置とサイズを調整します。 この計算は、必要とするスケール ファクターのビットマップと移行先の四角形のサイズに基づく値とその領域内の四角形の中心を計算するために少し複雑です。

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

ビットマップがまだ読み込まれていません画像ライブラリの場合、`else`ブロックには、画面をタップするユーザー入力を求めるテキストが表示されます。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [画像ライブラリから写真を選択](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
