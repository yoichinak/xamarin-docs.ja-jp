---
title: Xamarin.iOS で強化されたユーザー通知
description: この記事では、iOS 10 で導入されたユーザー通知フレームワークについて説明します。 ローカル通知、リモート通知、通知の管理、通知アクション、および詳細を説明します。
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: d1b1a59b432315532844f8fca3b613ff3392a7b5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108250"
---
# <a name="enhanced-user-notifications-in-xamarinios"></a>Xamarin.iOS で強化されたユーザー通知

新しい ios 10 では、ユーザー通知の配信とローカルとリモート通知の処理のフレームワークを使用します。 このフレームワークを使用して、アプリまたはアプリ拡張機能をスケジュールできますローカル通知の配信の場所などの条件のセットまたは 1 日の時刻を指定することで。

## <a name="about-user-notifications"></a>ユーザーへの通知について

前述のように、配信とローカルとリモート通知の処理のため、新しいユーザー通知フレームワークができます。 このフレームワークを使用して、アプリまたはアプリ拡張機能をスケジュールできますローカル通知の配信の場所などの条件のセットまたは 1 日の時刻を指定することで。

さらに、アプリまたは拡張機能が表示される (および可能性のある変更できます) ローカルとリモートの両方の通知、ユーザーの iOS デバイスに配信されるようにします。

新しいユーザー通知の UI フレームワークは、アプリまたはアプリの拡張機能をユーザーに表示するときに、ローカルとリモートの両方の通知の外観をカスタマイズできます。

このフレームワークは、アプリはユーザーに通知を配信することができます、次の方法を提供します。

- **Visual アラート**- バナーとして画面の一番上から下、通知ロール場所。
- **サウンドと振動**-通知を関連付けることができます。
- **アプリ アイコンのバッジ取得**- アプリのアイコンが新しいコンテンツが利用可能な未読の電子メール メッセージの数などであることを示すバッジを表示します。

さらに、ユーザーの現在のコンテキストによっては、通知が表示されるさまざまな方法があります。

- デバイスがロックされている場合、通知が転がり落ちて、画面の上部からバナーとして。
- デバイスがロックされている場合は、ユーザーのロック画面に通知が表示されます。
- ユーザーは通知に失敗しました、通知センターを開き、でき、使用可能な待機通知が表示できます。

Xamarin.iOS アプリでは、2 種類のユーザー通知を送信することですがあります。

- **ローカル通知**-これらは、ユーザーのデバイスでローカルにインストールされているアプリによって送信されます。
- **リモート通知**-リモートから送信されるサーバーといずれかが、ユーザーに表示されるか、アプリのコンテンツのバック グラウンドの更新をトリガーします。

### <a name="about-local-notifications"></a>ローカル通知について

IOS アプリを送信できるローカル通知は、次の機能と属性があります。

- アプリでは、ローカル ユーザーのデバイス上で送信されます。 
- されるように構成できますベースのトリガーの時間や場所のいずれかを使用します。 
- アプリ ユーザーのデバイスで通知をスケジュールして、トリガー条件が満たされたときに表示されます。
- ユーザー通知と対話をアプリにコールバックが発生します。

ローカル通知の例をいくつか次のとおりです。

- 予定表の通知
- アラームのアラート
- 場所の認識トリガー

詳細については、Apple を参照してください[ローカルとリモート通知プログラミング ガイド](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)ドキュメント。

### <a name="about-remote-notifications"></a>リモート通知について

IOS アプリを送信できるリモート通知は、次の機能と属性があります。

- アプリでは、その通信相手とサーバー側コンポーネントがあります。
- Apple Push Notification Service (APNs) は、ユーザーのデバイスに開発者のクラウド ベースのサーバーからのリモート通知のベストエフォートの配信の送信に使用されます。
- アプリは、リモート通知を受信するとは、ユーザーに表示されます。
- ユーザー通知と対話をアプリにコールバックが発生します。

