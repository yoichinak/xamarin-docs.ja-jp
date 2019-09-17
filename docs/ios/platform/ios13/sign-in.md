---
title: Xamarin で Apple にサインインします。 iOS
description: Apple でサインインすると、サードパーティの認証サービスを使用するときに id 保護が提供されます。
ms.prod: xamarin
ms.assetid: CDA59BBF-AAE1-4D4F-B4AE-DB37220D41EB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/10/2019
ms.openlocfilehash: a8ea06d81fcc79a24f155a1562818daea3ba982a
ms.sourcegitcommit: 13e43f510da37ad55f1c2f5de1913fb0aede6362
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2019
ms.locfileid: "71021396"
---
# <a name="sign-in-with-apple-in-xamarinios"></a>Xamarin で Apple にサインインします。 iOS

![この API は現在プレビューの段階です](~/media/shared/preview.png)

Apple でのサインインは、サードパーティの認証サービスのユーザーの id 保護を提供する新しいサービスです。 IOS 13 以降では、サードパーティの認証サービスを使用する新しいアプリでも、Apple でのサインインを提供する必要があります。 更新中の既存のアプリは、2020年4月まで Apple にサインインを追加する必要はありません。

このドキュメントでは、Apple for iOS 13 アプリケーションにサインインを追加する方法について説明します。

## <a name="requirements"></a>必要条件

この機能には次のものが必要です。

* iOS 13
* Xamarin. iOS 12.99
* Xcode 11 をサポートする visual Studio 2019 または Visual Studio 2019 for Mac。

詳細[につい](get-started.md)ては、「はじめに」を参照してください。

## <a name="apple-developer-setup"></a>Apple developer セットアップ

Apple でサインインしてアプリをビルドして実行する前に、次の手順を完了する必要があります。 [Apple Developer 証明書では、識別子 & プロファイル][5]ポータル:

1. 新しい**アプリ id**識別子を作成します。
2. **[説明]** フィールドに説明を設定します。
3. **明示的**なバンドル ID を選択`com.xamarin.AddingTheSignInWithAppleFlowToYourApp`し、フィールドにを設定します。
4. **Apple 機能でのサインイン**を有効にし、新しい id を登録します。
5. 新しい Id で新しいプロビジョニングプロファイルを作成します。
6. デバイスにダウンロードしてインストールします。
7. Visual Studio で、「Apple の機能**を使用してサインイン**する **」ファイルを**有効にします。

## <a name="check-sign-in-status"></a>サインインの状態を確認する

アプリが開始されるとき、またはユーザーの認証状態を最初に確認する必要がある`ASAuthorizationAppleIdProvider`ときに、をインスタンス化し、現在の状態を確認します。

```csharp
var appleIdProvider = new ASAuthorizationAppleIdProvider ();
appleIdProvider.GetCredentialState (KeychainItem.CurrentUserIdentifier, (credentialState, error) => {
    switch (credentialState) {
    case ASAuthorizationAppleIdProviderCredentialState.Authorized:
        // The Apple ID credential is valid.
        break;
    case ASAuthorizationAppleIdProviderCredentialState.Revoked:
        // The Apple ID credential is revoked.
        break;
    case ASAuthorizationAppleIdProviderCredentialState.NotFound:
        // No credential was found, so show the sign-in UI.
        InvokeOnMainThread (() => {
            var storyboard = UIStoryboard.FromName ("Main", null);

            if (!(storyboard.InstantiateViewController (nameof (LoginViewController)) is LoginViewController viewController))
                return;

            viewController.ModalPresentationStyle = UIModalPresentationStyle.FormSheet;
            viewController.ModalInPresentation = true;
            Window?.RootViewController?.PresentViewController (viewController, true, null);
        });
        break;
    }
});
```

このコードでは、の`FinishedLaunching` `AppDelegate.cs`中で、 `LoginViewController`状態が`NotFound`である場合にアプリが処理され、ユーザーにが表示されます。 状態がまたは`Authorized` `Revoked`を返した場合は、ユーザーに別のアクションが表示されることがあります。

## <a name="a-loginviewcontroller-for-sign-in-with-apple"></a>Apple でサインインするための LoginViewController

ログインロジックを実装し、次の`LoginViewController`例に示すように、 `IASAuthorizationControllerDelegate` Apple `IASAuthorizationControllerPresentationContextProviding`によるサインインで、とを実装する必要がある。`UIViewController`

