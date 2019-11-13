---
title: Id プロバイダーを使用した AuthenticateUsers
description: この記事では、xamarin. Auth を使用して、Xamarin. フォームアプリケーションで認証プロセスを管理する方法について説明します。
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2019
ms.openlocfilehash: 83fbad8a9bbb9afef5ee80705fe9e86e51284e7d
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842979"
---
# <a name="authenticate-users-with-an-identity-provider"></a>Id プロバイダーを使用したユーザーの認証

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)

_Xamarin. Auth は、ユーザーを認証し、アカウントを格納するためのクロスプラットフォーム SDK です。これには、Google、Microsoft、Facebook、Twitter などの id プロバイダーの使用をサポートする OAuth 認証子が含まれています。この記事では、xamarin. Auth を使用して、Xamarin. フォームアプリケーションで認証プロセスを管理する方法について説明します。_

OAuth は認証用のオープン標準であり、リソース所有者は、リソース所有者の id を共有することなく、第三者にアクセス許可を付与する必要があることをリソースプロバイダーに通知できます。 たとえば、ユーザーが id プロバイダー (Google、Microsoft、Facebook、Twitter など) に対して、ユーザーの id を共有することなく、アプリケーションにアクセス許可を与える必要があることをユーザーが通知できるようにします。 これは、ユーザーが id プロバイダーを使用して web サイトやアプリケーションにサインインする方法としてよく使用されますが、web サイトやアプリケーションにパスワードを公開する必要はありません。

OAuth id プロバイダーを使用する場合の認証フローの概要を次に示します。

1. アプリケーションは、ブラウザーを id プロバイダーの URL に移動します。
1. Id プロバイダーは、ユーザー認証を処理し、アプリケーションに認証コードを返します。
1. アプリケーションは、id プロバイダーからアクセストークンの認証コードを交換します。
1. アプリケーションは、アクセストークンを使用して、id プロバイダーの Api (基本的なユーザーデータを要求する API など) にアクセスします。

サンプルアプリケーションは、Xamarin. Auth を使用して、Google に対してネイティブ認証フローを実装する方法を示しています。 このトピックでは、id プロバイダーとして Google が使用されていますが、この方法は他の id プロバイダーにも同様に適用されます。 Google の OAuth 2.0 エンドポイントを使用した認証の詳細については、「OAuth 2.0 を使用した google の web サイトでの[Google Api へのアクセス](https://developers.google.com/identity/protocols/OAuth2)」を参照してください。

> [!NOTE]
> IOS 9 以降では、アプリトランスポートセキュリティ (ATS) によって、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続が適用されるため、機密情報が誤って開示されるのを防ぐことができます。 IOS 9 用に構築されたアプリでは、ATS が既定で有効になっているため、すべての接続は、ATS のセキュリティ要件の対象となります。 接続がこれらの要件を満たしていない場合、例外が発生して失敗します。
> `HTTPS` プロトコルを使用できず、インターネットリソースに対してセキュリティで保護された通信を行うことができない場合は、ATS をオプトアウトできます。 これは、アプリの**情報**ファイルを更新することで実現できます。 詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)」を参照してください。

## <a name="using-xamarinauth-to-authenticate-users"></a>Xamarin. Auth を使用してユーザーを認証する

Xamarin では、アプリケーションが id プロバイダーの承認エンドポイントと対話するための2つの方法をサポートしています。

1. 埋め込み web ビューを使用する。 これは一般的な方法ですが、次の理由から推奨されなくなりました。

    - Web ビューをホストするアプリケーションは、アプリケーション用の OAuth 承認付与だけでなく、ユーザーの完全な認証資格情報にアクセスできます。 これは最小限の特権の原則に違反するため、アプリケーションは必要以上に強力な資格情報にアクセスできるため、アプリケーションの攻撃対象領域が増加する可能性があります。
    - ホストアプリケーションは、ユーザー名とパスワードをキャプチャし、フォームを自動的に送信してユーザーの同意を回避し、セッション cookie をコピーして、認証されたアクションをユーザーとして実行します。
    - 埋め込み web ビューでは、認証状態が他のアプリケーションやデバイスの web ブラウザーと共有されないため、ユーザーは、ユーザーエクスペリエンスの低下と見なされる承認要求ごとにサインインする必要があります。
    - 一部の承認エンドポイントは、web ビューからの承認要求を検出してブロックするための手順を実行します。

