---
title: Xamarin. Forms マップジオコーディング
description: この記事では、geocode を使用して geocode マップデータを逆に変換する方法について説明します。
ms.prod: xamarin
ms.assetid: DE7DB31A-8921-4614-8B49-DAEF1E7B03B3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/22/2019
ms.openlocfilehash: 6df7a2e0056cd2d448206b56e1e9d2600b342b3b
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490260"
---
# <a name="xamarinforms-map-geocoding"></a>Xamarin. Forms マップジオコーディング

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)名前空間は、 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder)クラスを提供します。このクラスは、 [`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトに格納されている文字列アドレスと緯度座標と経度座標を変換します。 [`Position`](xref:Xamarin.Forms.Maps.Position)構造体の詳細については、「[マップの位置と距離](position-distance.md)」を参照してください。

## <a name="geocode-an-address"></a>Geocode のアドレス

[`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder)インスタンスを作成し `Geocoder` インスタンスで[`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*)メソッドを呼び出すことによって、市区町村を緯度および経度の座標にコード化できます。

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

IEnumerable<Position> approximateLocations = await geoCoder.GetPositionsForAddressAsync("Pacific Ave, San Francisco, California");
Position position = approximateLocations.FirstOrDefault();
string coordinates = $"{position.Latitude}, {position.Longitude}";
```

[`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*)メソッドは、アドレスを表す `string` 引数を受け取り、アドレスを表す[`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトのコレクションを非同期に返します。

## <a name="reverse-geocode-an-address"></a>Geocode の逆引き

[`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder)インスタンスを作成し、`Geocoder` インスタンスで[`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*)メソッドを呼び出すことによって、緯度と経度の座標を住所に逆方向に変換できます。

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

Position position = new Position(37.8044866, -122.4324132);
IEnumerable<string> possibleAddresses = await geoCoder.GetAddressesForPositionAsync(position);
string address = possibleAddresses.FirstOrDefault();
```

[`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*)メソッドは、緯度と経度の座標で構成される[`Position`](xref:Xamarin.Forms.Maps.Position)引数を受け取り、その位置の近くにあるアドレスを表す文字列のコレクションを非同期に返します。

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin. フォームマップの位置と距離](position-distance.md)
- [Geocoder API](xref:Xamarin.Forms.Maps.Geocoder)
