---
title: HttpClient および iOS/macOS の SSL/TLS の実装のセレクター
description: SSL/TLS と HttpClient スタック実装セレクター Xamarin iOS、tvOS、または macOS アプリで使用する HttpClient および SSL や TLS の実装で指定します。
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 9de2c97933bd33111a751be51e06dffe09794f15
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782269"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>HttpClient および iOS/macOS の SSL/TLS の実装のセレクター

**HttpClient の実装のセレクター** Xamarin.iOS、Xamarin.tvOS、および Xamarin.Mac を制御する`HttpClient`使用する実装。 ネイティブ iOS、tvOS、または macOS のトランスポートを使用して実装を切り替えることができます (`NSUrlSession`または`CFNetwork`、オペレーティング システムによって異なります)。 メリットは、TLS 1.2 サポート、小さなバイナリ、およびダウンロードも高速です。欠点は、非同期操作の実行を実行しているにイベント ループを必要とすることです。

プロジェクトを参照する必要があります、 **System.Net.Http**アセンブリ。

> [!WARNING]
> **年 4 月、2018年**– セキュリティの向上のため要件、PCI コンプライアンスを含むメジャー クラウド プロバイダーおよび TLS バージョン 1.2 より前のサポートを停止する web サーバーが必要です。  Xamarin プロジェクトが以前のバージョンの Visual Studio の既定値は、古いバージョンの TLS を使用して作成します。
>
> アプリがこれらのサーバーと、サービスの操作を続行することを確認するために **、Xamarin のプロジェクトを更新する必要があります、`NSUrlSession`次のように設定すると、再構築し、アプリを再デプロイ**をユーザーにします。

<a name="Selecting-a-HttpClient-Stack" />

### <a name="selecting-a-httpclient-stack"></a>HttpClient スタックを選択します。

調整する、`HttpClient`アプリによって使用されています。

1. ダブルクリックして、**プロジェクト名**で、**ソリューション エクスプ ローラー**を開くには、プロジェクトのオプションです。
2. 切り替え、**ビルド**プロジェクトの設定 (たとえば、 **iOS ビルド**Xamarin.iOS アプリ)。
3. **HttpClient の実装**ドロップダウンで、、`HttpClient`として、次のいずれかの型: **NSUrlSession** (推奨)、 **CFNetwork**、または**マネージ**です。

[![管理されている、CFNetwork、NSUrlSession から HttpClient の実装を選択します。](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> TLS 1.2 サポートについては、`NSUrlSession`オプションをお勧めします。

<a name="NSUrlSession" />

### <a name="nsurlsession"></a>NSUrlSession

`NSURLSession`-ベースのハンドラーは、ネイティブに基づいて`NSURLSession`framework iOS 7 で使用できると、それ以降。 
**これは、推奨される設定です。**

#### <a name="pros"></a>担当者

- ネイティブの Api を使用するパフォーマンスと実行可能ファイルのサイズを小さくします。
- TLS 1.2 など最新の標準のサポート。

#### <a name="cons"></a>短所

- IOS 7 以降が必要です。
- いくつか`HttpClient`機能/オプションは使用できません。

<a name="CFNetwork" />

### <a name="cfnetwork"></a>CFNetwork

`CFNetwork`-ベースのハンドラーは、ネイティブに基づいて`CFNetwork`framework iOS 6 で使用できると、それ以降。

#### <a name="pros"></a>担当者

- ネイティブの Api を使用するパフォーマンスと実行可能ファイルのサイズを小さくします。
- TLS 1.2 などの新しい標準をサポートします。

#### <a name="cons"></a>短所

- IOS 6 以降が必要です。
- WatchOS では使用できません。
- 一部の HttpClient 機能/オプションは使用できません。

<a name="Managed" />

### <a name="managed"></a>マネージ

マネージ ハンドラーは、Xamarin の以前のバージョンから同梱されている完全に管理された HttpClient ハンドラーです。

#### <a name="pros"></a>担当者

- 最も互換性のある機能の Microsoft .NET および古いバージョンの Xamarin のセットがあります。

#### <a name="cons"></a>短所

- Apple の Os と完全に統合されていないと、TLS 1.0 に制限されます。 Web サーバーを保護またはクラウド サービスを今後に接続することはできません。
- これは通常より非常に遅い暗号化などに、ネイティブの Api。
- マネージ コード、大規模なアプリを配布可能な作成が必要です。

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

## <a name="ssltls-implementation"></a>SSL や TLS の実装

SSL (Secure Socket Layer) とその後続バージョンの TLS (Transport Layer Security)、HTTP およびその他のネットワーク接続経由でのサポートを提供`System.Net.Security.SslStream`です。 Xamarin.iOS、Xamarin.tvOS または Xamarin.Mac の`System.Net.Security.SslStream`実装はモノラルによって提供されるマネージ実装を使用する代わりに、Apple のネイティブの SSL/TLS 実装を呼び出します。 Apple のネイティブ実装では、TLS 1.2 をサポートします。

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
