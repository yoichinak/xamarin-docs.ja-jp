---
title: "アプリのトレーニング"
description: "拡張機能を取り上げて watchOS 3、および Xamarin でそれらを実装する方法でアプリのトレーニングを Apple が発生しました。"
ms.topic: article
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 2282a340811d9932f9df3a1343b22ffc35247e54
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="workout-apps"></a>アプリのトレーニング

_拡張機能を取り上げて watchOS 3、および Xamarin でそれらを実装する方法でアプリのトレーニングを Apple が発生しました。_


新しい watchOS 3、トレーニング関連するアプリには、Apple Watch でバック グラウンドで実行し、HealthKit データにアクセスする機能が設定されています。 その親 iOS 10 ベースのアプリでは、ユーザーの介入なし watchOS 3 ベースのアプリを起動する機能もあります。

次のトピックで、詳しく説明します。

## <a name="about-workout-apps"></a>トレーニングのアプリについて

適合性およびトレーニングのアプリのユーザーできる高専用の正常性および適合性の目標に向けてのいくつかの時間を設けることです。 その結果、正確に収集し、データを表示し、Apple の正常性とシームレスに統合するアプリが応答性の高い、使いやすいを享受できます。

適切に設計の適合性またはトレーニング アプリを使うと、グラフのそれらの活動の適合性の目標を達成できます。 Apple Watch を使用すると、適合性およびトレーニングのアプリにアクセス インスタント心拍数にカロリー burn とアクティビティの検出。

[![](workout-apps-images/workout01.png "アプリの適合性とトレーニングの例")](workout-apps-images/workout01.png#lightbox)

WatchOS 3 を新しい_バック グラウンドで実行されている_によりトレーニング関連アプリ Apple Watch でバック グラウンドで実行し、HealthKit データにアクセスする機能。

このドキュメントはバック グラウンドで実行されている機能が導入、トレーニングのアプリのライフ サイクルをカバーし、ユーザーのトレーニング アプリをどのように影響することが表示_アクティビティ リング_Apple Watch でします。

## <a name="about-workout-sessions"></a>トレーニングのセッションについて

すべてのアプリにトレーニングの中核部分は、_トレーニング セッション_(`HKWorkoutSession`) をユーザーが開始および停止できます。 トレーニング セッション API は、簡単に実装しなど、トレーニングのアプリにいくつかの利点を提供します。

- モーションおよびカロリー アクティビティ タイプに基づいて検出に書き込みます。
- ユーザーのアクティビティまでの呼び出しに自動貢献します。
- セッション中に、アプリケーションが自動的に表示されます、ユーザーのいずれかによって、手首を発生させるまたは Apple Watch との対話) デバイスをスリープ解除するたびにします。

## <a name="about-background-running"></a>バック グラウンドの実行について

前述のように、watchOS 3 とトレーニング アプリに設定できます、バック グラウンドで実行します。 バック グラウンド トレーニングのアプリを実行してを使用して、バック グラウンドで実行中に、Apple Watch のセンサーからデータを処理できます。 たとえば、アプリを引き続き監視ユーザーの心拍数、場合でも、画面に表示されません。

バック グラウンドの実行中には、その進行状況を現在のユーザーに通知する haptic アラートを送信するなど、アクティブなトレーニング セッション中にいつでもライブのフィードバックをユーザーに提供する機能も用意されています。

さらに、バック グラウンドで実行されていると、アプリへのため、すぐに、Apple Watch で概要、ときに、ユーザーがある最新のデータにそのユーザー インターフェイスをすばやく更新が許可します。

Apple Watch で高いパフォーマンスを維持するためにバック グラウンドで実行されているを使用して watch アプリは、バッテリを節約するためにバック グラウンド作業の量を制限する必要があります。 アプリがバック グラウンドで過剰な CPU を使用している場合は、watchOS によって中断を取得できます。

### <a name="enabling-background-running"></a>実行しているバック グラウンドの有効化

