---
title: バッテリの状態の確認
description: この記事では、Xamarin.Forms の DependencyService クラスを使用して、各プラットフォームのバッテリ情報にネイティブにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: CF1C5A73-84ED-407D-BDC5-EB1D83D2D3DB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 2c9fae211e6a88944c94ca265409865b3252a10a
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/06/2019
ms.locfileid: "66740928"
---
# <a name="checking-battery-status"></a>バッテリの状態の確認

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/)

この記事では、バッテリの状態を確認するアプリケーションの作成手順について説明します。 この記事は、James Montemagno によるバッテリ プラグインに基づいています。 詳しくは、[GitHub リポジトリ](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery)をご覧ください。

Xamarin.Forms には現在のバッテリの状態を確認するための機能が含まれていないため、このアプリケーションでは [`DependencyService` ](xref:Xamarin.Forms.DependencyService) を使用して、ネイティブ API を活用する必要があります。  この記事では、`DependencyService` を使用するための以下の手順について説明します。

- **[インターフェイスの作成](#Creating_the_Interface)** &ndash; 共有コード内にインターフェイスを作成する方法を理解します。
- **[iOS での実装](#iOS_Implementation)** &ndash; iOS 用のネイティブ コード内にインターフェイスを実装する方法を学習します。
- **[Android での実装](#Android_Implementation)** &ndash; Android 用のネイティブ コード内にインターフェイスを実装する方法を学習します。
- **[ユニバーサル Windows プラットフォームでの実装](#UWPImplementation)** &ndash; ユニバーサル Windows プラットフォーム (UWP) 用のネイティブ コード内にインターフェイスを実装する方法を学習します。
- **[共有コード内への実装](#Implementing_in_Shared_Code)** &ndash; `DependencyService` を使用して共有コードからネイティブ実装を呼び出す方法を学習します。

完了すると、`DependencyService` を使用するアプリケーションは次のような構造になります。

![](battery-info-images/battery-diagram.png "DependencyService アプリケーションの構造")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>インターフェイスの作成

最初に、目的の機能を表すインターフェイスを共有コード内に作成します。 バッテリ チェック アプリケーションの場合、関連する情報は、バッテリの残量の割合、デバイスが充電中かどうか、およびデバイスへの電力の供給方法です。

```csharp
namespace DependencyServiceSample
{
  public enum BatteryStatus
  {
    Charging,
    Discharging,
    Full,
    NotCharging,
    Unknown
  }

  public enum PowerSource
  {
    Battery,
    Ac,
    Usb,
    Wireless,
    Other
  }

  public interface IBattery
  {
    int RemainingChargePercent { get; }
    BatteryStatus Status { get; }
    PowerSource PowerSource { get; }
  }
}
```

共有コード内でこのインターフェイスに対してコーディングすると、Xamarin.Forms アプリから各プラットフォーム上の電源管理 API にアクセスできます。

> [!NOTE]
> このインターフェイスを実装するクラスで `DependencyService` を使用するには、パラメーターのないコンストラクターが必要です。 インターフェイスによってコンストラクターを定義することはできません。

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS での実装

各プラットフォームに固有のアプリケーション プロジェクト内に、`IBattery` インターフェイスを実装する必要があります。 iOS での実装では、ネイティブの [`UIDevice`](xref:UIKit.UIDevice) API を使用して、バッテリ情報にアクセスします。 `DependencyService` で新しいインスタンスを作成できるように、次のクラスにはパラメーターなしのコンストラクターがあることに注意してください。

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;

namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery
  {
    public BatteryImplementation()
    {
      UIDevice.CurrentDevice.BatteryMonitoringEnabled = true;
    }

    public int RemainingChargePercent
    {
      get
      {
        return (int)(UIDevice.CurrentDevice.BatteryLevel * 100F);
      }
    }

    public BatteryStatus Status
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return BatteryStatus.Charging;
          case UIDeviceBatteryState.Full:
            return BatteryStatus.Full;
          case UIDeviceBatteryState.Unplugged:
            return BatteryStatus.Discharging;
          default:
            return BatteryStatus.Unknown;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Full:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Unplugged:
            return PowerSource.Battery;
          default:
            return PowerSource.Other;
        }
      }
    }
  }
}
```

最後に、必要なすべての `using` ステートメントと共に、次の `[assembly]` 属性をクラスの上 (そして、定義されているすべての名前空間の外側) に追加します。

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;//necessary for registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery {
    ...
```

この属性により、クラスが `IBattery` インターフェイスの実装として登録されます。つまり、共有コード内で `DependencyService.Get<IBattery>` を使用してそのインスタンスを作成できます。

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android での実装

Android での実装では、[`Android.OS.BatteryManager`](https://developer.xamarin.com/api/type/Android.OS.BatteryManager/) API が使用されます。 この実装では、バッテリへのアクセス許可の欠如を処理するためのチェックが必要であるため、iOS 版よりも複雑になります。

```csharp
using System;
using Android;
using Android.Content;
using Android.App;
using Android.OS;
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid;

namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery
  {
    private BatteryBroadcastReceiver batteryReceiver;
    public BatteryImplementation() { }

    public int RemainingChargePercent
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              var level = battery.GetIntExtra(BatteryManager.ExtraLevel, -1);
              var scale = battery.GetIntExtra(BatteryManager.ExtraScale, -1);

              return (int)Math.Floor(level * 100D / scale);
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }

      }
    }

    public DependencyServiceSample.BatteryStatus Status
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;
              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);
              if (isCharging)
                return DependencyServiceSample.BatteryStatus.Charging;

              switch(status)
              {
                case (int)BatteryStatus.Charging:
                  return DependencyServiceSample.BatteryStatus.Charging;
                case (int)BatteryStatus.Discharging:
                  return DependencyServiceSample.BatteryStatus.Discharging;
                case (int)BatteryStatus.Full:
                  return DependencyServiceSample.BatteryStatus.Full;
                case (int)BatteryStatus.NotCharging:
                  return DependencyServiceSample.BatteryStatus.NotCharging;
                default:
                  return DependencyServiceSample.BatteryStatus.Unknown;
              }
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;

              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);

              if (!isCharging)
                return DependencyServiceSample.PowerSource.Battery;
              else if (usbCharge)
                return DependencyServiceSample.PowerSource.Usb;
              else if (acCharge)
                return DependencyServiceSample.PowerSource.Ac;
              else if (wirelessCharge)
                return DependencyServiceSample.PowerSource.Wireless;
              else
                return DependencyServiceSample.PowerSource.Other;
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }
  }
}
```

必要なすべての `using` ステートメントと共に、次の `[assembly]` 属性をクラスの上 (そして、定義されているすべての名前空間の外側) に追加します。

```csharp
...
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery {
    ...
```

この属性により、クラスが `IBattery` インターフェイスの実装として登録されます。つまり、共有コード内で `DependencyService.Get<IBattery>` を使用してそのインスタンスを作成できます。

<a name="UWPImplementation" />

## <a name="universal-windows-platform-implementation"></a>ユニバーサル Windows プラットフォームでの実装

UWP の実装では、`Windows.Devices.Power` API を使用して、バッテリの状態情報を取得します。

```csharp
using DependencyServiceSample.UWP;
using Xamarin.Forms;

[assembly: Dependency(typeof(BatteryImplementation))]
namespace DependencyServiceSample.UWP
{
    public class BatteryImplementation : IBattery
    {
        private BatteryStatus status = BatteryStatus.Unknown;
        Windows.Devices.Power.Battery battery;

        public BatteryImplementation()
        {
        }

        private Windows.Devices.Power.Battery DefaultBattery
        {
            get
            {
                return battery ?? (battery = Windows.Devices.Power.Battery.AggregateBattery);
            }
        }

        public int RemainingChargePercent
        {
            get
            {
                var finalReport = DefaultBattery.GetReport();
                var finalPercent = -1;

                if (finalReport.RemainingCapacityInMilliwattHours.HasValue && finalReport.FullChargeCapacityInMilliwattHours.HasValue)
                {
                    finalPercent = (int)((finalReport.RemainingCapacityInMilliwattHours.Value /
                        (double)finalReport.FullChargeCapacityInMilliwattHours.Value) * 100);
                }
                return finalPercent;
            }
        }

        public BatteryStatus Status
        {
            get
            {
                var report = DefaultBattery.GetReport();
                var percentage = RemainingChargePercent;

                if (percentage >= 1.0)
                {
                    status = BatteryStatus.Full;
                }
                else if (percentage < 0)
                {
                    status = BatteryStatus.Unknown;
                }
                else
                {
                    switch (report.Status)
                    {
                        case Windows.System.Power.BatteryStatus.Charging:
                            status = BatteryStatus.Charging;
                            break;
                        case Windows.System.Power.BatteryStatus.Discharging:
                            status = BatteryStatus.Discharging;
                            break;
                        case Windows.System.Power.BatteryStatus.Idle:
                            status = BatteryStatus.NotCharging;
                            break;
                        case Windows.System.Power.BatteryStatus.NotPresent:
                            status = BatteryStatus.Unknown;
                            break;
                    }
                }
                return status;
            }
        }

        public PowerSource PowerSource
        {
            get
            {
                if (status == BatteryStatus.Full || status == BatteryStatus.Charging)
                {
                    return PowerSource.Ac;
                }
                return PowerSource.Battery;
            }
        }
    }
}
```

名前空間の宣言の上にある `[assembly]` 属性により、クラスが `IBattery` インターフェイスの実装として登録されます。つまり、共有コード内で `DependencyService.Get<IBattery>`を使用してそのインスタンスを作成できます。

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>共有コードでの実装

各プラットフォーム用のインターフェイスが実装されたので、それを利用する共有アプリケーションを記述できます。 このアプリケーションは、タップされたときにテキストを現在のバッテリの状態に更新するボタンがあるページで構成されます。 それは、`DependencyService` を使用して、`IBattery` インターフェイスのインスタンスを取得します。 実行時に、このインスタンスがネイティブ SDK にフル アクセスできるプラットフォーム固有の実装になります。

```csharp
public MainPage ()
{
    var button = new Button {
        Text = "Click for battery info",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    button.Clicked += (sender, e) => {
        var bat = DependencyService.Get<IBattery>();

        switch (bat.PowerSource){
          case PowerSource.Battery:
            button.Text = "Battery - ";
            break;
          case PowerSource.Ac:
            button.Text = "AC - ";
            break;
          case PowerSource.Usb:
            button.Text = "USB - ";
            break;
          case PowerSource.Wireless:
            button.Text = "Wireless - ";
            break;
          case PowerSource.Other:
          default:
            button.Text = "Other - ";
            break;
        }
        switch (bat.Status){
          case BatteryStatus.Charging:
            button.Text += "Charging";
            break;
          case BatteryStatus.Discharging:
            button.Text += "Discharging";
            break;
          case BatteryStatus.NotCharging:
            button.Text += "Not Charging";
            break;
          case BatteryStatus.Full:
            button.Text += "Full";
            break;
          case BatteryStatus.Unknown:
          default:
            button.Text += "Unknown";
            break;
        }
    };
    Content = button;
}
```

iOS、Android、または UWP プラットフォーム上でこのアプリケーションを実行してボタンを押すと、ボタンのテキストがデバイスの現在の電源の状態に更新されます。

![](battery-info-images/battery.png "バッテリの状態の例")


## <a name="related-links"></a>関連リンク

- [DependencyService (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/)
- [DependencyService の使用 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
