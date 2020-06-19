---
title: 'Xamarin.Forms図形: ポリライン'
description: Xamarin.Formsポリラインクラスを使用すると、接続された一連の直線を描画できます。
ms.prod: xamarin
ms.assetid: 15D02690-AC03-457E-8815-8E4C17E4D642
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a5c7dc7af657dbb988ca51eb82cf60ca8b033cbd
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990835"
---
# <a name="xamarinforms-shapes-polyline"></a>Xamarin.Forms図形: ポリライン

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polyline`クラスはクラスから派生し、接続された `Shape` 一連の直線を描画するために使用できます。 クラスから継承されるプロパティの詳細につい `Polyline` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

`Polyline` は次の特性を定義します。

- `Points`型の。 `PointCollection` これは、 `Point` ポリラインの頂点を記述する構造体のコレクションです。
- `FillRule`型の。 `FillRule` これは、ポリライン内の交差する領域の結合方法を指定します。 このプロパティの既定値は `FillRule.EvenOdd` です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

## <a name="create-a-polyline"></a>ポリラインを作成する

次の XAML の例は、多角形を描画する方法を示しています。

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red"
          HeightRequest="100"
          WidthRequest="500" />
```

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形](index.md)
