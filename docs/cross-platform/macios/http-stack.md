---
title: IOS/macOS 用の HttpClient と SSL/TLS 実装セレクター
description: HttpClient スタックと SSL/TLS 実装セレクターによって、Xamarin iOS、tvOS、または macOS アプリで使用される HttpClient と SSL/TLS の実装が決まります。
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: davidortinau
ms.author: daortin
ms.date: 04/20/2018
ms.openlocfilehash: 3b87eb6ae9e97be4ee34b092efce72c0d97c88b3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015849"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>IOS/macOS 用の HttpClient と SSL/TLS 実装セレクター

Xamarin.iOS、Xamarin.tvOS、および Xamarin.Mac 用の **Httpclient 実装セレクター** は、使用する実装 `HttpClient` を制御します。 iOS、tvOS、または macOS ネイティブトランスポート (OS によっては`NSUrlSession` または `CFNetwork`) を使用する実装に切り替えることができます。 利点は、TLS 1.2-サポート、より小さなバイナリ、および高速ダウンロードです。欠点は、非同期操作を実行するためにイベントループを実行する必要があることです。

プロジェクトは、**System.Net.Http**アセンブリを参照する必要があります。

> [!WARNING]
> **2018 年4月、** セキュリティ要件が増加しているため (PCI コンプライアンスを含む)、主要クラウドプロバイダーと web サーバーは、1.2 より前の TLS バージョンのサポートを停止することが予想されます。 以前のバージョンの Visual Studio で作成された Xamarin プロジェクトは、既定で古いバージョンの TLS を使用します。
>
> アプリがこれらのサーバーとサービスを引き続き使用できるようにするに**は、次に示す `NSUrlSession` 設定を使用して Xamarin プロジェクトを更新し、アプリを再ビルドしてユーザーに再配置する必要があり**ます。

## <a name="selecting-an-httpclient-stack"></a>HttpClient スタックの選択

アプリによって使用されている `HttpClient` を調整するには、次のようにします。

1. **ソリューションエクスプローラー**で**プロジェクト名**をダブルクリックして、[プロジェクトオプション] を開きます。
2. プロジェクトの**ビルド**設定 (たとえば、Xamarin. ios アプリの**ios ビルド**) に切り替えます。
3. **[Httpclient 実装]** ドロップダウンから、次のいずれかとして `HttpClient` の種類を選択します。 **nn session** (推奨)、 **Cfnetwork**、または**Managed**。

