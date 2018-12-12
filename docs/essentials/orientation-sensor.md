---
title: 'Xamarin.Essentials: OrientationSensor'
description: OrientationSensor クラスでは、3 次元空間内のデバイスの向きを監視できます。
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: f1fceaef93e7ac30bbbe0f13da7dde3cde5275fd
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898671"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

**OrientationSensor** クラスでは、3 次元空間内のデバイスの向きを監視できます。

> [!NOTE]
> これは、3D 空間内のデバイスの向きを決定するためのクラスです。 デバイスのビデオ ディスプレイが縦向きモードか横向きモードかを判断する必要がある場合は、[`DeviceDisplay`](device-display.md) クラスから利用できる `ScreenMetrics` オブジェクトの `Orientation` プロパティを使用します。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-orientationsensor"></a>OrientationSensor の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

`Start` メソッドを呼び出して `OrientationSensor` を有効にし、デバイスの向きの変更を監視します。無効にする場合は `Stop` メソッドを呼び出します。 すべての変更は `ReadingChanged` イベントを通じて戻されます。 使用例を次に示します。

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

`OrientationSensor` の読み取り値が [`Quaternion`](xref:System.Numerics.Quaternion) の形式で返されます。これは、2 つの 3D 座標系に基づくデバイスの向きを表現しています。

デバイス (通常は電話またはタブレット) には、次の軸を持つ 3D 座標系があります。

- 正の X 軸は、縦向きモードの画面の右方向を指します。
- 正の Y 軸は、縦向きモードの画面の上方向を指します。
- 正の Z 軸は、画面から外れます。

地球の 3D 座標系には、次の軸があります。

- 正の X 軸は地球の表面に対する接線であり、東を指します。
- 正の Y 軸もまた地球の表面に対する接線であり、北を指します。
- 正の Z 軸は地球の表面に対する垂直線であり、上方を指します。

`Quaternion` では、地球の座標系に対するデバイスの座標系の回転が表現されます。

`Quaternion` の値は、軸の周りの回転と密接に関係しています。 回転軸が正規化されたベクター (a<sub>x</sub>, a<sub>y</sub>, a<sub>z</sub>) であり、回転角度が Θ であった場合、四元数の (X, Y, Z, W) 成分は次のようになります。

(a<sub>x</sub>·sin(Θ/2), a<sub>y</sub>·sin(Θ/2), a<sub>z</sub>·sin(Θ/2), cos(Θ/2))

これらは右手座標系です。したがって、右手の親指で回転軸の正の方向を指すと、その他の指の曲がる方向が正の角度の回転方向を示します。

次に例を示します。

* デバイスが画面を上に向けてテーブルの上に平らに置かれ、デバイスの上部 (縦向きモード) が北を指している場合、2 つの座標系は揃っています。 `Quaternion` の値は単位四元数 (0, 0, 0, 1) を示します。 すべての回転は、この位置を基準として分析できます。

* デバイスが画面を上に向けてテーブルの上に平らに置かれ、デバイスの上部 (縦向きモード) が西を指している場合、`Quaternion` の値は (0, 0, 0.707, 0.707) になります。 デバイスは、地球の Z 軸の周りを 90 度回転しています。

* デバイスの上部 (縦向きモード) が空を指すようにデバイスが垂直に固定され、デバイスの背面が北を向いている場合、デバイスは X 軸の周りを 90 度回転しています。 `Quaternion` の値は (0.707, 0, 0, 0.707) です。

* デバイスの左端がテーブルに付くようにデバイスが置かれ、上部が北を指している場合、デバイスは Y 軸の周りを &ndash;90 度 (または、負の Y 軸の周りを 90 度) 回転しています。 `Quaternion` の値は (0, -0.707, 0, 0.707) です。

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [OrientationSensor のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [OrientationSensor API ドキュメント](xref:Xamarin.Essentials.OrientationSensor)
