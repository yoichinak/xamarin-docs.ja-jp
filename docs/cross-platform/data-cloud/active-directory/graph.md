---
title: Graph API へのアクセス
description: このドキュメントでは、Xamarin でビルドされたモバイルアプリケーションに Azure Active Directory 認証を追加する方法について説明します。
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 7ccfc082f86d0a0c6f8d29a477101edb72f9c92f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016628"
---
# <a name="accessing-the-graph-api"></a>Graph API へのアクセス

Xamarin アプリケーション内から Graph API を使用するには、次の手順に従います。

1. *Windowsazure.com* portal での Azure Active Directory への[登録](~/cross-platform/data-cloud/active-directory/get-started/register.md)、
2. [サービスを構成](~/cross-platform/data-cloud/active-directory/get-started/configure.md)します。

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>手順 3. アプリへの Active Directory 認証の追加

アプリケーションで、Visual Studio または Visual Studio for Mac の NuGet パッケージマネージャーを使用して**Azure Active Directory 認証ライブラリ (AZURE ADAL)** への参照を追加します。
まだプレビュー段階であるため、[このパッケージを含めるには**プレリリースパッケージを表示**する] を選択してください。

> [!IMPORTANT]
> 注: Azure ADAL 3.0 は現在プレビュー版であり、最終バージョンがリリースされる前に、互換性に影響する変更がある可能性があります。 

![](graph-images/06.-adal-nuget-package.jpg "Add a reference to Azure Active Directory Authentication Library (Azure ADAL)")

アプリケーションで、認証フローに必要な次のクラスレベル変数を追加する必要があります。

```csharp
//Client ID
public static string clientId = "25927d3c-.....-63f2304b90de";
public static string commonAuthority = "https://login.windows.net/common"
//Redirect URI
public static Uri returnUri = new Uri("http://xam-demo-redirect");
//Graph URI if you've given permission to Azure Active Directory
const string graphResourceUri = "https://graph.windows.net";
public static string graphApiVersion = "2013-11-08";
//AuthenticationResult will hold the result after authentication completes
AuthenticationResult authResult = null;
```

ここで注意すべき点は、`commonAuthority`です。 認証エンドポイントが `common`と、アプリは**マルチテナント**になります。つまり、すべてのユーザーが Active Directory の資格情報でログインを使用できます。 認証後、そのユーザーは自分の Active Directory のコンテキストで作業します。つまり、Active Directory に関連する詳細が表示されます。

### <a name="write-method-to-acquire-access-token"></a>アクセストークンを取得するための書き込みメソッド

次のコード (Android の場合) では、認証が開始され、完了時に `authResult`に結果が割り当てられます。 IOS と Windows Phone の実装は少し異なります。2番目のパラメーター (`Activity`) は、iOS では異なり、Windows Phone では存在しません。

```csharp
public static async Task<AuthenticationResult> GetAccessToken
            (string serviceResourceId, Activity activity)
{
    authContext = new AuthenticationContext(Authority);
    if (authContext.TokenCache.ReadItems().Count() > 0)
        authContext = new AuthenticationContext(authContext.TokenCache.ReadItems().First().Authority);
    var authResult = await authContext.AcquireTokenAsync(serviceResourceId, clientId, returnUri, new AuthorizationParameters(activity));
    return authResult;
}  
```

上記のコードでは、`AuthenticationContext` は commonAuthority による認証を担当します。 このメソッドには、アクセスする必要があるリソース (この場合 `graphResourceUri`、`clientId`、および `returnUri`) としてパラメーターを受け取る `AcquireTokenAsync` メソッドがあります。 認証が完了すると、アプリは `returnUri` に戻ります。 このコードはすべてのプラットフォームで同じままですが、最後のパラメーターである `AuthorizationParameters`は、プラットフォームによって異なり、認証フローの管理を担当します。

Android または iOS の場合は、`this` パラメーターを `AuthorizationParameters(this)` に渡してコンテキストを共有します。一方、Windows では、新しい `AuthorizationParameters()`としてパラメーターを指定せずに渡されます。

### <a name="handle-continuation-for-android"></a>Android の継続を処理する

認証が完了すると、フローはアプリに戻る必要があります。 Android の場合、次のコードによって処理されます。これは**MainActivity.cs**に追加する必要があります。

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
```

### <a name="handle-continuation-for-windows-phone"></a>Windows Phone の継続の処理

Windows Phone、次のコードを使用して、 **App.xaml.cs**ファイルの `OnActivated` メソッドを変更します。

```csharp
protected override void OnActivated(IActivatedEventArgs args)
{
#if WINDOWS_PHONE_APP
  if (args is IWebAuthenticationBrokerContinuationEventArgs)
  {
     WebAuthenticationBrokerContinuationHelper.SetWebAuthenticationBrokerContinuationEventArgs(args as IWebAuthenticationBrokerContinuationEventArgs);
  }
#endif
  base.OnActivated(args);
}
```

アプリケーションを実行すると、認証ダイアログが表示されます。
認証が成功すると、リソースにアクセスするためのアクセス許可を要求します (この例では Graph API)。

![](graph-images/08.-authentication-flow.jpg "Upon successful authentication, it will ask your permissions to access the resources in our case Graph API")

認証が成功し、リソースへのアクセスをアプリに許可した場合は、`authResult`で `AccessToken` と `RefreshToken` コンボボックスが表示されます。 これらのトークンは、API 呼び出しをさらに追加するために必要であり、バックグラウンドで Azure Active Directory の承認に使用されます。

![](graph-images/07.-access-token-for-authentication.jpg "These tokens are   required for further API calls and for authorization with Azure Active Directory behind the scenes")

たとえば、次のコードを使用すると、Active Directory からユーザー一覧を取得できます。 Web API URL を Azure AD によって保護されている Web API に置き換えることができます。

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```
