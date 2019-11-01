---
title: IOS でのプッシュ通知
description: このドキュメントでは、iOS 9 以前でプッシュ通知を使用する方法について説明します。 証明書、Apple Push notification Gateway サービス (APNS) に登録する方法などについて説明します。
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 468d0e16a3bd5745a243b2d7c09e642e3aeffd1d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031366"
---
# <a name="push-notifications-in-ios"></a>IOS でのプッシュ通知

> [!IMPORTANT]
> このセクションの情報は、iOS 9 とそれ以前のバージョンをサポートするために残されています。 IOS 10 以降については、iOS デバイスでのローカル通知とリモート通知の両方をサポートするための[ユーザー通知フレームワークガイド](~/ios/platform/user-notifications/index.md)を参照してください。

プッシュ通知は、サーバーアプリケーションに更新プログラムを問い合わせる必要があることをモバイルアプリケーションに通知するのに十分なデータのみを保持しておく必要があります。 たとえば、新しい電子メールが届いた場合、サーバーアプリケーションは、新しい電子メールが届いたことをモバイルアプリケーションに通知するだけです。 通知には、新しい電子メール自体は含まれません。 モバイルアプリケーションは、適切なときにサーバーから新しいメールを取得します。

IOS でのプッシュ通知の中心は、 *Apple Push Notification Gateway サービス (APNS)* です。 これは、アプリケーションサーバーから iOS デバイスへの通知のルーティングを行う、Apple が提供するサービスです。
次の図は、iOS のプッシュ通知トポロジを示しています。![](remote-notifications-in-ios-images/image4.png "このイメージは、iOS のプッシュ通知トポロジを示しています。")

