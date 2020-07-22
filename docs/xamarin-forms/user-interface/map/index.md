---
title: Xamarin.Forms付け
description: マップコントロールにマップが表示され、が必要 Xamarin.Forms です。NuGet パッケージをマップします。
ms.prod: xamarin
ms.assetid: B669B5EE-D24C-4C69-93E1-2CA5CC9108B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2461ffa8168207e6a57fae005f752be48772a34a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139829"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms付け

## <a name="initialization-and-configuration"></a>[初期化と構成](setup.md)

[ Xamarin.Forms 。Maps NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Forms.Maps/)は、アプリケーションで maps 機能を使用するために必要です。 さらに、ユーザーの場所にアクセスするには、アプリケーションに対する場所のアクセス許可が必要です。

## <a name="map-control"></a>[マップ コントロール](map.md)

コントロールは、 [`Map`](xref:Xamarin.Forms.Maps.Map) マップを表示して注釈を付けるためのクロスプラットフォームビューです。 プラットフォームごとにネイティブマップコントロールを使用して、ユーザーに高速で使い慣れた maps エクスペリエンスを提供します。

## <a name="position-and-distance"></a>[位置と距離](position-distance.md)

[`Position`](xref:Xamarin.Forms.Maps.Position)構造体は、通常、マップとそのピン、および [`Distance`](xref:Xamarin.Forms.Maps.Distance) マップを配置するときに必要に応じて使用できる構造体を配置するときに使用されます。

## <a name="pins"></a>[ピン留め](pins.md)

コントロールを使用 [`Map`](xref:Xamarin.Forms.Maps.Map) すると、位置をオブジェクトでマークでき [`Pin`](xref:Xamarin.Forms.Maps.Pin) ます。 は、 `Pin` タップしたときに情報ウィンドウを開くためのマップマーカーです。

## <a name="polygons-polylines-and-circles"></a>[多角形、ポリライン、円](polygons.md)

`Polygon`、、およびの各要素を使用すると、 `Polyline` `Circle` マップ上の特定の領域を強調表示できます。 は、 `Polygon` ストロークと塗りつぶしの色を持つことができる、完全に囲まれた形状です。 は、 `Polyline` 領域を完全に囲む線ではありません。 は、 `Circle` マップの円形の領域を強調表示します。

## <a name="geocoding"></a>[ジオコーディング](geocoder.md)

クラスは、 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) オブジェクトに格納されている文字列のアドレスと緯度と経度の座標を変換し [`Position`](xref:Xamarin.Forms.Maps.Position) ます。

## <a name="launch-the-native-map-app"></a>[ネイティブ マップ アプリを起動する](native-map-app.md)

各プラットフォームのネイティブマップアプリは、 Xamarin.Forms クラスによってアプリケーションから起動でき Xamarin.Essentials `Launcher` ます。
