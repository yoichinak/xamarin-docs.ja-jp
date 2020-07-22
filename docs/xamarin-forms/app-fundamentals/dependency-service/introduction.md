---
title: Xamarin.Forms の DependencyService の概要
description: この記事では、Xamarin.Forms の DependencyService クラスを使用してネイティブ プラットフォームの機能を呼び出す方法について説明します。
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/12/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f4d43a0c9c4878733d65b170c27e744b397aa4d0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138360"
---
# <a name="xamarinforms-dependencyservice-introduction"></a>Xamarin.Forms の DependencyService の概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)

[`DependencyService`](xref:Xamarin.Forms.DependencyService) クラスは、Xamarin.Forms アプリケーションで共有コードからネイティブ プラットフォームの機能を起動できるようにするサービス ロケーターです。

[`DependencyService`](xref:Xamarin.Forms.DependencyService) を使用してネイティブ プラットフォームの機能を呼び出すプロセスは、次のとおりです。

1. 共有コード内に、ネイティブ プラットフォームの機能用のインターフェイスを作成します。 詳細については、「[インターフェイスの作成](#create-an-interface)」をご覧ください。
1. 必要なプラットフォーム プロジェクト内にインターフェイスを実装します。 詳細については、「[各プラットフォームでのインターフェイスの実装](#implement-the-interface-on-each-platform)」をご覧ください。
1. [`DependencyService`](xref:Xamarin.Forms.DependencyService) にプラットフォームの実装を登録します。 これにより、実行時に Xamarin.Forms でプラットフォームの実装を検索できます。 詳細については、「[プラットフォームの実装の登録](#register-the-platform-implementations)」をご覧ください。
1. 共有コードからプラットフォームの実装を解決し、それらを呼び出します。 詳細については、「[プラットフォームの実装の解決](#resolve-the-platform-implementations)」をご覧ください。

次の図は、ネイティブ プラットフォームの機能が Xamarin.Forms アプリケーション内でどのように呼び出されるかを示しています。

![Xamarin.Forms の DependencyService クラスを使用したサービスの場所の概要](introduction-images/dependency-service.png "DependencyService サービスの場所")

## <a name="create-an-interface"></a>インターフェイスの作成

共有コードからネイティブ プラットフォームの機能を呼び出せるようにするための最初の手順は、ネイティブ プラットフォームの機能と対話するための API を定義するインターフェイスの作成です。 このインターフェイスは、共有コード プロジェクト内に配置する必要があります。

次の例は、デバイスの向きを取得するために使用できる API 用のインターフェイスを示しています。

```csharp
public interface IDeviceOrientationService
{
    DeviceOrientation GetOrientation();
}
```

## <a name="implement-the-interface-on-each-platform"></a>各プラットフォームでのインターフェイスの実装

ネイティブ プラットフォームの機能と対話するための API を定義するインターフェイスを作成した後で、各プラットフォーム プロジェクト内にインターフェイスを実装する必要があります。

### <a name="ios"></a>iOS

次のコード例は、iOS 上の `IDeviceOrientationService` インターフェイスの実装を示しています。

```csharp
namespace DependencyServiceDemos.iOS
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            UIInterfaceOrientation orientation = UIApplication.SharedApplication.StatusBarOrientation;

            bool isPortrait = orientation == UIInterfaceOrientation.Portrait ||
                orientation == UIInterfaceOrientation.PortraitUpsideDown;
            return isPortrait ? DeviceOrientation.Portrait : DeviceOrientation.Landscape;
        }
    }
}
```

### <a name="android"></a>Android

次のコード例は、Android 上の `IDeviceOrientationService` インターフェイスの実装を示しています。

```csharp
namespace DependencyServiceDemos.Droid
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            IWindowManager windowManager = Android.App.Application.Context.GetSystemService(Context.WindowService).JavaCast<IWindowManager>();

            SurfaceOrientation orientation = windowManager.DefaultDisplay.Rotation;
            bool isLandscape = orientation == SurfaceOrientation.Rotation90 ||
                orientation == SurfaceOrientation.Rotation270;
            return isLandscape ? DeviceOrientation.Landscape : DeviceOrientation.Portrait;
        }
    }
}
```

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

次のコード例は、ユニバーサル Windows プラットフォーム (UWP) 上の `IDeviceOrientationService` インターフェイスの実装を示しています。

```csharp
namespace DependencyServiceDemos.UWP
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            ApplicationViewOrientation orientation = ApplicationView.GetForCurrentView().Orientation;
            return orientation == ApplicationViewOrientation.Landscape ? DeviceOrientation.Landscape : DeviceOrientation.Portrait;
        }
    }
}
```

## <a name="register-the-platform-implementations"></a>プラットフォームの実装の登録

各プラットフォーム プロジェクト内にインターフェイスを実装した後で、プラットフォームの実装を [`DependencyService`](xref:Xamarin.Forms.DependencyService) に登録して、Xamarin.Forms でそれらを実行時に検索できるようにする必要があります。 これは一般に [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) を使用して実行され、指定した型によってインターフェイスの実装が提供されることを示します。

次の例は、[`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) を使用した `IDeviceOrientationService` インターフェイスの iOS 実装の登録を示しています。

```csharp
using Xamarin.Forms;

[assembly: Dependency(typeof(DependencyServiceDemos.iOS.DeviceOrientationService))]
namespace DependencyServiceDemos.iOS
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            ...
        }
    }
}
```

この例では、[`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) によって `DeviceOrientationService` を [`DependencyService`](xref:Xamarin.Forms.DependencyService) に登録しています。 同様に、[`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) を使用して他のプラットフォーム上の `IDeviceOrientationService` インターフェイスの実装を登録する必要があります。

[`DependencyService`](xref:Xamarin.Forms.DependencyService) へのプラットフォームの実装の登録の詳細については、「[Xamarin.Forms の DependencyService の登録と解決](registration-and-resolution.md)」をご覧ください。

## <a name="resolve-the-platform-implementations"></a>プラットフォームの実装の解決

[`DependencyService`](xref:Xamarin.Forms.DependencyService) にプラットフォームの実装を登録した後、呼び出される前にその実装を解決する必要があります。 これは一般に、[`DependencyService.Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) メソッドを使用して共有コード内で実行されます。

次のコードは、[`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) メソッドを呼び出して `IDeviceOrientationService` インターフェイスを解決してから、その `GetOrientation` メソッドを呼び出す例を示しています。

```csharp
IDeviceOrientationService service = DependencyService.Get<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

または、このコードは、1 行に圧縮できます。

```csharp
DeviceOrientation orientation = DependencyService.Get<IDeviceOrientationService>().GetOrientation();
```

[`DependencyService`](xref:Xamarin.Forms.DependencyService) でのプラットフォームの実装の解決の詳細については、「[Xamarin.Forms の DependencyService の登録と解決](registration-and-resolution.md)」をご覧ください。

## <a name="related-links"></a>関連リンク

- [DependencyService のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)
- [Xamarin.Forms の DependencyService の登録と解決](registration-and-resolution.md)
