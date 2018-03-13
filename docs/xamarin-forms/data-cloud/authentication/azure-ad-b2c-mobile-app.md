---
title: "Azure のモバイル アプリを使用して Azure Active Directory B2C の統合"
description: "Azure Active Directory B2C は、消費者向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。 この記事では、Xamarin.Forms を使用した Azure Mobile Apps インスタンスに対する認証および承認を提供する Azure Active Directory B2C を使用する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: c28ddc09b07066de67f5c974cf5c2128726c6932
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Azure のモバイル アプリを使用して Azure Active Directory B2C の統合

_Azure Active Directory B2C は、消費者向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。この記事では、Xamarin.Forms を使用した Azure Mobile Apps インスタンスに対する認証および承認を提供する Azure Active Directory B2C を使用する方法を示します。_

![](~/media/shared/preview.png "この API は、現在のリリース前")

> [!NOTE]
> [認証ライブラリの Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client)は、プレビュー段階では、運用環境での使用に適しています。 ただし、ある可能性がある重大な変更、API、内部キャッシュ形式、およびその他のメカニズム、ライブラリでは、アプリケーションに影響を与える可能性のです。

## <a name="overview"></a>概要

Azure のモバイル アプリでは、モバイルの認証、オフライン sync、およびプッシュ通知のサポートにより、Azure App Service でホストされているスケーラブルなバックエンドでアプリケーションを開発できます。 Azure Mobile Apps の詳細については、次を参照してください。 [Azure Mobile App を消費して](~/xamarin-forms/data-cloud/consuming/azure.md)、および[Azure Mobile Apps にユーザーの認証](~/xamarin-forms/data-cloud/authentication/azure.md)です。

Azure Active Directory B2C は、id 管理サービス コンシューマー向けアプリケーションでは、コンシューマーによってアプリケーションにサインインできるようにします。

- 既存のソーシャル アカウント (Microsoft、Google、Facebook、Amazon、LinkedIn) を使用しています。
- 新しい資格情報 (電子メール アドレスとパスワード、またはユーザー名とパスワード) を作成します。 これらの資格情報をいいます*ローカル*アカウント。

Azure Active Directory B2C の詳細については、次を参照してください。 [Azure Active Directory B2C のユーザーの認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)です。

Azure Active Directory B2C は、Azure のモバイル アプリの認証ワークフローの管理に使用できます。 この方法は、id 管理エクスペリエンスを完全にクラウドでは、定義し、モバイル アプリケーションのコードを変更することがなく変更できます。

Azure Mobile Apps インスタンスと Azure Active Directory B2C テナントの統合に採用できる 2 つの認証ワークフローがあります。

