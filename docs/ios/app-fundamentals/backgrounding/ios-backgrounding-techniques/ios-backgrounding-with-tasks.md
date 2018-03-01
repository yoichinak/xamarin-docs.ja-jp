---
title: "iOS のタスク Backgrounding"
ms.topic: article
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 0be4e7f1d8719fdd174d51399178eb1bc000c4b3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="ios-backgrounding-with-tasks"></a>iOS のタスク Backgrounding

IOS で backgrounding を実行する最も簡単な方法を開始、backgrounding 要件をタスクに分割し、バック グラウンドでタスクを実行します。 タスクでは、厳密な時間制限値は、下にあるし、通常 iOS 7 以降で約 600 秒 (10 分) アプリケーションは、iOS 6 でバック グラウンドに移動した後の処理時間と 10 分未満を取得します。

バック グラウンド タスクは、次の 3 つのカテゴリに分けることができます。

1.  **バック グラウンド セーフ タスク**: 任意の場所で呼び出されます、タスクがあるアプリケーションが中断された場合、アプリケーションを入力する必要があります、バック グラウンドをしたくないです。
1.  **DidEnterBackground タスク**: 中に呼び出されます、`DidEnterBackground`クリーンアップと状態の保存を支援するアプリケーションのライフ サイクル メソッドです。
1.  **バック グラウンドの転送 (iOS 7 以降)** -iOS 7 でネットワーク転送を実行する特別な種類のバック グラウンド タスクのために使用します。 通常のタスクとは異なりバック グラウンド転送しても、あらかじめ決められた時間制限はありません。


バック グラウンド セーフと`DidEnterBackground`タスクは、iOS 6 と iOS 7、若干の違いの両方で使用しても安全です。 これら 2 種類のより詳細にタスクを調査してみましょう。

## <a name="creating-background-safe-tasks"></a>セーフでバック グラウンド タスクを作成します。

一部のアプリケーションには、べきによって中断される iOS アプリケーションで状態を変更する必要がありますタスクが含まれています。 これらのタスクが中断されることを防ぐ方法の 1 つ実行時間の長いタスクとして ios の登録を開始します。 パターンを使用できますこの任意の場所、アプリケーションで必要がある中断されているタスクには、ユーザー put アプリ背景にする必要があります。 このパターンの最適な候補は、サーバーに、新しいユーザーの登録情報を送信するか、ログイン情報を確認するなどのタスクになります。

次のコード スニペットでは、バック グラウンドで実行するタスクの登録を示しています。

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

登録プロセスのペアを一意の識別子を持つタスク`taskID`、照合にラップし、`BeginBackgroundTask`と`EndBackgroundTask`呼び出しです。 呼び出しを行う場合、識別子を生成する、`BeginBackgroundTask`メソッドを`UIApplication`オブジェクト、および新しいスレッド上で通常実行時間の長いタスクを開始します。 タスクが完了したらと呼ば`EndBackgroundTask`と同じ識別子を渡します。 場合、iOS のアプリケーションは終了することが重要な`BeginBackgroundTask`呼び出しが、一致する`EndBackgroundTask`です。

> [!IMPORTANT]
> **注**: メイン スレッドまたはアプリケーションのニーズに応じて、バック グラウンド スレッドでバック グラウンド セーフであるタスクを実行できます。


## <a name="performing-tasks-during-didenterbackground"></a>DidEnterBackground 中にタスクを実行します。

バック グラウンド セーフを実行時間の長いタスクを行うだけでなく、アプリケーションはバック グラウンドで配置されているようにタスクを開始する登録を使用できます。 iOS でのイベント メソッドを提供する、 *AppDelegate*と呼ばれるクラス`DidEnterBackground`をアプリケーションの状態を保存し、ユーザー データを保存し、アプリケーションはバック グラウンドに入る前に、機密性の高いコンテンツを暗号化を使用できます。 アプリケーションには約 5 秒間に、このメソッドから返すまたは終了を取得します。 そのため、クリーンアップ タスクの完了に 5 秒以上かかる場合がありますから呼び出せる内、`DidEnterBackground`メソッドです。 これらのタスクは、別のスレッドで呼び出す必要があります。

プロセスは、実行時間の長いタスクを登録するのとほぼ同じです。 次のコード スニペットの動作を示します。

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

