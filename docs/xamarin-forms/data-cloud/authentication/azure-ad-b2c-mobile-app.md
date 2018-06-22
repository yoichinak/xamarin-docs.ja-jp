---
title: Azure Active Directory B2C を Azure Mobile Apps に統合する
description: Azure Active Directory B2C は、消費者向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。 この記事では、Xamarin.Forms で Azure Active Directory B2C を使って、認証および承認を Azure Mobile Apps インスタンスに提供する方法を示します。
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: cafc1e78779dc393fa0409daa08b3daa8948a1ee
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Azure Active Directory B2C を Azure Mobile Apps に統合する

_Azure Active Directory B2C は、コンシューマー向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。この記事では、Xamarin.Forms で Azure Active Directory B2C を使って、認証および承認を Azure Mobile Apps インスタンスに提供する方法を示します。_

![](~/media/shared/preview.png "この API は、現在プレリリースです。")

> [!NOTE]
> [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) は、まだプレビュー段階ですが、運用環境での使用に適しています。 ただし、ライブラリのAPIや内部のキャッシュ形式、およびその他のメカニズムに重大な変更の可能性があり、アプリケーションに多大な影響を与える可能性があります。

## <a name="overview"></a>概要

Azure Mobile Apps では、モバイルの認証、オフライン同期、およびプッシュ通知のサポートにより、Azure App Service でホストされているスケーラブルなバックエンドでアプリケーションを開発できます。 Azure Mobile Apps の詳細については、[Azure Mobile App の使用](~/xamarin-forms/data-cloud/consuming/azure.md)、および[Azure Mobile Apps のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure.md) を参照してください。

Azure Active Directory B2C はコンシューマー向けのアプリケーションのための id 管理サービスで、コンシューマーがアプリケーションに次のような方法でサインインできるようにします。

- 既存のソーシャル アカウント (Microsoft、Google、Facebook、Amazon、LinkedIn) を使用する。
- 新しい資格情報 (電子メール アドレスとパスワード、またはユーザー名とパスワード) を作成する。これらの資格情報は *ローカル*アカウント といわれます。

Azure Active Directory B2C の詳細については、[Azure Active Directory B2C のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md) を参照してください。

Azure Active Directory B2C は、Azure Mobile App の認証ワークフローの管理に使用できます。 この方法を使えば、id 管理エクスペリエンスを完全にクラウドで定義でき、モバイル アプリケーションのコードを変更することなく修正できます。

Azure Mobile Apps インスタンスと Azure Active Directory B2C テナントの統合に採用できる 2 つの認証ワークフローがあります。

