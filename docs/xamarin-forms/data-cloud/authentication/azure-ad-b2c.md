---
title: Azure Active Directory B2C のユーザーの認証
description: Azure Active Directory B2C は、消費者向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。 この記事では、Microsoft の認証ライブラリと Azure Active Directory B2C を使用して、モバイル アプリケーションにコンシューマーの id 管理を統合する方法を示します。
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 627c6773c099c9cf45f871a9bb73a201bf98271a
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Azure Active Directory B2C のユーザーの認証

_Azure Active Directory B2C は、消費者向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。この記事では、Microsoft の認証ライブラリと Azure Active Directory B2C を使用して、モバイル アプリケーションにコンシューマーの id 管理を統合する方法を示します。_

![](~/media/shared/preview.png "この API は、現在のリリース前")

> [!NOTE]
> [認証ライブラリの Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client)は、プレビュー段階では、運用環境での使用に適しています。 ただし、ある可能性がある重大な変更、API、内部キャッシュ形式、およびその他のメカニズム、ライブラリでは、アプリケーションに影響を与える可能性のです。

## <a name="overview"></a>概要

Azure Active Directory B2C は、id 管理サービス コンシューマー向けアプリケーションでは、コンシューマーによってアプリケーションにサインインできるようにします。

- 既存のソーシャル アカウント (Microsoft、Google、Facebook、Amazon、LinkedIn) を使用しています。
- 新しい資格情報 (電子メール アドレスとパスワード、またはユーザー名とパスワード) を作成します。 これらの資格情報をいいます*ローカル*アカウント。

モバイル アプリケーションを Azure Active Directory B2C identity management サービスを統合するためのプロセスは次のとおりです。

1. Azure Active Directory B2C テナントを作成します。 詳細については、次を参照してください。 [Azure ポータルで Azure Active Directory B2C テナントを作成する](/azure/active-directory-b2c/active-directory-b2c-get-started/)です。
1. Azure Active Directory B2C テナントには、モバイル アプリケーションを登録します。 登録プロセスを割り当てます、**アプリケーション ID** 、アプリケーションを一意に識別して、**リダイレクト URL**をアプリケーションに応答を返信を送信するため使用できます。 詳細については、次を参照してください。 [Azure Active Directory B2C: アプリケーションの登録](/azure/active-directory-b2c/active-directory-b2c-app-registration/)です。
1. サインアップとサインインのポリシーを作成します。 このポリシーは、コンシューマーはサインアップとサインイン、間に通過するエクスペリエンスを定義し、成功でも、アプリケーションが受け取るトークンの内容を指定しますサインアップまたはサインインします。 詳細については、次を参照してください。 [Azure Active Directory B2C: 組み込みポリシー](/azure/active-directory-b2c/active-directory-b2c-reference-policies/)です。
1. 使用して、[認証ライブラリの Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) をモバイル アプリケーションを Azure Active Directory B2C テナントの認証ワークフローの開始にします。

