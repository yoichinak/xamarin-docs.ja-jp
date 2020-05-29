---
title: Xamarin.Formsマップジオコーディング
description: この記事では、を使用して geocode マップデータと reverse コードマップデータを逆にする方法について説明し Xamarin.Forms ます。Geocoder クラスをマップします。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fe099235857f6bd0531539e3aa84e41bf59b50ba
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139868"
---
# <a name="xamarinforms-map-geocoding"></a>Xamarin.Formsマップジオコーディング

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)名前空間は、 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) オブジェクトに格納されている文字列のアドレスと緯度、経度の座標を変換するクラスを提供し [`Position`](xref:Xamarin.Forms.Maps.Position) ます。 構造体の詳細については [`Position`](xref:Xamarin.Forms.Maps.Position) 、「[マップの位置と距離](position-distance.md)」を参照してください。

## <a name="geocode-an-address"></a>Geocode のアドレス

インスタンスを作成 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) し、インスタンスでメソッドを呼び出すことによって、市区町村を緯度と経度の座標にコード化でき [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) `Geocoder` ます。

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

IEnumerable<Position> approximateLocations = await geoCoder.GetPositionsForAddressAsync("Pacific Ave, San Francisco, California");
Position position = approximateLocations.FirstOrDefault();
string coordinates = $"{position.Latitude}, {position.Longitude}";
```

メソッドは、 [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) アドレスを表す引数を受け取り、 `string` アドレスを表すオブジェクトのコレクションを非同期的に返し [`Position`](xref:Xamarin.Forms.Maps.Position) ます。

## <a name="reverse-geocode-an-address"></a>Geocode の逆引き

インスタンスを作成 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) し、インスタンスでメソッドを呼び出すことによって、緯度と経度の座標を住所に逆方向に変換でき [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) `Geocoder` ます。

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

Position position = new Position(37.8044866, -122.4324132);
IEnumerable<string> possibleAddresses = await geoCoder.GetAddressesForPositionAsync(position);
string address = possibleAddresses.FirstOrDefault();
```

メソッドは、 [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) [`Position`](xref:Xamarin.Forms.Maps.Position) 緯度と経度の座標で構成される引数を受け取り、その位置の近くにあるアドレスを表す文字列のコレクションを非同期に返します。

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.Formsマップの位置と距離](position-distance.md)
- [Geocoder API](xref:Xamarin.Forms.Maps.Geocoder)
