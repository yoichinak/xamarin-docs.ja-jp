---
title: SkiaSharp での単純な円の描画
description: この記事では、アプリケーションでのキャンバスおよびペイントオブジェクトを含む SkiaSharp drawing の基本について説明 Xamarin.Forms し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 39e9effa1c43a4456a1e75e391e14804e2aa0011
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375097"
---
# <a name="drawing-a-simple-circle-in-skiasharp"></a>SkiaSharp での単純な円の描画

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_キャンバスオブジェクトと paint オブジェクトを含む SkiaSharp drawing の基本について説明します。_

この記事では、SkiaSharp を使用したグラフィックスの描画の概念について説明し Xamarin.Forms `SKCanvasView` ます。たとえば、グラフィックスをホストするオブジェクトの作成、イベントの処理 `PaintSurface` 、オブジェクトを使用した `SKPaint` 色やその他の描画属性の指定などです。

[**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)プログラムには、この一連の SkiaSharp 記事のすべてのサンプルコードが含まれています。 最初のページには、 **単純な円** が付き、ページクラスを呼び出し [`SimpleCirclePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) ます。 このコードは、100ピクセルの半径でページの中央に円を描画する方法を示しています。 円の輪郭は赤、円の内部は青になります。

![赤で囲まれた青い円](circle-images/circleexample.png)

[`SimpleCircle`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs)ページクラスはから派生 `ContentPage` し、 `using` SkiaSharp 名前空間の2つのディレクティブを含みます。

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

クラスの次のコンストラクターは、オブジェクトを作成し、 [`SKCanvasView`](xref:SkiaSharp.Views.Forms.SKCanvasView) イベントのハンドラーをアタッチして、 [`PaintSurface`](xref:SkiaSharp.Views.Forms.SKCanvasView.PaintSurface) `SKCanvasView` オブジェクトをページのコンテンツとして設定します。

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

は、 `SKCanvasView` ページのコンテンツ領域全体を占有します。 `SKCanvasView` Xamarin.Forms `View` 他の例に示すように、を他の派生物と組み合わせることもできます。

`PaintSurface`イベントハンドラーは、すべての描画を行います。 このメソッドは、プログラムの実行中に複数回呼び出すことができるため、グラフィックス表示を再作成するために必要なすべての情報を保持する必要があります。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[`SKPaintSurfaceEventArgs`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs)イベントに付随するオブジェクトには、次の2つのプロパティがあります。

- [`SKImageInfo`](xref:SkiaSharp.SKImageInfo) 型の [`Info`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info)
- [`SKSurface`](xref:SkiaSharp.SKSurface) 型の [`Surface`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface)

構造体には、 `SKImageInfo` 描画サーフェイスに関する情報が含まれます。最も重要なのは、ピクセル単位の幅と高さです。 オブジェクトは、 `SKSurface` 描画サーフェイス自体を表します。 このプログラムでは、描画サーフェイスはビデオディスプレイですが、他のプログラムでは、 `SKSurface` オブジェクトは SkiaSharp を使用して描画するビットマップを表すこともできます。

の最も重要なプロパティ `SKSurface` は [`Canvas`](xref:SkiaSharp.SKSurface.Canvas) 型 [`SKCanvas`](xref:SkiaSharp.SKCanvas) です。 このクラスは、実際の描画を実行するために使用するグラフィックス描画コンテキストです。 オブジェクトは、グラフィックスの `SKCanvas` 変換やクリッピングなど、グラフィックスの状態をカプセル化します。

イベントハンドラーの典型的な開始例を `PaintSurface` 次に示します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();
    ...
}

```

メソッドは、 [`Clear`](xref:SkiaSharp.SKCanvas.Clear) 透明色でキャンバスをクリアします。 オーバーロードを使用すると、キャンバスの背景色を指定できます。

ここでの目的は、青で塗りつぶされた赤い円を描画することです。 このグラフィックイメージには2つの異なる色が含まれているため、ジョブは2つの手順で実行する必要があります。 最初の手順では、円の輪郭を描画します。 線の色やその他の特性を指定するには、オブジェクトを作成して初期化し [`SKPaint`](xref:SkiaSharp.SKPaint) ます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 25
    };
    ...
}
```

プロパティは、 [`Style`](xref:SkiaSharp.SKPaint.Style) 内部を *塗りつぶす* のではなく、線 (この場合は円の輪郭) を *描画* することを示します。 列挙体の3つのメンバーは次のとおりです [`SKPaintStyle`](xref:SkiaSharp.SKPaintStyle) 。

- [`Fill`](xref:SkiaSharp.SKPaintStyle.Fill)
- [`Stroke`](xref:SkiaSharp.SKPaintStyle.Stroke)
- [`StrokeAndFill`](xref:SkiaSharp.SKPaintStyle.StrokeAndFill)

既定値は、`Fill` です。 3番目のオプションを使用して、線のストロークを描画し、同じ色で内部を塗りつぶします。

プロパティを [`Color`](xref:SkiaSharp.SKPaint.Color) 型の値に設定 [`SKColor`](xref:SkiaSharp.SKColor) します。 値を取得する方法の1つ `SKColor` は Xamarin.Forms `Color` `SKColor` 、拡張メソッドを使用して値を値に変換することです [`ToSKColor`](xref:SkiaSharp.Views.Forms.Extensions.ToSKColor*) 。 名前空間のクラスには、 [`Extensions`](xref:SkiaSharp.Views.Forms.Extensions) `SkiaSharp.Views.Forms` 値と SkiaSharp 値の間で変換を行う他のメソッドが含まれてい Xamarin.Forms ます。

プロパティは、 [`StrokeWidth`](xref:SkiaSharp.SKPaint.StrokeWidth) 線の太さを示します。 ここでは25ピクセルに設定されています。

このオブジェクトを使用して `SKPaint` 、円を描画します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

座標は、表示サーフェイスの左上隅に対して相対的に指定されます。 X 座標が右に上がり、Y 座標が増加します。 グラフィックスについては、多くの場合、点を示すために数学的表記 (x、y) が使用されます。 ポイント (0, 0) は、表示サーフェイスの左上隅で、多くの場合、 *原点* と呼ばれます。

の最初の2つの引数は、 `DrawCircle` 円の中心の X 座標と Y 座標を示します。 これらは、表示サーフェイスの中央に円の中心を配置するために、画面の幅と高さの半分に割り当てられます。 3番目の引数は円の半径を指定し、最後の引数は `SKPaint` オブジェクトです。

円の内部を塗りつぶすには、オブジェクトの2つのプロパティを変更 `SKPaint` して、を `DrawCircle` 再度呼び出します。 このコードは、 `SKColor` 構造体の多くのフィールドのいずれかから値を取得する別の方法も示してい [`SKColors`](xref:SkiaSharp.SKColors) ます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```