> [!NOTE]
> モバイル アプリケーションへの Azure Active Directory B2C の id 管理の統合、および MSAL をモバイル アプリケーションを Azure Active Directory id 管理を統合する使用もできます。 これには、Azure Active Directory でモバイル アプリケーションを登録することによって、[アプリケーション登録ポータル](https://apps.dev.microsoft.com/)です。 登録プロセスを割り当てます、**アプリケーション ID**一意に識別する、アプリケーションでは、MSAL を使用する場合に指定する必要があります。 詳細については、次を参照してください。 [v2.0 エンドポイントでアプリを登録する方法](/azure/active-directory/develop/active-directory-v2-app-registration/)、および[認証、モバイル アプリを使用して Microsoft Authentication Library](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) Xamarin ブログ。

MSAL では、デバイスの web ブラウザーを使用して、認証を行います。 サインインしたら、各デバイスにアプリケーションのフローでのサインインと承認の換算率を向上するだけでユーザーと、アプリケーションの使いやすさが向上します。 デバイスのブラウザーでは、強化されたセキュリティも提供します。 ユーザーには、認証プロセスが完了すると後 web ブラウザーのタブからコントロールは、アプリケーションに返します。これは、リダイレクト URL、認証プロセスを検出して送信されると、カスタムの URL を処理から返されるため、カスタムの URL スキームを登録することによって実現されます。 カスタム URL スキームの選択の詳細については、次を参照してください。[ネイティブ アプリのリダイレクト URI を選択する](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/)です。

> [!NOTE]
> オペレーティング システムにカスタム URL スキームを登録するスキームを処理するためのメカニズムは、各プラットフォームに固有です。

Azure Active Directory B2C テナントに送信される各要求を指定します、*ポリシー*です。 ポリシーでは、サインアップまたはサインインなどのコンシューマー ユーザー エクスペリエンスについて説明します。 たとえば、サインアップ ポリシーは、次の設定を使用して構成する Azure Active Directory B2C テナントの動作を使用できます。

- コンシューマーは、アプリケーションへのサインインに使用できるアカウントの種類。
- サインアップ時に、コンシューマーから収集するデータ。
- 多要素認証。
- サインアップ ページのコンテンツ。
- ポリシーが実行されたときに、モバイル アプリケーションが受け取るトークンのクレーム。

Azure Active Directory テナントには、必要に応じて、アプリケーションで使用して、さまざまな種類の複数のポリシーを含めることができます。 さらに、ポリシーは、複数のアプリケーションで定義およびコードを変更せずにコンシューマーの id のエクスペリエンスを変更することができる再利用できます。 ポリシーの詳細については、次を参照してください。 [Azure Active Directory B2C: 組み込みポリシー](/azure/active-directory-b2c/active-directory-b2c-reference-policies/)です。

## <a name="setup"></a>セットアップ

Microsoft 認証ライブラリ (MSAL) NuGet ライブラリは、Xamarin.Forms ソリューション内のプラットフォーム プロジェクト、ポータブル クラス ライブラリ (PCL) プロジェクトに追加する必要があります。 次のセクションでは、MSAL を使用して、モバイル アプリケーションからの Azure Active Directory B2C テナントと通信するための追加のセットアップ手順を説明します。

### <a name="portable-class-library"></a>ポータブル クラス ライブラリ

Pcl MSAL を使用するには、Profile7 を使用する再ターゲットする必要があります。 PCL の詳細については、「[Introduction to Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md)」(ポータブル クラス ライブラリの概要) を参照してください。

### <a name="ios"></a>iOS

Ios の場合は、Azure Active Directory B2C に登録されているカスタムの URL スキームに登録する必要があります**Info.plist**の次のスクリーン ショットに示すようにします。

![](azure-ad-b2c-images/customurl-ios.png "IOS でカスタム URL スキームを登録します。")

Azure Active Directory B2C には、承認要求が完了すると、ときに、登録されたリダイレクト URL にリダイレクトします。 によって処理される、起動パラメーターとして渡すと共に、URL の URL は、iOS のモバイル アプリケーションの起動においてカスタム スキームを使用するため、 `OpenUrl` 、アプリケーションのオーバーライド`AppDelegate`クラスは、次のコードに示されています。例:

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
            return true;
        }
    }
}
```

内のコード、`OpenURL`メソッドは、認証ワークフローの対話型の部分が終了した後は、MSAL に制御が戻ることを確認します。

### <a name="android"></a>Android

Android では、Azure Active Directory B2C に登録されているカスタムの URL スキームに登録する必要があります**AndroidManifest.xml**、追加することによって、 `<activity>` 、既存の内部要素`<application>`要素。 `<activity>`要素を指定します、`IntentFilter`上、`Activity`スキームを処理し、次の例では表示します。

```xml
<application ...>
  <activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
      <action android:name="android.intent.action.VIEW" />
      <category android:name="android.intent.category.DEFAULT" />
      <category android:name="android.intent.category.BROWSABLE" />
      <data android:scheme="INSERT_URL_SCHEME_HERE" android:host="auth" />
    </intent-filter>
  </activity>
</application>
```

Azure Active Directory B2C には、承認要求が完了すると、ときに、登録されたリダイレクト URL にリダイレクトします。 によって処理される、起動パラメーターとして渡すと共に、URL の URL は、Android のモバイル アプリケーションの起動においてカスタム スキームを使用するため、`microsoft.identity.client.BrowserTabActivity`です。 なお、`data android:scheme`プロパティは、Azure Active Directory B2C のアプリケーションに登録されているカスタムの URL スキームに設定する必要があります。

さらに、`MainActivity`クラスは次のコード例に示すように、変更する必要があります。

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            global::Xamarin.Forms.Forms.Init(this, bundle);
            Microsoft.WindowsAzure.MobileServices.CurrentPlatform.Init();
            LoadApplication(new App());
            App.UiParent = new UIParent(this);
        }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
        }
    }
}

```

