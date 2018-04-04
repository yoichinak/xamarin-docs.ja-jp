---
title: 強化されたユーザーへの通知
description: この記事では、すべての方法を Xamarin.iOS アプリで使用する方法と iOS 10 ユーザー通知が強化されたことについて説明します。
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 9fd3ff17dc9af3fd30a7d5b31e8cea7ff8669a51
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="enhanced-user-notifications"></a>強化されたユーザーへの通知

_この記事では、すべての方法を Xamarin.iOS アプリで使用する方法と iOS 10 ユーザー通知が強化されたことについて説明します。_

初めて使用する iOS 10 では、ユーザー通知の配信とローカルおよびリモートの通知の処理のフレームワークを利用します。 このフレームワークを使用して、アプリまたはアプリ拡張機能をスケジュールできますローカルに関する通知の配信場所などの条件のセットまたは 1 日の時刻を指定することで。

## <a name="about-user-notifications"></a>ユーザー通知について

前述のように、新しいユーザーの通知フレームワークにより配信とローカルおよびリモートの通知の処理。 このフレームワークを使用して、アプリまたはアプリ拡張機能をスケジュールできますローカルに関する通知の配信場所などの条件のセットまたは 1 日の時刻を指定することで。

さらに、アプリまたは拡張機能が表示される (および可能性のある変更できます) ローカルとリモートの両方の通知、ユーザーの iOS デバイスに配信されるようにします。

新しいユーザー通知の UI フレームワークが使用するは、アプリまたはアプリの拡張機能をユーザーに表示するときに、ローカルおよびリモートの両方の通知の外観をカスタマイズします。

このフレームワークは、アプリがユーザーに通知を配信できる、次の方法を提供します。

- **Visual アラート**バナーとして画面の上部から下へ通知ロールアップ場所 - です。
- **音と振動**-通知を関連付けることができます。
- **アプリのアイコンのバッジ**- アプリのアイコンが新しいコンテンツが利用可能な未読の電子メール メッセージの数などであることを示すバッジを表示します。

さらに、ユーザーの現在のコンテキストによっては、通知が表示されるさまざまな方法です。

- デバイスがロックされている場合は、通知はロール ダウン、画面の上部からバナーとして。
- デバイスがロックされている場合は、ユーザーのロック画面で通知が表示されます。
- ユーザーが通知を受信しなかった場合は、通知センターを開き、任意の使用可能な待機通知がありますを表示、ことができます。

Xamarin.iOS アプリには、2 種類のユーザーへの通知を送信することがあることがあります。

- **ローカル通知**-これらは、ユーザーのデバイスでローカルにインストールされているアプリによって送信されます。
- **リモート通知**-リモートから送信されたサーバーといずれかのユーザーに提示またはアプリのコンテンツのバック グラウンドの更新をトリガーします。

### <a name="about-local-notifications"></a>ローカルの通知について

IOS アプリを送信できるローカル通知は、次の機能と属性があります。

- アプリでは、ローカル ユーザーのデバイス上で送信されます。 
- 構成できます時間または場所を使用するベース トリガーします。 
- アプリがユーザーのデバイスで通知をスケジュールし、トリガーの条件が満たされたときに表示されます。
- ユーザーは、通知を使用して、アプリにコールバックが表示されます。

ローカルの通知の例をいくつかは次のとおりです。

- 予定表の通知
- アラーム
- 対応するトリガーの場所

詳細については、Apple を参照してください[ローカルおよびリモート通知プログラミング ガイド](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)ドキュメント。

### <a name="about-remote-notifications"></a>リモートの通知について

IOS アプリを送信できるリモート通知は、次の機能と属性があります。

- アプリでは、その通信相手とサーバー側コンポーネントがあります。
- Apple Push Notification サービス (APNs) を使用すると、ユーザーのデバイスに開発者のクラウド ベースのサーバーからリモートの通知の最善の努力配信を送信します。
- アプリがリモートの通知を受信するとは、ユーザーに表示されます。
- ユーザーは、通知を使用して、アプリにコールバックが表示されます。

