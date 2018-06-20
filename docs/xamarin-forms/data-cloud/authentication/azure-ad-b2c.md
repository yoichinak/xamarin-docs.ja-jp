---
title: Azure Active Directory B2C のユーザーの認証
description: Azure Active Directory B2C は、コンシューマー向けの web アプリケーションとモバイル アプリケーションのクラウド ID 管理ソリューションです。 この記事では、Microsoft Authentication Library と Azure Active Directory B2C を使用して、モバイル アプリケーションにコンシューマーの ID 管理を統合する方法を示します。
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 627c6773c099c9cf45f871a9bb73a201bf98271a
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
ms.locfileid: "32020340"
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Azure Active Directory B2C のユーザーの認証

_Azure Active Directory B2C は、コンシューマー向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。この記事では、Microsoft Authentication Library と Azure Active Directory B2C を使用して、モバイル アプリケーションにコンシューマーの id 管理を統合する方法を示します。_

![](~/media/shared/preview.png "この API は、現在プレリリースです")

> [!NOTE]
> [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client)は、まだプレビュー段階ですが、運用環境での使用に適しています。 ただし、ライブラリのAPIや内部のキャッシュ形式、およびその他のメカニズムに破壊的変更の可能性があり、アプリケーションに重大な影響を与える可能性があります。

## <a name="overview"></a>概要

Azure Active Directory B2C は、コンシューマー向けのアプリケーションのための ID 管理サービスで、コンシューマーがアプリケーションに次のような方法でサインインできるようにします。

- 既存のソーシャル アカウント (Microsoft、Google、Facebook、Amazon、LinkedIn) を使用する。
- 新しい資格情報 (電子メール アドレスとパスワード、またはユーザー名とパスワード) を作成する。 これらの資格情報は *ローカル* アカウント といわれます。

モバイル アプリケーションに Azure Active Directory B2C ID 管理サービスを統合するための手順は次のとおりです。

1. Azure Active Directory B2C テナントを作成します。 詳細については、[Azure ポータルで Azure Active Directory B2C テナントを作成する](/azure/active-directory-b2c/active-directory-b2c-get-started/) を参照してください。
1. Azure Active Directory B2C テナントに、モバイル アプリケーションを登録します。 登録処理では、アプリケーションを一意に識別する **アプリケーション ID** やアプリケーションへ戻って応答を伝えるために使用できる **リダイレクト URL** を割り当てます。 詳細については、[Azure Active Directory B2C: アプリケーションの登録](/azure/active-directory-b2c/active-directory-b2c-app-registration/) を参照してください。
1. サインアップとサインインのポリシーを作成します。 このポリシーは、コンシューマーがサインアップとサインインの間に通過するエクスペリエンスを定義し、またアプリケーションがサインアップやサインインの成功で受け取るトークンの内容を指定します。 詳細については、[Azure Active Directory B2C: 組み込みポリシー](/azure/active-directory-b2c/active-directory-b2c-reference-policies/) を参照してください。
1. [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) をモバイルアプリケーションで使用して、Azure Active Directory B2C テナントを使った認証ワークフローを開始します。

