---
title: Android のジョブのスケジューラ
description: このガイドでは、Android のジョブ スケジューラ API を使用してバック グラウンド処理をスケジュールする方法について説明します。
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: dc72b7e4da330185b00541f923d9c4b64b91bc95
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2018
ms.locfileid: "30005688"
---
# <a name="android-job-scheduler"></a>Android のジョブのスケジューラ

_Android 5.0 (API レベル 21) を実行している Android デバイスで利用可能な Android ジョブ スケジューラ API を使用してバック グラウンド処理をスケジュールする方法について説明しより高い。_


## <a name="overview"></a>概要 

複雑なまたは長時間実行される作業をバック グラウンドで実行するために Android アプリケーションのユーザーに応答性を維持する最善の方法の 1 つです。 ただし、これがいるバック グラウンド作業悪影響を与えないことがユーザーのエクスペリエンス、デバイスに重要です。 

たとえば、バック グラウンド ジョブ可能性があります web サイト分ごとにポーリング 3 または 4 つのクエリの特定のデータセットへの変更。 これが、バッテリの寿命障害の原因の影響を与えるには、害のないようです。 アプリケーションは繰り返しデバイスを起動、通常の電力状態に、CPU を昇格させる、無線の電源、ネットワーク要求とし、結果の処理を行います。 これは、デバイスはすぐに電源を切り、低電力アイドル状態に戻すため悪化を取得します。 適切にスケジュールされたバック グラウンド作業は、不要な過剰な電力要件の状態でデバイスを誤って維持できます。 この一見無害であるアクティビティを (ポーリング web サイト) デバイスは比較的短時間で使用できない表示されます。

Android には、バック グラウンドでの作業の実行に役立てるために次の Api が用意されていますが単独でないインテリジェントなジョブ スケジュールを行うための十分なできます。 

