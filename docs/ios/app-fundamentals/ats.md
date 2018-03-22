---
title: "アプリのトランスポート セキュリティ"
description: "アプリのトランスポート セキュリティ (ATS) は、インターネット リソースの (アプリのバック エンド サーバーなど) と、アプリ間でセキュリティで保護された接続を強制します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 357528c559de36329ca4bf12ab2597247a17222d
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="app-transport-security"></a>アプリのトランスポート セキュリティ

_アプリのトランスポート セキュリティ (ATS) は、インターネット リソースの (アプリのバック エンド サーバーなど) と、アプリ間でセキュリティで保護された接続を強制します。_

この記事で iOS 9 のアプリでアプリのトランスポート セキュリティを適用するセキュリティの変更を導入して[、Xamarin.iOS プロジェクト用つまり](#xamarinsupport)に適用されます、 [ATS 構成オプション](#config)と対応する方法[ATS のオプトアウト](#optout)ATS 必要な場合です。 (明示的に許可した) 場合を除き、ATS が既定で有効になっているためには、すべてのセキュリティ保護されていないインターネット接続で iOS 9 のアプリで例外が発生します。


## <a name="about-app-transport-security"></a>アプリのトランスポート セキュリティの概要

前述のように、ATS により、iOS 9 および OS X 許可されてにすべてのインターネット通信がセキュリティで保護された接続のベスト プラクティスに準拠しているアプリまたはそのがライブラリを通じて直接の機密情報の誤った情報開示を防ぐ消費します。

既存のアプリでは、実装、`HTTPS`プロトコルの可能な場合です。 使用する必要があります、Xamarin.iOS アプリが新しい`HTTPS`インターネット リソースと通信するときに排他的です。 さらに、高度な API の通信は、前方の機密性と TLS バージョン 1.2 を使用して暗号化する必要があります。

との接続[NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)、 [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/)または[NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) iOS 9 および OS X 10.11 (許可されて) 用にビルドされたアプリでは既定で ATS が使用されます。

## <a name="default-ats-behavior"></a>ATS の既定の動作

使用するすべての接続を iOS 9 および OS X 10.11 (許可されて El) 用に開発されたアプリの既定で有効になって ATS のため[NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)、 [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/)または[NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/)対象になります。ATS セキュリティ要件です。 接続にはこれらの要件を満たしていない場合は、例外で失敗します。

### <a name="ats-connection-requirements"></a>ATS 接続要件

ATS すべてのインターネット接続の次の要件が適用されます。

- すべての接続の暗号は、前方の機密性を使用している必要があります。 受理された暗号の下の一覧を参照してください。
- トランスポート層セキュリティ (TLS) プロトコルは、バージョン 1.2 以上である必要があります。
- すべての証明書の 2048 ビットまたは大きい値の RSA キーまたは 256 ビットまたは大きい楕円曲線 (ECC) のキーと、少なくとも SHA256 指紋を使用する必要があります。

もう一度、ATS が iOS 9 に既定で有効になるために、これらの要件に適合しない接続を確立しようとするとは、例外がスローされるが発生します。 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>ATS 互換性のある暗号

ATS インターネット通信をセキュリティで保護では、次の前方 secrecy 暗号型が受け入れられます。

- `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA`
- `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`

IOS のインターネット通信のクラスの扱いの詳細については、Apple を参照してください[NSURLConnection クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755)、 [CFURL 参照](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206)または[NSURLSession クラス リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Xamarin.iOS で ATS のサポート

Xamarin.iOS アプリまたはライブラリや使用されているサービスがインターネットに接続する場合、既定では、iOS 9 および OS X 許可されて、ATS が有効に何らかのアクションを実行する必要があります。 またはの接続に例外がスローされます。

サポートするを Apple、既存のアプリの提案、`HTTPS`できるだけ早くプロトコルです。 できない場合はいずれかのサード パーティがサポートされていない web サービスに接続しているため`HTTPS`をサポートする場合または`HTTPS`非現実的、することができますオプトアウト ATS です。 参照してください、 [ATS の Opting アウト](#optout)詳細については、後述の「します。

新しい Xamarin.iOS アプリで使用する必要があります`HTTPS`インターネット リソースと通信するときに排他的です。 ここでも、ある可能性があります (サード パーティの web サービスを使用して) などの状況ですることはできず、ATS の脱退する必要があります。

さらに、ATS は前方の機密性と TLS バージョン 1.2 を使用して暗号化する高度な API 通信を強制します。 参照してください、 [ATS 接続要件](#ATS-Connection-Requirements)と[ATS 互換性のある暗号](#ATS-Compatible-Ciphers)上セクションでは、詳細についてはします。

TLS について熟知してはない可能性があります ([トランスポート層セキュリティ](https://en.wikipedia.org/wiki/Transport_Layer_Security)) SSL に代わるものである ([Secure Socket Layer](https://en.wikipedia.org/wiki/Transport_Layer_Security)) 経由でセキュリティを適用する暗号化プロトコルのコレクションを提供し、ネットワーク接続です。

TLS レベルを使用している web サービスによって制御され、アプリのコントロールの外部であるためです。 両方の`HttpClient`と`ModernHttpClient`最高レベルのサーバーでサポートされている TLS 暗号化を自動的に使用する必要があります。

によって、サーバー (特に、サード パーティ サービスである) 場合に説明することは、前方の機密性を無効にするか、TLS の下位レベルを選択する必要があります。 参照してください、 [ATS オプションの構成](#Configuring-ATS-Options)詳細については、後述の「します。

> [!IMPORTANT]
> 使用した Xamarin アプリにアプリのトランスポート セキュリティは適用されません**マネージ HTTPClient の実装**です。 CFNetwork を使用した接続に適用される**HTTPClient の実装**または**NSURLSession HTTPClient の実装**のみです。

### <a name="setting-the-httpclient-implementation"></a>HTTPClient の実装の設定

IOS アプリで使用する HTTPClient の実装を設定するには、ダブルクリック、**プロジェクト**で、**ソリューション エクスプ ローラー**を開くには、**プロジェクト オプション**です。 移動**iOS ビルド**下にある目的のクライアントの種類を選択し、 **HttpClient の実装**ドロップダウン。

![](ats-images/client01.png "IOS のビルド オプションの設定")


#### <a name="managed-handler"></a>マネージ ハンドラー

マネージ ハンドラーは、Xamarin.iOS の以前のバージョンから付属し、既定のハンドラーは、完全に管理された HttpClient ハンドラーです。

長所:

- Microsoft .NET および Xamarin の以前のバージョンと最も互換性勧めします。

短所:

- ない完全に統合されている iOS (例: TLS 1.0 に制限されます)。
- 通常、ネイティブの Api よりもかなり遅くなります。
- 複数のマネージ コードが必要ですし、大規模なアプリを作成します。

#### <a name="cfnetwork-handler"></a>CFNetwork ハンドラー

ベース CFNetwork ハンドラーは、ネイティブに基づいて`CFNetwork`フレームワークです。

長所:

- ネイティブ API を使用して、パフォーマンスと実行可能ファイルのサイズを小さくします。
- TLS 1.2 などの新しい標準のサポートを追加します。

短所:

- IOS 6 以降が必要です。
- WatchOS は使用できません。
- HttpClient の機能とオプションの一部では、使用できません。

#### <a name="nsurlsession-handler"></a>NSUrlSession ハンドラー

ベース NSUrlSession ハンドラーは、ネイティブに基づいて`NSUrlSession`API です。

長所:

- ネイティブ API を使用して、パフォーマンスと実行可能ファイルのサイズを小さくします。
- TLS 1.2 などの新しい標準のサポートを追加します。

短所:

- IOS 7 以降が必要です。
- HttpClient の機能とオプションの一部では、使用できません。 

## <a name="diagnosing-ats-issues"></a>ATS 問題を診断します。

直接、または iOS 9 の web ビューから、インターネットに接続するときに、フォームでエラーが発生する可能性があります。

> アプリのトランスポート セキュリティには、クリア テキストの HTTP がブロックされている (http://www.-the-blocked-domain.com)リソースの負荷がセキュリティで保護されていないためです。 一時的な例外は、アプリの Info.plist ファイルを使用して構成できます。

IOS9 では、アプリのトランスポート セキュリティ (ATS) は、インターネット リソースの (アプリのバック エンド サーバーなど) と、アプリ間でセキュリティで保護された接続を強制します。 さらに、ATS が必要な通信、`HTTPS`プロトコルと転送の機密性と TLS バージョン 1.2 を使用して暗号化する高レベルの API 通信します。

使用するすべての接続を iOS 9 および OS X 10.11 (許可されて El) 用に開発されたアプリの既定で有効になって ATS のため`NSURLConnection`、`CFURL`または`NSURLSession`ATS セキュリティ要件に従うことになります。 接続にはこれらの要件を満たしていない場合は、例外で失敗します。

Apple にも用意されています、 [TLSTool サンプル アプリ](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2)をコンパイルすることができます (または Xamarin と C# の場合にトランス コード必要に応じて) ATS/TLS の問題を診断するために使用します。 参照してください、 [ATS の Opting アウト](#optout)この問題を解決する方法については、後述の「します。


<a name="config" />

## <a name="configuring-ats-options"></a>ATS オプションを構成します。

ATS の機能のいくつかを構成するには、アプリので特定のキーの値を設定して**Info.plist**ファイル。 以下のキーは ATS を制御するために使用可能な (_ネストされている方法を表示するには、インデント_)。

```csharp
NSAppTransportSecurity
    NSAllowsArbitraryLoads
    NSAllowsArbitraryLoadsInWebContent
    NSExceptionDomains
    <domain-name-for-exception-as-string>
        NSExceptionMinimumTLSVersion
        NSExceptionRequiresForwardSecrecy
        NSExceptionAllowsInsecureHTTPLoads
        NSRequiresCertificateTransparency
        NSIncludesSubdomains
        NSThirdPartyExceptionMinimumTLSVersion
        NSThirdPartyExceptionRequiresForwardSecrecy
        NSThirdPartyExceptionAllowsInsecureHTTPLoads
```

各キーには、次の型と意味があります。

- **NSAppTransportSecurity** (`Dictionary`) - すべての設定キーが含まれています、ATS の値します。
- **NSAllowsArbitraryLoads** (`Boolean`) - 場合`YES`ATS は任意のドメインを無効になります**いない**記載`NSExceptionDomains`です。 表示されているドメインは、指定されたセキュリティ設定が使用されます。
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean`) - 場合`YES`を Apple のトランスポート セキュリティ (ATS) の保護は引き続き有効、アプリの残りの部分の中に正しく読み込む web ページを許可します。
- **NSExceptionDomains** (`Dictionary`) のドメインのコレクションをおよび ATS が特定のドメインを使用するセキュリティ設定。
- **< Domain-name-for-exception-as-string >** (`Dictionary`)-特定のドメイン (例外のコレクション `www.xamarin.com`)
- **NSExceptionMinimumTLSVersion** (`String`) のいずれかとして、最小限の TLS バージョン`TLSv1.0`、`TLSv1.1`または`TLSv1.2`(これは既定です)。
- **NSExceptionRequiresForwardSecrecy** (`Boolean`) - 場合`NO`ドメインは、暗号転送セキュリティが使用する必要はありません。 既定値は `YES` です。
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`) - 場合`NO`(既定値) をこのドメインのすべての通信がである必要があります、`HTTPS`プロトコルです。
- **NSRequiresCertificateTransparency** (`Boolean`) - 場合`YES`ドメインのセキュリティで保護された Sockets Layer (SSL) が有効な透過性のデータを含める必要があります。 既定値は `NO` です。
- **NSIncludesSubdomains** (`Boolean`) - 場合`YES`これらの設定は、このドメインのすべてのサブドメインを上書きします。 既定値は `NO` です。
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`)-TLS バージョンが、ドメインが、開発者のコントロールの外部のサード パーティ サービスである場合に使用します。
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`) - If `YES` a 3rd party domain requires forward secrecy.
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`) - If `YES` the ATS will allow non-secure communication with 3rd party domains.

<a name="optout" />

### <a name="opting-out-of-ats"></a>ATS アウトを無効にします。

使用して Apple が高提案中に、`HTTPS`プロトコルとセキュリティで保護された通信にインターネット ベースの情報、これは必ずしも可能な時間がある可能性があります。 たとえば、サード パーティの web サービスとの通信または広告を配信、アプリでインターネットを使用している場合。

次の変更が、アプリの場合は、Xamarin.iOS アプリは、セキュリティ保護されていないドメインに要求を行う必要があります、 **Info.plist**ファイルは ATS によって特定のドメインに適用される既定のセキュリティを無効になります。

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>www.the-domain-name.com</key>
        <dict>
            <key>NSExceptionMinimumTLSVersion</key>
            <string>TLSv1.0</string>
            <key>NSExceptionRequiresForwardSecrecy</key>
            <false/>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <true/>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>
```

Mac 用 Visual Studio 内をダブルクリックして、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**に切り替え、**ソース**表示し、上記のキーを追加。

[![](ats-images/ats01.png "Info.plist ファイルのソース ビュー")](ats-images/ats01.png#lightbox)


アプリを読み込んで、セキュリティで保護されたサイトから web コンテンツを表示する場合は、次をアプリに追加の**Info.plist**ファイルを Apple のトランスポート セキュリティ (ATS) 保護は引き続き有効の残りの部分の中に正しく読み込む web ページを許可するにはアプリ。

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

必要に応じて、アプリの次の変更を加えた**Info.plist**ファイルすべてのドメインとインターネット通信の ATS を完全に無効にします。

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

Mac 用 Visual Studio 内をダブルクリックして、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**に切り替え、**ソース**表示し、上記のキーを追加。

[![](ats-images/ats02.png "Info.plist ファイルのソース ビュー")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> アプリケーションでは、安全ではないの web サイトへの接続を必要とする場合は**常に**を使用して、例外として、ドメインを入力`NSExceptionDomains`ATS を使用して完全にオフではなく`NSAllowsArbitraryLoads`です。 `NSAllowsArbitraryLoads` 緊急の極端な状況でのみ使用する必要があります。




もう一度、ATS を無効にする必要があります_のみ_使用できなくなったか不可能な場合は、セキュリティで保護された接続に切り替える場合、最後の手段として使用します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事がアプリのトランスポート セキュリティ (ATS) を導入され、インターネットとのセキュリティで保護された通信を確保できる方法を説明します。 まず、Xamarin.iOS アプリの iOS 9 で実行されている必要があります ATS 変更内容を説明します。 制御の ATS 機能およびオプションを説明します。 最後に、オプトイン ATS 外 Xamarin.iOS アプリについて説明します。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
