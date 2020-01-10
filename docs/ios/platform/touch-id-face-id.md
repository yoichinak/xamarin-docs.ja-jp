---
title: Xamarin.iOS で Touch ID と Face ID を使用する
description: このドキュメントでは、iOS での生体認証の概要について説明します。
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: profexorgeek
ms.author: jusjohns
ms.date: 12/16/2019
ms.openlocfilehash: 744a07343b9da87f196c0664f57b7d950844ab04
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490428"
---
# <a name="use-touch-id-and-face-id-with-xamarinios"></a>Xamarin で Touch ID と Face ID を使用する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-faceidsample/)

iOS では、次の2つの生体認証システムがサポートされます。

1. **Touch ID** では、[ホーム] ボタンの下に指紋センサーが使用されます。
1. **Face ID** は、顔スキャンを使用してユーザーを認証するために、正面カメラセンサーを使用します。

Touch ID は iOS 7 で、Face ID は iOS 11 で導入されました。

これらの認証システムは、_セキュリティで保護さ_れたエンクレーブと呼ばれるハードウェアベースのセキュリティプロセッサに依存します。 セキュリティで保護されたエンクレーブは、顔と指紋のデータの数学的表現を暗号化し、この情報を使用してユーザーを認証する役割を担います。 Apple、顔、および指紋データはデバイスから離れていないため、iCloud にバックアップされません。 アプリは、_ローカル認証_API を介して Secure エンクレーブと対話します。また、顔や指紋データを取得したり、secure エンクレーブに直接アクセスしたりすることはできません。

Touch ID と Face ID は、保護されたコンテンツへのアクセスを提供する前に、アプリがユーザーを認証するために使用できます。

## <a name="local-authentication-context"></a>ローカル認証コンテキスト

IOS での生体認証は、`LAContext` クラスのインスタンスである_ローカル認証コンテキスト_オブジェクトに依存します。 `LAContext` クラスを使用すると、次のことができます。

- 生体認証ハードウェアが使用可能かどうかを確認します。
- 認証ポリシーを評価します。
- アクセス制御を評価します。
- 認証プロンプトをカスタマイズして表示します。
- 認証状態を再利用または無効にします。
- 資格情報を管理します。

## <a name="detect-available-authentication-methods"></a>使用可能な認証方法の検出

サンプルプロジェクトには、`AuthenticationViewController`によってサポートされる `AuthenticationView` が含まれています。 このクラスは、使用可能な認証方法を検出するために `ViewWillAppear` メソッドをオーバーライドします。

```csharp
partial class AuthenticationViewController: UIViewController
{
    // ...
    string BiometryType = "";

    public override void ViewWillAppear(bool animated)
    {
        base.ViewWillAppear(animated);
        unAuthenticatedLabel.Text = "";
    
        var context = new LAContext();
        var buttonText = "";

        // Is login with biometrics possible?
        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out var authError1))
        {
            // has Touch ID or Face ID
            if (UIDevice.CurrentDevice.CheckSystemVersion(11, 0))
            {
                context.LocalizedReason = "Authorize for access to secrets"; // iOS 11
                BiometryType = context.BiometryType == LABiometryType.TouchId ? "Touch ID" : "Face ID";
                buttonText = $"Login with {BiometryType}";
            }
            // No FaceID before iOS 11
            else
            {
                buttonText = $"Login with Touch ID";
            }
        }

        // Is pin login possible?
        else if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, out var authError2))
        {
            buttonText = $"Login"; // with device PIN
            BiometryType = "Device PIN";
        }

        // Local authentication not possible
        else
        {
            // Application might choose to implement a custom username/password
            buttonText = "Use unsecured";
            BiometryType = "none";
        }
        AuthenticateButton.SetTitle(buttonText, UIControlState.Normal);
    }
}
```

`ViewWillAppear` メソッドは、UI をユーザーに表示しようとしているときに呼び出されます。 このメソッドは、`LAContext` の新しいインスタンスを定義し、`CanEvaluatePolicy` メソッドを使用して、生体認証が有効かどうかを判断します。 その場合は、システムのバージョンを確認し、`BiometryType` 列挙型を使用して、利用可能な生体認証オプションを判別します。

