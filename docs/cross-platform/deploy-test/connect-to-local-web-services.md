---
title: iOS シミュレーターと Android エミュレーターからローカル Web サービスに接続する
description: この記事では、iOS シミュレーターまたは Android エミュレーターで実行している Xamarin モバイル アプリケーションで、ローカルで実行されている ASP.NET Core Web サービスを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: FD8FE199-898B-4841-8041-CC9CA1A00917
author: davidbritch
ms.author: dabritch
ms.date: 04/29/2020
ms.openlocfilehash: 3dc1a2cb99c5ef018807a8ac81139a6cace3c66f
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516496"
---
# <a name="connect-to-local-web-services-from-ios-simulators-and-android-emulators"></a>iOS シミュレーターと Android エミュレーターからローカル Web サービスに接続する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest/)

モバイル アプリケーションの多くが Web サービスを使用します。 開発フェーズ中では、Web サービスをローカルに展開して、iOS シミュレーターまたは Android エミュレーター内で実行しているモバイル アプリケーションからそれを使うことは一般的です。 これにより、ホストされているエンドポイントに Web サービスを展開する必要がなくなります。また、モバイル アプリケーションと Web サービスの両方がローカルで実行されるため、わかりやすいデバッグ エクスペリエンスが実現します。

iOS シミュレーターまたは Android エミュレーターで実行されているモバイル アプリケーションでは、次のように、ローカルで実行されているか、HTTP 経由で公開されている ASP.NET Core Web サービスを使用できます。

- iOS シミュレーターで実行されているアプリケーションは、ご自分のコンピューターの IP アドレス、または `localhost` ホスト名を使って、ローカルの HTTP Web サービスに接続できます。 たとえば、相対 URI `/api/todoitems/` を使って GET 操作を公開しているローカル HTTP Web サービスがある場合、iOS シミュレーターで実行されているアプリケーションでは、`http://localhost:<port>/api/todoitems/` に GET 要求を送信することでその操作を使用できます。
- Android エミュレーターで実行されているアプリケーションでは、ホスト ループバック インターフェイスに対する別名である `10.0.2.2` アドレス (開発用マシンでは `127.0.0.1`) 経由でローカル HTTP Web サービスに接続できます。 たとえば、相対 URI `/api/todoitems/` を使って GET 操作を公開しているローカル HTTP Web サービスがある場合、Android エミュレーターで実行されているアプリケーションでは、`http://10.0.2.2:<port>/api/todoitems/` に GET 要求を送信することでその操作を使用できます。

ただし、iOS シミュレーターまたは Android エミュレーターで実行されているアプリケーションで、HTTPS 経由で公開されているローカル Web サービスを使用する場合は、追加の作業が必要です。 このシナリオ用のプロセスは次のようになります。

