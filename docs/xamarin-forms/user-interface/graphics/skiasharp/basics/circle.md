---
title: 単純な円を描画
description: キャンバスとペイントを含め、SkiaSharp 図面の基本をについてください。
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 402470c3a27ba4327afa6e77336d60748abad436
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="drawing-a-simple-circle"></a>単純な円を描画

_キャンバスとペイントを含め、SkiaSharp 図面の基本をについてください。_

この記事の作成など、SkiaSharp を使用して Xamarin.Forms でグラフィックスを描画の概念を説明する、`SKCanvasView`グラフィック、処理をホストするオブジェクト、`PaintSurface`イベント、およびを使用して、`SKPaint`色と他の描画を指定するオブジェクト属性。

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)プログラムには、この一連の SkiaSharp 資料のすべてのサンプル コードが含まれています。 最初のページに権利がある**単純な円**ページ クラスを呼び出すと[ `SimpleCirclePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs)です。 このコードは、半径が 100 ピクセルのページの中央の円を描画する方法を示します。 円のアウトラインが赤であり、円の内部は青で表示します。

![](circle-images/circleexample.png "赤で青い円形")

[ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs)ページ クラスから派生`ContentPage`は 2 つと`using`SkiaSharp 名前空間のディレクティブ。

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

クラスの次のコンス トラクターを作成、 [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/)オブジェクトのハンドラーをアタッチ、 [ `PaintSurface` ](https://developer.xamarin.com/api/event/SkiaSharp.Views.Forms.SKCanvasView.PaintSurface/)イベント、およびセット、`SKCanvasView`オブジェクト、ページのコンテンツとして。

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvasView`がページの全体のコンテンツ領域を占有します。 代わりに組み合わせることができます、`SKCanvasView`他 Xamarin.Forms を使用した`View`から派生したと表示されます他の例でします。

`PaintSurface`イベント ハンドラーは、すべての描画を実行します。 このメソッドは、プログラムの実行中に複数回呼び出す一般に、グラフィックスの表示を再作成するために必要なすべての情報が維持にする必要があります。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[ `SKPaintSurfaceEventArgs` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs/)イベントに付属しているオブジェクトが 2 つのプロパティ。

- [`Info`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info/) 型の [`SKImageInfo`](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/)
- [`Surface`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface/) 型の [`SKSurface`](https://developer.xamarin.com/api/type/SkiaSharp.SKSurface/)

`SKImageInfo`構造が含まれています、描画サーフェイスについてには最も重要なの幅と高さ (ピクセル単位)。 `SKSurface`オブジェクト自体、描画サーフェイスを表します。 このプログラムで描画サーフェイスは、ビデオの表示が、他のプログラムで、`SKSurface`オブジェクトを表す場合も、ビットマップ SkiaSharp 上に描画するために使用します。

最も重要なプロパティ`SKSurface`は[ `Canvas` ](https://developer.xamarin.com/api/property/SkiaSharp.SKSurface.Canvas/)型の[ `SKCanvas`](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/)です。 このクラスは、グラフィックを使用する実際の描画を実行するコンテキストを描画します。 `SKCanvas`オブジェクトは、グラフィックスの変換およびの領域が含まれています、グラフィックスの状態をカプセル化します。

一般的な起動をここでは、`PaintSurface`イベントのハンドラー。

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

[ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/)メソッドは、透明色のキャンバスをクリアします。 オーバー ロードを使用して、キャンバスの背景色を指定できます。

ここでの目的は青で塗りつぶされます赤い円を描画することです。 この特定のグラフィック イメージに 2 つの異なる色が含まれているため、ジョブは 2 つの手順で行う必要があります。 最初の手順では、円のアウトラインを描画します。 色および線の他の特性を指定する作成し、初期化、 [ `SKPaint` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaint/)オブジェクト。

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

[ `Style` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Style/)プロパティは、することを示します*ストローク*(ここでは、円のアウトライン) 行なく*fill*内部です。 3 つのメンバー、 [ `SKPaintStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaintStyle/)列挙体は、次のようにします。

- [`Fill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Fill/)
- [`Stroke`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Stroke/)
- [`StrokeAndFill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.StrokeAndFill/)

既定値は、`Fill` です。 3 番目のオプションを使用して線を描くし、同じ色の内部を入力します。

設定、 [ `Color` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Color/)プロパティ型の値を[ `SKColor`](https://developer.xamarin.com/api/type/SkiaSharp.SKColor/)です。 取得する方法の 1 つ、`SKColor`値は、Xamarin.Forms を変換することで、`Color`値を`SKColor`拡張メソッドを使用して値[ `ToSKColor`](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.Extensions.ToSKColor/p/Xamarin.Forms.Color/)です。 [ `Extensions` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.Extensions/)クラス内で、`SkiaSharp.Views.Forms`名前空間には、Xamarin.Forms 値と SkiaSharp 値の間で変換できるその他のメソッドが含まれています。

[ `StrokeWidth` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeWidth/)プロパティは、線の太さを示します。 ここで 25 ピクセルに設定されます。

使用する`SKPaint`円を描画するオブジェクト。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

ディスプレイ画面の左上隅に対する相対座標が指定されます。 X 座標を右側に Y 座標の増加がダウンしています。 グラフィックスの詳細については、多くの場合、数学的な表記法 (x, y) を使用、ポイントを表すためにします。 ポイント (0, 0) は、ディスプレイ画面の左上隅とも呼ばれます、*原点*です。

最初の 2 つの引数`DrawCircle`円の中心の X および Y 座標を指定します。 これらには、半分の幅と高さの円の中心をディスプレイ画面の中央に配置する画面の表面が割り当てられます。 3 番目の引数は、円の半径を指定し、最後の引数は、`SKPaint`オブジェクト。

円の内部を塗りつぶすには、2 つのプロパティを変更することができます、`SKPaint`オブジェクトと呼び出し`DrawCircle`もう一度です。 このコードでは、取得する別の方法も示しています、`SKColor`の多くのフィールドのいずれかの値、 [ `SKColors` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColors/)構造体。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
このとき、`DrawCircle`呼び出しの新しいプロパティを使用して円を格納する、`SKPaint`オブジェクト。

IOS、Android、およびユニバーサル Windows プラットフォームで実行されているプログラムを次に示します。

[![](circle-images/simplecircle-small.png "単純な円ページのスクリーン ショットをトリプル")](circle-images/simplecircle-large.png#lightbox "単純な円ページのトリプル スクリーン ショット")

プログラムを自分で実行するときに、電話またはグラフィックが再描画される方法を表示する、シミュレーターを横有効にすることができます。 グラフィックは、再描画する必要があるたびに、`PaintSurface`イベント ハンドラーが再度呼び出されます。

`SKPaint`オブジェクトがプロパティを描画する画像のコレクションよりも若干高です。 これらのオブジェクトは、非常に軽量です。 再利用できる`SKPaint`オブジェクトをこのプログラムでは、または複数を作成することができます`SKPaint`描画のさまざまな組み合わせのオブジェクト。 作成してこれらのオブジェクトの外部の初期化、`PaintSurface`して、イベント ハンドラーとして保存できますフィールドで、ページ クラスです。

円のアウトラインの幅が 25 ピクセルとして指定されて&mdash;または 4 分の 1 の円の半径&mdash;が幅が狭いほど、あるし、相応の理由がある: 線の幅の半分が青い円形によって隠されています。 引数、`DrawCircle`メソッドは、円の抽象幾何学的座標を定義します。 青の内部のサイズが、最も近いピクセルにそのディメンションが、輪郭の 25 ピクセル幅にまたがっている幾何学模様の円&mdash;内側と外側の半分の半分です。

次のサンプル、 [Xamarin.Forms との統合](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)資料は、これを視覚的に示します。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
