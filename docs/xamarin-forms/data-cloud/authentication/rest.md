---
title: RESTful Web サービスを認証する
description: 基本認証は、正しい資格情報を持つクライアントのみにリソースへのアクセスを提供します。 この記事では、基本認証を使用して、RESTful web サービスリソースへのアクセスを保護する方法について説明します。
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
ms.openlocfilehash: 23516603633116a8e28ae33004bdb6fc8764ec21
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032824"
---
# <a name="authenticate-a-restful-web-service"></a>RESTful Web サービスを認証する

_HTTP は、複数の認証メカニズムを使用してリソースへのアクセスを制御することをサポートしています。基本認証は、正しい資格情報を持つクライアントのみにリソースへのアクセスを提供します。この記事では、基本認証を使用して、RESTful web サービスリソースへのアクセスを保護する方法について説明します。_

> [!NOTE]
> IOS 9 以降では、アプリトランスポートセキュリティ (ATS) によって、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続が適用されるため、機密情報が誤って開示されるのを防ぐことができます。 IOS 9 用に構築されたアプリでは、ATS が既定で有効になっているため、すべての接続は、ATS のセキュリティ要件の対象となります。 接続がこれらの要件を満たしていない場合、例外が発生して失敗します。
> `HTTPS` プロトコルを使用できず、インターネットリソースに対してセキュリティで保護された通信を行うことができない場合は、ATS をオプトアウトできます。 これは、アプリの**情報**ファイルを更新することで実現できます。 詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)」を参照してください。

## <a name="authenticating-users-over-http"></a>HTTP 経由でのユーザーの認証

基本認証は、HTTP でサポートされている最も単純な認証メカニズムであり、クライアントはユーザー名とパスワードを暗号化されていない base64 エンコードテキストとして送信します。 次のように動作します。

- Web サービスは、保護されたリソースの要求を受信すると、HTTP 状態コード 401 (アクセスが拒否されました) の要求を拒否し、次の図に示すように、WWW 認証応答ヘッダーを設定します。

![](rest-images/basic-authentication-fail.png "Basic Authentication Failing")

- Web サービスが保護されたリソースの要求を受信し、`Authorization` ヘッダーが正しく設定されている場合、web サービスは HTTP 状態コード200で応答します。これは、要求が成功したことと、要求された情報が応答内にあることを示します。 このシナリオを次の図に示します。

![](rest-images/basic-authentication-success.png "Basic Authentication Succeeding")

> [!NOTE]
> 基本認証は、HTTPS 接続経由でのみ使用する必要があります。 Http 接続で使用する場合、HTTP トラフィックが攻撃者によってキャプチャされると、`Authorization` ヘッダーを簡単にデコードできます。

## <a name="specifying-basic-authentication-in-a-web-request"></a>Web 要求での基本認証の指定

基本認証の使用は、次のように指定します。

1. 文字列 "Basic" が要求の `Authorization` ヘッダーに追加されます。
1. ユーザー名とパスワードは、"username: password" という形式の文字列に結合され、base64 でエンコードされ、要求の `Authorization` ヘッダーに追加されます。

このため、ユーザー名が ' XamarinUser ' でパスワードが ' XamarinPassword ' の場合、ヘッダーは次のようになります。

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

`HttpClient` クラスでは、`HttpClient.DefaultRequestHeaders.Authorization` プロパティで `Authorization` ヘッダー値を設定できます。 `HttpClient` インスタンスは複数の要求にわたって存在するので、次のコード例に示すように、`Authorization` ヘッダーは、すべての要求を行うときではなく、1回だけ設定する必要があります。

```csharp
public class RestService : IRestService
{
  HttpClient _client;
  ...

  public RestService ()
  {
    var authData = string.Format ("{0}:{1}", Constants.Username, Constants.Password);
    var authHeaderValue = Convert.ToBase64String (Encoding.UTF8.GetBytes (authData));

    _client = new HttpClient ();
    _client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue ("Basic", authHeaderValue);
  }
  ...
}
```

その後、web サービス操作に対して要求が行われると、要求は `Authorization` ヘッダーを使用して署名されます。これは、ユーザーが操作を呼び出すためのアクセス許可を持っているかどうかを示します。

> [!NOTE]
> このコードでは、資格情報は定数として格納されますが、公開されたアプリケーションで安全ではない形式で格納することはできません。 [Xamarith](https://www.nuget.org/packages/Xamarin.Auth/) NuGet には、資格情報を安全に格納する機能が用意されています。 詳細については[、「デバイスにアカウント情報を保存および取得](~/xamarin-forms/data-cloud/authentication/oauth.md)する」を参照してください。

## <a name="processing-the-authorization-header-server-side"></a>Authorization ヘッダーサーバー側の処理

REST サービスは、各アクションを `[BasicAuthentication]` 属性で装飾する必要があります。 この属性は、`Authorization` ヘッダーを解析し、base64 でエンコードされた資格情報*が web.config に*格納されている値と比較することによって有効かどうかを判断するために使用されます。この方法はサンプルサービスに適していますが、公開されている web サービスの拡張が必要です。

IIS によって使用される基本認証モジュールでは、ユーザーは Windows 資格情報に対して認証されます。 そのため、ユーザーは、サーバーのドメインのアカウントを持っている必要があります。 ただし、基本認証モデルはカスタム認証を許可するように構成できます。この場合、ユーザーアカウントは、データベースなどの外部ソースに対して認証されます。 詳細については、ASP.NET web サイトの「 [ASP.NET Web API の基本認証](https://www.asp.net/web-api/overview/security/basic-authentication)」を参照してください。

> [!NOTE]
> 基本認証は、ログアウトを管理するように設計されていません。そのため、ログアウトの標準的な基本認証方法として、セッションを終了することが挙げられます。

## <a name="related-links"></a>関連リンク

- [RESTful web サービスを使用する](~/xamarin-forms/data-cloud/web-services/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
