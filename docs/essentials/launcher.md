---
title: Xamarin.Essentials ランチャー
description: Xamarin.Essentials でランチャー クラスは、システムによって、URI を開くためのアプリケーションを使用できます。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 252bb873c1494265aafb2285057490ca29ce7419
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573634"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: ランチャー

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**ランチャー**クラスは、システムによって、URI を開くためのアプリケーションを使用できます。 これは、ディープ リンクを別のアプリケーションのカスタム URI スキームに設定するときに多くの場合、使用されます。 Web サイトにブラウザーを起動しようとするかどうかを参照してください、 **[ブラウザー](open-browser.md)**  API。

## <a name="using-launcher"></a>ランチャーを使用してください。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

ランチャー機能の呼び出しを使用する、`OpenAsync`メソッドを渡します、`string`または`Uri`を開きます。 必要に応じて、`CanOpenAsync`メソッドを使用して、デバイス上のアプリケーションで URI のスキーマを処理できるかどうかを確認します。

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## <a name="api"></a>API

- [ランチャーのソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [ランチャー API ドキュメント](xref:Xamarin.Essentials.Launcher)
