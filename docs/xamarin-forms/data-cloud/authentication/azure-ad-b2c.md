---
title: "Azure Active Directory B2C を使用したユーザーの認証" 説明: "Azure Active Directory B2C は、コンシューマー向け web アプリケーションおよびモバイルアプリケーション用のクラウド id 管理を提供します。 この記事では、Azure Active Directory B2C を使用して、Microsoft 認証ライブラリを使用して id 管理をモバイルアプリケーションに統合する方法について説明します。 "
ms. 製品: xamarin ms. assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 12/04/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="authenticate-users-with-azure-active-directory-b2c"></a>Azure Active Directory B2C を使用してユーザーを認証する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azureadb2cauth)

_Azure Active Directory B2C は、コンシューマー向け web アプリケーションおよびモバイルアプリケーション用のクラウド id 管理を提供します。この記事では、Azure Active Directory B2C を使用して、Microsoft 認証ライブラリを使用して id 管理をモバイルアプリケーションに統合する方法について説明します。_

## <a name="overview"></a>概要

Azure Active Directory B2C (ADB2C) は、コンシューマー向けアプリケーション用の id 管理サービスです。 ユーザーは、既存のソーシャルアカウント、または電子メールやユーザー名、パスワードなどのカスタム資格情報を使用して、アプリケーションにサインインできます。 カスタム資格情報アカウントは、_ローカル_アカウントと呼ばれます。

Azure Active Directory B2C id 管理サービスをモバイルアプリケーションに統合するプロセスは次のとおりです。

