---
title: チュートリアル-Xamarin でのローカル通知の使用
description: このセクションでは、Xamarin iOS アプリケーションでローカル通知を使用する方法について説明します。 ここでは、アプリで受信したときにアラートをポップアップ表示する通知を作成して公開する方法の基本について説明します。
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: f7290d8284f90db4f53a38dc89f7b5a1a10213c0
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655537"
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>チュートリアル-Xamarin でのローカル通知の使用

_このセクションでは、Xamarin iOS アプリケーションでローカル通知を使用する方法について説明します。ここでは、アプリで受信したときにアラートをポップアップ表示する通知を作成して公開する方法の基本について説明します。_

> [!IMPORTANT]
> このセクションの情報は、iOS 9 とそれ以前のバージョンをサポートするために残されています。 IOS 10 以降については、iOS デバイスでのローカル通知とリモート通知の両方をサポートするための[ユーザー通知フレームワークガイド](~/ios/platform/user-notifications/index.md)を参照してください。

## <a name="walkthrough"></a>チュートリアル

ローカル通知を実際に表示する単純なアプリケーションを作成してみましょう。 このアプリケーションには1つのボタンがあります。 ボタンをクリックすると、ローカル通知が作成されます。 指定された期間が経過すると、通知が表示されます。


1. Visual Studio for Mac で、新しい単一ビューの iOS ソリューションを作成し、 `Notifications`それを呼び出します。
1. `Main.storyboard`ファイルを開き、ボタンをビューにドラッグします。 **ボタンに**名前を付けて、タイトルを**追加通知**にします。 この時点で、いくつかの[制約](~/ios/user-interface/designer/designer-auto-layout.md)をボタンに設定することもできます。 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "ボタンにいくつかの制約を設定する")
1. `ViewController`クラスを編集し、次のイベントハンドラーを ViewDidLoad メソッドに追加します。

    ```csharp
    button.TouchUpInside += (sender, e) =>
    {
        // create the notification
        var notification = new UILocalNotification();

        // set the fire date (the date time in which it will fire)
        notification.FireDate = NSDate.FromTimeIntervalSinceNow(60);

        // configure the alert
        notification.AlertAction = "View Alert";
        notification.AlertBody = "Your one minute alert has fired!";

        // modify the badge
        notification.ApplicationIconBadgeNumber = 1;

        // set the sound to be the default sound
        notification.SoundName = UILocalNotification.DefaultSoundName;

        // schedule it
        UIApplication.SharedApplication.ScheduleLocalNotification(notification);
    };
    ```

    このコードは、サウンドを使用する通知を作成し、アイコンバッジの値を1に設定して、ユーザーに警告を表示します。

1. 次に、ファイル`AppDelegate.cs`を編集し、まず次のコード`FinishedLaunching`をメソッドに追加します。 デバイスで iOS 8 が実行されているかどうかを確認しました。その場合は、通知を受信するためのユーザーのアクセス許可を要求する**必要**があります。

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
            var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
                UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
            );

            application.RegisterUserNotificationSettings (notificationSettings);
        }
    ```

1. まだの`AppDelegate.cs`場合は、通知の受信時に呼び出される次のメソッドを追加します。

    ```csharp
    public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
            {
                // show an alert
                UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
                okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

                UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(okayAlertController, true, null);

                // reset our badge
                UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
            }

    ```

1. ローカル通知によって通知が開始されたケースを処理する必要があります。 でメソッド`FinishedLaunching`を編集して、次のコードスニペットを含めます。`AppDelegate`


    ```csharp
    // check for a notification

    if (launchOptions != null)
    {
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
    }

    ```

1. 最後に、アプリケーションを実行します。 IOS 8 では、通知を許可するように求められます。 [ **OK]** をクリックし、 **[通知の追加]** ボタンをクリックします。 短い一時停止の後、次のスクリーンショットに示すように、アラートダイアログが表示されます。

    ![](local-notifications-in-ios-walkthrough-images/image0.png "通知を送信することを確認する") ![](local-notifications-in-ios-walkthrough-images/image1.png "通知の追加ボタン") ![](local-notifications-in-ios-walkthrough-images/image2.png "通知アラート ダイアログ")

## <a name="summary"></a>まとめ

このチュートリアルでは、さまざまな API を使用して、iOS で通知を作成および発行する方法について説明しました。 また、アプリケーションアイコンをバッジで更新して、アプリケーション固有のフィードバックをユーザーに提供する方法についても説明します。


## <a name="related-links"></a>関連リンク

- [ローカル通知 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/localnotifications)
- [ローカルおよびプッシュ通知のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
