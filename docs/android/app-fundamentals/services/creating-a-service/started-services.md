---
title: Xamarin Android でサービスを開始しました
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 6075b125f36625a8dec12c041631e3794a71cc6a
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455367"
---
# <a name="started-services-with-xamarinandroid"></a>Xamarin Android でサービスを開始しました

## <a name="started-services-overview"></a>開始されたサービスの概要

通常、開始されたサービスは、直接のフィードバックや結果をクライアントに提供することなく、作業単位を実行します。 作業単位の例としては、ファイルをサーバーにアップロードするサービスがあります。 クライアントは、デバイスから web サイトにファイルをアップロードするようにサービスに要求します。 サービスは、(アプリがフォアグラウンドにアクティビティがない場合でも) ファイルをサイレントにアップロードし、アップロードが完了したらそれ自体を終了します。 開始したサービスは、アプリケーションの UI スレッドで実行されることに注意してください。 これは、サービスが UI スレッドをブロックする作業を実行する場合、必要に応じてスレッドを作成して破棄する必要があることを意味します。

バインドされたサービスとは異なり、"純粋な" サービスとそのクライアントの間に通信チャネルはありません。 これは、開始されたサービスが、バインドされたサービスとは異なるライフサイクルメソッドを実装することを意味します。 次の一覧は、開始されたサービスの共通ライフサイクルメソッドを示しています。

- `OnCreate`&ndash;サービスが最初に開始されたときに1回呼び出されます。 ここで、初期化コードを実装する必要があります。
- `OnBind`&ndash;このメソッドは、すべてのサービスクラスで実装する必要があります。ただし、開始されたサービスには、通常、クライアントがバインドされていません。 このため、開始されたサービスはを返し `null` ます。 これに対して、ハイブリッドサービス (バインドされたサービスと開始されたサービスの組み合わせ) は、クライアントのを実装して返す必要があり `Binder` ます。
- `OnStartCommand`の &ndash; 呼び出し `StartService` またはシステムによる再起動に応じて、サービスを開始する要求ごとに呼び出されます。 これは、サービスが実行時間の長いタスクを開始できる場所です。 このメソッドは、  `StartCommandResult` メモリ不足によるシャットダウン後に、システムがサービスの再起動を処理する必要があるかどうかを示す値を返します。 この呼び出しはメインスレッドで行われます。 この方法については、以下で詳しく説明します。
- `OnDestroy`&ndash;このメソッドは、サービスが破棄されているときに呼び出されます。 これは、最終的に必要なクリーンアップを実行するために使用されます。

開始したサービスの重要なメソッドは、 `OnStartCommand` メソッドです。 サービスが何らかの処理を行う要求を受け取るたびに呼び出されます。 次のコードスニペットは、の例です `OnStartCommand` 。 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

最初のパラメーターは、 `Intent` 実行する作業に関するメタデータを含むオブジェクトです。 2番目のパラメーターには、 `StartCommandFlags` メソッド呼び出しに関する情報を提供する値が含まれています。 このパラメーターには、次の2つの値のいずれかを指定できます。

- `StartCommandFlag.Redelivery`&ndash;これ `Intent` は、が以前のの再配信であることを意味し `Intent` ます。 この値は、サービスが返されたときに、 `StartCommandResult.RedeliverIntent` 正常にシャットダウンされる前に停止された場合に提供されます。
-`StartCommandFlag.Retry`&dash;この値は、前回の `OnStartCommand` 呼び出しが失敗し、Android が前回失敗した試行と同じ目的でサービスを再開しようとしたときに受信されます。

