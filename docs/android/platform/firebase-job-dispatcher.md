---
title: "Firebase ジョブ ディスパッチャー"
description: "このガイドでは、Google から Firebase ジョブ ディスパッチャー ライブラリを使用してバック グラウンド処理をスケジュールする方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/20/2018
ms.openlocfilehash: 20fc48c5d308a64d88308562a18e961c288b5669
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="firebase-job-dispatcher"></a>Firebase ジョブ ディスパッチャー

_このガイドでは、Google から Firebase ジョブ ディスパッチャー ライブラリを使用してバック グラウンド処理をスケジュールする方法について説明します。_

## <a name="overview"></a>概要

複雑なまたは長時間実行される作業をバック グラウンドで実行するために Android アプリケーションのユーザーに応答性を維持する最善の方法の 1 つです。 ただし、これがいるバック グラウンド作業悪影響を与えないことがユーザーのエクスペリエンス、デバイスに重要です。 

たとえば、バック グラウンド ジョブ可能性があります web サイト分ごとにポーリング 3 または 4 つのクエリの特定のデータセットへの変更。 これが、バッテリの寿命障害の原因の影響を与えるには、害のないようです。 アプリケーションは繰り返しデバイスを起動、通常の電力状態に、CPU を昇格させる、無線の電源、ネットワーク要求とし、結果の処理を行います。 これは、デバイスはすぐに電源を切り、低電力アイドル状態に戻すため悪化を取得します。 適切にスケジュールされたバック グラウンド作業は、不要な過剰な電力要件の状態でデバイスを誤って維持できます。 この一見無害であるアクティビティを (ポーリング web サイト) デバイスは比較的短時間で使用できない表示されます。

Android には、バック グラウンドでの作業の実行に役立てるために次の Api が用意されていますが単独でないインテリジェントなジョブ スケジュールを行うための十分なできます。 

