---
title: Xamarin でのユーザー通知の強化
description: この記事では、iOS 10 で導入されたユーザー通知フレームワークについて説明します。 ローカル通知、リモート通知、通知管理、通知アクションなどについて説明します。
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/02/2017
ms.openlocfilehash: d4fe04412eb4fb456bc49d71c1e5fe87df5f9e76
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939642"
---
# <a name="enhanced-user-notifications-in-xamarinios"></a>Xamarin でのユーザー通知の強化

IOS 10 の新機能であるユーザー通知フレームワークを使用すると、ローカルおよびリモートの通知を配信および処理することができます。 このフレームワークを使用すると、アプリまたはアプリ拡張機能は、場所や時間などの条件のセットを指定することによって、ローカル通知の配信をスケジュールすることができます。

## <a name="about-user-notifications"></a>ユーザーへの通知について

前述のように、新しいユーザー通知フレームワークでは、ローカルおよびリモートの通知の配信と処理を行うことができます。 このフレームワークを使用すると、アプリまたはアプリ拡張機能は、場所や時間などの条件のセットを指定することによって、ローカル通知の配信をスケジュールすることができます。

また、アプリまたは拡張機能は、ローカルとリモートの両方の通知をユーザーの iOS デバイスに配信するときに、それらを受信 (および変更する可能性があります) することができます。

新しいユーザー通知 UI フレームワークでは、アプリまたはアプリの拡張機能を使用して、ユーザーに表示されるときに、ローカルとリモートの両方の通知の外観をカスタマイズできます。

このフレームワークには、アプリがユーザーに通知を配信するための次の方法が用意されています。

- **視覚的な警告**-通知が画面の上部からバナーとしてロールダウンされます。
- **Sound と Vibrations** -通知に関連付けることができます。
- **アプリアイコン**のバッジ: アプリのアイコンに、未読の電子メールメッセージの数など、新しいコンテンツが利用可能であることを示すバッジが表示されます。

さらに、ユーザーの現在のコンテキストによっては、通知が表示されるさまざまな方法があります。

- デバイスのロックが解除されている場合、通知は画面の上部からバナーとしてロールダウンされます。
- デバイスがロックされている場合は、ユーザーのロック画面に通知が表示されます。
- ユーザーが通知を失った場合、通知センターを開いて、利用可能な待機中の通知を表示できます。

Xamarin iOS アプリには、次の2種類のユーザー通知を送信できます。

- **ローカル通知**-これらは、ユーザーのデバイスにローカルにインストールされたアプリによって送信されます。
- **リモート通知**-リモートサーバーから送信され、ユーザーに表示されるか、アプリのコンテンツのバックグラウンド更新をトリガーします。

### <a name="about-local-notifications"></a>ローカル通知について

IOS アプリが送信できるローカル通知には、次の機能と属性があります。

- これらは、ユーザーのデバイスのローカルにあるアプリによって送信されます。 
- これらは、時刻または場所ベースのトリガーを使用するように構成できます。 
- アプリはユーザーのデバイスで通知をスケジュールし、トリガー条件が満たされたときに表示されます。
- ユーザーが通知を操作すると、アプリケーションはコールバックを受け取ります。

ローカル通知の例を次に示します。

- 予定表のアラート
- リマインダーアラート
- 場所を認識するトリガー

