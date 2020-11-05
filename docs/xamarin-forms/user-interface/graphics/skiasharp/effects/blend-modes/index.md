---
title: SkiaSharp blend モード
description: Blend モードを使用すると、グラフィカルオブジェクトが互いに積み上げられている場合の動作を定義できます。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CE1B222E-A2D0-4016-A532-EC1E59EE3D6B
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1c345edf4c9980497d1fcd877a9142819afa9b56
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368649"
---
# <a name="skiasharp-blend-modes"></a>SkiaSharp blend モード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

これらの記事では、のプロパティに焦点を当てて [`BlendMode`](xref:SkiaSharp.SKPaint.BlendMode) [`SKPaint`](xref:SkiaSharp.SKPaint) います。 `BlendMode`プロパティの型は [`SKBlendMode`](xref:SkiaSharp.SKBlendMode) で、値は29メンバーで列挙されます。

プロパティは、( `BlendMode` 通常は _ソース_ と呼ばれる) グラフィックオブジェクトが既存のグラフィカルオブジェクト ( _変換先_ と呼ばれます) の上に描画されるときの動作を決定します。 通常、新しいグラフィカルオブジェクトでは、その下のオブジェクトが見えにくくなることが予想されます。 ただし、これは既定の blend モードがであるためにのみ発生し `SKBlendMode.SrcOver` ます。 _over_ つまり、ソースが変換先に描画されます。 他の28のメンバーは、 `SKBlendMode` 他の効果を引き起こします。 グラフィックスプログラミングでは、さまざまな方法でグラフィカルオブジェクトを組み合わせる手法を _複合_ と呼びます。

## <a name="the-skblendmodes-enumeration"></a>SKBlendModes 列挙型

SkiaSharp blend モードは、W3C [**合成および Blend Level 1**](https://www.w3.org/TR/compositing-1/) 仕様で説明されているものと密接に対応しています。 Skia [**Skblendmode の概要**](https://skia.org/user/api/SkBlendMode_Overview) では、有益な背景情報も提供されます。 Blend モードの概要については、Wikipedia の [**blend モード**](https://en.wikipedia.org/wiki/Blend_modes) に関する記事をお勧めします。 Blend モードは Adobe Photoshop でサポートされているため、そのコンテキストでは blend モードに関する追加のオンライン情報が追加されています。

列挙体の29メンバーは、 `SKBlendMode` 次の3つのカテゴリに分けることができます。

| Porter-Duff | 分離可能な    | 分離不可 |
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

これら3つのカテゴリの名前は、次のディスカッションでより多くの意味を持ちます。 メンバーが一覧表示される順序は、列挙型の定義と同じです `SKBlendMode` 。 最初の列の13個の列挙型のメンバーは、0 ~ 12 の整数値を持ちます。 2番目の列は、13 ~ 24 の整数に対応する列挙メンバーで、3番目の列のメンバーは 25 ~ 28 の値を持ちます。

これらの blend モードについては、W3C **合成および Blend Level 1** のドキュメントと _ほぼ_ 同じ順序で説明されていますが、いくつかの違いがあります。 `Src` モードは w3c ドキュメントでは _Copy_ と呼ばれ、 `Plus` _軽い_ と呼ばれます。 W3C ドキュメントでは、と同じであるためにに含まれていない _通常_ の blend モードが定義されてい `SKBlendModes` `SrcOver` ます。 `Modulate`(最初の列の一番上にある) blend モードは W3C ドキュメントに含まれていません。また、モードの説明は `Multiply` 前に `Screen` あります。

`Modulate`Blend モードは Skia に固有であるため、追加の Porter-Duff モードとして、分離可能モードとして説明します。

## <a name="the-importance-of-transparency"></a>透明度の重要性

従来、複合は、 _アルファチャネル_ の概念と共に開発されました。 オブジェクトやフルカラービットマップなどの表示画面では `SKCanvas` 、各ピクセルは4バイトで構成されます。各ピクセルは、赤、緑、および青のコンポーネント用に1バイト、透明度の追加のバイトです。 このアルファ成分は、完全な透過性の場合は0、完全な不透明度の場合は0xFF であり、これらの値の間には異なるレベルの透明度があります。

Blend モードの多くは、透明度に依存しています。 通常、が最初にハンドラーで取得されるとき、またはが作成されてビットマップに描画されるときには、 `SKCanvas` `PaintSurface` `SKCanvas` 最初の手順は次の呼び出しです。

```csharp
canvas.Clear();
```

このメソッドは、キャンバスのすべてのピクセルを、 `new SKColor(0, 0, 0, 0)` または整数の0x00000000 に相当する透明な黒いピクセルに置き換えます。 すべてのピクセルのすべてのバイトがゼロに初期化されます。

`SKCanvas`ハンドラーで取得されるの描画サーフェイスは、 `PaintSurface` 白い背景を持つように見えることがありますが、それは背景が透明で、ページに白い背景があるためです `SKCanvasView` 。 のプロパティを色に設定することによって、この事実を実際に示すことができ Xamarin.Forms `BackgroundColor` `SKCanvasView` Xamarin.Forms ます。

```csharp
canvasView.BackgroundColor = Color.Red;
```

または、から派生したクラスで、 `ContentPage` ページの背景色を設定できます。

```csharp
BackgroundColor = Color.Red;
```

SkiaSharp キャンバス自体は透明であるため、SkiaSharp グラフィックスの背後にこの赤の背景が表示されます。

[**SkiaSharp**](../../basics/transparency.md)の記事では、透明度を使用して複合画像に複数のグラフィックを配置する基本的な手法をいくつか紹介しました。 Blend モードはそれ以上ではありませんが、blend モードでは透明度が重要です。

## <a name="skiasharp-porter-duff-blend-modes"></a>[SkiaSharp Porter-Duff blend モード](porter-duff.md)

Porter-Duff blend モードを使用して、コピー元とコピー先のイメージに基づいてシーンを作成します。

## <a name="skiasharp-separable-blend-modes"></a>[SkiaSharp 分離可能 blend モード](separable.md)

分離可能な blend モードを使用して、赤、緑、および青の色を変更します。

## <a name="skiasharp-non-separable-blend-modes"></a>[SkiaSharp 分離不可ブレンドモード](non-separable.md)

分離不可能な blend モードを使用して、色合い、鮮やかさ、または輝度を変更します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)