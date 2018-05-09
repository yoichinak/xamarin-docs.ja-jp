---
title: Xamarin.Essentials デバイス情報を表示
description: DeviceDisplay クラス実行している、デバイスの画面メトリック アプリケーションに関する情報を提供します。
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 4e78d0d22ee0766bbccdcd1617003cc9d0ccd73c
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials デバイス情報を表示

![プレリリース NuGet](~/media/shared/pre-release.png)

**DeviceDisplay**クラスでアプリケーションが実行されているデバイスの画面メトリックに関する情報を提供します。

## <a name="using-devicedisplay"></a>DeviceDisplay を使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>画面のメトリック

基本的なデバイス情報に加え、 **DeviceDisplay**クラスには、デバイスの画面と印刷の向きに関する情報が含まれています。

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

**DeviceDisplay**クラスもの公開と任意の画面のメトリック値を変更するたびにイベントをトリガーするイベントをサブスクライブすることができます。

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChanagedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>API

- [DeviceDisplay ソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/DeviceDisplay)
- [DeviceDisplay API ドキュメント](xref:Xamarin.Essentials.DeviceDisplay)
