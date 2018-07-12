---
title: 'Xamarin.Essentials: 画面のロック'
description: このドキュメントでは、アプリケーションを実行するときにスリープ状態の画面を要求できる Xamarin.Essentials で ScreenLock クラスについて説明します。
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c8110b7abc86fe1d12485579f134997718540e6
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848571"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: 画面のロック

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**ScreenLock**クラスは、アプリケーションが実行されているときにスリープ状態の画面を要求できます。

## <a name="using-screenlock"></a>ScreenLock を使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

画面のロック機能を呼び出すことで機能、`RequestActive`と`RequestRelease`からオフにすると、画面を要求するメソッド。

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

- [画面のロックのソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [画面のロックの API ドキュメント](xref:Xamarin.Essentials.ScreenLock)
