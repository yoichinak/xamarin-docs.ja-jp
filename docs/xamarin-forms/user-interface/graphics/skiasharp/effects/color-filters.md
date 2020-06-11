---
title: "SkiaSharp color filters" description: "色フィルターを使用して、変換またはテーブルで色を変換します。"
ms. 製品: xamarin ms テクノロジ: skiasharp: 774E7B55-AEC8-4F12-B657-1C0CEE01AD63 author: davidbritch dabritch: ms. date: 08/28/2018 no loc: [ Xamarin.Forms ,] を指定します。 Xamarin.Essentials
---

# <a name="skiasharp-color-filters"></a>SkiaSharp カラーフィルター

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

色フィルターは、ビットマップ (またはその他のイメージ) の色を、posterization などの効果のために他の色に変換できます。

![色フィルターの例](color-filters-images/ColorFiltersExample.png "色フィルターの例")

色フィルターを使用するには、 [`ColorFilter`](xref:SkiaSharp.SKPaint.ColorFilter) のプロパティを、 `SKPaint` [`SKColorFilter`](xref:SkiaSharp.SKColorFilter) そのクラスのいずれかの静的メソッドによって作成された型のオブジェクトに設定します。 この記事では、次のことを示します。 

- メソッドを使用して作成されたカラー変換 [`CreateColorMatrix`](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*) 。
- メソッドを使用して作成されたカラーテーブル [`CreateTable`](xref:SkiaSharp.SKColorFilter.CreateTable*) 。

## <a name="the-color-transform"></a>カラー変換

カラー変換では、マトリックスを使用して色を変更します。 ほとんどの2D グラフィックスシステムと同様に、SkiaSharp では、 [**SkiaSharp の行列変換**](../transforms/matrix.md)の iscussed として座標点を変換するために、ほとんどの場合、マトリックスを使用します。 は [`SKColorFilter`](xref:SkiaSharp.SKColorFilter) マトリックス変換もサポートしていますが、マトリックスは RGB 色を変換します。 これらの色の変換を理解するには、マトリックスの概念に関する知識が必要です。 

カラー変換行列には、4つの行と5つの列からなるディメンションがあります。

<pre>
| M11 M12 M13 M14 M15 |
| M21 M22 M23 M24 M25 |
| M31 M32 M33 M34 M35 |
| M41 M42 M43 M44 M45 |
</pre>

RGB のソースカラー (R、G、B、A) をコピー先の色 (R '、G '、B '、A ') に変換します。 

行列乗算の準備として、ソースの色は5×1の行列に変換されます。

<pre>
| R |
| G |
| B |
| A |
| 1 |
</pre>

これらの R、G、B、値は、0 ~ 255 の範囲の元のバイトです。 これらは、0 ~ 1 の範囲の浮動小数点値に正規化され_ません_。

