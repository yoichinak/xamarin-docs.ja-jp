---
title: "Xamarin.Android で開始されているサービス"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 8c2d68f08fea5b808b627803da9f9ddb7d62eb6c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="started-services-with-xamarinandroid"></a>Xamarin.Android で開始されているサービス

## <a name="started-services-overview"></a>開始されているサービスの概要

通常、開始されているサービスは、クライアントに直接フィードバックまたは結果を提供せず作業の単位を実行します。 作業の単位の例は、サービスをサーバーにファイルをアップロードします。 クライアントでは、web サイトにデバイスからファイルをアップロードするサービスを要求を行います。 サービスはサイレント モードでファイルのアップロード (アプリには、フォア グラウンドでのアクティビティがあるない) 場合でも、し、アップロードが完了したときに終了します。 開始されるサービス アプリケーションの UI スレッドで実行されることに注意してに重要です。 つまりサービスが、UI スレッドをブロックする作業を実行する場合は、その必要がありますを作成、必要に応じて、スレッドの破棄します。

バインドされているサービスとは異なり、「純粋」開始されるサービスとクライアント間の通信チャネルはありません。 これは、開始されるサービスがバインドされているサービスよりもいくつかの別のライフ サイクル メソッドを実装することを意味します。 次の一覧は、開始されるサービスの一般的なライフ サイクル メソッドを示しています。

* `OnCreate` &ndash; サービスは最初の起動時に 1 回呼び出されます。 これは、初期化コードを実装する必要があります。
* `OnBind` &ndash; このメソッドは、開始されるサービスは通常ありませんにバインドされているクライアントが、すべてのサービス クラスで実装する必要があります。 このため、開始されるサービスを返すだけ`null`です。 実装し、返す (あるバインドされているサービスと開始されるサービスの組み合わせ)、ハイブリッド サービスがこれに対しが、`Binder`クライアント。
* `OnStartCommand` &ndash; 呼び出しに応答するか、サービスを開始するには、各要求に対して呼び出さ`StartService`またはシステムで再起動します。 これは、サービスが、実行時間の長いタスクを開始できます。 このメソッドを返します、`StartCommandResult`を示す値方法か、システムがメモリ不足のため、シャット ダウン後、サービスの再起動を処理する必要があります。 この呼び出しは、メイン スレッドで行われます。 このメソッドで詳しく説明します。
* `OnDestroy` &ndash; このメソッドは、サービスが破棄されるときに呼び出されます。 最後の実行に使用されるために必要なクリーンアップします。

開始されるサービスの重要なメソッドは、`OnStartCommand`メソッドです。 たびに呼び出されること、サービスをいくつかの作業を行うには要求を受信します。 次のコード スニペットの例に示します`OnStartCommand`: 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

最初のパラメーターは、`Intent`を実行する作業についてのメタ データを含むオブジェクト。 2 番目のパラメーターが含まれています、`StartCommandFlags`メソッドの呼び出しに関する情報を提供する値。 このパラメーターは、2 つの値のいずれか。

* `StartCommandFlag.Redelivery` &ndash; つまり、`Intent`は、以前の再配信`Intent`です。 サービスが返されるときに、この値が指定されて`StartCommandResult.RedeliverIntent`にありますが正しくシャット ダウンする前に停止されました。
* `StartCommandFlag.Retry` &dash; 前にこの値が受信した`OnStartCommand`呼び出しに失敗しました、Android が以前の処理が失敗として同じ目的でサービスを再開しようとします。
 
