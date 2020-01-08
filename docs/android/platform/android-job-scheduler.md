---
title: Android ジョブのスケジューラ
description: このガイドでは、Android ジョブスケジューラ API を使用してバックグラウンド作業をスケジュールする方法について説明します。
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/19/2018
ms.openlocfilehash: 4b1e0b32050b22a63bb89b28107877ef3e196b16
ms.sourcegitcommit: 6de849e2feca928ce5d91a3897e7d4049301081c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2020
ms.locfileid: "75667040"
---
# <a name="android-job-scheduler"></a>Android ジョブのスケジューラ

_このガイドでは、android ジョブスケジューラ API を使用してバックグラウンド作業をスケジュールする方法について説明します。 android 5.0 (API レベル 21) 以降を実行している Android デバイスで使用できます。_

## <a name="overview"></a>の概要 

Android アプリケーションをユーザーに応答するための最善の方法の1つは、複雑な作業や長時間実行される作業がバックグラウンドで実行されるようにすることです。 ただし、バックグラウンド作業がデバイスに対するユーザーのエクスペリエンスに悪影響を及ぼさないようにすることが重要です。 

たとえば、バックグラウンドジョブは、3 ~ 4 分ごとに web サイトをポーリングして、特定のデータセットへの変更を照会することができます。 これには問題がないように思えますが、バッテリの寿命に大きな影響があります。 アプリケーションは、デバイスのスリープ状態を解除し、CPU を高い電力状態に昇格させ、無線の電源を入れ、ネットワーク要求を行い、結果を処理します。 デバイスの電源がすぐに切断されず、電力が低下した状態に戻ることがないため、この操作は悪くなります。 適切にスケジュールされていないバックグラウンド作業では、不必要で過剰な電力要件によってデバイスの状態が誤って保持される可能性があります。 この一見無害なアクティビティ (web サイトのポーリング) では、比較的短い時間でデバイスが使用不可能と表示されます。

Android には、バックグラウンドでの作業の実行に役立つ次の Api が用意されていますが、それ自体でインテリジェントなジョブのスケジュール設定には不十分です。 

