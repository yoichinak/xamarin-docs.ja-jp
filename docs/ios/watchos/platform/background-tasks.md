---
title: "バックグラウンド タスク"
description: "Watch アプリは、常に最新のデータとドッキングのスナップショットに持つようにするためには、新しいバック グラウンド タスク watchOS 3 を使用します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/13/2017
ms.openlocfilehash: 8fd2b5069e175a68ff7609e75775db1929507582
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="background-tasks"></a>バックグラウンド タスク

_Watch アプリは、常に最新のデータとドッキングのスナップショットに持つようにするためには、新しいバック グラウンド タスク watchOS 3 を使用します。_

WatchOS 3 には、こと watch アプリがその情報を最新に保つ 3 つの主な方法があります。 

- いくつかの新しいバック グラウンド タスクのいずれかを使用します。 
- その複雑さの一部のいずれかを (更新する余分な時間に与える) ウォッチの文字盤で発生しています。 
- 新しいドッキング ステーションをアプリにユーザー暗証番号 (pin) を持つ (ここで、メモリに保持され、頻繁に更新) します。 

## <a name="keeping-an-app-up-to-date"></a>アプリの最新の状態を維持します。

すべての現在および更新後に、開発者が watchOS アプリのデータとユーザー インターフェイスを保つことができる方法について説明する、前にこのセクションでを見て使用パターン、ユーザー、iPhone に基づく 1 日を通して、Apple Watch 間の移動が方法の標準的なセット 時刻、およびアクティビティ現在行って (駆動) などです。

次の例を参照してください。

