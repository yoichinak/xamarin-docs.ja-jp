---
title: Xamarin.iOS でのアプリの検索の機能強化
description: この記事が、拡張機能では iOS 10 と Xamarin.iOS でそれらを実装する方法にアプリを検索する Apple を行ってきました。
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: e61cb20f12f6373ef57beb759b933d3dd9b6e76e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110584"
---
# <a name="app-search-enhancements-in-xamarinios"></a>Xamarin.iOS でのアプリの検索の機能強化

_この記事が、拡張機能では iOS 10 と Xamarin.iOS でそれらを実装する方法にアプリを検索する Apple を行ってきました。_

Ios 10 では、Apple は引き出しますディープ リンク、アプリ内検索、検索継続および検証結果の視覚化などのアプリの検索へいくつかの機能強化が確立します。 この記事では、Xamarin.iOS アプリでこれらの機能の実装について説明します。

## <a name="about-app-search-enhancements"></a>アプリの検索の機能強化について

IOS 10 でコア スポット ライトは、次のアプリを検索するいくつかの機能強化を提供します。

- **(差分プライバシー) のディープ リンクの支持を引き出します**-検索結果でのディープ リンク アプリのコンテンツを昇格する方法を提供します。
- **アプリ内検索**-使用して、新しい`CSSearchQuery`メール、Messages や Notes アプリの動作方法と同様のアプリ内スポット ライト検索機能を提供するクラス。
- **継続を検索**- ユーザーをスポット ライトや、Safari で検索を開始し、アプリを開くし、その検索を続行します。
- **検証結果の視覚化**-Apple の[アプリ検索 API 検証ツール](https://search.developer.apple.com/appsearch-validation-tool)テストの実行時に web サイトのマークアップとディープ リンクのビジュアル表現が表示されます。
- **アプリの画像の共有をメッセージ**-スポット ライト検索で表示するメッセージ (メッセージ アプリ拡張機能) 経由で共有するために提供される一般的なアプリ内のイメージを使用します。

次のセクションには、これらのトピックについて詳しくは説明します。

## <a name="crowdsourced-deep-link-popularity"></a>人気の引き出しますディープ リンク

iOS 10 をアプリに一般的なディープ リンクの後、ユーザーでは、使用して、アプリの順位付けを向上させるためにこの情報の検索結果でコンテンツを使用して、ユーザーの id を保護しながら、頻度をカウントするためのメカニズムを提供する*差分プライバシー*します。

使用するアプリの`NSUserActivity`ディープ リンク Url を提供し、オブジェクト、`EligibleForPublicIndexing`プロパティに設定`true`、iOS 10 のサブセットを送信する*差分プライバシー ハッシュ*Apple のサーバーにします。 この情報は、検索結果で人気のあるアプリでコンテンツを昇格させるし、使用されます。

Xamarin.iOS アプリでのディープ リンクの実装の詳細についてを参照してください、 [NSUserActivity 検索](~/ios/platform/search/nsuseractivity.md)ドキュメント。

## <a name="in-app-searching"></a>アプリ内の検索

新しい実装することによって[CSSearchQuery](https://developer.apple.com/reference/corespotlight/cssearchquery)クラス、アプリはスポット ライトの検索とアプリ (にどのようなメール、Messages や Notes アプリケーションのままにする必要はありません、自体の内部コンテンツを検索する照合ルール テクノロジを提供できます作業時間)。

通常、アプリをサポートする`CSSearchQuery`各自で別の検索インデックスを維持する必要はありません。 

## <a name="search-continuation"></a>検索の継続

Ios 9 では、Apple は、Search Api を導入しました。 (コア スポット ライトなど`NSUserActivity`と web マークアップ) ディープ-好みのユーザーをスポット ライトと Safari の両方の検索インターフェイスを使用してそのコンテンツの検索を許可するアプリ内のコンテンツを提供します。 参照してください、[新しい Search APIs](~/ios/platform/search/index.md)詳細についてはドキュメントです。

IOS 10 Apple は、ユーザーをスポット ライトや、Safari で検索を開始し、アプリを開くと、検索を続行できるようにしてこの機能によって構築します。 

この機能を実装するには、編集、アプリの`Info.plist`ファイルを追加、`CoreSpotlightContinuation`型のキー**ブール**に値を設定および`YES`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](app-search-enhancements-images/search01.png "CoreSpotlightContinuation Info.plist ファイルの編集")](app-search-enhancements-images/search01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](app-search-enhancements-images/searchw01.png "CoreSpotlightContinuation Info.plist ファイルの編集")](app-search-enhancements-images/search01.png#lightbox)

-----

検索結果を引き続きユーザーに応答する (`NSUserActivity`)、編集、`AppDelegate.cs`オーバーライド ファイルを開き、`ContinueUserActivity`メソッド。 例えば:

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

このコードは、クエリの継続アクションの種類 (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`)、次に、ユーザーの現在のクエリから、読み取り、`NSUserActivity`クラスのユーザー情報のディクショナリ (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`)。 ここでは、アプリは、ユーザーの検索を続行するアクションを実行する必要があります。

Xamarin.iOS アプリでの検索の操作方法の詳細についてを参照してください、[コア スポット ライト検索](~/ios/platform/search/corespotlight.md)ドキュメント。

## <a name="visualization-of-validation-results"></a>検証結果の視覚化

Apple の[アプリ検索 API 検証ツール](https://search.developer.apple.com/appsearch-validation-tool)に web サイトのマークアップとディープ リンクのビジュアル表現が表示されます (などで定義されたマークアップを含む[Schema.org](http://schema.org/)) テストの実行時にします。

検証ツールを使用すると、開発者は Applebot Web クローラーがなど、タイトル、説明、URL およびその他のサイトのインデックス付けされる情報には、要素がサポートされているを確認できます。

Web マークアップの操作方法の詳細についてを参照してください、 [Web マークアップと検索](~/ios/platform/search/web-markup.md)ドキュメント。

## <a name="message-app-image-sharing"></a>メッセージ アプリの画像の共有

メッセージ アプリ拡張機能は、メッセージを共有するためのイメージを提供する場合は、ユーザーがアプリを終了することがなく、メッセージ内からよく使われるイメージのスポット ライト検索を実行できるようにする、拡張機能を構成できます。

この機能を有効にするには、次の操作を行います。

1. メッセージ アプリ拡張機能を作成します。
2. 追加、`com.apple.developer.associated-domains`アプリの権利をされメッセージ アプリ拡張機能を共有するイメージをホストする web ドメインの一覧が含まれます。 ドメインごとに、指定、`spotlight-image-search`サービス。
3. 追加、`apple-app-site-association`ファイル、イメージをホストしている web サイトにします。 このファイルにはディクショナリが含まれています、`spotlight-image-search`サービスし、アプリの ID には、チーム ID またはアプリ ID のプレフィックスの後に、バンドル ID が含まれています ファイルは、最大 500 台までのパスとがスポット ライトによってインデックスが作成され、一般的なイメージの検索に含めるパターンに含めることができます。 詳細については、Apple を参照してください[の作成と関連ファイルをアップロード](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4)ドキュメント。
4. Web サイトをクロールする Applebot を許可します。 Apple を参照してください[について Applebot](https://support.apple.com/HT204683)ドキュメント。

参照してください、[メッセージ アプリ統合](~/ios/platform/message-app-integration/index.md)詳細についてはドキュメントです。

## <a name="summary"></a>まとめ

この記事では、拡張機能をカバーされて Apple iOS 10 と Xamarin.iOS でそれらを実装する方法にアプリを検索する行ってきました。



## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
