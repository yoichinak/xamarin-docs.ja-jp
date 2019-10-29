---
title: Android 用の HttpClient スタックと SSL/TLS 実装セレクター
description: HttpClient スタックと SSL/TLS 実装セレクターによって、Xamarin Android アプリで使用される HttpClient と SSL/TLS の実装が決まります。
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/20/2018
ms.openlocfilehash: f9f9b112a083615f9cf1d74d7cf81f5ca4f7901f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019235"
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>Android 用の HttpClient スタックと SSL/TLS 実装セレクター

HttpClient スタックと SSL/TLS 実装セレクターによって、Xamarin Android アプリで使用される HttpClient と SSL/TLS の実装が決まります。

プロジェクトは、**システムの .net. Http**アセンブリを参照する必要があります。

> [!WARNING]
> **2018 年4月、** セキュリティ要件が増加しているため (PCI コンプライアンスを含む)、主要クラウドプロバイダーと web サーバーは、1.2 より前の TLS バージョンのサポートを停止することが予想されます。 以前のバージョンの Visual Studio で作成された Xamarin プロジェクトは、既定で古いバージョンの TLS を使用します。
>
> アプリがこれらのサーバーとサービスを引き続き使用できるようにするに**は、次に示す `Android HttpClient` と `Native TLS 1.2` の設定を使用して Xamarin プロジェクトを更新し、アプリを再ビルドしてユーザーに再配置する必要があり**ます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin HttpClient の構成は、 **[プロジェクトオプション > Android オプション]** にあり、 **[詳細オプション]** ボタンをクリックします。

TLS 1.2 サポートに推奨される設定は次のとおりです。

