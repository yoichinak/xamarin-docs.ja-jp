---
title: Xamarin.Essentials 開くブラウザー
description: Xamarin.Essentials でブラウザー クラスは、最適化されたシステムの推奨されるブラウザーや外部のブラウザーで web リンクを開くためのアプリケーションを使用できます。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e58d439f5a6eaafe9b1b5e7ca874a986e468cb9
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353283"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: ブラウザー

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**ブラウザー**クラスは、最適化されたシステムの推奨されるブラウザーや外部のブラウザーで web リンクを開くためのアプリケーションを使用できます。

## <a name="using-browser"></a>ブラウザーを使用してください。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

ブラウザーの機能を呼び出すことで機能、`OpenAsync`メソッドを`Uri`と`BrowserLaunchMode`します。

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

起動モードは、ブラウザーを起動する方法を決定します。

## <a name="system-preferred"></a>優先システム

[Chrome のカスタム タブ](https://developer.chrome.com/multidevice/android/customtabs)使用しようとしましたが、Uri を読み込み、ナビゲーションの認識を保持します。

## <a name="external"></a>外部

`Intent`システムの通常のブラウザーで開く、Uri を要求するために使用されます。

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>優先システム

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/)に Uri を読み込み、ナビゲーションの認識の保持に使用します。

## <a name="external"></a>外部

標準`OpenUrl`メイン アプリケーションでは、アプリケーションの外部で既定のブラウザーの起動を使用します。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

ユーザーの既定のブラウザーに関係なく常に起動する、`BrowserLaunchMode`します。

--------------

## <a name="api"></a>API

- [ブラウザーのソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [ブラウザー API ドキュメント](xref:Xamarin.Essentials.Browser)
