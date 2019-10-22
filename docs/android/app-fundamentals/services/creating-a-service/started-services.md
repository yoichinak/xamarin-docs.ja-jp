---
title: Xamarin Android でサービスを開始しました
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 1b7bed0fc6dba1d9f80524ac3429b7fdcb751ab9
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "70755071"
---
# <a name="started-services-with-xamarinandroid"></a>Xamarin Android でサービスを開始しました

## <a name="started-services-overview"></a>開始されたサービスの概要

通常、開始されたサービスは、直接のフィードバックや結果をクライアントに提供することなく、作業単位を実行します。 作業単位の例としては、ファイルをサーバーにアップロードするサービスがあります。 クライアントは、デバイスから web サイトにファイルをアップロードするようにサービスに要求します。 サービスは、(アプリがフォアグラウンドにアクティビティがない場合でも) ファイルをサイレントにアップロードし、アップロードが完了したらそれ自体を終了します。 開始したサービスは、アプリケーションの UI スレッドで実行されることに注意してください。 これは、サービスが UI スレッドをブロックする作業を実行する場合、必要に応じてスレッドを作成して破棄する必要があることを意味します。

バインドされたサービスとは異なり、"純粋な" サービスとそのクライアントの間に通信チャネルはありません。 これは、開始されたサービスが、バインドされたサービスとは異なるライフサイクルメソッドを実装することを意味します。 次の一覧は、開始されたサービスの共通ライフサイクルメソッドを示しています。

- `OnCreate` &ndash;、サービスが最初に開始されたときに1回呼び出されます。 ここで、初期化コードを実装する必要があります。
- `OnBind` &ndash; このメソッドはすべてのサービスクラスで実装する必要がありますが、開始されたサービスには通常、クライアントがバインドされていません。 このため、開始されたサービスは `null` を返します。 これに対して、ハイブリッドサービス (バインドされたサービスと開始されたサービスの組み合わせ) は、クライアントの `Binder` を実装して返す必要があります。
- `OnStartCommand` は、`StartService` への呼び出しまたはシステムによる再起動に応じて、サービスを開始する要求ごとに呼び出さ &ndash; ます。 これは、サービスが実行時間の長いタスクを開始できる場所です。 このメソッドは、メモリ不足によるシャットダウン後に、システムがサービスの再起動を処理する必要があるかどうかを示す `StartCommandResult` 値を返します。 この呼び出しはメインスレッドで行われます。 この方法については、以下で詳しく説明します。
- このメソッドは、サービスが破棄されているときに呼び出さ &ndash; `OnDestroy` ます。 これは、最終的に必要なクリーンアップを実行するために使用されます。

開始したサービスの重要な方法は、`OnStartCommand` メソッドです。 サービスが何らかの処理を行う要求を受け取るたびに呼び出されます。 @No__t_0 の例を次のコードスニペットに示します。 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

最初のパラメーターは、実行する作業に関するメタデータを含む `Intent` オブジェクトです。 2番目のパラメーターには、メソッドの呼び出しに関する情報を提供する `StartCommandFlags` 値が含まれています。 このパラメーターには、次の2つの値のいずれかを指定できます。

- &ndash; `StartCommandFlag.Redelivery` は、`Intent` が前の `Intent` の再配信であることを意味します。 この値は、サービスが `StartCommandResult.RedeliverIntent` を返したときに、正常にシャットダウンされる前に停止された場合に提供されます。
- `StartCommandFlag.Retry` &dash; 前の `OnStartCommand` の呼び出しが失敗したときに、Android が前回失敗した試行と同じ目的でサービスを再開しようとしたときに、この値を受け取ります。

