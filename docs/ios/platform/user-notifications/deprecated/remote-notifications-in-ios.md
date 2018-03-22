---
title: "IOS でのプッシュ通知"
description: "このセクションでは、iOS でプッシュ通知を説明します。 Apple Push 通知ゲートウェイ サービスと iOS アプリケーションに公開通知で再生されるロールが導入されています。 プッシュ通知を有効にして説明するために必要なセキュリティ証明書を作成する方法を説明します。 最後にこのセクションでは、いくつかのアプリケーション サーバーのクライアントのモバイル デバイスを追跡するために必要ハウスキーピング タスクについてはします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3af74fb9d93e22e361f2e3db00961d7955eda689
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="push-notifications-in-ios"></a>IOS でのプッシュ通知

_このセクションでは、iOS でプッシュ通知を説明します。Apple Push 通知ゲートウェイ サービスと iOS アプリケーションに公開通知で再生されるロールが導入されています。プッシュ通知を有効にして説明するために必要なセキュリティ証明書を作成する方法を説明します。最後にこのセクションでは、いくつかのアプリケーション サーバーのクライアントのモバイル デバイスを追跡するために必要ハウスキーピング タスクについてはします。_

> [!IMPORTANT]
> このセクションの情報が iOS 9 に関連し、前に、ままになっていますここで以前の iOS バージョンをサポートするためにします。 IOS 10 以降では、次を参照してください、[ユーザー通知フレームワーク ガイド](~/ios/platform/user-notifications/index.md)iOS デバイスでローカルとリモートの通知の両方をサポートするためです。

プッシュ通知を使用して、簡単な維持する必要があります、モバイル アプリケーションの更新プログラムのサーバー アプリケーションを接続する必要があることを通知するための十分なデータだけを含めます。 たとえば、新しい電子メールが到着すると、サーバー アプリケーションは新しい電子メールが到着するモバイル アプリケーションは通知のみです。 通知には、新しい電子メール自体が含まれていません。 モバイル アプリケーションは、新しい電子メール サーバーから取得が適切です

IOS での通知は、プッシュの中央にある、 *Apple Push Notification ゲートウェイ サービス (APNS)*です。 これは、iOS デバイスにアプリケーション サーバーからルーティング通知は、Apple によって提供されるサービスです。
次の図は、iOS のプッシュ通知のトポロジを示しています。: ![ ](remote-notifications-in-ios-images/image4.png "このイメージは、iOS のプッシュ通知のトポロジを示しています。")

