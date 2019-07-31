---
title: Xamarin. iOS の動的通知アクションボタン
description: IOS 12 では、通知コンテンツの拡張機能を使用して、通知と共に表示されるアクションボタンを追加、削除、更新できます。 このドキュメントでは、Xamarin iOS で動的通知アクションボタンを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 6B34AD78-5117-42D0-B6E7-C8B4B453EAFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: 1a4380e321035b8948f9b40bdce052161025d5f3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652580"
---
# <a name="dynamic-notification-action-buttons-in-xamarinios"></a>Xamarin. iOS の動的通知アクションボタン

IOS 12 では、通知によって、関連付けられているアクションボタンを動的に追加、削除、および更新できます。 このようなカスタマイズにより、通知のコンテンツに直接関連するアクションをユーザーに提供できるようになり、ユーザーとの対話が可能になります。

## <a name="sample-app-redgreennotifications"></a>サンプルアプリ:RedGreenNotifications

このガイドのコードスニペットは、 [RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)サンプルアプリから抜粋したものです。 ios 12 では、Xamarin を使用して通知アクションボタンを操作する方法を示しています。

このサンプルアプリでは、赤と緑の2種類のローカル通知を送信します。
アプリが通知を送信したら、3D タッチを使用してそのカスタムユーザーインターフェイスを表示します。 次に、通知の [アクション] ボタンを使用して、表示される画像を回転します。 画像が回転すると、 **[回転のリセット]** ボタンが表示され、必要に応じて消えます。

このガイドのコードスニペットは、このサンプルアプリから抜粋したものです。

## <a name="default-action-buttons"></a>既定のアクションボタン

通知のカテゴリによって、既定のアクションボタンが決まります。

アプリケーションの起動時に、通知カテゴリを作成して登録します。
たとえば、[サンプルアプリ](#sample-app-redgreennotifications) `FinishedLaunching`では、の`AppDelegate`メソッドは次のことを実行します。

- 赤の通知に1つのカテゴリを定義し、緑の通知に対して別のカテゴリを定義します
- を呼び出して、これらのカテゴリを登録します。[`SetNotificationCategories`](xref:UserNotifications.UNUserNotificationCenter.SetNotificationCategories*)
のメソッド`UNUserNotificationCenter`
- 1つのをアタッチします[`UNNotificationAction`](xref:UserNotifications.UNNotificationAction)
各カテゴリに

次のサンプルコードは、このしくみを示しています。

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

このコードに基づいて、[`Content.CategoryIdentifier`](xref:UserNotifications.UNNotificationContent.CategoryIdentifier)
"red category" または "green-category" は、既定では **[20 °回転]** アクションボタンを表示します。

## <a name="in-app-handling-of-notification-action-buttons"></a>通知アクションボタンのアプリ内処理

`UNUserNotificationCenter`に[型`IUNUserNotificationCenterDelegate`](xref:UserNotifications.IUNUserNotificationCenterDelegate)のプロパティがあります。 `Delegate`

サンプルアプリでは、 `AppDelegate`は、で`FinishedLaunching`ユーザー通知センターのデリゲートとして自身を設定します。

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

次に`AppDelegate` 、を実装します。[`DidReceiveNotificationResponse`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.DidReceiveNotificationResponse*)
アクションボタンのタップを処理するには:

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

の`DidReceiveNotificationResponse`この実装は、通知の **[20 °回転]** アクションボタンを処理しません。 代わりに、通知のコンテンツ拡張機能によって、このボタンのタップが処理されます。 次のセクションでは、通知アクションボタンの処理について詳しく説明します。

## <a name="action-buttons-in-the-notification-content-extension"></a>Notification content 拡張機能のアクションボタン

通知コンテンツ拡張機能には、通知のカスタムインターフェイスを定義するビューコントローラーが含まれています。

このビューコントローラーは`GetNotificationActions` `SetNotificationActions` 、その[`ExtensionContext`](xref:UIKit.UIViewController.ExtensionContext)
通知のアクションボタンにアクセスして変更するためのプロパティです。

サンプルアプリでは、通知コンテンツ拡張機能のビューコントローラーは、既に存在するアクションボタンの tap に応答する場合にのみ、アクションボタンを変更します。

> [!NOTE]
> 通知コンテンツの拡張機能は、ビューコントローラーの[`DidReceiveNotificationResponse`](xref:UserNotificationsUI.UNNotificationContentExtension_Extensions.DidReceiveNotificationResponse*)メソッドで、 [iunnotificationcontentextension](xref:UserNotificationsUI.IUNNotificationContentExtension)の一部として宣言されているアクションボタンをタップすると応答できます。
>
> `DidReceiveNotificationResponse`前に[説明](#in-app-handling-of-notification-action-buttons)した方法で名前を共有しますが、これは別の方法です。
>
> 通知コンテンツの拡張機能がボタンのタップの処理を終了したら、メインアプリケーションに対して同じボタンのタップを処理するように指示するかどうかを選択できます。 これを行うには、次のように、 [Unnotificationcontentextensionresponseoption](xref:UserNotificationsUI.UNNotificationContentExtensionResponseOption)の適切な値を完了ハンドラーに渡す必要があります。
>
> - `Dismiss`通知インターフェイスを破棄する必要があり、メインアプリがボタンのタップを処理する必要がないことを示します。
> - `DismissAndForwardAction`通知インターフェイスを破棄する必要があり、メインアプリもボタンのタップを処理する必要があることを示します。
> - `DoNotDismiss`通知インターフェイスを破棄しないこと、およびメインアプリがボタンのタップを処理する必要がないことを示します。

コンテンツ拡張機能の`DidReceiveNotificationResponse`メソッドは、タップされたアクションボタンを判別し、通知のインターフェイスで画像を回転し、 **[リセット]** アクションボタンの表示と非表示を切り替えます。

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

この場合、メソッドは完了ハンドラー `UNNotificationContentExtensionResponseOption.DoNotDismiss`にを渡します。 これは、通知のインターフェイスが開いたままになることを意味します。

## <a name="related-links"></a>関連リンク

- [サンプルアプリ– RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Xamarin. iOS のユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [実行可能な通知の種類の宣言](https://developer.apple.com/documentation/usernotifications/declaring_your_actionable_notification_types?language=objc)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [ユーザー通知の新機能 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [ベストプラクティスとユーザー通知の新機能 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [リモート通知の生成 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
