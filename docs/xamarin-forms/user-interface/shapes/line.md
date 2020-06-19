---
title: 'Xamarin.Forms図形: 線'
description: 線 Xamarin.Forms クラスを使用して線を描画できます。
ms.prod: xamarin
ms.assetid: 384F1A72-6D3B-4FD3-BC40-E00A73A463EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8419a9880bb22980fd0f5465a813cd000838976c
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990808"
---
# <a name="xamarinforms-shapes-line"></a>Xamarin.Forms図形: 線

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Line`クラスはクラスから派生 `Shape` し、線を描画するために使用できます。 クラスから継承されるプロパティの詳細につい `Line` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

`Line` は次の特性を定義します。

- `X1`double 型のは、直線の始点の x 座標を示します。 このプロパティの既定値は0.0 です。
- `X2`double 型のは、直線の終点の x 座標を示します。 このプロパティの既定値は0.0 です。
- `Y1`double 型のは、直線の始点の y 座標を示します。 このプロパティの既定値は0.0 です。
- `Y2`double 型のは、直線の終点の y 座標を示します。 このプロパティの既定値は0.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

## <a name="create-a-line"></a>線を作成する

次の XAML の例は、直線を描画する方法を示しています。

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="DarkBlue"
      StrokeThickness="4"
      HeightRequest="120"
      WidthRequest="120" />
```

> [!NOTE]
> 行に `Fill` は内部がないため、のプロパティを設定して `Line` も効果はありません。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形](index.md)
