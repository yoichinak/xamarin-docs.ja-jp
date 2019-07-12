---
title: 'Xamarin.Essentials: コンパス'
description: このドキュメントで説明する Xamarin.Essentials の Compass クラスを使用すると、デバイスの磁北方位を監視することができます。
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 55dd10bff21b7d082b225277d0100232d5efd4f3
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898785"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials: コンパス

**Compass** クラスを使用すると、デバイスの磁北方位を監視することができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-compass"></a>Compass の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

Compass の機能は、ジコンパスの変化をリッスンする `Start` および `Stop` メソッドを呼び出すことで動作します。 すべての変更は `ReadingChanged` イベントを通じて戻されます。 次に例を示します。

```csharp
public class CompassTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(object sender, CompassChangedEventArgs e)
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
            // Some other exception has occurred
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android には、コンパスの方位を取得するための API はありません。 Google の推奨に従い、加速度計と磁気探知器を利用して磁北方位を計算します。

まれに、8 の字を描くようにデバイスを動かしたときなど、センサーの調整が必要なために、一貫性のない結果が表示されることがあります。 これを行う最善の方法は、Google マップを開き、場所のドットをタップして、 **[Calibrate compass]\(場所の調整\)** を選択します。

アプリから同時に複数のセンサーを実行すると、センサーの速度が調整される場合があることに注意してください。

## <a name="low-pass-filter"></a>ローパス フィルター

Android コンパスの値が更新および計算される方法のため、値の平滑化が必要な場合があります。 角度のサイン値とコサイン値の平均を計算する "_ローパス フィルター_" を適用でき、`bool applyLowPassFilter` パラメーターを受け入れる `Start` メソッドのオーバーロードを使用してオンにできます。

```csharp
Compass.Start(SensorSpeed.UI, applyLowPassFilter: true);
```

これは、Android のプラットフォームでのみ適用され、iOS および UWP ではパラメーターが無視されます。  詳しくは、[こちら](https://github.com/xamarin/Essentials/pull/354#issuecomment-405316860)をご覧ください。

--------------

## <a name="api"></a>API

- [Compass のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Compass API のドキュメント](xref:Xamarin.Essentials.Compass)
