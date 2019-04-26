---
title: Firebase ジョブ ディスパッチャー
description: このガイドでは、Google の Firebase ジョブ ディスパッチャー ライブラリを使用してバック グラウンド作業をスケジュールする方法について説明します。
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: 91bafbbdaee805ad128766bf0a770cb711597a85
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023360"
---
# <a name="firebase-job-dispatcher"></a>Firebase ジョブ ディスパッチャー

_このガイドでは、Google の Firebase ジョブ ディスパッチャー ライブラリを使用してバック グラウンド作業をスケジュールする方法について説明します。_


## <a name="overview"></a>概要

Android アプリケーションのユーザーに応答性を維持する最善の方法の 1 つは、複雑なまたは実行時間の長い作業をバック グラウンドで実行するためにです。 ただし、これことバック グラウンド作業が影響を与えないユーザー エクスペリエンスのデバイスに重要です。 

たとえば、バック グラウンド ジョブ可能性があります web サイト分ごとにポーリング 3、4 クエリの特定のデータセットへの変更。 これは、バッテリの寿命悲惨な影響があるただし、問題のないようです。 アプリケーションは繰り返しウェイク アップ、デバイス、CPU を通常の電力状態に昇格、無線の電源、ネットワーク要求と、結果を処理します。 すぐには、デバイスの電源を切り、低電力アイドル状態に戻るために取得します。 適切にスケジュールされたバック グラウンド作業は、デバイスが不要なと過剰な能力の要件の状態にしてください誤って可能性があります。 (Web サイトのポーリング) この一見無害なアクティビティ、デバイスは比較的短時間で使用できないレンダリングされます。

Android は、バック グラウンドで作業を実行すると、次の Api を提供しますが単独ではありませんインテリジェントなジョブのスケジュール設定のための十分です。 

