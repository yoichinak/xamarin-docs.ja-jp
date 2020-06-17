---
タイトル: "Xamarin.Essentials:シェイクの検出'' description:''Xamarin.Essentials の Accelerometer クラスを利用すると、デバイスが揺れる動きを検出できます。''
ms.assetid:07513D32-120F-4F12-8757-A47802A8027B author: jamesmontemagno ms.author: jamont ms.date:05/28/2019 ms.custom: video no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-detect-shake"></a>Xamarin.Essentials:シェイクの検出

**[Accelerometer](accelerometer.md)** クラスでは、デバイスの加速度を 3 次元空間で示す、デバイスの加速度計センサーを監視できます。 また、ユーザーがデバイスを振るイベントを登録できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-detect-shake"></a>Detect Shake の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

デバイスの揺れを検出するには、Accelerometer 機能を使用する必要があります。具体的には、`Start` メソッドと `Stop` メソッドを呼び出すことで、加速の変化を待ち受け、揺れを検出します。 揺れが検出されるたびに `ShakeDetected` イベントが始動します。 `SensorSpeed` には `Game` またはそれより高速を使うことをお勧めします。 以下がサンプルの使用方法です。

```csharp

public class DetectShakeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Game;

    public DetectShakeTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ShakeDetected  += Accelerometer_ShakeDetected ;
    }

    void Accelerometer_ShakeDetected (object sender, EventArgs e)
    {
        // Process shake event
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

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="implementation-details"></a>実装の詳細

Detect Shake API では、加速度計で読み取られた値が未加工のまま使用され、加速が計算されます。 単純なキュー メカニズムを利用し、最近の加速度計イベントの 3/4 が最後の 0.5 秒以内に発生したかどうかを検出します。 加速は、加速度計から読み取った X、Y、Z 値の自乗を加算し、それを特定のしきい値と比較することで計算されます。

## <a name="api"></a>API

- [加速度計のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [加速度計の API ドキュメント](xref:Xamarin.Essentials.Accelerometer)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Detect-Shake-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
