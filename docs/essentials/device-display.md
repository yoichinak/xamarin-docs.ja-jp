---
title: 'Xamarin.Essentials: デバイス情報の表示'
description: このドキュメントでは、アプリケーションが実行されているデバイスの画面のメトリックを提供する、Xamarin.Essentials で DeviceDisplay クラスについて説明します。
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3060d56e14fb0d3801a96ec0fe6e24c9efda4dac
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38816517"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: デバイス情報の表示

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**DeviceDisplay**クラスで、アプリケーションが実行されているデバイスの画面のメトリックについての情報を提供します。

## <a name="using-devicedisplay"></a>DeviceDisplay を使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>画面のメトリック

デバイスの基本情報に加えて、 **DeviceDisplay**クラスには、デバイスの画面と印刷の向きに関する情報が含まれています。

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

**DeviceDisplay**クラスでは、画面のいずれかのメトリック値を変更するたびにトリガーされるイベントをサブスクライブすることができますも公開します。

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChangedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>API

- [DeviceDisplay ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API ドキュメント](xref:Xamarin.Essentials.DeviceDisplay)
