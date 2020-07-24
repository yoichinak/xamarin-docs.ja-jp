---
title: Xamarin での watchOS のトレーニングアプリ
description: この記事では、watchOS 3 でのアプリのトレーニングと Xamarin での実装方法について、Apple が行った機能強化について説明します。
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: c4fc1607667dd6201c28c4d00a2938760e429f0f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938984"
---
# <a name="watchos-workout-apps-in-xamarin"></a>Xamarin での watchOS のトレーニングアプリ

_この記事では、watchOS 3 でのアプリのトレーニングと Xamarin での実装方法について、Apple が行った機能強化について説明します。_

WatchOS 3 の新機能であるトレーニング関連のアプリには、Apple Watch のバックグラウンドで実行し、HealthKit データにアクセスできるようになりました。 また、その親 iOS 10 ベースのアプリには、ユーザーの介入なしに watchOS 3 ベースのアプリを起動する機能もあります。

次のトピックで、詳しく説明します。

## <a name="about-workout-apps"></a>トレーニングアプリについて

適合性とトレーニングのアプリのユーザーは、非常に専用で、健康と適合性の目標に対して数時間取り上げます。 その結果、データを正確に収集して表示し、Apple Health とシームレスに統合する、応答性が高く使いやすいアプリが期待されます。

適切に設計された適合性またはトレーニングアプリを使用すると、ユーザーは自分のアクティビティをグラフ化して適合性の目標を達成できます。 Apple Watch を使用することにより、適合性とトレーニングのアプリは、ハートレート、calorie 書き込み、アクティビティ検出に瞬時にアクセスできます。