* **[インテント サービス](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash;目的としたサービスは、作業をスケジュールする方法を提供されませんが、作業を実行するのに適しています。
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash;これらの Api のみ許可する作業をスケジュールするが、実際に作業を実行する方法を指定しません。 また、AlarmManager は、時間ベースの制約、つまり、特定の時間または一定の時間が経過した後、アラームを発生させるです。 
* **[レシーバーをブロードキャスト](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android アプリはシステム全体のイベントまたは意図的に応答の作業を実行する放送受信機をセットアップできます。 ただし、放送受信機は、ジョブの実行と制御を提供しません。 Android オペレーティング システムでの変更の制限も放送受信機は機能、またはに対応する作業の種類。 

バック グラウンド作業を効率的に実行する 2 つの主要機能がある (とも呼ば、_バック グラウンド ジョブ_または_ジョブ_)。

1. **作業を適切にスケジューリング**&ndash;を実施する際に、アプリケーションはバック グラウンドでの作業をこれは良き市民として重要です。 理想的には、アプリケーションを要求できません、ジョブを実行します。 代わりに、アプリケーションでは、ジョブが実行、および条件が満たされたときに、作業を実行するオペレーティング システムとそのジョブをスケジュールするときに満たす必要がある条件を指定する必要があります。 これにより、Android デバイスで最大の効率性を確保するジョブを実行できます。 たとえば、ネットワーク要求の場合は、すべてで最大に活用するためのオーバーヘッド ネットワークに関連する同時に実行にバッチ処理される場合があります。
2. **作業をカプセル化**&ndash;不連続コンポーネント ユーザー インターフェイスとは無関係に実行でき、比較的簡単に作業を完了できない場合に再スケジュールすることで、バック グラウンド処理を実行するコードをカプセル化する必要がありますどうもですね。

Android のジョブのスケジューラは、スケジュールのバック グラウンド処理を簡略化する fluent API を提供する Android オペレーティング システムに組み込まれているフレームワークです。  Android のジョブのスケジューラは、次の種類で構成されます。

* `Android.App.Job.JobScheduler`スケジュール、execute、および必要に応じてジョブを取り消す、Android アプリケーションの代わりに使用されるシステム サービスです。
* `Android.App.Job.JobService`抽象クラスで、アプリケーションのメイン スレッドで、ジョブを実行するロジックで拡張する必要があります。 つまり、`JobService`は非同期に実行される作業の方法を担当します。
* `Android.App.Job.JobInfo`オブジェクトは、ジョブを実行するときに、Android をガイドする基準を格納します。

Xamarin.Android アプリケーション スケジュールするには、Android のジョブのスケジューラでの作業を拡張するクラス内のコードをカプセル化する必要があります、`JobService`クラスです。 `JobService` 次の 3 つのライフ サイクル メソッドを呼び出すことのできるジョブの有効期間中に。

* **bool OnStartJob (JobParameters パラメーター)** &ndash;このメソッドは、`JobScheduler`作業、およびアプリケーションのメイン スレッドで実行されているを実行します。 役割です、`JobService`作業を非同期的に実行して`true`がある場合、残り時間を操作または`false`の作業が実行される場合。
    
    ときに、`JobScheduler`このメソッドを呼び出しますが要求され、ジョブの実行中の Android から wakelock を保持します。 役割は、ジョブが完了すると、`JobService`への通知、`JobScheduler`呼び出しでは、このファクトの`JobFinished`メソッド (次に説明します)。

* **JobFinished (JobParameters パラメーター、bool needsReschedule)** &ndash;このメソッドを呼び出す必要があります、`JobService`への通知、`JobScheduler`の処理を実行します。 場合`JobFinished`は呼び出されません、`JobScheduler`原因で不要なバッテリ ドレイン、wakelock は削除されません。 

* **bool OnStopJob (JobParameters パラメーター)** &ndash; Android によってジョブが停止されてしまうときに呼び出されます。 返します`true`かどうか、ジョブを再スケジュールする (さらに詳しく後述) 再試行の条件を基にします。

指定することは_制約_または_トリガー_ジョブができる、実行する必要がありますを制御します。 たとえば、デバイスが充電中または画像のときにジョブを開始するが実行されるときにのみ実行されるようにジョブを制限することはできます。

このガイドを実装する方法の詳細について説明は、`JobService`クラスし、のスケジュールを設定、`JobScheduler`です。

## <a name="requirements"></a>要件

Android のジョブのスケジューラには、Android API レベル 21 (Android 5.0) が必要がありますまたはそれ以降。 

## <a name="using-the-android-job-scheduler"></a>Android のジョブのスケジューラを使用します。

Android JobScheduler API を使用するための 3 つのステップがあります。

1. 作業をカプセル化する JobService 型を実装します。
2. 使用して、`JobInfo.Builder`を作成するオブジェクト、`JobInfo`の条件を保持するオブジェクト、`JobScheduler`ジョブを実行します。 
3. 使用してジョブをスケジュール`JobScheduler.Schedule`です。

### <a name="implement-a-jobservice"></a>実装する JobService

拡張する型では、ジョブのスケジューラを Android ライブラリによって実行されるすべての作業を行う必要があります、`Android.App.Job.JobService`抽象クラスです。 作成する、`JobService`の作成によく似ています、 `Service` Android framework を使用します。 

1. 拡張、`JobService`クラスです。
2. サブクラスを装飾、`ServiceAttribute`設定と、`Name`パッケージ名とクラスの名前で構成される文字列へのパラメーター (次の例を参照してください)。
3. 設定、`Permission`プロパティを`ServiceAttribute`を文字列に`android.permission.BIND_JOB_SERVICE`です。
4. 上書き、`OnStartJob`メソッド、作業を実行するコードを追加します。 Android では、ジョブを実行するアプリケーションのメイン スレッドに対してこのメソッドを呼び出します。 作業に時間がかかりますスレッドが、アプリケーションがブロックされないようにするには数ミリ秒を実行することをします。
5. 作業が終わったときに、`JobService`呼び出す必要があります、`JobFinished`メソッドです。 このメソッドはどのように`JobService`指示、`JobScheduler`作業を行うことです。 呼び出しに失敗する`JobFinished`になります、`JobService`バッテリの寿命が短く、デバイスへの不要な要求を配置します。 
6. またをオーバーライドすることをお勧め、`OnStopJob`メソッドです。 このメソッドは Android によってジョブがシャット ダウンが完了して提供する前に、`JobService`適切にすべてのリソースの破棄する機会をします。 このメソッドが返す`true`、ジョブを再スケジュールする必要がある場合または`false`望ましい、ジョブを再実行されていない場合。
   

次のコードは、最も簡単な例`JobService`アプリケーションは、いくつかの作業を非同期的に実行する、TPL を使用します。

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

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>ジョブをスケジュールする JobInfo を作成します。

Xamarin.Android アプリケーションをインスタンス化しないで、`JobService`直接、代わりに通過するときは、`JobInfo`オブジェクトを`JobScheduler`です。 `JobScheduler` 、要求されたインスタンスが作成されます`JobService`オブジェクト、スケジュールおよび実行する、`JobService`内のメタデータに基づいて、`JobInfo`です。 A`JobInfo`オブジェクトは、次の情報を含める必要があります。

* **JobId** &ndash;これは、`int`するジョブの識別に使用される値、`JobScheduler`です。 この値を再利用すると、既存のジョブが更新されます。 値は、アプリケーションの一意である必要があります。 
* **JobService** &ndash;このパラメーターは、`ComponentName`を明示的に識別する型を`JobScheduler`ジョブの実行に使用する必要があります。 
  

この拡張メソッドを作成する方法を示しています、 `JobInfo.Builder` 、Android と`Context`、アクティビティなど。

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

// Sample usage - creates a JobBuilder for a DownloadJob andsets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creats a JobInfo object.
```

Android のジョブのスケジューラの強力な機能は、ときに、ジョブを実行または条件ジョブが実行を制御する機能です。 次の表の手法のいくつかの`JobInfo.Builder`ジョブを実行するときに影響するアプリを許可します。  


|  メソッド | 説明   |
|---|---|
| `SetMinimumLatency`  | ジョブの前に監視される遅延 (ミリ秒単位) を実行することを指定します。 |
| `SetOverridingDeadline`  | ジョブを宣言する、この時間 (ミリ秒単位) が経過する前に実行する必要があります。 |
| `SetRequiredNetworkType`  | ジョブのネットワーク要件を指定します。 |
| `SetRequiresBatteryNotLow` | ジョブには、デバイスがユーザーに「バッテリ」警告を表示されていない場合にのみ実行できます。 |
| `SetRequiresCharging` | ジョブには、バッテリの充電場合にのみ実行できます。 |
| `SetDeviceIdle` | ジョブは、デバイスがビジー状態のときに実行されます。 |
| `SetPeriodic` | ジョブを定期的に実行することを指定します。 |
| `SetPersisted` | ジョブでは、デバイスの再起動後に perisist 必要があります。 | 


`SetBackoffCriteria`についてのガイダンスを説明します。 時間、`JobScheduler`ジョブをもう一度実行する前に待機する必要があります。 バックオフの条件に 2 つの部分があります: ために使用されるミリ秒 (30 秒の既定値)、バックオフの種類の遅延 (とも呼ば、_バックオフ ポリシー_または_再試行ポリシー_). 2 つのポリシーにカプセル化、`Android.App.Job.BackoffPolicy`列挙型。

* `BackoffPolicy.Exponential` &ndash; 指数バックオフ ポリシーは、各エラーの発生後初期バックオフ値を指数関数的に大きくは。 ジョブが失敗すると、最初に、ライブラリには、30 秒の使用例 – ジョブのスケジュールを変更する前に指定されている初期間隔は待機します。 ジョブが失敗すると、2 回目、ライブラリには、ジョブを実行する前に 60 秒以上には待機します。 3 番目の試行に失敗した後、ライブラリは 120 秒間待機しなどです。 これが既定値です。
* `BackoffPolicy.Linear` &ndash; この方法は、線形バックオフにジョブのスケジュールが変更する必要があります (これが成功すると) までに設定された間隔で実行します。 線形バックオフでは、できるだけ早く完了する必要がある作業や自体を迅速に解決する問題に最も適しています。 

詳細についてを作成する、`JobInfo`オブジェクト、お読みください[Google のドキュメントを`JobInfo.Builder`クラス](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html)です。

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>パラメーター、JobInfo 経由でジョブを渡す

作成することで、ジョブにパラメーターを渡す、`PersistableBundle`と共に渡される、`Job.Builder.SetExtras`メソッド。

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

`PersistableBundle`からアクセス、`Android.App.Job.JobParameters.Extras`プロパティに、`OnStartJob`のメソッド、 `JobService`:

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>ジョブのスケジュール設定

ジョブをスケジュールする Xamarin.Android アプリケーションがへの参照を取得、 `JobScheduler` system サービスとの呼び出し、`JobScheduler.Schedule`メソッドを`JobInfo`前の手順で作成されたオブジェクト。 `JobScheduler.Schedule` 2 つの整数値のいずれかですぐに返されます。

* **JobScheduler.ResultSuccess** &ndash;ジョブは正常にスケジュールされています。 
* **JobScheduler.ResultFailure** &ndash;ジョブをスケジュールできませんでした。 競合している原因は通常`JobInfo`パラメーター。

このコードは、ジョブのスケジュールとスケジュールの試行の結果のユーザーへの通知の例を示します。

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

スケジュールされているすべてのジョブをキャンセルすることまたはだけでは、1 つのジョブを使用して、`JobsScheduler.CancelAll()`メソッドまたは`JobScheduler.Cancel(jobId)`メソッド。

```csharp
// Cancel all jobs
jobSchduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>まとめ

このガイドでは、Android のジョブのスケジューラを使用して、適切に作業をバック グラウンドで実行する方法について説明します。 として実行される作業をカプセル化する方法が説明されている、`JobService`と使用方法、`JobScheduler`で条件を指定する、その作業をスケジュールする、`JobTrigger`でのエラーの処理方法と、`RetryStrategy`です。

## <a name="related-links"></a>関連リンク

- [インテリジェントなジョブ スケジュール](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler API リファレンス](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [JobScheduler にプロのようにジョブのスケジュール設定](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android のバッテリとメモリ最適化の Google I/O 2016 (ビデオ)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler - René Ruppert - Xamarin 大学](https://www.youtube.com/watch?v=aSjBBPYjelE)