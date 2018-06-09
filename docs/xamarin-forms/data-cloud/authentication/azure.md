---
title: Azure のモバイル アプリを使用してユーザーの認証
description: この記事では、Azure Mobile Apps を使用して、Xamarin.Forms アプリケーションでの認証プロセスを管理する方法について説明します。
ms.prod: xamarin
ms.assetid: D50D6F56-8B19-44E7-81F3-E0E1C6E240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: fc6206a22d7527ea38a39ab034c424bfe7730abb
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241716"
---
# <a name="authenticating-users-with-azure-mobile-apps"></a>Azure のモバイル アプリを使用してユーザーの認証

_Azure のモバイル アプリでは、認証および承認する Facebook、Google、Microsoft、Twitter、および Azure Active Directory を含む、アプリケーションのユーザーをサポートするために、さまざまな外部の id プロバイダーを使用します。認証されたユーザーのみにアクセスを制限するテーブルに対するアクセス許可を設定できます。この記事では、Azure Mobile Apps を使用して、Xamarin.Forms アプリケーションでの認証プロセスを管理する方法について説明します。_

## <a name="overview"></a>概要

Azure Mobile Apps が Xamarin.Forms アプリケーションでの認証プロセスを管理することにより、プロセスは次のとおりです。

1. Id プロバイダーのサイトで、Azure のモバイル アプリを登録し、モバイル アプリ バックエンドで、プロバイダーによって生成された資格情報を設定します。 詳細については、次を参照してください。[認証用のアプリを登録し、アプリのサービスを構成](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#register-your-app-for-authentication-and-configure-app-services)です。
1. により、認証システムは、認証プロセスが完了した後に、Xamarin.Forms アプリケーションにリダイレクトする、Xamarin.Forms アプリケーションの新しい URL スキームを定義します。 詳細については、次を参照してください。 [、許可されている外部リダイレクト Url に、アプリの追加](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#redirecturl)です。
1. 認証されたクライアントのみに Azure Mobile Apps バック エンドへのアクセスを制限します。 詳細については、次を参照してください。[認証されたユーザーへのアクセス許可を制限する](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#restrict-permissions-to-authenticated-users)です。
1. Xamarin.Forms アプリケーションからの認証を呼び出します。 詳細については、次を参照してください[ポータブル クラス ライブラリに追加の認証](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-portable-class-library)、 [iOS アプリケーションへの追加認証](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-ios-app)、 [Android のアプリへの追加認証](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-android-app)、および。[Windows 10 アプリのプロジェクトに認証を追加](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-windows-10-including-phone-app-projects)です。

> [!NOTE]
> iOS 9 以降では、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。  ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することがない場合のうち ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 アプリケーションを更新することによってこれを行う**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)です。

従来は、モバイル アプリケーションでは、id プロバイダーで認証を実行するのに組み込み web ビューが使用されます。 これは推奨されなくなりました、次の理由。

- Web ビューをホストするアプリケーションは、意図的に、アプリケーションの承認の付与だけでなく、ユーザーの完全な認証資格情報にアクセスできます。 アプリケーションには必要な可能性のあるアプリケーションの攻撃対象領域を増やすことよりも強力な資格情報へのアクセスがあるために、最低限の特権の原則が違反しています。
- ホスト アプリケーションでしたユーザー名とパスワードをキャプチャ、自動的にフォームを送信バイパス ユーザーの同意を求める、およびセッション cookie をコピーし、それらユーザーとして認証されている操作を実行します。
- 埋め込みの web ビューは、他のアプリケーションまたはデバイスの web ブラウザー、下位のユーザー エクスペリエンスと考えられる承認要求ごとにサインインする必要はありませんと認証の状態を共有しません。
- いくつかの認証エンドポイントは、検出し、web ビューから入手した承認要求をブロックする手順を実行します。

代替手段は、デバイスの web ブラウザーを使用して、最新のバージョンで採用されているアプローチは、認証を実行するには、 [Azure Mobile クライアント SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)です。 認証要求に、デバイスのブラウザーを使用して、アプリケーションのサインインと承認のフローの換算率を向上させる、デバイスごとに 1 回、id プロバイダーにサインインするだけでユーザーと、アプリケーションの使いやすさが向上します。 デバイスのブラウザーでは、アプリケーションは、検査およびコンテンツ、web ビューでもない、ブラウザーに表示を変更できるように強化されたセキュリティも提供します。

## <a name="using-an-azure-mobile-apps-instance"></a>Azure のモバイル アプリのインスタンスを使用

[Azure Mobile クライアント SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)提供、`MobileServiceClient`クラスは、Xamarin.Forms のアプリケーションで Azure Mobile Apps インスタンスにアクセスするために使用します。

サンプル アプリケーションは、Xamarin.Forms アプリケーションにログインするための Google アカウントを持つユーザーが id プロバイダーとして Google を使用します。 この記事で id プロバイダーとして Google を使用すると、アプローチが他の id プロバイダーにも適用されます。

<a name="logging-in" />

### <a name="logging-in-users"></a>ユーザーのログイン

次のスクリーン ショットでは、サンプル アプリケーションでログイン画面が表示されます。

![](azure-images/login.png "ログイン ページ")

Id プロバイダーとして Google を使用すると、さまざまな他の id プロバイダーとして使用できます、Facebook、Microsoft、Twitter、Azure Active Directory などです。

次のコード例は、ログイン プロセスを呼び出す方法を示しています。

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

`App.Authenticator`プロパティは、`IAuthenticate`プラットフォーム固有の各プロジェクトで設定されているインスタンス。 `IAuthenticate`インターフェイスを指定します、`AuthenticateAsync`操作を各プラットフォームのプロジェクトによって提供される必要があります。 そのため、呼び出し、`App.Authenticator.AuthenticateAsync`メソッドの実行、`IAuthenticate.AuthenticateAsync`プラットフォームのプロジェクト内のメソッドです。

すべてのプラットフォームの`IAuthenticate.AuthenticateAsync`メソッドの呼び出し、`MobileServiceClient.LoginAsync`ログイン インターフェイスとキャッシュ データを表示するメソッド。 次のコード例は、 `LoginAsync` iOS プラットフォーム用のメソッド。

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

次のコード例は、 `LoginAsync` Android プラットフォームのメソッド。

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

すべてのプラットフォームで、`MobileServiceAuthenticationProvider`列挙体を使用して、認証プロセスで使用される id プロバイダーを指定します。 ときに、`MobileServiceClient.LoginAsync`メソッドが呼び出され、選択したプロバイダーのログイン ページを表示することによって、id プロバイダーに正常にログインした後、認証トークンを生成することによって、Azure Mobile Apps が認証フローを開始します。 `MobileServiceClient.LoginAsync`メソッドを返します、`MobileServiceUser`で格納されるインスタンス、`MobileServiceClient.CurrentUser`プロパティです。 このプロパティでは`UserId`と`MobileServiceAuthenticationToken`プロパティです。 これらは、認証されたユーザーとユーザーの認証トークンを表します。 認証トークンは、Xamarin.Forms アプリケーションを認証されたユーザーのアクセス許可を必要とするモバイル アプリの Azure インスタンスで操作を実行することにより、Azure Mobile Apps インスタンスに加えるすべての要求に含まれます。

<a name="logging-out" />

### <a name="logging-out-users"></a>ユーザーがログ記録

次のコード例は、ログアウト プロセスを呼び出す方法を示しています。

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

`App.Authenticator`プロパティは、`IAuthenticate`各 platformproject で設定されているインスタンス。 `IAuthenticate`インターフェイスを指定します、`LogoutAsync`操作を各プラットフォームのプロジェクトによって提供される必要があります。 そのため、呼び出し、`App.Authenticator.LogoutAsync`メソッドの実行、`IAuthenticate.LogoutAsync`プラットフォームのプロジェクト内のメソッドです。

すべてのプラットフォームの`IAuthenticate.LogoutAsync`メソッドの呼び出し、`MobileServiceClient.LogoutAsync`解除 id プロバイダーにログインしているユーザーを認証する方法です。 次のコード例は、 `LogoutAsync` iOS プラットフォーム用のメソッド。

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

次のコード例は、 `LogoutAsync` Android プラットフォームのメソッド。

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

ときに、`IAuthenticate.LogoutAsync`メソッドが呼び出され、前に、cookie id プロバイダーによって設定がクリアされた、`MobileServiceClient.LogoutAsync`除外ユーザーを認証のログイン id プロバイダーとメソッドが呼び出されます。

## <a name="summary"></a>まとめ

この記事では、Azure Mobile Apps を使用して、Xamarin.Forms アプリケーションでの認証プロセスを管理する方法について説明します。 Azure のモバイル アプリでは、認証および承認する Facebook、Google、Microsoft、Twitter、および Azure Active Directory を含む、アプリケーションのユーザーをサポートするために、さまざまな外部の id プロバイダーを使用します。 `MobileServiceClient`クラスは、Azure Mobile Apps インスタンスへのアクセスを制御する Xamarin.Forms アプリケーションによって使用されます。


## <a name="related-links"></a>関連リンク

- [TodoAzureAuth (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)
- [Azure のモバイル アプリの使用](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Xamarin.Forms アプリに認証を追加します。](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users/)
- [Azure のモバイル クライアント SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
