---
title: "RESTful Web サービスを認証"
description: "HTTP では、リソースへのアクセスを制御するいくつかの認証メカニズムの使用をサポートします。 基本認証では、正しい資格情報を持つクライアントのみにリソースへのアクセスを提供します。 この記事では、基本認証を使用して、RESTful web サービスのリソースへのアクセスを保護する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 7aea74f95e8738cc415eaac3a5ac4f86b069d0f7
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="authenticating-a-restful-web-service"></a>RESTful Web サービスを認証

_HTTP では、リソースへのアクセスを制御するいくつかの認証メカニズムの使用をサポートします。基本認証では、正しい資格情報を持つクライアントのみにリソースへのアクセスを提供します。この記事では、基本認証を使用して、RESTful web サービスのリソースへのアクセスを保護する方法を示します。_

付随する Xamarin.Forms サンプル アプリケーションでは、web サービスへの読み取り専用のアクセスを提供する REST の Xamarin でホストされるサービスを使用します。 そのため、作成、更新、およびデータを削除する操作では、アプリケーションで使用するデータは変更されません。 ただし、REST サービスのホスト可能なバージョンは利用で、 *TodoRESTService*フォルダー サンプル アプリケーションとサービスを設定する方法の手順ではありますがあります。 REST サービスのホスト可能なこのバージョンでは、完全作成、更新、読み取り、および delete データへのアクセスを提供します。

> [!NOTE]
> Ios 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することがない場合のうち ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 アプリケーションを更新することによってこれを行う**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)です。

## <a name="authenticating-users-over-http"></a>HTTP 経由でユーザーの認証

基本認証は、HTTP でサポートされている最も簡単な認証メカニズム、暗号化されていない base64 でエンコードされたテキストとしてユーザー名とパスワードを送信するクライアントでは、 次のように動作します。

- Web サービスは、保護されたリソースの要求を受信した場合は HTTP ステータス コード 401 (アクセスが拒否されました) で要求は拒否され、次の図に示すように、応答の WWW 認証ヘッダーを設定します。

![](rest-images/basic-authentication-fail.png "基本認証の失敗")

- Web サービスで保護されたリソースに対する要求を受け取るかどうか、`Authorization`ヘッダー、正しく設定、web サービスが応答 HTTP ステータス コード 200 で要求が成功したことを示すことと、必要な情報が、応答にします。 このシナリオは、次の図に表示されます。

![](rest-images/basic-authentication-success.png "基本認証の成功")

> [!NOTE]
> 基本認証は、HTTPS 接続経由でのみ使用する必要があります。 HTTP 接続経由で使用する場合、<code>Authorization</code>ヘッダーは、HTTP トラフィックは、攻撃者によってキャプチャされた場合に簡単にデコードできます。

## <a name="specifying-basic-authentication-in-a-web-request"></a>Web 要求で基本認証を指定します。

基本認証の使用はよう指定します。

1. "Basic"が追加する文字列、`Authorization`要求のヘッダー。
1. ユーザー名とパスワードを組み合わせて構成文字列に「ユーザー名: パスワード」、base64 でエンコードされてに追加では、その形式、`Authorization`要求のヘッダー。

そのため、'XamarinUser' のユーザー名とパスワードは 'XamarinPassword'、ヘッダーになります。

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

`HttpClient`クラスを設定できます、`Authorization`でヘッダーの値、`HttpClient.DefaultRequestHeaders.Authorization`プロパティです。 `HttpClient`インスタンスが複数の要求が存在する、`Authorization`ときではなく、1 回設定する場合にのみ必要なヘッダーの次のコード例に示すようにすべての要求を行います。

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    var authData = string.Format ("{0}:{1}", Constants.Username, Constants.Password);
    var authHeaderValue = Convert.ToBase64String (Encoding.UTF8.GetBytes (authData));

    client = new HttpClient ();
    ...
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue ("Basic", authHeaderValue);
  }
  ...
}
```

要求に署名する web サービス操作に要求が行われたときにしてから、`Authorization`ヘッダー、ユーザーが操作を呼び出す権限を持っているかどうかどうかを示すです。

> [!NOTE]
> REST サービスのサンプルでは、定数として資格情報を格納、公開されたアプリケーションでセキュリティ保護されていない形式でない格納必要があります。 [Xamarith.Auth](https://www.nuget.org/packages/Xamarin.Auth/) NuGet は、資格情報を安全に格納するための機能を提供します。 詳細については、次を参照してください。[の格納とデバイスのアカウント情報の取得](~/xamarin-forms/data-cloud/authentication/oauth.md)です。


## <a name="processing-the-authorization-header-server-side"></a>承認ヘッダーのサーバー側の処理

付属の REST サービスのサンプルでは、各アクションの装飾、`[BasicAuthentication]`属性。 この属性はによって実装される、`BasicAuthenticationAttribute`ソリューションでは、クラスし、解析に使用される、`Authorization`ヘッダーに格納された値との比較によって、base64 でエンコードされた資格情報が有効なかどうかが判断と*Web.config*.この方法は、サンプルのサービスに適した、公開された web サービスを拡張する必要があります。

IIS で使用する基本的な認証モジュールでは、ユーザーが自分の Windows 資格情報に対して認証されます。 そのため、ユーザーは、サーバーのドメインにアカウントが必要です。 ただし、データベースなどの外部ソースに対してユーザー アカウントを認証する、カスタムの認証を許可する、基本認証モデルを構成できます。 詳細については、次を参照してください。 [ASP.NET Web API で基本認証](http://www.asp.net/web-api/overview/security/basic-authentication)ASP.NET web サイトです。

> [!NOTE]
> 基本認証は、ログアウトを管理する想定していません。したがって、ログアウトの標準の基本的な認証方法は、セッションを終了します。

## <a name="summary"></a>まとめ

この記事には、Xamarin.Forms を使用してアプリケーションから web 要求に基本認証を追加する方法が示されている、`HttpClient`クラスです。 基本認証では、正しい資格情報を持つクライアントのみにリソースへのアクセスを提供します。 使用する方法については[Xamarin.Auth](https://www.nuget.org/packages/Xamarin.Auth/) Xamarin.Forms アプリケーションでの認証プロセスを管理ユーザーがデータへのアクセスを持つのみ中に、バックエンドを共有できるように、次を参照してください[ユーザーの認証。Id プロバイダーと](~/xamarin-forms/data-cloud/authentication/oauth.md)です。


## <a name="related-links"></a>関連リンク

- [TodoREST (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [RESTful web サービスの使用](~/xamarin-forms/data-cloud/consuming/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
