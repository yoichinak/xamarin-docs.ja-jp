---
title: IOS でプッシュ通知
description: このドキュメントでは、iOS 9 およびそれ以前のプッシュ通知と連携する方法について説明します。 証明書の場合、Apple プッシュ通知ゲートウェイ サービス (APNS)、その他の登録がについて説明します。
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f11f5d1cbde0f5eae27215af8eb6544be46c0206
ms.sourcegitcommit: ef04a4ae1b19c1854a8e4e8315516d4030f4bbd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/08/2018
ms.locfileid: "39654816"
---
# <a name="push-notifications-in-ios"></a>IOS でプッシュ通知

> [!IMPORTANT]
> このセクションの情報は、iOS 9 に関連し、前に、そのままにしたここで以前の iOS バージョンをサポートするためにします。 IOS 10 以降を参照してください、[ユーザー通知フレームワーク ガイド](~/ios/platform/user-notifications/index.md)iOS デバイスでローカルとリモート通知の両方をサポートするためです。

プッシュ通知は、簡単に保持する必要がありが更新プログラムのサーバー アプリケーションを接続する必要がありますが、モバイル アプリケーションに通知するための十分なデータだけを格納します。 たとえば、新しい電子メールが到着すると、サーバー アプリケーションは新しい電子メールが到着したモバイル アプリケーションは通知のみです。 通知には、新しい電子メール自体が含まれていません。 モバイル アプリケーションは、適切な時に、サーバーから新しい電子メールを取得

IOS での通知は、プッシュの中央にある、 *Apple プッシュ通知ゲートウェイ サービス (APNS)* します。 これは、アプリケーション サーバーから iOS デバイスへの通知のルーティングを担当する Apple によって提供されるサービスです。
次の図は、iOS のプッシュ通知のトポロジを示しています。: ![](remote-notifications-in-ios-images/image4.png "このイメージは、iOS のプッシュ通知のトポロジを示しています。")