リモート通知の例をいくつか次のとおりです。

- ニュースのアラート
- スポーツの最新情報
- インスタント メッセージングのメッセージ

2 つの種類のリモート通知が iOS アプリに使用できます。

- **ユーザー向け**-デバイス上のユーザーに表示されます。
- **サイレント更新**-バック グラウンドでの iOS アプリの内容を更新するためのメカニズムを提供します。 サイレント更新プログラムが受信したときに、アプリは最新のコンテンツを削除サーバー プルに連絡できます。

詳細については、Apple を参照してください[ローカルとリモート通知プログラミング ガイド](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)ドキュメント。

### <a name="about-the-existing-notifications-api"></a>既存の通知 API について

IOS 10 では、前に、iOS アプリの使用`UIApplication`(かによって、時間や場所) の通知のトリガー方法をスケジュールして、システムに通知を登録します。

これには、開発者が既存の通知 API を使用する場合に発生可能性があるいくつかの問題があります。

- ローカルまたはリモート通知のコードの重複を招く可能性があるに必要なさまざまなコールバックが発生しました。
- アプリは、システムとそのスケジュールされている必要がある後に、通知の制御に限られていました。
- すべての Apple の既存のプラットフォーム間で異なるレベルのサポートがありました。

### <a name="about-the-new-user-notification-framework"></a>新しいユーザー通知フレームワークについて

Apple iOS 10 での既存に代わる新しいユーザー通知フレームワークを導入しましたが`UIApplication`上記で説明したメソッド。

次のユーザー通知フレームワークを提供します。

- 簡単にコードを移植する既存のフレームワークから以前の方法で機能パリティを含む使い慣れた API。
- 高度な通知をユーザーに送信できるコンテンツのオプションの拡張セットが含まれています。
- 同じコードとコールバックでは、ローカルとリモート通知の両方を処理できます。
- ユーザーが通知と対話するときに、アプリに送信されるコールバックを処理するプロセスを簡略化します。
- 保留中と配信の両方の通知を削除または通知を更新する機能などの管理が強化されています。
- 通知のアプリでプレゼンテーションを行う機能が追加されます。
- スケジュールを設定して、アプリ拡張機能内からの通知を処理する機能を追加します。
- 通知自体の新しい拡張機能ポイントを追加します。 

新しいユーザー通知フレームワークでは、統一された通知 API プラットフォームの複数 Apple がなどをサポートしていることを提供します。 

- **iOS** -完全に管理し、通知のスケジュール設定をサポートします。
- **tvOS** -ローカルおよびリモート通知のバッジ アプリ アイコンに機能を追加します。
- **watchOS** - ユーザーのペアになっている iOS デバイスから、Apple Watch への通知を転送する機能を追加し、watch アプリを使用するには、watch 自体で直接ローカル通知を実行できます。

