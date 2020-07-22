---
title: Xamarin.Essentials:磁力計
description: Xamarin.Essentials の Magnetometer クラスを使用すると、地球の磁場を基準にしてデバイスの向きを示すデバイスの磁力計センサーを監視できます。
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b01bc1fd9c65186952635c5f472b1ac6beb0c9bd
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802289"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials:磁力計

**Magnetometer** クラスを使用すると、地球の磁場を基準にしてデバイスの向きを示すデバイスの磁力計センサーを監視できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-magnetometer"></a>Magnetometer の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

Magnetometer の機能は、磁力計の変化をリッスンする `Start` および `Stop` メソッドを呼び出すことで動作します。 すべての変更は `ReadingChanged` イベントを通じて戻されます。 以下がサンプルの使用方法です。

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometer_ReadingChanged(object sender, MagnetometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process MagneticField X, Y, and Z
        Console.WriteLine($"Reading: X: {data.MagneticField.X}, Y: {data.MagneticField.Y}, Z: {data.MagneticField.Z}");
    }

    public void ToggleMagnetometer()
    {
        try
        {
            if (Magnetometer.IsMonitoring)
              Magnetometer.Stop();
            else
              Magnetometer.Start(speed);
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

すべてのデータは µT (マイクロテスラ) で返されます。

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Magnetometer のソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Magnetometer)
- [Magnetometer API のドキュメント](xref:Xamarin.Essentials.Magnetometer)
