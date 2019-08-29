---
title: Firebase ジョブ ディスパッチャー
description: このガイドでは、Google の焼討ベースのジョブディスパッチャーライブラリを使用して、バックグラウンド作業をスケジュールする方法について説明します。
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: c0a35414cce6ff9981ad7825c8158a2f6f707585
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119494"
---
# <a name="firebase-job-dispatcher"></a>Firebase ジョブ ディスパッチャー

_このガイドでは、Google の焼討ベースのジョブディスパッチャーライブラリを使用して、バックグラウンド作業をスケジュールする方法について説明します。_


## <a name="overview"></a>概要

Android アプリケーションをユーザーに応答するための最善の方法の1つは、複雑な作業や長時間実行される作業がバックグラウンドで実行されるようにすることです。 ただし、バックグラウンド作業がデバイスに対するユーザーのエクスペリエンスに悪影響を及ぼさないようにすることが重要です。 

たとえば、バックグラウンドジョブは、3 ~ 4 分ごとに web サイトをポーリングして、特定のデータセットへの変更を照会することができます。 これには問題がないように思えますが、バッテリの寿命に大きな影響があります。 アプリケーションは、デバイスのスリープ状態を解除し、CPU を高い電力状態に昇格させ、無線の電源を入れ、ネットワーク要求を行い、結果を処理します。 デバイスの電源がすぐに切断されず、電力が低下した状態に戻ることがないため、この操作は悪くなります。 適切にスケジュールされていないバックグラウンド作業では、不必要で過剰な電力要件によってデバイスの状態が誤って保持される可能性があります。 この一見無害なアクティビティ (web サイトのポーリング) では、比較的短い時間でデバイスが使用不可能と表示されます。

Android には、バックグラウンドでの作業の実行に役立つ次の Api が用意されていますが、それ自体でインテリジェントなジョブのスケジュール設定には不十分です。 

