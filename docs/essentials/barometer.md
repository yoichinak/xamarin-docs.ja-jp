---
title: Xamarin.Essentials:バロメーター
description: Xamarin.Essentials の Barometer クラスを使用すると、負荷を測定するデバイスのバロメーター センサーを監視できます。
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 5a496fc06732be1cf104cfbaffa8ff4b68c8f564
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "69976434"
---
# <a name="xamarinessentials-barometer"></a>Xamarin.Essentials:バロメーター

**Barometer** クラスを使用すると、負荷を測定するデバイスのバロメーター センサーを監視できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-barometer"></a>バロメーターの使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

バロメーターの機能は、`Start` および `Stop` メソッドを呼び出し、バロメーターの負荷の読み取り値の変化をヘクトパスカル単位でリッスンすることで動作します。 すべての変更は `ReadingChanged` イベントを通じて戻されます。 以下がサンプルの使用方法です。

```csharp

public class BarometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public BarometerTest()
    {
        // Register for reading changes.
        Barometer.ReadingChanged += Barometer_ReadingChanged;
    }

    void Barometer_ReadingChanged(object sender, BarometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Pressure
        Console.WriteLine($"Reading: Pressure: {data.PressureInHectopascals} hectopascals");
    }

    public void ToggleBarometer()
    {
        try
        {
            if (Barometer.IsMonitoring)
              Barometer.Stop();
            else
              Barometer.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="android"></a>[Android](#tab/android)

プラットフォーム固有の実装の詳細はありません。

# <a name="ios"></a>[iOS](#tab/ios)

この API では [CMAltimeter](https://developer.apple.com/documentation/coremotion/cmaltimeter#//apple_ref/occ/cl/CMAltimeter) を使用して気圧の変化を監視します。これは、iPhone 6 以降のデバイスに追加されたハードウェアの機能です。 高度計をサポートしていないデバイスの場合は、`FeatureNotSupportedException` がスローされます。

`SensorSpeed` は、iOS ではサポートされていないため、使用されません。

# <a name="uwp"></a>[UWP](#tab/uwp)

プラットフォーム固有の実装の詳細はありません。

-----

## <a name="api"></a>API

- [Barometer のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Barometer)
- [Barometer API ドキュメント](xref:Xamarin.Essentials.Barometer)
