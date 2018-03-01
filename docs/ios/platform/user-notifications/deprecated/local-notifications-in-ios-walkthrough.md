---
title: "チュートリアル - Xamarin.iOS でローカルの通知の使用"
description: "このセクションで Xamarin.iOS アプリケーションでローカルの通知を使用する方法を説明します。 これは、作成して、アプリで受信された場合にアラートが表示される通知の発行の基本を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 72a66fae7a1d4554643c1b52796256cc0b3dbd31
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>チュートリアル - Xamarin.iOS でローカルの通知の使用

_このセクションで Xamarin.iOS アプリケーションでローカルの通知を使用する方法を説明します。これは、作成して、アプリで受信された場合にアラートが表示される通知の発行の基本を示します。_

> [!IMPORTANT]
> **注:** iOS 9 にこのセクションの情報が関連し、前に、ままになっていますここで以前の iOS バージョンをサポートするためにします。 IOS 10 以降では、次を参照してください、[ユーザー通知フレームワーク ガイド](~/ios/platform/user-notifications/index.md)iOS デバイスでローカルとリモートの通知の両方をサポートするためです。

## <a name="walkthrough"></a>チュートリアル

アクションにローカルの通知を表示する簡単なアプリケーションを作成することができます。 このアプリケーションには、それに 1 つのボタンがあります。 ボタンをクリックしたときは、ローカルの通知を作成します。 指定した期間が経過した後、表示通知が表示されます。


1. Mac 用 Visual Studio で新しい 1 つのビュー iOS ソリューションを作成し、それを呼び出す`Notifications`です。
1. 開く、`Main.storyboard`ファイルを開き、ボタン、ビューにドラッグします。 ボタンの名前を付けます**ボタン**、し、タイトルを付けます**追加通知**です。 一部を設定することも[制約](~/ios/user-interface/designer/designer-auto-layout.md)をこの時点で、ボタン。 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "ボタンにいくつかの制約を設定")
1. 編集、`ViewController`クラス、および ViewDidLoad メソッドに次のイベント ハンドラーを追加します。

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

    このコードでは、サウンドを使用して、アイコン バッジの値を 1 に設定およびをユーザーにアラートを表示することを示す通知を作成します。

1. 次に、ファイルを編集`AppDelegate.cs`、最初に次のコードを追加、`FinishedLaunching`メソッドです。 デバイスが iOS 8 を実行しているかどうかはそのためチェック**必要**通知を受信するユーザーのアクセス許可を要求します。

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
            var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
                UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
            );

            application.RegisterUserNotificationSettings (notificationSettings);
        }
    ```

1. まだ`AppDelegate.cs`通知を受信したときに呼び出される次のメソッドを追加します。

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

1. ローカルの通知のため、通知を起動した、ケースを処理する必要があります。 メソッドを編集`FinishedLaunching`で、`AppDelegate`をコードの次のスニペットを含めます。


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

1. 最後に、アプリケーションを実行します。 IOS で、通知を許可する 8 するように求められます。 をクリックして**OK**をクリックし、**通知を追加**ボタンをクリックします。 しばらくすると、次のスクリーン ショットに示すように、警告 ダイアログ ボックスが表示されます。

    ![](local-notifications-in-ios-walkthrough-images/image0.png "通知を送信する機能を確認する") ![ ](local-notifications-in-ios-walkthrough-images/image1.png "通知の追加ボタン") ![ ](local-notifications-in-ios-walkthrough-images/image2.png "通知の警告ダイアログ ボックス")

## <a name="summary"></a>まとめ

このチュートリアルでは、さまざまな API の作成、および iOS での通知の公開を使用する方法を示しました。 ユーザーにアプリケーション固有フィードバックを提供するバッジにアプリケーション アイコンを更新する方法も示されています。


## <a name="related-links"></a>関連リンク

- [ローカル通知 (サンプル)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [ローカルおよびプッシュ通知プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
