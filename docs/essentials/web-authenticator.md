---
title: Xamarin.Essentials:Web Authenticator
description: このドキュメントでは、Xamarin.Essentials の WebAuthenticator クラスについて説明します。これを使用すると、アプリへのコールバックをリッスンする、ブラウザー ベースの認証フローを開始できます。
ms.assetid: 3D95371E-5D59-440E-8D31-F3C04E493DC1
author: redth
ms.author: jodick
ms.date: 03/26/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f05868bbf8da9597c4290ba687f767f3995ba437
ms.sourcegitcommit: 07ee6a95f77f9a12fadb857e549cdcdb1928c7d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904985"
---
# <a name="no-locxamarinessentials-web-authenticator"></a>Xamarin.Essentials:Web Authenticator

**WebAuthenticator** クラスを使用すると、アプリに登録されている特定の URL へのコールバックをリッスンする、ブラウザー ベースのフローを開始できます。

## <a name="overview"></a>概要

多くのアプリにはユーザー認証を追加する必要があり、これは多くの場合、ユーザーが既存の Microsoft、Facebook、Google、および Apple サインイン アカウントにサインインできるようにすることを意味します。

[Microsoft Authentication Library (MSAL)](/azure/active-directory/develop/msal-overview) には、アプリに認証を追加するための優れたターンキー ソリューションが用意されています。 クライアントの NuGet パッケージでは、Xamarin アプリもサポートされています。

認証用に独自の Web サービスを使用する場合は、**WebAuthenticator** を使用してクライアント側の機能を実装することが可能です。

## <a name="why-use-a-server-back-end"></a>サーバー バックエンドを使用する理由

多くの認証プロバイダーは、セキュリティを強化するために、明示的または 2 本足の認証フローのみの提供へと移行しました。 つまり、認証フローを完了するには、プロバイダーからの "_クライアント シークレット_" が必要です。 残念ながら、モバイル アプリはシークレットを格納するための最適な場所ではありません。モバイル アプリのコード、バイナリ、またはその他に格納されているデータは、通常、安全でないと見なされます。

ここでのベスト プラクティスは、モバイル アプリと認証プロバイダーの間の中間レイヤーとして、Web バックエンドを使用することです。

