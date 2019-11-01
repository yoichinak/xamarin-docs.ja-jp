---
title: Xamarin で Web マークアップを使用して検索する
description: このドキュメントでは、Xamarin iOS アプリにリンクする web ベースの検索結果を作成する方法について説明します。 ここでは、web コンテンツのインデックス作成を有効にする方法、アプリの web サイトを検出可能にする方法、スマートアプリバナー、ユニバーサルリンクなどについて説明します。
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 3d5db2f060b59fc689bea99141342b0447ac8933
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031524"
---
# <a name="search-with-web-markup-in-xamarinios"></a>Xamarin で Web マークアップを使用して検索する

Web サイトを介してコンテンツへのアクセスを提供するアプリ (アプリ内からだけでなく) は、Apple によってクロールされる特別なリンクを使用して web コンテンツをマークし、ユーザーの iOS 9 デバイスでアプリへのディープリンクを提供することができます。

IOS アプリが既にモバイルディープリンクをサポートしていて、web サイトにアプリ内のコンテンツへのディープリンクが表示されている場合、Apple の_Applebot_ web クローラーはこのコンテンツのインデックスを作成し、クラウドインデックスに自動的に追加します。

[![](web-markup-images/webmarkup01.png "Cloud Index overview")](web-markup-images/webmarkup01.png#lightbox)

Apple は、これらの結果をスポットライト検索および Safari 検索結果に表示します。
ユーザーがこれらの結果のいずれかをタップした場合 (およびアプリがインストールされている場合)、アプリのコンテンツに移動します。

[![](web-markup-images/webmarkup02.png "Deep linking from a website in search results")](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>Web コンテンツのインデックス作成を有効にする

Web マークアップを使用してアプリのコンテンツを検索できるようにするには、次の4つの手順が必要です。

1. アプリの web サイトを検出してインデックスを作成するには、iTunes Connect の**サポート**または**マーケティング**web サイトとして定義します。
2. モバイルディープリンクを実装するために、アプリの web サイトに必要なマークアップが含まれていることを確認します。 詳細については、以下のセクションを参照してください。
3. IOS アプリでディープリンクの処理を有効にします。
4. アプリの web サイトに表示される構造化データのマークアップを追加して、エンドユーザーに豊富で魅力的な結果を提供します。 この手順は厳密には必須ではありませんが、Apple では強くお勧めします。

以下のセクションでは、これらの手順について詳しく説明します。

## <a name="make-your-apps-website-discoverable"></a>アプリの Web サイトを検出可能にする

Apple がアプリの web サイトを検索する最も簡単な方法は、iTunes Connect を使用して Apple にアプリを送信するときに、**サポート**または**マーケティング**web サイトとして使用することです。

## <a name="using-smart-app-banners"></a>スマートアプリバナーの使用

アプリに明確なリンクを表示するには、web サイトにスマートアプリバナーを提供します。 アプリがまだインストールされていない場合、Safari は自動的にアプリをインストールするようにユーザーに求めます。 それ以外の場合は、 **[表示]** リンクをタップして、web サイトからアプリを起動することができます。 たとえば、スマートアプリバナーを作成するには、次のコードを使用します。

```html
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

詳細については、Apple の「[アプリをスマートアプリのバナーで昇格](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html)する」を参照してください。

## <a name="using-universal-links"></a>ユニバーサルリンクの使用

IOS 9 の新機能であるユニバーサルリンクを使用すると、次のようなスマートアプリバナーまたは既存のカスタム URL スキームに代わる優れた方法となります。

- **一意**–同じ URL を複数の web サイトで要求することはできません。
- **Secure** – web サイトが所有者であり、アプリに有効なリンクがあることを確認するために、web サイトに署名入り証明書が必要です。
- **柔軟**: エンドユーザーは、URL が web サイトまたはアプリを起動するかどうかを制御できます。
- **Universal** –同じ URL を使用して、web サイトとアプリのコンテンツの両方を定義できます。

## <a name="using-twitter-cards"></a>Twitter カードの使用

Twitter カードを使用して、アプリのコンテンツへのディープリンクを提供できます。 (例:

```html
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

詳細については、Twitter の[Twitter カードのプロトコル](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/overview/abouts-cards)に関するドキュメントを参照してください。

## <a name="using-facebook-app-links"></a>Facebook アプリリンクの使用

Facebook アプリリンクを使用して、アプリのコンテンツへのディープリンクを提供できます。 (例:

```html
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

詳細については、Facebook の[アプリリンク](https://developers.facebook.com/docs/applinks)に関するドキュメントを参照してください。

## <a name="opening-deep-links"></a>ディープリンクを開く

Xamarin. iOS アプリでディープリンクを開いて表示するためのサポートを追加する必要があります。 **AppDelegate.cs**ファイルを編集し、`OpenURL` メソッドをオーバーライドして、カスタム URL 形式を処理します。 (例:

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

上記のコードでは、`/appname` を含む URL を探し、`query` (この例では`123`) の値をアプリのカスタムビューコントローラーに渡して、要求されたコンテンツをユーザーに表示します。

## <a name="providing-rich-results-with-structured-data"></a>構造化データを使用した豊富な結果の提供

構造化されたデータマークアップを含めることにより、タイトルと説明だけではなく、豊富な検索結果をエンドユーザーに提供できます。 構造化データマークアップを使用して、画像、アプリ固有のデータ (評価など)、結果に対するアクションを含めます。

豊富な結果を得ることができ、より多くのユーザーを魅力的して、クラウドベースの検索インデックスの順位を向上させることができます。

構造化データマークアップを提供するオプションの1つに、Open Graph を使用する方法があります。 (例:

```html
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

詳細については、「 [Open Graph](https://ogp.me) 」 web サイトを参照してください。

構造化データマークアップのもう1つの一般的な形式は、schema. org のマイクロデータ形式です。 (例:

```html
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
  <span itemprop="ratingValue">4** stars -
  <span itemprop="reviewCount">255** reviews
```

同じ情報は、次のようにスキーマで表すことができます。組織の JSON-LD 形式:

```html
<script type="application/ld+json">
  "@content":"http://schema.org",
  "@type":"AggregateRating",
  "ratingValue":"4",
  "reviewCount":"255"
</script>
```

Web サイトのメタデータの例を次に示します。この例では、リッチな検索結果をエンドユーザーに提供しています。

[![](web-markup-images/deeplink01.png "Rich search results via Structured Data Markup")](web-markup-images/deeplink01.png#lightbox)

現在、Apple は schema.org からの次のスキーマの種類をサポートしています。

- 集積
- ImageObject
- InteractionCount
- 提案
- 部門
- PriceRange
- レシピ
- SearchAction

これらのスキームの種類の詳細については、 [schema.org](https://schema.org)を参照してください。

## <a name="providing-actions-with-structured-data"></a>構造化データを使用したアクションの提供

特定の種類の構造化データを使用すると、エンドユーザーが検索結果を実行できるようになります。 現在、次のアクションがサポートされています。

- 電話番号をダイヤルしています。
- 指定されたアドレスへのマップの方向を取得しています。
- オーディオファイルまたはビデオファイルを再生しています。

たとえば、電話番号をダイヤルするアクションを定義すると、次のようになります。

```html
<div itemscope itemtype="http://schema.org/Organization">
  <span itemprop="telephone">(408) 555-1212**
```

この検索結果をエンドユーザーに表示すると、小さいスマートフォンアイコンが結果に表示されます。 ユーザーがアイコンをタップすると、指定した数値が呼び出されます。

次の HTML は、検索結果からオーディオファイルを再生するアクションを追加します。

```html
<div itemscope itemtype="http://schema.org/AudioObject">
  <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**
```

最後に、次の HTML では、検索結果から方向を取得するアクションを追加します。

```html
<div itemscope itemtype="http://schema.org/PostalAddress">
  <span itemprop="streetAddress">1 Infinite Loop**
  <span itemprop="addressLocality">Cupertino**
  <span itemprop="addressRegion">CA**
  <span itemprop="postalCode">95014**
```

詳細については、Apple の[アプリ検索開発者向けサイト](https://developer.apple.com/ios/search/)を参照してください。

## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [アプリ検索のプログラミングガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
