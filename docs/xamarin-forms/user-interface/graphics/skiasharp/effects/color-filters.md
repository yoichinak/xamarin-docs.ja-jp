---
title: SkiaSharp の色フィルター
description: カラー フィルターを使用して、色変換またはテーブルに変換します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 774E7B55-AEC8-4F12-B657-1C0CEE01AD63
author: davidbritch
ms.author: dabritch
ms.date: 08/28/2018
ms.openlocfilehash: 7edb504a228612d7f1f1fee10a50a467fbb5fc6c
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057101"
---
# <a name="skiasharp-color-filters"></a>SkiaSharp の色フィルター

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

カラー フィルターは、ポスタリゼーションなどの効果の他の色をビットマップ (またはその他の画像) での色を変換することができます。

![カラー フィルター例](color-filters-images/ColorFiltersExample.png "カラー フィルターの例")

色フィルターを使用する設定、 [ `ColorFilter` ](xref:SkiaSharp.SKPaint.ColorFilter)プロパティの`SKPaint`型のオブジェクトに[ `SKColorFilter` ](xref:SkiaSharp.SKColorFilter)そのクラスの静的メソッドのいずれかで作成します。 この記事を示しています。 

- 色変換を使用して作成、 [ `CreateColorMatrix` ](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*)メソッド。
- カラー テーブルが作成、 [ `CreateTable` ](xref:SkiaSharp.SKColorFilter.CreateTable*)メソッド。

## <a name="the-color-transform"></a>色の変換

色変換では、行列を使用して色を変更する必要があります。 SkiaSharp ほとんどの 2D グラフィックス システムと同様に、情報の記事で iscussed として座標点を変換するためには、ほとんどの場合マトリックスを使用して[ **SkiaSharp の変換の行列**](../transforms/matrix.md)します。 [ `SKColorFilter` ](xref:SkiaSharp.SKColorFilter)も行列の変換が RGB カラー行列変換をサポートしています。 マトリックスの概念をある程度は、これらの色変換を理解する必要があります。 

色変換マトリックスでは、4 つの行と列の 5 つのディメンションがあります。

<pre>
| M11 M12 M13 M14 M15 |
| M21 M22 M23 M24 M25 |
| M31 M32 M33 M34 M35 |
| M41 M42 M43 M44 M45 |
</pre>

コピー先の色を RGB ソース色 (R、G、B、A) を変換し (R'、G'、B ',')。 

行列乗算の準備として、元の色は、5 × 1 マトリックスに変換されます。

<pre>
| R |
| G |
| B |
| A |
| 1 |
</pre>

これらの R、G、B、および A の値は、元のバイト 0 から 255 までです。 _いない_0 ~ 1 の範囲の浮動小数点値に正規化されます。

余分なセルでは、変換係数必要があります。 これは、セクションで説明した 2 次元座標点を変換するマトリックスを 3 × 3 の使用に似ています[ **、理由、3-3 でマトリックスの**](../transforms/matrix.md#the-reason-for-the-3-by-3-matrix)マトリックスを使用して変換するための記事ポイントを調整します。

5 × 1 行列によって 4 × 5 マトリックスが乗算され、製品は、変換後の色で 4 × 1 マトリックス。

<pre>
| M11 M12 M13 M14 M15 |    | R |   | R' |
| M21 M22 M23 M24 M25 |    | G |   | G' |
| M31 M32 M33 M34 M35 |  × | B | = | B' |
| M41 M42 M43 M44 M45 |    | A |   | A' |
                           | 1 |
</pre>

R の別の数式をここでは '、G'、B' と A'。

R' = M11・R + M12・G + M13・B + M14・A + M15

G' = M21・R + M22・G + M23・B + M24・A + M25

B' = M31・R + M32・G + M33・B + M34・A + M35 

A' = M41・R + M42・G + M43・B + M44・A + M45

行列のほとんどは、通常 0 ~ 2 の範囲内にある乗法の要素で構成されます。 ただし、最後の列 (M15 M45 経由) は、数式で追加された値が含まれています。 これらの値は、一般に、255 0 範囲です。 結果は、値を 0 ~ 255 の範囲にクランプされます。

行列は次のとおりです。

<pre>
| 1 0 0 0 0 |
| 0 1 0 0 0 |
| 0 0 1 0 0 |
| 0 0 0 1 0 |
</pre>

これは、色を変化なし。 変換式は次のとおりです。

R' = R 

G' = G

B' = B

A' = A

不透明度を保持するので、M44 セルは非常に重要です。 おそらく、赤、緑、および青の値に基づいて、不透明度をしたくないため、M41、M42、および M43 はすべて 0 の場合、あるケースは一般的に。 M44 が 0、A の場合は ' は、0 になり、何が表示されます。

カラー マトリックスの最も一般的な用途の 1 つは、グレースケールのビットマップにカラー ビットマップを変換します。 これには、赤、緑、青の値の加重平均の式が含まれます。 ビデオが表示されたら、(「標準的な赤、緑 blue」) の sRGB 色空間を使用して、この数式は次のとおりです。

グレースケール = 0.2126・R + 0.7152・G + 0.0722・B

カラービットマップをグレースケールビットマップに変換するには、R '、G'、およびB 'の結果がすべて同じ値に等しくなければなりません マトリックスは。

<pre>
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0    0    0    1 0 |
</pre>

このマトリックスに対応する SkiaSharp データ型はありません。 代わりに、マトリックスを 20 の配列として表す必要があります`float`行の順序の値: 最初行、し、2 番目の行となどです。

静的な[ `SKColorFilter.CreateColorMatrix` ](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*)メソッドには、次の構文。

```csharp
public static SKColorFilter CreateColorMatrix (float[] matrix);
```

場所`matrix`、20 の配列は、`float`値。 内の配列を作成するときにC#、4 × 5 マトリックスに似ているため、数値の書式を設定するは簡単です。 これは、方法については、**グレースケール マトリックス**ページで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)サンプル。

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

`DrawBitmap`からがこのコードで使用する方法、 **BitmapExtension.cs**ファイルに含まれている、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)サンプル。 

