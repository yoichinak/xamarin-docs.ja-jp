---
title: Xamarin.Essentials:加速度計
description: Xamarin.Essentials の加速度計クラスでは、デバイスの加速を 3 つの次元空間で示す、デバイスの加速度計センサーを監視できます。
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
ms.openlocfilehash: f22563aeaeb7bec2f87ca7892005f2cfb1c4e84b
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870379"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials:加速度計

**Accelerometer** クラスでは、デバイスの加速度を 3 次元空間で示す、デバイスの加速度計センサーを監視できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-accelerometer"></a>加速度計の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

加速度計の機能は、加速度計の変更をリッスンする `Start` と `Stop` のメソッドを呼び出すことで動作します。 すべての変更は `ReadingChanged` イベントを通じて戻されます。 以下がサンプルの使用方法です。

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(object sender, AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Acceleration.X}, Y: {data.Acceleration.Y}, Z: {data.Acceleration.Z}");
        // Process Acceleration X, Y, and Z
    }

    public void ToggleAccelerometer()
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

加速度計の測定値は G の単位で報告されます。A G は地球の重力場にかかる重力 (9.81 m/s^2) と同等の重力の単位です。

座標系は携帯電話の既定の画面の向きを基準にして定義されます。 デバイスの画面の向きが変わっても軸は切り替えられません。

X 軸は水平方向で右に、Y 軸は垂直方向で上に、Z 軸は画面前面から外側に向かいます。 このシステムでは画面より後ろにある座標の Z 値は負の値になります。

次に例を示します。 

- テーブルの上に水平に置いたデバイスを、左側から右側に向かって押した場合、X の加速値は正の値になります。

- デバイスがテーブルの上に水平に置かれている場合、加速値は +1.00 G または (+9.81 m/s^2) になります。これは、デバイスの加速 (0 m/s^2) から重力 (-9.81 m/s^2) を引き、G の単位で正規化したものに相当します。

- テーブルの上に水平に置いたデバイスを加速度 A m/s^2 で上に持ち上げた場合、加速度の値は A+9.81 と等しくなります。これは、デバイスの加速度 (+A m/s^2) から重力 (-9.81 m/s^2) を引き、G の単位で正規化したものに相当します。

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Accelerometer のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Accelerometer API のドキュメント](xref:Xamarin.Essentials.Accelerometer)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Accelerometer-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]

