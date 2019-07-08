---
title: Xamarin.Essentials:ジオコーディング
description: Xamarin.Essentials の Geocoding クラスでは、placemark を位置座標にジオコーディングするための API と、逆に座標を placemark にジオコーディングする API の両方が提供されています。
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/28/2019
ms.custom: video
ms.openlocfilehash: 157eb3116f09268790036f8983543114e7a58276
ms.sourcegitcommit: 4a1520dee7759f8355ea65c8bb3d1bac8ba58122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66354108"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials:ジオコーディング

**Geocoding** クラスでは、placemark を位置座標にジオコーディングするための API と、逆に座標を placemark にジオコーディングする API が提供されています。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**Geocoding** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

追加の設定は必要ありません。

# <a name="iostabios"></a>[iOS](#tab/ios)

追加の設定は必要ありません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

ジオコーディング機能を使用するには、Bing Maps API キーが必要です。 無料の [Bing Maps](https://www.bingmapsportal.com/) アカウントにサインアップします。 **[My account]\(マイ アカウント\) > [My keys]\(マイ キー\)** で、新しいキーを作成し、アプリケーションの種類に基づいて情報を入力します (UWP アプリの場合は、 **[Public Windows App (UWP, 8.x, and earlier)]\(パブリック Windows アプリ (UWP、8.x 以前向け)\)** にする必要があります)。

**Geocoding** のメソッドを呼び出す前のアプリケーションのライフサイクルの早い段階で、API キーを設定します (UWP でのみ可能です)。

```csharp
Platform.MapServiceToken = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Geocoding の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

住所の[場所](xref:Xamarin.Essentials.Location)の座標の取得:

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

高度は常に使用できるとは限りません。 使用できない場合、`Altitude` プロパティは `null` または 0 になることがあります。 高度を使用できる場合、値は海抜メートル単位です。

## <a name="using-reverse-geocoding"></a>逆ジオコーディングの使用

逆ジオコーディングは、既存の座標セットに対する [placemarks](xref:Xamarin.Essentials.Placemark) を取得するプロセスです。

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

[`Location`](xref:Xamarin.Essentials.Location) クラスおよび [`LocationExtensions`](xref:Xamarin.Essentials.LocationExtensions) クラスでは、2 つの場所の間の距離を計算するメソッドが定義されています。 例については、[**Xamarin.Essentials:位置情報**](geolocation.md#calculate-distance)に関する記事をご覧ください。

## <a name="api"></a>API

- [Geocoding のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Geocoding API のドキュメント](xref:Xamarin.Essentials.Geocoding)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Geocoding-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
