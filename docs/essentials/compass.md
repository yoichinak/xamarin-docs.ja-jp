---
title: 'Xamarin.Essentials: Compass'
description: このドキュメントでは、Xamarin.Essentials で、デバイスの方位は磁北見出しを監視することができます、コンパス クラスについて説明します。
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cf41948c55c742140896bfb48d9bb4abf25c8d68
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947414"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials: Compass

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**Compass**クラスを使用して、デバイスの方位は磁北見出しを監視できます。

## <a name="using-compass"></a>Compass を使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

Compass 機能が呼び出すことによって、`Start`と`Stop`コンパスへの変更をリッスンするメソッド。 加えた変更は通過して送信、`ReadingChanged`イベント。 次に例を示します。

```csharp
public class CompassTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(CompassChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: {data.HeadingMagneticNorth} degrees");
        // Process Heading Magnetic North
    }

    public void ToggleCompass()
    {
        try
        {
            if (Compass.IsMonitoring)
              Compass.Stop();
            else
              Compass.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Some other exception has occured
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android では、コンパスを取得するため、API は提供されません。 Google によって推奨されている方位は磁北見出しを計算する磁力計、加速度計を利用します。 

まれに、おそらく結果が表示一貫性のない、センサーを調整する必要があるため図 8 を描くように、デバイスを移動する処理も行われます。 Google マップを開きのドットを現在の場所をタップして選択にこれは最善の方法で行う**調整 compass**します。

センサー速度の調整を同時に、アプリから複数のセンサーを実行している可能性がありますに注意します。

--------------

## <a name="api"></a>API

- [コンパスのソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [コンパスの API ドキュメント](xref:Xamarin.Essentials.Compass)
