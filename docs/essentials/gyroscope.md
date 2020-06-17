---
タイトル: "Xamarin.Essentials: Gyroscope" の説明: "Xamarin.Essentials の Gyroscope クラスを使用すると、デバイスの 3 つの主軸の周りの回転角度を測定するデバイスのジャイロスコープ センサーを監視できます。"
ms.assetid:DA4F968A-D988-41F5-8745-1BEE693660A1 author: jamesmontemagno ms.author: jamont ms.date:11/04/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials:ジャイロスコープ

**Gyroscope** クラスを使用すると、デバイスの 3 つの主軸の周りの回転角度であるデバイスのジャイロスコープ センサーを監視できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-gyroscope"></a>Gyroscope の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

Gyroscope の機能は、ジャイロスコープの変化をリッスンする `Start` および `Stop` メソッドを呼び出すことで動作します。 すべての変更は `ReadingChanged` イベントを通じて rad/秒単位で戻されます。 以下がサンプルの使用方法です。

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(object sender, GyroscopeChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Angular Velocity X, Y, and Z reported in rad/s
        Console.WriteLine($"Reading: X: {data.AngularVelocity.X}, Y: {data.AngularVelocity.Y}, Z: {data.AngularVelocity.Z}");
    }

    public void ToggleGyroscope()
    {
        try
        {
            if (Gyroscope.IsMonitoring)
              Gyroscope.Stop();
            else
              Gyroscope.Start(speed);
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

## <a name="api"></a>API

- [Gyroscope のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Gyroscope API のドキュメント](xref:Xamarin.Essentials.Gyroscope)
