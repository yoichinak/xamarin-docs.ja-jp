---
title: Porter blend モード
description: Porter-Duff blend モードを使用して、コピー元とコピー先のイメージに基づいてシーンを作成します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 57F172F8-BA03-43EC-A215-ED6B78696BB5
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 12e3e95b0f87d0e93d157bebe057874430866c2b
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91560781"
---
# <a name="porter-duff-blend-modes"></a>Porter blend モード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Porter-Duff blend モードには、トーマス Porter と Tom Duff の後に名前が付けられます。この場合、Lucasfilm の作業中に合成の代数を開発しました。 これらの紙 [_合成デジタル画像_](https://graphics.pixar.com/library/Compositing/paper.pdf) は、1984年7月の _コンピューターグラフィックス_の問題である253から259に発行されました。 これらの blend モードは、複合シーンにさまざまなイメージをアセンブルする合成に不可欠です。

![Porter-Duff サンプル](porter-duff-images/PorterDuffSample.png "Porter-Duff サンプル")

## <a name="porter-duff-concepts"></a>Porter-Duff の概念

次のように、brownish 四角形が表示サーフェイスの左側と上部の 2 ~ 3 を占めているとします。

![Porter-Duff Destination](porter-duff-images/PorterDuffDst.png "Porter-Duff Destination")

この領域は、 _変換先_ 、または _背景_ また _は_背景と呼ばれます。

次の四角形を描画します。これは、変換先のサイズと同じです。 四角形は、右側と下 2 ~ 3 を占める bluish 領域を除いて透明です。

![Porter-Duff ソース](porter-duff-images/PorterDuffSrc.png "Porter-Duff ソース")

これは、 _ソース_ または _フォアグラウンド_と呼ばれます。

変換先にソースを表示すると、次のようになります。

![Porter-Duff ソース](porter-duff-images/PorterDuffSrcOver.png "Porter-Duff ソース")

ソースの透明ピクセルを使用すると、背景が透けて見えるようになります。一方、bluish ソースピクセルでは背景が見えにくくなります。 これは通常のケースであり、SkiaSharp ではと呼ばれて `SKBlendMode.SrcOver` います。 この値は、 `BlendMode` `SKPaint` オブジェクトが最初にインスタンス化されるときのプロパティの既定の設定です。

ただし、別の効果に対して異なる blend モードを指定することもできます。 を指定した場合、 `SKBlendMode.DstOver` ソースとターゲットが交差する領域では、変換先がソースの代わりに表示されます。

![Porter-Duff 転送先](porter-duff-images/PorterDuffDstOver.png "Porter-Duff 転送先")

Blend モードでは、ターゲットの `SKBlendMode.DstIn` 色を使用して、変換先とコピー元の交差部分のみが表示されます。

![Porter-Duff の宛先](porter-duff-images/PorterDuffDstIn.png "Porter-Duff の宛先")

Blend モード `SKBlendMode.Xor` (排他または) では、2つの領域が重なっている場所は何も表示されません。

![Porter-Duff 排他的または](porter-duff-images/PorterDuffXor.png "Porter-Duff 排他的または")

塗りつぶされた変換先とソースの四角形を使用すると、表示サーフェイスが実質的に4つの一意の領域に分割されます。これは、変換先と変換元の四角形の存在に対応するさまざまな方法で色付けできます。

![Porter-Duff](porter-duff-images/PorterDuff.png "Porter-Duff")

これらの領域では、変換先とソースの両方が透明であるため、右上隅と左下の四角形は常に空白になります。 コピー先の色は、左上の領域を占めているので、塗りつぶしの色には塗りつぶしの色を使用できます。 同様に、ソースの色は右下の領域を占めているので、領域には元の色で色付けすることも、まったく使用しないこともできます。 中央にあるコピー先とコピー元の交差部分には、コピー先の色、基になる色、またはまったく色を付けないようにすることができます。

組み合わせの合計数は 2 (左上)、2回目 (右下)、3 (中央)、または12になります。 これらは、12個の基本的な Porter-Duff モードです。

_デジタルイメージの合成_(ページ 256)、Porter、および duff の終わりに向かって、_プラス_(SkiaSharp メンバーに対応する) と `SKBlendMode.Plus` w3c_薄_モード (w3c の_明るく_モードと混同しない) の13番目のモードを追加します。この `Plus` モードでは、出力先とコピー元の色が追加されます。このプロセスの詳細については、後で詳しく説明します。

Skia はと呼ばれる14のモードを追加し `Modulate` ますが、これは、 `Plus` 変換先とソースの色が乗算される点が異なります。 これは、追加の Porter Ff blend モードとして扱うことができます。

SkiaSharp で定義されている14の Porter Ff モードを次に示します。 次の表は、上の図の3つの空白以外の各領域の色を示しています。

