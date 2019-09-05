---
title: Xamarin. iOS のアプリトランスポートセキュリティ
description: アプリトランスポートセキュリティ (ATS) は、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続を適用します。
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/13/2017
ms.openlocfilehash: dc435f486d0020ab339ebd8f537f749f44493fe0
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289494"
---
# <a name="app-transport-security-in-xamarinios"></a>Xamarin. iOS のアプリトランスポートセキュリティ

_アプリトランスポートセキュリティ (ATS) は、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続を適用します。_

この記事では、iOS 9 アプリでアプリトランスポートセキュリティによって適用されるセキュリティの変更について説明します。また、Xamarin のプロジェクトでは、この方法について説明します。また、 [ats の構成オプション](#config)について説明し、 [ats のオプトアウト](#optout)方法についても説明し[ます。](#xamarinsupport)ATS (必要な場合)。 この記事はATS構成オプションと必要な場合にATSからオプトアウトする方法を説明します。ATSは既定で有効となっているため、iOS 9アプリでは(明示的に許可しない限り)あらゆるセキュアでないインターネット接続によって例外が発生します。


## <a name="about-app-transport-security"></a>アプリトランスポートセキュリティについて

前述のように、ATS は、iOS 9 と OS X El Capitan のすべてのインターネット通信が安全な接続のベストプラクティスに準拠していることを保証します。これにより、アプリまたはライブラリを通じて直接機密情報を漏洩することを防ぐことができます。多大.

既存のアプリの場合は`HTTPS` 、可能な限りプロトコルを実装します。 新しい Xamarin iOS アプリでは、インターネットリソースと`HTTPS`通信するときにのみを使用する必要があります。 さらに、高レベルの API 通信は、転送機密性を持つ TLS バージョン1.2 を使用して暗号化する必要があります。

[NCapitan lconnection](xref:Foundation.NSUrlConnection)、 [cfurl](xref:CoreFoundation.CFUrl) 、または[nの lsession](xref:Foundation.NSUrlSession)を使用して確立された接続は、iOS 9 および OS X 10.11 (El) 用に構築されたアプリでは既定で ATS を使用します。

## <a name="default-ats-behavior"></a>既定の ATS 動作

IOS 9 と OS X 10.11 (El Capitan) 用に構築されたアプリでは、ATS が既定で有効になっているため、 [n Lconnection](xref:Foundation.NSUrlConnection)、 [cfurl](xref:CoreFoundation.CFUrl) 、または[nを](xref:Foundation.NSUrlSession)使用した接続はすべて、ats セキュリティ要件の対象となります。 接続がこれらの要件を満たしていない場合、例外が発生して失敗します。

### <a name="ats-connection-requirements"></a>ATS 接続の要件

ATS は、すべてのインターネット接続に対して次の要件を適用します。

- すべての接続暗号では、転送機密性を使用する必要があります。 受け入れられた暗号の一覧については、以下を参照してください。
- トランスポート層セキュリティ (TLS) プロトコルは、バージョン1.2 以上である必要があります。
- 少なくとも2048ビットまたはそれ以上の RSA キーを含む SHA256 フィンガープリント、またはすべての証明書に256ビットまたはそれ以上の楕円曲線 (ECC) キーを使用する必要があります。

ここでも、ATS は iOS 9 で既定で有効になっているため、これらの要件を満たしていない接続を確立しようとすると、例外がスローされます。

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>ATS 互換の暗号

次の転送機密性暗号の種類は、ATS によってセキュリティで保護されたインターネット通信で受け入れられます。

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

IOS インターネット通信クラスの使用の詳細については、Apple の[Nn 接続クラスリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755)、 [cfurl 参照](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206)、または[Nの lsession クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435)を参照してください。

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Xamarin での ATS のサポート

IOS 9 と OS X El Capitan では、ATS が既定で有効になっているため、使用している Xamarin アプリまたは任意のライブラリまたはサービスがインターネットに接続している場合は、何らかの操作を行う必要があります。そうしないと、接続により例外がスローされます。

既存のアプリの場合、Apple はできるだけ早く`HTTPS`プロトコルをサポートすることを提案します。 をサポート`HTTPS`していないサードパーティの web サービスに接続しているためにができ`HTTPS`ない場合、またはサポートが実用的でない場合は、ATS をオプトアウトすることができます。 詳細については、以下の「 [ATS のオプトアウト](#optout)」セクションを参照してください。

新しい Xamarin iOS アプリの場合は、インターネットリソースと`HTTPS`通信するときにのみを使用する必要があります。 ここでも、(サードパーティの web サービスを使用するなど) 状況が考えられますが、これは不可能であり、ATS をオプトアウトする必要があります。

さらに、ATS は、上位レベルの API 通信を、転送機密性を持つ TLS バージョン1.2 を使用して暗号化します。 詳細については、上記の「 [Ats 接続の要件](#ats-connection-requirements)」と「 [ats と互換性のある暗号](#ats-compatible-ciphers)」を参照してください。

TLS ([トランスポート層セキュリティ](https://en.wikipedia.org/wiki/Transport_Layer_Security)) には慣れていないかもしれませんが、SSL ([Secure Socket layer](https://en.wikipedia.org/wiki/Transport_Layer_Security)) の後継であり、ネットワーク接続に対してセキュリティを適用するための暗号プロトコルのコレクションを提供しています。

TLS レベルは、使用している web サービスによって制御されているため、アプリの制御外にあります。 `HttpClient`との`ModernHttpClient`両方で、サーバーでサポートされている最高レベルの TLS 暗号化を自動的に使用する必要があります。

通信先のサーバー (特にサードパーティのサービスの場合) によっては、転送の機密性を無効にしたり、より低い TLS レベルを選択したりする必要があります。 詳細については、以下の「 [ATS オプションの構成](#configuring-ats-options)」セクションを参照してください。

> [!IMPORTANT]
> アプリトランスポートセキュリティは、**マネージ HTTPClient 実装**を使用した Xamarin アプリには適用されません。 これは、CFNetwork **Httpclient の実装**を使用した接続、または**Nn1 Lsession httpclient の実装**のみに適用されます。

### <a name="setting-the-httpclient-implementation"></a>HTTPClient 実装の設定

IOS アプリで使用される HTTPClient 実装を設定するには、**ソリューションエクスプローラー**で**プロジェクト**をダブルクリックして、**プロジェクトオプション**を開きます。 **[IOS ビルド]** に移動し、 **[httpclient 実装]** ドロップダウンで目的のクライアントの種類を選択します。

![](ats-images/client01.png "IOS のビルドオプションの設定")


#### <a name="managed-handler"></a>マネージハンドラー

マネージハンドラーは、以前のバージョンの Xamarin. iOS に付属していた、完全に管理された HttpClient ハンドラーであり、既定のハンドラーです。

プロフェッショナル

- これは、Microsoft .NET と以前のバージョンの Xamarin と互換性があります。

マイナス

- IOS と完全に統合されていません (たとえば、TLS 1.0 に制限されています)。
- 通常、ネイティブ Api よりもはるかに低速です。
- これは、より多くのマネージコードを必要とし、より大規模なアプリを作成します。

#### <a name="cfnetwork-handler"></a>CFNetwork ハンドラー

Cfnetwork ベースのハンドラーは、ネイティブ`CFNetwork`フレームワークに基づいています。

プロフェッショナル

- は、パフォーマンスを向上させ、実行可能ファイルのサイズを小さくするためにネイティブ API を使用します。
- では、TLS 1.2 などの新しい標準のサポートが追加されています。

マイナス

- IOS 6 以降が必要です。
- WatchOS では使用できません。
- HttpClient の一部の機能およびオプションは使用できません。

#### <a name="nsurlsession-handler"></a>Nの Lsession ハンドラー

Nの lsession ベースのハンドラーは、ネイティブ`NSUrlSession` API に基づいています。

プロフェッショナル

- は、パフォーマンスを向上させ、実行可能ファイルのサイズを小さくするためにネイティブ API を使用します。
- では、TLS 1.2 などの新しい標準のサポートが追加されています。

マイナス

- IOS 7 以降が必要です。
- HttpClient の一部の機能およびオプションは使用できません。

## <a name="diagnosing-ats-issues"></a>ATS の問題の診断

直接、または iOS 9 の web ビューからインターネットに接続しようとすると、次の形式でエラーが表示されることがあります。

> アプリトランスポートセキュリティは、安全ではない http://www.-the-blocked-domain.com) ため、クリアテキスト HTTP (リソースの読み込みをブロックしました。 一時的な例外は、アプリの情報の plist ファイルを使用して構成できます。

IOS9 では、アプリトランスポートセキュリティ (ATS) は、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続を適用します。 さらに、ATS では、 `HTTPS`プロトコルを使用した通信と高レベルの API 通信が TLS バージョン1.2 を使用して転送される必要があります。

IOS 9 および OS X 10.11 (El Capitan) 用に構築されたアプリでは、ats が既定で`NSURLConnection`有効`CFURL`に`NSURLSession`なっているため、またはを使用するすべての接続は、ats のセキュリティ要件の対象となります。 接続がこれらの要件を満たしていない場合、例外が発生して失敗します。

また、Apple は、 [TLSTool サンプルアプリ](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2)を提供しています。このアプリはC#、コンパイル (または、必要に応じて Xamarin とにトランスコードされます) し、ATS/TLS の問題の診断に使用できます。 この問題を解決する方法については、以下の「 [ATS のオプトアウト](#optout)」セクションを参照してください。


<a name="config" />

## <a name="configuring-ats-options"></a>ATS オプションの構成

アプリの**情報の plist**ファイルで特定のキーの値を設定することによって、ATS の機能のいくつかを構成できます。 次のキーを使用して、ATS を制御できます (_入れ子になっていることを示すためにインデント_されています)。

```
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

各キーの種類と意味は次のとおりです。

- **Nsapptransportsecurity**(`Dictionary`)-ATS のすべての設定キーと値が含まれています。
- **NSAllowsArbitraryLoads**(`Boolean`)-に`YES` 記載`NSExceptionDomains`されて**いない**ドメインに対して、ATS を無効にする場合。 リストされているドメインの場合は、指定されたセキュリティ設定が使用されます。
- **NSAllowsArbitraryLoadsInWebContent**(`Boolean`)-Apple `YES` Transport Security (ATS) の保護がアプリの残りの部分で有効になっている間、web ページが正しく読み込まれるようにする場合は。
- **Nsexceptiondomains**(`Dictionary`)-特定のドメインに対して、ATS が使用する必要があるセキュリティ設定とドメインのコレクション。
- `Dictionary` **ドメイン名-例外としての>()-特定のドメインの例外\<** のコレクション (例: `www.xamarin.com`)
- **NSExceptionMinimumTLSVersion**(`String`)-最小 TLS バージョン`TLSv1.0` `TLSv1.1` 。または`TLSv1.2` (既定値) です。
- **NSExceptionRequiresForwardSecrecy**(`Boolean`)-ドメイン`NO`が転送セキュリティ付きの暗号を使用する必要がない場合。 既定値は `YES` です。
- **Nsexceptionallowsinsecurehttの**発行(`Boolean`)-( `NO`既定値) の場合、この`HTTPS`ドメインとのすべての通信がプロトコルに含まれている必要があります。
- **NSRequiresCertificateTransparency**(`Boolean`)-ドメイン`YES`の Secure Sockets Layer (SSL) に有効な透過性データを含める必要がある場合。 既定値は `NO` です。
- **Nsa サブドメイン**(`Boolean`)-これら`YES`の設定は、このドメインのすべてのサブドメインを上書きします。 既定値は `NO` です。
- **NSThirdPartyExceptionMinimumTLSVersion**(`String`)-ドメインが開発者の管理外にあるサードパーティのサービスである場合に使用される TLS のバージョン。
- **NSThirdPartyExceptionRequiresForwardSecrecy**(`Boolean`)-サード`YES`パーティのドメインが上位の機密性を必要とする場合。
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`) - If `YES` the ATS will allow non-secure communication with 3rd party domains.

<a name="optout" />

### <a name="opting-out-of-ats"></a>ATS のオプトアウト

Apple は`HTTPS`プロトコルを使用してインターネットベースの情報へのセキュリティで保護された通信を推奨していますが、これが常に可能であるとは限りません。 たとえば、サードパーティの web サービスと通信している場合や、インターネットで配信された広告をアプリで使用している場合などです。

Xamarin iOS アプリがセキュリティで保護されていないドメインに対して要求を行う必要がある場合、アプリの**情報の plist**ファイルに次の変更を加えると、特定のドメインに対して ATS が適用するセキュリティの既定値が無効になります。

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

Visual Studio for Mac 内で、**ソリューションエクスプローラー**内の`Info.plist`ファイルをダブルクリックし、**ソース**ビューに切り替えて、上記のキーを追加します。

[![](ats-images/ats01.png "情報 plist ファイルのソースビュー")](ats-images/ats01.png#lightbox)


アプリでセキュリティで保護されていないサイトの web コンテンツを読み込んで表示する必要がある場合は、アプリの**情報 plist**ファイルに次の内容を追加して、web ページが正しく読み込まれるようにします。ただし、Apple Transport SECURITY (ATS) 保護はアプリの残りの部分でも有効になっています。

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

必要に応じて、すべてのドメインとインターネット通信の ATS を完全に無効にするために、アプリの**情報の plist**ファイルに次の変更を加えることができます。

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

Visual Studio for Mac 内で、**ソリューションエクスプローラー**内の`Info.plist`ファイルをダブルクリックし、**ソース**ビューに切り替えて、上記のキーを追加します。

[![](ats-images/ats02.png "情報 plist ファイルのソースビュー")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> アプリケーションがセキュリティで保護されていない web サイトへの接続を必要とする場合は`NSExceptionDomains` 、を`NSAllowsArbitraryLoads`使用して完全にオフにするのではなく、**常に**を使用してドメインを例外として入力してください。 `NSAllowsArbitraryLoads` 極端な緊急の状況でのみ使用する必要があります。




ここでも、セキュリティで保護された接続への切り替えが利用できないか、または実用的でない場合は、最後の手段として_のみ_、ATS を無効にしてください。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、アプリトランスポートセキュリティ (ATS) を導入し、インターネットとのセキュリティで保護された通信を実施する方法について説明しました。 まず、iOS 9 で実行されている Xamarin の iOS アプリに対して、ATS に必要な変更について説明します。 次に、ATS の機能とオプションの制御について説明します。 最後に、Xamarin. iOS アプリの使用を停止します。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
