---
title: SkiaSharp ビットマップのタイル
description: ビットマップを使用して水平方向および垂直方向に連続して領域を並べます。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 9ED14E07-4DC8-4B03-8A33-772838BF51EA
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c6d139e47974247ce4af6bfa6c32331fcf7c824
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563134"
---
# <a name="skiasharp-bitmap-tiling"></a>SkiaSharp ビットマップのタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock)

前の2つの記事で説明したように、 [`SKShader`](xref:SkiaSharp.SKShader) クラスは線状または円形のグラデーションを作成できます。 この記事では、ビットマップを使用して領域をタイルするオブジェクトに焦点を当てて `SKShader` います。 ビットマップは、水平方向および垂直方向に繰り返すことができます。元の向きにするか、水平方向と垂直方向に反転させることができます。 反転を行うと、タイル間の不連続性が回避されます。

![ビットマップタイルのサンプル](bitmap-tiling-images/BitmapTilingSample.png "ビットマップタイルのサンプル")

[`SKShader.CreateBitmap`](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode))このシェーダーを作成する静的メソッドには、 `SKBitmap` パラメーターと列挙体の2つのメンバーがあり [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) ます。

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy)
```

2つのパラメーターは、水平方向のタイル分割と垂直方向の並べ表示に使用されるモードを示します。 これは、 `SKShaderTileMode` グラデーションメソッドでも使用される同じ列挙体です。

オーバーロードには、タイル化され [`CreateBitmap`](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) `SKMatrix` たビットマップに対して変換を実行するための引数が含まれています。

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy, SKMatrix localMatrix)
```

この記事では、タイル化されたビットマップでこのマトリックス変換を使用する例をいくつか紹介します。

## <a name="exploring-the-tile-modes"></a>タイルモードの調査

