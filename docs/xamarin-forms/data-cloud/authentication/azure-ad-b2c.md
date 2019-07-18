---
title: Azure Active Directory B2C でユーザーを認証します。
description: Azure Active Directory B2C はコンシューマー向け web およびモバイル アプリケーション用のクラウド id 管理を提供します。 この記事では、Azure Active Directory B2C を使用して、Microsoft 認証ライブラリを使用したモバイル アプリケーションに id 管理を統合する方法を示しています。
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2019
ms.openlocfilehash: 06f5716c8decb21de39fd46abe734b5fdcd6bd43
ms.sourcegitcommit: 0f78ec17210b915b43ddab75937de8063e472c70
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2019
ms.locfileid: "67417948"
---
# <a name="authenticate-users-with-azure-active-directory-b2c"></a>Azure Active Directory B2C でユーザーを認証します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)

_Azure Active Directory B2C はコンシューマー向け web およびモバイル アプリケーション用のクラウド id 管理を提供します。この記事では、Azure Active Directory B2C を使用して、Microsoft 認証ライブラリを使用したモバイル アプリケーションに id 管理を統合する方法を示しています。_

## <a name="overview"></a>概要

Azure Active Directory B2C (ADB2C) は、コンシューマー向けアプリケーションの id 管理サービス。 既存のソーシャル アカウントまたは電子メール アドレスまたはユーザー名とパスワードなどのカスタム資格情報を使用して、アプリケーションにサインインすることができます。 カスタム資格情報のアカウントと呼びます_ローカル_アカウント。

モバイル アプリケーションに Azure Active Directory B2C ID 管理サービスを統合するための手順は次のとおりです。

1. Azure Active Directory B2C テナントを作成します。
1. モバイル アプリケーションを Azure Active Directory B2C テナントに登録します。
1. サインアップとサインイン ポリシーを作成し、ユーザー フローのパスワードを忘れた場合
1. Azure Active Directory B2C テナントを認証ワークフローを開始するのにには、Microsoft Authentication Library (MSAL) を使用します。

> [!NOTE]
> Azure Active Directory B2C では、Microsoft、GitHub、Facebook、Twitter などを含む複数の id プロバイダーをサポートします。 Azure Active Directory B2C の機能の詳細については、次を参照してください。 [Azure Active Directory B2C のドキュメント](/azure/active-directory-b2c/)します。
>
> Microsoft 認証ライブラリは、複数のアプリケーション アーキテクチャとプラットフォームをサポートします。 MSAL の機能については、次を参照してください。 [Microsoft Authentication Library](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) GitHub でします。

## <a name="configure-an-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C テナントを構成します。

サンプル プロジェクトを実行するには、Azure Active Directory B2C テナントを作成する必要があります。 詳細については、[Azure ポータルで Azure Active Directory B2C テナントを作成する](/azure/active-directory-b2c/active-directory-b2c-get-started/) を参照してください。

テナントを作成する必要があります、**テナント名**と**テナント ID**モバイル アプリケーションを構成します。 テナント ID と名前は、テナントの URL を作成したときに生成されたドメインで定義されます。 生成されたテナント URL がの場合`https://contoso20190410tenant.onmicrosoft.com/`、**テナント ID**は`contoso20190410tenant.onmicrosoft.com`と**テナント名**は`contoso20190410tenant`します。 クリックして、Azure portal でテナントのドメインを検索、**ディレクトリとサブスクリプション フィルター**上部のメニュー。 次のスクリーン ショットは、Azure ディレクトリとサブスクリプション フィルター をクリックし、テナント ドメインを示しています。

