---
title: SkiaSharp のビットマップのセグメント化された表示
description: SkiaSharp のビットマップを表示するは、いくつかの領域が拡大されており、一部の領域でないようにします。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 79AE2033-C41C-4447-95A6-76D22E913D19
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: f1f148f0b510b55aebb70a03e0d5beb89c4044c4
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/18/2018
ms.locfileid: "39131484"
---
# <a name="segmented-display-of-skiasharp-bitmaps"></a>SkiaSharp のビットマップのセグメント化された表示

SkiaSharp、`SKCanvas`オブジェクトという名前のメソッドを定義する`DrawBitmapNinePatch`という 2 つのメソッドと`DrawBitmapLattice`非常に類似しています。 両方は、これらのメソッドは、先の四角形のサイズにビットマップをレンダリングがビットマップを一様に拡大するには、代わりにそのピクセル寸法で、ビットマップの部分を表示に四角形に収まるように、ビットマップの他の部分を拡張します。

![サンプルをセグメント化された](segmented-images/SegmentedSample.png "サンプルをセグメント化")

これらのメソッドは通常、ビットマップをレンダリングするには、ボタンなどのユーザー インターフェイス オブジェクトの一部を形成するために使用します。 一般に、ボタンのコンテンツに基づいてボタンのサイズがするボタンを設計するときがボタンのコンテンツに関係なく同じ幅にするボタンの境界線をする可能性があります。 最適なアプリケーションは`DrawBitmapNinePatch`します。

`DrawBitmapNinePatch` 特殊なケースは、`DrawBitmapLattice`しますが、2 つの方法について理解すると簡単です。

## <a name="the-nine-patch-display"></a>9-更新プログラムの表示 

概念的には、 [ `DrawBitmapNinePatch` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmapNinePatch/p/SkiaSharp.SKBitmap/SkiaSharp.SKRectI/SkiaSharp.SKRect/SkiaSharp.SKPaint/)ビットマップを 9 個の正方形に分割します。

![9 つの修正プログラム](segmented-images/NinePatch.png "9 つの修正プログラム")

4 つの角にある四角形のピクセル サイズが表示されます。 矢印の示すビットマップの端に他の領域は水平方向または垂直方向にように引き伸ばさ先の四角形の領域。 中央に四角形は、水平および垂直に拡大されます。 

先の四角形のピクセル寸法にも 4 つの角を表示するには十分な領域がない場合に、使用可能なサイズと何もスケール ダウンしますが、4 つの角が表示されます。

これら 9 つの四角形にビットマップを除算するには、中央に四角形を指定する必要はのみです。 構文は、`DrawBitmapNinePatch`メソッド。

```csharp
canvas.DrawBitmapNinePatch(bitmap, centerRectangle, destRectangle, paint);
```

ビットマップに対する相対パス center 四角形です。 `SKRectI`値 (整数のバージョンの`SKRect`) およびすべての座標とサイズのピクセル単位で。 先の四角形では、画面に対して相対的です。 `paint` 引数は省略可能です。

**9 個の更新プログラムの表示**ページで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)サンプルは、型のパブリック静的プロパティを作成する静的コンス トラクターを使用する最初`SKBitmap`:

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

この記事の他の 2 つのページでは、その同じビットマップを使用します。 ビットマップは 500 ピクセルの正方形、および、すべてが同じ 100 ピクセルの正方形の領域を占有している各サイズの 25 の円の配列で構成します。

![グリッドを circle](segmented-images/CircleGrid.png "Circle グリッド")

プログラムのインスタンスのコンス トラクターを作成、`SKCanvasView`で、`PaintSurface`を使用するハンドラー`DrawBitmapNinePatch`ビットマップを拡大する画面を全体表示を表示します。

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

`centerRect`四角形には 16 の円の中央の配列が含まれます。 角の円は、ピクセル数で表示され、他のすべてが適宜拡大されます。