リモートの通知の例をいくつかは次のとおりです。

- ニュースのアラート
- スポーツの最新情報
- インスタント メッセージのメッセージ

2 つの種類のリモートの通知は iOS アプリで使用です。

- **ユーザー向け**-デバイスのユーザーにこれらが表示されます。
- **更新プログラムをサイレント**-バック グラウンドで iOS アプリの内容を更新するメカニズムを提供します。 サイレント更新プログラムを受信すると、最新のコンテンツを削除するサーバー pull にアプリがに到達できます。

詳細については、Apple を参照してください[ローカルおよびリモート通知プログラミング ガイド](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)ドキュメント。

### <a name="about-the-existing-notifications-api"></a>既存の通知 API について

10、iOS の前に iOS アプリを使用して、`UIApplication`システムに通知を登録して (または使用して、時間の場所) の通知のトリガー方法をスケジュールします。

開発者は、既存の通知 API を使用する場合に発生する可能性がありますをいくつかの問題があります。

- ローカルまたはリモートの通知のコードの重複を招く可能性があるに必要なさまざまなコールバックが発生しました。
- アプリは、システムでスケジュールされていた後されていた通知の制御を限られていました。
- すべての Apple の既存のプラットフォーム間で異なるレベルのサポートがありました。

### <a name="about-the-new-user-notification-framework"></a>新しいユーザーの通知フレームワークについて

Apple iOS 10 で新しいユーザーに通知のフレームワークは、既存の置換は導入されましたが`UIApplication`上記で説明したメソッド。

ユーザー通知の framework には、次の用意されています。

- 機能パリティを含むことが容易なコードを移植する既存のフレームワークから以前の方法を理解して API です。
- ユーザーに送信する豊富な通知を許可するコンテンツのオプションの拡張セットが含まれます。
- 同じコードおよびコールバックでは、ローカルおよびリモートの通知の両方を処理できます。
- 通知をユーザーが操作するときに、アプリに送信されるコールバックを処理するプロセスを簡略化します。
- 保留中と配信の両方の通知を削除するか、通知を更新する機能などの管理を強化します。
- 通知のアプリでプレゼンテーションを行う機能を追加します。
- スケジュールを設定し、アプリの拡張機能内からの通知を処理する機能を追加します。
- 通知自体の新しい拡張機能ポイントを追加します。 

新しいユーザー通知の framework には、統一された通知 API、プラットフォームの複数を含む Apple をサポートしているが用意されています。 

- **iOS** -完全な管理および通知をスケジュール設定をサポートします。
- **tvOS** -ローカルおよびリモートの通知のバッジ アプリのアイコンに機能を追加します。
- **watchOS** -、Apple Watch、ユーザーのペアの iOS デバイスから通知を転送する機能を追加し、ウォッチ アプリをウォッチ自体で直接のローカルの通知を行うには、します。

