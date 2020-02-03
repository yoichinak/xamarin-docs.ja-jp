---
title: Id プロバイダーを使用した AuthenticateUsers
description: この記事では、Xamarin.Auth を使用して、Xamarin.Forms アプリケーションの認証プロセスを管理する方法について説明します。
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2019
ms.openlocfilehash: 0fa433de7fd1acb6fb27741f1615a644315f373f
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725560"
---
# <a name="authenticate-users-with-an-identity-provider"></a>Id プロバイダーを使用したユーザーの認証

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)

_Xamarin. Auth は、ユーザーを認証し、アカウントを格納するためのクロスプラットフォーム SDK です。これには、Google、Microsoft、Facebook、Twitter などの id プロバイダーの使用をサポートする OAuth 認証子が含まれています。この記事では、xamarin. Auth を使用して、Xamarin. フォームアプリケーションで認証プロセスを管理する方法について説明します。_

OAuth は、認証のためのオープン標準ででき、サード パーティのリソース所有者の id を共有することがなく、情報にアクセスする権限を付与する必要がある、リソース プロバイダーに通知するリソースの所有者。 この例は、ユーザーの id を共有することがなく、データにアクセスするアプリケーションに権限を付与する必要がある、id プロバイダー (Google、Microsoft、Facebook、Twitter など) に通知するユーザー有効化します。 サインインでき、パスワード、web サイトまたはアプリケーションを公開することがなくが、web サイトおよびアプリケーションを id プロバイダーを使用するユーザーのための方法としてよく使用されます。

OAuth id プロバイダーを使用するときに、認証フローの概要は次のとおりです。

1. アプリケーションでは、id プロバイダーの URL をブラウザーが移動します。
1. Id プロバイダーでは、ユーザー認証を処理して、アプリケーションに承認コードを返します。
1. アプリケーションでは、id プロバイダーからアクセス トークンの承認コードを交換します。
1. アプリケーションでは、アクセス トークンを使用して、基本的なユーザー データを要求するための API など、id プロバイダーの Api にアクセスします。

サンプル アプリケーションでは、Xamarin.Auth を使用して Google に対してネイティブ認証フローを実装する方法を示します。 このトピックでは、id プロバイダーとして Google を使用すると、方法は他の id プロバイダーに等しく適用できます。 Google の OAuth 2.0 エンドポイントを使用した認証の詳細については、「OAuth 2.0 を使用した google の web サイトでの[Google Api へのアクセス](https://developers.google.com/identity/protocols/OAuth2)」を参照してください。

> [!NOTE]
> iOS 9 以降では、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> `HTTPS` プロトコルを使用できず、インターネットリソースに対してセキュリティで保護された通信を行うことができない場合は、ATS をオプトアウトできます。 これは、アプリの**情報**ファイルを更新することで実現できます。 詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)」を参照してください。

## <a name="using-xamarinauth-to-authenticate-users"></a>Xamarin.Auth を使用してユーザーを認証するには

Xamarin.Auth には、id プロバイダーの承認エンドポイントと対話するアプリケーションの 2 つの方法がサポートされています。

1. 埋め込みの webview を使用します。 一般的な方法と、この中に、次の理由では推奨されなく。

    - Web ビューをホストするアプリケーションは、アプリケーションのためのものが OAuth 承認付与だけでなく、ユーザーの完全な認証資格情報にアクセスできます。 可能性のあるアプリケーションの攻撃対象領域を増やすことで、必要以上のより強力な資格情報へのアクセスをアプリケーションから、最小限の特権の原則が違反しています。
    - ホスト アプリケーションでしたユーザー名とパスワードをキャプチャ、自動的にフォームを送信およびユーザーの同意のバイパスしコピー セッションの cookie を使用してにユーザーとして認証済みの操作を実行します。
    - 埋め込み web ビューでは、他のアプリケーションまたはデバイスの web ブラウザー、添字形式のユーザー エクスペリエンスと見なされます承認要求のたびにサインインするユーザーを必要とすると、認証状態を共有しないでください。
    - いくつかの承認エンドポイントでは、検出し、web ビューから取得した承認要求をブロックする手順を実行します。

