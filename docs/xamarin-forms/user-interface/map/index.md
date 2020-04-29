---
title: Xamarin. Forms マップ
description: マップコントロールにマップが表示され、必要な場合は、Xamarin. Forms. map NuGet パッケージが必要です。
ms.prod: xamarin
ms.assetid: B669B5EE-D24C-4C69-93E1-2CA5CC9108B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: ffd8f7cc31707d09bb3442c180a867d31afcef0f
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517494"
---
# <a name="xamarinforms-map"></a>Xamarin. Forms マップ

## <a name="initialization-and-configuration"></a>[初期化と構成](setup.md)

アプリケーションで maps 機能を使用するには、 [Xamarin. Forms. map](https://www.nuget.org/packages/Xamarin.Forms.Maps/) NuGet パッケージが必要です。 さらに、ユーザーの場所にアクセスするには、アプリケーションに対する場所のアクセス許可が必要です。

## <a name="map-control"></a>[マップ コントロール](map.md)

[`Map`](xref:Xamarin.Forms.Maps.Map)コントロールは、マップを表示して注釈を付けるためのクロスプラットフォームビューです。 プラットフォームごとにネイティブマップコントロールを使用して、ユーザーに高速で使い慣れた maps エクスペリエンスを提供します。

## <a name="position-and-distance"></a>[位置と距離](position-distance.md)

[`Position`](xref:Xamarin.Forms.Maps.Position)構造体は、通常、マップとそのピン、およびマップを配置[`Distance`](xref:Xamarin.Forms.Maps.Distance)するときに必要に応じて使用できる構造体を配置するときに使用されます。

## <a name="pins"></a>[エジェクタピン](pins.md)

コントロール[`Map`](xref:Xamarin.Forms.Maps.Map)を使用すると、位置を[`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトでマークできます。 `Pin`は、タップしたときに情報ウィンドウを開くためのマップマーカーです。

## <a name="polygons-polylines-and-circles"></a>[多角形、ポリライン、および円](polygons.md)

`Polygon`、 `Polyline`、および`Circle`の各要素を使用すると、マップ上の特定の領域を強調表示できます。 は`Polygon` 、ストロークと塗りつぶしの色を持つことができる、完全に囲まれた形状です。 は`Polyline` 、領域を完全に囲む線ではありません。 は`Circle` 、マップの円形の領域を強調表示します。

## <a name="geocoding"></a>[ジオコーディング](geocoder.md)

クラス[`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder)は、オブジェクトに[`Position`](xref:Xamarin.Forms.Maps.Position)格納されている文字列のアドレスと緯度と経度の座標を変換します。

## <a name="launch-the-native-map-app"></a>[ネイティブマップアプリを起動する](native-map-app.md)

各プラットフォームのネイティブマップアプリは、xamarin. Essentials `Launcher`クラスによって Xamarin. Forms アプリケーションから起動できます。
