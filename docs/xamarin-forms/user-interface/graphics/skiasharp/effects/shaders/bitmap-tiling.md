---
title: SkiaSharp のビットマップのタイル
description: 水平および垂直方向に繰り返されるビットマップを使用して領域を並べて表示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 9ED14E07-4DC8-4B03-8A33-772838BF51EA
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 24c33c61002130fe645bba54c307394bbc2e0656
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61322256"
---
# <a name="skiasharp-bitmap-tiling"></a>SkiaSharp のビットマップのタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/CatClock/)

2 つの以前の記事で説明したように、 [ `SKShader` ](xref:SkiaSharp.SKShader)クラスは、線形または円形グラデーションを作成できます。 この記事の重点、`SKShader`領域をタイルにビットマップを使用するオブジェクト。 水平および垂直に、ビットマップを繰り返すことが元の方向のいずれかであるかまたは水平方向および垂直方向を反転します。 タイルの間の不連続性を回避、反転します。

![タイル サンプルのビットマップ](bitmap-tiling-images/BitmapTilingSample.png "ビットマップ タイルのサンプル")

静的な[ `SKShader.CreateBitmap` ](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode))このシェーダーを作成するメソッドが、`SKBitmap`パラメーターと 2 つのメンバー、 [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode)列挙体。

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy)
```

2 つのパラメーターは、タイルが水平方向と垂直方向のタイルを使用するモードを示します。 これは、同じ`SKShaderTileMode`グラデーション メソッドでも使用される列挙体。

A [ `CreateBitmap` ](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))オーバー ロードが含まれています、`SKMatrix`タイル化されたビットマップの変換を実行する引数。

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy, SKMatrix localMatrix)
```

この記事には、このマトリックス変換を使用して、ビットマップが並べて表示されたいくつかの例が含まれています。

## <a name="exploring-the-tile-modes"></a>タイル モードの調査

最初のプログラム、**ビットマップ タイル**のセクション、**シェーダーとその他の効果**のページ、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)サンプル2 つの効果を示します`SKShaderTileMode`引数。 **ビットマップ タイル反転モード**XAML ファイルのインスタンスを作成、`SKCanvasView`と 2 つ`Picker`ビューを選択するための`SKShaderTilerMode`水平および垂直に並べて表示の値。 注意の配列、`SKShaderTileMode`でメンバーが定義されている、`Resources`セクション。

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

分離コード ファイルのコンス トラクターは、座って monkey を示しているビットマップ リソースで読み込まれます。 使用してイメージをトリミングが、 [ `ExtractSubset` ](xref:SkiaSharp.SKBitmap.ExtractSubset(SkiaSharp.SKBitmap,SkiaSharp.SKRectI))メソッドの`SKBitmap`ヘッドとフィートがビットマップの端を触れることができるようにします。 コンス トラクターを使用し、 [ `Resize` ](xref:SkiaSharp.SKBitmap.Resize(SkiaSharp.SKImageInfo,SkiaSharp.SKBitmapResizeMethod))サイズの半分のもう 1 つのビットマップを作成します。 これらの変更は、並べて表示用、もう少し適切なビットマップをによりします。

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

`PaintSurface`ハンドラーを取得、 `SKShaderTileMode` 2 つからの設定`Picker`を表示し、作成、`SKShader`ビットマップ、およびこれら 2 つの値に基づいて、オブジェクト。 このシェーダーは、キャンバスの塗りつぶしに使用します。