1. Azure Active Directory B2C テナントを作成します。
1. Azure Active Directory B2C テナントにモバイルアプリケーションを登録します。
1. サインアップとサインインのためのポリシーを作成し、パスワードのユーザーフローを忘れます。
1. Microsoft Authentication Library (MSAL) を使用して、Azure Active Directory B2C テナントで認証ワークフローを開始します。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)をお持ちでない場合は、開始する前に[無料アカウント](https://aka.ms/azfree-docs-mobileapps)を作成してください。

Azure Active Directory B2C は、Microsoft、GitHub、Facebook、Twitter などの複数の id プロバイダーをサポートしています。 Azure Active Directory B2C 機能の詳細については、 [Azure Active Directory B2C のドキュメント](/azure/active-directory-b2c/)を参照してください。

Microsoft 認証ライブラリでは、複数のアプリケーションアーキテクチャとプラットフォームがサポートされています。 MSAL の機能の詳細については、GitHub の「 [Microsoft 認証ライブラリ](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)」を参照してください。

## <a name="configure-an-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C テナントを構成する

サンプルプロジェクトを実行するには、Azure Active Directory B2C テナントを作成する必要があります。 詳細については、「 [Azure portal での Azure Active Directory B2C テナントの作成](/azure/active-directory-b2c/active-directory-b2c-get-started/)」を参照してください。

テナントを作成した後、モバイルアプリケーションを構成するには、**テナント名**と**テナント ID**が必要になります。 テナント ID と名前は、テナント URL を作成したときに生成されたドメインによって定義されます。 生成されたテナント URL がの場合、テナント ID はで、 `https://contoso20190410tenant.onmicrosoft.com/` **tenant ID** `contoso20190410tenant.onmicrosoft.com` **テナント名**はです `contoso20190410tenant` 。 上部のメニューにある [**ディレクトリとサブスクリプション] フィルター**をクリックして、Azure portal でテナントドメインを見つけます。 次のスクリーンショットは、[Azure directory とサブスクリプションのフィルター] ボタンとテナントドメインを示しています。

[![Azure ディレクトリとサブスクリプションのフィルタービューでのテナント名](azure-ad-b2c-images/azure-tenant-name-cropped.png)](azure-ad-b2c-images/azure-tenant-name.png#lightbox)

サンプルプロジェクトで、 **Constants.cs**ファイルを編集し `tenantName` て、フィールドとフィールドを設定 `tenantId` します。 次のコードは、テナントドメインがの場合にこれらの値を設定する方法を示してい `https://contoso20190410tenant.onmicrosoft.com/` ます。これらの値は、ポータルの値に置き換えてください。

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    ...
}
```

## <a name="register-your-mobile-application-with-azure-active-directory-b2c"></a>Azure Active Directory B2C にモバイルアプリケーションを登録する

接続してユーザーを認証するには、事前にモバイルアプリケーションをテナントに登録しておく必要があります。 登録プロセスでは、アプリケーションに一意の**アプリケーション ID**と、認証後に応答をアプリケーションに返す**リダイレクト URL**が割り当てられます。 詳細については、「 [Azure Active Directory B2C: アプリケーションの登録](/azure/active-directory-b2c/active-directory-b2c-app-registration/)」を参照してください。 アプリケーションに割り当てられている**アプリケーション ID**を把握しておく必要があります。これは、[プロパティ] ビューのアプリケーション名の後に表示されます。 次のスクリーンショットは、アプリケーション ID を見つける場所を示しています。

[![Azure アプリケーションのプロパティビューのアプリケーション ID](azure-ad-b2c-images/azure-application-id-cropped.png)](azure-ad-b2c-images/azure-application-id.png#lightbox)

Microsoft 認証ライブラリでは、アプリケーションの**リダイレクト URL**が、"msal" というテキストが付いた**アプリケーション ID**になり、その後に "auth" という名前のエンドポイントが含まれていることを想定しています。 アプリケーション ID が "1234abcd" の場合、完全な URL はである必要があり `msal1234abcd://auth` ます。 アプリケーションで**Native client**設定が有効になっていることを確認し、次のスクリーンショットに示すように、アプリケーション ID を使用して**カスタムリダイレクト URI**を作成します。

![Azure アプリケーションのプロパティビューのカスタムリダイレクト URI](azure-ad-b2c-images/azure-redirect-uri.png)

この URL は、後で Android **ApplicationManifest.xml**と iOS**情報 plist**の両方で使用されます。

サンプルプロジェクトで、 **Constants.cs**ファイルを編集し `clientId` て、フィールドに**アプリケーション ID**を設定します。 次のコードは、アプリケーション ID がの場合にこの値を設定する方法を示してい `1234abcd` ます。

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    static readonly string clientId = "1234abcd";
    ...
}
```

## <a name="create-sign-up-and-sign-in-policies-and-forgot-password-policies"></a>サインアップとサインインのポリシーを作成し、パスワードポリシーを忘れた場合

ポリシーとは、ユーザーがアカウントの作成やパスワードのリセットなどのタスクを完了するために実行するエクスペリエンスです。 ポリシーでは、ユーザーが経験から戻ったときにアプリケーションが受け取るトークンの内容を指定することもできます。 アカウントのサインアップとサインインの両方に対してポリシーを設定し、パスワードをリセットする必要があります。 Azure には、一般的なポリシーの作成を簡略化する組み込みのポリシーがあります。 詳細については、「 [Azure Active Directory B2C: 組み込みポリシー](/azure/active-directory-b2c/active-directory-b2c-reference-policies/)」を参照してください。

ポリシーの設定が完了したら、Azure portal の [**ユーザーフロー (ポリシー)** ] ビューに2つのポリシーを設定する必要があります。 次のスクリーンショットは、Azure portal に構成されている2つのポリシーを示しています。

![Azure ユーザーフロー (ポリシー) ビューで構成された2つのポリシー](azure-ad-b2c-images/azure-application-policies.png)

サンプルプロジェクトで、 **Constants.cs**ファイルを編集し `policySignin` て、ポリシーの `policyPassword` 設定時に選択した名前を反映するようにフィールドとフィールドを設定します。

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

## <a name="use-the-microsoft-authentication-library-msal-for-authentication"></a>認証に Microsoft Authentication Library (MSAL) を使用する

Microsoft Authentication Library (MSAL) NuGet パッケージは、ソリューション内の共有、.NET Standard プロジェクト、プラットフォームプロジェクトに追加する必要があり Xamarin.Forms ます。 MSAL には、 `PublicClientApplicationBuilder` インターフェイスに準拠したオブジェクトを構築するクラスが含まれてい `IPublicClientApplication` ます。 MSAL で `With` は、コンストラクターと認証メソッドに追加のパラメーターを指定するために句を利用しています。

サンプルプロジェクトでは、 **app.xaml**の分離コードがおよびという名前の静的プロパティを定義 `AuthenticationClient` し、 `UIParent` コンストラクター内のオブジェクトをインスタンス化し `AuthenticationClient` ます。 句は、 `WithIosKeychainSecurityGroup` iOS アプリケーションのセキュリティグループ名を提供します。 句には、 `WithB2CAuthority` ユーザーの認証に使用される既定の**機関**(ポリシー) が用意されています。 句は、 `WithRedirectUri` 複数の uri が指定されている場合に使用するリダイレクト URI を Azure Notification Hubs インスタンスに指示します。 次の例は、をインスタンス化する方法を示してい `PublicClientApplication` ます。

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
            .WithRedirectUri($"msal{Constants.ClientId}://auth")
            .Build();

        MainPage = new NavigationPage(new LoginPage());
    }

    ...
```

> [!NOTE]
> Azure Notification Hubs インスタンスで1つのリダイレクト URI のみが定義されている場合、インスタンスは、 `AuthenticationClient` 句でリダイレクト uri を指定しなくても動作する可能性があり `WithRedirectUri` ます。 ただし、他のクライアントまたは認証方法をサポートするように Azure の構成を拡張する場合は、常にこの値を指定する必要があります。

