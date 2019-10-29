---
title: タスクを使用した iOS バックグラウンド処理
description: このドキュメントでは、バックグラウンドタスクを使用して、アプリケーションがバックグラウンドで配置された後に長時間実行されるタスクを実行する方法について説明します。
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: f39ab83e00db1abd6508d26a9280fb708e681445
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73004729"
---
# <a name="ios-backgrounding-with-tasks"></a>タスクを使用した iOS バックグラウンド処理

IOS でバックグラウンド処理を実行する最も簡単な方法は、バックグラウンド処理の要件をタスクに分割し、タスクをバックグラウンドで実行することです。 タスクには厳格な時間制限が適用されており、通常は、アプリケーションが iOS 6 のバックグラウンドに移行してから、iOS 7 以降で10分未満になった後の処理時間が約600秒 (10 分) になります。

バックグラウンドタスクは、次の3つのカテゴリに分けることができます。

1. **バックグラウンドセーフタスク**-アプリケーション内の任意の場所で呼び出されます。中断したくないタスクは、アプリケーションによってバックグラウンドで入力されます。
1. **DidEnterBackground Tasks** -クリーンアップおよび状態保存を支援するために `DidEnterBackground` アプリケーションライフサイクルメソッドの実行中に呼び出されます。
1. **バックグラウンド転送 (ios 7 以降)** -ios 7 でネットワーク転送を実行するために使用される、特殊な種類のバックグラウンドタスク。 通常のタスクとは異なり、バックグラウンド転送には事前に決められた時間制限はありません。

バックグラウンドセーフタスクと `DidEnterBackground` タスクは、iOS 6 と iOS 7 の両方で安全に使用できますが、若干の違いがあります。 これらの2種類のタスクについて詳しく説明します。

## <a name="creating-background-safe-tasks"></a>バックグラウンドセーフタスクの作成

アプリケーションによっては、iOS によって中断されないタスクが含まれている場合があります。 これらのタスクを中断から保護する方法の1つとして、長時間実行されるタスクとして iOS に登録する方法があります。 このパターンは、ユーザーがアプリをバックグラウンドに配置する場合に、タスクが中断されないようにするアプリケーションのどこでも使用できます。 このパターンの優れた候補は、新しいユーザーの登録情報をサーバーに送信したり、ログイン情報を確認したりするなどのタスクです。

次のコードスニペットは、バックグラウンドで実行するタスクを登録する方法を示しています。

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

登録プロセスでは、一意の識別子を持つタスクを `taskID`し、そのタスクを一致する `BeginBackgroundTask` と `EndBackgroundTask` の呼び出しでラップします。 識別子を生成するには、`UIApplication` オブジェクトの `BeginBackgroundTask` メソッドを呼び出し、長時間実行されるタスクを (通常は新しいスレッドで) 開始します。 タスクが完了したら `EndBackgroundTask` を呼び出し、同じ識別子を渡します。 これは、`BeginBackgroundTask` の呼び出しに一致する `EndBackgroundTask`がない場合に、iOS によってアプリケーションが終了するため、重要です。

> [!IMPORTANT]
> バックグラウンドセーフタスクは、アプリケーションのニーズに応じて、メインスレッドまたはバックグラウンドスレッドで実行できます。

## <a name="performing-tasks-during-didenterbackground"></a>DidEnterBackground 中のタスクの実行

実行時間の長いタスクをバックグラウンドセーフにするだけでなく、アプリケーションがバックグラウンドに配置されているときにタスクを開始するために登録を使用することもできます。 iOS では、アプリケーションの状態を保存したり、ユーザーデータを保存したり、アプリケーションがバックグラウンドに入る前に機密コンテンツを暗号化したりするために使用できる `DidEnterBackground` という*Appdelegate*クラスのイベントメソッドを提供しています。 アプリケーションはこのメソッドから返されるまでに約5秒かかります。また、アプリケーションは終了します。 そのため、完了するまでに5秒以上かかる可能性のあるクリーンアップタスクは、`DidEnterBackground` メソッド内から呼び出すことができます。 これらのタスクは、別のスレッドで呼び出す必要があります。

このプロセスは、実行時間の長いタスクの登録とほぼ同じです。 次のコードスニペットは、実際の動作を示しています。

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

