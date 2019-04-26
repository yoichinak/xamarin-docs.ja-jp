---
title: Android ジョブのスケジューラ
description: このガイドでは、Android のジョブ スケジューラ API を使用してバック グラウンド作業をスケジュールする方法について説明します。
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/19/2018
ms.openlocfilehash: c0f638afbf044a2e3e6f309839cb22137cf95912
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60956284"
---
# <a name="android-job-scheduler"></a>Android ジョブのスケジューラ

_Android 5.0 (API レベル 21) を実行している Android デバイスで使用できるは Android ジョブ スケジューラ API を使用してバック グラウンド作業をスケジュールする方法について説明しより高い。_


## <a name="overview"></a>概要 

Android アプリケーションのユーザーに応答性を維持する最善の方法の 1 つは、複雑なまたは実行時間の長い作業をバック グラウンドで実行するためにです。 ただし、これことバック グラウンド作業が影響を与えないユーザー エクスペリエンスのデバイスに重要です。 

たとえば、バック グラウンド ジョブ可能性があります web サイト分ごとにポーリング 3、4 クエリの特定のデータセットへの変更。 これは、バッテリの寿命悲惨な影響があるただし、問題のないようです。 アプリケーションは繰り返しウェイク アップ、デバイス、CPU を通常の電力状態に昇格、無線の電源、ネットワーク要求と、結果を処理します。 すぐには、デバイスの電源を切り、低電力アイドル状態に戻るために取得します。 適切にスケジュールされたバック グラウンド作業は、デバイスが不要なと過剰な能力の要件の状態にしてください誤って可能性があります。 (Web サイトのポーリング) この一見無害なアクティビティ、デバイスは比較的短時間で使用できないレンダリングされます。

Android は、バック グラウンドで作業を実行すると、次の Api を提供しますが単独ではありませんインテリジェントなジョブのスケジュール設定のための十分です。 

