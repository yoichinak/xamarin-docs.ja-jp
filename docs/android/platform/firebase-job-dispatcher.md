---
title: Firebase ジョブ ディスパッチャー
description: このガイドでは、Google の Firebase のジョブディスパッチャーライブラリを使用して、バックグラウンド作業をスケジュールする方法について説明します。
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: 280fe11f935db0a364f3342b22bb9544cdda1e6d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020247"
---
# <a name="firebase-job-dispatcher"></a>Firebase ジョブ ディスパッチャー

_このガイドでは、Google の焼討ベースのジョブディスパッチャーライブラリを使用して、バックグラウンド作業をスケジュールする方法について説明します。_

## <a name="overview"></a>概要

Android アプリケーションをユーザーに応答するための最善の方法の1つは、複雑な作業や長時間実行される作業がバックグラウンドで実行されるようにすることです。 ただし、バックグラウンド作業がデバイスに対するユーザーのエクスペリエンスに悪影響を及ぼさないようにすることが重要です。 

たとえば、バックグラウンドジョブは、3 ~ 4 分ごとに web サイトをポーリングして、特定のデータセットへの変更を照会することができます。 これには問題がないように思えますが、バッテリの寿命に大きな影響があります。 アプリケーションは、デバイスのスリープ状態を解除し、CPU を高い電力状態に昇格させ、無線の電源を入れ、ネットワーク要求を行い、結果を処理します。 デバイスの電源がすぐに切断されず、電力が低下した状態に戻ることがないため、この操作は悪くなります。 適切にスケジュールされていないバックグラウンド作業では、不必要で過剰な電力要件によってデバイスの状態が誤って保持される可能性があります。 この一見無害なアクティビティ (web サイトのポーリング) では、比較的短い時間でデバイスが使用不可能と表示されます。

Android には、バックグラウンドでの作業の実行に役立つ次の Api が用意されていますが、それ自体でインテリジェントなジョブのスケジュール設定には不十分です。 

