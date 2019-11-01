---
title: Xamarin のアプリ検索の機能強化
description: この記事では、iOS 10 のアプリ検索に加えられた Apple の拡張機能と、それらを Xamarin. iOS に実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/15/2017
ms.openlocfilehash: f0d638d566290dd2ae0d8453133ee340d5b4ce3f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031589"
---
# <a name="app-search-enhancements-in-xamarinios"></a>Xamarin のアプリ検索の機能強化

_この記事では、iOS 10 のアプリ検索に加えられた Apple の拡張機能と、それらを Xamarin. iOS に実装する方法について説明します。_

IOS 10 では、Apple は引き出しディープリンク、アプリ内検索、検索継続、検証結果の視覚化など、アプリ検索に対していくつかの機能強化を行っています。 この記事では、これらの機能を Xamarin iOS アプリに実装する方法について説明します。

## <a name="about-app-search-enhancements"></a>アプリ検索の機能強化について

IOS 10 のコアスポットライトは、次のようなアプリ検索に対していくつかの機能強化を提供します。

- **引き出しディープリンクの人気度 (差分プライバシー)** -検索結果でディープリンクアプリのコンテンツを昇格する方法を提供します。
- **アプリ内検索**-新しい `CSSearchQuery` クラスを使用して、メール、メッセージ、ノートアプリの動作と同様に、アプリ内スポットライト検索機能を提供します。
- **[検索の継続]** -ユーザーがスポットライトまたは Safari で検索を開始し、アプリを開いて検索を続行できるようにします。
- **検証結果の視覚化**-Apple の[App Search API 検証ツール](https://search.developer.apple.com/appsearch-validation-tool)では、テストを事前に形成するときに、web サイトのマークアップとディープリンクが視覚的に表示されるようになりました。
- **メッセージアプリイメージの共有**-メッセージ (メッセージアプリ拡張機能を使用) での共有用に提供された、人気のあるアプリ内イメージがスポットライト検索に表示されます。

以下のセクションでは、これらのトピックについて詳しく説明します。

## <a name="crowdsourced-deep-link-popularity"></a>引き出しディープリンクの人気度

iOS 10 では、アプリへの一般的なディープリンクの後にユーザーが続く頻度をカウントし、この情報を使用して検索結果に含まれるアプリのコンテンツの順位を上げながら、差分を使用してユーザーの id を保護することができます。 *プライバシー*。

`NSUserActivity` オブジェクトを使用してディープリンク Url を提供し、`EligibleForPublicIndexing` プロパティを `true`に設定しているアプリの場合、iOS 10 は、一部の*プライバシーハッシュ*のサブセットを Apple のサーバーに送信します。 この情報は、検索結果で人気のあるアプリ内コンテンツを昇格させるために使用されます。

Xamarin. iOS アプリでディープリンクを実装する方法の詳細については、「 [NSUserActivity を使用した検索](~/ios/platform/search/nsuseractivity.md)」のドキュメントを参照してください。

## <a name="in-app-searching"></a>アプリ内検索

新しい[Cssearchquery](https://developer.apple.com/reference/corespotlight/cssearchquery)クラスを実装することで、アプリはスポットライトの検索と照合ルールテクノロジを提供して、ユーザーがアプリを離れる必要はありません (メール、メッセージ、ノートアプリの動作に似ています)。

通常、`CSSearchQuery` をサポートするアプリでは、独自の個別の検索インデックスを維持する必要はありません。

## <a name="search-continuation"></a>継続の検索

IOS 9 では、検索 Api (コアスポットライト、`NSUserActivity`、web マークアップなど) を使用して、ユーザーがスポットライトインターフェイスと Safari 検索インターフェイスの両方を使用してコンテンツを検索できるようにするために、アプリ内のコンテンツの詳細を提供しています。 詳細については、[新しい Search api](~/ios/platform/search/index.md)のドキュメントを参照してください。

IOS 10 Apple では、ユーザーがスポットライトまたは Safari で検索を開始できるようにすることでこの機能をビルドし、アプリを開いたときに検索を続行します。

この機能を実装するには、アプリの `Info.plist` ファイルを編集し、**ブール**型の `CoreSpotlightContinuation` キーを追加し、その値を `YES`に設定します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](app-search-enhancements-images/search01.png "Editing CoreSpotlightContinuation in the Info.plist file")](app-search-enhancements-images/search01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](app-search-enhancements-images/searchw01.png "Editing CoreSpotlightContinuation in the Info.plist file")](app-search-enhancements-images/search01.png#lightbox)

-----

ユーザーに対して検索結果 (`NSUserActivity`) を続行するには、`AppDelegate.cs` ファイルを編集し、`ContinueUserActivity` メソッドをオーバーライドします。 (例:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchQuery.ContinuationActionType) {
            var search = userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString);
            // Continue user's search here...
        }
        break;
    }

    return true;
}
```

このコードは、クエリ継続アクションの種類 (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`) を検索し、`NSUserActivity` クラスのユーザー情報ディクショナリ (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`) からユーザーの現在のクエリを読み取ります。 ここから、アプリはユーザーの検索を続行するためにアクションを実行する必要があります。

Xamarin iOS アプリでの検索の操作の詳細については、[コアスポットライトのドキュメントを](~/ios/platform/search/corespotlight.md)参照してください。

## <a name="visualization-of-validation-results"></a>検証結果の視覚化

Apple の[App SEARCH API 検証ツール](https://search.developer.apple.com/appsearch-validation-tool)では、テストを事前に実行するときに、web サイトのマークアップとディープリンク ( [Schema.org](https://schema.org/)で定義されているようなマークアップを含む) が視覚的に表示されるようになりました。

検証ツールを使用すると、開発者は、タイトル、説明、URL、その他のサポートされている要素など、Applebot Web クローラーによってサイトのインデックスが作成された情報を確認できます。

Web マークアップの使用方法の詳細については、 [Web マークアップ](~/ios/platform/search/web-markup.md)のドキュメントを参照してください。

## <a name="message-app-image-sharing"></a>メッセージアプリのイメージ共有

メッセージアプリ拡張機能がメッセージ内で共有するためのイメージを提供する場合、拡張機能を構成して、ユーザーがアプリを離れることなく、メッセージ内から人気のある画像を検索できるようにすることができます。

この機能を有効にするには、次の手順を実行します。

1. メッセージアプリ拡張機能を作成します。
2. アプリの権利に `com.apple.developer.associated-domains` を追加し、メッセージアプリ拡張機能が共有しているイメージをホストする web ドメインの一覧を含めます。 各ドメインに対して、`spotlight-image-search` サービスを指定します。
3. イメージをホストしている web サイトに `apple-app-site-association` ファイルを追加します。 このファイルには、`spotlight-image-search` サービスの辞書が含まれており、アプリの ID が含まれています。これには、チーム ID またはアプリ ID プレフィックスの後にバンドル ID が続きます。 このファイルには、最大500のパスと、スポットライトによってインデックスが作成され、一般的な画像検索に含まれるパターンを含めることができます。 詳細については、「Apple による[関連付けファイルの作成とアップロード](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4)」を参照してください。
4. Applebot が web サイトをクロールできるようにします。 Apple の[Applebot に関する](https://support.apple.com/HT204683)ドキュメントを参照してください。

詳細については、[メッセージアプリの統合](~/ios/platform/message-app-integration/index.md)に関するドキュメントを参照してください。

## <a name="summary"></a>まとめ

この記事では、iOS 10 でアプリ検索に加えられた Apple の拡張機能と、それらを Xamarin. iOS で実装する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
