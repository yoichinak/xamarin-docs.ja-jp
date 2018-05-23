---
title: Xamarin.Essentials 地理的位置情報
description: 地理的位置情報クラスは、デバイスの現在の地理座標を取得するための Api を提供します。
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: bf0fa7d2caf7c8857bc1272f4471def04100383f
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/23/2018
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials 地理的位置情報

![プレリリース NuGet](~/media/shared/pre-release.png)

**Geolocation**クラスには、デバイスの現在の地理座標を取得するための Api が用意されています。

## <a name="getting-started"></a>作業の開始

アクセスする、**地理的位置情報**機能、次のプラットフォーム固有のセットアップが必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

粗いと問題の場所のアクセス許可は、必要な Android プロジェクトで構成する必要があります。 さらに、アプリケーションが Android 5.0 (API レベル 21) を対象または以降では、する必要がありますを宣言する場合、アプリは、マニフェスト ファイルでのハードウェア機能を使用します。 これは、次の方法で追加できます。

開く、 **AssemblyInfo.cs**下にあるファイル、**プロパティ**フォルダーを追加。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

または、Android のマニフェストを更新します。

開く、 **AndroidManifest.xml**下にあるファイル、**プロパティ**フォルダー内の次の追加と、**マニフェスト**ノード。

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **Android マニフェスト**検索、**権限が必要:** 領域とチェック、 **ACCESS_COARSE_LOCATION**と**ACCESS_FINE_LOCATION**アクセス許可。 これは自動的に更新、 **AndroidManifest.xml**ファイル。

# <a name="iostabios"></a>[iOS](#tab/ios)

アプリの**Info.plist**含める必要があります、`NSLocationWhenInUseUsageDescription`デバイスの場所にアクセスするためにキー。

Plist エディターを開き、追加、**プライバシー - 場所ときに使用する使用状況の説明**プロパティと、ユーザーを表示する値に入力します。

または手動でファイルを編集し、次を追加します。

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

設定する必要があります、`Location`アプリケーションに対するアクセス許可。 開くことによってこれ行う、 **Package.appxmanifest**を選択し、**機能**タブとチェック**場所**です。

-----

## <a name="using-geolocation"></a>地理的位置情報を使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

Geoloation API には、必要な場合に、アクセス許可をユーザーをプロンプトもされます。

前回正常に取得できます[場所](xref:Xamarin.Essentials.Location)呼び出すことによって、デバイスの`GetLastKnownLocationAsync`メソッドです。 これは、完全なクエリを実行速度が、多くの場合が、精度が低下することができます。

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

現在のデバイスを照会する[場所](xref:Xamarin.Essentials.Location)、座標、`GetLocationAsync`使用できます。 完全に渡すことをお勧め`GeolocationRequest`と`CancellationToken`ので、デバイスの場所を取得する時間がかかる場合があります。

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

次の表は、プラットフォームごとの精度を示します。

### <a name="lowest"></a>最低

| プラットフォーム | 距離 (メートル) |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| UWP | 1000 ~ 5000 |

### <a name="low"></a>Low

| プラットフォーム | 距離 (メートル) |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| UWP | 300 3000 |

### <a name="medium-default"></a>Medium (既定)

| プラットフォーム | 距離 (メートル) |
| --- | --- |
| Android | 100 ~ 500 |
| iOS | 100 |
| UWP | 30 ~ 500 |

### <a name="high"></a>High

| プラットフォーム | 距離 (メートル) |
| --- | --- |
| Android | 0 ~ 100 |
| iOS | 10 |
| UWP | < 10 を = |

### <a name="best"></a>最適な

| プラットフォーム | 距離 (メートル) |
| --- | --- |
| Android | 0 ~ 100 |
| iOS | ~0 |
| UWP | < 10 を = |

## <a name="api"></a>API

- [地理的位置情報のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [地理的位置情報 API のドキュメント](xref:Xamarin.Essentials.Geolocation)