- **[インテントサービス &ndash; インテント](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** サービスは、作業の実行に適していますが、作業をスケジュールする方法はありません。
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; これらの api では、作業をスケジュールできますが、実際に作業を実行する方法はありません。 また、AlarmManager では時間ベースの制約のみが許可されます。これは、特定の時間または特定の期間が経過した後にアラームを発生させることを意味します。 
- **[Jobscheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)** &ndash; jobscheduler は、オペレーティングシステムと連携してジョブをスケジュールする優れた API です。 ただし、API レベル21以上を対象とする Android アプリでのみ使用できます。 
- **[ブロードキャストレシーバー](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android アプリでは、システム全体のイベントまたはインテントに応答して作業を実行するようにブロードキャストレシーバーを設定できます。 ただし、ブロードキャストレシーバーでは、ジョブをいつ実行するかを制御することはできません。 また、Android オペレーティングシステムでの変更によって、ブロードキャスト受信側が動作するタイミングや、応答できる作業の種類が制限されます。 

バックグラウンド作業を効率的に実行するには、次の2つの重要な機能があります (_バックグラウンドジョブ_または_ジョブ_と呼ばれることもあります)。

1. **作業をインテリジェントにスケジュール**する &ndash;、アプリケーションがバックグラウンドで作業を行っているときに、優れた市民として作業を行うことが重要です。 アプリケーションでは、ジョブの実行を要求しないことが理想的です。 代わりに、アプリケーションでは、ジョブを実行できるときに満たす必要がある条件を指定し、条件が満たされたときに実行するようにスケジュールを設定する必要があります。 これにより、Android は作業をインテリジェントに実行できます。 たとえば、ネットワーク要求をバッチ処理して、ネットワークに関連するオーバーヘッドを最大限に活用するために、すべてを同時に実行することができます。
2. &ndash;**作業をカプセル**化することで、バックグラウンド作業を実行するコードを、ユーザーインターフェイスとは無関係に実行できる個別のコンポーネントにカプセル化する必要があります。また、作業の完了に失敗した場合には、比較的簡単に再スケジュールすることができます。理由.

Firebase ジョブディスパッチャーは、fluent API を提供する Google のライブラリであり、バックグラウンド処理のスケジューリングを簡略化します。 これは、Google Cloud Manager の後継となることを目的としています。 Firebase ジョブディスパッチャーは、次の Api で構成されています。

- `Firebase.JobDispatcher.JobService` は、バックグラウンドジョブで実行されるロジックで拡張する必要がある抽象クラスです。
- ジョブをいつ開始するかを `Firebase.JobDispatcher.JobTrigger` が宣言します。 通常、これは時間枠として表現されます。たとえば、ジョブを開始する前に少なくとも30秒待機しますが、5分以内にジョブを実行します。
- `Firebase.JobDispatcher.RetryStrategy` には、ジョブが正常に実行されなかった場合に実行する処理に関する情報が含まれています。 再試行戦略では、ジョブの実行を再試行するまでの待機時間を指定します。 
- `Firebase.JobDispatcher.Constraint` は、ジョブを実行する前に満たす必要のある条件を記述するオプションの値です。たとえば、デバイスが従量制のネットワークまたは充電されていない場合などです。
- `Firebase.JobDispatcher.Job` は、の以前の Api を、`JobDispatcher`によってスケジュールできる作業単位に統合する API です。 `Job.Builder` クラスは、`Job`をインスタンス化するために使用されます。
- `Firebase.JobDispatcher.JobDispatcher` は、前の3つの Api を使用して、オペレーティングシステムでの作業のスケジュールを設定し、必要に応じてジョブを取り消す方法を提供します。

Firebase のジョブディスパッチャーを使用して作業をスケジュールするには、Xamarin Android アプリケーションで、`JobService` クラスを拡張する型にコードをカプセル化する必要があります。 `JobService` には、ジョブの有効期間中に呼び出すことができるライフサイクルメソッドが3つあります。

- **`bool OnStartJob(IJobParameters parameters)`** &ndash; このメソッドは、作業が発生し、常に実装する必要があります。 メインスレッドで実行されます。 このメソッドは、残存作業がある場合は `true` を返し、作業が完了した場合は `false` します。 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash; これは、何らかの理由でジョブが停止されたときに呼び出されます。 後でジョブを再スケジュールする必要がある場合は `true` が返されます。
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; このメソッドは、`JobService` が非同期処理を完了したときに呼び出されます。 

ジョブをスケジュールするために、アプリケーションは `JobDispatcher` オブジェクトをインスタンス化します。 次に、`Job.Builder` を使用して `Job` オブジェクトを作成します。このオブジェクトは、実行するジョブのスケジュールを設定する `JobDispatcher` に提供されます。

このガイドでは、Firebase ジョブディスパッチャーを Xamarin Android アプリケーションに追加し、それを使用してバックグラウンド作業をスケジュールする方法について説明します。

## <a name="requirements"></a>［要件］

Firebase ジョブディスパッチャーには、Android API レベル9以上が必要です。 焼討 Base ジョブディスパッチャーライブラリは、Google Play 開発者サービスによって提供されるいくつかのコンポーネントに依存しています。デバイスに Google Play 開発者サービスがインストールされている必要があります。

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Xamarin. Android での焼討 Base ジョブディスパッチャーライブラリの使用

焼討ベースのジョブディスパッチャーの使用を開始するには、最初に xamarin. の[NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)を Xamarin. Android プロジェクトに追加します。 NuGet パッケージマネージャーを検索します。このパッケージは、まだプレリリース段階**にあります**。

Firebase ジョブディスパッチャーライブラリを追加した後、`JobService` クラスを作成し、`FirebaseJobDispatcher`のインスタンスを使用して実行するようにスケジュールを設定します。

### <a name="creating-a-jobservice"></a>JobService の作成

Firebase ジョブディスパッチャーライブラリによって実行されるすべての作業は、`Firebase.JobDispatcher.JobService` 抽象クラスを拡張する型で実行する必要があります。 `JobService` の作成は、Android フレームワークで `Service` を作成することとよく似ています。 

1. `JobService` クラスを拡張する
2. サブクラスを `ServiceAttribute`で修飾します。 厳密には必須ではありませんが、`JobService`のデバッグに役立つように `Name` パラメーターを明示的に設定することをお勧めします。 
3. **Androidmanifest .xml**で `JobService` を宣言する `IntentFilter` を追加します。 これにより、`JobService`を特定して呼び出すこともできます。

次のコードは、TPL を使用して何らかの処理を非同期に実行するアプリケーションの最も単純な `JobService` の例です。

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

### <a name="creating-a-firebasejobdispatcher"></a>Firebasejobdispatcher の作成

作業をスケジュールする前に、`Firebase.JobDispatcher.FirebaseJobDispatcher` オブジェクトを作成する必要があります。 `FirebaseJobDispatcher` は、`JobService`をスケジュールする役割を担います。 次のコードスニペットは、`FirebaseJobDispatcher`のインスタンスを作成する方法の1つです。 

 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

前のコードスニペットでは、`GooglePlayDriver` は、`FirebaseJobDispatcher` がデバイス上の Google Play 開発者サービスのいくつかのスケジュール Api と対話できるようにするクラスです。 パラメーター `context` は、アクティビティなどの Android `Context`です。 現時点では、`GooglePlayDriver` は、Firebase ジョブディスパッチャーライブラリの唯一の実装 `IDriver` です。 

焼討ベースジョブディスパッチャーの Xamarin. Android バインドは、`Context`から `FirebaseJobDispatcher` を作成するための拡張メソッドを提供します。 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

`FirebaseJobDispatcher` がインスタンス化されると、`Job` を作成し、`JobService` クラスでコードを実行できるようになります。 `Job` は `Job.Builder` オブジェクトによって作成され、次のセクションで説明します。

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>このジョブを使用して、JobDispatcher. ジョブを作成しています。

`Firebase.JobDispatcher.Job` クラスは、`JobService`を実行するために必要なメタデータをカプセル化します。 @ No__t_0_ には、ジョブを実行する前に満たす必要がある制約などの情報、`Job` が定期的に行われる場合、またはジョブが実行されるトリガーが含まれます。  最小要件として、`Job` には_タグ_(ジョブを `FirebaseJobDispatcher`に識別する一意の文字列) と実行する `JobService` の種類が必要です。 このジョブを実行する時間になると、Firebase ジョブディスパッチャーによって `JobService` がインスタンス化されます。  `Job` は、`Firebase.JobDispatcher.Job.JobBuilder` クラスのインスタンスを使用して作成されます。 

次のコードスニペットは、Xamarin. Android バインドを使用して `Job` を作成する方法の最も簡単な例です。

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` は、ジョブの入力値に対していくつかの基本的な検証チェックを実行します。 `Job.Builder` が `Job`を作成できない場合は、例外がスローされます。  `Job.Builder` によって、次の既定値で `Job` が作成されます。

- デバイスが再起動されると `Job` が失われる &ndash;、`Job`の_有効期間_(実行のスケジュールが設定されるまでの時間) は、デバイスが再起動されるまでの間だけです。
- `Job` は定期的に実行されません &ndash; 1 回だけ実行されます。
- `Job` は、できるだけ早く実行するようにスケジュールされます。
- `Job` の既定の再試行戦略では、_指数関数的なバックオフ_を使用します (詳細については、「 [Retrystrategy の設定](#Setting_a_RetryStrategy)」セクションを参照してください)。

### <a name="scheduling-a-job"></a>ジョブのスケジュール設定

`Job`を作成した後は、実行する前に `FirebaseJobDispatcher` でスケジュールする必要があります。 `Job`をスケジュールするには、次の2つの方法があります。

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

`FirebaseJobDispatcher.Schedule` によって返される値は、次のいずれかの整数値になります。

- `Job` が正常にスケジュールされた &ndash; を `FirebaseJobDispatcher.ScheduleResultSuccess` します。
- `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; 不明な問題が発生したため、`Job` をスケジュールできませんでした。
- 無効な `IDriver` が使用された &ndash;、または `IDriver` がなんらかの方法で使用できなかった `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable`。 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; `Trigger` はサポートされていませんでした。
- サービスが正しく構成されていないか使用できない &ndash; `FirebaseJobDispatcher.ScheduleResultBadService`。

### <a name="configuring-a-job"></a>ジョブの構成

ジョブをカスタマイズすることもできます。 ジョブをカスタマイズする方法の例を次に示します。

- `Job` &ndash;[ジョブにパラメーターを渡す](#Passing_Parameters_to_a_Job)と、ファイルのダウンロードなどの処理を実行するために追加の値が必要になる場合があります。
- [制約を設定](#Setting_Constraints)する &ndash;、特定の条件が満たされた場合にのみジョブを実行する必要がある場合があります。 たとえば、デバイスが充電されている場合にのみ、`Job` を実行します。 
- [`Job` をいつ実行するかを指定し](#Setting_Job_Triggers)ます。これにより、焼討 Base ジョブディスパッチャーによって、アプリケーションでジョブを実行する時刻を指定でき &ndash; ます。  
- [失敗したジョブの再試行戦略を宣言](#Setting_a_RetryStrategy)する &ndash;_再試行戦略_では、完了できなかった `Jobs` を処理する `FirebaseJobDispatcher` についてのガイダンスを提供します。 

これらの各トピックについては、以降のセクションで詳しく説明します。

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>ジョブへのパラメーターの引き渡し

パラメーターは、`Job.Builder.SetExtras` メソッドと共に渡される `Bundle` を作成することによって、ジョブに渡されます。

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

`Bundle` には、`OnStartJob` メソッドの `IJobParameters.Extras` プロパティからアクセスします。

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>制約の設定

制約を使用すると、デバイスのコストを削減したり、バッテリを消費したりすることができます。 `Firebase.JobDispatcher.Constraint` クラスは、これらの制約を整数値として定義します。

- `Constraint.OnUnmeteredNetwork` &ndash;、デバイスが従量制のネットワークに接続されている場合にのみジョブを実行します。 これは、ユーザーがデータ料金を発生させないようにするために役立ちます。
- デバイスが接続されている任意のネットワークでジョブを実行 &ndash; `Constraint.OnAnyNetwork` ます。 `Constraint.OnUnmeteredNetwork`と共に指定した場合、この値は優先されます。
- `Constraint.DeviceCharging` &ndash;、デバイスが充電されている場合にのみジョブを実行します。

制約は、`Job.Builder.SetConstraint` メソッドを使用して設定します。 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

`JobTrigger` は、ジョブをいつ開始するかについて、オペレーティングシステムに関するガイダンスを提供します。 `JobTrigger` には、`Job` を実行する予定時刻を定義する、_実行中のウィンドウ_があります。 実行ウィンドウの [_開始]_ ウィンドウの値と [_終了] ウィンドウ_の値が表示されます。 開始ウィンドウは、ジョブを実行する前にデバイスが待機する秒数です。終了ウィンドウの値は、`Job`を実行する前に待機する最大秒数です。 

`JobTrigger` は、`Firebase.Jobdispatcher.Trigger.ExecutionWindow` メソッドを使用して作成できます。  たとえば `Trigger.ExecutionWindow(15,60)` は、スケジュールされた時刻から 15 ~ 60 秒の間にジョブを実行する必要があることを意味します。 `Job.Builder.SetTrigger` メソッドを使用して 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

ジョブの既定の `JobTrigger` は `Trigger.Now`値によって表されます。これは、スケジュールされた後、ジョブをできるだけ早く実行することを指定します。

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>RetryStrategy の設定

`Firebase.JobDispatcher.RetryStrategy` は、失敗したジョブを再実行する前にデバイスが使用する遅延の量を指定するために使用されます。 `RetryStrategy` には、失敗したジョブのスケジュールを再設定するために使用する時間ベースのアルゴリズムと、ジョブをスケジュールするウィンドウを指定する実行ウィンドウを定義する_ポリシー_があります。 この再_スケジュールウィンドウ_は、2つの値によって定義されます。 最初の値は、ジョブを再スケジュールする前に待機する秒数 (_初期バックオフ_値) です。2番目の数値は、ジョブを実行するまでの最大秒数 (_バックオフの最大_値) です。 

次の2種類の再試行ポリシーは、これらの int 値によって識別されます。

- _指数バックオフ_ポリシーを &ndash; `RetryStrategy.RetryPolicyExponential` と、エラーが発生するたびに初期バックオフ値が指数関数的に増加します。 ジョブが初めて失敗したときに、ライブラリは、ジョブのスケジュールを再スケジュールする前に指定された初期の間隔を待機し &ndash; 例は30秒です。 ジョブが2回目に失敗した場合、ライブラリはジョブの実行を試行する前に少なくとも60秒間待機します。 3回目の試行が失敗すると、ライブラリは120秒ほど待機します。 Firebase ジョブディスパッチャーライブラリの既定の `RetryStrategy` は、`RetryStrategy.DefaultExponential` オブジェクトによって表されます。 初期バックオフは30秒、最大バックオフは3600秒です。
- この方法 &ndash; `RetryStrategy.RetryPolicyLinear`、この方法では、ジョブを (成功まで) 設定さ_れた間隔_で実行するように再スケジュールする必要があります。 線形バックオフは、可能な限り早く完了する必要がある作業や、自動的に解決される問題に最適です。 Firebase ジョブディスパッチャーライブラリは、30秒以上3600秒の再スケジュールウィンドウを持つ `RetryStrategy.DefaultLinear` を定義します。

`FirebaseJobDispatcher.NewRetryStrategy` メソッドを使用してカスタム `RetryStrategy` を定義することもできます。 次の3つのパラメーターを受け取ります。

1. `int policy` &ndash;_ポリシー_は、以前の `RetryStrategy` 値、`RetryStrategy.RetryPolicyLinear`、または `RetryStrategy.RetryPolicyExponential`のいずれかです。
2. `int initialBackoffSeconds` &ndash;_最初のバックオフ_は、ジョブを再実行する前に必要な遅延時間 (秒単位) です。 既定値は30秒です。 
3. `int maximumBackoffSeconds` &ndash;_最大バックオフ_の値は、ジョブの実行を再試行するまでの最大待機時間を秒数で宣言します。 既定値は3600秒です。 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>ジョブの取り消し

スケジュールされたすべてのジョブ、または `FirebaseJobDispatcher.CancelAll()` 方法または `FirebaseJobDispatcher.Cancel(string)` 方法を使用して1つのジョブだけを取り消すことができます。

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

どちらの方法でも、整数値が返されます。

- ジョブが正常に取り消された &ndash; を `FirebaseJobDispatcher.CancelResultSuccess` しています。
- `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; エラーが発生したため、ジョブを取り消すことができませんでした。
- 有効な `IDriver` が使用できないため、`FirebaseJobDispatcher` がジョブを取り消すことができない &ndash; `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` ます。

## <a name="summary"></a>まとめ

このガイドでは、Firebase のジョブディスパッチャーを使用して、バックグラウンドで作業をインテリジェントに実行する方法について説明しました。 ここでは、`JobService` として実行される作業をカプセル化する方法と、`FirebaseJobDispatcher` を使用してその作業をスケジュールする方法、`JobTrigger` で条件を指定する方法、および `RetryStrategy`でエラーを処理する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [NuGet 上の Xamarin. JobDispatcher](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [Firebase-ジョブ-dispatcher (GitHub)](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin. JobDispatcher バインド](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [インテリジェントなジョブ-スケジュール設定](https://developer.android.com/topic/performance/scheduling.html)
- [Android のバッテリとメモリの最適化-Google i/o 2016 (ビデオ)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
