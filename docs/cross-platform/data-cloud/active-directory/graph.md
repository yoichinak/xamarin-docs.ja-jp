---
title: Graph API へのアクセス
description: このドキュメントでは、Xamarin でビルドされたモバイル アプリケーションを Azure Active Directory 認証を追加する方法について説明します。
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c43dfa79831f22e55490b27c3c360602ae717627
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61190014"
---
# <a name="accessing-the-graph-api"></a>Graph API へのアクセス

Xamarin アプリケーション内から Graph API を使用してこれらの手順に従います。

1. [Azure Active Directory に登録](~/cross-platform/data-cloud/active-directory/get-started/register.md)上、 *windowsazure.com*しポータル
2. [サービスを構成する](~/cross-platform/data-cloud/active-directory/get-started/configure.md)します。

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>手順 3. Active Directory 認証をアプリに追加します。

アプリケーションへの参照を追加**Azure Active Directory Authentication Library (ADAL Azure)** for mac Visual Studio または Visual Studio で NuGet パッケージ マネージャーを使用する。
選択するかどうかを確認**プレリリース パッケージを表示する**をまだプレビュー段階であるため、このパッケージを含めます。

> [!IMPORTANT]
> メモ:Azure ADAL 3.0 は、現在、プレビューして、最終バージョンがリリースされる前に、重大な変更にすることがあります。 


![](graph-images/06.-adal-nuget-package.jpg "Azure Active Directory Authentication Library (ADAL Azure) への参照を追加します。")

アプリケーションではするようになりました、認証フローに必要な次のクラス レベル変数を追加する必要があります。

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

ここでは、注意が必要です`commonAuthority`します。 認証エンドポイントの場合は`common`、アプリが**マルチ テナント**、つまり、すべてのユーザーが Active Directory の資格情報でログインを使用できます。 認証後、そのユーザーは、独自の Active Directory のコンテキストで動作します-つまり、Active Directory に関連する詳細が表示されます。

### <a name="write-method-to-acquire-access-token"></a>アクセス トークンを取得するメソッドを作成します。

(Android) 用の次のコードは認証を開始および完了した場合の結果を割り当てる`authResult`します。 IOS および Windows Phone の実装が若干異なる: 2 番目のパラメーター (`Activity`) iOS に異なるし、存在しない Windows Phone で。

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

上記のコードで、 `AuthenticationContext` commonAuthority を使用して認証を担当します。 `AcquireTokenAsync`メソッドで、この場合、アクセスする必要があるリソースとしてパラメーターを受け取る`graphResourceUri`、 `clientId`、および`returnUri`します。 アプリに戻ります、`returnUri`認証が完了したとき。 このコードはすべてのプラットフォーム、ただし、最後のパラメーターでは、同じまま`AuthorizationParameters`認証フローを管理する責任を負いますが、さまざまなプラットフォームで異なります。

Android や iOS の場合は、伝え`this`パラメーターを`AuthorizationParameters(this)`として新しい Windows でパラメーターを指定しないで渡されますが、コンテキストを共有する`AuthorizationParameters()`します。

### <a name="handle-continuation-for-android"></a>Android 用の継続を処理します。

認証が完了した後、フローは、アプリに戻ります。 次のコードによって処理される Android の場合に追加する必要があります**MainActivity.cs**:


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);

    
}
```

### <a name="handle-continuation-for-windows-phone"></a>Windows Phone の継続を処理します。

Windows Phone の変更、`OnActivated`メソッドで、 **App.xaml.cs**ファイルと、次のコード。

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

今すぐアプリケーションを実行する場合は、認証のダイアログ ボックスが表示されます。
認証が成功すると (この例では Graph API) でリソースにアクセスするアクセス許可を求められます。

![](graph-images/08.-authentication-flow.jpg "認証が成功すると、ケースの Graph API のリソースにアクセスへのアクセス許可を問われます")

取得する必要がありますの認証が成功すると、リソースにアクセスするアプリを承認した場合、`AccessToken`と`RefreshToken`でコンボ`authResult`します。 これらのトークンは、バック グラウンドでの Azure Active Directory による承認の API 呼び出しではさらに必要です。

![](graph-images/07.-access-token-for-authentication.jpg "これらのトークンが API 呼び出しではさらに、バック グラウンドでの Azure Active Directory による承認必要です。")

たとえば、次のコードを使用すると、Active Directory からユーザーの一覧を取得できます。 Azure AD によって保護されている、Web API を使用した Web API の URL を置き換えることができます。

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

