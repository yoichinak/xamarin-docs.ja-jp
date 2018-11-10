---
title: 'Xamarin.Essentials: 画面ロック'
description: このドキュメントでは、アプリケーションの実行中に画面がスリープ状態にならないように要求できる Xamarin.Essentials の ScreenLock クラスについて説明します。
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3bf8c949650cf9f039a5a516366a90e717dc944b
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675316"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: 画面ロック

![プレリリースの NuGet](~/media/shared/pre-release.png)

**ScreenLock** クラスでは、アプリケーションの実行中に画面がスリープ状態にならないように要求できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-screenlock"></a>ScreenLock の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

画面ロック機能を使用するには、`RequestActive` および `RequestRelease` メソッドを使用して、画面のロックを有効または無効にします。

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (!ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>API

- [ScreenLock のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [ScreenLock API のドキュメント](xref:Xamarin.Essentials.ScreenLock)
