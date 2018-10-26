---
title: watchOS Xamarin でのバック グラウンド タスク
description: このドキュメントでは、Xamarin では、バック グラウンド タスクの種類について見て、リソースを使用してバック グラウンド タスク、スケジュール設定、ベスト プラクティス、および複数の実装で watchOS でバック グラウンド タスクを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/13/2017
ms.openlocfilehash: 4105193ea69eaf369ae62632090a281e641303f7
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110369"
---
# <a name="watchos-background-tasks-in-xamarin"></a>watchOS Xamarin でのバック グラウンド タスク

WatchOS 3 でを watch アプリに保つできますについては、次の 3 つの主な方法です。 

- いくつかの新しいバック グラウンド タスクのいずれかを使用します。 
- その複雑さのいずれかを (更新する余分な時間に与える) ウォッチの文字盤でがあります。 
- 新しいドッキング ステーションにアプリにユーザー暗証番号 (pin) を持つ (場所のメモリに保持され、頻繁に更新) します。 

## <a name="keeping-an-app-up-to-date"></a>アプリを最新に保つ

すべての開発者から現在と更新された、watchOS アプリのデータとユーザー インターフェイスを保持できる方法を説明する前にこのセクションでは見て一般的な一連の使用パターンと、iPhone とに基づいて、1 日を通して、Apple Watch との間、ユーザーの行動可能性があります。 時刻およびアクティビティが現在により、(運転) など。

次の例を参照してください。

