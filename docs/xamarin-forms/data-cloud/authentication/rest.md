---
title: RESTful Web サービスの認証
description: 基本認証では、正しい資格情報を持つクライアントにのみリソースへのアクセスが提供されます。 この記事では、基本認証を使用して RESTful Web サービス リソースへのアクセスを保護する方法について説明します。
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
ms.openlocfilehash: 65606b72c3944809a09b8c70b9cdbb5524e0f856
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388591"
---
# <a name="authenticate-a-restful-web-service"></a>RESTful Web サービスの認証

_HTTP では、リソースへのアクセスを制御するための認証メカニズムの使用がサポートされています。基本認証では、正しい資格情報を持つクライアントにのみリソースへのアクセスが提供されます。この記事では、基本認証を使用して RESTful Web サービス リソースへのアクセスを保護する方法を示します。_

> [!NOTE]
> iOS 9 以上では、アプリ トランスポート セキュリティ (ATS) は、インターネット リソース (アプリのバックエンド サーバーなど) とアプリの間の安全な接続を強制し、機密情報の誤った漏洩を防止します。 IOS 9 用に構築されたアプリでは ATS が既定で有効になっているため、すべての接続は ATS のセキュリティ要件の対象となります。 接続がこれらの要件を満たさない場合、例外で失敗します。
> インターネットリソースに対してプロトコルとセキュリティで保護された通信を`HTTPS`使用できない場合、ATS はオプトアウトできます。 これは、アプリの**Info.plist**ファイルを更新することで実現できます。 詳細については、「[アプリケーショントランスポートセキュリティ](~/ios/app-fundamentals/ats.md)」を参照してください。

## <a name="authenticating-users-over-http"></a>HTTP 経由でのユーザーの認証

基本認証は、HTTP でサポートされる最も単純な認証メカニズムであり、クライアントは、暗号化されていない base64 エンコード テキストとしてユーザー名とパスワードを送信します。 手順は次のようになります。

- Web サービスは、保護されたリソースに対する要求を受信すると、HTTP ステータス コード 401 (アクセスが拒否されました) で要求を拒否し、次の図に示すように WWW 認証応答ヘッダーを設定します。

![](rest-images/basic-authentication-fail.png "Basic Authentication Failing")

- `Authorization` Web サービスが、ヘッダーが正しく設定された保護リソースの要求を受信すると、Web サービスは HTTP ステータス コード 200 で応答します。 このシナリオは、次の図に示します。

![](rest-images/basic-authentication-success.png "Basic Authentication Succeeding")

> [!NOTE]
> 基本認証は、HTTPS 接続上でのみ使用してください。 HTTP 接続を介して使用すると`Authorization`、攻撃者が HTTP トラフィックをキャプチャした場合に、ヘッダーを簡単にデコードできます。

## <a name="specifying-basic-authentication-in-a-web-request"></a>Web 要求での基本認証の指定

基本認証の使用は、次のように指定します。

1. 文字列 "Basic" が要求の`Authorization`ヘッダーに追加されます。
1. ユーザー名とパスワードは、"username:password" という形式の文字列に結合され、base64 がエンコードされてリクエストの`Authorization`ヘッダーに追加されます。

したがって、'XamarinUser' のユーザー名と 'XamarinPassword' のパスワードを使用すると、ヘッダーは次のようになります。

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

クラス`HttpClient`は、プロパティの`Authorization`ヘッダー値を`HttpClient.DefaultRequestHeaders.Authorization`設定できます。 インスタンスは`HttpClient`複数の要求にまたがって`Authorization`存在するため、次のコード例に示すように、すべての要求を行う場合ではなく、ヘッダーを 1 回だけ設定する必要があります。

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

その後、Web サービス操作に対して要求が行われると、その`Authorization`要求はヘッダーで署名され、ユーザーが操作を呼び出す権限を持っているかどうかを示します。

> [!IMPORTANT]
> このコードは資格情報を定数として格納しますが、公開されたアプリケーションに安全でない形式で格納しないでください。

## <a name="processing-the-authorization-header-server-side"></a>承認ヘッダー サーバー側の処理

REST サービスは、各アクションを属性で`[BasicAuthentication]`修飾する必要があります。 この属性は、ヘッダーを`Authorization`解析し *、Base64*でエンコードされた資格情報が有効かどうかを判断するために使用されます。このアプローチはサンプル サービスに適していますが、公開 Web サービス用に拡張する必要があります。

IIS で使用される基本認証モジュールでは、ユーザーは Windows 資格情報に対して認証されます。 したがって、ユーザーはサーバーのドメインにアカウントを持っている必要があります。 ただし、基本認証モデルは、ユーザー アカウントがデータベースなどの外部ソースに対して認証されるカスタム認証を許可するように構成できます。 詳細については、ASP.NET Web サイト[の「ASP.NET Web API の基本認証](https://www.asp.net/web-api/overview/security/basic-authentication)」を参照してください。

> [!NOTE]
> 基本認証は、ログアウトを管理するようには設計されていません。したがって、ログアウトの標準的な基本認証方法は、セッションを終了することです。

## <a name="related-links"></a>関連リンク

- [レストフル Web サービスを使用する](~/xamarin-forms/data-cloud/web-services/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
