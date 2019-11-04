---
title: Xamarin. Forms マップ
description: マップコントロールにマップが表示され、必要な場合は、Xamarin. Forms. map NuGet パッケージが必要です。
ms.prod: xamarin
ms.assetid: B669B5EE-D24C-4C69-93E1-2CA5CC9108B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: 013e126b76de08442327707cd0502f207826dad8
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425594"
---
# <a name="xamarinforms-map"></a>Xamarin. Forms マップ

## <a name="initialization-and-configurationsetupmd"></a>[初期化と構成](setup.md)

アプリケーションで maps 機能を使用するには、 [Xamarin. Forms. map](https://www.nuget.org/packages/Xamarin.Forms.Maps/) NuGet パッケージが必要です。 さらに、ユーザーの場所にアクセスするには、アプリケーションに対する場所のアクセス許可が必要です。

## <a name="map-controlmapmd"></a>[マップ コントロール](map.md)

[`Map`](xref:Xamarin.Forms.Maps.Map)コントロールは、マップを表示して注釈を付けるためのクロスプラットフォームビューです。 プラットフォームごとにネイティブマップコントロールを使用して、ユーザーに高速で使い慣れた maps エクスペリエンスを提供します。

## <a name="position-and-distanceposition-distancemd"></a>[位置と距離](position-distance.md)

[`Position`](xref:Xamarin.Forms.Maps.Position)構造体は、通常、マップとそのピン、およびマップを配置するときに必要に応じて使用できる[`Distance`](xref:Xamarin.Forms.Maps.Distance)構造体を配置するときに使用されます。

## <a name="pinspinsmd"></a>[Pin](pins.md)

[`Map`](xref:Xamarin.Forms.Maps.Map)コントロールを使用すると、 [`Pin`](xref:Xamarin.Forms.Maps.Pin)オブジェクトで場所をマークできます。 `Pin` は、タップしたときに情報ウィンドウを開くためのマップマーカーです。

## <a name="polygons-and-polylinespolygonsmd"></a>[多角形とポリライン](polygons.md)

`Polygon` 要素と `Polyline` 要素を使用すると、マップ上の特定の領域を強調表示できます。 `Polygon` は、ストロークと塗りつぶしの色を持つ、完全に囲まれた図形です。 `Polyline` は、領域を完全に囲む線ではありません。

## <a name="geocodinggeocodermd"></a>[ジオコーディング](geocoder.md)

[`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder)クラスは、 [`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトに格納されている文字列のアドレスと緯度と経度の座標を変換します。

## <a name="launch-the-native-map-appnative-map-appmd"></a>[ネイティブマップアプリを起動する](native-map-app.md)

各プラットフォームのネイティブマップアプリは、xamarin. Essentials `Launcher` クラスを使用して Xamarin アプリケーションから起動できます。