| モード       | Destination | 合う | source |
| ---------- |:-----------:|:------------:|:------:|
| `Clear`    |             |              |        |
| `Src`      |             | source       | X      |
| `Dst`      | X           | Destination  |        |
| `SrcOver`  | X           | source       | X      |
| `DstOver`  | X           | Destination  | X      |
| `SrcIn`    |             | source       |        |
| `DstIn`    |             | Destination  |        |
| `SrcOut`   |             |              | X      |
| `DstOut`   | X           |              |        |
| `SrcATop`  | X           | source       |        |
| `DstATop`  |             | Destination  | X      |
| `Xor`      | X           |              | X      |
| `Plus`     | X           | SUM          | X      |
| `Modulate` |             | 製品      |        | 

これらの blend モードは対称です。 転送元と転送先を交換し、すべてのモードを引き続き使用できます。

モードの命名規則は、次のいくつかの簡単な規則に従います。

- **Src** または **Dst** 自体は、変換元または変換先のピクセルだけが表示されることを意味します。
- [サフィックス ( **上** ) の値は、交差部分に表示される内容を示します。 転送元または転送先がもう一方の "上" に描画されます。
- **In**サフィックスは、交差部分だけが色分けされていることを意味します。 出力は、転送元または転送先の部分のみに制限されます。
- **出力**サフィックスは、交差部分に色が付いていないことを意味します。 出力は、交差部分の送信元または送信先の一部にすぎません。
- **一番**上のサフィックスは、 **In**と**Out**の和集合です。これには、転送元または転送先が "一番上" である領域が含まれます。

モードとモードの違いに注意して `Plus` `Modulate` ください。 これらのモードでは、ソースとターゲットのピクセルで異なる種類の計算が実行されます。 これらの詳細については、すぐに説明します。

**Porter-Duff グリッド**ページでは、1つの画面に表示される14のすべてのモードがグリッド形式で表示されます。 各モードは、の別のインスタンスです `SKCanvasView` 。 そのため、クラスは、 `SKCanvasView` という名前のから派生 `PorterDuffCanvasView` します。 静的コンストラクターは、同じサイズの2つのビットマップを作成します。1つは brownish 四角形を左領域に、もう1つは bluish 四角形を使用します。

```csharp
class PorterDuffCanvasView : SKCanvasView
{
    static SKBitmap srcBitmap, dstBitmap;

    static PorterDuffCanvasView()
    {
        dstBitmap = new SKBitmap(300, 300);
        srcBitmap = new SKBitmap(300, 300);

        using (SKPaint paint = new SKPaint())
        {
            using (SKCanvas canvas = new SKCanvas(dstBitmap))
            {
                canvas.Clear();
                paint.Color = new SKColor(0xC0, 0x80, 0x00);
                canvas.DrawRect(new SKRect(0, 0, 200, 200), paint);
            }
            using (SKCanvas canvas = new SKCanvas(srcBitmap))
            {
                canvas.Clear();
                paint.Color = new SKColor(0x00, 0x80, 0xC0);
                canvas.DrawRect(new SKRect(100, 100, 300, 300), paint);
            }
        }
    }
    ···
}
```

インスタンスコンストラクターには型のパラメーターがあり `SKBlendMode` ます。 このパラメーターはフィールドに保存されます。 

```csharp
class PorterDuffCanvasView : SKCanvasView
{
    ···
    SKBlendMode blendMode;

    public PorterDuffCanvasView(SKBlendMode blendMode)
    {
        this.blendMode = blendMode;
    }

    protected override void OnPaintSurface(SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find largest square that fits
        float rectSize = Math.Min(info.Width, info.Height);
        float x = (info.Width - rectSize) / 2;
        float y = (info.Height - rectSize) / 2;
        SKRect rect = new SKRect(x, y, x + rectSize, y + rectSize);

        // Draw destination bitmap
        canvas.DrawBitmap(dstBitmap, rect);

        // Draw source bitmap
        using (SKPaint paint = new SKPaint())
        {
            paint.BlendMode = blendMode;
            canvas.DrawBitmap(srcBitmap, rect, paint);
        }

        // Draw outline
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 2;
            rect.Inflate(-1, -1);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

オーバーライドは、 `OnPaintSurface` 2 つのビットマップを描画します。 最初のは正常に描画されます。

```csharp
canvas.DrawBitmap(dstBitmap, rect);
```

2番目のは、 `SKPaint` `BlendMode` プロパティがコンストラクター引数に設定されているオブジェクトを使用して描画されます。

```csharp
using (SKPaint paint = new SKPaint())
{
    paint.BlendMode = blendMode;
    canvas.DrawBitmap(srcBitmap, rect, paint);
}
```

オーバーライドの残りの部分では、 `OnPaintSurface` ビットマップの周囲に四角形を描画してサイズを示します。

`PorterDuffGridPage`クラスは `PorterDurffCanvasView` 、配列の各メンバーに1つずつ、の14個のインスタンスを作成し `blendModes` ます。 `SKBlendModes`同様のモードを隣接するように配置するために、配列内のメンバーの順序はテーブルと少し異なります。 の14個のインスタンス `PorterDuffCanvasView` は、のラベルと共にまとめられてい `Grid` ます。

```csharp
public class PorterDuffGridPage : ContentPage
{
    public PorterDuffGridPage()
    {
        Title = "Porter-Duff Grid";

        SKBlendMode[] blendModes =
        {
            SKBlendMode.Src, SKBlendMode.Dst, SKBlendMode.SrcOver, SKBlendMode.DstOver,
            SKBlendMode.SrcIn, SKBlendMode.DstIn, SKBlendMode.SrcOut, SKBlendMode.DstOut,
            SKBlendMode.SrcATop, SKBlendMode.DstATop, SKBlendMode.Xor, SKBlendMode.Plus,
            SKBlendMode.Modulate, SKBlendMode.Clear
        };

        Grid grid = new Grid
        {
            Margin = new Thickness(5)
        };

        for (int row = 0; row < 4; row++)
        {
            grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
            grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Star });
        }

        for (int col = 0; col < 3; col++)
        {
            grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });
        }

        for (int i = 0; i < blendModes.Length; i++)
        {
            SKBlendMode blendMode = blendModes[i];
            int row = 2 * (i / 4);
            int col = i % 4;

            Label label = new Label
            {
                Text = blendMode.ToString(),
                HorizontalTextAlignment = TextAlignment.Center
            };
            Grid.SetRow(label, row);
            Grid.SetColumn(label, col);
            grid.Children.Add(label);

            PorterDuffCanvasView canvasView = new PorterDuffCanvasView(blendMode);

            Grid.SetRow(canvasView, row + 1);
            Grid.SetColumn(canvasView, col);
            grid.Children.Add(canvasView);
        }

        Content = grid;
    }
}
```

結果は次のとおりです。

[![Porter-Duff グリッド](porter-duff-images/PorterDuffGrid.png "Porter-Duff グリッド")](porter-duff-images/PorterDuffGrid-Large.png#lightbox)

Porter の blend モードが適切に機能するためには、透明度が重要であることを理解しておく必要があります。 クラスには、 `PorterDuffCanvasView` メソッドへの合計3つの呼び出しが含まれてい `Canvas.Clear` ます。 すべてのピクセルを透明に設定するパラメーターなしのメソッドを使用します。

```csharp
canvas.Clear();
```

これらの呼び出しのいずれかを変更して、ピクセルが不透明な白に設定されるようにします。

```csharp
canvas.Clear(SKColors.White);
```

その後、一部の blend モードは動作しているように見えますが、それ以外は使用できません。 ソースビットマップの背景を白に設定した場合、 `SrcOver` 変換先が表示されないようにソースビットマップに透明なピクセルがないため、モードは機能しません。 コピー先のビットマップまたはキャンバスの背景を白に設定した場合、 `DstOver` 変換先には透明ピクセルがないため、は機能しません。

**Porter-Duff Grid**ページのビットマップを簡単な呼び出しで置き換えることが考えられ `DrawRect` ます。 コピー先の四角形に対しては機能しますが、ソースの四角形には使用できません。 コピー元の四角形には、bluish 色の領域だけを含める必要があります。 コピー元の四角形には、変換先の色分けされた領域に対応する透明領域を含める必要があります。 その後は、これらの blend モードが機能します。

## <a name="using-mattes-with-porter-duff"></a>Porter-Duff でのマットの使用

**レンガの合成**のページには、従来の複合タスクの例が示されています。画像は、削除する必要がある背景を持つビットマップを含め、いくつかの部分から構成する必要があります。 次に、 **SeatedMonkey.jpg** のビットマップと、問題のある背景を示します。

![取り付けられるサル](porter-duff-images/SeatedMonkey.jpg "取り付けられるサル")

合成の準備として、対応する _マット_ が作成されています。これは、イメージを表示したり透明にしたりするための黒の別のビットマップです。 このファイルには**SeatedMonkeyMatte.png**という名前が付けられ、 [**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの**メディア**フォルダーのリソースの中にあります。

![固定したサルのマット](porter-duff-images/SeatedMonkeyMatte.png "固定したサルのマット")

これは、expertly に作成されたマットでは _ありません_ 。 最適な場合、マットには黒いピクセルの端に部分的に透明なピクセルを含める必要があります。また、このマットには含まれません。

**ブリックの合成**ページの XAML ファイルは、とをインスタンス化します。このファイルは、 `SKCanvasView` 最終的な `Button` イメージを作成するプロセスをユーザーに案内します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BrickWallCompositingPage"
             Title="Brick-Wall Compositing">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Button Text="Show sitting monkey"
                HorizontalOptions="Center"
                Margin="0, 10"
                Clicked="OnButtonClicked" />

    </StackLayout>
</ContentPage>
```

