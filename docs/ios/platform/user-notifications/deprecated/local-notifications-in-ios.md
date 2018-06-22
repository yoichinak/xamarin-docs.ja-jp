---
title: Xamarin.iOS での通知
description: このセクションでは、Xamarin.iOS でローカルの通知を実装する方法を示します。 IOS 通知のさまざまな UI 要素について説明するされ API について説明を作成して、メッセージを表示するのに含まれているのです。
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: d4dd759f52e52f417441f69e6417fab536daf6d3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30777913"
---
# <a name="notifications-in-xamarinios"></a>Xamarin.iOS での通知

_このセクションでは、Xamarin.iOS でローカルの通知を実装する方法を示します。IOS 通知のさまざまな UI 要素について説明するされ API について説明を作成して、メッセージを表示するのに含まれているのです。_

> [!IMPORTANT]
> このセクションの情報が iOS 9 に関連し、前に、ままになっていますここで以前の iOS バージョンをサポートするためにします。 IOS 10 以降では、次を参照してください、[ユーザー通知フレームワーク ガイド](~/ios/platform/user-notifications/index.md)iOS デバイスでローカルとリモートの通知の両方をサポートするためです。

iOS では、ユーザーに通知が受信されたことを示すために 3 つの方法があります。

-  **サウンドまたは振動**-iOS は、ユーザーに通知するサウンドを再生できます。 サウンドが無効になっている場合、デバイスを振動させられる構成できます。
-  **アラート**-通知に関する情報が画面にダイアログ ボックスを表示することはできます。
-  **バッジ**- 通知を発行すると、アプリケーションのアイコンを (バッジの付いた)、番号が表示されることができます。


iOS、*通知センター*をユーザーにすべての通知、ローカルとリモートの両方が表示されます。 ユーザーはこの画面の一番上から下へスワイプしてアクセス可能性があります。

 ![](local-notifications-in-ios-images/image13.png "通知センター")

## <a name="creating-local-notifications-in-ios"></a>IOS でのローカルの通知の作成

iOS では、非常に簡単に作成し、ローカルの通知を処理します。
最初に、iOS 8 では、通知を表示するユーザーのアクセス許可を要求するアプリケーションが必要です。 ローカルの通知を送信する前に、アプリに次のコードを追加-付属のサンプルが配置で、 **AppDelegate**の**FinishedLaunching**メソッドです。

```csharp
var settings = UIUserNotificationSettings.GetSettingsForTypes(
  UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound
  , null);
UIApplication.SharedApplication.RegisterUserNotificationSettings (settings);
```

  [![](local-notifications-in-ios-images/image0-sml.png "ローカルの通知を送信する機能を確認します。")](local-notifications-in-ios-images/image0.png#lightbox)

スケジュールを作成するローカルの通知を設定する、`UILocalNotification`オブジェクト、設定、`FireDate`を使用してスケジュールを設定し、`ScheduleLocalNotification`メソッドを`UIApplication.SharedApplication`オブジェクト。 次のコード スニペットでは、1 分間、将来を起動し、メッセージのアラートを表示する通知のスケジュールを設定する方法を示します。

```csharp
UILocalNotification notification = new UILocalNotification();
NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

次のスクリーン ショットは、このアラートがどのように表示します。

  [![](local-notifications-in-ios-images/image2-sml.png "例アラート")](local-notifications-in-ios-images/image2.png#lightbox)

場合は、ユーザーが選択した*許可しない*通知何が表示されます。

数が、アプリケーションのアイコンにバッジを適用する場合は、次の行のコードに示すようを設定できます。

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

順序再生で次のコード スニペットで示すように、サウンド、アイコンはの通知 SoundName プロパティを設定します。

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Apple ヒューマン インターフェイス ガイドラインに従って、通知、サウンドを再生する場合にもが付属バッジや、アラートが発生したアプリケーションを特定のユーザーを支援する警告です。 また、サウンドが 30 秒より長い場合は、iOS は音を鳴らす既定代わりにします。

> [!IMPORTANT]
> デリゲートの通知を 2 回起動する iOS シミュレーターには、バグがあります。 デバイスにアプリケーションを実行している場合は、この問題は発生しません。

## <a name="handling-notifications"></a>通知の処理

iOS アプリケーションでは、ほぼ同じ方法でリモートとローカルの通知を処理します。 アプリケーションが実行されているときに、`ReceivedLocalNotification`メソッドまたは`ReceivedRemoteNotification`AppDelegate クラスのメソッドが呼び出され、通知情報をパラメーターとして渡されます。

アプリケーションは、さまざまな方法で通知を処理することができます。 たとえば、アプリケーション可能性がありますに何らかのイベントについてユーザーに通知するアラートを表示するだけです。 または、プロセスが完了したら、サーバーに同期ファイルなどのユーザーにアラートを表示する通知を使用する場合があります。

次のコードは、ローカルの通知の処理し、アラートを表示およびバッジ番号はゼロにリセットする方法を示しています。

```csharp
 public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
        {
            // show an alert
            UIAlertController okayAlertController = UIAlertController.Create (notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
            okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
            viewController.PresentViewController (okayAlertController, true, null);

            // reset our badge
            UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
        }
```

アプリケーションが実行されていない場合、iOS はサウンドを再生や、該当するアイコンのバッジを更新します。 ユーザーがアラートに関連付けられているアプリケーションを起動するときに、アプリケーションは起動アプリ デリゲートで FinishedLaunching メソッドが呼び出され、options パラメーター経由での通知情報が渡されました。 オプションの辞書にキーが含まれている場合`UIApplication.LaunchOptionsLocalNotificationKey`、AppDelegate がローカルの通知からアプリケーションを起動したことを認識します。 次のコード スニペットでは、このプロセスを示しています。

```csharp
// check for a local notification
if (options.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
 {
     var localNotification = options[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
     if (localNotification != null)
     {
        UIAlertController okayAlertController = UIAlertController.Create (localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
        viewController.PresentViewController (okayAlertController, true, null);

         // reset our badge
         UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
     }
 }
```

オプションのディクショナリには代わりに、リモートの通知である場合、 `LaunchOptionsRemoteNotificationKey`、し、そのキーの結果として得られる値は、`NSDictionary`リモート通知のペイロードを持つオブジェクト。 アラート、バッジ、サウンド キーによる通知のペイロードを抽出することができます。 次のコード スニペットは、リモートの通知を取得する方法を示しています。

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>まとめ

このセクションでは、作成し、Xamarin.iOS で通知を発行する方法を示しました。 オーバーライドすることで、通知にアプリケーションがどのように対応可能性がありますかを表示する、`ReceivedLocalNotification`メソッドまたは`ReceivedRemoteNotification`メソッドで、`AppDelegate`です。


## <a name="related-links"></a>関連リンク

- [ローカル通知 (サンプル)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [ローカル開発者用のプッシュ通知と](https://developer.apple.com/notifications/)
- [ローカルおよびプッシュ通知プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
