---
title: "HttpClient スタックと Android 用の SSL や TLS 実装セレクター"
description: "HttpClient スタックと SSL/TLS 実装セレクターは、Xamarin.Android アプリで使用する HttpClient および SSL や TLS の実装を決定します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: bcb6f033c7fad76a17a7a5aa82f48a76b1ae501d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>HttpClient スタックと Android 用の SSL や TLS 実装セレクター

_HttpClient スタックと SSL/TLS 実装セレクターは、Xamarin.Android アプリで使用する HttpClient および SSL や TLS の実装を決定します。_

## <a name="overview"></a>概要

Xamarin.Android は、Android アプリ用の TLS の設定を制御する 2 つのコンボ ボックスを提供します。 1 つのコンボ ボックスを識別する`HttpMessageHandler`インスタンス化するときに使用する、`HttpClient`オブジェクト、TLS 実装は web 要求で使用される、他の識別中にします。

> [!NOTE]
> **注:**プロジェクトを参照する必要があります、 **System.Net.Http**アセンブリ。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

HttpClient スタックを設定すると、Xamarin.Android プロジェクトのプロジェクトのオプションがあります。 をクリックして、 **Android オプション** タブし、をクリックして、**詳細オプション**ボタンをクリックします。 これが表示されます、 **Android オプションの高度な**は、次の 2 つのコンボ ボックス、HttpClient の実装および SSL や TLS 実装では、ダイアログ ボックス。


[ ![Android の visual Studio オプション](http-stack-images/tls07-vs-sml.png)](http-stack-images/tls07-vs.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

HttpClient スタックの設定を Xamarin.Android プロジェクトのプロジェクトのオプションではあります。 をクリックして、**ビルド > Android ビルド**設定とをクリックして、**全般** タブ。

[ ![Visual Studio for Mac Android オプション](http-stack-images/tls07-xs-sml.png)](http-stack-images/tls07-xs.png)


-----

## <a name="httpclient-stack-selector"></a>HttpClient スタック セレクター

このプロジェクトのオプションを制御する`HttpMessageHandler`実装が使用するたびに、`HttpClient`オブジェクトをインスタンス化します。 既定では、これは、マネージ`HttpClientHandler`です。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Visual Studio での android HttpClient の実装のコンボ ボックス](http-stack-images/tls04-vs-sml.png)](http-stack-images/tls04-vs.png) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Mac 用の Visual Studio で android HttpClient の実装のコンボ ボックス](http-stack-images/tls04-xs.png )

-----

### <a name="managed-httpclienthandler"></a>マネージ (HttpClientHandler)

マネージ ハンドラーが、完全に管理された HttpClient のハンドラー Xamarin.Android の以前のバージョンから同梱されています。

#### <a name="pros"></a>長所:

- 最も互換性のある (機能) と MS .NET と古いバージョンの Xamarin をお勧めします。

#### <a name="cons"></a>短所:

- いない完全に統合されている OS などです。 制限された TLS 1.0)。
- (通常、非常に遅くなりますが 暗号化) のネイティブ API よりもします。
- マネージ コード、大規模なアプリケーションを作成することが必要です。

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler は、マネージ コードで実装するすべてのものではなく、ネイティブの Java/OS コードにデリゲートされた新しいハンドラーです。

#### <a name="pros"></a>長所:

- ネイティブ API を使用して、パフォーマンスと実行可能ファイルのサイズを小さくします。
- たとえば、最新の標準をサポートします。 TLS 1.2.

#### <a name="cons"></a>短所:

- Android 5.0 以降が必要です。
- 一部の HttpClient 機能/オプションは使用できません。


### <a name="programatically-using-androidclienthandler"></a>プログラムで使用します。 `AndroidClientHandler`

`Xamarin.Android.Net.AndroidClientHandler`は、 `HttpMessageHandler` Xamarin.Android 専用に実装します。 このクラスのインスタンスが、ネイティブを使用して`java.net.URLConnection`すべての HTTP 接続の実装です。 これは、HTTP のパフォーマンスと小さい APK サイズの増加を理論的に提供されます。

このコード スニペットは 1 つのインスタンスを明示的にする方法の例、`HttpClient`クラス。

```csharp
// Android 5.0 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
>  **注**: 基になる、Android デバイスは、TLS 1.2 (ie をサポートする必要があります。Android 5.0 以降)


## <a name="ssltls-implementation-build-option"></a>SSL や TLS 実装ビルド オプション

このプロジェクトのオプションは、すべての web 要求で使用されるどのような基になる TLS ライブラリを制御両方`HttpClient`と`WebRequest`です。 既定では、TLS 1.2 が選択されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Visual Studio での TLS と SSL の実装コンボ ボックス](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Mac 用の Visual Studio での TLS と SSL の実装コンボ ボックス](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png)

-----

例:

```csharp
var client = new HttpClient();
```

HttpClient の実装に設定された場合**マネージ**に設定されている TLS 実装と**ネイティブ TLS 1.2 +**、`client`オブジェクトに自動的に使用、マネージ`HttpClientHandler`とTLS 1.2 (BoringSSL ライブラリによって提供される)、HTTP 要求。

ただし場合、 **HttpClient の実装**に設定されている`AndroidHttpClient`、し、すべて`HttpClient`オブジェクトが基になる Java クラスを使用して`java.net.URLConnection`とするが影響を受ける、 **TLSとSSLの実装**値。 `WebRequest` オブジェクトは、BoringSSL ライブラリを使用します。

## <a name="other-ways-to-control-ssltls-configuration"></a>SSL や TLS の構成を制御する他の方法

Xamarin.Android アプリケーションが TLS の設定を制御できる 3 つの方法があります。

1. プロジェクトのオプションで、HttpClient の実装と既定の TLS ライブラリを選択します。
2. 使用してプログラムで`Xamarin.Android.Net.AndroidClientHandler`です。
3. (省略可能) 環境変数を宣言します。

Xamarin.Android プロジェクトのオプションを使用して、既定値を宣言する方法をお勧めは、3 つの選択肢の`HttpMessageHandler`と tls 暗号化は、アプリ全体を要求します。 次に、必要に応じて、プログラムによってインスタンス化`Xamarin.Android.Net.AndroidClientHandler`オブジェクト。
上記のこれらのオプション。

3 番目のオプション&ndash;環境変数を使用して&ndash;次について説明します。

### <a name="declare-environment-variables"></a>環境変数を宣言します

Xamarin.Android で TLS の使用に関連する 2 つの環境変数です。

-   `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; この環境変数が既定値を宣言して`HttpMessageHandler`にアプリケーションで使用されます。 例:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

-   `XA_TLS_PROVIDER` &ndash; TLS ライブラリを使用するか、この環境変数を宣言します`btls`、 `legacy`、または`default`(これはこの変数を省略することと同じ)。

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

追加することでこの環境変数を設定、_環境ファイル_をプロジェクトにします。 環境ファイルが Unix 形式のテキスト形式のファイルのビルド アクションで**AndroidEnvironment**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Visual Studio で AndroidEnvironment ビルド アクションのスクリーン ショット。](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![スクリーン ショット、AndroidEnvironment のビルド アクションは、Visual Studio で for mac](http-stack-images/tls03-xs.png)

-----

参照してください、 [Xamarin.Android 環境](~/android/deploy-test/environment.md)の詳細については、環境変数および Xamarin.Android をガイドします。


## <a name="related-links"></a>関連リンク

- [トランスポート層セキュリティ (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