分離コードファイルは、必要な2つのビットマップを読み込み、のイベントを処理し `Clicked` `Button` ます。 クリックするたびに、 `Button` `step` フィールドがインクリメントされ、に対して新しい `Text` プロパティが設定され `Button` ます。 `step`が5に達すると、0に戻ります。

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    SKBitmap monkeyBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BrickWallCompositingPage), 
        "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    SKBitmap matteBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BrickWallCompositingPage), 
        "SkiaSharpFormsDemos.Media.SeatedMonkeyMatte.png");

    int step = 0;

    public BrickWallCompositingPage ()
    {
        InitializeComponent ();
    }

    void OnButtonClicked(object sender, EventArgs args)
    {
        Button btn = (Button)sender;
        step = (step + 1) % 5;

        switch (step)
        {
            case 0: btn.Text = "Show sitting monkey"; break;
            case 1: btn.Text = "Draw matte with DstIn"; break;
            case 2: btn.Text = "Draw sidewalk with DstOver"; break;
            case 3: btn.Text = "Draw brick wall with DstOver"; break;
            case 4: btn.Text = "Reset"; break;
        }

        canvasView.InvalidateSurface();
    }
    
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        ···
    }
}
```

プログラムを初めて実行するときは、次の項目を除き、何も表示されません `Button` 。

[![ブリックの合成手順0](porter-duff-images/BrickWallCompositing0.png "ブリックの合成手順0")](porter-duff-images/BrickWallCompositing0-Large.png#lightbox)

を1回押すと、が `Button` `step` 1 にインクリメントし、 `PaintSurface` ハンドラーに **SeatedMonkey.jpg**が表示されるようになります。

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        float x = (info.Width - monkeyBitmap.Width) / 2;
        float y = info.Height - monkeyBitmap.Height;

        // Draw monkey bitmap
        if (step >= 1)
        {
            canvas.DrawBitmap(monkeyBitmap, x, y);
        }
        ···
    }
}
```