まず、オーバーライドすることで、`DidEnterBackground`メソッドで、 `AppDelegate`, を使用して、タスクを登録しましたが、`BeginBackgroundTask`前の例で行ったようにします。 次に、新しいスレッドを起動し、実行時間の長いタスクを実行します。 注意してください、`EndBackgroundTask`呼び出しは、今すぐ内で行われた実行時間の長いタスクので、`DidEnterBackground`メソッドも返される、既にあります。

> [!IMPORTANT]
> **注**: iOS を使用して、[メカニズムをウォッチドッグ](http://developer.apple.com/library/ios/qa/qa1693/_index.html)がアプリケーションの UI の応答性を保つためです。 アプリケーションであまり時間を費やす`DidEnterBackground`は UI の応答しなくなります。 により、バック グラウンドで実行するタスクを始める際`DidEnterBackground`適時、UI の応答性を維持して、監視がアプリケーションを強制終了するを妨げてに戻ります。


## <a name="handling-background-task-time-limits"></a>処理のバック グラウンド タスク時間の制限

iOS に配置厳密に制限時間の長い、バック グラウンド タスクを実行できますが、方法と場合、`EndBackgroundTask`割り当てられた時間内で呼び出しが行われない、アプリケーションは終了されます。 追跡することにより、残り backgrounding の時間と必要な場合に有効期限のハンドラーを使用して、iOS アプリケーションを終了して回避できます。

### <a name="accessing-background-time-remaining"></a>残り時間を背景にアクセスします。

登録済みのタスクを含むアプリケーションを取得移動した場合、バック グラウンド、登録されたタスクは実行に約 600 秒を取得します。 タスクが、静的なを使用して完了するまでの期間を確認する`BackgroundTimeRemaining`のプロパティ、`UIApplication`クラスです。 次のコードになりますが、バック グラウンド タスクのままを秒単位で、時間。

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>有効期限のハンドラーを使用してアプリの終了を回避します。

アクセスを与えるだけでなく、`BackgroundTimeRemaining`プロパティ、iOS により、正常なバック グラウンドの時間期限切れの処理、**有効期限ハンドラー**です。 これは、省略可能なコードのブロックをタスクに割り当てられた時間が間もなく切れる場合に実行されます。 有効期限ハンドラーのコードを呼び出す`EndBackgroundTask`を渡しますが、タスク ID、iOS が時間外のタスクが実行する場合でも、アプリを終了しない、アプリが適切に動作していることを示します。 `EndBackgroundTask` 有効期限ハンドラー内でだけでなく、通常の実行中に呼び出されなければなりません。 

有効期限のハンドラーは、以下に示すように、ラムダ式を使用して、匿名関数として表されます。

```csharp
Task.Factory.StartNew( () => {

    //expirationHandler only called if background time allowed exceeded
    var taskId = UIApplication.SharedApplication.BeginBackgroundTask(() => {
        Console.WriteLine("Exhausted time");
        UIApplication.SharedApplication.EndBackgroundTask(taskId); 
    });
    while(myFlag == true)
    {
        Console.WriteLine(UIApplication.SharedApplication.TimeRemaining);
        myFlag = SomeCalculationNeedsMoreTime();
    }
    //Only called if loop terminated due to myFlag and not expiration of time
    UIApplication.SharedApplication.EndBackgroundTask(taskId);
});
```

有効期限のハンドラーがコードを実行するために必要ではできませんが、バック グラウンド タスクで、有効期限のハンドラーを常に使用する必要があります。

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>IOS 7 以降のバック グラウンド タスク

バック グラウンド タスクに関して、iOS 7 での最も大きな変化がないタスクの実装方法、実行したときにします。

事前 iOS 7 では、バック グラウンドで実行中のタスクがある 600 秒を完了に注意してください。 この制限の理由の 1 つは、こと、バック グラウンドで実行中のタスクを保持するデバイス起動状態、タスクの期間を示します。

 [ ![](ios-backgrounding-with-tasks-images/ios6.png "アプリの起動前 iOS 7 を保持するタスクのグラフ")](ios-backgrounding-with-tasks-images/ios6.png)

iOS 7 のバック グラウンド処理は、バッテリを長持ちに最適です。 IOS 7、backgrounding 便宜的なりますデバイスがスリープ状態にし、デバイスが電話をかける、通知、着信電子メール、およびその他の処理を再開すると、チャンク単位で、処理に代わりに操作を行うになるデバイスを起動状態に保つには、代わりにタスクを尊重。一般的な中断します。 次の図は、タスクが破損する可能性がありますの方法を把握します。

 [ ![](ios-backgrounding-with-tasks-images/ios7.png "チャンク後 iOS 7 に分割するタスクのグラフ")](ios-backgrounding-with-tasks-images/ios7.png)

タスクの実行時はないためになった連続、iOS 7 でネットワーク転送を実行するタスクを異なる方法で処理しなければなりません。 開発者は、使用することをお勧め、`NSURlSession`ネットワーク転送を処理する API。 次のセクションでは、バック グラウンド転送の概要です。

 <a name="background-transfers" />

## <a name="background-transfers"></a>バック グラウンド転送

IOS 7 でバック グラウンド転送のバックボーンでは、新しい`NSURLSession`API です。 `NSURLSession` タスクを作成することを使用できます。

1.  ネットワークとデバイスの中断経由でコンテンツを転送します。
1.  アップロードし、ダウンロードのサイズの大きなファイル (*バック グラウンド転送サービス*)。


このしくみ詳しく見てをみましょう。

### <a name="nsurlsession-api"></a>NSURLSession API

 `NSURLSession` ネットワーク経由でコンテンツを転送するためには、強力な API。 ネットワークの中断とアプリケーションの状態の変更点を使用してデータの転送を処理するためのツールのセットを提供します。

`NSURLSession` API は、さらにタスクを作成する、ネットワーク経由での関連するデータ ブロックをシャトル 1 つまたは複数のセッションを作成します。 タスクは、迅速かつ確実にデータを転送する非同期的に実行されます。 `NSURLSession`は非同期のすべてのセッションが必要です、完了ハンドラー ブロックのシステムとアプリケーションが、転送が完了時期を知ることができます。

前 iOS 7 と iOS 7 の後の両方に対して有効なネットワーク転送を実行する場合を確認して、`NSURLSession`エンキュー転送に使用でき、ない場合は、転送を実行する定期的なバック グラウンド タスクを使用します。

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
> **注**: iOS 6 バック グラウンド UI の更新をサポートしないし、アプリケーションを終了よう、iOS 6 に準拠したコードの背景から UI を更新するための呼び出しを作成しないようにします。


`NSURLSession` API には、認証の処理、失敗した転送を管理およびクライアント側 - がないサーバー側のエラーを報告する機能の豊富なセットが含まれています。 により、ブリッジの作業の中断が iOS 7 で導入された時刻を実行し、迅速かつ確実に大きなファイルを転送するに対するサポートも提供します。 次のセクションでは、この 2 つ目の機能について説明します。

### <a name="background-transfer-service"></a>バック グラウンド転送サービス

IOS 7 では、前にアップロードまたはバック グラウンドでファイルをダウンロードする不完全でした。 バック グラウンド タスクが実行するには限られた時間を取得しますが、ファイル転送にかかる時間、ネットワーク、およびファイルのサイズによって異なります。 IOS 7 で使用できる、`NSURLSession`を正常にアップロードし、大きなファイルをダウンロードします。 特定`NSURLSession`バック グラウンドで大きなファイルのネットワーク転送を処理するセッションの種類と呼ばれる、*バック グラウンド転送サービス*です。

バック グラウンド転送サービスを使用して開始転送はオペレーティング システムによって管理され、認証およびエラーを処理するための Api を提供します。 転送が任意の時間制限によってバインドされていないために、アップロードまたはサイズの大きなファイルをダウンロード、自動更新、バック グラウンドでのコンテンツを使用できます。 参照してください、[バック グラウンド転送チュートリアル](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md)サービスを実装する方法の詳細。

バック グラウンド転送サービスは、バック グラウンドでフェッチまたはアプリケーションをバック グラウンドでコンテンツを更新する通知をリモートで多くの場合、ペアリングされています。 次の 2 つのセクションでは、iOS 6、iOS 7 の両方で、バック グラウンドで実行する全体のアプリケーションを登録する次の概念を紹介します。

