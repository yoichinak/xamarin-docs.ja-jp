---
title: Porter Duff ブレンド モード
description: Porter Duff blend モードを使用すると、ソースと宛先のイメージに基づくシーンを作成できます。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 57F172F8-BA03-43EC-A215-ED6B78696BB5
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: c15dd4606a75cc3cdffbad71f15299568157213a
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306513"
---
# <a name="porter-duff-blend-modes"></a>Porter Duff ブレンド モード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Thomas Porter と Tom Duff、Lucasfilm に作業している複合の代数を開発した後、Porter Duff blend モードと呼びます。 これらの紙[_合成デジタル画像_](https://graphics.pixar.com/library/Compositing/paper.pdf)は、1984年7月の_コンピューターグラフィックス_の問題である253から259に発行されました。 これらの描画モード複合シーンにさまざまなイメージまとめての複合に不可欠です。

![Porter-Duff サンプル](porter-duff-images/PorterDuffSample.png "Porter-Duff サンプル")

## <a name="porter-duff-concepts"></a>Porter Duff 概念

茶色がかった四角形の左と上の 2 つの 3 分の表示画面を占有するとします。

![Porter-Duff Destination](porter-duff-images/PorterDuffDst.png "Porter-Duff Destination")

この領域は、_変換先_、または_背景_また_は_背景と呼ばれます。

変換先の同じサイズは、次の四角形を描画するには。 四角形が右や下、2/3 以上を占めている青み領域を除く transparent:

![Porter-Duff ソース](porter-duff-images/PorterDuffSrc.png "Porter-Duff ソース")

これは、_ソース_または_フォアグラウンド_と呼ばれます。

コピー先で、ソースを表示すると次期待どおりに示します。

![Porter-Duff ソース](porter-duff-images/PorterDuffSrcOver.png "Porter-Duff ソース")

ソースの透明なピクセルを使用すると、背景が透けて青みソース ピクセルには、バック グラウンドがわかりにくくなる中。 これは通常のケースであり、`SKBlendMode.SrcOver`として SkiaSharp で参照されます。 この値は、`SKPaint` オブジェクトが最初にインスタンス化されるときの `BlendMode` プロパティの既定の設定です。

ただし、別の効果のさまざまなブレンド モードを指定することはできます。 `SKBlendMode.DstOver`を指定した場合、コピー元とコピー先が交差する領域では、変換先がソースの代わりに表示されます。

![Porter-Duff 転送先](porter-duff-images/PorterDuffDstOver.png "Porter-Duff 転送先")

`SKBlendMode.DstIn` blend モードでは、ターゲットの色を使用してターゲットとソースの交差部分のみが表示されます。

![Porter-Duff の宛先](porter-duff-images/PorterDuffDstIn.png "Porter-Duff の宛先")

Blend モードの `SKBlendMode.Xor` (排他または) では、2つの領域が重なっている場所が何も表示されません。

![Porter-Duff 排他的または](porter-duff-images/PorterDuffXor.png "Porter-Duff 排他的または")

効果的に色付きの送信先と送信元の四角形が、表示画面を送信先と送信元の四角形の存在に対応するさまざまな方法で色を指定できる 4 つの一意の領域に分割します。

![Porter-Duff](porter-duff-images/PorterDuff.png "Porter Duff")

右上および左下の四角形は、ソースと宛先の両方が透明な領域にあるために常に空白です。 領域は、コピー先の色、またはまったくないか、色できるように、コピー先の色は、左上隅の領域を占有します。 同様に、領域は、元の色またはまったく色できるように、元の色は、右下の領域を占有します。 コピー先の色、元の色、またはすべてではなく、中央のソースの保存先との積集合を色付けすることができます。

組み合わせの総数は (左) 用の 2 を使用 (中央)、3、または 12 回 (右下) の 2 回です。 これらは、12 の基本的な Porter Duff 複合モードです。