[**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの [**シェーダーとその他の効果**] ページの [**ビットマップのタイル**] セクションにある最初のプログラムは、2つの引数の効果を示して `SKShaderTileMode` います。 **ビットマップタイルフリップモード**XAML ファイルは、 `SKCanvasView` `Picker` `SKShaderTilerMode` 水平および垂直のタイルの値を選択できるようにすると2つのビューをインスタンス化します。 次のように、メンバーの配列 `SKShaderTileMode` がセクションで定義されていることに注意して `Resources` ください。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BitmapTileFlipModesPage"
             Title="Bitmap Tile Flip Modes">

    <ContentPage.Resources>
        <x:Array x:Key="tileModes"
                 Type="{x:Type skia:SKShaderTileMode}">
            <x:Static Member="skia:SKShaderTileMode.Clamp" />
            <x:Static Member="skia:SKShaderTileMode.Repeat" />
            <x:Static Member="skia:SKShaderTileMode.Mirror" />
        </x:Array>
    </ContentPage.Resources>

    <StackLayout>
        <skiaforms:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="xModePicker"
                Title="Tile X Mode"
                Margin="10, 0"
                ItemsSource="{StaticResource tileModes}"
                SelectedIndex="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />

        <Picker x:Name="yModePicker"
                Title="Tile Y Mode"
                Margin="10, 10"
                ItemsSource="{StaticResource tileModes}"
                SelectedIndex="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />

    </StackLayout>
</ContentPage>
```

分離コードファイルのコンストラクターは、サルを示すビットマップリソースに読み込まれます。 まず、のメソッドを使用してイメージをトリミングし、 [`ExtractSubset`](xref:SkiaSharp.SKBitmap.ExtractSubset(SkiaSharp.SKBitmap,SkiaSharp.SKRectI)) `SKBitmap` ヘッドと脚がビットマップの端に接するようにします。 次に、コンストラクターはメソッドを使用して、 [`Resize`](xref:SkiaSharp.SKBitmap.Resize(SkiaSharp.SKImageInfo,SkiaSharp.SKBitmapResizeMethod)) サイズの半分の別のビットマップを作成します。 これらの変更により、ビットマップがより適したタイルになります。

```csharp
public partial class BitmapTileFlipModesPage : ContentPage
{
    SKBitmap bitmap;

    public BitmapTileFlipModesPage ()
    {
        InitializeComponent ();

        SKBitmap origBitmap = BitmapExtensions.LoadBitmapResource(
            GetType(), "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

        // Define cropping rect
        SKRectI cropRect = new SKRectI(5, 27, 296, 260);

        // Get the cropped bitmap
        SKBitmap croppedBitmap = new SKBitmap(cropRect.Width, cropRect.Height);
        origBitmap.ExtractSubset(croppedBitmap, cropRect);

        // Resize to half the width and height
        SKImageInfo info = new SKImageInfo(cropRect.Width / 2, cropRect.Height / 2);
        bitmap = croppedBitmap.Resize(info, SKBitmapResizeMethod.Box);
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get tile modes from Pickers
        SKShaderTileMode xTileMode =
            (SKShaderTileMode)(xModePicker.SelectedIndex == -1 ?
                                        0 : xModePicker.SelectedItem);
        SKShaderTileMode yTileMode =
            (SKShaderTileMode)(yModePicker.SelectedIndex == -1 ?
                                        0 : yModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateBitmap(bitmap, xTileMode, yTileMode);
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

ハンドラーは、 `PaintSurface` `SKShaderTileMode` 2 つのビューから設定を取得 `Picker` し、 `SKShader` ビットマップとその2つの値に基づいてオブジェクトを作成します。 このシェーダーは、キャンバスの塗りつぶしに使用されます。

[![ビットマップタイルフリップモード](bitmap-tiling-images/BitmapTileFlipModes.png "ビットマップタイルフリップモード")](bitmap-tiling-images/BitmapTileFlipModes-Large.png#lightbox)

左側の iOS 画面は、の既定値の効果を示して `SKShaderTileMode.Clamp` います。 ビットマップは、左上隅にあります。 ビットマップの下には、ピクセルの一番下の行がすべての方向に繰り返されます。 ビットマップの右側には、ピクセルの右端の列が全体にわたって繰り返されます。 キャンバスの残りの部分は、ビットマップの右下隅にある濃い茶色のピクセルで色付けされます。 この `Clamp` オプションはビットマップのタイルではほとんど使用されないことが明らかです。

中央の Android 画面には、両方の引数のの結果が表示され `SKShaderTileMode.Repeat` ます。 タイルは水平方向および垂直方向に繰り返されます。 ユニバーサル Windows プラットフォーム画面が表示さ `SKShaderTileMode.Mirror` れます。 タイルは繰り返しますが、水平方向と垂直方向に反転されます。 このオプションの利点は、タイル間に不連続性がないことです。

水平および垂直の繰り返しでは、異なるオプションを使用できることに注意してください。 `SKShaderTileMode.Mirror`2 番目の引数として、 `CreateBitmap` 3 番目の引数としてを指定でき `SKShaderTileMode.Repeat` ます。 各行では、猿は通常のイメージとミラーイメージの間で交互に切り替わりますが、猿は反転されません。

## <a name="patterned-backgrounds"></a>パターンの背景

ビットマップのタイルは、通常、比較的小さいビットマップからパターン化された背景を作成するために使用されます。 従来の例は、ブリックウォールです。

**アルゴリズムブリックの壁**のページでは、ブリック全体に似た小さなビットマップが作成され、ブリックの2つの半分が従来のように分離されています。 このブリックは次のサンプルでも使用されるため、静的コンストラクターによって作成され、静的なプロパティを使用してパブリックになります。

```csharp
public class AlgorithmicBrickWallPage : ContentPage
{
    static AlgorithmicBrickWallPage()
    {
        const int brickWidth = 64;
        const int brickHeight = 24;
        const int morterThickness = 6;
        const int bitmapWidth = brickWidth + morterThickness;
        const int bitmapHeight = 2 * (brickHeight + morterThickness);

        SKBitmap bitmap = new SKBitmap(bitmapWidth, bitmapHeight);

        using (SKCanvas canvas = new SKCanvas(bitmap))
        using (SKPaint brickPaint = new SKPaint())
        {
            brickPaint.Color = new SKColor(0xB2, 0x22, 0x22);

            canvas.Clear(new SKColor(0xF0, 0xEA, 0xD6));
            canvas.DrawRect(new SKRect(morterThickness / 2,
                                       morterThickness / 2,
                                       morterThickness / 2 + brickWidth,
                                       morterThickness / 2 + brickHeight),
                                       brickPaint);

            int ySecondBrick = 3 * morterThickness / 2 + brickHeight;

            canvas.DrawRect(new SKRect(0,
                                       ySecondBrick,
                                       bitmapWidth / 2 - morterThickness / 2,
                                       ySecondBrick + brickHeight),
                                       brickPaint);

            canvas.DrawRect(new SKRect(bitmapWidth / 2 + morterThickness / 2,
                                       ySecondBrick,
                                       bitmapWidth,
                                       ySecondBrick + brickHeight),
                                       brickPaint);
        }

        // Save as public property for other programs
        BrickWallTile = bitmap;
    }

    public static SKBitmap BrickWallTile { private set; get; }
    ···
}
```

結果のビットマップの幅は70ピクセル、高さは60ピクセルです。

![アルゴリズムブリックの壁面タイル](bitmap-tiling-images/AlgorithmicBrickWallTile.png "アルゴリズムブリックの壁面タイル")

**アルゴリズムブリックの壁**のページの残りの部分では、 `SKShader` このイメージを水平方向および垂直方向に繰り返すオブジェクトを作成します。

```csharp
public class AlgorithmicBrickWallPage : ContentPage
{
    ···
    public AlgorithmicBrickWallPage ()
    {
        Title = "Algorithmic Brick Wall";

        // Create SKCanvasView
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

        using (SKPaint paint = new SKPaint())
        {
            // Create bitmap tiling
            paint.Shader = SKShader.CreateBitmap(BrickWallTile,
                                                 SKShaderTileMode.Repeat,
                                                 SKShaderTileMode.Repeat);
            // Draw background
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

結果は次のとおりです。

[![アルゴリズムブリックの壁面](bitmap-tiling-images/AlgorithmicBrickWall.png "アルゴリズムブリックの壁面")](bitmap-tiling-images/AlgorithmicBrickWall-Large.png#lightbox)

もう少し現実的なことをお勧めします。 このような場合は、実際のレンガ壁の写真を撮影し、トリミングすることができます。 このビットマップの幅は300ピクセル、高さは150ピクセルです。

![ブリックウォールタイル](bitmap-tiling-images/BrickWallTile.jpg "ブリックウォールタイル")

このビットマップは、 **写真ブリックの壁** のページで使用されます。

```csharp
public class PhotographicBrickWallPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(PhotographicBrickWallPage),
                        "SkiaSharpFormsDemos.Media.BrickWallTile.jpg");

    public PhotographicBrickWallPage()
    {
        Title = "Photographic Brick Wall";

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

        using (SKPaint paint = new SKPaint())
        {
            // Create bitmap tiling
            paint.Shader = SKShader.CreateBitmap(bitmap,
                                                 SKShaderTileMode.Mirror,
                                                 SKShaderTileMode.Mirror);
            // Draw background
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

の引数は両方とも同じであることに注意して `SKShaderTileMode` `CreateBitmap` `Mirror` ください。 このオプションは、通常、実際のイメージから作成されたタイルを使用する場合に必要です。 タイルをミラーリングすると、不連続性が回避されます。

[![写真ブリックの壁面](bitmap-tiling-images/PhotographicBrickWall.png "写真ブリックの壁面")](bitmap-tiling-images/PhotographicBrickWall-Large.png#lightbox)

タイルに適切なビットマップを取得するには、いくつかの作業が必要です。 これは、暗いブリックがあまり大きくないため、非常にうまく機能しません。 繰り返し画像内に定期的に表示されます。このブリック壁面は小さいビットマップから構築されたという事実を明らかにしています。

[**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの**メディア**フォルダーには、次のようなストーンウォールのイメージも含まれています。

![ストーンウォールタイル](bitmap-tiling-images/StoneWallTile.jpg "ストーンウォールタイル")

ただし、元のビットマップはタイルには大きすぎます。 サイズを変更することもでき `SKShader.CreateBitmap` ますが、変換を適用してタイルのサイズを変更することもできます。 このオプションを **次に示し** ます。

```csharp
public class StoneWallPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(StoneWallPage),
                        "SkiaSharpFormsDemos.Media.StoneWallTile.jpg");

    public StoneWallPage()
    {
        Title = "Stone Wall";

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

        using (SKPaint paint = new SKPaint())
        {
            // Create scale transform
            SKMatrix matrix = SKMatrix.MakeScale(0.5f, 0.5f);

            // Create bitmap tiling
            paint.Shader = SKShader.CreateBitmap(bitmap,
                                                 SKShaderTileMode.Mirror,
                                                 SKShaderTileMode.Mirror,
                                                 matrix);
            // Draw background
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

次のように、 `SKMatrix` イメージを元のサイズの半分にスケーリングするための値が作成されます。

[![石の壁](bitmap-tiling-images/StoneWall.png "石の壁")](bitmap-tiling-images/StoneWall-Large.png#lightbox)

変換は、メソッドで使用されている元のビットマップに対して動作し `CreateBitmap` ますか。 または、タイルの結果配列を変換しますか。 

この質問に答える簡単な方法は、変換の一部として回転を含めることです。

```csharp
SKMatrix matrix = SKMatrix.MakeScale(0.5f, 0.5f);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(15));
```

変換が個々のタイルに適用されている場合は、タイルのそれぞれの繰り返し画像を回転させる必要があり、結果には多数の不連続性が含まれます。 ただし、このスクリーンショットでは、タイルの複合配列が変換されていることがわかります。

[![石の壁が横](bitmap-tiling-images/StoneWallRotated.png "石の壁が横")](bitmap-tiling-images/StoneWallRotated-Large.png#lightbox)

セクションタイルの [**配置**](#tile-alignment)では、シェーダーに適用された変換変換の例が表示されます。

スタンドアロンの [**Cat Clock**](/samples/xamarin/xamarin-forms-samples/catclock) サンプル ( **SkiaSharpFormsDemos**の一部ではありません) は、この240ピクセルの四角形のビットマップに基づいてビットマップのタイルを使用して、木材粒子の背景をシミュレートします。

![木材粒子](bitmap-tiling-images/WoodGrain.png "木材粒子")

これは、木の写真です。 `SKShaderTileMode.Mirror`オプションを使用すると、木のより大きな領域として表示できます。

[![Cat の時計](bitmap-tiling-images/CatClock.png "Cat の時計")](bitmap-tiling-images/CatClock-Large.png#lightbox)

## <a name="tile-alignment"></a>タイルの配置

これまでに示したすべての例では、によって作成されたシェーダーを使用して、キャンバス全体をカバーしてい `SKShader.CreateBitmap` ます。 ほとんどの場合、小さな領域をファイリングする場合はビットマップタイルを使用し、太い線の場合は (よりまれに) を使用します。 次に、小さな四角形に使用される写真ブリック壁面タイルを示します。

[![タイルの配置](bitmap-tiling-images/TileAlignment.png "タイルの配置")](bitmap-tiling-images/TileAlignment-Large.png#lightbox)

これには問題がないかもしれません。 タイルパターンが四角形の左上隅の完全なブリックから始まっていない可能性があります。 これは、装飾したグラフィックオブジェクトではなく、キャンバスにシェーダーがアラインされているためです。

修正は単純です。 `SKMatrix`変換変換に基づいて値を作成します。 この変換は、タイルの左上隅を配置するポイントに、タイル化されたパターンを効果的にシフトします。 この方法は、上に示した整列されていないタイルのイメージを作成した [ **タイルの配置** ] ページで示されています。

```csharp
public class TileAlignmentPage : ContentPage
{
    bool isAligned;

    public TileAlignmentPage()
    {
        Title = "Tile Alignment";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;

        // Add tap handler
        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            isAligned ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);

        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            SKRect rect = new SKRect(info.Width / 7,
                                     info.Height / 7,
                                     6 * info.Width / 7,
                                     6 * info.Height / 7);

            // Get bitmap from other program
            SKBitmap bitmap = AlgorithmicBrickWallPage.BrickWallTile;

            // Create bitmap tiling
            if (!isAligned)
            {
                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat);
            }
            else
            {
                SKMatrix matrix = SKMatrix.MakeTranslation(rect.Left, rect.Top);

                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat,
                                                     matrix);
            }

            // Draw rectangle
            canvas.DrawRect(rect, paint);
        }
    }
}
```

**タイルの配置**ページにはが含まれてい `TapGestureRecognizer` ます。 画面をタップまたはクリックすると、プログラムは引数を使用してメソッドに切り替わり `SKShader.CreateBitmap` `SKMatrix` ます。 この変換は、左上隅に完全なブリックが含まれるようにパターンをシフトします。

[![タップしたタイルの配置](bitmap-tiling-images/TileAlignmentTapped.png "タップしたタイルの配置")](bitmap-tiling-images/TileAlignmentTapped-Large.png#lightbox)

また、この方法を使用すると、タイル化されたビットマップパターンを、描画する領域内の中央に配置することもできます。 中央の **タイル** ページでは、 `PaintSurface` 最初に、キャンバスの中央に1つのビットマップを表示する場合のように、ハンドラーによって座標が計算されます。 次に、これらの座標を使用して、の変換変換を作成し `SKShader.CreateBitmap` ます。 この変換は、タイルが中央揃えになるように、パターン全体をシフトします。

```csharp
public class CenteredTilesPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(CenteredTilesPage),
                        "SkiaSharpFormsDemos.Media.monkey.png");

    public CenteredTilesPage ()
    {
        Title = "Centered Tiles";

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

        // Find coordinates to center bitmap in canvas...
        float x = (info.Width - bitmap.Width) / 2f;
        float y = (info.Height - bitmap.Height) / 2f;

        using (SKPaint paint = new SKPaint())
        {
            // ... but use them to create a translate transform
            SKMatrix matrix = SKMatrix.MakeTranslation(x, y);
            paint.Shader = SKShader.CreateBitmap(bitmap, 
                                                 SKShaderTileMode.Repeat, 
                                                 SKShaderTileMode.Repeat, 
                                                 matrix);

            // Use that tiled bitmap pattern to fill a circle
            canvas.DrawCircle(info.Rect.MidX, info.Rect.MidY,
                              Math.Min(info.Width, info.Height) / 2,
                              paint);
        }
    }
}
```

この `PaintSurface` ハンドラーの最後には、キャンバスの中央に円を描画します。 十分であることを確認してください。タイルの1つは、円の中央にあり、他のタイルは対称パターンで並べられています。

[![中央のタイル](bitmap-tiling-images/CenteredTiles.png "中央のタイル")](bitmap-tiling-images/CenteredTiles-Large.png#lightbox)

もう1つのセンタリングアプローチは少し簡単です。 タイルを中央に配置する変換を作成するのではなく、タイル化されたパターンのコーナーを中心にすることができます。 呼び出しで `SKMatrix.MakeTranslation` 、キャンバスの中央の引数を使用します。

```csharp
SKMatrix matrix = SKMatrix.MakeTranslation(info.Rect.MidX, info.Rect.MidY);
```

このパターンは、中央および対称になっていますが、中央にタイルがありません。

[![中央のタイルの代替](bitmap-tiling-images/CenteredTilesAlternate.png "中央のタイルの代替")](bitmap-tiling-images/CenteredTilesAlternate-Large.png#lightbox)

## <a name="simplification-through-rotation"></a>ローテーションによる単純化

メソッドで回転変換を使用すると `SKShader.CreateBitmap` 、ビットマップタイルが簡略化される場合があります。 これは、チェーンリンクフェンスのタイルを定義しようとしたときに明らかになります。 **ChainLinkTile.cs**ファイルにより、ここに表示されるタイルが作成されます (わかりやすくするためにピンク色の背景があります)。

![ハードチェーン-リンクタイル](bitmap-tiling-images/HardChainLinkTile.png "ハードチェーン-リンクタイル")

タイルには2つのリンクが含まれている必要があるため、コードによってタイルが4つの作業領域に分割されます。 左上隅と右下の作業領域は同じですが、完了していません。 このワイヤには、右上および左下にあるいくつかの描画で処理する必要がある小さなノッチがあります。 この作業をすべて実行するファイルは、174行の長さです。

このタイルを作成する方がはるかに簡単であることがわかります。

![より簡単なチェーンリンクタイル](bitmap-tiling-images/EasierChainLinkTile.png "より簡単なチェーンリンクタイル")

ビットマップタイルシェーダーが90°回転している場合、ビジュアルはほぼ同じです。

より簡単なチェーンリンクタイルを作成するコードは、[ **チェーンリンク] タイル** ページの一部です。 コンストラクターは、プログラムが実行されているデバイスの種類に基づいてタイルサイズを決定した後 `CreateChainLinkTile` 、を呼び出します。これは、線、パス、およびグラデーションシェーダーを使用してビットマップに描画します。

```csharp
public class ChainLinkFencePage : ContentPage
{
    ···
    SKBitmap tileBitmap;

    public ChainLinkFencePage ()
    {
        Title = "Chain-Link Fence";

        // Create bitmap for chain-link tiling
        int tileSize = Device.Idiom == TargetIdiom.Desktop ? 64 : 128;
        tileBitmap = CreateChainLinkTile(tileSize);

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    SKBitmap CreateChainLinkTile(int tileSize)
    {
        tileBitmap = new SKBitmap(tileSize, tileSize);
        float wireThickness = tileSize / 12f;

        using (SKCanvas canvas = new SKCanvas(tileBitmap))
        using (SKPaint paint = new SKPaint())
        {
            canvas.Clear();
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = wireThickness;
            paint.IsAntialias = true;

            // Draw straight wires first
            paint.Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0),
                                                         new SKPoint(0, tileSize),
                                                         new SKColor[] { SKColors.Silver, SKColors.Black },
                                                         new float[] { 0.4f, 0.6f },
                                                         SKShaderTileMode.Clamp);

            canvas.DrawLine(0, tileSize / 2,
                            tileSize / 2, tileSize / 2 - wireThickness / 2, paint);

            canvas.DrawLine(tileSize, tileSize / 2,
                            tileSize / 2, tileSize / 2 + wireThickness / 2, paint);

            // Draw curved wires
            using (SKPath path = new SKPath())
            {
                path.MoveTo(tileSize / 2, 0);
                path.LineTo(tileSize / 2 - wireThickness / 2, tileSize / 2);
                path.ArcTo(wireThickness / 2, wireThickness / 2,
                           0,
                           SKPathArcSize.Small,
                           SKPathDirection.CounterClockwise,
                           tileSize / 2, tileSize / 2 + wireThickness / 2);

                paint.Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0),
                                                             new SKPoint(0, tileSize),
                                                             new SKColor[] { SKColors.Silver, SKColors.Black },
                                                             null,
                                                             SKShaderTileMode.Clamp);
                canvas.DrawPath(path, paint);

                path.Reset();
                path.MoveTo(tileSize / 2, tileSize);
                path.LineTo(tileSize / 2 + wireThickness / 2, tileSize / 2);
                path.ArcTo(wireThickness / 2, wireThickness / 2,
                           0,
                           SKPathArcSize.Small,
                           SKPathDirection.CounterClockwise,
                           tileSize / 2, tileSize / 2 - wireThickness / 2);

                paint.Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0),
                                                             new SKPoint(0, tileSize),
                                                             new SKColor[] { SKColors.White, SKColors.Silver },
                                                             null,
                                                             SKShaderTileMode.Clamp);
                canvas.DrawPath(path, paint);
            }
            return tileBitmap;
        }
    }
    ···
}
```

ワイヤを除き、タイルは透明になっています。つまり、タイルを別のものの上に表示できます。 このプログラムは、ビットマップリソースの1つを読み込み、キャンバスに収まるようにそのファイルを表示して、シェーダーを上に描画します。

```csharp
public class ChainLinkFencePage : ContentPage
{
    SKBitmap monkeyBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(ChainLinkFencePage), "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");
    ···

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawBitmap(monkeyBitmap, info.Rect, BitmapStretch.UniformToFill, 
                            BitmapAlignment.Center, BitmapAlignment.Start);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateBitmap(tileBitmap, 
                                                 SKShaderTileMode.Repeat,
                                                 SKShaderTileMode.Repeat,
                                                 SKMatrix.MakeRotationDegrees(45));
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