[![](background-tasks-images/update00.png "ユーザー、iPhone、日中、Apple Watch 間の移動が方法")](background-tasks-images/update00.png#lightbox)

1. 朝、待っている間はコーヒーの行には、ユーザーは、数分、iPhone の現在のニュースを参照します。
2. コーヒー ショップを終了する前に、天気、ウォッチの文字盤でコンプリケーションと簡単にチェックします。
3. 昼食前に、を使用してマップ アプリ iPhone の近くにあるレストランを検索して予約をクライアントを満たすために予約。
4. Restaurant に出張中、Apple Watch でクイック グランスを使用して、通知を受け取りますが、その昼食の予定が遅延実行されているが認識します。
5. 夜間に、マップ アプリを使用して、iPhone でホームを促進する前にトラフィックを確認します。
6. 途中で一部 milk を取得するように求める、Apple Watch での iMessage 通知を受け取ったホーム、コマンドレットを使用して、クイック応答機能"OK"という応答を送信します。

「概要」ため (3 秒未満) を使用して、Apple Watch アプリが通常にユーザーを検索する方法の性質がされていないアプリが必要な情報をフェッチおよびユーザーに表示する前に、UI を更新するための十分な時間。

新しい Api Apple が含まれている watchOS 3 で、アプリをスケジュールできます、_バック グラウンドで更新_ユーザーを要求する前に準備が必要な情報があるとします。 上記で説明した気象コンプリケーションの例を実行します。

[![](background-tasks-images/update01.png "気象コンプリケーションの例")](background-tasks-images/update01.png#lightbox)

1. ウェイク アップ、システムによって、特定の時点でアプリのスケジュール。 
2. アプリでは、更新プログラムを生成する必要があります、情報をフェッチします。
3. アプリは、新しいデータを反映するように、ユーザー インターフェイスを再生成します。
4. ユーザーは、アプリのコンプリケーションの概要、ときに、最新の情報を更新を待たずにユーザーがいない状態がします。

上で説明、watchOS システムは、使用可能な非常に限定されたプールがあり、1 つまたは複数のタスクを使用してアプリを起動します。

[![](background-tasks-images/update02.png "WatchOS システムが 1 つまたは複数のタスクを使用してアプリを起動します。")](background-tasks-images/update02.png#lightbox)

Apple を提案を押しながら、アプリがそれ自体の更新プロセスを終了するまで (アプリに制限されたリソースである) ために、このタスクのほとんどを行います。

システム提供これらのタスクを呼び出して、新しい`HandleBackgroundTasks`のメソッド、`WKExtensionDelegate`を委任します。 例えば:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Constructors
        public ExtensionDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request here
            ...
        }
        #endregion
    }
}
```

アプリには、指定されたタスクが完了したらを返します、システムに完了マークすることによって。

[![](background-tasks-images/update03.png "タスクが完了したことをマークすることによって、システムに戻ります")](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>新しいバック グラウンド タスク

watchOS 3 には、コンテンツがあることを確認など、アプリを開く前に、ユーザーが必要な情報を更新するアプリが使用できるいくつかのバック グラウンド タスクが導入されています。

- **アプリの更新をバック グラウンド**- [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask)タスクがバック グラウンドでその状態を更新するアプリを許可します。 通常これを使用して、インターネットから新しいコンテンツのダウンロードなどの別のタスクが含まれます、 [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession)です。
- **スナップショットの更新をバック グラウンド**- [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask)タスクは、システムはドッキング ステーションの設定に使用されるスナップショットを取得する前に、そのコンテンツと UI の両方を更新するアプリを許可します。
- **バック グラウンドのウォッチ接続**- [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask)ペア iPhone からバック グラウンド データを受け取ると、アプリのタスクが開始します。
- **URL のセッションをバック グラウンド**- [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask)バック グラウンド転送は、承認を必要とまたは (正常またはエラー) を完了すると、アプリのタスクが開始します。

これらのタスクは、以下のセクションで詳しく取り上げます。

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

`WKApplicationRefreshBackgroundTask`ウェイク状態が、後でアプリがあるためにスケジュールできる一般的なタスクです。

[![](background-tasks-images/update04.png "将来の日付でウェイク アップ WKApplicationRefreshBackgroundTask")](background-tasks-images/update04.png#lightbox)

タスクの実行時、アプリできますコンプリケーション タイムライン任意の種類の更新などのローカルの処理を行う内またはでいくつか必要なデータをフェッチする`NSUrlSession`です。


<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

システムの送信、`WKURLSessionRefreshBackgroundTask`ダウンロードして、アプリで処理する準備ができて、データが終了した場合。

[![](background-tasks-images/update05.png "データのダウンロードが完了したときに WKURLSessionRefreshBackgroundTask")](background-tasks-images/update05.png#lightbox)

アプリがないバック グラウンドでデータのダウンロード中を実行したままにします。 代わりに、アプリが、データの要求をスケジュールが中断し、システムが、ダウンロードが完了したら、アプリを reawakening のみ、データのダウンロードを処理します。

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

WatchOS 3 で Apple がドッキング ステーション、ユーザーが自分のお気に入りのアプリをピン留めし、すばやくアクセスを追加します。 ユーザーは、Apple Watch での横のボタンを押すと、固定されたアプリのスナップショットのギャラリーが表示されます。 ユーザーは、左または右に目的のアプリを検索し、スナップショットを置き換えて実行中のアプリのインターフェイスを起動するアプリをタップにスワイプします。

[![](background-tasks-images/update06.png "スナップショットを置き換えて、実行中のアプリ インターフェイス")](background-tasks-images/update06.png#lightbox)

システムが、アプリの UI のスナップショットを定期的に、(送信することによって、 `WKSnapshotRefreshBackgroundTask`) を使用してそれらのスナップショットをドッキング ステーションを設定します。 watchOS 正しければ、アプリをこのスナップショットが作成される前に、そのコンテンツと UI を更新します。

スナップショットは、アプリのプレビューおよび起動の両方のイメージとして機能させるために watchOS 3 で非常に重要です。 場合は、ユーザーは、ドッキング ステーションでのアプリで決済、全画面表示を展開し、フォア グラウンドを入力を開始するを実行しているため、スナップショットが最新の状態にある命令型。

[![](background-tasks-images/update07.png "場合は、ユーザーは、ドッキング ステーションでのアプリで決済、全画面表示に展開されます。")](background-tasks-images/update07.png#lightbox)

ここでも、システムが発行する、`WKSnapshotRefreshBackgroundTask`スナップショットを作成できるように、アプリを更新することにより、データと UI) する前に準備できます。

[![](background-tasks-images/update08.png "スナップショットが作成される前に、データと UI を更新することによって、アプリを準備します。")](background-tasks-images/update08.png#lightbox)

アプリをマークすると、`WKSnapshotRefreshBackgroundTask`完了すると、システムは自動的のスナップショットを取ってアプリの UI。

> [!IMPORTANT]
> 常にスケジュールを設定することが重要な` WKSnapshotRefreshBackgroundTask`後、アプリが新しいデータを受信して、ユーザー インターフェイスを更新または変更した情報はユーザーに表示されません。




さらに、ユーザーがアプリから通知を受信してタップが前面に、アプリを表示、ときに、スナップショットをも起動画面として動作しているために、最新の状態にする必要があります。

[![](background-tasks-images/update09.png "ユーザーがアプリから通知を受信し、タップが前面に、アプリを表示")](background-tasks-images/update09.png#lightbox)

された場合に 1 時間以上 watchOS アプリと、ユーザーが操作した後は、既定の状態に戻ることがあります。 既定の状態が違うと意味が異なるアプリをアプリの設計に基づき、いる可能性がありますいない既定の状態にします。

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

WatchOS 3、Apple が統合ウォッチ接続経由で、新しいバック グラウンド更新 API`WKWatchConnectivityRefreshBackgroundTask`です。 この新しい機能を使用して、iPhone アプリを配信できますの新しいデータのウォッチ アプリ対応する watchOS アプリのバック グラウンドで実行中に。

[![](background-tasks-images/update10.png "IPhone アプリは、watchOS アプリのバック グラウンドで実行中に、watch アプリ対応するの新しいデータを配信できます。")](background-tasks-images/update10.png#lightbox)

コンプリケーションのプッシュを開始するには、アプリのコンテキスト、ファイルの送信、または iPhone アプリからユーザー情報の更新は、バック グラウンドで、Apple Watch アプリ復帰します。

ときに、watch アプリはウェイク状態が使用して、 `WKWatchConnectivityRefreshBackgroundTask` iPhone アプリからデータを受信する、標準的な API のメソッドを使用する必要があります。

[![](background-tasks-images/update11.png "WKWatchConnectivityRefreshBackgroundTask データ フロー")](background-tasks-images/update11.png#lightbox)

1. セッションがアクティブになっていることを確認します。
2. 新しい監視`HasContentPending`プロパティの値とならない限り`true`アプリがまだデータを処理します。 としてする前に、アプリはタスク保持すべてのデータの処理が完了するまでです。
3. 処理するデータがある場合 (`HasContentPending = false`)、システムに戻すための完了タスクをマークします。 これに失敗するいると、アプリの割り当てられているバック グラウンドの実行時の結果として、クラッシュ レポートは排気です。

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>バック グラウンド API ライフ サイクル

すべてのバック グラウンド タスクの新しい API の要素を一緒に配置する、相互作用の一般的なセットは、次のようになります。

[![](background-tasks-images/update12.png "バック グラウンド API ライフ サイクル")](background-tasks-images/update12.png#lightbox)

1. まず、watchOS アプリは、バック グラウンド タスクを後でスリープがいくつかの点として状態をスケジュールします。
2. アプリはウェイク状態が、システムと、タスクを送信します。
3. アプリでは、どのような作業が必要なを完了するタスクを処理します。
4. その結果のタスクを処理するには、アプリ必要があります詳しい背景情報を使用して複数のコンテンツのダウンロードなど、将来より多くの作業を完了するタスクをスケジュールする、`NSUrlSession`です。
5. アプリでは、タスクの完了のマークし、システムに返します。

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>リソースの責任を持って使用

WatchOS アプリに動作する責任を持ってこのエコシステム内でそのシステムの共有リソースを消費を制限することで重要です。

次のシナリオで見てをみましょう。

[![](background-tasks-images/update13.png "WatchOS アプリ、システムの共有リソース上のドレインを制限します。")](background-tasks-images/update13.png#lightbox)

1. ユーザーが午後 1時 00分 watchOS アプリを起動します。
2. アプリでは、ウェイク アップし、2時 00分 PM に、1 時間に新しいコンテンツをダウンロードするタスクをスケジュールします。
3. 午後 1 時 50 分に、ユーザーは再度、この時点で、データと UI を更新する許可アプリを開きます。
4. 10 分後にもう一度タスク ウェイク アプリではなく、アプリは、2時 50分 PM に後で、1 時間を実行するタスクを再スケジュールする必要があります。

すべてのアプリは異なりますが、中に、Apple はシステム リソースを節約するために、上記に示すように、使用状況のパターンの検索を提案します。

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>バック グラウンド タスクを実装します。

この例では、このドキュメントはサッカー スコアをユーザーに直属する偽の MonkeySoccer スポーツ アプリを使用します。 

次の一般的な使用シナリオで見てをみましょう。

[![](background-tasks-images/update14.png "一般的な使用シナリオ")](background-tasks-images/update14.png#lightbox)

ユーザーのお気に入りサッカー チーム再生午後 7時 00分から 9時 00分 PM に一致する大規模なため、アプリは、スコアを定期的にチェックしてユーザーを想定する必要があり、30 分間の更新間隔を決定します。

1. ユーザーがアプリを開くし、30 分後にバック グラウンドの更新のタスクをスケジュールします。 バック グラウンド API は、背景に特定の時点で実行されているタスクの 1 つだけの種類を使用します。
2. アプリは、タスクを受信し、データ ファイルと、UI を更新し、バック グラウンド タスクの 30 分後に別のスケジュールします。 開発者が別のバック グラウンド タスクのスケジュールを設定する記憶または複数の更新プログラムを取得する、アプリが再度アクティブしないことが重要です。
3. もう一度、アプリの受信タスクのデータを更新、その UI が更新および別のバック グラウンド タスクの 30 分後にスケジュールします。
4. 同じプロセスがもう一度繰り返されます。
5. タスクが受信した最後の背景と、アプリは、もう一度、データと UI を更新します。 これは、最終的なスコアからは、新しいバック グラウンド更新をスケジュールしません。 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>バック グラウンド更新をスケジュールしています

上記のシナリオを与え、MonkeySoccer アプリはバック グラウンドの更新のスケジュールを設定する次のコードを使用できます。

```csharp
private void ScheduleNextBackgroundUpdate ()
{
    // Create a fire date 30 minutes into the future
    var fireDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

    // Create 
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("LastActiveDate"), NSDate.FromTimeIntervalSinceNow(0));
    userInfo.Add (new NSString ("Reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleBackgroundRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

新たに作成、`NSDate`将来 when に 30 分アプリ アクティブするしようとして作成、`NSMutableDictionary`要求されたタスクの詳細を保持するためにします。 `ScheduleBackgroundRefresh`のメソッド、`SharedExtension`タスクを要求するために使用します。

返されます、`NSError`場合は、要求されたタスクをスケジュールできませんでした。

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>更新の処理

次に、詳しく見て、スコアの更新に必要な手順を示す 5 分のウィンドウで。

[![](background-tasks-images/update15.png "スコアの更新に必要な手順を示す 5 分のウィンドウ")](background-tasks-images/update15.png#lightbox)

1. 7時 30分: 02 PM に、アプリがシステムによって起こさされ、更新プログラムのバック グラウンド タスクを指定します。 その優先サーバーから最新のスコアの取得を開始します。 参照してください[スケジューリング、NSUrlSession](#Scheduling-a-NSUrlSession)以下です。
2. 7時 30分: 05 までに、アプリには、元のタスクが完了する、システムをスリープ状態にアプリを配置して、バック グラウンドで要求されたデータのダウンロードが続行されます。
3. システムには、ダウンロードが完了すると、ダウンロードした情報を処理できるように、アプリをスリープ解除する新しいタスクを作成します。 参照してください[バック グラウンド タスクを処理](#Handling-Background-Tasks)と[ダウンロードの完了を処理](#Handling-the-Download-Completing)以下です。 
4. アプリでは、更新された情報を保存し、タスクの完了をマークします。 開発者は、Apple の提案をそのプロセスを処理するスナップショット タスクのスケジューリングがこの時点で、アプリのユーザー インターフェイスを更新したくなる可能性があります。 参照してください[スナップショットの更新をスケジューリング](#Scheduling-a-Snapshot-Update)以下です。
5. アプリは、スナップショットのタスクを受信、ユーザー インターフェイスを更新、およびタスクの完了のマークを付けます。 参照してください[スナップショットの更新を処理](#Handling-a-Snapshot-Update)以下です。

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>NSUrlSession のスケジュール設定

次のコードは、最新のスコアのダウンロードのスケジュールを使用できます。

```csharp
private void ScheduleURLUpdateSession ()
{
    // Create new configuration
    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.example.urlsession");

    // Create new session
    var backgroundSession = NSUrlSession.FromConfiguration (configuration);

    // Create and start download task
    var downloadTask = backgroundSession.CreateDownloadTask (new NSUrl ("https://example.com/gamexxx/currentScores.json"));
    downloadTask.Resume ();
}
```

構成し、新たに作成`NSUrlSession`、そのセッションを使用して、新しいダウンロードを作成するタスクを使用して、`CreateDownloadTask`メソッドです。 呼び出す、`Resume`ダウンロード セッションを開始するタスクのメソッドです。

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>バック グラウンド タスクの処理

オーバーライドすることで、`HandleBackgroundTasks`のメソッド、`WKExtensionDelegate`アプリは、入力方向のバック グラウンド タスクを処理できます。

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
        #endregion

        ...
        
        #region Public Methods
        public void CompleteTask (WKRefreshBackgroundTask task)
        {
            // Mark the task completed and remove from the collection
            task.SetTaskCompleted ();
            PendingTasks.Remove (task);
        }
        #endregion 

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request
            foreach (WKRefreshBackgroundTask task in backgroundTasks) {
                // Is this a background session task?
                var urlTask = task as WKUrlSessionRefreshBackgroundTask;
                if (urlTask != null) {
                    // Create new configuration
                    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

                    // Create new session
                    var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

                    // Keep track of all pending tasks
                    PendingTasks.Add (task);
                } else {
                    // Ensure that all tasks are completed
                    task.SetTaskCompleted ();
                }
            }
        }
        #endregion
        
        ...
    }
}
```

`HandleBackgroundTasks`メソッド、システムが、アプリを送信したすべてのタスクを循環参照 (で`backgroundTasks`) を探して、`WKUrlSessionRefreshBackgroundTask`です。 1 つが見つかった場合、セッションに再度参加し、アタッチ、`NSUrlSessionDownloadDelegate`ダウンロードの完了を処理する (を参照してください[ダウンロードの完了を処理](#Handling-the-Download-Completing)下)。

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

タスクのハンドルは、コレクションに追加して、それが完了するまでそれ以降します。

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

すべてのアプリに送信タスク必要がありますは現在処理されているいずれかのタスクを完了することを完了としてマーク。

```csharp
if (urlTask != null) {
    ...
} else {
    // Ensure that all tasks are completed
    task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing" />

## <a name="handling-the-download-completing"></a>ダウンロードの完了を処理

MonkeySoccer アプリは、次を使用して`NSUrlSessionDownloadDelegate`ダウンロードの完了を処理し、要求されたデータを処理するデリゲート。

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class BackgroundSessionDelegate : NSUrlSessionDownloadDelegate
    {
        #region Computed Properties
        public ExtensionDelegate WatchExtensionDelegate { get; set; }

        public WKRefreshBackgroundTask Task { get; set; }
        #endregion

        #region Constructors
        public BackgroundSessionDelegate (ExtensionDelegate extensionDelegate, WKRefreshBackgroundTask task)
        {
            // Initialize
            this.WatchExtensionDelegate = extensionDelegate;
            this.Task = task;
        }
        #endregion

        #region Override Methods
        public override void DidFinishDownloading (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, NSUrl location)
        {
            // Handle the downloaded data
            ...

            // Mark the task completed
            WatchExtensionDelegate.CompleteTask (Task);

        }
        #endregion
    }
}
```

初期化されると、それを抑えるには、ハンドル両方、`ExtensionDelegate`と`WKRefreshBackgroundTask`生成します。 も優先、`DidFinishDownloading`ダウンロードの完了を処理するメソッド。 使用して、`CompleteTask`のメソッド、`ExtensionDelegate`通知するために、タスクが完了し、保留中のタスクのコレクションから削除します。 参照してください[バック グラウンド タスクを処理](#Handling-Background-Tasks)上。

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>スナップショットの更新のスケジュール設定

タスクをスケジュールするスナップショットを最新のスコアを持つ、UI を更新するには、次のコードを使用できます。

```csharp
private void ScheduleSnapshotUpdate ()
{
    // Create a fire date of now
    var fireDate = NSDate.FromTimeIntervalSinceNow (0);

    // Create user info dictionary
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
    userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleSnapshotRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

同じように`ScheduleURLUpdateSession`メソッドは上記、作成、新しい`NSDate`、アプリがアクティブでしたいと作成、`NSMutableDictionary`要求されたタスクの詳細を保持するためにします。 `ScheduleSnapshotRefresh`のメソッド、`SharedExtension`タスクを要求するために使用します。

返されます、`NSError`場合は、要求されたタスクをスケジュールできませんでした。

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>スナップショットの更新の処理

スナップショット タスクを処理するために、`HandleBackgroundTasks`メソッド (を参照してください[バック グラウンド タスクの処理](#Handling-Background-Tasks)上) は、次のように変更します。

```csharp
public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
{
    // Handle background request
    foreach (WKRefreshBackgroundTask task in backgroundTasks) {
        // Take action based on task type
        if (task is WKUrlSessionRefreshBackgroundTask) {
            var urlTask = task as WKUrlSessionRefreshBackgroundTask;

            // Create new configuration
            var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

            // Create new session
            var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

            // Keep track of all pending tasks
            PendingTasks.Add (task);
        } else if (task is WKSnapshotRefreshBackgroundTask) {
            var snapshotTask = task as WKSnapshotRefreshBackgroundTask;

            // Update UI
            ...

            // Create a expiration date 30 minutes into the future
            var expirationDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

            // Create user info dictionary
            var userInfo = new NSMutableDictionary ();
            userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
            userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

            // Mark task complete
            snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
        } else {
            // Ensure that all tasks are completed
            task.SetTaskCompleted ();
        }
    }
}
```

メソッドは、処理されているタスクの種類をテストします。 ある場合、`WKSnapshotRefreshBackgroundTask`タスクへのアクセスを取得します。

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

メソッドは、ユーザー インターフェイスを更新し、作成、`NSDate`スナップショットが古くなっているときに、システムに通知します。 作成、`NSMutableDictionary`の新しいスナップショットとスナップショット タスクのこの情報により完了のマークを記述するユーザー情報を使用します。

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

さらに、また、確認スナップショット タスク (最初のパラメーター) で既定の状態に、アプリが返されないことです。 既定の状態の概念がないアプリ常にこのプロパティを設定`true`です。

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>効率的な動作

例のように、上記 MonkeySoccer アプリを適切に機能して、新しい watchOS を使用して、そのスコアを更新するために要する 5 分ウィンドウの 3 つのバック グラウンド タスクをアプリは 15 秒の合計アクティブであったのみ。 

[![](background-tasks-images/update16.png "アプリが 15 秒の合計アクティブであったのみ")](background-tasks-images/update16.png#lightbox)

これは、アプリが使用可能な Apple Watch リソースとバッテリの寿命の両方に与える影響を減らすことができ、また、ウォッチで実行されている他のアプリケーションでうまく機能するアプリです。

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>スケジュール設定の動作

WatchOS 3 アプリは、フォア グラウンドでは、中には、実行し任意の種類のデータの更新などに必要な処理を実行したり、UI を再描画を常にスケジュールされます。 アプリは、バック グラウンドに移動したときが通常、システムによって中断されているし、すべてのランタイム操作が停止されるためです。 

アプリは、バック グラウンドでは、迅速に特定のタスクを実行するシステムで対象となる可能性があります。 したがって、watchOS 2、システムは長い外観通知の処理などの作業を行うまたはアプリのコンプリケーションの更新には、バック グラウンド アプリを起動して一時的に可能性があります。 WatchOS 3 では、バック グラウンドでアプリを実行できるいくつかの新しい方法があります。

アプリは、バック グラウンドでは、システムはいくつかの制限があります。

- 特定のタスクを完了するまでにしばらくがのみ与えられます。 渡された時間だけでなく CPU 電力量にも、アプリがこの制限を派生させる消費している場合、システムは考慮します。
- 上限を超えるすべてのアプリは、次のエラー コードでは強制終了されます。
    - **CPU** - 0xc51bad01
    - **時間**-0xc51bad02
- システムにはを実行するアプリを依頼がバック グラウンド タスクの種類に基づいてさまざまな制限が課すされます。 たとえば、`WKApplicationRefreshBackgroundTask`と`WKURLSessionRefreshBackgroundTask`タスクは、他の種類のバック グラウンド タスクより少し長くなるランタイムを指定します。

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>複雑さの一部とアプリの更新プログラム

Apple watchOS 3 に追加した新しいバック グラウンド タスクに加え watchOS アプリの複雑な問題で、アプリがバック グラウンド更新プログラムを受信する方法とタイミングに影響することができます。

複雑さの一部は、一目で有用な情報を提供する小規模のビジュアル要素です。 選択されているウォッチの文字盤、によっては、そのユーザーは、watchOS 3 で watch アプリで指定できる 1 つまたは複数のコンプリケーションとウォッチの文字盤をカスタマイズする機能を持ちます。

ユーザーには、そのウォッチの文字盤でアプリの複雑な問題のいずれかが含まれている場合になります、アプリを更新する、次の利点があります。

- システムが準備完了の起動にアプリを保持する状態、場所、バック グラウンドでアプリを起動しようとするメモリと、その余分なまでに時間内に維持します。
- 複雑さの一部には、1 日あたりの少なくとも 50 のプッシュ更新プログラムが保証されます。

開発者は必要がありますがアプリをユーザーが上記の理由により、ウォッチの文字盤を追加することを誘いに説得力のある複雑になるに常に努めています。

WatchOS 2 では、複雑さの一部は、アプリがバック グラウンドで実行時に受信したことの主要な手段をいました。 WatchOS 3 でコンプリケーション アプリもを確保する複数の 1 時間あたりの更新を受信する、ただし、使用できる`WKExtensions`をその複雑さの一部を更新する複数のランタイムを要求します。

次のコードを使用して接続している iPhone アプリからコンプリケーションの更新を参照してください。

```csharp
using System;
using WatchConnectivity;
using UIKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
...

private void UpdateComplication ()
{

    // Get session and the number of remaining transfers
    var session = WCSession.DefaultSession;
    var transfers = session.RemainingComplicationUserInfoTransfers;

    // Create user info dictionary
    var iconattrs = new Dictionary<NSString, NSObject>
        {
            {new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0)},
            {new NSString ("reason"), new NSString ("UpdateScore")}
        };

    var userInfo = NSDictionary<NSString, NSObject>.FromObjectsAndKeys (iconattrs.Values.ToArray (), iconattrs.Keys.ToArray ());

    // Take action based on the number of transfers left
    if (transfers < 1) {
        // No transfers left, either attempt to send or inform
        // user of situation.
        ...
    } else if (transfers < 11) {
        // Running low on transfers, only send on important updates
        // else conserve for a significant change.
        ...
    } else {
        // Send data
        session.TransferCurrentComplicationUserInfo (userInfo);
    }
}
```

使用して、`RemainingComplicationUserInfoTransfers`のプロパティ、`WCSession`左への数の優先度の高いアプリの転送を参照してください。 は、日とし、その数に基づいてはアクション用です。 アプリは、転送上での実行を開始、する場合は保留マイナーな更新を送信し、重大な変更がある場合にのみ情報を送信できます。

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>スケジュール設定とドッキング ステーション

WatchOS 3 で Apple がドッキング ステーション、ユーザーが自分のお気に入りのアプリをピン留めし、すばやくアクセスを追加します。 ユーザーは、Apple Watch での横のボタンを押すと、固定されたアプリのスナップショットのギャラリーが表示されます。 ユーザーは、左または右に目的のアプリを検索し、スナップショットを置き換えて実行中のアプリのインターフェイスを起動するアプリをタップにスワイプします。

[![](background-tasks-images/dock01.png "ドッキング ステーション")](background-tasks-images/dock01.png#lightbox)

システムは定期的に、アプリの UI のスナップショットし、それらのスナップショットを使用してドキュメントを作成します。watchOS 正しければ、アプリをこのスナップショットが作成される前に、そのコンテンツと UI を更新します。

ドッキング ステーションに固定されているアプリは、次の期待される結果します。

- 1 時間あたりの更新が表示されます。 これには、スナップショット タスクされ、アプリの更新タスクの両方が含まれます。
- 更新プログラムの割り当ては、すべてドッキング ステーションでは、アプリの間で分散されます。 したがって少数アプリ、ユーザーが固定されている各アプリは受信より潜在的な更新プログラム。
- ドッキング ステーションから選択すると、アプリがすばやく再開ように、アプリがメモリに保持されます。

ユーザーが実行された最後のアプリと見なされます、_最近使用した_アプリは、ドッキング ステーションに最後のスロットを占有するとします。 そこから、あるユーザーは、ドッキング ステーションに完全にピン留めを選択できます。 最も最近使用したは、他のお気に入りアプリ、ユーザーがドッキング ステーションにピン留め済みと同じように処理されます。

> [!IMPORTANT]
> 正規のスケジュールのみが、ホーム画面に追加されているアプリが付与されませんされます。 正規のスケジュール設定と背景が表示される更新プログラムをアプリ_必要があります_ドッキング ステーションに追加します。

このドキュメントで前述したように、スナップショットの値は、アプリのプレビューおよび起動の両方のイメージとして機能させるため watchOS 3 で非常に重要です。 場合は、ユーザーは、ドッキング ステーションでのアプリで決済、全画面表示を展開し、フォア グラウンドの入力を開始するを実行しているため、スナップショットが最新の状態にする必要があります。

システムには決定、アプリの UI の新しいスナップショットが必要な時間である可能性があります。 この状況では、スナップショットの要求は、アプリの実行時の予算としてはカウントされません。 次は要求をトリガーするシステムのスナップショット。

- コンプリケーション タイムライン更新します。
- アプリの通知がユーザーによって操作します。
- フォア グラウンドからバック グラウンドの状態への切り替え。
- バック グラウンドの状態にあるものの 1 時間後にそのため、アプリ返すことができます既定の状態にします。
- ときに watchOS を初めて起動します。

<a name="Best-Practices" />

## <a name="best-practices"></a>ベスト プラクティス 

Apple は、バック グラウンド タスクで操作するときに、次のベスト プラクティスを示しています。

- アプリを更新する必要があるため、多くの場合、スケジュールを設定します。 アプリを実行するたびを再、将来のニーズを評価し、必要に応じてこのスケジュールを調整します。
- システムが更新のバック グラウンド タスクを送信し、アプリは、更新プログラムを必要としない場合、は、更新プログラムが実際に必要になるまで、作業を延期します。
- アプリを利用可能なすべてのランタイム営業案件を考慮してください。
    - ドッキング ステーションとフォア グラウンドのアクティブ化します。
    - 通知です。
    - コンプリケーションを更新します。
    - バック グラウンド更新します。
- 使用して`ScheduleBackgroundRefresh`などの汎用的なバック グラウンド ランタイム。
    - については、システムをポーリングしています。
    - 将来のスケジュール`NSURLSessions`バック グラウンド データを要求します。 
    - 時間の切り替えを知られています。
    - コンプリケーションの更新をトリガーします。

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>スナップショットのベスト プラクティス

スナップショットの更新を使用するときに、Apple は、次の提案を加えます。

- 重要な内容の変更がある場合など、必要時にのみスナップショットを無効にします。
- 高周波数のスナップショットの無効化しないでください。 たとえば、タイマー アプリでも、スナップショットを毎秒更新しないでくださいする必要がありますのみで行う必要が、タイマーが終了したときにします。

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>アプリのデータ フロー

Apple では、データ フローを操作するため、次を提案します。

[![](background-tasks-images/update17.png "アプリのデータ フロー ダイアグラム")](background-tasks-images/update17.png#lightbox)

(ウォッチ接続がある) などの外部イベントには、アプリが起動します。 これにより、(をアプリの現在の状態を表す) のデータ モデルを更新するアプリです。 その結果データ モデルの変更のアプリは必要があります、複雑さの一部を更新する、新しいスナップショットを要求、開始、バック グラウンド`NSURLSession`より多くのデータをプルし、さらにバック グラウンドのスケジュールを更新します。

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>アプリのライフ サイクル

ドッキング ステーションとしてより多くのアプリ間で移動するユーザーには Apple だと考えるのお気に入りのアプリをピン留めすることによりはるかに多く、しの場合と同じ watchOS 2 です。 その結果、アプリは、この変更を処理およびフォア グラウンドとバック グラウンドの状態をすばやく移動する準備がする必要があります。

Apple では、以下の推奨事項があります。

- アプリがフォア グラウンドのアクティベーションを入力時にできるだけ早くすべてのバック グラウンド タスクを終了することを確認します。
- すべてのフォア グラウンド処理を完了するように呼び出すことによって、バック グラウンドに入る前に`NSProcessInfo.PerformExpiringActivity`です。
- WatchOS シミュレーターでアプリをテストするときにようになる機能を正しくテストに必要なアプリを更新できるようにタスク予算のいずれも適用されます。
- 常に、iTunes Connect に過去のパブリッシュする前に、予算、アプリが実行されていないことを確認する実際の Apple Watch ハードウェアでテストします。
- Apple では、Apple Watch に充電器のテストとデバッグ中に保持することをお勧めします。
- コールド両方を起動して、アプリの再開がかもしれないテストされることを確認します。
- アプリのすべてのタスクが完了していることを確認します。
- 最高と最低値の両方をテストするドッキング ステーションにピン留めするアプリの数を変更するケース シナリオです。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Apple が watchOS およびそのを使用して watch アプリを最新に保つ方法に対する機能強化について説明しました。 最初に、新しい内容をすべて説明 watchOS 3 でバック グラウンド タスク Apple が追加されます。 次に、バック グラウンド API のライフ サイクル、および Xamarin watchOS アプリのバック グラウンド タスクを実装する方法について説明します。 最後に、スケジュールする方法を説明し、ベスト プラクティスを指定します。



## <a name="related-links"></a>関連リンク

- [iOS 10 サンプル](https://developer.xamarin.com/samples/ios/iOS10/)
