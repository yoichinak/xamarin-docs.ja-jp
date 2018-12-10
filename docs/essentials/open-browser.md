---
title: Xamarin.Essentials ブラウザーを開く
description: Xamarin.Essentials の Browser クラスを使用すると、最適化されたシステム推奨のブラウザーまたは外部のブラウザーを使って、アプリケーションで Web リンクを開くことができます。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: ea2a10c11a77fcb2b3ce142d176522ebf0310725
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898879"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: ブラウザー

**Browser** クラスを使用すると、最適化されたシステム推奨のブラウザーまたは外部のブラウザーを使って、アプリケーションで Web リンクを開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-browser"></a>ブラウザーの使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

ブラウザー機能を動作させるには、`Uri`、`BrowserLaunchMode` と共に `OpenAsync` メソッドを呼び出します。

```csharp

public class BrowserTest
{
    public async Task<bool> OpenBrowser(Uri uri)
    {
        return await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

このメソッドは、ブラウザーが_起動_した後に返されるもので、必ずしもブラウザーがユーザーによって_終了_されるわけではありません。  `bool` の結果は起動が成功したかどうかを示しています。

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

起動モードによってブラウザーを起動する方法が決定されます。

## <a name="system-preferred"></a>システム推奨

[Chrome Custom Tabs](https://developer.chrome.com/multidevice/android/customtabs) を使用して URI を読み込み、ナビゲーション認識を保持することが試みられます。

## <a name="external"></a>外部

`Intent` を使用して、システムの通常のブラウザーで URI を開くよう要求します。

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>システム推奨

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) を使用して、URI を読み込み、ナビゲーション認識を保持します。

## <a name="external"></a>外部

メイン アプリケーションの標準の `OpenUrl` を使用して、アプリケーションの外部で既定のブラウザーを起動します。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

`BrowserLaunchMode` に関係なく、常にユーザーの既定のブラウザーが起動します。

--------------

## <a name="api"></a>API

- [Browser のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Browser API ドキュメント](xref:Xamarin.Essentials.Browser)
