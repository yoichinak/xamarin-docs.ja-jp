---
title: HttpClient スタックと SSL/TLS の実装セレクター for Android
description: HttpClient スタックと SSL/TLS の実装セレクターは、Xamarin.Android アプリで使用する HttpClient と SSL/TLS の実装を決定します。
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/20/2018
ms.openlocfilehash: a3704552c8fc147588919ecdde2813e831237d89
ms.sourcegitcommit: cc750b0d8086ed14f84cd8eb9a06f45c719b3cf4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59239902"
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>HttpClient スタックと SSL/TLS の実装セレクター for Android

HttpClient スタックと SSL/TLS の実装セレクターは、Xamarin.Android アプリで使用する HttpClient と SSL/TLS の実装を決定します。

プロジェクトを参照する必要があります、 **System.Net.Http**アセンブリ。

> [!WARNING]
> **2018 年 4 月、** : セキュリティ強化のため主要なクラウド プロバイダーの要件、PCI のコンプライアンスを含むし、web サーバーが TLS バージョン 1.2 より前のサポートを停止する必要があります。  以前のバージョンの TLS を使用する Visual Studio の既定値の以前のバージョンで作成した Xamarin プロジェクト。
>
> アプリは引き続きこれらのサーバーとサービスを使用することを確認するには**で Xamarin プロジェクトを更新する必要があります、`Android HttpClient`と`Native TLS 1.2`、次に示す設定を再構築し、アプリを再デプロイ**に、ユーザー。

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

Xamarin.Android HttpClient 構成が**プロジェクト オプション > Android オプション**、 をクリックし、**詳細オプション**ボタン。

TLS 1.2 のサポートの推奨設定を次に示します。