詳細については、Apple を参照してください[UserNotifications フレームワーク参照](https://developer.apple.com/reference/usernotifications)と[UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)ドキュメント。

## <a name="preparing-for-notification-delivery"></a>通知の配信の準備

IOS の前にアプリは、システムにアプリを登録する必要があります、ユーザーに通知を送信でき、通知がユーザーに中断であるため、アプリにそれらを送信する前にアクセス許可を明示的に要求する必要があります。

これには、ユーザーがアプリを承認できます通知要求の 3 つのレベルがあります。

- バナーが表示されます。
- サウンドは警告。
- アプリ アイコンのバッジを取得します。

さらに、これらの承認レベルを要求し、ローカルとリモートの両方の通知を設定する必要があります。

次のコードを追加することで、アプリが起動するとすぐに通知のアクセス許可を要求する必要があります、`FinishedLaunching`のメソッド、`AppDelegate`目的の通知の種類を設定し、(`UNAuthorizationOptions`)。

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

さらに、ユーザー常に権限を変更通知を使用して、時間でアプリのため、**設定**デバイス上のアプリ。 次のコードを使用して通知を表示する前に、ユーザーの通知を要求された特権のアプリを確認します。

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
}); 
``` 

### <a name="configuring-the-remote-notifications-environment"></a>リモート通知環境の構成

新しい開発者を iOS 10 では、どのような環境のプッシュ通知が開発または実稼働環境のいずれか実行されている OS を伝える必要があります。 この情報を提供する発生する可能性が次のような通知 iTune App Store に送信されたときに拒否されているアプリ。

> 不足しているプッシュ通知の権利でアプリの Apple の Push Notification service、API が含まれていますが、`aps-environment`権利が、アプリの署名にありません。

必要な権利を提供するには、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. ダブルクリックして、`Entitlements.plist`ファイル、 **Solution Pad**編集用に開きます。
2. 切り替えて、**ソース**ビュー。 

    [![](enhanced-user-notifications-images/setup01.png "ソース ビュー")](enhanced-user-notifications-images/setup01.png#lightbox)
3. をクリックして、 **+** 新しいキーを追加するボタンをクリックします。
4. 入力`aps-environment`の**プロパティ**、ままにして、**型**として`String`いずれかの入力と`development`または`production`の**値**: 

    [![](enhanced-user-notifications-images/setup02.png "Aps 環境プロパティ")](enhanced-user-notifications-images/setup02.png#lightbox)
5. 変更内容をファイルに保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. ダブルクリックして、`Entitlements.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。
3. をクリックして、 **+** 新しいキーを追加するボタンをクリックします。
4. 入力`aps-environment`の**プロパティ**、ままにして、**型**として`String`いずれかの入力と`development`または`production`の**値**: 

    [![](enhanced-user-notifications-images/setup02w.png "Aps 環境プロパティ")](enhanced-user-notifications-images/setup02.png#lightbox)
5. 変更内容をファイルに保存します。

-----

### <a name="registering-for-remote-notifications"></a>リモート通知を登録します。

実行する必要がある場合は、アプリは送信して、リモート通知の受信、_トークン登録_既存を使用して`UIApplication`API。 この登録には、デバイスには、ライブ ネットワーク接続のアクセス、APNs、アプリに送信されるために必要なトークンを生成する必要があります。 アプリは、開発者のサーバー側のアプリをリモート通知に登録するには、このトークンを転送する必要があります。

[![](enhanced-user-notifications-images/token01.png "トークンの登録の概要")](enhanced-user-notifications-images/token01.png#lightbox)

必須の登録を初期化するために、次のコードを使用します。

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

開発者のサーバー側のアプリに送信されるトークンは、リモート通知を送信するときに、サーバーからの通知ペイロードを get の一部が APNs に送信するようにインクルードする必要があります。

[![](enhanced-user-notifications-images/token02.png "通知ペイロードの一部として含まれるトークン")](enhanced-user-notifications-images/token02.png#lightbox)

トークンを連結、通知とアプリを開くか、通知に応答するために使用するキーとして機能します。

詳細については、Apple を参照してください[ローカルとリモート通知プログラミング ガイド](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)ドキュメント。

## <a name="notification-delivery"></a>通知の配信

アプリが完全に登録されてから必要なアクセス許可が要求され、許可されている、ユーザーが、アプリは通知を送受信する準備が。 

### <a name="providing-notification-content"></a>通知の内容を提供します。

新しい ios 10 では、すべての通知を含む両方、**タイトル**と**サブタイトル**で常に表示される、**本文**通知の内容の。 また、新しいは追加する機能**メディア添付ファイル**通知の内容にします。

ローカル通知のコンテンツを作成するには、次のコードを使用します。

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

リモートの通知のプロセスは似ています。

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

アプリを作成された通知のコンテンツには、スケジュールを設定して、ユーザーに通知が表示 必要がある、*トリガー*します。 iOS 10 では、次の 4 つの異なるトリガーの種類を提供します。

- **プッシュ通知**- リモート通知だけを使用し、APNs 通知が送信されますが、デバイスで実行されているアプリをパッケージ化とがトリガーされます。
- **間隔**-間隔の開始と終了点がいくつかに将来の時刻からスケジュールをローカル通知を許可します。 たとえば、`var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`
- **日付をカレンダー** -特定の日付と時刻のスケジュールを設定するローカル通知を許可します。
- **場所ベース**-iOS デバイスの入力や特定の地理的な場所を離れずまたは特定の近くの Bluetooth ビーコンには、スケジュールをローカル通知を許可します。

アプリを呼び出す必要があるローカル通知の準備ができたら、`Add`のメソッド、`UNUserNotificationCenter`スケジュールをユーザーに表示するオブジェクト。 リモート通知は、サーバー側のアプリ通知ペイロードに送信、APNs では、ユーザーのデバイスにパケットを送信します。

すべての部分の融合ようサンプル ローカル通知になります。

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

新しい ios 10 では、アプリ通知を処理できる異なる方法で、フォア グラウンドであり、通知がトリガーされた場合にします。 提供することで、`UNUserNotificationCenterDelegate`を実装して、`UserNotificationCenter`メソッドでは、アプリ引き継ぐことができます、通知を表示するための責任です。 例えば:

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

内容を次のコードを記述して単に、`UNNotification`通知の標準のアラートを表示するアプリケーションの出力と、システムを確認します。 

アプリが、フォア グラウンドのときに、通知自体を表示しないシステムの既定値を使用して、以下を渡す場合`None`完了ハンドラーにします。 例:

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

このコードを開いて、`AppDelegate.cs`ファイルを編集し、変更、`FinishedLaunching`メソッドを次のように。

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

このコードは、カスタムのアタッチ`UNUserNotificationCenterDelegate`上から現在`UNUserNotificationCenter`がアクティブなアプリで通知を処理できるようにし、フォア グラウンドでします。

## <a name="notification-management"></a>通知の管理

新しい通知の管理を iOS 10 では、保留中と配信の両方の通知にアクセスでき、削除、更新、またはこれらの通知を昇格する機能を追加します。

通知の管理の重要な部分は、_要求識別子_が作成され、システムでスケジュールされたときに、通知に割り当てられました。 リモート通知は、これはを介して割り当てられている新しい`apps-collapse-id`HTTP 要求ヘッダー フィールド。

要求識別子を使用して、アプリで通知の管理を実行することを希望する通知を選択します。

### <a name="removing-notifications"></a>通知を削除します。

保留中の通知をシステムから削除するには、次のコードを使用します。

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

既に配信の通知を削除するには、次のコードを使用します。

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>既存の通知を更新しています

既存の通知を更新するには、変更 (新しいトリガー日時) など、目的のパラメーターで新しい通知を作成し、変更する必要があることを示す通知と同じ要求の識別子を持つシステムに追加します。 例:


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

既に配信された通知は、既存の通知は更新され、ユーザーが既に読み取られた場合、またはロック画面では、通知センターで、一覧の一番上に昇格が取得します。

## <a name="working-with-notification-actions"></a>通知アクションの使用

Ios 10 で、ユーザーに配信される通知は、静的でないと (カスタム動作に組み込み) からそれらにユーザーが操作できるいくつかの方法を提供します。

IOS アプリが応答するアクションの 3 つの種類があります。

- **既定のアクションが**-これは、ユーザーがアプリを開き、特定の通知の詳細を表示する通知をタップします。
- **カスタム アクション**-iOS 8 で追加されたこれらをすばやく通知から直接、アプリを起動することがなくカスタム タスクを実行するユーザーを指定するとします。 カスタマイズ可能なタイトルとボタンの一覧またはテキスト入力フィールドをバック グラウンド (アプリは指定した場合、要求を満たすために時間の短時間) またはフォア グラウンドで実行することができます (場所、アプリの起動時に fu フォア グラウンドでとしてを表示します。lfill 要求)。 カスタム アクションは、iOS および watchOS の両方で使用できます。
- **アクションを破棄**-ユーザーが特定の通知を閉じるときにこのアクションは、アプリに送信されます。

### <a name="creating-custom-actions"></a>カスタム アクションの作成

作成して、カスタム アクションをシステムに登録を次のコードを使用します。

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

新しいを作成するときに`UNNotificationAction`、割り当てられている一意の ID と、ボタンに表示されるタイトル。 既定でが (たとえば設定にフォア グラウンド操作)、アクションの動作を調整するオプションを指定することができますに操作をバック グラウンド操作として作成されます。

カテゴリと関連付ける作成操作の必要があります。 新しいを作成するときに`UNNotificationCategory`、一意の ID が割り当てられている、実行できるアクションの一覧、カテゴリ内のアクションの目的に関する詳細と、カテゴリの動作を制御するいくつかのオプションを提供する目的とした Id の一覧。

使用して、システムに登録されたすべてのカテゴリの最後に、`SetNotificationCategories`メソッド。

### <a name="presenting-custom-actions"></a>カスタム アクションを表示します。

一連のカスタム動作とカテゴリを作成し、システムに登録されていますは、ローカルまたはリモート通知から表示できます。

リモート通知は、設定、`category`上記で作成したカテゴリのいずれかに一致するリモートの通知ペイロードでします。 例えば:

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

ここでも、この ID は、上記で作成したカテゴリの 1 つと一致する必要があります。

### <a name="handling-dismiss-actions"></a>処理操作を無視します。

前述のように、ユーザーが通知を閉じるときに無視操作は、アプリに送信できます。 これは標準のアクションではないため、オプションは、カテゴリの作成時に設定する必要があります。 例えば:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>アクションの応答を処理します。

カスタム アクションとカテゴリの上に作成された、ユーザーが、アプリは要求されたタスクを実行するために必要があります。 これは、提供することで、`UNUserNotificationCenterDelegate`を実装して、`UserNotificationCenter`メソッド。 例えば:

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

渡されたで`UNNotificationResponse`クラスには、`ActionIdentifier`できますいずれかのプロパティが既定のアクションまたはアクションを無視します。 使用`response.Notification.Request.Identifier`カスタム動作をテストします。

`UserText`プロパティは、ユーザーのテキスト入力の値を保持します。 `Notification`プロパティは、トリガーで要求を含む元の通知および通知のコンテンツを保持します。 アプリは、ローカルされたかどうか、またはリモート通知がトリガーの種類に基づいて決定できます。

> [!NOTE]
> iOS 12 を使うと、実行時にアクション ボタンを変更するカスタム通知 UI。 詳細についてを参照してください、[動的通知アクション ボタン](~/ios/platform/introduction-to-ios12/notifications/dynamic-actions.md)ドキュメント。

## <a name="working-with-service-extensions"></a>サービスの拡張機能の使用

リモートの通知を使用する場合_サービス拡張_内部通知ペイロードでエンド ツー エンドの暗号化を有効にする方法を提供します。 サービスの拡張機能は、ユーザー インターフェイスでない延長 (iOS 10 で使用可能) を追加することや、ユーザーに提示される前に表示される通知のコンテンツを置き換えるの主な目的でバック グラウンドで実行できます。 

[![](enhanced-user-notifications-images/extension01.png "サービスの拡張機能の概要")](enhanced-user-notifications-images/extension01.png#lightbox)

サービスの拡張機能では、高速に実行するためのものし、システムによって実行時間の短い時間がのみ与えられます。 サービス拡張機能は、割り当てられた時間でタスクが完了する失敗すると、あるフォールバック メソッドが呼び出されます。 フォールバックが失敗した場合、元 Notification Content が表示されます、ユーザーにします。

サービスの拡張機能のいくつかの潜在的な用途は次のとおりです。

- リモート通知の内容のエンド ツー エンドの暗号化を提供します。
- それらを強化するリモート通知への添付ファイルを追加します。

### <a name="implementing-a-service-extension"></a>サービスの拡張機能の実装

Xamarin.iOS アプリでサービスの拡張機能を実装するには、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Visual studio for mac アプリのソリューションを開きます
2. ソリューション名を右クリックし、 **Solution Pad**選択**追加** > **新しいプロジェクトの追加**します。
3. 選択**iOS** > **拡張機能** > **通知サービスの拡張機能** をクリックし、**次**ボタン。 

    [![](enhanced-user-notifications-images/extension02.png "通知サービスの拡張機能を選択します。")](enhanced-user-notifications-images/extension02.png#lightbox)
4. 入力、**名前**拡張機能をクリックして、**次**ボタン。 

    [![](enhanced-user-notifications-images/extension03.png "拡張機能の名前を入力します")](enhanced-user-notifications-images/extension03.png#lightbox)
5. 調整、**プロジェクト名**や**ソリューション名**ために必要なクリックすると、**作成**ボタン。 

    [![](enhanced-user-notifications-images/extension04.png "プロジェクト名またはソリューション名を調整します。")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio で、アプリのソリューションを開きます。
2. ソリューション名を右クリックし、**ソリューション エクスプ ローラー**選択**追加 > 新しいプロジェクト.**.
3. 選択**Visual C# > iOS 拡張機能 > Notification Service 拡張機能**:

    [![](enhanced-user-notifications-images/extension01.w157-sml.png "通知サービスの拡張機能を選択します。")](enhanced-user-notifications-images/extension01.w157.png#lightbox)
4. 入力、**名前**拡張機能をクリックして、 **OK**ボタン。

-----

> [!IMPORTANT]
> サービス拡張機能のバンドル識別子がでメイン アプリケーションのバンドル識別子と一致する必要があります`.appnameserviceextension`末尾に追加されます。 たとえば、メイン アプリケーションがある、バンドル識別子が`com.xamarin.monkeynotify`、サービス拡張機能のバンドル識別子が必要`com.xamarin.monkeynotify.monkeynotifyserviceextension`します。 これは自動的に、ソリューションに、拡張機能が追加されたときに設定する必要があります。 

必要な機能を提供するように変更する必要のある Notification Service 拡張機能では、1 つのメイン クラスです。 例えば:

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

最初のメソッドで`DidReceiveNotificationRequest`、通知 Id だけでなく Notification Content 経由で渡される、`request`オブジェクト。 渡されたで`contentHandler`ユーザーに通知を表示するために呼び出す必要があります。

2 番目のメソッドでは、`TimeWillExpire`が時間が要求を処理するサービスの拡張機能実行する直前に呼び出されます。 サービス拡張機能は、呼び出しが失敗した場合、`contentHandler`をユーザーに、割り当てられた時間内、元のコンテンツが表示されます。

### <a name="triggering-a-service-extension"></a>サービスの拡張機能をトリガーします。

作成され、アプリで提供されるサービスの拡張子を持つデバイスに送信されるリモート通知ペイロードを変更することでトリガーできます。 例えば:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

新しい`mutable-content`キーは、サービス拡張機能が起動するようにリモート通知の内容を更新する必要があることを指定します。 `encrypted-content`キーはサービス拡張機能は、ユーザーに提示する前に復号化できる暗号化されたデータを保持します。

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

このコードから暗号化されたコンテンツを復号化、`encrypted-content`キー、新たに作成`UNMutableNotificationContent`、設定、`Body`プロパティを使用して復号化されたコンテンツ、`contentHandler`ユーザーに通知を表示します。

## <a name="summary"></a>まとめ

この記事では、すべてのユーザー通知が iOS 10 で強化されましたする方法について説明しました。 新しいユーザー通知フレームワークと Xamarin.iOS アプリまたはアプリ拡張機能内で使用する方法が表示されます。



## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications フレームワーク参照](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [ローカルとリモート通知プログラミング ガイド](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