> [!NOTE]
> モバイル アプリケーションへの Azure Active Directory B2C の ID 管理の統合だけでなく、モバイル アプリケーションへの Azure Active Directory の ID 管理の統合にも MSAL を使用することができます。 これは、[アプリケーション登録ポータル](https://apps.dev.microsoft.com/) でモバイル アプリケーションを Azure Active Directory に登録することによって可能です。 登録処理では、アプリケーションを一意に識別する **アプリケーション ID** を割り当て、それを MSAL を使用する際に指定する必要があります。 詳細については、[v2.0 エンドポイントでアプリを登録する方法](/azure/active-directory/develop/active-directory-v2-app-registration/)、および [Microsoft Authentication Library を使ったモバイル アプリ認証](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) Xamarin ブログを参照してください。

MSAL では、デバイスの web ブラウザーを使用して、認証を行います。 これにより、ユーザーは各デバイスにつき1回のサインインで済むようになるので、アプリケーションのユーザビリティが向上します。またアプリケーションのサインインと認証フローのコンバージョンレートが向上します。 デバイスのブラウザーでは、強化されたセキュリティも提供します。 ユーザーが認証プロセスを完了させると、web ブラウザーのタブからアプリケーションに制御が返されます。これは、認証プロセスから返されるリダイレクト URL にカスタム URL スキームを登録することによって実現されます。そして、その送信されたカスタム URL を検知して処理します。 カスタム URL スキームの選択の詳細については、[ネイティブ アプリのリダイレクト URI を選択する](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/) を参照してください。

> [!NOTE]
> オペレーティング システムにカスタム URL スキームを登録するメカニズムと、そのスキームを処理するためのメカニズムは、各プラットフォームに固有です。

Azure Active Directory B2C テナントに送信される各要求には *ポリシー* を指定します。 ポリシーでは、サインアップまたはサインインなどのコンシューマー ID エクスペリエンスを記述します。 たとえば、サインアップ ポリシーは、Azure Active Directory B2C テナントの動作を次の設定を使用して構成することができます。

- コンシューマーが、アプリケーションへのサインインに使用できるアカウントの種類。
- サインアップ時に、コンシューマーから収集するデータ。
- 多要素認証。
- サインアップ ページのコンテンツ。
- ポリシーが実行されたときに、モバイル アプリケーションが受け取るトークンの要求。

Azure Active Directory テナントには、さまざまな種類の複数のポリシーを含めることができ、必要に応じて、アプリケーションで使用することができます。 さらに、ポリシーは、複数のアプリケーションで再利用でき、コードを変更せずにコンシューマー ID エクスペリエンスの定義や変更をすることができます。 ポリシーの詳細については、[Azure Active Directory B2C: 組み込みポリシー](/azure/active-directory-b2c/active-directory-b2c-reference-policies/)を参照してください。

## <a name="setup"></a>セットアップ

Microsoft Authentication Library (MSAL) NuGet ライブラリは、Xamarin.Forms ソリューション内のポータブル クラス ライブラリ (PCL) およびプラットフォーム プロジェクトに追加する必要があります。 次のセクションでは、MSAL を使用して、モバイル アプリケーションから Azure Active Directory B2C テナントに通信するための追加のセットアップ手順を説明します。

### <a name="portable-class-library"></a>ポータブル クラス ライブラリ

MSAL を使用する PCL には、Profile7 を使用して再ターゲットする必要があります。 PCL の詳細については、「[ポータブル クラス ライブラリの概要](~/cross-platform/app-fundamentals/pcl.md)」(ポータブル クラス ライブラリの概要) を参照してください。

### <a name="ios"></a>iOS

Ios の場合は、Azure Active Directory B2C に登録されているカスタムの URL スキームに登録する必要があります**Info.plist**の次のスクリーン ショットに示すようにします。

![](azure-ad-b2c-images/customurl-ios.png "iOS でカスタム URL スキームを登録する。")

Azure Active Directory B2C が認証要求を完了させると、登録されたリダイレクト URL にリダイレクトします。 その URL はカスタム スキームを使用しているため、iOS がモバイル アプリケーションを起動した時に、起動パラメータとしてその URL が渡され、それをアプリケーションの `AppDelegate` クラスの `OpenUrl` メソッドのオーバーライドによって処理するということになります。それを次のコードサンプルで示します。

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

`OpenURL` メソッド内のコードは、認証ワークフローの対話型の部分が終了した後は、MSAL に制御が戻るようにしています。

### <a name="android"></a>Android

Android では、Azure Active Directory B2C に登録されているカスタム URL スキームを **AndroidManifest.xml** に登録する必要があります。それは、既存の `<application>` 要素内に `<activity>` 要素を追加することによって行います。 `<activity>` 要素には、そのスキームを処理する `Activity` 上の `IntentFilter` を指定します。次の例でそれを示します。

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

Azure Active Directory B2C が認証要求を完了させると、登録されたリダイレクト URL にリダイレクトします。 その URL はカスタム スキームを使用しているため、Android がモバイル アプリケーションを起動した時に、起動パラメータとしてその URL が渡され、それを `microsoft.identity.client.BrowserTabActivity` によって処理するということになります。 なお、`data android:scheme` プロパティは、Azure Active Directory B2C のアプリケーションに登録されているカスタム URL スキームを設定する必要があります。

さらに、`MainActivity` クラスは次のコードサンプルのように修正する必要があります。

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

`OnCreate` メソッドは、`App.UiParent` プロパティに `UIParent` インスタンスを割り当てることによって変更します。 これにより、現在のアクティビティのコンテキストで認証フローが発生するようにします。

`OnActivityResult` メソッド内のコードは、認証ワークフローの対話型の部分が終了した後は、MSAL に制御が戻るようにしています。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

Universal Windows Platform に MSAL を使用する追加の設定は必要ありません。

## <a name="initialization"></a>初期化

Microsoft Authentication Library は、`PublicClientApplication` クラスのメンバーを使用して、認証ワークフローを開始します。 サンプル アプリケーションは、宣言および初期化し、`public`この型は、という名前のプロパティ`ADB2CClient`で、`AuthenticationProvider`クラスです。 次のコードサンプルは、このプロパティを初期化する方法を示しています。

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

モバイル アプリケーションを Azure Active Directory B2C テナントに登録したときに、その登録処理で **アプリケーション ID** を割り当てました。 この ID を `PublicClientApplication` のコンストラクタで、実行するための base URL と Azure Active Directory B2C ポリシーで構成された `Authority` 定数と共に指定する必要があります。

## <a name="signing-in"></a>サインイン

次のスクリーン ショットでは、サンプル アプリケーションのサインイン画面を示しています。

![](azure-ad-b2c-images/login.png "ログイン ページ")

サインインのソーシャル id プロバイダーまたはローカル アカウントを使用して、許可されます。 Microsoft、Google、Facebook、上記のように組み合わせて使用する場合、ソーシャル id プロバイダーとして他の id プロバイダーこともできます。

次のコードサンプルは、サインイン処理を呼び出す方法を示しています。

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

`AcquireTokenAsync`メソッドは、デバイスの web ブラウザーを起動し、`Constants.Authority` 定数を通じて参照されたポリシーによって指定されている Azure Active Directory B2C ポリシーで定義済みの認証オプションを表示します。 このポリシーは、サインアップやサインイン中にコンシューマーが通過するエクスペリエンスやアプリケーションがサインアップまたはサインインに成功したときに受信する要求が定義されています。

`AcquireTokenAsync` メソッドの呼び出しの結果は、`AuthenticationResult` インスタンスを返します。 認証が成功した場合、`AuthenticationResult` インスタンスには ID トークンが含まれ、それはローカルにキャッシュされます。 認証が失敗した場合、`AuthenticationResult` インスタンスには、認証が失敗した理由を示すデータが含まれます。

サンプル アプリケーションでは、認証が成功した場合、`TodoList`ページに移動します。

## <a name="silent-re-authentication"></a>サイレント 再認証

サンプル アプリケーションの `LoginPage` が表示されたとき、認証のユーザーインターフェイスを何も表示せずにユーザートークンを取得しよう試みます。 これは次のコードサンプルで示すように `AcquireTokenSilentAsync` メソッド を使って実現できます。

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

`AcquireTokenSilentAsync`メソッドは、サインインをユーザーに要求することなく、キャッシュからユーザートークンを取得することを試みます。 これは適切なトークンが前回のセッションから既にキャッシュに存在しているというシナリオを処理します。 トークンの取得に成功した場合、`TodoList` ページに移動します。 トークンの取得に成功しなかった場合は、何も起こらず、ユーザーは新しい認証ワークフローを開始する選択肢を持ちます。

## <a name="signing-out"></a>サインアウト

次のコードサンプルは、サインアウト処理を呼び出す方法を示しています。

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

これは、ローカル キャッシュからすべての認証トークンを削除します。

## <a name="summary"></a>まとめ

この記事では、Microsoft Authentication Library (MSAL) と Azure Active Directory B2C を使用して、モバイル アプリケーションにコンシューマーの ID 管理を統合する方法を説明しました。 Azure Active Directory B2C は、コンシューマー向けの web アプリケーションとモバイル アプリケーションのクラウド ID 管理ソリューションです。


## <a name="related-links"></a>関連リンク

- [AzureADB2CAuth (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft 認証ライブラリ](https://www.nuget.org/packages/Microsoft.Identity.Client)
