---
title: 'Xamarin.Essentials: ジオコーディング'
description: Xamarin.Essentials のジオコーディング クラスは、Api を提供両方ジオコードを位置指定の座標の placemark、placemark に geocode 座標を破棄したりします。
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a4d6e4d9b32e665893d82693a3c858630b63d372
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353676"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials: ジオコーディング

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**ジオコーディング**クラスは、Api geocode、placemark 位置座標を placemark に geocode 座標を破棄したりします。

## <a name="getting-started"></a>作業の開始

アクセスする、**ジオコーディング**次のプラットフォーム固有設定の機能が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

追加の設定が必要です。

# <a name="iostabios"></a>[iOS](#tab/ios)

追加の設定が必要です。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

ジオコーディングの機能を使用するには、Bing Maps API キーが必要です。 無料のサインアップ[Bing Maps](https://www.bingmapsportal.com/)アカウント。 **自分のアカウント > キーを**新しいキーを作成し、アプリケーションの種類に基づく情報を入力 (が**向けの Windows アプリ (UWP、8.x、以前のバージョン)** UWP アプリ用)。

呼び出す前に、アプリケーションのライフ サイクルの早い段階で**ジオコーディング**メソッドは、API キーを設定します。

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>ジオコーディングの使用

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

取得する[場所](xref:Xamarin.Essentials.Location)住所の座標。

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

高度は常に使用できません。 使用できない場合、`Altitude`プロパティがあります`null`または値を 0 にすることがあります。 高度を使用できる場合、値は海面上のメートル単位では。

取得する[placemarks](xref:Xamarin.Essentials.Placemark)の既存の座標のセット。

```csharp
try
{
    var lat = 47.673988;
    var lon = -122.121513;

    var placemarks = await Geocoding.GetPlacemarksAsync(lat, lon);

    var placemark = placemarks?.FirstOrDefault();
    if (placemark != null)
    {
        var geocodeAddress =
            $"AdminArea:       {placemark.AdminArea}\n" +
            $"CountryCode:     {placemark.CountryCode}\n" +
            $"CountryName:     {placemark.CountryName}\n" +
            $"FeatureName:     {placemark.FeatureName}\n" +
            $"Locality:        {placemark.Locality}\n" +
            $"PostalCode:      {placemark.PostalCode}\n" +
            $"SubAdminArea:    {placemark.SubAdminArea}\n" +
            $"SubLocality:     {placemark.SubLocality}\n" +
            $"SubThoroughfare: {placemark.SubThoroughfare}\n" +
            $"Thoroughfare:    {placemark.Thoroughfare}\n";

        Console.WriteLine(geocodeAddress);
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

## <a name="distance-between-two-locations"></a>2 つの場所の間の距離

[ `Location` ](xref:Xamarin.Essentials.Location)と[ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions)クラスは、2 つの場所間の距離を計算するメソッドを定義します。 記事をご覧ください[ **Xamarin.Essentials: 地理的位置情報**](geolocation.md#calculate-distance)例についてはします。

## <a name="api"></a>API

- [ジオコーディングのソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [ジオコーディング API ドキュメント](xref:Xamarin.Essentials.Geocoding)
