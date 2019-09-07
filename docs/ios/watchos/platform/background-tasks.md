---
title: watchOS のバックグラウンドタスク (Xamarin)
description: このドキュメントでは、Xamarin で watchOS を使用してバックグラウンドタスクを使用する方法、リソースを使用する方法、バックグラウンドタスクを実装する方法、スケジュール設定、ベストプラクティスなどについて説明します。
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/13/2017
ms.openlocfilehash: 0e0336e65532c4487e3ec8c1984b132544b5b547
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768658"
---
# <a name="watchos-background-tasks-in-xamarin"></a>watchOS のバックグラウンドタスク (Xamarin)

WatchOS 3 では、watch アプリで情報を最新の状態に保つには、主に次の3つの方法があります。 

- いくつかの新しいバックグラウンドタスクのいずれかを使用します。 
- ウォッチ式の複雑さの1つ (更新に余計な時間を与える)。 
- ユーザーが新しい Dock にアプリにピン留めする (メモリ内に保持され、頻繁に更新される)。 

## <a name="keeping-an-app-up-to-date"></a>アプリを最新の状態に保つ

開発者が watchOS アプリのデータとユーザーインターフェイスを最新かつ最新の状態に保つためのすべての方法について説明する前に、このセクションでは、使用パターンの一般的なセットと、次に基づいてユーザーが iPhone とその Apple Watch 間をどのように移動するかについて説明します。 時刻と、現在実行されているアクティビティ (運転など)。

次の例を参照してください。