[Visual Studio Android オプションの![](http-stack-images/android-win-sml.png)](http-stack-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Xamarin の HttpClient の構成は、 **[プロジェクトオプション]** で、Android のビルド設定 > ビルドし > **[全般]** タブをクリックします。

TLS 1.2 サポートに推奨される設定は次のとおりです。

[![Visual Studio for Mac Android オプション](http-stack-images/android-mac-sml.png)](http-stack-images/android-mac.png#lightbox)

-----

## <a name="alternative-configuration-options"></a>その他の構成オプション

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler は、マネージコードですべてを実装するのではなく、ネイティブ Java/OS コードにデリゲートする新しいハンドラーです。
**このオプションを選択することをお勧めします。**

#### <a name="pros"></a>プロフェッショナル

- パフォーマンスを向上させ、実行可能ファイルのサイズを小さくするには、ネイティブ API を使用します。
- 最新の標準のサポート (例) TLS 1.2。

#### <a name="cons"></a>マイナス

- Android 4.1 以降が必要です。
- 一部の HttpClient 機能/オプションは使用できません。

### <a name="managed-httpclienthandler"></a>マネージド (HttpClientHandler)

マネージハンドラーは、以前の Xamarin. Android バージョンに付属していた、完全に管理された HttpClient ハンドラーです。

#### <a name="pros"></a>プロフェッショナル

- これは、MS .NET と以前の Xamarin バージョンで最も互換性のある (機能) です。

#### <a name="cons"></a>マイナス

- OS と完全に統合されていません (例: TLS 1.0) に制限されています。
- 通常は非常に遅くなります ( 暗号化) をネイティブ API よりも後に行います。
- より大きなアプリケーションを作成するために、より多くのマネージコードが必要になります。

### <a name="choosing-a-handler"></a>ハンドラーの選択

`AndroidClientHandler` と `HttpClientHandler` のどちらを選択するかは、アプリケーションのニーズによって異なります。 最新のセキュリティサポートには `AndroidClientHandler` が推奨されます。

- TLS 1.2 以降のサポートが必要です。
- アプリは Android 4.1 (API 16) 以降を対象としています。
- `HttpClient`には TLS 1.2 以降のサポートが必要です。
- TLS 1.2 以降の `WebClient`のサポートは必要ありません。

TLS 1.2 以降のサポートが必要であるが、android 4.1 より前のバージョンの Android をサポートする必要がある場合は、`HttpClientHandler` が適しています。 また、`WebClient`に TLS 1.2 以降のサポートが必要な場合にも適しています。

Xamarin Android 8.3 以降では、`HttpClientHandler` 既定では、基になる TLS プロバイダーとして、退屈な SSL (`btls`) が使用されます。 退屈な SSL TLS プロバイダーには、次のような利点があります。

- TLS 1.2 以降がサポートされています。
- すべての Android バージョンがサポートされています。
- TLS 1.2 以降のサポートは、`HttpClient` と `WebClient`の両方でサポートされています。

退屈な SSL を使用していない TLS プロバイダーとして使用する場合の欠点は、結果として得られる APK のサイズを増やすことができることです (サポートされている ABI ごとに追加の APK サイズの約 1 MB を追加します)。

Xamarin Android 8.3 以降では、既定の TLS プロバイダーは、退屈な SSL (`btls`) です。 退屈な SSL を使用しない場合は、`$(AndroidTlsProvider)` プロパティを `legacy` に設定して、管理された SSL 実装の履歴に戻すことができます (ビルドプロパティの設定の詳細については、「[ビルドプロセス](~/android/deploy-test/building-apps/build-process.md)」を参照してください)。

### <a name="programatically-using-androidclienthandler"></a>`AndroidClientHandler` を使用したプログラム

`Xamarin.Android.Net.AndroidClientHandler` は、Xamarin Android 専用の `HttpMessageHandler` の実装です。
このクラスのインスタンスは、すべての HTTP 接続に対してネイティブ `java.net.URLConnection` の実装を使用します。 これにより、理論的に HTTP パフォーマンスが向上し、APK サイズが小さくなります。

次のコードスニペットは、`HttpClient` クラスの1つのインスタンスに対して明示的にを実行する方法の例です。

```csharp
// Android 4.1 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
> 基になる Android デバイスは、TLS 1.2 (ie をサポートしている必要があります。Android 4.1 以降)。 TLS 1.2 の公式のサポートは Android 5.0 以降であることに注意してください。 ただし、一部のデバイスでは、Android 4.1 以降で TLS 1.2 がサポートされています。

## <a name="ssltls-implementation-build-option"></a>SSL/TLS 実装のビルドオプション

このプロジェクトオプションでは、`HttpClient` と `WebRequest`の両方で、すべての web 要求で使用される基になる TLS ライブラリを制御します。 既定では、TLS 1.2 が選択されています。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[Visual Studio での TLS/SSL 実装コンボボックスの![](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[Visual Studio for Mac での TLS/SSL 実装コンボボックスの![](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png#lightbox)

-----

(例:

```csharp
var client = new HttpClient();
```

HttpClient 実装が "**マネージ**" に設定されていて、tls 実装が**ネイティブ tls 1.2 以降**に設定されている場合、`client` オブジェクトは、HTTP に対してマネージ `HttpClientHandler` と tls 1.2 (BoringSSL ライブラリによって提供される) を自動的に使用します。要求.

ただし、 **Httpclient 実装**が `AndroidHttpClient`に設定されている場合、すべての `HttpClient` オブジェクトは基になる Java クラス `java.net.URLConnection` を使用し、 **TLS/SSL 実装**値の影響を受けることはありません。 `WebRequest` オブジェクトは、BoringSSL ライブラリを使用します。

## <a name="other-ways-to-control-ssltls-configuration"></a>SSL/TLS の構成を制御するその他の方法

Xamarin Android アプリケーションで TLS 設定を制御するには、次の3つの方法があります。

1. [プロジェクトオプション] で、HttpClient 実装と既定の TLS ライブラリを選択します。
2. `Xamarin.Android.Net.AndroidClientHandler`を使用したプログラムでの使用。
3. 環境変数を宣言します (省略可能)。

3つの選択肢のうち、推奨される方法は、Xamarin. Android プロジェクトオプションを使用して、アプリ全体の既定の `HttpMessageHandler` と TLS を宣言することです。 次に、必要に応じて、`Xamarin.Android.Net.AndroidClientHandler` オブジェクトをプログラムによってインスタンス化します。 これらのオプションについては、上記で説明します。

3番目のオプション &ndash; 環境変数を使用して &ndash; について説明します。

### <a name="declare-environment-variables"></a>環境変数の宣言

Xamarin の TLS の使用に関連する環境変数には、次の2つがあります。

- `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; この環境変数は、アプリケーションが使用する既定の `HttpMessageHandler` を宣言します。 (例:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

- この環境変数を `XA_TLS_PROVIDER` &ndash;、`btls`、`legacy`、または `default` (この変数を省略した場合と同じ) のいずれかの TLS ライブラリを使用するように宣言します。

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

この環境変数は、_環境ファイル_をプロジェクトに追加することによって設定されます。 環境ファイルは、 **Androidenvironment**のビルドアクションを含む Unix 形式のプレーンテキストファイルです。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio の AndroidEnvironment ビルドアクションのスクリーンショット。](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Visual Studio for Mac の AndroidEnvironment ビルドアクションのスクリーンショット。](http-stack-images/tls03-xs.png)

-----

環境変数と Xamarin Android の詳細については、「 [Xamarin Android 環境](~/android/deploy-test/environment.md)ガイド」を参照してください。

## <a name="related-links"></a>関連リンク

- [トランスポート層セキュリティ (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
