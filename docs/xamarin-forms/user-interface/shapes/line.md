---
title: 'Xamarin.Forms図形: 線'
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
ms.openlocfilehash: 9d79f232a77972b6abbce23ba65d9c277b090311
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935448"
---
# <a name="xamarinforms-shapes-line"></a>Xamarin.Forms図形: 線

![プレリリース API](~/media/shared/preview.png "この API は現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Line`クラスはクラスから派生 `Shape` し、線を描画するために使用できます。 クラスから継承されるプロパティの詳細につい `Line` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

`Line` は次の特性を定義します。

- `X1`double 型のは、直線の始点の x 座標を示します。 このプロパティの既定値は0.0 です。
- `Y1`double 型のは、直線の始点の y 座標を示します。 このプロパティの既定値は0.0 です。
- `X2`double 型のは、直線の終点の x 座標を示します。 このプロパティの既定値は0.0 です。
- `Y2`double 型のは、直線の終点の y 座標を示します。 このプロパティの既定値は0.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

線の端を描画する方法を制御する方法については、「[制御線の端点](index.md#control-line-ends)」を参照してください。

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
      Stroke="Red" />
```

この例では、赤い斜線が (40, 0) から (0120) に描画されます。

![Line](line-images/line.png "行")

、、 `X1` `Y1` 、およびの各プロパティの `X2` `Y2` 既定値は0であるため、最小限の構文でいくつかの行を描画することができます。

```xaml
<Line Stroke="Red"
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
      StrokeDashArray="1,1"
      StrokeDashOffset="6" />
```

この例では、ダーク青い破線の対角線が (40, 0) から (0120) に引かれています。

![破線](line-images/dashed-line.png "破線")

破線の描画の詳細については、「[破線の図形を描画](index.md#draw-dashed-shapes)する」を参照してください。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形](index.md)
