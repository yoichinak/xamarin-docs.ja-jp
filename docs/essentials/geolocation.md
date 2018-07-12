---
title: 'Xamarin.Essentials: 地理的位置情報'
description: このドキュメントでは、デバイスの現在の地理座標を取得する Api を提供する、Xamarin.Essentials で地理的位置情報クラスについて説明します。
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 11749107403fc99e1d49b63ee3b50ff105abaa57
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848752"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials: 地理的位置情報

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**地理的位置情報**クラスには、デバイスの現在の地理座標を取得する Api が用意されています。

## <a name="getting-started"></a>作業の開始

アクセスする、**地理的位置情報**機能では、次のプラットフォームに固有の設定が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

粗いと問題の場所のアクセス許可は、必要な Android プロジェクトで構成する必要があります。 さらに、アプリの対象が Android 5.0 (API レベル 21) または取引以降に宣言する必要があります、アプリによってマニフェスト ファイルでハードウェア機能が使用されます。 これは、次の方法で追加できます。

開く、 **AssemblyInfo.cs**ファイル、**プロパティ**フォルダーを追加。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

または、Android マニフェストを更新します。

開く、 **AndroidManifest.xml**ファイル、**プロパティ**フォルダー内の次の追加と、**マニフェスト**ノード。

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **Android マニフェスト**検索、**ために必要なアクセス許可:** 領域とチェック、 **ACCESS_COARSE_LOCATION**と**ACCESS_FINE_LOCATION**アクセス許可。 これは自動的に更新、 **AndroidManifest.xml**ファイル。

# <a name="iostabios"></a>[iOS](#tab/ios)

アプリの**Info.plist**含める必要があります、`NSLocationWhenInUseUsageDescription`デバイスの場所にアクセスするためにキー。

Plist エディターを開き、追加、**プライバシー - 場所ときに使用して利用状況の説明**プロパティと、ユーザーを表示する値を入力します。

または手動でファイルを編集し、以下を追加します。

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

設定する必要があります、`Location`アプリケーションのアクセスを許可します。 これを行うを開く、 **Package.appxmanifest**を選択し、**機能** タブとチェック**場所**します。

-----

## <a name="using-geolocation"></a>地理的位置情報を使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

Geoloation API は、ユーザーに必要な場合のアクセス許可も求められます。

前回正常に取得できます[場所](xref:Xamarin.Essentials.Location)呼び出すことによって、デバイスの`GetLastKnownLocationAsync`メソッド。 これにより、完全なクエリの実行速度は多くの場合は、精度が低下することができます。

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

高度は常に使用できません。 使用できない場合、`Altitude`プロパティがあります`null`または値を 0 にすることがあります。 高度を使用できる場合、値は海面上のメートル単位では。 

現在のデバイスを照会する[場所](xref:Xamarin.Essentials.Location)、座標、`GetLocationAsync`ことができます。 完全に渡すことをお勧め`GeolocationRequest`と`CancellationToken`のため、デバイスの場所を取得するまでに時間がかかる場合があります。

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

## <a name="geolocation-accuracy"></a>地理的位置情報の精度

次の表では、プラットフォームごとの精度を示します。

### <a name="lowest"></a>最低

| プラットフォーム | メートル単位で距離 |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| UWP | 1000 ~ 5000 |

### <a name="low"></a>Low

| プラットフォーム | メートル単位で距離 |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| UWP | 300-3000 |

### <a name="medium-default"></a>Medium (既定値)

| プラットフォーム | メートル単位で距離 |
| --- | --- |
| Android | 100 ~ 500 |
| iOS | 100 |
| UWP | 30 ~ 500 |

### <a name="high"></a>High

| プラットフォーム | メートル単位で距離 |
| --- | --- |
| Android | 0 - 100 |
| iOS | 10 |
| UWP | < = 10 |

### <a name="best"></a>最高

| プラットフォーム | メートル単位で距離 |
| --- | --- |
| Android | 0 - 100 |
| iOS | ~0 |
| UWP | < = 10 |

<a name="calculate-distance" />

## <a name="distance-between-two-locations"></a>2 つの場所の間の距離

[ `Location` ](xref:Xamarin.Essentials.Location)と[ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions)クラス定義`CalculateDistance`2 つの地理的場所間の距離を計算するためのメソッド。 距離は考慮されません道路またはその他の経路、とも呼ばれる、地球の表面上、2 つのポイント間の最短距離だけでは、この計算される、_大圏距離_話題、または、crow 飛行の"と"距離

次に例を示します。

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

`Location`コンス トラクターでは、緯度と経度の引数を持つその順序で。 緯度の値は、赤道より北、子午線の正の値の経度の値は、正の値。 最後の引数を使用して、`CalculateDistance`マイルまたはキロメートル単位を指定します。 `Location`クラスも定義`KilometersToMiles`と`MilesToKilometers`を 2 つのユニット間で変換するメソッド。

## <a name="api"></a>API

- [地理的位置情報のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [地理的位置情報 API のドキュメント](xref:Xamarin.Essentials.Geolocation)
