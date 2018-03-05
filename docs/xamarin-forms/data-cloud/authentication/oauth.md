---
title: "Id プロバイダーとユーザーの認証"
description: "Xamarin.Auth では、クロス プラットフォーム SDK はユーザーを認証して、自分のアカウントを格納するためです。 これには、Google、Microsoft、Facebook、Twitter などの id プロバイダーを使用するためのサポートを提供する OAuth 認証子が含まれます。 この記事では、Xamarin.Auth を使用して、Xamarin.Forms アプリケーションでの認証プロセスを管理する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: ff0403fedf75ab22986f5fc83d16db3dbf8d92b6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="authenticating-users-with-an-identity-provider"></a>Id プロバイダーとユーザーの認証

_Xamarin.Auth では、クロス プラットフォーム SDK はユーザーを認証して、自分のアカウントを格納するためです。これには、Google、Microsoft、Facebook、Twitter などの id プロバイダーを使用するためのサポートを提供する OAuth 認証子が含まれます。この記事では、Xamarin.Auth を使用して、Xamarin.Forms アプリケーションでの認証プロセスを管理する方法について説明します。_

OAuth は、認証のオープン標準とリソースの所有者に、サード パーティのリソース所有者の id を共有せず、それらの情報にアクセスする権限を付与する必要がありますをリソース プロバイダーに通知できます。 この例は、ユーザーの id を共有せず、データにアクセスするアプリケーションに権限を付与する必要があります (Google、Microsoft、Facebook、Twitter など) を id プロバイダーに通知するユーザー有効化します。 サインインで、web サイトまたはアプリケーションへのパスワードを公開することがなく web サイトおよびアプリケーションが、id プロバイダーを使用するユーザーのための方法としてよく使用されます。

OAuth id プロバイダーを使用するときに、認証フローの概要は次のとおりです。

1. アプリケーションには、id プロバイダーの URL をブラウザーが移動します。
1. Id プロバイダーは、ユーザー認証を処理し、アプリケーションに認証コードを返します。
1. アプリケーションでは、id プロバイダーからアクセス トークンの認証コードを交換します。
1. アプリケーションでは、アクセス トークンを使用して、基本的なユーザー データを要求するための API など、id プロバイダーの Api にアクセスします。

サンプル アプリケーションでは、Xamarin.Auth を使用して Google に対してネイティブ認証フローを実装する方法を示します。 このトピックで id プロバイダーとして Google を使用すると、アプローチが他の id プロバイダーにも適用されます。 Google の OAuth 2.0 エンドポイントを使用する認証の詳細については、次を参照してください。 [Google Api へのアクセスに使用する OAuth2.0](https://developers.google.com/identity/protocols/OAuth2) Google の web サイトです。

> [!NOTE]
> Ios 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することがない場合のうち ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 アプリケーションを更新することによってこれを行う**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)です。

## <a name="using-xamarinauth-to-authenticate-users"></a>Xamarin.Auth を使用してユーザーを認証するには

Xamarin.Auth には、アプリケーションが id プロバイダーの認証エンドポイントと対話するための 2 つの方法がサポートされています。

1. 埋め込みの web ビューを使用します。 これは一般的な方法でした、中に、次の理由では推奨されなく。

    - Web ビューをホストするアプリケーションにユーザーの完全な認証資格情報でアプリケーションのためのものが OAuth authorization grant だけでなくアクセスできます。 アプリケーションには必要な可能性のあるアプリケーションの攻撃対象領域を増やすことよりも強力な資格情報へのアクセスがあるために、最低限の特権の原則が違反しています。
    - ホスト アプリケーションでしたユーザー名とパスワードをキャプチャ、自動的にフォームを送信バイパス ユーザーの同意を求める、およびセッション cookie をコピーし、それらユーザーとして認証されている操作を実行します。
    - 埋め込みの web ビューは、他のアプリケーションまたはデバイスの web ブラウザー、下位のユーザー エクスペリエンスと考えられる承認要求ごとにサインインする必要はありませんと認証の状態を共有しません。
    - いくつかの認証エンドポイントは、検出し、web ビューから入手した承認要求をブロックする手順を実行します。

1. デバイスの web ブラウザーは、推奨される方法を使用します。 OAuth 要求に、デバイスのブラウザーを使用して、アプリケーションのサインインと承認のフローの換算率を向上させる、デバイスごとに 1 回、id プロバイダーにサインインするだけでユーザーと、アプリケーションの使いやすさが向上します。 デバイスのブラウザーでは、アプリケーションは、検査およびコンテンツ、web ビューでもない、ブラウザーに表示を変更できるように強化されたセキュリティも提供します。 これは、この記事とサンプル アプリケーションで採用されているアプローチです。

