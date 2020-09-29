---
title: Xamarin のバックグラウンド転送と Nの Lsession
description: このドキュメントでは、バックグラウンド転送と Nq&a Lsession を使用して大きなイメージのダウンロードを開始し、アプリがバックグラウンドで配置されたときにそのダウンロードを続行する方法を示すチュートリアルを提供します。
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/02/2020
ms.openlocfilehash: b95b78ed36fcec122006cab7ce9663c43f9b3096
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436662"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>Xamarin のバックグラウンド転送と Nの Lsession

バックグラウンド転送は、バックグラウンドを構成し、 `NSURLSession` アップロードまたはダウンロードのタスクをエンキューすることによって開始されます。 アプリケーションが backgrounded、中断、または終了している間にタスクが完了すると、iOS はアプリケーションの *Appdelegate*で完了ハンドラーを呼び出すことによってアプリケーションに通知します。 次の図は、実際の動作を示しています。

 [![バックグラウンド転送を開始するには、バックグラウンドの Nn セッションを構成し、アップロードまたはダウンロードのタスクをエンキューします。](background-transfer-walkthrough-images/transfer.png)](background-transfer-walkthrough-images/transfer.png#lightbox)

## <a name="configuring-a-background-session"></a>バックグラウンドセッションの構成

バックグラウンドセッションを作成するには、新しいを作成 `NSUrlSession` し、オブジェクトを使用して構成し `NSUrlSessionConfiguration` ます。

構成オブジェクトは、セッションが実行できる操作と実行できるタスクの種類を決定します。
メソッドを使用して構成されたセッションは、独立した `CreateBackgroundSessionConfiguration` プロセスで実行され、随意 (WiFi) 転送を実行してデータとバッテリの寿命を保持します。
次のコードサンプルは、 `CreateBackgroundSessionConfiguration` メソッドと一意の文字列識別子を使用して、バックグラウンド転送セッションを適切に設定する方法を示しています。

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

バックグラウンドセッションを構成したので、転送を処理するタスクを開始してみましょう。 セッションデリゲートと呼ばれるインスタンスを使用して、これらのタスクを追跡でき `NSUrlSessionDelegate` ます。 セッションデリゲートは、バックグラウンドで終了または中断されたアプリケーションをスリープ解除して、認証、エラー、または転送の完了を処理する役割を担います。

には、 `NSUrlSessionDelegate` 転送の状態を確認するための次の基本的な方法が用意されています。

- *DidFinishEventsForBackgroundSession* -このメソッドは、すべてのタスクが完了し、転送が完了すると呼び出されます。
- *DidReceiveChallenge* -承認が必要なときに資格情報を要求するために呼び出されます。
- *DidBecomeInvalidWithError* -が無効になった場合に呼び出され  `NSURLSession` ます。

バックグラウンドセッションでは、実行しているタスクの種類に応じて、より特殊なデリゲートが必要になります。 バックグラウンドセッションは、次の2種類のタスクに制限されます。

- *アップロードタスク* -型のタスク  `NSUrlSessionUploadTask` は、を `INSUrlSessionTaskDelegate` 実装するインターフェイスを使用し `INSUrlSessionDelegate` ます。 これにより、アップロードの進行状況を追跡したり、HTTP リダイレクトを処理したりするための追加のメソッドが提供されます。
- *タスクのダウンロード* -種類がのタスク  `NSUrlSessionDownloadTask` は、 `INSUrlSessionDownloadDelegate` およびを実装するインターフェイスを使用し `INSUrlSessionDelegate` `INSUrlSessionTaskDelegate` ます。 これにより、アップロードタスクのすべてのメソッドに加えて、ダウンロードの進行状況を追跡し、ダウンロードタスクが再開または完了したことを確認するためのダウンロード固有のメソッドが提供されます。

次のコードは、URL からイメージをダウンロードするために使用できるタスクを定義します。 タスクは、バックグラウンドセッションでを呼び出し、URL 要求を渡すことによって開始され `CreateDownloadTask` ます。

```csharp
const string DownloadURLString = "http://xamarin.com/images/xamarin.png"; // or other hosted file
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

次に、新しいセッションダウンロードのデリゲートを作成して、このセッションのすべてのダウンロードタスクを追跡します。 デリゲートクラスは、を継承 `NSObject` し、必要なインターフェイスを実装する必要があります。

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

ダウンロードタスクの進行状況を確認するには、メソッドをオーバーライドして `DidWriteData` 進行状況を追跡し、UI も更新します。 アプリケーションがフォアグラウンドにある場合、または次回アプリケーションを開いたときにユーザーが待機する場合、UI の更新はすぐに表示されます。

Session delegate API は、タスクと対話するための広範なツールキットを提供します。 セッションデリゲートメソッドの完全な一覧については、API のドキュメントを参照してください `NSUrlSessionDelegate` 。

> [!IMPORTANT]
> バックグラウンドセッションはバックグラウンドスレッドで開始されるため、UI を更新するためのすべての呼び出しは、を呼び出して、iOS がアプリを終了しないようにするために、UI スレッドで明示的に実行する必要があり `InvokeOnMainThread` ます。 

## <a name="handling-transfer-completion"></a>転送の完了の処理

最後の手順は、セッションに関連付けられたすべてのタスクが完了したことをアプリケーションに通知し、新しいコンテンツを処理することです。

で、 `AppDelegate` イベントをサブスクライブし `HandleEventsForBackgroundUrl` ます。 アプリケーションがバックグラウンドに入り、転送セッションが実行されている場合、このメソッドが呼び出され、システムは完了ハンドラーを渡します。

```csharp
public System.Action backgroundSessionCompletionHandler;

public void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

完了ハンドラーを使用して、アプリケーションの処理が完了したことを iOS に知らせます。

セッションでは、転送を処理する複数のタスクが生成される可能性があることを思い出してください。 最後のタスクが完了すると、中断または終了したアプリケーションがバックグラウンドで再起動されます。 次に、アプリケーションは、一意のセッション識別子を使用してに再接続 `NSURLSession` し、 `DidFinishEventsForBackgroundSession` セッションデリゲートでを呼び出します。 このメソッドは、新しいコンテンツを処理するためのアプリケーションの機会です。これには、転送の結果を反映するための UI の更新が含まれます。

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

- [単純なバックグラウンド転送 (サンプル)](/samples/xamarin/ios-samples/simplebackgroundtransfer)