[![9-更新プログラムの表示](segmented-images/NinePatchDisplay.png "9-更新プログラムの表示")](segmented-images/NinePatchDisplay-Large.png#lightbox)

UWP ページでは、幅 500 ピクセルにし、そのため一連の同じサイズの円として上部と下部にある行を表示します。 それ以外の場合、コーナーではありません。 すべての円はフォームの省略記号を拡大します。

楕円と円の組み合わせで構成されるオブジェクトの奇妙な表示は、円の行と列と重なるように center 四角形を定義してください。

```csharp
SKRectI centerRect = new SKRectI(150, 150, 350, 350);
```

## <a name="the-lattice-display"></a>格子の表示

2 つ`DrawBitmapLattice`メソッドはのような`DrawBitmapNinePatch`、水平または垂直分割の任意の数が一般化されたが、します。 これらの分割は、ピクセルに対応する整数の配列によって定義されます。 

[ `DrawBitmapLattice` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmapLattice/p/SkiaSharp.SKBitmap/System.Int32[]/System.Int32[]/SkiaSharp.SKRect/SkiaSharp.SKPaint/)整数の配列はこれらのパラメーターを持つメソッドは動作しません。 [ `DrawBitmapLattice` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmapLattice/p/SkiaSharp.SKBitmap/SkiaSharp.SKLattice/SkiaSharp.SKRect/SkiaSharp.SKPaint/)型のパラメーターを持つメソッド`SKLattice`作業は、以下に示す例で使用したものです。

[ `SKLattice` ](https://developer.xamarin.com/api/type/SkiaSharp.SKLattice/)構造が 4 つのプロパティを定義します。

- [`XDivs`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.XDivs/)、整数の配列
- [`YDivs`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.YDivs/)、整数の配列
- [`Flags`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.Flags/)、配列の`SKLatticeFlags`、列挙型
- [`Bounds`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.Bounds/) 型の`Nullable<SKRectI>`ビットマップ内の省略可能なソース四角形を指定するには

`XDivs`配列は、垂直方向のストリップにビットマップの幅を分割します。 最初のストリップを拡張する左側にあるピクセル 0 から`XDivs[0]`します。 このストリップは、ピクセル幅で表示されます。 2 番目のストリップをから拡張`XDivs[0]`に`XDivs[1]`が拡大するとします。 3 番目のストリップをから拡張`XDivs[1]`に`XDivs[2]`とは、ピクセル幅で表示されます。 ビットマップの右端に、配列の最後の要素から最後のストリップを拡張します。 配列の要素数が偶数の場合は、ピクセル幅で表示されます。 それ以外の場合、拡大されます。 垂直方向のストリップの合計数は 1 つの配列の要素の数よりも詳細です。

`YDivs`配列に似ています。 水平方向のストリップに、配列の高さを分割します。

同時に、`XDivs`と`YDivs`配列は、ビットマップを四角形に分割します。 四角形の数が、水平方向のストリップの数と垂直方向のストリップの数の積になります。

Skia のドキュメントに従って、`Flags`配列に含まれる 1 つの要素ごとの四角形の最初の四角形、一番上の行、2 番目の行となどです。 `Flags`配列の型は[ `SKLatticeFlags` ](https://developer.xamarin.com/api/type/SkiaSharp.SKLatticeFlags/)、次のメンバーを持つ列挙体。

- `Default` 値は 0
- `Transparent` 値 1 を持つ

ただし、これらのフラグは足りない動作、メッセージが表示され、それを無視することをお勧めします。 設定はありませんが、`Flags`プロパティを`null`します。 配列に設定`SKLatticeFlags`四角形の合計数を網羅するのに十分な大きさの値。

**格子 9 パッチ**ページ使用`DrawBitmapLattice`を模倣する`DrawBitmapNinePatch`します。 作成した同じビットマップを使用して`NinePatchDisplayPage`:

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

両方の`XDivs`と`YDivs`プロパティ水平および垂直には、3 つのストリップにビットマップを除算した 2 つの整数の配列に設定されます: ピクセル 0 からピクセル (レンダリング ピクセル サイズ) 100 から 400 ピクセルにピクセル 100 (拡大) と(ピクセル サイズ) を 500 ピクセルに 400 ピクセルにします。 同時に、`XDivs`と`YDivs`9 の四角形の合計サイズの定義の`Flags`配列。 配列を作成するための十分なは、配列を作成するだけ`SKLatticeFlags.Default`値。

表示は、前のプログラムと同じです。

[![格子 9 パッチ](segmented-images/LatticeNinePatch.png "格子 9-更新プログラム")](segmented-images/LatticeNinePatch-Large.png#lightbox)

**格子表示**ページは、ビットマップを 16 の正方形に分割します。

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

`XDivs`と`YDivs`配列が多少異なるため、かなり前の例として、対称できない表示。

[![格子表示](segmented-images/LatticeDisplay.png "格子の表示")](segmented-images/LatticeDisplay-Large.png#lightbox)

IOS と Android のイメージ左上では、小さな円だけは、そのピクセル サイズで表示されます。 他のすべてが拡大されます。

**格子表示**ページ一般化の作成、`Flags`を実験することができます、配列`XDivs`と`YDivs`より簡単にします。 具体的には、紹介するは、最初の要素を設定するときの動作を参照してください、`XDivs`または`YDivs`0 の配列。 

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
