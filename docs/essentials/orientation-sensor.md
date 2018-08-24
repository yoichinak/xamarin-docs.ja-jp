---
title: 'Xamarin.Essentials: OrientationSensor'
description: OrientationSensor クラスでは、3 次元空間内のデバイスの向きを監視できます。
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: a15338795424885882ed9c86288342d196f6fda2
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353816"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**OrientationSensor**クラスでは、次の 3 つの次元空間内のデバイスの向きを監視することができます。

> [!NOTE]
> クラスは、3 D 空間内のデバイスの向きを決定するためです。 表示が縦または横モードでは、デバイスにビデオのかどうかを判断する必要がある場合を使用して、`Orientation`のプロパティ、`ScreenMetrics`から使用可能なオブジェクト、 [ `DeviceDisplay` ](device-display.md)クラス。

## <a name="using-orientationsensor"></a>OrientationSensor を使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

`OrientationSensor`が呼び出すことによって有効になって、`Start`メソッドを呼び出すことによって、デバイスの向き、および無効になっているへの変更を監視、`Stop`メソッド。 加えた変更は通過して送信、`ReadingChanged`イベント。 サンプルの使用法を次に示します。

```csharp

public class OrientationSensorTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public OrientationSensorTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        OrientationSensor.ReadingChanged += OrientationSensor_ReadingChanged;
    }

    void OrientationSensor_ReadingChanged(object sender, OrientationSensorChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Orientation.X}, Y: {data.Orientation.Y}, Z: {data.Orientation.Z}, W: {data.Orientation.W}");
        // Process Orientation quaternion (X, Y, Z, and W)
    }

    public void ToggleOrientationSensor()
    {
        try
        {
            if (OrientationSensor.IsMonitoring)
                OrientationSensor.Stop();
            else
                OrientationSensor.Start(speed);
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

`OrientationSensor` 測定値の形式で示されます、 [ `Quaternion` ](xref:System.Numerics.Quaternion) 2 つの 3D 座標システムに基づいて、デバイスの向きをについて説明します。

(一般に、電話またはタブレット) デバイスでは、次の軸を持つ 3D 座標システムがあります。

- X 縦モードで画面の右側にポイントを軸の正の値。
- 正の Y 軸は縦モードでデバイスの上部をポイントします。
- 正の Z 軸は、画面の手前ポイントします。

地球の 3D 座標システムでは、次の軸があります。

- 正の X 軸が地球の表面に正接し、東ポイントします。
- 正の Y 軸は、ポイント北、地球の表面に接線もあります。
- 正の Z 軸は、地球と上向きのサーフェイスに対して垂直なです。

`Quaternion`地球の座標系を基準とした、デバイスの座標システムの回転について説明します。

A`Quaternion`回転の軸を中心に非常に密接に関連値。 回転の軸が正規化されたベクターである場合 (、<sub>x</sub>、<sub>y</sub>、<sub>z</sub>)、および回転角度 Θ、その後、(X、Y、Z, W) は、四元数のコンポーネントは。

(、<sub>x</sub>·sin(Θ/2)、<sub>y</sub>·sin(Θ/2)、<sub>z</sub>·sin(Θ/2)、cos(Θ/2))

これらは、右側の座標系の回転の軸の正方向を向く、右側にある、つまみで本の指の曲線が正の角度の回転の方向を示すようにします。

次に例を示します。

* 上向きにして、北を指し示す (縦向きモード) では、デバイスの上部にその画面にあるテーブルの上のデバイスの場合は、2 つの座標系を配置します。 `Quaternion`値は、恒等四元数 (0、0, 0, 1) を表します。 この位置から見て、すべての回転を分析できます。

* デバイスが上向きにしてその画面、西を指し示す (縦向きモード) では、デバイスの上部とフラット テーブルであるときに、`Quaternion`値は (0, 0、0.707, 0.707)。 デバイスが地球の Z 軸を中心に 90 度回転されています。

* (縦向きモード) の先頭が、空をポイントして、デバイスの背面に直面して北ように、このデバイスは垂直保持されている、デバイスが X 軸を中心に 90 度回転にされました。 `Quaternion`値は (0.707、0, 0, 0.707)。

* テーブルで、左端があり、先頭が北、ポイント、デバイスの回転されているため、デバイスが配置されている場合&ndash;90 °、Y 軸を中心 (または負の値の Y 軸を中心に 90 度)。 `Quaternion`値は (0,-0.707、0, 0.707)。

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [OrientationSensor ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [OrientationSensor API ドキュメント](xref:Xamarin.Essentials.OrientationSensor)
