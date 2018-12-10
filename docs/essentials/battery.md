---
title: 'Xamarin.Essentials: バッテリ'
description: このドキュメントで説明する Xamarin.Essentials の Battery クラスを使用すると、デバイスのバッテリに関する情報を確認し、変化を監視できます。
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 5c457bb8ad9796396f24264e27f6762569ea542c
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898860"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: バッテリ

**Battery** クラスでは、デバイスのバッテリ情報を確認し、変更を監視し、デバイスが省電力モードで実行されているかどうかを示す、デバイスの省電力状態に関する情報を提供できます。 デバイスの省電力状態がオンになっている場合、アプリケーションはバックグラウンド処理を避ける必要があります。

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
var level = Battery.ChargeLevel; // returns 0.0 to 1.0 or 1.0 when on AC or no battery.

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

    void Battery_BatteryChanged(object sender, BatteryInfoChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

バッテリで動作するデバイスは、低電力の省電力モードに切り替えることができます。 デバイスが自動的にこのモードに切り替わる場合があります。たとえば、バッテリ残量が 20% を下回ったときなどです。 オペレーティング システムは、バッテリを消耗させる傾向があるアクティビティを減らすことで、省電力モードに対応します。 アプリケーションでは、バックグラウンド処理やその他の電力消費の大きいアクティビティを回避することで、省電力モードがオンになった場合をサポートできます。

静的プロパティ `Battery.EnergySaverStatus` を使用して、デバイスの現在の省電力状態を取得することもできます。

```csharp
// Get energy saver status
var status = Battery.EnergySaverStatus;
```

このプロパティは `EnergySaverStatus` 列挙型のメンバーを返します。それは `On`、`Off`、または `Unknown` です。 プロパティが `On` を返す場合、アプリケーションでは、バックグラウンド処理やその他の電力消費の大きいアクティビティを回避する必要があります。

アプリケーションでは、イベント ハンドラーもインストールする必要があります。 **Battery** クラスでは、省電力状態が変更されたときにトリガーされるイベントが公開されています。

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Batter.EnergySaverStatusChanged += OnEnergySaverStatusChanged;
    }

    private void OnEnergySaverStatusChanged(EnergySaverStatusChangedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

省電力の状態が `On` に変わった場合、アプリケーションはバックグラウンド処理の実行を停止させる必要があります。 状態が `Unknown` または `Off` に変わった場合、アプリケーションはバックグラウンド処理を再開することができます。


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