> [!IMPORTANT]
> 認証フローで Web バックエンドを利用しない古いモバイル専用認証ライブラリとパターンは、クライアント シークレットの格納に対する本質的なセキュリティ不足のため、使用しないようにすることを強くお勧めします。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**WebAuthenticator** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="android"></a>[Android](#tab/android)

Android では、コールバック URI を処理するためにインテント フィルターの設定が必要です。 これは、`WebAuthenticatorCallbackActivity` クラスをサブクラス化することで簡単に実現できます。

```csharp
const string CALLBACK_SCHEME = "myapp";

[Activity(NoHistory = true, LaunchMode = LaunchMode.SingleTop)]
[IntentFilter(new[] { Android.Content.Intent.ActionView },
    Categories = new[] { Android.Content.Intent.CategoryDefault, Android.Content.Intent.CategoryBrowsable },
    DataScheme = CALLBACK_SCHEME)]
public class WebAuthenticationCallbackActivity : Xamarin.Essentials.WebAuthenticatorCallbackActivity
{
}
```

# <a name="ios"></a>[iOS](#tab/ios)

iOS では、アプリのコールバック URI パターンを次のような Info.plist に追加する必要があります。

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLName</key>
        <string>xamarinessentials</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>xamarinessentials</string>
        </array>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
    </dict>
</array>
```

また、Essentials を呼び出すために `AppDelegate` の `OpenUrl` および `ContinueUserActivity` メソッドをオーバーライドする必要があります。

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    if (Xamarin.Essentials.Platform.OpenUrl(app, url, options))
        return true;

    return base.OpenUrl(app, url, options);
}

public override bool ContinueUserActivity(UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    if (Xamarin.Essentials.Platform.ContinueUserActivity(application, userActivity, completionHandler))
        return true;
    return base.ContinueUserActivity(application, userActivity, completionHandler);
}
```

# <a name="uwp"></a>[UWP](#tab/uwp)

UWP の場合は、`Package.appxmanifest` ファイル内でコールバック URI を宣言する必要があります。

```xml
    <Extensions>
        <uap:Extension Category="windows.protocol">
            <uap:Protocol Name="myapp">
                <uap:DisplayName>My App</uap:DisplayName>
            </uap:Protocol>
        </uap:Extension>
    </Extensions>
```

-----

## <a name="using-webauthenticator"></a>WebAuthenticator の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

この API は、主に 1 つのメソッド `AuthenticateAsync` で構成されています。これは 2 つのパラメーターを受け取ります。Web ブラウザー フローを開始するために使用される URL と、フローが最終的にコールバックする予定の、アプリが処理できるように登録されている URI です。

結果は、コールバック URI から解析されたすべてのクエリ パラメーターを含む `WebAuthenticatorResult` です。

```csharp
var authResult = await WebAuthenticator.AuthenticateAsync(
        new Uri("https://mysite.com/mobileauth/Microsoft"),
        new Uri("myapp://"));

var accessToken = authResult?.AccessToken;
```

`WebAuthenticator` API により、ブラウザーでの URL の起動と、コールバックを受信するまでの待機を管理できます。

![一般的な Web 認証フロー](images/web-authenticator.png)

任意の時点でユーザーがフローをキャンセルした場合、`TaskCanceledException` がスローされます。

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="android"></a>[Android](#tab/android)

使用可能な場合は常にカスタム タブが使用されます。それ以外の場合は、URL のインテントが開始されます。

# <a name="ios"></a>[iOS](#tab/ios)

iOS 12 以降では、`ASWebAuthenticationSession` が使用されます。  iOS 11 では、`SFAuthenticationSession` が使用されます。  それ以前のバージョンの iOS では、使用可能なら `SFSafariViewController` が使用され、それ以外の場合は Safari が使用されます。

# <a name="uwp"></a>[UWP](#tab/uwp)

UWP では、サポートされている場合は `WebAuthenticationBroker` が使用され、それ以外の場合はシステム ブラウザーが使用されます。

-----

## <a name="apple-sign-in"></a>Apple サインイン

[Apple のレビュー ガイドライン](https://developer.apple.com/app-store/review/guidelines/#sign-in-with-apple)に従い、自分のアプリで認証のためにソーシャル ログイン サービスを使用する場合は、オプションとして Apple サインインも提供する必要があります。

アプリに Apple サインインを追加するには、まず [Apple サインインを使用するようにアプリを構成する](../ios/platform/ios13/sign-in.md)必要があります。

iOS 13 以降では、`AppleSignInAuthenticator.AuthenticateAsync()` メソッドを呼び出すことをお勧めします。 これは内部でネイティブの Apple サインイン API が使用されるため、ユーザーは各自のデバイス上で最良のエクスペリエンスを得ることができます。 次のように、実行時に適切な API を使用するように共有コードを記述できます。

```csharp
var scheme = "..."; // Apple, Microsoft, Google, Facebook, etc.
WebAuthenticatorResult r = null;

if (scheme.Equals("Apple")
    && DeviceInfo.Platform == DevicePlatform.iOS
    && DeviceInfo.Version.Major >= 13)
{
    // Use Native Apple Sign In API's
    r = await AppleSignInAuthenticator.AuthenticateAsync();
}
else
{
    // Web Authentication flow
    var authUrl = new Uri(authenticationUrl + scheme);
    var callbackUrl = new Uri("xamarinessentials://");

    r = await WebAuthenticator.AuthenticateAsync(authUrl, callbackUrl);
}

var authToken = string.Empty;
if (r.Properties.TryGetValue("name", out var name) && !string.IsNullOrEmpty(name))
    authToken += $"Name: {name}{Environment.NewLine}";
if (r.Properties.TryGetValue("email", out var email) && !string.IsNullOrEmpty(email))
    authToken += $"Email: {email}{Environment.NewLine}";

// Note that Apple Sign In has an IdToken and not an AccessToken
authToken += r?.AccessToken ?? r?.IdToken;
```

> [!TIP]
> iOS 13 以外のデバイスの場合、これによって Web 認証フローが開始されます。これを使用して、Android および UWP デバイスで Apple サインインを有効にすることもできます。
> iOS シミュレーターで iCloud アカウントにサインインして、Apple サインインをテストできます。

-----

## <a name="aspnet-core-server-back-end"></a>ASP.NET Core サーバー バックエンド

任意の Web バックエンド サービスと共に `WebAuthenticator` API を使用することができます。  ASP.NET Core アプリと共に使用するには、まず、次の手順で Web アプリを構成する必要があります。

1. ASP.NET Core Web アプリで、必要な[外部ソーシャル認証プロバイダー](/aspnet/core/security/authentication/social/?tabs=visual-studio)をセットアップします。
2. `.AddAuthentication()` の呼び出しで、既定の認証スキームを `CookieAuthenticationDefaults.AuthenticationScheme` に設定します。
3. Startup.cs の `.AddAuthentication()` の呼び出しで `.AddCookie()` を使用します。
4. すべてのプロバイダーは `.SaveTokens = true;` を指定して構成する必要があります。


```csharp
services.AddAuthentication(o =>
    {
        o.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddFacebook(fb =>
    {
        fb.AppId = Configuration["FacebookAppId"];
        fb.AppSecret = Configuration["FacebookAppSecret"];
        fb.SaveTokens = true;
    });
```

> [!TIP]
> Apple サインインを含める場合は、`AspNet.Security.OAuth.Apple` NuGet パッケージを使用できます。  Essentials の GitHub リポジトリで完全な [Startup.cs のサンプル](https://github.com/xamarin/Essentials/blob/develop/Samples/Sample.Server.WebAuthenticator/Startup.cs#L32-L60)を見ることができます。

### <a name="add-a-custom-mobile-auth-controller"></a>カスタム モバイル認証コントローラーを追加する

モバイル認証フローでは、通常、ユーザーが選択したプロバイダーに対して直接フローを開始することをお勧めします (たとえば、アプリのサインイン画面で [Microsoft] ボタンをクリックする)。  また、特定のコールバック URI でアプリに関連情報を返し、認証フローを終了できるようにすることも重要です。

これを実現するには、カスタム API コントローラーを使用します。

```csharp
[Route("mobileauth")]
[ApiController]
public class AuthController : ControllerBase
{
    const string callbackScheme = "myapp";

    [HttpGet("{scheme}")] // eg: Microsoft, Facebook, Apple, etc
    public async Task Get([FromRoute]string scheme)
    {
        // 1. Initiate authentication flow with the scheme (provider)
        // 2. When the provider calls back to this URL
        //    a. Parse out the result
        //    b. Build the app callback URL
        //    c. Redirect back to the app
    }
}
```

このコントローラーの目的は、アプリが要求しているスキーム (プロバイダー) を推測し、ソーシャル プロバイダーとの認証フローを開始することです。 プロバイダーが Web バックエンドにコールバックすると、コントローラーによって結果が解析され、パラメーターを使用してアプリのコールバック URI にリダイレクトされます。

プロバイダーの `access_token` などのデータをアプリに戻すことが必要になる場合があります。これは、コールバック URI のクエリ パラメーターを使用して実現できます。 または、代わりにサーバー上に独自の ID を作成して、独自のトークンをアプリに渡すこともできます。 この部分で何をどのように実行するかは、お客様の自由です。

Essentials のリポジトリで[完全なコントローラーのサンプル](https://github.com/xamarin/Essentials/blob/develop/Samples/Sample.Server.WebAuthenticator/Controllers/MobileAuthController.cs)を確認してください。

> [!NOTE]
> 上のサンプルでは、サード パーティの認証 (つまりOAuth) プロバイダーからアクセ ストークンを返す方法が示されています。 Web バックエンド自体への Web 要求を承認するために使用できるトークンを取得するには、Web アプリで独自のトークンを作成し、代わりにそれを返す必要があります。  「[ASP.NET Core の認証の概要](/aspnet/core/security/authentication)」には、ASP.NET Core での高度な認証シナリオに関する詳細が記載されています。

-----
## <a name="api"></a>API

- [WebAuthenticator のソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/WebAuthenticator)
- [WebAuthenticator API のドキュメント](xref:Xamarin.Essentials.WebAuthenticator)
- [ASP.NET Core サーバー サンプル](https://github.com/xamarin/Essentials/blob/develop/Samples/Sample.Server.WebAuthenticator/)
