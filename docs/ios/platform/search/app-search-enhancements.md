---
title: アプリの検索の機能強化
description: 拡張機能を取り上げて Apple iOS 10 および Xamarin.iOS でそれらを実装する方法にアプリを検索する行ってきました。
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 0df51429ea9655b0a72d9f4c1e413fa7e37410ac
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
---
# <a name="app-search-enhancements"></a>アプリの検索の機能強化

_拡張機能を取り上げて Apple iOS 10 および Xamarin.iOS でそれらを実装する方法にアプリを検索する行ってきました。_

10、iOS では、Apple と Crowdsourced ディープ リンク アプリ内の検索、継続タスクを検索および検証結果の視覚エフェクトなどのアプリの検索のいくつかの機能強化が行わします。 この記事では、Xamarin.iOS アプリでこれらの機能を実装することを説明します。

## <a name="about-app-search-enhancements"></a>アプリの検索の機能強化について

IOS 10 の主要なメディアは、次のアプリを検索するいくつかの拡張機能を提供します。

- **(差分プライバシー) の Crowdsourced ディープ リンク人気**-検索結果にコンテンツをディープ リンク アプリを昇格する方法を提供します。
- **アプリ内検索**-新しい`CSSearchQuery`メール、Messages や Notes アプリの作業と同様に、アプリ内・ Spotlight 検索機能を提供するクラス。
- **継続タスクを検索**- により、ユーザーはアプリを開き、メディアまたは Safari、検索を開始して、その検索を続行します。
- **検証結果の視覚化**-Apple の[アプリの検索 API 検証ツール](https://search.developer.apple.com/appsearch-validation-tool)preforming テストと web サイトのマークアップとディープ リンクの視覚的表現が表示されます。
- **アプリ画像の共有をメッセージ**-Spotlight 検索で表示するメッセージ (メッセージ アプリ拡張機能) 経由で共有するために提供される一般的なアプリでイメージを使用します。

次のセクションは、これらのトピックで詳しく説明します。

## <a name="crowdsourced-deep-link-popularity"></a>Crowdsourced ディープ リンク人気

iOS 10 は、人気のあるディープ リンクをアプリには、ユーザーに続けて、使用して、アプリの順位付けを向上させるためにこの情報の検索結果にコンテンツを使用して、ユーザーの id を保護しながら、頻度をカウントするメカニズムを備えています*。差分プライバシー*です。

アプリの使用`NSUserActivity`ディープ リンクの Url を提供するオブジェクト、`EligibleForPublicIndexing`プロパティに設定`true`、iOS 10 のサブセットを送信する*差分プライバシー ハッシュ*Apple のサーバーにします。 この情報は、検索結果によく使用されるアプリでコンテンツを昇格させるのには使用されます。

Xamarin.iOS アプリのディープ リンクの実装の詳細についてを参照してください、 [NSUserActivity を使って検索](~/ios/platform/search/nsuseractivity.md)ドキュメント。

## <a name="in-app-searching"></a>アプリ内の検索

新しい実装することによって[CSSearchQuery](https://developer.apple.com/reference/corespotlight/cssearchquery)クラス、アプリはスポット ライトの検索およびユーザーがアプリ (のような方法、メール、Messages や Notes アプリのままにしておく必要がなく、それ自体の内部でのコンテンツの検索に一致するルール テクノロジを提供できます作業時間)。

通常、アプリをサポートする`CSSearchQuery`独自、別の検索インデックスを保持する必要はありません。 

## <a name="search-continuation"></a>継続タスクを検索

Apple ios 9、検索 Api が導入されました (コアのスポット ライトなど`NSUserActivity`と web マークアップ) Spotlight および Safari の両方の検索インターフェイスを使用してそのコンテンツを検索できるようにするアプリ内のコンテンツの好みに応じての詳細を提供します。 参照してください、[新しい検索 Api](~/ios/platform/search/index.md)詳細についてはドキュメントです。

IOS 10 Apple は、メディアまたは Safari、検索を開始し、アプリを開いたときに、検索を継続するユーザーを許可することでこの機能を基に構築します。 

この機能を実装するには、編集、アプリの`Info.plist`ファイルに追加し、`CoreSpotlightContinuation`型のキー**ブール**にその値を設定および`YES`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](app-search-enhancements-images/search01.png "CoreSpotlightContinuation Info.plist ファイルを編集")](app-search-enhancements-images/search01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](app-search-enhancements-images/searchw01.png "CoreSpotlightContinuation Info.plist ファイルを編集")](app-search-enhancements-images/search01.png#lightbox)

-----

表示するには、検索結果の操作を続行する (`NSUserActivity`)、編集、`AppDelegate.cs`ファイルし、オーバーライド、`ContinueUserActivity`メソッドです。 例えば:

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

このコードは次のクエリの継続の処理の種類 (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`) から、ユーザーの現在のクエリを読み取り、`NSUserActivity`クラスのユーザー情報ディクショナリ (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`)。 ここでは、アプリはユーザーの検索を続行するアクションを実行する必要があります。

Xamarin.iOS アプリでの検索操作の詳細についてを参照してください、[コア Spotlight 検索](~/ios/platform/search/corespotlight.md)ドキュメント。

## <a name="visualization-of-validation-results"></a>検証結果の視覚化

Apple の[アプリの検索 API 検証ツール](https://search.developer.apple.com/appsearch-validation-tool)に web サイトのマークアップとディープ リンクの視覚的表現が表示されます (などで定義されているマークアップを含む[Schema.org](http://schema.org/)) preforming テスト時にします。

検証ツールを使用すると、開発者はなど、タイトル、説明、URL および他のサイトの Applebot の Web クローラーがインデックスを作成する情報がサポートされている要素を確認できます。

Web のマークアップと操作の詳細についてを参照してください、 [Web マークアップを含む検索](~/ios/platform/search/web-markup.md)ドキュメント。

## <a name="message-app-image-sharing"></a>メッセージ アプリ画像の共有

メッセージ アプリ拡張機能は、メッセージ内の共有のイメージを提供する場合は、ユーザーがアプリを離れることがなく、メッセージの中から一般的な画像のスポット ライト検索を実行できるようにする、拡張機能を構成できます。

この機能を有効にするには、次の操作を行います。

1. メッセージ アプリ拡張機能を作成します。
2. 追加、`com.apple.developer.associated-domains`アプリの権利をメッセージ アプリ拡張機能を共有してイメージをホストする web ドメインの一覧が含まれています。 ドメインごとに、指定、`spotlight-image-search`サービス。
3. 追加、`apple-app-site-association`ファイル、イメージをホストしている web サイトを参照します。 このファイルにはディクショナリが含まれています、`spotlight-image-search`サービスし、アプリの ID には、バンドル id 続けてチーム ID またはアプリ ID のプレフィックスが含まれています。 ファイルには、最大 500 のパスとスポット ライトでインデックス付けされたとする一般的な画像の検索に含めるパターンを含めることができます。 詳細については、Apple を参照してください[作成し、関連ファイルをアップロード](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4)ドキュメント。
4. Web サイトをクロールする Applebot を許可します。 Apple を参照してください[に関する Applebot](https://support.apple.com/HT204683)ドキュメント。

参照してください、[メッセージ アプリ統合](~/ios/platform/message-app-integration/index.md)詳細についてはドキュメントです。

## <a name="summary"></a>まとめ

この記事は、拡張機能をカバーされて Apple iOS 10 および Xamarin.iOS でそれらを実装する方法にアプリを検索する行ってきました。



## <a name="related-links"></a>関連リンク

- [iOS 10 サンプル](https://developer.xamarin.com/samples/ios/iOS10/)
