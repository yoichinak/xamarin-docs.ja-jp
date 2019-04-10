---
title: Xamarin.iOS で動的に通知アクション ボタン
description: Notification content の拡張機能が追加できます ios 12 には、削除、および、通知の横に表示するアクション ボタンを更新します。 このドキュメントでは、Xamarin.iOS で動的に通知アクション ボタンを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 6B34AD78-5117-42D0-B6E7-C8B4B453EAFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: 5611d673ecc7af896fd3a9e566e184e408b6b367
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870093"
---
# <a name="dynamic-notification-action-buttons-in-xamarinios"></a>Xamarin.iOS で動的に通知アクション ボタン

Ios 12 で通知できます動的に追加、削除、およびが関連付けられているアクション ボタンを更新します。 このようなカスタマイズでは、通知のコンテンツとユーザーの対話に直接関連するアクションをユーザーに提供することです。

## <a name="sample-app-redgreennotifications"></a>サンプル アプリ:RedGreenNotifications

このガイドのコード スニペットに由来します[RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)サンプル アプリは、12 iOS の通知動作設定ボタンを使用する Xamarin.iOS を使用する方法を示します。

このサンプル アプリは 2 つの種類のローカル通知を送信します。 赤と緑。
アプリの送信通知を構成した後、そのカスタム ユーザー インターフェイスを表示するのに 3D タッチを使用します。 次に、イメージを表示するのに通知の動作設定ボタンを使用します。 イメージの回転に応じて、として、**回転をリセット**ボタンが表示され、必要に応じて、表示されなくなります。

このガイドでのコード スニペットは、このサンプル アプリから取得されます。

## <a name="default-action-buttons"></a>既定のアクション ボタン

通知のカテゴリには、その既定のアクション ボタンが決定します。

作成し、アプリケーションの起動中に、通知のカテゴリを登録します。
たとえば、[サンプル アプリ](#sample-app-redgreennotifications)、`FinishedLaunching`メソッドの`AppDelegate`は次の処理します。

- 赤の通知の 1 つのカテゴリと緑の通知用に別を定義します。
- 呼び出すことによってこれらのカテゴリを登録します [`SetNotificationCategories`](xref:UserNotifications.UNUserNotificationCenter.SetNotificationCategories*)
メソッド `UNUserNotificationCenter`
- 1 つをアタッチします。 [`UNNotificationAction`](xref:UserNotifications.UNNotificationAction)
各カテゴリに

次のサンプル コードは、このしくみを示しています。

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional | UNAuthorizationOptions.ProvidesAppNotificationSettings;
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        // ...
        var rotateTwentyDegreesAction = UNNotificationAction.FromIdentifier("rotate-twenty-degrees-action", "Rotate 20°", UNNotificationActionOptions.None);

        var redCategory = UNNotificationCategory.FromIdentifier(
            "red-category",
            new UNNotificationAction[] { rotateTwentyDegreesAction },
            new string[] { },
            UNNotificationCategoryOptions.CustomDismissAction
        );

        var greenCategory = UNNotificationCategory.FromIdentifier(
            "green-category",
            new UNNotificationAction[] { rotateTwentyDegreesAction },
            new string[] { },
            UNNotificationCategoryOptions.CustomDismissAction
        );

        var set = new NSSet<UNNotificationCategory>(redCategory, greenCategory);
        center.SetNotificationCategories(set);
    });
    // ...
}
```

このコードでは、任意の通知に基づく持つ [`Content.CategoryIdentifier`](xref:UserNotifications.UNNotificationContent.CategoryIdentifier)
"red category"または「緑カテゴリ」は、既定で表示するには、 **20 ° 回転**アクション ボタンをクリックします。

## <a name="in-app-handling-of-notification-action-buttons"></a>通知アクション ボタンのアプリでの処理

`UNUserNotificationCenter` `Delegate`型のプロパティ[ `IUNUserNotificationCenterDelegate`](xref:UserNotifications.IUNUserNotificationCenterDelegate)します。

サンプル アプリで`AppDelegate`内のユーザー通知センターのデリゲートとして設定`FinishedLaunching`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = // ...
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        center.Delegate = this;
        // ...
```

次に、`AppDelegate`実装 [`DidReceiveNotificationResponse`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.DidReceiveNotificationResponse*)
アクション ボタンを処理するために次のようにタップします。

```csharp
[Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
{
    if (response.IsDefaultAction)
    {
        Console.WriteLine("ACTION: Default");
    }
    if (response.IsDismissAction)
    {
        Console.WriteLine("ACTION: Dismiss");
    }
    else
    {
        Console.WriteLine($"ACTION: {response.ActionIdentifier}");
    }

    completionHandler();
        }
```

