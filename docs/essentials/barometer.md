---
title: 'Xamarin.Essentials: バロメーター'
description: Xamarin.Essentials の Barometer クラスを使用すると、負荷を測定するデバイスのバロメーター センサーを監視できます。
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 08/16/2018
ms.openlocfilehash: 0217b89bc3f45347acecbdb3b8cdccfeed751744
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50130299"
---
# <a name="xamarinessentials-barometer"></a>Xamarin.Essentials: バロメーター

![プレリリースの NuGet](~/media/shared/pre-release.png)

**Barometer** クラスを使用すると、負荷を測定するデバイスのバロメーター センサーを監視できます。

## <a name="using-barometer"></a>バロメーターの使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

バロメーターの機能は、`Start` および `Stop` メソッドを呼び出し、バロメーターの負荷の読み取り値の変化をキロパスカル単位でリッスンすることで動作します。 すべての変更は `ReadingChanged` イベントを通じて戻されます。 以下がサンプルの使用方法です。

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
        Console.WriteLine($"Reading: Pressure: {data.Pressure} kilopascals");
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

# <a name="androidtabandroid"></a>[Android](#tab/android)

プラットフォーム固有の実装の詳細はありません。

# <a name="iostabios"></a>[iOS](#tab/ios)

この API では [CMAltimeter](https://developer.apple.com/documentation/coremotion/cmaltimeter#//apple_ref/occ/cl/CMAltimeter) を使用して負荷の変化を監視します。これは、iPhone 6 以降のデバイスに追加されたハードウェアの機能です。 高度計をサポートしていないデバイスの場合は、`FeatureNotSupportedException` がスローされます。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

プラットフォーム固有の実装の詳細はありません。

-----

## <a name="api"></a>API

- [Barometer のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Barometer)
- [Barometer API ドキュメント](xref:Xamarin.Essentials.Barometer)
