---
title: Xamarin.Essentials:位置情報
description: このドキュメントでは、デバイスの現在の位置座標を取得する API を提供する Xamarin.Essentials の Geolocation クラスについて説明します。
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: 4ac6344165730bc8c348c16fe8f3a932d4ac3548
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2019
ms.locfileid: "67268626"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials:位置情報

**Geolocation** クラスには、デバイスの現在の位置座標を取得する API が用意されています。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**Geolocation** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

Coarse および Fine Location アクセス許可が必要であり、Android プロジェクトで構成する必要があります。 さらに、アプリの対象が Android 5.0 (API レベル 21) 以降である場合は、アプリがマニフェスト ファイルのハードウェア機能を使用することを宣言する必要があります。 これは次の方法で追加できます。

**[プロパティ]** フォルダーにある **AssemblyInfo.cs** ファイルを開き、以下を追加します。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

または、Android マニフェストを更新します。

**[プロパティ]** フォルダーにある **AndroidManifest.xml** ファイルを開き、**manifest** ノードの内部に以下を追加します。

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **[Android マニフェスト]** の下で **[必要なアクセス許可:]** 領域を探し、 **[ACCESS_COARSE_LOCATION]** および **[ACCESS_FINE_LOCATION]** アクセス許可をオンにします。 これにより、**AndroidManifest.xml** ファイルが自動的に更新されます。

# <a name="iostabios"></a>[iOS](#tab/ios)

デバイスの位置情報にアクセスするには、アプリの **Info.plist** に `NSLocationWhenInUseUsageDescription` キーが含まれる必要があります。

plist エディターを開き、 **[プライバシー - 位置情報 (使用時) の利用状況の説明]** プロパティを追加して、ユーザーに表示する値を入力します。

または、ファイルを手動で編集し、以下を追加して、根拠を更新します。

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>Fill in a reason why your app needs access to location.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

アプリケーションに `Location` アクセス許可を設定する必要があります。 これは、**Package.appxmanifest** を開き、 **[機能]** タブを選択して **[場所]** をオンにすることによって行うことができます。

-----

## <a name="using-geolocation"></a>Geolocation の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

Geolocation API では、必要に応じてユーザーにアクセス許可も求められます。

`GetLastKnownLocationAsync` メソッドを呼び出すことにより、デバイスの最後の既知の[場所](xref:Xamarin.Essentials.Location)を取得できます。 多くの場合、この方が完全なクエリを行うより速くわかりますが、精度が低下することがあります。

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
catch (FeatureNotEnabledException fneEx)
{
    // Handle not enabled on device exception
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

高度は常に使用できるとは限りません。 使用できない場合、`Altitude` プロパティは `null` または 0 になることがあります。 高度を使用できる場合、値は海抜メートル単位です。 

現在のデバイスの[場所](xref:Xamarin.Essentials.Location)の座標を照会するには、`GetLocationAsync` を使用できます。 デバイスの場所を取得するには時間がかかる場合があるので、完全な `GeolocationRequest` と `CancellationToken` を渡すのが最善です。

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
catch (FeatureNotEnabledException fneEx)
{
    // Handle not enabled on device exception
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

## <a name="geolocation-accuracy"></a>Geolocation の精度

次の表では、プラットフォームごとの精度を示します。

### <a name="lowest"></a>最低

| プラットフォーム | 距離 (メートル単位) |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| UWP | 1000 ～ 5000 |

### <a name="low"></a>Low

| プラットフォーム | 距離 (メートル単位) |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| UWP | 300 ～ 3000 |

### <a name="medium-default"></a>中 (既定値)

| プラットフォーム | 距離 (メートル単位) |
| --- | --- |
| Android | 100 ～ 500 |
| iOS | 100 |
| UWP | 30 ～ 500 |

### <a name="high"></a>High

| プラットフォーム | 距離 (メートル単位) |
| --- | --- |
| Android | 0 ～ 100 |
| iOS | 10 |
| UWP | 10 以下 |

### <a name="best"></a>最高

| プラットフォーム | 距離 (メートル単位) |
| --- | --- |
| Android | 0 ～ 100 |
| iOS | ~0 |
| UWP | 10 以下 |

<a name="calculate-distance" />

## <a name="detecting-mock-locations"></a>擬似ロケーションの検出
一部のデバイスは、プロバイダーからの擬似ロケーションを返します。擬似ロケーションを提供するアプリケーションによって擬似ロケーションを返すこともあります。 [`Location`](xref:Xamarin.Essentials.Location) で `IsFromMockProvider` を使用することでこれを検出できます。

```csharp
var request = new GeolocationRequest(GeolocationAccuracy.Medium);
var location = await Geolocation.GetLocationAsync(request);

if (location != null)
{
    if(location.IsFromMockProvider)
    {
        // location is from a mock provider
    }
}
```

## <a name="distance-between-two-locations"></a>2 つの場所の間の距離

[`Location`](xref:Xamarin.Essentials.Location) クラスおよび [`LocationExtensions`](xref:Xamarin.Essentials.LocationExtensions) クラスでは、2 つの地理的な場所の間の距離を計算できる `CalculateDistance` メソッドが定義されています。 この計算では道路またはその他の経路は考慮されず、あくまでも地球の表面に沿った 2 点間の最短距離なので、"_大圏距離_" または "直線距離" とも呼ばれます。

次に例を示します。

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

`Location` コンストラクターは、緯度引数と経度引数をこの順序で受け取ります。 正の緯度値は北半球を示し、正の緯度値は東半球を示します。 マイルまたはキロメートルを指定するには、`CalculateDistance` に対する最後の引数を使用します。 `UnitConverters` クラスでは、2 つの単位の間で変換を行う `KilometersToMiles` および `MilesToKilometers` メソッドも定義されています。

## <a name="api"></a>API

- [Geolocation のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [Geolocation API のドキュメント](xref:Xamarin.Essentials.Geolocation)
