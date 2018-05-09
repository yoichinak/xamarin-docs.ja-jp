---
title: Xamarin.Essentials 開くブラウザー
description: ブラウザーのクラスには、最適化されたシステム優先ブラウザーまたは外部のブラウザーで web リンクを開くためのアプリケーションができるようにします。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: e8e1a6973e7928032b2aa8669d36325b93671624
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials ブラウザー

![プレリリース NuGet](~/media/shared/pre-release.png)

**ブラウザー**クラスには、最適化されたシステム優先ブラウザーまたは外部のブラウザーで web リンクを開くためのアプリケーションができるようにします。

## <a name="using-browser"></a>ブラウザーを使用してください。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

ブラウザーの機能の動作を呼び出して、`OpenAsync`メソッドを`Uri`と`BrowserLaunchType`です。

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.RequestAsync(uri, BrowserLaunchType.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

起動の種類では、ブラウザーを起動する方法を決定します。

## <a name="system-preferred"></a>優先システム

[ユーザー設定 タブのクロム](https://developer.chrome.com/multidevice/android/customtabs)使用しようとしていますが、Uri を読み込み、ナビゲーション認識を保持します。

## <a name="external"></a>外部

`Intent`システムの通常のブラウザーで開く、Uri を要求するために使用されます。

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>優先システム

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) Uri を読み込み、ナビゲーション認識を保持するには使用できます。

## <a name="external"></a>外部

標準`OpenUrl`メイン アプリケーションでは、アプリケーションの外部で既定のブラウザーの起動に使用します。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

ユーザーの既定のブラウザーに関係なく常に起動する、`BrowserLaunchType`です。

--------------

## <a name="api"></a>API

- [ブラウザー ソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/Browser)
- [ブラウザー API ドキュメント](xref:Xamarin.Essentials.Browser)