今回は、 `DrawCircle` オブジェクトの新しいプロパティを使用して円を塗りつぶし `SKPaint` ます。

IOS と Android で実行されているプログラムは次のようになります。

[![単純な円ページのトリプルスクリーンショット](circle-images/simplecircle-small.png)](circle-images/simplecircle-large.png#lightbox "単純な円ページのトリプルスクリーンショット")

自分でプログラムを実行するときに、電話またはシミュレーターを横向きにして、グラフィックがどのように再描画されるかを確認できます。 グラフィックを再描画する必要があるたびに、 `PaintSurface` イベントハンドラーが再度呼び出されます。

また、グラデーションやビットマップタイルを使用してグラフィカルオブジェクトの色を設定することもできます。 これらのオプションについては、 [**SkiaSharp シェーダー**](../effects/shaders/index.md)に関するセクションで説明します。

`SKPaint`オブジェクトは、グラフィックスの描画プロパティのコレクションよりもわずかです。 これらのオブジェクトは軽量です。 このプログラムと同じようにオブジェクトを再利用することも `SKPaint` 、 `SKPaint` 描画プロパティのさまざまな組み合わせに対して複数のオブジェクトを作成することもできます。 これらのオブジェクトは、イベントハンドラーの外部で作成して初期化することができ `PaintSurface` 、ページクラスのフィールドとして保存できます。

> [!NOTE]
> `SKPaint`クラスは、 [`IsAntialias`](xref:SkiaSharp.SKPaint.IsAntialias) グラフィックスのレンダリングでアンチエイリアシングを有効にするを定義します。 アンチエイリアシングを使用すると、通常は視覚的に滑らかになります。そのため、ほとんどのオブジェクトでこのプロパティをに設定することをお勧めし `true` `SKPaint` ます。 わかりやすくするために、このプロパティはほとんどのサンプルページでは設定され _ていません_ 。

円の輪郭の幅は25ピクセル、 &mdash; または円の半径の1分の1になるように指定されていますが、それは細くなっているように見えます。そのため、 &mdash; 線の幅の半分は青い円で隠されています。 メソッドの引数は、 `DrawCircle` 円の抽象ジオメトリック座標を定義します。 Blue の内部は、そのディメンションに対して最も近いピクセルにサイズ変更されますが、25ピクセル幅のアウトラインでは、内側と外側の半分に幾何学のまたがっがあり &mdash; ます。

次のサンプルでは、記事[と Xamarin.Forms の統合](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)について説明します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)