[![ビットマップのタイルにフリップ モード](bitmap-tiling-images/BitmapTileFlipModes.png "ビットマップのタイルにフリップ モード")](bitmap-tiling-images/BitmapTileFlipModes-Large.png#lightbox)

左側にある iOS の画面は、の既定値の効果を示しています。`SKShaderTileMode.Clamp`します。 ビットマップは、左上隅に配置します。 ビットマップの下の最後まですべてのピクセルの一番下の行が繰り返されます。 ビットマップの右側に、すべてのピクセルの右端の列が繰り返されます。 キャンバスの残りの部分は、ビットマップの右上隅にある濃い茶色ピクセル色が設定します。 明らかな場合がある、`Clamp`オプションがほとんど使用されないビットマップのタイルを使用。

Android の画面中央には、結果を示しています。`SKShaderTileMode.Repeat`両方の引数。 タイルが水平および垂直方向に繰り返されます。 ユニバーサル Windows プラットフォームの画面に示す`SKShaderTileMode.Mirror`します。 タイルは繰り返されますが、または水平方向および垂直方向を反転します。 このオプションの利点は、タイル間で不連続性がないことです。

水平および垂直の繰り返しのさまざまなオプションを使用できることに注意してください。 指定できる`SKShaderTileMode.Mirror`2 番目の引数として`CreateBitmap`が`SKShaderTileMode.Repeat`3 番目の引数として。 行ごとに、通常のイメージと、ミラー イメージですが、猿はいずれも代替猿が上下します。

## <a name="patterned-backgrounds"></a>背景のパターン

ビットマップのタイルは、比較的小さなビットマップからパターンの背景を作成する通常使用されます。 典型的な例では、レンガの壁です。

**アルゴリズム レンガの壁**ページ全体のブリックと従来型で区切られたブリックの 2 つの要素のような小さいビットマップを作成します。 このブリックで次のサンプルにも使用しているため、静的コンス トラクターによって作成され、パブリックな静的プロパティは。

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

結果のビットマップは、60 ピクセル、高さ、幅 70 のピクセルには。

![アルゴリズムのレンガ壁タイル](bitmap-tiling-images/AlgorithmicBrickWallTile.png "アルゴリズム レンガ壁のタイル")

残りの部分、**アルゴリズム レンガの壁**ページを作成、`SKShader`オブジェクトを水平および垂直には、このイメージを繰り返します。

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

結果を次に示します。

[![アルゴリズムのレンガの壁](bitmap-tiling-images/AlgorithmicBrickWall.png "アルゴリズム レンガの壁")](bitmap-tiling-images/AlgorithmicBrickWall-Large.png#lightbox)

もう少し現実的なものをお勧めします。 その場合を実際のレンガの壁の写真を取得し、それをトリミングできます。 このビットマップは、幅 300 ピクセル、高さ 150 ピクセルです。

![レンガ壁タイル](bitmap-tiling-images/BrickWallTile.jpg "レンガ壁のタイル")

このビットマップがで使用される、 **Photographic レンガの壁**ページ。

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

注意、`SKShaderTileMode`引数`CreateBitmap`はどちらも`Mirror`します。 このオプションは、実際のイメージから作成されたタイルを使用する場合は、通常必要があります。 タイルをミラーリング不連続性を回避できます。

[![写真レンガの壁](bitmap-tiling-images/PhotographicBrickWall.png "Photographic レンガの壁")](bitmap-tiling-images/PhotographicBrickWall-Large.png#lightbox)

タイルの適切なビットマップを取得するには、いくつか作業が必要です。 この 1 つは、うまく非常に濃いブリックが目立つのでが多すぎます。 このレンガの壁が、小さいビットマップから構築されたという事実を公開、繰り返されるイメージ内で定期的に表示します。

**メディア**のフォルダー、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)サンプルには、石壁のこのイメージも含まれています。

![石壁タイル](bitmap-tiling-images/StoneWallTile.jpg "石壁タイル")

ただし、元のビットマップは少しタイルに対して大きすぎます。 このサイズを変更する可能性がありますが、`SKShader.CreateBitmap`メソッドが、変換を適用することでタイルのサイズ変更できることもできます。 このオプションの説明については、**石壁**ページ。

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

`SKMatrix`元の半分のサイズにイメージをスケールする値が作成されます。

[![壁を石](bitmap-tiling-images/StoneWall.png "石壁")](bitmap-tiling-images/StoneWall-Large.png#lightbox)

変換で使用される元のビットマップの動作、`CreateBitmap`メソッドでしょうか。 または、タイルの結果の配列が変換されることですか。 

この質問に回答する簡単な方法は、変換の一部としての回転を含めるには。

```csharp
SKMatrix matrix = SKMatrix.MakeScale(0.5f, 0.5f);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(15));
```

個々 のタイルに、変換を適用する場合、タイルの各繰り返しのイメージを回転するか、および結果には、多数の不連続性にが含まれます。 このスクリーン ショットのタイルの複合配列が変換されることから明らかです。

[![回転の壁を石](bitmap-tiling-images/StoneWallRotated.png "石壁の回転")](bitmap-tiling-images/StoneWallRotated-Large.png#lightbox)

セクションで[**タイルの配置**](#tile-alignment)、平行移動変換が、シェーダーを適用する例を確認します。

スタンドアロン[ **Cat クロック**](https://developer.xamarin.com/samples/xamarin-forms/CatClock)サンプル (の一部ではなく**SkiaSharpFormsDemos**) この 240 ピクセルの正方形のビットマップに基づくビットマップのタイルを使用して粒度が木の背景をシミュレートします。

![粒度の木](bitmap-tiling-images/WoodGrain.png "粒度の木")

木の床面の写真です。 `SKShaderTileMode.Mirror`木に大きな領域として表示するオプションを使用できます。

[![クロックを cat](bitmap-tiling-images/CatClock.png "Cat クロック")](bitmap-tiling-images/CatClock-Large.png#lightbox)

## <a name="tile-alignment"></a>タイルの配置

これまでに示したすべての例は、によって作成されたシェーダーを使用して`SKShader.CreateBitmap`キャンバス全体をカバーします。 ほとんどの場合に使用するビットマップ タイル小さな領域または (複数ことはほとんどありません) を提出するための太い線の内側を塗りつぶすとき。 レンガ壁の写真タイルが小さい四角形の使用を示します。

[![タイルの配置](bitmap-tiling-images/TileAlignment.png "タイルの配置")](bitmap-tiling-images/TileAlignment-Large.png#lightbox)

違いますは、問題になります。 タイル パターンの先頭が四角形の左上隅で完全なブリックでないことを望んでいるかもしれません。 シェーダーは、キャンバスと、グラフィカル オブジェクトではなくで使用するために調整されているためにです。

修正プログラムは簡単です。 作成、`SKMatrix`値変換の変換に基づいています。 変換は、配置するタイルの左上隅にあるポイントを並べて表示されたパターンを効果的に移ります。 この方法の説明については、**タイルの配置**ページで前に、示したアラインされていないタイルの画像の作成します。

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

**タイルの配置**ページが含まれています、`TapGestureRecognizer`します。 タップまたはクリックして、画面とプログラムにスイッチ、`SKShader.CreateBitmap`メソッドを`SKMatrix`引数。 この変換は、左上隅にあるすべてのブリックに含まれるように、パターンをシフトします。

[![タップされた位置をタイル](bitmap-tiling-images/TileAlignmentTapped.png "タップの配置を並べて表示")](bitmap-tiling-images/TileAlignmentTapped-Large.png#lightbox)

ビットマップが並べて表示されたパターンを描画する領域の中央に配置することを確認するのにこの手法を使用することもできます。 **タイルの中心** ページで、`PaintSurface`ハンドラーは、キャンバスの中央に 1 つのビットマップを表示する場合、最初の座標を計算します。 使用して、これらの座標に平行移動変換を作成する`SKShader.CreateBitmap`します。 この変換は、タイルが中央に配置されるように、パターン全体を移動します。

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

`PaintSurface`ハンドラーの最後に、キャンバスの中央に円を描画します。 確か、円の中心で正確には、タイルのいずれかと、対称パターンでは、他の配置。

[![タイルを中央揃え](bitmap-tiling-images/CenteredTiles.png "タイルを中央揃え")](bitmap-tiling-images/CenteredTiles-Large.png#lightbox)

中央の別の方法は、実際には少し簡単になります。 タイルを中央に格納された平行移動変換を構築するのではなく、並べて表示するパターンの中央に配置できます。 `SKMatrix.MakeTranslation`呼び出すをキャンバスの中央の引数を使用します。

```csharp
SKMatrix matrix = SKMatrix.MakeTranslation(info.Rect.MidX, info.Rect.MidY);
```

パターンには、中央揃えであり、対称ですがタイルの中心ではありません。

[![タイルの代替を中央揃え](bitmap-tiling-images/CenteredTilesAlternate.png "タイルの代替を中央揃え")](bitmap-tiling-images/CenteredTilesAlternate-Large.png#lightbox)

## <a name="simplification-through-rotation"></a>回転を単純化

回転変換を使用する、`SKShader.CreateBitmap`メソッドは、ビットマップのタイルを簡略化できます。 これは、チェーン リンク フェンスのタイルを定義しようとしています。 明らかになります。 **ChainLinkTile.cs**ファイルは、次に示します (わかりやすくするためのピンク色の背景) タイルを作成します。

![チェーン ハードリンク タイル](bitmap-tiling-images/HardChainLinkTile.png "チェーン ハードリンク タイル")

タイルは、コードは、4 つの象限タイルに分割するために 2 つのリンクを含める必要があります。 左上隅および右下の四分区間は同じですが完全ではありません。 ワイヤでは、右上および左下の四分区間にいくつか追加の描画に少しノッチ処理する必要があります。 このすべての処理を行うファイルは、長い 174 行です。

実はこのタイルを作成する方が楽になります。

![簡単にチェーン リンク タイル](bitmap-tiling-images/EasierChainLinkTile.png "簡単チェーン リンク タイル")

ビットマップ タイル シェーダーが 90 度回転の場合は、ビジュアルはほぼ同じです。

簡単チェーン リンク タイルを作成するコードの一部である、**チェーン リンク タイル**ページ。 コンス トラクターは、プログラムで実行されているデバイスと、呼び出しの種類に基づくタイルのサイズを決定します。 `CreateChainLinkTile`、行、パス、およびグラデーション シェーダーを使用してビットマップを描画します。

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

ワイヤ、を除き、タイルは透過的なつまりことは、別のものの上に表示できます。 プログラムは、ビットマップ リソースのいずれかで読み込み、キャンバスがいっぱいになるようを表示し、上部で、シェーダーを描画します。

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

シェーダーは、実際のチェーン リンク フェンスのような向きがあるため 45 度回転させることに注意してください。

[![チェーン リンク フェンス](bitmap-tiling-images/ChainLinkFence.png "チェーン リンク フェンス")](bitmap-tiling-images/ChainLinkFence-Large.png#lightbox)

## <a name="animating-bitmap-tiles"></a>アニメーションのビットマップのタイル

行列変換をアニメーション化して、全体のビットマップ タイル パターンをアニメーション化することができます。 おそらく横方向または垂直方向に移動するパターンまたはその両方が必要です。 変化の激しい座標に基づいて変換を作成しているを行うことができます。

小規模のビットマップを描画する、または秒間に 60 回の割合で、ビットマップのピクセル ビットを操作することもできます。 そのビットマップを並べて表示、使用でき、全体に並べて表示されたパターンがアニメーション化すると思われることができます。 

**アニメーション ビットマップ タイル**ページは、この方法を示します。 ビットマップは、64 ピクセルの正方形にするフィールドとしてインスタンス化されます。 コンス トラクター呼び出し`DrawBitmap`を初期の外観を付けます。 場合、`angle`フィールドは 0 (は、メソッドが初めて呼び出された場合) と、ビットマップが x マークを超えました。 2 つの行が含まれます。行が行われるに関係なく、ビットマップの端に常に到達するのに十分な長さ、`angle`値。 

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

アニメーションのオーバーヘッドが発生した、`OnAppearing`と`OnDisappearing`よりも優先されます。 `OnTimerTick`メソッドをアニメーション化、`angle`値 0 度から 360 度を 10 秒ごとに、ビットマップ内の図は、X を回転します。

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

X の図の対称性のためこれは、回転と同じ、`angle`値を 0 度から 90 度、2.5 秒ごとにします。

`PaintSurface`ハンドラーがビットマップからシェーダーを作成し、キャンバス全体に色をペイント オブジェクトを使用します。

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

`SKShaderTileMode.Mirror`を推奨します。 単純なアニメーションよりも複雑になるほどの多くと思われる全体的なアニメーション化されたパターンを作成する隣接するビットマップの X 印が付いた各ビットマップの X の腕が参加オプションを確認します。

[![ビットマップのタイルをアニメーション化](bitmap-tiling-images/AnimatedBitmapTile.png "ビットマップのタイルをアニメーション化")](bitmap-tiling-images/AnimatedBitmapTile-Large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [CatClock (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/CatClock/)