最後に、3番目のパラメーターは、要求を識別するアプリケーションに固有の整数値です。 複数の呼び出し元が同じサービスオブジェクトを呼び出す可能性があります。 この値は、サービスを停止する要求を、サービスを開始するための特定の要求に関連付けるために使用されます。 詳細については、「[サービスを停止](#Stopping_the_Service)する」セクションを参照してください。 

リソース制約のためにサービスが強制終了された場合の対処方法については、サービスによって `StartCommandResult` 値が Android に提示されます。 @No__t_0 には、次の3つの値を指定できます。

- **[Startcommandresult. NotSticky](xref:Android.App.StartCommandResult.NotSticky)** &ndash; この値は、強制終了されたサービスを再起動する必要がないことを Android に通知します。 この例として、アプリでギャラリーのサムネイルを生成するサービスについて考えてみます。 サービスを強制終了した場合は、サムネイルをすぐに再作成することは重要ではなく、次にアプリを実行するときにサムネイルを再作成 &ndash; ます。
- **[Startcommandresult. スティッキービット](xref:Android.App.StartCommandResult.Sticky)** &ndash; これは、サービスを再起動するように Android に指示しますが、サービスを開始した最後のインテントを配信しません。 処理する保留中のインテントが存在しない場合は、目的のパラメーターに対して `null` が提供されます。 この例としては、音楽プレーヤーアプリがあります。サービスは、音楽を再生する準備ができましたが、最後の曲を再生します。
- **[RedeliverIntent](xref:Android.App.StartCommandResult.RedeliverIntent)** &ndash; この値は、サービスを再起動し、最後の `Intent` を再配信するように Android に指示します。 この例として、アプリのデータファイルをダウンロードするサービスがあります。 サービスを強制終了した場合でも、データファイルをダウンロードする必要があります。 @No__t_0 を返すことによって、Android がサービスを再起動するときに、サービスに対して (ダウンロードするファイルの URL を保持する) 目的も提供されます。 これにより、ダウンロードは再起動または再開できます (コードの正確な実装によって異なります)。

@No__t_0 &ndash; `StartCommandResult.ContinuationMask` には4番目の値があります。 この値は `OnStartCommand` によって返され、Android が強制終了したサービスを続行する方法を説明します。 通常、この値はサービスを開始するために使用されません。

開始したサービスのキーライフサイクルイベントを次の図に示します。 

![ライフサイクルメソッドが呼び出される順序を示す図](started-services-images/started-service-01.png "ライフサイクルメソッドが呼び出される順序を示す図。")

<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>サービスを停止しています

開始したサービスは無期限に実行され続けます。Android では、十分なシステムリソースがある限り、サービスは実行されたままになります。 クライアントがサービスを停止する必要があるか、処理が完了したときにサービスが停止することがあります。 サービスを停止するには、次の2つの方法があります。 

- クライアント (アクティビティなど **[) &ndash; は](xref:Android.Content.Context.StopService*)** 、`StopService` メソッドを呼び出すことによってサービスの停止を要求できます。

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

- **[サービスは](xref:Android.App.Service.StopSelf*)** 、`StopSelf` を呼び出すことによってシャットダウンされる場合があり &ndash;。

    ```csharp
    StopSelf();
    ```

### <a name="using-startid-to-stop-a-service"></a>StartId を使用したサービスの停止

複数の呼び出し元は、サービスの開始を要求できます。 未処理の開始要求がある場合、サービスは、`OnStartCommand` に渡された `startId` を使用して、サービスが途中で停止されるのを防ぐことができます。 @No__t_0 は `StartService` の最新の呼び出しに対応し、が呼び出されるたびにインクリメントされます。 したがって、後続の `StartService` 要求によって `OnStartCommand` の呼び出しがまだ行われていない場合、サービスは `StopSelfResult` を呼び出し、受信した `startId` の最新の値 (単に `StopSelf` を呼び出すのではなく) を渡すことができます。 @No__t_0 の呼び出しによって `OnStartCommand` への対応する呼び出しがまだ行われていない場合、`StopSelf` の呼び出しで使用されている `startId` は最新の `StartService` 呼び出しに対応していないため、システムはサービスを停止しません。

## <a name="related-links"></a>関連リンク

- [サービスのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-startedservicesdemo)
- [Android. App. サービス](xref:Android.App.Service)
- [Android. StartCommandFlags](xref:Android.App.StartCommandFlags)
- [Android. StartCommandResult](xref:Android.App.StartCommandResult)
- [BroadcastReceiver](xref:Android.Content.BroadcastReceiver)
- [Android. コンテンツインテント](xref:Android.Content.Intent)
- [Android. OS. ハンドラー](xref:Android.OS.Handler)
- [Android. ウィジェット. トースト](xref:Android.Widget.Toast)
- [ステータスバーのアイコン](https://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
