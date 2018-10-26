---
title: バック グラウンド転送サービスと NSURLSession Xamarin.iOS で
description: このドキュメントでは、バック グラウンド転送サービスと NSUrlSession を使用して、大きなイメージのダウンロードを開始し、バック グラウンドでアプリを配置すると、そのダウンロードを続行する方法を説明するチュートリアルを提供します。
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4e525388290d92901e68e61f1ffa81866f5aac4d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114237"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>バック グラウンド転送サービスと NSURLSession Xamarin.iOS で

バック グラウンド転送は背景を構成することによって開始された`NSURLSession`とアップロードまたはダウンロード タスクをエンキューします。 IOS アプリケーションの完了ハンドラーを呼び出すことによってアプリケーションに通知される場合は、アプリケーションが backgrounded、中断、または終了したときに、タスクが完了、 *AppDelegate*します。 次の図の動作例を示します。

 [![](background-transfer-walkthrough-images/transfer.png "バック グラウンド NSURLSession を構成することでバック グラウンド転送が開始され、エンキューがアップロードまたはダウンロード タスク")](background-transfer-walkthrough-images/transfer.png#lightbox)

これでどのようにコードを見てみましょう。

## <a name="configuring-a-background-session"></a>バック グラウンド セッションを構成します。

バック グラウンド セッションをするためには、作成、新しい`NSUrlSession`を使用して構成し、`NSUrlSessionConfiguration`オブジェクト。

構成オブジェクトがセッションで実行できる内容を決定し、実行できるタスクの種類。
セッション構成を使用して、`CreateBackgroundSessionConfiguration`メソッドは別のプロセスで実行され、データ、およびバッテリ寿命を保持するために任意の (WiFi) 転送を実行します。
次のコード サンプルでは使用してバック グラウンド転送セッションの適切なセットアップ、`CreateBackgroundSessionConfiguration`メソッドと一意の文字列識別子。

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

構成オブジェクトとは別のセッションも必要ですセッション デリゲートとキュー。
キューは、タスクが完了する順序を決定します。 セッションのデリゲートは chaperones、転送プロセスと認証を処理、キャッシュ、およびその他のセッションに関連する問題です。

## <a name="working-with-tasks-and-delegates"></a>タスクとデリゲートの使用

バック グラウンド セッションを構成しましたが、これでは、みましょうの転送を処理するタスクを開始します。 私たちの追跡できるこれらのタスクを使用して、`NSUrlSessionDelegate`と呼ばれるセッション デリゲートのインスタンス。 セッションのデリゲートはハンドルの認証、エラー、または転送の完了をバック グラウンドで、終了または中断したアプリケーションをウェイク アップします。

`NSUrlSessionDelegate`転送状態を確認する次の基本的なメソッドを提供します。

-  *DidFinishEventsForBackgroundSession* -すべてのタスクが完了したら、および転送が完了するときにこのメソッドが呼び出されます。
-  *DidReceiveChallenge* : 要求の資格情報の承認が必要な場合に呼び出されます。
-  *DidBecomeInvalidWithError* -呼び出された場合、`NSURLSession`は無効になります。


バック グラウンド セッションより特化されたデリゲートを実行しているタスクの種類に応じて必要があります。 バック グラウンド セッションでは、2 つの種類のタスクに限定されます。

-  *タスクのアップロード*-種類のタスク`NSUrlSessionUploadTask`を使用して、`NSUrlSessionTaskDelegate`から継承される`NSUrlSessionDelegate`します。 このデリゲートは、追跡するために追加のメソッドをアップロードの進行状況や、HTTP リダイレクトのハンドルを提供します。
-  *タスクをダウンロード*の種類のタスク`NSUrlSessionDownloadTask`を使用して、`NSUrlSessionDownloadDelegate`から継承される`NSUrlSessionTaskDelegate`します。 このデリゲートは、すべてのメソッドをアップロード、ダウンロードの進行状況の追跡を確認してダウンロード タスクが再開または完了したときにダウンロード固有のメソッドと同様に、タスクを提供します。


次のコードでは、URL からイメージをダウンロードするために使用するタスクを定義します。 呼び出すことによって、タスクを開始します`CreateDownloadTask`バック グラウンド セッションでは、URL 要求を渡すことで。

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

次に、すべてのダウンロード タスクでは、このセッションの追跡する新しいセッション ダウンロード デリゲートを作成します。

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

無効にできるダウンロード タスクの進行状況を確認したい場合、`DidWriteData`進行状況を追跡するメソッドが UI を更新します。 UI の更新は、アプリケーションがフォア グラウンドでまたはが待機中のユーザーの次回がアプリケーションを開いた場合、すぐに表示されます。

セッションのデリゲートの API は、タスクと対話するため、広範なツールキットを提供します。 セッションの完全な一覧については、メソッドのデリゲートを参照してください、 `NSUrlSessionDelegate` API のドキュメント。

> [!IMPORTANT]
> 呼び出しが UI を更新する必要があります明示的に実行できません、UI スレッドで呼び出すことによって、バック グラウンド スレッド上でバック グラウンド セッションを起動`InvokeOnMainThread`iOS アプリの終了を回避するためにします。 


## <a name="handling-transfer-completion"></a>処理の転送の完了

最後の手順は、アプリケーションに、セッションに関連付けられているすべてのタスクが完了すると、通知させることです。 新しいコンテンツおよび処理します。

*AppDelegate*、購読、`HandleEventsForBackgroundUrl`イベント。 アプリケーションがバック グラウンドを入力して、転送セッションが実行されている、ときに、このメソッドが呼び出され、システムは私たちに完了ハンドラーを渡します。

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

完了ハンドラーを使用して iOS アプリケーションが終了すると理解できるように処理します。

セッションが、転送を処理するいくつかのタスクを生成できることを思い出してください。 最後のタスクが完了したら、中断または終了したアプリケーションがバック グラウンドに再度起動します。 をアプリケーションに再度接続、`NSURLSession`一意のセッション識別子、および呼び出しを使用して`DidFinishEventsForBackgroundSession`セッション デリゲートにします。 このメソッドは、転送の結果を反映するように、UI の更新を含む、新しいコンテンツを処理するために、アプリケーションの機会です。

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

新しいコンテンツの処理が完了すると、させるをシステムに通知をアプリケーションのスナップショットを取得し、スリープ状態に戻るには、安全では、完了ハンドラーを呼び出します。

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
