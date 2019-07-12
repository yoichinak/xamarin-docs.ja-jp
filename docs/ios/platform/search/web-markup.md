---
title: Xamarin.iOS での Web マークアップの検索
description: このドキュメントでは、Xamarin.iOS アプリにリンクされる web ベースの検索結果を作成する方法について説明します。 これには、インデックス作成、アプリの web サイトを探索、スマート アプリ バナーやユニバーサル リンクを使用して可能にする web コンテンツを有効にする方法について説明します。
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: a9cf3dab9c112bf7ff99cbc0dd9541c3c1e35142
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830136"
---
# <a name="search-with-web-markup-in-xamarinios"></a>Xamarin.iOS での Web マークアップの検索

Web サイト経由でそのコンテンツへのアクセスを提供するアプリ (からだけでなく、アプリ内で)、web コンテンツを Apple によってクロールして、ユーザーの iOS 9 デバイス上のアプリにディープ リンクを提供できる特別なリンクでマークすることができます。

IOS アプリが既にモバイル ディープ リンクをサポートしている web サイトには、アプリ内で、Apple のコンテンツへのディープ リンクが表示される場合_Applebot_ web クローラーがこのコンテンツのインデックスし、そのクラウド インデックスに自動的に追加します。

[![](web-markup-images/webmarkup01.png "クラウドのインデックスの概要")](web-markup-images/webmarkup01.png#lightbox)

Apple では、スポット ライト検索と Safari の検索結果にこれらの結果を提示します。
ユーザーがタップした場合これらのいずれかの結果 (およびがインストールされているアプリ) し、アプリのコンテンツが表示されます。

[![](web-markup-images/webmarkup02.png "ディープ検索結果での web サイトからのリンク")](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>Web コンテンツのインデックス作成を有効にします。

4 つの手順をするアプリのコンテンツを検索できるように Web マークアップを使用する必要があります。

1. Apple が検出およびとして定義することで、アプリの web サイトのインデックスを作成できるよう、**サポート**または**マーケティング**iTunes Connect での web サイト。
2. アプリの web サイトにモバイル ディープ リンクを実装するために必要なマークアップが含まれていることを確認します。 詳細については、以下のセクションを参照してください。
3. IOS アプリでの処理のディープ リンクを有効にします。
4. リッチで魅力的な結果をエンドユーザーに提供する、アプリの web サイトで表示される構造化データ用のマークアップを追加します。 この手順は必須ではありませんが、Apple によってが強く勧めします。

次のセクションでは、これらの手順を詳しく見ていきます。

## <a name="make-your-apps-website-discoverable"></a>アプリの web サイトを探索可能に

Apple のアプリの web サイトを検索する最も簡単な方法がいずれかとして使用するには、**サポート**または**マーケティング**iTunes Connect 経由で Apple にアプリを送信するときに、web サイト。

## <a name="using-smart-app-banners"></a>スマート アプリ バナーを使用します。

アプリにクリアのリンクを表示するには、web サイト上のスマート アプリ バナーを提供します。 アプリがインストールされていない場合は、Safari は自動的にアプリをインストールするユーザーを求められます。 それ以外の場合、使用してをタップできます、**ビュー** web サイトからアプリを起動するリンク。 たとえば、スマート アプリ バナーを作成するには、次のコードを使用できます。

```xml
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

詳細については、Apple を参照してください[スマート アプリ バナーでアプリを昇格させる](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html)ドキュメント。

## <a name="using-universal-links"></a>ユニバーサル リンクを使用します。

新しいを iOS 9 の場合は、ユニバーサル リンクは、次を提供することでスマート アプリ バナー、または既存のカスタム URL スキームに代替を提供します。

- **一意**– 1 つ以上の web サイトで同じ URL を要求することはできません。
- **セキュリティで保護された**– に確実に自分でお持ちのお客様と、web サイトが所有する web サイトにリンクされているは、署名証明書が必要です、アプリ。
- **柔軟な**– エンドユーザーは、URL が、web サイトまたはアプリを起動するかどうかを制御できます。
- **ユニバーサル**– web サイトのと、アプリの両方のコンテンツを定義するのと同じ URL を使用できます。

## <a name="using-twitter-cards"></a>Twitter のカードを使用します。

Twitter のカードを使用して、アプリのコンテンツへのディープ リンクを行うことができます。 例えば:

```xml
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

詳細については、Twitter を参照してください[カード プロトコルの Twitter](http://dev.twitter.com/cards/mobile)ドキュメント。

## <a name="using-facebook-app-links"></a>Facebook アプリへのリンクを使用します。

Facebook アプリのリンクを使用して、アプリのコンテンツへのディープ リンクを行うことができます。 例えば:

```xml
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

詳細については、Facebook を参照してください[アプリ リンク](http://applinks.org)ドキュメント。

## <a name="opening-deep-links"></a>ディープ リンクを開く

開くと、Xamarin.iOS アプリでのディープ リンクの表示のサポートを追加する必要があります。 編集、 **AppDelegate.cs**オーバーライド ファイルを開き、`OpenURL`カスタムの URL 形式を処理するメソッド。 例えば:

```csharp
public override bool OpenUrl (UIApplication application, NSUrl url, string sourceApplication, NSObject annotation)
{

    // Handling a URL in the form http://company.com/appname/?123
    try {
        var components = new NSUrlComponents(url,true);
        var path = components.Path;
        var query = components.Query;

        // Is this a known format?
        if (path == "/appname") {
            // Display the view controller for the content
            // specified in query (123)
            return ContentViewController.LoadContent(query);
        }
    } catch {
        // Ignore issue for now
    }

    return false;
}
```

上記のコードを探しています、URL を含む`/appname`の値を渡すと`query`(`123`この例では) ユーザーに要求されたコンテンツを表示するようアプリのカスタム ビュー コント ローラーにします。

## <a name="providing-rich-results-with-structured-data"></a>構造化データを豊富な結果を提供します。

構造化データのマークアップを含めることによって単純にタイトルと説明だけでなくエンドユーザーに高度な検索結果を行うことができます。 イメージ、アプリ (評価など) の特定のデータおよび構造化データのマークアップを使用して結果にアクションが含まれます。

豊富な結果の詳細魅力的なと向上に役立つ、順位付け、クラウド内でより多くのユーザー対話機能を使用する魅力的な検索インデックスのベースします。

構造化データのマークアップを提供するための 1 つのオプションは、Open Graph を使用することです。 例えば:

```xml
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

詳細についてを参照してください、 [Open Graph](http://ogp.me) web サイト。

構造化データのマークアップのもう 1 つの一般的な形式は、schema.org のアフター形式です。 例えば:

```xml
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
    <span itemprop="ratingValue">4** stars -
    <span itemprop="reviewCount">255** reviews


```

Schema.org の %ld 個の JSON 形式では、同じ情報を表示することができます。

```xml
<script type="application/ld+json">
    "@content":"http://schema.org",
    "@type":"AggregateRating",
    "ratingValue":"4",
    "reviewCount":"255"
</script>
```

エンドユーザーに高度な検索結果を提供する web サイトからのメタデータの例を次に示します。

[![](web-markup-images/deeplink01.png "豊富なデータの構造化されたマークアップを使用して結果を検索します。")](web-markup-images/deeplink01.png#lightbox)

Apple は、現在 schema.org 次のスキーマの種類をサポートします。

- AggregateRating
- ImageObject
- InteractionCount
- 提供しています
- 組織
- PriceRange
- レシピ
- SearchAction

これらのスキームの種類の詳細についてを参照してください[schema.org](http://schema.org)します。

## <a name="providing-actions-with-structured-data"></a>構造化データをアクションを提供します。

特定の種類の構造化データには、エンドユーザーが対処できる検索結果をことができます。 現在、次の操作がサポートされています。

- 電話番号をダイヤルします。
- 指定されたアドレスにマップの方向を取得します。
- オーディオまたはビデオ ファイルを再生します。

たとえば、電話番号をダイヤルするアクションを定義するは、次のようになります可能性があります。

```xml
<div itemscope itemtype="http://schema.org/Organization">
    <span itemprop="telephone">(408) 555-1212**


```

この検索結果には、エンドユーザーに表示されたら、スマート フォンの小さなアイコンが結果に表示されます。 アイコンをタップすると、指定した数値が呼び出されます。

次の HTML では、検索結果からオーディオ ファイルを再生するアクションを追加します。

```xml
<div itemscope itemtype="http://schema.org/AudioObject">
    <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**


```

最後に、次の HTML は、検索結果からの方向を取得するアクションを追加します。

```xml
<div itemscope itemtype="http://schema.org/PostalAddress">
    <span itemprop="streetAddress">1 Infinite Loop**
    <span itemprop="addressLocality">Cupertino**
    <span itemprop="addressRegion">CA**
    <span itemprop="postalCode">95014**


```

詳細については、Apple を参照してください[アプリ検索開発者向けサイト](https://developer.apple.com/ios/search/)します。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [アプリの検索のプログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
