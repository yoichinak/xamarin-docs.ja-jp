---
title: 'Xamarin.Forms図形: 多角形'
description: Xamarin.FormsPolygon クラスを使用すると、閉じた図形を形成する一連の線が接続されたポリゴンを描画できます。
ms.prod: xamarin
ms.assetid: D6539F60-A5AC-46EF-86EB-E9F508EB1FA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 455221b71c612b7987f876e8e9b82e33e5d91129
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990871"
---
# <a name="xamarinforms-shapes-polygon"></a>Xamarin.Forms図形: 多角形

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polygon`クラスはクラスから派生 `Shape` し、閉じた図形を形成する一連の線が接続されている多角形を描画するために使用できます。 クラスから継承されるプロパティの詳細につい `Polygon` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

`Polygon` は次の特性を定義します。

- `Points`型の。 `PointCollection` これは、 `Point` 多角形の頂点を記述する構造体のコレクションです。
- `FillRule``FillRule`形状の内部の塗りつぶしを決定する方法を指定する型の。 このプロパティの既定値は `FillRule.EvenOdd` です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

## <a name="create-a-polygon"></a>多角形を作成する

次の XAML の例は、多角形を描画する方法を示しています。

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5"
         HeightRequest="100"
         WidthRequest="200" />
```

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形](index.md)