[![Visual Studio Android オプション](http-stack-images/android-win-sml.png)](http-stack-images/android-win.png#lightbox)


# [<a name="visual-studio-for-mac"></a>Visual Studio for Mac](#tab/macos)

Xamarin.Android HttpClient 構成が**プロジェクト オプション > ビルド > Android のビルド**設定とをクリックして、**全般** タブ。

TLS 1.2 のサポートの推奨設定を次に示します。

[![Visual Studio for Mac の [Android オプション](http-stack-images/android-mac-sml.png)](http-stack-images/android-mac.png#lightbox)

-----

## <a name="alternative-configuration-options"></a>代替の構成オプション

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler は、マネージ コードで実装するすべてのものではなく、ネイティブの Java/OS コードにデリゲートする新しいハンドラーです。
**これが推奨されるオプションです。**

#### <a name="pros"></a>プロフェッショナル

- ネイティブ API を使用して、パフォーマンスと実行可能ファイルのサイズを小さくします。
- 次のような最新の標準をサポートします。 TLS 1.2.

#### <a name="cons"></a>短所

- Android 4.1 以降が必要です。
- 一部の HttpClient 機能/オプションは使用できません。

### <a name="managed-httpclienthandler"></a>マネージド (HttpClientHandler)

マネージ ハンドラーは、Xamarin.Android の以前のバージョンから同梱されている完全管理型の HttpClient ハンドラーです。

#### <a name="pros"></a>プロフェッショナル

- 最も互換性のある (機能) と MS .NET および Xamarin の古いバージョンをお勧めします。

#### <a name="cons"></a>短所

- いない完全に統合されて、OS (例。 制限あり TLS 1.0)。
- 通常はかなり遅くなります (例。 暗号化) のネイティブ API よりもします。
- マネージ コード、大規模なアプリケーションを作成することが必要です。



### <a name="choosing-a-handler"></a>ハンドラーを選択します。

間で choice`AndroidClientHandler`と`HttpClientHandler`アプリケーションのニーズによって異なります。 `AndroidClientHandler` お勧め、最新のセキュリティ サポートでは、次のような。

-   TLS 1.2 + サポートが必要です。
-   アプリが Android 4.1 (API 16) を対象とするまたはそれ以降。
-   必要な TLS 1.2 + サポート`HttpClient`します。
-   不要な TLS 1.2 + サポート`WebClient`します。

`HttpClientHandler` TLS 1.2 + 必要がある場合、適切な選択は、サポートしますが、Android 4.1 より前のバージョンの Android をサポートする必要があります。 これもをお勧め TLS 1.2 + が必要な場合のサポート`WebClient`します。

以降では、Xamarin.Android 8.3、`HttpClientHandler`退屈 SSL の既定値 (`btls`) 基になる TLS プロバイダーとして。 SSL/TLS を退屈プロバイダーには次の利点があります。

-   TLS 1.2 + をサポートします。
-   すべての Android バージョンをサポートします。
-   TLS 1.2 + が両方のサポートを提供します`HttpClient`と`WebClient`します。

退屈 SSL を使用して、元になる TLS プロバイダーとしての欠点は、(サポートされる ABI ごとに追加の APK サイズの約 1 MB を追加)、結果として得られる APK のサイズを増やすことできます。

Xamarin.Android 8.3 以降、既定の TLS プロバイダーは退屈 SSL (`btls`)。 設定して、履歴マネージ SSL 実装に戻すことができます、ボーリングの SSL を使用しない場合、`$(AndroidTlsProvider)`プロパティを`legacy`(ビルドのプロパティの設定の詳細については、次を参照してください。[ビルド プロセス](~/android/deploy-test/building-apps/build-process.md))。


### <a name="programatically-using-androidclienthandler"></a>プログラムで使用します。 `AndroidClientHandler`

`Xamarin.Android.Net.AndroidClientHandler`は、 `HttpMessageHandler` Xamarin.Android 向けの実装。
このクラスのインスタンスが、ネイティブを使用して`java.net.URLConnection`すべての HTTP 接続を実装します。 これは、HTTP パフォーマンス、および小規模な APK のサイズの増加を理論的に提供されます。

このコード スニペットの 1 つのインスタンスを明示的にする方法の例に示します、`HttpClient`クラス。

```csharp
// Android 4.1 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
> 基になる Android デバイスが TLS 1.2 (つまりをサポートする必要があります。Android 4.1 以降)


## <a name="ssltls-implementation-build-option"></a>SSL/TLS 実装ビルド オプション

このプロジェクトのオプションは、すべての web 要求で使用されるどのような基になる TLS ライブラリを制御します。 どちらも`HttpClient`と`WebRequest`します。 既定では、TLS 1.2 が選択されます。

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

[![TVisual Studio での LS/SSL の実装のコンボ ボックス](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# [<a name="visual-studio-for-mac"></a>Visual Studio for Mac](#tab/macos)

[![TLS/SSL 実装コンボは、Visual studio for Mac ボックス](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png#lightbox)

-----

例えば:

```csharp
var client = new HttpClient();
```

HttpClient 実装に設定された場合**マネージ**に設定されている TLS の実装と**ネイティブ TLS 1.2 +**、`client`オブジェクトが、管理対象に自動的には使用`HttpClientHandler`と用に TLS 1.2 (BoringSSL ライブラリによって提供される) その HTTP 要求。

ただし場合、 **HttpClient 実装**に設定されている`AndroidHttpClient`、し、すべて`HttpClient`オブジェクトが基になる Java クラスを使用して`java.net.URLConnection`と影響を受けることはできません、 **TLS/SSLの実装**値。 `WebRequest` オブジェクトでは、BoringSSL ライブラリを使用します。

## <a name="other-ways-to-control-ssltls-configuration"></a>SSL や TLS の構成を制御する他の方法

3 つの方法が、Xamarin.Android アプリケーションが TLS の設定を制御できます。

1. プロジェクト オプションには、HttpClient の実装と既定の TLS ライブラリを選択します。
2. 使用してプログラムで`Xamarin.Android.Net.AndroidClientHandler`します。
3. (省略可能) 環境変数を宣言します。

3 つの選択肢の推奨方法は、Xamarin.Android プロジェクトのオプションを使用して、既定値を宣言する`HttpMessageHandler`およびアプリ全体の TLS です。 次に、必要に応じて、プログラムによってインスタンス化`Xamarin.Android.Net.AndroidClientHandler`オブジェクト。 上記のこれらのオプション。

3 番目のオプション&ndash;環境変数を使用して&ndash;次に説明します。

### <a name="declare-environment-variables"></a>環境変数を宣言します。

Xamarin.Android で TLS の使用に関連する 2 つの環境変数です。

- `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; この環境変数は、既定値を宣言します。`HttpMessageHandler`アプリケーションで使用されます。 例:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

- `XA_TLS_PROVIDER` &ndash; TLS ライブラリを使用するか、この環境変数を宣言します`btls`、 `legacy`、または`default`(これは、この変数を省略することと同じ)。

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

追加することでこの環境変数を設定、_環境ファイル_をプロジェクトにします。 環境ファイルは Unix 形式のプレーン テキスト ファイルのビルド アクションを持つ**AndroidEnvironment**:

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

![Visual Studio で AndroidEnvironment ビルド アクションのスクリーン ショット。](http-stack-images/tls03-vs.png)

# [<a name="visual-studio-for-mac"></a>Visual Studio for Mac](#tab/macos)

![Visual studio for mac のアクションを構築、AndroidEnvironment のスクリーン ショット](http-stack-images/tls03-xs.png)

-----

参照してください、 [Xamarin.Android 環境](~/android/deploy-test/environment.md)の詳細については、環境変数と Xamarin.Android のガイド。


## <a name="related-links"></a>関連リンク

- [トランスポート層セキュリティ (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
