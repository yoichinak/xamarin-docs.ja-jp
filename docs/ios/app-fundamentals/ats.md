---
title: Xamarin.iOS でのアプリのトランスポート セキュリティ
description: アプリケーション トランスポート セキュリティ (ATS) は、インターネット リソース (アプリのバック エンド サーバーなど) と、アプリの間のセキュリティで保護された接続を強制します。
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2017
ms.openlocfilehash: f9308d3a746a5a0a43cf47cc5ea809c0f82bbe7b
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233823"
---
# <a name="app-transport-security-in-xamarinios"></a>Xamarin.iOS でのアプリのトランスポート セキュリティ

_アプリケーション トランスポート セキュリティ (ATS) は、インターネット リソース (アプリのバック エンド サーバーなど) と、アプリの間のセキュリティで保護された接続を強制します。_

この記事では、iOS 9 のアプリでアプリのトランスポート セキュリティを適用するセキュリティの変更を紹介し、 [、Xamarin.iOS プロジェクト用つまり](#xamarinsupport)が含まれます、 [ATS 構成オプション](#config)と対応する方法[ATS のオプトアウト](#optout)ATS が必要な場合。 この記事はATS構成オプションと必要な場合にATSからオプトアウトする方法を説明します。ATSは既定で有効となっているため、iOS 9アプリでは(明示的に許可しない限り)あらゆるセキュアでないインターネット接続によって例外が発生します。


## <a name="about-app-transport-security"></a>アプリケーション トランスポート セキュリティについて

前述のように、ATS により、iOS 9 アプリと OS X El Capitan ですべてのインターネット通信がセキュリティで保護された接続のベスト プラクティスに準拠しているか、アプリまたはライブラリがそれを使用して直接、機密情報の誤った情報開示を防ぐ消費しています。

既存のアプリでは、実装、`HTTPS`可能な限りプロトコルします。 新しい Xamarin.iOS アプリで使用する必要があります`HTTPS`インターネット リソースと通信するときに排他的です。 さらに、高度な API の通信は、前方の機密性と TLS バージョン 1.2 を使用して暗号化する必要があります。

による接続[NSUrlConnection](xref:Foundation.NSUrlConnection)、 [CFUrl](xref:CoreFoundation.CFUrl)または[NSUrlSession](xref:Foundation.NSUrlSession) iOS 9 および OS X 10.11 (El Capitan) 用に開発されたアプリの既定で ATS が使用されます。

## <a name="default-ats-behavior"></a>既定の ATS 動作

ATS が iOS 9 および OS X 10.11 (El Capitan) を使用してすべての接続に開発されたアプリの既定で有効になっているため[NSUrlConnection](xref:Foundation.NSUrlConnection)、 [CFUrl](xref:CoreFoundation.CFUrl)または[NSUrlSession](xref:Foundation.NSUrlSession)対象になります。ATS セキュリティ要件です。 接続はこれらの要件を満たしていない場合は、例外で失敗します。

### <a name="ats-connection-requirements"></a>ATS 接続の要件

ATS すべてのインターネット接続用の次の要件が適用されます。

- すべての接続の暗号は、前方の機密性を使用する必要があります。 承認済みの暗号を次の一覧を参照してください。
- トランスポート層セキュリティ (TLS) プロトコルはバージョン 1.2 以降である必要があります。
- すべての証明書は、2048 ビット、または大きい RSA キー、または 256 ビットまたは大きい楕円曲線 (ECC) キーを少なくとも SHA256 指紋を使用する必要があります。

ここでも、ATS が iOS 9 で既定で有効にするためにこれらの要件を満たしていない接続を確立しようとするとになります例外がスローします。 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>ATS 互換性のある暗号

インターネット通信をセキュリティで保護された ATS で次の前方 secrecy 暗号の種類は受け入れられます。

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

IOS のインターネット通信のクラスの使用方法の詳細については、Apple を参照してください[NSURLConnection クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755)、 [CFURL 参照](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206)または[NSURLSession クラスのリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Xamarin.iOS での ATS のサポート

Xamarin.iOS アプリまたはライブラリやサービスを使用していますが、インターネットに接続を行う場合、既定では、iOS 9 および OS X El Capitan、ATS が有効には、いくつかのアクションを実行する必要があります。 またはの接続がスローされる例外になります。

サポートするを Apple、既存のアプリの提案、`HTTPS`できるだけ早くプロトコルします。 サード パーティでサポートされていない web サービスに接続しているため、接続できないか場合`HTTPS`をサポートしている場合、または`HTTPS`非現実的、することができますオプトアウト ATS します。 参照してください、 [ATS の Opting アウト](#optout)詳細については後述します。

新しい Xamarin.iOS アプリでは、使用する必要があります`HTTPS`インターネット リソースと通信するときに排他的です。 ここでも、する可能性があります (サード パーティの web サービスを使用して) ような場合は、これが不可能な ATS のオプトアウトする必要があります。

さらに、ATS が前方の機密性と TLS バージョン 1.2 を使用して暗号化する高度な API の通信を強制します。 参照してください、 [ATS 接続要件](#ATS-Connection-Requirements)と[ATS 互換性のある暗号](#ATS-Compatible-Ciphers)詳細については、上記セクションします。

TLS について熟知するはない可能性があります ([トランスポート層セキュリティ](https://en.wikipedia.org/wiki/Transport_Layer_Security)) は SSL の後継 ([Secure Socket Layer](https://en.wikipedia.org/wiki/Transport_Layer_Security)) 経由でセキュリティを適用する暗号化プロトコルのコレクションを提供し、ネットワーク接続。

TLS レベルでは、消費している web サービスによって制御され、アプリのコントロールの外部では、そのためです。 両方の`HttpClient`、`ModernHttpClient`最高レベルのサーバーでサポートされている TLS 暗号化を自動的に使用する必要があります。

サーバーによって異なります (特にサード パーティのサービスがある) 場合に説明することは、前方の機密性を無効にするか、または下位の TLS を選択する必要があります。 参照してください、 [ATS オプションの構成](#Configuring-ATS-Options)詳細については後述します。

> [!IMPORTANT]
> 使用して Xamarin アプリにアプリのトランスポート セキュリティは適用されません**マネージ HTTPClient の実装**します。 CFNetwork を使用した接続に適用される**HTTPClient の実装**または**NSURLSession HTTPClient の実装**のみです。

### <a name="setting-the-httpclient-implementation"></a>HTTPClient 実装の設定

IOS アプリで使用する HTTPClient 実装を設定するには、ダブルクリック、**プロジェクト**で、**ソリューション エクスプ ローラー**を開く、**プロジェクト オプション**します。 移動します**iOS ビルド**で目的のクライアントの種類を選択し、 **HttpClient 実装**ドロップダウン。

![](ats-images/client01.png "IOS ビルド オプションの設定")


#### <a name="managed-handler"></a>マネージ ハンドラー

マネージ ハンドラーは、以前のバージョンの Xamarin.iOS から付属し、既定のハンドラーは、完全に管理された HttpClient ハンドラーです。

長所:

- 以前のバージョンの Xamarin と Microsoft .NET 最も互換性があります。

短所:

- IOS (例: TLS 1.0 に制限されます) と完全に統合されていません。
- 通常はネイティブ Api よりかなり遅くなります。
- 多くのマネージ コードが必要ですし、大規模なアプリを作成します。

#### <a name="cfnetwork-handler"></a>CFNetwork ハンドラー

CFNetwork ベースのハンドラーは、ネイティブに基づいて`CFNetwork`フレームワーク。

長所:

- ネイティブ API を使用して、パフォーマンスと実行可能ファイルのサイズが小さい。
- TLS 1.2 などの新しい標準のサポートを追加します。

短所:

- IOS 6 以降が必要です。
- WatchOS は使用できません。
- HttpClient 機能とオプションの一部は使用できません。

#### <a name="nsurlsession-handler"></a>NSUrlSession ハンドラー

NSUrlSession ベースのハンドラーは、ネイティブに基づいて`NSUrlSession`API。

長所:

- ネイティブ API を使用して、パフォーマンスと実行可能ファイルのサイズが小さい。
- TLS 1.2 などの新しい標準のサポートを追加します。

短所:

- IOS 7 以降が必要です。
- HttpClient 機能とオプションの一部は使用できません。 

## <a name="diagnosing-ats-issues"></a>ATS 問題を診断します。

直接、または iOS 9 の web ビューから、インターネットに接続するときに、フォームでエラーが発生する可能性があります。

> アプリのトランスポート セキュリティには、クリア HTTP によってブロックされている (http://www.-the-blocked-domain.com)安全でないため、リソースの読み込み。 一時的な例外は、アプリの Info.plist ファイルで構成できます。

Ios 9、App Transport Security (ATS) は、インターネット リソース (アプリのバック エンド サーバーなど) と、アプリの間のセキュリティで保護された接続を強制します。 さらに、ATS でを使用して通信が必要です、`HTTPS`プロトコルと TLS バージョン 1.2 を使用して、前方の機密性を暗号化する高度な API の通信。

ATS が iOS 9 および OS X 10.11 (El Capitan) を使用してすべての接続に開発されたアプリの既定で有効になっているため`NSURLConnection`、`CFURL`または`NSURLSession`ATS セキュリティ要件が適用されます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。

Apple から提供されても、 [TLSTool サンプル アプリ](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2)をコンパイルすることができます (または Xamarin に必要に応じてトランス コードされたとC#) ATS や TLS の問題を診断するために使用します。 参照してください、 [ATS の Opting アウト](#optout)この問題を解決する方法については後述します。


<a name="config" />

## <a name="configuring-ats-options"></a>ATS オプションを構成します。

いくつかの ATS の機能を構成するには、アプリの特定のキーの値を設定して**Info.plist**ファイル。 次のキーは、ATS を制御するため利用 (_インデントが設定された入れ子にする方法を表示する_)。

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

- **NSAppTransportSecurity** (`Dictionary`) - すべての設定キーが含まれていて、ATS の値します。
- **NSAllowsArbitraryLoads** (`Boolean`) - 場合`YES`ATS が任意のドメインを無効になります**いない**記載`NSExceptionDomains`します。 表示されているドメインは、指定されたセキュリティ設定が使用されます。
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean`) - 場合`YES`できるように、アプリの残りの部分の Apple Transport Security (ATS) の保護が有効にも中に正しく読み込む web ページになります。
- **NSExceptionDomains** (`Dictionary`) のドメインのコレクションを特定のドメインについて ATS が使用するセキュリティ設定。
- **< Domain-name-for-exception-as-string >** (`Dictionary`)-(例: 特定のドメインについての例外のコレクション `www.xamarin.com`)
- **NSExceptionMinimumTLSVersion** (`String`) のいずれかとして最低限の TLS バージョン`TLSv1.0`、`TLSv1.1`または`TLSv1.2`(これは既定です)。
- **NSExceptionRequiresForwardSecrecy** (`Boolean`) - 場合`NO`ドメインは転送セキュリティと暗号を使用する必要はありません。 既定値は `YES` です。
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`) - 場合`NO`(既定値) で、すべてこのドメインとの通信がある必要があります、`HTTPS`プロトコル。
- **NSRequiresCertificateTransparency** (`Boolean`) - 場合`YES`ドメインの Secure Sockets Layer (SSL) が有効な透明度データを含める必要があります。 既定値は `NO` です。
- **NSIncludesSubdomains** (`Boolean`) - 場合`YES`これらの設定は、このドメインのすべてのサブドメインを上書きします。 既定値は `NO` です。
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`)-TLS バージョンが、ドメインが、開発者のコントロールの外部でサード パーティのサービスを使用します。
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`) - 場合`YES`サード パーティのドメインが前方の機密性が必要です。
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`) - If `YES` the ATS will allow non-secure communication with 3rd party domains.

<a name="optout" />

### <a name="opting-out-of-ats"></a>ATS のオプトイン アウト

使用して Apple が強く推奨中に、`HTTPS`プロトコルとインターネットに通信をセキュリティで保護されたベースの情報、これが不可能な常に時間がある可能性があります。 たとえば、サード パーティの web サービスとの通信または広告に配信されたアプリでインターネットを使用している場合。

次の変更、アプリの場合は、Xamarin.iOS アプリでは、安全でないドメインに要求を行う必要があります、 **Info.plist**ファイルは、特定のドメインに対して ATS が適用される既定のセキュリティを無効になります。

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

Visual Studio for Mac での内部をダブルクリック、`Info.plist`ファイル、**ソリューション エクスプ ローラー**に切り替え、**ソース**表示し、上記のキーを追加。

[![](ats-images/ats01.png "Info.plist ファイルのソース ビュー")](ats-images/ats01.png#lightbox)


アプリの読み込みおよび保護されていないサイトから web コンテンツを表示する場合、次のアプリに追加**Info.plist**ファイル読み込みが正しく、残りの部分で Apple Transport Security (ATS) の保護がまだ有効になっている web ページを許可するにはアプリ。

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

必要に応じて、アプリの次の変更を行った**Info.plist**ファイルを完全にすべてのドメインとインターネット通信の ATS を無効にします。

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

Visual Studio for Mac での内部をダブルクリック、`Info.plist`ファイル、**ソリューション エクスプ ローラー**に切り替え、**ソース**表示し、上記のキーを追加。

[![](ats-images/ats02.png "Info.plist ファイルのソース ビュー")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> アプリケーションは、安全でない web サイトへの接続を必要とする場合は**常に**を使用して、例外として、ドメインを入力します。 `NSExceptionDomains` ATS を使用して完全にオフにすることではなく`NSAllowsArbitraryLoads`します。 `NSAllowsArbitraryLoads` 極端な緊急の状況でのみ使用する必要があります。




ここでも、ATS を無効にする必要があります_のみ_セキュリティで保護された接続への切り替えが使用できないか、実用的でない場合、最後の手段として使用します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事でが App Transport Security (ATS) を導入し、インターネットとのセキュリティで保護された通信を強制する方法を説明します。 最初に、iOS 9 で実行されている Xamarin.iOS アプリの ATS が必要です。 変更を説明します。 制御の ATS 機能とオプションを説明します。 最後に、ATS で Xamarin.iOS アプリを無効にするについて説明します。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
