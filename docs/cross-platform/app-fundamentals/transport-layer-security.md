---
title: トランスポート層セキュリティ (TLS) 1.2
description: このドキュメントでは、Xamarin の iOS、Xamarin、Android、および Xamarin. Mac プロジェクトの TLS 1.2 を有効にする方法について説明します。 これは、Visual Studio 2019 と Visual Studio for Mac の両方で実行する方法を示しています。
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: davidortinau
ms.author: daortin
ms.date: 04/20/2018
ms.openlocfilehash: 2beb33915d1dd03184d784ea998f91fcb64f423f
ms.sourcegitcommit: e27e29c14b783263e063baaa65d4eecb8dd31f57
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98628905"
---
# <a name="transport-layer-security-tls-12"></a>トランスポート層セキュリティ (TLS) 1.2

アプリケーションのネットワーク通信をセキュリティで保護するには、最新バージョンの [_トランスポート層セキュリティ_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) を使用することが重要です。

> [!WARNING]
> **2018 年4月、** セキュリティ要件が増加しているため (PCI コンプライアンスを含む)、主要クラウドプロバイダーと web サーバーは、1.2 より前の TLS バージョンのサポートを停止することが予想されます。 以前のバージョンの Visual Studio で作成された Xamarin プロジェクトは、既定で古いバージョンの TLS を使用します。
>
> アプリがこれらのサーバーとサービスを引き続き使用できるようにするに **は、以下の設定を使用するように Xamarin プロジェクトを更新してから、アプリを再構築してユーザーに再デプロイする必要があり** ます。

プロジェクトは、次に示すように、 **システムの .net. Http** アセンブリを参照し、構成する必要があります。

## <a name="update-xamarinandroid-to-tls-12"></a>Xamarin Android を TLS 1.2 に更新する

**Httpclient** の実装オプションと **SSL/tls 実装** オプションを更新して、tls 1.2 のセキュリティを有効にします。

> [!NOTE]
> Android 5.0 以降が必要です。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

これらの設定は、[ **プロジェクトのプロパティ > Android オプション** ] にあり、[ **詳細** 設定] ボタンをクリックします。

