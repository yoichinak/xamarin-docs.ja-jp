---
title: Xamarin.iOS での通知
description: このセクションでは、Xamarin. iOS でローカル通知を実装する方法について説明します。 ここでは、iOS の通知のさまざまな UI 要素について説明し、通知の作成と表示に関連する API について説明します。
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/13/2018
ms.openlocfilehash: f99776a0e6a84c193362d0e5a4efb68e70717ad0
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436111"
---
# <a name="notifications-in-xamarinios"></a>Xamarin.iOS での通知

> [!IMPORTANT]
> このセクションの情報は、iOS 9 以前に関連しています。 IOS 10 以降については、「 [ユーザー通知フレームワークガイド](~/ios/platform/user-notifications/index.md)」を参照してください。

iOS には、通知が受信されたことをユーザーに示す3つの方法があります。

- **サウンドまたは振動** -iOS はサウンドを再生して、ユーザーに通知することができます。 サウンドが無効になっている場合は、デバイスをバイブレーションに構成できます。
- **アラート** -通知に関する情報を含むダイアログを画面に表示することができます。
- **バッジ** -通知が発行されると、アプリケーションアイコンに数字 (バッジ) が表示されます。

また、iOS には、ローカルとリモートの両方の通知をユーザーに表示する *通知センター* も用意されています。 ユーザーは、画面の上部からスワイプして、これにアクセスできます。

![通知センター](local-notifications-in-ios-images/image13.png "通知センター")

## <a name="creating-local-notifications-in-ios"></a>IOS でのローカル通知の作成

iOS を使用すると、ローカル通知を作成して処理することが非常に簡単になります。
まず、iOS 8 では、通知を表示するためのユーザーのアクセス許可を要求するアプリケーションが必要です。 ローカル通知を送信する前に、アプリに次のコードを追加します。 [添付さ](/samples/xamarin/ios-samples/localnotifications) れたサンプルは、 **Appdelegate**の **FinishedLaunching** メソッドに配置します。

```csharp
var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes(
    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
);
application.RegisterUserNotificationSettings(notificationSettings);
```

[![ローカル通知を送信する機能を確認しています](local-notifications-in-ios-images/image0-sml.png "ローカル通知を送信する機能を確認しています")](local-notifications-in-ios-images/image0.png#lightbox)

ローカル通知をスケジュールするには、オブジェクトを作成し、 `UILocalNotification` を設定して、 `FireDate` オブジェクトのメソッドを使用してスケジュールを設定し `ScheduleLocalNotification` `UIApplication.SharedApplication` ます。 次のコードスニペットは、1分後に起動される通知をスケジュールし、メッセージと共にアラートを表示する方法を示しています。

```csharp
UILocalNotification notification = new UILocalNotification();
notification.FireDate = NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

次のスクリーンショットは、このアラートの内容を示しています。

[![アラートの例](local-notifications-in-ios-images/image2-sml.png)](local-notifications-in-ios-images/image2.png#lightbox)

ユーザーが通知を *許可しない* ことを選択した場合は、何も表示されないことに注意してください。

バッジを数値付きでアプリケーションアイコンに適用する場合は、次の行コードに示すように設定できます。

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

アイコンでサウンドを再生するには、次のコードスニペットに示すように、通知の SoundName プロパティを設定します。

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

通知サウンドが30秒より長い場合は、代わりに既定のサウンドが再生されます。

> [!IMPORTANT]
> IOS シミュレーターには、デリゲート通知を2回起動するバグがあります。 デバイスでアプリケーションを実行する場合、この問題は発生しません。

## <a name="handling-notifications"></a>通知の処理

iOS アプリケーションは、リモートおよびローカルの通知をほぼ同じ方法で処理します。 アプリケーションが実行されている場合、 `ReceivedLocalNotification` クラスのメソッドまたは `ReceivedRemoteNotification` メソッドが `AppDelegate` 呼び出され、通知情報がパラメーターとして渡されます。

アプリケーションでは、通知をさまざまな方法で処理できます。 たとえば、アプリケーションでは、ある程度のイベントについてユーザーに通知するアラートを表示できます。 または、通知を使用して、プロセスが完了したことを示す警告をユーザーに表示することができます (ファイルをサーバーに同期するなど)。

次のコードは、ローカル通知を処理し、アラートを表示し、バッジ番号をゼロにリセットする方法を示しています。

```csharp
public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
{
    // show an alert
    UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
    okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

    Window.RootViewController.PresentViewController(okayAlertController, true, null);

    // reset our badge
    UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
}
```

アプリケーションが実行されていない場合、iOS はサウンドを再生するか、アイコンバッジを適宜更新します。 ユーザーがアラートに関連付けられているアプリケーションを起動すると、アプリケーションが起動し、アプリのデリゲートのメソッドが呼び出され、パラメーターを使用して `FinishedLaunching` 通知情報が渡され `launchOptions` ます。 オプションディクショナリにキーが含まれている場合、 `UIApplication.LaunchOptionsLocalNotificationKey` は `AppDelegate` アプリケーションがローカル通知から起動されたことを認識します。 次のコードスニペットは、このプロセスを示しています。

```csharp
// check for a local notification
if (launchOptions.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
{
    var localNotification = launchOptions[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
    if (localNotification != null)
    {
        UIAlertController okayAlertController = UIAlertController.Create(localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

        Window.RootViewController.PresentViewController(okayAlertController, true, null);

        // reset our badge
        UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
    }
}
```

リモート通知の場合、には、 `launchOptions` `LaunchOptionsRemoteNotificationKey` `NSDictionary` リモート通知ペイロードを含むが関連付けられています。 、、およびの各キーを使用して、通知ペイロードを抽出でき `alert` `badge` `sound` ます。 次のコードスニペットは、リモート通知を取得する方法を示しています。

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>まとめ

このセクションでは、Xamarin. iOS で通知を作成して発行する方法について説明しました。 この例 `ReceivedLocalNotification` では、でメソッドまたはメソッドをオーバーライドすることによって、アプリケーションが通知にどのように対処するかを示し `ReceivedRemoteNotification` `AppDelegate` ます。

## <a name="related-links"></a>関連リンク

- [ローカル通知 (サンプル)](/samples/xamarin/ios-samples/localnotifications)
- [開発者向けのローカル通知とプッシュ通知](https://developer.apple.com/notifications/)
- [Local and Push Notification Programming Guide (ローカルおよびプッシュ通知プログラミング ガイド) (ローカルおよびプッシュ通知プログラミング ガイド)](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](/dotnet/api/uikit.uiapplication)
- [UILocalNotification](/dotnet/api/uikit.UILocalNotification)