この実装の`DidReceiveNotificationResponse`の通知を処理しない**20 ° 回転**アクション ボタンをクリックします。 代わりに、通知のコンテンツの拡張機能は、このボタンをタップを処理します。 さらに、次のセクションでは、通知アクション ボタンの処理について説明します。

## <a name="action-buttons-in-the-notification-content-extension"></a>Notification content 拡張機能の動作設定ボタン

Notification content の拡張機能には、通知のカスタム インターフェイスを定義するビュー コント ローラーが含まれています。

このビュー コント ローラーを使用できる、`GetNotificationActions`と`SetNotificationActions`メソッドで、 [`ExtensionContext`](xref:UIKit.UIViewController.ExtensionContext)
プロパティにアクセスして変更通知の動作設定ボタン。

サンプル アプリで、notification content 拡張機能のビュー コント ローラーは、既存のアクション ボタンのタップに応答する場合にのみ、動作設定ボタンを変更します。

> [!NOTE]
> コンテンツの拡張は、ビュー コント ローラーのアクション ボタンのタップに応答できる通知[ `DidReceiveNotificationResponse` ](xref:UserNotificationsUI.UNNotificationContentExtension_Extensions.DidReceiveNotificationResponse*)の一部として宣言されたメソッドは、 [IUNNotificationContentExtension](xref:UserNotificationsUI.IUNNotificationContentExtension)します。
>
> 同じ名前でも、`DidReceiveNotificationResponse`メソッド[上記で説明した](#in-app-handling-of-notification-action-buttons)、これはさまざまな方法です。
>
> Notification content の拡張機能では、ボタンのタップの処理が完了すると、それをその同じボタンのタップを処理するためにメイン アプリケーションに指示するかどうかを選択できます。 これには、これには、適切な値の渡す必要があります[UNNotificationContentExtensionResponseOption](xref:UserNotificationsUI.UNNotificationContentExtensionResponseOption)を完了ハンドラーに。
>
> - `Dismiss` 通知インターフェイスを終了することと、メイン アプリケーションがボタンのタップを処理する必要がないことを示します。
> - `DismissAndForwardAction` 通知インターフェイスを終了することと、メイン アプリケーションは、ボタンのタップにも処理する必要がありますを示します。
> - `DoNotDismiss` 通知インターフェイスを無視できませんが、メイン アプリケーションがボタンのタップを処理する必要がないことを示します。

コンテンツの拡張機能の`DidReceiveNotificationResponse`メソッドは、どのアクション ボタンがタップされたかを決定、通知のインターフェイス、および表示または非表示にイメージを回転、**リセット**アクション ボタン。

```csharp
[Export("didReceiveNotificationResponse:completionHandler:")]
public void DidReceiveNotificationResponse(UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
{
    var rotationAction = ExtensionContext.GetNotificationActions()[0];

    if (response.ActionIdentifier == "rotate-twenty-degrees-action")
    {
        rotationButtonTaps += 1;

        double radians = (20 * rotationButtonTaps) * (2 * Math.PI / 360.0);
        Xamagon.Transform = CGAffineTransform.MakeRotation((float)radians);

        // 9 rotations * 20 degrees = 180 degrees. No reason to
        // show the reset rotation button when the image is half
        // or fully rotated.
        if (rotationButtonTaps % 9 == 0)
        {
            ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction });
        }
        else if (rotationButtonTaps % 9 == 1)
        {
            var resetRotationAction = UNNotificationAction.FromIdentifier("reset-rotation-action", "Reset rotation", UNNotificationActionOptions.None);
            ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction, resetRotationAction });
        }
    }

    if (response.ActionIdentifier == "reset-rotation-action")
    {
        rotationButtonTaps = 0;

        double radians = (20 * rotationButtonTaps) * (2 * Math.PI / 360.0);
        Xamagon.Transform = CGAffineTransform.MakeRotation((float)radians);

        ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction });
    }

    completionHandler(UNNotificationContentExtensionResponseOption.DoNotDismiss);
}
```

この場合、メソッドに渡します`UNNotificationContentExtensionResponseOption.DoNotDismiss`を完了ハンドラーにします。 通知のインターフェイスが開いたまま、つまりこれです。

## <a name="related-links"></a>関連リンク

- [サンプル アプリ – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Xamarin.iOS でのユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [実践的な通知の種類を宣言します。](https://developer.apple.com/documentation/usernotifications/declaring_your_actionable_notification_types?language=objc)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [ユーザーへの通知 (WWDC 2018) の新機能新機能](https://developer.apple.com/videos/play/wwdc2018/710/)
- [ユーザーへの通知 (WWDC 2017) の新機能新機能およびベスト プラクティス](https://developer.apple.com/videos/play/wwdc2017/708/)
- [リモート通知 (Apple) を生成します。](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
