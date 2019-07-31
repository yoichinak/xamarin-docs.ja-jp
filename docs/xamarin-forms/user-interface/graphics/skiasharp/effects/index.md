---
title: SkiaSharp の効果
description: グラデーションによるグラフィックスの通常の表示を変更、タイルのビットマップ、描画モード、ぼかし方法およびその他の効果について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B3E06572-8E2A-49FA-90D1-444C394CD516
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2018
ms.openlocfilehash: da923a3542a57b6150e536ecb6649140e57c81e1
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655350"
---
# <a name="skiasharp-effects"></a>SkiaSharp の効果

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp、 [ `SKPaint` ](xref:SkiaSharp.SKPaint)クラスの一般的な用語の下に分類できる 6 つのプロパティを定義_効果_します。 これらは、何らかの方法でグラフィックスの通常の表示を変更するプロパティです。 SkiaSharp の効果は、6 つのカテゴリに分類されます。

## <a name="path-effectscurveseffectsmd"></a>[パスの効果](../curves/effects.md)

設定、 [ `PathEffect` ](xref:SkiaSharp.SKPaint.PathEffect)プロパティの`SKPaint`型のオブジェクトに[ `SKPathEffect` ](xref:SkiaSharp.SKPathEffect)破線の線を表示したり、ストロークを描画またはパスから作成されたパターンを使って領域を塗りつぶすのためです。 パスの効果は、情報の記事では、このシリーズで前カバーされた[ **SkiaSharp パス効果**](../curves/effects.md)します。

## <a name="shadersshadersindexmd"></a>[シェーダー](shaders/index.md)

設定、 [ `Shader` ](xref:SkiaSharp.SKPaint.Shader)プロパティの`SKPaint`型のオブジェクトに[ `SKShader` ](xref:SkiaSharp.SKShader)線形または円形グラデーション、タイル化されたビットマップ、およびパーリン ノイズのパターンを表示します。

## <a name="blend-modesblend-modesindexmd"></a>[ブレンド モード](blend-modes/index.md)

設定、 [ `BlendMode` ](xref:SkiaSharp.SKPaint.BlendMode)プロパティの`SKPaint`のメンバーに、 [ `SKBlendMode` ](xref:SkiaSharp.SKBlendMode)ソース グラフィックが変換先に表示されるときの動作を制御する列挙体。 SkiaSharp モードをサポートしてすべて CSS 合成と blend、Porter Duff モード、分離可能なブレンド モード、分離以外のブレンド モードなど。

## <a name="mask-filtersmask-filtersmd"></a>[フィルターのマスク](mask-filters.md)

設定、 [ `MaskFilter` ](xref:SkiaSharp.SKPaint.MaskFilter)プロパティの`SKPaint`型のオブジェクトに[ `SKMaskFilter` ](xref:SkiaSharp.SKMaskFilter)ぼかしおよびその他のアルファ効果。

## <a name="image-filtersimage-filtersmd"></a>[イメージのフィルター](image-filters.md)

設定、 [ `ImageFilter` ](xref:SkiaSharp.SKPaint.ImageFilter)プロパティの`SKPaint`型のオブジェクトに[ `SKImageFilter` ](xref:SkiaSharp.SKImageFilter)ぼかしビットマップと作成にドロップ シャドウ、エンボス、または彫刻に影響します。

## <a name="color-filterscolor-filtersmd"></a>[カラー フィルター](color-filters.md)

設定、 [ `ColorFilter` ](xref:SkiaSharp.SKPaint.ColorFilter)プロパティの`SKPaint`型のオブジェクトに[ `SKColorFilter` ](xref:SkiaSharp.SKColorFilter)テーブルを使用する色を変更するのには、またはマトリックスに変換します。

すべてのサンプル コードでは、次の記事、 [ **SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)します。 ホーム ページから次のように選択します。 **SkiaSharp 効果**します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
