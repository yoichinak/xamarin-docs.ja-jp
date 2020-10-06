---
title: 'Xamarin.Essentials: スクリーン ショット'
description: このドキュメントでは、アプリの現在表示されている画面をキャプチャできる Xamarin.Essentials のスクリーンショット クラスについて説明します。
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 09/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 085da722aa2e893f97efb1c89f20b03da330ac3e
ms.sourcegitcommit: 744f977b0595f489c592e29c8a3ba548fde02b6f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2020
ms.locfileid: "91414711"
---
# <a name="no-locxamarinessentials-screenshot"></a>Xamarin.Essentials: スクリーン ショット

**スクリーンショット** クラスを使用すると、アプリの現在表示されている画面をキャプチャできます。

![プレリリース API](~/media/shared/preview.png)


## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-screenshot"></a>スクリーンショットの使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

次に、`CaptureAsync` を呼び出して、実行中のアプリケーションの現在の画面のスクリーンショットを取得します。 これにより、取得したスクリーンショットの `Width`、`Height`、`Stream` を取得するために使用できる `ScreenshotResult` が返されます。


```csharp
async Task CaptureScreenshot()
{
    var screenshot = await Screenshot.CaptureAsync();
    var stream = await screenshot.OpenReadAsync();

    Image = ImageSource.FromStream(() => stream);
}
```


## <a name="api"></a>API

- [ソース コードのスクリーンショット](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Screenshot)
- [API ドキュメントのスクリーンショット](xref:Xamarin.Essentials.Screenshot)
