---
title: 'Xamarin.Essentials: 画面のロック'
description: このドキュメントが画面をアプリケーションが実行されているときにスリープ状態の保持を要求する Xamarin.Essentials で ScreenLock クラスについて説明します。
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c8110b7abc86fe1d12485579f134997718540e6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782909"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: 画面のロック

![プレリリース NuGet](~/media/shared/pre-release.png)

**ScreenLock**フォール、アプリケーションが実行されているときにスリープ状態から画面を保持するクラスを要求できます。

## <a name="using-screenlock"></a>ScreenLock を使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

画面のロック機能に呼び出すことによって動作、`RequestActive`と`RequestRelease`をオフに画面を要求するメソッド。

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
