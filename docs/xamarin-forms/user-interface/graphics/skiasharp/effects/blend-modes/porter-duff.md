---
title: Porter Duff ブレンド モード
description: Porter Duff blend モードを使用すると、ソースと宛先のイメージに基づくシーンを作成できます。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 57F172F8-BA03-43EC-A215-ED6B78696BB5
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: ebd4db28b2c20bd2b9e1d93e03dd101ebc5da663
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131682"
---
# <a name="porter-duff-blend-modes"></a>Porter Duff ブレンド モード

Thomas Porter と Tom Duff、Lucasfilm に作業している複合の代数を開発した後、Porter Duff blend モードと呼びます。 その用紙[_合成デジタル画像_](https://graphics.pixar.com/library/Compositing/paper.pdf) 1984 年 7 月号の発行された_コンピューター_グラフィックス_253 を 259 ページします。 これらの描画モード複合シーンにさまざまなイメージまとめての複合に不可欠です。

![Porter Duff サンプル](porter-duff-images/PorterDuffSample.png "Porter Duff サンプル")

## <a name="porter-duff-concepts"></a>Porter Duff 概念

茶色がかった四角形の左と上の 2 つの 3 分の表示画面を占有するとします。

![Porter Duff 先](porter-duff-images/PorterDuffDst.png "Porter Duff 変換先")

この領域が呼び出されます、_先_されたり、_バック グラウンド_または_背景_。

変換先の同じサイズは、次の四角形を描画するには。 四角形が右や下、2/3 以上を占めている青み領域を除く transparent:

![Porter Duff ソース](porter-duff-images/PorterDuffSrc.png "Porter Duff ソース")

これと呼ばれますが、_ソース_されたり、_フォア グラウンド_。

コピー先で、ソースを表示すると次期待どおりに示します。

![Porter Duff ソース オーバー](porter-duff-images/PorterDuffSrcOver.png "Porter Duff ソース オーバー")

ソースの透明なピクセルを使用すると、背景が透けて青みソース ピクセルには、バック グラウンドがわかりにくくなる中。 通常の場合は、として SkiaSharp で呼ばれることと`SKBlendMode.SrcOver`します。 既定の設定には、`BlendMode`プロパティと、`SKPaint`オブジェクトが最初にインスタンス化します。

ただし、別の効果のさまざまなブレンド モードを指定することはできます。 指定した場合`SKBlendMode.DstOver`、ソースではなく、ソースと宛先が交差する領域で、変換先が表示されます。

![経由で Porter Duff 先](porter-duff-images/PorterDuffDstOver.png "経由で Porter Duff 変換先")

`SKBlendMode.DstIn` Blend モードには、ソースと変換先が交差するコピー先の色を使用して、領域のみが表示されます。

![Porter Duff 先](porter-duff-images/PorterDuffDstIn.png "Porter Duff 先")

ブレンド モードの`SKBlendMode.Xor`(排他的 OR) と 2 つの領域が重複して表示するもの。

![Porter Duff 排他または](porter-duff-images/PorterDuffXor.png "Porter Duff 排他または")

効果的に色付きの送信先と送信元の四角形が、表示画面を送信先と送信元の四角形の存在に対応するさまざまな方法で色を指定できる 4 つの一意の領域に分割します。

![Porter Duff](porter-duff-images/PorterDuff.png "Porter Duff")

右上および左下の四角形は、ソースと宛先の両方が透明な領域にあるために常に空白です。 領域は、コピー先の色、またはまったくないか、色できるように、コピー先の色は、左上隅の領域を占有します。 同様に、領域は、元の色またはまったく色できるように、元の色は、右下の領域を占有します。 コピー先の色、元の色、またはすべてではなく、中央のソースの保存先との積集合を色付けすることができます。

組み合わせの総数は (左) 用の 2 を使用 (中央)、3、または 12 回 (右下) の 2 回です。 これらは、12 の基本的な Porter Duff 複合モードです。

2018 年 2 月末日まで、_合成デジタル画像_(ページ 256) の Porter と Duff 追加という 13 モード_plus_ (、SkiaSharp に対応する`SKBlendMode.Plus`メンバーと、W3C_明るい_モード (これは、W3C と混同しないように_明るく_モード)。これは、`Plus`モードは後で詳しく説明するプロセスの送信先と送信元の色を追加します。

Skia という 14 モードを追加します`Modulate`に非常に似ています`Plus`送信先と送信元の色を乗算する点が異なります。 これは、追加 Porter Duff blend モードとして処理できます。

ここでは、14 Porter Duff モード SkiaSharp で定義されています。 表には、上記の図の 3 つの空白領域の各色方法を示します。

| モード       | 保存先 | 積集合 | ソース |
| ---------- |:-----------:|:------------:|:------:|
| `Clear`    |             |              |        |
| `Src`      |             | ソース       | x      |
| `Dst`      | x           | 保存先  |        |
| `SrcOver`  | x           | ソース       | x      |
| `DstOver`  | x           | 保存先  | x      |
| `SrcIn`    |             | ソース       |        |
| `DstIn`    |             | 保存先  |        |
| `SrcOut`   |             |              | x      |
| `DstOut`   | X           |              |        |
| `SrcATop`  | x           | ソース       |        |
| `DstATop`  |             | 保存先  | x      |
| `Xor`      | X           |              | X      |
| `Plus`     | x           | Sum          | x      |
| `Modulate` |             | 製品      |        | 

これらの描画モードは対称にします。 ソースおよびデスティネーションを交換して、すべてのモードは引き続き使用できます。

モードの名前付け規則では、いくつかの単純な規則に従います。

- **Src**または**Dst**を単独で送信元または送信先のピクセルが表示されていることを意味します。
- **経由で**サフィックスは、新機能の交差部分で表示されていることを示します。 「で」他のソースまたは宛先が描画されます。
- **で**サフィックスは交差部分だけの色が指定されたことを意味します。 出力は、変換元または転送先として"in"、他の部分のみに限定されます。
- **アウト**サフィックスは、交差部分の色が設定しないことを意味します。 出力は、変換元または転送先として、積集合の"out"の部分のみです。
- **一番上にある**サフィックスの和集合である**で**と**アウト**します。これには、他の「上」の送信元または送信先の領域が含まれます。

違いに注意してください、`Plus`と`Modulate`モード。 これらのモードでは、ソースと宛先のピクセルに別の種類の計算を実行しています。 まもなく詳細に記述されています。

**Porter Duff グリッド**ページのグリッド形式で 1 つの画面ですべての 14 モードが表示されます。 各モードは、別のインスタンスの`SKCanvasView`します。 そのため、クラスがから派生`SKCanvasView`という`PorterDuffCanvasView`します。 静的コンス トラクターは、左上隅にある茶色がかった四角形を持つ 1 つ、もう青み四角形は、同じサイズの 2 つのビットマップを作成します。

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

インスタンス コンス トラクターが型のパラメーターを持つ`SKBlendMode`します。 フィールドに、このパラメーターを保存します。 

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

`OnPaintSurface`上書きが 2 つのビットマップを描画します。 最初は通常どおり描画します。

```csharp
canvas.DrawBitmap(dstBitmap, rect);
```

2 つ目の描画に使用、`SKPaint`オブジェクトを`BlendMode`プロパティは、コンス トラクターの引数に設定されています。

```csharp
using (SKPaint paint = new SKPaint())
{
    paint.BlendMode = blendMode;
    canvas.DrawBitmap(srcBitmap, rect, paint);
}
```

残りの部分、`OnPaintSurface`上書きのサイズを示すビットマップを囲む四角形を描画します。

`PorterDuffGridPage`クラスの 14 個のインスタンスを作成`PorterDurffCanvasView`の各メンバーの 1 つ、`blendModes`配列。 順序、`SKBlendModes`互いに隣接するようなモードを配置するには、配列内のメンバーはテーブルよりも若干異なります。 14 個のインスタンスの`PorterDuffCanvasView`とでのラベルを編成、 `Grid`:

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

結果を次に示します。

[![Porter Duff グリッド](porter-duff-images/PorterDuffGrid.png "Porter Duff グリッド")](porter-duff-images/PorterDuffGrid-Large.png#lightbox)

透過性が Porter Duff blend モードが正しく機能するために重要であると自分でします。 `PorterDuffCanvasView`クラスには、次の 3 つの呼び出しの合計が含まれています、`Canvas.Clear`メソッド。 それらのすべては、パラメーターなしのメソッドは、すべてのピクセルを透明に設定を使用します。

```csharp
canvas.Clear();
```

ピクセルが不透明な白色に設定されるようには、これらの呼び出しのいずれかを変更してみてください。

```csharp
canvas.Clear(SKColors.White);
```

次の変更をするには、一部の blend モードと思われるが、他のユーザーは表示されません。 ソース ビットマップの背景を白に設定した場合、`SrcOver`モードは、変換先を表示できるようにするソース ビットマップの透明なピクセルがないために機能しません。 コピー先ビットマップまたは白とし、キャンバスの背景を設定した場合`DstOver`変換先が透明なピクセルがあるないために機能しません。

ビットマップを置換するという誘惑にある可能性があります、 **Porter Duff グリッド**ページ簡単`DrawRect`呼び出し。 先の四角形の元の四角形ではなくを機能します。 元の四角形には、単青み色分けエリアが含まれる必要があります。 元の四角形は、変換先の色付きの領域に対応する透明な領域を含める必要があります。 モードの作業をしてから、これらはブレンドします。

## <a name="using-mattes-with-porter-duff"></a>Porter Duff でマットを使用します。

**レンガ壁の複合**ページは、クラシック合成タスクの例を示しています。 画像を除去する必要がある背景ビットマップを含む、いくつかの情報を使用して組み立てする必要があります。 ここでは、 **SeatedMonkey.jpg**問題のある背景ビットマップを。

![Monkey を装着](porter-duff-images/SeatedMonkey.jpg "Monkey の取り付け")

合成、対応するための準備_マット_が作成されたイメージを表示する場合は黒と透明をそれ以外の場合は別のビットマップがあります。 このファイルの名前は**SeatedMonkeyMatte.png**でリソース間であり、**メディア**フォルダーで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)サンプル:

![Monkey マット取り付けられている](porter-duff-images/SeatedMonkeyMatte.png "Monkey マット取り付けられています")

これは_いない_エキスパートによって作成された、マットします。 最適な状態マットが黒のピクセルの端に部分的に透明なピクセルを含める必要があり、このマットはありません。

XAML ファイル、**レンガ壁合成**ページをインスタンス化、`SKCanvasView`と`Button`最終的なイメージの作成のプロセスを制御します。

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

分離コード ファイルを処理し、ある 2 つのビットマップを読み込み、`Clicked`のイベント、`Button`します。 すべて`Button` をクリックして、`step`フィールドがインクリメントされると、新しい`Text`プロパティに対して設定されて、`Button`します。 ときに`step`が 5 に達すると 0 に戻されます。

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

何も除く表示プログラムを最初に実行すると、 `Button`:

[![レンガ壁の複合手順 0](porter-duff-images/BrickWallCompositing0.png "レンガ壁の複合手順 0")](porter-duff-images/BrickWallCompositing0-Large.png#lightbox)

キーを押して、`Button`と、1 回`step`を 1 にインクリメントして、`PaintSurface`ハンドラーが表示されます**SeatedMonkey.jpg**:。

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

ない`SKPaint`オブジェクトのため、blend モードなし。 ビットマップは、画面の下部に表示されます。

[![レンガ壁の複合手順 1.](porter-duff-images/BrickWallCompositing1.png "レンガ壁の複合手順 1")](porter-duff-images/BrickWallCompositing1-Large.png#lightbox)

キーを押して、`Button`もう一度と`step`を 2 ずつ増加します。 これは、重要な手順を表示する、 **SeatedMonkeyMatte.png**ファイル。

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

ブレンド モードは`SKBlendMode.DstIn`ソースの非透過的な分野に対応する領域で、変換先が保持されることを意味します。 元のビットマップに対応する先の四角形の残りの部分は透明になります。

[![レンガ壁の複合手順 2.](porter-duff-images/BrickWallCompositing2.png "レンガ壁の複合手順 2")](porter-duff-images/BrickWallCompositing2-Large.png#lightbox)

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

ブレンド モードは、この歩道に、monkey の背後になる必要があります、ため`DstOver`します。 コピー先では、背景が透明な場所のみが表示されます。

[![レンガ壁の複合 3.](porter-duff-images/BrickWallCompositing3.png "レンガ壁の複合手順 3")](porter-duff-images/BrickWallCompositing3-Large.png#lightbox)

最後の手順では、レンガの壁を追加します。 プログラムは、静的プロパティとして、レンガ壁ビットマップ タイルを使用して`BrickWallTile`で、`AlgorithmicBrickWallPage`クラス。 変換を追加、`SKShader.CreateBitmap`一番下の行が完全なタイルをあるように、タイルをシフトする呼び出し。

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

便宜上、`DrawRect`呼び出しは、キャンバス全体については、一定のこのシェーダーが表示されますが、`DstOver`モードでも透過的なキャンバスの領域のみに出力を制限します。

[![レンガ壁の複合手順 4](porter-duff-images/BrickWallCompositing4.png "レンガ壁の複合手順 4")](porter-duff-images/BrickWallCompositing4-Large.png#lightbox)

明らかには、このシーンを作成するには、その他の方法があります。 これは、バック グラウンドで開始すると、フォア グラウンドに進行して構築できます。 柔軟性の方が blend モードを使用します。 具体的には、マットの使用には、構成済みのシーンから除外するビットマップの背景ができます。

この記事で説明したよう[パスおよび領域でクリッピング](../../curves/clipping.md)、`SKCanvas`クラス定義に対応するクリッピングの 3 つの種類、 [ `ClipRect` ](xref:SkiaSharp.SKCanvas.ClipRect*)、 [ `ClipPath` ](xref:SkiaSharp.SKCanvas.ClipPath*)、および[ `ClipRegion` ](xref:SkiaSharp.SKCanvas.ClipRegion*)メソッド。 Porter Duff blend モードでは、別の種類のビットマップを含む、描画できるものに画像を制限することにより、クリップを追加します。 使用されるマット**レンガ壁合成**本質的にはクリッピング領域を定義します。

## <a name="gradient-transparency-and-transitions"></a>グラデーションの透過性と遷移

Porter Duff の例では、この記事ではすべてありませんは、不透明なピクセルと透過的な (ピクセル単位) がない、部分的に透明なピクセルのイメージを前述のモードをブレンドします。 Blend モード関数は、これらのピクセルにも定義されます。 次の表は、Skia で見つかった表記を使用して、Porter Duff blend モードより正式な定義[ **SkBlendMode 参照**](https://skia.org/user/api/SkBlendMode_Reference)します。 (ため**SkBlendMode 参照**Skia の参照は、C++ 構文を使用します)。

概念的には、各ピクセルの赤、緑、青、およびアルファ コンポーネントは、0 ~ 1 の範囲の浮動小数点数にバイトから変換されます。 アルファ チャネルの 0 は完全に透明、1 は完全に不透明です

次の表に、表記法では、次の省略形を使用します。

- **Da**が変換先のアルファ チャネル
- **Dc** RGB 色の送信先
- **Sa**ソース アルファ チャネル
- **Sc** RGB 色のソース

RGB 色はアルファ値で乗算済み。 たとえば場合、 **Sc**純粋な赤色を表しますが、 **Sa** 0x80、RGB 色が **(0x80、0, 0)** します。 場合**Sa** 0 の場合は、RGB のすべてのコンポーネントもゼロです。

アルファ チャネルおよび RGB 色をコンマで区切られた、角かっこで、結果が示されます。 **[アルファ、color]** します。 色の計算は赤、緑、青のコンポーネントを個別に実行します。

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

これらの操作は簡単に分析するときに**Da**と**Sa**が 0 または 1 のいずれか。 たとえば、既定値`SrcOver`モードでは場合、 **Sa**し 0 の場合は、 **Sc**はも 0 の場合と、結果は **[Da, Dc]**、宛先アルファと色。 場合**Sa** 、結果は 1 に設定されて **[Sa, Sc]**、alpha のソースと色、または **[1, Sc]** します。

`Plus`と`Modulate`モードは、新しい色が、ソースとターゲットの組み合わせに起因することで、他のユーザーとは少し異なります。 `Plus`バイトまたは浮動小数点のコンポーネントのいずれにも、モードを解釈できます。 **Porter Duff グリッド**ページのコピー先の色は、前に示した **(0xC0、0x80、0x00)** 元の色は **(0x00、0x80、0xC0)** します。 コンポーネントの各ペアが追加されますが、合計は 0 xff で固定されます。 結果は、色 **(0xC0、0 xff 0xC0)** します。 交差部分に示すように色です。

`Modulate`モードでは、RGB 値は、浮動小数点に変換する必要があります。 コピー先の色は **(0.75, 0.5, 0)** かつ、ソースが **(0, 0.5, 0.75)** します。 RGB コンポーネントは同時に、各乗算され、結果は **(0, 0.25, 0)** します。 交差部分で示されているカラーは、 **Porter Duff グリッド**このモードのページ。

**Porter Duff 透明** ページでは、部分的に透明なグラフィカル オブジェクトに、Porter Duff blend モードの動作を確認できます。 XAML ファイルが含まれています、 `Picker` Porter Duff モードを使用します。

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

[![Porter Duff 透明](porter-duff-images/PorterDuffTransparency.png "Porter Duff 透過性")](porter-duff-images/PorterDuffTransparency-Large.png#lightbox)

送信先と送信元の構成が元の Porter Duff の 255 をページに表示されるダイアグラムとよく似ています[_合成デジタル画像_](https://graphics.pixar.com/library/Compositing/paper.pdf)ホワイト ペーパーがこのページを示しますが、ブレンド モードは、部分的に透明な領域の適切に動作します。

さまざまな効果をいくつかの透明なグラデーションを使用することができます。 1 つの可能性をマスクするで示したテクニックに似た、 [**マスク放射状グラデーション**](../shaders/circular-gradients.md#radial-gradients-for-masking)のセクション、 **SkiaSharp 円形グラデーション ページ**します。 多くの**合成マスク**ページはその以前のバージョンのプログラムに似ています。 ビットマップ リソースを読み込むし、それを表示する四角形を決定します。 放射状グラデーションは、事前に決められた center および radius に基づいて作成されます。

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

プログラムとの違いは、グラデーションが中心で色が黒で始まるし、透過性で終了です。 ブレンド モードのビットマップが表示されたら、それ`DstIn`、透過的ではないソースの領域でのみ、変換先が示されます。

後に、`DrawRect`呼び出し、キャンバスのサーフェイス全体は放射状のグラデーションで定義されている円を除く透過的です。 最終呼び出しが行われます。

```csharp
canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
```

キャンバスのすべての透明な領域はピンク色。

[![合成マスク](porter-duff-images/CompositingMask.png "合成マスク")](porter-duff-images/CompositingMask-Large.png#lightbox)

1 つのイメージを別の遷移の Porter Duff モードと部分的に透明なグラデーションを使用することもできます。 **グラデーションの遷移**ページが含まれています、`Slider`を 0 から 1 への移行の進行状況のレベルを示すために、`Picker`する切り替え効果の種類を選択します。

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

分離コード ファイルは、移行を示すために 2 つのビットマップ リソースを読み込みます。 これらは、同じ 2 つのイメージで使用される、**ビットマップ ディゾルブ**この記事で前述のページ。 コードは、3 つのグラデーションの種類に対応する 3 つのメンバーを持つ列挙型を定義するも&mdash;線形的で、半径、およびスイープします。 これらの値が読み込まれる、 `Picker`:

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

3 つの分離コード ファイルが作成`SKPaint`オブジェクト。 `paint0`オブジェクトはブレンド モードを使用しません。 この描画オブジェクトに記載されている透明な黒から移動するグラデーションの四角形を描画するために使用されます、`colors`配列。 `positions`の位置に基づいて配列、 `Slider`、ですが、ある程度を調整します。 場合、`Slider`の最小値または最大値には、`progress`値は 0 または 1 と 2 つのビットマップのいずれかが完全に表示されます。 `positions`配列を適宜設定する必要がこれらの値。

場合、`progress`値は 0 で、 `positions` -0.1 と 0 の値が配列に含まれています。 SkiaSharp では、それ以外の場合、グラデーションが 0 でのみ黒であることを意味する 0 かつ透過的にその最初の値が調整されます。 ときに`progress`0.5 は、配列に 0.45 と 0.55 の値が含まれています。 グラデーションは 0.45 には、0 から黒、透過的に移行し、0.55 から完全に透明を 1 にします。 ときに`progress`1 に設定されて、`positions`配列は 1、1.1、グラデーションが 0 から 1 に黒であることを意味します。

`colors`と`position`配列が両方の 3 つの方法で使用される`SKShader`グラデーションを作成します。 に基づいてこれらのシェーダーのいずれかが作成された場合のみ、`Picker`の選択。 

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

そのグラデーションが blend モードを使用せず、四角形で表示されます。 その後`DrawRect`呼び出し、キャンバスには、黒から透過的なへのグラデーションが含まれます。 以降に黒の量が増加`Slider`値。

最後の 4 つのステートメントで、`PaintSurface`ハンドラー、2 つのビットマップが表示されます。 `SrcOut` Blend モードでは、ビットマップを背景の透明な領域にのみ表示されていることを意味します。 `DstOver` 2 つ目のビットマップのモードは 2 つ目のビットマップがビットマップが表示されていない領域にのみ表示されることを意味します。

次のスクリーン ショットでは、それぞれ 50% のマークで 3 つの異なる遷移型を示します。

[![グラデーションの遷移](porter-duff-images/GradientTransitions.png "グラデーションの遷移")](porter-duff-images/GradientTransitions-Large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)