リモートの通知自体は JSON 形式の文字列形式に準拠しているで指定されたプロトコル[、通知のペイロード](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1)のセクションで、 [Local、およびプッシュ通知プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)で、 [iOS 開発者向けドキュメント](https://developer.apple.com/devcenter/ios/index.action)です。

Apple APNS の 2 つの環境の保持:*サンド ボックス*と*運用*環境。 サンド ボックス環境を開発段階におけるテストの目的し、してにある`gateway.sandbox.push.apple.com`TCP ポート 2195 します。 運用環境が配置されているしで見つかることをアプリケーションでは使用を意図したもの`gateway.push.apple.com`TCP ポート 2195 します。

## <a name="requirements"></a>要件

プッシュ通知は、APNS のアーキテクチャによって規定されている次の規則に従う必要があります。

-  **256 バイト メッセージ制限**-通知のメッセージ全体のサイズには 256 バイトを超えない必要があります。
-  **確認メッセージを確認できません**-APNS は、メッセージが目的の受信者に加え、通知と共に送信元が提供されません。 デバイスに到達できず、複数の連続した通知が送信された、最新以外のすべての通知は失われます。 最新の通知のみをデバイスに配信されます。
-  **各アプリケーションにセキュリティで保護された証明書が必要な**-APNS との通信は SSL 経由で行う必要があります。


## <a name="creating-and-using-certificates"></a>作成して、証明書の使用

前のセクションに記載されている環境には、独自の証明書が必要があります。 このセクションでは、証明書を作成、プロビジョニング プロファイルに関連付けるおよび PushSharp で使用するため、Personal Information Exchange 証明書を取得する方法を説明します。

1.  作成するには、証明書に移動し、iOS プロビジョニング ポータル Apple の web サイトで、次のスクリーン ショット (左上のアプリ Id のメニュー項目に注意してください) で示すように。

    [![](remote-notifications-in-ios-images/image5new.png "IOS プロビジョニング ポータル リンゴの web サイトでは、")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  次に、アプリ ID のセクションに移動し、次のスクリーン ショットに示すように新しいアプリ ID を作成します。

    [![](remote-notifications-in-ios-images/image6new.png "アプリ Id に移動し、新しいアプリ ID の作成")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  クリックすると、  **+** ボタン、ことができますアプリ id、説明、およびバンドル Id を入力するように次のスクリーン ショットに示すようにします。

    [![](remote-notifications-in-ios-images/image7new.png "アプリ id、説明、およびバンドル Id を入力します。")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. 選択することを確認**アプリ ID の明示的な**、バンドル Id が付いていないことと、`*`です。 これは、複数のアプリケーションに適している識別子が作成され、1 つのアプリケーションのプッシュ通知証明書がある必要があります。

1. [アプリ サービス]、次のように選択します**Push Notifications**:。

    [![](remote-notifications-in-ios-images/image8new.png "プッシュ通知を選択します。")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. キーを押します**送信**を新しいアプリ ID の登録を確認します。

    [![](remote-notifications-in-ios-images/image9new.png "新しいアプリ ID の登録を確認します。")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  次に、アプリ ID の証明書を作成する必要があります。 左側にあるナビゲーション ウィンドウを参照**証明書 > すべて**を選択し、 `+`  ボタン、次のスクリーン ショットに示すようにします。

    [![](remote-notifications-in-ios-images/image10new.png "アプリ ID の証明書を作成します。")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  開発または運用証明書を使用するかどうかを選択します。

    [![](remote-notifications-in-ios-images/image11new.png "開発または運用証明書を選択します。")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. 先ほど作成した新しいアプリ ID を選択します。

    [![](remote-notifications-in-ios-images/image12new.png "先ほど作成した新しいアプリ ID を選択します")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  これを作成するプロセスを実行する手順が表示されます、*証明書署名要求*を使用して、**キーチェーン アクセス**mac アプリケーション

7.  証明書を作成すると、アプリケーションに署名して APNs で登録することをビルド プロセスの一部として使用するあります。 これには、作成し、証明書を使用するプロビジョニング プロファイルのインストールが必要です。

8.  プロビジョニング プロファイルの開発を作成するに移動、**プロビジョニング プロファイル**セクションし、先ほど作成したアプリ Id を使用して、作成する手順に従います。

9.  プロビジョニング プロファイルを作成すると開きます**Xcode オーガナイザー**し、それを更新します。 プロビジョニング プロファイルを作成した場合は表示されません iOS プロビジョニング ポータルから、プロファイルをダウンロードして、手動でインポートする必要がある可能性があります。 次のスクリーン ショットは、追加されたプロビジョニング プロファイルで開催者の例を示します。

    [![](remote-notifications-in-ios-images/image13new.png "このスクリーン ショットは、追加のプロビジョニング プロファイルで開催者の例を示します")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  この時点で作成したプロビジョニング プロファイルを使用する Xamarin.iOS プロジェクトを構成する必要があります。 これから**プロジェクト オプション**ダイアログ **[iOS バンドル署名 *]** タブで、次のスクリーン ショットに示すとして。

    [![](remote-notifications-in-ios-images/image11.png "新しくプロビジョニング プロファイルを作成してこれを使用する Xamarin.iOS プロジェクトを構成します。")](remote-notifications-in-ios-images/image11.png#lightbox)



この時点でアプリケーションは、プッシュ通知を使用するように構成します。 ただし、証明書に必要ないくつかの手順がまだです。 この証明書は、Personal Information Exchange (PKCS12) 証明書を必要とする PushSharp と互換性がありません DER 形式でです。 PushSharp によって使用されるように、証明書を変換するには、これらの最終的な手順を実行します。

1.  **証明書ファイルをダウンロード**-iOS プロビジョニング ポータルにログインが証明書 タブを選択、プロビジョニング プロファイルの正しい選択と関連付けられている証明書を選択**ダウンロード**です。
1.  **キーチェーン アクセス を開きます**-これは、アプリケーションは、OS X 上でパスワード管理システムへの GUI インターフェイスです。
1.  **証明書をインポート**- 場合は、証明書が既にインストールされていない**ファイル.アイテムをインポート**キーチェーン アクセス メニューからです。 上、エクスポートした証明書に移動し、それを選択します。
1.  **証明書をエクスポート**関連秘密キーが表示されるように、証明書を展開 -、キーを右クリックし、エクスポートを選択します。 ファイル名と、エクスポートするファイルのパスワードの入力するように求められます。

この時点で証明書を使用して行われます。 IOS アプリケーションの署名に使用される証明書を作成して、その証明書をサーバー アプリケーションで PushSharp で使用できる形式に変換おがします。 [次へ] APNS と iOS アプリケーションが対話する方法を見てみましょう。

## <a name="registering-with-apns"></a>APNS との登録

IOS の前に、アプリケーションは APNS に登録する必要がありますリモートの通知を受け取ることができます。 APNS は、一意のデバイス トークンを生成し、iOS アプリケーションに返すことができます。 IOS アプリケーションは、デバイス トークンを受け取るし、アプリケーション サーバーに登録します。 すべてのこのようなし、登録が完了したら、アプリケーション サーバーは、モバイル デバイスに通知をプッシュする可能性があります。

理論上は、デバイス トークンが、実際には実行されませんを多くの場合、iOS アプリケーションでは、自身を登録、APNS と各時間を変更できます。 最適化の手法として、アプリケーションは、最新のデバイス トークンをキャッシュし、変更時にのみ、アプリケーション サーバーを更新可能性があります。 次の図は、登録し、デバイス トークンを取得するプロセスを示しています。

 ![](remote-notifications-in-ios-images/image12.png "この図は、登録と、デバイス トークンを取得するプロセスを示しています。")

APNS と登録の処理で、`FinishedLaunching`メソッドを呼び出してアプリケーション デリゲート クラスの`RegisterForRemoteNotificationTypes`現在`UIApplication`オブジェクト。 IOS アプリケーションには、登録すると、APNS とリモートの通知の種類が必要な受信することも指定があります。 これらのリモートの通知の種類が列挙体で宣言されている`UIRemoteNotificationType`です。 次のコード スニペットは、リモートのアラートおよびバッジ通知を受信する iOS アプリケーションを登録する方法の例を示します。

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

APNS 登録要求のバック グラウンド - で応答が受信されると、iOS メソッドを呼び出して`RegisteredForRemoteNotifications`で、`AppDelegate`クラスし、登録されたデバイス トークンを渡します。 トークンに含まれる、`NSData`オブジェクト。 次のコード スニペットは、指定されたその APNS デバイス トークンを取得する方法を示します。

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

IOS が呼び出す場合は、登録は、何らかの理由 (など、デバイスに接続されていないインターネット) 失敗すると、`FailedToRegisterForRemoteNotifications`デリゲート クラスのアプリケーションにします。 次のコード スニペットは、登録が失敗したことを知らせる、ユーザーにアラートを表示する方法を示しています。

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>デバイス トークン ハウスキーピング

デバイス トークンは期限切れか、時間の経過と共に変更します。 したがって、サーバー アプリケーションは、一部ハウス クリーニングを行うし、これらの有効期限が切れたまたは変更されたトークンを削除する必要があります。 アプリケーションを期限切れのトークンを持つデバイスにプッシュ通知として送信すると、APNS は記録して、その期限切れのトークンを保存します。 サーバーをどのようなトークンの有効期限が切れた調べる APNS をクエリし、可能性があります。

APNS を提供するために使用する*フィードバック サービス*-HTTPS エンドポイントを作成するデータを送り返すにプッシュ通知と送信はどのようなトークンの有効期限が切れている証明書を使用して認証します。 これは、Apple が推奨されなくなりました、削除されています。

代わりに、フィードバック サービスによって以前に報告された場合は、新しい HTTP ステータス コードがあります。

> 410-デバイスのトークンは、トピックに対して、アクティブではなくなりました。

さらに、新しい`timestamp`応答本文に JSON データのキーになります。

> 場合の値、: ステータス ヘッダーが 410 で、このキーの値が、前回の APNs がデバイスのトークンが、トピックの有効な不要になったことを確認します。
>
> デバイスは、プロバイダーとそれ以降のタイムスタンプを持つトークンを登録するまで、通知をプッシュを停止します。

## <a name="summary"></a>まとめ

このセクションでは、iOS でプッシュ通知を囲む主要な概念を紹介します。 Apple Push Notification ゲートウェイ サービス (APNS) の役割を説明します。 また、作成し、APNS に不可欠なセキュリティ証明書を使用し、説明します。 最後にこのドキュメントで終了しましたアプリケーション サーバーの使用方法について説明、*フィードバック サービス*追跡の停止 デバイス トークン有効期限が切れています。


## <a name="related-links"></a>関連リンク

- [通知 - ローカルおよびリモートの通知 (サンプル) のデモンストレーション](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [ローカル開発者用のプッシュ通知と](https://developer.apple.com/notifications/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
