---
title: 'Xamarin.Essentials: バッテリ'
description: このドキュメントでは、Xamarin.Essentials で、デバイスのバッテリに関する情報との変更の監視を確認することができますにバッテリ クラスについて説明します。
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1deafed85e9400bf7d4592fc06f71c22cc0015f0
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353455"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: バッテリ

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**バッテリ**クラスを使用して、デバイスのバッテリに関する情報との変更の監視を確認できます。

## <a name="getting-started"></a>作業の開始

アクセスする、**バッテリ**次のプラットフォーム固有設定の機能が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

`Battery`アクセス許可は必須であり、Android プロジェクトで構成する必要があります。 これは、次の方法で追加できます。

開く、 **AssemblyInfo.cs**ファイル、**プロパティ**フォルダーを追加。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.BatteryStats)]
```

または、Android マニフェストを更新します。

開く、 **AndroidManifest.xml**ファイル、**プロパティ**フォルダー内の次の追加と、**マニフェスト**ノード。

```xml
<uses-permission android:name="android.permission.BATTERY_STATS" />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **Android マニフェスト**検索、**ために必要なアクセス許可:** 領域とチェック、**バッテリ**権限。 これは自動的に更新、 **AndroidManifest.xml**ファイル。

# <a name="iostabios"></a>[iOS](#tab/ios)

追加の設定が必要です。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

追加の設定が必要です。

-----

## <a name="using-battery"></a>バッテリを使用します。

クラスで Xamarin.Essentials への参照を追加します。

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

バッテリのプロパティのいずれかが変更されるたびにイベントがトリガーされます。

```csharp
public class BatteryTest
{
    public BatteryTest()
    {
        // Register for battery changes, be sure to unsubscribe when needed
        Battery.BatteryChanged += Battery_BatteryChanged;
    }

    void Battery_BatteryChanged(BatteryChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

## <a name="platform-differences"></a>プラットフォームの違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

プラットフォームの違いはありません。

# <a name="iostabios"></a>[iOS](#tab/ios)

* デバイスは、Api をテストするために使用する必要があります。 
* しか返さない`AC`または`Battery`の`PowerSource`します。
* 振動をキャンセルすることはできません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* しか返さない`AC`または`Battery`の`PowerSource`します。

-----

## <a name="api"></a>API

- [バッテリのソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [バッテリの API ドキュメント](xref:Xamarin.Essentials.Battery)