オブジェクトはない `SKPaint` ため、blend モードはありません。 ビットマップが画面の下部に表示されます。

[![ブリックの合成手順1](porter-duff-images/BrickWallCompositing1.png "ブリックの合成手順1")](porter-duff-images/BrickWallCompositing1-Large.png#lightbox)

をもう一度押し、を `Button` `step` 2 に増やします。 これは、 **SeatedMonkeyMatte.png** ファイルを表示するための重要な手順です。

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        // Draw matte to exclude monkey's surroundings
        if (step >= 2)
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.BlendMode = SKBlendMode.DstIn;
                canvas.DrawBitmap(matteBitmap, x, y, paint);
            }
        }
        ···
    }
}
```

Blend モードはです `SKBlendMode.DstIn` 。これは、変換先が、ソースの非透過的領域に対応する領域に保持されることを意味します。 元のビットマップに対応するターゲットの四角形の残りの部分は透明になります。

[![ブリックの合成手順2](porter-duff-images/BrickWallCompositing2.png "ブリックの合成手順2")](porter-duff-images/BrickWallCompositing2-Large.png#lightbox)

背景が削除されました。 

次の手順では、サルが座っている歩道に似た四角形を描画します。 この歩道の外観は、ソリッドカラーシェーダーと Perlin ノイズシェーダーの2つのシェーダーの構成に基づいています。

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        const float sidewalkHeight = 80;
        SKRect rect = new SKRect(info.Rect.Left, info.Rect.Bottom - sidewalkHeight,
                                 info.Rect.Right, info.Rect.Bottom);

        // Draw gravel sidewalk for monkey to sit on
        if (step >= 3)
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.Shader = SKShader.CreateCompose(
                                    SKShader.CreateColor(SKColors.SandyBrown),
                                    SKShader.CreatePerlinNoiseTurbulence(0.1f, 0.3f, 1, 9));

                paint.BlendMode = SKBlendMode.DstOver;
                canvas.DrawRect(rect, paint);
            }
        }
        ···
    }
}
```

この歩道は、サルの背後に配置する必要があるため、blend モードは `DstOver` です。 出力先は、背景が透明になっている場所でのみ表示されます。

