---
title: 'Xamarin.Forms 図形: 線'
description: 線 Xamarin.Forms クラスを使用して線を描画できます。
ms.prod: xamarin
ms.assetid: 384F1A72-6D3B-4FD3-BC40-E00A73A463EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 78425617d95758d3663cb9f2c5ac9bebb463bd68
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374759"
---
# <a name="no-locxamarinforms-shapes-line"></a>Xamarin.Forms 図形: 線

![プレリリース API](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Line`クラスはクラスから派生 `Shape` し、線を描画するために使用できます。 クラスから継承されるプロパティの詳細につい `Line` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

`Line` は次の特性を定義します。

- `X1`double 型のは、直線の始点の x 座標を示します。 このプロパティの既定値は0.0 です。
- `Y1`double 型のは、直線の始点の y 座標を示します。 このプロパティの既定値は0.0 です。
- `X2`double 型のは、直線の終点の x 座標を示します。 このプロパティの既定値は0.0 です。
- `Y2`double 型のは、直線の終点の y 座標を示します。 このプロパティの既定値は0.0 です。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

線の端を描画する方法を制御する方法については、「 [制御線の端点](index.md#control-line-ends)」を参照してください。

## <a name="create-a-line"></a>線を作成する

線を描画するには、 `Line` オブジェクトを作成し、そのプロパティとプロパティをその開始点に設定し、そのプロパティと `X1` プロパティを終点に設定し `Y1` `X2` `Y` ます。 また、 `Stroke` ストロークのない線が非表示になっているため、プロパティをに設定し [`Color`](xref:Xamarin.Forms.Color) ます。

> [!NOTE]
> 行に `Fill` は内部がないため、のプロパティを設定して `Line` も効果はありません。

次の XAML の例は、直線を描画する方法を示しています。

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="Red"
      StrokeThickness="1" />
```

この例では、赤い斜線が (40, 0) から (0120) に描画されます。

![Line](line-images/line.png "行")

、、 `X1` `Y1` 、およびの各プロパティの `X2` `Y2` 既定値は0であるため、最小限の構文でいくつかの行を描画することができます。

```xaml
<Line Stroke="Red"
      StrokeThickness="1"
      X2="200" />
```

この例では、デバイスに依存しない200の長さの水平線が定義されています。 他のプロパティは既定では0であるため、線は (0, 0) から (200, 0) まで描画されます。

次の XAML の例は、破線を描画する方法を示しています。

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="DarkBlue"
      StrokeThickness="1"
      StrokeDashArray="1,1"
      StrokeDashOffset="6" />
```

この例では、ダーク青い破線の対角線が (40, 0) から (0120) に引かれています。

![破線](line-images/dashed-line.png "破線")

破線の描画の詳細については、「 [破線の図形を描画](index.md#draw-dashed-shapes)する」を参照してください。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms 図形](index.md)