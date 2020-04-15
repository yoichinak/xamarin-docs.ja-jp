---
title: Firebase ジョブ ディスパッチャー
description: このガイドでは、Google の Firebase ジョブ ディスパッチャー ライブラリを使用して、バックグラウンド処理をスケジュールする方法について説明します。
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: 280fe11f935db0a364f3342b22bb9544cdda1e6d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020247"
---
# <a name="firebase-job-dispatcher"></a>Firebase ジョブ ディスパッチャー

_このガイドでは、Google の Firebase ジョブ ディスパッチャー ライブラリを使用して、バックグラウンド処理をスケジュールする方法について説明します。_

## <a name="overview"></a>概要

ユーザーに対する Android アプリケーションのよい応答性を維持するための最善の方法の 1 つは、複雑な処理や長時間実行される処理がバックグラウンドで実行されるようにすることです。 ただし、バックグラウンド処理がデバイスのユーザー エクスペリエンスに悪影響を及ぼさないようにすることが重要です。 

たとえば、バックグラウンド ジョブでは、3 または 4 分ごとに Web サイトをポーリングして、特定のデータセットへの変更が照会されることがあります。 これは問題ないように思えますが、バッテリの寿命に大きな影響があります。 アプリケーションにより、繰り返し、デバイスがウェイクアップされ、CPU が高い電力状態に引き上げられて、無線の電源が入れられ、ネットワーク要求が行われて、結果が処理されます。 デバイスの電源がすぐに切れて、低電力のアイドル状態に戻ることはないため、この状態はいっそう悪くなります。 バックグラウンド処理が適切にスケジュールされていないと、デバイスが不必要で過剰な電力要件の状態のままになる可能性があります。 この一見無害なアクティビティ (Web サイトのポーリング) により、比較的短い時間でデバイスが使用できなくなります。

Android には、バックグラウンドでの処理の実行に役立つ次の API が用意されていますが、それだけではインテリジェントなジョブのスケジュール設定には不十分です。 

