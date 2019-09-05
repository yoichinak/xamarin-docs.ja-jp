---
title: Xamarin. iOS の通知管理
description: このドキュメントでは、Xamarin を使用して、iOS 12 で導入された新しい通知管理機能を活用する方法について説明します。
ms.prod: xamarin
ms.assetid: F1D90729-F85A-425B-B633-E2FA38FB4A0C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/04/2018
ms.openlocfilehash: b6c6baad2cbd923bde4dab3766040b5df4351787
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282112"
---
# <a name="notification-management-in-xamarinios"></a>Xamarin. iOS の通知管理

IOS 12 では、オペレーティングシステムが通知センターと設定アプリからアプリの通知管理画面に深くリンクできます。 この画面では、ユーザーは、アプリから送信されるさまざまな種類の通知をオプトインおよびオプトアウトできます。

## <a name="sample-app-redgreennotifications"></a>サンプルアプリ:RedGreenNotifications

通知管理のしくみの例については、 [RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)サンプルアプリを参照してください。

このサンプルアプリは、赤と緑の2種類の通知を送信し、ユーザーがいずれかの種類の選択を解除できる画面を提供します。

このガイドのコードスニペットは、このサンプルアプリから抜粋したものです。

## <a name="notification-management-screen"></a>通知管理画面

このサンプルアプリでは`ManageNotificationsViewController` 、は、ユーザーが赤の通知と緑色の通知を個別に有効または無効にするためのユーザーインターフェイスを定義します。 これは標準です。[`UIViewController`](xref:UIKit.UIViewController)
各通知[`UISwitch`](xref:UIKit.UISwitch)の種類のを格納している。 いずれかの種類の通知に対してスイッチを切り替えると、ユーザーの既定では、その種類の通知がユーザーに設定されます。

```csharp
partial void HandleRedNotificationsSwitchValueChange(UISwitch sender)
{
    NSUserDefaults.StandardUserDefaults.SetBool(sender.On, RedNotificationsEnabledKey);
}
```

> [!NOTE]
> また、通知管理画面では、ユーザーがアプリの通知をまったく無効にしているかどうかも確認されます。 その場合は、個々の通知の種類の切り替えを非表示にします。 これを行うには、通知管理画面を次のようにします。
>
> - を[`UNUserNotificationCenter.Current.GetNotificationSettingsAsync`](xref:UserNotifications.UNUserNotificationCenter.GetNotificationSettingsAsync)呼び出し、プロパティ[`AuthorizationStatus`](xref:UserNotifications.UNNotificationSettings.AuthorizationStatus)を調べます。
> - アプリで通知が完全に無効になっている場合、個々の通知の種類の切り替えを非表示にします。
> - アプリケーションがフォアグラウンドに移動するたびに通知が無効になっているかどうかを再確認します。これは、ユーザーがいつでも iOS 設定の通知を有効または無効にできるためです。

通知を送信する`ViewController`サンプルアプリのクラスは、ローカル通知を送信する前にユーザーの設定を確認して、ユーザーが実際に受け取る必要のある種類の通知であることを確認します。

```csharp
partial void HandleTapRedNotificationButton(UIButton sender)
{
    bool redEnabled = NSUserDefaults.StandardUserDefaults.BoolForKey(ManageNotificationsViewController.RedNotificationsEnabledKey);
    if (redEnabled)
    {
        // ...
```

## <a name="deep-link"></a>ディープリンク

iOS の詳細情報は、通知センターからのアプリの通知管理画面と、設定アプリのアプリの通知設定にリンクされています。 これを容易にするために、アプリは次のことを行う必要があります。

- アプリの通知承認要求に渡す`UNAuthorizationOptions.ProvidesAppNotificationSettings`ことによって、通知管理画面を使用できることを示します。
- `OpenSettings` [から`IUNUserNotificationCenterDelegate`](xref:UserNotifications.IUNUserNotificationCenterDelegate)メソッドを実装します。

### <a name="authorization-request"></a>承認要求

通知管理画面が使用可能であることをオペレーティングシステムに示すには、アプリで`UNAuthorizationOptions.ProvidesAppNotificationSettings`オプション (必要な他の通知配信オプションと共に) `RequestAuthorization`をの`UNUserNotificationCenter`メソッドに渡す必要があります。

たとえば、サンプルアプリの`AppDelegate`場合は、次のようになります。

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

アプリケーション`OpenSettings`の通知管理画面へのディープリンクを行うためにシステムによって呼び出されるメソッドは、ユーザーをその画面に直接移動する必要があります。

このサンプルアプリでは、必要に応じて、この`ManageNotificationsViewController`メソッドによってセグエが実行されます。

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

- [サンプルアプリ– RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Xamarin. iOS のユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [ユーザー通知の新機能 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [ベストプラクティスとユーザー通知の新機能 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [リモート通知の生成 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
