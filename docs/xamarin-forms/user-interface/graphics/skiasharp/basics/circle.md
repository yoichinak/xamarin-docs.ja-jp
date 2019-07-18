---
title: SkiaSharp の単純な円を描画
description: この記事では、Xamarin.Forms アプリケーションで、キャンバスとペイントのオブジェクトを含む、SkiaSharp 描画の基礎について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: b4cd84e9134db2b2106af3205f189fbc2a92bdcc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61018326"
---
# <a name="drawing-a-simple-circle-in-skiasharp"></a>SkiaSharp の単純な円を描画

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_SkiaSharp 描画、キャンバスのなどの基本を学習し、オブジェクトの塗りつぶし_

この記事の作成など、SkiaSharp を使った xamarin.forms のグラフィックスの描画の概念を説明する、 `SKCanvasView` 、グラフィックス、処理をホストするオブジェクト、`PaintSurface`イベント、およびを使用して、`SKPaint`とその他の描画の色を指定するオブジェクト属性。

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)プログラムには、この一連の SkiaSharp 記事のすべてのサンプル コードが含まれています。 最初のページは使用権を持って**単純な円**ページ クラスを呼び出すと[ `SimpleCirclePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs)します。 このコードは、半径が 100 ピクセルのページの中央に円を描画する方法を示します。 円のアウトラインが赤であり、円の内部が青。

![](circle-images/circleexample.png "赤で、青色の円")

[ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs)ページ クラスから派生`ContentPage`含まれていると`using`SkiaSharp 名前空間のディレクティブ。

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

次のクラスのコンス トラクターを作成、 [ `SKCanvasView` ](xref:SkiaSharp.Views.Forms.SKCanvasView)オブジェクト、ハンドラーをアタッチします、 [ `PaintSurface` ](xref:SkiaSharp.Views.Forms.SKCanvasView.PaintSurface)イベント、およびセット、`SKCanvasView`オブジェクトをページのコンテンツ。

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvasView`ページのコンテンツ全体の領域を占有します。 または組み合わせることができます、`SKCanvasView`を他の Xamarin.Forms で`View`から派生しますがわかります。 その他の例でします。

`PaintSurface`イベント ハンドラーは、すべての描画を実行します。 プログラムの実行中に、このメソッドで複数回が呼び出すことが、グラフィックの表示を再作成するために必要なすべての情報を維持する必要があります。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[ `SKPaintSurfaceEventArgs` ](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs)イベントに付属しているオブジェクトが 2 つのプロパティ。

- [`Info`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info) 型の [`SKImageInfo`](xref:SkiaSharp.SKImageInfo)
- [`Surface`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface) 型の [`SKSurface`](xref:SkiaSharp.SKSurface)

`SKImageInfo`構造については、描画サーフェイスに最も重要なは、含まれる、幅と高さ (ピクセル単位)。 `SKSurface`オブジェクト自体の描画サーフェイスを表します。 このプログラムで描画サーフェイスはビデオ ディスプレイの場合が他のプログラムで、`SKSurface`オブジェクトを描画する SkiaSharp を使用するビットマップを表すこともできます。

最も重要なプロパティ`SKSurface`は[ `Canvas` ](xref:SkiaSharp.SKSurface.Canvas)型の[ `SKCanvas`](xref:SkiaSharp.SKCanvas)します。 このクラスは、実際の描画の実行に使用するコンテキストを描画するグラフィックです。 `SKCanvas`オブジェクトは、グラフィックス変換やクリッピングが含まれています、グラフィックスの状態をカプセル化します。

ここでは、一般的なスタートを`PaintSurface`イベント ハンドラー。

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

[ `Clear` ](xref:SkiaSharp.SKCanvas.Clear)メソッドは、透明色でキャンバスをクリアします。 オーバー ロードを使用して、キャンバスの背景色を指定できます。

ここでの目的は、ブルーで塗りつぶされた赤い円を描画するためには。 この特定のグラフィック イメージには、2 つの異なる色が含まれている、ため、ジョブを 2 つの手順で行う必要があります。 最初の手順は、円のアウトラインを描画するためにです。 色と線の他の特性を指定する作成し、初期化、 [ `SKPaint` ](xref:SkiaSharp.SKPaint)オブジェクト。

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

[ `Style` ](xref:SkiaSharp.SKPaint.Style)プロパティは、することを示します*ストローク*行 (この例では、円のアウトライン) ではなく*塗りつぶし*内部。 3 つのメンバー、 [ `SKPaintStyle` ](xref:SkiaSharp.SKPaintStyle)列挙体は、次のとおり。

- [`Fill`](xref:SkiaSharp.SKPaintStyle.Fill)
- [`Stroke`](xref:SkiaSharp.SKPaintStyle.Stroke)
- [`StrokeAndFill`](xref:SkiaSharp.SKPaintStyle.StrokeAndFill)

既定値は `Fill` です。 線のストロークを描画し、内部を同じ色で塗りつぶすには、3 番目のオプションを使用します。

設定、 [ `Color` ](xref:SkiaSharp.SKPaint.Color)プロパティ型の値を[ `SKColor`](xref:SkiaSharp.SKColor)します。 取得する方法の 1 つ、`SKColor`値は、Xamarin.Forms を変換することで、`Color`値を`SKColor`拡張メソッドを使用して値[ `ToSKColor`](xref:SkiaSharp.Views.Forms.Extensions.ToSKColor*)します。 [ `Extensions` ](xref:SkiaSharp.Views.Forms.Extensions)クラス、`SkiaSharp.Views.Forms`名前空間には、Xamarin.Forms と SkiaSharp 値の間で変換できるその他のメソッドが含まれています。

[ `StrokeWidth` ](xref:SkiaSharp.SKPaint.StrokeWidth)プロパティは、線の太さを示します。 ここで 25 ピクセルに設定されます。

使用する`SKPaint`円を描画するオブジェクト。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

画面の左上隅に対する相対座標が指定されます。 X は、右側に Y 座標の増加がダウンして調整します。 グラフィックスに関する詳細については、多くの場合、(x, y) の数学的表記を使用するポイントを示すためにします。 (0, 0) のポイントが画面の左上隅とも呼ばれます、*原点*します。

最初の 2 つの引数の`DrawCircle`円の中心の X と Y 座標を示します。 これらは、ディスプレイ画面の中央に円の中心を配置して、画面の高さの半分の幅に割り当てられます。 3 番目の引数は、円の半径を指定し、最後の引数は、`SKPaint`オブジェクト。

円の内部を塗りつぶすの 2 つのプロパティを変更することができます、`SKPaint`オブジェクトと呼び出し`DrawCircle`もう一度です。 このコードも取得する代替の方法を示して、`SKColor`からの多数のフィールドのいずれかの値、 [ `SKColors` ](xref:SkiaSharp.SKColors)構造体。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
今回は、`DrawCircle`呼び出しの入力の新しいプロパティを使用して、円、`SKPaint`オブジェクト。

IOS、Android、およびユニバーサル Windows プラットフォームで実行されているプログラムを次に示します。

[![](circle-images/simplecircle-small.png "単純な円のページのスクリーン ショットをトリプル")](circle-images/simplecircle-large.png#lightbox "単純な円のページの 3 倍になるスクリーン ショット")

プログラムを自分で実行するときに、電話またはグラフィックを描画する方法を確認するシミュレーターを横方向が有効にできます。 グラフィックが再描画する必要があるたびに、`PaintSurface`イベント ハンドラーが再度呼び出されます。

色のグラデーション、ビットマップのタイルでのグラフィカル オブジェクトをこともできます。 これらのオプションは、上のセクションで説明が[ **SkiaSharp シェーダー**](../effects/shaders/index.md)します。

`SKPaint`オブジェクトは、グラフィックスの描画プロパティのコレクションよりも少し。 これらのオブジェクトは軽量です。 再利用できる`SKPaint`このプログラムでは、または複数を作成するオブジェクト`SKPaint`描画のさまざまな組み合わせのオブジェクト。 作成してこれらのオブジェクトの外部の初期化、`PaintSurface`して、イベント ハンドラーとして保存できますフィールド、ページ クラスにします。

> [!NOTE]
> `SKPaint`クラスを定義、 [ `IsAntialias` ](xref:SkiaSharp.SKPaint.IsAntialias)グラフィックスのレンダリングにアンチエイリアシングを有効にします。 アンチエイリアシング通常、結果視覚的により滑らかなエッジは、このプロパティに設定する可能性がありますので`true`のほとんどで、`SKPaint`オブジェクト。 このプロパティは、わかりやすくするためのために、_いない_のほとんどのサンプル ページで設定します。

円のアウトラインの幅が 25 ピクセルとして指定されていますが&mdash;または 4 分の 1 円の半径&mdash;幅が狭いほど、ように見え、する正当な理由があります。線の幅の半分は、青い円によって隠されています。 引数、`DrawCircle`メソッドは、円の抽象の幾何学的座標を定義します。 青の内部のサイズは、最も近いピクセルにそのディメンションが 25 ピクセル幅のアウトラインがジオメトリの円をまたぐ&mdash;内側と外側の半分の半分です。

次のサンプル、 [Xamarin.Forms との統合](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)記事では、これを視覚的に示します。


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
