---
title: RESTful Web サービスを認証する
description: 基本認証では、適切な資格情報を持つクライアントのみにリソースへのアクセスを提供します。 この記事では、基本認証を使用して RESTful web サービスのリソースへのアクセスを保護する方法について説明します。
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
ms.openlocfilehash: 5a0e820c8a9f04b7ad9173893852285d53dbe7a6
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529206"
---
# <a name="authenticate-a-restful-web-service"></a>RESTful Web サービスを認証する

_HTTP では、リソースへのアクセスを制御するいくつかの認証メカニズムの使用をサポートします。基本認証では、適切な資格情報を持つクライアントのみにリソースへのアクセスを提供します。この記事では、基本認証を使用して RESTful web サービスのリソースへのアクセスを保護する方法を示します。_

> [!NOTE]
> iOS 9 以降では、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することができない場合の ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 これは、アプリの更新することで実現できます**Info.plist**ファイル。 詳細については、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)を参照してください。

## <a name="authenticating-users-over-http"></a>HTTP 経由でユーザーの認証

基本認証では、HTTP でサポートされる最も単純な認証メカニズムし、暗号化されていない base64 でエンコードされたテキストとしてユーザー名とパスワードを送信するクライアントが含まれます。 次のように動作します。

- Web サービスは、保護されたリソースの要求を受信した場合は HTTP 状態コード 401 (アクセスが拒否されました) で要求を拒否し、次の図に示すように、Www-authenticate 応答ヘッダーを設定します。

![](rest-images/basic-authentication-fail.png "基本認証の失敗")

- Web サービスがで保護されたリソースの要求を受信するかどうか、`Authorization`ヘッダーが正しく設定で、web サービスが応答 HTTP 状態コード 200、要求が成功したことを示すと、要求された情報が、応答であります。 このシナリオは、次の図に示されます。

![](rest-images/basic-authentication-success.png "基本認証の成功")

> [!NOTE]
> 基本認証は、HTTPS 接続経由でのみ使用する必要があります。 HTTP 接続経由で使用すると、`Authorization`ヘッダーは、HTTP トラフィックが、攻撃者によってキャプチャされた場合に簡単にデコードできます。

## <a name="specifying-basic-authentication-in-a-web-request"></a>Web 要求で基本認証を指定します。

基本認証の使用はよう指定します。

1. "Basic"が追加する文字列、`Authorization`要求のヘッダー。
1. ユーザー名とパスワードは、組み合わせてを文字列に「ユーザー名: パスワード」、base64 でエンコードされてに追加では、その形式、`Authorization`要求のヘッダー。

そのため、'XamarinUser' のユーザー名と 'XamarinPassword' のパスワード、ヘッダーになります。

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

`HttpClient`クラスを設定できる、`Authorization`でヘッダー値、`HttpClient.DefaultRequestHeaders.Authorization`プロパティ。 `HttpClient`インスタンスが複数の要求が存在する、`Authorization`ときではなく、1 回設定する場合にのみ必要なヘッダーに次のコード例で示すように、すべての要求を行います。

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

要求に署名する web サービス操作に要求が行われたときにその後、`Authorization`ユーザーが操作を呼び出すアクセス許可を持つかどうかを示すヘッダー。

> [!NOTE]
> このコードでは、資格情報は定数として格納されますが、公開されたアプリケーションで安全ではない形式で格納することはできません。 [Xamarith.Auth](https://www.nuget.org/packages/Xamarin.Auth/) NuGet が資格情報を安全に格納するための機能を提供します。 詳細については、[の格納とデバイスのアカウント情報の取得](~/xamarin-forms/data-cloud/authentication/oauth.md)を参照してください。

## <a name="processing-the-authorization-header-server-side"></a>承認ヘッダーのサーバー側の処理

REST サービスは、各アクションを`[BasicAuthentication]`属性で装飾する必要があります。 この属性は、 `Authorization`ヘッダーを解析し、base64 でエンコードされた資格情報が web.config に格納されている値と比較することによって有効かどうかを判断するために使用されます。この方法はサンプルサービスに適していますが、公開されている web サービスの拡張が必要です。

IIS によって使用される基本的な認証モジュールでは、ユーザーが自分の Windows 資格情報に対して認証されます。 そのため、ユーザーは、サーバーのドメインにアカウントが必要です。 ただし、ユーザー アカウントがデータベースなどの外部ソースに対して認証されて、カスタム認証を許可する、基本認証モデルを構成できます。 詳細については、次を参照してください。 [ASP.NET Web API で基本認証](http://www.asp.net/web-api/overview/security/basic-authentication)、ASP.NET web サイト。

> [!NOTE]
> 基本認証では、ログアウト管理設計されていません。そのため、ログアウト、基本認証の標準的なアプローチは、セッションを終了です。

## <a name="related-links"></a>関連リンク

- [RESTful web サービスを使用する](~/xamarin-forms/data-cloud/web-services/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
