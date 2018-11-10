---
title: 'Xamarin.Essentials: バッテリ'
description: このドキュメントで説明する Xamarin.Essentials の Battery クラスを使用すると、デバイスのバッテリに関する情報を確認し、変化を監視できます。
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6a14c939064538a405a1fe64061e0bb2e903fedd
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675433"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: バッテリ

![プレリリースの NuGet](~/media/shared/pre-release.png)

**Battery** クラスを使用すると、デバイスのバッテリに関する情報を確認し、変化を監視できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**Battery** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

`Battery` アクセス許可が必要です。Android プロジェクト内で構成する必要があります。 これは次の方法で追加できます。

**[プロパティ]** フォルダーにある **AssemblyInfo.cs** ファイルを開き、以下を追加します。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.BatteryStats)]
```

または、Android マニフェストを追加します。

**[プロパティ]** フォルダーにある **AndroidManifest.xml** ファイルを開き、**manifest** ノードの内部に以下を追加します。

```xml
<uses-permission android:name="android.permission.BATTERY_STATS" />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **[Android マニフェスト]** の下で **[必要なアクセス許可:]** 領域を探し、**Battery** アクセス許可をオンにします。 これにより、**AndroidManifest.xml** ファイルが自動的に更新されます。

# <a name="iostabios"></a>[iOS](#tab/ios)

追加の設定は必要ありません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

追加の設定は必要ありません。

-----

## <a name="using-battery"></a>Battery の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

現在のバッテリに関する情報を確認します。

```csharp
var level = Battery.ChargeLevel; // returns 0.0 to 1.0 or -1.0 if unable to determine.

var state = Battery.State;

switch (state)
{
    case BatteryState.Charging:
        // Currently charging
        break;
    case BatteryState.Full:
        // Battery is full
        break;
    case BatteryState.Discharging:
    case BatteryState.NotCharging:
        // Currently discharging battery or not being charged
        break;
    case BatteryState.NotPresent:
        // Battery doesn't exist in device (desktop computer)
    case BatteryState.Unknown:
        // Unable to detect battery state
        break;
}

var source = Battery.PowerSource;

switch (source)
{
    case BatteryPowerSource.Battery:
        // Being powered by the battery
        break;
    case BatteryPowerSource.AC:
        // Being powered by A/C unit
        break;
    case BatteryPowerSource.Usb:
        // Being powered by USB cable
        break;
    case BatteryPowerSource.Wireless:
        // Powered via wireless charging
        break;
    case BatteryPowerSource.Unknown:
        // Unable to detect power source
        break;
}
```

バッテリのプロパティのいずれかが変化するたびに、イベントがトリガーされます。

```csharp
public class BatteryTest
{
    public BatteryTest()
    {
        // Register for battery changes, be sure to unsubscribe when needed
        Battery.BatteryChanged += Battery_BatteryChanged;
    }

    void Battery_BatteryChanged(object sender, BatteryChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

プラットフォームによる違いはありません。

# <a name="iostabios"></a>[iOS](#tab/ios)

* API をテストするには、デバイスを使用する必要があります。 
* `PowerSource` に対しては、`AC` または `Battery` しか返されません。
* バイブレーションを取り消すことはできません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `PowerSource` に対しては、`AC` または `Battery` しか返されません。

-----

## <a name="api"></a>API

- [Battery のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Battery API のドキュメント](xref:Xamarin.Essentials.Battery)
