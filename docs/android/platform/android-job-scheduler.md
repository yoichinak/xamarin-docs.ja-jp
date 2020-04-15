---
title: Android ジョブのスケジューラ
description: このガイドでは、Android ジョブ スケジューラ API を利用し、バックグラウンド作業をスケジュールする方法について説明します。
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/19/2018
ms.openlocfilehash: 10d2ae6ac35f02d75ef6e04a0531ec3f5dafd668
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76940816"
---
# <a name="android-job-scheduler"></a>Android ジョブのスケジューラ

_このガイドでは、Android ジョブ スケジューラ API を利用し、バックグラウンド作業をスケジュールする方法について説明します。この API は、Android 5.0 (API レベル 21) 以降を実行している Android デバイスで利用できます。_

## <a name="overview"></a>概要 

ユーザーに対する Android アプリケーションのよい応答性を維持するための最善の方法の 1 つは、複雑な処理や長時間実行される処理がバックグラウンドで実行されるようにすることです。 ただし、バックグラウンド処理がデバイスのユーザー エクスペリエンスに悪影響を及ぼさないようにすることが重要です。 

たとえば、バックグラウンド ジョブでは、3 または 4 分ごとに Web サイトをポーリングして、特定のデータセットへの変更が照会されることがあります。 これは問題ないように思えますが、バッテリの寿命に大きな影響があります。 アプリケーションにより、繰り返し、デバイスがウェイクアップされ、CPU が高い電力状態に引き上げられて、無線の電源が入れられ、ネットワーク要求が行われて、結果が処理されます。 デバイスの電源がすぐに切れて、低電力のアイドル状態に戻ることはないため、この状態はいっそう悪くなります。 バックグラウンド処理が適切にスケジュールされていないと、デバイスが不必要で過剰な電力要件の状態のままになる可能性があります。 この一見無害なアクティビティ (Web サイトのポーリング) により、比較的短い時間でデバイスが使用できなくなります。

Android には、バックグラウンドでの処理の実行に役立つ次の API が用意されていますが、それだけではインテリジェントなジョブのスケジュール設定には不十分です。 