前にログインした `OnAppearing` ユーザーの認証トークンを更新するために、 **LoginPage.xaml.cs**分離コード内のイベントハンドラーが `AcquireTokenSilentAsync` を呼び出します。 成功した場合、認証プロセスはにリダイレクトされ、 `LogoutPage` 失敗した場合は何も実行されません。 次の例は、のサイレント再認証プロセスを示してい `OnAppearing` ます。

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

`OnLoginButtonClicked`([ログイン] ボタンがクリックされたときに発生する) イベントハンドラーがを呼び出し `AcquireTokenAsync` ます。 MSAL ライブラリは、自動的にモバイルデバイスのブラウザーを開き、ログインページに移動します。 **証明機関**と呼ばれるサインイン URL は、 **Constants.cs**ファイルで定義されているテナント名とポリシーを組み合わせたものです。 ユーザーが [パスワードを忘れた場合] オプションを選択すると、例外が発生してアプリに返されます。これにより、パスワードを忘れたことが起動します。 次の例は、認証プロセスを示しています。

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

`OnForgotPassword`メソッドはサインインプロセスに似ていますが、カスタムポリシーを実装しています。 `OnForgotPassword`は、の異なるオーバーロードを使用し `AcquireTokenAsync` ます。これにより、特定の**権限**を提供できます。 次の例は、トークンを取得するときにカスタム**証明機関**を指定する方法を示しています。

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

認証の最後の部分は、サインアウトプロセスです。 `OnLogoutButtonClicked`メソッドは、ユーザーがサインアウトボタンを押したときに呼び出されます。 すべてのアカウントをループ処理し、トークンが無効になっていることを確認します。 次のサンプルは、サインアウトの実装を示しています。

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

IOS では、Azure Active Directory B2C に登録されたカスタム URL スキームを、**情報 plist**に登録する必要があります。 MSAL では、URL スキームが特定のパターンに準拠していることが想定されています。前述の「[モバイルアプリケーションを Azure Active Directory B2C に登録](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c)する」で説明しています。 次のスクリーンショットは、**ユーザーのカスタム URL スキームを示し**ています。

!["IOS でのカスタム URL スキームの登録"](azure-ad-b2c-images/customurl-ios.png)

MSAL には、次のスクリーンショットに示すように、 **Entitilements**に登録されている IOS のキーチェーンの権利も必要です。

!["IOS でのアプリケーション権利の設定"](azure-ad-b2c-images/entitlements-ios.png)

Azure Active Directory B2C が承認要求を完了すると、登録されたリダイレクト URL にリダイレクトされます。 カスタム URL スキームは、iOS でモバイルアプリケーションを起動して、起動パラメーターとして URL を渡して、アプリケーションのクラスのオーバーライドによって処理されるように `OpenUrl` `AppDelegate` し、エクスペリエンスの制御を msal に戻します。 の `OpenUrl` 実装を次のコード例に示します。

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

Android では、Azure Active Directory B2C に登録されたカスタム URL スキームが**AndroidManifest.xml**に登録されている必要があります。 MSAL では、URL スキームが特定のパターンに準拠していることが想定されています。前述の「[モバイルアプリケーションを Azure Active Directory B2C に登録](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c)する」で説明しています。 次の例では、 **AndroidManifest.xml**のカスタム URL スキームを示します。

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
        <!-- example -->
        <!-- <data android:scheme="msalaaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" android:host="auth" /> -->
        <data android:scheme="INSERT_URI_SCHEME_HERE" android:host="auth" />
      </intent-filter>
    </activity>"
  </application>
</manifest>
```

`MainActivity` `UIParent` 呼び出し中にオブジェクトをアプリケーションに提供するには、クラスを変更する必要があり `OnCreate` ます。 Azure Active Directory B2C が承認要求を完了すると、 **AndroidManifest.xml**から登録された URL スキームにリダイレクトされます。 登録された URI スキームは、メソッドに `OnActivityResult` よって処理される起動パラメーターとして、URL を使用してメソッドを呼び出し `SetAuthenticationContinuationEventArgs` ます。

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

ユニバーサル Windows プラットフォームで MSAL を使用するための追加のセットアップは必要ありません

## <a name="run-the-project"></a>プロジェクトを実行する

仮想デバイスまたは物理デバイスでアプリケーションを実行します。 [**ログイン**] ボタンをタップすると、ブラウザーが開き、サインインするかアカウントを作成できるページに移動します。 サインインプロセスが完了すると、アプリケーションのログアウトページに戻ります。 次のスクリーンショットは、Android と iOS で実行されているユーザーサインイン画面を示しています。

!["Android と iOS での Azure ADB2C サインイン画面"](azure-ad-b2c-images/login.png)

## <a name="related-links"></a>関連リンク

- [AzureADB2CAuth (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azureadb2cauth)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client)
- [Microsoft 認証ライブラリのドキュメント](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)
