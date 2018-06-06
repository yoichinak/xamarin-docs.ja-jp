---
title: バック グラウンド転送および Xamarin.iOS で NSURLSession
description: このドキュメントでは、バック グラウンド転送および NSUrlSession を使用して、大きなイメージのダウンロードを開始し、バック グラウンドでアプリを配置すると、そのダウンロードを続行する方法を示すチュートリアルを提供します。
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 08a0ba1337c0d28d1f0d60d04394ccaf4a9ccfc7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783740"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>バック グラウンド転送および Xamarin.iOS で NSURLSession

バック グラウンド転送は、バック グラウンドの構成によって開始される`NSURLSession`およびアップロードまたはダウンロード タスクをエンキューします。 IOS がアプリケーションの完了ハンドラーを呼び出すことによって、アプリケーションを通知する場合は、アプリケーションが backgrounded、中断、または終了中にタスクが完了*AppDelegate*です。 次の図の動作例を示します。

 [![](background-transfer-walkthrough-images/transfer.png "バック グラウンド NSURLSession を構成することによって、バック グラウンド転送が開始され、エンキューのアップロードまたはダウンロード タスク")](background-transfer-walkthrough-images/transfer.png#lightbox)

この内でどのようにコードを見てみましょう。

## <a name="configuring-a-background-session"></a>バック グラウンド セッションを構成します。

バック グラウンド セッションを作成、新しい`NSUrlSession`を使用して構成し、`NSUrlSessionConfiguration`オブジェクト。

構成オブジェクトは、セッションが実行できる内容を決定し、実行できるタスクの種類。
セッション構成を使用して、`CreateBackgroundSessionConfiguration`メソッドは別のプロセスで実行され、データとバッテリ寿命を保持するために任意の (WiFi) 転送を実行します。
次のコード サンプルではバック グラウンド転送セッションを使用して、適切にセットアップ、`CreateBackgroundSessionConfiguration`メソッドと一意の文字列識別子。

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, (NSUrlSessionDelegate) new MySessionDelegate(), new NSOperationQueue());

}
```

構成オブジェクトとは別のセッションも必要です、セッションのデリゲートとキュー。
キューでは、タスクが完了する順序を決定します。 セッションのデリゲートは chaperones 転送プロセスと認証を処理、キャッシュ、およびその他のセッションに関連する問題です。

## <a name="working-with-tasks-and-delegates"></a>タスクとデリゲートを使用します。

バック グラウンド セッションを構成しましたが、これでは、ここの転送を処理するタスクを開始します。 おできますの追跡を使用してこれらのタスクは、`NSUrlSessionDelegate`インスタンスにセッション デリゲートが呼び出されます。 セッションのデリゲートはハンドル認証、エラー、または転送の完了をバック グラウンドで、終了または中断されているアプリケーションのウェイク アップします。

`NSUrlSessionDelegate`転送状態を確認する次の基本的なメソッドを提供します。

-  *DidFinishEventsForBackgroundSession* -すべてのタスクが完了したら、および、転送が完了時にこのメソッドが呼び出されます。
-  *DidReceiveChallenge* : 要求の資格情報の承認が必要な場合に呼び出されます。
-  *DidBecomeInvalidWithError* -呼び出された場合、`NSURLSession`は無効になります。


バック グラウンド セッションでは、実行されているタスクの種類に応じてより専門的なデリゲートが必要です。 バック グラウンド セッションでは、次の 2 つの種類のタスクに限定されます。

-  *タスクをアップロード*-種類のタスク`NSUrlSessionUploadTask`を使用して、`NSUrlSessionTaskDelegate`から継承される`NSUrlSessionDelegate`です。 このデリゲートは、追跡するために追加のメソッドをアップロードの進行状況、ハンドルの HTTP リダイレクトを提供します。
-  *タスクをダウンロード*-種類のタスク`NSUrlSessionDownloadTask`を使用して、`NSUrlSessionDownloadDelegate`から継承される`NSUrlSessionTaskDelegate`です。 このデリゲートは、ダウンロードの進行状況を追跡し、ダウンロード タスクの再開または完了する場合を判断するダウンロードに固有のメソッドと同様に、タスクのすべてのメソッドがアップロードを提供します。


次のコードでは、URL からイメージをダウンロードするために使用するタスクを定義します。 呼び出して、タスクがキックオフお`CreateDownloadTask`バック グラウンド セッション、および URL 要求を渡すことで。

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

次を追跡するすべてのダウンロード タスクこのセッションで新しいセッション ダウンロード デリゲートを作成します。

```csharp
public class MySessionDelegate : NSUrlSessionDownloadDelegate
{
  public override void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
  {
    Console.WriteLine (string.Format ("DownloadTask: {0}  progress: {1}", downloadTask, progress));
    InvokeOnMainThread( () => {
      // update UI with progress bar, if desired
    });
  }
  ...
}
```

オーバーライドできるダウンロード タスクの進行状況を確認したい場合、`DidWriteData`進行状況を追跡するメソッドが UI を更新します。 UI の更新は、アプリケーションがフォア グラウンドでまたはが待機中のユーザーの次回アプリケーションを開く場合、すぐに表示されます。

セッション デリゲート API では、タスクと対話するため、幅広いツールキットを提供します。 セッションの完全な一覧については、メソッドのデリゲートを参照してください、 `NSUrlSessionDelegate` API のドキュメントです。

> [!IMPORTANT]
> すべての呼び出しが UI を更新する必要があります明示的に実行できません、UI スレッドで呼び出すことによって、バック グラウンド スレッドでバック グラウンド セッションが開始`InvokeOnMainThread`iOS アプリを終了してを回避します。 


## <a name="handling-transfer-completion"></a>処理の転送の完了

アプリケーションをセッションに関連付けられているすべてのタスクが完了すると、次のトピックの最後の手順は新しいコンテンツおよび処理します。

*AppDelegate*、サブスクライブ、`HandleEventsForBackgroundUrl`イベント。 アプリケーションがバック グラウンド転送セッションが実行されている場合は、このメソッドが呼び出され、システムに渡します us 完了ハンドラー。

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

完了ハンドラーを使用して iOS アプリケーションが完了すると理解できるように処理します。

セッションが、転送を処理するいくつかのタスクを生成できますに注意してください。 最後のタスクが完了したら、中断または終了したアプリケーションは、バック グラウンドに再公開されました。 次に、アプリケーションに再度接続、`NSURLSession`一意のセッション識別子、および呼び出しを使用して`DidFinishEventsForBackgroundSession`セッション デリゲートでします。 このメソッドは、転送の結果を反映するように、UI の更新を含む、新しいコンテンツを処理するアプリケーションの営業案件を示します。

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

により、新しいコンテンツの処理が完了したら後、システムがアプリケーションのスナップショットを取得し、スリープ状態に戻るには安全では認識できるようにする、完了ハンドラーを呼び出します。

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
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

このチュートリアルでは、iOS 7 でバック グラウンド転送サービスを実装する基本的な手順について説明します。



## <a name="related-links"></a>関連リンク

- [単純なバック グラウンド転送 (サンプル)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
