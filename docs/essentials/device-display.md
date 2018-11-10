---
title: 'Xamarin.Essentials: デバイス ディスプレイ情報'
description: このドキュメントでは、アプリケーションが実行されているデバイスの画面のメトリックを提供する Xamarin.Essentials の DeviceDisplay クラスについて説明します。
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ebe97cf7fbb78bff17196110e835bd35ff76b826
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674887"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: デバイス ディスプレイ情報

![プレリリースの NuGet](~/media/shared/pre-release.png)

**DeviceDisplay** クラスでは、アプリケーションが実行されているデバイスの画面のメトリックに関する情報が提供されます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-devicedisplay"></a>DeviceDisplay の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>画面のメトリック

デバイスの基本情報に加えて、**DeviceDisplay** クラスには、デバイスの画面と向きに関する情報が含まれています。

```csharp
// Get Metrics
var metrics = DeviceDisplay.ScreenMetrics;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = metrics.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = metrics.Rotation;

// Width (in pixels)
var width = metrics.Width;

// Height (in pixels)
var height = metrics.Height;

// Screen density
var density = metrics.Density;
```

**DeviceDisplay** クラスでは、画面のメトリックが変化すると常にトリガーされるイベントも公開されており、サブスクライブすることができます。

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChangedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

違いはありません。

# <a name="iostabios"></a>[iOS](#tab/ios)

* `ScreenMetrics` へのアクセスは UI スレッドで行う必要があります。そうしないと、例外がスローされます。 [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) メソッドを使用して、UI スレッドでそのコードを実行できます。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

違いはありません。

--------------


## <a name="api"></a>API

- [DeviceDisplay のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API のドキュメント](xref:Xamarin.Essentials.DeviceDisplay)