[![ブリックの合成手順3](porter-duff-images/BrickWallCompositing3.png "ブリックの合成手順3")](porter-duff-images/BrickWallCompositing3-Large.png#lightbox)

最後の手順では、ブリックウォールを追加します。 このプログラムでは、クラスの静的プロパティとして使用できるブリック壁面ビットマップタイルを使用し `BrickWallTile` `AlgorithmicBrickWallPage` ます。 次のように、タイルをシフトするための呼び出しに変換変換が追加され、 `SKShader.CreateBitmap` 一番下の行が完全なタイルになります。

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        // Draw bitmap tiled brick wall behind monkey
        if (step >= 4)
        {
            using (SKPaint paint = new SKPaint())
            {
                SKBitmap bitmap = AlgorithmicBrickWallPage.BrickWallTile;
                float yAdjust = (info.Height - sidewalkHeight) % bitmap.Height;

                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat,
                                                     SKMatrix.MakeTranslation(0, yAdjust));
                paint.BlendMode = SKBlendMode.DstOver;
                canvas.DrawRect(info.Rect, paint);
            }
        }
    }
}
```

便宜上、呼び出しでは、 `DrawRect` キャンバス全体にこのシェーダーが表示され `DstOver` ますが、モードでは、出力は、透明なキャンバスの領域のみに制限されます。

[![ブリックの合成手順4](porter-duff-images/BrickWallCompositing4.png "ブリックの合成手順4")](porter-duff-images/BrickWallCompositing4-Large.png#lightbox)

当然ながら、このシーンを作成する方法は他にもあります。 バックグラウンドで構築し、フォアグラウンドに向かって進行することができます。 ただし、blend モードを使用すると、柔軟性が向上します。 特に、マットを使用すると、作成されたシーンからビットマップの背景を除外することができます。

記事「 [パスと領域の使用](../../curves/clipping.md)」で学習したように、 `SKCanvas` クラスは、、、およびの各メソッドに対応する3種類のクリッピングを定義し [`ClipRect`](xref:SkiaSharp.SKCanvas.ClipRect*) [`ClipPath`](xref:SkiaSharp.SKCanvas.ClipPath*) [`ClipRegion`](xref:SkiaSharp.SKCanvas.ClipRegion*) ます。 Porter blend モードでは、別の種類のクリッピングが追加されます。これにより、ビットマップなど、描画可能なあらゆるものに画像を制限できます。 **レンガの合成**で使用されるマットは、基本的にクリッピング領域を定義します。

## <a name="gradient-transparency-and-transitions"></a>グラデーションの透明度と遷移

この記事で前述した Porter の blend モードの例では、不透明ピクセルと透明ピクセルで、部分的に透明なピクセルではなく、すべてのイメージが関係しています。 Blend モード関数は、これらのピクセルにも定義されています。 次の表は、Skia [**Skblendmode のリファレンス**](https://skia.org/user/api/SkBlendMode_Reference)に記載されている表記法を使用する、Porter のブレンドモードのより正式な定義です。 ( **Skblendmode 参照** は skia 参照であるため、C++ 構文が使用されます)。

概念的には、各ピクセルの赤、緑、青、アルファの各要素が、0 ~ 1 の範囲の浮動小数点数に変換されます。 アルファチャネルの場合、0は完全に透明で、1は完全に不透明です。

次の表に示す表記法では、次の省略形が使用されています。

- 宛先のアルファチャネルである**Da**
- **Dc** は移動先の RGB 色です
- **Sa** はソースのアルファチャネルです
- **Sc** はソースの RGB 色です

RGB 色は、アルファ値で事前乗算されます。 たとえば、 **Sc** が純粋な赤を表し、 **Sa** が0X80 の場合、RGB 色は **(0x80, 0, 0)** になります。 **Sa**が0の場合は、すべての RGB コンポーネントも0になります。

結果は、アルファチャネルを含む角かっこと、コンマで区切られた RGB 色で示されます。 **[アルファ,、色]** です。 色の場合は、赤、緑、および青のコンポーネントに対して個別に計算が実行されます。

| モード       | 操作 |
| ---------- | --------- |
| `Clear`    | [0, 0]    |
| `Src`      | [Sa、Sc]  |
| `Dst`      | [Da、Dc]  |
| `SrcOver`  | [Sa + Da ·(1 – Sa)、Sc + Dc(1 – Sa) | 
| `DstOver`  | [Da + Sa(1 – Da)、Dc + Sc(1 ~ Da) |
| `SrcIn`    | SaDa、Scオーディオ |
| `DstIn`    | オーディオSa、Dc、Sa |
| `SrcOut`   | Sa(1 – Da)、Sc(1 – Da)] |
| `DstOut`   | オーディオ(1 – Sa)、Dc(1 – Sa)] |
| `SrcATop`  | [Da、Sc、Da + Dc ·(1 – Sa)] |
| `DstATop`  | [Sa、Dc]Sa + Sc ·(1 – Da)] |
| `Xor`      | [Sa + Da – 2]SaDa、Sc(1 – Da) + Dc ·(1 – Sa)] |
| `Plus`     | [Sa + Da、Sc + Dc] |
| `Modulate` | SaDa、Sc修飾 | 

これらの操作は、 **Da** と **Sa** が0または1の場合に分析しやすくなります。 たとえば、既定のモードでは、 `SrcOver` **Sa** が0の場合、 **Sc** も0になり、結果は **[Da, Dc]**、送信先のアルファと色になります。 **Sa**が1の場合、結果は **[sa, sc]**、ソースのアルファと色、または **[1, sc]** になります。

`Plus`との `Modulate` モードは、変換元と変換先の組み合わせによって新しい色が生成される可能性があるという、他のモードとは少し異なります。 モードは、 `Plus` バイトコンポーネントまたは浮動小数点コンポーネントで解釈できます。 前に示した **Porter-Duff グリッド** ページでは、変換先の色は **(0xC0、0x80、0x00)** で、ソースの色は **(0x00、0x80、0xC0)** です。 コンポーネントの各ペアが追加されますが、合計は0xFF で固定されます。 結果は、色 **(0xC0、0xff、0xC0)** になります。 これは、交差部分に表示される色です。

モードでは、 `Modulate` RGB 値を浮動小数点型に変換する必要があります。 コピー先の色は **(0.75、0.5、0)** で、ソースは **(0、0.5、0.75)** です。 RGB コンポーネントはそれぞれ乗算され、結果は **(0, 0.25, 0)** になります。 このモードでは、 **Porter-Duff グリッド** ページの交差部分に表示される色です。

**Porter の透明度**ページでは、部分的に透明なグラフィックオブジェクトに対して Porter の blend モードがどのように作用するかを調べることができます。 XAML ファイルには、Porter モードのが含まれてい `Picker` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.PorterDuffTransparencyPage"
             Title="Porter-Duff Transparency">

    <StackLayout>
        <skiaviews:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blendModePicker"
                Title="Blend Mode"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlendMode}">
                    <x:Static Member="skia:SKBlendMode.Clear" />
                    <x:Static Member="skia:SKBlendMode.Src" />
                    <x:Static Member="skia:SKBlendMode.Dst" />
                    <x:Static Member="skia:SKBlendMode.SrcOver" />
                    <x:Static Member="skia:SKBlendMode.DstOver" />
                    <x:Static Member="skia:SKBlendMode.SrcIn" />
                    <x:Static Member="skia:SKBlendMode.DstIn" />
                    <x:Static Member="skia:SKBlendMode.SrcOut" />
                    <x:Static Member="skia:SKBlendMode.DstOut" />
                    <x:Static Member="skia:SKBlendMode.SrcATop" />
                    <x:Static Member="skia:SKBlendMode.DstATop" />
                    <x:Static Member="skia:SKBlendMode.Xor" />
                    <x:Static Member="skia:SKBlendMode.Plus" />
                    <x:Static Member="skia:SKBlendMode.Modulate" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                3
            </Picker.SelectedIndex>
        </Picker>
    </StackLayout>
</ContentPage>
```