シェーダーが45°回転して、実際のチェーンリンクフェンスのようになっていることに注意してください。

[![チェーンリンクフェンス](bitmap-tiling-images/ChainLinkFence.png "チェーンリンクフェンス")](bitmap-tiling-images/ChainLinkFence-Large.png#lightbox)

## <a name="animating-bitmap-tiles"></a>ビットマップタイルのアニメーション化

マトリックス変換をアニメーション化することにより、ビットマップタイルパターン全体をアニメーション化することができます。 パターンを水平方向または垂直方向、または両方に移動させたい場合があります。 これを行うには、シフト座標に基づいて平行移動変換を作成します。

また、小さなビットマップを描画したり、1秒あたり60倍の速度でビットマップのピクセルビットを操作したりすることもできます。 そのビットマップを並べて表示することができ、タイル化されたパターン全体をアニメーション化することができます。 

アニメーション化された **ビットマップタイル** ページは、この方法を示しています。 ビットマップは、64ピクセルの四角形として、フィールドとしてインスタンス化されます。 コンストラクターは `DrawBitmap` を呼び出して、初期外観を与えます。 `angle`フィールドがゼロの場合 (メソッドが最初に呼び出されたときと同じ)、ビットマップには X として交差する2つの行が含まれます。次の行は、値に関係なく、常にビットマップの端に移動できるようになり `angle` ます。 

```csharp
public class AnimatedBitmapTilePage : ContentPage
{
    const int SIZE = 64;

    SKCanvasView canvasView;
    SKBitmap bitmap = new SKBitmap(SIZE, SIZE);
    float angle;
    ···

    public AnimatedBitmapTilePage ()
    {
        Title = "Animated Bitmap Tile";

        // Initialize bitmap prior to animation
        DrawBitmap();

        // Create SKCanvasView 
        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
    void DrawBitmap()
    {
        using (SKCanvas canvas = new SKCanvas(bitmap))
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = SIZE / 8;

            canvas.Clear();
            canvas.Translate(SIZE / 2, SIZE / 2);
            canvas.RotateDegrees(angle);
            canvas.DrawLine(-SIZE, -SIZE, SIZE, SIZE, paint);
            canvas.DrawLine(-SIZE, SIZE, SIZE, -SIZE, paint);
        }
    }
    ···
}
```

アニメーションのオーバーヘッドはに発生し、は `OnAppearing` を `OnDisappearing` オーバーライドします。 この `OnTimerTick` メソッドは、 `angle` 0 ~ 360 度の値を10秒ごとにアニメーション化して、ビットマップ内の X 図形を回転させます。

```csharp
public class AnimatedBitmapTilePage : ContentPage
{
    ···
    // For animation
    bool isAnimating;
    Stopwatch stopwatch = new Stopwatch();
    ···

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 10;     // seconds
        angle = (float)(360f * (stopwatch.Elapsed.TotalSeconds % duration) / duration);
        DrawBitmap();
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

X の図が対称であるため、これは、 `angle` 2.5 秒ごとに0°から90度に値を回転させることと同じです。

`PaintSurface`ハンドラーはビットマップからシェーダーを作成し、描画オブジェクトを使用してキャンバス全体に色を設定します。

```csharp
public class AnimatedBitmapTilePage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateBitmap(bitmap,
                                                 SKShaderTileMode.Mirror,
                                                 SKShaderTileMode.Mirror);
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

`SKShaderTileMode.Mirror`オプションを使用すると、各ビットマップ内の x のアームが隣接するビットマップの x と結合して、単純なアニメーションよりもはるかに複雑なアニメーションパターンを作成できます。

[![アニメーション化したビットマップタイル](bitmap-tiling-images/AnimatedBitmapTile.png "アニメーション化したビットマップタイル")](bitmap-tiling-images/AnimatedBitmapTile-Large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [CatClock (サンプル)](/samples/xamarin/xamarin-forms-samples/catclock)