[![適合性とトレーニングアプリの例](workout-apps-images/workout01.png)](workout-apps-images/workout01.png#lightbox)

WatchOS 3 の新機能である_バックグラウンド実行_により、トレーニング関連のアプリが Apple Watch のバックグラウンドで実行し、HealthKit データにアクセスできるようになります。

このドキュメントでは、バックグラウンドで動作する機能を紹介し、トレーニングアプリのライフサイクルについて説明します。また、トレーニングアプリが Apple Watch のユーザーの_アクティビティリング_にどのように貢献するかを示します。

## <a name="about-workout-sessions"></a>トレーニングセッションについて

すべてのトレーニングアプリの中核となる_Workout Session_のは、 `HKWorkoutSession` ユーザーが開始および停止できるトレーニングセッション () です。 トレーニングセッション API は簡単に実装でき、次のようなトレーニングアプリにはいくつかの利点があります。

- アクティビティの種類に基づいたモーションおよび calorie 書き込みの検出。
- ユーザーのアクティビティリングに対する自動的な貢献。
- セッション中は、ユーザーがデバイスをスリープ解除するたびに、アプリが自動的に表示されます (手首を上げたり、Apple Watch と対話したりすることによって)。

## <a name="about-background-running"></a>バックグラウンドでの実行について

前述のように、watchOS 3 では、トレーニングアプリをバックグラウンドで実行するように設定できます。 トレーニングアプリを実行しているバックグラウンドを使用すると、バックグラウンドで実行しているときに、Apple Watch のセンサーのデータを処理できます。 たとえば、アプリは画面に表示されなくなった場合でも、ユーザーのハートレートを引き続き監視できます。

バックグラウンド実行では、アクティブなトレーニングセッション中にいつでもライブフィードバックをユーザーに提示する機能が提供されます。たとえば、haptic アラートを送信して、現在の進行状況をユーザーに通知することができます。

また、バックグラウンドで実行すると、アプリはユーザーインターフェイスをすばやく更新できるので、ユーザーが Apple Watch をひとめで確認できるようになります。

Apple Watch の高いパフォーマンスを維持するには、バックグラウンドで実行する Watch アプリで、バッテリを節約するためのバックグラウンド作業の量を制限する必要があります。 アプリがバックグラウンドで過剰に CPU を使用している場合は、watchOS によって中断される可能性があります。

### <a name="enabling-background-running"></a>バックグラウンド実行の有効化

バックグラウンドでの実行を有効にするには、次の操作を行います。

1. **ソリューションエクスプローラー**で、ウォッチ拡張機能の関連 iPhone アプリのファイルをダブルクリックし `Info.plist` て、編集用に開きます。
2. **ソース**ビューに切り替えます。 

    [![ソースビュー](workout-apps-images/plist01.png)](workout-apps-images/plist01.png#lightbox)
3. という名前の新しいキーを追加 `WKBackgroundModes` し、**型**をに設定し `Array` ます。 

    [![WKBackgroundModes という名前の新しいキーを追加します。](workout-apps-images/plist02.png)](workout-apps-images/plist02.png#lightbox)
4. の**型**と値を使用して、配列に新しい項目を追加 `String` し `workout-processing` ます。 

    [![文字列の型とトレーニング処理の値を使用して、配列に新しい項目を追加します。](workout-apps-images/plist03.png)](workout-apps-images/plist03.png#lightbox)
5. 変更をファイルに保存します。

## <a name="starting-a-workout-session"></a>トレーニングセッションの開始

トレーニングセッションを開始するには、次の3つの主要な手順を実行します。

[![トレーニングセッションを開始するための3つの主要な手順](workout-apps-images/workout02.png)](workout-apps-images/workout02.png#lightbox)

1. アプリは、HealthKit 内のデータにアクセスするための承認を要求する必要があります。
2. 開始するトレーニングの種類に対応するトレーニング構成オブジェクトを作成します。
3. 新しく作成したトレーニングの構成を使用して、トレーニングセッションを作成して開始します。

### <a name="requesting-authorization"></a>承認の要求

アプリは、ユーザーの HealthKit データにアクセスする前に、ユーザーの承認を要求および受信する必要があります。 トレーニングアプリの性質に応じて、次の種類の要求を行うことができます。

- データを書き込むための承認:
  - ワークアウト
- データを読み取るための承認:
  - 書き込み済みエネルギー
  - 距離
  - ハートレート  

アプリが承認を要求できるようにするには、HealthKit にアクセスするようにアプリを構成する必要があります。

次の手順を実行します。

1. **ソリューション エクスプローラー**で `Entitlements.plist` ファイルをダブルクリックして、編集用に開きます。
2. 一番下までスクロールし、[ **Enable HealthKit**] チェックボックスをオンにします。 

    [![Enable HealthKit を確認する](workout-apps-images/auth01.png)](workout-apps-images/auth01.png#lightbox)
3. 変更をファイルに保存します。
4. 「 [HealthKit の概要](~/ios/platform/healthkit.md)」の記事の「[明示的なアプリ Id とプロビジョニングプロファイル](~/ios/platform/healthkit.md)」の指示に従って、アプリ[Id とプロビジョニングプロファイルを Xamarin. iOS アプリに関連付け](~/ios/platform/healthkit.md)て、アプリを正しくプロビジョニングします。
5. 最後に、 [HealthKit の概要](~/ios/platform/healthkit.md)に関する記事のユーザーセクションの「[プログラミング正常性キット](~/ios/platform/healthkit.md)」の指示に従って[アクセス許可](~/ios/platform/healthkit.md)を要求し、ユーザーの HealthKit データストアにアクセスするための承認を要求します。

### <a name="setting-the-workout-configuration"></a>トレーニング構成の設定

トレーニングセッションは、トレーニングの `HKWorkoutConfiguration` 種類 (など) とトレーニングの場所 (など) を指定するトレーニング構成オブジェクト () を使用して作成され `HKWorkoutActivityType.Running` `HKWorkoutSessionLocationType.Outdoor` ます。

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
  ActivityType = HKWorkoutActivityType.Running,
  LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>トレーニングセッションデリゲートの作成 

トレーニングセッション中に発生する可能性のあるイベントを処理するには、アプリでトレーニングセッションデリゲートインスタンスを作成する必要があります。 新しいクラスをプロジェクトに追加し、クラスの基本にし `HKWorkoutSessionDelegate` ます。 屋外実行の例では、次のようになります。

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

このクラスは、トレーニングセッションの変化 () と、トレーニングセッションが失敗した場合 () に発生するいくつかのイベントを作成し `DidChangeToState` `DidFail` ます。 

### <a name="creating-a-workout-session"></a>トレーニングセッションの作成

上記で作成したトレーニング構成とトレーニングセッションデリゲートを使用して、新しいトレーニングセッションを作成し、ユーザーの既定の HealthKit ストアに対して開始します。

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

アプリがこのトレーニングセッションを開始し、ユーザーが自分のウォッチ顔に戻ると、小さい緑色の "実行中の man" アイコンが顔の上に表示されます。

[![顔の上に表示される小さな緑色の実行中の man アイコン](workout-apps-images/workout03.png)](workout-apps-images/workout03.png#lightbox)

ユーザーがこのアイコンをタップすると、アプリに戻ります。

## <a name="data-collection-and-control"></a>データの収集と制御

トレーニングセッションが構成され、開始されると、アプリはセッションに関するデータ (ユーザーのハートレートなど) を収集し、セッションの状態を制御する必要があります。

[![データの収集と制御の図](workout-apps-images/workout04.png)](workout-apps-images/workout04.png#lightbox)

1. **サンプル**を確認する-アプリは、ユーザーに対して処理され、表示される情報を HealthKit から取得する必要があります。
2. **イベントの監視**-アプリは、HealthKit またはアプリの UI (トレーニングを一時停止しているユーザーなど) から生成されたイベントに応答する必要があります。
3. **実行状態を入力**-セッションが開始され、現在実行中です。
4. **一時停止状態を入力**-ユーザーは現在のトレーニングセッションを一時停止し、後で再起動することができます。 ユーザーは、1回のトレーニングセッションで、実行中と一時停止の両方の状態を複数回切り替えることができます。
5. **トレーニングセッションを終了**する-ユーザーがトレーニングセッションを終了できるようになるか、従量制のトレーニング (2 マイルの実行など) であった場合は、有効期限が切れ、独自に終了することができます。

最後の手順は、トレーニングセッションの結果をユーザーの HealthKit データストアに保存することです。

### <a name="observing-healthkit-samples"></a>HealthKit サンプルの観察

アプリは、関心のある HealthKit データポイント (ハートレートやアクティブエネルギーの書き込みなど) ごとに_アンカーオブジェクトクエリ_を開く必要があります。 監視対象のデータポイントごとに、更新ハンドラーを作成して、アプリに送信される新しいデータをキャプチャする必要があります。

これらのデータポイントから、アプリは総実行距離などの合計を累積し、必要に応じてユーザーインターフェイスを更新することができます。 さらに、アプリは、実行の次のマイルの完了など、特定の目標や達成に達したときにユーザーに通知することができます。

次のサンプルコードを見てみましょう。

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

この例では、メソッドを使用してデータを取得する開始日を設定する述語を作成し `GetPredicateForSamples` ます。 この例では、メソッドを使用して HealthKit 情報をプルするデバイスのセットを作成し `GetPredicateForObjectsFromDevices` ます。この場合、ローカル Apple Watch のみ ( `HKDevice.LocalDevice` ) です。 2つの述語は、 `NSCompoundPredicate` メソッドを使用して複合述語 () に結合され `CreateAndPredicate` ます。

目的の `HKAnchoredObjectQuery` データポイント (この場合 `HKQuantityTypeIdentifier.ActiveEnergyBurned` は、アクティブなエネルギー書き込みデータポイント) に対して新しいが作成されます。返されるデータの量に制限はありません ( `HKSampleQuery.NoLimit` )。更新ハンドラーは、HealthKit からアプリに返されるデータを処理するために定義されます。 

更新ハンドラーは、指定されたデータポイントのアプリに新しいデータが配信されるたびに呼び出されます。 エラーが返されない場合、アプリは安全にデータを読み取り、必要な計算を行い、必要に応じて UI を更新できます。

このコードは、配列で返されたすべてのサンプル ( `HKSample` ) `addedObjects` をループし、それらを Quantity サンプル () にキャストし `HKQuantitySample` ます。 次に、サンプルの double 値をジュール () として取得 `HKUnit.Joule` し、トレーニング用に書き込んだアクティブなエネルギーの累計に蓄積して、ユーザーインターフェイスを更新します。

### <a name="achieved-goal-notification"></a>達成目標の通知

前述のように、ユーザーがトレーニングアプリの目標を達成すると (実行の最初のマイルの完了など)、Taptic Engine によってユーザーに haptic フィードバックを送信できます。 また、アプリはこの時点で UI を更新する必要があります。ユーザーが手首を上げて、フィードバックを求めるイベントを表示する可能性が高くなるためです。

Haptic のフィードバックを再生するには、次のコードを使用します。

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>イベントの監視

イベントは、ユーザーのトレーニング中に特定のポイントを強調表示するためにアプリが使用できるタイムスタンプです。 一部のイベントはアプリによって直接作成され、トレーニングに保存され、一部のイベントは HealthKit によって自動的に作成されます。

HealthKit によって作成されたイベントを観察するために、アプリはのメソッドをオーバーライドし `DidGenerateEvent` `HKWorkoutSessionDelegate` ます。

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

Apple では、watchOS 3 に次の新しいイベントの種類が追加されました。

- `HKWorkoutEventType.Lap`-トレーニングを同じ距離部分に分割するイベント用です。 たとえば、実行中にトラックの周りに1つのマークを付けることができます。
- `HKWorkoutEventType.Marker`-トレーニング中の任意のポイントに対応しています。 たとえば、屋外実行のルート上の特定のポイントに到達します。

これらの新しい型は、アプリによって作成され、後でグラフや統計の作成に使用できるように、トレーニングに格納されます。

マーカーイベントを作成するには、次の手順を実行します。

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

このコードは、マーカーイベント () の新しいインスタンスを作成 `HKWorkoutEvent` し、イベントのプライベートコレクション (後でトレーニングセッションに書き込まれる) に保存して、haptics 経由でイベントをユーザーに通知します。

### <a name="pausing-and-resuming-workouts"></a>ワークスペースの一時停止と再開

トレーニングセッションのどの時点でも、ユーザーは一時的にトレーニングを一時停止し、後で再開することができます。 たとえば、室内の実行を一時停止して重要な呼び出しを行い、呼び出しが完了した後に実行を再開することができます。

アプリの UI には、(HealthKit を呼び出すことによって) トレーニングを一時停止および再開する方法が用意されています。これにより、ユーザーがアクティビティを中断している間に Apple Watch、電力とデータ領域の両方を節約できるようになります。 また、トレーニングセッションが一時停止状態のときに受信される可能性のある新しいデータポイントは、アプリによって無視される必要があります。

HealthKit は、pause イベントと Resume イベントを生成することによって、一時停止および再開呼び出しに応答します。 トレーニングセッションが一時停止されている間、セッションが再開されるまで、新しいイベントやデータは HealthKit によってアプリに送信されません。

次のコードを使用して、トレーニングセッションを一時停止および再開します。

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

HealthKit から生成される Pause イベントと Resume イベントは、のメソッドをオーバーライドすることによって処理できます `DidGenerateEvent` `HKWorkoutSessionDelegate` 。

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

### <a name="motion-events"></a>モーションイベント

また、watchOS 3 に新しく追加されました。これは、モーション一時停止 ( `HKWorkoutEventType.MotionPaused` ) イベントとモーション再開 ( `HKWorkoutEventType.MotionResumed` ) イベントです。 これらのイベントは、ユーザーが開始して移動を停止したときに、トレーニングの実行中に HealthKit によって自動的に発生します。

アプリは、モーション一時停止イベントを受け取ると、ユーザーがモーションを再開し、モーション再開イベントが受信されるまで、データの収集を停止する必要があります。 アプリでは、モーション一時停止イベントに応答して、トレーニングセッションを一時停止しないでください。

> [!IMPORTANT]
> モーション一時停止イベントとモーション再開イベントは、RunningWorkout アクティビティタイプ () でのみサポートされてい `HKWorkoutActivityType.Running` ます。

ここでも、これらのイベントは、のメソッドをオーバーライドすることによって処理できます `DidGenerateEvent` `HKWorkoutSessionDelegate` 。

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

## <a name="ending-and-saving-the-workout-session"></a>トレーニングセッションの終了と保存

ユーザーがトレーニングを完了したら、アプリは現在のトレーニングセッションを終了し、HealthKit データベースに保存する必要があります。 HealthKit に保存されたワークスペースは、[トレーニング活動] 一覧に自動的に表示されます。

IOS 10 を初めて利用する場合は、ユーザーの iPhone の [トレーニング活動リスト] リストも含まれます。 したがって、Apple Watch が近くにない場合でも、スマートフォンにトレーニングが表示されます。

エネルギーサンプルを含むワークスペースは、アクティビティアプリのユーザーの移動リングを更新します。これにより、サードパーティのアプリは、ユーザーの日常の移動目標に貢献できるようになります。

トレーニングセッションを終了して保存するには、次の手順を実行する必要があります。

[![トレーニングセッション図の終了と保存](workout-apps-images/workout05.png)](workout-apps-images/workout05.png#lightbox)

1. まず、アプリはトレーニングセッションを終了する必要があります。
2. トレーニングセッションは HealthKit に保存されます。
3. 保存されているトレーニングセッションに、すべてのサンプル (エネルギー書き込みや距離など) を追加します。

### <a name="ending-the-session"></a>セッションを終了しています

トレーニングセッションを終了するには、を渡すのメソッドを呼び出し `EndWorkoutSession` `HKHealthStore` `HKWorkoutSession` ます。

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

これにより、デバイスセンサーが通常モードにリセットされます。 HealthKit がトレーニングを終了すると、のメソッドへのコールバックが返され `DidChangeToState` `HKWorkoutSessionDelegate` ます。

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

アプリがトレーニングセッションを終了したら、トレーニング () を作成 `HKWorkout` し、(イベントと共に) HealthKit データストアに保存する必要があります ( `HKHealthStore` )。

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

このコードによって、トレーニングの合計エネルギー量と、オブジェクトとしてのトレーニングの距離が作成され `HKQuantity` ます。 トレーニングを定義するメタデータのディクショナリが作成され、トレーニングの場所が指定されます。

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

と `HKWorkout` 同じ、 `HKWorkoutActivityType` `HKWorkoutSession` 開始日と終了日、イベントのリスト (上記のセクションから累積される)、書き込み済みエネルギー、合計距離、メタデータディクショナリと同じを使用して、新しいオブジェクトが作成されます。 このオブジェクトは、正常性ストアと処理されたエラーに保存されます。  

### <a name="adding-samples"></a>サンプルの追加

アプリが一連のサンプルをトレーニングに保存すると、HealthKit はサンプルとトレーニングの間の接続を生成します。これにより、アプリは、特定のトレーニングに関連付けられているすべてのサンプルについて、後日 HealthKit にクエリを実行できるようになります。 この情報を使用すると、アプリは、トレーニングデータからグラフを生成し、それらをトレーニングタイムラインにプロットできます。

アプリがアクティビティアプリの移動リングに参加するには、保存したトレーニングを含むエネルギーサンプルが含まれている必要があります。 さらに、距離とエネルギーの合計は、アプリが保存されているトレーニングに関連付けられているサンプルの合計と一致している必要があります。

保存したトレーニングにサンプルを追加するには、次の手順を実行します。

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

必要に応じて、アプリでは、保存されているトレーニングに関連付けられるサンプルの小さなサブセットまたは1つのメガサンプリング (トレーニングの範囲全体にまたがる) を計算して作成できます。

## <a name="workouts-and-ios-10"></a>ワークアウトと iOS 10

WatchOS 3 のすべてのトレーニングアプリには、ios 10 ベースのトレーニングアプリがあります。 iOS 10 を初めて使用する場合は、この iOS アプリを使用して、(ユーザーの介入なしで) トレーニングモードに Apple Watch を配置し、バックグラウンドで実行モードで watchOS アプリを実行するためのトレーニングを開始できます (詳細[については、前述の](#about-background-running)「

WatchOS アプリの実行中は、メッセージングのための WatchConnectivity と、親 iOS アプリとの通信に使用できます。

このプロセスのしくみを見てみましょう。

[![iPhone と Apple Watch 通信の図](workout-apps-images/workout06.png)](workout-apps-images/workout06.png#lightbox)

1. IPhone アプリでは、 `HKWorkoutConfiguration` オブジェクトを作成し、トレーニングの種類と場所を設定します。
2. `HKWorkoutConfiguration`オブジェクトは、Apple Watch バージョンのアプリに送信されます。まだ実行されていない場合は、システムによって開始されます。
3. WatchOS 3 アプリは、渡されたトレーニングトレーニング構成を使用して、新しいトレーニングセッション () を開始し `HKWorkoutSession` ます。

> [!IMPORTANT]
> 親 iPhone アプリが Apple Watch でトレーニングを開始するには、watchOS 3 アプリでバックグラウンドを有効にする必要があります。 詳細については、前の「[バックグラウンドの有効化](#enabling-background-running)」を参照してください。

このプロセスは、watchOS 3 アプリでトレーニングセッションを直接開始するプロセスとよく似ています。 IPhone で、次のコードを使用します。

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

このコードにより、watchOS バージョンのアプリがインストールされ、iPhone のバージョンが最初に接続できるようになります。

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
  ...
}
```

その後、を `HKWorkoutConfiguration` 通常どおりに作成し、のメソッドを使用して `StartWatchApp` `HKHealthStore` Apple Watch に送信し、アプリとトレーニングセッションを開始します。

監視 OS アプリで、で次のコードを使用し `WKExtensionDelegate` ます。

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

を受け取り、 `HKWorkoutConfiguration` 新しいを作成 `HKWorkoutSession` し、カスタムのインスタンスをアタッチし `HKWorkoutSessionDelegate` ます。 トレーニングセッションは、ユーザーの HealthKit Health ストアに対して開始されます。

## <a name="bringing-all-the-pieces-together"></a>すべての要素をまとめて

このドキュメントに記載されているすべての情報を取得するために、watchOS 3 ベースのトレーニングアプリとその親 iOS 10 ベースのトレーニングアプリには、次の部分が含まれます。

1. **iOS 10 `ViewController.cs` **-監視接続セッションの開始を処理し、Apple Watch のトレーニングを行います。
2. **watchOS 3 `ExtensionDelegate.cs` **-WatchOS 3 バージョンのトレーニングアプリを処理します。
3. **watchOS 3 `OutdoorRunDelegate.cs` **- `HKWorkoutSessionDelegate` トレーニングのイベントを処理するカスタム。

> [!IMPORTANT]
> 次のセクションに示すコードには、watchOS 3 のトレーニングアプリに用意されている新しい拡張機能を実装するために必要な部分のみが含まれています。 UI を表示および更新するためのすべてのサポートコードとコードは含まれていませんが、他の watchOS のドキュメントに従って簡単に作成できます。<p/>

### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController.cs`IOS 10 の親であるトレーニングアプリのファイルには、次のコードが含まれています。

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

`ExtensionDelegate.cs`WatchOS 3 バージョンのトレーニングアプリのファイルには、次のコードが含まれています。

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

`OutdoorRunDelegate.cs`WatchOS 3 バージョンのトレーニングアプリのファイルには、次のコードが含まれています。

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

Apple では、watchOS 3 および iOS 10 でトレーニングアプリを設計および実装するときに、次のベストプラクティスを使用することを提案しています。

- IPhone と iOS 10 バージョンのアプリに接続できない場合でも、watchOS 3 のトレーニングアプリが機能していることを確認します。
- Gps が使用できない場合は、gps なしで距離のサンプルを生成できるため、HealthKit distance を使用します。
- Apple Watch または iPhone からトレーニングを開始することをユーザーに許可します。
- 履歴データビュー内の他のソース (他のサードパーティ製アプリなど) からのワークスペースの表示をアプリに許可します。
- 履歴データで、アプリの削除されたワークスペースが表示されないことを確認します。

## <a name="summary"></a>まとめ

この記事では、watchOS 3 でのアプリのトレーニングと Xamarin での実装方法について、Apple が行った機能強化について説明しました。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [HealthKit の概要](~/ios/platform/healthkit.md)