- **[Intent Services](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; Intent Services は、処理の実行には適していますが、作業のスケジュールを設定する方法はありません。
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; これらの API では、処理のスケジュールを設定することだけができ、実際に処理を実行する方法はありません。 また、AlarmManager では時間ベースの制約のみが可能です。これは、特定の時刻または特定の時間経過後にアラームを発生させることを意味します。 
- **[ブロードキャスト レシーバー](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android アプリでは、システム全体のイベントまたはインテントに応答して処理を実行するように、ブロードキャスト レシーバーを設定できます。 ただし、ブロードキャスト レシーバーでは、ジョブをいつ実行するかを制御することはできません。 また、Android オペレーティング システムでの変更によって、ブロードキャスト レシーバーが動作するタイミングや、応答できる処理の種類が制限されます。 

バックグラウンド処理を ("_バックグラウンド ジョブ_" または "_ジョブ_" とも呼ばれます) を効率的に実行するには、次の 2 つの重要な機能があります。

1. **処理のインテリジェントなスケジュール設定** &ndash; バックグラウンドで処理を行うアプリケーションが善良に振る舞うことが重要です。 理想的には、アプリケーションからはジョブの実行を要求するべきではありません。 代わりに、アプリケーションでは、ジョブを実行するために満たす必要がある条件を指定し、条件が満たされたときに作業を実行するオペレーティング システムでそのジョブのスケジュールを設定してください。 これによって、Android では、デバイスで効率性が最大化されるようにジョブを実行できます。 たとえば、ネットワーク要求をバッチ処理してすべてを同時に実行し、ネットワーク関連のオーバーヘッドを最大限に利用することができます。
2. **処理のカプセル化** &ndash; バックグラウンド処理を実行するコードは、ユーザー インターフェイスとは関係なく実行できる個別のコンポーネントにカプセル化する必要があります。それにより、何らかの理由で処理を完了できなかった場合でも、比較的簡単にスケジュールを再設定できます。

Android ジョブ スケジューラは Android オペレーティング システムに組み込まれるフレームワークであり、バックグラウンド作業のスケジュールを簡単にする fluent API を提供します。  Android ジョブ スケジューラは、次の種類で構成されています。

- `Android.App.Job.JobScheduler` は、Android アプリケーションの代わりにジョブをスケジュール、実行、必要に応じてキャンセルするシステム サービスです。
- `Android.App.Job.JobService` は抽象クラスであり、アプリケーションのメイン スレッドでジョブを実行するロジックで拡張する必要があります。 つまり、作業の非同期実行は `JobService` が担います。
- `Android.App.Job.JobInfo` オブジェクトには、ジョブを実行する必要があるとき、Android を誘導するための条件が保持されます。

Android ジョブ スケジューラで作業をスケジュールするには、Xamarin.Android アプリケーションで、`JobService` クラスを拡張するクラスでコードをカプセル化する必要があります。 `JobService` には、ジョブの有効期間中に呼び出すことができるライフサイクル メソッドが 3 つあります。

- **bool OnStartJob(JobParameters パラメーター)** &ndash; このメソッドは作業を実行する `JobScheduler` によって呼び出され、アプリケーションのメイン スレッドで実行されます。 作業を非同期で実行すること、作業が残っている場合に `true` を返すこと、作業が完了している場合に `false` を返すことは `JobService` が行います。
    
    `JobScheduler` はこのメソッドを呼び出すと、Android に wakelock を要求し、ジョブの間保持します。 ジョブが完了したら、`JobFinished` メソッドを呼び出し、この事実を `JobScheduler` に伝えるのは `JobService` が行います (次で説明します)。

- **JobFinished(JobParameters parameters, bool needsReschedule)** &ndash; このメソッドは、作業が完了していることを `JobScheduler` に伝える目的で `JobService` によって呼び出される必要があります。 `JobFinished` が呼び出されない場合、`JobScheduler` は wakelock を削除しないため、不必要にバッテリを消費します。 

- **bool OnStopJob(JobParameters パラメーター)** &ndash; ジョブが Android によって途中で停止されたときにこれが呼び出されます。 再試行条件に基づき、ジョブをスケジュールし直す必要がある場合、`true` を返す必要があります (詳細は次で説明します)。

ジョブを実行できる、あるいはジョブを実行する必要があるタイミングを制御する "_制約_" または "_トリガー_" を指定できます。 たとえば、デバイスの充電中にのみ実行されるようにジョブを制約したり、写真を撮ったときにジョブが開始されるようにすることができます。

このガイドでは、`JobService` クラスを実装し、`JobScheduler` でそれをスケジュールする方法について詳しく説明します。

## <a name="requirements"></a>必要条件

Android ジョブ スケジューラには、Android API レベル 21 (Android 5.0) 以降が必要です。 

## <a name="using-the-android-job-scheduler"></a>Android ジョブ スケジューラの使用

Android ジョブ スケジューラ API は 3 つの手順で使用します。

1. JobService 型を実装し、作業をカプセル化します。
2. `JobInfo.Builder` オブジェクトを使用することで、`JobScheduler` でジョブを実行するための条件を保持する `JobInfo` オブジェクトを作成します。 
3. `JobScheduler.Schedule` を使用してジョブをスケジュールします。

### <a name="implement-a-jobservice"></a>JobService を実装する

Android ジョブ スケジューラ ライブラリによって実行されるすべての処理は、`Android.App.Job.JobService` 抽象クラスを拡張する型で実行される必要があります。 `JobService` の作成は、Android フレームワークでの `Service` の作成とよく似ています。 

1. `JobService` クラスを拡張します。
2. サブクラスを `ServiceAttribute` で修飾し、パッケージ名とクラス名で構成された文字列に `Name` パラメーターを設定します (次の例を参照)。
3. `ServiceAttribute` の `Permission` プロパティを文字列 `android.permission.BIND_JOB_SERVICE` に設定します。
4. `OnStartJob` メソッドをオーバーライドし、作業を実行するコードを追加します。 Android では、ジョブを実行するアプリケーションのメイン スレッドでこのメソッドを呼び出します。 アプリケーションがブロックされることを回避するため、数ミリ秒以上かかる作業はスレッドで実行しなければなりません。
5. 作業が完了したら、`JobService` で `JobFinished` メソッドを呼び出す必要があります。 このメソッドによって、作業が完了したことが `JobService` から `JobScheduler` に伝えられます。 `JobFinished` を呼び出さないと、`JobService` によってデバイスに不要な要求が行われ、バッテリの耐用年数を減らします。 
6. `OnStopJob` メソッドをオーバーライドすることもお勧めします。 このメソッドは、ジョブが終了前にシャットダウンになると呼び出され、リソースを正しく破棄する機会を `JobService` に与えます。 このメソッドは、ジョブを再スケジュールする必要がある場合に `true` を返し、ジョブの再実行が望まれない場合に `false` を返すようにします。

次のコードは、TPL を使用して何らかの処理を非同期に実行する、アプリケーションに対する最も簡単な `JobService` の例です。

```csharp
[Service(Name = "com.xamarin.samples.downloadscheduler.DownloadJob", 
         Permission = "android.permission.BIND_JOB_SERVICE")]
public class DownloadJob : JobService
{
    public override bool OnStartJob(JobParameters jobParams)
    {            
        Task.Run(() =>
        {
            // Work is happening asynchronously
                      
            // Have to tell the JobScheduler the work is done. 
            JobFinished(jobParams, false);
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(JobParameters jobParams)
    {
        // we don't want to reschedule the job if it is stopped or cancelled.
        return false; 
    }
}
```

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>ジョブをスケジュールするための JobInfo の作成

Xamarin.Android アプリケーションでは、`JobService` を直接インスタンス化することがありません。代わりに、`JobInfo` オブジェクトが `JobScheduler` に渡されます。 `JobScheduler` によって、要求された `JobService` オブジェクトがインスタンス化され、`JobInfo` のメタデータに基づいて `JobService` をスケジュールし、実行します。 `JobInfo` オブジェクトには次の情報が含まれる必要があります。

- **JobId** &ndash; これは `JobScheduler` にジョブ ID を知らせる `int` 値です。 この値を再利用すると、既存のジョブがすべて更新されます。 この値は、アプリケーションで一意にする必要があります。 
- **JobService** &ndash; このパラメーターは、ジョブを実行するために `JobScheduler` で使用する型を明示的に識別する `ComponentName` です。 

この拡張メソッドは、アクティビティなど、Android `Context` で `JobInfo.Builder` を作成する方法を示しています。

```csharp
public static class JobSchedulerHelpers
{
    public static JobInfo.Builder CreateJobBuilderUsingJobId<T>(this Context context, int jobId) where T:JobService
    {
        var javaClass = Java.Lang.Class.FromType(typeof(T));
        var componentName = new ComponentName(context, javaClass);
        return new JobInfo.Builder(jobId, componentName);
    }
}

// Sample usage - creates a JobBuilder for a DownloadJob and sets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creates a JobInfo object.
```

Android ジョブ スケジューラの強力な機能として、ジョブを実行するタイミングや、ジョブが実行される条件を制御できます。 次の表には `JobInfo.Builder` で使用されるメソッドの一部を掲載していますが、これらのメソッドを使用することで、ジョブを実行できるタイミングをアプリで制御できます。  

|  メソッド | 説明   |
|---|---|
| `SetMinimumLatency`  | ジョブを実行するまでの待機時間 (ミリ秒単位) を指定します。 |
| `SetOverridingDeadline`  | この時間 (ミリ秒単位) が経過する前にジョブを実行する必要があることを宣言します。 |
| `SetRequiredNetworkType`  | ジョブのネットワーク要件を指定します。 |
| `SetRequiresBatteryNotLow` | このジョブは、デバイスからユーザーに "バッテリ低下" 警告が表示されていないときにのみ実行できます。 |
| `SetRequiresCharging` | ジョブは、バッテリが充電中のときにのみ実行できます。 |
| `SetDeviceIdle` | ジョブは、デバイスがビジー状態のときに実行されます。 |
| `SetPeriodic` | ジョブを定期的に実行するように指定します。 |
| `SetPersisted` | デバイスを再起動してもジョブは存続します。 | 

`SetBackoffCriteria` からは、ジョブの再実行を試行する前に `JobScheduler` に待機する時間についていくつかの指示が与えられます。 バックオフ条件には 2 つの部分があります。ミリ秒単位で指定される遅延 (既定値は 30 秒) と使用すべきバックオフの種類です ("_バックオフ ポリシー_" や "_再試行ポリシー_" と呼ばれることがあります)。 この 2 つのポリシーは `Android.App.Job.BackoffPolicy` 列挙型にカプセル化されます。

- `BackoffPolicy.Exponential` &ndash; エクスポネンシャル バックオフ ポリシーでは、失敗するたびに、初期バックオフ値が指数的に増加します。 ジョブが初めて失敗したときは、ライブラリは、指定されている初回間隔だけ待ってからジョブを再スケジュールします (たとえば 30 秒)。 ジョブが 2 回目に失敗した場合は、ライブラリはジョブの実行を試行する前に少なくとも 60 秒間待機します。 3 回目の試行が失敗すると、ライブラリは 120 秒待機します。以下同様です。 これが既定値です。
- `BackoffPolicy.Linear` &ndash; この戦略は直線的バックオフであり、ジョブの実行は (成功するまで) 設定された一定の間隔で再スケジュールされます。 直線的バックオフは、可能な限り早く完了する必要がある処理や、短時間で自動的に解決される問題に最適です。 

`JobInfo` オブジェクトの作成方法について詳しくは、[`JobInfo.Builder` クラスに関する Google のドキュメント](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html)をお読みください。

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>JobInfo 経由でジョブにパラメーターを渡す

ジョブにパラメーターを渡すには、`Job.Builder.SetExtras` メソッドと共に渡される `PersistableBundle` を作成します。

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

`PersistableBundle` には、`JobService` の `OnStartJob` メソッドの `Android.App.Job.JobParameters.Extras` プロパティからアクセスします。

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>ジョブのスケジュール設定

ジョブをスケジュールするために、Xamarin.Android アプリケーションでは、`JobScheduler` システム サービスの参照を受け取り、前の手順で作成した `JobInfo` オブジェクトを指定して `JobScheduler.Schedule` メソッドを呼び出します。 `JobScheduler.Schedule` が直後に、2 つの整数値のいずれかと共に返されます。

- **JobScheduler.ResultSuccess** &ndash; ジョブは正常にスケジュールされました。 
- **JobScheduler.ResultFailure** &ndash; ジョブはスケジュールできませんでした。 これは通常、`JobInfo` パラメーターの競合で引き起こされます。

次のサンプル コードでは、ジョブをスケジュールし、スケジュール試行の結果をユーザーに通知しています。

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    var snackBar = Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
    snackBar.Show();
}
else
{
    var snackBar = Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
    snackBar.Show();
}
```

### <a name="cancelling-a-job"></a>ジョブの取り消し

`JobsScheduler.CancelAll()` メソッドまたは `JobScheduler.Cancel(jobId)` メソッドを使用して、スケジュール設定されたすべてのジョブまたは 1 つのジョブだけを取り消すことができます。

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>まとめ

このガイドでは、Android ジョブ スケジューラを使用して、バックグラウンドで処理をインテリジェントに実行する方法について説明しました。 ここでは、実行される処理を `JobService` としてカプセル化する方法、`JobScheduler` を使用してその処理のスケジュールを設定する方法、`JobTrigger` で条件を指定する方法、`RetryStrategy` でエラーを処理する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [インテリジェントなジョブ スケジュール設定](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler API リファレンス](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [JobScheduler でプロのようにジョブをスケジュールする](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android のバッテリとメモリの最適化 - Google I/O 2016 (ビデオ)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler - René Ruppert](https://www.youtube.com/watch?v=aSjBBPYjelE)