`OnCreate`メソッドが割り当てることによって変更された、`UIParent`インスタンスを`App.UiParent`プロパティです。 これにより、現在のアクティビティのコンテキストでの認証フローが発生することです。

内のコード、`OnActivityResult`メソッドは、認証ワークフローの対話型の部分が終了した後は、MSAL に制御が戻ることを確認します。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォームに MSAL を使用する追加の設定は必要ありません。

## <a name="initialization"></a>初期化

Microsoft 認証ライブラリのメンバーを使用する、`PublicClientApplication`認証ワークフローを開始するクラス。 サンプル アプリケーションは、宣言および初期化し、`public`この型は、という名前のプロパティ`ADB2CClient`で、`AuthenticationProvider`クラスです。 次のコード例は、このプロパティを初期化する方法を示しています。

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

登録プロセスが割り当てられているモバイル アプリケーションは、Azure Active Directory B2C テナントに登録されました、ときに、**アプリケーション ID**です。 この ID を指定する必要があります、`PublicClientApplication`コンス トラクター、と共に、`Authority`を実行するには、ベース URL が、および Azure Active Directory B2C ポリシーで構成された定数です。

## <a name="signing-in"></a>サインイン

次のスクリーン ショットでは、サンプル アプリケーションにサインイン画面が表示されます。

![](azure-ad-b2c-images/login.png "ログイン ページ")

サインインのソーシャル id プロバイダーまたはローカル アカウントを使用して、許可されます。 Microsoft、Google、Facebook、上記のように組み合わせて使用する場合、ソーシャル id プロバイダーとして他の id プロバイダーこともできます。

次のコード例は、サインイン プロセスを呼び出す方法を示しています。

```csharp
using Microsoft.Identity.Client;

public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);
    ...
}

```

`AcquireTokenAsync`メソッドは、デバイスの web ブラウザーを起動し、を通じて参照ポリシーで指定されている Azure Active Directory B2C ポリシーで定義されている認証オプションを表示、`Constants.Authority`定数。 このポリシーは、サインアップ中に、サインイン、コンシューマーを通過するエクスペリエンスと、アプリケーションでサインアップまたはサインインに成功受信クレームを定義します。

結果、`AcquireTokenAsync`メソッドの呼び出し、`AuthenticationResult`インスタンス。 認証が成功した場合、`AuthenticationResult`インスタンスは、ローカルにキャッシュされている id トークンが含まれます。 認証が成功した場合、`AuthenticationResult`インスタンスは、認証が失敗した理由を示すデータが含まれます。

サンプル アプリケーションで認証が成功した場合、`TodoList`ページに移動します。

## <a name="silent-re-authentication"></a>サイレントの再認証

ときに、`LoginPage`サンプル アプリケーションが表示されたら、任意の認証のユーザー インターフェイスを表示せずにユーザー トークンを取得しようとしましたがします。 これを実現すると、`AcquireTokenSilentAsync`メソッドを次のコード例に示されています。

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult;

    if (useSilent)
    {
        authenticationResult = await ADB2CClient.AcquireTokenSilentAsync(
            Constants.Scopes,
            GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
            Constants.Authority,
            false);
    }
    ...
}
```

`AcquireTokenSilentAsync`メソッドは、キャッシュからユーザーがサインインをしなくてユーザー トークンを取得しようとしています。 適切なトークンが前回のセッション キャッシュに存在して既にことがあるシナリオが処理されます。 トークンを取得する試行が成功した場合、`TodoList`ページに移動します。 トークンを取得する試行が成功しなかった場合は、何も起こりません、ユーザーは新しい認証ワークフローを開始する選択肢があります。

## <a name="signing-out"></a>サインアウト

次のコード例は、サインアウト プロセスを呼び出す方法を示しています。

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

これは、ローカル キャッシュからすべての認証トークンをクリアします。

## <a name="summary"></a>まとめ

この記事では、Microsoft の認証ライブラリ (MSAL) と Azure Active Directory B2C を使用して、モバイル アプリケーションにコンシューマーの id 管理を統合する方法が示されています。 Azure Active Directory B2C は、消費者向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。


## <a name="related-links"></a>関連リンク

- [AzureADB2CAuth (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft 認証ライブラリ](https://www.nuget.org/packages/Microsoft.Identity.Client)
