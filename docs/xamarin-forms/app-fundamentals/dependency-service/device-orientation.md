---
title: デバイスの向きを確認する
description: この記事では、Xamarin.Forms の DependencyService クラスを使用して、共有コードからデバイスの向きにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: d8763c6fe8e330181c836bc8d10923ea676a07c1
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/06/2019
ms.locfileid: "66740947"
---
# <a name="checking-device-orientation"></a>デバイスの向きを確認する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)

この記事では、各プラットフォームでネイティブ API を使用して共有コードからデバイスの向きを確認するために、[`DependencyService`](xref:Xamarin.Forms.DependencyService) を使用する方法を説明します。 このチュートリアルは、Ali Özgür による既存の `DeviceOrientation` プラグインが基になっています。 詳しくは、[GitHub リポジトリ](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation)をご覧ください。

- **[インターフェイスの作成](#Creating_the_Interface)** &ndash; 共有コードでインターフェイスを作成する方法を理解します。
- **[iOS での実装](#iOS_Implementation)** &ndash; iOS 用のネイティブ コードでインターフェイスを実装する方法を学習します。
- **[Android での実装](#Android_Implementation)** &ndash; Android 用のネイティブ コードでインターフェイスを実装する方法を学習します。
- **[UWP での実装](#WindowsImplementation)** &ndash; ユニバーサル Windows プラットフォーム (UWP) 用のネイティブ コードでインターフェイスを実装する方法を学習します。
- **[共有コードでの実装](#Implementing_in_Shared_Code)** &ndash; `DependencyService` を使用して共有コードからネイティブ実装を呼び出す方法を学習します。

`DependencyService` を使用するアプリケーションは次のような構造になります。

![](device-orientation-images/orientation-diagram.png "DependencyService アプリケーションの構造")

> [!NOTE]
> 「[デバイスの向き](~/xamarin-forms/user-interface/layouts/device-orientation.md#Reacting_to_Changes_in_Orientation)」に示されているように、デバイスの向きが縦長または横長のどちらになっているかを、共有コードで検出することができます。 この記事で説明する方法では、ネイティブ機能を使用して、デバイスが上下逆かどうかなど、向きについての詳細な情報を取得します。

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>インターフェイスの作成

最初に、実装する機能を表すインターフェイスを共有コードで作成します。 この例では、インターフェイスには 1 つのメソッドが含まれます。

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

共有コードでこのインターフェイスに対してコーディングすると、Xamarin.Forms アプリから各プラットフォームのデバイスの向き API にアクセスできます。

> [!NOTE]
> インターフェイスを実装するクラスで `DependencyService` を使用するには、パラメーターのないコンストラクターが必要です。

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS での実装

各プラットフォームに固有のアプリケーション プロジェクトで、インターフェイスを実装する必要があります。 `DependencyService` で新しいインスタンスを作成できるよう、クラスにパラメーターなしのコンストラクターがあることに注意してください。

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

最後に、必要なすべての `using` ステートメントと共に、次の `[assembly]` 属性をクラスの上 (そして、定義されているすべての名前空間の外側) に追加します。

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

この属性では、クラスが `IDeviceOrientation` インターフェイスの実装として登録されます。つまり、共有コードで `DependencyService.Get<IDeviceOrientation>` を使用してそのインスタンスを作成できます。

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android での実装

Android では、次のコードで `IDeviceOrientation` を実装します。

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

必要なすべての `using` ステートメントと共に、次の `[assembly]` 属性をクラスの上 (そして、定義されているすべての名前空間の外側) に追加します。

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

この属性では、クラスが `IDeviceOrientaiton` インターフェイスの実装として登録されます。つまり、共有コードで `DependencyService.Get<IDeviceOrientation>` を使用してそのインスタンスを作成できます。

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>ユニバーサル Windows プラットフォームでの実装

ユニバーサル Windows プラットフォームでは、次のコードで `IDeviceOrientation` インターフェイスを実装します。

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

必要なすべての `using` ステートメントと共に、`[assembly]` 属性をクラスの上 (そして、定義されているすべての名前空間の外側) に追加します。

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

この属性では、クラスが `DeviceOrientationImplementation` インターフェイスの実装として登録されます。つまり、共有コードで `DependencyService.Get<IDeviceOrientation>` を使用してそのインスタンスを作成できます。

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>共有コードでの実装

これで、`IDeviceOrientation` インターフェイスにアクセスする共有コードを記述してテストできます。 このシンプルなページには、デバイスの向きに基づいて、それ自体のテキストを更新するボタンが含まれます。 `DependencyService` を使用して、`IDeviceOrientation` インターフェイスのインスタンスを取得します。実行時には、このインスタンスは、ネイティブ SDK にフル アクセスできるプラットフォーム固有の実装になります。

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

iOS、Android、または Windows プラットフォームでこのアプリケーションを実行してボタンを押すと、ボタンのテキストがデバイスの向きで更新されます。

![](device-orientation-images/orientation.png "デバイスの向きサンプル")


## <a name="related-links"></a>関連リンク

- [DependencyService の使用 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyService (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
