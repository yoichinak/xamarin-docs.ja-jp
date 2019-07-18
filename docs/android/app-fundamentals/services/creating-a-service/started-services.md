---
title: Xamarin.Android で開始されているサービス
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: df3d1bba57c1caf23c615410a184bc5458fc848b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013368"
---
# <a name="started-services-with-xamarinandroid"></a>Xamarin.Android で開始されているサービス

## <a name="started-services-overview"></a>開始されているサービスの概要

開始されているサービスは、通常、直接のフィードバックや結果をクライアントに提供することがなく作業単位を実行します。 作業の単位の例は、サーバーにファイルをアップロードするサービスです。 クライアントは、web サイトにデバイスからファイルをアップロードするサービスに要求を行います。 (アプリには、フォア グラウンドでのアクティビティがあるない) 場合でも、ファイルをアップロード サイレント モードでは、アップロードが完了すると、自身を終了するとします。 アプリケーションの UI スレッドで開始されるサービスを実行するには重要です。 つまり、サービスが、UI スレッドをブロックする作業を実行する場合にする必要がありますを作成、必要に応じて、スレッドの破棄します。

バインドされているサービスとは異なり、「純粋」開始サービスとクライアント間の通信チャネルはありません。 これは、開始サービスがバインドされているサービスよりもいくつかの別のライフ サイクル メソッドを実装することを意味します。 次の一覧は、開始されるサービスで共通のライフ サイクル メソッドを示しています。

* `OnCreate` &ndash; サービスは最初の起動時に 1 回呼び出されます。 これは、初期化コードを実装する必要があります。
* `OnBind` &ndash; このメソッドは、開始されるサービスは通常ありませんにバインドされているクライアントが、すべてのサービス クラスによって実装する必要があります。 このため、開始されるサービスを返すだけ`null`です。 実装し、返す (つまり、バインドされているサービスと開始されるサービスの組み合わせ)、ハイブリッド サービスがこれに対しが、`Binder`クライアント。
* `OnStartCommand` &ndash; 呼び出しに応答でサービスを開始するには、各要求に対して呼び出されて`StartService`またはシステムを再起動します。 これは、サービスが、実行時間の長いタスクを開始できます。 メソッドを返します、`StartCommandResult`を示す値方法か、システムがメモリ不足のため、シャット ダウン後、サービスの再起動を処理する必要があります。 この呼び出しでは、メイン スレッドで行われます。 このメソッドは、以下で詳しく説明します。
* `OnDestroy` &ndash; このメソッドは、サービスが破棄されるときに呼び出されます。 任意の最後の実行に使用されますクリーンアップが必要です。

開始されるサービスの重要なメソッドは、`OnStartCommand`メソッド。 呼び出されるたびに、サービスがいくつかの作業を行う要求を受信します。 次のコード スニペットの例に示します`OnStartCommand`: 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

最初のパラメーターは、`Intent`を実行する作業に関するメタデータを格納しているオブジェクト。 2 番目のパラメーターが含まれています、`StartCommandFlags`メソッドの呼び出しに関する情報を提供する値。 このパラメーターは、2 つの値のいずれか。

* `StartCommandFlag.Redelivery` &ndash; つまり、`Intent`は前回の再配信`Intent`します。 サービスが返されるときに、この値が提供される`StartCommandResult.RedeliverIntent`ですが、正しくシャット ダウンする前に停止されました。
* `StartCommandFlag.Retry` &dash; 前にこの値が受信した`OnStartCommand`呼び出しに失敗しました、Android が以前の失敗として同じ目的で、もう一度サービスを開始しようとします。
 