[![Azure ディレクトリとサブスクリプション フィルター ビューでのテナント名](azure-ad-b2c-images/azure-tenant-name-cropped.png)](azure-ad-b2c-images/azure-tenant-name.png#lightbox)

サンプル プロジェクトでは、編集、 **Constants.cs**を設定するファイル、`tenantName`と`tenantId`フィールド。 次のコードはこれらの値を設定する方法のかどうか、テナントのドメインは`https://contoso20190410tenant.onmicrosoft.com/`、これらの値をポータルからの値に置き換えます。

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    ...
}
```

## <a name="register-your-mobile-application-with-azure-active-directory-b2c"></a>Azure Active Directory B2C でのモバイル アプリケーションを登録します。

接続してユーザーを認証する前に、モバイル アプリケーションをテナントに登録する必要があります。 登録プロセスを一意割り当てます**アプリケーション ID** 、アプリケーションに、**リダイレクト URL**認証後にアプリケーションへの応答を送信します。 詳細については、次を参照してください[Azure Active Directory B2C:。アプリケーションを登録する](/azure/active-directory-b2c/active-directory-b2c-app-registration/)します。 知る必要があります、**アプリケーション ID**プロパティ ビューでアプリケーション名の後に表示される、アプリケーションに割り当てられます。 次のスクリーン ショットでは、アプリケーション ID を検索する場所を示します。

[![Azure アプリケーションのプロパティ ビュー内のアプリケーション ID](azure-ad-b2c-images/azure-application-id-cropped.png)](azure-ad-b2c-images/azure-application-id.png#lightbox)

Microsoft 認証ライブラリでは、**リダイレクト URL**のようにアプリケーションを**アプリケーション ID**テキスト"msal"プレフィックスおよび後に"auth"と呼ばれるエンドポイント。 完全な URL には、アプリケーション ID が"1234abcd"の場合は、`msal1234abcd://auth`します。 アプリケーションが有効になっていることを確認、 **Native client**を設定し、作成、**カスタム リダイレクト URI**の次のスクリーン ショットに示すように、アプリケーション ID を使用します。

![Azure アプリケーションのプロパティ ビューでのカスタム リダイレクト URI](azure-ad-b2c-images/azure-redirect-uri.png)

Android の両方で後で使用される URL **ApplicationManifest.xml**と iOS **Info.plist**します。

サンプル プロジェクトでは、編集、 **Constants.cs**を設定するファイル、`clientId`フィールドを**アプリケーション ID**します。 次のコードはこの値を設定する方法、アプリケーション ID が`1234abcd`:

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    static readonly string clientId = "1234abcd";
    ...
}
```

## <a name="create-sign-up-and-sign-in-policies-and-forgot-password-policies"></a>サインアップとサインイン ポリシーを作成し、パスワード ポリシーを忘れた場合

ポリシーとは、ユーザーがアカウントの作成やパスワードのリセットなどのタスクを完了する前に、体験です。 ポリシーには、経験から、ユーザーが返されるときに、アプリケーションが受け取るトークンの内容も指定します。 サインアップとサインイン アカウントの両方のポリシーを設定し、パスワードをリセットする必要があります。 Azure では、一般的なポリシーの作成を簡略化する組み込みのポリシーがあります。 詳細については、次を参照してください[Azure Active Directory B2C:。組み込みのポリシー](/azure/active-directory-b2c/active-directory-b2c-reference-policies/)します。

ポリシーのセットアップが完了したら、2 つのポリシーを含めるか、**ユーザー フロー (ポリシー)** Azure portal で表示します。 次のスクリーン ショットでは、Azure portal で構成されている 2 つのポリシーを示しています。

![Azure ユーザー フロー (ポリシー) で構成されている 2 つのポリシーの表示します。](azure-ad-b2c-images/azure-application-policies.png)

サンプル プロジェクトでは、編集、 **Constants.cs**を設定するファイル、`policySignin`と`policyPassword`ポリシーのセットアップ時に選択した名前を反映するフィールド。

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    static readonly string clientId = "1234abcd";
    static readonly string policySignin = "B2C_1_signupsignin1";
    static readonly string policyPassword = "B2C_1_passwordreset";
    ...
}
```

## <a name="use-the-microsoft-authentication-library-msal-for-authentication"></a>認証用の Microsoft Authentication Library (MSAL) の使用します。

Microsoft Authentication Library (MSAL) NuGet パッケージは、共有、.NET Standard プロジェクト、および Xamarin.Forms ソリューションで、プラットフォーム プロジェクトに追加する必要があります。 MSAL が含まれています、`PublicClientApplicationBuilder`に準拠するオブジェクトを構築するクラス、`IPublicClientApplication`インターフェイス。 MSAL を使用して`With`コンス トラクターと認証メソッドに追加のパラメーターを指定する句。

サンプル プロジェクトの分離コードで**App.xaml**という名前の静的プロパティを定義`AuthenticationClient`と`UIParent`、インスタンス化します、`AuthenticationClient`コンス トラクター内のオブジェクト。 `WithIosKeychainSecurityGroup`句は、iOS アプリケーションのセキュリティ グループ名を提供します。 `WithB2CAuthority`句により、既定値**機関**、またはポリシーは、ユーザーを認証するために使用されます。 次の例では、インスタンス化する方法、 `PublicClientApplication`:

