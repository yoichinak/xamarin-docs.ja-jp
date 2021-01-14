---
title: 'Xamarin.Essentials: スクリーン ショット'
description: このドキュメントでは、アプリの現在表示されている画面をキャプチャできる Xamarin.Essentials のスクリーンショット クラスについて説明します。
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 01/04/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e907cf06814a5b14678e4584f9aabc56f39bb164
ms.sourcegitcommit: 995ee23d93e08dceb8754cc6c682cd2f4594345b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2021
ms.locfileid: "97972267"
---
# <a name="no-locxamarinessentials-screenshot"></a>Xamarin.Essentials: スクリーン ショット

**スクリーンショット** クラスを使用すると、アプリの現在表示されている画面をキャプチャできます。

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
