---
title: SkiaSharp ビットマップのセグメント化表示
description: SkiaSharp ビットマップを表示して、一部の領域が拡大され、一部の領域が機能しないようにします。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79AE2033-C41C-4447-95A6-76D22E913D19
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fe63f3fdab5d508ab0202fbfe93bdc223f97d28a
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370173"
---
# <a name="segmented-display-of-skiasharp-bitmaps"></a>SkiaSharp ビットマップのセグメント化表示

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp オブジェクトは、 `SKCanvas` という名前のメソッド `DrawBitmapNinePatch` と、 `DrawBitmapLattice` 非常によく似た2つのメソッドを定義します。 これらのメソッドはどちらもビットマップを変換先の四角形のサイズに描画しますが、ビットマップを均一に拡張するのではなく、ビットマップの一部をピクセルディメンションに表示し、ビットマップの他の部分を拡大して四角形に収まるようにします。

![分割したサンプル](segmented-images/SegmentedSample.png "セグメント化サンプル")

これらのメソッドは、通常、ボタンなどのユーザーインターフェイスオブジェクトの一部を形成するビットマップをレンダリングするために使用されます。 ボタンをデザインするときは、通常、ボタンのコンテンツに基づいてボタンのサイズを設定しますが、ボタンのコンテンツに関係なく、ボタンの境界線の幅を同じにすることをお勧めします。 これは、の理想的なアプリケーション `DrawBitmapNinePatch` です。

`DrawBitmapNinePatch` はの特殊なケースですが、を `DrawBitmapLattice` 使用して理解するには、次の2つの方法が簡単です。

## <a name="the-nine-patch-display"></a>9パッチ表示 

概念的には、 [`DrawBitmapNinePatch`](xref:SkiaSharp.SKCanvas.DrawBitmapNinePatch(SkiaSharp.SKBitmap,SkiaSharp.SKRectI,SkiaSharp.SKRect,SkiaSharp.SKPaint)) ビットマップを9つの四角形に分割します。

![9パッチ](segmented-images/NinePatch.png "9パッチ")

4つのコーナーの四角形がピクセルサイズで表示されます。 矢印が示すように、ビットマップの端にあるその他の領域は、水平方向または垂直方向に配置され、コピー先の四角形の領域に拡大されます。 中央の四角形は、水平方向と垂直方向に拡大されます。 

ピクセルディメンションに4つのコーナーも表示するために、コピー先の四角形に十分な領域がない場合は、使用可能なサイズに縮小され、4つのコーナーが表示されます。

ビットマップをこの9つの四角形に分割するには、中央に四角形を指定するだけで済みます。 メソッドの構文は次の `DrawBitmapNinePatch` とおりです。

```csharp
canvas.DrawBitmapNinePatch(bitmap, centerRectangle, destRectangle, paint);
```

中央の四角形は、ビットマップを基準としています。 これは `SKRectI` 値 (の整数バージョン `SKRect` ) であり、すべての座標とサイズはピクセル単位で表されます。 コピー先の四角形は、表示サーフェイスに対して相対的です。 `paint` 引数は省略可能です。

