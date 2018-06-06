---
title: 'Xamarin.Essentials: ジオコード化'
description: ジオコード化クラス Xamarin.Essentials では、提供 Api 両方 geocode を位置指定の座標に placemark および、placemark に geocode 座標を破棄します。
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 0b6cbf9ee5621466285656a5efee68ccc2c85211
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783029"
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

ジオコード化 %1%2 を使用するのには、Bing maps API キーが必要です。 無料のサインアップ[Bing Maps](https://www.bingmapsportal.com/)アカウント。 **マイ アカウント > マイ キー**新しいキーを作成し、アプリケーションの種類に基づいて情報を入力します。

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
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

## <a name="api"></a>API

- [ジオコード化ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [ジオコード化 API ドキュメント](xref:Xamarin.Essentials.Geocoding)