1. ご自身のコンピューター上に自己署名済み開発証明書を作成します。 詳しくは、「[Create a development certificate (開発証明書を作成する)](#create-a-development-certificate)」をご覧ください。
1. デバッグ ビルド用に適切な `HttpClient` ネットワーク スタックを使うようプロジェクトを構成します。 詳細については、「[Configure your project (プロジェクトを構成する)](#configure-your-project)」をご覧ください。
1. ご自分のローカル コンピューターのアドレスを指定します。 詳細については、「[Specify the local machine address (ローカル コンピューターのアドレスを指定する)](#specify-the-local-machine-address)」をご覧ください。
1. ローカル開発証明書のセキュリティ チェックをバイパスします。 詳細については、「[Bypass the certificate security check (証明書のセキュリティ チェックをバイパスする)](#bypass-the-certificate-security-check)」をご覧ください。

それぞれの項目について順番に説明します。

## <a name="create-a-development-certificate"></a>開発証明書を作成する

.NET Core SDK をインストールすると、ローカル ユーザーの証明書ストアに ASP.NET Core HTTPS 開発証明書がインストールされます。 ただし、証明書はインストールされましたが、信頼されていません。 証明書を信頼するには、次の 1 回限りの手順を実行して dotnet の `dev-certs` ツールを実行します。

```dotnetcli
dotnet dev-certs https --trust
```

次のコマンドにより、`dev-certs` ツールに関するヘルプが表示されます。

```dotnetcli
dotnet dev-certs https --help
```

または、HTTPS を使用する、ASP.NET Core 2.1 (またはそれ以降) のプロジェクトを実行する場合、Visual Studio によって開発証明書が不足しているかどうかが検出され、それをインストールして信頼するよう提案されます。

> [!NOTE]
> ASP .NET Core HTTPS 開発証明書は自己署名です。

コンピューターでローカルの HTTPS を有効にする方法について詳しくは、「[ローカル HTTPS を有効にする](/aspnet/core/getting-started#enable-local-https)」をご覧ください。

## <a name="configure-your-project"></a>プロジェクトを構成する

iOS および Android 上で実行する Xamarin アプリケーションでは、`HttpClient` クラスでどのネットワーク スタックを使うか指定できます。選択はマネージド ネットワーク スタックまたはネイティブ ネットワーク スタックです。 マネージド スタックには既存の .NET コードとの互換性が高いレベルで備わっていますが、TLS 1.0 に制限されていて、低速になり実行可能ファイルのサイズがより大きくなる可能性があります。 ネイティブ スタックはより高速で、よりセキュリティに優れていますが、`HttpClient` クラスのすべての機能が備わっていない場合があります。

### <a name="ios"></a>iOS

iOS 上で実行される Xamarin アプリケーションでは、マネージド ネットワーク スタック、またはネイティブの `CFNetwork` または `NSUrlSession` ネットワーク スタックを使用できます。 既定では、新しい iOS プラットフォームのプロジェクトでは `NSUrlSession` ネットワーク スタックが使われ、TLS 1.2 がサポートされ、またパフォーマンスの向上と実行可能ファイルのサイズの縮小のためにネイティブ API が使われます。 詳細については、「[HttpClient and SSL/TLS implementation selector for iOS/macOS (iOS/macOS 用の HttpClient と SSL/TLS の実装セレクター)](~/cross-platform/macios/http-stack.md)」をご覧ください。

### <a name="android"></a>Android

Android 上で実行される Xamarin アプリケーションでは、マネージド `HttpClient` ネットワーク スタック、またはネイティブの `AndroidClientHandler` ネットワーク スタックを使用できます。 既定では、新しい Android プラットフォームのプロジェクトでは `AndroidClientHandler` ネットワーク スタックが使われ、TLS 1.2 がサポートされ、またパフォーマンスの向上と実行可能ファイルのサイズの縮小のためにネイティブ API が使われます。 Android のネットワーク スタックについて詳しくは、「[Android 用の HttpClient スタックと SSL/TLS の実装セレクター](~/android/app-fundamentals/http-stack.md)」をご覧ください。

## <a name="specify-the-local-machine-address"></a>ローカル コンピューターのアドレスを指定する

iOS シミュレーターと Android エミュレーターのどちらも、ローカル コンピューター上で実行されているセキュリティで保護された Web サービスにアクセスできます。 ただし、ローカル コンピューターのアドレスはそれぞれで異なります。

### <a name="ios"></a>iOS

iOS シミュレーターでは、ホスト コンピューターのネットワークを使います。 そのため、シミュレーターで実行されているアプリケーションでは、コンピューターの IP アドレス、または `localhost` ホスト名を使って、ローカル コンピューター上で実行されている Web サービスに接続できます。 たとえば、相対 URI `/api/todoitems/` を使って GET 操作を公開しているローカルのセキュリティで保護された Web サービスがある場合、iOS シミュレーターで実行されているアプリケーションでは、`https://localhost:<port>/api/todoitems/` に GET 要求を送信することでその操作を使用できます。

> [!NOTE]
> Windows から iOS シミュレーターでモバイル アプリケーションを実行している場合、アプリケーションは [Windows 用のリモートの iOS シミュレーター](~/tools/ios-simulator/index.md)で表示されます。 ただし、アプリケーションはペアリング済みの Mac 上で実行されます。 そのため、Mac 上で実行されている iOS アプリケーション用の、Windows で実行されている Web サービスへの localhost アクセスはありません。

### <a name="android"></a>Android

Android エミュレーターの各インスタンスは、開発用コンピューターのネットワーク インターフェイスから分離され、仮想ルーターの背後で実行されます。 そのため、エミュレートされたデバイスでは、開発用コンピューターやネットワーク上のその他のエミュレーター インスタンスを確認できません。

ただし、各エミュレーターの仮想ルーターで、事前に割り当てられたアドレスを含む特殊なネットワーク空間が管理されます。ここで、`10.0.2.2` アドレスはホスト ループバック インターフェイス (開発用コンピューター上の 127.0.0.1) の別名です。 そのため、相対 URI `/api/todoitems/` を使って GET 操作を公開しているローカルのセキュリティで保護された Web サービスがある場合、Android エミュレーターで実行されているアプリケーションでは、`https://10.0.2.2:<port>/api/todoitems/` に GET 要求を送信することでその操作を使用できます。

### <a name="xamarinforms-example"></a>Xamarin.Forms の例

Xamarin.Forms アプリケーションで、[`Device`](xref:Xamarin.Forms.Device) クラスを使って、アプリケーションが実行されているプラットフォームを検出することができます。 次に、ローカルのセキュリティで保護された Web サービスへのアクセスを実現する適切なホスト名は、次のように設定できます。

```csharp
public static string BaseAddress =
    Device.RuntimePlatform == Device.Android ? "https://10.0.2.2:5001" : "https://localhost:5001";
public static string TodoItemsUrl = $"{BaseAddress}/api/todoitems/";
```

## <a name="bypass-the-certificate-security-check"></a>証明書のセキュリティ チェックをバイパスする

iOS シミュレーターまたは Android エミュレーターで実行されているアプリケーションからローカルのセキュリティで保護された Web サービスを呼び出そうとすると、各プラットフォーム上でマネージド ネットワーク スタックを使っている場合でも、`HttpRequestException` がスローされます。 これは、ローカルの HTTPS 開発証明書が自己署名であり、自己署名された証明書は iOS または Android で信頼されないためです。 したがって、アプリケーションでローカルのセキュリティで保護された Web サービスを使用するときに、SSL エラーを無視する必要があります。 iOS と Android でマネージドとネイティブ両方のネットワーク スタックを使っている場合、`HttpClientHandler` オブジェクトの `ServerCertificateCustomValidationCallback` プロパティを、ローカルの HTTPS 開発証明書に向けた証明書のセキュリティ チェックの結果を無視するコールバックに設定することで、これを実現できます。

```csharp
// This method must be in a class in a platform project, even if
// the HttpClient object is constructed in a shared project.
public HttpClientHandler GetInsecureHandler()
{
    HttpClientHandler handler = new HttpClientHandler();
    handler.ServerCertificateCustomValidationCallback = (message, cert, chain, errors) =>
    {
        if (cert.Issuer.Equals("CN=localhost"))
            return true;
        return errors == System.Net.Security.SslPolicyErrors.None;
    };
    return handler;
}
```

このコード例では、検証が行われた証明書が `localhost` 証明書ではない場合に、サーバー証明書の検証結果が返されます。 この証明書に対して、検証結果が無視され、証明書が有効であることを示す `true` が返されます。 生成される `HttpClientHandler` オブジェクトを、`HttpClient` コンストラクターに対する引数として渡す必要があります。

## <a name="related-links"></a>関連リンク

- [TodoREST (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest/)
- [ローカル HTTPS を有効にする](/aspnet/core/getting-started#enable-local-https)
- [iOS/macOS 用の HttpClient と SSL/TLS の実装セレクター](~/cross-platform/macios/http-stack.md)
- [Android 用の HttpClient スタックと SSL/TLS の実装セレクター](~/android/app-fundamentals/http-stack.md)