[![](background-tasks-images/update00.png "ユーザーが1日中に iPhone とその Apple Watch 間を移動する方法")](background-tasks-images/update00.png#lightbox)

1. 朝には、コーヒーの行を待っている間、ユーザーは iPhone の現在のニュースを数分間参照します。
2. コーヒーショップを離れる前に、時計を簡単に確認してください。
3. 昼食する前に、iPhone の Maps アプリを使用して近くのレストランを検索し、クライアントを満たす予約を予約します。
4. レストランへの出張中には、Apple Watch についての通知が表示されます。また、昼食の予定が遅れていることを一目で確認できます。
5. 夜間には、iPhone の Maps アプリを使用して、ホームを運転する前にトラフィックを確認します。
6. 自宅では、iMessage Apple Watch 通知を受け取り、牛乳を選択するように指示します。また、クイック応答機能を使用して応答 "OK" を送信します。

ユーザーが Apple Watch アプリを使用する方法の "概要" (3 秒未満) のため、通常は、アプリが必要な情報をフェッチして、ユーザーに表示する前に UI を更新するのに十分な時間がありません。

Apple が watchOS 3 に追加した新しい Api を使用することにより、アプリは_バックグラウンド更新_のスケジュールを設定し、必要な情報をユーザーに要求する前に準備できます。 上記の気象の複雑な例を見てください。

[![](background-tasks-images/update01.png "天気の複雑な例")](background-tasks-images/update01.png#lightbox)

1. アプリは、特定の時刻にシステムによってウェイクアップされるようにスケジュールします。 
2. アプリでは、更新プログラムを生成するために必要な情報がフェッチされます。
3. 新しいデータを反映するために、アプリによってユーザーインターフェイスが再生成されます。
4. ユーザーがアプリの複雑さについての概要を説明すると、ユーザーが更新を待つ必要がなく、最新の情報が含まれています。

前述のように、watchOS システムは、使用可能なプールが非常に限られている1つ以上のタスクを使用してアプリをスリープ解除します。

[![](background-tasks-images/update02.png "WatchOS システムは、1つまたは複数のタスクを使用してアプリをスリープ解除します")](background-tasks-images/update02.png#lightbox)

Apple は、アプリがそれ自体を更新するプロセスを完了するまで、このタスクを保持することによって (アプリに限定されたリソースであるため)、このタスクを最大限に活用することを提案します。

システムは、 `HandleBackgroundTasks` `WKExtensionDelegate`デリゲートの新しいメソッドを呼び出すことによってこれらのタスクを提供します。 例えば:

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

アプリは、指定されたタスクを完了すると、完了をマークしてシステムに返します。

[![](background-tasks-images/update03.png "完了をマークすることで、タスクがシステムに戻る")](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>新しいバックグラウンドタスク

watchOS 3 では、アプリを開く前にユーザーが必要とするコンテンツがあることを確認するために、アプリで使用できるいくつかのバックグラウンドタスクについて説明します。次に例を示します。

- **バックグラウンドアプリの更新**- [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask)タスクを使用すると、アプリの状態をバックグラウンドで更新できます。 通常、これには、 [Nq&a Lsession](https://developer.apple.com/reference/foundation/nsurlsession)を使用したインターネットからの新しいコンテンツのダウンロードなど、別のタスクが含まれます。
- **バックグラウンドスナップショット更新**- [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask)タスクを使用すると、アプリケーションは、ドッキングの設定に使用されるスナップショットを取得する前に、コンテンツと UI の両方を更新できます。
- **バックグラウンドウォッチ接続**-ペアになっている iPhone からバックグラウンドデータを受信すると、アプリの[WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask)タスクが開始されます。
- **バックグラウンド URL セッション**-バックグラウンド転送に承認が必要な場合や、(正常にまたはエラーが発生したときに) 完了した場合に、アプリの[WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask)タスクが開始されます。

これらのタスクについては、以下のセクションで詳しく説明します。

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

は`WKApplicationRefreshBackgroundTask` 、アプリを将来の日付でウェイクアップするようにスケジュールできる汎用タスクです。

[![](background-tasks-images/update04.png "将来の日付で WKApplicationRefreshBackgroundTask がウェイクアップ")](background-tasks-images/update04.png#lightbox)

タスクの実行時に、アプリは、複雑なタイムラインを更新したり、を`NSUrlSession`使用して必要なデータを取得したりするなど、あらゆる種類のローカル処理を実行できます。

<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

データのダウンロードが完了`WKURLSessionRefreshBackgroundTask`し、アプリで処理できる状態になると、システムからが送信されます。

[![](background-tasks-images/update05.png "データのダウンロードが完了したときの WKURLSessionRefreshBackgroundTask")](background-tasks-images/update05.png#lightbox)

データがバックグラウンドでダウンロードされている間、アプリは実行されません。 代わりに、アプリはデータの要求をスケジュールし、中断された後、データのダウンロードを処理します。ダウンロードが完了すると、アプリの reawakening のみを行います。

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

WatchOS 3 では、Apple がドッキングを追加し、ユーザーがお気に入りのアプリをピン留めしてすぐにアクセスできるようにしました。 ユーザーが Apple Watch の横にあるボタンを押すと、ピン留めされたアプリスナップショットのギャラリーが表示されます。 ユーザーは、左または右にスワイプして目的のアプリを見つけ、アプリをタップして起動し、スナップショットを実行中のアプリのインターフェイスに置き換えることができます。

[![](background-tasks-images/update06.png "実行中のアプリのインターフェイスにスナップショットを置き換える")](background-tasks-images/update06.png#lightbox)

システムは、(を`WKSnapshotRefreshBackgroundTask`送信することによって) アプリの UI のスナップショットを定期的に取得し、それらのスナップショットを使用してドックにデータを設定します。 watchOS は、このスナップショットを取得する前に、アプリにコンテンツと UI を更新する機会を与えます。

スナップショットは、アプリのプレビューイメージと起動イメージの両方として機能するため、watchOS 3 では非常に重要です。 ドック内のアプリでユーザーがを終了すると、全画面に展開され、フォアグラウンドに入り、実行が開始されるため、スナップショットを最新の状態にする必要があります。

[![](background-tasks-images/update07.png "ユーザーがドッキング中のアプリでの作業を行うと、[全画面表示] に展開されます。")](background-tasks-images/update07.png#lightbox)

この場合も、システムによっ`WKSnapshotRefreshBackgroundTask`てが発行されます。これにより、アプリはスナップショットが作成される前に (データと UI を更新して) 準備できるようになります。

[![](background-tasks-images/update08.png "アプリは、スナップショットが作成される前にデータと UI を更新することによって準備できます。")](background-tasks-images/update08.png#lightbox)

アプリがを`WKSnapshotRefreshBackgroundTask`完了すると、システムは自動的にアプリの UI のスナップショットを取得します。

> [!IMPORTANT]
> アプリが新しいデータを受信し`WKSnapshotRefreshBackgroundTask`てユーザーインターフェイスを更新した後にを常にスケジュールすることが重要です。そうしないと、変更された情報がユーザーに表示されません。

さらに、ユーザーがアプリから通知を受信し、タップしてアプリをフォアグラウンドにすると、起動画面としても動作しているため、スナップショットを最新の状態にする必要があります。

[![](background-tasks-images/update09.png "ユーザーはアプリから通知を受け取り、タップしてアプリを前景に移動します。")](background-tasks-images/update09.png#lightbox)

ユーザーが watchOS アプリと対話してから1時間以上経過している場合は、既定の状態に戻ることができます。 既定の状態は、アプリによって異なることを意味します。アプリの設計によっては、既定の状態がまったくない場合があります。

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

WatchOS 3 では、Apple は新しい`WKWatchConnectivityRefreshBackgroundTask`を通じて、バックグラウンド更新 API との監視接続を統合しています。 この新機能を使用すると、iPhone アプリは、watchOS アプリがバックグラウンドで実行されている間、watch アプリに新しいデータを配信できます。

[![](background-tasks-images/update10.png "IPhone アプリでは、watchOS アプリがバックグラウンドで実行されている間、対応する watch アプリに新しいデータを配信できます。")](background-tasks-images/update10.png#lightbox)

複雑なプッシュ、アプリコンテキスト、iPhone アプリからのユーザー情報の更新を開始すると、Apple Watch アプリがバックグラウンドでスリープ解除されます。

Watch アプリがに`WKWatchConnectivityRefreshBackgroundTask`よってウェイクアップされた場合、iPhone アプリからデータを受信するには、標準の API メソッドを使用する必要があります。

[![](background-tasks-images/update11.png "WKWatchConnectivityRefreshBackgroundTask データフロー")](background-tasks-images/update11.png#lightbox)

1. セッションがアクティブになっていることを確認します。
2. 新しい`HasContentPending`プロパティを監視します。値が`true`の場合は、アプリに処理するデータが残っています。 前と同様に、アプリはすべてのデータの処理が完了するまで、タスクを保持する必要があります。
3. 処理するデータがなくなった場合 (`HasContentPending = false`)、タスクが完了したことをシステムに返すようにマークします。 これを行わないと、アプリに割り当てられたバックグラウンドランタイムが枯渇し、クラッシュレポートが生成されます。

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>バックグラウンド API のライフサイクル

新しいバックグラウンドタスク API のすべての部分を一緒に配置すると、一般的な一連の対話は次のようになります。

[![](background-tasks-images/update12.png "バックグラウンド API のライフサイクル")](background-tasks-images/update12.png#lightbox)

1. まず、watchOS アプリでは、バックグラウンドタスクが将来のある時点としてスリープ状態されるようにスケジュールします。
2. アプリはシステムによってウェイクアップされ、タスクが送信されます。
3. アプリは、必要な作業を完了するためにタスクを処理します。
4. タスクを処理した結果として、アプリでは、を`NSUrlSession`使用してより多くのコンテンツをダウンロードするなど、今後より多くの作業を完了するために、より多くのバックグラウンドタスクをスケジュールする必要がある場合があります。
5. アプリによってタスクが完了したとマークされ、システムに返されます。

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>リソースを責任を持って使用する

WatchOS アプリは、システムの共有リソースのドレインを制限することで、このエコシステム内で確実に動作することが重要です。

次のシナリオを見てみましょう。

[![](background-tasks-images/update13.png "WatchOS アプリは、システムの共有リソースに対するドレインを制限します。")](background-tasks-images/update13.png#lightbox)

1. ユーザーは、午後1:00 に watchOS アプリを起動します。
2. アプリは、午後2:00 時に、新しいコンテンツをウェイクアップしてダウンロードするタスクをスケジュールします。
3. 午後1:50 に、ユーザーはアプリを再度開いて、この時点でデータと UI を更新することができます。
4. タスクが10分後に再びアプリをスリープ解除するのではなく、アプリはタスクを再スケジュールして、2:50 PM に1時間後に実行するようにします。

すべてのアプリは異なりますが、Apple では、システムリソースを節約するために、上記のような使用パターンを見つけることが提案されています。

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>バックグラウンドタスクの実装

たとえば、このドキュメントでは、サッカーのスコアをユーザーに報告するフェイク MonkeySoccer スポーツアプリを使用します。 

一般的な使用シナリオを次に示します。

[![](background-tasks-images/update14.png "一般的な使用シナリオ")](background-tasks-images/update14.png#lightbox)

ユーザーのお気に入りのサッカーチームは、7:00 PM から 9:00 PM までの大きな一致を再生しているので、アプリはユーザーがスコアを定期的にチェックし、30分の更新間隔を決定します。

1. ユーザーはアプリを開き、バックグラウンド更新のタスクを30分後にスケジュールします。 バックグラウンド API では、一度に実行できるバックグラウンドタスクの種類は1つだけです。
2. アプリはタスクを受信し、そのデータと UI を更新した後、30分後に別のバックグラウンドタスクをスケジュールします。 開発者が別のバックグラウンドタスクをスケジュールすることを記憶しているか、アプリが再 awoken されて更新が増えることはありません。
3. ここでも、アプリはタスクを受け取り、そのデータを更新し、UI を更新して、30分後に別のバックグラウンドタスクをスケジュールします。
4. 同じプロセスが再度繰り返されます。
5. 最後のバックグラウンドタスクが受信され、そのデータと UI が再びアプリによって更新されます。 これは最終的なスコアであるため、新しいバックグラウンド更新のスケジュールは設定しません。 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>バックグラウンド更新のスケジュール設定

上記のシナリオでは、MonkeySoccer アプリで次のコードを使用して、バックグラウンド更新のスケジュールを設定できます。

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

アプリの awoken が`NSDate`必要になったときに新しい30分が作成され、要求`NSMutableDictionary`されたタスクの詳細を保持するためのが作成されます。 `ScheduleBackgroundRefresh` のメソッドは、タスクをスケジュールするように要求するために`SharedExtension`使用されます。

要求されたタスク`NSError`をスケジュールできなかった場合、システムはを返します。

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>更新を処理しています

次に、スコアを更新するために必要な手順を示す5分のウィンドウについて詳しく見てみましょう。

[![](background-tasks-images/update15.png "スコアを更新するために必要な手順を示す5分のウィンドウ")](background-tasks-images/update15.png#lightbox)

1. 午後7:30:02 に、システムによってアプリがスリープ解除され、更新プログラムのバックグラウンドタスクが付与されます。 最初の優先順位は、サーバーから最新のスコアを取得することです。 後述[の「nq&a をスケジュールする」を](#Scheduling-a-NSUrlSession)参照してください。
2. 7:30:05 では、アプリが元のタスクを完了すると、システムはアプリをスリープ状態にし、要求されたデータをバックグラウンドでダウンロードし続けます。
3. システムがダウンロードを完了すると、ダウンロードした情報を処理できるように、アプリをスリープ解除する新しいタスクが作成されます。 [バックグラウンドタスクの処理](#Handling-Background-Tasks)と以下[のダウンロードの処理に](#Handling-the-Download-Completing)関するページを参照してください。 
4. アプリは更新された情報を保存し、タスクが完了したことをマークします。 この時点で、開発者はアプリのユーザーインターフェイスを更新することが必要になる場合がありますが、Apple では、そのプロセスを処理するようにスナップショットタスクをスケジュールすることが提案されています。 「[スナップショット更新のスケジュール」を](#Scheduling-a-Snapshot-Update)参照してください。
5. アプリはスナップショットタスクを受信し、そのユーザーインターフェイスを更新して、タスクが完了したことをマークします。 「[スナップショットの更新の処理」を](#Handling-a-Snapshot-Update)参照してください。

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>Nq&a Lsession のスケジュール設定

次のコードを使用して、最新のスコアのダウンロードをスケジュールすることができます。

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

新しい`NSUrlSession`を構成して作成し、そのセッションを使用して、 `CreateDownloadTask`メソッドを使用して新しいダウンロードタスクを作成します。 ダウンロードタスクの`Resume`メソッドを呼び出して、セッションを開始します。

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>バックグラウンドタスクの処理

`HandleBackgroundTasks`のメソッドをオーバーライドすることにより、アプリは受信バックグラウンドタスクを処理できます。`WKExtensionDelegate`

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

メソッド`HandleBackgroundTasks`は、システムがを検索するため`WKUrlSessionRefreshBackgroundTask`にアプリに送信したすべての`backgroundTasks`タスク (では) を順番に実行します。 見つかった場合は、セッションに再度参加し、ダウンロード`NSUrlSessionDownloadDelegate`の完了を処理するためのをアタッチします (以下[のダウンロードの処理](#Handling-the-Download-Completing)に関する説明を参照してください)。

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

コレクションに追加することによって完了するまで、タスクのハンドルを保持します。

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

現在処理されていないタスクについては、アプリに送信されるすべてのタスクが完了していることを示す必要があります。

```csharp
if (urlTask != null) {
  ...
} else {
  // Ensure that all tasks are completed
  task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing" />

## <a name="handling-the-download-completing"></a>ダウンロードの完了の処理

Monkeysoccer アプリでは、次`NSUrlSessionDownloadDelegate`のデリゲートを使用して、要求されたデータのダウンロードを完了し、処理します。

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

初期化されると、 `ExtensionDelegate`とそれを生成したの`WKRefreshBackgroundTask`両方へのハンドルを保持します。 `DidFinishDownloading`メソッドをオーバーライドして、ダウンロードの完了を処理します。 は、の`CompleteTask`メソッドを使用`ExtensionDelegate`して、タスクが完了したことを通知し、保留中のタスクのコレクションから削除します。 前の「[バックグラウンドタスクの処理](#Handling-Background-Tasks)」を参照してください。

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>スナップショット更新のスケジュール設定

次のコードを使用して、最新のスコアで UI を更新するようにスナップショットタスクをスケジュールすることができます。

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

上記の`ScheduleURLUpdateSession`メソッドと同様に、アプリの`NSDate` awoken `NSMutableDictionary`が必要になったときにの新しいを作成し、要求されたタスクの詳細を保持するを作成します。 `ScheduleSnapshotRefresh` のメソッドは、タスクをスケジュールするように要求するために`SharedExtension`使用されます。

要求されたタスク`NSError`をスケジュールできなかった場合、システムはを返します。

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>スナップショット更新の処理

スナップショットタスク`HandleBackgroundTasks`を処理するには、メソッド (上記の[バックグラウンドタスクの処理](#Handling-Background-Tasks)に関するページを参照) を次のように変更します。

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

メソッドは、処理されているタスクの種類をテストします。 次の場合は`WKSnapshotRefreshBackgroundTask` 、タスクにアクセスできます。

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

メソッドは、ユーザーインターフェイスを更新した`NSDate`後、スナップショットが古くなったことをシステムに通知するを作成します。 新しいスナップショットを`NSMutableDictionary`記述するユーザー情報を持つを作成し、この情報を使用して完了したスナップショットタスクをマークします。

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

また、アプリが既定の状態 (最初のパラメーター) に戻らないことをスナップショットタスクに通知します。 既定の状態の概念がないアプリでは、常にこのプロパティ`true`をに設定する必要があります。

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>効率的な作業

前の例では、MonkeySoccer アプリによってスコアが更新されました。これは、効率的に作業し、新しい watchOS 3 バックグラウンドタスクを使用することにより、アプリは合計15秒間のみアクティブでした。 

[![](background-tasks-images/update16.png "アプリは合計15秒間アクティブでした")](background-tasks-images/update16.png#lightbox)

これにより、アプリが使用可能な Apple Watch リソースとバッテリ寿命の両方に与える影響が少なくなります。また、アプリは、ウォッチ中に実行されている他のアプリとの連携を向上させることができます。

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>スケジュール設定のしくみ

WatchOS 3 アプリがフォアグラウンドにある間は、常に実行がスケジュールされ、データの更新や UI の再描画など、必要なあらゆる種類の処理を実行できます。 アプリがバックグラウンドに移行すると、通常、システムによって中断され、すべてのランタイム操作が停止します。 

アプリがバックグラウンドで実行されている間、特定のタスクをすばやく実行するためにシステムの対象となる場合があります。 そのため、watchOS 2 では、システムがバックグラウンドアプリを一時的にスリープ解除して、長い外観の通知を処理したり、アプリの複雑さを更新したりします。 WatchOS 3 では、バックグラウンドでアプリを実行するための新しい方法がいくつかあります。

アプリがバックグラウンドで実行されている間、システムにはいくつかの制限があります。

- 特定のタスクを完了するのに数秒しか与えられません。 システムは、渡される時間だけでなく、アプリがこの制限を導き出すために使用している CPU 電力の量も考慮します。
- 制限を超えたすべてのアプリは、次のエラーコードを使用して強制終了されます。
  - **CPU** - 0xc51bad01
  - **時間**-0xc51bad02
- システムは、アプリで実行するように要求されたバックグラウンドタスクの種類に基づいて、異なる制限を課します。 たとえば、タスク`WKApplicationRefreshBackgroundTask`と`WKURLSessionRefreshBackgroundTask`タスクには、他の種類のバックグラウンドタスクよりもやや長いランタイムが指定されています。

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>複雑さとアプリの更新

Apple が watchOS 3 に追加した新しいバックグラウンドタスクに加えて、watchOS アプリの複雑さは、アプリがバックグラウンド更新を受信する方法とタイミングに影響を与える可能性があります。

複雑さは、役に立つ情報を一目で示す小さな視覚要素です。 選択したウォッチフェイスに応じて、watchOS 3 の watch アプリによって提供される1つまたは複数の複雑なウォッチ式を使用して、ウォッチフェイスをカスタマイズできます。

ユーザーがウォッチ顔にアプリの複雑さの1つを含めると、アプリに次のような更新された特典が付与されます。

- これにより、システムはアプリを起動可能な状態に保ちます。この状態では、アプリがバックグラウンドで起動され、メモリに保持され、更新に余分な時間が与えられます。
- 複雑さは、1日に50以上のプッシュ更新が保証されることを保証します。

開発者は、ユーザーが上記の理由でユーザーをウォッチ式に追加することをユーザーに誘導するために、常に説得力のあるアプリの複雑さを作る必要があります。

WatchOS 2 では、アプリがバックグラウンドでランタイムを受信した主な方法は複雑でした。 WatchOS 3 では、1時間に複数の更新プログラムを受信するように、複雑なアプリが引き続き`WKExtensions`保証されますが、を使用して、その複雑さを更新するためにより多くのランタイムを要求することができます。

接続されている iPhone アプリからの複雑さを更新するために使用される次のコードを見てみましょう。

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

の`RemainingComplicationUserInfoTransfers` プロパティ`WCSession`を使用して、アプリがその日に残された高優先度転送の数を確認し、その数に基づいてアクションを実行します。 アプリが低レベルの転送の実行を開始すると、マイナー更新を送信しても、重要な変更がある場合にのみ情報を送信することができます。

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>スケジュールとドック

WatchOS 3 では、Apple がドッキングを追加し、ユーザーがお気に入りのアプリをピン留めしてすぐにアクセスできるようにしました。 ユーザーが Apple Watch の横にあるボタンを押すと、ピン留めされたアプリスナップショットのギャラリーが表示されます。 ユーザーは、左または右にスワイプして目的のアプリを見つけ、アプリをタップして起動し、スナップショットを実行中のアプリのインターフェイスに置き換えることができます。

[![](background-tasks-images/dock01.png "Dock")](background-tasks-images/dock01.png#lightbox)

システムは、アプリの UI のスナップショットを定期的に取得し、それらのスナップショットを使用してドキュメントを作成します。watchOS は、このスナップショットを取得する前に、アプリにコンテンツと UI を更新する機会を与えます。

Dock にピン留めされているアプリは、次のことを想定できます。

- 少なくとも1時間に1回更新されます。 これには、アプリの更新タスクとスナップショットタスクの両方が含まれます。
- 更新の予算は、Dock 内のすべてのアプリ間で配布されます。 そのため、ユーザーがピン留めしたアプリの数が減り、アプリごとに更新される可能性が高くなります。
- アプリはメモリに保持されるため、Dock から選択したときにアプリがすぐに再開されます。

ユーザーが最後に実行したアプリは、_最近使用_したアプリと見なされ、Dock の最後のスロットが占有されます。 そこから、ユーザーはドッキングに固定することを選択できます。 最近使用したは、ユーザーが既にドックにピン留めされている他のお気に入りアプリと同様に扱われます。

> [!IMPORTANT]
> ホーム画面に追加されただけのアプリには、定期的なスケジュールは付与されません。 定期的なスケジュール設定とバックグラウンド更新を受け取るには、アプリを Dock に追加する_必要があり_ます。

このドキュメントで既に説明したように、スナップショットは、アプリのプレビューイメージと起動イメージの両方として機能するため、watchOS 3 では非常に重要です。 ユーザーがドッキング中のアプリでを終了すると、[全画面表示] に展開され、フォアグラウンドに入り、実行が開始されるため、スナップショットを最新の状態にする必要があります。

アプリケーションの UI の新しいスナップショットが必要であると判断した場合があります。 このような状況では、スナップショット要求はアプリの実行時の予算に対してカウントされません。 システムスナップショット要求は、次のようにトリガーされます。

- 複雑なタイムライン更新。
- アプリの通知を使用したユーザー操作。
- フォアグラウンドからバックグラウンド状態に切り替えます。
- 1時間後にバックグラウンド状態になると、アプリは既定の状態に戻ることができます。
- WatchOS が最初に起動したとき。

<a name="Best-Practices" />

## <a name="best-practices"></a>ベスト プラクティス 

Apple では、バックグラウンドタスクを操作するときに、次のベストプラクティスを提案しています。

- アプリを更新する必要がある頻度でスケジュールします。 アプリを実行するたびに、将来のニーズを再評価し、必要に応じてこのスケジュールを調整する必要があります。
- システムがバックグラウンド更新タスクを送信し、アプリが更新を必要としない場合は、更新が実際に必要になるまで作業を延期します。
- アプリで利用できるすべてのランタイムの営業案件を検討します。
  - Dock とフォアグラウンドのアクティブ化。
  - 警告.
  - 複雑になった更新。
  - バックグラウンド更新。
- 次`ScheduleBackgroundRefresh`のような汎用のバックグラウンドランタイムに使用します。
  - システムに情報をポーリングしています。
  - バックグラウンドデータ`NSURLSessions`を要求するように未来をスケジュールします。 
  - 既知の時間遷移。
  - 複雑になった更新をトリガーしています。

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>スナップショットのベストプラクティス

スナップショット更新を使用する場合、Apple は次の提案を行います。

- は、必要な場合にのみスナップショットを無効にします。たとえば、コンテンツを大幅に変更する場合などです。
- 高頻度のスナップショットの無効化は避けてください。 たとえば、タイマーアプリはスナップショットを毎秒更新しないようにする必要があります。これは、タイマーが終了したときにのみ実行します。

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>アプリデータフロー

Apple では、データフローを操作するために次のことを提案しています。

[![](background-tasks-images/update17.png "アプリデータフローダイアグラム")](background-tasks-images/update17.png#lightbox)

外部イベント (Watch の接続など) によってアプリがウェイクアップされます。 これにより、アプリはそのデータモデルを強制的に更新します (アプリの現在の状態を表します)。 データモデルの変更の結果、アプリの複雑さを更新し、新しいスナップショットを要求する必要があります。バックグラウンド`NSURLSession`を起動して、より多くのデータをプルし、バックグラウンド更新をスケジュールすることができます。

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>アプリのライフサイクル

ドッキングとお気に入りのアプリをピン留めする機能があるため、Apple は、ユーザーがはるかに多くのアプリ間を移動することを認識しています。これは watchOS 2 で行われました。 その結果、アプリはこの変更を処理し、フォアグラウンドとバックグラウンドの状態をすばやく移動できるようになります。

Apple には次のような提案があります。

- フォアグラウンドアクティベーションを入力したときに、できるだけ早く、アプリがバックグラウンドタスクを完了するようにします。
- を呼び出し`NSProcessInfo.PerformExpiringActivity`て背景を入力する前に、すべてのフォアグラウンド作業を完了していることを確認します。
- WatchOS シミュレーターでアプリをテストする場合、アプリが機能を適切にテストするために必要な量だけ更新できるように、どのタスク予算も適用されません。
- ITunes Connect に発行する前に、アプリが予算を超えて実行されていないことを確認するために、実際の Apple Watch ハードウェアで常にテストしてください。
- Apple は、テストとデバッグの際に、充電時の Apple Watch を維持することを提案します。
- コールド起動とアプリの再開の両方が十分にテストされていることを確認します。
- すべてのアプリタスクが完了していることを確認します。
- ドッキングにピン留めされているアプリの数を変更して、最良と最悪の両方のシナリオをテストします。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、Apple が watchOS に加えた拡張機能と、それらを使用して監視アプリを最新の状態に保つ方法について説明しました。 まず、Apple によって watchOS 3 に追加された新しいバックグラウンドタスクのすべてについて説明しました。 次に、バックグラウンド API のライフサイクルと、Xamarin watchOS アプリにバックグラウンドタスクを実装する方法について説明します。 最後に、スケジューリングのしくみとベストプラクティスについて説明しました。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