1. 推奨されるアプローチですが、デバイスの web ブラウザーを使用します。 OAuth 要求にデバイスのブラウザーを使用して、ユーザーは、アプリケーションのサインインと承認のフローのコンバージョン率の向上、デバイスごとに 1 回、id プロバイダーにサインインするだけが必要と、アプリケーションの使いやすさが向上します。 デバイスのブラウザーでは、検査して、web ビュー内のコンテンツがブラウザーに表示するコンテンツは変更できないのアプリケーションとセキュリティの強化も提供します。 これは、この記事とサンプル アプリケーションで使用される手法です。

サンプル アプリケーションが Xamarin.Auth を使用してユーザーを認証し、その基本的なデータを取得する方法の概要については、次の図に示されます。

![](oauth-images/google-auth.png "Using Xamarin.Auth to Authenticate with Google")

アプリケーションは、`OAuth2Authenticator` クラスを使用して Google に認証要求を行います。 後のサインイン ページで、アクセス トークンを含む、ユーザーが正常に認証で Google 認証の応答が返されます。 次に、アプリケーションは、`OAuth2Request` クラスを使用して、要求に含まれるアクセストークンと共に、基本的なユーザーデータの Google に要求を行います。

### <a name="setup"></a>セットアップ

Xamarin.Forms アプリケーションで Google サインインを統合する Google API コンソール プロジェクトを作成する必要があります。 これは次のようにして実装します。

