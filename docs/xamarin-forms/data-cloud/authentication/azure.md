---
title: Azure Mobile Apps でのユーザーの認証
description: この記事では、Azure Mobile Apps を使用して、Xamarin.Forms アプリケーションの認証プロセスを管理する方法について説明します。
ms.prod: xamarin
ms.assetid: D50D6F56-8B19-44E7-81F3-E0E1C6E240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 428e536d6895ff16a928f8cc40a8a7976d087471
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61331325"
---
# <a name="authenticating-users-with-azure-mobile-apps"></a>Azure Mobile Apps でのユーザーの認証

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)

_Azure Mobile Apps の認証と承認など、Facebook、Google、Microsoft、Twitter、および Azure Active Directory のアプリケーションのユーザーをサポートするためにさまざまな外部 id プロバイダーを使用します。認証されたユーザーのみにアクセスを制限するテーブルのアクセス許可を設定できます。この記事では、Azure Mobile Apps を使用して、Xamarin.Forms アプリケーションの認証プロセスを管理する方法について説明します。_

## <a name="overview"></a>概要

Azure Mobile Apps の管理、Xamarin.Forms アプリケーションの認証プロセスのプロセスは次のとおりです。

1. Id プロバイダーのサイトには、Azure モバイル アプリを登録し、Mobile Apps バックエンドにプロバイダーによって生成された資格情報を設定します。 詳細については、次を参照してください。[認証用のアプリを登録し、アプリ サービスを構成する](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#register-your-app-for-authentication-and-configure-app-services)します。
1. これにより、認証プロセスが完了すると、Xamarin.Forms アプリケーションにリダイレクトすると認証システム、Xamarin.Forms アプリケーションの新しい URL スキームを定義します。 詳細については、次を参照してください。[アプリ、許可されている外部リダイレクト Url を追加する](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#redirecturl)します。
1. 認証済みクライアントのみに Azure Mobile Apps バックエンドへのアクセスを制限します。 詳細については、次を参照してください。[認証されたユーザーへのアクセス許可を制限](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#restrict-permissions-to-authenticated-users)します。
1. Xamarin.Forms アプリケーションからの認証を呼び出します。 詳細については、次を参照してください[ポータブル クラス ライブラリに追加の認証](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-portable-class-library)、 [iOS アプリに認証の追加](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-ios-app)、 [Android アプリに認証の追加](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-android-app)、および。[Windows 10 アプリ プロジェクトに認証を追加](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-windows-10-including-phone-app-projects)します。

> [!NOTE]
> iOS 9 以降では、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。   ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することができない場合の ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 これは、アプリの更新することで実現できます**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)します。

従来、モバイル アプリケーションでは、認証 id プロバイダーを実行するのに埋め込み web ビューを使用するがします。 次の理由でこれを不要になったお勧めします。

- Web ビューをホストするアプリケーションは、ユーザーの完全な認証資格情報、アプリケーションが想定されている承認付与だけでなくにアクセスできます。 可能性のあるアプリケーションの攻撃対象領域を増やすことで、必要以上のより強力な資格情報へのアクセスをアプリケーションから、最小限の特権の原則が違反しています。
- ホスト アプリケーションでしたユーザー名とパスワードをキャプチャ、自動的にフォームを送信およびユーザーの同意のバイパスしコピー セッションの cookie を使用してにユーザーとして認証済みの操作を実行します。
- 埋め込み web ビューでは、他のアプリケーションまたはデバイスの web ブラウザー、添字形式のユーザー エクスペリエンスと見なされます承認要求のたびにサインインするユーザーを必要とすると、認証状態を共有しないでください。
- いくつかの承認エンドポイントでは、検出し、web ビューから取得した承認要求をブロックする手順を実行します。

デバイスの web ブラウザーを使用して、認証は、最新のバージョンで採用を実行する場合は、 [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)します。 認証要求にデバイスのブラウザーを使用して、ユーザーは、アプリケーションのサインインと承認のフローのコンバージョン率の向上、デバイスごとに 1 回、id プロバイダーにサインインするだけが必要と、アプリケーションの使いやすさが向上します。 デバイスのブラウザーでは、検査して、web ビュー内のコンテンツがブラウザーに表示するコンテンツは変更できないのアプリケーションとセキュリティの強化も提供します。

## <a name="using-an-azure-mobile-apps-instance"></a>Azure Mobile Apps のインスタンスを使用

[Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)提供、`MobileServiceClient`クラスは、Xamarin.Forms アプリケーションで Azure Mobile Apps インスタンスにアクセスするために使用します。

サンプル アプリケーションは、Xamarin.Forms アプリケーションにログインするための Google アカウントを持つユーザーが id プロバイダーとして Google を使用します。 この記事では、id プロバイダーとして Google を使用すると、方法は他の id プロバイダーに等しく適用できます。

<a name="logging-in" />

### <a name="logging-in-users"></a>ユーザーのログイン

サンプル アプリケーションでは、ログイン画面は、次のスクリーン ショットに示されます。

![](azure-images/login.png "ログイン ページ")

Id プロバイダーとして Google を使用すると、他の id プロバイダーのさまざまな使用できます、Facebook、Microsoft、Twitter、および Azure Active Directory を含みます。

次のコード例では、ログイン プロセスを呼び出す方法を示します。

```csharp
async void OnLoginButtonClicked(object sender, EventArgs e)
{
  ...
  if (App.Authenticator != null)
  {
    authenticated = await App.Authenticator.AuthenticateAsync();
  }
  ...
}
```

`App.Authenticator`プロパティは、`IAuthenticate`各プラットフォーム固有プロジェクトで設定されているインスタンス。 `IAuthenticate`インターフェイスを指定します、`AuthenticateAsync`操作の各プラットフォーム プロジェクトで提供する必要があります。 そのため、呼び出し、`App.Authenticator.AuthenticateAsync`メソッドが実行される、`IAuthenticate.AuthenticateAsync`プラットフォーム プロジェクト内のメソッド。

すべてのプラットフォーム`IAuthenticate.AuthenticateAsync`メソッドを呼び出す、`MobileServiceClient.LoginAsync`ログイン インターフェイスとキャッシュのデータを表示するメソッド。 次のコード例は、 `LoginAsync` iOS プラットフォーム用のメソッド。

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    UIApplication.SharedApplication.KeyWindow.RootViewController,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

次のコード例は、 `LoginAsync` Android プラットフォーム用のメソッド。

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    this,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

次のコード例は、`LoginAsync`ユニバーサル Windows プラットフォーム用のメソッド。

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

すべてのプラットフォームで、`MobileServiceAuthenticationProvider`列挙体を使用して、認証プロセスで使用される id プロバイダーを指定します。 ときに、`MobileServiceClient.LoginAsync`メソッドが呼び出される、選択したプロバイダーのログイン ページを表示することで、id プロバイダーでログインが成功した後、認証トークンを生成することによって Azure Mobile Apps が認証フローを開始します。 `MobileServiceClient.LoginAsync`メソッドは、`MobileServiceClient.CurrentUser` プロパティに格納される `MobileServiceUser` インスタンスを返します このプロパティは、`UserId` と `MobileServiceAuthenticationToken` プロパティを提供します。 認証されたユーザーおよびユーザーの認証トークンを表します。 認証トークンは、Xamarin.Forms アプリケーションを認証されたユーザーのアクセス許可を必要とする Azure Mobile App インスタンスに対して操作を実行することにより、Azure Mobile Apps インスタンスに加えられたすべての要求に含まれます。

<a name="logging-out" />

### <a name="logging-out-users"></a>ユーザーをログアウトします。

次のコード例では、ログアウト プロセスを呼び出す方法を示します。

```csharp
async void OnLogoutButtonClicked(object sender, EventArgs e)
{
  bool loggedOut = false;

  if (App.Authenticator != null)
  {
    loggedOut = await App.Authenticator.LogoutAsync ();
  }
  ...
}
```

`App.Authenticator`プロパティは、`IAuthenticate`各 platformproject で設定されているインスタンス。 `IAuthenticate`インターフェイスを指定します、`LogoutAsync`操作の各プラットフォーム プロジェクトで提供する必要があります。 そのため、呼び出し、`App.Authenticator.LogoutAsync`メソッドが実行される、`IAuthenticate.LogoutAsync`プラットフォーム プロジェクト内のメソッド。

すべてのプラットフォーム`IAuthenticate.LogoutAsync`メソッドを呼び出す、`MobileServiceClient.LogoutAsync`メソッドを id プロバイダーでログインしているユーザーの認証を解除します。 次のコード例は、 `LogoutAsync` iOS プラットフォーム用のメソッド。

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  foreach (var cookie in NSHttpCookieStorage.SharedStorage.Cookies)
  {
    NSHttpCookieStorage.SharedStorage.DeleteCookie (cookie);
  }
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

次のコード例は、 `LogoutAsync` Android プラットフォーム用のメソッド。

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  CookieManager.Instance.RemoveAllCookie();
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

次のコード例は、`LogoutAsync`ユニバーサル Windows プラットフォーム用のメソッド。

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

ときに、`IAuthenticate.LogoutAsync`メソッドが呼び出される、id プロバイダーによって設定された cookie のクリアされますが、前に、 `MobileServiceClient.LogoutAsync` id プロバイダーでログインしているユーザーの認証を解除するメソッドが呼び出されます。

## <a name="summary"></a>まとめ

この記事では、Azure Mobile Apps を使用して、Xamarin.Forms アプリケーションの認証プロセスを管理する方法について説明します。 Azure Mobile Apps の認証と承認など、Facebook、Google、Microsoft、Twitter、および Azure Active Directory のアプリケーションのユーザーをサポートするためにさまざまな外部 id プロバイダーを使用します。 `MobileServiceClient`クラスは、Azure Mobile Apps のインスタンスへのアクセスを制御するため、Xamarin.Forms アプリケーションによって使用されます。


## <a name="related-links"></a>関連リンク

- [TodoAzureAuth (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)
- [Azure Mobile Apps の使](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Xamarin.Forms アプリに認証を追加します。](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