詳細については、Apple を参照してください[UserNotifications フレームワーク参照](https://developer.apple.com/reference/usernotifications)と[UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)ドキュメント。

## <a name="preparing-for-notification-delivery"></a>通知の配信の準備

IOS の前にアプリは、システムにアプリを登録する必要があります、ユーザーに通知を送信でき、通知がユーザーに中断であるため、アプリに送信する前にアクセス許可を明示的に要求する必要があります。

これには、ユーザーがアプリを承認通知要求の 3 つのレベルがあります。

- バナーが表示されます。
- サウンド警告を出します。
- アプリのアイコンをバッジです。

さらに、これらの承認レベルを要求され、ローカルおよびリモートの両方の通知を設定する必要があります。

次のコードを追加することによって、アプリを起動するとすぐに通知のアクセス許可を要求すること、`FinishedLaunching`のメソッド、`AppDelegate`希望する通知の種類と設定 (`UNAuthorizationOptions`)。

```csharp
using UserNotifications;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    return true;
}
```

さらに、ユーザー常に権限を変更通知を使用して、時、アプリケーションのため、**設定**デバイス上のアプリです。 次のコードを使用して通知を表示する前に、ユーザーの要求の通知の特権のアプリを確認します。

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
}); 
``` 

### <a name="configuring-the-remote-notifications-environment"></a>リモート通知環境の構成

新しい iOS 10 に、開発者が開発または運用環境のいずれかとしてどのような環境のプッシュ通知がで実行されている OS を伝える必要があります。 この情報にアクセスできないことができます、次のような通知と iTune App Store に送信されるときに拒否されているアプリで生成されます。

> 不足しているプッシュ通知の権利でアプリには、Apple の Push Notification サービスの API が含まれていますが、`aps-environment`権利がアプリの署名にありません。

必要な権利を提供するには、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. ダブルクリックして、`Entitlements.plist`ファイルで、**ソリューション パッド**編集用に開きます。
2. 切り替えて、**ソース**ビュー。 

    [![](enhanced-user-notifications-images/setup01.png "ソース ビュー")](enhanced-user-notifications-images/setup01.png#lightbox)
3. クリックして、 **+**新しいキーを追加するボタンをクリックします。
4. 入力`aps-environment`の**プロパティ**のままにして、**型**として`String`いずれかを入力および`development`または`production`の**値**: 

    [![](enhanced-user-notifications-images/setup02.png "Aps 環境プロパティ")](enhanced-user-notifications-images/setup02.png#lightbox)
5. 変更内容をファイルに保存します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. ダブルクリックして、`Entitlements.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
3. クリックして、 **+**新しいキーを追加するボタンをクリックします。
4. 入力`aps-environment`の**プロパティ**のままにして、**型**として`String`いずれかを入力および`development`または`production`の**値**: 

    [![](enhanced-user-notifications-images/setup02w.png "Aps 環境プロパティ")](enhanced-user-notifications-images/setup02.png#lightbox)
5. 変更内容をファイルに保存します。

-----

### <a name="registering-for-remote-notifications"></a>リモートの通知の登録

実行する必要があります、アプリは送信すると、リモートの通知を受け取る、_トークン登録_既存を使用して`UIApplication`API です。 この登録には、デバイスには、ライブ ネットワーク接続のアクセス、APNs、アプリに送信されるために必要なトークンを生成する必要があります。 アプリは、開発者のサーバー側のアプリをリモートの通知を登録するには、このトークンを転送する必要があります。

[![](enhanced-user-notifications-images/token01.png "トークンの登録の概要")](enhanced-user-notifications-images/token01.png#lightbox)

必要な登録を初期化するために、次のコードを使用します。

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

開発者のサーバー側のアプリに送信されるトークンは、通知のペイロードの get の一部に送信されるとき、サーバーから APNs リモート通知を送信するときに含まれる必要があります。

[![](enhanced-user-notifications-images/token02.png "通知のペイロードの一部として含まれているトークン")](enhanced-user-notifications-images/token02.png#lightbox)

トークンは、通知と開くか、通知に応答するために使用するアプリに一緒に結合するキーとして機能します。

詳細については、Apple を参照してください[ローカルおよびリモート通知プログラミング ガイド](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)ドキュメント。

## <a name="notification-delivery"></a>通知の配信

アプリの完全に登録されているとから必要なアクセス許可を要求および付与、ユーザーがアプリ今すぐ通知の送受信を行う準備ができてです。 

### <a name="providing-notification-content"></a>通知の内容を提供します。

新しい 10、iOS にすべての通知の両方を含めること、**タイトル**と**字幕**が常に表示されます、**本文**の通知の内容。 また、新しいは追加する機能**メディアの添付ファイル**通知の内容にします。

ローカル通知の内容を作成するには、次のコードを使用します。

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

リモートの通知プロセスは似ています。

```csharp
{
    "aps":{
        "alert":{
            "title":"Notification Title",
            "subtitle":"Notification Subtitle",
            "body":"This is the message body of the notification."
        },
        "badge":1
    }
}
```

### <a name="scheduling-when-a-notification-is-sent"></a>送信されるときに、通知のスケジュール設定

アプリを作成された通知のコンテンツには、スケジュールを設定して、ユーザーに通知が表示 必要があります、*トリガー*です。 iOS 10 では、次の 4 つの異なるトリガーの種類を提供します。

- **プッシュ通知**- リモート通知だけを使用し、デバイスで実行されているアプリをパッケージ化 APNs 通知が送信時に発生します。
- **間隔を時間**-間隔を今すぐ終了点がいくつかに将来と開始時刻をスケジュールするローカル通知を許可します。 たとえば、`var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`
- **日付をカレンダー** -特定の日付と時刻のスケジュールを設定するローカル通知を許可します。
- **場所ベース**-iOS デバイスが入力または特定の地理的位置から離れずまたは任意の Bluetooth ビーコンに対して指定されたに近づいているときにスケジュールするローカル通知を許可します。

アプリを呼び出す必要があるローカルの通知の準備ができたら、`Add`のメソッド、`UNUserNotificationCenter`スケジュールをユーザーに表示されるオブジェクト。 リモート通知は、サーバー側のアプリ通知のペイロードを送信、APNs、ユーザーのデバイスにログオン パケットを送信します。

すべての断片の統合サンプル ローカル通知ようになります。

```csharp
using UserNotifications;
...

var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);

var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

## <a name="handling-foreground-app-notifications"></a>フォア グラウンド アプリの通知の処理

新しい 10、iOS にアプリ通知を処理できる異なる方法で、通知がトリガーされる、フォア グラウンドである場合。 提供することによって、`UNUserNotificationCenterDelegate`を実装して、`UserNotificationCenter`メソッド、アプリは、通知を表示するための責任を占有できます。 例えば:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        #region Constructors
        public UserNotificationCenterDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void WillPresentNotification (UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
        {
            // Do something with the notification
            Console.WriteLine ("Active Notification: {0}", notification);

            // Tell system to display the notification anyway or use
            // `None` to say we have handled the display locally.
            completionHandler (UNNotificationPresentationOptions.Alert);
        }
        #endregion
    }
}
```

このコードは、単にコンテンツを書き込むの`UNNotification`通知の標準的な警告を表示するアプリケーションの出力およびシステムに要求します。 

アプリが前面になっていた場合に通知をそれ自体を表示およびいないを使用するシステムの既定値を渡す場合`None`完了ハンドラーにします。 例:

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

配置でこのコードでは、開く、`AppDelegate.cs`ファイルを編集するため、変更、`FinishedLaunching`など、次のメソッドを検索します。

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    // Watch for notifications while the app is active
    UNUserNotificationCenter.Current.Delegate = new UserNotificationCenterDelegate ();

    return true;
}
```

このコードは、カスタムのアタッチ`UNUserNotificationCenterDelegate`上から現在`UNUserNotificationCenter`がアクティブであるときに、アプリで通知を処理できるようにし、フォア グラウンドでします。

## <a name="notification-management"></a>通知の管理

新しい 10、iOS に通知管理が保留中と配信の両方の通知にへアクセスを提供し、削除、更新、またはこれらの通知を昇格する機能を追加します。

通知の管理の重要な部分は、_要求識別子_が作成され、システムでスケジュールされているときに、通知に割り当てられました。 リモート通知は、これを割り当てる新しいを介して`apps-collapse-id`の HTTP 要求ヘッダー フィールド。

要求識別子を使用して、アプリで通知の管理を実行することを希望する通知を選択します。

### <a name="removing-notifications"></a>通知を削除します。

保留中の通知は、システムから削除するには、次のコードを使用します。

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

既に配信された通知を削除するには、次のコードを使用します。

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>既存の通知の更新

既存の通知を更新するには、変更 (新しいトリガー時) など必要なパラメーターを使用して、新しい通知を作成し、変更する必要があることを示す通知と同じ要求の識別子を使用してシステムに追加します。 例:


```csharp
using UserNotifications;
...

// Rebuild notification
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

// New trigger time
var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger (10, false);

// ID of Notification to be updated
var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

// Add to system to modify existing Notification
UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

既に配信された通知は、既存の通知は更新され、ユーザーによって既に読み取られている場合は、リスト、ホームとロック画面で、通知センターの上部に昇格されますを取得します。

## <a name="working-with-notification-actions"></a>通知アクションの使用

10、iOS では、ユーザーに配信される通知は、静的なものでと、(からカスタム動作に組み込み) に、ユーザーが操作できるいくつかの方法を提供します。

IOS アプリが応答するアクションの 3 つの種類があります。

- **既定のアクションが**-これは、ユーザーがアプリを開き、特定の通知の詳細を表示する通知をタップしたときにします。
- **カスタム アクション**-これら iOS 8 で追加され、通知から直接アプリを起動することがなくカスタム タスクを実行するユーザーに簡単です。 カスタマイズ可能なタイトルを含むボタンの一覧またはテキスト入力フィールド (ここで、アプリは指定された少量要求を処理する時間の) 背景または前景のいずれかを実行することができます (ここで、アプリを起動 fu をフォア グラウンドでのいずれかとしてを提示します。lfill 要求)。 カスタム アクションは、iOS および watchOS の両方で使用できます。
- **アクションを破棄**-ユーザーが特定の通知を閉じるときにこのアクションは、アプリに送信されます。

### <a name="creating-custom-actions"></a>カスタム アクションの作成

作成してシステムにカスタム アクションを登録するには、次のコードを使用します。

```csharp
// Create action
var actionID = "reply";
var title = "Reply";
var action = UNNotificationAction.FromIdentifier (actionID, title, UNNotificationActionOptions.None);

// Create category
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);
    
// Register category
var categories = new UNNotificationCategory [] { category };
UNUserNotificationCenter.Current.SetNotificationCategories (new NSSet<UNNotificationCategory>(categories)); 
```

新たに作成するときに`UNNotificationAction`とは、ボタンに表示されるタイトルの一意の ID が割り当てられます。 既定では、アクションはするとして作成、バック グラウンド操作が、(たとえば設定して、フォア グラウンド処理)、アクションの動作を調整するオプションを指定することができます。

カテゴリに関連付けられる作成操作の必要があります。 新たに作成するときに`UNNotificationCategory`、一意の ID が割り当てられているが、実行できるアクションの一覧、カテゴリ内のアクションの詳細については、目的と動作を制御するカテゴリのいくつかのオプションの提供を目的とした Id の一覧です。

最後に、すべてのカテゴリを使用して、システムに登録されて、`SetNotificationCategories`メソッドです。

### <a name="presenting-custom-actions"></a>カスタム アクションを表示します。

カスタム アクションとカテゴリのセットを作成し、システムに登録されましたは、ローカルまたはリモートの通知から表示できます。

リモートの通知の設定、`category`上記で作成したカテゴリの 1 つに一致するリモート通知ペイロードにします。 例えば:

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

ローカル通知は、設定、`CategoryIdentifier`のプロパティ、`UNMutableNotificationContent`オブジェクト。 例えば:

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

ここでも、この ID は、上記で作成したが、カテゴリの 1 つと一致する必要があります。

### <a name="handling-dismiss-actions"></a>処理操作を消去します。

前述のよう、ユーザーが通知を閉じるときに破棄操作は、アプリに送信できます。 これは標準アクションではないため、オプションは、カテゴリが作成されるときに設定する必要があります。 例えば:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>操作の応答を処理します。

をカスタム アクションとカテゴリの上に作成されたユーザーが操作するときに、アプリは要求されたタスクを実行する必要があります。 これは、提供することで、`UNUserNotificationCenterDelegate`を実装して、`UserNotificationCenter`メソッドです。 例えば:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        ...

        #region Override Methods
        public override void DidReceiveNotificationResponse (UNUserNotificationCenter center, UNNotificationResponse response, Action completionHandler)
        {
            // Take action based on Action ID
            switch (response.ActionIdentifier) {
            case "reply":
                // Do something
                break;
            default:
                // Take action based on identifier
                if (response.IsDefaultAction) {
                    // Handle default action...
                } else if (response.IsDismissAction) {
                    // Handle dismiss action
                }
                break;
            }

            // Inform caller it has been handled
            completionHandler();
        }
        #endregion
    }
}
```

渡されたで`UNNotificationResponse`クラスには、`ActionIdentifier`できますいずれかのプロパティは既定のアクションまたは消去アクションにします。 使用して`response.Notification.Request.Identifier`すべてのカスタム動作をテストします。

`UserText`プロパティは、ユーザーのテキスト入力の値を保持します。 `Notification`プロパティは、トリガーを持つ要求を含む元の通知と通知のコンテンツを保持します。 アプリには、ローカルされたかどうか、またはトリガーの種類に基づいて、リモートの通知を決定できます。

## <a name="working-with-service-extensions"></a>サービス拡張を機能を使用

リモートの通知を使用するときに_サービス拡張_通知のペイロード内のエンド ツー エンドの暗号化を有効にする方法を提供します。 サービス拡張は、ユーザー インターフェイスでない拡張子 (iOS 10 で使用可能) の拡張、またはユーザーに表示する前に、通知の表示されているコンテンツを交換の主な目的でバック グラウンドで実行されています。 

[![](enhanced-user-notifications-images/extension01.png "サービスの拡張機能の概要")](enhanced-user-notifications-images/extension01.png#lightbox)

サービス拡張機能は迅速に実行するものし、システムで実行にかかる時間のわずかな時間がのみ与えられます。 イベントで、サービス拡張できない場合は、割り当てられた時間内のタスクが完了、フォールバック メソッドが呼び出されます。 フォールバックが失敗した場合、元の通知のコンテンツが表示されますをユーザーにします。

サービスの拡張機能のいくつかの潜在的な用途は次のとおりです。

- リモートの通知の内容のエンド ツー エンドの暗号化を提供します。
- それらの情報を追加するリモートの通知への添付ファイルを追加します。

### <a name="implementing-a-service-extension"></a>サービス拡張機能の実装

Xamarin.iOS アプリでは、サービス拡張機能を実装するには、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual studio for mac アプリのソリューションを開く
2. ソリューション名を右クリックし、**ソリューション パッド**選択**追加** > **新しいプロジェクトの追加**です。
3. 選択**iOS** > **拡張機能** > **通知サービスの拡張機能** をクリックし、**次**ボタン。 

    [![](enhanced-user-notifications-images/extension02.png "通知サービスの拡張機能を選択します。")](enhanced-user-notifications-images/extension02.png#lightbox)
4. 入力、**名前**拡張機能とクリック、 **[次へ]**ボタン。 

    [![](enhanced-user-notifications-images/extension03.png "拡張機能の名前を入力します。")](enhanced-user-notifications-images/extension03.png#lightbox)
5. 調整、**プロジェクト名**や**ソリューション名**要求されて、をクリックして、**作成**ボタン。 

    [![](enhanced-user-notifications-images/extension04.png "プロジェクトの名前、およびソリューション名を調整します。")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio でアプリのソリューションを開きます。
2. ソリューション名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいプロジェクトの追加**です。
3. 選択**iOS** > **拡張** > **通知サービスの拡張機能**: 

    [![](enhanced-user-notifications-images/extension01w.png "通知サービスの拡張機能を選択します。")](enhanced-user-notifications-images/extension01w.png#lightbox)
4. 入力してください、**名**をクリックして、拡張機能の**OK**ボタンをクリックします。

-----

> [!IMPORTANT]
> サービス拡張機能のバンドル Id がでメインのアプリのバンドル Id に一致する必要があります`.appnameserviceextension`末尾に追加します。 たとえば、メインのアプリのバンドル Id が含まれていた`com.xamarin.monkeynotify`、サービス拡張機能のバンドル Id を持つ必要があります`com.xamarin.monkeynotify.monkeynotifyserviceextension`です。 これは自動的に、ソリューションに拡張機能が追加されたときに設定する必要があります。 

必要な機能を提供するように変更する必要があります通知サービス拡張機能で 1 つのメイン クラスです。 例えば:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyChatServiceExtension
{
    [Register ("NotificationService")]
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Computed Properties
        public Action<UNNotificationContent> ContentHandler { get; set; }
        public UNMutableNotificationContent BestAttemptContent { get; set; }
        #endregion

        #region Constructors
        protected NotificationService (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            ContentHandler = contentHandler;
            BestAttemptContent = (UNMutableNotificationContent)request.Content.MutableCopy ();

            // Modify the notification content here...
            BestAttemptContent.Title = $"{BestAttemptContent.Title}[modified]";

            ContentHandler (BestAttemptContent);
        }

        public override void TimeWillExpire ()
        {
            // Called just before the extension will be terminated by the system.
            // Use this as an opportunity to deliver your "best attempt" at modified content, otherwise the original push payload will be used.

            ContentHandler (BestAttemptContent);
        }
        #endregion
    }
}
```