1. [GOOGLE API コンソール](https://console.developers.google.com)の web サイトにアクセスし、google アカウントの資格情報でサインインします。
1. プロジェクト ドロップダウン リストから、既存のプロジェクトを選択するか、新しく作成します。
1. サイドバーの API Manager の下で、**資格情報** を選択し、 **OAuth 同意画面 タブ**を選択します。**電子メールアドレス**を選択し、**ユーザーに表示される製品名**を指定して、**保存** を押します。
1. **[資格情報]** タブで、 **[資格情報の作成]** ドロップダウンリストを選択し、 **[OAuth クライアント ID]** を選択します。
1. **[アプリケーションの種類]** で、モバイルアプリケーションを実行するプラットフォーム (**IOS**または**Android**) を選択します。
1. 必要な詳細を入力し、 **[作成]** ボタンを選択します。

> [!NOTE]
> クライアント ID は、により、アプリケーションは有効になっている Google Api へのアクセスし、モバイル アプリケーションの 1 つのプラットフォームに固有です。 そのため、Google サインインを使用するプラットフォームごとに**OAuth クライアント ID**を作成する必要があります。

次の手順を実行した後で Google の OAuth2 認証フローを開始する Xamarin.Auth を使用できます。

### <a name="creating-and-configuring-an-authenticator"></a>作成と認証子の構成

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
- **スコープ**–アプリケーションによって要求されている API アクセスを識別し、値はユーザーに表示される同意画面に通知します。 スコープの詳細については、「Google の web サイトで[要求を承認](https://developers.google.com/docs/api/how-tos/authorizing)する」を参照してください。
- **Url の承認**–認証コードが取得される url を指定します。
- **リダイレクト url** –応答が送信される url を指定します。 このパラメーターの値は、 [Google 開発者コンソール](https://console.developers.google.com/)のプロジェクトの **[資格情報]** タブに表示される値のいずれかと一致する必要があります。
- **AccessToken url** –認証コードの取得後にアクセストークンを要求するために使用される url を指定します。
- **GetUserNameAsync Func** –アカウントが正常に認証された後に、アカウントのユーザー名を非同期に取得するために使用される省略可能な `Func` です。
- **ネイティブ UI を使用**する–デバイスの web ブラウザーを使用して認証要求を実行するかどうかを示す `boolean` 値です。

### <a name="setup-authentication-event-handlers"></a>認証イベント ハンドラーをセットアップします。

次のコード例に示すように、ユーザーインターフェイスを表示する前に、`OAuth2Authenticator.Completed` イベントのイベントハンドラーを登録する必要があります。

```csharp
authenticator.Completed += OnAuthCompleted;
```

ユーザーが正常に認証またはサインインがキャンセル時に、このイベントが発生します。

必要に応じて、`OAuth2Authenticator.Error` イベントのイベントハンドラーを登録することもできます。

### <a name="presenting-the-sign-in-user-interface"></a>サインイン ユーザー インターフェイスを表示します。

サインイン ユーザー インターフェイスは、Xamarin.Auth ログイン プレゼンターに各プラットフォーム プロジェクトで初期化する必要がありますを使用して、ユーザーに表示することができます。 次のコード例は、iOS プロジェクトの `AppDelegate` クラスでログインプレゼンターを初期化する方法を示しています。

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

次のコード例は、Android プロジェクトの `MainActivity` クラスでログインプレゼンターを初期化する方法を示しています。

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

.NET Standard ライブラリ プロジェクトできるように呼び出しますログイン プレゼンター。

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

`Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` メソッドの引数は、`OAuth2Authenticator` インスタンスであることに注意してください。 `Login` メソッドが呼び出されると、デバイスの web ブラウザーのタブでユーザーにサインインユーザーインターフェイスが表示されます。次のスクリーンショットを参照してください。

![](oauth-images/login.png "Google Sign-In")

### <a name="processing-the-redirect-url"></a>リダイレクト URL の処理

ユーザーが認証プロセスを完了すると、コントロールは [web ブラウザー] タブからアプリケーションに戻ります。これを実現するには、認証プロセスから返されるリダイレクト URL にカスタム URL スキームを登録し、送信されたカスタム URL を検出して処理します。

アプリケーションに関連付けるカスタムの URL スキームを選択するときに、アプリケーションは、管理対象の名前に基づくスキームを使用する必要があります。 これは、バンドル識別子名を使用して、iOS と android では、パッケージ名に、URL スキームを作成することを反転して実現できます。 ただし、Google など、いくつかの id プロバイダーは取り消され、URL スキームとして使用し、ドメイン名に基づくクライアント識別子を割り当てます。 たとえば、Google が `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`のクライアント id を作成した場合は、URL スキームが `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`されます。 スキームコンポーネントの後に使用できるのは、1つの `/` のみであることに注意してください。 そのため、カスタム URL スキームを使用するリダイレクト URL の完全な例は `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`です。

Web ブラウザーでは、カスタムの URL スキームを格納している id プロバイダーから応答を受信するが失敗すると、URL を読み込もうとします。 代わりに、カスタムの URL スキームは、イベントを発生させることによって、オペレーティング システムに報告されます。 オペレーティング システムは、登録されているスキームは、し、チェックし、1 つが見つかった場合、オペレーティング システムは、スキームが登録されているアプリケーションを起動し、リダイレクト URL を送信します。

オペレーティング システムにカスタム URL スキームを登録するメカニズムと、そのスキームを処理するためのメカニズムは、各プラットフォームに固有です。

#### <a name="ios"></a>iOS

IOS では、次のスクリーンショットに示すように、カスタム URL スキームが**情報 plist**に登録されます。

![](oauth-images/info-plist.png "URL Scheme Registration")

**識別子**の値には任意の値を指定でき、**ロール**の値は **[ビューアー]** に設定する必要があります。 `com.googleusercontent.apps`で始まる**Url スキーム**の値は、 [Google API コンソール](https://console.developers.google.com)でプロジェクトの iOS クライアント id から取得できます。

Id プロバイダーには、承認要求が完了すると、アプリケーションのリダイレクト URL にリダイレクトします。 この URL はカスタムスキームを使用するので、iOS はアプリケーションを起動し、起動時のパラメーターとして URL を渡します。これは、次のコード例に示すように、アプリケーションの `AppDelegate` クラスの `OpenUrl` オーバーライドによって処理されます。

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

`OpenUrl` メソッドは、パブリック `OAuth2Authenticator` オブジェクトの `OnPageLoading` メソッドを使用してリダイレクト URL を処理する前に、受信した URL を `NSUrl` から .NET `Uri`に変換します。 これにより、Xamarin.Auth を web ブラウザーのタブを閉じると、受信した OAuth データを解析します。

#### <a name="android"></a>Android

Android では、スキームを処理する `Activity` で[`IntentFilter`](xref:Android.App.IntentFilterAttribute)属性を指定することによって、カスタム URL スキームが登録されます。 Id プロバイダーには、承認要求が完了すると、アプリケーションのリダイレクト URL にリダイレクトします。 URL はカスタムスキームを使用するため、Android はアプリケーションを起動します。これは、URL を起動パラメーターとして渡し、カスタム URL スキームを処理するために登録 `Activity` の `OnCreate` メソッドによって処理されます。 次のコード例では、カスタムの URL スキームを処理するサンプル アプリケーションからクラスを示します。

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

`OnCreate` メソッドは、パブリック `OAuth2Authenticator` オブジェクトの `OnPageLoading` メソッドを使用してリダイレクト URL を処理する前に、受信した URL を `Android.Net.Url` から .NET `Uri`に変換します。 これにより、web ブラウザーのタブを閉じて、受信した OAuth データを解析する Xamarin.Auth です。

> [!IMPORTANT]
> Android では、Xamarin. Auth は `CustomTabs` API を使用して、web ブラウザーとオペレーティングシステムと通信します。 ただし、`CustomTabs` 互換性のあるブラウザーがユーザーのデバイスにインストールされることは保証されていません。

### <a name="examining-the-oauth-response"></a>OAuth の応答を検証

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

認証が成功したときに収集されたデータは、`AuthenticatorCompletedEventArgs.Account` プロパティで確認できます。 これには、id プロバイダーによって提供される API へのデータの要求の署名に使用できるアクセス トークンが含まれます。

### <a name="making-requests-for-data"></a>データの要求を行う

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

`OAuth2Request` インスタンスでは、HTTP メソッドと API URL に加えて、`Constants.UserInfoUrl` プロパティによって指定された URL に要求を署名するアクセストークンを含む `Account` インスタンスも指定します。 Id プロバイダーは、アクセス トークン有効期間として認識されているユーザーの名前と電子メール アドレスを含む、JSON の応答としてし、基本的なユーザー データを返します。 JSON 応答は、`user` 変数に読み取られ、逆シリアル化されます。

詳細については、Google 開発者ポータルでの[GOOGLE API の呼び出し](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi)に関する説明を参照してください。

### <a name="storing-and-retrieving-account-information-on-devices"></a>格納して、デバイスのアカウント情報の取得

Xamarin は、アプリケーションが常にユーザーを再認証する必要がないように、`Account` オブジェクトをアカウントストアに安全に格納します。 `AccountStore` クラスは、アカウント情報を格納する役割を担い、iOS のキーチェーンサービスと Android の `KeyStore` クラスによってサポートされます。

> [!IMPORTANT]
> Xamarin. Auth の `AccountStore` クラスは非推奨とされています。代わりに、Xamarin `SecureStorage` クラスを使用する必要があります。 詳細については、「 [AccountStore から Xamarin. Essentials SecureStorage への移行」を](https://github.com/xamarin/Xamarin.Auth/wiki/Migrating-from-AccountStore-to-Xamarin.Essentials-SecureStorage)参照してください。

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

この記事では、Xamarin.Auth を使用して、Xamarin.Forms アプリケーションの認証プロセスを管理する方法について説明します。 Xamarin. Auth には、Google、Microsoft、Facebook、Twitter などの id プロバイダーを使用するために Xamarin. Forms アプリケーションで使用される `OAuth2Authenticator` クラスと `OAuth2Request` クラスが用意されています。

## <a name="related-links"></a>関連リンク

- [Oauthのフロー (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)
- [ネイティブアプリの OAuth 2.0](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [OAuth 2.0 を使用した Google Api へのアクセス](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin. Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin. Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