1. デバイスの web ブラウザーを使用します。これは推奨される方法です。 OAuth 要求にデバイスブラウザーを使用すると、アプリケーションの使いやすさが向上します。これは、ユーザーがデバイスごとに1回だけ id プロバイダーにサインインする必要があるためです。これにより、アプリケーションでのサインインと承認フローの変換率が向上します。 また、アプリケーションは web ビューでコンテンツを検査および変更することはできますが、ブラウザーに表示されないコンテンツに対しては、セキュリティが強化されます。 これは、この記事とサンプルアプリケーションで採用されている方法です。

サンプルアプリケーションでユーザーを認証し、基本データを取得する方法の概要を次の図に示します。

![](oauth-images/google-auth.png "Using Xamarin.Auth to Authenticate with Google")

アプリケーションは、`OAuth2Authenticator` クラスを使用して Google に認証要求を行います。 認証応答が返されるのは、ユーザーがサインインページ (アクセストークンを含む) を通じて Google で正常に認証された後です。 次に、アプリケーションは、`OAuth2Request` クラスを使用して、要求に含まれるアクセストークンと共に、基本的なユーザーデータの Google に要求を行います。

### <a name="setup"></a>セットアップ

Google サインインを Xamarin. Forms アプリケーションと統合するには、Google API コンソールプロジェクトを作成する必要があります。 これは次のようにして実装します。

