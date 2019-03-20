---
title: Xamarin.Essentials ブラウザーを開く
description: Xamarin.Essentials の Browser クラスを使用すると、最適化されたシステム推奨のブラウザーまたは外部のブラウザーを使って、アプリケーションで Web リンクを開くことができます。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: 4a822b4b6738e261b9ddaee02334ad629e1d4879
ms.sourcegitcommit: 64d6da88bb6ba222ab2decd2fdc8e95d377438a6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/18/2019
ms.locfileid: "58175318"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials:ブラウザー

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

## <a name="customization"></a>カスタマイズ

システム推奨ブラウザーの使用時、iOS と Android ではカスタマイズ オプションをいくつか利用できます。 たとえば、`TitleMode` (Android のみ)、`Toolbar` に推奨される色の選択肢 (iOS と Android)、`Controls` の表示 (iOS のみ) があります。 

このようなオプションは `OpenAsync` の呼び出し時、`BrowserLaunchOptions` を使用することで指定されます。

```csharp
await Browser.OpenAsync(uri, new BrowserLaunchOptions
                {
                    LaunchMode = BrowserLaunchMode.SystemPreferred,
                    TitleMode = BrowserTitleMode.Show,
                    PreferredToolbarColor = Color.AliceBlue,
                    PreferredControlColor = Color.Violet
                });
```

![ブラウザー オプション](images/browser-options.png)

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
