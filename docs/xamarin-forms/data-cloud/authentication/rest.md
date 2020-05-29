---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d62e533d127294c77c0779c20fd9c78ef2231200
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135721"
---
# <a name="authenticate-a-restful-web-service"></a>RESTful Web サービスを認証する

_HTTP は、複数の認証メカニズムを使用してリソースへのアクセスを制御することをサポートしています。基本認証は、正しい資格情報を持つクライアントのみにリソースへのアクセスを提供します。この記事では、基本認証を使用して、RESTful web サービスリソースへのアクセスを保護する方法について説明します。_

> [!NOTE]
> IOS 9 以降では、アプリトランスポートセキュリティ (ATS) によって、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続が適用されるため、機密情報が誤って開示されるのを防ぐことができます。 IOS 9 用に構築されたアプリでは、ATS が既定で有効になっているため、すべての接続は、ATS のセキュリティ要件の対象となります。 接続がこれらの要件を満たしていない場合、例外が発生して失敗します。
> `HTTPS`インターネットリソースに対してプロトコルとセキュリティで保護された通信を使用できない場合は、ATS をオプトアウトできます。 これは、アプリの**情報**ファイルを更新することで実現できます。 詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)」を参照してください。

## <a name="authenticating-users-over-http"></a>HTTP 経由でのユーザーの認証

基本認証は、HTTP でサポートされている最も単純な認証メカニズムであり、クライアントはユーザー名とパスワードを暗号化されていない base64 エンコードテキストとして送信します。 手順は次のようになります。

- Web サービスは、保護されたリソースの要求を受信すると、HTTP 状態コード 401 (アクセスが拒否されました) の要求を拒否し、次の図に示すように、WWW 認証応答ヘッダーを設定します。

![](rest-images/basic-authentication-fail.png "Basic Authentication Failing")

- Web サービスが保護されたリソースの要求を受信し、ヘッダーが正しく設定されている場合、 `Authorization` web サービスは HTTP 状態コード200で応答します。これは、要求が成功したことと、要求された情報が応答内にあることを示します。 このシナリオを次の図に示します。

![](rest-images/basic-authentication-success.png "Basic Authentication Succeeding")

> [!NOTE]
> 基本認証は、HTTPS 接続経由でのみ使用する必要があります。 Http 接続で使用する場合、 `Authorization` http トラフィックが攻撃者によってキャプチャされると、ヘッダーを簡単にデコードできます。

## <a name="specifying-basic-authentication-in-a-web-request"></a>Web 要求での基本認証の指定

基本認証の使用は、次のように指定します。

1. 文字列 "Basic" が要求のヘッダーに追加され `Authorization` ます。
1. ユーザー名とパスワードは、"username: password" という形式の文字列に結合されます。これは base64 でエンコードされ、要求のヘッダーに追加され `Authorization` ます。

このため、ユーザー名が ' XamarinUser ' でパスワードが ' XamarinPassword ' の場合、ヘッダーは次のようになります。

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

クラスでは、 `HttpClient` プロパティのヘッダー値を設定でき `Authorization` `HttpClient.DefaultRequestHeaders.Authorization` ます。 インスタンスは複数の要求にわたって存在するので、 `HttpClient` `Authorization` 次のコード例に示すように、ヘッダーは、すべての要求を行うときではなく、1回だけ設定する必要があります。

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

その後、web サービス操作に対して要求が行われると、ユーザーが `Authorization` 操作を呼び出すためのアクセス許可を持っているかどうかを示すヘッダーで要求が署名されます。

> [!IMPORTANT]
> このコードでは、資格情報は定数として格納されますが、公開されたアプリケーションで安全ではない形式で格納することはできません。

## <a name="processing-the-authorization-header-server-side"></a>Authorization ヘッダーサーバー側の処理

REST サービスは、各アクションを属性で装飾する必要があり `[BasicAuthentication]` ます。 この属性は、ヘッダーを解析 `Authorization` し、base64 でエンコードされた資格情報が web.config に格納されて*Web.config*いる値と比較することによって有効かどうかを判断するために使用されます。この方法はサンプルサービスに適していますが、公開されている web サービスの拡張が必要です。

IIS によって使用される基本認証モジュールでは、ユーザーは Windows 資格情報に対して認証されます。 そのため、ユーザーは、サーバーのドメインのアカウントを持っている必要があります。 ただし、基本認証モデルはカスタム認証を許可するように構成できます。この場合、ユーザーアカウントは、データベースなどの外部ソースに対して認証されます。 詳細については、ASP.NET web サイトの「 [ASP.NET Web API の基本認証](https://www.asp.net/web-api/overview/security/basic-authentication)」を参照してください。

> [!NOTE]
> 基本認証は、ログアウトを管理するように設計されていません。そのため、ログアウトの標準的な基本認証方法として、セッションを終了することが挙げられます。

## <a name="related-links"></a>関連リンク

- [RESTful web サービスを使用する](~/xamarin-forms/data-cloud/web-services/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
