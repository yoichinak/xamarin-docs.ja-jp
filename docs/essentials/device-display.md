---
title: 'Xamarin.Essentials: デバイス情報を表示'
description: このドキュメントでは、アプリケーションが実行されているデバイスの画面の指標を提供する Xamarin.Essentials で DeviceDisplay クラスについて説明します。
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 830b96bcc21397047cb5aaacb5c568bc2ee863c4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782373"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: デバイス情報を表示

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

- [DeviceDisplay ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API ドキュメント](xref:Xamarin.Essentials.DeviceDisplay)