1. [GOOGLE API コンソール](https://console.developers.google.com)の web サイトにアクセスし、google アカウントの資格情報でサインインします。
1. [プロジェクト] ドロップダウンで、既存のプロジェクトを選択するか、新しいプロジェクトを作成します。
1. サイドバーの API Manager の下で、**資格情報** を選択し、 **OAuth 同意画面 タブ**を選択します。**電子メールアドレス**を選択し、**ユーザーに表示される製品名**を指定して、**保存** を押します。
1. **[資格情報]** タブで、 **[資格情報の作成]** ドロップダウンリストを選択し、 **[OAuth クライアント ID]** を選択します。
1. **[アプリケーションの種類]** で、モバイルアプリケーションを実行するプラットフォーム (**IOS**または**Android**) を選択します。
1. 必要な詳細を入力し、 **[作成]** ボタンを選択します。

> [!NOTE]
> クライアント ID を使用すると、アプリケーションは Google Api にアクセスできるようになり、モバイルアプリケーションの場合は1つのプラットフォームで一意になります。 そのため、Google サインインを使用するプラットフォームごとに**OAuth クライアント ID**を作成する必要があります。

これらの手順を実行した後、OAuth2 を使用して Google の認証フローを開始できます。

### <a name="creating-and-configuring-an-authenticator"></a>認証システムの作成と構成

Xamarin. Auth の `OAuth2Authenticator` クラスは、OAuth 認証フローの処理を担当します。 次のコード例は、デバイスの web ブラウザーを使用して認証を実行するときの `OAuth2Authenticator` クラスのインスタンス化を示しています。

```csharp
var authenticator = new OAuth2Authenticator(
    clientId,
    null,
    Constants.Scope,
    new Uri(Constants.AuthorizeUrl),
    new Uri(redirectUri),
    new Uri(Constants.AccessTokenUrl),
    null,
    true);
```

`OAuth2Authenticator` クラスには、次のようないくつかのパラメーターが必要です。

- **クライアント ID** –要求を行っているクライアントを識別し、 [Google API コンソール](https://console.developers.google.com)でプロジェクトから取得できます。
- **クライアントシークレット**–これは `null` または `string.Empty`である必要があります。
- **スコープ**–アプリケーションによって要求されている API アクセスを識別し、値はユーザーに表示される同意画面に通知します。 スコープの詳細については、Google の web サイトの「 [API 要求の承認](https://developers.google.com/+/web/api/rest/oauth)」を参照してください。
- **Url の承認**–認証コードが取得される url を指定します。
- **リダイレクト url** –応答が送信される url を指定します。 このパラメーターの値は、 [Google 開発者コンソール](https://console.developers.google.com/)のプロジェクトの **[資格情報]** タブに表示される値のいずれかと一致する必要があります。
- **AccessToken url** –認証コードの取得後にアクセストークンを要求するために使用される url を指定します。
- **GetUserNameAsync Func** –アカウントが正常に認証された後に、アカウントのユーザー名を非同期に取得するために使用される省略可能な `Func` です。
- **ネイティブ UI を使用**する–デバイスの web ブラウザーを使用して認証要求を実行するかどうかを示す `boolean` 値です。

### <a name="setup-authentication-event-handlers"></a>認証イベントハンドラーの設定

次のコード例に示すように、ユーザーインターフェイスを表示する前に、`OAuth2Authenticator.Completed` イベントのイベントハンドラーを登録する必要があります。

```csharp
authenticator.Completed += OnAuthCompleted;
```

このイベントは、ユーザーがサインインを正常に認証またはキャンセルしたときに発生します。

必要に応じて、`OAuth2Authenticator.Error` イベントのイベントハンドラーを登録することもできます。

### <a name="presenting-the-sign-in-user-interface"></a>サインインユーザーインターフェイスの表示

サインインユーザーインターフェイスは、各プラットフォームプロジェクトで初期化する必要がある Xamarin. Auth login プレゼンターを使用してユーザーに提示できます。 次のコード例は、iOS プロジェクトの `AppDelegate` クラスでログインプレゼンターを初期化する方法を示しています。

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

次のコード例は、Android プロジェクトの `MainActivity` クラスでログインプレゼンターを初期化する方法を示しています。

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

その後、.NET Standard ライブラリプロジェクトは、次のようにログインプレゼンターを呼び出すことができます。

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

`Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` メソッドの引数は、`OAuth2Authenticator` インスタンスであることに注意してください。 `Login` メソッドが呼び出されると、デバイスの web ブラウザーのタブでユーザーにサインインユーザーインターフェイスが表示されます。次のスクリーンショットを参照してください。

![](oauth-images/login.png "Google Sign-In")

### <a name="processing-the-redirect-url"></a>リダイレクト URL の処理

ユーザーが認証プロセスを完了すると、コントロールは [web ブラウザー] タブからアプリケーションに戻ります。これを実現するには、認証プロセスから返されるリダイレクト URL にカスタム URL スキームを登録し、送信されたカスタム URL を検出して処理します。

アプリケーションに関連付けるカスタム URL スキームを選択する場合、アプリケーションでは、コントロールの下の名前に基づいたスキームを使用する必要があります。 これは、iOS でバンドル識別子名と Android のパッケージ名を使用して、URL スキームを作成するために逆にすることで実現できます。 ただし、Google などの一部の id プロバイダーでは、ドメイン名に基づいてクライアント識別子が割り当てられ、その後、URL スキームとして使用されます。 たとえば、Google が `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`のクライアント id を作成した場合は、URL スキームが `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`されます。 スキームコンポーネントの後に使用できるのは、1つの `/` のみであることに注意してください。 そのため、カスタム URL スキームを使用するリダイレクト URL の完全な例は `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`です。

Web ブラウザーは、カスタム URL スキームを含む id プロバイダーからの応答を受信すると、URL の読み込みを試みますが、失敗します。 代わりに、イベントを発生させることによって、カスタム URL スキームがオペレーティングシステムに報告されます。 次に、オペレーティングシステムは、登録されているスキームを確認し、見つかった場合は、構成を登録したアプリケーションを起動して、リダイレクト URL を送信します。

カスタム URL スキームをオペレーティングシステムに登録し、スキームを処理するメカニズムは、各プラットフォームに固有です。

#### <a name="ios"></a>iOS

IOS では、次のスクリーンショットに示すように、カスタム URL スキームが**情報 plist**に登録されます。

![](oauth-images/info-plist.png "URL Scheme Registration")

**識別子**の値には任意の値を指定でき、**ロール**の値は **[ビューアー]** に設定する必要があります。 `com.googleusercontent.apps`で始まる**Url スキーム**の値は、 [Google API コンソール](https://console.developers.google.com)でプロジェクトの iOS クライアント id から取得できます。

Id プロバイダーが承認要求を完了すると、アプリケーションのリダイレクト URL にリダイレクトされます。 この URL はカスタムスキームを使用するので、iOS はアプリケーションを起動し、起動時のパラメーターとして URL を渡します。これは、次のコード例に示すように、アプリケーションの `AppDelegate` クラスの `OpenUrl` オーバーライドによって処理されます。:

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    // Convert NSUrl to Uri
    var uri = new Uri(url.AbsoluteString);

    // Load redirectUrl page
    AuthenticationState.Authenticator.OnPageLoading(uri);

    return true;
}
```

`OpenUrl` メソッドは、パブリック `OAuth2Authenticator` オブジェクトの `OnPageLoading` メソッドを使用してリダイレクト URL を処理する前に、受信した URL を `NSUrl` から .NET `Uri`に変換します。 これにより、Xamarin. Auth は web ブラウザーのタブを閉じ、受信した OAuth データを解析します。

#### <a name="android"></a>Android

Android では、スキームを処理する `Activity` で[`IntentFilter`](xref:Android.App.IntentFilterAttribute)属性を指定することによって、カスタム URL スキームが登録されます。 Id プロバイダーが承認要求を完了すると、アプリケーションのリダイレクト URL にリダイレクトされます。 URL はカスタムスキームを使用するため、Android はアプリケーションを起動します。これは、URL を起動パラメーターとして渡し、カスタム URL スキームを処理するために登録 `Activity` の `OnCreate` メソッドによって処理されます。 次のコード例は、カスタム URL スキームを処理するサンプルアプリケーションのクラスを示しています。

```csharp
[Activity(Label = "CustomUrlSchemeInterceptorActivity", NoHistory = true, LaunchMode = LaunchMode.SingleTop )]
[IntentFilter(
    new[] { Intent.ActionView },
    Categories = new [] { Intent.CategoryDefault, Intent.CategoryBrowsable },
    DataSchemes = new [] { "<insert custom URL here>" },
    DataPath = "/oauth2redirect")]
public class CustomUrlSchemeInterceptorActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        // Convert Android.Net.Url to Uri
        var uri = new Uri(Intent.Data.ToString());

        // Load redirectUrl page
        AuthenticationState.Authenticator.OnPageLoading(uri);

        Finish();
    }
}
```

[`IntentFilter`](xref:Android.App.IntentFilterAttribute)の `DataSchemes` プロパティは、 [Google API コンソール](https://console.developers.google.com)でプロジェクトの Android クライアント id から取得された反転クライアント識別子に設定する必要があります。

`OnCreate` メソッドは、パブリック `OAuth2Authenticator` オブジェクトの `OnPageLoading` メソッドを使用してリダイレクト URL を処理する前に、受信した URL を `Android.Net.Url` から .NET `Uri`に変換します。 これにより、Xamarin. Auth は web ブラウザーのタブを閉じ、受信した OAuth データを解析します。

> [!IMPORTANT]
> Android では、Xamarin. Auth は `CustomTabs` API を使用して、web ブラウザーとオペレーティングシステムと通信します。 ただし、`CustomTabs` 互換性のあるブラウザーがユーザーのデバイスにインストールされることは保証されていません。

### <a name="examining-the-oauth-response"></a>OAuth 応答を調べています

受信した OAuth データを解析すると、`OAuth2Authenticator.Completed` イベントが発生します。 このイベントのイベントハンドラーでは、次のコード例に示すように、`AuthenticatorCompletedEventArgs.IsAuthenticated` プロパティを使用して認証が成功したかどうかを識別できます。

```csharp
async void OnAuthCompleted(object sender, AuthenticatorCompletedEventArgs e)
{
  ...
  if (e.IsAuthenticated)
  {
    ...
  }
}
```

認証が成功したときに収集されたデータは、`AuthenticatorCompletedEventArgs.Account` プロパティで確認できます。 これには、id プロバイダーによって提供される API にデータの要求を署名するために使用できるアクセストークンが含まれます。

### <a name="making-requests-for-data"></a>データの要求を作成しています

アプリケーションは、アクセストークンを取得した後、そのトークンを使用して、id プロバイダーの基本的なユーザーデータを要求するために、`https://www.googleapis.com/oauth2/v2/userinfo` API への要求を行います。 この要求は、次のコード例に示すように、`OAuth2Authenticator` インスタンスから取得したアカウントを使用して認証された要求を表す、Xamarin. Auth の `OAuth2Request` クラスを使用して作成されます。

```csharp
// UserInfoUrl = https://www.googleapis.com/oauth2/v2/userinfo
var request = new OAuth2Request ("GET", new Uri (Constants.UserInfoUrl), null, e.Account);
var response = await request.GetResponseAsync ();
if (response != null)
{
  string userJson = response.GetResponseText ();
  var user = JsonConvert.DeserializeObject<User> (userJson);
}
```

`OAuth2Request` インスタンスでは、HTTP メソッドと API URL に加えて、`Constants.UserInfoUrl` プロパティによって指定された URL に要求を署名するアクセストークンを含む `Account` インスタンスも指定します。 次に、id プロバイダーは、アクセストークンが有効であると認識した場合、ユーザーの名前と電子メールアドレスを含む基本的なユーザーデータを JSON 応答として返します。 JSON 応答は、`user` 変数に読み取られ、逆シリアル化されます。

詳細については、Google 開発者ポータルでの[GOOGLE API の呼び出し](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi)に関する説明を参照してください。

### <a name="storing-and-retrieving-account-information-on-devices"></a>デバイスでのアカウント情報の保存と取得

Xamarin は、アプリケーションが常にユーザーを再認証する必要がないように、`Account` オブジェクトをアカウントストアに安全に格納します。 `AccountStore` クラスは、アカウント情報を格納する役割を担い、iOS のキーチェーンサービスと Android の `KeyStore` クラスによってサポートされます。

次のコード例は、`Account` オブジェクトが安全に保存される方法を示しています。

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

保存されたアカウントは、アカウントの `Username` プロパティとサービス ID で構成されるキーを使用して一意に識別されます。これは、アカウントストアからアカウントをフェッチするときに使用される文字列です。 `Account` が既に保存されている場合は、`Save` メソッドを再度呼び出すと、上書きされます。

サービスの `Account` オブジェクトは、次のコード例に示すように、`FindAccountsForService` メソッドを呼び出すことによって取得できます。

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

`FindAccountsForService` メソッドは `Account` オブジェクトの `IEnumerable` コレクションを返します。コレクション内の最初の項目は、一致したアカウントとして設定されます。

## <a name="troubleshooting"></a>トラブルシューティング

- Android で、認証後にブラウザーを閉じたときにトースト通知を受信し、トースト通知を停止する場合は、Xamarin. Auth を初期化した後、Android プロジェクトに次のコードを追加します。

```csharp
Xamarin.Auth.CustomTabsConfiguration.CustomTabsClosingMessage = null;
```

- Android では、ブラウザーが自動的に閉じない場合は、Xamarin. Auth パッケージをバージョン1.5.0.3 にダウングレードすることが一時的な回避策です。 次に、 [PCL Crypto v 2.0.147](https://www.nuget.org/packages/PCLCrypto/2.0.147)を Android プロジェクトに追加します。

## <a name="summary"></a>まとめ

この記事では、xamarin. Auth を使用して、Xamarin. フォームアプリケーションで認証プロセスを管理する方法について説明しました。 Xamarin. Auth には、Google、Microsoft、Facebook、Twitter などの id プロバイダーを使用するために Xamarin. Forms アプリケーションで使用される `OAuth2Authenticator` クラスと `OAuth2Request` クラスが用意されています。

## <a name="related-links"></a>関連リンク

- [Oauthのフロー (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)
- [ネイティブアプリの OAuth 2.0](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [OAuth 2.0 を使用した Google Api へのアクセス](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin. Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin. Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