生体認証が有効になっていない場合、アプリは PIN 認証にフォールバックしようとします。 生体認証と PIN 認証のいずれも使用できない場合、デバイスの所有者はセキュリティ機能を有効にせず、コンテンツをローカル認証で保護することはできません。

## <a name="authenticate-a-user"></a>ユーザーを認証する

サンプルプロジェクトの `AuthenticationViewController` には、ユーザーの認証を担当する `AuthenticateMe` メソッドが含まれています。

```csharp
partial class AuthenticationViewController: UIViewController
{
    // ...
    string BiometryType = "";

    partial void AuthenticateMe(UIButton sender)
    {
        var context = new LAContext();
        NSError AuthError;
        var localizedReason = new NSString("To access secrets");
    
        // Because LocalAuthentication APIs have been extended over time,
        // you must check iOS version before setting some properties
        context.LocalizedFallbackTitle = "Fallback";
    
        if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
        {
            context.LocalizedCancelTitle = "Cancel";
        }
        if (UIDevice.CurrentDevice.CheckSystemVersion(11, 0))
        {
            context.LocalizedReason = "Authorize for access to secrets";
            BiometryType = context.BiometryType == LABiometryType.TouchId ? "TouchID" : "FaceID";
        }
    
        // Check if biometric authentication is possible
        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError))
        {
            replyHandler = new LAContextReplyHandler((success, error) =>
            {
                // This affects UI and must be run on the main thread
                this.InvokeOnMainThread(() =>
                {
                    if (success)
                    {
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        unAuthenticatedLabel.Text = $"{BiometryType} Authentication Failed";
                    }
                });
    
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, localizedReason, replyHandler);
        }

        // Fall back to PIN authentication
        else if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, out AuthError))
        {
            replyHandler = new LAContextReplyHandler((success, error) =>
            {
                // This affects UI and must be run on the main thread
                this.InvokeOnMainThread(() =>
                {
                    if (success)
                    {
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        unAuthenticatedLabel.Text = "Device PIN Authentication Failed";
                        AuthenticateButton.Hidden = true;
                    }
                });
    
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, localizedReason, replyHandler);
        }

        // User hasn't configured any authentication: show dialog with options
        else
        {
            unAuthenticatedLabel.Text = "No device auth configured";
            var okCancelAlertController = UIAlertController.Create("No authentication", "This device does't have authentication configured.", UIAlertControllerStyle.Alert);
            okCancelAlertController.AddAction(UIAlertAction.Create("Use unsecured", UIAlertActionStyle.Default, alert => PerformSegue("AuthenticationSegue", this)));
            okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine("Cancel was clicked")));
            PresentViewController(okCancelAlertController, true, null);
        }
    } 
}
```

`AuthenticateMe` メソッドは、ユーザーが **[ログイン]** ボタンをタップしたときに呼び出されます。 新しい `LAContext` オブジェクトがインスタンス化され、デバイスのバージョンがチェックされ、ローカル認証コンテキストに設定するプロパティが決定されます。

`CanEvaluatePolicy` メソッドを呼び出して、生体認証が有効になっているかどうかを確認し、可能であれば PIN 認証にフォールバックします。また、認証が使用できない場合は、セキュリティで保護されていないモードを提供します。 認証方法が使用可能な場合、UI を表示して認証プロセスを完了するために、`EvaluatePolicy` メソッドが使用されます。

サンプルプロジェクトには、認証が成功した場合にデータを表示するためのモックデータとビューが含まれています。

## <a name="related-links"></a>関連リンク

- [Touch ID または Face ID を使用したローカル認証のサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-faceidsample/)
- Support.apple.com の[Touch ID について](https://support.apple.com/en-us/HT204587)
- Support.apple.com の[Face ID について](https://support.apple.com/en-us/HT208108)
