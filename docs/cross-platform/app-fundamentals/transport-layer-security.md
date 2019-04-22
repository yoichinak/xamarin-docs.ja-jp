---
title: トランスポート層セキュリティ (TLS) 1.2
description: このドキュメントについて説明する方法を Xamarin.iOS、Xamarin.Android、Xamarin.Mac プロジェクトの TLS 1.2 を有効にします。 Visual Studio 2019 と Visual Studio for mac。 そう方法を示します
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 26870ae0e84a84a7b78f7766a8e134ecfc7b223e
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58855004"
---
# <a name="transport-layer-security-tls-12"></a>トランスポート層セキュリティ (TLS) 1.2

最新バージョンを使用して[_トランスポート層セキュリティ_(TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security)アプリケーション ネットワークの通信はセキュリティで保護されたことが重要です。

> [!WARNING]
> **2018 年 4 月、** : セキュリティ強化のため主要なクラウド プロバイダーの要件、PCI のコンプライアンスを含むし、web サーバーが TLS バージョン 1.2 より前のサポートを停止する必要があります。  以前のバージョンの TLS を使用する Visual Studio の既定値の以前のバージョンで作成した Xamarin プロジェクト。
>
> アプリは引き続きこれらのサーバーとサービスを使用することを確認するには**Xamarin プロジェクトは、次の設定を使用して、再構築し、アプリを再デプロイを更新する必要があります**をユーザーにします。

プロジェクトを参照する必要があります、 **System.Net.Http**アセンブリと、次に示すように構成します。

## <a name="update-xamarinandroid-to-tls-12"></a>Xamarin.Android を TLS 1.2 に更新します。

更新プログラム、 **HttpClient 実装**と**SSL/TLS 実装**TLS 1.2 セキュリティを有効にするオプション。

> [!NOTE]
> Android 5.0 以降が必要です。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

これらの設定が記載されて**プロジェクトのプロパティ > Android オプション**をクリックし、 **[詳細設定]** ボタン。

[![Visual Studio での HttpClient と TLS を構成します。](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

これらの設定が記載されて**プロジェクト オプション > ビルド > Android のビルド** タブ。

[![Visual studio for Mac HttpClient と TLS を構成します。](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-xamarinios-to-tls-12"></a>Xamarin.iOS を TLS 1.2 に更新します。

更新プログラム、 **HttpClient 実装**TSL 1.2 セキュリティを有効にするオプション。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

この設定が記載されて**プロジェクトのプロパティ > iOS ビルド**:

[![Visual Studio での HttpClient と TLS を構成します。](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

この設定が記載されて**プロジェクト オプション > ビルド > iOS ビルド** タブ。

[![Visual studio for Mac の HttpClient を構成します。](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-xamarinmac-to-tls-12"></a>Xamarin.Mac を TLS 1.2 に更新します。

Visual Studio for Mac で Xamarin.Mac アプリで TLS 1.2 を有効にする更新、 **HttpClient 実装**オプション**プロジェクト オプション > ビルド > Mac ビルド**:

[![Visual studio for Mac の HttpClient を構成します。](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

> [!WARNING]
> 今度の Xamarin.Mac 4.8 リリースでは、macOS 10.9 以降のみをサポートします。
> 以前のバージョンの Xamarin.Mac では macOS 10.7 以降をサポートしていましたが、これらの古い macOS バージョンは TLS 1.2 をサポートするための十分な TLS インフラストラクチャがありませんでした。 macOS 10.7 または macOS 10.8 をターゲットにするには、Xamarin.Mac 4.6 以前を使用してください。

## <a name="alternative-configuration-options"></a>代替の構成オプション

このセクションでは、上記の TLS 1.2 でサポートされている構成に代わる方法について説明します。
さまざまなレベルの TLS サポートを使用してリスクを理解している場合、アプリケーション開発者はこれらの選択肢を検討のみ必要があります。

### <a name="httpclient-implementation"></a>HttpClient 実装

Xamarin の開発者は、コードでネイティブのネットワークのクラスを使用することが常にされているでどのネットワーク スタックを指定するオプションを使用することもありますが、`HttpClient`クラス。 これは、ネイティブ プラットフォームの速度とセキュリティの利点のある使い慣れた .NET API を提供します。

次のオプションがあります。

- **マネージ スタック**– Mono で提供されるネットワーク機能、または
- **ネイティブ スタック**– さまざまなネットワーク Api (Android、iOS、または macOS) は、基になるプラットフォームによって提供されます。

マネージ スタックは、低下し、実行可能ファイルのサイズを大きくすると、ただし、最高レベルの既存の .NET コードとの互換性を提供します。

ネイティブのオプションは、高速化できます (TLS 1.2 を含む)、セキュリティの強化が、すべての機能とオプションのことはできません、`HttpClient`クラス。

### <a name="ssltls-implementation-android"></a>SSL/TLS 実装 (Android)

Android プロジェクトのオプションでは、サポートするために SSL や TLS 実装を選択することもできます。

- **Mono/管理対象**– Android では、TLS 1.1
- **ネイティブ**– Android で TLS 1.2 です。

新しい Xamarin プロジェクトの既定値は (この操作は、すべてのプロジェクトを推奨) TLS 1.2 をサポートするネイティブ実装、互換性のために必要なただし切り替えることができます、マネージ コードに戻す場合。

> [!IMPORTANT]
> **Mono/マネージ**オプション[iOS と Mac から削除](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/)プロジェクト オプション。
>
> IOS および Mac プラットフォームでネイティブのオプションが常に使用します。

## <a name="platform-specific-details"></a>プラットフォーム固有の詳細

上記の概要では、Xamarin プロジェクトでは、HttpClient と SSL/TLS の実装プロジェクト レベルの設定について説明します。 HttpClient 実装は、コードで動的に設定することもできます。 これらの詳細についてはプラットフォーム固有のガイドを参照してください。

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS および Mac**](~/cross-platform/macios/http-stack.md)

## <a name="summary"></a>まとめ

アプリケーションは、可能な限り、トランスポート層セキュリティ (TLS) 1.2 を使用する必要があります。
この記事では、」の説明に従って既存のアプリケーションの設定を更新し、、再構築して、顧客に再デプロイします。

## <a name="related-links"></a>関連リンク

- [アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin Cycle 9 (2017 年 2 月)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Mono 4.8 リリース ノートには、TLS 1.2 をサポート](https://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient、HttpClientHandler、および WebRequestHandler の説明](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](https://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](xref:CoreFoundation.CFNetwork)
- [Foundation.NSUrlConnection](xref:Foundation.NSUrlConnection)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP クライアント (サンプル)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