[![](background-tasks-images/update00.png "IPhone と、1 日を通して、Apple Watch との間、ユーザーの行動可能性があります。")](background-tasks-images/update00.png#lightbox)

1. 、コーヒーの行で待機中に、朝には、ユーザーは、数分、iPhone で最新のニュースを参照します。
2. コーヒー ショップを終了する前に、迅速で、ウォッチの文字盤を合わせると、天気を確認します。
3. 昼食の前に、アプリを使用してマップ iPhone の近くのレストランを検索し、クライアントを満たすために予約を予約します。
4. レストランに出張中で、Apple Watch の [概要] を使用して通知を受け取りますが、昼食の予定を遅れてを知っています。
5. 夜間に使用するマップ アプリ、iPhone で家の前にトラフィックをチェックします。
6. 方法にいくつかの牛乳を取得するかを確認する、Apple Watch で、iMessage 通知を受け取ったホームとクイック返信機能を使用して"OK"応答を送信します。

「概要」ため (3 秒未満)、ユーザーが通常存在、Apple Watch アプリを使用しようとしている方法の性質のない十分な時間をアプリに必要な情報をフェッチし、ユーザーに表示する前に、UI を更新します。

新しいを使用して、Apple の Api が含まれているの watchOS 3 で、アプリをスケジュールでき、_バック グラウンド更新_ユーザーを要求する前に準備が必要な情報があるとします。 天気の複雑な前述の例を実行します。

[![](background-tasks-images/update01.png "天気の複雑な例")](background-tasks-images/update01.png#lightbox)

1. 特定の時点で、システムによって起こされるアプリのスケジュール。 
2. アプリでは、更新プログラムを生成することを必要とする情報をフェッチします。
3. アプリは、新しいデータを反映するように、ユーザー インターフェイスを再生成します。
4. ユーザーは、アプリのコンプリケーションの概要、ときに、更新プログラムを待機する必要はありません最新の情報があります。

上記のよう、watchOS システム スリープするが非常に限定されたプールの使用可能な 1 つまたは複数のタスクを使用してアプリに。

[![](background-tasks-images/update02.png "WatchOS システムを 1 つまたは複数のタスクを使用してアプリを起動します。")](background-tasks-images/update02.png#lightbox)

Apple の提案に押し、アプリ自体の更新プロセスが完了するまで (制限付きのリソースをアプリにある) ために、このタスクのほとんどを行います。

システムが提供これらのタスクを呼び出して、新しい`HandleBackgroundTasks`のメソッド、`WKExtensionDelegate`を委任します。 例えば:

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

アプリには、特定のタスクが完了したらを返します、システムに完了したことをマークすることで。

[![](background-tasks-images/update03.png "タスクが完了したことをマークすることによってシステムに返す")](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>新しいバック グラウンド タスク

watchOS 3 には、コンテンツがあることを確認など、アプリを開く前に、ユーザーが必要な情報を更新するアプリが使用できるいくつかのバック グラウンド タスクが導入されています。

- **アプリの更新をバック グラウンド**- [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask)タスクがバック グラウンドでは、その状態を更新するアプリを許可します。 通常、これを使用して、インターネットから新しいコンテンツのダウンロードなどの別のタスクが含まれます、 [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession)します。
- **スナップショットの更新をバック グラウンド**- [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask)タスクは、システムは、ドッキング ステーションを設定するために使用するスナップショットを取得する前に、そのコンテンツと UI の両方を更新するアプリを許可します。
- **ウォッチの接続をバック グラウンド**- [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask)ペアになっている iPhone からバック グラウンド データを受信すると、アプリのタスクが開始します。
- **URL のセッションをバック グラウンド**- [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask)バック グラウンド転送が承認を必要があります。 または (正常にまたはエラー) を完了すると、アプリのタスクが開始します。

これらのタスクは、以下のセクションで詳しく説明されます。

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

`WKApplicationRefreshBackgroundTask`はアプリを将来の日付でウェイク アップするようにスケジュールする一般的なタスクです。

[![](background-tasks-images/update04.png "将来の日付でウェイク アップ WKApplicationRefreshBackgroundTask")](background-tasks-images/update04.png#lightbox)

タスクのランタイム、内で、アプリはコンプリケーション タイムラインの任意の種類の更新などのローカル処理を行うか、で必要なデータをフェッチする`NSUrlSession`します。


<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

システムは送信、`WKURLSessionRefreshBackgroundTask`ダウンロードしてすぐに、アプリによって処理できるデータが完了するとします。

[![](background-tasks-images/update05.png "データのダウンロードが完了すると、WKURLSessionRefreshBackgroundTask")](background-tasks-images/update05.png#lightbox)

アプリでは、データがバック グラウンドでダウンロード中に実行されてい。 代わりに、アプリがデータの要求をスケジュールし、ジョブは中断され、システムは、ダウンロードが完了したら、アプリを reawakening のみ、データのダウンロードを処理します。

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

WatchOS 3、Apple ドッキング ステーション ユーザーが、お気に入りのアプリをピン留めしてにすばやくアクセスできますが追加されます。 ユーザーは、Apple Watch の横のボタンを押すと、アプリをピン留めされたスナップショットのギャラリーが表示されます。 ユーザーは、左または右に、目的のアプリを検索し、そのスナップショットを置き換えて、実行中のアプリのインターフェイスを起動するアプリをタップにスワイプします。

[![](background-tasks-images/update06.png "スナップショットを置き換えて、実行中のアプリのインターフェイス")](background-tasks-images/update06.png#lightbox)

システムは、アプリの UI のスナップショットが定期的に取得 (送信することによって、 `WKSnapshotRefreshBackgroundTask`) し、それらのスナップショットを使用して、ドッキング ステーションを作成します。 watchOS では、アプリに、このスナップショットを作成する前に、そのコンテンツと UI を更新する機会が与えられます。

アプリのプレビューと起動イメージとして機能させるため、スナップショットは watchOS 3 で非常に重要です。 ドッキング ステーションでのアプリで、ユーザーが安定、全画面表示を展開し、フォア グラウンドの入力、実行が開始できるようになりますが、スナップショットが最新の状態にある命令型しても。

[![](background-tasks-images/update07.png "ドッキング ステーションでのアプリで、ユーザーが安定している場合は、全画面表示を拡張します。")](background-tasks-images/update07.png#lightbox)

ここでも、システムが発行されます、`WKSnapshotRefreshBackgroundTask`スナップショットの作成前に (データと、UI の更新) をアプリを準備できるようにします。

[![](background-tasks-images/update08.png "スナップショットを作成する前に、データと UI を更新することで、アプリを準備します。")](background-tasks-images/update08.png#lightbox)

アプリをマークすると、`WKSnapshotRefreshBackgroundTask`完了すると、システムは自動的に取得、アプリの UI のスナップショット。

> [!IMPORTANT]
> 常にスケジュールを設定することが重要な` WKSnapshotRefreshBackgroundTask`後、アプリが新しいデータを受信して、ユーザー インターフェイスを更新またはユーザーが変更された情報は表示されません。




さらに、ユーザーは、アプリから通知を受信し、タップによって、アプリがフォア グラウンドに表示する、スナップショットをも起動画面として動作しているために、最新の状態にする必要があります。

[![](background-tasks-images/update09.png "ユーザーがアプリから通知を受け取るし、タップによって、アプリがフォア グラウンドに表示します。")](background-tasks-images/update09.png#lightbox)

場合は、ユーザーの操作、watchOS アプリをしてから 1 時間以上経過したを既定の状態を返すことがあります。 既定の状態はさまざまなアプリへのさまざまなことを意味し、アプリの設計に基づき、持つことができない既定の状態にします。

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

WatchOS 3、Apple が統合ウォッチ接続経由で、新しいバック グラウンド更新 API を使用した`WKWatchConnectivityRefreshBackgroundTask`します。 この新しい機能を使用して、iPhone アプリに配信できます最新のデータ、watch アプリ対応 watchOS アプリのバック グラウンドで実行中に。

[![](background-tasks-images/update10.png "IPhone アプリは、watchOS アプリがバック グラウンドで実行中に、watch アプリ対応するの新しいデータを配信できます。")](background-tasks-images/update10.png#lightbox)

コンプリケーションのプッシュを開始するには、アプリのコンテキストのファイルの送信または iPhone アプリからユーザー情報の更新は、バック グラウンドで、Apple Watch アプリ wake します。

使用して、watch アプリがウェイク アップを`WKWatchConnectivityRefreshBackgroundTask`iPhone アプリからデータを受信する標準的な API メソッドを使用する必要があります。

[![](background-tasks-images/update11.png "WKWatchConnectivityRefreshBackgroundTask データ フロー")](background-tasks-images/update11.png#lightbox)

1. セッションがアクティブになっていることを確認します。
2. 新しい監視`HasContentPending`プロパティの値が限り`true`アプリがまだデータを処理します。 ようにする前に、アプリはすべてのデータの処理が完了するまでに、タスクを保持する必要があります。
3. 処理するデータがある場合 (`HasContentPending = false`)、システムに戻すために完了したタスクをマークします。 これに失敗すると、結果として、クラッシュ レポート、アプリの割り当てられたバック グラウンドのランタイムは使い果たしてしまいます。

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>バック グラウンド API ライフ サイクル

新しいバック グラウンド タスク API の構成要素のすべてにまとめて配置する、相互作用の標準的なセットは次のようになります。

[![](background-tasks-images/update12.png "バック グラウンド API ライフ サイクル")](background-tasks-images/update12.png#lightbox)

1. 最初に、watchOS アプリはバック グラウンド タスクを後で、スリープがいくつかの点として状態をスケジュールします。
2. アプリは、システムによってウェイク アップし、タスクを送信します。
3. アプリでは、どのような作業が必要でしたが完了するタスクを処理します。
4. その結果のタスクを処理するには、アプリケーションする必要があります詳しい背景情報を使用して複数のコンテンツのダウンロードなど、将来より多くの作業を完了するタスクをスケジュール、`NSUrlSession`します。
5. アプリでは、タスクの完了をマークし、システムに返します。

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>責任を持ってリソースの使用

WatchOS アプリに動作する責任を持ってこのエコシステム内でそのシステムの共有リソースを消費を制限することで重要です。

次のシナリオを参照してください。

[![](background-tasks-images/update13.png "WatchOS アプリの制限、システムの共有リソース上のドレイン")](background-tasks-images/update13.png#lightbox)

1. ユーザーは、午後 1 時、watchOS アプリを起動します。
2. アプリでは、ウェイク アップし、午後 2 時に 1 時間に新しいコンテンツをダウンロードするタスクをスケジュールします。
3. 午後 1時 50分ユーザーでは、この時点で、データと UI を更新するため、アプリが再が開きます。
4. 10 分後にもう一度タスク ウェイク アプリだけでなく、アプリは、午後 2 時 50 分は後で、1 時間に実行するタスクを再スケジュールする必要があります。

すべてのアプリは異なるが、Apple がシステム リソースを節約するために、上記に示すように、使用状況のパターンの検索をお勧めします。

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>バック グラウンド タスクを実装します。

この例では、このドキュメントはサッカー スコアをユーザーに報告される偽 MonkeySoccer スポーツ アプリを使用します。 

次の一般的な使用シナリオを参照してください。

[![](background-tasks-images/update14.png "一般的な使用シナリオ")](background-tasks-images/update14.png#lightbox)

ユーザーのお気に入りのサッカー チームの 7時 00分 PM から 9時 00分 PM に一致するビッグ再生アプリ スコアを定期的にチェックしてユーザーを想定する必要があり、30 分間の更新間隔を決定します。

1. ユーザーは、アプリを開き、30 分後のタスクをバック グラウンドの更新をスケジュールします。 バック グラウンド API は、背景が特定の時点で実行されているタスクの 1 つだけの種類を使用できます。
2. アプリがタスクを受信し、そのデータと、UI を更新し、別のスケジュールでバック グラウンド タスクの 30 分後です。 開発者が別のバック グラウンド タスクをスケジュールする記憶こと、またはその他の更新を取得する、アプリが再度いなければことはありませんが重要です。
3. ここでも、アプリ、タスクを受け取るとそのデータを更新、その UI を更新および別のバック グラウンド タスクの 30 分後にスケジュールします。
4. 同じプロセスがもう一度繰り返されます。
5. タスクが受信した最後の背景と、アプリは、データ ファイルと UI をもう一度更新します。 最終的なスコアは、これ以降は、新しいバック グラウンド更新をスケジュールしません。 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>バック グラウンドの更新のスケジュール設定

上記のシナリオを指定するには、MonkeySoccer アプリはバック グラウンドの更新のスケジュールを設定する次のコードを使用できます。

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

新たに作成しますが`NSDate`将来のときに 30 分アプリいなければするしようとして作成、`NSMutableDictionary`要求されたタスクの詳細を保持します。 `ScheduleBackgroundRefresh`のメソッド、`SharedExtension`はタスクをスケジュールする要求に使用します。

返されます、`NSError`要求されたタスクのスケジュールを設定できなかった場合。

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>更新の処理

次に、5 分 ウィンドウで、スコアの更新に必要な手順を示すについて詳しく見てをみましょう。

[![](background-tasks-images/update15.png "スコアを更新するために必要な手順を示す 5 分のウィンドウ")](background-tasks-images/update15.png#lightbox)

1. 午後 7時 30分: 02、アプリが、システムによって起こさされ、更新プログラムのバック グラウンド タスクを指定します。 その最初の優先順位では、サーバーから最新のスコアを取得します。 参照してください[スケジューリング、NSUrlSession](#Scheduling-a-NSUrlSession)以下。
2. アプリを 7時 30分: 05 では、元のタスクが完了すると、システムがスリープ状態にアプリを配置し、引き続きバック グラウンドで要求されたデータをダウンロードします。
3. システムでは、ダウンロードが完了したら、ダウンロードした情報を処理できるように、アプリをウェイクするための新しいタスクを作成します。 参照してください[バック グラウンド タスクの処理](#Handling-Background-Tasks)と[ダウンロードの完了を処理する](#Handling-the-Download-Completing)以下。 
4. アプリでは、更新された情報を保存し、タスクが完了したをマークします。 Apple は、そのプロセスを処理するために、スナップショット タスクのスケジュール設定を示しますが、開発者がこの時点で、アプリのユーザー インターフェイスを更新したくなる可能性があります。 参照してください[スナップショットの更新のスケジュール設定](#Scheduling-a-Snapshot-Update)以下。
5. アプリは、スナップショット タスクの受信、ユーザー インターフェイスを更新、およびタスクの完了のマークを付けます。 参照してください[スナップショットの更新を処理](#Handling-a-Snapshot-Update)以下。

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>NSUrlSession のスケジュール設定

最新のスコアのダウンロードをスケジュールする、次のコードを使用できます。

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

構成し、新たに作成します`NSUrlSession`、そのセッションを使用して、新しいダウンロードの作成タスクを使用して、`CreateDownloadTask`メソッド。 呼び出す、`Resume`ダウンロード セッションを開始するタスクのメソッド。

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>バック グラウンド タスクの処理

オーバーライドすることで、`HandleBackgroundTasks`のメソッド、`WKExtensionDelegate`アプリが受信のバック グラウンド タスクを処理できます。

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

`HandleBackgroundTasks`メソッド サイクルを通じてすべてのタスク、システムが、アプリを送信する (で`backgroundTasks`) を探して、`WKUrlSessionRefreshBackgroundTask`します。 再度セッションに参加し、アタッチが見つかった場合、`NSUrlSessionDownloadDelegate`ダウンロードの完了を処理するために (を参照してください[ダウンロードの完了を処理](#Handling-the-Download-Completing)の下)。

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

コレクションに追加することによってこれが完了するまでのハンドルをタスクの保持します。

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

すべてのタスクをアプリに送信される必要がありますは現在処理されているすべてのタスクを完了する完了としてマーク。

```csharp
if (urlTask != null) {
    ...
} else {
    // Ensure that all tasks are completed
    task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing" />

## <a name="handling-the-download-completing"></a>ダウンロードの完了を処理します。

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

初期化されると、両方に識別するハンドルを保持、 `ExtensionDelegate` 、`WKRefreshBackgroundTask`すると、生成されました。 これは、上書き、`DidFinishDownloading`ダウンロードの完了を処理するメソッド。 使用して、`CompleteTask`のメソッド、`ExtensionDelegate`し、完了したタスクを通知し、保留中のタスクのコレクションから削除します。 参照してください[バック グラウンド タスクの処理](#Handling-Background-Tasks)上。

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>スナップショットの更新のスケジュール設定

次のコードは、最新のスコアを持つ UI を更新するスナップショット タスクのスケジュール設定を使用できます。

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

同じように`ScheduleURLUpdateSession`上記のメソッドを作成、新しい`NSDate`、アプリがいなければするしたいと作成、`NSMutableDictionary`要求されたタスクの詳細を保持します。 `ScheduleSnapshotRefresh`のメソッド、`SharedExtension`はタスクをスケジュールする要求に使用します。

返されます、`NSError`要求されたタスクのスケジュールを設定できなかった場合。

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>スナップショットの更新の処理

スナップショットのタスクを処理するために、`HandleBackgroundTasks`メソッド (を参照してください[バック グラウンド タスクの処理](#Handling-Background-Tasks)上)、次のように変更されます。

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

メソッドは、処理されているタスクの種類をテストします。 ある場合、`WKSnapshotRefreshBackgroundTask`タスクへのアクセスが得られます。

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

メソッドは、ユーザー インターフェイスを更新し、作成、`NSDate`スナップショットが古くなっているときに、システムに通知します。 作成されます、`NSMutableDictionary`新しいスナップショットとスナップショット タスクのこの情報により完了のマークを説明するユーザーの情報を記入します。

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

さらに、また、確認、スナップショット タスク (最初のパラメーター) の既定の状態に、アプリが返されないこと。 既定の状態の概念がないアプリこのプロパティを設定する必要があります常に`true`します。

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>適切に機能してください。

上記の例に示す MonkeySoccer アプリが適切に機能して、新しい watchOS を使用して、そのスコアを更新するのに要した 5 つ分の 3 つのバック グラウンド タスクをアプリは、15 秒の合計アクティブであったのみ。 

[![](background-tasks-images/update16.png "アプリが 15 秒の合計アクティブのみ")](background-tasks-images/update16.png#lightbox)

これは、アプリが Apple Watch の使用可能なリソースとバッテリの寿命の両方に与える影響が削減されるウォッチで実行されている他のアプリでうまく機能するアプリにもできます。

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>スケジューリングのしくみ

WatchOS 3 アプリがフォア グラウンドでは、実行し任意の種類のデータの更新などに必要な処理を実行したり、その UI を再描画を常にスケジュール設定します。 バック グラウンドに移動すると、アプリと、システムによって、通常は中断されていたすべてのランタイム操作が停止しました。 

アプリは、バック グラウンドでは、迅速に特定のタスクを実行するシステムで対象となる可能性があります。 したがって、watchOS 2、システムはロング ルック通知の処理などを行うやアプリのコンプリケーションの更新には、バック グラウンド アプリを起動して一時的に可能性があります。 WatchOS 3 では、アプリはバック グラウンドで実行できるいくつかの新しい方法があります。

アプリは、バック グラウンドでは、システムでいくつかの制限が課せられます。

- 指定したタスクの実行に数秒が与えられますのみです。 経過時間の量が CPU 電力量にも、アプリがこの制限を派生させる消費しているだけでなく、システムでの考慮事項が考慮されます。
- 次のエラー コードがその制限を超えるすべてのアプリを強制終了されます。
    - **CPU** - 0xc51bad01
    - **時間**-0xc51bad02
- システムは、アプリを実行する、要求がバック グラウンド タスクの種類に基づいてさまざまな制限を適用します。 たとえば、`WKApplicationRefreshBackgroundTask`と`WKURLSessionRefreshBackgroundTask`タスクは、他の種類のバック グラウンド タスクより若干長くなるランタイムを指定します。

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>複雑な問題と、アプリの更新

Apple は watchOS 3 に追加できる新しいバック グラウンド タスク、に加えて watchOS アプリの複雑な問題には、アプリがバック グラウンド更新を受け取る方法とタイミングに影響することができます。

コンプリケーションとは、ひとめで有益な情報を提供する小規模なビジュアル要素です。 選択されている腕時計によっては、ユーザーは、watchOS 3 で watch アプリによって提供できる 1 つまたは複数のコンプリケーションとウォッチの文字盤をカスタマイズする機能を持ちます。

アプリ、次の更新は、そのユーザーには、ウォッチの文字盤の上のアプリの複雑な問題のいずれかが含まれている場合の利点。

- システムが準備完了の起動でアプリを保持し、更新する時間を余分なメモリ内でバック グラウンドでアプリを起動しようとする状態に維持します。
- 複雑な問題には、1 日あたり少なくとも 50 のプッシュの更新プログラムが保証されます。

開発者は常に、それぞれのアプリのユーザーが上記の理由で、ウォッチの文字盤を追加することを誘いを説得力のある複雑になるに努めています。

WatchOS 2、複雑な問題は、アプリがバック グラウンドでランタイムを受信する主な方法でした。 WatchOS 3 で複雑なアプリをまだを確保する 1 時間あたり複数の更新プログラムを受信する、ただし、使用できる`WKExtensions`その複雑さを更新する複数のランタイムを要求します。

接続している iPhone アプリからコンプリケーションを更新するために使用する次のコードを参照してください。

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

使用して、`RemainingComplicationUserInfoTransfers`のプロパティ、`WCSession`数の優先度の高いアプリに転送してが 1 日とし、その数値に基づいて必要なアクションのままです。 場合は、アプリは、転送上での実行を開始、留保マイナー更新を送信し、重大な変更がある場合にのみ情報を送信できます。

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>スケジュール設定とドッキング ステーション

WatchOS 3、Apple ドッキング ステーション ユーザーが、お気に入りのアプリをピン留めしてにすばやくアクセスできますが追加されます。 ユーザーは、Apple Watch の横のボタンを押すと、アプリをピン留めされたスナップショットのギャラリーが表示されます。 ユーザーは、左または右に、目的のアプリを検索し、そのスナップショットを置き換えて、実行中のアプリのインターフェイスを起動するアプリをタップにスワイプします。

[![](background-tasks-images/dock01.png "ドッキング ステーション")](background-tasks-images/dock01.png#lightbox)

システムは定期的には、アプリの UI のスナップショットを取得し、それらのスナップショットを使用してドキュメントを事前設定。watchOS では、アプリに、このスナップショットを作成する前に、そのコンテンツと UI を更新する機会が与えられます。

ドッキング ステーションにピン留めしたアプリには、次が想定されます。

- 最小で 1 時間あたりの更新が表示されます。 これには、アプリの更新のタスクと、スナップショット タスクの両方が含まれます。
- 更新プログラムの予算は、すべてドッキング ステーション内のアプリの間で分散されます。 したがって少数アプリ ユーザーが固定されてより潜在的な更新プログラムが各アプリが表示されます。
- アプリは、ドッキング ステーションから選択すると、アプリがすぐに再開するため、メモリに保持されます。

最後のアプリ、ユーザーが実行したと見なされます、_最近使用した_アプリとは、ドッキング ステーション内の最終スロットを占有します。 そこからユーザーをドッキング ステーションに完全にピン留めを選択できます。 最も最近使用したは、ドッキング ステーションには、任意の他のお気に入りアプリ ユーザーは固定されて既にと同様に扱われます。

> [!IMPORTANT]
> いない通常のスケジュールのみが、ホーム画面に追加されているアプリが与えられます。 定期的なスケジュール設定と背景を受信する更新プログラム、アプリ_する必要があります_ドッキング ステーションに追加します。

このドキュメントで既に述べたように、スナップショットは、アプリのプレビューと起動イメージとして機能させるため watchOS 3 で非常に重要です。 ドッキング ステーションでのアプリで、ユーザーが安定している場合を全画面表示を展開、フォア グラウンドの入力、実行が開始できるようになりますが、スナップショットが最新の状態にある命令型、されます。

システムには決定、アプリの UI の新しいスナップショットが必要な時間である可能性があります。 この状況では、アプリのランタイムの予算と比較のスナップショット要求はカウントされません。 次のスナップショットのシステム要求はトリガーします。

- コンプリケーションのタイムライン更新します。
- アプリの通知との対話をユーザー。
- フォア グラウンドからバック グラウンド状態への切り替え。
- バック グラウンド状態になることの 1 時間後にので、アプリを返せる既定の状態にします。
- WatchOS を初めて起動します。

<a name="Best-Practices" />

## <a name="best-practices"></a>ベスト プラクティス 

Apple は、バック グラウンド タスクを使用する場合、次のベスト プラクティスを示しています。

- スケジュールの頻度、アプリを更新する必要があります。 アプリを実行するたびには、将来のニーズを再評価する必要があり、必要に応じてこのスケジュールを調整します。
- バック グラウンド更新作業をシステムに送信アプリは、更新プログラムを必要としない場合は、更新プログラムが実際に必要になるまで、作業を延期します。
- アプリに使用可能なすべてのランタイム営業案件を検討してください。
    - ドッキング ステーションとフォア グラウンドのアクティブ化します。
    - 通知します。
    - コンプリケーションの更新プログラム。
    - バック グラウンド更新します。
- 使用`ScheduleBackgroundRefresh`などの汎用的なバック グラウンド ランタイム。
    - については、システムをポーリングします。
    - 将来のスケジュール`NSURLSessions`バック グラウンド データを要求します。 
    - 時間の切り替えを知られています。
    - コンプリケーションの更新をトリガーします。

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>スナップショットのベスト プラクティス

スナップショットの更新プログラムを使用する場合は、次の推奨事項が、Apple:

- 重要な内容の変更がある場合に、必要ななどの場合にのみスナップショットを無効にします。
- 頻度の高いスナップショットの無効化しないでください。 たとえば、タイマーのアプリでも、スナップショットを毎秒更新しないでくださいには、タイマーが終了したときにのみ実行してあります。

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>アプリのデータ フロー

Apple では、データ フローを操作するため、次をお勧めします。

[![](background-tasks-images/update17.png "アプリのデータ フロー ダイアグラム")](background-tasks-images/update17.png#lightbox)

(ウォッチ接続がある) などの外部イベントは、アプリを解除します。 これにより、そのデータ モデル (つまり、アプリの現在の状態を表します) を更新するアプリです。 その結果のデータ モデルの変更、アプリに必要な複雑な問題を更新するに新しいスナップショットを要求、開始、バック グラウンド`NSURLSession`より多くのデータをプルし、さらにバック グラウンドのスケジュール設定を更新します。

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>アプリのライフ サイクル

ドッキング ステーションと、Apple が考えているより多くのアプリ間で移動するユーザーにお気に入りのアプリをピン留めすることによりよりはるかに多くの場合、しの場合と同じ watchOS 2。 その結果、アプリは、この変更の処理し、フォア グラウンドとバック グラウンドの状態をすばやく移動できるようにする必要があります。

Apple では、次の推奨事項があります。

- アプリがフォア グラウンドのアクティベーションを入力すると、できるだけ早くすべてのバック グラウンド タスクが完了したことを確認します。
- すべてのフォア グラウンドの作業を完了するように呼び出すことによって、バック グラウンドに入る前に`NSProcessInfo.PerformExpiringActivity`します。
- WatchOS シミュレーターでアプリをテストするときに適切に機能をテストするために必要な限りアプリを更新できるようにタスク予算のいずれも適用されます。
- 実際の Apple Watch のハードウェアを iTunes Connect に、アプリを過去のパブリッシュする前に、予算が実行されていることを確認するを常にテストします。
- Apple では、テストおよびデバッグ中に電池を Apple Watch を維持することをお勧めします。
- コールド両方を起動して、アプリの再開はテストかもしれないことを確認します。
- アプリのすべてのタスクが完了することを確認します。
- 最高と最低値の両方をテストするドッキング ステーションにピン留めされているアプリの数を変更するケース シナリオです。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Apple は watchOS とそれらを使用して watch アプリを最新に保つ方法に対する機能強化について説明しました。 最初に、新しい内容をすべて説明バック グラウンド タスクの Apple は watchOS 3 に追加します。 次に、バック グラウンド API ライフ サイクルと Xamarin の watchOS アプリでバック グラウンド タスクを実装する方法について説明します。 最後に、スケジュールする方法を説明し、いくつかのベスト プラクティスを提供しました。



## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
