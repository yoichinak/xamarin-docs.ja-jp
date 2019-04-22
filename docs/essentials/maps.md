---
title: Xamarin.Essentials の Map
description: Xamarin.Essentials の Map クラスを使用すると、アプリケーションによってインストールされているマップ アプリケーションを使用して、特定の場所または placemark を開くことができます。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
ms.openlocfilehash: c0875534d88ea5b66b3072c35b9d38894fe98934
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870028"
---
# <a name="xamarinessentials-map"></a>Xamarin.Essentials:マップ

**Map** クラスを使用すると、アプリケーションによってインストールされているマップ アプリケーションを使用して、特定の場所または placemark を開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-map"></a>Map の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

Map 機能は、`OpenAsync` メソッドを、開く `Location` または `Placemark` と省略可能な `MapLaunchOptions` と共に呼び出すことで動作します。

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapLaunchOptions { Name = "Microsoft Building 25" };

        await Map.OpenAsync(location, options);
    }
}
```

`Placemark` と共に開く場合、次の情報が必要です。

- `CountryName`
- `AdminArea`
- `Thoroughfare`
- `Locality`

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var placemark = new Placemark
            {
                CountryName = "United States",
                AdminArea = "WA",
                Thoroughfare = "Microsoft Building 25",
                Locality = "Redmond"
            };
        var options =  new MapLaunchOptions { Name = "Microsoft Building 25" };

        await Map.OpenAsync(placemark, options);
    }
}
```

## <a name="extension-methods"></a>拡張メソッド

`Location` または `Placemark` への参照が既にある場合は、省略可能な `MapLaunchOptions` と共に組み込みの拡張メソッド `OpenMapAsync` を使用することができます。

```csharp
public class MapTest
{
    public async Task OpenPlacemarkOnMap(Placemark placemark)
    {
        await placemark.OpenMapAsync();
    }
}
```

## <a name="directions-mode"></a>ルート案内

`MapLaunchOptions` なしで `OpenMapAsync` を呼び出した場合、指定した場所でマップが起動します。 必要に応じて、デバイスの現在位置から計算されるナビゲーション ルートを取得することができます。 これは、`MapLaunchOptions` の `NavigationMode` を設定することによって行います。

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapLaunchOptions { NavigationMode = NavigationMode.Driving };

        await Map.OpenAsync(location, options);
    }
}
```

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

- `NavigationMode` ではサイクリング、ドライビング、徒歩がサポートされています。

# <a name="iostabios"></a>[iOS](#tab/ios)

- `NavigationMode` ではドライビング、路線、徒歩がサポートされています。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- `NavigationMode` ではドライビング、路線、徒歩がサポートされています。

--------------

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android では、URI スキーム `geo:` を使用してデバイス上のマップ アプリケーションを起動します。 これにより、この URI スキームをサポートしている既存のアプリから選択するよう、ユーザーが求められる場合があります。  Xamarin.Essentials は、このスキームをサポートしている Google マップを使用してテストされます。

# <a name="iostabios"></a>[iOS](#tab/ios)

プラットフォーム固有の実装の詳細はありません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

プラットフォーム固有の実装の詳細はありません。

--------------

## <a name="api"></a>API

- [Map のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Map)
- [Map API のドキュメント](xref:Xamarin.Essentials.Map)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Maps-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