最後に、3 番目のパラメーターは、要求を識別するアプリケーションに一意の整数値です。 複数の呼び出し元が同じサービス オブジェクトを呼び出すことができます可能性があります。 この値は、サービスを開始する特定の要求でサービスを停止する要求を関連付けに使用されます。 セクションで詳しく説明[サービスを停止する](#Stopping_the_Service)します。 

値`StartCommandResult`で返される、サービス、修正候補として android リソースの制約のため、サービスが強制終了された場合に実行します。 3 つの値がある`StartCommandResult`:

* **[StartCommandResult.NotSticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.NotSticky/)**  &ndash;この値は、これが強制終了サービスを再起動する必要はありません Android を示します。 この例として、アプリ内でギャラリーのサムネイルを生成するサービスを検討してください。 サムネイルをすぐに再作成するために重要でないサービスが強制終了された場合&ndash;アプリが実行されて、次回、サムネイルを再作成できます。
* **[StartCommandResult.Sticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.Sticky/)**  &ndash;このサービスを再起動するが、配信サービスを開始した最後のインテントが Android に指示します。 保留中のインテントを処理するにはない場合、`null`インテントのパラメーターが提供されます。 この例で、音楽プレーヤー アプリ; 可能性があります。サービスは、音楽を再生する準備ができたら再開されますが、最後の曲が再生されます。 
* **[StartCommandResult.RedeliverIntent](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.RedeliverIntent/)** &ndash; This value is will tell Android to restart the service and re-deliver the last `Intent`. この例は、アプリのデータ ファイルをダウンロードするサービスです。 サービスが強制終了された場合、データ ファイルがダウンロードする必要があります。 返すことによって`StartCommandResult.RedeliverIntent`Android は、サービスを (ダウンロードするファイルの URL を保持) することを目的にも、サービスを再起動するとします。 これは、再起動するか、(によって、コードの正確な実装) を再開するためにダウンロードを有効になります。

場合、4 つ目の値がある`StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`します。 この値がによって返される`OnStartCommand`Android がサービスを続行する方法を説明しますが強制終了にします。 この値は、サービスを開始する通常使用されません。

このダイアグラムに開始されるサービスのキーのライフ サイクル イベントを示します。 

![ライフ サイクル メソッドが呼び出される順序を示す図](started-services-images/started-service-01.png "ライフ サイクル メソッドが呼び出される順序を示す図。")


<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>サービスを停止します。

開始サービスが無制限に実行し続ける十分なシステム リソースがある限り、android は、サービスは実行中に保持されます。 クライアントがサービスを停止する必要がありますか、サービスが停止自体の作業が完了するとします。 サービスを停止する 2 つの方法はあります。 
 
* **[Android.Content.Context.StopService()](https://developer.xamarin.com/api/member/Android.Content.Context.StopService/p/Android.Content.Intent/)** &ndash; A client (such as an Activity) can request a service stop by calling the `StopService` method: 

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

* **[Android.App.Service.StopSelf()](https://developer.xamarin.com/api/member/Android.App.Service.StopSelf()/)** &ndash; A service may shut itself down by invoking the `StopSelf`:

    ```csharp
    StopSelf();
    ```
    
### <a name="using-startid-to-stop-a-service"></a>StartId を使用してサービスを停止するには

複数の呼び出し元は、サービスを開始することを要求できます。 サービスが使用できる、未処理の開始要求がある場合、`startId`に渡される`OnStartCommand`サービスが途中で停止するを防ぐためにします。 `startId`は最新の呼び出しに対応`StartService`、され、それが呼び出されるたびにインクリメントされます。 そのため、後続の要求の場合`StartService`への呼び出しでまだ結果がない`OnStartCommand`、サービスを呼び出すことができます`StopSelfResult`の最新の値を渡す`startId`受信した (呼び出すだけではなく`StopSelf`)。 呼び出し`StartService`に対応する呼び出しでまだ結果がない`OnStartCommand`、ために、システムは、サービスを停止できませんが、`startId`で使用される、`StopSelf`呼び出しは、最新バージョンに対応していない`StartService`を呼び出します。


## <a name="related-links"></a>関連リンク

- [StartedServicesDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/StartedServicesDemo/)
- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service)
- [Android.App.StartCommandFlags](https://developer.xamarin.com/api/type/Android.App.StartCommandFlags)
- [Android.App.StartCommandResult](https://developer.xamarin.com/api/type/Android.App.StartCommandResult)
- [Android.Content.BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Android.Content.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent)
- [Android.OS.Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Android.Widget.Toast](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
- [ステータス バーのアイコン](https://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