- **[Intent Services](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; Intent Services は、処理の実行には適していますが、作業のスケジュールを設定する方法はありません。
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; これらの API では、処理のスケジュールを設定することだけができ、実際に処理を実行する方法はありません。 また、AlarmManager では時間ベースの制約のみが可能です。これは、特定の時刻または特定の時間経過後にアラームを発生させることを意味します。 
- **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)** &ndash; JobSchedule は、オペレーティング システムと連携してジョブのスケジュールを設定する優れた API です。 ただし、API レベル 21 以上を対象とする Android アプリでのみ使用できます。 
- **[ブロードキャスト レシーバー](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android アプリでは、システム全体のイベントまたはインテントに応答して処理を実行するように、ブロードキャスト レシーバーを設定できます。 ただし、ブロードキャスト レシーバーでは、ジョブをいつ実行するかを制御することはできません。 また、Android オペレーティング システムでの変更によって、ブロードキャスト レシーバーが動作するタイミングや、応答できる処理の種類が制限されます。 

バックグラウンド処理を ("_バックグラウンド ジョブ_" または "_ジョブ_" とも呼ばれます) を効率的に実行するには、次の 2 つの重要な機能があります。

1. **処理のインテリジェントなスケジュール設定** &ndash; バックグラウンドで処理を行うアプリケーションが善良に振る舞うことが重要です。 理想的には、アプリケーションでジョブの実行を要求しないようにする必要があります。 代わりに、アプリケーションでは、ジョブを実行できるために満たされる必要がある条件を指定し、条件が満たされたときに実行する処理のスケジュールを設定する必要があります。 これにより、Android は処理をインテリジェントに実行できます。 たとえば、ネットワーク要求をバッチ処理してすべてを同時に実行し、ネットワーク関連のオーバーヘッドを最大限に利用することができます。
2. **処理のカプセル化** &ndash; バックグラウンド処理を実行するコードは、ユーザー インターフェイスとは関係なく実行できる個別のコンポーネントにカプセル化する必要があります。それにより、何らかの理由で処理を完了できなかった場合でも、比較的簡単にスケジュールを再設定できます。

Google のライブラリである Firebase ジョブ ディスパッチャーでは、バックグラウンド処理のスケジュール設定を簡単にする fluent API が提供されています。 これは、Google Cloud Manager の代わりになるものです。 Firebase ジョブ ディスパッチャーは、次の API で構成されています。

- `Firebase.JobDispatcher.JobService` は抽象クラスであり、バックグラウンド ジョブで実行されるロジックではこれを拡張する必要があります。
- `Firebase.JobDispatcher.JobTrigger` では、ジョブを開始するタイミングを宣言します。 通常、これは時間枠として表されます (例: 少なくとも 30 秒待ってからジョブを開始するが、5 分以内にジョブを実行する)。
- `Firebase.JobDispatcher.RetryStrategy` には、ジョブを正常に実行できないときに行う必要があることに関する情報が含まれています。 再試行戦略では、ジョブの再実行を試みるまでの待機時間を指定します。 
- `Firebase.JobDispatcher.Constraint` は、ジョブを実行する前に満たされる必要がある条件を記述するオプションの値です (例: デバイスが従量課金制ではないネットワークを使用している場合や、充電されている場合)。
- `Firebase.JobDispatcher.Job` は、これまでの API を、`JobDispatcher` によってスケジュール設定できる処理単位にまとめる API です。 `Job.Builder` クラスは、`Job` をインスタンス化するために使用されます。
- `Firebase.JobDispatcher.JobDispatcher` では、前の 3 つの API を使用して、オペレーティング システムでの処理がスケジュール設定され、必要に応じてジョブを取り消す手段が提供されます。

Firebase ジョブ ディスパッチャーで処理のスケジュールを設定するには、Xamarin.Android アプリケーションで、`JobService` クラスを拡張する型にコードをカプセル化する必要があります。 `JobService` には、ジョブの有効期間中に呼び出すことができるライフサイクル メソッドが 3 つあります。

- **`bool OnStartJob(IJobParameters parameters)`** &ndash; このメソッドは処理が行われる場所であり、常に実装する必要があります。 メイン スレッドで実行されます。 このメソッドでは、残っている処理がある場合は `true` が返され、処理が完了した場合は `false` が返されます。 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash; これは、何らかの理由でジョブが停止されたときに呼び出されます。 後でジョブのスケジュールを再設定する必要がある場合は、`true` が返されます。
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; このメソッドは、`JobService` で非同期処理が完了したときに呼び出されます。 

ジョブのスケジュールを設定するには、アプリケーションで `JobDispatcher` オブジェクトをインスタンス化します。 次に、`Job.Builder` を使用して作成した `Job` オブジェクトを `JobDispatcher` に提供すると、そこでジョブの実行が試みられ、スケジュールが設定されます。

このガイドでは、Firebase ジョブ ディスパッチャーを Xamarin.Android アプリケーションに追加し、それを使用してバックグラウンド処理のスケジュールを設定する方法について説明します。

## <a name="requirements"></a>必要条件

Firebase ジョブ ディスパッチャーには、Android API レベル9 以上が必要です。 Firebase ジョブ ディスパッチャー ライブラリは、Google Play 開発者サービスによって提供されるいくつかのコンポーネントに依存しています。デバイスに Google Play 開発者サービスがインストールされている必要があります。

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Xamarin.Android での Firebase ジョブ ディスパッチャー ライブラリの使用

Firebase ジョブ ディスパッチャーを使い始めるには、まず [Xamarin.Firebase.JobDispatcher NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)を Xamarin.Android プロジェクトに追加します。 **Xamarin.Firebase.JobDispatcher** パッケージ (まだプレリリース段階) 用の NuGet パッケージ マネージャーを検索します。

Firebase ジョブ ディスパッチャー ライブラリを追加した後、`JobService` クラスを作成し、`FirebaseJobDispatcher` のインスタンスを使用して実行するようにスケジュールを設定します。

### <a name="creating-a-jobservice"></a>JobService の作成

Firebase ジョブ ディスパッチャー ライブラリによって実行されるすべての処理は、`Firebase.JobDispatcher.JobService` 抽象クラスを拡張する型で実行される必要があります。 `JobService` の作成は、Android フレームワークでの `Service` の作成とよく似ています。 

1. `JobService` クラスを拡張します
2. サブクラスを `ServiceAttribute` で修飾します。 厳密には必要ではありませんが、`JobService` のデバッグで役立つように `Name` パラメーターを明示的に設定することをお勧めします。 
3. **AndroidManifest.xml** に `IntentFilter` を追加して `JobService` を宣言します。 これは、Firebase ジョブ ディスパッチャー が `JobService` を探して呼び出す場合にも役立ちます。

次のコードは、TPL を使用して何らかの処理を非同期に実行する、アプリケーションに対する最も簡単な `JobService` の例です。

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

### <a name="creating-a-firebasejobdispatcher"></a>FirebaseJobDispatcher の作成

処理のスケジュールを設定する前に、`Firebase.JobDispatcher.FirebaseJobDispatcher` オブジェクトを作成する必要があります。 `FirebaseJobDispatcher` の役割は、`JobService` のスケジュールを設定することです。 次のコード スニペットは、`FirebaseJobDispatcher` のインスタンスを作成する 1 つの方法です。 

 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

上のコード スニペットで、`GooglePlayDriver` は、`FirebaseJobDispatcher` がデバイス上の Google Play 開発者サービスのスケジューリング API と対話できるようにするクラスです。 `context` パラメーターは、任意の Android `Context` (Activity など) です。 現在、`GooglePlayDriver` は、Firebase ジョブ ディスパッチャー ライブラリ内で唯一の `IDriver` の実装です。 

Firebase ジョブ ディスパッチャーに対する Xamarin. Android のバインドによって、`Context` から `FirebaseJobDispatcher` を作成するための拡張メソッドが提供されます。 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

`FirebaseJobDispatcher` がインスタンス化されたら、`Job` を作成し、`JobService` クラスでコードを実行できるようになります。 `Job` は `Job.Builder` オブジェクトによって作成されます。これについては、次のセクションで説明します。

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Job.Builder での Firebase.JobDispatcher.Job の作成

`Firebase.JobDispatcher.Job` クラスの役割は、`JobService` の実行に必要なメタデータをカプセル化することです。 `Job` には、ジョブが実行可能になる前に満たされる必要がある制約、`Job` が定期的かどうか、ジョブが実行されるトリガーなどの情報が含まれます。  少なくとも、`Job` には、"_タグ_" (ジョブを `FirebaseJobDispatcher` に対して一意に示す文字列) と、実行する必要のある `JobService` の種類が必要です。 ジョブを実行する時間になると、Firebase ジョブ ディスパッチャーによって `JobService` がインスタンス化されます。  `Job` は、`Firebase.JobDispatcher.Job.JobBuilder` クラスのインスタンスを使用して作成されます。 

次のコード スニペットは、Xamarin.Android のバインドを使用して `Job` を作成する方法の最も簡単な例です。

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` によって、ジョブに対する入力値についてのいくつかの基本的な検証チェックが実行されます。 `Job.Builder` で `Job` を作成できない場合は、例外がスローされます。  `Job.Builder` で作成される `Job` の既定値は次のとおりです。

- `Job` の "_有効期間_" (実行のスケジュールが設定されている期間) は、デバイスの再起動までだけです。デバイスが再起動されると `Job` は失われます。
- `Job` は定期的に実行されません。1 回実行されるだけです。
- `Job` は、できるだけ早く実行されるようにスケジュール設定されます。
- `Job` の既定の再試行戦略では、"_エクスポネンシャル バックオフ_" が使用されます (詳しくは、後の「[RetryStrategy の設定](#Setting_a_RetryStrategy)」セクションで説明します)

### <a name="scheduling-a-job"></a>ジョブのスケジュール設定

`Job` を作成した後は、実行する前に、`FirebaseJobDispatcher` でスケジュールを設定する必要があります。 `Job` のスケジュールを設定するには、次の 2 つの方法があります。

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

`FirebaseJobDispatcher.Schedule` によって返される値は、次のいずれかの整数値です。

- `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; `Job` は正常にスケジュール設定されました。
- `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; 不明な問題が発生したため、`Job` のスケジュールを設定できませんでした。
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; 無効な `IDriver` が使用されたか、または `IDriver` がなんらかの理由で使用できませんでした。 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; `Trigger` はサポートされていませんでした。
- `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; サービスが正しく構成されていないか、または使用できません。

### <a name="configuring-a-job"></a>ジョブの構成

ジョブをカスタマイズすることができます。 ジョブをカスタマイズする方法の例を次に示します。

- [ジョブにパラメーターを渡す](#Passing_Parameters_to_a_Job) &ndash; `Job` で処理を実行するために追加の値が必要になる場合があります (ファイルのダウンロードなど)。
- [制約を設定する](#Setting_Constraints) &ndash; 特定の条件が満たされた場合にのみ、ジョブが実行されるようにすることが必要な場合があります。 たとえば、デバイスが充電されている場合にのみ、`Job` を実行するような場合です。 
- [`Job` をいつ実行するかを指定する](#Setting_Job_Triggers) &ndash; Firebase ジョブ ディスパッチャーでは、ジョブを実行する必要がある時刻をアプリケーションで指定できます。  
- [失敗したジョブの再試行戦略を宣言する](#Setting_a_RetryStrategy) &ndash; "_再試行戦略_" では、完了できなかった `Jobs` への対処方法についてのガイダンスを `FirebaseJobDispatcher` に提供します。 

これらの各トピックについては、以下のセクションで詳しく説明します。

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>ジョブにパラメーターを渡す

ジョブにパラメーターを渡すには、`Job.Builder.SetExtras` メソッドと共に渡される `Bundle` を作成します。

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

制約を使用すると、デバイスでのコストやバッテリ消費を削減できます。 `Firebase.JobDispatcher.Constraint` クラスでは、これらの制約を整数値として定義します。

- `Constraint.OnUnmeteredNetwork` &ndash; デバイスが従量課金制ではないネットワークに接続されている場合にのみ、ジョブを実行します。 これは、ユーザーにデータ料金が発生しないようにするのに役立ちます。
- `Constraint.OnAnyNetwork` &ndash; デバイスがどのようなネットワークに接続されていても、ジョブを実行します。 `Constraint.OnUnmeteredNetwork` と共に指定した場合は、この値が優先されます。
- `Constraint.DeviceCharging` &ndash; デバイスが充電されている場合にのみ、ジョブを実行します。

制約を設定するには、`Job.Builder.SetConstraint` メソッドを使用します。 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

`JobTrigger` では、ジョブをいつ開始する必要があるかについて、オペレーティング システムにガイダンスが提供されます。 `JobTrigger` の "_実行時間枠_" では、`Job` を実行する必要があるスケジュール設定された時刻が定義されています。 実行時間枠には、"_開始枠_" の値と "_終了枠_" の値があります。 開始枠は、ジョブを実行する前にデバイスが待機する秒数です。終了枠の値は、`Job` を実行する前に待機する最大秒数です。 

`JobTrigger` は、`Firebase.Jobdispatcher.Trigger.ExecutionWindow` メソッドを使用して作成できます。  たとえば、`Trigger.ExecutionWindow(15,60)` は、スケジュール設定された時刻から 15 から 60 秒の間にジョブを実行する必要があることを意味します。 `Job.Builder.SetTrigger` メソッドが使用されます 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

ジョブの既定の `JobTrigger` は、`Trigger.Now` の値によって表されます。この値は、スケジュールが設定された後、ジョブをできるだけ早く実行することを指定します。

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>RetryStrategy の設定

`Firebase.JobDispatcher.RetryStrategy` は、失敗したジョブを再実行する前にデバイスで使用する遅延の量を指定するために使用されます。 `RetryStrategy` の "_ポリシー_" では、失敗したジョブのスケジュールを再設定するために使用する時間ベースのアルゴリズムと、ジョブをスケジュールする時間枠を指定する実行時間枠が定義されています。 この "_再スケジュール時間枠_" は、2 つの値によって定義されます。 1 つ目の値は、ジョブを再スケジュールする前に待機する秒数です ("_初期バックオフ_" 値)。2 つ目の値は、ジョブを実行するまでの最大秒数です ("_最大バックオフ_" 値)。 

次の int 値によって、2 種類の再試行ポリシーが識別されます。

- `RetryStrategy.RetryPolicyExponential` &ndash; "_エクスポネンシャル バックオフ_" ポリシーでは、失敗するたびに、初期バックオフ値が指数的に増加します。 ジョブが初めて失敗したときは、ライブラリは、指定されている _initial 間隔だけ待ってからジョブを再スケジュールします (たとえば 30 秒)。 ジョブが 2 回目に失敗した場合は、ライブラリはジョブの実行を試行する前に少なくとも 60 秒間待機します。 3 回目の試行が失敗すると、ライブラリは 120 秒待機します。以下同様です。 Firebase ジョブ ディスパッチャー ライブラリの既定の `RetryStrategy` は、`RetryStrategy.DefaultExponential` オブジェクトによって表されます。 初期バックオフは 30 秒、最大バックオフは 3600 秒です。
- `RetryStrategy.RetryPolicyLinear` &ndash; この戦略は "_直線的バックオフ_" であり、ジョブの実行は (成功するまで) 設定された一定の間隔で再スケジュールされます。 直線的バックオフは、可能な限り早く完了する必要がある処理や、短時間で自動的に解決される問題に最適です。 Firebase ジョブ ディスパッチャー ライブラリで定義されている `RetryStrategy.DefaultLinear` では、再スケジュール時間枠は 30 秒以上 3600 秒以下です。

`FirebaseJobDispatcher.NewRetryStrategy` メソッドを使用してカスタム `RetryStrategy` を定義できます。 3 つのパラメーターを受け取ります。

1. `int policy` &ndash; "_ポリシー_" は、前の `RetryStrategy` 値、`RetryStrategy.RetryPolicyLinear`、または `RetryStrategy.RetryPolicyExponential` のいずれかです。
2. `int initialBackoffSeconds` &ndash; "_初期バックオフ_" は、ジョブの再実行を試みる前に必要な遅延時間 (秒単位) です。 これの既定値は 30 秒です。 
3. `int maximumBackoffSeconds` &ndash; "_最大バックオフ_" 値は、ジョブの実行を再試行するまでの最大待機時間を秒単位で指定します。 既定値は 3600 秒です。 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>ジョブの取り消し

`FirebaseJobDispatcher.CancelAll()` メソッドまたは `FirebaseJobDispatcher.Cancel(string)` メソッドを使用して、スケジュール設定されたすべてのジョブまたは 1 つのジョブだけを取り消すことができます。

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

どちらのメソッドでも、整数値が返されます。

- `FirebaseJobDispatcher.CancelResultSuccess` &ndash; ジョブは正常に取り消されました。
- `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; エラーが発生したため、ジョブを取り消せませんでした。
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; 使用できる有効な `IDriver` がないため、`FirebaseJobDispatcher` はジョブを取り消せません。

## <a name="summary"></a>まとめ

このガイドでは、Firebase ジョブ ディスパッチャーを使用して、バックグラウンドで処理をインテリジェントに実行する方法について説明しました。 ここでは、実行される処理を `JobService` としてカプセル化する方法、`FirebaseJobDispatcher` を使用してその処理のスケジュールを設定する方法、`JobTrigger` で条件を指定する方法、`RetryStrategy` でエラーを処理する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [NuGet での Xamarin.Firebase.JobDispatcher](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [GitHub での firebase-job-dispatcher](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher のバインド](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [インテリジェントなジョブ スケジュール設定](https://developer.android.com/topic/performance/scheduling.html)
- [Android のバッテリとメモリの最適化 - Google I/O 2016 (ビデオ)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