翻訳要素には余分なセルが必要です。 これは、座標点を変換するためのマトリックスの使用に関する[**記事で、3×**](../transforms/matrix.md#the-reason-for-the-3-by-3-matrix) 3 の行列を使用して2次元の座標点を変換することに似ています。

4×5行列は5×1行列で乗算され、製品は変換された色を持つ4×1行列になります。

<pre>
| M11 M12 M13 M14 M15 |    | R |   | R' |
| M21 M22 M23 M24 M25 |    | G |   | G' |
| M31 M32 M33 M34 M35 |  × | B | = | B' |
| M41 M42 M43 M44 M45 |    | A |   | A' |
                           | 1 |
</pre>

R '、G '、B '、および A ' の個別の式を次に示します。

`R' = M11·R + M12·G + M13·B + M14·A + M15` 

`G' = M21·R + M22·G + M23·B + M24·A + M25` 

`B' = M31·R + M32·G + M33·B + M34·A + M35` 

`A' = M41·R + M42·G + M43·B + M44·A + M45` 

ほとんどのマトリックスは、通常 0 ~ 2 の範囲で構成される乗法要因で構成されています。 ただし、最後の列 (M15 から M45) には、数式に追加された値が含まれています。 通常、これらの値は 0 ~ 255 の範囲です。 結果は 0 ~ 255 の値にクランプされます。

Id 行列は次のとおりです。

<pre>
| 1 0 0 0 0 |
| 0 1 0 0 0 |
| 0 0 1 0 0 |
| 0 0 0 1 0 |
</pre>

これにより、色が変更されることはありません。 変換式は次のとおりです。

`R' = R` 

`G' = G`

`B' = B`

`A' = A`

M44 セルは、不透明度を維持するため、非常に重要です。 通常、M41、M42、および M43 はすべて0です。これは、赤、緑、および青の値に基づいて不透明度を設定したくない場合があるためです。 ただし、M44 が0の場合、' は0になり、何も表示されません。

カラーマトリックスを使用する最も一般的な用途の1つは、カラービットマップをグレースケールビットマップに変換することです。 これには、赤、緑、および青の値の加重平均を計算する数式が含まれます。 SRGB ("標準の赤緑の青") の色空間を使用してビデオを表示する場合、この式は次のようになります。

灰色-網掛け = 0.2126 ·R + 0.7152 ·G + 0.0722 ·B

カラービットマップをグレースケールビットマップに変換するには、R '、G '、および B ' の結果がすべて同じ値である必要があります。 マトリックスは次のとおりです。

<pre>
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0    0    0    1 0 |
</pre>

このマトリックスに対応する SkiaSharp データ型はありません。 代わりに、マトリックスを行の順序にした20の値の配列 ( `float` 最初の行、2番目の行など) として表す必要があります。

静的 [`SKColorFilter.CreateColorMatrix`](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*) メソッドの構文は次のとおりです。

```csharp
public static SKColorFilter CreateColorMatrix (float[] matrix);
```

ここで `matrix` 、は20の値の配列 `float` です。 C# で配列を作成すると、4×5行列に似た数値を書式設定することが簡単になります。 これは、 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの**グレースケールマトリックス**ページで説明されています。

```csharp
public class GrayScaleMatrixPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(CenteredTilesPage),
                        "SkiaSharpFormsDemos.Media.Banana.jpg");

    public GrayScaleMatrixPage()
    {
        Title = "Gray-Scale Matrix";

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
            paint.ColorFilter =
                SKColorFilter.CreateColorMatrix(new float[]
                {
                    0.21f, 0.72f, 0.07f, 0, 0,
                    0.21f, 0.72f, 0.07f, 0, 0,
                    0.21f, 0.72f, 0.07f, 0, 0,
                    0,     0,     0,     1, 0
                });

            canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

`DrawBitmap`このコードで使用されるメソッドは、 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルに含まれている**BitmapExtension.cs**ファイルからのものです。 

IOS、Android、ユニバーサル Windows プラットフォームで実行されている結果を次に示します。

[![グレースケールのマトリックス](color-filters-images/GrayScaleMatrix.png "グレースケールのマトリックス")](color-filters-images/GrayScaleMatrix-Large.png#lightbox)

4番目の行と4番目の列の値を確認します。 これは、変換された色の "値" の元の色の値を乗算した重要な要素です。 そのセルが0の場合は何も表示されず、問題を見つけるのが困難な場合があります。

カラーマトリックスを試してみると、変換元の観点から、または変換先の観点から変換を処理できます。 ソースの赤のピクセルが変換先の赤、緑、および青のピクセルにどのように作用するかを指定します。 これは、マトリックスの最初の_列_の値によって決まります。 または、変換先の赤のピクセルがソースの赤、緑、および青のピクセルにどのように影響するかを確認します。 これは、マトリックスの最初の_行_によって決まります。

色の変換を使用する方法については、「[**画像**](https://docs.microsoft.com/dotnet/framework/winforms/advanced/recoloring-images)の色の設定」ページを参照してください。 ディスカッションには Windows フォームがあり、マトリックスは異なる形式ですが、概念は同じです。

**パステル行列**は、ソースの赤ピクセルを attenuating し、赤と緑のピクセルを少し強調することで、宛先の赤のピクセルを計算します。 このプロセスは、緑と青のピクセルに対しても同様に発生します。

```csharp
public class PastelMatrixPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(PastelMatrixPage),
                        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    public PastelMatrixPage()
    {
        Title = "Pastel Matrix";

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
            paint.ColorFilter =
                SKColorFilter.CreateColorMatrix(new float[]
                {
                    0.75f, 0.25f, 0.25f, 0, 0,
                    0.25f, 0.75f, 0.25f, 0, 0,
                    0.25f, 0.25f, 0.75f, 0, 0,
                    0, 0, 0, 1, 0
                });

            canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

結果として、次に示すように色の輝度がミュートされます。

[![パステル行列](color-filters-images/PastelMatrix.png "パステル行列")](color-filters-images/PastelMatrix-Large.png#lightbox)

## <a name="color-tables"></a>カラーテーブル

静的メソッドには、 [`SKColorFilter.CreateTable`](xref:SkiaSharp.SKColorFilter.CreateTable*) 次の2つのバージョンがあります。

```csharp
public static SKColorFilter CreateTable (byte[] table);

public static SKColorFilter CreateTable (byte[] tableA, byte[] tableR, byte[] tableG, byte[] tableB);
```

配列には、常に256エントリが含まれています。 `CreateTable`1 つのテーブルを持つメソッドでは、赤、緑、および青のコンポーネントに同じテーブルが使用されます。 単純な参照テーブルです。ソースの色が (R、G、B) で、ターゲットの色が (R '、B '、G ') の場合、変換先コンポーネントは、ソースコンポーネントでインデックスを作成することによって取得され `table` ます。

`R' = table[R]`

`G' = table[G]`

`B' = table[B]`

2番目の方法では、4つの各カラーコンポーネントに個別の色テーブルを含めることも、同じ色テーブルを複数のコンポーネント間で共有することもできます。

2番目のメソッドへの引数の1つを、 `CreateTable` 順序が 0 ~ 255 の値を含むカラーテーブルに設定する場合は、代わりにを使用でき `null` ます。 多くの場合、 `CreateTable` 呼び出しには `null` アルファチャネルの最初の引数があります。

[SkiaSharp ビットマップピクセルビット](../bitmaps/pixel-bits.md#posterization)へのアクセスに関する記事の**Posterization**のセクションでは、ビットマップのピクセルビットを変更して色解像度を下げる方法を説明しました。 これは、 _posterization_と呼ばれる手法です。 

カラーテーブルを使用してビットマップを表示することもできます。 [**ポスタリゼーションテーブル**] ページのコンストラクターは、下位6ビットがゼロに設定されたバイトにインデックスをマップするカラーテーブルを作成します。

```csharp
public class PosterizeTablePage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(PosterizeTablePage),
                        "SkiaSharpFormsDemos.Media.MonkeyFace.png");

    byte[] colorTable = new byte[256];

    public PosterizeTablePage()
    {
        Title = "Posterize Table";

        // Create color table
        for (int i = 0; i < 256; i++)
        {
            colorTable[i] = (byte)(0xC0 & i);
        }

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
            paint.ColorFilter =
                SKColorFilter.CreateTable(null, null, colorTable, colorTable);

            canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

この色テーブルを使用するのは、緑と青のチャネルに対してのみです。 赤のチャネルは引き続き完全に解決されます。

[![ポスタリゼーションテーブル](color-filters-images/PosterizeTable.png "ポスタリゼーションテーブル")](color-filters-images/PosterizeTable-Large.png#lightbox)

さまざまな色のテーブルを使用して、さまざまな効果を適用できます。 

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