IOS、Android、およびユニバーサル Windows プラットフォームで実行されている結果を次に示します。

[![グレースケールのマトリックス](color-filters-images/GrayScaleMatrix.png "グレースケールのマトリックス")](color-filters-images/GrayScaleMatrix-Large.png#lightbox)

4 番目の行、第 4 列の値に注意します。 A の元の色の A の値を乗算した重要な要素 ' 変換後の色の値。 そのセルが 0 の場合は、何もしないして、問題を見つけにくいことがあります。

カラー行列をみると扱うことができます、変換のソースのパースペクティブまたは変換先の観点から。 ソースの赤のピクセルが、変換先の赤、緑、および青のピクセルに協力する必要がある方法をでしょうか。 最初の値によって決まる_列_行列。 または、する必要があります、赤の送信先のピクセル影響ソースの赤、緑、および青のピクセルでしょうか。 まずによって決まる_行_行列。

いくつかのアイデア色変換を使用する方法については、次を参照してください。、 [**イメージのカラー変更情報**](https://docs.microsoft.com/dotnet/framework/winforms/advanced/recoloring-images)ページ。 については、Windows フォームに関係し、マトリックスが別の形式が、概念は同じです。

**パステル マトリックス**赤いソース ピクセルの減衰と少し赤と緑のピクセルに重点を置いたして対象の赤のピクセルを計算します。 このプロセスは、緑、青のピクセルの同様に発生します。

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

結果は、ここでわかるように、色の彩度をミュートするのには。

[![パステル マトリックス](color-filters-images/PastelMatrix.png "パステル マトリックス")](color-filters-images/PastelMatrix-Large.png#lightbox)

## <a name="color-tables"></a>カラー テーブル

静的な[ `SKColorFilter.CreateTable` ](xref:SkiaSharp.SKColorFilter.CreateTable*)メソッドが 2 つのバージョン。

```csharp
public static SKColorFilter CreateTable (byte[] table);

public static SKColorFilter CreateTable (byte[] tableA, byte[] tableR, byte[] tableG, byte[] tableB);
```

常に、配列には、256 エントリが含まれます。 `CreateTable`赤、緑、青のコンポーネントの 1 つのテーブル、同じテーブルを持つメソッドを使用します。 これは、単純なルックアップ テーブル: 元の色 (R、G、B)、およびコピー先の色である場合 (R'、B'、G')、変換先コンポーネントは、インデックス作成で取得し、`table`ソース コンポーネントで。

R' = table[R]

G' = table[G]

B' = table[B]

2 番目のメソッドでの各色コンポーネントの 4 つは個別のカラー テーブル、または同じカラー テーブルは、2 つまたは複数のコンポーネント間で共有する可能性があります。

2 番目の引数のいずれかに設定する`CreateTable`使用する方法をシーケンス内の 0 255 までからの値を含むカラー テーブルは、`null`代わりにします。 非常に多くの場合、`CreateTable`の呼び出しでは、`null`アルファ チャネルの最初の引数。

セクションで**ポスタリゼーション**についての記事で[にアクセスする SkiaSharp ビットマップのピクセル ビット](../bitmaps/pixel-bits.md#posterization)、その色の解像度を低くビットマップの個々 のピクセル ビットを変更する方法を説明しました。 これと呼ばれる手法_ポスタリゼーション_します。 

カラー テーブルを持つビットマップもポスタリゼーションことができます。 コンス トラクター、**ポスタリゼーション テーブル**ページ 6 ビットが 0 に設定を下部にバイトのインデックスをマップするカラー テーブルを作成します。

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

プログラムは、このカラー テーブルを使用して、緑、青のチャネルに対してのみを選択します。 赤チャネルは引き続きフル解像度に。

[![テーブルをポスタリゼーション](color-filters-images/PosterizeTable.png "ポスタリゼーション テーブル")](color-filters-images/PosterizeTable-Large.png#lightbox)

さまざまなカラー テーブルは、さまざまな効果のさまざまなカラー チャネルを使用できます。 

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
