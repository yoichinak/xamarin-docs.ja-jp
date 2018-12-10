---
title: 'Xamarin.Essentials: デバイス ディスプレイ情報'
description: このドキュメントでは、アプリケーションが実行されているデバイスの画面のメトリックを提供する Xamarin.Essentials の DeviceDisplay クラスについて説明します。
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: d3102f0a4ed5f16c77c4a4768feb4a1565f2dd1a
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898892"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: デバイス ディスプレイ情報

**DeviceDisplay** クラスは、アプリケーションが実行されているデバイスの画面のメトリックに関する情報を提供し、アプリケーションの実行中に画面がスリープ状態にならないように要求することができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-devicedisplay"></a>DeviceDisplay の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

## <a name="main-display-info"></a>メインの表示情報

デバイスの基本情報に加えて、**DeviceDisplay** クラスには、デバイスの画面と向きに関する情報が含まれています。

```csharp
// Get Metrics
var mainDisplayInfo = DeviceDisplay.MainDisplayInfo;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = mainDisplayInfo.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = mainDisplayInfo.Rotation;

// Width (in pixels)
var width = mainDisplayInfo.Width;

// Height (in pixels)
var height = mainDisplayInfo.Height;

// Screen density
var density = mainDisplayInfo.Density;
```

**DeviceDisplay** クラスでは、画面のメトリックが変化すると常にトリガーされるイベントも公開されており、サブスクライブすることができます。

```csharp
public class DisplayInfoTest
{
    public DisplayInfoTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.MainDisplayInfoChanged += OnMainDisplayInfoChanged;
    }

    void OnMainDisplayInfoChanged(DisplayInfoChangedEventArgs  e)
    {
        // Process changes
        var displayInfo = e.DisplayInfo;
    }
}
```

**DeviceDisplay** クラスは、デバイスの画面がオフになったりロックされたりしないために設定できる、`KeepScreenOn` と呼ばれる `bool` プロパティを公開しています。

```csharp
public class KeepScreenOnTest
{
    public void ToggleScreenLock()
    {
        DeviceDisplay.KeepScreenOn = !DeviceDisplay.KeepScreenOn;
    }
}
```

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

違いはありません。

# <a name="iostabios"></a>[iOS](#tab/ios)

* `DeviceDisplay` へのアクセスは UI スレッドで行う必要があります。そうしないと、例外がスローされます。 [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) メソッドを使用して、UI スレッドでそのコードを実行できます。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

違いはありません。

--------------


## <a name="api"></a>API

- [DeviceDisplay のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API のドキュメント](xref:Xamarin.Essentials.DeviceDisplay)