* **[インテント サービス](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash;目的としたサービスは、作業をスケジュールする方法を提供しないが、作業を実行するのに最適です。
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash;これらの Api はスケジュールが実際には、作業を実行する方法を指定しない作業のみを許可します。 また、AlarmManager のみを許可時間ベースの制約、特定の時間または一定の時間が経過した後にアラートを発行することを意味します。 
* **[ブロードキャスト レシーバー](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; An Android アプリは、システム全体のイベントまたはインテントへの応答で作業を実行するブロードキャスト レシーバーを設定できます。 ただし、ブロードキャスト レシーバーは、ジョブの実行と制御を提供しません。 Android オペレーティング システムの変更の制限もブロードキャスト レシーバーは機能、またはに対応する作業の種類。 

バック グラウンド作業を効率的に実行する 2 つの主要機能がある (とも呼ば、_バック グラウンド ジョブ_または_ジョブ_)。

1. **作業を適切にスケジューリング**&ndash;を行う際に、アプリケーションがバック グラウンドでの作業は良き市民として重要です。 理想的には、アプリケーションを要求するジョブを実行します。 代わりに、アプリケーションでは、ジョブが実行、および条件が満たされたときに、作業を実行するオペレーティング システムでは、そのジョブをスケジュールし、ときに満たす必要がある条件を指定する必要があります。 これにより、Android デバイスで最大の効率性を確保するジョブを実行できます。 たとえば、ネットワーク要求は、ネットワークに関連するオーバーヘッドの最大使用数を同時にすべてを実行するバッチ処理可能性があります。
2. **作業をカプセル化する**&ndash;バック グラウンド処理を実行するコードは、ユーザー インターフェイスとは無関係に実行でき、作業を完了できない場合に再スケジュールするは比較的簡単になりますが、個別のコンポーネントにカプセル化する必要がありますどうもですね。

Android のジョブ スケジューラは、スケジュールのバック グラウンド作業を簡略化に fluent API を提供する Android オペレーティング システムに組み込まれているフレームワークです。  Android のジョブ スケジューラは、次の種類で構成されます。

* `Android.App.Job.JobScheduler`はスケジュールを設定し、実行、および、必要に応じてジョブの取り消し、Android アプリケーションの代わりに使用されるシステム サービスです。
* `Android.App.Job.JobService`は、アプリケーションのメイン スレッドで、ジョブを実行するロジックを使用して拡張する必要がある抽象クラスです。 つまり、`JobService`は非同期的に実行する作業が責任を負います。
* `Android.App.Job.JobInfo`オブジェクトは、ジョブを実行するときに、Android をガイドする基準を保持します。

Android のジョブ スケジューラで作業をスケジュールするを拡張するクラスのコードで Xamarin.Android アプリケーションによってカプセル化する必要があります、`JobService`クラス。 `JobService` ジョブの有効期間中に呼び出すことができる 3 つのライフ サイクルの方法があります。

* **bool OnStartJob (JobParameters パラメーター)** &ndash;このメソッドを呼び出して、`JobScheduler`作業、およびアプリケーションのメイン スレッドで実行を実行します。 役割です、`JobService`非同期的に作業を実行して`true`がある場合の動作の残りまたは`false`場合は、作業が行われます。
    
    ときに、`JobScheduler`このメソッドを呼び出す要求され、ジョブの実行中の android の wakelock を保持します。 役割は、ジョブが完了したら、`JobService`への通知、`JobScheduler`呼び出しによってこの事実、`JobFinished`メソッド (次に説明します)。

* **(JobParameters パラメーター、bool needsReschedule) JobFinished** &ndash; 、このメソッドを呼び出す必要があります、`JobService`への通知、`JobScheduler`作業が行われます。 場合`JobFinished`は呼び出されません、`JobScheduler`不要なバッテリの消耗を原因と、wakelock は削除されません。 

* **bool OnStopJob (JobParameters パラメーター)** &ndash; Android によってジョブが停止されてしまうときに呼び出されます。 返されます`true`(さらに詳しく後述) の再試行条件に基づいて、ジョブをスケジュールするかどうか。

指定することは_制約_または_トリガー_制御するジョブができるかを実行する必要があります。 たとえば、デバイスを充電するか、または画像のときにジョブを開始する運用時にのみ実行されるようにジョブを制限することは。

このガイドで取り上げる詳細を実装する方法、`JobService`クラスし、のスケジュールを設定、`JobScheduler`します。

## <a name="requirements"></a>必要条件

Android のジョブ スケジューラには、Android API レベル 21 (Android 5.0) が必要がありますまたはそれ以降。 

## <a name="using-the-android-job-scheduler"></a>Android ジョブ スケジューラを使用します。

Android の JobScheduler API を使用するための 3 つの手順があります。

1. 作業をカプセル化する JobService 型を実装します。
2. 使用して、`JobInfo.Builder`オブジェクトを作成する、`JobInfo`の基準を保持するオブジェクト、`JobScheduler`ジョブを実行します。 
3. 使用してジョブをスケジュール`JobScheduler.Schedule`します。

### <a name="implement-a-jobservice"></a>実装を JobService

拡張する型では、ジョブ スケジューラの Android ライブラリによって実行されるすべての作業を行う必要がある、`Android.App.Job.JobService`抽象クラス。 作成、`JobService`作成によく似ていますが、 `Service` Android フレームワークと。 

1. 拡張、`JobService`クラス。
2. サブクラスを装飾、`ServiceAttribute`設定と、`Name`パッケージ名とクラスの名前で構成される文字列のパラメーター (次の例を参照してください)。
3. 設定、`Permission`プロパティを`ServiceAttribute`を文字列に`android.permission.BIND_JOB_SERVICE`します。
4. 上書き、`OnStartJob`メソッド、作業を実行するコードを追加します。 Android では、ジョブを実行するアプリケーションのメイン スレッドでは、このメソッドを呼び出します。 かかる時間が長く、数ミリ秒、アプリケーションがブロックされないように、スレッドで実行することを作業します。
5. 作業が完了したら、`JobService`呼び出す必要があります、`JobFinished`メソッド。 このメソッドは、どのように`JobService`指示、`JobScheduler`作業が完了しました。 呼び出しに失敗する`JobFinished`なります、`JobService`バッテリの寿命を短く、デバイスへの不要な要求を配置します。 
6. オーバーライドすることをお勧め、`OnStopJob`メソッド。 終了して提供する前に、ジョブをシャット ダウンされている、ときに、Android によってこのメソッドが呼び出されます、`JobService`を適切にすべてのリソースの破棄できます。 このメソッドが返す必要があります`true`、ジョブのスケジュールを変更する必要がある場合または`false`望ましいジョブを再実行されていない場合。
   

次のコードは、最も簡単な例`JobService`アプリケーションは、TPL を使用していくつかの作業を非同期で実行します。

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

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>ジョブをスケジュールする、JobInfo を作成します。

Xamarin.Android アプリケーションをインスタンス化しないで、`JobService`渡しますが代わりに、直接、`JobInfo`オブジェクトを`JobScheduler`します。 `JobScheduler` 、要求されたインスタンスが作成されます`JobService`スケジュールおよび実行するときに、オブジェクト、`JobService`内のメタデータに基づいて、`JobInfo`します。 A`JobInfo`オブジェクトは、次の情報を含める必要があります。

* **JobId** &ndash;これは、`int`するジョブを識別するために使用される値、`JobScheduler`します。 この値を再利用すると、任意の既存のジョブが更新されます。 値は、アプリケーションの一意である必要があります。 
* **JobService** &ndash;このパラメーターは、`ComponentName`型を明示的に識別する、`JobScheduler`ジョブの実行に使用する必要があります。 
  

この拡張メソッドは、作成する方法を示します、 `JobInfo.Builder` Android で`Context`などのアクティビティ。

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

Android のジョブ スケジューラの強力な機能と、ジョブを実行または条件、ジョブが実行を制御する機能があります。 次の表は、メソッドの一部をについて説明します`JobInfo.Builder`アプリに影響するは、ジョブを実行できるようにします。  


|  メソッド | 説明   |
|---|---|
| `SetMinimumLatency`  | ジョブの前に監視する遅延 (ミリ秒単位) を実行することを指定します。 |
| `SetOverridingDeadline`  | ジョブを宣言する、ミリ秒単位では、この期間が経過する前に実行する必要があります。 |
| `SetRequiredNetworkType`  | ジョブのネットワーク要件を指定します。 |
| `SetRequiresBatteryNotLow` | ジョブは、デバイスをユーザーに「バッテリ」警告も表示されていない場合にのみ実行できます。 |
| `SetRequiresCharging` | ジョブは、バッテリの充電中のみ実行できます。 |
| `SetDeviceIdle` | ジョブは、デバイスがビジー状態のときに実行されます。 |
| `SetPeriodic` | ジョブを定期的に実行することを指定します。 |
| `SetPersisted` | ジョブには、perisist デバイスの再起動後も維持する必要があります。 | 


`SetBackoffCriteria`はいくつか紹介しますが、時間の長い`JobScheduler`ジョブをもう一度実行する前に待機する必要があります。 2 つの部分にバックオフ条件: ミリ秒 (30 秒の既定値)、バックオフの種類の遅延で、使用する必要があります (とも呼ば、_バックオフ ポリシー_または_再試行ポリシー_). 2 つのポリシーにカプセル化、`Android.App.Job.BackoffPolicy`列挙型。

* `BackoffPolicy.Exponential` &ndash; 指数関数的バックオフ ポリシーでは、各障害の発生後、初期バックオフ値を指数関数的に大きくは。 ジョブが失敗した場合、最初に、ライブラリには、30 秒の使用例 – ジョブのスケジュールを変更する前に指定されている初期間隔は待機します。 2 回目のジョブが失敗すると、ライブラリには、ジョブを実行する前に、少なくとも 60 秒間は待機します。 試行が 3 つ目に失敗した後、ライブラリは、120 秒待ってし、具合です。 これが既定値です。
* `BackoffPolicy.Linear` &ndash; この方法は、ジョブを再スケジュールする必要がありますが、線形バックオフ (成功) するまでに設定された間隔で実行します。 線形バックオフでは、できるだけ早く完了する必要がある作業や自体を迅速に解決する問題に最適です。 

詳細についてを作成、`JobInfo`オブジェクト、お読みください[Google のドキュメントを`JobInfo.Builder`クラス](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html)。

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>JobInfo 経由でのジョブにパラメーターを渡す

作成して、ジョブに渡されるパラメーターを`PersistableBundle`と共に渡される、`Job.Builder.SetExtras`メソッド。

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

`PersistableBundle`からアクセスは、`Android.App.Job.JobParameters.Extras`プロパティ、`OnStartJob`のメソッド、 `JobService`:

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>ジョブをスケジュールします。

ジョブをスケジュールするには、Xamarin.Android アプリケーションがへの参照を取得、 `JobScheduler` system サービスと呼び出し、`JobScheduler.Schedule`メソッドを`JobInfo`前の手順で作成されたオブジェクト。 `JobScheduler.Schedule` 2 つの整数値のいずれかですぐに返されます。

* **JobScheduler.ResultSuccess** &ndash;ジョブが正常にスケジュールされています。 
* **JobScheduler.ResultFailure** &ndash;ジョブをスケジュールできませんでした。 これは通常の競合が原因`JobInfo`パラメーター。

このコードは、ジョブのスケジュールとスケジュールの試行の結果のユーザーに通知の例を示します。

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
}
else
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
}
```
 
### <a name="cancelling-a-job"></a>ジョブのキャンセル

スケジュールされているすべてのジョブを取り消すことができるかを使用して、1 つのジョブ、`JobsScheduler.CancelAll()`メソッドまたは`JobScheduler.Cancel(jobId)`メソッド。

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>まとめ

このガイドでは、Android のジョブ スケジューラを使用して、インテリジェントなバック グラウンドで作業を実行する方法について説明します。 として実行される作業をカプセル化する方法が説明されている、`JobService`と使用方法、`JobScheduler`で条件を指定する、その作業をスケジュールする、`JobTrigger`でエラーを処理する方法と、`RetryStrategy`します。

## <a name="related-links"></a>関連リンク

- [インテリジェントなジョブ スケジュール](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler API リファレンス](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [JobScheduler でプロのようなジョブのスケジュール](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android のバッテリとメモリの最適化 - Google I/O 2016 (ビデオ)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler - René Ruppert - Xamarin University](https://www.youtube.com/watch?v=aSjBBPYjelE)