リモート通知自体は JSON 形式の文字列形式に準拠しているで指定されたプロトコル[、通知ペイロード](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1)のセクション、[ローカルおよびプッシュ通知プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)で、 [iOS 開発者向けドキュメント](https://developer.apple.com/devcenter/ios/index.action)します。

Apple APNS の 2 つの環境の保持: を*サンド ボックス*と*運用*環境。 サンド ボックス環境の開発フェーズ中にテストするためのものし、で見つかります`gateway.sandbox.push.apple.com`2195 のポート TCP でします。 運用環境が配置されているしにあるアプリケーションで使用されるものでは`gateway.push.apple.com`2195 のポート TCP でします。

## <a name="requirements"></a>要件

プッシュ通知は、APNS のアーキテクチャによって決まりますが、次の規則に従う必要があります。

-  **256 バイトのメッセージ制限**-通知のメッセージ全体のサイズは、256 バイトを超えていない必要があります。
-  **受信確認なし**-APNS には、送信者の通知メッセージは、目的の受信者を作成するには示しません。 デバイスに到達できず、複数の連続した通知が送信される、最も最近を除くすべての通知は失われます。 最新の通知のみをデバイスに配信されます。
-  **各アプリケーションにセキュリティで保護された証明書が必要な**-APNS との通信は SSL 経由で行う必要があります。


## <a name="creating-and-using-certificates"></a>作成して、証明書の使用

前のセクションで説明されている環境には、独自の証明書が必要です。 このセクションでは、証明書を作成、プロビジョニング プロファイルに関連付けるおよび PushSharp で使用するため、Personal Information Exchange 証明書を取得する方法について説明します。

1.  作成するには、証明書を参照してください、iOS Provisioning Portal Apple の web サイトの (左側のアプリ Id のメニュー項目に注意してください) の次のスクリーン ショット。

    [![](remote-notifications-in-ios-images/image5new.png "IOS プロビジョニング ポータルりんごの web サイト")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  次に、アプリ ID のセクションに移動し、次のスクリーン ショットに示すように新しいアプリ ID の作成します。

    [![](remote-notifications-in-ios-images/image6new.png "アプリ Id のセクションに移動し、新しいアプリ ID の作成")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  クリックすると、 **+** ボタンができます、アプリ ID の説明とバンドル識別子を入力する次のスクリーン ショットに示すようにします。

    [![](remote-notifications-in-ios-images/image7new.png "アプリ id、説明、およびバンドル Id を入力します。")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. 選択することを確認**明示的なアプリ ID**およびバンドル Id で終わるわけで、`*`します。 これは、複数のアプリケーションの適切な識別子が作成され、1 つのアプリケーションのプッシュ通知証明書がある必要があります。

1. [アプリ サービス]、次のように選択します**Push Notifications**:。

    [![](remote-notifications-in-ios-images/image8new.png "プッシュ通知を選択します。")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. キーを押します**送信**を新しいアプリ ID の登録を確認します。

    [![](remote-notifications-in-ios-images/image9new.png "新しいアプリ ID の登録を確認します。")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  次に、アプリ ID の証明書を作成する必要があります。 左側のナビゲーションで**証明書 > すべて**を選択し、 `+`  ボタン、次のスクリーン ショットに示すように。

    [![](remote-notifications-in-ios-images/image10new.png "アプリ ID の証明書を作成します。")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  開発または実稼働の証明書を使用したいかどうかを選択します。

    [![](remote-notifications-in-ios-images/image11new.png "開発または実稼働の証明書を選択します。")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. 先ほど作成した新しいアプリ ID を選択します。

    [![](remote-notifications-in-ios-images/image12new.png "先ほど作成した新しいアプリ ID を選択します。")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  これを作成するプロセスを実行する命令が表示されます、*証明書署名要求*を使用して、 **Keychain Access** mac アプリケーション

7.  証明書を作成するは、ことで APNs に登録することができるように、アプリケーションの署名にビルド プロセスの一環としてに使用する必要があります。 これは、作成し、証明書を使用するプロビジョニング プロファイルのインストールが必要です。

8.  開発プロビジョニング プロファイルを作成するに移動、 **Provisioning Profiles**セクションし、先ほど作成したアプリ Id を使用してこれを作成する手順に従います。

9.  プロビジョニング プロファイルを作成したら、開く**Xcode オーガナイザー**し、それを更新します。 プロビジョニング プロファイルを作成した場合は表示されません、プロファイルを iOS プロビジョニング ポータルからダウンロードし、手動でインポートする必要があります。 次のスクリーン ショットは、プロビジョニング プロファイルを追加、オーガナイザーの例を示します。

    [![](remote-notifications-in-ios-images/image13new.png "このスクリーン ショットがプロビジョニング プロファイルを追加、オーガナイザーの例を示します")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  この時点でこの新しく作成されたプロビジョニング プロファイルを使用する Xamarin.iOS プロジェクトを構成する必要があります。 これから**プロジェクト オプション**ダイアログ **[iOS バンドル署名]** タブで、次のスクリーン ショットに示すように。

    [![](remote-notifications-in-ios-images/image11.png "この新しく作成されたプロビジョニング プロファイルを使用する Xamarin.iOS プロジェクトを構成します。")](remote-notifications-in-ios-images/image11.png#lightbox)

この時点でアプリケーションがプッシュ通知と連携するように構成します。 ただし、証明書に必要ないくつかの手順がまだです。 この証明書では、DER 形式では、Personal Information Exchange (PKCS12) 証明書を必要とする、PushSharp と互換性がありません。 PushSharp で使用できるように、証明書を変換するには、最後の手順を実行します。

1.  **証明書ファイルをダウンロード**-iOS プロビジョニング ポータルにログインが証明書 タブを選択で、正しいプロビジョニング プロファイルと選択に関連付けられている証明書を選択します。**ダウンロード**します。
1.  **Keychain Access を開く**-これは、アプリケーションは OS X でパスワード管理システムの GUI インターフェイスです。
1.  **証明書をインポート**証明書、既にインストールされていない場合、-**ファイル.アイテムをインポート**Keychain Access メニューから。 上、エクスポートされる証明書に移動し、それを選択します。
1.  **証明書をエクスポート**- 関連付けられている秘密キーが表示されるように、証明書を展開し、キーを右クリックし、エクスポートを選択します。 ファイル名と、エクスポートするファイルのパスワードのメッセージが表示されます。

この時点で証明書の使用が完了します。 IOS アプリケーションの署名に使用される証明書を作成し、PushSharp をサーバー アプリケーションで使用できる形式にその証明書を変換しました。 [次へ] iOS アプリケーションを APNS と対話する方法を見てみましょう。

## <a name="registering-with-apns"></a>APNS に登録します。

アプリケーションは iOS の前に、APNS に登録する必要がありますリモート通知を受信できます。 APNS では、一意のデバイス トークンを生成し、いる iOS アプリケーションに返されます。 IOS アプリケーションは、デバイス トークンを取得しと、アプリケーション サーバーに登録します。 すべてのこのようなし、登録が完了すると、アプリケーション サーバーはモバイル デバイスに通知をプッシュする可能性があります。

理論上は、デバイス トークンが、実際にはこれが発生しないを多くの場合、iOS アプリケーションでは、自身を登録、APNS で毎回を変更できます。 最適化の手法としては、アプリケーションは、最新のデバイス トークンをキャッシュし、変更時にのみ、アプリケーション サーバーを更新可能性があります。 次の図は、登録とデバイス トークンを取得するプロセスを示しています。

 ![](remote-notifications-in-ios-images/image12.png "この図は、登録とデバイス トークンを取得するプロセスを示しています。")

APNS 登録の処理、`FinishedLaunching`メソッドを呼び出すことによって、アプリケーション デリゲート クラスの`RegisterForRemoteNotificationTypes`現在`UIApplication`オブジェクト。 APNS に登録される iOS アプリケーションをどのような種類のリモート通知が受信するようにすることも指定があります。 これらのリモート通知の種類が列挙体で宣言されている`UIRemoteNotificationType`します。 次のコード スニペットでは、リモート通知、バッジ通知を受信する iOS アプリケーションを登録する方法の例を示します。

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

APNS 登録要求のバック グラウンド - で、応答が受信されると、iOS メソッドを呼び出して`RegisteredForRemoteNotifications`で、`AppDelegate`クラスし、登録済みのデバイス トークンを渡します。 トークンが含まれる、`NSData`オブジェクト。 次のコード スニペットは、提供されるその APNS にデバイス トークンを取得する方法を示します。

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

IOS が呼び出されます (など、デバイスに接続されていないインターネット) 何らかの理由で登録が失敗した場合、`FailedToRegisterForRemoteNotifications`デリゲート クラスをアプリケーションにします。 次のコード スニペットでは、登録が失敗したことを知らせる、ユーザーにアラートを表示する方法を示します。

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>デバイス トークン ハウスキーピング

デバイス トークンは有効期限が切れるか、時間の経過と共に変化します。 したがって、サーバー アプリケーションをいくつかの家のクリーニングを行い、これらの有効期限が切れた、または変更されたトークンを削除する必要があります。 アプリケーションを期限切れのトークンを持つデバイスにプッシュ通知として送信すると、APNS が記録して、その期限切れのトークンを保存します。 サーバーは、APNS はどのようなトークンが期限切れを確認するを照会し、可能性があります。

APNS を提供するために使用する*フィードバック サービス*-HTTPS エンドポイントを介してデータを送信するプッシュ通知と送信バックに関するどのようなトークンの有効期限が切れた作成された証明書認証を行う。 これは、Apple で非推奨と、削除されています。

代わりに、フィードバックのサービスによって以前に報告された場合の新しい HTTP ステータス コードがあります。

> 410 - デバイス トークンは、トピックでは、アクティブではなくなりました。

さらに、新しい`timestamp`応答本文で JSON データのキーになります。

> 場合の値、: 状態のヘッダーは 410 で、このキーの値は前回 APNs が、デバイス トークンのトピックでは、有効なが不要になったことを確認します。
>
> デバイスは、プロバイダーとそれ以降のタイムスタンプを持つトークンを登録するまでのプッシュ通知を停止します。

## <a name="summary"></a>まとめ

このセクションでは、iOS でプッシュ通知に関連する主要な概念を紹介します。 ロールの Apple プッシュ通知ゲートウェイ サービス (APNS) がについて説明します。 また、作成し、APNS に不可欠なセキュリティ証明書を使用し、について説明します。 このドキュメントのアプリケーション サーバーの使用方法についての完了最後に、*フィードバック Services*期限切れのデバイス トークンの追跡の停止にします。


## <a name="related-links"></a>関連リンク

- [通知 - ローカルおよびリモートの通知 (サンプル) のデモ](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [ローカルおよびプッシュ通知を開発者向け](https://developer.apple.com/notifications/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
