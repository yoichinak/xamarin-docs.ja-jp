---
title: "トランスポート層セキュリティ (TLS)"
description: "Android、iOS、Mac で Xamarin プロジェクトの TLS 1.2 の有効化"
ms.topic: article
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/10/2017
ms.openlocfilehash: b0f205c5ab2c65f0e2a99f912f3961f12a4f2b7a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="transport-layer-security-tls"></a>トランスポート層セキュリティ (TLS)

_Android、iOS、Mac で Xamarin プロジェクトの TLS 1.2 の有効化_

最新バージョンを使用して[_トランスポート層セキュリティ_(TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security)アプリケーションでネットワーク通信がセキュリティで保護されたことを確認することが重要です。

> [!NOTE]
> Xamarin のリリース以降[2017 年 2 月](https://releases.xamarin.com/stable-release-cycle-9/)既定では、新しいプロジェクトで TLS 1.2 を使用します。

TLS 1.2 サポートで使用できるようになりました。

* モノラル 4.8 (が含まれています[TLS 1.2 サポート](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support))
* Xamarin.iOS
* Xamarin.Mac
* Xamarin.Android (Android 5.0 以降が必要)

プロジェクトを参照する必要があります、 **System.Net.Http**アセンブリ。 

## <a name="updating-to-tls-12"></a>TLS 1.2 に更新

このセクションの内容について説明します、Xamarin のプロジェクトでのネットワークの構成オプションの一部を更新できるように、_既存_より安全なプロトコルを利用するアプリです。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

これらの設定は含まれて**プロジェクトのオプション > Android オプション**をクリックし、 **[詳細設定]**ボタン。 

[![Visual Studio での HttpClient および TLS を構成します。](transport-layer-security-images/properties-vs-sml.png)](transport-layer-security-images/properties-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
これらの設定は含まれて**プロジェクト プロパティ > ビルド オプション > 詳細設定**  タブ。

[![Mac 用 Xamarin Studio および Visual Studio で HttpClient と TLS を構成します。](transport-layer-security-images/properties-xs-sml.png)](transport-layer-security-images/properties-xs.png#lightbox)

-----


### <a name="httpclient-implementation"></a>HttpClient の実装

Xamarin の開発者は、コードでネイティブのネットワークのクラスを使用することが常にされている、ただしどのネットワーク スタックを決定するオプションを使っても、`HttpClient`クラスです。 これには、ネイティブ プラットフォームの速度とセキュリティの利点のある使い慣れた .NET API が用意されています。

次のオプションがあります。

- **マネージ スタック**– モノラル標準のネットワーク機能、または
- **ネイティブ スタック**– さまざまなネットワーク Api (Android、iOS、または macOS) は、基になるプラットフォームによって提供されます。

マネージ スタックは、低速して実行可能ファイルのサイズを大きくすると、ただし、最高レベルの既存の .NET コードとの互換性を提供します。

ネイティブのオプションは処理が速くなることができます (TLS 1.2 を含む)、セキュリティが向上し、すべての機能とオプションの可能性があります提供しない場合が、`HttpClient`クラスです。


### <a name="ssltls-implementation"></a>SSL や TLS の実装

プロジェクトのオプションをサポートするためにどの SSL/TLS 実装を選択することもできます。

- **モノラル/マネージ**– Android で TLS 1.1、TLS 1.0 iOS および macOS でします。
- **ネイティブ**– Android、iOS、および macOS の両方で TLS 1.2 です。

新しい Xamarin プロジェクトの既定 (これは、すべてのプロジェクトの推奨) TLS 1.2 をサポートするネイティブの実装に、互換性のため必要ただしを切り替えることができます、マネージ コードに戻る場合です。

> [!IMPORTANT]
> **モノラル/マネージ**オプションで削除される予定、[将来のリリース](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/)です。
>
> ネイティブのオプションをお勧めします。

## <a name="platform-specific-details"></a>プラットフォーム固有の詳細

上記の概要では、Xamarin のプロジェクトで HttpClient および SSL や TLS の実装のプロジェクト レベルの設定について説明します。 HttpClient の実装がコードでは、動的に設定することもでき、iOS では、次の 2 つのネイティブ オプションを選択します。

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS および Mac**](~/cross-platform/macios/http-stack.md)


## <a name="summary"></a>まとめ

アプリケーションは、可能な限り、トランスポート層セキュリティ (TLS) 1.2 を使用する必要があります。
この記事の説明に従って既存のアプリケーションの設定を更新する必要がありますが、この構成に新しいアプリが既定でします。

## <a name="related-links"></a>関連リンク

- [アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin サイクル 9 (2017 年 2 月)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [モノラル 4.8 リリース ノートには、TLS 1.2 をサポート](http://www.mono-project.com/docs/about-monohttps://developer.xamarin.com/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient、HttpClientHandler、および WebRequestHandler の説明](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/en-us/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP クライアント (サンプル)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