リモート通知自体は、[IOS 開発者ドキュメント](https://developer.apple.com/devcenter/ios/index.action)の「[ローカルおよびプッシュ通知のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)」の「[通知ペイロード](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1)」セクションで指定されている形式とプロトコルに準拠する JSON 形式の文字列です。

Apple は APNS の2つの環境を保持します。*サンドボックス*と*運用*環境です。 サンドボックス環境は、開発段階でのテストを目的としており、TCP ポート2195の `gateway.sandbox.push.apple.com` を参照してください。 運用環境は、展開されたアプリケーションで使用することを意図しており、TCP ポート2195の `gateway.push.apple.com` にあります。

## <a name="requirements"></a>要件

プッシュ通知は、APNS のアーキテクチャによって規定された次の規則に従う必要があります。

- **256 バイトのメッセージ制限**-通知のメッセージサイズ全体は、256バイトを超えてはなりません。
- **確認メッセージ**が表示されません。 APNS は、送信者に対し、メッセージが目的の受信者にメッセージを送信したことを通知しません。 デバイスにアクセスできない場合に、複数のシーケンシャル通知が送信されると、最新の通知を除くすべての通知が失われます。 最新の通知のみがデバイスに配信されます。
- **各アプリケーションにはセキュリティで保護された証明書が必要です**。 APNS との通信は SSL 経由で行う必要があります。

## <a name="creating-and-using-certificates"></a>証明書の作成と使用

前のセクションで説明した各環境には、独自の証明書が必要です。 このセクションでは、証明書を作成し、プロビジョニングプロファイルに関連付ける方法について説明します。次に、PushSharp で使用する Personal Information Exchange 証明書を取得します。

1. 証明書を作成するには、次のスクリーンショットに示すように、Apple の web サイトで iOS プロビジョニングポータルに移動します (左側の [アプリ Id] メニュー項目に注意してください)。

    [![](remote-notifications-in-ios-images/image5new.png "The iOS Provisioning Portal on Apples website")](remote-notifications-in-ios-images/image5new.png#lightbox)

2. 次に、次のスクリーンショットに示すように、アプリ ID のセクションに移動し、新しいアプリ ID を作成します。

    [![](remote-notifications-in-ios-images/image6new.png "Navigate to the App IDs section and create a new app ID")](remote-notifications-in-ios-images/image6new.png#lightbox)

3. [ **+** ] ボタンをクリックすると、次のスクリーンショットに示すように、アプリ ID の説明とバンドル識別子を入力できるようになります。

    [![](remote-notifications-in-ios-images/image7new.png "Enter the description and a Bundle Identifier for the app ID")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. 必ず **[明示的なアプリ ID]** を選択し、バンドル識別子が `*` で終了しないことを確認します。 これにより、複数のアプリケーションに適した識別子が作成され、プッシュ通知証明書が1つのアプリケーションに対して必要になります。

5. App Services で、 **[プッシュ通知]** を選択します。

    [![](remote-notifications-in-ios-images/image8new.png "Select Push Notifications")](remote-notifications-in-ios-images/image8new.png#lightbox)

6. 次に、 **[送信]** をクリックして、新しいアプリ ID の登録を確認します。

    [![](remote-notifications-in-ios-images/image9new.png "Confirm registration of the new App ID")](remote-notifications-in-ios-images/image9new.png#lightbox)

7. 次に、アプリ ID の証明書を作成する必要があります。 左側のナビゲーションで、 **[証明書]** を参照し、次のスクリーンショットに示すように [`+`] ボタンを選択し > ます。

    [![](remote-notifications-in-ios-images/image10new.png "Create the certificate for the app ID")](remote-notifications-in-ios-images/image8.png#lightbox)

8. 開発証明書と運用証明書のどちらを使用するかを選択します。

    [![](remote-notifications-in-ios-images/image11new.png "Select a Development or Production certificate")](remote-notifications-in-ios-images/image11new.png#lightbox)

9. 次に、先ほど作成した新しいアプリ ID を選択します。

    [![](remote-notifications-in-ios-images/image12new.png "Select the new App ID just created")](remote-notifications-in-ios-images/image12new.png#lightbox)

10. これにより、Mac で**キーチェーンアクセス**アプリケーションを使用して*証明書署名要求*を作成する手順が表示されます。

11. 証明書が作成されたので、アプリケーションに署名して APNs に登録できるようにするには、その証明書をビルドプロセスの一部として使用する必要があります。 そのためには、証明書を使用するプロビジョニングプロファイルを作成およびインストールする必要があります。

12. 開発プロビジョニングプロファイルを作成するには、 **[プロビジョニングプロファイル]** セクションに移動し、先ほど作成したアプリ Id を使用して作成した手順に従います。

13. プロビジョニングプロファイルを作成したら、 **Xcode オーガナイザー**を開き、更新します。 作成したプロビジョニングプロファイルが表示されない場合は、iOS プロビジョニングポータルからプロファイルをダウンロードして手動でインポートすることが必要な場合があります。 次のスクリーンショットは、プロビジョニングプロファイルが追加されたオーガナイザーの例を示しています。  
    [![](remote-notifications-in-ios-images/image13new.png "This screen shot shows an example of the Organizer with the provision profile added")](remote-notifications-in-ios-images/image13new.png#lightbox)

14. この時点で、新しく作成されたプロビジョニングプロファイルを使用するように Xamarin プロジェクトを構成する必要があります。 これは、次のスクリーンショットに示すように、 **[IOS バンドル署名]** タブの **[プロジェクトオプション]** ダイアログで行います。  
    [![](remote-notifications-in-ios-images/image11.png "Configure the Xamarin.iOS project to use this newly created provisioning profile")](remote-notifications-in-ios-images/image11.png#lightbox)

この時点で、アプリケーションはプッシュ通知と連携するように構成されています。 ただし、証明書に必要な手順は他にもいくつかあります。 この証明書は、(Personal Information Exchange) 証明書を必要とする PushSharp と互換性のない DER 形式です。 PushSharp で証明書を使用できるように変換するには、次の最後の手順を実行します。

1. **証明書ファイルをダウンロード**します。 IOS プロビジョニングポータルにログインし、証明書 タブを選択して、適切なプロビジョニングプロファイルに関連付けられている証明書を選択し、**ダウンロード** を選択します。
1. **キーチェーンアクセスを開く**-これは、OS X のパスワード管理システムへの GUI インターフェイスです。
1. **証明書をインポートする**-証明書がまだインストールされていない場合は、**ファイル...** キーチェーンアクセスメニューから項目をインポートします。 上記でエクスポートした証明書に移動し、それを選択します。
1. **証明書をエクスポートする**-関連付けられている秘密キーが表示されるように証明書を展開し、キーを右クリックして、[エクスポート] を選択します。 エクスポートされたファイルのファイル名とパスワードを入力するように求められます。

この時点で、証明書が完成しました。 IOS アプリケーションの署名に使用する証明書を作成し、その証明書をサーバーアプリケーションで PushSharp と共に使用できる形式に変換しました。 次に、iOS アプリケーションが APNS とどのように対話するかを見てみましょう。

## <a name="registering-with-apns"></a>APNS に登録しています

IOS アプリケーションでリモート通知を受信する前に、APNS に登録する必要があります。 APNS は、一意のデバイストークンを生成し、それを iOS アプリケーションに返します。 IOS アプリケーションは、デバイストークンを取得し、それ自体をアプリケーションサーバーに登録します。 すべての処理が完了すると、登録が完了し、アプリケーションサーバーがモバイルデバイスに通知をプッシュする場合があります。

理論的には、デバイストークンは、iOS アプリケーションが自分自身を APNS に登録するたびに変わる可能性がありますが、実際には、これは頻繁には行われません。 最適化として、アプリケーションは最新のデバイストークンをキャッシュし、変更された場合にのみアプリケーションサーバーを更新することができます。 次の図は、登録とデバイストークンの取得のプロセスを示しています。

 ![](remote-notifications-in-ios-images/image12.png "This diagram illustrates the process of registration and obtaining a device token")

APNS による登録は、現在の `UIApplication` オブジェクトで `RegisterForRemoteNotificationTypes` を呼び出すことによって、アプリケーションデリゲートクラスの `FinishedLaunching` メソッドで処理されます。 IOS アプリケーションが APNS に登録するときは、受信するリモート通知の種類も指定する必要があります。 これらのリモート通知の種類は、列挙 `UIRemoteNotificationType`で宣言されています。 次のコードスニペットは、iOS アプリケーションを登録して、リモート警告とバッジ通知を受信する方法を示しています。

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
    var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

    UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications ();
} else {
    UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
    UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
}
```

APNS 登録要求はバックグラウンドで行われます。応答を受信すると、iOS は `AppDelegate` クラスのメソッド `RegisteredForRemoteNotifications` を呼び出し、登録されたデバイストークンを渡します。 トークンは、`NSData` オブジェクトに格納されます。 次のコードスニペットは、APNS が提供するデバイストークンを取得する方法を示しています。

```csharp
public override void RegisteredForRemoteNotifications (
UIApplication application, NSData deviceToken)
{
    // Get current device token
    var DeviceToken = deviceToken.Description;
    if (!string.IsNullOrWhiteSpace(DeviceToken)) {
        DeviceToken = DeviceToken.Trim('<').Trim('>');
    }

    // Get previous device token
    var oldDeviceToken = NSUserDefaults.StandardUserDefaults.StringForKey("PushDeviceToken");

    // Has the token changed?
    if (string.IsNullOrEmpty(oldDeviceToken) || !oldDeviceToken.Equals(DeviceToken))
    {
        //TODO: Put your own logic here to notify your server that the device token has changed/been created!
    }

    // Save new device token
    NSUserDefaults.StandardUserDefaults.SetString(DeviceToken, "PushDeviceToken");
}
```

何らかの理由で登録が失敗した場合 (デバイスがインターネットに接続されていない場合など)、iOS はアプリケーションデリゲートクラスで `FailedToRegisterForRemoteNotifications` を呼び出します。 次のコードスニペットは、登録が失敗したことを通知する警告をユーザーに表示する方法を示しています。

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>デバイストークンのハウスキーピング

デバイストークンの有効期限が切れるか、時間の経過と共に変化します。 このため、サーバーアプリケーションでは、一部の住宅クリーニングを実行し、期限切れまたは変更されたトークンを削除する必要があります。 期限切れのトークンを持つデバイスにアプリケーションがプッシュ通知として送信すると、APNS は、期限切れのトークンを記録して保存します。 サーバーは APNS を照会して、期限切れになったトークンを確認できます。

APNS は、*フィードバックサービス*を提供するために使用されます。これは、プッシュ通知を送信するために作成された証明書によって認証され、期限切れになったトークンに関するデータを返す HTTPS エンドポイントです。 これは、Apple によって非推奨とされ、削除されました。

代わりに、フィードバックサービスによって以前に報告されたケースに対して、新しい HTTP 状態コードがあります。

> 410-デバイストークンがトピックでアクティブではなくなりました。

さらに、新しい `timestamp` JSON データキーが応答本文に含まれます。

> : Status ヘッダーの値が410の場合、このキーの値は、デバイストークンがトピックに対して有効ではなくなったことが APNs によって確認された最後の時刻になります。
>
> デバイスが新しいタイムスタンプを持つトークンをプロバイダーに登録するまで、通知のプッシュを停止します。

## <a name="summary"></a>まとめ

このセクションでは、iOS でのプッシュ通知に関する主要な概念を紹介します。 Apple Push Notification Gateway サービス (APNS) の役割について説明しました。 次に、APNS に不可欠なセキュリティ証明書の作成と使用について説明します。 最後に、このドキュメントでは、アプリケーションサーバーが*フィードバックサービス*を使用して、期限切れのデバイストークンの追跡を停止する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [通知-ローカル通知とリモート通知のデモンストレーション (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/notifications)
- [開発者向けのローカル通知とプッシュ通知](https://developer.apple.com/notifications/)
- [UIApplication](https://docs.microsoft.com/dotnet/api/uikit.uiapplication)
- [UIRemoteNotificationType](https://docs.microsoft.com/dotnet/api/uikit.UIRemoteNotificationType)