- **[インテントサービス &ndash; インテント](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** サービスは、作業の実行に適していますが、作業をスケジュールする方法はありません。
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; これらの api では、作業をスケジュールできますが、実際に作業を実行する方法はありません。 また、AlarmManager では時間ベースの制約のみが許可されます。これは、特定の時間または特定の期間が経過した後にアラームを発生させることを意味します。 
- **[ブロードキャストレシーバー](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android アプリでは、システム全体のイベントまたはインテントに応答して作業を実行するようにブロードキャストレシーバーを設定できます。 ただし、ブロードキャストレシーバーでは、ジョブをいつ実行するかを制御することはできません。 また、Android オペレーティングシステムでの変更によって、ブロードキャスト受信側が動作するタイミングや、応答できる作業の種類が制限されます。 

バックグラウンド作業を効率的に実行するには、次の2つの重要な機能があります (_バックグラウンドジョブ_または_ジョブ_と呼ばれることもあります)。

1. **作業をインテリジェントにスケジュール**する &ndash;、アプリケーションがバックグラウンドで作業を行っているときに、優れた市民として作業を行うことが重要です。 アプリケーションでは、ジョブの実行を要求しないことが理想的です。 代わりに、アプリケーションは、ジョブを実行できる条件を指定して、条件が満たされたときに処理を実行するオペレーティングシステムでそのジョブをスケジュールする必要があります。 これにより、Android はジョブを実行して、デバイスの効率を最大限に高めることができます。 たとえば、ネットワーク要求をバッチ処理して、ネットワークに関連するオーバーヘッドを最大限に活用するために、すべてを同時に実行することができます。
2. &ndash;**作業をカプセル**化すると、バックグラウンド作業を実行するコードを、ユーザーインターフェイスとは関係なく実行できる個別のコンポーネントにカプセル化する必要があります。また、何らかの理由で作業が完了しなかった場合に、比較的簡単に再スケジュールすることができます。

Android ジョブスケジューラは、Android オペレーティングシステムに組み込まれているフレームワークであり、バックグラウンド処理のスケジューリングを簡略化するための fluent API を提供します。  Android ジョブスケジューラは、次の種類で構成されています。

- `Android.App.Job.JobScheduler` は、Android アプリケーションに代わってジョブのスケジュール設定、実行、および必要に応じてキャンセルを行うために使用されるシステムサービスです。
- `Android.App.Job.JobService` は、アプリケーションのメインスレッドでジョブを実行するロジックで拡張する必要がある抽象クラスです。 つまり、`JobService` は、作業を非同期に実行する方法を担当します。
- `Android.App.Job.JobInfo` オブジェクトは、ジョブの実行時に Android をガイドするための条件を保持します。

Android ジョブスケジューラを使用して作業をスケジュールするには、Xamarin Android アプリケーションで、`JobService` クラスを拡張するクラスにコードをカプセル化する必要があります。 `JobService` には、ジョブの有効期間中に呼び出すことができるライフサイクルメソッドが3つあります。

- **Bool OnStartJob (JobParameters parameters)** &ndash; このメソッドは、作業を実行するために `JobScheduler` によって呼び出され、アプリケーションのメインスレッドで実行されます。 作業を非同期に実行し、残存作業がある場合は `true` を返し、作業が完了している場合は `false` するのは、`JobService` の役割です。
    
    `JobScheduler` がこのメソッドを呼び出すと、ジョブの期間中、Android から wakelock が要求され、保持されます。 ジョブが完了したら、`JobFinished` メソッド (次に説明します) を呼び出すことによって、このファクトの `JobScheduler` を `JobService` する必要があります。

- **Jobfinished (Jobfinished パラメーター、ブール値の再スケジュール)** &ndash; このメソッドを `JobService` が呼び出して、作業が完了したことを `JobScheduler` に通知する必要があります。 `JobFinished` が呼び出されていない場合、`JobScheduler` は wakelock を削除しないため、不要なバッテリのドレインが発生します。 

- **Bool OnStopJob (JobParameters parameters)** &ndash; これは、Android によってジョブが途中で停止されたときに呼び出されます。 再試行条件に基づいてジョブを再スケジュールする必要がある場合は `true` が返されます (以下で詳細に説明します)。

ジョブをいつ実行できるかを制御する_制約_または_トリガー_を指定することができます。 たとえば、デバイスが充電されたときにのみ実行されるようにジョブを制限したり、画像が取得されたときにジョブを開始したりすることができます。

このガイドでは、`JobService` クラスを実装し、`JobScheduler`でスケジュールを設定する方法について詳しく説明します。

## <a name="requirements"></a>要件

Android ジョブスケジューラには、Android API レベル 21 (Android 5.0) 以降が必要です。 

## <a name="using-the-android-job-scheduler"></a>Android ジョブスケジューラの使用

Android JobScheduler API を使用するには、次の3つの手順を実行します。

1. JobService の種類を実装して作業をカプセル化します。
2. `JobInfo.Builder` オブジェクトを使用して、ジョブを実行する `JobScheduler` の条件を保持する `JobInfo` オブジェクトを作成します。 
3. `JobScheduler.Schedule`を使用してジョブのスケジュールを設定します。

### <a name="implement-a-jobservice"></a>JobService を実装する

Android ジョブスケジューラライブラリによって実行されるすべての作業は、`Android.App.Job.JobService` 抽象クラスを拡張する型で実行する必要があります。 `JobService` の作成は、Android フレームワークで `Service` を作成することとよく似ています。 

1. `JobService` クラスを拡張します。
2. サブクラスを `ServiceAttribute` で修飾し、`Name` パラメーターをパッケージ名とクラス名で構成された文字列に設定します (次の例を参照)。
3. `ServiceAttribute` の `Permission` プロパティを文字列 `android.permission.BIND_JOB_SERVICE`に設定します。
4. `OnStartJob` メソッドをオーバーライドして、作業を実行するコードを追加します。 Android は、ジョブを実行するアプリケーションのメインスレッドでこのメソッドを呼び出します。 アプリケーションがブロックされないようにするために、スレッドに対して数ミリ秒の実行時間がかかる作業です。
5. 作業が完了したら、`JobService` が `JobFinished` メソッドを呼び出す必要があります。 このメソッドは、`JobService` が `JobScheduler` を実行する方法を示しています。 `JobFinished` を呼び出さないと `JobService`、デバイスに不要な要求が発生し、バッテリの寿命が短縮されます。 
6. `OnStopJob` メソッドをオーバーライドすることもお勧めします。 このメソッドは、ジョブが完了する前にシャットダウンされているときに Android によって呼び出され、`JobService` にリソースを適切に破棄する機会を提供します。 このメソッドは、ジョブを再スケジュールする必要がある場合は `true` を返し、ジョブを再実行するために望ましいされていない場合は `false` を返します。

次のコードは、TPL を使用して何らかの処理を非同期に実行するアプリケーションの最も単純な `JobService` の例です。

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

Xamarin Android アプリケーションは、`JobService` を直接インスタンス化しません。代わりに、`JobInfo` オブジェクトを `JobScheduler`に渡します。 `JobScheduler` は、要求された `JobService` オブジェクトをインスタンス化し、`JobInfo`のメタデータに従って `JobService` をスケジュールして実行します。 `JobInfo` オブジェクトには、次の情報が含まれている必要があります。

- **JobId** &ndash; これは、`JobScheduler`に対するジョブを識別するために使用される `int` 値です。 この値を再利用すると、既存のジョブがすべて更新されます。 この値は、アプリケーションに対して一意である必要があります。 
- **Jobservice** &ndash; このパラメーターは、`JobScheduler` がジョブを実行するために使用する必要がある型を明示的に識別する `ComponentName` です。 

この拡張メソッドは、アクティビティなどの Android `Context`で `JobInfo.Builder` を作成する方法を示しています。

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

Android ジョブスケジューラの強力な機能として、ジョブを実行するタイミングや、ジョブが実行される条件を制御できます。 次の表では、ジョブの実行時にアプリが影響を与えるようにする `JobInfo.Builder` のメソッドの一部について説明します。  

|  メソッド | 説明   |
|---|---|
| `SetMinimumLatency`  | ジョブが実行されるまでの待機時間 (ミリ秒単位) を指定します。 |
| `SetOverridingDeadline`  | この時間 (ミリ秒単位) が経過する前にジョブを実行する必要があるを宣言します。 |
| `SetRequiredNetworkType`  | ジョブのネットワーク要件を指定します。 |
| `SetRequiresBatteryNotLow` | このジョブは、デバイスでユーザーに "バッテリ低下" の警告が表示されない場合にのみ実行できます。 |
| `SetRequiresCharging` | このジョブは、バッテリが充電されている場合にのみ実行できます。 |
| `SetDeviceIdle` | デバイスがビジー状態のときにジョブが実行されます。 |
| `SetPeriodic` | ジョブを定期的に実行することを指定します。 |
| `SetPersisted` | このジョブは、デバイスの再起動に perisist 必要があります。 | 

`SetBackoffCriteria` は、`JobScheduler` がジョブの実行を再試行するまでの待機時間に関するガイダンスを提供します。 バックオフ条件には2つの部分があります。遅延時間はミリ秒 (既定値は30秒)、もう1つは使用する必要があります (バックオフ_ポリシー_または_再試行ポリシー_と呼ばれることもあります)。 2つのポリシーは `Android.App.Job.BackoffPolicy` 列挙型にカプセル化されます。

- 指数バックオフポリシーを &ndash; `BackoffPolicy.Exponential` と、エラーが発生するたびに初期バックオフ値が指数関数的に増加します。 ジョブが初めて失敗したとき、ライブラリはジョブの再スケジュールの前に指定されている最初の間隔 (例:30 秒) を待機します。 ジョブが2回目に失敗した場合、ライブラリはジョブの実行を試行する前に少なくとも60秒間待機します。 3回目の試行が失敗すると、ライブラリは120秒ほど待機します。 これは既定値です。
- この方法 &ndash; `BackoffPolicy.Linear`、この方法では、ジョブを (成功まで) 設定された間隔で実行するように再スケジュールする必要があります。 線形バックオフは、可能な限り早く完了する必要がある作業や、自動的に解決される問題に最適です。 

`JobInfo` オブジェクトの作成の詳細については、 [`JobInfo.Builder` クラスに関する Google のドキュメント](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html)を参照してください。

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>JobInfo を使用したジョブへのパラメーターの引き渡し

パラメーターは、`Job.Builder.SetExtras` メソッドと共に渡される `PersistableBundle` を作成することによって、ジョブに渡されます。

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

`PersistableBundle` には、`JobService`の `OnStartJob` メソッドの `Android.App.Job.JobParameters.Extras` プロパティからアクセスします。

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>ジョブのスケジュール設定

ジョブのスケジュールを設定するために、Xamarin アプリケーションは `JobScheduler` システムサービスへの参照を取得し、前の手順で作成した `JobInfo` オブジェクトを使用して `JobScheduler.Schedule` メソッドを呼び出します。 `JobScheduler.Schedule` は、次の2つの整数値のいずれかを使用して直ちにを返します。

- ジョブが正常にスケジュールされた &ndash;、ジョブ**スケジューラ。 ResultSuccess** 。 
- **Jobscheduler. ResultFailure** &ndash; ジョブをスケジュールできませんでした。 これは、通常、`JobInfo` パラメーターの競合が原因で発生します。

このコードは、ジョブをスケジュールし、スケジュールの試行結果をユーザーに通知する例です。

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

スケジュールされたすべてのジョブ、または `JobsScheduler.CancelAll()` 方法または `JobScheduler.Cancel(jobId)` 方法を使用して1つのジョブだけを取り消すことができます。

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>要約

このガイドでは、Android ジョブスケジューラを使用して、バックグラウンドで作業をインテリジェントに実行する方法について説明しました。 ここでは、`JobService` として実行される作業をカプセル化する方法と、`JobScheduler` を使用してその作業をスケジュールする方法、`JobTrigger` で条件を指定する方法、および `RetryStrategy`でエラーを処理する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [インテリジェントなジョブ-スケジュール設定](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler API リファレンス](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [JobScheduler を使用した pro のようなジョブのスケジュール設定](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android のバッテリとメモリの最適化-Google i/o 2016 (ビデオ)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler-René Ruppert](https://www.youtube.com/watch?v=aSjBBPYjelE)
