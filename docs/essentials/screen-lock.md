---
title: Xamarin.Essentials 画面のロック
description: ScreenLock クラスは、アプリケーションが実行されているときにスリープ状態の画面を保持する要求できます。
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 66702e9f8f5e6ad07f8f7c96f739edf1b2e66617
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials 画面のロック

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
        if (ScreenLock.IsMonitoring)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease;
    }
}
```

## <a name="api"></a>API

- [画面のロックのソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/ScreenLock)
- [画面のロックの API ドキュメント](xref:Xamarin.Essentials.ScreenLock)
