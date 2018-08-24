---
title: HttpClient および iOS と macOS の SSL/TLS の実装セレクター
description: HttpClient スタックと SSL/TLS の実装セレクターは、Xamarin iOS、tvOS、または macOS のアプリで使用される HttpClient と SSL/TLS の実装を決定します。
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: fd48c7148aadd8d156544113e2d719295294bf40
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251001"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>IOS と macOS の HttpClient と SSL/TLS の実装セレクター

**HttpClient 実装セレクター** Xamarin.iOS、Xamarin.tvOS、および Xamarin.Mac を制御する`HttpClient`使用する実装。 ネイティブ iOS、tvOS、または macOS のトランスポートを使用する実装を切り替えることができます (`NSUrlSession`または`CFNetwork`、OS によって異なります)。 メリットは TLS 1.2 のサポート、小さいバイナリ、および高速化をダウンロードします。欠点を実行する非同期操作が実行されているイベント ループが必要であります。

プロジェクトを参照する必要があります、 **System.Net.Http**アセンブリ。

> [!WARNING]
> **2018 年 4 月、** : セキュリティ強化のため主要なクラウド プロバイダーの要件、PCI のコンプライアンスを含むし、web サーバーが TLS バージョン 1.2 より前のサポートを停止する必要があります。  以前のバージョンの TLS を使用する Visual Studio の既定値の以前のバージョンで作成した Xamarin プロジェクト。
>
> アプリは引き続きこれらのサーバーとサービスを使用することを確認するには**で Xamarin プロジェクトを更新する必要があります、`NSUrlSession`再ビルドして、アプリを再デプロイし、下図のように設定すると、** をユーザーにします。

### <a name="selecting-an-httpclient-stack"></a>HttpClient スタックを選択します。

調整する、`HttpClient`アプリで使用されています。

