---
title: Id プロバイダーを使用した AuthenticateUsers
description: この記事では、Xamarin.Auth を使用して、Xamarin.Forms アプリケーションの認証プロセスを管理する方法について説明します。
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: 3bc001c048129851a3604752fdfbd45d72d4c3d3
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760481"
---
# <a name="authenticate-users-with-an-identity-provider"></a>Id プロバイダーを使用したユーザーの認証

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)

_Xamarin.Auth は、ユーザーを認証し、自分のアカウントを格納するをクロス プラットフォーム SDK です。これには、Google、Microsoft、Facebook、Twitter などの id プロバイダーを使用するためのサポートを提供する OAuth 認証子が含まれます。この記事では、Xamarin.Auth を使用して、Xamarin.Forms アプリケーションの認証プロセスを管理する方法について説明します。_

OAuth は、認証のためのオープン標準ででき、サード パーティのリソース所有者の id を共有することがなく、情報にアクセスする権限を付与する必要がある、リソース プロバイダーに通知するリソースの所有者。 この例は、ユーザーの id を共有することがなく、データにアクセスするアプリケーションに権限を付与する必要がある、id プロバイダー (Google、Microsoft、Facebook、Twitter など) に通知するユーザー有効化します。 サインインでき、パスワード、web サイトまたはアプリケーションを公開することがなくが、web サイトおよびアプリケーションを id プロバイダーを使用するユーザーのための方法としてよく使用されます。

OAuth id プロバイダーを使用するときに、認証フローの概要は次のとおりです。

1. アプリケーションでは、id プロバイダーの URL をブラウザーが移動します。
1. Id プロバイダーでは、ユーザー認証を処理して、アプリケーションに承認コードを返します。
1. アプリケーションでは、id プロバイダーからアクセス トークンの承認コードを交換します。
1. アプリケーションでは、アクセス トークンを使用して、基本的なユーザー データを要求するための API など、id プロバイダーの Api にアクセスします。

サンプル アプリケーションでは、Xamarin.Auth を使用して Google に対してネイティブ認証フローを実装する方法を示します。 このトピックでは、id プロバイダーとして Google を使用すると、方法は他の id プロバイダーに等しく適用できます。 Google の OAuth 2.0 エンドポイントを使用する認証の詳細については、次を参照してください。 [Google Api へのアクセスに oauth 2.0 を使用して](https://developers.google.com/identity/protocols/OAuth2)Google の web サイト。

> [!NOTE]
> iOS 9 以降では、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することができない場合の ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 これは、アプリの更新することで実現できます**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)します。

## <a name="using-xamarinauth-to-authenticate-users"></a>Xamarin.Auth を使用してユーザーを認証するには

Xamarin.Auth には、id プロバイダーの承認エンドポイントと対話するアプリケーションの 2 つの方法がサポートされています。

1. 埋め込みの webview を使用します。 一般的な方法と、この中に、次の理由では推奨されなく。

    - Web ビューをホストするアプリケーションは、アプリケーションのためのものが OAuth 承認付与だけでなく、ユーザーの完全な認証資格情報にアクセスできます。 可能性のあるアプリケーションの攻撃対象領域を増やすことで、必要以上のより強力な資格情報へのアクセスをアプリケーションから、最小限の特権の原則が違反しています。
    - ホスト アプリケーションでしたユーザー名とパスワードをキャプチャ、自動的にフォームを送信およびユーザーの同意のバイパスしコピー セッションの cookie を使用してにユーザーとして認証済みの操作を実行します。
    - 埋め込み web ビューでは、他のアプリケーションまたはデバイスの web ブラウザー、添字形式のユーザー エクスペリエンスと見なされます承認要求のたびにサインインするユーザーを必要とすると、認証状態を共有しないでください。
    - いくつかの承認エンドポイントでは、検出し、web ビューから取得した承認要求をブロックする手順を実行します。

1. 推奨されるアプローチですが、デバイスの web ブラウザーを使用します。 OAuth 要求にデバイスのブラウザーを使用して、ユーザーは、アプリケーションのサインインと承認のフローのコンバージョン率の向上、デバイスごとに 1 回、id プロバイダーにサインインするだけが必要と、アプリケーションの使いやすさが向上します。 デバイスのブラウザーでは、検査して、web ビュー内のコンテンツがブラウザーに表示するコンテンツは変更できないのアプリケーションとセキュリティの強化も提供します。 これは、この記事とサンプル アプリケーションで使用される手法です。