[![Visual Studio for Android デバイスで HttpClient と TLS を構成します。](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

これらの設定は、[ **プロジェクトオプション > ビルド > Android ビルド** ] タブにあります。

[![Visual Studio for Mac での HttpClient と TLS の構成](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-xamarinios-to-tls-12"></a>Xamarin. iOS を TLS 1.2 に更新します。

**Httpclient 実装** オプションを更新して、TSL 1.2 のセキュリティを有効にします。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

この設定は、[ **プロジェクトのプロパティ] > [IOS ビルド**] で確認できます。

[![Visual Studio for i O S デバイスで HttpClient と TLS を構成します。](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

この設定は、[ **プロジェクトオプション] > [ビルド > IOS ビルド** ] タブにあります。

[![I O S デバイスの Visual Studio for Mac で HttpClient を構成します。](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-xamarinmac-to-tls-12"></a>Xamarin. Mac を TLS 1.2 に更新します。

Visual Studio for Mac で、Xamarin. Mac アプリで TLS 1.2 を有効にするには、[プロジェクトオプション] の **Httpclient 実装** オプションを更新して **> Mac ビルドをビルド >** ます。

[![Visual Studio for Mac での HttpClient の構成](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

> [!WARNING]
> 今度の Xamarin.Mac 4.8 リリースでは、macOS 10.9 以降のみをサポートします。
> 以前のバージョンの Xamarin.Mac では macOS 10.7 以降をサポートしていましたが、これらの古い macOS バージョンは TLS 1.2 をサポートするための十分な TLS インフラストラクチャがありませんでした。 macOS 10.7 または macOS 10.8 をターゲットにするには、Xamarin.Mac 4.6 以前を使用してください。

## <a name="alternative-configuration-options"></a>その他の構成オプション

このセクションでは、上記の TLS 1.2 でサポートされる構成の代替について説明します。
アプリケーション開発者は、さまざまなレベルの TLS サポートを使用するリスクを理解している場合にのみ、これらの代替手段を検討する必要があります。

### <a name="httpclient-implementation"></a>HttpClient の実装

Xamarin の開発者は、コード内でネイティブネットワーククラスを常に使用できるようになりましたが、クラスで使用されるネットワークスタックを決定するオプションもあり `HttpClient` ます。 これにより、ネイティブプラットフォームの速度とセキュリティ上の利点を備えた使い慣れた .NET API が提供されます。

オプションは次のとおりです。

- **マネージスタック** – Mono が提供するネットワーク機能。
- **ネイティブスタック** –基になるプラットフォーム (Android、iOS、または macOS) によって提供されるさまざまなネットワーク api。

マネージスタックは、既存の .NET コードと最高レベルの互換性を提供しますが、処理速度が低下し、実行可能ファイルのサイズが大きくなる可能性があります。

ネイティブオプションは、より高速で、より高いセキュリティ (TLS 1.2 を含む) を持つことができますが、クラスのすべての機能とオプションが提供されるとは限りません `HttpClient` 。

### <a name="ssltls-implementation-android"></a>SSL/TLS の実装 (Android)

Android プロジェクトのオプションを使用すると、サポートする SSL/TLS 実装を選択することもできます。

- **Mono/Managed** – Android での TLS 1.1
- **ネイティブ** – TLS 1.2 (Android)。

新しい Xamarin プロジェクトでは、TLS 1.2 をサポートするネイティブ実装 (すべてのプロジェクトに推奨) が既定で使用されますが、互換性の理由から必要に応じて、マネージコードに戻すことができます。

> [!IMPORTANT]
> **Mono/Managed** オプションは、 [iOS および Mac のプロジェクトオプションから削除](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_10/xamarin.ios_10.8.md)されました。
>
> ネイティブオプションは、iOS および Mac プラットフォームで常に使用されます。

## <a name="platform-specific-details"></a>プラットフォーム固有の詳細

上記の概要では、Xamarin プロジェクトでの HttpClient と SSL/TLS の実装に関するプロジェクトレベルの設定について説明します。 HttpClient の実装は、コードで動的に設定することもできます。 詳細については、次のプラットフォーム固有のガイドを参照してください。

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS と Mac**](~/cross-platform/macios/http-stack.md)

## <a name="summary"></a>まとめ

アプリケーションでは、可能な限り Transport Layer Security (TLS) 1.2 を使用する必要があります。
この記事の手順に従って既存のアプリケーションの設定を更新し、再構築して顧客に再デプロイする必要があります。

## <a name="related-links"></a>関連リンク

- [アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin サイクル 9 (2017 年2月)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Mono 4.8 リリースノート-TLS 1.2 のサポート](https://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient、HttpClientHandler、および WebRequestHandler について説明します。](/archive/blogs/henrikn/httpclient-httpclienthandler-and-webrequesthandler-explained)
- [システム .Net. HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))
- [システム .Net. HttpClientHandler](/previous-versions/visualstudio/hh138157(v=vs.118))
- [システム .Net. HttpMessageHandler](/previous-versions/visualstudio/hh138091(v=vs.118))
- [HttpWebRequest](/dotnet/api/system.net.httpwebrequest)
- [System.Net.WebClient](/dotnet/api/system.net.webclient)
- [システム .Net. WebRequest](/dotnet/api/system.net.webrequest)
- [java. URLConnection](https://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation. CFNetwork](xref:CoreFoundation.CFNetwork)
- [Foundation. Nn 接続](xref:Foundation.NSUrlConnection)
- [システム .Net. WebRequest](/dotnet/api/system.net.webrequest)
- [HTTP クライアント (サンプル)](/samples/xamarin/ios-samples/httpclient/)