最後に、3 番目のパラメーターは、要求を識別する、アプリケーションに一意の整数値です。 複数の呼び出し元が、同じサービス オブジェクトを呼び出すことがあります。 サービスを開始する特定の要求にサービスを停止する要求を関連付けるには、この値が使用されます。 セクションで詳しく取り上げます[サービスを停止する](#Stopping_the_Service)です。 

値`StartCommandResult`サービスによって返される、修正案として Android をリソースの制約のため、サービスが中止された場合の対処方法。 3 つの値がある`StartCommandResult`:

* **[StartCommandResult.NotSticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.NotSticky/)** &ndash; This value tells Android that it is not necessary to restart the service that it has killed. この例としては、アプリ内でギャラリーのサムネイルを生成するサービスを検討してください。 サムネイルをすぐに再作成するために重要でない場合は、サービスを強制終了すると、&ndash;次回アプリを実行、サムネイルを再作成できます。
* **[StartCommandResult.Sticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.Sticky/)**  &ndash;これにより、サービスを再開するが、サービスを開始した最後の目的を伝えるには、Android に指示します。 処理するための保留中のインテントが存在しない場合、`null`インテント パラメーターに提供されます。 この例には、音楽プレーヤー アプリ; 可能性があります。サービスは、音楽を再生する準備が整って再開されますが、最後の曲が再生されます。 
* **[StartCommandResult.RedeliverIntent](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.RedeliverIntent/)** &ndash; This value is will tell Android to restart the service and re-deliver the last `Intent`. この例は、サービス アプリケーションのデータ ファイルをダウンロードします。 サービスが強制終了された場合、データ ファイルがダウンロードする必要があります。 返すことによって`StartCommandResult.RedeliverIntent`Android がサービスにインテント (をダウンロードするファイルの URL を保持) も提供サービスを再起動すると、します。 これは、再起動するか、(に応じてコードの正確な実装) を再開するためにダウンロードを有効になります。

場合、4 つ目の値がある`StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`です。 この値がによって返される`OnStartCommand`Android は、サービスを続行する方法を説明します、終了しました。 この値は、サービスを開始する通常使用されません。

このダイアグラムに開始されるサービスのキーのライフ サイクル イベントを示します。 

![ライフ サイクル メソッドが呼び出される順序を示すダイアグラム](started-services-images/started-service-01.png "ライフ サイクル メソッドが呼び出される順序を表示する図。")


<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>サービスを停止します。

開始されるサービスは引き続き実行無制限です。実行中のサービスは、android が十分なシステム リソースがある限り保持されます。 クライアントがサービスを停止する必要がありますか、サービスが停止自体の作業が終了します。 これには、サービスを停止する 2 つの方法があります。 
 
* **[Android.Content.Context.StopService()](https://developer.xamarin.com/api/member/Android.Content.Context.StopService/p/Android.Content.Intent/)** &ndash; A client (such as an Activity) can request a service stop by calling the `StopService` method: 

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

* **[Android.App.Service.StopSelf()](https://developer.xamarin.com/api/member/Android.App.Service.StopSelf()/)** &ndash; A service may shut itself down by invoking the `StopSelf`:

    ```csharp
    StopSelf();
    ```
    
### <a name="using-startid-to-stop-a-service"></a>StartId を使用してサービスを停止するには

複数の呼び出し元は、サービスを開始することを要求できます。 サービスが使用できる未処理の開始要求がある場合、`startId`に渡される`OnStartCommand`を途中で停止してから、サービスを防ぐためにします。 `startId`最新の呼び出しに対応する`StartService`、されが呼び出されるたびにインクリメントされます。 そのため場合に、後続の要求`StartService`への呼び出しがまだ生まれましたいない`OnStartCommand`、サービスを呼び出すことができます`StopSelfResult`の最新の値を渡す`startId`受信した (だけを呼び出す代わりに`StopSelf`)。 呼び出し`StartService`に対応する呼び出しがまだ生まれましたいない`OnStartCommand`、ために、システムは、サービスを停止できませんが、`startId`で使用される、`StopSelf`呼び出しは、最新バージョンに対応していない`StartService`を呼び出します。


## <a name="related-links"></a>関連リンク

- [StartedServicesDemo (sample)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/StartedServicesDemo/)
- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service)
- [Android.App.StartCommandFlags](https://developer.xamarin.com/api/type/Android.App.StartCommandFlags)
- [Android.App.StartCommandResult](https://developer.xamarin.com/api/type/Android.App.StartCommandResult)
- [Android.Content.BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Android.Content.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent)
- [Android.OS.Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Android.Widget.Toast](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
- [ステータス バーのアイコン](http://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
