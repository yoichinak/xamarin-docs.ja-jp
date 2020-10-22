---
title: Xamarin.Essentials ブラウザーを開く
description: Xamarin.Essentials の Browser クラスを使用すると、アプリケーションから、最適化されたシステム推奨のブラウザーまたは外部のブラウザーで Web リンクを開くことができます。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 09/24/2020
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d4e89a30075cf0d59c90d386403c3ff533b518d6
ms.sourcegitcommit: dac04cec56290fb19034f3e135708f6966a8f035
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/19/2020
ms.locfileid: "92169941"
---
# <a name="no-locxamarinessentials-browser"></a>Xamarin.Essentials:ブラウザー

**Browser** クラスを使用すると、最適化されたシステム推奨のブラウザーまたは外部のブラウザーを使って、アプリケーションで Web リンクを開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**ブラウザー**機能にアクセスするには、次のプラットフォーム固有設定が必要です。

# <a name="android"></a>[Android](#tab/android)

プロジェクトのターゲット Android バージョンが **Android 11 (R API 30)** に設定される場合、新しい[パッケージの可視性要件](https://developer.android.com/preview/privacy/package-visibility)で使用されるクエリで Android マニフェストを更新する必要があります。

**[プロパティ]** フォルダーにある **AndroidManifest.xml** ファイルを開き、**manifest** ノードの内部に以下を追加します。

```xml
<queries>
  <intent>
    <action android:name="android.intent.action.VIEW" />
    <data android:scheme="http"/>
  </intent>
  <intent>
    <action android:name="android.intent.action.VIEW" />
    <data android:scheme="https"/>
  </intent>
</queries>
```

# <a name="ios"></a>[iOS](#tab/ios)

追加の設定は必要ありません。

# <a name="uwp"></a>[UWP](#tab/uwp)

プラットフォームによる違いはありません。

-----

## <a name="using-browser"></a>ブラウザーの使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

ブラウザー機能を動作させるには、`Uri`、`BrowserLaunchMode` と共に `OpenAsync` メソッドを呼び出します。

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        try
        {
            await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
        }
        catch(Exception ex)
        {
            // An unexpected error occured. No browser may be installed on the device.
        }
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

# <a name="android"></a>[Android](#tab/android)

起動モードによってブラウザーを起動する方法が決定されます。

## <a name="system-preferred"></a>システム推奨

[Custom Tabs](https://developer.chrome.com/multidevice/android/customtabs) を使用して URI を読み込み、ナビゲーション認識を保持することが試みられます。

## <a name="external"></a>外部

`Intent` を使用して、システムの通常のブラウザーで URI を開くよう要求します。

# <a name="ios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>システム推奨

[SFSafariViewController](xref:SafariServices.SFSafariViewController) を使用して、URI を読み込み、ナビゲーション認識を保持します。

## <a name="external"></a>外部

メイン アプリケーションの標準の `OpenUrl` を使用して、アプリケーションの外部で既定のブラウザーを起動します。

# <a name="uwp"></a>[UWP](#tab/uwp)

`BrowserLaunchMode` に関係なく、常にユーザーの既定のブラウザーが起動します。

--------------

## <a name="api"></a>API

- [Browser のソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Browser)
- [Browser API ドキュメント](xref:Xamarin.Essentials.Browser)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Open-Browser-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
