---
title: 'Xamarin.Essentials: OrientationSensor'
description: OrientationSensor クラスでは、3 次元空間内のデバイスの向きを監視できます。
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c7bbc849e5fa0b901f5b54e77d548b28bc2a72c6
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080503"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

![プレリリース NuGet](~/media/shared/pre-release.png)

**OrientationSensor**クラスでは、次の 3 つの次元空間内のデバイスの向きを監視することができます。

> [!NOTE]
> このクラスでは、3 D 空間内のデバイスの向きを決定するためです。 表示が縦または横モードを使用して、デバイス、ビデオのかどうかを決定する必要がある場合、`Orientation`のプロパティ、`ScreenMetrics`から使用可能なオブジェクト、 [ `DeviceDisplay` ](device-display.md)クラスです。

## <a name="using-orientationsensor"></a>OrientationSensor を使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

`OrientationSensor`が呼び出すことによって有効になって、`Start`メソッドを呼び出すことで、デバイスの向き、および無効になっているへの変更を監視、`Stop`メソッドです。 加えた変更はを通じて送信、`ReadingChanged`イベント。 サンプルの使用方法を次に示します。

```csharp

public class OrientationSensorTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public OrientationSensorTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        OrientationSensor.ReadingChanged += OrientationSensor_ReadingChanged;
    }

    void OrientationSensor_ReadingChanged(AccelerometerChangedEventArgs e)
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

`OrientationSensor` 測定値が報告の形式で、 [ `Quaternion` ](xref:System.Numerics.Quaternion) 3D の 2 つの座標システムに基づいて、デバイスの向きを説明します。

デバイス (携帯電話に通常やタブレット) には、次の軸を持つ 3 D の座標系があります。

- 正の X 軸から縦モードで表示の右にポイントします。
- 正の Y 軸は縦向きモードのデバイスの先頭を指します。
- 正の Z 軸は、画面外ポイントします。

地球の 3D の座標系には、次の軸があります。

- 正の X 軸は、地球の表面に正接し、東ポイントします。
- 正の Y 軸は、ポイント北、地球の表面に正接もあります。
- 正の Z 軸は上向き、地球の表面に垂直なです。

`Quaternion`地球の座標系に関連するデバイスの座標システムの回転について説明します。

A`Quaternion`値は、軸の周りの回転を非常に密接に関連します。 回転の軸は、正規化されたベクター場合 (、<sub>x</sub>、<sub>y</sub>、<sub>z</sub>)、回転の角度は、Θ し、(X、Y、Z、W) と四元数のコンポーネントは。

(、<sub>x</sub>·sin(Θ/2)、<sub>y</sub>·sin(Θ/2)、<sub>z</sub>·sin(Θ/2)、cos(Θ/2))

これらは、右側の座標システムでは、本の指の曲線が回転の角度に正の方向を示す回転軸の正の値の方向で指し示される右側のスクロール ボックスのようにします。

次に例を示します。

* デバイスに向けて、北、(縦向きモード) では、デバイスの最上部で、画面フラット テーブルにあるときに 2 つの座標系が配置されます。 `Quaternion`値が単位四元数 (0、0, 0, 1) を表します。 この位置に対しては、すべての回転を分析できます。

* デバイスがテーブルにフラット表向きの画面と西を指し示す (縦向きモード) では、デバイスの最上部にあるときに、`Quaternion`値は (0, 0、0.707, 0.707)。 デバイスが地球の Z 軸を中心 90 度回転されました。

* デバイスは、空をポイントして最上位 (縦向きモード) であると、デバイスの背面に直面して北できるように、垂直保持されている、ときに、デバイスは X 軸の周りに 90 度回転にされました。 `Quaternion`値は (0.707、0, 0, 0.707)。

* ツールバーの左端が、テーブル上にあり、上部ポイント北、デバイスの回転されているため、デバイスが配置されている場合&ndash;90 ° の Y 軸 (または負の値、Y 軸の周りに 90 度) です。 `Quaternion`値は (0,-0.707、0, 0.707)。

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[センサー速度](xref:Xamarin.Essentials.SensorSpeed)

- **最も高速な**– (UI スレッドで返されるとは限りません) できる限り高速のセンサー データを取得します。
- **ゲーム**– ゲーム (UI スレッドで返されるとは限りません) に適したを評価します。
- **標準**– 既定のレートが画面の向きの変更に適してします。
- **Ui** – 一般的なユーザー インターフェイスの適切なを評価します。

かどうか、イベント ハンドラーは、UI スレッドでを実行し、イベント ハンドラーは、ユーザー インターフェイス要素にアクセスする必要がある場合は保証されません、 [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md)メソッドを UI スレッドでそのコードを実行します。

## <a name="api"></a>API

- [OrientationSensor ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [OrientationSensor API ドキュメント](xref:Xamarin.Essentials.OrientationSensor)
