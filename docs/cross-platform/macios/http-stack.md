---
title: "HttpClient スタックと iOS/macOS の SSL/TLS の実装のセレクター"
description: "HttpClient スタック、および SSL や TLS 実装セレクターは、Xamarin iOS、tvOS、または macOS アプリで使用する HttpClient および SSL や TLS の実装を決定します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 06/12/2017
ms.openlocfilehash: 01b316e296f78ea2739e2f3ed1bd8d8ec112fca8
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-iosmacos"></a>HttpClient スタックと iOS/macOS の SSL/TLS の実装のセレクター

## <a name="httpclient-stack-selector"></a>HttpClient スタック セレクター

Xamarin.iOS、Xamarin.tvOS、および Xamarin.Mac: これを制御する`HttpClient`使用する実装。 既定値は引き続き、電源が入っている HttpClient`HttpWebRequest`をネイティブ iOS、tvOS、または macOS のトランスポートを使用して実装に必要に応じて切り替えるようになりました中に、(`NSUrlSession`または`CFNetwork`OS によって異なります)。 メリットは、小さなバイナリ、およびダウンロード時間が短縮、欠点は、非同期操作の実行を実行しているにイベント ループが必要です。

プロジェクトを参照する必要があります、 **System.Net.Http**アセンブリ。

<a name="Selecting-a-HttpClient-Stack" />

### <a name="selecting-a-httpclient-stack"></a>HttpClient スタックを選択します。

アプリで使用されている HttpClient を調整するには。

1. ダブルクリックして、**プロジェクト名**で、**ソリューション エクスプ ローラー**を開くには、プロジェクトのオプションです。
2. 切り替え、**ビルド**プロジェクトの設定 (たとえば、 **iOS ビルド**Xamarin.iOS アプリ)。
3. **HttpClient の実装**ドロップダウン リストで、次の 1 つとして、HttpClient を入力:**マネージ**、 **CFNetwork**または**NSUrlSession**.

