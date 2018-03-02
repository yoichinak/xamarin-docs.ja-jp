---
title: "デバイスの方向を確認しています"
description: "DependencyService を使用して共有コードからアクセス デバイスの向き"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: ee91f0ebdc07f03831ae95a4b8ae6f85c3eb549e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="checking-device-orientation"></a>デバイスの方向を確認しています

この記事を使用する方法は[ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/)を各プラットフォームでネイティブの Api を使用して共有コードから、デバイスの方向を確認します。 このチュートリアルは、既存の基づいて`DeviceOrientation`アリ Özgür によるプラグインします。 参照してください、 [GitHub リポジトリの](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation)詳細についてはします。

- **[インターフェイスを作成する](#Creating_the_Interface)** &ndash;を理解するインターフェイスに方法は共有コードで作成します。
- **[iOS 実装](#iOS_Implementation)** &ndash; iOS 用のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[Android 実装](#Android_Implementation)** &ndash; for Android のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[Windows 実装](#WindowsImplementation)** &ndash; Windows Phone とユニバーサル Windows プラットフォーム (UWP) のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[共有コードで実装する](#Implementing_in_Shared_Code)** &ndash;を使用する方法を学習`DependencyService`に共有コードからネイティブの実装を呼び出します。

アプリケーションを使用して、`DependencyService`次のような構造になります。

![](device-orientation-images/orientation-diagram.png "DependencyService アプリケーション構造")

> [!NOTE]
> **注:**に示すようにするかどうか、デバイスが縦または横方向に共有コードを検出するために可能であればの [デバイス Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation). この記事で説明した方法では、ネイティブの機能を使用して、詳細については、デバイスが上下逆にかどうかなどの向きを取得します。

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

共有コードでは、このインターフェイスに対するコーディングすると、各プラットフォームでデバイスの向き Api にアクセスする Xamarin.Forms アプリが許可されます。

> [!NOTE]
> **注**: インターフェイスを実装するクラスを使用するパラメーターなしのコンス トラクターを持つ必要があります、`DependencyService`です。

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS の実装

各プラットフォームに固有のアプリケーション プロジェクトでこのインターフェイスを実装する必要があります。 クラスが、パラメーターなしのコンス トラクターを持つことに注意してくださいできるように、`DependencyService`新しいインスタンスを作成できます。

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

最後に、この追加`[assembly]`など必要な属性のクラス上 (および定義されている任意の名前空間の外部)、`using`ステートメント。

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

次のコードを実装して`IDeviceOrientation`Android で。

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

この追加`[assembly]`など必要な属性のクラス上 (および定義されている任意の名前空間の外部)、`using`ステートメント。

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

この属性の実装としてクラスを登録する、`IDeviceOrientaiton`インターフェイス、つまり`DependencyService.Get<IDeviceOrientation>`で使用できる共有コードは、そのインスタンスを作成できます。

<a name="WindowsImplementation" />

## <a name="windows-phone-and-universal-windows-platform-implementation"></a>Windows Phone とユニバーサル Windows プラットフォームの実装

次のコードを実装して、`IDeviceOrientation`とユニバーサル Windows プラットフォームに Windows Phone 上のインターフェイス。

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

追加、`[assembly]`など必要な属性のクラス上 (および定義されている任意の名前空間の外部)、`using`ステートメント。

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

この属性の実装としてクラスを登録する、`DeviceOrientationImplementation`インターフェイス、つまり`DependencyService.Get<IDeviceOrientation>`で使用できる共有コードは、そのインスタンスを作成できます。

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>共有コードで実装します。

お記述して共有にアクセスするコードをテストできるようになりました、`IDeviceOrientation`インターフェイスです。 この単純なページには、デバイスの向きに基づく独自のテキストを更新するボタンが含まれています。 使用して、`DependencyService`のインスタンスを取得する、`IDeviceOrientation`インターフェイス&ndash;実行時にこのインスタンスになりますが、ネイティブの SDK へのフル アクセス プラットフォームに固有の実装。

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

IOS、Android、または Windows プラットフォームにこのアプリケーションを実行し、ボタンを押すが、デバイスの向きを使用する更新ボタンのテキスト。

![](device-orientation-images/orientation.png "デバイスの向きのサンプル")


## <a name="related-links"></a>関連リンク

- [DependencyService (サンプル) を使用します。](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (サンプル)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
