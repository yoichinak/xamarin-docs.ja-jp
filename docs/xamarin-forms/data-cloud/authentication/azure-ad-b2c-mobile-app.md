---
title: Azure Active Directory B2C を Azure Mobile Apps に統合する
description: Azure Active Directory B2C は、コンシューマー向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。 この記事では、Xamarin.Forms で Azure Active Directory B2C を使って、認証および承認を Azure Mobile Apps インスタンスに提供する方法を示します。
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2019
ms.openlocfilehash: 2f9cfceee4765dea946dfdac974ac2d56595ef94
ms.sourcegitcommit: 9aaae4f0af096cd136b3dcfbb9af591ba307dc25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/26/2019
ms.locfileid: "67398692"
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Azure Active Directory B2C を Azure Mobile Apps に統合する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)

_Azure Active Directory B2C はコンシューマー向けアプリケーション用のクラウド id 管理を提供します。この記事では、Azure Active Directory B2C を使用して、Xamarin.Forms を使用して Azure Mobile Apps インスタンスに id 管理を統合する方法について説明します。_

## <a name="overview"></a>概要

Azure Mobile Apps を使用すると、Azure App Service でホストされているスケーラブルなバックエンドを使用してアプリケーションを開発できます。 モバイル認証、オフラインの同期、プッシュがサポートしている通知します。 Azure Mobile Apps の詳細については、[Azure Mobile App の使用](~/xamarin-forms/data-cloud/consuming/azure.md)、および[Azure Mobile Apps のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure.md) を参照してください。

Azure Active Directory B2C はコンシューマーを許可するコンシューマー向けアプリケーションの id 管理サービスを示します。

- 既存のソーシャル アカウント (Microsoft、Google、Facebook、Amazon、LinkedIn) でサインインします。
- 新しい資格情報 (電子メール アドレスとパスワード、またはユーザー名とパスワード) を作成します。 これらの資格情報は *ローカル*アカウント といわれます。

Azure Active Directory B2C の詳細については、[Azure Active Directory B2C のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md) を参照してください。

Azure Active Directory B2C は、Azure Mobile App の認証ワークフローの管理に使用できます。 この方法では、クラウド id 管理を構成し、により、アプリケーション コードを変更することがなく変更。


Azure Active Directory B2C での azure の Mobile Apps は、2 つの認証方法を使用できます。