- [クライアントで管理された](#client_managed)– このアプローチを Azure Active Directory B2C テナントの認証処理 Xamarin.Forms モバイル アプリケーションを開始し、Azure Mobile Apps インスタンスに受信した認証トークンを渡します。
- [サーバーで管理された](#server_managed)– Azure Mobile Apps この方法でインスタンスが web ベースのワークフローによって、認証プロセスを開始する Azure Active Directory B2C テナントを使用します。

どちらの場合は、認証エクスペリエンスは Azure Active Directory B2C テナントによって提供されます。 サンプル アプリケーションでは、[サインイン] 画面で、次のスクリーン ショットに示すようにこの結果します。

![](azure-ad-b2c-mobile-app-images/screenshots.png "ログイン ページ")

サインインのソーシャル id プロバイダーまたはローカル アカウントを使用して、許可されます。 Microsoft、Facebook、Google は、この例でのソーシャル id プロバイダーとして使用されますが、他の id プロバイダーこともできます。

## <a name="setup"></a>セットアップ

使用される認証ワークフローに関係なく、Azure Mobile Apps インスタンスと Azure Active Directory B2C テナントを統合するための初期のプロセスのとおりです。

1. Azure Mobile Apps インスタンスを作成します。 詳細については、次を参照してください。 [Azure Mobile App を消費して](~/xamarin-forms/data-cloud/consuming/azure.md)です。
1. Azure Mobile Apps インスタンスおよび Xamarin.Forms アプリケーションでの認証を有効にします。 詳細については、次を参照してください。 [Azure Mobile Apps にユーザーの認証](~/xamarin-forms/data-cloud/authentication/azure.md)です。
1. Azure Active Directory B2C テナントを作成します。 詳細については、次を参照してください。 [Azure Active Directory B2C のユーザーの認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)です。

認証のクライアント管理ワークフローを使用したときに Microsoft の認証ライブラリ (MSAL) が必要であることを注意してください。 MSAL では、デバイスの web ブラウザーを使用して、認証を行います。 サインインしたら、各デバイスにアプリケーションのフローでのサインインと承認の換算率を向上するだけでユーザーと、アプリケーションの使いやすさが向上します。 デバイスのブラウザーでは、強化されたセキュリティも提供します。 ユーザーには、認証プロセスが完了すると後 web ブラウザーのタブからコントロールは、アプリケーションに返します。これは、リダイレクト URL、認証プロセスを検出して送信されると、カスタムの URL を処理から返されるため、カスタムの URL スキームを登録することによって実現されます。 MSAL を使用して、Azure Active Directory B2C テナントと通信する方法の詳細については、次を参照してください。 [Azure Active Directory B2C のユーザーの認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)です。

<a name="client_managed" />

## <a name="client-managed-authentication"></a>クライアントで管理された認証

認証のクライアント管理では、Xamarin.Forms のモバイル アプリケーションは、認証フローを開始する、Azure Active Directory B2C テナントを接続します。 成功したサインオンで Azure Active Directory B2C の後に、テナントは、Azure Mobile Apps インスタンスへのサインイン時に、提供されている id トークンを返します。 これにより、Xamarin.Forms アプリケーションを認証されたユーザーのアクセス許可を必要とする Azure Mobile Apps インスタンスで操作を実行できます。

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Azure Active Directory B2C テナントの構成

認証のクライアント管理ワークフローの場合は、Azure Active Directory B2C テナントを次のよう構成する必要があります。

- ネイティブ クライアントが含まれます。
- 続けて、モバイル アプリケーションを一意に識別する URL スキームにカスタムのリダイレクト URI を設定`://auth/`です。 カスタム URL スキームの選択の詳細については、次を参照してください。[ネイティブ アプリのリダイレクト URI を選択する](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri)です。

次のスクリーン ショットは、この構成を示しています。

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Azure Active Directory B2C 構成")](azure-ad-b2c-mobile-app-images/client-flow-config.png#lightbox "Azure Active Directory B2C の構成")

Azure Active Directory B2C テナントは、応答 URL が同じカスタム URL スキームに設定できるようにも構成するのに使用するポリシーが続く`://auth/`です。 次のスクリーン ショットは、この構成を示しています。

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Azure Active Directory B2C のポリシー")

### <a name="azure-mobile-app-configuration"></a>Azure のモバイル アプリの構成

認証のクライアント管理ワークフローの場合は、Azure Mobile Apps インスタンス必要がありますように構成します。

- アプリ サービスの認証を有効にする必要があります。
- 要求が認証されていないときに実行するアクションを設定する必要があります**Azure Active Directory でのログイン**です。

次のスクリーン ショットは、この構成を示しています。

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Azure のモバイル アプリ構成")

Azure Mobile Apps インスタンスは、Azure Active Directory B2C テナントとの通信にも構成する必要があります。 これには、有効にすると**詳細**、Azure Active Directory の認証プロバイダーのモードで、**クライアント ID**されている、**アプリケーション ID** Azure のActive Directory B2C テナント、および**発行者 Url** Azure Active Directory B2C ポリシーのメタデータ エンドポイントであります。 次のスクリーン ショットは、この構成を示しています。

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Azure のモバイル アプリ構成の詳細")

### <a name="signing-in"></a>サインイン

次のコード例では、認証のクライアント管理ワークフローを開始する方法を示します。

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

Microsoft 認証ライブラリ (MSAL) を使用して、Azure Active Directory B2C テナントとの認証ワークフローを開始します。 `AcquireTokenAsync`メソッドは、デバイスの web ブラウザーを起動し、を通じて参照ポリシーで指定されている Azure Active Directory B2C ポリシーで定義されている認証オプションを表示、`Constants.Authority`定数。 このポリシーは、サインアップ中に、サインイン、コンシューマーを通過するエクスペリエンスと、アプリケーションでサインアップまたはサインインに成功受信クレームを定義します。

結果、`AcquireTokenAsync`メソッドの呼び出し、`AuthenticationResult`インスタンス。 認証が成功した場合、`AuthenticationResult`インスタンスは、ローカルにキャッシュされている id トークンが含まれます。 認証が成功した場合、`AuthenticationResult`インスタンスは、認証が失敗した理由を示すデータが含まれます。 MSAL を使用して、Azure Active Directory B2C テナントと通信する方法については、次を参照してください。 [Azure Active Directory B2C のユーザーの認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)です。

ときに、`MobileServiceClient.LoginAsync`メソッドが呼び出され、Azure Mobile Apps インスタンスにラップ id トークンを受信する、`JObject`です。 Azure Mobile Apps インスタンスが、独自の OAuth 2.0 認証フローを開始する必要があることを意味有効なトークンが存在します。 代わりに、`MobileServiceClient.LoginAsync`メソッドを返します、`MobileServiceUser`で格納されるインスタンス、`MobileServiceClient.CurrentUser`プロパティです。 このプロパティでは`UserId`と`MobileServiceAuthenticationToken`プロパティです。 これらは、認証されたユーザーと有効期限が切れるまでに使用できるユーザーの認証トークンを表します。 認証トークンは、Xamarin.Forms アプリケーションを認証されたユーザーのアクセス許可を必要とする Azure Mobile Apps インスタンスに対して操作を実行することにより、Azure Mobile Apps インスタンスに加えるすべての要求に含まれます。

### <a name="signing-out"></a>サインアウト

次のコード例は、クライアントで管理されたサインアウト プロセスを呼び出す方法を示しています。

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

`MobileServiceClient.LogoutAsync`メソッドが逆に、Azure Mobile Apps インスタンスにユーザーを認証し、MSAL によって作成されたローカル キャッシュからのすべての認証トークンのクリアし、します。

<a name="server_managed" />

## <a name="server-managed-authentication"></a>サーバーで管理された認証

サーバー管理の認証では、Xamarin.Forms アプリケーションは B2C ポリシーで定義されている、サインイン ページを表示することによって、OAuth 2.0 認証フローを管理する Azure Active Directory B2C テナントを使用して、Azure Mobile Apps インスタンスを接続します。 次の成功のサインオンは、Azure Mobile Apps インスタンスは、Xamarin.Forms アプリケーションを認証されたユーザーのアクセス許可を必要とする Azure Mobile Apps インスタンスで操作を実行できるようにするトークンを返します。

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Azure Active Directory B2C テナントの構成

サーバーで管理された認証ワークフローの場合は、Azure Active Directory B2C テナントを次のよう構成する必要があります。

- Web アプリまたは web API を含めるし、暗黙のフローを許可します。
- Azure のモバイル アプリのアドレスへの応答 URL を設定、その後で`/.auth/login/aad/callback`です。

次のスクリーン ショットは、この構成を示しています。

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Azure Active Directory B2C 構成")](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "Azure Active Directory B2C の構成")

Azure Active Directory B2C テナントは、応答 URL は、Azure のモバイル アプリのアドレスに設定できるようにも構成するのに使用するポリシーが続く`/.auth/login/aad/callback`です。 次のスクリーン ショットは、この構成を示しています。

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Azure Active Directory B2C のポリシー")

### <a name="azure-mobile-apps-instance-configuration"></a>Azure のモバイル アプリのインスタンスの構成

サーバーで管理された認証ワークフローの場合は、Azure Mobile Apps インスタンス必要がありますように構成します。

- アプリ サービスの認証を有効にする必要があります。
- 要求が認証されていないときに実行するアクションを設定する必要があります**Azure Active Directory でのログイン**です。

次のスクリーン ショットは、この構成を示しています。

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Azure のモバイル アプリ構成")

Azure Mobile Apps インスタンスは、Azure Active Directory B2C テナントとの通信にも構成する必要があります。 これには、有効にすると**詳細**、Azure Active Directory の認証プロバイダーのモードで、**クライアント ID**されている、**アプリケーション ID** Azure のActive Directory B2C テナント、および**発行者 Url** Azure Active Directory B2C ポリシーのメタデータ エンドポイントであります。 次のスクリーン ショットは、この構成を示しています。

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Azure のモバイル アプリ構成の詳細")

### <a name="signing-in"></a>サインイン

次のコード例では、認証のサーバーで管理されたワークフローを開始する方法を示します。

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

ときに、`MobileServiceClient.LoginAsync`メソッドが呼び出され、Azure Mobile Apps インスタンス実行リンクの Azure Active Directory B2C のポリシーは、OAuth 2.0 認証フローを開始します。 `AuthenticateAsync`メソッドはプラットフォームに固有です。 ただし、各`AuthenticateAsync`メソッドを使用、`MobileServiceClient.LoginAsync`メソッドし、Azure Active Directory テナントが、認証プロセスで使用されることを指定します。 詳細については、次を参照してください。[ユーザー ログイン](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in)です。

`MobileServiceClient.LoginAsync`メソッドを返します、`MobileServiceUser`で格納されるインスタンス、`MobileServiceClient.CurrentUser`プロパティです。 このプロパティでは`UserId`と`MobileServiceAuthenticationToken`プロパティです。 これらは、認証されたユーザーと有効期限が切れるまでに使用できるユーザーの認証トークンを表します。 認証トークンは、Xamarin.Forms アプリケーションを認証されたユーザーのアクセス許可を必要とする Azure Mobile Apps インスタンスに対して操作を実行することにより、Azure Mobile Apps インスタンスに加えるすべての要求に含まれます。

### <a name="signing-out"></a>サインアウト

次のコード例は、サーバー管理のサインアウト プロセスを呼び出す方法を示しています。

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
    ...
}
```

`MobileServiceClient.LogoutAsync`メソッド解除、Azure Mobile Apps インスタンスを持つユーザーを認証します。 詳細については、次を参照してください。 [ユーザーのログイン](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out)です。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms を使用した Azure Mobile Apps インスタンスに対する認証および承認を提供する Azure Active Directory B2C を使用する方法を示しました。 Azure Active Directory B2C は、消費者向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。


## <a name="related-links"></a>関連リンク

- [TodoAzureAuth ServerFlow (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Azure のモバイル アプリの使用](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure のモバイル アプリを使用してユーザーの認証](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure Active Directory B2C のユーザーの認証](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Microsoft 認証ライブラリ](https://www.nuget.org/packages/Microsoft.Identity.Client)
