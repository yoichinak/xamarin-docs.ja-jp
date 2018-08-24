---
title: Xamarin.Essentials マップ
description: Xamarin.Essentials でマップ クラスは、特定の場所または placemark にインストールされているマップ アプリケーションを開くためのアプリケーションを使用できます。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 445e2da84e9a9aaf1ce4d836af11cfba963b8cbb
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353944"
---
# <a name="xamarinessentials-maps"></a>Xamarin.Essentials: マップ

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**マップ**クラスは、特定の場所または placemark にインストールされているマップ アプリケーションを開くためのアプリケーションを使用できます。

## <a name="using-maps"></a>マップを使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

マップ機能が呼び出すことによって、`OpenAsync`メソッドを`Location`または`Placemark`でオプションを開く`MapsLaunchOptions`します。

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

開くときに、`Placemark`次の情報が必要です。

* `CountryName`
* `AdminArea`
* `Thoroughfare`
* `Locality`

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

参照が既にある場合、`Location`または`Placemark`組み込みの拡張メソッドを使用する`OpenMapsAsync`でオプション`MapsLaunchOptions`:

```csharp
public class MapsTest
{
    public async Task OpenPlacemarkOnMaps(Placemark placemark)
    {
        await placemark.OpenMapsAsync();
    }
}
```

## <a name="platform-differences"></a>プラットフォームの違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `MapDirectionsMode` サポートされていませんし、影響を与えません。

# <a name="iostabios"></a>[iOS](#tab/ios)

* `MapDirectionsMode` マップ アプリを開いたときに、既定の方向のモードを設定します。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `MapDirectionsMode` サポートされていませんし、影響を与えません。

--------------

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android を使用して、 `geo:` Uri スキームをデバイス上のマップ アプリケーションを起動します。 ユーザーがこの Uri スキームをサポートする既存のアプリから選択を求めるこのことができます。  このスキームをサポートする Google Maps を使用した Xamarin.Essentials がテストされます。

# <a name="iostabios"></a>[iOS](#tab/ios)

プラットフォーム固有の実装詳細はありません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

プラットフォーム固有の実装詳細はありません。

--------------

## <a name="api"></a>API

- [マップのソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Maps)
- [Maps API のドキュメント](xref:Xamarin.Essentials.Maps)
