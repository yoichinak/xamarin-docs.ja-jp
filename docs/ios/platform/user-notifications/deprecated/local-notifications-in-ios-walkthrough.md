---
title: チュートリアル - Xamarin.iOS でのローカル通知の使用
description: このセクションでは、Xamarin.iOS アプリケーションでローカル通知を使用する方法について説明します。 これを作成して、アプリで受信された場合にアラートがポップアップ表示される通知の発行の基礎を紹介します。
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 7589784563906d60fc8026feac9ea16362463bfa
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102634"
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>チュートリアル - Xamarin.iOS でのローカル通知の使用

_このセクションでは、Xamarin.iOS アプリケーションでローカル通知を使用する方法について説明します。これを作成して、アプリで受信された場合にアラートがポップアップ表示される通知の発行の基礎を紹介します。_

> [!IMPORTANT]
> このセクションの情報は、iOS 9 に関連し、前に、そのままにしたここで以前の iOS バージョンをサポートするためにします。 IOS 10 以降を参照してください、[ユーザー通知フレームワーク ガイド](~/ios/platform/user-notifications/index.md)iOS デバイスでローカルとリモート通知の両方をサポートするためです。

## <a name="walkthrough"></a>チュートリアル

操作にローカル通知を表示する単純なアプリケーションを作成することができます。 このアプリケーションでは、それを 1 つのボタンがあります。 ボタンをクリックしたときは、ローカル通知を作成します。 指定した期間が経過した後、表示通知が表示されます。


1. Visual studio for Mac では、新しい単一ビュー iOS ソリューションを作成し、それを呼び出す`Notifications`します。
1. 開く、`Main.storyboard`ファイル、およびボタンをビューにドラッグします。 ボタンの名前を付けます**ボタン**、し、タイトル**通知の追加**します。 設定することも[制約](~/ios/user-interface/designer/designer-auto-layout.md)をこの時点で、ボタンをクリックします。 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "ボタンをいくつかの制約を設定")
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

    このコードでは、サウンドを使用して、1、アイコンのバッジの値を設定およびユーザーにアラートを表示する通知を作成します。

1. 次に、ファイルを編集`AppDelegate.cs`、最初に次のコードを追加、`FinishedLaunching`メソッド。 デバイスが iOS 8 を実行しているかどうかを確認する場合はそのためチェック**必要**通知を受信するユーザーのアクセス許可を要求します。

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
            var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
                UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
            );

            application.RegisterUserNotificationSettings (notificationSettings);
        }
    ```

1. 引き続き`AppDelegate.cs`通知を受信したときに呼び出すことが次のメソッドを追加します。

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

1. ローカル通知のため、通知が起動されたケースを処理する必要があります。 メソッドを編集`FinishedLaunching`で、`AppDelegate`に次のコード スニペットを含めます。


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

1. 最後に、アプリケーションを実行します。 IOS で、通知を許可する 8 ように求められます。 をクリックして**OK**順にクリックします、**通知を追加する**ボタンをクリックします。 しばらくすると次のスクリーン ショットで示すように、警告のダイアログが表示されます。

    ![](local-notifications-in-ios-walkthrough-images/image0.png "通知を送信することを確認する") ![](local-notifications-in-ios-walkthrough-images/image1.png "通知の追加ボタン") ![](local-notifications-in-ios-walkthrough-images/image2.png "通知アラート ダイアログ")

## <a name="summary"></a>まとめ

このチュートリアルでは、さまざまな API の作成と iOS の通知を発行を使用する方法を示しました。 バッジをユーザーにアプリケーション固有フィードバックを提供すると、アプリケーションのアイコンを更新する方法も示されています。


## <a name="related-links"></a>関連リンク

- [ローカル通知 (サンプル)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [ローカルおよびプッシュ通知プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