- [クライアント管理](#client_managed)– この方法では、Xamarin.Forms モバイルアプリケーションは Azure Active Directory B2C テナントを使って認証処理を開始し、Azure Mobile Apps インスタンスに受信した認証トークンを渡します。
- [サーバー管理](#server_managed)– この方法では、Azure Mobile Apps インスタンスが Azure Active Directory B2C テナントを使用して、web ベースのワークフローによって、認証プロセスを開始します。

どちらの場合も、認証エクスペリエンスは Azure Active Directory B2C テナントによって提供されます。 サンプル アプリケーションでは、次のスクリーン ショットに示すような [サインイン] 画面になります。

![](azure-ad-b2c-mobile-app-images/screenshots.png "ログイン ページ")

ソーシャル id プロバイダーまたはローカル アカウントでのサインインが使用できます。このサンプルでは Microsoft、Facebook、Google をソーシャル id プロバイダーとして使用していますが、他の id プロバイダーを使うこともできます。

## <a name="setup"></a>セットアップ

使用する認証ワークフローに関係なく、Azure Mobile Apps インスタンスと Azure Active Directory B2C テナントを統合するための初期化処理は次のとおりです。

1. Azure Mobile Apps インスタンスを作成します。 詳細については、[Azure Mobile App の使用](~/xamarin-forms/data-cloud/consuming/azure.md) を参照してください。
2. Azure Mobile Apps インスタンスおよび Xamarin.Forms アプリケーションでの認証を有効にします。 詳細については、[Azure Mobile Apps のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure.md) を参照してください。
3. Azure Active Directory B2C テナントを作成します。 詳細については、[Azure Active Directory B2C のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md) を参照してください。

クライアント管理の認証ワークフローを使用したときに Microsoft Authentication Library (MSAL) が必要であることを注意してください。 MSAL では、デバイスの web ブラウザーを使用して、認証を行います。 これにより、ユーザーは各デバイスにつき1回のサインインで済むようになるので、アプリケーションのユーザビリティが向上します。またアプリケーションのサインインと認証フローのコンバージョンレートが向上します。 デバイスのブラウザーでは、強化されたセキュリティも提供します。 ユーザーが認証プロセスを完了させると、web ブラウザーのタブからアプリケーションに制御が返されます。これは、認証プロセスから返されるリダイレクト URL にカスタム URL スキームを登録することによって実現されます。そして、その送信されたカスタム URL を検知して処理します。 MSAL を使用して、Azure Active Directory B2C テナントと通信する方法の詳細については、[Azure Active Directory B2C のユーザーの認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md) を参照してください。

<a name="client_managed" />

## <a name="client-managed-authentication"></a>クライアントで管理する認証

クライアント管理の認証では、Xamarin.Forms モバイル アプリケーションは、Azure Active Directory B2C テナントに接続して、認証フローを開始します。 サインオンが成功すると、Azure Active Directory B2C テナントは、Azure Mobile Apps インスタンスにサインインする時に提供する id トークンを返します。 これにより、Xamarin.Forms アプリケーションは、認証されたユーザーのアクセス許可を必要とする Azure Mobile Apps インスタンスでの操作を実行できます。

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Azure Active Directory B2C テナントの構成

クライアント管理の認証ワークフローの場合は、Azure Active Directory B2C テナントを次のように構成する必要があります。

- ネイティブ クライアントを含めます。
- カスタムのリダイレクト URI にモバイル アプリケーションを一意に識別する URL スキームを設定します。後ろには `://auth/` を記述します。 カスタム URL スキームの選択の詳細については、[ネイティブ アプリのリダイレクト URI を選択する](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri) を参照してください。

次のスクリーン ショットは、この構成を示しています。

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Azure Active Directory B2C 構成")](azure-ad-b2c-mobile-app-images/client-flow-config.png#lightbox "Azure Active Directory B2C の構成")

Azure Active Directory B2C テナントで使用するポリシーも、応答 URL に 後ろに`://auth/` が続く同じカスタム URL スキームが設定されるように構成する必要があります。次のスクリーン ショットは、この構成を示しています。

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Azure Active Directory B2C のポリシー")

### <a name="azure-mobile-app-configuration"></a>Azure Mobile App の構成

クライアント管理の認証ワークフローの場合は、Azure Mobile Apps インスタンスは次のように構成する必要があります。

- App Service の認証を有効にする必要があります。
- 要求が認証されない場合に実行するアクションを **Azure Active Directory でのログイン** に設定する必要があります。

次のスクリーン ショットは、この構成を示しています。

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Azure Mobile Apps の構成")

Azure Mobile Apps インスタンスは、Azure Active Directory B2C テナントに接続するための構成も必要です。 この構成は、Azure Active Directory の認証プロバイダーの **詳細**モード を有効にして、**クライアント ID** に Azure のActive Directory B2C テナントの **アプリケーション ID** を、**発行者の URL** に Azure Active Directory B2C ポリシーのメタデータ エンドポイントを設定することによって完了できます。次のスクリーン ショットは、この構成を示しています。

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Azure Mobile Apps の詳細な構成")

### <a name="signing-in"></a>サインイン

次のコード例では、クライアント管理の認証ワークフローを開始する方法を示します。

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);

    ...
    var payload = new JObject();
    payload["access_token"] = authenticationResult.IdToken;

    User = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        payload);
    ...
}
```

Microsoft Authentication Library (MSAL) を使用して、Azure Active Directory B2C テナントとの認証ワークフローを開始します。 `AcquireTokenAsync`メソッドは、デバイスの web ブラウザーを起動し、`Constants.Authority` 定数を通じて参照されたポリシーによって指定されている Azure Active Directory B2C ポリシーで定義済みの認証オプションを表示します。このポリシーは、サインアップやサインイン中にコンシューマーが通過するエクスペリエンスやアプリケーションがサインアップまたはサインインに成功したときに受信する要求が定義されています。

`AcquireTokenAsync` メソッドの呼び出しの結果は、`AuthenticationResult` インスタンスを返します。認証が成功した場合、`AuthenticationResult` インスタンスには ID トークンが含まれ、それはローカルにキャッシュされます。認証が失敗した場合、`AuthenticationResult` インスタンスには、認証が失敗した理由を示すデータが含まれます。 MSAL を使用して、Azure Active Directory B2C テナントと通信する方法については、[Azure Active Directory B2C のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md) を参照してください。

`MobileServiceClient.LoginAsync`メソッドが呼び出されると、Azure Mobile Apps インスタンスは、`JObject` でラップされた id トークンを受信します。有効なトークンが存在することは、Azure Mobile Apps インスタンスが、独自の OAuth 2.0 認証フローを開始する必要がないことを意味します。 その代わりに、`MobileServiceClient.LoginAsync`メソッドは、`MobileServiceClient.CurrentUser` プロパティに格納されている`MobileServiceUser` インスタンスを返します。このプロパティは `UserId` と `MobileServiceAuthenticationToken` プロパティを提供します。 これらは、認証されたユーザーと有効期限が切れるまで使用できるユーザーの認証トークンを表します。 認証トークンは、Azure Mobile Apps インスタンスに向けて作られるすべての要求に含まれ、Xamarin.Forms アプリケーションが、認証されたユーザーのアクセス許可を必要とする Azure Mobile Apps インスタンス上の操作の実行を可能にします。

### <a name="signing-out"></a>サインアウト

次のコード例は、クライアントで管理するサインアウト処理を呼び出す方法を示しています。

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();

    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

`MobileServiceClient.LogoutAsync` メソッドは、Azure Mobile Apps インスタンスを使ってユーザーの認証を解除し、MSAL によって作成されたローカル キャッシュからのすべての認証トークンをクリアします。

<a name="server_managed" />

## <a name="server-managed-authentication"></a>サーバーで管理する認証

サーバー管理の認証では、Xamarin.Forms アプリケーションは、Azure Mobile Apps インスタンスに接続し、Azure Active Directory B2C テナントを使用して、B2C ポリシーで定義されているサインイン ページを表示することによって、OAuth 2.0 認証フローを管理します。サインオンが成功すると、Azure Mobile Apps インスタンスは、Xamarin.Forms アプリケーションが、認証されたユーザーのアクセス許可を必要とする Azure Mobile Apps インスタンスで操作を実行できるようにするトークンを返します。

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Azure Active Directory B2C テナントの構成

サーバー管理の認証ワークフローの場合は、Azure Active Directory B2C テナントを次のように構成する必要があります。

- Web アプリ/Web API を含め、暗黙的フローを許可します。
- 応答 URL に Azure Mobile App のアドレスを設定し、末尾に `/.auth/login/aad/callback` を加えます。

次のスクリーン ショットは、この構成を示しています。

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Azure Active Directory B2C の構成")](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "Azure Active Directory B2C の構成")

Azure Active Directory B2C テナントが使用するポリシーも、応答 URL に Azure Mobile App のアドレスに `/.auth/login/aad/callback` を加えたものを設定するように構成する必要があります。次のスクリーン ショットは、この構成を示しています。

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Azure Active Directory B2C のポリシー")

### <a name="azure-mobile-apps-instance-configuration"></a>Azure Mobile Apps のインスタンス構成

サーバー管理の認証ワークフローの場合は、Azure Mobile Apps インスタンスは次のように構成する必要があります。

- App Service の認証を有効にする必要があります。
- 要求が認証されない場合に実行するアクションを **Azure Active Directory でのログイン** に設定する必要があります。

次のスクリーン ショットは、この構成を示しています。

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Azure Mobile Apps の構成")

Azure Mobile Apps インスタンスは、Azure Active Directory B2C テナントに接続するための構成も必要です。 この構成は、Azure Active Directory の認証プロバイダーの **詳細** モード を有効にして、**クライアント ID** に Azure のActive Directory B2C テナントの **アプリケーション ID** を、**発行者の URL** に Azure Active Directory B2C ポリシーのメタデータ エンドポイントを設定することによって完了できます。次のスクリーン ショットは、この構成を示しています。

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Azure Mobile Apps の詳細な構成")

### <a name="signing-in"></a>サインイン

次のコード例では、サーバー管理の認証ワークフローを開始する方法を示します。

```csharp
public async Task<bool> AuthenticateAsync()
{
    ...
    MobileServiceUser user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        UIApplication.SharedApplication.KeyWindow.RootViewController,
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        Constants.URLScheme);
    ...
}
```

`MobileServiceClient.LoginAsync` メソッドが呼ばれると、Azure Mobile Apps インスタンスは、リンクされた Azure Active Directory B2C のポリシーを実行し、OAuth 2.0 認証フローを開始します。 各 `AuthenticateAsync` メソッドはプラットフォームに固有であることに注意してください。ただし、どちらの `AuthenticateAsync` メソッドも、`MobileServiceClient.LoginAsync` メソッドを使用して、Azure Active Directory テナントが、認証プロセスで使用されることを指定します。 詳細については、[ユーザー ログイン](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in) を参照してください。

`MobileServiceClient.LoginAsync`メソッドは、`MobileServiceClient.CurrentUser` プロパティに格納される `MobileServiceUser` インスタンスを返します。このプロパティは、`UserId` と `MobileServiceAuthenticationToken` プロパティを提供します。これらは、認証されたユーザーと有効期限が切れるまで使用できるユーザーの認証トークンを表します。認証トークンは、Azure Mobile Apps インスタンスに向けて作られるすべての要求に含まれ、Xamarin.Forms アプリケーションが、認証されたユーザーのアクセス許可を必要とする Azure Mobile Apps インスタンス上の操作の実行を可能にします。

### <a name="signing-out"></a>サインアウト

次のコード例は、サーバー管理のサインアウト処理を呼び出す方法を示しています。

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
    ...
}
```

`MobileServiceClient.LogoutAsync` メソッドは、Azure Mobile Apps インスタンスを使ってユーザーの認証を解除します。 詳細については、[ユーザー ログイン](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out) を参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms で Azure Active Directory B2C を使って、認証および承認を Azure Mobile Apps インスタンスに提供する方法を示しました。 Azure Active Directory B2C は、コンシューマー向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。


## <a name="related-links"></a>関連リンク

- [TodoAzureAuth ServerFlow (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Azure Mobile Apps の使用](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Mobile Apps を使ったユーザー認証](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure Active Directory B2C のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client)
