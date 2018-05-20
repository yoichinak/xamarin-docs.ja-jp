---
title: Xamarin.Essentials 開くブラウザー
description: ブラウザーのクラスには、最適化されたシステム優先ブラウザーまたは外部のブラウザーで web リンクを開くためのアプリケーションができるようにします。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2a95d0b89b78ce8b0ddfdc94d86c284a6f8b2bbe
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2018
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
        await Browser.OpenAsync(uri, BrowserLaunchType.SystemPreferred);
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

- [ブラウザー ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [ブラウザー API ドキュメント](xref:Xamarin.Essentials.Browser)
