---
title: Xamarin.Essentials コンパス
description: コンパス クラスを使用して、デバイスの磁気北見出しを監視できます。
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 9e0d0a709006a0d185a0c79b7b03d1672f06c4b6
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials コンパス

![プレリリース NuGet](~/media/shared/pre-release.png)

**コンパス**クラスを使用して、デバイスの磁気北見出しを監視できます。

## <a name="using-compass"></a>コンパスを使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

コンパス機能が呼び出すことによって、`Start`と`Stop`コンパスへの変更をリッスンするメソッド。 加えた変更はを通じて送信、`ReadingChanged`イベント。 次に例を示します。

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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[センサー速度](xref:Xamarin.Essentials.SensorSpeed)

- **最も高速な**– (UI スレッドで返されるとは限りません) できる限り高速のセンサー データを取得します。
- **ゲーム**– ゲーム (UI スレッドで返されるとは限りません) に適したを評価します。
- **標準**– 既定のレートが画面の向きの変更に適してします。
- **Ui** – 一般的なユーザー インターフェイスの適切なを評価します。

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android では、コンパスの見出しを取得するため、API は提供されません。 加速度計、および Google によって推奨される磁気北部の見出しを計算する磁力計を利用します。 

まれに、状況によって異なる結果が表示一貫性のないセンサーを調整する必要があるため図 8 を描くように、デバイスを移動する必要があります。 Google マップを開き、現在の場所、ドットでタップして選択にこれは最善の方法で行う**調整コンパス**です。

アプリから同時に複数のセンサーを実行するとセンサーの速度を調整する可能性がありますに注意してください。

--------------

## <a name="api"></a>API

- [コンパスのソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/Compass)
- [コンパスの API ドキュメント](xref:Xamarin.Essentials.Compass)