詳細については、Apple の[ローカルおよびリモート通知のプログラミングガイド](https://developer.apple.com/documentation/usernotifications)に関するドキュメントを参照してください。

### <a name="about-remote-notifications"></a>リモート通知について

IOS アプリが送信できるリモート通知には、次の機能と属性があります。

- アプリには、通信するサーバー側のコンポーネントがあります。
- Apple Push Notification Service (APNs) は、開発者のクラウドベースのサーバーからユーザーのデバイスにリモート通知のベストエフォート配信を送信するために使用されます。
- アプリがリモート通知を受け取ると、ユーザーに表示されます。
- ユーザーが通知を操作すると、アプリケーションはコールバックを受け取ります。

リモート通知の例を次に示します。

- ニュースのアラート
- スポーツの更新
- インスタントメッセージングメッセージ

IOS アプリで使用可能なリモート通知には、次の2種類があります。

- **ユーザー**への接続-デバイス上のユーザーに表示されます。
- **サイレント更新**-バックグラウンドで iOS アプリのコンテンツを更新するためのメカニズムが用意されています。 サイレント更新を受信すると、アプリはリモートサーバーに接続して最新のコンテンツを取得することができます。

詳細については、Apple の[ローカルおよびリモート通知のプログラミングガイド](https://developer.apple.com/documentation/usernotifications)に関するドキュメントを参照してください。

### <a name="about-the-existing-notifications-api"></a>既存の通知 API について

Ios 10 より前では、iOS アプリはを使用して `UIApplication` 通知をシステムに登録し、通知のトリガー方法 (時刻または場所) をスケジュールします。

既存の notification API を使用しているときに開発者が遭遇する可能性がある問題がいくつかあります。

- ローカルまたはリモートの通知には、コードの重複を招く可能性があるさまざまなコールバックが必要でした。
- アプリは、システムでスケジュールされた後、通知の制御を制限されていました。
- Apple の既存のすべてのプラットフォームにおいて、さまざまなレベルのサポートがありました。

### <a name="about-the-new-user-notification-framework"></a>新しいユーザー通知フレームワークについて

IOS 10 では、Apple は、前述の既存の方法を置き換える新しいユーザー通知フレームワークを導入しました `UIApplication` 。

ユーザー通知フレームワークには、次のものがあります。

- 以前の方法と同等の機能を備えた使い慣れた API により、既存のフレームワークからコードを簡単に移植できるようになります。
- には、高度な通知をユーザーに送信できるようにする、拡張されたコンテンツオプションのセットが含まれています。
- ローカルとリモートの両方の通知は、同じコードおよびコールバックによって処理できます。
- ユーザーが通知を操作するときにアプリに送信されるコールバックを処理するプロセスを簡略化します。
- 通知を削除または更新する機能など、保留中および配信済みの通知の両方の管理機能が強化されました。
- 通知のアプリ内プレゼンテーションを実行する機能を追加します。
- アプリ拡張機能内から通知をスケジュールおよび処理する機能を追加します。
- 通知自体に新しい拡張ポイントを追加します。 

新しいユーザー通知フレームワークは、Apple がサポートする次のような複数のプラットフォームにわたって統合された notification API を提供します。 

- **iOS** -通知の管理とスケジュール設定を完全にサポートします。
- **tvOS** -ローカル通知とリモート通知のアプリアイコンをバッジする機能を追加します。
- **watchOS** -ユーザーのペアリングされた iOS デバイスから Apple Watch に通知を転送する機能を追加します。 watch アプリは、ウォッチ自体でローカル通知を直接行うことができます。

詳細については、Apple の[Usernotifications フレームワークリファレンス](https://developer.apple.com/reference/usernotifications)と[UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)のドキュメントを参照してください。

## <a name="preparing-for-notification-delivery"></a>通知配信の準備

IOS アプリからユーザーに通知を送信する前に、アプリケーションをシステムに登録する必要があります。また、通知はユーザーに中断されるため、アプリは送信前にアクセス許可を明示的に要求する必要があります。

ユーザーがアプリに対して承認できる通知要求には、次の3つのレベルがあります。

- バナーが表示されます。
- サウンドアラート。
- アプリアイコンをバッジします。

また、これらの承認レベルは、ローカルとリモートの両方の通知を要求して設定する必要があります。

通知アクセス許可は、アプリが起動するとすぐに、のメソッドに次のコードを追加し、 `FinishedLaunching` `AppDelegate` 必要な通知の種類 () を設定することによって要求する必要があり `UNAuthorizationOptions` ます。

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

さらに、ユーザーは、デバイスの**設定**アプリを使用していつでも、いつでもアプリの通知特権をいつでも変更できます。 アプリは、次のコードを使用して通知を提示する前に、ユーザーの要求された通知特権を確認する必要があります。

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
});    
``` 

### <a name="configuring-the-remote-notifications-environment"></a>リモート通知環境の構成

IOS 10 の新機能として、開発者は、どの環境プッシュ通知がどの環境で実行されているかを、開発と運用のどちらとして OS に通知する必要があります この情報を指定しなかった場合、次のような通知を含む iTune App ストアに送信されると、アプリが拒否される可能性があります。

> プッシュ通知の権利がありません-アプリに Apple のプッシュ通知サービス用の API が含まれていますが、 `aps-environment` アプリの署名に権利がありません。

必要な権利を提供するには、次の手順を実行します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. Solution Pad 内のファイルをダブルクリックし `Entitlements.plist` て、編集用に**Solution Pad**開きます。
2. **ソース**ビューに切り替えます。 

    [![ソースビュー](enhanced-user-notifications-images/setup01.png)](enhanced-user-notifications-images/setup01.png#lightbox)
3. **+** 新しいキーを追加するには、このボタンをクリックします。
4. プロパティとして「」と入力し、[型] をそのままにして、値として「 `aps-environment` **Property** **Type** `String` 」または「」を入力し `development` `production` ます。 **Value** 

    [![Aps-environment プロパティ](enhanced-user-notifications-images/setup02.png)](enhanced-user-notifications-images/setup02.png#lightbox)
5. 変更をファイルに保存します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. ソリューションエクスプローラー内のファイルをダブルクリックし `Entitlements.plist` て、編集用に**Solution Explorer**開きます。
2. **+** 新しいキーを追加するには、このボタンをクリックします。
3. プロパティとして「」と入力し、[型] をそのままにして、値として「 `aps-environment` **Property** **Type** `String` 」または「」を入力し `development` `production` ます。 **Value** 

    [![Aps-environment プロパティ](enhanced-user-notifications-images/setup02w.png)](enhanced-user-notifications-images/setup02.png#lightbox)
4. 変更をファイルに保存します。

-----

### <a name="registering-for-remote-notifications"></a>リモート通知の登録

アプリがリモート通知を送受信する場合でも、既存の API を使用して_トークン登録_を行う必要があり `UIApplication` ます。 この登録を行うには、デバイスが APNs にライブネットワーク接続アクセスする必要があります。これにより、アプリに送信される必要なトークンが生成されます。 次に、アプリはこのトークンを開発者のサーバー側アプリに転送して、リモート通知を登録する必要があります。

[![トークン登録の概要](enhanced-user-notifications-images/token01.png)](enhanced-user-notifications-images/token01.png#lightbox)

次のコードを使用して、必要な登録を初期化します。

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

開発者のサーバー側アプリに送信されるトークンは、リモート通知を送信するときにサーバーから APNs に送信される通知ペイロードの一部として含める必要があります。

[![通知ペイロードの一部として含まれるトークン](enhanced-user-notifications-images/token02.png)](enhanced-user-notifications-images/token02.png#lightbox)

このトークンは、通知と、通知を開いたり応答したりするために使用されるアプリを相互に結び付けるキーとして機能します。

詳細については、Apple の[ローカルおよびリモート通知のプログラミングガイド](https://developer.apple.com/documentation/usernotifications)に関するドキュメントを参照してください。

## <a name="notification-delivery"></a>通知配信

アプリが完全に登録され、ユーザーによってから要求される必要なアクセス許可が付与されると、アプリは通知を送受信する準備ができました。 

### <a name="providing-notification-content"></a>通知コンテンツの提供

IOS 10 を初めて使用する場合、すべての通知には、通知コンテンツの**本文**と共に常に表示される**タイトル**と**サブタイトル**の両方が含まれます。 また、新しいは、通知コンテンツに**メディア添付ファイル**を追加する機能です。

ローカル通知の内容を作成するには、次のコードを使用します。

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

リモート通知の場合、プロセスは次のようになります。

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

### <a name="scheduling-when-a-notification-is-sent"></a>通知が送信されたときのスケジュール

通知の内容を作成したら、*トリガー*を設定してユーザーに通知を表示するタイミングをアプリでスケジュールする必要があります。 iOS 10 には、次の4種類のトリガーがあります。

- **プッシュ通知**-リモート通知でのみ使用され、APNs がデバイスで実行されているアプリに通知パッケージを送信するときにトリガーされます。
- [**時間間隔**]-現在から開始し、将来のある時点を終了するまでの時間間隔からローカル通知をスケジュールできます。 たとえば、`var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);` のように指定します。
- **カレンダー日付**-ローカル通知を特定の日時にスケジュールすることを許可します。
- **場所ベース**-iOS デバイスが特定の地理的な場所に出入りする場合、または Bluetooth ビーコンに隣接している場合に、ローカル通知をスケジュールすることを許可します。

ローカル通知の準備ができたら、アプリケーションはオブジェクトのメソッドを呼び出して、 `Add` `UNUserNotificationCenter` ユーザーに表示をスケジュールする必要があります。 リモート通知の場合、サーバー側アプリは APNs に通知ペイロードを送信します。これにより、パケットがユーザーのデバイスに送信されます。

すべての要素をまとめると、ローカル通知のサンプルは次のようになります。

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

## <a name="handling-foreground-app-notifications"></a>フォアグラウンドアプリの通知の処理

IOS 10 を初めて使用する場合、アプリは、フォアグラウンドにいるときに通知を別の方法で処理でき、通知がトリガーされます。 を提供 `UNUserNotificationCenterDelegate` し、メソッドを実装することで `WillPresentNotification` 、アプリは通知を表示する責任を負うことができます。 次に例を示します。

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

このコードでは、の内容を `UNNotification` アプリケーション出力に書き込み、通知の標準アラートを表示するようシステムに要求します。 

アプリがフォアグラウンドにあったときに通知を表示し、システムの既定値を使用しないようにする場合は、 `None` 完了ハンドラーにを渡します。 例:

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

このコードを配置したら、 `AppDelegate.cs` ファイルを編集用に開き、 `FinishedLaunching` メソッドを次のように変更します。

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

このコードでは、上記のカスタムを現在のにアタッチしてい `UNUserNotificationCenterDelegate` `UNUserNotificationCenter` ます。これにより、アプリは、アクティブなときとフォアグラウンドで通知を処理できるようになります。

## <a name="notification-management"></a>通知管理

IOS 10 の新機能である通知管理を使用すると、保留中と配信済みの両方の通知にアクセスでき、これらの通知を削除、更新、または昇格する機能が追加されます。

通知管理の重要な部分は、システムで作成およびスケジュールされたときに通知に割り当てられた_要求識別子_です。 リモート通知の場合は、HTTP 要求ヘッダーの新しいフィールドを通じて割り当てられ `apps-collapse-id` ます。

要求 Id は、アプリで通知管理を実行する通知を選択するために使用されます。

### <a name="removing-notifications"></a>通知の削除

保留中の通知をシステムから削除するには、次のコードを使用します。

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

既に配信された通知を削除するには、次のコードを使用します。

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>既存の通知を更新する

既存の通知を更新するには、必要なパラメーターを変更して新しい通知 (新しいトリガー時間など) を作成し、変更する必要のある通知と同じ要求識別子を使用してシステムに追加するだけです。 例:

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

既に配信済みの通知については、既存の通知が更新され、ホーム画面とロック画面、および通知センターの一覧の先頭に昇格されます (ユーザーによって既に読み取られている場合)。

## <a name="working-with-notification-actions"></a>通知アクションの操作

IOS 10 では、ユーザーに配信される通知は静的なものではなく、ユーザーがこれらの通知を操作するためのいくつかの方法を提供しています (組み込みのアクションからカスタムアクションへ)。

IOS アプリが応答できるアクションには、次の3種類があります。

- **既定のアクション**-ユーザーが通知をタップしてアプリを開き、指定された通知の詳細を表示します。
- **カスタムアクション**-これらは iOS 8 で追加されたものであり、ユーザーがアプリを起動しなくても、通知から直接カスタムタスクを実行することができます。 これらは、カスタマイズ可能なタイトルを持つボタンのリストとして、またはバックグラウンドで (アプリが要求を処理するための短い時間が与えられている) バックグラウンドで実行できるテキスト入力フィールドとして、またはフォアグラウンド (アプリが要求を処理するためにフォアグラウンドで起動される) のいずれかとして表示できます。 カスタムアクションは、iOS と watchOS の両方で使用できます。
- **アクションを無視**する-このアクションは、ユーザーが特定の通知を終了したときにアプリに送信されます。

### <a name="creating-custom-actions"></a>カスタムアクションの作成

システムでカスタムアクションを作成して登録するには、次のコードを使用します。

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

新しいを作成する場合 `UNNotificationAction` は、一意の ID と、ボタンに表示されるタイトルが割り当てられます。 既定では、アクションはバックグラウンドアクションとして作成されますが、アクションの動作を調整するオプションを指定できます (たとえば、フォアグラウンドアクションとして設定します)。

作成された各アクションは、カテゴリに関連付けられている必要があります。 新しいを作成するときに `UNNotificationCategory` は、一意の id、実行できるアクションのリスト、カテゴリ内のアクションの目的に関する詳細情報を提供するためのインテント id の一覧、およびカテゴリの動作を制御するためのオプションが割り当てられます。

最後に、メソッドを使用して、すべてのカテゴリがシステムに登録され `SetNotificationCategories` ます。

### <a name="presenting-custom-actions"></a>カスタムアクションの表示

一連のカスタムアクションとカテゴリが作成され、システムに登録されると、ローカルまたはリモートの通知から表示されるようになります。

リモート通知の場合は、 `category` 上で作成したいずれかのカテゴリに一致するリモート通知ペイロードでを設定します。 次に例を示します。

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

ローカル通知の場合は、 `CategoryIdentifier` オブジェクトのプロパティを設定し `UNMutableNotificationContent` ます。 次に例を示します。

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

ここでも、この ID は、上記で作成したいずれかのカテゴリと一致する必要があります。

### <a name="handling-dismiss-actions"></a>破棄操作の処理

前述のように、ユーザーが通知を終了したときに、無視アクションをアプリに送信できます。 これは標準の操作ではないため、カテゴリの作成時にオプションを設定する必要があります。 次に例を示します。

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>アクションの応答の処理

ユーザーが、上で作成されたカスタムアクションおよびカテゴリを操作する場合、アプリは要求されたタスクを満たす必要があります。 これを行うには、を提供 `UNUserNotificationCenterDelegate` し、メソッドを実装し `UserNotificationCenter` ます。 次に例を示します。

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

渡されたクラスには、 `UNNotificationResponse` `ActionIdentifier` 既定のアクションまたは無視アクションのいずれかのプロパティがあります。 `response.Notification.Request.Identifier`カスタムアクションをテストするには、を使用します。

プロパティは、 `UserText` 任意のユーザーテキスト入力の値を保持します。 プロパティは、 `Notification` トリガーと通知の内容を含む要求を含む送信元の通知を保持します。 アプリは、トリガーの種類に基づいて、ローカルまたはリモートの通知であるかどうかを判断できます。

> [!NOTE]
> iOS 12 を使用すると、カスタム通知 UI で実行時にアクションボタンを変更することができます。 詳細については、[動的通知アクションボタン](~/ios/platform/introduction-to-ios12/notifications/dynamic-actions.md)のドキュメントを参照してください。

## <a name="working-with-service-extensions"></a>サービス拡張機能の使用

リモート通知を使用する場合、_サービス拡張_は、通知ペイロード内でエンドツーエンドの暗号化を有効にする方法を提供します。 サービス拡張は、ユーザーに表示される前に通知の表示内容を補強または置換するための主な目的として、バックグラウンドで実行される非ユーザーインターフェイス拡張 (iOS 10 で利用可能) です。 

[![サービス拡張機能の概要](enhanced-user-notifications-images/extension01.png)](enhanced-user-notifications-images/extension01.png#lightbox)

サービス拡張は迅速に実行することを意図しており、システムによって実行されるのに短時間しか与えられません。 サービス拡張が、割り当てられた時間内にタスクの完了に失敗した場合、フォールバックメソッドが呼び出されます。 フォールバックに失敗した場合、元の通知コンテンツがユーザーに表示されます。

サービス拡張には、次のような用途が考えられます。

- リモート通知コンテンツのエンドツーエンドの暗号化を提供します。
- 添付ファイルをリモート通知に追加して、それらを強化します。

### <a name="implementing-a-service-extension"></a>サービス拡張機能の実装

Xamarin iOS アプリでサービス拡張機能を実装するには、次の手順を実行します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac でアプリのソリューションを開きます。
2. **Solution Pad**でソリューション名を右クリックし、[**追加**] [  >  **新しいプロジェクト**] の順に選択します。
3. [ **IOS**  >  **extensions**  >  **Notification Service の拡張機能**] を選択し、[**次へ**] ボタンをクリックします。 

    [![Notification Service 拡張機能の選択](enhanced-user-notifications-images/extension02.png)](enhanced-user-notifications-images/extension02.png#lightbox)
4. 拡張機能の**名前**を入力し、[**次へ**] ボタンをクリックします。 

    [![拡張機能の名前を入力してください](enhanced-user-notifications-images/extension03.png)](enhanced-user-notifications-images/extension03.png#lightbox)
5. 必要に応じて**プロジェクト名**または**ソリューション名**を調整し、[**作成**] ボタンをクリックします。 

    [![プロジェクト名またはソリューション名の調整](enhanced-user-notifications-images/extension04.png)](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Visual Studio でアプリのソリューションを開きます。
2. **ソリューションエクスプローラー**でソリューション名を右クリックし、[ **> 新しいプロジェクトの追加**] を選択します。
3. [ **Visual C# > IOS 拡張機能 > Notification Service 拡張機能**] を選択します。

    [![Notification Service 拡張機能の選択](enhanced-user-notifications-images/extension01.w157-sml.png)](enhanced-user-notifications-images/extension01.w157.png#lightbox)
4. 拡張機能の**名前**を入力し、[ **OK** ] をクリックします。

-----

> [!IMPORTANT]
> サービス拡張機能のバンドル識別子は、メインアプリのバンドル識別子を末尾に追加したものと一致している必要があり `.appnameserviceextension` ます。 たとえば、メインアプリのバンドル識別子がの場合、 `com.xamarin.monkeynotify` サービス拡張はのバンドル識別子を持つ必要があり `com.xamarin.monkeynotify.monkeynotifyserviceextension` ます。 これは、拡張機能をソリューションに追加するときに自動的に設定されます。 

Notification Service 拡張機能には、必要な機能を提供するために変更する必要があるメインクラスが1つあります。 次に例を示します。

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

最初のメソッドであるには、通知の Id だけでなく、通知の `DidReceiveNotificationRequest` 内容がオブジェクトを介して渡され `request` ます。 ユーザーに通知を表示するには、渡されたを `contentHandler` 呼び出す必要があります。

2番目のメソッドであるは、 `TimeWillExpire` 要求を処理するためにサービス拡張に対して実行される直前に呼び出されます。 割り当てられた時間内にサービス拡張がを呼び出すことができない場合は、 `contentHandler` 元のコンテンツがユーザーに表示されます。

### <a name="triggering-a-service-extension"></a>サービス拡張機能のトリガー

サービス拡張を作成してアプリと共に配信することで、デバイスに送信されるリモート通知ペイロードを変更することによって、サービス拡張をトリガーできます。 次に例を示します。

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

新しいキーは、 `mutable-content` リモート通知コンテンツを更新するためにサービス拡張を起動する必要があることを指定します。 キーは、 `encrypted-content` ユーザーに提示する前にサービス拡張が復号化できる暗号化されたデータを保持します。

次のサービス拡張機能の例を見てみましょう。

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

このコードは、暗号化されたコンテンツをキーから復号化し、 `encrypted-content` 新しいを作成し、プロパティを復号化さ `UNMutableNotificationContent` `Body` れたコンテンツに設定し、を使用して `contentHandler` 通知をユーザーに提示します。

## <a name="summary"></a>まとめ

この記事では、iOS 10 によってユーザー通知が強化されたすべての方法について説明しました。 この例では、新しいユーザー通知フレームワークと、Xamarin iOS アプリまたはアプリ拡張機能での使用方法について説明しています。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [UserNotifications フレームワークリファレンス](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [ローカルおよびリモートの通知プログラミングガイド](https://developer.apple.com/documentation/usernotifications)