* **[インテント サービス](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash;目的としたサービスは、作業をスケジュールする方法を提供しないが、作業を実行するのに最適です。
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash;これらの Api はスケジュールが実際には、作業を実行する方法を指定しない作業のみを許可します。 また、AlarmManager のみを許可時間ベースの制約、特定の時間または一定の時間が経過した後にアラートを発行することを意味します。 
* **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)**  &ndash;の JobSchedule がジョブをスケジュールするオペレーティング システムで動作する優れた API。 ただし、これは Android API レベル 21 を対象とするアプリの使用可能な以上のみです。 
* **[ブロードキャスト レシーバー](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; An Android アプリは、システム全体のイベントまたはインテントへの応答で作業を実行するブロードキャスト レシーバーを設定できます。 ただし、ブロードキャスト レシーバーは、ジョブの実行と制御を提供しません。 Android オペレーティング システムの変更の制限もブロードキャスト レシーバーは機能、またはに対応する作業の種類。 

バック グラウンド作業を効率的に実行する 2 つの主要機能がある (とも呼ば、_バック グラウンド ジョブ_または_ジョブ_)。

1. **作業を適切にスケジューリング**&ndash;を行う際に、アプリケーションがバック グラウンドでの作業は良き市民として重要です。 理想的には、アプリケーションを要求するジョブを実行します。 代わりに、アプリケーションでは、ジョブが実行、および、条件が満たされたときに実行するには、その作業をスケジュールし、ときに満たす必要がある条件を指定する必要があります。 これにより、インテリジェントに処理を実行する Android です。 たとえば、ネットワーク要求は、ネットワークに関連するオーバーヘッドの最大使用数を同時にすべてを実行するバッチ処理可能性があります。
2. **作業をカプセル化する**&ndash;バック グラウンド処理を実行するコードは、ユーザー インターフェイスとは無関係に実行でき、作業を完了できない場合に再スケジュールするは比較的簡単になりますが、個別のコンポーネントにカプセル化する必要がありますどうもですね。

Firebase ジョブ ディスパッチャーは、スケジュールのバック グラウンド作業を簡略化に fluent API を提供する、Google からのライブラリです。 Google クラウド マネージャーの置換をするものでは。 Firebase ジョブ ディスパッチャーは、次の Api で構成されます。

* A`Firebase.JobDispatcher.JobService`はバック グラウンド ジョブで実行されるロジックで拡張する必要があります、抽象クラスです。
* A`Firebase.JobDispatcher.JobTrigger`ジョブを開始する必要がありますを宣言します。 これは通常として表現される、時間帯など、ジョブを開始する前に少なくとも 30 秒間待ってから、5 分以内に、ジョブを実行します。
* A`Firebase.JobDispatcher.RetryStrategy`適切に動作する、ジョブが失敗した場合の処理に関する情報を格納します。 再試行戦略は、ジョブを再実行する前に待機する期間を指定します。 
* A`Firebase.JobDispatcher.Constraint`を満たす必要があるジョブを実行する前に、デバイスが unmetered のネットワーク上など、条件を示す省略可能な値は、または充電中です。
* `Firebase.JobDispatcher.Job` 、--作業単位でスケジュールできるの以前の Api を統一する api、`JobDispatcher`します。 `Job.Builder`クラスがインスタンス化に使用する`Job`します。
* A`Firebase.JobDispatcher.JobDispatcher`前の 3 つの Api を使用して、オペレーティング システムで作業をスケジュールし、必要な場合に、ジョブを取り消す方法を提供します。

Firebase ジョブ ディスパッチャーと作業をスケジュールするには、コードを拡張する型で Xamarin.Android アプリケーションによってカプセル化する必要があります、`JobService`クラス。 `JobService` ジョブの有効期間中に呼び出すことができる 3 つのライフ サイクルの方法があります。

* **`bool OnStartJob(IJobParameters parameters)`** &ndash; このメソッドは、作業が発生し、常に実装する必要があります。 メイン スレッドで実行されます。 このメソッドは`true`がある場合の動作の残りまたは`false`場合の処理を実行します。 
* **`bool OnStopJob(IJobParameters parameters)`** &ndash; これは、何らかの理由により、ジョブが停止したときに呼び出されます。 返されます`true`場合は、ジョブを後で再スケジュールする必要があります。
* **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; このメソッドが呼び出されます、`JobService`非同期作業が完了します。 

ジョブをスケジュールするには、アプリケーションがインスタンスを作成、`JobDispatcher`オブジェクト。 次に`Job.Builder`を作成するために使用、`Job`オブジェクトに提供される、`JobDispatcher`お試しくださいされを実行するジョブのスケジュール設定します。

このガイドは、Xamarin.Android アプリケーションに Firebase ジョブ ディスパッチャーを追加し、使用してバック グラウンド作業をスケジュールする方法について説明します。

## <a name="requirements"></a>必要条件

Firebase ジョブ ディスパッチャーには、Android API レベル 9 以降が必要です。 Firebase ジョブ ディスパッチャー ライブラリは、Google Play Services; によって提供される一部のコンポーネントに依存していますデバイスには、Google play 開発者サービスがインストールされている必要があります。

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Xamarin.Android で Firebase ジョブ ディスパッチャー ライブラリの使用

Firebase ジョブ ディスパッチャーを開始するには、追加、 [Xamarin.Firebase.JobDispatcher NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)Xamarin.Android プロジェクトにします。 検索用の NuGet パッケージ マネージャー、 **Xamarin.Firebase.JobDispatcher**パッケージ (これは、まだプレリリース版では)。

Firebase ジョブ ディスパッチャー ライブラリを追加すると、作成、`JobService`クラスし、のインスタンスで実行をスケジュールし、`FirebaseJobDispatcher`します。


### <a name="creating-a-jobservice"></a>JobService を作成します。

Firebase ジョブ ディスパッチャー ライブラリによって実行されるすべての作業を拡張する型で行う必要がある、`Firebase.JobDispatcher.JobService`抽象クラス。 作成、`JobService`作成によく似ていますが、 `Service` Android フレームワークと。 

1. 拡張、`JobService`クラス
2. サブクラスを装飾、`ServiceAttribute`します。 明示的に設定するが、必須ではありませんをお勧め、`Name`デバッグに役立てるためのパラメーター、`JobService`します。 
3. 追加、`IntentFilter`を宣言する、`JobService`で、 **AndroidManifest.xml**します。 Firebase ジョブ ディスパッチャー ライブラリを検索し、呼び出すことができます、`JobService`します。

次のコードは、最も簡単な例`JobService`アプリケーションは、TPL を使用していくつかの作業を非同期で実行します。

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

作成する必要がすべての作業をスケジュールすることができます、前に、`Firebase.JobDispatcher.FirebaseJobDispatcher`オブジェクト。 `FirebaseJobDispatcher`スケジューリングを担当する`JobService`します。 次のコード スニペットは、のインスタンスを作成する方法の 1 つ、 `FirebaseJobDispatcher`: 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

前のコード スニペットで、`GooglePlayDriver`に役立つクラスが、`FirebaseJobDispatcher`デバイス上の Google play 開発者サービスでスケジュールの Api の一部を操作します。 パラメーター`context`は任意の Android `Context`、アクティビティなどです。 現在、`GooglePlayDriver`唯一`IDriver`Firebase ジョブ ディスパッチャー ライブラリで実装します。 

Firebase ジョブ ディスパッチャーの Xamarin.Android バインドを作成する拡張メソッドを提供する、`FirebaseJobDispatcher`から、 `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

1 回、`FirebaseJobDispatcher`がインスタンス化することは作成、`Job`でコードを実行し、`JobService`クラス。 `Job`によって作成されたが、`Job.Builder`オブジェクトし、は、次のセクションで説明します。

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Job.Builder、Firebase.JobDispatcher.Job を作成します。

`Firebase.JobDispatcher.Job`メタ データをカプセル化するため、クラスを実行するために必要な`JobService`します。 A`Job`場合、ジョブを実行すると、前に満たしている必要があるすべての制約などの情報が含まれています、`Job`は定期的に実行またはジョブを実行すると、すべてのトリガー。  最低限、として、`Job`必要があります、_タグ_(にジョブを識別する一意の文字列、 `FirebaseJobDispatcher`) の種類と、`JobService`を実行する必要があります。 Firebase ジョブ ディスパッチャーがインスタンス化、`JobService`ジョブを実行する時します。  A`Job`のインスタンスを使用して作成、`Firebase.JobDispatcher.Job.JobBuilder`クラス。 

次のコード スニペットを作成する方法の最も簡単な例を`Job`Xamarin.Android バインドを使用します。

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder`ジョブの入力値に対していくつかの基本的な検証チェックを実行します。 できない場合、例外がスローされます、`Job.Builder`を作成する、`Job`します。  `Job.Builder`が作成されます、`Job`次の既定値を使用します。

* A`Job`の_有効期間_のみ、デバイスが再起動されるまでは、(どのくらいの期間には実行をスケジュールする)&ndash;デバイスが再起動したら、`Job`は失われます。
* A`Job`は繰り返されません&ndash;1 回しか実行されません。
* A`Job`できるだけ早く実行するようにスケジュールされます。
* 既定の再試行戦略を`Job`を使用して、_指数関数的バックオフ_(セクションで以下の詳細に記載されている[設定、RetryStrategy](#Setting_a_RetryStrategy))

### <a name="scheduling-a-job"></a>ジョブをスケジュールします。

作成した後、 `Job`、スケジュールを設定する必要がある、`FirebaseJobDispatcher`に実行される前にします。 2 つのメソッドをスケジュールするため、 `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

によって返される値`FirebaseJobDispatcher.Schedule`整数値は次のいずれかになります。

* `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; `Job`正常にスケジュールされました。
* `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; 実行できない不明な問題が発生しました、`Job`スケジュールされているからです。
* `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; 無効な`IDriver`使用された、または`IDriver`何らかの方法で使用できませんでした。 
* `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; `Trigger`サポートされていませんでした。
* `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; サービスが正しく構成されていないまたは使用できません。
 
### <a name="configuring-a-job"></a>ジョブを構成します。

ジョブをカスタマイズすることになります。 ジョブをカスタマイズする方法の例については、次のとおりです。

* [ジョブにパラメーターを渡す](#Passing_Parameters_to_a_Job) &ndash; A`Job`ファイルのダウンロードなど、作業を実行する追加の値を必要があります。
* [制約を設定](#Setting_Constraints)&ndash;のみ特定の条件が満たされたときにジョブを実行する必要があります。 たとえば、のみを実行、`Job`デバイスが充電中です。 
* [指定するときに、`Job`実行する必要があります](#Setting_Job_Triggers) &ndash; Firebase ジョブ ディスパッチャーにより、ジョブが実行する時刻を指定するアプリケーション。  
* [失敗したジョブの再試行戦略を宣言](#Setting_a_RetryStrategy) &ndash; A_再試行戦略_ガイダンスを提供、`FirebaseJobDispatcher`で何`Jobs`を正常に完了します。 

次の各トピックは、以下のセクションで詳しく説明されます。

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>ジョブにパラメーターの引き渡し

作成して、ジョブに渡されるパラメーターを`Bundle`と共に渡される、`Job.Builder.SetExtras`メソッド。

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

`Bundle`からアクセスは、`IJobParameters.Extras`プロパティを`OnStartJob`メソッド。

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>制約を設定

制約が役立つことができます、デバイス上のコストやバッテリのドレインを削減します。 `Firebase.JobDispatcher.Constraint`クラスは、整数値としてこれらの制約を定義します。

* `Constraint.OnUnmeteredNetwork` &ndash; Unmetered のネットワークにデバイスが接続されている場合にのみ、ジョブを実行します。 これは、ユーザーがデータ料金が発生するを防ぐために役立ちます。
* `Constraint.OnAnyNetwork` &ndash; デバイスが接続されている任意のネットワーク上のジョブを実行します。 と共に指定されている場合`Constraint.OnUnmeteredNetwork`、この値が優先されます。
* `Constraint.DeviceCharging` &ndash; デバイスが充電中の場合にのみ、ジョブを実行します。

制約が設定され、`Job.Builder.SetConstraint`メソッド。 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

`JobTrigger`オペレーティング システムはジョブを開始するためのガイダンスを提供します。 A`JobTrigger`が、_実行ウィンドウ_時期のスケジュールされた時刻を定義する、`Job`実行する必要があります。 実行ウィンドウに、_ウィンドウを起動_値と_終了時間帯_値。 開始時間帯は、デバイスが、ジョブを実行する前に待機する秒数と、ウィンドウの最後の値は、実行する前に待機する秒の最大数、`Job`します。 

A`JobTrigger`で作成できる、`Firebase.Jobdispatcher.Trigger.ExecutionWindow`メソッド。  たとえば`Trigger.ExecutionWindow(15,60)`ジョブがスケジュールされてから 15 ~ 60 秒間実行することを意味します。 `Job.Builder.SetTrigger`メソッドを使用する 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

既定の`JobTrigger`のジョブは、値によって表される`Trigger.Now`ジョブをスケジュール設定した後、できるだけ早く実行することを指定する.

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>設定、RetryStrategy

`Firebase.JobDispatcher.RetryStrategy`デバイスの遅延がどの程度使用するように指定試す前に失敗したジョブを再実行するために使用します。 A`RetryStrategy`が、_ポリシー_、失敗したジョブおよびジョブをスケジュール設定する必要があります ウィンドウを指定する実行ウィンドウを再スケジュールに使用されるどのような時間ベースのアルゴリズムを定義します。 これは、_再スケジュール ウィンドウ_2 つの値によって定義されます。 最初の値が、ジョブのスケジュールを変更する前に待機する秒数 (、_初期バックオフ_値)、2 番目の数値は、ジョブを実行する必要があるまでの秒の最大数 (、_最大バックオフ_値)。 
 
2 つの種類の再試行ポリシーは、これらの整数値で識別されます。

* `RetryStrategy.RetryPolicyExponential` &ndash; _指数関数的バックオフ_ポリシーは各障害が発生した初期バックオフ値を指数関数的に増加されます。 ジョブが失敗した場合、最初に、ライブラリの待機は、ジョブのスケジュールを変更する前に指定されている (_i) 間隔&ndash;30 秒の例です。 2 回目のジョブが失敗すると、ライブラリには、ジョブを実行する前に、少なくとも 60 秒間は待機します。 試行が 3 つ目に失敗した後、ライブラリは、120 秒待ってし、具合です。 既定の`RetryStrategy`Firebase ジョブ ディスパッチャーのライブラリがによって表される、`RetryStrategy.DefaultExponential`オブジェクト。 30 秒間の初期のバックオフと 3600 秒の最大バックオフ備えています。
* `RetryStrategy.RetryPolicyLinear` &ndash; この方法は、_線形バックオフ_(成功) するまでの間隔を設定で実行するジョブを再スケジュールすることです。 線形バックオフでは、できるだけ早く完了する必要がある作業や自体を迅速に解決する問題に最適です。 Firebase ジョブ ディスパッチャー ライブラリを定義、`RetryStrategy.DefaultLinear`少なくとも 30 秒間の再スケジュール ウィンドウが 3600 秒にまでです。

カスタムを定義することは`RetryStrategy`で、`FirebaseJobDispatcher.NewRetryStrategy`メソッド。 次の 3 つのパラメーターを受け取ります。

1. `int policy` &ndash; _ポリシー_は、前の 1 つ`RetryStrategy`値、 `RetryStrategy.RetryPolicyLinear`、または`RetryStrategy.RetryPolicyExponential`します。
2. `int initialBackoffSeconds` &ndash; _初期バックオフ_ジョブを再実行する前に必要な遅延を秒単位。 この既定値は 30 秒です。 
3. `int maximumBackoffSeconds` &ndash; _最大バックオフ_値はジョブを再実行する前に待機する秒の最大数を宣言します。 既定値は、3,600 秒です。 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>ジョブのキャンセル

スケジュールされているすべてのジョブを取り消すことができるかを使用して、1 つのジョブ、`FirebaseJobDispatcher.CancelAll()`メソッドまたは`FirebaseJobDispatcher.Cancel(string)`メソッド。

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

いずれかの方法では、整数値を返します。

* `FirebaseJobDispatcher.CancelResultSuccess` &ndash; ジョブが正常に取り消されました。
* `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; エラーでは、ジョブが取り消されてできなくなります。
* `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; `FirebaseJobDispatcher`が有効なジョブをキャンセルできない`IDriver`使用できます。

## <a name="summary"></a>まとめ

このガイドでは、Firebase ジョブ ディスパッチャーを使用して、インテリジェントなバック グラウンドで作業を実行する方法について説明します。 として実行される作業をカプセル化する方法が説明されている、`JobService`と使用方法、`FirebaseJobDispatcher`で条件を指定する、その作業をスケジュールする、`JobTrigger`でエラーを処理する方法と、`RetryStrategy`します。


## <a name="related-links"></a>関連リンク

- [NuGet で Xamarin.Firebase.JobDispatcher](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [firebase ジョブ-ディスパッチャー github](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher Binding](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [インテリジェントなジョブ スケジュール](https://developer.android.com/topic/performance/scheduling.html)
- [Android のバッテリとメモリの最適化 - Google I/O 2016 (ビデオ)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