```csharp
public partial class App : Application
{
    public static IPublicClientApplication AuthenticationClient { get; private set; }

    public static object UIParent { get; set; } = null;

    public App()
    {
        InitializeComponent();

        AuthenticationClient = PublicClientApplicationBuilder.Create(Constants.ClientId)
            .WithIosKeychainSecurityGroup(Constants.IosKeychainSecurityGroups)
            .WithB2CAuthority(Constants.AuthoritySignin)
            .Build();

        MainPage = new NavigationPage(new LoginPage());
    }

    ...
```

`OnAppearing`内のイベント ハンドラー、 **LoginPage.xaml.cs**呼び出しの背後にあるコード`AcquireTokenSilentAsync`する前にログオンしたユーザーの認証トークンを更新します。 認証プロセスにリダイレクト、`LogoutPage`成功した場合と、失敗時に何も行われません。 次の例では、サイレント再認証プロセス`OnAppearing`:

```csharp
public partial class LoginPage : ContentPage
{
    ...

    protected override async void OnAppearing()
    {
        try
        {
            // Look for existing account
            IEnumerable<IAccount> accounts = await App.AuthenticationClient.GetAccountsAsync();

            AuthenticationResult result = await App.AuthenticationClient
                .AcquireTokenSilent(Constants.Scopes, accounts.FirstOrDefault())
                .ExecuteAsync();

            await Navigation.PushAsync(new LogoutPage(result));
        }
        catch
        {
            // Do nothing - the user isn't logged in
        }
        base.OnAppearing();
    }

    ...
}
```

`OnLoginButtonClicked` (ログイン ボタンがクリックされたときに発生する)、イベント ハンドラー呼び出し`AcquireTokenAsync`します。 MSAL ライブラリは自動的にモバイル デバイスのブラウザーを開きし、ログイン ページに移動します。 サインイン URL と呼ばれる、**機関**テナント名の組み合わせであり、ポリシーで定義、 **Constants.cs**ファイル。 ユーザーが選択した場合、パスワードのオプションを忘れた場合、例外を起動すると、アプリに返されます、エクスペリエンスのパスワードを忘れた場合します。 次の例では、認証プロセスを示します。

```csharp
public partial class LoginPage : ContentPage
{
    ...

    async void OnLoginButtonClicked(object sender, EventArgs e)
    {
        AuthenticationResult result;
        try
        {
            result = await App.AuthenticationClient
                .AcquireTokenInteractive(Constants.Scopes)
                .WithPrompt(Prompt.SelectAccount)
                .WithParentActivityOrWindow(App.UIParent)
                .ExecuteAsync();
    
            await Navigation.PushAsync(new LogoutPage(result));
        }
        catch (MsalException ex)
        {
            if (ex.Message != null && ex.Message.Contains("AADB2C90118"))
            {
                result = await OnForgotPassword();
                await Navigation.PushAsync(new LogoutPage(result));
            }
            else if (ex.ErrorCode != "authentication_canceled")
            {
                await DisplayAlert("An error has occurred", "Exception message: " + ex.Message, "Dismiss");
            }
        }
    }

    ...
}
```

`OnForgotPassword`メソッドは、サインイン プロセスに似ていますが、カスタム ポリシーを実装します。 `OnForgotPassword` さまざまなオーバー ロードを使用して`AcquireTokenAsync`、特定の提供することのできる**機関**します。 次の例では、カスタムの提供方法を示しています。**機関**トークンを取得する際に。

```csharp
public partial class LoginPage : ContentPage
{
    ...
    async Task<AuthenticationResult> OnForgotPassword()
    {
        try
        {
            return await App.AuthenticationClient
                .AcquireTokenInteractive(Constants.Scopes)
                .WithPrompt(Prompt.SelectAccount)
                .WithParentActivityOrWindow(App.UIParent)
                .WithB2CAuthority(Constants.AuthorityPasswordReset)
                .ExecuteAsync();
        }
        catch (MsalException)
        {
            // Do nothing - ErrorCode will be displayed in OnLoginButtonClicked
            return null;
        }
    }
}
```