_複合デジタルイメージ_(ページ 256)、Porter、および duff の終わりに向かって、_プラス_(SkiaSharp `SKBlendMode.Plus` メンバーに対応する13番目のモードと W3C_薄_モード (w3c の_明るく_モードと混同しない) を追加します。この `Plus` モードでは、出力先とコピー元の色が追加されます。このプロセスの詳細については、後で詳しく説明します。

Skia は、コピー先とコピー元の色が乗算される点を除いて、`Plus` と非常によく似た `Modulate` と呼ばれる14のモードを追加します。 これは、追加 Porter Duff blend モードとして処理できます。

ここでは、14 Porter Duff モード SkiaSharp で定義されています。 表には、上記の図の 3 つの空白領域の各色方法を示します。

| モード       | [Destination] | 積集合 | ソース |
| ---------- |:-----------:|:------------:|:------:|
| `Clear`    |             |              |        |
| `Src`      |             | ソース       | X      |
| `Dst`      | X           | [Destination]  |        |
| `SrcOver`  | X           | ソース       | X      |
| `DstOver`  | X           | [Destination]  | X      |
| `SrcIn`    |             | ソース       |        |
| `DstIn`    |             | [Destination]  |        |
| `SrcOut`   |             |              | X      |
| `DstOut`   | X           |              |        |
| `SrcATop`  | X           | ソース       |        |
| `DstATop`  |             | [Destination]  | X      |
| `Xor`      | X           |              | X      |
| `Plus`     | X           | Sum          | X      |
| `Modulate` |             | Product      |        | 

これらの描画モードは対称にします。 ソースおよびデスティネーションを交換して、すべてのモードは引き続き使用できます。

モードの名前付け規則では、いくつかの単純な規則に従います。

- **Src**または**Dst**自体は、変換元または変換先のピクセルだけが表示されることを意味します。
- サフィックス (**上**) の値は、交差部分に表示される内容を示します。 「で」他のソースまたは宛先が描画されます。
- **In**サフィックスは、交差部分だけが色分けされていることを意味します。 出力は、変換元または転送先として"in"、他の部分のみに限定されます。
- **出力**サフィックスは、交差部分に色が付いていないことを意味します。 出力は、変換元または転送先として、積集合の"out"の部分のみです。
- **一番**上のサフィックスは、 **In**と**Out**の和集合です。これには、転送元または転送先が "一番上" である領域が含まれます。

`Plus` モードと `Modulate` モードとの違いに注意してください。 これらのモードでは、ソースと宛先のピクセルに別の種類の計算を実行しています。 まもなく詳細に記述されています。

**Porter-Duff グリッド**ページでは、1つの画面に表示される14のすべてのモードがグリッド形式で表示されます。 各モードは、`SKCanvasView`の個別のインスタンスです。 そのため、クラスは `PorterDuffCanvasView`という名前の `SKCanvasView` から派生します。 静的コンス トラクターは、左上隅にある茶色がかった四角形を持つ 1 つ、もう青み四角形は、同じサイズの 2 つのビットマップを作成します。

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

インスタンスコンストラクターには `SKBlendMode`型のパラメーターがあります。 フィールドに、このパラメーターを保存します。 

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

`OnPaintSurface` のオーバーライドは、2つのビットマップを描画します。 最初は通常どおり描画します。

```csharp
canvas.DrawBitmap(dstBitmap, rect);
```

2番目のは、`BlendMode` プロパティがコンストラクター引数に設定されている `SKPaint` オブジェクトを使用して描画されます。

```csharp
using (SKPaint paint = new SKPaint())
{
    paint.BlendMode = blendMode;
    canvas.DrawBitmap(srcBitmap, rect, paint);
}
```

`OnPaintSurface` override の残りの部分では、ビットマップの周囲に四角形を描画してサイズを示します。

`PorterDuffGridPage` クラスは、`blendModes` 配列のメンバーごとに1つずつ、`PorterDurffCanvasView`の14個のインスタンスを作成します。 配列内の `SKBlendModes` メンバーの順序は、互いに隣接する同様のモードを配置するためにテーブルと少し異なります。 `PorterDuffCanvasView` の14個のインスタンスは、`Grid`内のラベルと共に編成されます。

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

透過性が Porter Duff blend モードが正しく機能するために重要であると自分でします。 `PorterDuffCanvasView` クラスには、`Canvas.Clear` メソッドへの合計3つの呼び出しが含まれています。 それらのすべては、パラメーターなしのメソッドは、すべてのピクセルを透明に設定を使用します。

```csharp
canvas.Clear();
```

ピクセルが不透明な白色に設定されるようには、これらの呼び出しのいずれかを変更してみてください。

```csharp
canvas.Clear(SKColors.White);
```

次の変更をするには、一部の blend モードと思われるが、他のユーザーは表示されません。 ソースビットマップの背景を白に設定した場合、`SrcOver` モードは動作しません。これは、ソースビットマップに透明なピクセルがないためです。 コピー先のビットマップまたはキャンバスの背景を白に設定した場合、`DstOver` は動作しません。これは、変換先に透明ピクセルがないためです。

**Porter-Duff グリッド**ページのビットマップを、単純な `DrawRect` 呼び出しで置き換えることが考えられます。 先の四角形の元の四角形ではなくを機能します。 元の四角形には、単青み色分けエリアが含まれる必要があります。 元の四角形は、変換先の色付きの領域に対応する透明な領域を含める必要があります。 モードの作業をしてから、これらはブレンドします。

## <a name="using-mattes-with-porter-duff"></a>Porter Duff でマットを使用します。

**レンガの合成**のページには、従来の複合タスクの例が示されています。画像は、削除する必要がある背景を持つビットマップを含め、いくつかの部分から構成する必要があります。 次の例は、問題のある背景を持つ、 **Seatedmonkey .jpg**ビットマップです。

![取り付けられるサル](porter-duff-images/SeatedMonkey.jpg "取り付けられるサル")

合成の準備として、対応する_マット_が作成されています。これは、イメージを表示したり透明にしたりするための黒の別のビットマップです。 このファイルの名前は、 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの**メディア**フォルダー内のリソースの中にあり**ます。**

![固定したサルのマット](porter-duff-images/SeatedMonkeyMatte.png "固定したサルのマット")

これは、expertly に作成されたマットでは_ありません_。 最適な状態マットが黒のピクセルの端に部分的に透明なピクセルを含める必要があり、このマットはありません。

**ブリックの合成**ページの XAML ファイルは、`SKCanvasView` と、最終的なイメージを作成するプロセスをユーザーに指示する `Button` をインスタンス化します。

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

分離コードファイルは、必要な2つのビットマップを読み込み、`Button`の `Clicked` イベントを処理します。 `Button` クリックするたびに、[`step`] フィールドがインクリメントされ、`Button`に新しい `Text` プロパティが設定されます。 `step` が5に達すると、0に戻ります。

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

プログラムを初めて実行する場合、`Button`を除き、何も表示されません。

[![ブリックの合成手順0](porter-duff-images/BrickWallCompositing0.png "ブリックの合成手順0")](porter-duff-images/BrickWallCompositing0-Large.png#lightbox)

`Button` を1回押すと、`step` が1に増加し、`PaintSurface` ハンドラーによって**Seatedmonkey .jpg**が表示されるようになります。

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

`SKPaint` オブジェクトはないため、blend モードはありません。 ビットマップは、画面の下部に表示されます。

[![ブリックの合成手順1](porter-duff-images/BrickWallCompositing1.png "ブリックの合成手順1")](porter-duff-images/BrickWallCompositing1-Large.png#lightbox)

もう一度 `Button` を押し、`step` を2に増やします。 これは、 **Seatedmonkeymatte**ファイルを表示するための重要な手順です。

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

Blend モードは `SKBlendMode.DstIn`です。これは、変換先が、ソースの非透過的領域に対応する領域に保持されることを意味します。 元のビットマップに対応する先の四角形の残りの部分は透明になります。

[![ブリックの合成手順2](porter-duff-images/BrickWallCompositing2.png "ブリックの合成手順2")](porter-duff-images/BrickWallCompositing2-Large.png#lightbox)

バック グラウンドが削除されました。 

次の手順では、monkey を置いている歩道のような四角形を描画します。 この歩道の外観が 2 つのシェーダーのコンポジションに基づいて: 単色シェーダーとパーリン ノイズ シェーダー。

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

この歩道は、サルを支える必要があるため、blend モードは `DstOver`です。 コピー先では、背景が透明な場所のみが表示されます。

[![ブリックの合成手順3](porter-duff-images/BrickWallCompositing3.png "ブリックの合成手順3")](porter-duff-images/BrickWallCompositing3-Large.png#lightbox)

最後の手順では、レンガの壁を追加します。 このプログラムでは、`AlgorithmicBrickWallPage` クラスの静的プロパティ `BrickWallTile` として使用できるブリック壁面ビットマップタイルを使用します。 `SKShader.CreateBitmap` の呼び出しに変換変換が追加され、一番下の行が完全なタイルになるようにタイルをシフトします。

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

便宜上、`DrawRect` の呼び出しでは、キャンバス全体にこのシェーダーが表示されますが、`DstOver` モードでは、出力は、透明なキャンバスの領域のみに制限されます。

[![ブリックの合成手順4](porter-duff-images/BrickWallCompositing4.png "ブリックの合成手順4")](porter-duff-images/BrickWallCompositing4-Large.png#lightbox)

明らかには、このシーンを作成するには、その他の方法があります。 これは、バック グラウンドで開始すると、フォア グラウンドに進行して構築できます。 柔軟性の方が blend モードを使用します。 具体的には、マットの使用には、構成済みのシーンから除外するビットマップの背景ができます。

記事「[パスと領域の使用](../../curves/clipping.md)」で学習したように、`SKCanvas` クラスは、 [`ClipRect`](xref:SkiaSharp.SKCanvas.ClipRect*)、 [`ClipPath`](xref:SkiaSharp.SKCanvas.ClipPath*)、および[`ClipRegion`](xref:SkiaSharp.SKCanvas.ClipRegion*)の各メソッドに対応する3種類のクリッピングを定義します。 Porter Duff blend モードでは、別の種類のビットマップを含む、描画できるものに画像を制限することにより、クリップを追加します。 **レンガの合成**で使用されるマットは、基本的にクリッピング領域を定義します。

## <a name="gradient-transparency-and-transitions"></a>グラデーションの透過性と遷移

Porter Duff の例では、この記事ではすべてありませんは、不透明なピクセルと透過的な (ピクセル単位) がない、部分的に透明なピクセルのイメージを前述のモードをブレンドします。 Blend モード関数は、これらのピクセルにも定義されます。 次の表は、Skia [**Skblendmode のリファレンス**](https://skia.org/user/api/SkBlendMode_Reference)に記載されている表記法を使用する、Porter のブレンドモードのより正式な定義です。 ( **Skblendmode 参照**は skia 参照なのでC++ 、構文が使用されます)。

概念的には、各ピクセルの赤、緑、青、およびアルファ コンポーネントは、0 ~ 1 の範囲の浮動小数点数にバイトから変換されます。 アルファ チャネルの 0 は完全に透明、1 は完全に不透明です

次の表に、表記法では、次の省略形を使用します。

- 宛先のアルファチャネルである**Da**
- **Dc**は移動先の RGB 色です
- **Sa**はソースのアルファチャネルです
- **Sc**はソースの RGB 色です

RGB 色はアルファ値で乗算済み。 たとえば、 **Sc**が純粋な赤を表し、 **Sa**が0X80 の場合、RGB 色は **(0x80, 0, 0)** になります。 **Sa**が0の場合は、すべての RGB コンポーネントも0になります。

結果は、アルファチャネルを含む角かっこと、コンマで区切られた RGB 色で示されます。 **[アルファ,、色]** です。 色の計算は赤、緑、青のコンポーネントを個別に実行します。

| モード       | 操作 |
| ---------- | --------- |
| `Clear`    | [0, 0]    |
| `Src`      | [Sa、Sc]  |
| `Dst`      | [Da、Dc]  |
| `SrcOver`  | [Sa + Da·(1 日 ~ Sa)、Sc + Dc·(1 – Sa) | 
| `DstOver`  | [Da + Sa·(1 日 ~ Da) Dc + Sc·(1 – Da) |
| `SrcIn`    | [Sa·Da、Sc·Da] |
| `DstIn`    | [Da·Sa、Dc·Sa] |
| `SrcOut`   | [Sa·(1 日 ~ Da) Sc·(1 日 ~ Da)] |
| `DstOut`   | [Da·(1 日 ~ Sa)、Dc·(1 日 ~ Sa)] |
| `SrcATop`  | [Da、Sc·Da + Dc·(1 日 ~ Sa)] |
| `DstATop`  | [Sa、Dc·Sa + Sc·(1 日 ~ Da)] |
| `Xor`      | [Sa + Da – 2·Sa·Da、Sc·(1 日 ~ Da) + Dc·(1 日 ~ Sa)] |
| `Plus`     | [Sa + Da、Sc + Dc] |
| `Modulate` | [Sa·Da、Sc·Dc] | 

これらの操作は、 **Da**と**Sa**が0または1の場合に分析しやすくなります。 たとえば、既定の `SrcOver` モードでは、 **Sa**が0の場合、 **Sc**も0になり、結果は **[Da, Dc]** 、送信先のアルファと色になります。 **Sa**が1の場合、結果は **[sa, sc]** 、ソースのアルファと色、または **[1, sc]** になります。

`Plus` モードと `Modulate` モードは、変換元と変換先の組み合わせによって新しい色が生成される可能性があるため、他のモードとは少し異なります。 `Plus` モードは、バイトコンポーネントまたは浮動小数点コンポーネントのいずれかで解釈できます。 前に示した**Porter-Duff グリッド**ページでは、変換先の色は **(0xC0、0x80、0x00)** で、ソースの色は **(0x00、0x80、0xC0)** です。 コンポーネントの各ペアが追加されますが、合計は 0 xff で固定されます。 結果は、色 **(0xC0、0xff、0xC0)** になります。 交差部分に示すように色です。

`Modulate` モードでは、RGB 値を浮動小数点型に変換する必要があります。 コピー先の色は **(0.75、0.5、0)** で、ソースは **(0、0.5、0.75)** です。 RGB コンポーネントはそれぞれ乗算され、結果は **(0, 0.25, 0)** になります。 このモードでは、 **Porter-Duff グリッド**ページの交差部分に表示される色です。

**Porter の透明度**ページでは、部分的に透明なグラフィックオブジェクトに対して Porter の blend モードがどのように作用するかを調べることができます。 XAML ファイルには、Porter モードの `Picker` が含まれています。

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

分離コード ファイルは、線形グラデーションを使用して、同じサイズの 2 つの四角形を塗りつぶします。 変換先のグラデーションは、上から右左下にあるからは。 右上隅にある茶色がかったが中心に向かって、透過的にフェーディングを開始し、左上隅にあるに対して透過的です。 

元の四角形は、左から右下へのグラデーションが。 左上隅は青みが、もう一度透明で、フェードインし、右上隅にある透過的です。 

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

このプログラムは、以外のビットマップ グラフィック オブジェクトを Porter Duff blend モードを使用できることを示しています。 ただし、ソースでは、透明な領域を含める必要があります。 これは、グラデーションは、四角形を塗りつぶしますがグラデーションの一部が透過的なので、ここです。

3 つの例を次に示します。

[![Porter-Duff 透明度](porter-duff-images/PorterDuffTransparency.png "Porter-Duff 透明度")](porter-duff-images/PorterDuffTransparency-Large.png#lightbox)

コピー先とコピー元の構成は、元の Porter-Duff の[_デジタル画像_](https://graphics.pixar.com/library/Compositing/paper.pdf)用紙のページ255に示されている図と非常によく似ていますが、このページでは、blend モードが部分的な透明度の領域に対して適切に動作していることを示しています。

さまざまな効果をいくつかの透明なグラデーションを使用することができます。 マスクは、 **SkiaSharp の丸いグラデーションページ**の [[**マスクの放射状グラデーション**](../shaders/circular-gradients.md#radial-gradients-for-masking)] セクションに示されている方法と似ています。 **[合成マスク]** ページの多くは、その前のプログラムに似ています。 ビットマップ リソースを読み込むし、それを表示する四角形を決定します。 放射状グラデーションは、事前に決められた center および radius に基づいて作成されます。

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

プログラムとの違いは、グラデーションが中心で色が黒で始まるし、透過性で終了です。 これは、`DstIn`の blend モードでビットマップに表示されます。このモードでは、透明でないソースの領域にのみターゲットが表示されます。

`DrawRect` の呼び出しの後、キャンバスの表面全体が透明になります。ただし、放射状グラデーションで定義されている円は除きます。 最終呼び出しが行われます。

```csharp
canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
```

キャンバスのすべての透明な領域はピンク色。

[![マスクの合成](porter-duff-images/CompositingMask.png "マスクの合成")](porter-duff-images/CompositingMask-Large.png#lightbox)

1 つのイメージを別の遷移の Porter Duff モードと部分的に透明なグラデーションを使用することもできます。 **[グラデーションの切り替え]** ページには、0から1への移行の進行レベルを示す `Slider` と、必要な移行の種類を選択するための `Picker` が含まれています。

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

分離コード ファイルは、移行を示すために 2 つのビットマップ リソースを読み込みます。 これらは、この記事の前の「**ビットマップのディゾルブ**」ページで使用されているのと同じ2つのイメージです。 また、このコードでは、線形、放射状、スイープ &mdash; 3 種類のグラデーションに対応する3つのメンバーを持つ列挙も定義します。 これらの値は、`Picker`に読み込まれます。

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

分離コードファイルは、3つの `SKPaint` オブジェクトを作成します。 `paint0` オブジェクトは blend モードを使用しません。 この描画オブジェクトを使用して、`colors` 配列に示されているように、黒から透明になるグラデーションを持つ四角形を描画します。 `positions` 配列は、`Slider`の位置に基づいていますが、多少調整されています。 `Slider` の値が最小値または最大値の場合、`progress` 値は0または1になり、2つのビットマップのいずれかが完全に表示されます。 `positions` 配列は、これらの値に応じて設定する必要があります。

`progress` 値が0の場合、`positions` 配列には-0.1 と0の値が含まれます。 SkiaSharp では、それ以外の場合、グラデーションが 0 でのみ黒であることを意味する 0 かつ透過的にその最初の値が調整されます。 `progress` が0.5 の場合、配列には0.45 と0.55 の値が格納されます。 グラデーションは 0.45 には、0 から黒、透過的に移行し、0.55 から完全に透明を 1 にします。 `progress` が1の場合、`positions` 配列は1と1.1 であり、グラデーションが 0 ~ 1 であることを意味します。

`colors` と `position` の配列は両方とも、グラデーションを作成する `SKShader` の3つの方法で使用されます。 `Picker` の選択に基づいて、次のいずれかのシェーダーのみが作成されます。 

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

そのグラデーションが blend モードを使用せず、四角形で表示されます。 この `DrawRect` を呼び出すと、キャンバスには黒から透明のグラデーションが単純に含まれます。 `Slider` 値が大きいほど、黒の量が増加します。

`PaintSurface` ハンドラーの最後の4つのステートメントでは、2つのビットマップが表示されます。 Blend モード `SrcOut` は、最初のビットマップが背景の透明な領域にのみ表示されることを意味します。 2番目のビットマップの `DstOver` モードは、最初のビットマップが表示されていない領域にのみ2番目のビットマップが表示されることを意味します。

次のスクリーン ショットでは、それぞれ 50% のマークで 3 つの異なる遷移型を示します。

[![グラデーションの遷移](porter-duff-images/GradientTransitions.png "グラデーションの遷移")](porter-duff-images/GradientTransitions-Large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
