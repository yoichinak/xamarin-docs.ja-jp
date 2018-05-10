---
title: Graph API にアクセスします。
description: Active Directory を使用して、Xamarin を使用して Graph API を照会するには
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e177ac680a100a2723732c2ee7252ea0c16ea972
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="accessing-the-graph-api"></a>Graph API にアクセスします。

_Active Directory を使用して、Xamarin を使用して Graph API を照会するには_

Xamarin アプリケーション内での Graph API を使用してこれらの手順に従います。

1. [Azure Active Directory に登録する](~/cross-platform/data-cloud/active-directory/get-started/register.md)上、 *windowsazure.com*ポータルで、
2. [サービスを構成する](~/cross-platform/data-cloud/active-directory/get-started/configure.md)です。

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>手順 3. Active Directory 認証をアプリに追加します。

参照を追加、アプリケーションで**Azure Active Directory 認証ライブラリ (Azure ADAL)** for mac Visual Studio または Visual Studio で NuGet パッケージ マネージャーを使用します。
選択するかどうかを確認**プレリリースのパッケージを表示**プレビュー段階では、このパッケージを含める。

> [!IMPORTANT]
> 注: Azure ADAL 3.0 は、現在プレビューおよび最終バージョンがリリースされる前に、重大な変更にすることがあります。 


![](graph-images/06.-adal-nuget-package.jpg "Azure Active Directory Authentication Library (ADAL Azure) への参照を追加します。")

アプリケーションでするが必要になります、認証フローに必要な次のクラスのレベル変数を追加します。

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

1 つの点に注意`commonAuthority`です。 認証エンドポイントは、いつ`common`、アプリが**マルチ テナント**、Active Directory 資格情報でログインを使用できますので、すべてのユーザー。 認証後、そのユーザーは、Active Directory のコンテキストで動作します-つまり、Active Directory に関連する詳細が表示されます。

### <a name="write-method-to-acquire-access-token"></a>アクセス トークンを取得するメソッドを作成します。

(Android) 用の次のコードは認証を開始および完了時に結果を割り当てる`authResult`です。 IOS と Windows Phone の実装が若干異なります: 2 番目のパラメーター (`Activity`) が iOS で異なると、不在がち、Windows Phone でします。

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

上記のコードで、 `AuthenticationContext` commonAuthority を使用して認証を担当します。 `AcquireTokenAsync`メソッドで、ここでは、アクセスする必要があるリソースとしてパラメーターを受け取る`graphResourceUri`、 `clientId`、および`returnUri`です。 アプリに戻ります、`returnUri`認証が完了するとします。 このコードはすべてのプラットフォーム、ただし、最後のパラメーターを同じになります`AuthorizationParameters`はさまざまなプラットフォームで、認証フローを制御するを担当します。

Android や iOS の場合は、渡す`this`パラメーターを`AuthorizationParameters(this)`として新しいウィンドウでパラメーターを指定しないで渡されますが、コンテキストを共有する`AuthorizationParameters()`です。

### <a name="handle-continuation-for-android"></a>Android 用の継続を処理します。

認証が完了したら、フローは、アプリに戻る必要があります。 次のコードによって処理される Android の場合に追加する必要があります**MainActivity.cs**:


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);

    
}
```

### <a name="handle-continuation-for-windows-phone"></a>Windows Phone の継続を処理します。

Windows Phone の変更、`OnActivated`メソッドで、 **App.xaml.cs**ファイルと、コードの下。

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

今すぐアプリケーションを実行すると、認証ダイアログが表示されます。
認証が成功すると (この例では Graph API) のリソースにアクセスするアクセス許可を求められます。

![](graph-images/08.-authentication-flow.jpg "認証が成功すると場合、Graph API のリソースにアクセスするアクセス許可を求められます")

認証に成功して、アプリのリソースにアクセスする権限を与えて、取得する必要があります、`AccessToken`と`RefreshToken`でコンボ`authResult`です。 これらのトークンは、API 呼び出しではさらに、バック グラウンドで Azure Active Directory による承認に必要があります。

![](graph-images/07.-access-token-for-authentication.jpg "これらのトークンが必要なは、API 呼び出しではさらに、バック グラウンドで Azure Active Directory による承認です。")

たとえば、次のコードには、Active Directory からユーザーの一覧を取得することができます。 Azure AD によって保護されている、Web API では、Web API URL を置き換えることができます。

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

