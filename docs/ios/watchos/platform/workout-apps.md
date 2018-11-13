---
title: watchOS で Xamarin トレーニング アプリ
description: この記事が、拡張機能では Apple は watchOS 3 と Xamarin で実装する方法でアプリをトレーニングするしました。
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: fd677aa802adf32ac81396f81c67264d88639967
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528781"
---
# <a name="watchos-workout-apps-in-xamarin"></a>watchOS で Xamarin トレーニング アプリ

_この記事が、拡張機能では Apple は watchOS 3 と Xamarin で実装する方法でアプリをトレーニングするしました。_


新しい watchOS 3、トレーニングに関連するアプリには、Apple Watch をバック グラウンドで実行し、HealthKit のデータにアクセスできます。 その親 iOS 10 ベースのアプリでは、ユーザーの介入なし watchOS 3 ベースのアプリを起動する機能もあります。

次のトピックで、詳しく説明します。

## <a name="about-workout-apps"></a>トレーニング アプリについて

適合性およびトレーニングのアプリのユーザーは、正常性および適合性の目標に向けての日付のいくつかの時間を設けること、高専用であることができます。 その結果、正確に収集し、データを表示し、Apple の正常性とシームレスに統合するための応答性の高い、使いやすいアプリに期待されます。

適切に設計されたへの適合またはトレーニングのアプリを使用して、グラフの適合性、目標を達成するには、そのアクティビティやすくなります。 Apple Watch を使用すると、アプリへの適合性とトレーニング アプリがある心拍数への即時アクセス カロリー書き込みおよびアクティビティの検出。