サンプル アプリケーションが Xamarin.Auth を使用してユーザーを認証し、その基本的なデータを取得する方法の概要については、次の図に示されます。

![](oauth-images/google-auth.png "Xamarin.Auth を使用して Google による認証するには")

Google を使用する認証要求は、アプリケーションは、`OAuth2Authenticator`クラス。 後のサインイン ページで、アクセス トークンを含む、ユーザーが正常に認証で Google 認証の応答が返されます。 次に、アプリケーションは、要求を Google を使用して、基本的なユーザー データの作成、`OAuth2Request`クラスは、要求に含まれるアクセス トークンを使用します。

### <a name="setup"></a>セットアップ

Xamarin.Forms アプリケーションで Google サインインを統合する Google API コンソール プロジェクトを作成する必要があります。 これは次のようにして実装します。

1. 移動して、 [Google API コンソール](http://console.developers.google.com)web サイト、および Google アカウントの資格情報でサインインします。
1. プロジェクト ドロップダウン リストから、既存のプロジェクトを選択するか、新しく作成します。
1. 補足記事「「API マネージャー」の下で選択**資格情報**を選択し、、 **OAuth 同意画面タブ**します。選択、**電子メール アドレス**、指定、**ユーザーに表示されている製品名**、キーを押します**保存**します。
1. **資格情報**] タブで、[、**資格情報を作成**ドロップダウン ボックスの一覧、 **OAuth クライアント ID**します。
1. **アプリケーションの種類**、プラットフォームで実行されるモバイル アプリケーションを選択します (**iOS**または**Android**)。
1. 必要な情報を入力し、選択、**作成**ボタンをクリックします。

> [!NOTE]
> クライアント ID は、により、アプリケーションは有効になっている Google Api へのアクセスし、モバイル アプリケーションの 1 つのプラットフォームに固有です。 そのため、 **OAuth クライアント ID**で Google サインインを使用する各プラットフォーム用に作成する必要があります。

次の手順を実行した後で Google の OAuth2 認証フローを開始する Xamarin.Auth を使用できます。

### <a name="creating-and-configuring-an-authenticator"></a>作成と認証子の構成

Xamarin.Auth の`OAuth2Authenticator`クラスは、OAuth 認証フローを処理するために行います。 次のコード例のインスタンス化を示しています、`OAuth2Authenticator`クラスのデバイスの web ブラウザーを使用して認証を実行する場合。

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

`OAuth2Authenticator`クラスには、次のようには、パラメーターの番号が必要です。

- **クライアント ID** – は、要求を行って、内のプロジェクトから取得できるクライアントを識別、 [Google API コンソール](http://console.developers.google.com)します。
- **クライアント シークレット**– このアカウントは`null`または`string.Empty`します。
- **スコープ**: アプリケーションで要求されている API のアクセスを識別し、値は、ユーザーに表示される同意画面に通知します。 スコープの詳細については、次を参照してください。 [API の承認要求](https://developers.google.com/+/web/api/rest/oauth)Google の web サイト。
- **URL 承認**– 認証コードの取得から URL を識別します。
- **リダイレクト URL** – これは、応答を送信する URL を識別します。 このパラメーターの値に表示される値のいずれかと一致する必要があります、**資格情報**タブで、プロジェクトを[Google Developers Console](https://console.developers.google.com/)します。
- **AccessToken Url** – これは、認証コードの取得後に、アクセス トークンを要求するために使用する URL を識別します。
- **GetUserNameAsync Func** – 省略可能な`Func`正常に認証された後、アカウントのユーザー名を非同期的に取得に使用されます。
- **ネイティブ UI を使用して、** –`boolean`デバイスの web ブラウザーを使用して、認証要求を実行するかどうかを示す値。

### <a name="setup-authentication-event-handlers"></a>認証イベント ハンドラーをセットアップします。

ユーザー インターフェイスのイベント ハンドラーを表示する前に、`OAuth2Authenticator.Completed`次のコード例に示すように、イベントを登録する必要があります。

```csharp
authenticator.Completed += OnAuthCompleted;
```

ユーザーが正常に認証またはサインインがキャンセル時に、このイベントが発生します。

必要に応じて、イベント ハンドラー、`OAuth2Authenticator.Error`イベントを登録することもできます。

### <a name="presenting-the-sign-in-user-interface"></a>サインイン ユーザー インターフェイスを表示します。

サインイン ユーザー インターフェイスは、Xamarin.Auth ログイン プレゼンターに各プラットフォーム プロジェクトで初期化する必要がありますを使用して、ユーザーに表示することができます。 次のコード例でのログイン プレゼンターを初期化する方法を示します、`AppDelegate`の iOS プロジェクトでクラス。

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

次のコード例でのログイン プレゼンターを初期化する方法を示します、 `MainActivity` Android プロジェクトでクラス。

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

.NET Standard ライブラリ プロジェクトできるように呼び出しますログイン プレゼンター。

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

注意の引数、`Xamarin.Auth.Presenters.OAuthLoginPresenter.Login`メソッドは、`OAuth2Authenticator`インスタンス。 ときに、`Login`メソッドは、次のスクリーン ショットに示されているデバイスの web ブラウザーからタブでユーザーをサインイン ユーザー インターフェイスが表示されます。

![](oauth-images/login.png "Google のサインイン")

### <a name="processing-the-redirect-url"></a>リダイレクト URL の処理

ユーザーが認証プロセスを完了させると、web ブラウザーのタブからアプリケーションに制御が返されます。これは、認証プロセスから返されるリダイレクト URL にカスタム URL スキームを登録することによって実現されます。そして、その送信されたカスタム URL を検知して処理します。

アプリケーションに関連付けるカスタムの URL スキームを選択するときに、アプリケーションは、管理対象の名前に基づくスキームを使用する必要があります。 これは、バンドル識別子名を使用して、iOS と android では、パッケージ名に、URL スキームを作成することを反転して実現できます。 ただし、Google など、いくつかの id プロバイダーは取り消され、URL スキームとして使用し、ドメイン名に基づくクライアント識別子を割り当てます。 たとえば、Google のクライアント id を作成します`902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`、URL スキーム`com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`します。 1 つだけに注意してください`/`スキーム コンポーネントの後に表示されることができます。 そのため、カスタムの URL スキームを使用してリダイレクト URL の完全な例は`com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`します。

Web ブラウザーでは、カスタムの URL スキームを格納している id プロバイダーから応答を受信するが失敗すると、URL を読み込もうとします。 代わりに、カスタムの URL スキームは、イベントを発生させることによって、オペレーティング システムに報告されます。 オペレーティング システムは、登録されているスキームは、し、チェックし、1 つが見つかった場合、オペレーティング システムは、スキームが登録されているアプリケーションを起動し、リダイレクト URL を送信します。

オペレーティング システムにカスタム URL スキームを登録するメカニズムと、そのスキームを処理するためのメカニズムは、各プラットフォームに固有です。

#### <a name="ios"></a>iOS

Ios では、カスタムの URL スキームが登録されている**Info.plist**の次のスクリーン ショットに示すようにします。

![](oauth-images/info-plist.png "URL スキームを登録します。")

**識別子**値は、何も指定できます、**ロール**に値を設定する必要があります**ビューアー**します。 **Url スキーム**値で始まる`com.googleusercontent.apps`で、プロジェクトの iOS クライアント id から取得できます[Google API コンソール](http://console.developers.google.com)します。

Id プロバイダーには、承認要求が完了すると、アプリケーションのリダイレクト URL にリダイレクトします。 によって処理される、起動パラメーターとして渡すこと、URL で、URL は、iOS アプリケーションの起動においてカスタム スキームを使用しているため、`OpenUrl`のアプリケーションのオーバーライド`AppDelegate`クラスは、次のコード例に示されています。

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

`OpenUrl`メソッドから受信した URL の変換、 `NSUrl` .net`Uri`とリダイレクト URL を処理する前に、`OnPageLoading`のパブリック メソッド`OAuth2Authenticator`オブジェクト。 これにより、Xamarin.Auth を web ブラウザーのタブを閉じると、受信した OAuth データを解析します。

#### <a name="android"></a>Android

指定することで android では、カスタムの URL スキームが登録されている、 [ `IntentFilter` ](xref:Android.App.IntentFilterAttribute)属性を`Activity`スキームを処理します。 Id プロバイダーには、承認要求が完了すると、アプリケーションのリダイレクト URL にリダイレクトします。 によって処理される、起動パラメーターとして渡すこと、URL で、URL は、Android アプリケーションの起動においてカスタム スキームを使用すると、`OnCreate`のメソッド、`Activity`カスタムの URL スキームを処理するために登録します。 次のコード例では、カスタムの URL スキームを処理するサンプル アプリケーションからクラスを示します。

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

`DataSchemes`のプロパティ、 [ `IntentFilter` ](xref:Android.App.IntentFilterAttribute)で、プロジェクトの Android クライアント id から取得した反転クライアント識別子を設定する必要があります[Google API コンソール](http://console.developers.google.com)します。

`OnCreate`メソッドから受信した URL の変換、 `Android.Net.Url` .net`Uri`とリダイレクト URL を処理する前に、`OnPageLoading`のパブリック メソッド`OAuth2Authenticator`オブジェクト。 これにより、web ブラウザーのタブを閉じて、受信した OAuth データを解析する Xamarin.Auth です。

> [!IMPORTANT]
> Android では、Xamarin.Auth を使用して、 `CustomTabs` web ブラウザーとオペレーティング システムと通信する API。 ただしとは限りませんが、`CustomTabs`互換性のあるブラウザーは、ユーザーのデバイスにインストールされます。

### <a name="examining-the-oauth-response"></a>OAuth の応答を検証

Xamarin.Auth を発生させる、受信した OAuth データを解析した後、`OAuth2Authenticator.Completed`イベント。 このイベントのイベント ハンドラーで、`AuthenticatorCompletedEventArgs.IsAuthenticated`プロパティは、次のコード例に示すように、認証に成功したかどうかを識別するために使用できます。

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

成功した認証から収集されたデータが表示されます、`AuthenticatorCompletedEventArgs.Account`プロパティ。 これには、id プロバイダーによって提供される API へのデータの要求の署名に使用できるアクセス トークンが含まれます。

### <a name="making-requests-for-data"></a>データの要求を行う

アプリケーションがアクセス トークンを取得した後に要求を行い、使用、 `https://www.googleapis.com/oauth2/v2/userinfo` API は、id プロバイダーからの基本的なユーザー データを要求します。 この要求は、Xamarin.Auth の`OAuth2Request`をから取得したアカウントを使用して認証される要求を表すクラス、`OAuth2Authenticator`の次のコード例に示すように、インスタンスします。

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

HTTP メソッドと、API URL、`OAuth2Request`インスタンスも指定します、`Account`インスタンスで指定された URL への要求に署名するアクセス トークンを含む、`Constants.UserInfoUrl`プロパティ。 Id プロバイダーは、アクセス トークン有効期間として認識されているユーザーの名前と電子メール アドレスを含む、JSON の応答としてし、基本的なユーザー データを返します。 JSON 応答は読み取りとに逆シリアル化し、`user`変数。

詳細については、次を参照してください。 [Google API を呼び出す](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi)Google 開発者ポータルにします。

### <a name="storing-and-retrieving-account-information-on-devices"></a>格納して、デバイスのアカウント情報の取得

Xamarin.Auth を安全に格納`Account`アプリケーションは常にユーザーを再認証する必要があるないように、アカウント内のオブジェクトが格納されます。 `AccountStore`クラスは、アカウント情報を格納しては、iOS の Keychain サービスによって支えられて、 `KeyStore` Android でのクラス。

次のコード例に示す方法、`Account`オブジェクトを安全に保存します。

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

保存されたアカウントは、構成のアカウントのキーを使用して一意に識別されます`Username`プロパティとサービス ID には、アカウント ストアからアカウントをフェッチするときに使用される文字列です。 場合、`Account`以前に保存した、呼び出し、`Save`メソッドもう一度が上書きされます。

`Account` オブジェクトのサービスを呼び出すことによって取得できます、`FindAccountsForService`メソッドを次のコード例に示すようにします。

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

`FindAccountsForService`メソッドが返す、`IEnumerable`のコレクション`Account`と一致するアカウントとして設定されているコレクションの最初の項目のオブジェクト。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Auth を使用して、Xamarin.Forms アプリケーションの認証プロセスを管理する方法について説明します。 Xamarin.Auth を提供、`OAuth2Authenticator`と`OAuth2Request`Google、Microsoft、Facebook、Twitter などの id プロバイダーを使用する Xamarin.Forms アプリケーションで使用されるクラス。

## <a name="related-links"></a>関連リンク

- [OAuthNativeFlow (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)
- [OAuth 2.0 のネイティブ アプリ](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Oauth 2.0 を使用して Google の Api にアクセスするには](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