認証の最後の部分は、サインアウト プロセスです。 `OnLogoutButtonClicked`メソッドは、ユーザーがサインアウト ボタンを押したときに呼び出されます。 すべてのアカウントをループ処理し、により、トークンが無効になりました。 次の例では、サインアウトの実装を示しています。

```csharp
public partial class LogoutPage : ContentPage
{
    ...
    async void OnLogoutButtonClicked(object sender, EventArgs e)
    {
        IEnumerable<IAccount> accounts = await App.AuthenticationClient.GetAccountsAsync();

        while (accounts.Any())
        {
            await App.AuthenticationClient.RemoveAsync(accounts.First());
            accounts = await App.AuthenticationClient.GetAccountsAsync();
        }

        await Navigation.PopAsync();
    }
}
```

### <a name="ios"></a>iOS

Ios では、Azure Active Directory B2C に登録されたカスタムの URL スキームに登録する必要があります**Info.plist**します。 MSAL で既に説明した、特定のパターンに従う URL スキームが必要ですが[モバイル アプリケーションを Azure Active Directory B2C に登録](/docs/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c)します。 次のスクリーン ショットでは、カスタムの URL スキーム**Info.plist**します。

![「IOS でのカスタム URL スキームを登録」](azure-ad-b2c-images/customurl-ios.png)

MSAL に登録されている ios キーチェーン権利も必要です、 **Entitilements.plist**の次のスクリーン ショットに示すようにします。

![「Ios アプリケーションの権利の設定」](azure-ad-b2c-images/entitlements-ios.png)

Azure Active Directory B2C が認証要求を完了させると、登録されたリダイレクト URL にリダイレクトします。 カスタムの URL スキームの iOS モバイル アプリケーションを起動して、によって処理される、起動パラメーターとして URL を渡して結果、`OpenUrl`のアプリケーションのオーバーライド`AppDelegate`クラス、および MSAL のエクスペリエンスのコントロールを返します。 `OpenUrl`実装を次のコード例に示します。

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        ...
        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
            return base.OpenUrl(app, url, options);
        }
    }
}
```

### <a name="android"></a>Android

Android では、Azure Active Directory B2C に登録されたカスタムの URL スキームに登録する必要があります、 **AndroidManifest.xml**します。 MSAL で既に説明した、特定のパターンに従う URL スキームが必要ですが[モバイル アプリケーションを Azure Active Directory B2C に登録](/docs/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c)します。 次の例では、カスタムの URL スキーム、 **AndroidManifest.xml**します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="com.xamarin.adb2cauthorization">
  <uses-sdk android:minSdkVersion="15" />
  <application android:label="ADB2CAuthorization">
    <activity android:name="microsoft.identity.client.BrowserTabActivity">
      <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="INSERT_URI_SCHEME_HERE" android:host="auth" />"
      </intent-filter>
    </activity>"
  </application>
</manifest>
```

`MainActivity`クラスは、提供するように変更する必要があります、`UIParent`中に、アプリケーションのオブジェクト、`OnCreate`呼び出します。 Azure Active Directory B2C では、承認要求が完了したらの登録済みの URL スキームにリダイレクト、 **AndroidManifest.xml**します。 登録済みの URI スキームが Android 呼び出しの結果、`OnActivityResult`メソッドによって処理される、起動パラメーターとして URL を使用して、`SetAuthenticationContinuationEventArgs`メソッド。

```csharp
public class MainActivity : FormsAppCompatActivity
{
    protected override void OnCreate(Bundle bundle)
    {
        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        LoadApplication(new App());
        App.UIParent = this;
    }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
    }
}
```

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォームで MSAL を使用する追加のセットアップは必要ありません。

## <a name="run-the-project"></a>プロジェクトを実行する

仮想または物理デバイスでアプリケーションを実行します。 タップすると、**ログイン**ボタンは、ブラウザーを開き、サインインまたはアカウントを作成することができますのページに移動する必要があります。 サインイン プロセスを完了すると、アプリケーションのログアウト ページに返される必要があります。 次のスクリーン ショットは Android および iOS で実行されている画面のユーザーのサインインを示しています。

![「Azure ADB2C サインイン画面の Android および iOS で」](azure-ad-b2c-images/login.png)

## <a name="related-links"></a>関連リンク

- [AzureADB2CAuth (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client)
- [Microsoft 認証ライブラリのドキュメント](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)
