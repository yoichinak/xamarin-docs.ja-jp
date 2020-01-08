---
title: Xamarin のバックグラウンド転送と Nの Lsession
description: このドキュメントでは、バックグラウンド転送と Nq&a Lsession を使用して大きなイメージのダウンロードを開始し、アプリがバックグラウンドで配置されたときにそのダウンロードを続行する方法を示すチュートリアルを提供します。
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/02/2020
ms.openlocfilehash: 90d5034f332b311ee83ac2654bcbbc2cde2b11c0
ms.sourcegitcommit: 6f09bc2b760e76a61a854f55d6a87c4f421ac6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/02/2020
ms.locfileid: "75607829"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>Xamarin のバックグラウンド転送と Nの Lsession

バックグラウンド転送を開始するには、バックグラウンド `NSURLSession` を構成し、アップロードまたはダウンロードのタスクをキューに置いてください。 アプリケーションが backgrounded、中断、または終了している間にタスクが完了すると、iOS はアプリケーションの*Appdelegate*で完了ハンドラーを呼び出すことによってアプリケーションに通知します。 次の図は、実際の動作を示しています。

 [バックグラウンド転送を開始するには、バックグラウンドの Nn セッションを構成し、アップロードまたはダウンロードのタスクをキューに置いて、バックグラウンド転送を ![します。](background-transfer-walkthrough-images/transfer.png)](background-transfer-walkthrough-images/transfer.png#lightbox)

## <a name="configuring-a-background-session"></a>バックグラウンドセッションの構成

バックグラウンドセッションを作成するには、新しい `NSUrlSession` を作成し、`NSUrlSessionConfiguration` オブジェクトを使用して構成します。

構成オブジェクトは、セッションが実行できる操作と実行できるタスクの種類を決定します。
`CreateBackgroundSessionConfiguration` 方法を使用して構成されたセッションは、独立したプロセスで実行され、随意 (WiFi) 転送を実行してデータとバッテリの寿命を維持します。
次のコードサンプルは、`CreateBackgroundSessionConfiguration` メソッドと一意の文字列識別子を使用して、バックグラウンド転送セッションを適切に設定する方法を示しています。

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, new MySessionDelegate(), new NSOperationQueue());

}
```

セッションには、構成オブジェクトとは別に、セッションデリゲートとキューも必要です。
キューによって、タスクが完了する順序が決まります。 セッションデリゲートは、転送プロセスを chaperones し、認証、キャッシュ、その他のセッション関連の問題を処理します。

## <a name="working-with-tasks-and-delegates"></a>タスクとデリゲートの操作

バックグラウンドセッションを構成したので、転送を処理するタスクを開始してみましょう。 セッションデリゲートと呼ばれる `NSUrlSessionDelegate` インスタンスを使用して、これらのタスクを追跡できます。 セッションデリゲートは、バックグラウンドで終了または中断されたアプリケーションをスリープ解除して、認証、エラー、または転送の完了を処理する役割を担います。

`NSUrlSessionDelegate` には、転送の状態を確認するための次の基本的な方法が用意されています。

- *DidFinishEventsForBackgroundSession* -このメソッドは、すべてのタスクが完了し、転送が完了すると呼び出されます。
- *DidReceiveChallenge* -承認が必要なときに資格情報を要求するために呼び出されます。
- *DidBecomeInvalidWithError* -`NSURLSession` が無効になった場合に呼び出されます。

バックグラウンドセッションでは、実行しているタスクの種類に応じて、より特殊なデリゲートが必要になります。 バックグラウンドセッションは、次の2種類のタスクに制限されます。

- *アップロードタスク*-`NSUrlSessionUploadTask` 種類のタスクは、`INSUrlSessionDelegate`を実装する `INSUrlSessionTaskDelegate` インターフェイスを使用します。 これにより、アップロードの進行状況を追跡したり、HTTP リダイレクトを処理したりするための追加のメソッドが提供されます。
- [*タスクのダウンロード*]-`NSUrlSessionDownloadTask` 種類のタスクは、`INSUrlSessionDelegate` と `INSUrlSessionTaskDelegate`を実装する `INSUrlSessionDownloadDelegate` インターフェイスを使用します。 これにより、アップロードタスクのすべてのメソッドに加えて、ダウンロードの進行状況を追跡し、ダウンロードタスクが再開または完了したことを確認するためのダウンロード固有のメソッドが提供されます。

次のコードは、URL からイメージをダウンロードするために使用できるタスクを定義します。 タスクは、バックグラウンドセッションで `CreateDownloadTask` を呼び出し、URL 要求を渡すことによって開始されます。

```csharp
const string DownloadURLString = "http://xamarin.com/images/xamarin.png"; // or other hosted file
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