- **[インテントサービス](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash;インテントサービスは作業の実行に適していますが、作業をスケジュールする方法はありません。
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash;これらの api では、作業をスケジュールすることのみが許可されますが、実際に作業を実行することはできません。 また、AlarmManager では時間ベースの制約のみが許可されます。これは、特定の時間または特定の期間が経過した後にアラームを発生させることを意味します。 
- **[Jobscheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)** &ndash; Jobschedule は、オペレーティングシステムと連携してジョブをスケジュールする優れた API です。 ただし、API レベル21以上を対象とする Android アプリでのみ使用できます。 
- **[ブロードキャストレシーバー](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android アプリでは、システム全体のイベントまたはインテントに応じて処理を実行するようにブロードキャストレシーバーを設定できます。 ただし、ブロードキャストレシーバーでは、ジョブをいつ実行するかを制御することはできません。 また、Android オペレーティングシステムでの変更によって、ブロードキャスト受信側が動作するタイミングや、応答できる作業の種類が制限されます。 

バックグラウンド作業を効率的に実行するには、次の2つの重要な機能があります (_バックグラウンドジョブ_または_ジョブ_と呼ばれることもあります)。

1. **作業をインテリジェントにスケジュールする**&ndash;アプリケーションがバックグラウンドで作業を行っているときは、それが優れた市民となることが重要です。 アプリケーションでは、ジョブの実行を要求しないことが理想的です。 代わりに、アプリケーションでは、ジョブを実行できるときに満たす必要がある条件を指定し、条件が満たされたときに実行するようにスケジュールを設定する必要があります。 これにより、Android は作業をインテリジェントに実行できます。 たとえば、ネットワーク要求をバッチ処理して、ネットワークに関連するオーバーヘッドを最大限に活用するために、すべてを同時に実行することができます。
2. **作業のカプセル**化&ndash;バックグラウンド作業を実行するコードは、ユーザーインターフェイスとは無関係に実行できる個別のコンポーネントにカプセル化する必要があります。また、何らかの理由で作業が完了しない場合は、比較的簡単に再スケジュールすることができます。

焼討 Base ジョブディスパッチャーは、fluent API を提供する Google のライブラリであり、バックグラウンド処理のスケジューリングを簡略化します。 これは、Google Cloud Manager の後継となることを目的としています。 焼討 Base ジョブディスパッチャーは、次の Api で構成されています。

- `Firebase.JobDispatcher.JobService`は、バックグラウンドジョブで実行されるロジックで拡張する必要がある抽象クラスです。
- は`Firebase.JobDispatcher.JobTrigger` 、ジョブをいつ開始するかを宣言します。 通常、これは時間枠として表現されます。たとえば、ジョブを開始する前に少なくとも30秒待機しますが、5分以内にジョブを実行します。
- に`Firebase.JobDispatcher.RetryStrategy`は、ジョブが正常に実行されなかった場合に実行する処理に関する情報が含まれています。 再試行戦略では、ジョブの実行を再試行するまでの待機時間を指定します。 
- は、ジョブを実行する前に満たす必要のある条件を記述するオプションの値です。たとえば、デバイスが従量制のネットワークや充電されていない場合などです。`Firebase.JobDispatcher.Constraint`
- は、以前の api をに`JobDispatcher`よってスケジュールできる作業単位に統合する api です。 `Firebase.JobDispatcher.Job` クラスは、を`Job`インスタンス化するために使用されます。 `Job.Builder`
- は`Firebase.JobDispatcher.JobDispatcher` 、前の3つの api を使用して、オペレーティングシステムでの作業のスケジュールを設定し、必要に応じてジョブを取り消す方法を提供します。

焼討ベースのジョブディスパッチャーを使用して作業をスケジュールするには、Xamarin アプリケーションで`JobService`クラスを拡張する型にコードをカプセル化する必要があります。 `JobService`には、ジョブの有効期間中に呼び出すことができるライフサイクルメソッドが3つあります。

- **`bool OnStartJob(IJobParameters parameters)`** &ndash;このメソッドは作業が発生し、常に実装する必要があります。 メインスレッドで実行されます。 このメソッドは、 `true`残存作業がある場合、また`false`は作業が完了した場合にを返します。 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash;これは、何らかの理由でジョブが停止されたときに呼び出されます。 後でジョブ`true`を再スケジュールする必要がある場合は、を返します。
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** このメソッドは、が非同期`JobService`処理を完了したときに呼び出されます。 &ndash; 

ジョブをスケジュールするために、アプリケーションはオブジェクト`JobDispatcher`をインスタンス化します。 次に、を使用して`Job`オブジェクトを作成します。このオブジェクト`JobDispatcher`は、実行するジョブのスケジュールを設定するに提供されます。 `Job.Builder`

このガイドでは、焼討ベースジョブディスパッチャーを Xamarin Android アプリケーションに追加し、それを使用してバックグラウンド作業をスケジュールする方法について説明します。

## <a name="requirements"></a>必要条件

焼討 Base ジョブディスパッチャーには、Android API レベル9以上が必要です。 焼討 Base ジョブディスパッチャーライブラリは、Google Play 開発者サービスによって提供されるいくつかのコンポーネントに依存しています。デバイスに Google Play 開発者サービスがインストールされている必要があります。

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Xamarin. Android での焼討 Base ジョブディスパッチャーライブラリの使用

焼討ベースのジョブディスパッチャーの使用を開始するには、最初に xamarin. の[NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)を Xamarin. Android プロジェクトに追加します。 NuGet パッケージマネージャーを検索します 。このパッケージは、まだプレリリース段階にあります。

焼討 base ジョブディスパッチャーライブラリを追加した後、 `JobService`クラスを作成し、 `FirebaseJobDispatcher`のインスタンスを使用して実行するようにスケジュールを設定します。


### <a name="creating-a-jobservice"></a>JobService の作成

焼討ベースのジョブディスパッチャーライブラリによって実行されるすべての作業は、 `Firebase.JobDispatcher.JobService`抽象クラスを拡張する型で実行する必要があります。 の`JobService`作成は、Android フレームワークを使用`Service`したの作成によく似ています。 

1. クラスを`JobService`拡張する
2. サブクラスをで`ServiceAttribute`修飾します。 厳密には必須ではありませんが、 `Name`パラメーターを明示的に設定して、の`JobService`デバッグを支援することをお勧めします。 
3. **Androidmanifest .xml** `JobService`でを宣言するを追加します。`IntentFilter` これにより、 `JobService`焼討 base ジョブディスパッチャーライブラリがを検索して呼び出すのにも役立ちます。

次のコードは、TPL を使用し`JobService`て何らかの処理を非同期に実行するアプリケーションの最も単純な例です。

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

### <a name="creating-a-firebasejobdispatcher"></a>焼討 Basejobdispatcher の作成

作業をスケジュールする前に、オブジェクトを`Firebase.JobDispatcher.FirebaseJobDispatcher`作成する必要があります。 は、をスケジュールする`JobService`役割を担います。 `FirebaseJobDispatcher` 次のコードスニペットは、 `FirebaseJobDispatcher`のインスタンスを作成する方法の1つです。 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

前のコードスニペット`GooglePlayDriver`では、は、デバイス上の Google Play 開発者サービスの一部のスケジュール api との`FirebaseJobDispatcher`対話を支援するクラスです。 パラメーター `context`は、アクティビティなど`Context`の任意の Android です。 現在、 `GooglePlayDriver`は、焼討`IDriver` base ジョブディスパッチャーライブラリの唯一の実装です。 

焼討 base ジョブディスパッチャーの Xamarin. Android バインドは、 `FirebaseJobDispatcher` `Context`からを作成するための拡張メソッドを提供します。 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

がインスタンス化される`Job`と、を作成し、 `JobService`クラスのコードを実行できます。 `FirebaseJobDispatcher` は`Job` 、 `Job.Builder`オブジェクトによって作成され、次のセクションで説明します。

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>このジョブを使用して、JobDispatcher. ジョブを作成しています。

クラスは、を`JobService`実行するために必要なメタデータをカプセル化します。 `Firebase.JobDispatcher.Job` には、ジョブを実行する前に満たす必要がある制約、 `Job`が定期的に行われる場合、またはジョブを実行するトリガーが含まれます。`Job`  最小要件`Job`として、には_タグ_(ジョブを`FirebaseJobDispatcher`識別する一意の文字列) と実行するの`JobService`種類が必要です。 このジョブを実行する時間が経過`JobService`すると、焼討 base ジョブディスパッチャーによってがインスタンス化されます。  は、 `Firebase.JobDispatcher.Job.JobBuilder`クラスのインスタンスを使用して作成されます。 `Job` 

次のコードスニペットは、Xamarin. Android バインドを使用し`Job`てを作成する方法の最も簡単な例です。

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

は`Job.Builder` 、ジョブの入力値に対していくつかの基本的な検証チェックを実行します。 でを`Job.Builder` `Job`作成することができない場合、例外がスローされます。  は、次の`Job`既定値を使用してを作成します。 `Job.Builder`

- デバイスの再起動&ndash; 後にデバイスが再起動されるまで、の有効期間(実行のスケジュールされる期間)は失われます。`Job` `Job`
- が`Job` 繰り返し&ndash;実行されていない場合は、1回だけ実行されます。
- は`Job` 、できるだけ早く実行するようにスケジュールされます。
- の`Job`既定の再試行戦略では、_指数関数的なバックオフ_が使用されます (詳細については、「 [retrystrategy の設定](#Setting_a_RetryStrategy)」セクションを参照してください)。

### <a name="scheduling-a-job"></a>ジョブのスケジュール設定

を作成`Job`した後は、を実行する前`FirebaseJobDispatcher`に、を使用してスケジュールする必要があります。 をスケジュールするには、 `Job`次の2つの方法があります。

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

によって`FirebaseJobDispatcher.Schedule`返される値は、次のいずれかの整数値になります。

- `FirebaseJobDispatcher.ScheduleResultSuccess`&ndash; は`Job`正常にスケジュールされました。
- `FirebaseJobDispatcher.ScheduleResultUnknownError`不明な問題が発生した`Job`ため、をスケジュールできませんでした。 &ndash;
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable`無効なが使用され`IDriver`たか、何らかの方法で使用できませんでした。 `IDriver` &ndash; 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger`&ndash; は`Trigger`サポートされていませんでした。
- `FirebaseJobDispatcher.ScheduleResultBadService`&ndash;サービスが正しく構成されていないか、使用できません。
 
### <a name="configuring-a-job"></a>ジョブの構成

ジョブをカスタマイズすることもできます。 ジョブをカスタマイズする方法の例を次に示します。

- [ジョブへのパラメーターの引き渡し](#Passing_Parameters_to_a_Job)では`Job` 、ファイルのダウンロードなど、作業を実行するために追加の値が必要になる場合があります。 &ndash;
- [制約の設定](#Setting_Constraints)&ndash;特定の条件が満たされた場合にのみジョブを実行する必要がある場合があります。 たとえば、デバイスが充電さ`Job`れている場合にのみ、を実行します。 
- [が起動`Job` ](#Setting_Job_Triggers) するタイミングを指定します。これにより、アプリケーションは、いつジョブを実行するかを指定できます。&ndash;  
- [失敗したジョブの再試行戦略を宣言する](#Setting_a_RetryStrategy)再試行戦略では、の処理`FirebaseJobDispatcher` `Jobs`を完了できなかった場合のについてのガイダンスを提供します。 &ndash; 

これらの各トピックについては、以降のセクションで詳しく説明します。

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>ジョブへのパラメーターの引き渡し

パラメーターは、 `Job.Builder.SetExtras`メソッドと共に渡される`Bundle`を作成することによって、ジョブに渡されます。

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

は`Bundle` 、 `OnStartJob`メソッドの`IJobParameters.Extras`プロパティからアクセスします。

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>制約の設定

制約を使用すると、デバイスのコストを削減したり、バッテリを消費したりすることができます。 クラス`Firebase.JobDispatcher.Constraint`は、これらの制約を整数値として定義します。

- `Constraint.OnUnmeteredNetwork`&ndash;デバイスが従量制以外のネットワークに接続されている場合にのみジョブを実行します。 これは、ユーザーがデータ料金を発生させないようにするために役立ちます。
- `Constraint.OnAnyNetwork`&ndash;デバイスが接続されている任意のネットワークでジョブを実行します。 と共に`Constraint.OnUnmeteredNetwork`指定した場合、この値は優先されます。
- `Constraint.DeviceCharging`&ndash;デバイスが充電されている場合にのみジョブを実行します。

制約は、メソッドを`Job.Builder.SetConstraint`使用して設定します。 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

は`JobTrigger` 、ジョブをいつ開始するかについて、オペレーティングシステムに関するガイダンスを提供します。 には、 `Job`を実行する予定時刻を定義する、実行中のウィンドウがあります。`JobTrigger` 実行ウィンドウの [_開始]_ ウィンドウの値と [_終了] ウィンドウ_の値が表示されます。 開始ウィンドウは、ジョブを実行する前にデバイスが待機する秒数です。終了ウィンドウの値は、 `Job`を実行する前に待機する最大秒数です。 

は`JobTrigger` 、 `Firebase.Jobdispatcher.Trigger.ExecutionWindow`メソッドを使用して作成できます。  たとえば`Trigger.ExecutionWindow(15,60)` 、ジョブがスケジュールされてから 15 ~ 60 秒後に実行されることを意味します。 メソッド`Job.Builder.SetTrigger`を使用して、 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

ジョブの`JobTrigger`既定値は、ジョブがスケジュールさ`Trigger.Now`れた後すぐに実行されることを指定する値で表されます。

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>RetryStrategy の設定

は`Firebase.JobDispatcher.RetryStrategy` 、失敗したジョブを再実行する前にデバイスが使用する遅延の量を指定するために使用されます。 には、失敗したジョブのスケジュールを再設定するために使用する時間ベースのアルゴリズムと、ジョブをスケジュールするウィンドウを指定する実行ウィンドウを定義するポリシーがあります。 `RetryStrategy` この再_スケジュールウィンドウ_は、2つの値によって定義されます。 最初の値は、ジョブを再スケジュールする前に待機する秒数 (_初期バックオフ_値) です。2番目の数値は、ジョブを実行するまでの最大秒数 (_バックオフの最大_値) です。 
 
次の2種類の再試行ポリシーは、これらの int 値によって識別されます。

- `RetryStrategy.RetryPolicyExponential`_指数バックオフ_ポリシーでは、エラーが発生するたびに初期バックオフ値が指数関数的に増加します。 &ndash; ジョブが初めて失敗したとき、ライブラリは、ジョブ&ndash;の例を30秒に再スケジュールする前に、指定されている最初の間隔を待機します。 ジョブが2回目に失敗した場合、ライブラリはジョブの実行を試行する前に少なくとも60秒間待機します。 3回目の試行が失敗すると、ライブラリは120秒ほど待機します。 の既定`RetryStrategy`値は、 `RetryStrategy.DefaultExponential`オブジェクトによって表示されます。 初期バックオフは30秒、最大バックオフは3600秒です。
- `RetryStrategy.RetryPolicyLinear`この戦略は、(成功するまで) 設定された間隔で実行するようにジョブを再スケジュールする必要がある_線形バックオフ_です。 &ndash; 線形バックオフは、可能な限り早く完了する必要がある作業や、自動的に解決される問題に最適です。 焼討ベースジョブディスパッチャーライブラリは、30 `RetryStrategy.DefaultLinear`秒以上3600秒の再スケジュールウィンドウを持つを定義します。

メソッド`FirebaseJobDispatcher.NewRetryStrategy`を使用してカスタム`RetryStrategy`を定義することもできます。 次の3つのパラメーターを受け取ります。

1. `int policy``RetryStrategy.RetryPolicyExponential` `RetryStrategy` `RetryStrategy.RetryPolicyLinear`ポリシーは、以前の値、、またはのいずれかです。 &ndash;
2. `int initialBackoffSeconds`_最初のバックオフ_は、ジョブを再実行する前に必要な遅延時間 (秒単位) です。 &ndash; 既定値は30秒です。 
3. `int maximumBackoffSeconds`_最大バックオフ_値は、ジョブの実行を再試行するまでの最大待機時間を秒単位で宣言します。 &ndash; 既定値は3600秒です。 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>ジョブの取り消し

スケジュールされているすべてのジョブ、または`FirebaseJobDispatcher.CancelAll()`メソッド`FirebaseJobDispatcher.Cancel(string)`またはメソッドを使用して1つのジョブだけを取り消すことができます。

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

どちらの方法でも、整数値が返されます。

- `FirebaseJobDispatcher.CancelResultSuccess`&ndash;ジョブは正常に取り消されました。
- `FirebaseJobDispatcher.CancelResultUnknownError`&ndash;エラーが発生したため、ジョブを取り消すことができませんでした。
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable`有効な使用可能ながないため、 `FirebaseJobDispatcher`はジョブを取り消すことができません。 `IDriver` &ndash;

## <a name="summary"></a>Summary

このガイドでは、焼討ベースのジョブディスパッチャーを使用して、バックグラウンドで作業をインテリジェントに実行する方法について説明しました。 ここで`JobService` `JobTrigger`は、として実行される作業をカプセル化する方法と`FirebaseJobDispatcher` 、を使用して、で条件を指定し、を使用してエラーを`RetryStrategy`処理する方法について説明しました。


## <a name="related-links"></a>関連リンク

- [NuGet 上の Xamarin. JobDispatcher](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [焼討 base-ジョブ-dispatcher (GitHub)](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher Binding](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [インテリジェントなジョブ-スケジュール設定](https://developer.android.com/topic/performance/scheduling.html)
- [Android のバッテリとメモリの最適化-Google i/o 2016 (ビデオ)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
