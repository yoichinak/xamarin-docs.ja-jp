---
title: Xamarin.iOS での通知の管理
description: このドキュメントでは、iOS 12 で導入された新しい通知管理機能を活用するために Xamarin.iOS を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: F1D90729-F85A-425B-B633-E2FA38FB4A0C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 96fce269784ad0ac41fd1685ac7ac6b957932bd8
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233173"
---
# <a name="notification-management-in-xamarinios"></a>Xamarin.iOS での通知の管理

12、iOS オペレーティング システムには、通知センターからディープ リンクと、アプリの管理 画面で通知する設定アプリことができます。 この画面が参加をユーザーに許可する必要があり、さまざまな種類の通知からアプリを送信します。

## <a name="sample-app-redgreennotifications"></a>サンプル アプリ:RedGreenNotifications

通知の管理のしくみの例を確認するを参照してください、 [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)サンプル アプリです。

このサンプル アプリでは、– 赤、緑 – 2 つの種類の通知を送信し、ユーザーがいずれかの種類を有効または無効にできる画面を提供します。

このガイドでのコード スニペットは、このサンプル アプリから取得されます。

## <a name="notification-management-screen"></a>通知の管理 画面

サンプル アプリで`ManageNotificationsViewController`ユーザーを個別に有効にし、赤の通知、緑色の通知を無効にできるユーザー インターフェイスを定義します。 これは標準 [`UIViewController`](xref:UIKit.UIViewController)
含む、 [ `UISwitch` ](xref:UIKit.UISwitch)の各通知の種類。 通知のいずれかの種類のスイッチを切り替え保存すると、ユーザーの既定値、通知の種類、ユーザーの設定で。

```csharp
partial void HandleRedNotificationsSwitchValueChange(UISwitch sender)
{
    NSUserDefaults.StandardUserDefaults.SetBool(sender.On, RedNotificationsEnabledKey);
}
```

> [!NOTE]
> 通知の管理 画面では、ユーザーがアプリに通知を完全に無効かどうかも確認します。 そうである場合、個々 の通知の種類の切り替えは非表示になります。 これは、通知の管理画面を行うには。
>
> - 呼び出し[ `UNUserNotificationCenter.Current.GetNotificationSettingsAsync` ](xref:UserNotifications.UNUserNotificationCenter.GetNotificationSettingsAsync)され、検査、 [ `AuthorizationStatus` ](xref:UserNotifications.UNNotificationSettings.AuthorizationStatus)プロパティ。
> - 通知をアプリの完全に無効にされている場合は、個々 の通知の種類の切り替えを非表示にします。
> - かどうかの通知が無効になっているアプリケーションは、のでユーザーできます有効/無効通知 iOS の設定でいつでも、前面に移動するたびに再チェックします。

サンプル アプリの`ViewController`クラスは、実際が受信することを確認、通知の種類のユーザーのローカル通知を送信する前に、通知、チェックのユーザーの設定を送信します。

```csharp
partial void HandleTapRedNotificationButton(UIButton sender)
{
    bool redEnabled = NSUserDefaults.StandardUserDefaults.BoolForKey(ManageNotificationsViewController.RedNotificationsEnabledKey);
    if (redEnabled)
    {
        // ...
```

## <a name="deep-link"></a>ディープ リンク

iOS ディープ リンク アプリの通知の管理画面に通知センター、設定アプリでアプリの通知設定から。 そのため、アプリが必要です。

- 渡すことによって、通知の管理 画面が使用可能なであることを示す`UNAuthorizationOptions.ProvidesAppNotificationSettings`アプリの通知の承認要求にします。
- 実装、`OpenSettings`メソッドから[ `IUNUserNotificationCenterDelegate`](xref:UserNotifications.IUNUserNotificationCenterDelegate)します。

### <a name="authorization-request"></a>承認要求

通知の管理 画面が使用可能なことは、アプリに渡す必要があります、オペレーティング システムに示すために、 `UNAuthorizationOptions.ProvidesAppNotificationSettings` (およびその他の通知の配信オプション必要があります) オプションを`RequestAuthorization`メソッドを`UNUserNotificationCenter`します。

たとえば、サンプル アプリので`AppDelegate`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.ProvidesAppNotificationSettings | UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional;
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        // ...
```

### <a name="opensettings-method"></a>OpenSettings メソッド

`OpenSettings`をディープ リンク アプリの通知の管理画面に、システムによって呼び出されたメソッドは、その画面に直接ユーザーを移動する必要があります。

サンプル アプリでこのメソッドを実行するセグエ、`ManageNotificationsViewController`必要な場合。

```csharp
[Export("userNotificationCenter:openSettingsForNotification:")]
public void OpenSettings(UNUserNotificationCenter center, UNNotification notification)
{
    var navigationController = Window.RootViewController as UINavigationController;
    if (navigationController != null)
    {
        var currentViewController = navigationController.VisibleViewController;
        if (currentViewController is ViewController)
        {
            currentViewController.PerformSegue(ManageNotificationsViewController.ShowManageNotificationsSegue, this);
        }

    }
}
```

## <a name="related-links"></a>関連リンク

- [サンプル アプリ – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Xamarin.iOS でのユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [ユーザーへの通知 (WWDC 2018) の新機能新機能](https://developer.apple.com/videos/play/wwdc2018/710/)
- [ユーザーへの通知 (WWDC 2017) の新機能新機能およびベスト プラクティス](https://developer.apple.com/videos/play/wwdc2017/708/)
- [リモート通知 (Apple) を生成します。](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
