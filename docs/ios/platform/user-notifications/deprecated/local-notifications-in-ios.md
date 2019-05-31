---
title: Xamarin.iOS での通知
description: このセクションでは、Xamarin.iOS でローカル通知を実装する方法を示します。 IOS 通知のさまざまな UI 要素について説明し、API の説明を作成して、通知を表示するのに関係するのです。
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/13/2018
ms.openlocfilehash: 6710abd28a2b0f992296008d12950b95ec29783d
ms.sourcegitcommit: dd73477b1bccbd7ca45c1fb4e794da6b36ca163d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/30/2019
ms.locfileid: "66394689"
---
# <a name="notifications-in-xamarinios"></a>Xamarin.iOS での通知

> [!IMPORTANT]
> このセクションの情報は、iOS 9 と旧バージョンに関係します。 IOS 10 以降を参照してください、[ユーザー通知フレームワーク ガイド](~/ios/platform/user-notifications/index.md)します。

iOS では、通知が受信されたことをユーザーに示す 3 つの方法があります。

- **サウンドまたは振動**-iOS は、ユーザーに通知するサウンドを再生できます。 サウンドが無効になっている場合は、バイブレーションにデバイスを構成することができます。
- **アラート**-通知に関する情報を含む画面でダイアログを表示することができます。
- **バッジ**- 通知が発行されると、アプリケーション アイコン (認定)、番号が表示されることができます。

iOS にも用意されています、*通知センター*をユーザーにすべての通知、ローカルとリモートの両方が表示されます。 ユーザーはこの画面の一番上から下へスワイプすると、アクセス可能性があります。

![通知センター](local-notifications-in-ios-images/image13.png "通知センター")

## <a name="creating-local-notifications-in-ios"></a>IOS でのローカル通知の作成

iOS では、非常に簡単に作成し、ローカル通知を処理します。
最初に、iOS 8 では、アプリケーション通知を表示するユーザーのアクセス許可を要求する必要があります。 ローカル通知を送信する前に、アプリに次のコードを追加 -[サンプルに接続されている](https://developer.xamarin.com/samples/monotouch/LocalNotifications/)に格納、 **AppDelegate**の**FinishedLaunching**メソッド。

```csharp
var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes(
    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
);
application.RegisterUserNotificationSettings(notificationSettings);
```

[![ローカル通知を送信する機能を確認する](local-notifications-in-ios-images/image0-sml.png "ローカル通知を送信する機能を確認します。")](local-notifications-in-ios-images/image0.png#lightbox)

ローカル通知をスケジュールするには、作成、`UILocalNotification`オブジェクト、設定、`FireDate`を使用してスケジュールを設定し、`ScheduleLocalNotification`メソッドを`UIApplication.SharedApplication`オブジェクト。 次のコード スニペットでは、今後は、1 つの分を起動され、メッセージと警告を表示することを示す通知をスケジュールする方法を示します。

```csharp
UILocalNotification notification = new UILocalNotification();
notification.FireDate = NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

このアラートの次のスクリーン ショットに示します。

[![](local-notifications-in-ios-images/image2-sml.png "アラートの例")](local-notifications-in-ios-images/image2.png#lightbox)

場合は、ユーザーが選択した*を許可しない*通知し、何が表示されます。

数が、アプリケーションのアイコンにバッジを適用する場合は、次の行のコードに示すようを設定できます。

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

順序の再生で次のコード スニペットに示すように、サウンド、アイコンは通知を SoundName プロパティを設定します。

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

通知サウンドが 30 秒より長い場合は、iOS、サウンドの再生既定の代わりにします。

> [!IMPORTANT]
> デリゲートの通知を 2 回起動する iOS シミュレーターでは、バグがあります。 デバイスでアプリケーションを実行するときに、この問題は発生しません。

## <a name="handling-notifications"></a>通知の処理

iOS アプリケーションでは、ほぼ同じ方法でリモートとローカルの通知を処理します。 アプリケーションが実行されているときに、`ReceivedLocalNotification`メソッドまたは`ReceivedRemoteNotification`メソッドを`AppDelegate`クラスの呼び出しは、および通知情報がパラメーターとして渡されます。

アプリケーションでは、さまざまな方法で通知を処理できます。 たとえば、アプリケーションがあります何らかのイベントについてユーザーに通知するアラートを表示するだけです。 または、プロセスが完了したら、サーバーに同期ファイルなどのユーザーにアラートを表示する通知を使用する場合があります。

次のコードでは、ローカル通知を処理し、アラートを表示およびバッジ番号はゼロにリセットする方法を示します。

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

アプリケーションが実行されていない場合、iOS はサウンドを再生や、として該当するアイコンのバッジを更新します。 ユーザーは、アラートに関連付けられているアプリケーションを起動すると、アプリケーションが起動し、`FinishedLaunching`アプリ デリゲートのメソッドが呼び出され、経由で通知情報が渡される、`launchOptions`パラメーター。 オプションの辞書にキーが含まれている場合`UIApplication.LaunchOptionsLocalNotificationKey`、`AppDelegate`ローカル通知からアプリケーションを起動したことを認識します。 次のコード スニペットでは、このプロセスを示しています。

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

リモート通知を`launchOptions`必要があります、`LaunchOptionsRemoteNotificationKey`関連付けられている`NSDictionary`リモート通知ペイロードを格納しています。 使用して、通知ペイロードを抽出することができます、 `alert`、 `badge`、および`sound`キー。 次のコード スニペットでは、リモート通知を取得する方法を示します。

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>まとめ

このセクションで作成し、Xamarin.iOS で通知を発行する方法を示しました。 オーバーライドすることで、通知に、アプリケーションがどのように対応可能性がありますかを表示する、`ReceivedLocalNotification`メソッドまたは`ReceivedRemoteNotification`メソッドで、`AppDelegate`します。

## <a name="related-links"></a>関連リンク

- [ローカル通知 (サンプル)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [ローカルおよびプッシュ通知を開発者向け](https://developer.apple.com/notifications/)
- [ローカルおよびプッシュ通知プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