最初に、前の例で行ったように `BeginBackgroundTask` を通じてタスクを登録する、`AppDelegate`の `DidEnterBackground` メソッドをオーバーライドします。 次に、新しいスレッドを生成し、実行時間の長いタスクを実行します。 `DidEnterBackground` メソッドが既に返されているため、`EndBackgroundTask` の呼び出しが実行時間の長いタスクの内部から行われることに注意してください。

> [!IMPORTANT]
> iOS では、[ウォッチドッグメカニズム](https://developer.apple.com/library/ios/qa/qa1693/_index.html)を使用して、アプリケーションの UI の応答性を維持します。 `DidEnterBackground` に時間がかかりすぎるアプリケーションは、UI で応答しなくなります。 開始がバックグラウンドで実行されるようにすると、UI の応答性を維持し、ウォッチドッグがアプリケーションを強制終了するのを防ぐために、`DidEnterBackground` を適時に返すことができます。

## <a name="handling-background-task-time-limits"></a>バックグラウンドタスクの時間制限の処理

iOS では、バックグラウンドタスクの実行時間が厳密に制限されており、割り当てられた時間内に `EndBackgroundTask` 呼び出しが行われなかった場合、アプリケーションは終了します。 残りのバックグラウンド処理時間を追跡し、必要に応じて有効期限ハンドラーを使用することにより、iOS がアプリケーションを終了しないようにすることができます。

### <a name="accessing-background-time-remaining"></a>残りのバックグラウンド時間へのアクセス

タスクが登録されているアプリケーションをバックグラウンドに移動すると、登録されたタスクの実行に約600秒かかります。 `UIApplication` クラスの静的 `BackgroundTimeRemaining` プロパティを使用して、タスクが完了するまでの時間を確認できます。 次のコードは、バックグラウンドタスクが残された時間を秒単位で示します。

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>有効期限ハンドラーによるアプリの終了の回避

IOS では、`BackgroundTimeRemaining` プロパティへのアクセス権を付与するだけでなく、**有効期限ハンドラー**を使用して、バックグラウンド時間の有効期限を適切に処理することができます。 これは、タスクに割り当てられた時間が経過すると実行される、省略可能なコードブロックです。 有効期限ハンドラー内のコードは `EndBackgroundTask` を呼び出し、タスク ID を渡します。これは、アプリが正常に動作していることを示し、タスクが期限切れになった場合でも iOS がアプリを終了できないようにします。 `EndBackgroundTask` は、通常の実行の場合と同様に、有効期限ハンドラー内で呼び出す必要があります。 

有効期限ハンドラーは、次に示すように、ラムダ式を使用して匿名関数として表現されます。

```csharp
Task.Factory.StartNew( () => {

    //expirationHandler only called if background time allowed exceeded
    var taskId = UIApplication.SharedApplication.BeginBackgroundTask(() => {
        Console.WriteLine("Exhausted time");
        UIApplication.SharedApplication.EndBackgroundTask(taskId); 
    });
    while(myFlag == true)
    {
        Console.WriteLine(UIApplication.SharedApplication.BackgroundTimeRemaining);
        myFlag = SomeCalculationNeedsMoreTime();
    }
    //Only called if loop terminated due to myFlag and not expiration of time
    UIApplication.SharedApplication.EndBackgroundTask(taskId);
});
```

コードを実行するために有効期限ハンドラーは必要ありませんが、常にバックグラウンドタスクで有効期限ハンドラーを使用する必要があります。

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>IOS 7 以降のバックグラウンドタスク

バックグラウンドタスクに関しては、iOS 7 の最大の変更点は、タスクの実装方法ではなく、実行時の動作です。

IOS より前の7では、バックグラウンドで実行されているタスクが完了するまでに600秒かかりました。 この制限の理由の1つは、バックグラウンドで実行されているタスクが、タスクの実行中にデバイスのスリープ状態を維持することです。

 [![](ios-backgrounding-with-tasks-images/ios6.png "Graph of the task keeping the app awake pre-iOS 7")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

iOS 7 のバックグラウンド処理は、より長いバッテリ寿命に合わせて最適化されています。 IOS 7 では、バックグラウンド処理が便宜になります。デバイスを起動したままにするのではなく、デバイスがスリープ状態になったときにタスクを実行し、デバイスがスリープ状態になったときに、電話、通知、着信メールなどを処理するように処理を行います。一般的な中断。 次の図は、タスクがどのように分割されるかについての洞察を示しています。

 [![](ios-backgrounding-with-tasks-images/ios7.png "Graph of the task being broken into chunks post-iOS 7")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

タスクの実行時間が長くなることはないため、ネットワーク転送を実行するタスクは、iOS 7 では異なる方法で処理される必要があります。 開発者は、`NSURlSession` API を使用してネットワーク転送を処理することをお勧めします。 次のセクションでは、バックグラウンド転送の概要について説明します。

 <a name="background-transfers" />

## <a name="background-transfers"></a>バックグラウンド転送

IOS 7 でのバックグラウンド転送のバックボーンは、新しい `NSURLSession` API です。 `NSURLSession` では、次のタスクを作成できます。

1. ネットワークとデバイスの中断によってコンテンツを転送します。
1. 大きなファイルをアップロードしてダウンロードします (*バックグラウンド転送サービス*)。

これがどのように動作するかについて詳しく見ていきましょう。

### <a name="nsurlsession-api"></a>Nの Lsession API

 `NSURLSession` は、ネットワーク経由でコンテンツを転送するための強力な API です。 ネットワークの中断とアプリケーションの状態の変化によってデータの転送を処理するための一連のツールが用意されています。

`NSURLSession` API は、1つまたは複数のセッションを作成します。これにより、ネットワーク経由で関連データのシャトルブロックにタスクが生成されます。 タスクは、データを迅速かつ確実に転送するために非同期的に実行されます。 `NSURLSession` は非同期であるため、すべてのセッションで、転送が完了したときにシステムとアプリケーションに通知するための完了ハンドラーブロックが必要になります。

IOS 7 以降と iOS 7 以降の両方で有効なネットワーク転送を実行するには、エンキュー転送に `NSURLSession` が使用可能かどうかを確認し、通常のバックグラウンドタスクを使用して転送を実行します (そうでない場合)。

```csharp
if ([NSURLSession class]) {
  // Create a background session and enqueue transfers
}
else {
  // Start a background task and transfer directly
  // Do NOT make calls to update the UI here!
}
```

> [!IMPORTANT]
> Ios 6 はバックグラウンド UI 更新をサポートしておらず、アプリケーションを終了するため、iOS 6 に準拠したコードのバックグラウンドから UI を更新する呼び出しは避けてください。

`NSURLSession` API には、認証の処理、失敗した転送の管理、およびレポートクライアント側でのサーバー側エラーの管理を行うための豊富な機能セットが含まれています。 これは、iOS 7 で導入されたタスクの実行時間の中断をブリッジするのに役立ちます。また、大規模なファイルを迅速かつ確実に転送するためのサポートも提供します。 次のセクションでは、この2番目の機能について説明します。

### <a name="background-transfer-service"></a>バックグラウンド転送サービス

IOS 7 より前では、バックグラウンドでのファイルのアップロードまたはダウンロードに信頼性がありませんでした。 バックグラウンドタスクの実行には時間がかかりますが、ファイルの転送にかかる時間は、ネットワークとファイルのサイズによって異なります。 IOS 7 では、`NSURLSession` を使用して、大きなファイルを正常にアップロードしてダウンロードすることができます。 バックグラウンドで大きなファイルのネットワーク転送を処理する特定の `NSURLSession` セッションの種類は、*バックグラウンド転送サービス*と呼ばれます。

バックグラウンド転送サービスを使用して開始される転送は、オペレーティングシステムによって管理され、認証とエラーを処理するための Api を提供します。 転送は、任意の時間制限によってバインドされていないため、大きなファイルのアップロードやダウンロード、バックグラウンドでのコンテンツの自動更新などに使用できます。 サービスを実装する方法の詳細については、[バックグラウンド転送のチュートリアル](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md)を参照してください。

バックグラウンド転送サービスは、アプリケーションがバックグラウンドでコンテンツを更新できるように、バックグラウンドフェッチまたはリモート通知と組み合わせられることがよくあります。 次の2つのセクションでは、iOS 6 と iOS 7 の両方で、アプリケーション全体をバックグラウンドで実行するための概念を紹介します。