分離コードファイルは、線形グラデーションを使用して同じサイズの2つの四角形を塗りつぶします。 移動先のグラデーションは、右上から左下にあります。 Brownish は右上隅にありますが、中央に向かって透明にフェードが開始され、左下隅で透明になっています。 

コピー元の四角形には、左上から右下へのグラデーションがあります。 左上隅は bluish ですが、再び透明にフェードし、右下隅に透明になっています。 

```csharp
public partial class PorterDuffTransparencyPage : ContentPage
{
    public PorterDuffTransparencyPage()
    {
        InitializeComponent();
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

        // Make square display rectangle smaller than canvas
        float size = 0.9f * Math.Min(info.Width, info.Height);
        float x = (info.Width - size) / 2;
        float y = (info.Height - size) / 2;
        SKRect rect = new SKRect(x, y, x + size, y + size);

        using (SKPaint paint = new SKPaint())
        {
            // Draw destination
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Right, rect.Top),
                                new SKPoint(rect.Left, rect.Bottom),
                                new SKColor[] { new SKColor(0xC0, 0x80, 0x00),
                                                new SKColor(0xC0, 0x80, 0x00, 0) },
                                new float[] { 0.4f, 0.6f },
                                SKShaderTileMode.Clamp);

            canvas.DrawRect(rect, paint);

            // Draw source
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Left, rect.Top),
                                new SKPoint(rect.Right, rect.Bottom),
                                new SKColor[] { new SKColor(0x00, 0x80, 0xC0), 
                                                new SKColor(0x00, 0x80, 0xC0, 0) },
                                new float[] { 0.4f, 0.6f },
                                SKShaderTileMode.Clamp);

            // Get the blend mode from the picker
            paint.BlendMode = blendModePicker.SelectedIndex == -1 ? 0 :
                                    (SKBlendMode)blendModePicker.SelectedItem;

            canvas.DrawRect(rect, paint);

            // Stroke surrounding rectangle
            paint.Shader = null;
            paint.BlendMode = SKBlendMode.SrcOver;
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 3;
            canvas.DrawRect(rect, paint);
        }
    }
}
```