次に、新しいセッションダウンロードのデリゲートを作成して、このセッションのすべてのダウンロードタスクを追跡します。 デリゲートクラスは `NSObject` から継承し、必要なインターフェイスを実装する必要があります。

```csharp
public class MySessionDelegate : NSObject, INSUrlSessionDownloadDelegate
{
  public void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
  {
    Console.WriteLine (string.Format ("DownloadTask: {0}  progress: {1}", downloadTask, progress));
    InvokeOnMainThread( () => {
      // update UI with progress bar, if desired
    });
  }
  ...
}
```

ダウンロードタスクの進行状況を確認するには、`DidWriteData` メソッドをオーバーライドして進行状況を追跡し、UI も更新します。 アプリケーションがフォアグラウンドにある場合、または次回アプリケーションを開いたときにユーザーが待機する場合、UI の更新はすぐに表示されます。

Session delegate API は、タスクと対話するための広範なツールキットを提供します。 セッションデリゲートメソッドの完全な一覧については、`NSUrlSessionDelegate` API のドキュメントを参照してください。

> [!IMPORTANT]
> バックグラウンドセッションはバックグラウンドスレッドで開始されるため、UI を更新するためのすべての呼び出しは、アプリの終了を回避するために `InvokeOnMainThread` を呼び出すことによって、UI スレッドで明示的に実行する必要があります。 

## <a name="handling-transfer-completion"></a>転送の完了の処理

最後の手順は、セッションに関連付けられたすべてのタスクが完了したことをアプリケーションに通知し、新しいコンテンツを処理することです。

`AppDelegate`で、`HandleEventsForBackgroundUrl` イベントをサブスクライブします。 アプリケーションがバックグラウンドに入り、転送セッションが実行されている場合、このメソッドが呼び出され、システムは完了ハンドラーを渡します。

```csharp
public System.Action backgroundSessionCompletionHandler;

public void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

完了ハンドラーを使用して、アプリケーションの処理が完了したことを iOS に知らせます。

セッションでは、転送を処理する複数のタスクが生成される可能性があることを思い出してください。 最後のタスクが完了すると、中断または終了したアプリケーションがバックグラウンドで再起動されます。 次に、アプリケーションは一意のセッション識別子を使用して `NSURLSession` に再接続し、セッションデリゲートで `DidFinishEventsForBackgroundSession` を呼び出します。 このメソッドは、新しいコンテンツを処理するためのアプリケーションの機会です。これには、転送の結果を反映するための UI の更新が含まれます。

```csharp
public void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

新しいコンテンツの処理が完了したら、完了ハンドラーを呼び出して、アプリケーションのスナップショットを取得してスリープ状態に戻すことが安全であることをシステムに知らせます。

```csharp
public void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  var appDelegate = UIApplication.SharedApplication.Delegate as AppDelegate;

  // Handle new information, update UI, etc.

  // call completion handler when you're done
  if (appDelegate.backgroundSessionCompletionHandler != null) {
    NSAction handler = appDelegate.backgroundSessionCompletionHandler;
    appDelegate.backgroundSessionCompletionHandler = null;
    handler.Invoke ();
  }
}
```

このチュートリアルでは、iOS 7 以降でバックグラウンド転送サービスを実装する基本的な手順について説明します。

## <a name="related-links"></a>関連リンク

- [単純なバックグラウンド転送 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplebackgroundtransfer)
