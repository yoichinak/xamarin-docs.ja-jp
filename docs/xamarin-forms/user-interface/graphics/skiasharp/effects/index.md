---
title: SkiaSharp の効果
description: グラデーション、ビットマップタイル、ブレンドモード、ぼかしなどの効果を使用して、グラフィックスの通常の表示を変更する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B3E06572-8E2A-49FA-90D1-444C394CD516
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c6179f94b43f12a7bf4b91a05702c0539f3c8658
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93367791"
---
# <a name="skiasharp-effects"></a>SkiaSharp の効果

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp クラスは、 [`SKPaint`](xref:SkiaSharp.SKPaint) 一般的な _効果_ の下で分類できる6つのプロパティを定義します。 これらは、何らかの形でグラフィックスの通常の表示を変更するプロパティです。 SkiaSharp 効果は、次の6つのカテゴリに分類されます。

## <a name="path-effects"></a>[パスの効果](../curves/effects.md)

[`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect)のプロパティを `SKPaint` 型のオブジェクトに設定し [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) て、破線を表示するか、パスから作成されたパターンで領域を塗りつぶすか塗りつぶします。 パスの効果については、SkiaSharp の記事「 [**パスの影響**](../curves/effects.md)」をご覧ください。

## <a name="shaders"></a>[シェーダー](shaders/index.md)

[`Shader`](xref:SkiaSharp.SKPaint.Shader)のプロパティを `SKPaint` 型のオブジェクトに設定して [`SKShader`](xref:SkiaSharp.SKShader) 、線状または円形のグラデーション、タイル化されたビットマップ、および Perlin ノイズパターンを表示します。

## <a name="blend-modes"></a>[ブレンド モード](blend-modes/index.md)

[`BlendMode`](xref:SkiaSharp.SKPaint.BlendMode)のプロパティを `SKPaint` 列挙体のメンバーに設定して、 [`SKBlendMode`](xref:SkiaSharp.SKBlendMode) 変換先にソースグラフィックが表示されるときの動作を制御します。 SkiaSharp は、Porter-Duff モード、分離可能な blend モード、分離不可能な blend モードなど、すべての CSS 複合モードと blend モードをサポートしています。

## <a name="mask-filters"></a>[マスク フィルター](mask-filters.md)

[`MaskFilter`](xref:SkiaSharp.SKPaint.MaskFilter)のプロパティを、 `SKPaint` [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) ぼかしとその他のアルファ効果の型のオブジェクトに設定します。

## <a name="image-filters"></a>[イメージ フィルター](image-filters.md)

[`ImageFilter`](xref:SkiaSharp.SKPaint.ImageFilter)のプロパティ `SKPaint` を型のオブジェクトに設定します。これにより、 [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) ビットマップのぼかしや、ドロップシャドウ、エンボス、engraving 効果の作成ができます。

## <a name="color-filters"></a>[色フィルター](color-filters.md)

[`ColorFilter`](xref:SkiaSharp.SKPaint.ColorFilter) `SKPaint` [`SKColorFilter`](xref:SkiaSharp.SKColorFilter) テーブルまたはマトリックス変換を使用して色を変更するには、のプロパティを型のオブジェクトに設定します。

これらの記事のすべてのサンプルコードは、 [**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)にあります。 ホームページで、[ **SkiaSharp Effects** ] を選択します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)