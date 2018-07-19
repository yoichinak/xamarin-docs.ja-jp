---
title: 'Xamarin.Essentials: 加速度計'
description: Xamarin.Essentials で加速度計クラスでは、次の 3 つの次元空間内のデバイスの高速化を示すデバイスの加速度計センサーを監視できます。
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 15e2cb69806f281e88e226b7bcd87a20e149d508
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947310"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials: 加速度計

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**加速度計**クラスでは、次の 3 つの次元空間内のデバイスの高速化を示すデバイスの加速度計センサーを監視できます。

## <a name="using-accelerometer"></a>加速度計を使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

加速度計機能が呼び出すことによって、`Start`と`Stop`アクセラレータへの変更をリッスンするメソッド。 加えた変更は通過して送信、`ReadingChanged`イベント。 使用例を次に示します。

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Acceleration.X}, Y: {data.Acceleration.Y}, Z: {data.Acceleration.Z}");
        // Process Acceleration X, Y, and Z
    }

    public void ToggleAcceleromter()
    {
        try
        {
            if (Accelerometer.IsMonitoring)
              Accelerometer.Stop();
            else
              Accelerometer.Start(speed);
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

加速度計の測定値を G. で示されますG が gravitation の単位を以下に、地球の重力フィールドに作用する強制 (9.81 m/s ^2)。

座標システムは、その既定の向きで、電話の画面を基準と定義されます。 デバイスの画面の向きが変更されたときに、軸は交換されません。

X 軸は水平方向と、右側にポイントして、Y 軸は縦方向上を向きし、画面の前面の外側をポイントして、Z 軸。 このシステムで座標は画面の背景には負の Z 値があります。

次に例を示します。

* デバイスは、フラット テーブル上にあるし、その左側にある右方向にプッシュされます、x の高速化の値が正の値。

* 高速化の値は +1.00 G でフラット テーブルで、デバイスがある場合または (9.81 m/s + ^2)、対応デバイスの高速化する (0 m/秒 ^2) 重力の force マイナス (-9.81 m/秒 ^2) と G. のように正規化されました。

* デバイスが平らなテーブル上にあるし、高速化の m/秒の空の方に移動されますが ^2 の高速化の値は、デバイスの高速化に対応する A + 9.81 と等しく (+ m/s ^2) 重力の force マイナス (-9.81 m/秒 ^2) と G. で正規化されました。 

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [加速度計のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [加速度計 API ドキュメント](xref:Xamarin.Essentials.Accelerometer)
