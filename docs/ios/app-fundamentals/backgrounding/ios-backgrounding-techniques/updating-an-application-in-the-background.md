---
title: バックグラウンドでの Xamarin iOS アプリの更新
description: このドキュメントでは、リージョンの監視、バックグラウンドフェッチ、リモート通知など、バックグラウンドにある Xamarin iOS アプリを更新するさまざまな方法について説明します。
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 2e0bb4fc0468f938e7a4403513fe101db2282561
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286986"
---
# <a name="updating-a-xamarinios-app-in-the-background"></a>バックグラウンドでの Xamarin iOS アプリの更新

バックグラウンド更新は、中断または実行されていないアプリケーションをスリープ解除し、新しいコンテンツで更新するプロセスです。 iOS には、バックグラウンドでコンテンツを更新するための3つのオプションが用意されています。

1. *リージョンの監視*と*重要な場所の変更サービス*-場所を認識する api は、ユーザーの場所の変更に基づいてバックグラウンド更新をトリガーします。 これらの Api を使用して、場所ベースではない iOS 6 アプリケーションのコンテンツを更新することができます。この場合、他のオプションは使用できません。
1. *バックグラウンドフェッチ (iOS 7 以降)* -*頻繁*に更新される*重要*ではないコンテンツを更新するための一時的なアプローチです。
1. *リモート通知 (iOS 7 以降)* -プッシュ通知を受信するアプリケーションは、通知を使用して、バックグラウンドコンテンツ更新をトリガーできます。 このメソッドを使用すると、*断続的*に更新される*重要な、時間の影響*を受けるコンテンツでを更新できます。


以下のセクションでは、これらのオプションの基本について説明します。

## <a name="region-monitoring-and-significant-location-changes"></a>リージョンの監視と重要な場所の変更

iOS には、バックグラウンド処理機能を備えた2つの場所認識 Api が用意されています。

1. *リージョンの監視*は、境界を持つリージョンを設定し、ユーザーがリージョンを入力または終了したときにデバイスをスリープ解除するプロセスです。 領域は循環しており、さまざまなサイズにすることができます。 ユーザーがリージョンの境界を越えると、デバイスはイベントを処理するためにウェイクアップします。通常は、通知を起動するか、タスクを開始します。 リージョンの監視には GPS が必要であり、バッテリとデータの使用量が増加します。
1. *重要な場所の変更サービス*は、携帯電話機を搭載したデバイスで使用できる、よりシンプルで節電可能なオプションです。 重要な場所の変更をリッスンしているアプリケーションは、デバイスがセル塔を切り替えると通知されます。 このサービスは、中断または終了したアプリケーションをスリープ解除するために使用でき、バックグラウンドで新しいコンテンツを確認する機会を提供します。 バックグラウンド[タスク](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md)とのペアがない限り、バックグラウンドアクティビティは約10秒に制限されます。


アプリケーションでは、これらの場所`UIBackgroundMode`を認識する api を使用する場所は必要ありません。 IOS では、ユーザーの場所での変更によってデバイスがウェイクアップされたときに実行できるタスクの種類は追跡されないため、これらの Api は iOS 6 のバックグラウンドでコンテンツを更新するための回避策を提供します。 *場所ベースの api を使用してバックグラウンド更新をトリガーすると、デバイスリソースが描画され、アプリケーションがその場所にアクセスする必要がある理由を理解していないユーザーを混乱させる可能性があることに注意してください*。 Location Api をまだ使用していないアプリケーションでは、バックグラウンド処理のためにリージョンの監視または重要な場所の変更を実装するときに、裁量を使用します。

バックグラウンド処理の場所の監視を使用するアプリは、iOS 6 の欠陥を公開します。アプリケーションのニーズがバックグラウンドで必要なカテゴリに合わない場合、バックグラウンド処理オプションは限られています。 2つの新しい Api (*バックグラウンドフェッチ*と*リモート通知*) の導入により、iOS 7 (およびそれ以降) は、より多くのアプリケーションにバックグラウンド処理機会を提供します。 次の2つのセクションでは、これらの新しい Api について説明します。

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>バックグラウンドフェッチ (iOS 7 以降)

IOS 6 では、フォアグラウンドに新しいコンテンツを読み込むために必要な時間を短縮し、既に見たコンテンツをユーザーに簡単に提示します。 バックグラウンドフェッチでは、ユーザーがアプリケーションを起動*する前*に、アプリケーションが新しいデータを読み込んで、最新のコンテンツをユーザーに提供できます。