[![](workout-apps-images/workout01.png "アプリの適合性およびトレーニングの例")](workout-apps-images/workout01.png#lightbox)

Watchos 3、新しい_バック グラウンドで実行されている_によりトレーニング関連アプリを Apple Watch 上のバック グラウンドで実行し、HealthKit のデータにアクセスできます。

このドキュメントはバック グラウンドで実行されている機能が導入、トレーニング アプリのライフ サイクルについて説明およびトレーニング アプリにどのようにユーザーの貢献を表示する_アクティビティ リング_Apple Watch にします。

## <a name="about-workout-sessions"></a>トレーニング セッションについて

すべてのアプリにトレーニングの中核部分は、_トレーニング セッション_(`HKWorkoutSession`) をユーザーが開始および停止できます。 トレーニング セッションの API は、簡単に実装できるなど、トレーニングのアプリにいくつかの利点を提供します。

- モーション センサーとカロリー、アクティビティの種類に基づく検出に書き込みます。
- ユーザーのアクティビティのリングを自動投稿します。
- セッション中に、アプリケーションが自動的に表示されます、ユーザー デバイス (、手首を発生させる、または Apple Watch との対話をか) を再開するたびにします。

## <a name="about-background-running"></a>バック グラウンド実行する方法

前述のように、watchOS 3 では、トレーニング アプリに設定できます、バック グラウンドで実行します。 トレーニング アプリを実行するバック グラウンドを使用して、バック グラウンドで実行中に、Apple Watch のセンサーからのデータを処理できます。 たとえば、アプリを引き続き監視ユーザーの心拍数、場合でも、画面が表示されなくします。

バック グラウンド実行には、ユーザーに、現在の進行状況のユーザーに通知するハプティクス通知を送信するなど、アクティブなトレーニング セッション中にいつでもライブのフィードバックを提供する機能も用意されています。

さらに、バック グラウンドで実行されていると、アプリに、すばやく、Apple Watch の概要と、ユーザーがある最新のデータのために、ユーザー インターフェイスをすばやく更新が許可します。

Apple Watch で高いパフォーマンスを維持するためにバック グラウンドで実行されているを使用して watch アプリは、バッテリを節約するためにバック グラウンド作業の量を制限する必要があります。 アプリがバック グラウンドで過剰な CPU を使用している場合は、watchOS によって中断を取得できます。

### <a name="enabling-background-running"></a>バック グラウンド実行を有効にします。

バック グラウンド実行を有効にするには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、ウォッチ拡張機能のコンパニオン iPhone アプリをダブルクリックして`Info.plist`ファイルを開き、編集します。
2. 切り替えて、**ソース**ビュー。 

    [![](workout-apps-images/plist01.png "ソース ビュー")](workout-apps-images/plist01.png#lightbox)
3. 呼ばれる新しいキーを追加`WKBackgroundModes`設定と、**型**に`Array`: 

    [![](workout-apps-images/plist02.png "WKBackgroundModes と呼ばれる新しいキーを追加します。")](workout-apps-images/plist02.png#lightbox)
4. 配列に新しい項目を追加、**型**の`String`の値と`workout-processing`: 

    [![](workout-apps-images/plist03.png "文字列型とトレーニング処理の値が配列に新しい項目を追加します。")](workout-apps-images/plist03.png#lightbox)
5. 変更内容をファイルに保存します。

## <a name="starting-a-workout-session"></a>トレーニング セッションの開始

これには、トレーニング セッションを開始する 3 つの主な手順があります。

[![](workout-apps-images/workout02.png "トレーニング セッションを開始する 3 つの主な手順")](workout-apps-images/workout02.png#lightbox)

1. アプリでは、HealthKit のデータにアクセスするための承認を要求する必要があります。
2. 開始されているトレーニングの種類のトレーニング構成オブジェクトを作成します。
3. 作成し、新しく作成されたトレーニング構成を使用してトレーニング セッションを開始します。

### <a name="requesting-authorization"></a>承認を要求します。

アプリは、HealthKit のユーザーのデータにアクセスできる、前に、要求し、ユーザーからの受信認証にする必要があります。 トレーニング アプリの性質によって、次の種類の要求の必要があります。

- データを書き込む許可。
    - ワークアウト
- データを読み取ることの承認:
    - 燃焼されるエネルギー
    - 距離
    - 心拍数  

アプリは承認を要求できる、HealthKit にアクセスするように構成する必要があります。

次の手順で行います。

1. **ソリューション エクスプローラー**で `Entitlements.plist` ファイルをダブルクリックして、編集用に開きます。
2. 下部までスクロールし、確認**HealthKit を有効にする**: 

    [![](workout-apps-images/auth01.png "HealthKit を有効にするを確認してください。")](workout-apps-images/auth01.png#lightbox)
3. 変更内容をファイルに保存します。
4. 指示に従って、[明示的なアプリ ID とプロビジョニング プロファイル](~/ios/platform/healthkit.md)と[アプリ ID とプロビジョニング プロファイルで Xamarin.iOS アプリを関連付ける](~/ios/platform/healthkit.md)のセクションでは、[の概要HealthKit](~/ios/platform/healthkit.md)に関する記事を正しくにアプリをプロビジョニングします。
5. 手順を最後に、使用、[ヘルス キットのプログラミング](~/ios/platform/healthkit.md)と[、ユーザーからアクセス許可を要求する](~/ios/platform/healthkit.md)のセクション、 [HealthKit の概要](~/ios/platform/healthkit.md)要求への記事ユーザーの HealthKit データストアへのアクセスを承認します。

### <a name="setting-the-workout-configuration"></a>トレーニングの構成を設定

トレーニングの構成オブジェクトを使用してトレーニング セッションが作成されます (`HKWorkoutConfiguration`) トレーニングの種類を指定する (など`HKWorkoutActivityType.Running`) とトレーニングの場所 (など`HKWorkoutSessionLocationType.Outdoor`)。

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
    ActivityType = HKWorkoutActivityType.Running,
    LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>トレーニング セッション デリゲートを作成します。 

トレーニング セッション中に発生するイベントを処理するために、アプリは、トレーニング セッション デリゲート インスタンスを作成する必要があります。 新しいクラスをプロジェクトに追加し、ログオフ、`HKWorkoutSessionDelegate`クラス。 屋外の実行の例では、次のようなことになります。

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }
        #endregion
    }
}
```

このクラスは、トレーニング セッションの変更の状態が発生するいくつかのイベントを作成します (`DidChangeToState`) し、トレーニング セッションが失敗した場合 (`DidFail`)。 

### <a name="creating-a-workout-session"></a>トレーニング セッションを作成します。

新しいトレーニング セッションを作成し、ユーザーの既定の HealthKit ストアに対して起動する作成トレーニング構成およびトレーニング セッションのデリゲートを使用します。

```csharp
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...

private void StartOutdoorRun ()
{
    // Create a workout configuration
    var configuration = new HKWorkoutConfiguration () {
        ActivityType = HKWorkoutActivityType.Running,
        LocationType = HKWorkoutSessionLocationType.Outdoor
    };

    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (configuration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

アプリは、このトレーニング セッションを開始、ウォッチの文字盤に切り替える場合は、表面上、小さなの緑色の「実行中の man」アイコンが表示されます。

[![](workout-apps-images/workout03.png "小さな緑実行中のマニュアルのアイコン イメージ上に表示されます。")](workout-apps-images/workout03.png#lightbox)

ユーザーがこのアイコンをタップした場合は、アプリにバックアップが表示されます。

## <a name="data-collection-and-control"></a>データの収集とコントロール

トレーニング セッションが構成されている、起動されると、アプリは、ユーザーの心拍数) などのセッションに関するデータを収集およびセッションの状態を制御する必要があります。

[![](workout-apps-images/workout04.png "データの収集とコントロールの図")](workout-apps-images/workout04.png#lightbox)

1. **サンプルを観察**-アプリが処理およびユーザーに表示される HealthKit から情報を取得する必要があります。
2. **イベントを観察**-アプリは HealthKit によって、または (など、ユーザーは、トレーニングを一時停止)、アプリの UI から生成されるイベントに応答する必要があります。
3. **実行中の状態を入力します。** -セッションが開始されており、現在実行中です。
4. **一時停止状態に入る**-ユーザーが現在のトレーニング セッションが一時停止し、後で再起動できます。 ユーザーは、単一のトレーニング セッションで複数回実行され、一時停止中の状態の間で切り替えることがあります。
5. **トレーニング セッションを終了**任意の時点で、ユーザーがトレーニング セッションを終了できます - または有効期限が切れるし、従量制課金のトレーニング (2 マイルの実行) などがあった場合、独自の終了が可能性があります。

最後の手順では、ユーザーの HealthKit のデータストアにトレーニング セッションの結果を保存します。

### <a name="observing-healthkit-samples"></a>HealthKit のサンプルの観察

アプリを開く必要があります、_アンカー オブジェクト クエリ_HealthKit データの各ポイントすることで関心のある心拍数またはアクティブなエネルギー量の書き込みなど。 監視対象のデータ ポイントごとに、更新ハンドラーは、アプリに送信されるので、新しいデータをキャプチャを作成する必要があります。

これらのデータ ポイントからは、アプリは (合計実行の距離など) の合計を蓄積しのユーザー インターフェイスを更新として必要です。 さらに、特定の目標または実行の次の工程の完了などの達成に到達したときに、アプリはユーザーに通知できます。

次のサンプル コードを参照してください。

```csharp
private void ObserveHealthKitSamples ()
{
    // Get the starting date of the required samples
    var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

    // Get data from the local device
    var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
    var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

    // Assemble compound predicate
    var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

    // Get ActiveEnergyBurned
    var queryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
        // Valid?
        if (error == null) {
            // Yes, process all returned samples
            foreach (HKSample sample in addedObjects) {
                var quantitySample = sample as HKQuantitySample;
                ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);
            }
            
            // Update User Interface
            ...
        }
    });

    // Start Query
    HealthStore.ExecuteQuery (queryActiveEnergyBurned);
                                          
}
```

使用してデータを取得する開始日を設定する述語を作成、`GetPredicateForSamples`メソッド。 HealthKit の情報を使用してからプルするデバイスのセットを作成、`GetPredicateForObjectsFromDevices`メソッドは、ここでローカルの Apple Watch のみ (`HKDevice.LocalDevice`)。 2 つの述語は、述語の複合に結合されます (`NSCompoundPredicate`) を使用して、`CreateAndPredicate`メソッド。

新しい`HKAnchoredObjectQuery`必要なデータ ポイントが作成されます (ここで`HKQuantityTypeIdentifier.ActiveEnergyBurned`Active エネルギー書き込んだデータ ポイントの)、返されるデータの量に制限は適用されません (`HKSampleQuery.NoLimit`) し、データがアプリに戻り値を処理するために、更新ハンドラーが定義されています。HealthKit から。 

更新ハンドラーはいつでも、特定のデータ ポイントのアプリに新しいデータが配信と呼ばれます。 エラーが返されない場合、アプリできます安全にデータの読み取り、要求された計算を行うおよび必要に応じてその UI を更新します。

ループしてすべてのサンプル コード (`HKSample`) で返される、`addedObjects`配列し、数量のサンプルではキャスト (`HKQuantitySample`)。 Joule としてサンプルの double 値を取得し、(`HKUnit.Joule`) と、トレーニング用、燃焼されるエネルギーの作業中の集計の途中に累計およびユーザー インターフェイスを更新します。

### <a name="achieved-goal-notification"></a>達成目標通知

前述のように、ユーザー (など、完了すると、実行の最初のマイル)、トレーニングのアプリでの目標を達成するときに送信できますハプティクス フィードバック Taptic エンジンを介してユーザーに。 アプリも更新してください UI の時点では、ユーザーは、手首、フィードバックを求めるメッセージが表示されるイベントを発生させる多くの場合ため。

Haptic フィードバックを再生するには、次のコードを使用します。

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>イベントの監視

イベントは、アプリがユーザーのトレーニング中に特定のポイントを強調表示に使用できるタイムスタンプです。 一部のイベントがアプリによって直接作成され、トレーニングに保存し、HealthKit によって一部のイベントを自動的に作成されます。

HealthKit によって作成されるイベントを確認するには、アプリが上書きされます、`DidGenerateEvent`のメソッド、 `HKWorkoutSessionDelegate`:

```csharp
using System.Collections.Generic;
...

public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Save HealthKit generated event
    WorkoutEvents.Add (@event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Lap:
        break;
    case HKWorkoutEventType.Marker:
        break;
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

Apple は watchOS 3 で次の新しいイベントの種類を追加されます。

- `HKWorkoutEventType.Lap` は、トレーニングを等しい距離部分に分割するイベントです。 たとえば、1 つの簡単な実行中にトラックをマークします。
- `HKWorkoutEventType.Marker` -関心のある任意のポイントは、トレーニング内です。 たとえば、屋外の実行のルート上の特定の時点に到達します。

これらの新しい型をアプリによって作成され、グラフと統計の作成の後で使用できるトレーニングに格納されていることができます。

マーカー イベントを作成するには、次の操作を行います。

```csharp
using System.Collections.Generic;
...

public float MilesRun { get; set; }
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public void ReachedNextMile ()
{
    // Create and save marker event
    var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
    WorkoutEvents.Add (markerEvent);

    // Notify user
    NotifyUserOfReachedMileGoal (++MilesRun);
}
```

このコードは、マーカー イベントの新しいインスタンスを作成します (`HKWorkoutEvent`) とイベント (つまり、トレーニング セッションに後で書き込まれます) のプライベート コレクションに保存され、haptics によって、イベントをユーザーに通知します。

### <a name="pausing-and-resuming-workouts"></a>一時停止と再開ワークアウト

トレーニング セッションでいつでも、ユーザーが一時的に、トレーニングを一時停止およびは後で再開できます。 など、重要な呼び出しを受け取り、呼び出しが完了した後に実行を再開に屋内の実行を一時停止する可能性があります。

アプリの UI には、一時停止および (HealthKit を呼び出すこと) によって、トレーニングを再開できるように、ユーザーが自分のアクティビティを中断している間、Apple Watch が両方の電源およびデータ領域を節約でく方法を提供する必要があります。 また、アプリは、トレーニング セッションが一時停止状態が受信されるすべての新しいデータ ポイントを無視してください。

HealthKit は一時停止および一時停止と再開イベントを生成して呼び出しを再開して応答します。 トレーニング セッションが一時停止中に新しいイベントやデータは送信されませんをアプリに HealthKit によってセッションが再開されるまでです。

トレーニング セッションを一時停止するには、次のコードを使用します。

```csharp
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public HKWorkoutSession WorkoutSession { get; set;}
...

public void PauseWorkout ()
{
    // Pause the current workout
    HealthStore.PauseWorkoutSession (WorkoutSession);
}

public void ResumeWorkout ()
{
    // Pause the current workout
    HealthStore.ResumeWorkoutSession (WorkoutSession);
}
```

HealthKit から生成される一時停止と再開イベントをオーバーライドすることで処理できる、`DidGenerateEvent`のメソッド、 `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);

    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

### <a name="motion-events"></a>モーション イベント

また新しい watchOS 3 には、アニメーションが一時停止 (`HKWorkoutEventType.MotionPaused`) モーションの再開 (`HKWorkoutEventType.MotionResumed`) イベント。 これらのイベント発生自動的に HealthKit によって実行中のトレーニング中に、ユーザーが開始され、移動を停止します。

アプリがモーションが一時停止イベントを受け取るときに、ユーザーは、モーションを再開して、モーションが再開されるイベントを受信するまでデータの収集を停止する必要があります。 アプリには、アニメーションが一時停止イベントに応答トレーニング セッションが一時停止する必要があります。

> [!IMPORTANT]
> アニメーションが一時停止とモーション再開イベントは RunningWorkout アクティビティの種類にのみサポートされます (`HKWorkoutActivityType.Running`)。

これらのイベントを処理して、オーバーライドすることで、もう一度、`DidGenerateEvent`のメソッド、 `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    }
}

```

## <a name="ending-and-saving-the-workout-session"></a>終了して、トレーニング セッションの保存

ユーザーには、トレーニングが完了したら、アプリは現在のトレーニング セッションを終了し、HealthKit のデータベースに保存する必要があります。 ワークアウト HealthKit に保存されますが、トレーニング活動の一覧に自動的に表示されます。

新しい ios 10 では、ユーザーの iphone にもトレーニング アクティビティ リストが含まれます。 したがって、Apple Watch が近くにいない場合でも、スマート フォン、トレーニングが表示されます。

エネルギーのサンプルを含むワークアウト サード パーティ製アプリがユーザーの 1 日の移動の目標に投稿できるようになりましたので、ユーザーのアクティビティ、アプリ内のリングの移動が更新されます。

終了し、トレーニング セッションの保存は、次の手順が必要です。

[![](workout-apps-images/workout05.png "終了して、トレーニング セッション ダイアグラムの保存")](workout-apps-images/workout05.png#lightbox)

1. 最初に、アプリは、トレーニング セッションを終了する必要があります。
2. トレーニング セッションは、HealthKit に保存されます。
3. 保存されたトレーニング セッションに任意のサンプル (燃焼されるエネルギーの距離など) を追加します。

### <a name="ending-the-session"></a>セッションの終了

トレーニング セッションを終了するには、呼び出し、`EndWorkoutSession`のメソッド、`HKHealthStore`を渡して、 `HKWorkoutSession`:

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
...

public void EndOutdoorRun ()
{
    // End the current workout session
    HealthStore.EndWorkoutSession (WorkoutSession);
}
```

これにより、その標準モードにデバイス センサーがリセットされます。 HealthKit は、トレーニングの終了が完了すると、コールバックを受信する、`DidChangeToState`のメソッド、 `HKWorkoutSessionDelegate`:

```csharp
public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
{
    // Take action based on the change in state
    switch (toState) {
    ...
    case HKWorkoutSessionState.Ended:
        StopObservingHealthKitSamples ();
        RaiseEnded ();
        break;
    }

}
```

### <a name="saving-the-session"></a>セッションを保存しています

アプリが、トレーニング セッションを終了するには、トレーニングを作成する必要があります (`HKWorkout`) (イベント) と共に、HealthKit のデータ ストアに保存 (`HKHealthStore`)。

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
public float MilesRun { get; set; }
public double ActiveEnergyBurned { get; set;}
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

private void SaveWorkoutSession ()
{
    // Build required workout quantities 
    var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
    var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

    // Create any required metadata
    var metadata = new NSMutableDictionary ();
    metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

    // Create workout
    var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                    WorkoutSession.StartDate, 
                                    NSDate.Now, 
                                    WorkoutEvents.ToArray (), 
                                    energyBurned, 
                                    distance, 
                                    metadata);

    // Save to HealthKit
    HealthStore.SaveObject (workout, (successful, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (successful) {

            }
        } else {
            // Report error
        }
    });

}
```

このコードを作成、燃焼されるエネルギーの総量との距離を必要としてトレーニングの`HKQuantity`オブジェクト。 トレーニングを定義するメタデータのディクショナリが作成され、トレーニングの場所を指定します。

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

新しい`HKWorkout`オブジェクトを作成すると、同じ`HKWorkoutActivityType`として、 `HKWorkoutSession`、開始と終了日、イベント (上記のセクションから累積されている)、書き込み時に、エネルギーの一覧は合計距離とメタデータのディクショナリ。 このオブジェクトが、正常性ストアに保存され、エラー処理します。  

### <a name="adding-samples"></a>サンプルを追加します。

アプリでサンプルのセットをトレーニングに保存すると、アプリは特定のトレーニングに関連付けられているすべてのサンプルについては、後で HealthKit をクエリすることができます、HealthKit はサンプルと自体トレーニングの間の接続を生成します。 この情報を使用して、アプリは、トレーニング データからグラフを生成し、プロットするには、トレーニングのタイムラインに対して。

アクティビティのアプリの移動のリングに貢献するアプリでは、保存したトレーニングのエネルギー サンプル含める必要があります。 さらに、エネルギーとの距離の合計は、アプリに関連付ける保存したトレーニング サンプルの合計と一致する必要があります。

保存したトレーニングには、サンプルを追加するには、次の操作を行います。

```csharp
using System.Collections.Generic;
using WatchKit;
using HealthKit;
...

public HKHealthStore HealthStore { get; private set; }
public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
...


private void SaveWorkoutSamples (HKWorkout workout)
{
    // Add samples to saved workout
    HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (success) {

            }
        } else {
            // Report error
        }
    });
}
```

必要に応じて、アプリは、計算し、サンプルまたは取得し、保存したトレーニングに関連付けられている (、トレーニングの範囲全体にまたがる) 1 つのメガ サンプルの小さなサブセットを作成します。

## <a name="workouts-and-ios-10"></a>ワークアウト、iOS 10

すべてのアプリに watchOS 3 トレーニングが親 iOS 10 ベースのトレーニング アプリと、10、iOS に新しい (ユーザーの介入) トレーニング モードで、Apple Watch の配置は、バック グラウンドで実行されているモードで watchOS アプリを実行しているトレーニングを開始するこの iOS アプリを使用できます (を参照してください[バック グラウンドの実行に関する](#About-Background-Running)上詳細)。

WatchOS アプリの実行中に WatchConnectivity メッセージングと親 iOS アプリとの通信に使用できます。

このプロセスのしくみを参照してください。

[![](workout-apps-images/workout06.png "iPhone、Apple Watch の通信ダイアグラム")](workout-apps-images/workout06.png#lightbox)

1. IPhone アプリの作成、`HKWorkoutConfiguration`オブジェクトし、トレーニングの種類と場所を設定します。
2. `HKWorkoutConfiguration`オブジェクトには、Apple Watch のバージョンのアプリが送信され、実行されていない場合、システムによって起動されました。
3. WatchOS 3 アプリ トレーニング構成では、渡されたを使用して、新しいトレーニング セッションを開始 (`HKWorkoutSession`)。

> [!IMPORTANT]
> 親の iPhone アプリでは、Apple Watch、トレーニングを開始するためには、3 watchOS アプリにバック グラウンド実行が有効になっている必要があります。 参照してください[バック グラウンド実行を有効にする](#Enabling-Background-Running)上の詳細。

このプロセスは、直接 3 watchOS アプリのトレーニング セッションの開始のプロセスによく似ています。 Iphone の場合は、次のコードを使用します。

```csharp
using System;
using HealthKit;
using WatchConnectivity;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
#endregion
...

private void StartOutdoorRun ()
{
    // Can the app communicate with the watchOS version of the app?
    if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
        // Create a workout configuration
        var configuration = new HKWorkoutConfiguration () {
            ActivityType = HKWorkoutActivityType.Running,
            LocationType = HKWorkoutSessionLocationType.Outdoor
        };

        // Start watch app
        HealthStore.StartWatchApp (configuration, (success, error) => {
            // Handle any errors
            if (error == null) {
                // Was the save successful
                if (success) {
                    ...
                }
            } else {
                // Report error
                ...
            }
        });
    }
}
```

このコードは、watchOS のバージョンのアプリがインストールされているし、iPhone バージョンが最初に接続できることを保証します。

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    ...
}
```

作成し、`HKWorkoutConfiguration`通常どおりを使用して、`StartWatchApp`のメソッド、 `HKHealthStore` Apple Watch に送信し、アプリと、トレーニング セッションを開始します。

Watch OS アプリでは、次のコードを使用して、 `WKExtensionDelegate`:

```csharp
using WatchKit;
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...


public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
{
    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

かかる、`HKWorkoutConfiguration`新たに作成および`HKWorkoutSession`し、カスタムのインスタンスをアタッチします`HKWorkoutSessionDelegate`します。 ユーザーの HealthKit の正常性ストアに対してトレーニング セッションが開始されます。

## <a name="bringing-all-the-pieces-together"></a>すべてのピース統合

このドキュメントで説明する情報はすべて使用すると、watchOS 3 ベースのトレーニングのアプリとその親 iOS 10 ベースのトレーニング アプリは、次の部分を含めることがあります。

1. **iOS 10 `ViewController.cs`**  -ウォッチ接続セッションと Apple Watch でのトレーニングの開始を処理します。
2. **watchOS 3 `ExtensionDelegate.cs`**  -トレーニング アプリの watchOS 3 バージョンを処理します。
3. **watchOS 3 `OutdoorRunDelegate.cs`**  -カスタム`HKWorkoutSessionDelegate`トレーニングのイベントを処理します。

> [!IMPORTANT]
> 次のセクションで示したコードには、watchOS 3 ではトレーニング アプリに提供される新しい高度な機能を実装するために必要な部分にはのみが含まれます。 サポートのすべてのコードと存在し、UI を更新するコードは含まれませんが、他の watchOS ドキュメントに従って簡単に作成できます。<p/>



### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController.cs`親 iOS 10 のバージョンのトレーニング アプリ内のファイルには、次のコードが含まれます。

```csharp
using System;
using HealthKit;
using UIKit;
using WatchConnectivity;

namespace MonkeyWorkout
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
        public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Private Methods
        private void InitializeWatchConnectivity ()
        {
            // Is Watch Connectivity supported?
            if (!WCSession.IsSupported) {
                // No, abort
                return;
            }

            // Is the session already active?
            if (ConnectivitySession.ActivationState != WCSessionActivationState.Activated) {
                // No, start session
                ConnectivitySession.ActivateSession ();
            }
        }

        private void StartOutdoorRun ()
        {
            // Can the app communicate with the watchOS version of the app?
            if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
                // Create a workout configuration
                var configuration = new HKWorkoutConfiguration () {
                    ActivityType = HKWorkoutActivityType.Running,
                    LocationType = HKWorkoutSessionLocationType.Outdoor
                };

                // Start watch app
                HealthStore.StartWatchApp (configuration, (success, error) => {
                    // Handle any errors
                    if (error == null) {
                        // Was the save successful
                        if (success) {
                            ...
                        }
                    } else {
                        // Report error
                        ...
                    }
                });
            }
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Start Watch Connectivity
            InitializeWatchConnectivity ();
        }
        #endregion
    }
}
```

### <a name="extensiondelegatecs"></a>ExtensionDelegate.cs

`ExtensionDelegate.cs` WatchOS 3 バージョンのトレーニング アプリ内のファイルには、次のコードが含まれます。

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
        public OutdoorRunDelegate RunDelegate { get; set; }
        #endregion

        #region Constructors
        public ExtensionDelegate ()
        {
            
        }
        #endregion

        #region Private Methods
        private void StartWorkoutSession (HKWorkoutConfiguration workoutConfiguration)
        {
            // Create workout session
            // Start workout session
            NSError error = null;
            var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

            // Successful?
            if (error != null) {
                // Report error to user and return
                return;
            }

            // Create workout session delegate and wire-up events
            RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

            RunDelegate.Failed += () => {
                // Handle the session failing
                ...
            };

            RunDelegate.Paused += () => {
                // Handle the session being paused
                ...
            };

            RunDelegate.Running += () => {
                // Handle the session running
                ...
            };

            RunDelegate.Ended += () => {
                // Handle the session ending
                ...
            };
            
            RunDelegate.ReachedMileGoal += (miles) => {
                // Handle the reaching a session goal
                ...
            };

            RunDelegate.HealthKitSamplesUpdated += () => {
                // Update UI as required
                ...
            };

            // Start session
            HealthStore.StartWorkoutSession (workoutSession);
        }

        private void StartOutdoorRun ()
        {
            // Create a workout configuration
            var workoutConfiguration = new HKWorkoutConfiguration () {
                ActivityType = HKWorkoutActivityType.Running,
                LocationType = HKWorkoutSessionLocationType.Outdoor
            };

            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion

        #region Override Methods
        public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
        {
            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion
    }
}
```

### <a name="outdoorrundelegatecs"></a>OutdoorRunDelegate.cs

`OutdoorRunDelegate.cs` WatchOS 3 バージョンのトレーニング アプリ内のファイルには、次のコードが含まれます。

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Private Variables
        private HKAnchoredObjectQuery QueryActiveEnergyBurned;
        #endregion

        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        public float MilesRun { get; set; }
        public double ActiveEnergyBurned { get; set;}
        public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
        public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;

        }
        #endregion

        #region Private Methods
        private void ObserveHealthKitSamples ()
        {
            // Get the starting date of the required samples
            var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

            // Get data from the local device
            var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
            var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

            // Assemble compound predicate
            var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

            // Get ActiveEnergyBurned
            QueryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
                // Valid?
                if (error == null) {
                    // Yes, process all returned samples
                    foreach (HKSample sample in addedObjects) {
                        // Accumulate totals
                        var quantitySample = sample as HKQuantitySample;
                        ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);

                        // Save samples
                        WorkoutSamples.Add (sample);
                    }

                    // Inform caller
                    RaiseHealthKitSamplesUpdated ();
                }
            });

            // Start Query
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
                                                  
        }

        private void StopObservingHealthKitSamples ()
        {
            // Stop query
            HealthStore.StopQuery (QueryActiveEnergyBurned);
        }

        private void ResumeObservingHealthkitSamples ()
        {
            // Resume current queries 
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
        }

        private void NotifyUserOfReachedMileGoal (float miles)
        {
            // Play haptic feedback
            WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);

            // Raise event
            RaiseReachedMileGoal (miles);
        }

        private void SaveWorkoutSession ()
        {
            // Build required workout quantities
            var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
            var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

            // Create any required metadata
            var metadata = new NSMutableDictionary ();
            metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

            // Create workout
            var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                            WorkoutSession.StartDate, 
                                            NSDate.Now, 
                                            WorkoutEvents.ToArray (), 
                                            energyBurned, 
                                            distance, 
                                            metadata);

            // Save to HealthKit
            HealthStore.SaveObject (workout, (successful, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (successful) {
                        // Add samples to workout
                        SaveWorkoutSamples (workout);
                    }
                } else {
                    // Report error
                    ...
                }
            });

        }

        private void SaveWorkoutSamples (HKWorkout workout)
        {
            // Add samples to saved workout
            HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (success) {
                        ...
                    }
                } else {
                    // Report error
                    ...
                }
            });
        }
        #endregion

        #region Public Methods
        public void PauseWorkout ()
        {
            // Pause the current workout
            HealthStore.PauseWorkoutSession (WorkoutSession);
        }

        public void ResumeWorkout ()
        {
            // Pause the current workout
            HealthStore.ResumeWorkoutSession (WorkoutSession);
        }

        public void ReachedNextMile ()
        {
            // Create and save marker event
            var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
            WorkoutEvents.Add (markerEvent);

            // Notify user
            NotifyUserOfReachedMileGoal (++MilesRun);
        }

        public void EndOutdoorRun ()
        {
            // End the current workout session
            HealthStore.EndWorkoutSession (WorkoutSession);
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                StopObservingHealthKitSamples ();
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                if (fromState == HKWorkoutSessionState.Paused) {
                    ResumeObservingHealthkitSamples ();
                } else {
                    ObserveHealthKitSamples ();
                }
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                StopObservingHealthKitSamples ();
                SaveWorkoutSession ();
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);

            // Save HealthKit generated event
            WorkoutEvents.Add (@event);

            // Take action based on the type of event
            switch (@event.Type) {
            case HKWorkoutEventType.Lap:
                ...
                break;
            case HKWorkoutEventType.Marker:
                ...
                break;
            case HKWorkoutEventType.MotionPaused:
                ...
                break;
            case HKWorkoutEventType.MotionResumed:
                ...
                break;
            case HKWorkoutEventType.Pause:
                ...
                break;
            case HKWorkoutEventType.Resume:
                ...
                break;
            }
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();
        public delegate void OutdoorRunMileGoalDelegate (float miles);

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }

        public event OutdoorRunMileGoalDelegate ReachedMileGoal;
        internal void RaiseReachedMileGoal (float miles)
        {
            if (this.ReachedMileGoal != null) this.ReachedMileGoal (miles);
        }

        public event OutdoorRunEventDelegate HealthKitSamplesUpdated;
        internal void RaiseHealthKitSamplesUpdated ()
        {
            if (this.HealthKitSamplesUpdated != null) this.HealthKitSamplesUpdated ();
        }
        #endregion
    }
}
```

## <a name="best-practices"></a>ベスト プラクティス

Apple は watchOS 3 および iOS 10 でのトレーニング アプリの実装の設計とは、次のベスト プラクティスを使用して示しています。

- IPhone と iOS 10 のバージョンのアプリに接続できない場合でも、watchOS 3 トレーニング アプリは引き続き機能であることを確認します。
- GPS は、GPS せず距離サンプルを生成することであるために、使用可能な場合は、HealthKit の距離を使用します。
- Apple Watch または iPhone のいずれかから、トレーニングを開始するユーザーを許可します。
- その履歴データ ビューで (他のサード パーティ製アプリ) などの他のソースからワークアウトを表示するアプリを許可します。
- 履歴データのない削除表示ワークアウト、アプリでは確認してください。

## <a name="summary"></a>まとめ

この記事では、拡張機能をカバーされて Apple は watchOS 3 と Xamarin で実装する方法でアプリをトレーニングするしました。



## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [HealthKit の概要](~/ios/platform/healthkit.md)