[管理対象、CFNetwork、または Nの Lsession から HttpClient の実装を選択![](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> TLS 1.2 サポートの場合は、`NSUrlSession` オプションを使用することをお勧めします。

### <a name="nsurlsession"></a>NSUrlSession

`NSURLSession`ベースのハンドラーは、iOS 7 以降で使用可能なネイティブ `NSURLSession` フレームワークに基づいています。 
**これは推奨される設定です。**

#### <a name="pros"></a>プロフェッショナル

- パフォーマンスを向上させ、実行可能ファイルのサイズを小さくするためにネイティブ Api を使用しています。
- TLS 1.2 などの最新の標準がサポートされます。

#### <a name="cons"></a>マイナス

- IOS 7 以降が必要です。
- 一部の `HttpClient` 機能またはオプションは使用できません。

### <a name="cfnetwork"></a>CFNetwork

`CFNetwork`ベースのハンドラーは、iOS 6 以降で使用可能なネイティブ `CFNetwork` フレームワークに基づいています。

#### <a name="pros"></a>プロフェッショナル

- パフォーマンスを向上させ、実行可能ファイルのサイズを小さくするためにネイティブ Api を使用しています。
- TLS 1.2 などの新しい標準がサポートされます。

#### <a name="cons"></a>マイナス

- IOS 6 以降が必要です。
- WatchOS では使用できません。
- 一部の HttpClient 機能/オプションは使用できません。

### <a name="managed"></a>マネージド

マネージハンドラーは、以前のバージョンの Xamarin に付属していた、完全に管理された HttpClient ハンドラーです。

#### <a name="pros"></a>プロフェッショナル

- これには、Microsoft .NET 以前の Xamarin バージョンと互換性のある機能セットがあります。

#### <a name="cons"></a>マイナス

- Apple Os と完全には統合されておらず、TLS 1.0 に制限されています。 今後、セキュリティで保護された web サーバーまたはクラウドサービスに接続できない可能性があります。
- 一般に、ネイティブ Api よりも暗号化のような場合には非常に遅くなります。
- これにより、より多くのマネージコードが必要になるため、より大きなアプリの再配布が可能になります。

### <a name="programmatically-setting-the-httpmessagehandler"></a>プログラムによる HttpMessageHandler の設定

上記のプロジェクト全体の構成に加えて、次のコードスニペットに示すように、`HttpClient` をインスタンス化し、コンストラクターを使用して目的の `HttpMessageHandler` を挿入することもできます。

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

これにより、 **[プロジェクトオプション]** ダイアログで宣言されているものとは異なる `HttpMessageHandler` を使用できるようになります。

## <a name="ssltls-implementation"></a>SSL/TLS の実装

SSL (Secure Socket Layer) とその後継 (TLS (Transport Layer Security)) は、`System.Net.Security.SslStream`を介して HTTP およびその他のネットワーク接続のサポートを提供します。 TvOS `System.Net.Security.SslStream` の実装では、Mono によって提供されるマネージ実装を使用する代わりに、Apple のネイティブの SSL/TLS 実装が呼び出されます。 Apple のネイティブ実装では、TLS 1.2 がサポートされています。

> [!WARNING]
> 今度の Xamarin.Mac 4.8 リリースでは、macOS 10.9 以降のみをサポートします。
> 以前のバージョンの Xamarin.Mac では macOS 10.7 以降をサポートしていましたが、これらの古い macOS バージョンは TLS 1.2 をサポートするための十分な TLS インフラストラクチャがありませんでした。 macOS 10.7 または macOS 10.8 をターゲットにするには、Xamarin.Mac 4.6 以前を使用してください。

## <a name="app-transport-security"></a>アプリケーション トランスポート セキュリティ

Apple の_App Transport Security_ (ATS) は、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続を適用します。 ATS は、すべてのインターネット通信がセキュリティで保護された接続のベストプラクティスに準拠していることを保証します。これにより、アプリまたは使用しているライブラリを通じて、機密情報が誤って開示されるのを防ぎます。

IOS 9、tvOS 9、OS X 10.11 (El Capitan) 用に構築されたアプリでは、既定で ATS が有効になっているため、`NSUrlConnection`、`CFUrl` または `NSUrlSession` を使用するすべての接続は、ATS のセキュリティ要件の対象となります。 接続がこれらの要件を満たしていない場合は、例外が発生して失敗します。

HttpClient スタックと SSL/TLS 実装の選択に基づいて、ATS で正常に動作するようにアプリを変更することが必要になる場合があります。

ATS の詳細については、「[アプリトランスポートセキュリティガイド](~/ios/app-fundamentals/ats.md)」を参照してください。

## <a name="known-issues"></a>既知の問題

このセクションでは、Xamarin. iOS での TLS サポートに関する既知の問題について説明します。

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>"要求された値 AppleTLS が見つかりませんでした" というエラーが発生し、プロジェクトを読み込めませんでした

Xamarin ios 9.8 では、Xamarin. iOS アプリケーションの **.csproj**ファイルを含むいくつかの新しい設定が導入されました。 これらの変更により、プロジェクトが以前のバージョンの Xamarin. iOS で開かれているときに問題が発生する可能性があります。 次のスクリーンショットは、このシナリオで表示される可能性のあるエラーメッセージの例です。

![プロジェクトを読み込もうとしてエラーのスクリーンショットが発生しました。要求された値が見つかりません](http-stack-images/tlserror-xs.png)

このエラーは、Xamarin. iOS 9.8 のプロジェクトファイルに `MtouchTlsProvider` 設定が導入されていることが原因で発生します。 Xamarin. iOS 9.8 (またはそれ以降) に更新できない場合は、 **.csproj**ファイルアプリケーションを手動で編集し、`MtouchTlsprovider` 要素を削除してから、変更したプロジェクトファイルを保存します。

次のスニペットは、 **.csproj**ファイル内で `MtouchTlsProvider` 設定がどのように見えるかを示しています。

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>関連リンク

- [トランスポート層セキュリティ (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)