バックグラウンドフェッチを実装するには、[*情報] plist*を編集し、[バックグラウンドモードおよび**バックグラウンドフェッチ** **を有効にする**] チェックボックスをオンにします。

 [![](updating-an-application-in-the-background-images/fetch.png "情報を編集し、[バックグラウンドモードとバックグラウンドフェッチを有効にする] チェックボックスをオンにします。")](updating-an-application-in-the-background-images/fetch.png#lightbox)

次に、で`AppDelegate`、 `FinishedLaunching`メソッドをオーバーライドして最小フェッチ間隔を設定します。 この例では、OS が新しいコンテンツを取得する頻度を決定します。

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

最後に、の`PerformFetch` `AppDelegate`メソッドをオーバーライドし、*完了ハンドラー*を渡すことによって、フェッチを実行します。 完了ハンドラーは、次の`UIBackgroundFetchResult`ものを受け取るデリゲートです。

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

コンテンツの更新が完了したら、適切な状態で完了ハンドラーを呼び出すことによって、OS に認識させます。 iOS には、完了ハンドラーの状態に関する3つのオプションが用意されています。

1. `UIBackgroundFetchResult.NewData`-新しいコンテンツがフェッチされ、アプリケーションが更新されたときに呼び出されます。
1. `UIBackgroundFetchResult.NoData`-新しいコンテンツのフェッチが行われたときに、コンテンツが使用できない場合に呼び出されます。
1. `UIBackgroundFetchResult.Failed`-エラー処理に役立ちます。これは、フェッチが失敗したときに呼び出されます。


バックグラウンドフェッチを使用するアプリケーションは、呼び出しを実行して UI をバックグラウンドから更新できます。 ユーザーがアプリを開くと、UI が最新の状態になり、新しいコンテンツが表示されます。 これにより、アプリケーションのアプリスイッチャースナップショットも更新されるため、ユーザーはアプリケーションに新しいコンテンツがあることを確認できます。

> [!IMPORTANT]
> `PerformFetch`が呼び出されると、アプリケーションは、新しいコンテンツのダウンロードを開始するまで約30秒かかり、完了ハンドラーブロックを呼び出します。 この処理に時間がかかりすぎると、アプリは終了します。 メディアまたはその他の大きなファイルをダウンロードする場合は、バックグラウンド_転送サービス_でバックグラウンドフェッチを使用することを検討してください。


### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

上記のサンプルコードでは、最小フェッチ間隔をに`BackgroundFetchIntervalMinimum`設定して、新しいコンテンツを取得する頻度を OS に決定します。 iOS には、次の3つのフェッチ間隔オプションが用意されています。

1. `BackgroundFetchIntervalNever`-新しいコンテンツがフェッチされないようにシステムに指示します。 ユーザーがサインインしていない場合など、特定の状況でのフェッチをオフにするには、このオプションを使用します。 これは、フェッチ間隔の既定値です。 
1. `BackgroundFetchIntervalMinimum`-システムで、ユーザーパターン、バッテリ寿命、データ使用量、および他のアプリケーションのニーズに基づいてフェッチする頻度を決定します。
1. `BackgroundFetchIntervalCustom`-アプリケーションのコンテンツが更新される頻度がわかっている場合は、すべてのフェッチの後に "スリープ" 間隔を指定できます。その間、アプリケーションは、新しいコンテンツをフェッチできなくなります。 この間隔が経過すると、コンテンツをいつフェッチするかがシステムによって決定されます。


`BackgroundFetchIntervalMinimum` と`BackgroundFetchIntervalCustom`はどちらもシステムを利用してフェッチをスケジュールします。 この間隔は動的であるため、デバイスのニーズに合わせて、個々のユーザーの習慣に適合させることができます。 たとえば、あるユーザーがアプリケーションを毎朝チェックし、1時間ごとに別のチェックを行う場合、iOS は、アプリケーションを開くたびに、両方のユーザーについてコンテンツが最新の状態であることを確認します。

バックグラウンドフェッチは、重要でないコンテンツを頻繁に更新するアプリケーションで使用する必要があります。 重要な更新プログラムが適用されているアプリケーションでは、リモート通知を使用する必要があります。 リモート通知は、バックグラウンドフェッチに基づいており、同じ完了ハンドラーを共有します。 次に、リモート通知について説明します。

 <a name="remote_notifications" />


## <a name="remote-notifications-ios-7-and-greater"></a>リモート通知 (iOS 7 以降)

プッシュ通知は、 *Apple Push Notification service (APNs)* を介してプロバイダーからデバイスに送信される JSON メッセージです。

IOS 6 では、受信プッシュ通知は、アプリケーションで問題が発生したことをユーザーに通知するようにシステムに指示します。 通知をクリックすると、中断状態または終了状態からアプリケーションが引き出され、アプリはコンテンツの更新を開始します。 iOS 7 (およびそれ以降) は、ユーザーに通知*する前に*バックグラウンドでコンテンツを更新する機会をアプリケーションに提供することで、通常のプッシュ通知を拡張します。これにより、ユーザーがアプリケーションを開いて新しいコンテンツをすぐに表示できるようになります。

リモート通知を実装するには、[*情報] plist*を編集し、[バックグラウンドモードと**リモート通知** **を有効にする**] チェックボックスをオンにします。

 [![](updating-an-application-in-the-background-images/remote.png "バックグラウンドモードとリモート通知を有効にするために設定されたバックグラウンドモード")](updating-an-application-in-the-background-images/remote.png#lightbox)

次に、プッシュ`content-available`通知自体のフラグを1に設定します。 これにより、アプリケーションは、アラートを表示する前に新しいコンテンツを取得することができます。

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

*Appdelegate*で、 `DidReceiveRemoteNotification`メソッドをオーバーライドして、利用可能なコンテンツの通知ペイロードを確認し、適切な完了ハンドラーブロックを呼び出します。

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

リモート通知は、アプリケーションの機能にとって重要なコンテンツを更新する頻度が低い場合に使用する必要があります。 リモート通知の詳細については、「 [iOS 用 Xamarin プッシュ通知](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)」ガイドを参照してください。

> [!IMPORTANT]
> リモート通知の更新メカニズムはバックグラウンドフェッチに基づいているため、アプリケーションは、新しいコンテンツのダウンロードを開始し、通知を受信してから30秒以内に完了ハンドラーブロックを呼び出す必要があります。これを行わないと、iOS はアプリケーションを終了します。 バックグラウンドでメディアまたはその他の大きなファイルをダウンロードする場合は、リモート通知を_バックグラウンド転送サービス_とペアリングすることを検討してください。


### <a name="silent-remote-notifications"></a>サイレントリモート通知

リモート通知は、更新プログラムをアプリケーションに通知し、新しいコンテンツのフェッチを開始する簡単な方法ですが、何かが変更されたことをユーザーに通知する必要がない場合もあります。 たとえば、ユーザーが同期のためにファイルにフラグを付ける場合、ファイルが更新されるたびに通知する必要はありません。 ファイルの同期は驚くほどのイベントではなく、ユーザーがすぐに対処する必要もありません。 ユーザーは、ファイルを開いたときに最新の状態になることを期待しています。

上記のような場合、iOS では、プッシュ通知を警告なしで自動的に送信することができます。 通常の通知をサイレントモードにするには、単に通知ペイロードからアラートを削除します。

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>速度の制限

開発者の観点からの通常の通知とサイレント通知の最大の違いは、サイレントプッシュのレートが制限されることです。 プッシュ率が高すぎると、APNs はデバイスへのサイレントプッシュの配信を遅らせます。 これは、アプリケーションがサイレント通知を多数含むデバイスリソースをドレインしないようにするためです。

ただし、APNs は、通常のリモート通知またはキープアライブ応答と共にサイレント通知 "便乗" を使用します。 次の図に示すように、通常の通知はレート制限されていないため、APNs からデバイスに保存されたサイレント通知をプッシュするために使用できます。

 [![](updating-an-application-in-the-background-images/silent.png "次の図に示すように、通常の通知を使用すると、保存されているサイレント通知を APNs からデバイスにプッシュできます。")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> Apple では、アプリケーションが必要とするたびにサイレントプッシュ通知を送信することを開発者に促し、APNs が配信をスケジュールできるようにします。


このセクションでは、バックグラウンドでコンテンツを更新して、バックグラウンドで必要なタスクを実行するためのさまざまなオプションについて説明しました。 では、これらの Api のいくつかを実際に見てみましょう。

 [次へ: パート 4-iOS バックグラウンド処理のチュートリアル](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