- [クライアントによって管理される](#client-managed-authentication)– Xamarin.Forms モバイル アプリケーションは、Azure Active Directory B2C テナントでは、認証プロセスを開始し、Azure Mobile Apps インスタンスに受信した認証トークンを渡します。
- [サーバーによって管理される](#server-managed-authentication)– Azure Mobile Apps のインスタンスは、Azure Active Directory B2C テナントを使用して web ベースのワークフローでの認証プロセスを開始します。

どちらの場合も、認証エクスペリエンスは Azure Active Directory B2C テナントによって提供されます。 サンプル アプリケーションで次のスクリーン ショットようサインイン画面になります。

![](azure-ad-b2c-mobile-app-images/screenshots.png "ログイン ページ")

この例はローカル アカウントでサインインが、Microsoft、Google、Facebook、およびその他の id プロバイダーを有効することもできます。

## <a name="setup"></a>セットアップ

両方のワークフローで、Azure Active Directory B2C テナントを Azure Mobile Apps と統合するため、最初のプロセスのとおりです。

1. Azure portal で Azure Mobile Apps インスタンスを作成します。 これには、サンプル プロジェクトのような開始プロジェクトが生成されます。 詳細については、[Azure Mobile App の使用](~/xamarin-forms/data-cloud/consuming/azure.md) を参照してください。
1. Azure Active Directory B2C テナントを作成します。 詳細については、[Azure Active Directory B2C のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md) を参照してください。
1. Azure Mobile Apps インスタンスおよび Xamarin.Forms アプリケーションでの認証を有効にします。 詳細については、[Azure Mobile Apps のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure.md) を参照してください。

> [!NOTE]
> クライアント側管理認証ワークフローを使用する場合、Microsoft Authentication Library (MSAL) が必要です。 MSAL では、デバイスの web ブラウザーを使用して認証します。 認証が完了したら、ブラウザーで、ユーザーは、カスタムの URL スキームにリダイレクトされます。 アプリケーションでは、ユーザー エクスペリエンスの制御を取り戻すし、認証に応答することができます、カスタムの URL スキームを登録します。 MSAL を使用して、Azure Active Directory B2C テナントと通信する方法の詳細については、[Azure Active Directory B2C のユーザーの認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md) を参照してください。

## <a name="client-managed-authentication"></a>クライアント側管理認証

クライアント側管理認証では、Xamarin.Forms アプリケーションは、認証フローを開始する、Azure Active Directory B2C テナントを接続します。 後は、Azure Active Directory B2C テナントは、Azure Mobile Apps インスタンスに提供された id トークンを返します。 これにより、Xamarin.Forms アプリケーションを認証されたユーザーのアクセス許可を必要とするアクションを実行できます。

### <a name="azure-active-directory-b2c-client-managed-tenant-configuration"></a>Azure Active Directory B2C テナントのクライアント管理の構成

クライアント管理の認証ワークフローの場合は、Azure Active Directory B2C テナントを次のように構成する必要があります。

- 設定**ネイティブ クライアントを含める**"Yes"にします。
- カスタム リダイレクト URI を設定します。 MSAL のドキュメントでは、"msal"を使用して、アプリ ID と組み合わせるし、続くことをお勧め"://認証/"。 詳細については、次を参照してください。 [MSAL クライアント アプリケーションのリダイレクト URI](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Client-Applications#redirect-uri)します。

次のスクリーン ショットは、この構成を示しています。

[![「Azure Active Directory B2C の構成」](azure-ad-b2c-mobile-app-images/azure-redirect-uri-cropped.png)](azure-ad-b2c-mobile-app-images/azure-redirect-uri.png#lightbox "Azure Active Directory B2C の構成")

Azure Active Directory B2C テナントで使用されるサインイン ポリシーは、応答 URL が同じカスタムの URL スキームに設定されるようにも構成する必要があります。 選択してこれを実現する**ユーザー フローを実行する**ポリシーの設定にアクセスする Azure ポータルでします。 モバイル アプリの構成の必要になるように、この画面で見つかったメタデータ URL を保存します。 次のスクリーン ショットでは、この構成と、URL をコピーする必要がありますを示しています。

[![「Azure Active Directory B2C のポリシー」](azure-ad-b2c-mobile-app-images/client-flow-policies-cropped.png)](azure-ad-b2c-mobile-app-images/client-flow-policies.png#lightbox "Azure Active Directory B2C のポリシーの構成")

### <a name="azure-mobile-app-configuration"></a>Azure のモバイル アプリ構成

クライアント側管理認証ワークフローでは、Azure Mobile Apps のインスタンスを構成します。

- App Service の認証を有効にする必要があります。
- 要求が認証されていない場合に実行するアクションを設定する必要があります**Azure Active Directory でサインイン**します。

次のスクリーン ショットは、この構成を示しています。

[![「Azure Mobile Apps の認証の構成」](azure-ad-b2c-mobile-app-images/ama-config-cropped.png)](azure-ad-b2c-mobile-app-images/ama-config.png#lightbox "Azure Mobile Apps の認証の構成")

Azure Active Directory B2C テナントとの通信に Azure Mobile Apps のインスタンスを構成します。

- Azure Active Directory の構成をクリックし、有効にする**詳細**Azure Active Directory 認証プロバイダーのモード。
- 設定**クライアント ID**を**アプリケーション ID**の Azure Active Directory B2C テナント。
- 設定、**発行者の Url**テナントの構成時に以前にコピーしたポリシーのメタデータの URL にします。

次のスクリーン ショットは、この構成を示しています。

![[Azure Mobile Apps の詳細構成]](azure-ad-b2c-mobile-app-images/ama-advanced-config.png)

### <a name="signing-in"></a>サインイン

次のコード例では、クライアント側管理認証ワークフローを開始するための重要なメソッド呼び出しを示します。

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...

    authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        string.Empty,
        UIBehavior.SelectAccount,
        string.Empty,
        App.UiParent);

    ...

    var payload = new JObject();
    if (authenticationResult != null && !string.IsNullOrWhiteSpace(authenticationResult.AccessToken))
    {
        payload["access_token"] = authenticationResult.AccessToken;
    }

    User = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        payload);
    success = true;

    ...
}
```

Microsoft Authentication Library (MSAL) は、Azure Active Directory B2C テナントの認証ワークフローの開始に使用されます。 `AcquireTokenAsync`メソッドは、デバイスの web ブラウザーを起動し、`Constants.AuthoritySignin` 定数を通じて参照されたポリシーによって指定されている Azure Active Directory B2C ポリシーで定義済みの認証オプションを表示します。 このポリシーは、ユーザーのサインインとサインアップのエクスペリエンスと認証が成功すると、アプリケーションが受信要求を定義します。

`AcquireTokenAsync` メソッドの呼び出しの結果は、`AuthenticationResult` インスタンスを返します。 認証が成功した場合、`AuthenticationResult`インスタンスがローカルにキャッシュされているアクセス トークンを格納します。 認証が失敗した場合、`AuthenticationResult` インスタンスには、認証が失敗した理由を示すデータが含まれます。 MSAL を使用して、Azure Active Directory B2C テナントと通信する方法については、[Azure Active Directory B2C のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md) を参照してください。

ときに、`CurrentClient.LoginAsync`メソッドが呼び出される、Azure Mobile Apps のインスタンスでラップされたアクセス トークンを受け取る、`JObject`します。 Azure Mobile Apps のインスタンスが、独自の OAuth 2.0 認証フローを開始する必要があることを意味有効なトークンの存在します。 その代わりに、`CurrentClient.LoginAsync`メソッドは、`MobileServiceClient.CurrentUser` プロパティに格納されている`MobileServiceUser` インスタンスを返します。 このプロパティは、`UserId` と `MobileServiceAuthenticationToken` プロパティを提供します。 認証されたユーザーおよび有効期限が切れるまでに使用できるトークンを表します。 認証トークンは、Xamarin.Forms アプリケーションを認証されたユーザーのアクセス許可を必要とするアクションを実行することにより、Azure Mobile Apps インスタンスに加えられたすべての要求に含まれます。

### <a name="signing-out"></a>サインアウト

次のコード例は、クライアントで管理するサインアウト処理を呼び出す方法を示しています。

```csharp
public async Task<bool> LogoutAsync()
{
    ...

    IEnumerable<IAccount> accounts = await ADB2CClient.GetAccountsAsync();
    while (accounts.Any())
    {
        await ADB2CClient.RemoveAsync(accounts.First());
        accounts = await ADB2CClient.GetAccountsAsync();
    }
    User = null;

    ...
}
```

`CurrentClient.LogoutAsync` メソッドは、Azure Mobile Apps インスタンスを使ってユーザーの認証を解除し、MSAL によって作成されたローカル キャッシュからのすべての認証トークンをクリアします。

## <a name="server-managed-authentication"></a>サーバー側管理認証

サーバー管理の認証では、Xamarin.Forms アプリケーションは、Azure Mobile Apps インスタンスに接続し、Azure Active Directory B2C テナントを使用して、B2C ポリシーで定義されているサインイン ページを表示することによって、OAuth 2.0 認証フローを管理します。 次の成功のサインオンは、Azure Mobile Apps のインスタンスは、認証されたユーザーのアクセス許可が必要な操作を実行する Xamarin.Forms アプリケーションがトークンを返します。

### <a name="azure-active-directory-b2c-server-managed-tenant-configuration"></a>Azure Active Directory B2C テナントのサーバー管理の構成

サーバー管理の認証ワークフローの場合は、Azure Active Directory B2C テナントを次のように構成する必要があります。

- Web アプリ/Web API を含め、暗黙的フローを許可します。
- 応答 URL に Azure Mobile App のアドレスを設定し、末尾に `/.auth/login/aad/callback` を加えます。

次のスクリーン ショットは、この構成を示しています。

[![「Azure Active Directory B2C の構成」](azure-ad-b2c-mobile-app-images/server-flow-config-cropped.png)](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "Azure Active Directory B2C の構成")

Azure Active Directory B2C テナントで使用されるポリシーは、応答 url が構成されている必要があります。 これは後に、Azure モバイル アプリのアドレスへの応答 URL を設定して実現されます`/.auth/login/aad/callback`します。 モバイル アプリの構成の必要になるようこの画面の上部にあるメタデータの URL を保存することも必要があります。 次のスクリーン ショットでは、この構成とメタデータ URL を保存する必要がありますを示しています。

![[Azure Active Directory B2C のポリシーの構成]](azure-ad-b2c-mobile-app-images/server-flow-policies.png)

### <a name="azure-mobile-apps-instance-configuration"></a>Azure Mobile Apps インスタンスの構成

サーバー管理の認証ワークフローの場合は、Azure Mobile Apps インスタンスは次のように構成する必要があります。

- App Service の認証を有効にする必要があります。
- 要求が認証されていない場合に実行するアクションを設定する必要があります**Azure Active Directory でサインイン**します。

次のスクリーン ショットは、この構成を示しています。

[![「Azure Mobile Apps の認証の構成」](azure-ad-b2c-mobile-app-images/ama-config-cropped.png)](azure-ad-b2c-mobile-app-images/ama-config.png#lightbox "Azure Mobile Apps の認証の構成")

Azure Mobile Apps のインスタンスは、Azure Active Directory B2C テナントとの通信にも構成する必要があります。

- Azure Active Directory の構成をクリックし、有効にする**詳細**Azure Active Directory 認証プロバイダーのモード。
- 設定**クライアント ID**を**アプリケーション ID**の Azure Active Directory B2C テナント。
- **発行者の Url**はテナントの構成時に以前にコピーしたポリシーのメタデータの URL です。

次のスクリーン ショットは、この構成を示しています。

![[Azure Mobile Apps の詳細構成]](azure-ad-b2c-mobile-app-images/ama-advanced-config.png)

### <a name="signing-in"></a>サインイン

次のコード例では、サーバー側管理認証ワークフローを開始する方法を示します。

```csharp
public async Task<bool> AuthenticateAsync()
{
    ...

    MobileServiceUser user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        Constants.URLScheme);

    ...
}
```

ときに、`CurrentClient.LoginAsync`メソッドが呼び出される、Azure Mobile Apps のインスタンスがリンクの Azure Active Directory B2C のポリシーは、OAuth 2.0 認証フローの開始を実行します。 各`AuthenticateAsync`メソッドは、プラットフォーム固有です。 ただし、各`AuthenticateAsync`メソッドの呼び出し、`CurrentClient.LoginAsync`メソッドと、Azure Active Directory テナントが、認証プロセスで使用されることを指定します。 詳細については、[ユーザー ログイン](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in) を参照してください。

`CurrentClient.LoginAsync`メソッドは、`CurrentClient.CurrentUser` プロパティに格納される `MobileServiceUser` インスタンスを返します このプロパティは、`UserId` と `MobileServiceAuthenticationToken` プロパティを提供します。 これらは、認証されたユーザーと有効期限が切れるまで使用できるユーザーの認証トークンを表します。 認証トークンは、Azure Mobile Apps インスタンスに向けて作られるすべての要求に含まれ、Xamarin.Forms アプリケーションが、認証されたユーザーのアクセス許可を必要とする Azure Mobile Apps インスタンス上の操作の実行を可能にします。

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

`MobileServiceClient.LogoutAsync`メソッドは、Azure Mobile Apps インスタンスを使ってユーザーの認証を解除します。 詳細については、[ユーザー ログイン](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out) を参照してください。


## <a name="related-links"></a>関連リンク

- [TodoAzureAuth ClientFlow (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [TodoAzureAuth ServerFlow (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [Azure Mobile Apps の使](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Mobile Apps を使ったユーザー認証](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure Active Directory B2C のユーザー認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Microsoft 認証ライブラリ nuget パッケージ](https://www.nuget.org/packages/Microsoft.Identity.Client)
- [Microsoft 認証ライブラリのドキュメント](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)
