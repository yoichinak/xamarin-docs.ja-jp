---
title: Xamarin.Essentials 磁力計
description: 磁力計クラスでは、地球の磁場の基準としたデバイスの向きを示すデバイスの磁力計センサーを監視できます。
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: e43834756e6a582bd0fd30a5655da8d087a971b7
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials 磁力計

![プレリリース NuGet](~/media/shared/pre-release.png)

**磁力計**クラスでは、地球の磁場の基準としたデバイスの向きを示すデバイスの磁力計センサーを監視することができます。

## <a name="using-magnetometer"></a>磁力計の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

磁力計機能が呼び出すことによって、`Start`と`Stop`磁力計への変更をリッスンするメソッド。 加えた変更はを通じて送信、`ReadingChanged`イベント。 サンプルの使用方法を次に示します。

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometerr_ReadingChanged(MagnetometerChangedEventArgs e)
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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[センサー速度](xref:Xamarin.Essentials.SensorSpeed)

- **最も高速な**– (UI スレッドで返されるとは限りません) できる限り高速のセンサー データを取得します。
- **ゲーム**– ゲーム (UI スレッドで返されるとは限りません) に適したを評価します。
- **標準**– 既定のレートが画面の向きの変更に適してします。
- **Ui** – 一般的なユーザー インターフェイスの適切なを評価します。

## <a name="api"></a>API

- [磁力計ソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/Magnetometer)
- [磁力計 API ドキュメント](xref:Xamarin.Essentials.Magnetometer)
