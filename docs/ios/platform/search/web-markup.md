---
title: "Web のマークアップの検索"
description: "アプリケーションにリンクできる web ベースの検索結果を追加します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 63bc1f0ed13fe65b36e95978da9ccc2ea8d4481c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="search-with-web-markup"></a>Web のマークアップの検索

Web サイトを使用して、コンテンツへのアクセスを提供するアプリ (からだけでなく、アプリ内で)、web コンテンツは、Apple によってクロールされ、ユーザーの iOS 9 デバイス上のアプリにディープ リンクを提供できる特別なリンクでマークできます。

IOS アプリが既にモバイル ディープ リンクをサポートしており、web サイトには、アプリ内で、Apple のコンテンツへのディープ リンクが表示される場合_Applebot_の web クローラーがこのコンテンツのインデックスし、自動的にそのクラウド インデックスに追加します。

[ ![](web-markup-images/webmarkup01.png "クラウドのインデックスの概要")](web-markup-images/webmarkup01.png)

Apple は、Spotlight 検索で、Safari の検索結果にこれらの結果が表面化します。
ユーザーはタップ操作でこれらのいずれかが (と、インストールされているアプリがある) に、アプリのコンテンツが行われます。

[ ![](web-markup-images/webmarkup02.png "詳細な検索結果での web サイトからのリンク")](web-markup-images/webmarkup02.png)

## <a name="enabling-web-content-indexing"></a>Web コンテンツのインデックス作成を有効にします。

アプリのコンテンツを検索できるように Web マークアップを使用するために必要な 4 つの手順があります。

1. Apple が検出およびとして定義することで、アプリの web サイトのインデックスを作成できるよう、**サポート**または**マーケティング**iTunes Connect で web サイトです。
2. アプリの web サイトにモバイル ディープ リンクを実装するように必要なマークアップが含まれていることを確認します。 詳細については、次のセクションを参照してください。
3. IOS アプリでの処理のディープ リンクを有効にします。
4. 多機能かつ魅力的な結果をエンドユーザーに提供するため、アプリの web サイトによって公開される構造化データ用のマークアップを追加します。 この手順は必須ではありませんが、Apple によって強くお勧めします。

次のセクションでは、次の手順を詳しく見ていきます。

## <a name="make-your-apps-website-discoverable"></a>アプリの web サイトを検出できるように

Apple のアプリの web サイトを検索する最も簡単な方法がいずれかとして使用するには、**サポート**または**マーケティング**iTunes Connect 経由でアプリを Apple に送信するときに web サイトです。

## <a name="using-smart-app-banners"></a>スマート アプリ バナーを使用します。

アプリにクリア リンクを表示する web サイトでスマート アプリ バナーを提供します。 アプリがインストールされていない場合は、Safari は自動的にアプリをインストールするユーザーを求められます。 使用することができます をタップそれ以外の場合、**ビュー** web サイトからアプリを起動するリンクです。 たとえば、スマート アプリ バナーを作成するには、次のコードを使用することができます。

```xml
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

詳細については、Apple を参照してください[スマート アプリ バナーを使用してアプリを昇格させる](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html)ドキュメント。

## <a name="using-universal-links"></a>汎用のリンクを使用します。

新しい iOS 9 にユニバーサル リンクは、次を提供することでスマート アプリ バナーまたは既存のカスタム URL スキームがより優れた代替を提供します。

- **一意な**– 同じ URL を複数の web サイトが付くことはできません。
- **セキュリティで保護された**– にリンクされている web サイトで、かつ有効で、web サイトが所有することを確認する署名入り証明書が必要、アプリ。
- **柔軟な**– エンドユーザーが URL が、web サイトまたはアプリケーションを起動するかどうかを制御することができます。
- **ユニバーサル**– web サイトのと、アプリのコンテンツの両方を定義する、同じ URL を使用できます。

## <a name="using-twitter-cards"></a>Twitter のカードを使用します。

Twitter のカードを使用して、アプリのコンテンツへのディープ リンクを提供できます。 例:

```xml
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

詳細についてを参照してください Twitter の[カード プロトコルの Twitter](http://dev.twitter.com/cards/mobile)ドキュメント。

## <a name="using-facebook-app-links"></a>Facebook アプリのリンクを使用します。

Facebook アプリのリンクを使用して、アプリのコンテンツへのディープ リンクを提供できます。 例:

```xml
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

詳細については、Facebook を参照してください[アプリへのリンク](http://applinks.org)ドキュメント。

## <a name="opening-deep-links"></a>ディープ リンクを開く

開くおよび Xamarin.iOS アプリのディープ リンクを表示するためのサポートを追加する必要があります。 編集、 **<code>appdelegate.cs</code>**ファイルし、オーバーライド、`OpenURL`カスタム URL の形式を処理するメソッド。 例:

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

上記のコードで探しているを含む URL`/appname`の値を渡すと`query`(`123`この例では)、ユーザーに要求されたコンテンツを表示したり、アプリケーション内でカスタム ビュー コント ローラーにします。

## <a name="providing-rich-results-with-structured-data"></a>構造化データを提供する豊富な結果

構造化データのマークアップを含めることで、タイトルと説明だけ以外にも、エンドユーザーに多機能な検索結果を提供できます。 イメージ、アプリ固有のデータ (評価など) と構造化データのマークアップを使用して、結果のアクションが含まれます。

リッチ結果詳細関与するされ、改善おびき寄せますより多くのユーザーが対話機能を使用して、クラウドで、順位付けが検索インデックスをベースです。

構造化データのマークアップを提供するための 1 つのオプションは、Open Graph を使用します。 例:

```xml
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

詳細についてを参照してください、 [Open Graph](http://ogp.me) web サイトです。

構造化データ マークアップの他の一般的な形式は、schema.org のマイクロ データ形式です。 例:

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

多機能な検索結果をエンドユーザーに提供する web サイトからのメタデータの例を次に示します。

[ ![](web-markup-images/deeplink01.png "豊富な構造化データのマークアップを使用して結果を検索します。")](web-markup-images/deeplink01.png)

Apple には、現在 schema.org から次のスキーマ型がサポートされています。

 - AggregateRating
 - ImageObject
 - InteractionCount
 - します。
 - 組織
 - PriceRange
 - レシピ
 - SearchAction

これらのパターンの種類の詳細についてを参照してください[schema.org](http://schema.org)です。

## <a name="providing-actions-with-structured-data"></a>構造化データを使って操作を提供します。

特定の種類の構造化データのエンドユーザーが操作可能である検索結果が許可されます。 現在、次の操作がサポートされています。

 - 電話番号をダイヤルします。
 - 指定したアドレスにマップの方向を取得します。
 - オーディオまたはビデオ ファイルを再生します。

たとえば、電話番号をダイヤルするアクションを定義するは、次のようになります可能性があります。

```xml
<div itemscope itemtype="http://schema.org/Organization">
    <span itemprop="telephone">(408) 555-1212**


```

この検索結果がエンドユーザーに表示されたら、スマート フォンの小さなアイコンが結果に表示されます。 ユーザーは、アイコンをタップした、指定した数値が呼び出されます。

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

詳細については、Apple を参照してください[アプリ検索開発者向けサイト](http://developer.apple.com/ios/search/)です。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [アプリの検索プログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