このプログラムは、Porter の blend モードが、ビットマップ以外のグラフィックオブジェクトで使用できることを示しています。 ただし、ソースには透明な領域が含まれている必要があります。 これは、グラデーションが四角形を塗りつぶすのに対して、グラデーションの一部が透明になるためです。

次の3つの例を示します。

[![Porter-Duff 透明度](porter-duff-images/PorterDuffTransparency.png "Porter-Duff 透明度")](porter-duff-images/PorterDuffTransparency-Large.png#lightbox)

コピー先とコピー元の構成は、元の Porter-Duff の [_デジタル画像_](https://graphics.pixar.com/library/Compositing/paper.pdf) 用紙のページ255に示されている図と非常によく似ていますが、このページでは、blend モードが部分的な透明度の領域に対して適切に動作していることを示しています。

透明なグラデーションは、いくつかの異なる効果に使用できます。 マスクは、 **SkiaSharp の丸いグラデーションページ**の [[**マスクの放射状グラデーション**](../shaders/circular-gradients.md#radial-gradients-for-masking)] セクションに示されている方法と似ています。 [ **合成マスク** ] ページの多くは、その前のプログラムに似ています。 ビットマップリソースを読み込み、それを表示する四角形を決定します。 放射状グラデーションは、事前に決定された中心と半径に基づいて作成されます。

```csharp
public class CompositingMaskPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(CompositingMaskPage),
        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    static readonly SKPoint CENTER = new SKPoint(180, 300);
    static readonly float RADIUS = 120;

    public CompositingMaskPage ()
    {
        Title = "Compositing Mask";

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

        // Find rectangle to display bitmap
        float scale = Math.Min((float)info.Width / bitmap.Width,
                               (float)info.Height / bitmap.Height);

        SKRect rect = SKRect.Create(scale * bitmap.Width, scale * bitmap.Height);

        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Display bitmap in rectangle
        canvas.DrawBitmap(bitmap, rect);

        // Adjust center and radius for scaled and offset bitmap
        SKPoint center = new SKPoint(scale * CENTER.X + x,
                                        scale * CENTER.Y + y);
        float radius = scale * RADIUS;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                center,
                                radius,
                                new SKColor[] { SKColors.Black,
                                                SKColors.Transparent },
                                new float[] { 0.6f, 1 },
                                SKShaderTileMode.Clamp);

            paint.BlendMode = SKBlendMode.DstIn;

            // Display rectangle using that gradient and blend mode
            canvas.DrawRect(rect, paint);
        }

        canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
    }
}
```

このプログラムとの違いは、グラデーションは中央の黒で始まり、透明度で終了することです。 これは、の blend モードでビットマップに表示されます。このモードでは、 `DstIn` 透明でないソースの領域にのみターゲットが表示されます。

呼び出しの後 `DrawRect` 、キャンバスの表面全体が透明になります。ただし、放射状グラデーションで定義されている円は除きます。 最後の呼び出しが行われます。

```csharp
canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
```

キャンバスの透明な領域はすべてピンク色で色分けされています。

[![マスクの合成](porter-duff-images/CompositingMask.png "マスクの合成")](porter-duff-images/CompositingMask-Large.png#lightbox)

また、Porter モードと部分的に透明なグラデーションを使用して、あるイメージから別のイメージへの遷移を行うこともできます。 [ **グラデーションの切り替え** ] ページには、 `Slider` 0 から1への移行の進行状況レベルを示すが含まれています。また、を使用して、 `Picker` 必要な移行の種類を選択します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.GradientTransitionsPage"
             Title="Gradient Transitions">
    
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="progressSlider"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference progressSlider},
                              Path=Value,
                              StringFormat='Progress = {0:F2}'}"
               HorizontalTextAlignment="Center" />

        <Picker x:Name="transitionPicker" 
                Title="Transition" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />
        
    </StackLayout>
</ContentPage>
```

分離コードファイルは、移行を示すために2つのビットマップリソースを読み込みます。 これらは、この記事の前の「 **ビットマップのディゾルブ** 」ページで使用されているのと同じ2つのイメージです。 このコードは、 &mdash; 線形、放射状、スイープという3種類のグラデーションに対応する3つのメンバーを持つ列挙体も定義します。 これらの値は、に読み込まれ `Picker` ます。

```csharp
public partial class GradientTransitionsPage : ContentPage
{
    SKBitmap bitmap1 = BitmapExtensions.LoadBitmapResource(
        typeof(GradientTransitionsPage),
        "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    SKBitmap bitmap2 = BitmapExtensions.LoadBitmapResource(
        typeof(GradientTransitionsPage),
        "SkiaSharpFormsDemos.Media.FacePalm.jpg");

    enum TransitionMode
    {
        Linear,
        Radial,
        Sweep
    };

    public GradientTransitionsPage ()
    {
        InitializeComponent ();

        foreach (TransitionMode mode in Enum.GetValues(typeof(TransitionMode)))
        {
            transitionPicker.Items.Add(mode.ToString());
        }

        transitionPicker.SelectedIndex = 0;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }
    ···
}
```

分離コードファイルは、3つのオブジェクトを作成し `SKPaint` ます。 `paint0`オブジェクトは blend モードを使用しません。 この描画オブジェクトを使用して、配列に示されているように、黒色から透明になるグラデーションを持つ四角形を描画し `colors` ます。 `positions`配列はの位置に基づいてい `Slider` ますが、多少調整されています。 `Slider`が最小値または最大値の場合、 `progress` 値は0または1になり、2つのビットマップのいずれかが完全に表示されます。 配列は、 `positions` これらの値に応じて設定する必要があります。

`progress`値が0の場合、配列には `positions` 値-0.1 と0が格納されます。 SkiaSharp は、最初の値が0になるように調整します。これは、グラデーションが0の場合にのみ黒で、それ以外の場合は透明であることを意味します。 `progress`が0.5 の場合、配列には0.45 と0.55 の値が格納されます。 グラデーションは、0 ~ 0.45 の黒色で、透明に遷移し、0.55 から1まで完全に透明になっています。 `progress`が1の場合、 `positions` 配列は1と1.1 で、グラデーションが 0 ~ 1 であることを意味します。

`colors`と `position` の配列は両方とも、 `SKShader` グラデーションを作成するの3つのメソッドで使用されます。 選択内容に基づいて、次のいずれかのシェーダーのみが作成され `Picker` ます。 

```csharp
public partial class GradientTransitionsPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Assume both bitmaps are square for display rectangle
        float size = Math.Min(info.Width, info.Height);
        SKRect rect = SKRect.Create(size, size);
        float x = (info.Width - size) / 2;
        float y = (info.Height - size) / 2;
        rect.Offset(x, y);

        using (SKPaint paint0 = new SKPaint())
        using (SKPaint paint1 = new SKPaint())
        using (SKPaint paint2 = new SKPaint())
        {
            SKColor[] colors = new SKColor[] { SKColors.Black,
                                               SKColors.Transparent };

            float progress = (float)progressSlider.Value;

            float[] positions = new float[]{ 1.1f * progress - 0.1f,
                                             1.1f * progress };

            switch ((TransitionMode)transitionPicker.SelectedIndex)
            {
                case TransitionMode.Linear:
                    paint0.Shader = SKShader.CreateLinearGradient(
                                        new SKPoint(rect.Left, 0),
                                        new SKPoint(rect.Right, 0),
                                        colors,
                                        positions,
                                        SKShaderTileMode.Clamp);
                    break;

                case TransitionMode.Radial:
                    paint0.Shader = SKShader.CreateRadialGradient(
                                        new SKPoint(rect.MidX, rect.MidY),
                                        (float)Math.Sqrt(Math.Pow(rect.Width / 2, 2) +
                                                         Math.Pow(rect.Height / 2, 2)),
                                        colors,
                                        positions,
                                        SKShaderTileMode.Clamp);
                    break;

                case TransitionMode.Sweep:
                    paint0.Shader = SKShader.CreateSweepGradient(
                                        new SKPoint(rect.MidX, rect.MidY),
                                        colors,
                                        positions);
                    break;
            }

            canvas.DrawRect(rect, paint0);

            paint1.BlendMode = SKBlendMode.SrcOut;
            canvas.DrawBitmap(bitmap1, rect, paint1);

            paint2.BlendMode = SKBlendMode.DstOver;
            canvas.DrawBitmap(bitmap2, rect, paint2);
        }
    }
}
```

このグラデーションは、blend モードなしで四角形に表示されます。 この呼び出しの後、キャンバスには、 `DrawRect` 黒から透明のグラデーションが単純に含まれています。 黒の値が大きいほど、値が大きくなり `Slider` ます。

ハンドラーの最後の4つのステートメントでは、 `PaintSurface` 2 つのビットマップが表示されます。 `SrcOut`Blend モードは、最初のビットマップが背景の透明な領域にのみ表示されることを意味します。 `DstOver`2 番目のビットマップのモードは、最初のビットマップが表示されていない領域にのみ、2番目のビットマップが表示されることを意味します。

次のスクリーンショットは、3つの異なる遷移の種類を示しています。それぞれが50% マークになっています。

[![グラデーションの遷移](porter-duff-images/GradientTransitions.png "グラデーションの遷移")](porter-duff-images/GradientTransitions-Large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)