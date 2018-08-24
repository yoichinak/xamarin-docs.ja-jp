---
title: 'Xamarin.Essentials: 磁力計'
description: Xamarin.Essentials で磁力計クラスでは、地球の磁場の基準としたデバイスの向きを示すデバイスの磁力計センサーを監視できます。
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3827b9a57ec2667a8716f5b56bfb4631b979d43a
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353790"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials: 磁力計

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**磁力計**クラスでは、地球の磁場の基準としたデバイスの向きを示すデバイスの磁力計センサーを監視することができます。

## <a name="using-magnetometer"></a>磁力計を使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

磁力計機能が呼び出すことによって、`Start`と`Stop`磁力計への変更をリッスンするメソッド。 加えた変更は通過して送信、`ReadingChanged`イベント。 使用例を次に示します。

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

Microteslas では、すべてのデータが返されます。

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [磁力計のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Magnetometer)
- [磁力計 API ドキュメント](xref:Xamarin.Essentials.Magnetometer)