最初のメソッドで`DidReceiveNotificationRequest`、通知 Id だけでなく通知コンテンツ経由で渡される、`request`オブジェクト。 渡されたで`contentHandler`ユーザーに通知を表示するために呼び出す必要があります。

2 番目のメソッドでは、`TimeWillExpire`が時間が要求を処理するサービス拡張実行する直前に呼び出されます。 呼び出して、サービス拡張に失敗した場合、`contentHandler`をユーザーに、割り当てられた時間で、元のコンテンツが表示されます。

### <a name="triggering-a-service-extension"></a>サービスの拡張機能をトリガーします。

サービスの拡張が作成され、アプリの配信に、デバイスに送信されるリモートの通知のペイロードを変更することによりトリガーできます。 例えば:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

新しい`mutable-content`キーは、サービス拡張がリモートの通知の内容を更新するを起動する必要があることを指定します。 `encrypted-content`キーはサービス拡張機能は、ユーザーに提示する前に復号化できる暗号化されたデータを保持します。

サービスの拡張機能の次の例を参照してください。

```csharp
using UserNotification;

namespace myApp {
    public class NotificationService : UNNotificationServiceExtension {
    
        public override void DidReceiveNotificationRequest(UNNotificationRequest request, contentHandler) {
            // Decrypt payload
            var decryptedBody = Decrypt(Request.Content.UserInfo["encrypted-content"]);
            
            // Modify Notification body
            var newContent = new UNMutableNotificationContent();
            newContent.Body = decryptedBody;
            
            // Present to user
            contentHandler(newContent);
        }
        
        public override void TimeWillExpire() {
            // Handle out-of-time fallback event
            ...
        }
        
    }
}
```

このコードから暗号化されたコンテンツを復号化、`encrypted-content`キー、新たに作成`UNMutableNotificationContent`、設定、`Body`プロパティを使用して復号化されたコンテンツ、`contentHandler`をユーザーに通知を表示します。

## <a name="summary"></a>まとめ

この記事は、すべてのユーザー通知が iOS 10 で強化されている方法について説明しました。 これには、新しいユーザーの通知フレームワークと Xamarin.iOS アプリまたはアプリ拡張機能で使用する方法が表示されます。



## <a name="related-links"></a>関連リンク

- [iOS 10 サンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications Framework リファレンス](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [ローカルおよびリモートの通知プログラミング ガイド](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