[**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの **9 つの更新プログラムの表示** ページでは、まず静的コンストラクターを使用して、型のパブリック静的プロパティを作成し `SKBitmap` ます。

```csharp
public partial class NinePatchDisplayPage : ContentPage
{
    static NinePatchDisplayPage()
    {
        using (SKCanvas canvas = new SKCanvas(FiveByFiveBitmap))
        using (SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 10
        })
        {
            for (int x = 50; x < 500; x += 100)
                for (int y = 50; y < 500; y += 100)
                {
                    canvas.DrawCircle(x, y, 40, paint);
                }
        }
    }

    public static SKBitmap FiveByFiveBitmap { get; } = new SKBitmap(500, 500);
    ···
}
```

この記事の他の2つのページでは、同じビットマップが使用されます。 ビットマップは500ピクセルの四角形で、25個の円の配列で構成されます。すべて同じサイズで、それぞれが100ピクセルの四角形領域を占有します。

![円グリッド](segmented-images/CircleGrid.png "円グリッド")

プログラムのインスタンスコンストラクターは、 `SKCanvasView` `PaintSurface` `DrawBitmapNinePatch` 表示サーフェイス全体に拡張されたビットマップを表示するためにを使用するハンドラーを持つを作成します。

```csharp
public class NinePatchDisplayPage : ContentPage
{
    ···
    public NinePatchDisplayPage()
    {
        Title = "Nine-Patch Display";

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

        SKRectI centerRect = new SKRectI(100, 100, 400, 400);
        canvas.DrawBitmapNinePatch(FiveByFiveBitmap, centerRect, info.Rect);
    }
}
```

四角形には `centerRect` 、16個の円の中央配列が含まれます。 角の円はピクセルディメンションで表示され、それ以外の場合はそれに合わせて拡大されます。

[![9-パッチ表示](segmented-images/NinePatchDisplay.png "Nine-Patch 表示")](segmented-images/NinePatchDisplay-Large.png#lightbox)

UWP ページは500ピクセル幅であるため、上と下の行は同じサイズの一連の円として表示されます。 それ以外の場合は、角にないすべての円が、楕円を形成するように拡大されます。

円と省略記号の組み合わせで構成されるオブジェクトが奇妙に表示されるようにするには、円の行と列が重なるように中心の四角形を定義します。

```csharp
SKRectI centerRect = new SKRectI(150, 150, 350, 350);
```

## <a name="the-lattice-display"></a>Lattice の表示

2つの `DrawBitmapLattice` メソッドはに似てい `DrawBitmapNinePatch` ますが、水平または垂直の任意の数の区分に対して一般化されています。 これらの区分は、ピクセルに対応する整数の配列によって定義されます。 

[`DrawBitmapLattice`](xref:SkiaSharp.SKCanvas.DrawBitmapLattice(SkiaSharp.SKBitmap,System.Int32[],System.Int32[],SkiaSharp.SKRect,SkiaSharp.SKPaint))これらの整数配列のパラメーターを持つメソッドは動作していないようです。 [`DrawBitmapLattice`](xref:SkiaSharp.SKCanvas.DrawBitmapLattice(SkiaSharp.SKBitmap,SkiaSharp.SKLattice,SkiaSharp.SKRect,SkiaSharp.SKPaint))型のパラメーターを持つメソッドは `SKLattice` 動作し、次に示すサンプルで使用されるメソッドです。

[`SKLattice`](xref:SkiaSharp.SKLattice)構造体は、次の4つのプロパティを定義します。

- [`XDivs`](xref:SkiaSharp.SKLattice.XDivs)、整数の配列
- [`YDivs`](xref:SkiaSharp.SKLattice.YDivs)、整数の配列
- [`Flags`](xref:SkiaSharp.SKLattice.Flags)、列挙型の配列、 `SKLatticeFlags`
- [`Bounds`](xref:SkiaSharp.SKLattice.Bounds)`Nullable<SKRectI>`ビットマップ内の任意のソース四角形を指定する型の。

配列は、 `XDivs` ビットマップの幅を垂直方向のストリップに分割します。 最初のストリップは、左のピクセル0からまでを範囲と `XDivs[0]` しています。 このストリップは、ピクセル幅で表示されます。 2つ目のストリップはからに拡張され、拡張 `XDivs[0]` `XDivs[1]` されます。 3番目のストリップはからに拡張され、 `XDivs[1]` `XDivs[2]` ピクセル幅でレンダリングされます。 最後のストリップは、配列の最後の要素からビットマップの右端までを範囲としています。 配列に偶数個の要素がある場合は、ピクセル幅で表示されます。 それ以外の場合は拡張されます。 垂直方向のストリップの合計数が、配列内の要素の数を超えています。

`YDivs`配列も似ています。 配列の高さを水平ストリップに分割します。

`XDivs`と配列は、 `YDivs` ビットマップを四角形に分割します。 四角形の数は、水平方向のストリップの数と垂直方向のストリップの数の積と等しくなります。

Skia のドキュメントによれば、配列には `Flags` 四角形ごとに1つの要素が含まれており、最初に四角形の先頭行、次に2番目の行になります。 `Flags`配列は [`SKLatticeFlags`](xref:SkiaSharp.SKLatticeFlags) 、次のメンバーを持つ列挙体である型です。

- `Default` 値0
- `Transparent` 値1を持つ

ただし、これらのフラグは意図したとおりに動作しないと思われるため、無視することをお勧めします。 ただし、 `Flags` プロパティをに設定しないで `null` ください。 `SKLatticeFlags`四角形の合計数を囲むのに十分な大きさの値の配列に設定します。

**Lattice 9 パッチ** ページはを使用して `DrawBitmapLattice` を模倣し `DrawBitmapNinePatch` ます。 次のように作成された同じビットマップを使用し `NinePatchDisplayPage` ます。

```csharp
public class LatticeNinePatchPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeNinePatchPage ()
    {
        Title = "Lattice Nine-Patch";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    `
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 400 };
        lattice.YDivs = new int[] { 100, 400 };
        lattice.Flags = new SKLatticeFlags[9]; 

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

`XDivs`プロパティとプロパティは、どちらも `YDivs` 2 つの整数の配列に設定され、ビットマップは水平方向と垂直方向の3つのストリップに分割されます。つまり、ピクセル0からピクセル 100 (ピクセルサイズでレンダリング)、ピクセル100からピクセル 400 (拡大)、および400ピクセル 500 (ピクセルサイズ) になります。 とを組み合わせ `XDivs` て、 `YDivs` 配列のサイズである合計9個の四角形を定義し `Flags` ます。 配列を作成するだけで、値の配列を作成 `SKLatticeFlags.Default` できます。

表示は、前のプログラムと同じです。

[![Lattice 9-Patch](segmented-images/LatticeNinePatch.png "Lattice Nine-Patch")](segmented-images/LatticeNinePatch-Large.png#lightbox)

**Lattice 表示** ページでは、ビットマップが16の四角形に分割されます。

```csharp
public class LatticeDisplayPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeDisplayPage()
    {
        Title = "Lattice Display";

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

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 200, 400 };
        lattice.YDivs = new int[] { 100, 300, 400 };

        int count = (lattice.XDivs.Length + 1) * (lattice.YDivs.Length + 1);
        lattice.Flags = new SKLatticeFlags[count];

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

`XDivs`配列と `YDivs` 配列は多少異なります。これは、前の例と同じように、表示があまり対称ではないことを示しています。

[![Lattice の表示](segmented-images/LatticeDisplay.png "Lattice の表示")](segmented-images/LatticeDisplay-Large.png#lightbox)

左側の iOS および Android イメージでは、小さい円だけがピクセルサイズでレンダリングされます。 他のすべてのものは拡張されます。

**Lattice Display** ページでは、配列の作成が一般化され、より簡単に試してみることができ `Flags` `XDivs` `YDivs` ます。 特に、 `XDivs` 配列または配列の最初の要素を0に設定した場合の動作を確認し `YDivs` ます。 

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)