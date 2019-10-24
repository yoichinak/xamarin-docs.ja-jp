---
title: Xamarin. Forms マップジオコーディング
description: この記事では、geocode を使用して geocode マップデータを逆に変換する方法について説明します。
ms.prod: xamarin
ms.assetid: DE7DB31A-8921-4614-8B49-DAEF1E7B03B3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/22/2019
ms.openlocfilehash: ce1f6751c0381ed41058784fbea3ebedefbdac6d
ms.sourcegitcommit: e4c23187874488ff55794d0e81a9bba30d2c2cd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2019
ms.locfileid: "72778794"
---
# <a name="xamarinforms-map-geocoding"></a>Xamarin. Forms マップジオコーディング

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[@No__t_1](xref:Xamarin.Forms.Maps.Geocoder)クラスを使用すると、 [`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトに格納されている文字列アドレスと緯度座標と経度座標を変換できます。

## <a name="geocode-an-address"></a>Geocode のアドレス

[@No__t_1](xref:Xamarin.Forms.Maps.Geocoder)インスタンスを作成し `Geocoder` インスタンスで[`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*)メソッドを呼び出すことによって、市区町村を緯度および経度の座標にコード化できます。

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder;

IEnumerable<Position> approximateLocations = await geoCoder.GetPositionsForAddressAsync("Pacific Ave, San Francisco, California");
Position position = approximateLocations.FirstOrDefault();
string coordinates = $"{position.Latitude}, {position.Longitude}";
```

[@No__t_1](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*)メソッドは、アドレスを表す `string` 引数を受け取り、アドレスを表す[`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトのコレクションを非同期に返します。

## <a name="reverse-geocode-an-address"></a>Geocode の逆引き

[@No__t_1](xref:Xamarin.Forms.Maps.Geocoder)インスタンスを作成し、`Geocoder` インスタンスで[`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*)メソッドを呼び出すことによって、緯度と経度の座標を住所に逆方向に変換できます。

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder;

Position position = new Position(37.8044866, -122.4324132);
IEnumerable<string> possibleAddresses = await geoCoder.GetAddressesForPositionAsync(position);
string address = possibleAddresses.FirstOrDefault();
```

[@No__t_1](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*)メソッドは、緯度と経度の座標で構成される[`Position`](xref:Xamarin.Forms.Maps.Position)引数を受け取り、その位置の近くにあるアドレスを表す文字列のコレクションを非同期に返します。

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Geocoder API](xref:Xamarin.Forms.Maps.Geocoder)