最後に、3番目のパラメーターは、要求を識別するアプリケーションに固有の整数値です。 複数の呼び出し元が同じサービスオブジェクトを呼び出す可能性があります。 この値は、サービスを停止する要求を、サービスを開始するための特定の要求に関連付けるために使用されます。 詳細については、「 [サービスを停止](#Stopping_the_Service)する」セクションを参照してください。 

この値 `StartCommandResult` は、リソースの制約によってサービスが強制終了された場合の対処方法について、サービスから Android への提案として返されます。 には、次の3つの値を指定でき `StartCommandResult` ます。

- **[Startcommandresult. NotSticky](xref:Android.App.StartCommandResult.NotSticky)** &ndash; この値は、強制終了されたサービスを再起動する必要がないことを Android に指示します。 この例として、アプリでギャラリーのサムネイルを生成するサービスについて考えてみます。 サービスを強制終了した場合は、サムネイルをすぐに再作成することは重要ではなく、 &ndash; 次回アプリを実行するときにサムネイルを再作成できます。
- **[Startcommandresult. スティッキービット](xref:Android.App.StartCommandResult.Sticky)** は、 &ndash; サービスを再起動するように Android に指示しますが、サービスを開始した最後のインテントは提供しません。 処理する保留中のインテントがない場合は、 `null` インテントパラメーターにが指定されます。 この例としては、音楽プレーヤーアプリがあります。サービスは、音楽を再生する準備ができましたが、最後の曲を再生します。
- **[Startcommandresult. RedeliverIntent](xref:Android.App.StartCommandResult.RedeliverIntent)** &ndash; この値は、サービスを再起動し、最後のを再配信するように Android に指示し `Intent` ます。 この例として、アプリのデータファイルをダウンロードするサービスがあります。 サービスを強制終了した場合でも、データファイルをダウンロードする必要があります。 を返すことによって `StartCommandResult.RedeliverIntent` 、Android がサービスを再起動すると、サービスに対して (ダウンロードするファイルの URL を保持する) インテントも提供されます。 これにより、ダウンロードは再起動または再開できます (コードの正確な実装によって異なります)。

には4番目の値があり `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask` ます。 この値はによって返され `OnStartCommand` 、Android が強制終了したサービスを続行する方法を説明します。 通常、この値はサービスを開始するために使用されません。

開始したサービスのキーライフサイクルイベントを次の図に示します。 

![ライフサイクルメソッドが呼び出される順序を示す図](started-services-images/started-service-01.png "ライフサイクルメソッドが呼び出される順序を示す図。")

<a name="Stopping_the_Service"></a>

## <a name="stopping-the-service"></a>サービスを停止しています

開始したサービスは無期限に実行され続けます。Android では、十分なシステムリソースがある限り、サービスは実行されたままになります。 クライアントがサービスを停止する必要があるか、処理が完了したときにサービスが停止することがあります。 サービスを停止するには、次の2つの方法があります。 

- **[Android... Context. StopService ()](xref:Android.Content.Context.StopService*)** &ndash; クライアント (アクティビティなど) は、メソッドを呼び出すことによって、サービスの停止を要求でき `StopService` ます。

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

- **[Android. Service. StopSelf ()](xref:Android.App.Service.StopSelf*)** &ndash; サービスは、次のものを呼び出すことによってシャットダウンされることがあり `StopSelf` ます。

    ```csharp
    StopSelf();
    ```

### <a name="using-startid-to-stop-a-service"></a>StartId を使用したサービスの停止

複数の呼び出し元は、サービスの開始を要求できます。 未処理の開始要求がある場合、サービスは、に渡されたを使用して、サービスが途中で停止されるのを防ぐことができ `startId` `OnStartCommand` ます。 は、の `startId` 最新の呼び出しに対応し、が呼び出されるたびに `StartService` インクリメントされます。 したがって、への後続の要求によっての `StartService` 呼び出しがまだ行われていない場合、 `OnStartCommand` サービスはを呼び出すことができ `StopSelfResult` ます。これは、を `startId` 呼び出すだけではなく、受信した最新の値を渡し `StopSelf` ます。 の呼び出しに `StartService` よってへの対応する呼び出しがまだ行われていない場合、 `OnStartCommand` `startId` 呼び出しで使用されるは最新の `StopSelf` 呼び出しに対応していないため、システムはサービスを停止しません `StartService` 。

## <a name="related-links"></a>関連リンク

- [サービスのデモ (サンプル)](/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-startedservicesdemo)
- [Android. App. サービス](xref:Android.App.Service)
- [Android. StartCommandFlags](xref:Android.App.StartCommandFlags)
- [Android. StartCommandResult](xref:Android.App.StartCommandResult)
- [BroadcastReceiver](xref:Android.Content.BroadcastReceiver)
- [Android. コンテンツインテント](xref:Android.Content.Intent)
- [Android. OS. ハンドラー](xref:Android.OS.Handler)
- [Android. ウィジェット. トースト](xref:Android.Widget.Toast)
- [ステータスバーのアイコン](https://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)