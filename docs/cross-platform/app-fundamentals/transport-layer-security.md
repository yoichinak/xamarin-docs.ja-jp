---
title: トランスポート層セキュリティ (TLS) 1.2
description: Android、iOS、Mac で Xamarin プロジェクトの TLS 1.2 の有効化
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 8e27801a9feb8cf7ba1534f88479dbf7259c3e85
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="transport-layer-security-tls-12"></a>トランスポート層セキュリティ (TLS) 1.2

最新バージョンを使用して[_トランスポート層セキュリティ_(TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security)アプリケーションでネットワーク通信がセキュリティで保護されたことを確認することが重要です。

> [!WARNING]
> **年 4 月、2018年**– セキュリティの向上のため要件、PCI コンプライアンスを含むメジャー クラウド プロバイダーおよび TLS バージョン 1.2 より前のサポートを停止する web サーバーが必要です。  Xamarin プロジェクトが以前のバージョンの Visual Studio の既定値は、古いバージョンの TLS を使用して作成します。
>
> アプリがこれらのサーバーと、サービスの操作を続行することを確認するために**、Xamarin のプロジェクトに、次の設定を使用して、再構築し、アプリを再展開を更新する必要があります**をユーザーにします。

プロジェクトを参照する必要があります、 **System.Net.Http**アセンブリ次に示すように構成するとします。

## <a name="update-android-to-tls-12"></a>TLS 1.2 に更新プログラムの Android

更新プログラム、 **HttpClient の実装**と**SSL/TLS 実装**TLS 1.2 セキュリティを有効にするオプションです。

> [!NOTE]
> Android 5.0 以降が必要です。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

これらの設定は含まれて**プロジェクトのプロパティ > Android オプション**をクリックし、**詳細**ボタン。

[![Visual Studio での HttpClient および TLS を構成します。](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

これらの設定は含まれて**プロジェクトのオプション > ビルド > Android ビルド** タブ。

[![Mac 用 Visual Studio で HttpClient と TLS を構成します。](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-ios-to-tls-12"></a>TLS 1.2 に更新プログラムの iOS

更新プログラム、 **HttpClient の実装**TSL 1.2 セキュリティを有効にするオプションです。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

この設定は含まれて**プロジェクト プロパティ > iOS ビルド**:

[![Visual Studio での HttpClient および TLS を構成します。](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

この設定は含まれて**プロジェクトのオプション > ビルド > iOS ビルド** タブ。

[![Mac 用の Visual Studio で HttpClient を構成します。](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-macos-to-tls-12-in-visual-studio-for-mac"></a>Mac 用の Visual Studio での TLS 1.2 に更新 macOS

更新プログラム、 **HttpClient の実装**オプション**プロジェクトのオプション > ビルド > Mac をビルド**TSL 1.2 セキュリティを有効にする タブ。

[![Mac 用の Visual Studio で HttpClient を構成します。](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

## <a name="alternative-configuration-options"></a>代替の構成オプション

このセクションでは、前に示した TLS 1.2 でサポートされている構成に代わる方法について説明します。
TLS サポートのさまざまなレベルの使用に伴うリスクを理解している場合、アプリケーション開発者はこれらの代替方法を検討のみ必要があります。

### <a name="httpclient-implementation"></a>HttpClient の実装

Xamarin の開発者は、コードでネイティブのネットワークのクラスを使用することが常にされている、ただしどのネットワーク スタックを決定するオプションを使っても、`HttpClient`クラスです。 これには、ネイティブ プラットフォームの速度とセキュリティの利点のある使い慣れた .NET API が用意されています。

次のオプションがあります。

- **マネージ スタック**– モノラル標準のネットワーク機能、または
- **ネイティブ スタック**– さまざまなネットワーク Api (Android、iOS、または macOS) は、基になるプラットフォームによって提供されます。

マネージ スタックは、低速して実行可能ファイルのサイズを大きくすると、ただし、最高レベルの既存の .NET コードとの互換性を提供します。

ネイティブのオプションは処理が速くなることができます (TLS 1.2 を含む)、セキュリティが向上し、すべての機能とオプションの可能性があります提供しない場合が、`HttpClient`クラスです。

### <a name="ssltls-implementation-android"></a>SSL や TLS 実装 (Android)

Android プロジェクトのオプションをサポートするためにどの SSL/TLS 実装を選択することもできます。

- **モノラル/管理対象**– Android で TLS 1.1
- **ネイティブ**– Android での TLS 1.2 です。

新しい Xamarin プロジェクトの既定 (これは、すべてのプロジェクトの推奨) TLS 1.2 をサポートするネイティブの実装に、互換性のため必要ただしを切り替えることができます、マネージ コードに戻る場合です。

> [!IMPORTANT]
> **モノラル/マネージ**オプション[から iOS と Mac 削除](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/)プロジェクト オプション。
>
> IOS および Mac プラットフォームでネイティブのオプションが常に使用します。

## <a name="platform-specific-details"></a>プラットフォーム固有の詳細

上記の概要では、Xamarin のプロジェクトで HttpClient および SSL や TLS の実装のプロジェクト レベルの設定について説明します。 HttpClient の実装は、コードで動的に設定することもできます。 詳細についてはこれらのプラットフォームに固有のガイドを参照してください。

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS および Mac**](~/cross-platform/macios/http-stack.md)


## <a name="summary"></a>まとめ

アプリケーションは、可能な限り、トランスポート層セキュリティ (TLS) 1.2 を使用する必要があります。
この記事の説明に従って既存のアプリケーションの設定を更新し、再構築して、顧客に再配置します。

## <a name="related-links"></a>関連リンク

- [アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin サイクル 9 (2017 年 2 月)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [モノラル 4.8 リリース ノートには、TLS 1.2 をサポート](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient、HttpClientHandler、および WebRequestHandler の説明](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP クライアント (サンプル)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