* **[インテント サービス](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash;目的としたサービスは、作業をスケジュールする方法を提供されませんが、作業を実行するのに適しています。
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash;これらの Api のみ許可する作業をスケジュールするが、実際に作業を実行する方法を指定しません。 また、AlarmManager は、時間ベースの制約、つまり、特定の時間または一定の時間が経過した後、アラームを発生させるです。 
* **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)**  &ndash;の JobSchedule がジョブのスケジュールを設定するオペレーティング システムで動作する優れた API です。 ただし、のみである以降の API レベル 21 を対象とするそれらの Android アプリで使用可能です。 
* **[レシーバーをブロードキャスト](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android アプリはシステム全体のイベントまたは意図的に応答の作業を実行する放送受信機をセットアップできます。 ただし、放送受信機は、ジョブの実行と制御を提供しません。 Android オペレーティング システムでの変更の制限も放送受信機は機能、またはに対応する作業の種類。 

バック グラウンド作業を効率的に実行する 2 つの主要機能がある (とも呼ば、_バック グラウンド ジョブ_または_ジョブ_)。

1. **作業を適切にスケジューリング**&ndash;を実施する際に、アプリケーションはバック グラウンドでの作業をこれは良き市民として重要です。 理想的には、アプリケーションを要求できません、ジョブを実行します。 代わりに、アプリケーションは、ジョブが実行して、条件が満たされたときに実行するには、その作業をスケジュールするときに満たす必要がある条件を指定する必要があります。 これにより、適切に作業を実行する Android できます。 たとえば、ネットワーク要求の場合は、すべてで最大に活用するためのオーバーヘッド ネットワークに関連する同時に実行にバッチ処理される場合があります。
2. **作業をカプセル化**&ndash;不連続コンポーネント ユーザー インターフェイスとは無関係に実行でき、比較的簡単に作業を完了できない場合に再スケジュールすることで、バック グラウンド処理を実行するコードをカプセル化する必要がありますどうもですね。

Firebase ジョブ ディスパッチャーは、スケジュールのバック グラウンド処理を簡略化する fluent API を提供する、Google からのライブラリです。 これは、Google クラウド マネージャーの置換をするものではします。 Firebase ジョブ ディスパッチャーは、次の Api で構成されます。

* A`Firebase.JobDispatcher.JobService`抽象クラスでバック グラウンド ジョブを実行するロジックで拡張する必要があります。
* A`Firebase.JobDispatcher.JobTrigger`ジョブを開始する必要があるときを宣言します。 これは通常として表現される、時間、枠など、ジョブを開始する前に少なくとも 30 秒間待ってから、5 分以内にジョブを実行します。
* A`Firebase.JobDispatcher.RetryStrategy`適切に動作する、ジョブが失敗した場合に実行する内容に関する情報を格納します。 再試行戦略は、ジョブを再実行する前に待機する時間を指定します。 
* A`Firebase.JobDispatcher.Constraint`が満たす必要がある、ジョブを実行すると、前に、デバイスが unmetered ネットワーク上など、状態を説明する省略可能な値または充電中です。
* `Firebase.JobDispatcher.Job` 、--作業単位でスケジュールできるを以前の Api を統一する API は、`JobDispatcher`です。 `Job.Builder`クラスがインスタンス化に使用する`Job`です。
* A`Firebasee.JobDispatcher.JobDispatcher`前の 3 つの Api を使用して、オペレーティング システムで作業をスケジュールし、必要な場合に、ジョブを取り消す方法を提供します。

Xamarin.Android アプリケーション Firebase ジョブ ディスパッチャーと作業スケジュールの設定、拡張する型のコードをカプセル化する必要があります、`JobService`クラスです。 `JobService` 次の 3 つのライフ サイクル メソッドを呼び出すことのできるジョブの有効期間中に。

* **`bool OnStartJob(IJobParameters parameters)`** &ndash; このメソッドは、作業が発生し、常に実装する必要があります。 これは、メイン スレッドで実行されます。 このメソッドは`true`がある場合、残り時間を操作または`false`の作業が実行される場合。 
* **`bool OnStopJob(IJobParameters parameters)`** &ndash; これは、何らかの理由により、ジョブが停止したときに呼び出されます。 返します`true`場合は、ジョブのスケジュールが後で変更する必要があります。
* **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; このメソッドが呼び出されます、`JobService`が任意の非同期操作を完了します。 

ジョブをスケジュールするには、アプリケーションがインスタンス化され、`JobDispatcher`オブジェクト。 次に`Job.Builder`作成に使用される、`Job`に提供されたオブジェクト、`JobDispatcher`をしておいてくださいを実行するジョブをスケジュールします。

このガイドでは、Firebase ジョブ ディスパッチャーを Xamarin.Android アプリケーションに追加して、バック グラウンド作業のスケジュールを使用する方法を説明します。

## <a name="requirements"></a>要件

Firebase ジョブ ディスパッチャーには、Android API レベル 9 以上が必要です。 Firebase ジョブ ディスパッチャー ライブラリは、Google Play サービス; によって提供される一部のコンポーネントに依存していますデバイスには、Google Play サービスがインストールされている必要があります。

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Xamarin.Android で Firebase ジョブ ディスパッチャー ライブラリを使用します。

Firebase ジョブ ディスパッチャーを開始するには、追加、 [Xamarin.Firebase.JobDispatcher NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher/0.6.0-beta1)Xamarin.Android プロジェクトにします。 NuGet パッケージ マネージャーを検索、 **Xamarin.Firebase.Jobdispatcher**パッケージです。  

Firebase ジョブ ディスパッチャー ライブラリを追加すると、作成、`JobService`クラスし、のインスタンスで実行するようにスケジュールし、`FirebaseJobDispatcher`です。

### <a name="creating-a-jobservice"></a>JobService を作成します。

拡張する型で Firebase ジョブ ディスパッチャー ライブラリによって実行されるすべての作業を行う必要があります、`Firebase.JobDispatcher.JobService`抽象クラスです。 作成する、`JobService`の作成によく似ています、 `Service` Android framework を使用します。 

1. 拡張、`JobService`クラス
2. サブクラスを装飾、`ServiceAttribute`です。 必須ではありませんが推奨を明示的に設定する、`Name`パラメーター デバッグに役立つよう、`JobService`です。 
3. 追加、`IntentFilter`を宣言する、`JobService`で、 **AndroidManifest.xml**です。 Firebase ジョブ ディスパッチャー ライブラリを検索し、呼び出すことができます、`JobService`です。

次のコードは、最も簡単な例`JobService`アプリケーションは、いくつかの作業を非同期的に実行する、TPL を使用します。

```csharp
[Service(Name = "com.xamarin.fjdtestapp.DemoJob")]
[IntentFilter(new[] {FirebaseJobServiceIntent.Action})]
public class DemoJob : JobService
{
    static readonly string TAG = "X:DemoService";

    public override bool OnStartJob(IJobParameters jobParameters)
    {
        Task.Run(() =>
        {
            // Work is happening asynchronously (code omitted)
                       
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // nothing to do.
        return false;
    }
}
```

### <a name="creating-a-firebasejobdispatcher"></a>FirebaseJobDispatcher を作成します。

作成する必要は、作業をスケジュールすることができます、前に、`Firebase.JobDispatcher.FirebaseJobDispatcher`オブジェクト。 `FirebaseJobDispatcher`スケジューリング担当、`JobService`です。 次のコード スニペットは、のインスタンスを作成する方法の 1 つ、 `FirebaseJobDispatcher`: 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

前のコード スニペットに、`GooglePlayDriver`に役立つクラスが、`FirebaseJobDispatcher`デバイスで Google Play サービスのスケジューリング Api の一部と対話します。 パラメーター`context`は任意の Android `Context`、アクティビティなどです。 現在、`GooglePlayDriver`唯一`IDriver`Firebase ジョブ ディスパッチャー ライブラリでの実装です。 

Firebase ジョブ ディスパッチャーの Xamarin.Android バインディングを作成する拡張メソッドを提供する、`FirebaseJobDispatcher`から、 `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

1 回、`FirebaseJobDispatcher`されましたが、インスタンス化可能であればを作成する、`Job`のコードを実行し、`JobService`クラスです。 `Job`によって作成された、`Job.Builder`オブジェクトし、は、次のセクションで説明します。

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Job.Builder、Firebase.JobDispatcher.Job を作成します。

`Firebase.JobDispatcher.Job`メタ データをカプセル化するため、クラスを実行するために必要な`JobService`です。 A`Job`場合、ジョブを実行すると、前に満たしている必要がある、制約などの情報が含まれています、`Job`が反復的なまたはジョブを実行すると、すべてのトリガーです。  最低限、として、`Job`必要があります、_タグ_(にジョブを識別する一意の文字列、 `FirebaseJobDispatcher`) との種類、`JobService`を実行する必要があります。 Firebase ジョブ ディスパッチャーがインスタンス化され、`JobService`ジョブを実行する時間である場合。  A`Job`のインスタンスを使用して作成された、`Firebase.JobDispatcher.Job.JobBuilder`クラスです。 

次のコード スニペットは、作成する方法の最も簡単な例を示します、 `Job` Xamarin.Android バインディングを使用します。

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder`ジョブに対して、入力値に対していくつかの基本的な検証チェックを実行します。 ことがない場合、例外がスローされます、`Job.Builder`を作成する、`Job`です。  `Job.Builder`が作成されます、`Job`次の既定値を使用します。

* A`Job`の_有効期間_(どのくらいの期間にはスケジュールを実行する) が、デバイスが再起動までにのみ&ndash;デバイスが再起動したら、`Job`は失われます。
* A`Job`は繰り返されません&ndash;1 回だけ実行されます。
* A`Job`をできるだけ早く実行するスケジュールされます。
* 既定の再試行戦略、`Job`を使用して、_指数バックオフ_(セクションで、以下の詳細に説明した[、RetryStrategy 設定](#Setting_a_RetryStrategy))

### <a name="scheduling-a-job"></a>ジョブのスケジュール設定

作成した後、 `Job`、スケジュールを設定する必要がある、`FirebaseJobDispatcher`の実行前にします。 スケジュールの 2 つのメソッド、 `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

によって返される値`FirebaseJobDispatcher.Schedule`整数値は次のいずれかになります。

* `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; `Job`正常にスケジュールされました。
* `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; 不明な問題が発生、`Job`でスケジュールします。
* `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; 無効な`IDriver`使用された、または`IDriver`何らかの理由で使用できませんでした。 
* `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; `Trigger`がサポートされていません。
* `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; サービスは、正しく構成されていないまたは使用できません。
 
### <a name="configuring-a-job"></a>ジョブを構成します。

ジョブをカスタマイズすることができます。 ジョブをカスタマイズする方法の例については、次のとおりです。

* [ジョブにパラメーターを渡す](#Passing_Parameters_to_a_Job) &ndash; A`Job`などのファイルのダウンロード、動作を実行するその他の値を必要があります。
* [制約を設定](#Setting_Constraints)&ndash;のみ特定の条件が満たされたときにジョブを実行する必要があります。 たとえば、実行するのみ、`Job`デバイスが充電中です。 
* [いつかを指定、`Job`実行する必要があります](#Setting_Job_Triggers) &ndash; Firebase ジョブ ディスパッチャーにより、アプリケーションは、ジョブの実行時間を指定します。  
* [失敗したジョブの再試行戦略を宣言](#Setting_a_RetryStrategy) &ndash; A_再試行戦略_ガイダンスを提供、`FirebaseJobDispatcher`何を行うには`Jobs`を正常に完了します。 

これらのトピックは、次のセクションで詳しく説明されます。

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-jarameters-to-a-job"></a>ジョブに渡す jarameters

作成することで、ジョブにパラメーターを渡す、`Bundle`と共に渡される、`Job.Builder.SetExtras`メソッド。

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

`Bundle`からアクセス、`IJobParameters.Extras`プロパティを`OnStartJob`メソッド。

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>制約を設定

制約が役立つことができます、デバイス上のコストまたはバッテリのドレインを削減します。 `Firebase.JobDispatcher.Constraint`クラスは、整数値としてこれらの制約を定義します。

* `Constraint.OnUnmeteredNetwork` &ndash; Unmetered ネットワークにデバイスが接続されている場合にのみ、ジョブを実行します。 これは、ユーザーがデータの課金が発生するを防ぐために役立ちます。
* `Constraint.OnAnyNetwork` &ndash; どのようなネットワークにデバイスが接続されているジョブを実行します。 と共に指定されている場合`Constraint.OnUnmeteredNetwork`、この値が優先されます。
* `Constraint.DeviceCharging` &ndash; デバイスが充電中の場合にのみ、ジョブを実行します。

制約が設定されて、`Job.Builder.SetConstraint`メソッド。 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

`JobTrigger`ガイダンスは、ジョブを開始するオペレーティング システムを提供します。 A`JobTrigger`が、_ウィンドウを実行する_時期のスケジュールされた時刻を定義する、`Job`実行する必要があります。 実行ウィンドウに、_ウィンドウを起動_値と_終了ウィンドウ_値。 スタート ウィンドウは、デバイスが、ジョブを実行する前に待機する秒数と、ウィンドウの終了値は、実行する前に待機する秒数の最大数、`Job`です。 

A`JobTrigger`で作成することができます、`Firebase.Jobdispatcher.Trigger.ExecutionWindow`メソッドです。  たとえば`Trigger.ExecutionWindow(15,60)`ジョブは、そのスケジュールから 15 ~ 60 秒間実行することを意味します。 `Job.Builder.SetTrigger`メソッドが目的で使用されます。 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

既定値`JobTrigger`値によって表されるジョブの`Trigger.Now`ジョブがスケジュールされている後にできるだけ早く実行することを指定する.

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>設定、RetryStrategy

`Firebase.JobDispatcher.RetryStrategy`デバイスの遅延の程度を使用する指定を試す前に失敗したジョブを再実行するために使用します。 A`RetryStrategy`が、_ポリシー_、失敗したジョブとジョブをスケジュール設定する必要があります期間を指定する実行ウィンドウを再スケジュールするどの時間ベース アルゴリズムが使用されますを定義します。 これは、_再スケジュール ウィンドウ_2 つの値によって定義されます。 最初の値は、ジョブのスケジュールを変更する前に待機する秒数 (、_初期バックオフ_値)、2 番目の数値は、ジョブを実行する必要があるまでの秒の最大数 (、_最大バックオフ_値)。 
 
2 つの種類の再試行ポリシーは、これらの整数値で識別されます。

* `RetryStrategy.RetryPolicyExponential` &ndash; _指数バックオフ_ポリシーが各エラーの発生後初期バックオフ値を指数関数的に増加されます。 ジョブが失敗すると、最初に、ライブラリの待機は、ジョブのスケジュールを変更する前に指定されている _initial 間隔&ndash;例 30 秒です。 ジョブが失敗すると、2 回目、ライブラリには、ジョブを実行する前に 60 秒以上には待機します。 3 番目の試行に失敗した後、ライブラリは 120 秒間待機しなどです。 既定値`RetryStrategy`Firebase ジョブ ディスパッチャー ライブラリで表されます。、`RetryStrategy.DefaultExponential`オブジェクト。 30 秒の初期のバックオフと 3600 秒の最大バックオフいます。
* `RetryStrategy.RetryPolicyLinear` &ndash; この方法は、_線形バックオフ_(成功) までの間隔を設定で実行するジョブを再スケジュールすることです。 線形バックオフでは、できるだけ早く完了する必要がある作業や自体を迅速に解決する問題に最も適しています。 Firebase ジョブ ディスパッチャー ライブラリが定義されて、`RetryStrategy.DefaultLinear`少なくとも 30 秒の再スケジュール ウィンドウがおよび 3600 秒にアップします。

カスタム定義することは`RetryStrategy`で、`FirebaseJobDispatcher.NewRetryStrategy`メソッドです。 これは、3 つのパラメーターを受け取ります。

1. `int policy` &ndash; _ポリシー_は、前の 1 つ`RetryStrategy`値、 `RetryStrategy.RetryPolicyLinear`、または`RetryStrategy.RetryPolicyExponential`です。
2. `int intialBackoffSeconds` &ndash; _初期バックオフ_的な遅延を秒単位でジョブを再実行する前に必要なはします。 この既定値は 30 秒です。 
3. `int maximumBackoffSeconds` &ndash; _最大バックオフ_値は、ジョブを再実行する前に遅延を秒単位の最大数を宣言します。 既定値は、3600 秒です。 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>ジョブのキャンセル

スケジュールされているすべてのジョブをキャンセルすることまたはだけでは、1 つのジョブを使用して、`FirebaseJobDispatcher.CancelAll()`メソッドまたは`FirebaseJobDispatcher.Cancel(string)`メソッド。

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

いずれかの方法では、整数値を返します。

* `FirebaseJobDispatcher.CancelResultSuccess` &ndash; ジョブが正常に取り消されました。
* `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; エラーでは、ジョブが取り消されてできなくなります。
* `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; `FirebaseJobDispatcher`は有効ながあるため、ジョブをキャンセルできません`IDriver`使用できます。

## <a name="summary"></a>まとめ

このガイドでは、Firebase ジョブ ディスパッチャーを使用して、適切に作業をバック グラウンドで実行する方法について説明します。 として実行される作業をカプセル化する方法が説明されている、`JobService`と使用方法、`FirebaseJobDispatcher`で条件を指定する、その作業をスケジュールする、`JobTrigger`でのエラーの処理方法と、`RetryStrategy`です。


## <a name="related-links"></a>関連リンク

- [NuGet で Xamarin.Firebase.JobDispatcher](https://www.nuget.org/packages/Xamarin.FirebaseJobDispatcher)
- [GitHub の firebase-ジョブのディスパッチャー](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher Binding](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [インテリジェントなジョブ スケジュール](https://developer.android.com/topic/performance/scheduling.html)
- [Android のバッテリとメモリ最適化の Google I/O 2016 (ビデオ)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
