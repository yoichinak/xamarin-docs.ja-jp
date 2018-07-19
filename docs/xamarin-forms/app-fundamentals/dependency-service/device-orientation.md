---
title: デバイスの向きの確認
description: この記事では、Xamarin.Forms DependencyService クラスを使用して共有コードからデバイスの向きにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 620404a217b2e8a31192ae6613dcec023ac366cd
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995642"
---
# <a name="checking-device-orientation"></a>デバイスの向きの確認

この記事では使用することに送り[ `DependencyService` ](xref:Xamarin.Forms.DependencyService)を各プラットフォームでネイティブ Api を使用して共有コードから、デバイスの方向を確認します。 このチュートリアルは、既存に基づいて`DeviceOrientation`Ali Özgür をプラグインします。 参照してください、 [GitHub リポジトリ](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation)詳細についてはします。

- **[インターフェイスを作成する](#Creating_the_Interface)** &ndash;を理解する方法、インターフェイスに共有コードに作成されます。
- **[iOS 実装](#iOS_Implementation)** &ndash; iOS のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[Android の実装](#Android_Implementation)** &ndash; Android のネイティブ コードでインターフェイスを実装する方法について説明します。
- **[UWP 実装](#WindowsImplementation)** &ndash;ユニバーサル Windows プラットフォーム (UWP) のネイティブ コードでインターフェイスを実装する方法について説明します。
- **[共有コードで実装する](#Implementing_in_Shared_Code)** &ndash;を使用する方法について説明します`DependencyService`共有コードからネイティブの実装を呼び出す。

アプリケーションを使用して、`DependencyService`次の構造になります。

![](device-orientation-images/orientation-diagram.png "DependencyService アプリケーション構造")

> [!NOTE]
> 説明するようデバイスの共有コードは、縦または横方向があるかどうかを検出することがでの [デバイス Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation)。 この記事で説明されているメソッドでは、向き、デバイスが上下かどうかなどの詳細を取得するのにネイティブ機能を使用します。

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>インターフェイスの作成

最初に、共有コードを実装する予定の機能を表すインターフェイスを作成します。 この例では、インターフェイスには、1 つのメソッドが含まれています。

```csharp
namespace DependencyServiceSample.Abstractions
{
    public enum DeviceOrientations
    {
        Undefined,
        Landscape,
        Portrait
    }

    public interface IDeviceOrientation
    {
        DeviceOrientations GetOrientation();
    }
}
```

共有コードでは、このインターフェイスに対するコーディングで、各プラットフォームでデバイスの向きの Api にアクセスする Xamarin.Forms アプリを許可します。

> [!NOTE]
> インターフェイスを実装するクラスには、パラメーターなしのコンス トラクターを使用する必要があります、`DependencyService`します。

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS の実装

各プラットフォームに固有のアプリケーション プロジェクトでインターフェイスを実装する必要があります。 クラスは、パラメーターなしのコンス トラクターように、`DependencyService`新しいインスタンスを作成できます。

```csharp
using UIKit;
using Foundation;

namespace DependencyServiceSample.iOS
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation(){ }

        public DeviceOrientations GetOrientation()
        {
            var currentOrientation = UIApplication.SharedApplication.StatusBarOrientation;
            bool isPortrait = currentOrientation == UIInterfaceOrientation.Portrait
                || currentOrientation == UIInterfaceOrientation.PortraitUpsideDown;

            return isPortrait ? DeviceOrientations.Portrait: DeviceOrientations.Landscape;
        }
    }
}
```

最後に、この追加`[assembly]`など必要な属性のクラスの上、定義されている任意の名前空間の外部)`using`ステートメント。

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

この属性の実装としてクラスを登録する、`IDeviceOrientation`インターフェイス、つまり`DependencyService.Get<IDeviceOrientation>`そのインスタンスを作成する共有コードで使用できます。

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android の実装

次のコードで実装`IDeviceOrientation`Android で。

```csharp
using DependencyServiceSample.Droid;
using Android.Hardware;

namespace DependencyServiceSample.Droid
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public static void Init() { }

        public DeviceOrientations GetOrientation()
        {
            IWindowManager windowManager = Android.App.Application.Context.GetSystemService(Context.WindowService).JavaCast<IWindowManager>();

            var rotation = windowManager.DefaultDisplay.Rotation;
            bool isLandscape = rotation == SurfaceOrientation.Rotation90 || rotation == SurfaceOrientation.Rotation270;
            return isLandscape ? DeviceOrientations.Landscape : DeviceOrientations.Portrait;
        }
    }
}
```

この追加`[assembly]`など必要な属性のクラスの上、定義されている任意の名前空間の外部)`using`ステートメント。

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

この属性の実装としてクラスを登録する、`IDeviceOrientaiton`インターフェイス、つまり`DependencyService.Get<IDeviceOrientation>`で使用できる共有コードは、そのインスタンスを作成できます。

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>ユニバーサル Windows プラットフォームの実装

次のコードの実装、`IDeviceOrientation`ユニバーサル Windows プラットフォーム上のインターフェイス。

```csharp
namespace DependencyServiceSample.WindowsPhone
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public DeviceOrientations GetOrientation()
        {
            var orientation = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            if (orientation == Windows.UI.ViewManagement.ApplicationViewOrientation.Landscape) {
                return DeviceOrientations.Landscape;
            }
            else {
                return DeviceOrientations.Portrait;
            }
        }
    }
}
```

追加、`[assembly]`など必要な属性のクラスの上、定義されている任意の名前空間の外部)`using`ステートメント。

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

この属性の実装としてクラスを登録する、`DeviceOrientationImplementation`インターフェイス、つまり`DependencyService.Get<IDeviceOrientation>`で使用できる共有コードは、そのインスタンスを作成できます。

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>共有コードで実装します。

記述し、アクセスする共有コードをテストするので、`IDeviceOrientation`インターフェイス。 この単純なページには、デバイスの向きに基づいて、独自のテキストを更新するボタンが含まれています。 使用して、`DependencyService`のインスタンスを取得する、`IDeviceOrientation`インターフェイス&ndash;実行時にこのインスタンスは、ネイティブ SDK へのフル アクセスのあるプラットフォーム固有の実装になります。

```csharp
public MainPage ()
{
    var orient = new Button {
        Text = "Get Orientation",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    orient.Clicked += (sender, e) => {
       var orientation = DependencyService.Get<IDeviceOrientation>().GetOrientation();
       switch(orientation){
           case DeviceOrientations.Undefined:
                orient.Text = "Undefined";
                break;
           case DeviceOrientations.Landscape:
                orient.Text = "Landscape";
                break;
           case DeviceOrientations.Portrait:
                orient.Text = "Portrait";
                break;
       }
    };
    Content = orient;
}
```

IOS、Android、または Windows プラットフォームでこのアプリケーションを実行し、ボタンを押すが、デバイスの向きを更新するボタンのテキスト。

![](device-orientation-images/orientation.png "デバイスの向きのサンプル")


## <a name="related-links"></a>関連リンク

- [DependencyService (サンプル) を使用します。](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (サンプル)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