```csharp
public partial class LoginViewController : UIViewController, IASAuthorizationControllerDelegate, IASAuthorizationControllerPresentationContextProviding {
    public LoginViewController (IntPtr handle) : base (handle)
    {
    }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.

        SetupProviderLoginView ();
    }

    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        PerformExistingAccountSetupFlows ();
    }

    void SetupProviderLoginView ()
    {
        var authorizationButton = new ASAuthorizationAppleIdButton (ASAuthorizationAppleIdButtonType.Default, ASAuthorizationAppleIdButtonStyle.White);
        authorizationButton.TouchUpInside += HandleAuthorizationAppleIDButtonPress;
        loginProviderStackView.AddArrangedSubview (authorizationButton);
    }

    // Prompts the user if an existing iCloud Keychain credential or Apple ID credential is found.
    void PerformExistingAccountSetupFlows ()
    {
        // Prepare requests for both Apple ID and password providers.
        ASAuthorizationRequest [] requests = {
            new ASAuthorizationAppleIdProvider ().CreateRequest (),
            new ASAuthorizationPasswordProvider ().CreateRequest ()
        };

        // Create an authorization controller with the given requests.
        var authorizationController = new ASAuthorizationController (requests);
        authorizationController.Delegate = this;
        authorizationController.PresentationContextProvider = this;
        authorizationController.PerformRequests ();
    }

    private void HandleAuthorizationAppleIDButtonPress (object sender, EventArgs e)
    {
        var appleIdProvider = new ASAuthorizationAppleIdProvider ();
        var request = appleIdProvider.CreateRequest ();
        request.RequestedScopes = new [] { ASAuthorizationScope.Email, ASAuthorizationScope.FullName };

        var authorizationController = new ASAuthorizationController (new [] { request });
        authorizationController.Delegate = this;
        authorizationController.PresentationContextProvider = this;
        authorizationController.PerformRequests ();
    }
}
```

![Apple でのサインインを使用したサンプルアプリのアニメーション](sign-in-images/sign-in-flow.png)

このコード例では、の`PerformExistingAccountSetupFlows`現在のログインの状態を確認し、現在のビューをデリゲートとして接続します。 既存の iCloud キーチェーン資格情報または Apple ID 資格情報が見つかった場合、ユーザーはそれを使用するように求められます。

Apple で`ASAuthorizationAppleIdButton`は、この目的に特化したボタンを提供しています。 このボタンをクリックすると、メソッド`HandleAuthorizationAppleIDButtonPress`で処理されたワークフローがトリガーされます。

## <a name="handling-authorization"></a>承認の処理

で、 `IASAuthorizationController`ユーザーのアカウントを格納するためのカスタムロジックを実装します。 次の例では、ユーザーのアカウントをキーチェーン (Apple の独自のストレージサービス) に格納します。

```csharp
#region IASAuthorizationController Delegate

[Export ("authorizationController:didCompleteWithAuthorization:")]
public void DidComplete (ASAuthorizationController controller, ASAuthorization authorization)
{
    if (authorization.GetCredential<ASAuthorizationAppleIdCredential> () is ASAuthorizationAppleIdCredential appleIdCredential) {
        var userIdentifier = appleIdCredential.User;
        var fullName = appleIdCredential.FullName;
        var email = appleIdCredential.Email;

        // Create an account in your system.
        // For the purpose of this demo app, store the userIdentifier in the keychain.
        try {
            new KeychainItem ("com.example.apple-samplecode.juice", "userIdentifier").SaveItem (userIdentifier);
        } catch (Exception) {
            Console.WriteLine ("Unable to save userIdentifier to keychain.");
        }

        // For the purpose of this demo app, show the Apple ID credential information in the ResultViewController.
        if (!(PresentingViewController is ResultViewController viewController))
            return;

        InvokeOnMainThread (() => {
            viewController.UserIdentifierText = userIdentifier;
            viewController.GivenNameText = fullName?.GivenName ?? "";
            viewController.FamilyNameText = fullName?.FamilyName ?? "";
            viewController.EmailText = email ?? "";

            DismissViewController (true, null);
        });
    } else if (authorization.GetCredential<ASPasswordCredential> () is ASPasswordCredential passwordCredential) {
        // Sign in using an existing iCloud Keychain credential.
        var username = passwordCredential.User;
        var password = passwordCredential.Password;

        // For the purpose of this demo app, show the password credential as an alert.
        InvokeOnMainThread (() => {
            var message = $"The app has received your selected credential from the keychain. \n\n Username: {username}\n Password: {password}";
            var alertController = UIAlertController.Create ("Keychain Credential Received", message, UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("Dismiss", UIAlertActionStyle.Cancel, null));

            PresentViewController (alertController, true, null);
        });
    }
}

[Export ("authorizationController:didCompleteWithError:")]
public void DidComplete (ASAuthorizationController controller, NSError error)
{
    Console.WriteLine (error);
}

#endregion
```

## <a name="authorization-controller"></a>承認コントローラー

この実装の最後の部分は、 `ASAuthorizationController`プロバイダーの承認要求を管理するです。

```csharp
#region IASAuthorizationControllerPresentation Context Providing

public UIWindow GetPresentationAnchor (ASAuthorizationController controller) => View.Window;

#endregion
```

## <a name="summary"></a>Summary

この記事では、iOS 用 Apple でのサインインについて紹介しました。

## <a name="related-links"></a>関連リンク

* [Apple でのサインインに関するガイドライン](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)
* [Apple の権利を使用してサインインします。][2]
* [WWDC 2019 セッション 706:Apple でのサインインの概要をご紹介します。][3]
* [Xamarin 用の Apple でのサインインの設定][4]

[1]: https://developer.apple.com/documentation/authenticationservices/adding_the_sign_in_with_apple_flow_to_your_app
[2]: https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_applesignin
[3]: https://developer.apple.com/videos/play/wwdc19/706/
[4]: ~/xamarin-forms/platform/sign-in-with-apple/setup.md
[5]: https://developer.apple.com/account/resources/identifiers/list