バック グラウンドの実行を有効にするには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、ウォッチ拡張機能のコンパニオン iPhone アプリをダブルクリックして`Info.plist`ファイルを開いて編集するファイル。
2. 切り替えて、**ソース**ビュー。 

    [![](workout-apps-images/plist01.png "ソース ビュー")](workout-apps-images/plist01.png#lightbox)
3. 呼ばれる新しいキーを追加`WKBackgroundModes`設定と、**型**に`Array`: 

    [![](workout-apps-images/plist02.png "WKBackgroundModes と呼ばれる新しいキーを追加します。")](workout-apps-images/plist02.png#lightbox)
4. 新しい項目の配列を追加、**型**の`String`と値の`workout-processing`: 

    [![](workout-apps-images/plist03.png "文字列型とトレーニング処理の値を持つ配列に新しい項目を追加します。")](workout-apps-images/plist03.png#lightbox)
5. 変更内容をファイルに保存します。

## <a name="starting-a-workout-session"></a>トレーニングのセッションの開始

これには、トレーニングのセッションを開始する次の 3 つの主要な手順があります。

[![](workout-apps-images/workout02.png "トレーニングのセッションを開始する次の 3 つの主要な手順")](workout-apps-images/workout02.png#lightbox)

1. アプリでは、HealthKit のデータにアクセスするための承認を要求する必要があります。
2. 開始されているトレーニングの型のトレーニング構成オブジェクトを作成します。
3. 作成し、新しく作成されたトレーニング構成を使用してトレーニング セッションを開始します。

### <a name="requesting-authorization"></a>承認を要求します。

アプリは、ユーザーの HealthKit データにアクセスできます、前に、要求およびユーザーから受信した認証があります。 トレーニングのアプリの性質によって次の種類の要求の必要があります。

- データの書き込みを許可します。
    - ワークアウト
- データの読み取りを許可します。
    - 書き込んだエネルギー
    - 距離
    - 心拍数  

アプリには、承認を要求できる、HealthKit にアクセスするように構成する必要があります。

次の手順で行います。

1. **ソリューション エクスプローラー**で `Entitlements.plist` ファイルをダブルクリックして、編集用に開きます。
2. 下部までスクロールし、確認**を有効にする HealthKit**: 

    [![](workout-apps-images/auth01.png "チェックを有効にする HealthKit")](workout-apps-images/auth01.png#lightbox)
3. 変更内容をファイルに保存します。
4. 指示に従って、[明示的なアプリ ID とプロビジョニング プロファイル](~/ios/platform/healthkit.md)と[アプリ ID とプロビジョニング プロファイルで Xamarin.iOS アプリに関連付ける](~/ios/platform/healthkit.md)のセクション、[の概要HealthKit](~/ios/platform/healthkit.md)資料を正しくアプリをプロビジョニングします。
5. 最後に、手順を使用して、[ヘルス キットのプログラミング](~/ios/platform/healthkit.md)と[、ユーザーからアクセス許可を要求する](~/ios/platform/healthkit.md)のセクションでは、 [HealthKit の概要](~/ios/platform/healthkit.md)要求に記事ユーザーの HealthKit データストアにアクセスを許可します。

### <a name="setting-the-workout-configuration"></a>トレーニングの構成を設定

トレーニング構成オブジェクトを使用してトレーニングのセッションが作成されます (`HKWorkoutConfiguration`) トレーニング型を指定する (など`HKWorkoutActivityType.Running`) とトレーニングの場所 (など`HKWorkoutSessionLocationType.Outdoor`)。

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

トレーニング セッション中に発生するイベントを処理するには、アプリは、トレーニング セッション デリゲート インスタンスを作成する必要があります。 新しいクラスをプロジェクトに追加し、基になるのではオフ、`HKWorkoutSessionDelegate`クラスです。 屋外実行の例では、次のようになることでした。

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

このクラスは、トレーニングのセッションの変更の状態として発生するいくつかのイベントを作成する (`DidChangeToState`) し、トレーニングのセッションが失敗した場合 (`DidFail`)。 

### <a name="creating-a-workout-session"></a>トレーニング セッションを作成します。

新しいトレーニング セッションを作成し、ユーザーの既定の HealthKit ストアに対して開始上、トレーニングの構成とトレーニング セッション デリゲートを使用しては作成されます。

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

アプリは、このトレーニング セッションを開始、ウォッチの文字盤に切り替える場合は、面の上、小さなの緑の「man を実行している」アイコンが表示されます。

[![](workout-apps-images/workout03.png "小さな緑色実行中のマニュアルのアイコン表面上に表示されます。")](workout-apps-images/workout03.png#lightbox)

場合は、ユーザーがこのアイコンをタップは、アプリに戻る、撮影されます。

## <a name="data-collection-and-control"></a>データの収集とコントロール

トレーニングのセッションが構成されている、起動されると、アプリは、セッション (など、ユーザーの心拍数) に関するデータを収集し、セッションの状態を制御する必要があります。

[![](workout-apps-images/workout04.png "データの収集とコントロールの図")](workout-apps-images/workout04.png#lightbox)

1. **サンプルを観察し**-アプリが処理してユーザーに表示される HealthKit から情報を取得する必要があります。
2. **イベントを観察し**-アプリは、ユーザーは、トレーニングの一時停止) など、アプリの UI からまたは HealthKit によって生成されるイベントに応答する必要があります。
3. **実行中の状態を入力**-セッションは開始されており、現在実行されています。
4. **一時停止状態に入る**-ユーザー トレーニングの現在のセッションが一時停止し、後で再起動できます。 ユーザーは、1 つのトレーニング セッション内で複数回実行され、一時停止状態の間で切り替えることがあります。
5. **トレーニングのセッションを終了する**- ユーザーが、トレーニングのセッションの終了時に任意の時点か、または可能性があります期限切れ、(2 つのマイル実行) などの従量制課金のトレーニングがあった場合、独自に終了します。

最後の手順では、ユーザーの HealthKit データストアにトレーニングのセッションの結果を保存します。

### <a name="observing-healthkit-samples"></a>HealthKit サンプルを確認します。

アプリを開く必要があります、_アンカー オブジェクト クエリ_のそれぞれの HealthKit データ ポイントを求めている、心拍数またはアクティブな電力が書き込んだなどです。 各データ ポイントが監視されて、更新ハンドラーは、アプリに送信されるときに、新しいデータをキャプチャするために作成する必要があります。

これらのデータ ポイントからは、アプリは集計 (合計実行の距離など) を蓄積されの更新のユーザー インターフェイスを必要に応じて。 さらに、アプリは、達成度に基づいて、実行の次のマイル単位の完了など、特定の目標に達しているときに、ユーザーを通知することができます。

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

使用してデータを取得する開始の日付を設定する述語を作成、`GetPredicateForSamples`メソッドです。 使用してから HealthKit 情報をプルするデバイスのセットを作成、`GetPredicateForObjectsFromDevices`メソッド、ローカルの Apple Watch のみの場合もこの (`HKDevice.LocalDevice`)。 2 つの述語は、複合述語に結合されます (`NSCompoundPredicate`) を使用して、`CreateAndPredicate`メソッドです。

新しい`HKAnchoredObjectQuery`が必要なデータ ポイントの作成 (ここでは`HKQuantityTypeIdentifier.ActiveEnergyBurned`Active エネルギー書き込んだデータ ポイントの)、返されるデータの量に制限は適用されません (`HKSampleQuery.NoLimit`) し、データがアプリに戻り値を処理する、更新ハンドラーが定義されています。HealthKit です。 

更新ハンドラーはいつでも、指定されたデータ ポイント用のアプリに新しいデータが配信と呼ばれます。 エラーが返されない場合、アプリ安全にデータの読み取り、必要な計算を作成および更新できます必要に応じてその UI。

すべてのサンプルをループ処理し、コード (`HKSample`) で返される、`addedObjects`配列し、数量のサンプルではキャスト (`HKQuantitySample`)。 Joule としてこのサンプルの double 値を取得し、(`HKUnit.Joule`)、トレーニングして書き込んだアクティブなエネルギーの累計に蓄積し、ユーザー インターフェイスを更新します。

### <a name="achieved-goal-notification"></a>達成目標通知

上記のように、ユーザーが、トレーニングなどのアプリで (完了すると、実行の最初のマイル)、目標を達成ときに送信できる haptic フィードバック Taptic エンジンを使用してユーザーにします。 アプリも更新してください UI の時点では、ユーザーは、フィードバックを要求したイベントを表示するには、その手首を発生させる複数の可能性があるためです。

Haptic フィードバックを再生するには、次のコードを使用します。

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>イベントを確認します。

イベントは、タイムスタンプをアプリは、ユーザーのトレーニング中に特定のポイントを強調表示することができます。 一部のイベントが、アプリで直接作成され、トレーニングに保存し、HealthKit によって一部のイベントを自動的に作成されます。

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

Apple には、watchOS 3 で、次の新しいイベントの種類が追加されます。

- `HKWorkoutEventType.Lap` は、トレーニングを等間隔で特定の部分に分割するイベントです。 たとえば、実行中のトラックの周囲の 1 つのラップをマークします。
- `HKWorkoutEventType.Marker` -は、関心のある任意のポイント、トレーニング内です。 たとえば、ルートの屋外実行の特定の時点に到達します。

これらの新しい型をアプリによって作成され、グラフと統計情報の作成に後で使用できるトレーニングに格納されていることができます。

マーカーのイベントを作成するには、次の操作を行います。

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

このコードは、マーカーのイベントの新しいインスタンスを作成 (`HKWorkoutEvent`) とプライベート イベント (つまり、トレーニングのセッションに後で書き込まれます) のコレクションに保存および haptics によって、イベントをユーザーに通知します。

### <a name="pausing-and-resuming-workouts"></a>一時停止と再開ワークアウト

トレーニングのセッションでの任意の時点で、ユーザーが一時的に、トレーニングを一時停止および後で再開できます。 たとえば、室内の実行、重要な呼び出しを行い、呼び出しが完了した後に実行を再開するには、一時停止可能性があります。

アプリの UI には、一時停止および (HealthKit を呼び出すこと) によって、トレーニングを再開できるように、ユーザーがユーザーのアクティビティを中断している間、Apple Watch は両方の電源およびデータ領域を節約でく方法を提供する必要があります。 また、アプリは、トレーニングのセッションが一時停止状態ではときに受信されるすべての新しいデータ ポイントを無視してください。

HealthKit が応答するには一時停止および一時停止と再開イベントを生成して呼び出しを再開します。 トレーニングのセッションが一時停止中に、新しいイベントやデータは送信されませんをアプリに HealthKit によってセッションが再開されるまでです。

一時停止およびトレーニング セッションを再開するには、次のコードを使用します。

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

オーバーライドすることで処理できます HealthKit から生成される一時停止と再開イベント、`DidGenerateEvent`のメソッド、 `HKWorkoutSessionDelegate`:

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

また新しい watchOS 3 には、動きが一時停止 (`HKWorkoutEventType.MotionPaused`) およびモーション再開 (`HKWorkoutEventType.MotionResumed`) イベント。 これらのイベントに自動的に HealthKit によって実行中のトレーニング中にときに発生する、ユーザーが開始時および移動を停止します。

アプリ、動きが一時停止イベントを受け取ると、ユーザーが動きを再開して、動きが再開されるイベントを受信するまでデータの収集を停止する必要があります。 アプリのアプリでは、動きが一時停止イベントに応答トレーニング セッションを一時停止する必要がありますされません。

> [!IMPORTANT]
> 動きが一時停止およびモーション再開イベントは、RunningWorkout アクティビティの種類でのみサポート (`HKWorkoutActivityType.Running`)。

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

## <a name="ending-and-saving-the-workout-session"></a>終了して、トレーニングのセッションの保存

ユーザーには、トレーニングが完了したら、アプリは、トレーニングの現在のセッションを終了して、HealthKit データベースに保存する必要があります。 ワークアウト HealthKit に保存は、トレーニング アクティビティの一覧に自動的に表示されます。

新しい iOS 10 に、ユーザーの iphone もトレーニング アクティビティ リストが含まれます。 ように、Apple Watch が近くにいない場合でも、トレーニングは、電話で表示されます。

ワークアウト エネルギー サンプルは含まれているサード パーティ製アプリは、ユーザーの 1 日の移動目標に行うようになりましたことができますので、ユーザーのアクティビティ アプリ内のリングの移動が更新されます。

次の手順を終了し、トレーニングのセッションを保存する必要があります。

[![](workout-apps-images/workout05.png "終了して、トレーニング セッション ダイアグラムの保存")](workout-apps-images/workout05.png#lightbox)

1. 最初に、アプリは、トレーニングのセッションを終了する必要があります。
2. トレーニングのセッションは、HealthKit に保存されます。
3. サンプル (書き込んだエネルギー距離など) を保存したトレーニング セッションに追加します。

### <a name="ending-the-session"></a>セッションの終了

トレーニングのセッションを終了する、`EndWorkoutSession`のメソッド、`HKHealthStore`を渡して、 `HKWorkoutSession`:

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

通常モードにこのデバイス センサーがリセットされます。 HealthKit では、トレーニングの終了が完了すると、コールバックを受信する、`DidChangeToState`のメソッド、 `HKWorkoutSessionDelegate`:

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

### <a name="saving-the-session"></a>セッションの保存

作成、トレーニングする必要があります、アプリがトレーニング セッションを終了すると、(`HKWorkout`) (イベント) と共に HealthKit データ ストアに保存し (`HKHealthStore`)。

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

このコードを作成、としてトレーニングを書き込んだエネルギーの総量との距離を必要と`HKQuantity`オブジェクト。 トレーニングを定義するメタデータのディクショナリが作成され、トレーニングの場所を指定します。

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

新しい`HKWorkout`オブジェクトが同じで作成された`HKWorkoutActivityType`として、`HKWorkoutSession`距離とメタデータのディクショナリ、開始日と終了日、一連のイベント (上記の各セクションから累積されている)、書き込み、電力の合計します。 このオブジェクトは、正常性ストアに保存し、任意のエラーを処理します。  

### <a name="adding-samples"></a>サンプルを追加します。

アプリでは、サンプルのセットを保存、トレーニングに、アプリが特定のトレーニングに関連付けられているすべてのサンプルについては後で HealthKit をクエリできますできるようにする、HealthKit はサンプルと自体トレーニングの間の接続を生成します。 この情報を使用して、アプリは、トレーニング データからグラフを生成し、プロットするには、トレーニングのタイムラインに対してします。

アクティビティ アプリの移動リングに貢献するアプリでは、保存したトレーニングとエネルギーのサンプルを含めることは。 さらに、距離とエネルギーの合計は、アプリとを関連付ける保存したトレーニング サンプルの合計と一致する必要があります。

保存したトレーニングにサンプルを追加するには、次の操作を行います。

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

必要に応じて、アプリを計算し、サンプルまたは取得し、保存済みのトレーニングに関連付けられている (、トレーニングの範囲全体にまたがる) 1 つのメガ サンプルの小さなサブセットを作成できます。

## <a name="workouts-and-ios-10"></a>ワークアウトおよび iOS 10

すべてのアプリに watchOS 3 トレーニングが親 iOS 10 ベースのトレーニング アプリと、10、iOS に新しい (ユーザーの介入) トレーニング モードで、Apple Watch の配置は、バック グラウンドで実行されているモードで watchOS アプリを実行しているトレーニングを開始するこの iOS アプリを使用できます (を参照してください[バック グラウンドの実行に関する](#About-Background-Running)上詳細)。

WatchOS アプリの実行中に WatchConnectivity メッセージングと親の iOS アプリとの通信に使用できます。

このプロセスのしくみを見てをみましょう。

[![](workout-apps-images/workout06.png "iPhone、Apple Watch 通信ダイアグラム")](workout-apps-images/workout06.png#lightbox)

1. IPhone アプリの作成、`HKWorkoutConfiguration`オブジェクトし、トレーニングの種類と場所を設定します。
2. `HKWorkoutConfiguration`オブジェクトには、Apple Watch アプリのバージョンが送信され、実行されていない場合、システムによって起動されました。
3. トレーニングの構成では、渡されたを使用して、watchOS 3 アプリケーションは新しいトレーニング セッションが開始される (`HKWorkoutSession`)。

> [!IMPORTANT]
> 親の iPhone アプリで、Apple Watch、トレーニングを開始するためには、watchOS 3 アプリにバック グラウンドが有効になっているを実行している必要があります。 参照してください[を有効にするバック グラウンドを実行している](#Enabling-Background-Running)上詳細についてはします。

このプロセスは、直接 watchOS 3 アプリでトレーニング セッションを開始するプロセスによく似ています。 IPhone では、次のコードを使用します。

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

このコードは、watchOS バージョンのアプリがインストールされ、iPhone のバージョンが最初に接続できることをによりします。

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    ...
}
```

作成し、`HKWorkoutConfiguration`通常どおりを使用して、`StartWatchApp`のメソッド、 `HKHealthStore` Apple Watch に送信し、アプリとトレーニングのセッションを開始します。

ウォッチ OS アプリ上で次のコードを使用して、 `WKExtensionDelegate`:

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

かかる、`HKWorkoutConfiguration`新たに作成および`HKWorkoutSession`し、カスタムのインスタンスをアタッチ`HKWorkoutSessionDelegate`です。 ユーザーの HealthKit 正常性ストアに対してトレーニング セッションが開始されます。

## <a name="bringing-all-the-pieces-together"></a>すべての要素 1 つにまとめて

すべての情報は、このドキュメントで説明すると、watchOS 3 に基づいてトレーニング アプリとその親 iOS 10 ベースのトレーニング アプリは、次の部分を含める場合があります。

1. **iOS 10 `ViewController.cs`**  -ウォッチ接続セッションおよび Apple Watch でのトレーニングの開始を処理します。
2. **watchOS 3 `ExtensionDelegate.cs`**  -watchOS 3 バージョンのトレーニング アプリを処理します。
3. **watchOS 3 `OutdoorRunDelegate.cs`**  -カスタム`HKWorkoutSessionDelegate`トレーニングのイベントを処理します。

> [!IMPORTANT]
> 次のセクションで示したコードには、watchOS 3 でのアプリのトレーニングに提供される、新しい拡張機能を実装するために必要な部分にはのみが含まれます。 すべてのサポート コードと存在し、UI を更新するコードが含まれていないが、他の watchOS ドキュメントに従って簡単に作成できます。<p/>



### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController.cs`親 iOS 10 バージョンのトレーニング アプリ内のファイルは次のコードを追加します。

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

`ExtensionDelegate.cs` WatchOS 3 バージョンのトレーニング アプリ内のファイルは次のコードを追加します。

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

`OutdoorRunDelegate.cs` WatchOS 3 バージョンのトレーニング アプリ内のファイルは次のコードを追加します。

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

設計と実装の watchOS 3 および iOS 10 でアプリのトレーニング時に、次のベスト プラクティスを使用して Apple を示しています。

- IPhone と iOS 10 バージョンのアプリに接続することができない場合でも、watchOS 3 トレーニング アプリが引き続き機能していることを確認します。
- GPS は、GPS せず距離サンプルを生成することであるために、使用可能な場合は、HealthKit 距離を使用します。
- Apple Watch または iPhone のいずれかから、その成果を起動するユーザーを許可します。
- その履歴データ ビューの (他のサード パーティ製アプリ) など他のソースからワークアウトを表示するアプリを許可します。
- 履歴データのない削除表示ワークアウト、アプリではことを確認します。

## <a name="summary"></a>まとめ

この記事は、拡張機能をカバーされて watchOS 3、および Xamarin でそれらを実装する方法でアプリのトレーニングを Apple が発生しました。



## <a name="related-links"></a>関連リンク

- [iOS 10 サンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [HealthKit の概要](~/ios/platform/healthkit.md)
