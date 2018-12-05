---
title: Xamarin.Essentials マップ
description: Xamarin.Essentials の Maps クラスを使用すると、アプリケーションによってインストールされているマップ アプリケーションを使用して、特定の場所または placemark を開くことができます。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: fb4cbc2fd334d574abc57a3359fa346bc6795408
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674775"
---
# <a name="xamarinessentials-maps"></a>Xamarin.Essentials: マップ

![プレリリースの NuGet](~/media/shared/pre-release.png)

**Maps** クラスを使用すると、アプリケーションによってインストールされているマップ アプリケーションを使用して、特定の場所または placemark を開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-maps"></a>マップの使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

マップの機能は、`OpenAsync` メソッドを、開く `Location` または `Placemark` と省略可能な `MapsLaunchOptions` と共に呼び出すことで動作します。

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(location, options);
    }
}
```

`Placemark` と共に開く場合、次の情報が必要です。

- `CountryName`
- `AdminArea`
- `Thoroughfare`
- `Locality`

```csharp
public class MapsTest
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
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(placemark, options);
    }
}
```

## <a name="extension-methods"></a>拡張メソッド

`Location` または `Placemark` への参照が既にある場合は、省略可能な `MapsLaunchOptions` と共に組み込みの拡張メソッド `OpenMapsAsync` を使用することができます。

```csharp
public class MapsTest
{
    public async Task OpenPlacemarkOnMaps(Placemark placemark)
    {
        await placemark.OpenMapsAsync();
    }
}
```

## <a name="directions-mode"></a>ルート案内

`MapsLaunchOptions` なしで `OpenMapsAsync` を呼び出した場合、指定した場所でマップが起動します。 必要に応じて、デバイスの現在位置から計算されるナビゲーション ルートを取得することができます。 これは、`MapsLaunchOptions` の `MapDirectionsMode` を設定することによって行います。

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapsLaunchOptions { MapDirectionsMode = MapDirectionsMode.Driving };

        await Maps.OpenAsync(location, options);
    }
}
```

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

- `MapDirectionsMode` ではサイクリング、ドライビング、徒歩がサポートされています。

# <a name="iostabios"></a>[iOS](#tab/ios)

- `MapDirectionsMode` ではドライビング、路線、徒歩がサポートされています。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- `MapDirectionsMode` ではドライビング、路線、徒歩がサポートされています。

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

- [Maps のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Maps)
- [Maps API ドキュメント](xref:Xamarin.Essentials.Maps)