サンプル アプリケーションが Xamarin.Auth を使用してユーザーを認証し、その基本的なデータを取得する方法の概要については、次の図で示されます。

![](oauth-images/google-auth.png "Xamarin.Auth を使用して Google で認証するには")

アプリケーションが Google を使用する認証要求を行う、`OAuth2Authenticator`クラスです。 ユーザーが正常に認証されると Google とのサインイン ページで、アクセス トークンを含む認証の応答が返されます。 その後、アプリケーションが要求を Google を使用して、基本的なユーザー データの`OAuth2Request`クラスは、要求に含まれるアクセス トークンを使用します。

### <a name="setup"></a>セットアップ

Google API コンソール プロジェクトは、Google のサインイン Xamarin.Forms アプリケーションとの統合を作成する必要があります。 これは次のようにして実装します。

1. 移動して、 [Google API Console](http://console.developers.google.com) web サイト、および Google アカウントの資格情報でサインインします。
1. プロジェクト ドロップダウン リストから、既存のプロジェクトを選択または新規に作成します。
1. 「API マネージャー」の下、サイドバーで次のように選択します。**資格情報**クリックし、、 **OAuth 同意画面タブ**です。選択、**電子メール アドレス**を指定、**製品名のユーザーに表示される**、キーを押します**保存**です。
1. **資格情報**] タブで、[、**資格情報を作成する**ドロップダウン ボックスの一覧**OAuth クライアント ID**です。
1. **アプリケーションの種類**でモバイル アプリケーションを実行するプラットフォームを選択 (**iOS**または**Android**)。
1. 必要な詳細情報を入力し、選択、**作成**ボタンをクリックします。

> [!NOTE]
> クライアント ID は、により、アプリケーションは有効になっている Google Api にアクセスし、モバイル アプリケーションは 1 つのプラットフォームに固有です。 したがって、 **OAuth クライアント ID** Google のサインインを使用する各プラットフォーム用に作成する必要があります。

次の手順を実行すると、Google との OAuth2 認証フローを開始する Xamarin.Auth を使用できます。

### <a name="creating-and-configuring-an-authenticator"></a>作成して、認証子を構成します。

Xamarin.Auth の`OAuth2Authenticator`クラスは、OAuth 認証フローを処理します。 次のコード例に示しますをインスタンス化、`OAuth2Authenticator`クラスのデバイスの web ブラウザーを使用して認証を実行するとき。

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

`OAuth2Authenticator`クラスには、次のようには、パラメーターの数が必要です。

- **クライアント ID** –、要求を行うと、プロジェクト内から取得できるクライアントを識別、 [Google API Console](http://console.developers.google.com)です。
- **クライアント シークレット**– これに`null`または`string.Empty`です。
- **スコープ**–、アプリケーションによって要求されている API のアクセスを識別し、値は、ユーザーに表示される同意画面に通知します。 スコープの詳細については、次を参照してください。 [API の承認要求](https://developers.google.com/+/web/api/rest/oauth)Google の web サイトです。
- **URL の承認**– この URL からの認証コードの取得場所を識別します。
- **リダイレクト URL** – 応答を送信する URL を識別します。 このパラメーターの値に表示される値のいずれかと一致する必要があります、**資格情報**タブで、プロジェクトを[Google Developers Console](https://console.developers.google.com/)です。
- **AccessToken Url** – 認証コードを取得した後に、アクセス トークンを要求するための URL を識別します。
- **GetUserNameAsync Func** – 省略可能な`Func`非同期的に正常に認証された後、アカウントのユーザー名を取得に使用されます。
- **ネイティブ UI を使用して**–`boolean`認証要求を実行するデバイスの web ブラウザーを使用するかどうかを示す値。

### <a name="setup-authentication-event-handlers"></a>認証のイベント ハンドラーをセットアップします。

ユーザー インターフェイスのイベント ハンドラーを表示する前に、`OAuth2Authenticator.Completed`イベントは次のコード例に示すように、登録する必要があります。

```csharp
authenticator.Completed += OnAuthCompleted;
```

このイベントは、ユーザーが正常に認証またはサインインがキャンセル時に起動されます。

必要に応じて、イベント ハンドラー、`OAuth2Authenticator.Error`イベントを登録することもできます。

### <a name="presenting-the-sign-in-user-interface"></a>サインイン ユーザー インターフェイスを表示します。

サインイン ユーザー インターフェイスは、Xamarin.Auth ログイン プレゼンター、各プラットフォームのプロジェクトで初期化する必要がありますを使用して、ユーザーに表示できます。 次のコード例でログイン発表者を初期化する方法を示しています、 `AppDelegate` iOS プロジェクトでクラス。

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

次のコード例でログイン発表者を初期化する方法を示しています、 `MainActivity` Android プロジェクトでクラス。

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

ポータブル クラス ライブラリ (PCL) プロジェクトを呼び出してログイン プレゼンターとおり。

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

なおへの引数、`Xamarin.Auth.Presenters.OAuthLoginPresenter.Login`メソッドは、`OAuth2Authenticator`インスタンス。 ときに、`Login`メソッドが呼び出され、サインイン ユーザー インターフェイスは、デバイスの web ブラウザーから、次のスクリーン ショットで示されているタブでユーザーに表示されます。

![](oauth-images/login.png "Google Sign-In")

### <a name="processing-the-redirect-url"></a>リダイレクト URL の処理

ユーザーには、認証プロセスが完了すると後 web ブラウザーのタブからコントロールは、アプリケーションに返します。これは、リダイレクト URL、認証プロセスを検出して送信されると、カスタムの URL を処理から返されるため、カスタムの URL スキームを登録することによって実現されます。

アプリケーションに関連付けるカスタム URL スキームを選択するときに、アプリケーションは、管理対象の名前に基づくスキームを使用する必要があります。 これは、iOS および android でのパッケージの名前でバンドル識別子名を使用していて、しに URL スキームを反転することによって実現できます。 ただし、Google など、いくつかの id プロバイダーは、これは、元に戻す、URL スキームとして使用されるドメイン名に基づくクライアント識別子を割り当てます。 たとえば、Google のクライアント id を作成する`902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`、URL スキームがなる`com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`です。 1 つだけであるに注意してください`/`スキーム コンポーネントの後に表示されることができます。 したがって、カスタムの URL スキームを使用して、リダイレクト URL の完全な例は`com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`します。

Web ブラウザーでは、カスタムの URL スキームを格納している id プロバイダーから応答を受信するときに失敗すると、URL をロードしようとします。 代わりに、カスタムの URL スキームは、イベントの発生によって、オペレーティング システムに報告されます。 オペレーティング システムは、登録されているスキームの場合、し、チェックし、1 つが見つかった場合、オペレーティング システムは、スキームが登録されているアプリケーションを起動し、リダイレクト URL を送信します。

オペレーティング システムにカスタム URL スキームを登録するスキームを処理するためのメカニズムは、各プラットフォームに固有です。

#### <a name="ios"></a>iOS

Ios の場合、カスタムの URL スキームが登録されている**Info.plist**の次のスクリーン ショットに示すようにします。

![](oauth-images/info-plist.png "URL スキームの登録")

**識別子**値、何でもかまいませんと**ロール**に値を設定する必要があります**ビューアー**です。 **Url スキーム**の値で始まる`com.googleusercontent.apps`で、プロジェクトの iOS クライアント id から取得できます[Google API Console](http://console.developers.google.com)です。

Id プロバイダーには、承認要求が完了すると、ときに、アプリケーションのリダイレクト URL にリダイレクトします。 によって処理される、起動パラメーターとして渡すと共に、URL の URL は、iOS アプリケーションの起動においてカスタム スキームを使用するため、 `OpenUrl` 、アプリケーションのオーバーライド`AppDelegate`クラスは、次のコード例に示されています。

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

`OpenUrl`メソッドから受信した URL の変換、 `NSUrl` .net`Uri`とリダイレクト URL を処理する前に、`OnPageLoading`のパブリック メソッド`OAuth2Authenticator`オブジェクト。 これにより、Xamarin.Auth web ブラウザー タブを閉じるにし、受信した OAuth データを解析します。

#### <a name="android"></a>Android

指定して、Android でカスタム URL スキームが登録されている、 [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)属性を`Activity`スキームを処理します。 Id プロバイダーには、承認要求が完了すると、ときに、アプリケーションのリダイレクト URL にリダイレクトします。 によって処理される、起動パラメーターとして渡すと共に、URL で URL は、Android アプリケーションの起動においてカスタム スキームを使用すると、`OnCreate`のメソッド、`Activity`カスタム URL スキームを処理するように登録します。 次のコード例では、カスタムの URL スキームを処理するサンプル アプリケーションからクラスを示します。

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

`DataSchemes`のプロパティ、 [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)でプロジェクトの Android クライアント id から取得される反転クライアント識別子に設定する必要があります[Google API Console](http://console.developers.google.com)です。

`OnCreate`メソッドから受信した URL の変換、 `Android.Net.Url` .net`Uri`とリダイレクト URL を処理する前に、`OnPageLoading`のパブリック メソッド`OAuth2Authenticator`オブジェクト。 これにより、web ブラウザー タブを閉じるし、受信した OAuth データを解析する Xamarin.Auth です。

> [!IMPORTANT]
> Android で Xamarin.Auth を使用して、 `CustomTabs` web ブラウザーとオペレーティング システムと通信する API。 ただしとは限りませんを`CustomTabs`互換性のあるブラウザーは、ユーザーのデバイスにインストールされます。

### <a name="examining-the-oauth-response"></a>OAuth の応答を確認します。

受信した OAuth データを解析するには、後に Xamarin.Auth を発生させる、`OAuth2Authenticator.Completed`イベント。 このイベントのイベント ハンドラーで、`AuthenticatorCompletedEventArgs.IsAuthenticated`の次のコード例に示すように、認証が成功したかどうかを識別するプロパティを使用できます。

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

認証が成功したから収集されたデータで使用できる、`AuthenticatorCompletedEventArgs.Account`プロパティです。 これには、id プロバイダーによって提供される API へのデータの要求の署名に使用できるアクセス トークンが含まれます。

### <a name="making-requests-for-data"></a>データの要求を作成します。

アプリケーションがアクセス トークンを入手した後に要求を行うため使用、 `https://www.googleapis.com/oauth2/v2/userinfo` API、id プロバイダーからの基本的なユーザー データを要求します。 この要求が行われた Xamarin.Auth の`OAuth2Request`をから取得したアカウントを使用して認証された要求を表すクラスを`OAuth2Authenticator`インスタンス、次のコード例に示すように。

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

だけでなく、HTTP メソッドおよび API URL、`OAuth2Request`インスタンスも指定、`Account`で指定された URL への要求に署名するアクセス トークンを格納しているインスタンス、`Constants.UserInfoUrl`プロパティです。 Id プロバイダーは、有効なアクセス トークンを認識する、ユーザーの名前と電子メール アドレスを含む、JSON の応答として、基本的なユーザー データを返します。 JSON 応答を読み取るし、逆シリアル化、`user`変数。

詳細については、次を参照してください。 [Google API を呼び出す](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi)Google 開発者ポータルにします。

### <a name="storing-and-retrieving-account-information-on-devices"></a>格納して、デバイスでアカウント情報を取得します。

安全に格納 Xamarin.Auth`Account`アプリケーションは常にユーザーを再認証する必要があるないように、アカウント内のオブジェクトが格納されます。 `AccountStore`クラスは、アカウント情報を格納しては、iOS でキーチェーン サービスによってバックアップされ、 `KeyStore` Android のクラスです。

次のコード例は、`Account`オブジェクトを安全に保存します。

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

保存されているアカウントは、アカウントので構成されるキーを使用して一意に識別されます`Username`プロパティとサービス ID には、アカウント ストアからアカウントをフェッチするときに使用される文字列です。 場合、`Account`以前に保存した、呼び出し、`Save`メソッド再度が上書きされます。

`Account` サービスを呼び出すことによって取得できるオブジェクト、`FindAccountsForService`メソッドを次のコード例に示すようにします。

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

`FindAccountsForService`メソッドを返します、`IEnumerable`のコレクション`Account`と一致するアカウントとして設定されているコレクションの最初の項目のオブジェクト。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Auth を使用して、Xamarin.Forms アプリケーションでの認証プロセスを管理する方法について説明します。 Xamarin.Auth 提供、`OAuth2Authenticator`と`OAuth2Request`Google、Microsoft、Facebook、Twitter などの id プロバイダーを使用する Xamarin.Forms アプリケーションによって使用されるクラスです。


## <a name="related-links"></a>関連リンク

- [OAuthNativeFlow (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/OAuthNativeFlow/)
- [ネイティブ アプリの OAuth 2.0](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [OAuth2.0 を使用して Google Api にアクセスするには](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
