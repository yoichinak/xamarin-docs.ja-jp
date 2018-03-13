---
title: "バッテリの状態を確認しています"
description: "各プラットフォームのネイティブ情報にアクセスするバッテリ DependencyService を使用します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: CF1C5A73-84ED-407D-BDC5-EB1D83D2D3DB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 78832c396f8968769f3134728cd435555aecbc8f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="checking-battery-status"></a>バッテリの状態を確認しています

この記事は、バッテリの状態を確認するアプリケーションの作成手順について説明します。 この記事の内容を James Montemagno によってバッテリ プラグインに基づきます。 詳細については、次を参照してください。、 [GitHub リポジトリの](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery)します。

このアプリケーションが使用する必要があります Xamarin.Forms にバッテリの現在の状態をチェックするための機能が含まれていないため[ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/)ネイティブ Api を活用するためにします。  この記事を使用して次の手順を説明します`DependencyService`:。

- **[インターフェイスを作成する](#Creating_the_Interface)** &ndash;共有コードで、インターフェイスを作成する方法を理解します。
- **[iOS 実装](#iOS_Implementation)** &ndash; iOS 用のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[Android 実装](#Android_Implementation)** &ndash; for Android のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[Windows Phone 実装](#Windows_Phone_Implementation)** &ndash; Windows Phone 用のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[ユニバーサル Windows プラットフォームの実装](#UWPImplementation)** &ndash;ユニバーサル Windows プラットフォーム (UWP) のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[共有コードで実装する](#Implementing_in_Shared_Code)** &ndash;を使用する方法を学習`DependencyService`に共有コードからネイティブの実装を呼び出します。

完了したら、アプリケーションを使用して、`DependencyService`次のような構造になります。

![](battery-info-images/battery-diagram.png "DependencyService アプリケーション構造")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>インターフェイスの作成

最初に、必要な機能を表現する共有コードでインターフェイスを作成します。 アプリケーションの確認、バッテリの場合は、関連の情報は、デバイスが充電してもそうでないと、デバイスでは次の方法で電力が供給されているかどうかにバッテリ残量のパーセンテージは。

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

共有コードでは、このインターフェイスに対するコーディングすると、各プラットフォームで電源管理 Api にアクセスする Xamarin.Forms アプリが許可されます。

> [!NOTE]
> インターフェイスを実装するクラスを使用するパラメーターなしのコンス トラクターを持つ必要があります、`DependencyService`です。 コンス トラクターは、インターフェイスによって定義することはできません。

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS の実装

`IBattery`プラットフォーム固有のアプリケーションの各プロジェクトでインターフェイスを実装する必要があります。 IOS の実装がネイティブを使用して[ `UIDevice` ](https://developer.xamarin.com/api/type/UIKit.UIDevice/)バッテリ情報にアクセスする Api。 次のクラスがパラメーターなしのコンス トラクターを持つことに注意してくださいできるように、`DependencyService`新しいインスタンスを作成できます。

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

最後に、この追加`[assembly]`など必要な属性のクラス上 (および定義されている任意の名前空間の外部)、`using`ステートメント。

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

この属性の実装としてクラスを登録する、`IBattery`インターフェイス、つまり`DependencyService.Get<IBattery>`そのインスタンスを作成する共有コードで使用できます。

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android の実装

Android の実装を使用して、 [ `Android.OS.BatteryManager` ](https://developer.xamarin.com/api/type/Android.OS.BatteryManager/) API です。 この実装は、バッテリのアクセス許可の不足を処理するためのチェックを必要とする、iOS のバージョンよりも複雑。

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

この追加`[assembly]`など必要な属性のクラス上 (および定義されている任意の名前空間の外部)、`using`ステートメント。

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

この属性の実装としてクラスを登録する、`IBattery`インターフェイス、つまり`DependencyService.Get<IBattery>`で使用できる共有コードは、そのインスタンスを作成できます。

<a name="Windows_Phone_Implementation" />

## <a name="windows-phone-implementation"></a>Windows Phone の実装

この実装では Windows Phone power API と同等の Android および iOS よりも小さい情報を提供するため、Android と iOS のバージョンよりも制限します。

```csharp
using System;
using Windows.ApplicationModel.Core;
using DependencyServiceSample.WinPhone;

namespace DependencyServiceSample.WinPhone
{
  public class BatteryImplementation : IBattery
  {
    private int last;
    private BatteryStatus status = BatteryStatus.Unknown;

    public BatteryImplementation()
    {
      last = DefaultBattery.RemainingChargePercent;
    }

    Windows.Phone.Devices.Power.Battery battery;
    private Windows.Phone.Devices.Power.Battery DefaultBattery
    {
      get { return battery ?? (battery = Windows.Phone.Devices.Power.Battery.GetDefault()); }
    }
    public int RemainingChargePercent
    {
      get
      { return DefaultBattery.RemainingChargePercent; }
    }

    public  BatteryStatus Status
    {
      get { return status; }
    }

    public PowerSource PowerSource
    {
      get
      {
        if (status == BatteryStatus.Full || status == BatteryStatus.Charging)
          return PowerSource.Ac;

        return PowerSource.Battery;
      }
    }
  }
}
```

この追加`[assembly]`など必要な属性のクラス上 (および定義されている任意の名前空間の外部)、`using`ステートメントです。

```csharp
using System;
using Windows.ApplicationModel.Core;
using DependencyServiceSample.WinPhone;

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.WinPhone {
  public class BatteryImplementation : IBattery {
    ...
```

この属性の実装としてクラスを登録する、`IBattery`インターフェイス、つまり`DependencyService.Get<IBattery>`そのインスタンスを作成する共有コードで使用できます。

<a name="UWPImplementation" />

## <a name="universal-windows-platform-implementation"></a>ユニバーサル Windows プラットフォームの実装

UWP 実装を使用して、`Windows.Devices.Power`バッテリの状態情報を取得するための Api:

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

`[assembly]`名前空間の宣言の上の属性の実装としてクラスを登録する、`IBattery`インターフェイス、つまり`DependencyService.Get<IBattery>`そのインスタンスを作成する共有コードで使用できます。

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>共有コードで実装します。

これで、プラットフォームごとに、インターフェイスが実装されているに活用するために、共有アプリケーションを記述できます。 アプリケーションには、ときに更新プログラムをタップするボタンがあるページのバッテリの現在の状態では、そのテキスト。 使用して、`DependencyService`のインスタンスを取得する、`IBattery`インターフェイスです。 実行時に、このインスタンスをネイティブの SDK へのフル アクセスのあるプラットフォームに固有の実装となります。

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

IOS、Android、または Windows プラットフォームにこのアプリケーションを実行して、ボタンを押すと、デバイスの現在の電源の状態を反映するように更新ボタンのテキストが発生します。

![](battery-info-images/battery.png "バッテリの状態のサンプル")


## <a name="related-links"></a>関連リンク

- [DependencyService (サンプル)](https://developer.xamarin.com/samples/DependencyService)
- [DependencyService (サンプル) を使用します。](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
