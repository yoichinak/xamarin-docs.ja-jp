---
title: 'Xamarin.Essentials: 加速度計'
description: Xamarin.Essentials で加速度計クラスでは、次の 3 つの次元空間内のデバイスのアクセラレータを示すデバイスの加速度計センサーを監視できます。
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 99529f08348254dff7577b7e82da739fabd63a14
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781866"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials: 加速度計

![プレリリース NuGet](~/media/shared/pre-release.png)

**加速度計**クラスでは、次の 3 つの次元空間内のデバイスのアクセラレータを示すデバイスの加速度計センサーを監視できます。

## <a name="using-accelerometer"></a>加速度計の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

加速度計機能が呼び出すことによって、`Start`と`Stop`アクセラレータへの変更をリッスンするメソッド。 加えた変更はを通じて送信、`ReadingChanged`イベント。 サンプルの使用方法を次に示します。

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

加速度計の測定値は、報告 G. でG は gravitation 単位 force 地球の重力フィールドに作用するのと等しい (= 9.81 m/秒 ^2)。

座標システムは、その既定の向きに電話の画面に対して相対的に定義されます。 デバイスの画面の向きが変更されたときに、軸は交換されません。

X 軸は水平方向と右へのポインター、Y 軸は縦方向上向き、画面の前面の外側をポイントして、Z 軸です。 このシステムでは、画面の座標は、負の値の Z 値を持ちます。

次に例を示します。

* デバイスは、テーブルの上にあるしがその左側にある右方向にプッシュ、アクセラレータの x 値が正の値。

* アクセラレータ値は +1.00 G でフラット テーブルで、デバイスがある場合または (+ = 9.81 m/秒 ^2)、デバイスの高速化に対応する (0 m/秒 ^2) 重力の force マイナス (-= 9.81 m/秒 ^2) と G. と同様に正規化されました。

* デバイスがフラット テーブル上にある、m/秒の高速化を空にプッシュは ^2、アクセラレータ値は、デバイスの高速化に対応する A + 9.81 に (+ m/秒 ^2) 重力の force マイナス (-= 9.81 m/秒 ^2) と G. で正規化されました。 

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[センサー速度](xref:Xamarin.Essentials.SensorSpeed)

- **最も高速な**– (UI スレッドで返されるとは限りません) できる限り高速のセンサー データを取得します。
- **ゲーム**– ゲーム (UI スレッドで返されるとは限りません) に適したを評価します。
- **標準**– 既定のレートが画面の向きの変更に適してします。
- **Ui** – 一般的なユーザー インターフェイスの適切なを評価します。

## <a name="api"></a>API

- [加速度計のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [加速度計 API ドキュメント](xref:Xamarin.Essentials.Accelerometer)
