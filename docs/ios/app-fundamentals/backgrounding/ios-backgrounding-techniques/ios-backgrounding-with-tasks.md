---
title: タスクを使用した iOS バックグラウンド処理
description: このドキュメントでは、バック グラウンド タスクを使用して、アプリケーションがバック グラウンドで配置された後に、実行時間の長いタスクを実行する方法について説明します。
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: c8d1abebf6dec2b7b5fe76d57ff851fad457f2a8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61170683"
---
# <a name="ios-backgrounding-with-tasks"></a>タスクを使用した iOS バックグラウンド処理

Ios バック グラウンド処理を実行する最も簡単な方法では、backgrounding、要件、タスクに分割し、バック グラウンドでタスクを実行します。 タスクは、厳密な時間制限では、下にあるし、通常 iOS 7 以降で、アプリケーションが ios 6、バック グラウンドに移動した後、処理時間の約 600 秒 (10 分) と 10 分未満を取得します。

バック グラウンド タスクは、次の 3 つのカテゴリに分けることができます。

1.  **バック グラウンド セーフ タスク**- 呼び出された任意の場所、タスクがあるアプリケーションでしない中断された、アプリケーションがバック グラウンドを入力する必要があります。
1.  **DidEnterBackground タスク**: 中に呼び出されます、`DidEnterBackground`クリーンアップと状態の保存を支援するアプリケーションのライフ サイクル メソッド。
1.  **転送 (iOS 7 以降) をバック グラウンド**-iOS 7 でネットワーク転送を実行する、特殊なバック グラウンド タスクのために使用します。 通常のタスクとは異なりバック グラウンド転送に事前に決められた時間制限はありません。


バック グラウンド セーフと`DidEnterBackground`タスクは、iOS 6 および iOS 7、わずかな違いの両方を使用しても安全です。 これら 2 種類のタスクをさらに詳しく見ていきましょう。

## <a name="creating-background-safe-tasks"></a>バック グラウンドの安全なタスクを作成します。

一部のアプリケーションには、べきではないによって中断される iOS アプリケーションで状態を変更する必要がありますタスクが含まれます。 これらのタスクが中断されることを防ぐ方法の 1 つでは、実行時間の長いタスクとして iOS に登録します。 パターンを使用できますこの任意の場所、アプリケーションで中断されているタスクには、アプリ ユーザーの put 背景にする必要がありますを必要はありません。 このパターンの優秀な候補は、サーバーに新しいユーザーの登録情報を送信することや、ログイン情報を確認するなどのタスクになります。

次のコード スニペットでは、バック グラウンドで実行するタスクの登録を示しています。

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

登録プロセスのペアを一意の識別子を持つタスク`taskID`、一致にラップし、`BeginBackgroundTask`と`EndBackgroundTask`呼び出し。 識別子を生成することを呼び出し、`BeginBackgroundTask`メソッドを`UIApplication`オブジェクト、および新しいスレッドでは、通常、実行時間の長いタスクを開始します。 タスクが完了したら、呼び出して`EndBackgroundTask`し、同じ識別子を渡します。 重要な場合は、iOS のアプリケーションが終了するため、これは、`BeginBackgroundTask`呼び出しが、一致する`EndBackgroundTask`します。

> [!IMPORTANT]
> バック グラウンドの安全なタスクは、メイン スレッドまたはアプリケーションのニーズに応じて、バック グラウンド スレッドで実行できます。


## <a name="performing-tasks-during-didenterbackground"></a>DidEnterBackground 中にタスクを実行します。

バック グラウンドの安全な実行時間の長いタスクを行う、だけでなく登録は、アプリケーションがバック グラウンドでアップロードされるようにタスクを開始するために使用できます。 iOS でのイベント メソッドを提供する、 *AppDelegate*と呼ばれるクラス`DidEnterBackground`アプリケーション状態を保存、ユーザー データを保存し、アプリケーションがバック グラウンドになる前に機密性の高いコンテンツを暗号化に使用できます。 アプリケーションには約 5 秒間に、このメソッドから返すまたは停止されます。 そのため、クリーンアップ タスクが完了する 5 秒以上かかるはから呼び出すことが内部で、`DidEnterBackground`メソッド。 これらのタスクは、別のスレッドで呼び出す必要があります。

