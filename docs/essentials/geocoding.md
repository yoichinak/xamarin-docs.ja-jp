---
title: 'Xamarin.Essentials: ジオコード化'
description: ジオコード化クラス Xamarin.Essentials では、提供 Api 両方 geocode を位置指定の座標に placemark および、placemark に geocode 座標を破棄します。
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 063adba82d96e7fcc64d7ec49a0c0133e1cef8ef
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080327"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials: ジオコード化

![プレリリース NuGet](~/media/shared/pre-release.png)

**ジオコード化**クラスは、Api geocode 位置指定の座標に placemark、placemark を逆に geocode coordincates とします。

## <a name="getting-started"></a>作業の開始

アクセスする、**ジオコード化**次のプラットフォーム固有のセットアップの機能が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

追加の設定が必要です。

# <a name="iostabios"></a>[iOS](#tab/ios)

追加の設定が必要です。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

ジオコード化 %1%2 を使用するのには、Bing maps API キーが必要です。 無料のサインアップ[Bing Maps](https://www.bingmapsportal.com/)アカウント。 **マイ アカウント > キー**新しいキーを作成し、アプリケーションの種類に基づいて情報を入力する (必要があります**パブリックの Windows アプリ (UWP、8.x、以前のバージョン)** UWP アプリの)。

呼び出す前に、アプリケーションのライフ サイクルの早い段階で**ジオコード化**メソッドは、API キーを設定します。

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>ジオコード化を使用します。

クラスの Xamarin.Essentials への参照を追加します。

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
    // Handle exception that may have occured in geocoding
}
```

高度は常に使用できません。 使用できない場合、`Altitude`プロパティがあります`null`または値を 0 にすることがあります。 高度が使用可能な場合は、値は、sea レベルより上の上メートル単位でです。 

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

[ `Location` ](xref:Xamarin.Essentials.Location)と[ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions)クラスは、2 つの場所の間の距離を計算するメソッドを定義します。 記事を参照して[ **Xamarin.Essentials: 地理的位置情報**](geolocation.md#calculate-distance)例についてはします。

## <a name="api"></a>API

- [ジオコード化ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [ジオコード化 API ドキュメント](xref:Xamarin.Essentials.Geocoding)
