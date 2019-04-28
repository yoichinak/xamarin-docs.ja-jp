---
title: SkiaSharp のブレンド モード
description: Blend のグラフィカル オブジェクトは、相互に積み重ねられているときの動作を定義するモードを使用。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CE1B222E-A2D0-4016-A532-EC1E59EE3D6B
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 3ea05563ecbca95d26d692d5424c30e961229ac5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61021213"
---
# <a name="skiasharp-blend-modes"></a>SkiaSharp のブレンド モード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

これらの記事の重点、 [ `BlendMode` ](xref:SkiaSharp.SKPaint.BlendMode)プロパティの[ `SKPaint`](xref:SkiaSharp.SKPaint)します。 `BlendMode`プロパティの型は[ `SKBlendMode` ](xref:SkiaSharp.SKBlendMode)、29 のメンバーを持つ列挙体。

`BlendMode`プロパティは、グラフィカル オブジェクトの動作を決定します。 (多くの場合と呼ばれる、_ソース_) 既存のグラフィカル オブジェクト上に描画されます (と呼ばれる、_先_)。 通常、その下にあるオブジェクトがわかりにくくなる新しいグラフィカル オブジェクトを予定です。 Blend の既定のモードはためのみに発生するが、 `SKBlendMode.SrcOver`、ソースを描画することを意味する_経由で_先。 他の 28 メンバー`SKBlendMode`その他の効果が発生します。 グラフィックス プログラミングでは、さまざまな方法でのグラフィカル オブジェクトを組み合わせた手法と呼ばれます_合成_します。

## <a name="the-skblendmodes-enumeration"></a>SKBlendModes 列挙型

SkiaSharp のブレンド モードは、W3C で説明したものに密接に対応[**合成とレベル 1 のブレンド**](https://www.w3.org/TR/compositing-1/)仕様。 Skia [ **SkBlendMode 参照**](https://skia.org/user/api/SkBlendMode_Reference)役立つ背景情報も提供します。 Blend のモードでは、一般的な概要について、 [**ブレンド モード**](https://en.wikipedia.org/wiki/Blend_modes) wikipedia の記事では有効な出発点です。 ブレンド モードは、そのコンテキストでの blend モードの詳細についての追加のオンライン情報があるために、Adobe の Photoshop でサポートされます。

29 のメンバー、`SKBlendMode`列挙体は、次の 3 つのカテゴリに分類できます。

| Porter Duff | 分離可能    | 非分離 |
| ----------- | ------------ | ------------- |
| `Clear`     | `Modulate`   | `Hue`         |
| `Src`       | `Screen`     | `Saturation`  |
| `Dst`       | `Overlay`    | `Color`       |
| `SrcOver`   | `Darken`     | `Luminosity`  |
| `DstOver`   | `Lighten`    |               |
| `SrcIn`     | `ColorDodge` |               |
| `DstIn`     | `ColorBurn`  |               |
| `SrcOut`    | `HardLight`  |               |
| `DstOut`    | `SoftLight`  |               |
| `SrcATop`   | `Difference` |               |
| `DstATop`   | `Exclusion`  |               |
| `Xor`       | `Multiply`   |               |
| `Plus`      |              |               |

これら 3 つのカテゴリの名前は、次のセクションでより多くの意味になる予定です。 メンバーがここに表示されていること、順序は、の定義と同様、`SKBlendMode`列挙体。 最初の列内の 13 の列挙体のメンバーでは、0 ~ 12 の整数値があります。 2 番目の列は整数 13 ~ 24 に対応する列挙型メンバーを 3 番目の列内のメンバーの 25 ~ 28 の値がします。

モードは、後ほどこれら blend_約_W3C で同じ順序**合成とレベル 1 のブレンド**ドキュメントがいくつか違いがあります。`Src`モードと呼びます_コピー_ 、W3C ドキュメント内および`Plus`と呼びます_明るい_。 W3C のドキュメント定義、_標準_に含まれていない blend モード`SKBlendModes`と同じであるため`SrcOver`します。 `Modulate` Blend モード (最初の列の上部) には、W3C のドキュメントとのディスカッションに含まれていない、`Multiply`モードよりも前`Screen`します。

`Modulate` Blend モードは Skia に一意な追加の Porter Duff モードとは分離モードとして、説明します。

## <a name="the-importance-of-transparency"></a>透過性の重要性

従来は、合成がと共に開発の概念、_アルファ チャネル_します。 などの表示画面、`SKCanvas`オブジェクトおよびフルカラー ビットマップ、各ピクセルは 4 バイト。1 バイト、赤、緑、および青のコンポーネントと追加のバイトの透明度。 このアルファ コンポーネントは完全な透明性の 0 のこれらの値の間での透過性のさまざまなレベルでの完全な不透明度 0 xff までです。

ブレンド モードの多くは、透過性に依存します。 通常はときに、`SKCanvas`で最初に取得されます、`PaintSurface`ハンドラー、または、`SKCanvas`が作成されるビットマップを描画するには、最初の手順はこの呼び出しです。

```csharp
canvas.Clear();
```

このメソッドは、キャンバスのすべてのピクセルを透明な黒 (ピクセル単位) に相当置き換えます`new SKColor(0, 0, 0, 0)`または 0x00000000 の整数。 すべてのすべてのピクセルのバイト数は 0 に初期化されます。

描画サーフェイス、`SKCanvas`で取得した、`PaintSurface`ためだけのですが、ハンドラーが白の背景に表示される、`SKCanvasView`自体に含まれて、透明な背景と、ページには白の背景。 Xamarin.Forms を設定して、自分にこの事実を示すことができます`BackgroundColor`プロパティの`SKCanvasView`Xamarin.Forms 色。

```csharp
canvasView.BackgroundColor = Color.Red;
```

またはから派生したクラスで`ContentPage`ページの背景色を設定することができます。

```csharp
BackgroundColor = Color.Red;
```

この赤い背景、SkiaSharp グラフィックスの背後にあるため、SkiaSharp キャンバス自体は透過的ですが表示されます。

この記事[ **SkiaSharp の透明度**](../../basics/transparency.md)複合イメージで複数のグラフィックの配置に透明度を使用して基本的な手法を説明しました。 ブレンド モードがさらに、移動しますが、透過性は blend モードに非常に重要です。 

## <a name="skiasharp-porter-duff-blend-modesporter-duffmd"></a>[SkiaSharp の Porter Duff ブレンド モード](porter-duff.md)

Porter Duff blend モードを使用すると、ソースと宛先のイメージに基づくシーンを作成できます。

## <a name="skiasharp-separable-blend-modesseparablemd"></a>[SkiaSharp 分離可能なブレンド モード](separable.md)

Blend の分離モードを使用すると、赤、緑、および青の色を変更できます。

## <a name="skiasharp-non-separable-blend-modesnon-separablemd"></a>[SkiaSharp の分離を可能なブレンド モード](non-separable.md)

Blend のない分離モードを使用すると、色相、彩度、または明るさを変更できます。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