プロセスは、実行時間の長いタスクを登録するのとほぼ同じです。 次のコード スニペットは、アクションで、これを示しています。

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

まず、オーバーライドすることで、`DidEnterBackground`メソッドで、`AppDelegate`を使用して、タスクを登録しますが、`BeginBackgroundTask`前の例で行ったようします。 次に、新しいスレッドを起動し、実行時間の長いタスクを実行します。 なお、`EndBackgroundTask`今すぐから呼び出し、実行時間の長いタスク内で後、`DidEnterBackground`メソッドも返される、既にあります。

> [!IMPORTANT]
> iOS を使用して、[メカニズムのウォッチドッグ](https://developer.apple.com/library/ios/qa/qa1693/_index.html)アプリケーションの UI の応答性を確保します。 アプリケーションで多くの時間を費やす`DidEnterBackground`は UI に応答しなくなります。 により、バック グラウンドで実行するタスクの開始`DidEnterBackground`適時、UI の応答性を維持する方法と、ウォッチドッグがアプリケーションを強制終了するを防ぐに戻ります。


## <a name="handling-background-task-time-limits"></a>バック グラウンド タスクの期限を処理

iOS では時間の長いバック グラウンド タスクを実行する方法と、場合に厳密に制限を配置、`EndBackgroundTask`割り当てられた時間内で呼び出しが行われていない、アプリケーションは終了されます。 追跡することにより、残り時間のバック グラウンド処理と必要な場合に有効期限のハンドラーを使用して、iOS アプリケーションを終了して使えるようにします。

### <a name="accessing-background-time-remaining"></a>残りの時間をバック グラウンドにアクセスします。

登録されたタスクを使用したアプリケーションのバック グラウンドに移動、登録されたタスクが実行する約 600 秒を発生します。 タスクが、静的なを使用して完了するまでの期間を確認しました`BackgroundTimeRemaining`のプロパティ、`UIApplication`クラス。 次のコードが得られますが、バック グラウンド タスクのままを秒単位で、時間。

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>有効期限のハンドラーを使用してアプリの終了を回避します。

アクセスを与えるだけでなく、`BackgroundTimeRemaining`プロパティ、iOS のバック グラウンドの時間期限切れを処理するために正常な手段を提供します、**有効期限ハンドラー**します。 これは、省略可能なタスクに割り当てられた時間の有効期限が近づいているときに実行される取得コードのブロックです。 有効期限のハンドラーのコードを呼び出す`EndBackgroundTask`を渡しますが、タスクの ID を示す、アプリが適切に動作して、iOS がタスクの実行時間がなくなった場合でも、アプリを終了するを防ぎます。 `EndBackgroundTask` 有効期限のハンドラー内と、通常の実行中に呼び出されなければなりません。 

次のように、有効期限のハンドラーは、ラムダ式を使用して匿名関数として表されます。

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

有効期限のハンドラーがコードを実行するために必要ないときに、バック グラウンド タスクを常に有効期限ハンドラーを使用する必要があります。

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>IOS 7 以降のバック グラウンド タスク

バック グラウンド タスクに関しては、iOS 7 での最大の変更とは、どのタスクが実装されていませんが実行したときにです。

事前 iOS 7 では、タスクをバック グラウンドで実行されている必要がある 600 秒完了を思い出してください。 この制限の理由の 1 つが、バック グラウンドで実行するタスクは保持デバイス起動状態、タスクの実行中です。

 [![](ios-backgrounding-with-tasks-images/ios6.png "アプリの起動前 iOS 7 を保持するタスクのグラフ")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

iOS 7 のバック グラウンド処理より長いバッテリ寿命に最適です。 IOS 7 では、バック グラウンド処理便宜的なります: デバイスがスリープ状態にし、代わりに、デバイスが電話呼び出し、通知、着信メール、およびその他の処理を再開したときのチャンク単位で、これらの処理を行うにはデバイスを起動状態に保つことに、代わりにタスクを尊重一般的なが中断します。 次の図は、タスクが違反する過程の方法を把握します。

 [![](ios-backgrounding-with-tasks-images/ios7.png "後の iOS 7 をチャンクに分割するタスクのグラフ")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

タスクの実行時が今後継続的でないため、ネットワーク転送を実行するタスクが iOS 7 で異なる方法で処理する必要があります。 使用する開発者は、`NSURlSession`ネットワーク転送を処理する API。 次のセクションでは、バック グラウンド転送の概要を示します。

 <a name="background-transfers" />

## <a name="background-transfers"></a>バック グラウンド転送

IOS 7 でのバック グラウンド転送のバックボーンでは、新しい`NSURLSession`API。 `NSURLSession` タスクを作成できます。

1.  ネットワークとデバイスの中断経由でコンテンツを転送します。
1.  アップロードして、大きなファイルのダウンロード (*バック グラウンド転送サービス*)。


このしくみについて詳しく見てをみましょう。

### <a name="nsurlsession-api"></a>NSURLSession API

 `NSURLSession` 強力な API は、ネットワーク経由でコンテンツを転送するためです。 ネットワークの中断とアプリケーションの状態の変更によってデータの転送を処理するためのツールのセットを提供します。

`NSURLSession` API がさらに、ネットワーク経由での関連データのブロックのシャトルにタスクを生成する 1 つまたは複数のセッションを作成します。 タスクは、迅速かつ確実にデータを転送する非同期的に実行されます。 `NSURLSession`は非同期でシステムとアプリケーションが完了すると、転送時に知っておくことができますを完了ハンドラー ブロックはすべてのセッションに必要です。

前の iOS 7 と iOS 7 の後の両方で有効なネットワーク転送を実行する場合を確認、`NSURLSession`エンキューの転送に使用でき、定期的なバック グラウンド タスクを使用して、ない場合、転送を実行します。

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
> IOS 6 がバック グラウンドの UI の更新をサポートしていませんし、アプリケーションが終了は、iOS 6 準拠のコードでバック グラウンドから UI を更新する呼び出しを回避します。


`NSURLSession` API に認証を処理、失敗の転送を管理およびクライアント側 - がないサーバー側のエラーを報告する機能の豊富なセットが含まれています。 により、ブリッジのタスクの中断が iOS 7 で導入された時刻を実行し、迅速かつ確実に大きなファイルを転送するためのサポートも提供します。 次のセクションでは、この 2 つ目の機能について説明します。

### <a name="background-transfer-service"></a>バック グラウンド転送サービス

IOS 7 では、前に、バック グラウンドでファイルのダウンロードをアップロードまたは不完全でした。 バック グラウンド タスクを実行する期限を取得するが、ファイルの転送にかかる時間、ネットワーク、およびファイルのサイズによって異なります。 Ios 7 で使用できます、`NSURLSession`を正常にアップロードし、大きなファイルをダウンロードします。 特定`NSURLSession`バック グラウンドで大きいファイルのネットワーク転送を処理するセッションの種類と呼ばれる、*バック グラウンド転送サービス*します。

バック グラウンド転送サービスを使用して開始転送では、オペレーティング システムで管理されており、Api 認証やエラーを処理するために用意されています。 転送は任意の時間制限によってバインドされていないためは、アップロードまたは大きなファイルのダウンロードはバック グラウンドでのコンテンツの自動更新を使用できます。 参照してください、[バック グラウンド転送チュートリアル](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md)サービスを実装する方法の詳細について。

バック グラウンド転送サービスは多くの場合、バック グラウンド フェッチまたはリモート通知アプリケーションをバック グラウンドでコンテンツを更新するとペアになります。 次の 2 つのセクションでは、iOS 6 および iOS 7 の両方で、バック グラウンドで実行する全体のアプリケーションの登録の概念を紹介します。