1. ダブルクリックして、**プロジェクト名**で、**ソリューション エクスプ ローラー**プロジェクト オプション を開きます。
2. 切り替えて、**ビルド**プロジェクトの設定 (たとえば、 **iOS ビルド**Xamarin.iOS アプリの)。
3. **HttpClient 実装**] ドロップダウンで、[、`HttpClient`型として、次のいずれか: **NSUrlSession** (推奨)、 **CFNetwork**、または**マネージ**します。

[![管理されている、CFNetwork、または NSUrlSession から HttpClient の実装を選択します。](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> TLS 1.2 のサポートの`NSUrlSession`オプションはお勧めします。

### <a name="nsurlsession"></a>NSUrlSession

`NSURLSession`-ベースのハンドラーは、ネイティブに基づいて`NSURLSession`iOS 7 で使用でき、新しいフレームワークです。 
**これは、推奨される設定です。**

#### <a name="pros"></a>プロフェッショナル

- パフォーマンスが向上し、実行可能ファイルのサイズを小さくネイティブ Api を使用します。
- TLS 1.2 など、最新の標準のサポート。

#### <a name="cons"></a>短所

- IOS 7 以降が必要です。
- いくつか`HttpClient`機能/オプションは使用できません。

### <a name="cfnetwork"></a>CFNetwork

`CFNetwork`-ベースのハンドラーは、ネイティブに基づいて`CFNetwork`iOS 6 で使用でき、新しいフレームワークです。

#### <a name="pros"></a>プロフェッショナル

- パフォーマンスが向上し、実行可能ファイルのサイズを小さくネイティブ Api を使用します。
- TLS 1.2 などの新しい標準のサポート。

#### <a name="cons"></a>短所

- IOS 6 以降が必要です。
- WatchOS では使用できません。
- 一部の HttpClient 機能/オプションは使用できません。

### <a name="managed"></a>マネージド

マネージ ハンドラーは、以前のバージョンの Xamarin から同梱されている完全管理型の HttpClient ハンドラーです。

#### <a name="pros"></a>プロフェッショナル

- 以前のバージョンの Xamarin と Microsoft .NET 最も互換性のある機能があります。

#### <a name="cons"></a>短所

- Apple Os と完全に統合されていないと、TLS 1.0 に制限されています。 Web サーバーの保護や、クラウド サービスを将来的に接続することはできません。
- これは通常より非常に遅い暗号化などのネイティブ Api。
- マネージ コード、大規模なアプリを配布可能な作成が必要です。

### <a name="programmatically-setting-the-httpmessagehandler"></a>プログラムによって、HttpMessageHandler を設定

前に示したプロジェクト全体の構成、に加えてもインスタンス化できます、`HttpClient`し、必要な挿入`HttpMessageHandler`コンス トラクターで、これらのコード スニペットで示した。

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

別の使用が可能になります。`HttpMessageHandler`で宣言されているから、**プロジェクト オプション**ダイアログ。

## <a name="ssltls-implementation"></a>SSL/TLS の実装

SSL (Secure Socket Layer) とその後継の TLS (トランスポート層セキュリティ)、HTTP およびその他のネットワーク接続経由でのサポートを提供します。`System.Net.Security.SslStream`します。 Xamarin.iOS など Xamarin.tvOS Xamarin.Mac の`System.Net.Security.SslStream`実装は Mono で提供されるマネージ実装を使用する代わりに、Apple のネイティブの SSL/TLS 実装を呼び出します。 Apple のネイティブ実装では、TLS 1.2 をサポートします。

> [!WARNING]
> Xamarin.Mac 4.8 の今後のリリースは、macOS 10.9 以降のみをサポートします。
> Xamarin.Mac の以前のバージョンには、macOS 10.7 以上がサポートされていますが、これらの古い macOS バージョンが TLS 1.2 をサポートするための十分な TLS インフラストラクチャがないです。 対象の macOS 10.7 または macOS 10.8、Xamarin.Mac 4.6 以前を使用します。

## <a name="app-transport-security"></a>アプリケーション トランスポート セキュリティ

Apple の_アプリ トランスポート セキュリティ_(ATS) は、インターネット リソース (アプリのバック エンド サーバーなど) と、アプリの間のセキュリティで保護された接続を強制します。 ATS により、すべてのインターネット通信がセキュリティで保護された接続のベスト プラクティスに準拠しているアプリまたはライブラリを消費するを通じて直接に機密情報の偶発的漏えいを防ぐします。

ATS が iOS 9、tvOS 9 および OS X 10.11 (El Capitan) 用にビルドされたアプリで既定で有効になるためおよびそれ以降、すべての接続を使用して`NSUrlConnection`、`CFUrl`または`NSUrlSession`ATS セキュリティ要件が適用されます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。

HttpClient スタックと SSL/TLS の実装の選択に基づいて、ATS では正しく動作するアプリを変更する必要があります。

ATS に関する詳細については、次を参照してください、[アプリ トランスポート セキュリティ ガイド](~/ios/app-fundamentals/ats.md)します。

## <a name="known-issues"></a>既知の問題

このセクションでは、Xamarin.iOS での TLS サポートに関する既知の問題を説明します。

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>プロジェクトは、「要求された値が AppleTLS が見つかりませんでした」エラーで読み込みに失敗しました

Xamarin.iOS 9.8 に含まれるいくつかの新しい設定が導入されています、 **.csproj** Xamarin.iOS アプリケーションのファイル。 これらの変更で、以前のバージョンの Xamarin.iOS プロジェクトを開いたときに、問題が生じる場合があります。 次のスクリーン ショットでは、このシナリオで表示される可能性のあるエラー メッセージの例を示します。

![プロジェクトの読み込みを試みているときにエラーのスクリーン ショットが見つかりません。 値の従来の要求](http-stack-images/tlserror-xs.png)

導入によってこのエラーが原因となった、 `MtouchTlsProvider` Xamarin.iOS 9.8 でプロジェクト ファイルに設定します。 Xamarin.iOS 9.8 に更新できます (またはそれ以降) でない場合、回避策としては手動で編集する、 **.csproj**アプリケーションのファイルで、削除、`MtouchTlsprovider`要素、変更されたプロジェクト ファイルを保存します。

次のスニペットは、例のような`MtouchTlsProvider`設定に見えます内など、 **.csproj**ファイル。

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>関連リンク

- [トランスポート層セキュリティ (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)