[![管理されている、CFNetwork、NSUrlSession から HttpClient の実装を選択します。](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

<a name="Managed" />

### <a name="managed-default"></a>管理された (既定)

マネージ ハンドラーは、Xamarin の以前のバージョンから同梱されている完全に管理された HttpClient ハンドラーです。

#### <a name="pros"></a>長所:

 - 最も互換性のある機能の Microsoft .NET および古いバージョンの Xamarin のセットがあります。

#### <a name="cons"></a>短所:

 - Apple の Os と完全に統合されていないと、TLS 1.0 に制限されます。
 - これは通常より非常に遅い暗号化などに、ネイティブの Api。
 - マネージ コード、大規模なアプリを配布可能な作成が必要です。

<a name="CFNetwork" />

### <a name="cfnetwork"></a>CFNetwork

CFNetwork ベースのハンドラーは、ネイティブに基づいて`CFNetwork`framework iOS 6 で使用できると、それ以降。

#### <a name="pros"></a>長所:

 - ネイティブの Api を使用するパフォーマンスと実行可能ファイルのサイズを小さくします。
 - TLS 1.2 などの新しい標準をサポートします。

#### <a name="cons"></a>短所:

 - IOS 6 以降が必要です。
 - WatchOS では使用できません。
 - 一部の HttpClient 機能/オプションは使用できません。

<a name="NSUrlSession" />

### <a name="nsurlsession"></a>NSUrlSession

NSURLSession ベースのハンドラーは、ネイティブに基づいて`NSURLSession`framework iOS 7 で使用できると、それ以降。

#### <a name="pros"></a>長所:

 - ネイティブの Api を使用するパフォーマンスと実行可能ファイルのサイズを小さくします。
 - TLS 1.2 など最新の標準をサポートします。

#### <a name="cons"></a>短所:

 - IOS 7 以降が必要です。
 - 一部の HttpClient 機能/オプションは使用できません。

### <a name="programmatically-setting-the-httpmessagehandler"></a>HttpMessageHandler をプログラムによって設定

前に示したプロジェクト全体の構成、に加えてもインスタンスを作成できる、`HttpClient`し、必要な挿入`HttpMessageHandler`コンス トラクターで、これらのコード スニペットに示すよう。

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

これにより、さまざまなを使用する`HttpMessageHandler`に宣言されているから、**プロジェクト オプション**ダイアログ。

<a name="New-SSL-TLS-implementation-build-option" />
<a name="Selecting-a-SSL-TLS-implementation" />
<a name="Apple-TLS" />

## <a name="ssltls-implementation-build"></a>SSL や TLS 実装のビルド

SSL (Secure Socket Layer) とその後続バージョンの TLS (Transport Layer Security)、HTTP およびその他のネットワーク接続経由でのサポートを提供`System.Net.Security.SslStream`です。 Xamarin.iOS、Xamarin.tvOS または Xamarin.Mac の`System.Net.Security.SslStream`実装はモノラルによって提供されるマネージ実装を使用する代わりに、Apple のネイティブの SSL/TLS 実装を呼び出します。 Apple のネイティブ実装では、TLS 1.2 をサポートします。

<a name="Mono" />

> [!WARNING]
> **モノラル/マネージ**TLS プロバイダーは SSL v3 と TLS v1 に制限されます。 この TLS プロバイダーは廃止されており、Xamarin.iOS アプリケーション用に使用することはできなくします。 

<a name="App-Transport-Security" />

## <a name="app-transport-security"></a>アプリのトランスポート セキュリティ

Apple の_アプリ トランスポート セキュリティ_(ATS) がインターネット リソースの (アプリのバック エンド サーバーなど) と、アプリ間でセキュリティで保護された接続を強制します。 ATS により、すべてのインターネット通信がセキュリティで保護された接続のベスト プラクティスに準拠しているアプリまたはそれが消費するためのライブラリを通じて直接の機密情報の誤った情報開示を回避します。

ATS が iOS 9、tvOS 9 および OS X 10.11 (許可されて) 用にビルドされたアプリで既定で有効にされてから以降ではすべての接続を使用して`NSUrlConnection`、`CFUrl`または`NSUrlSession`ATS セキュリティ要件に従うことになります。 接続にはこれらの要件を満たしていない場合は、例外で失敗します。

選択されている HttpClient スタックおよび SSL や TLS 実装に基づいて、ATS で正常に動作するアプリを変更する必要があります。

ATS の詳細についてを参照してください、[アプリ トランスポート セキュリティ ガイド](~/ios/app-fundamentals/ats.md)です。

## <a name="known-issues"></a>既知の問題

このセクションでは、Xamarin.iOS での TLS のサポートに関する既知の問題を説明します。

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>プロジェクトがエラー「要求された値が AppleTLS が見つかりませんでした。」を読み込めませんでした。

Xamarin.iOS 9.8 に含まれている一部の新しい設定が導入された、 **.csproj** Xamarin.iOS アプリケーション用のファイルです。 これらの変更で、Xamarin.iOS の旧バージョンで、プロジェクトが開かれたときに、問題が生じる場合があります。 次のスクリーン ショットは、このシナリオで表示される可能性がありますのあるエラー メッセージの例を示します。

![プロジェクトの読み込みを試みているときにエラーのスクリーン ショットの要求値レガシが見つかりません。](http-stack-images/tlserror-xs.png)

このエラーの原因の導入で、 `MtouchTlsProvider` Xamarin.iOS 9.8 でプロジェクト ファイルに設定します。 そうでない Xamarin.iOS 9.8 を更新すること (またはそれ以降) 場合、回避策は手動で編集する、 **.csproj**アプリケーションのファイル、削除、`MtouchTlsprovider`要素、変更されたプロジェクト ファイルを保存します。

次のスニペットは、例のような`MtouchTlsProvider`設定は一見内と同様に、 **.csproj**ファイル。

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>関連リンク

- [トランスポート層セキュリティ (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)
