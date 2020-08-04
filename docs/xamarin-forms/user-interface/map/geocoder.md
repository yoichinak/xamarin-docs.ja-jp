---
title: Xamarin.Formsマップジオコーディング
description: この記事では、を使用して geocode マップデータと reverse コードマップデータを逆にする方法について説明し Xamarin.Forms ます。Geocoder クラスをマップします。
ms.prod: xamarin
ms.assetid: DE7DB31A-8921-4614-8B49-DAEF1E7B03B3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/22/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7e92385f3a82c2d12881be4ff3e54bc47533696d
ms.sourcegitcommit: ea2abdc789d0e292c3e1700a2b53b92097e0e542
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517494"
---
# <a name="no-locxamarinforms-map-geocoding"></a>Xamarin.Formsマップジオコーディング

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)名前空間は、 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) オブジェクトに格納されている文字列のアドレスと緯度、経度の座標を変換するクラスを提供し [`Position`](xref:Xamarin.Forms.Maps.Position) ます。 構造体の詳細については [`Position`](xref:Xamarin.Forms.Maps.Position) 、「[マップの位置と距離](position-distance.md)」を参照してください。

> [!NOTE]
> 代替のジオコーディング API は avalible に Xamarin.Essentials あります。 Api は、 Xamarin.Essentials `Geocoding` この api によって返される文字列ではなく、アドレスをジオコーディングするときに、構造化されたアドレスデータを提供します。 詳細については、「 [ Xamarin.Essentials ジオコーディング](~/essentials/geocoding.md)」を参照してください。

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
