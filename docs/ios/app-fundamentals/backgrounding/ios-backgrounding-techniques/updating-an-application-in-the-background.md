---
title: バック グラウンドで Xamarin.iOS アプリの更新
description: このドキュメントでは、リージョンの監視、バック グラウンド フェッチすると、リモート通知など、バック グラウンドでは、Xamarin.iOS アプリを更新するさまざまな方法について説明します。
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 835dccaea79467582f56fd4b8b6b3b8f42acd632
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103232"
---
# <a name="updating-a-xamarinios-app-in-the-background"></a>バック グラウンドで Xamarin.iOS アプリの更新

バック グラウンド更新は、中断されているアプリケーションをスリープ解除またはが実行されていないと、新しいコンテンツに更新のプロセスです。 iOS では、バック グラウンドでコンテンツを更新するための 3 つのオプションが用意されています。

1.  *領域の監視*と*重要な場所の変更サービス*-位置認識 Api のトリガーのバック グラウンド更新で、ユーザーの位置に基づいて変更します。 これらの Api で使える裁量、場所ベースの iOS 6 アプリケーションでコンテンツを更新するその他のオプションは使用できません。
1.  *フェッチ (iOS 7 以降) をバック グラウンド*-テンポラル アプローチを更新する*重大でない*コンテンツを更新する*頻繁に*します。
1.  *リモート通知 (iOS 7 以降)* -プッシュ通知を受信するアプリケーションは、通知を使用してコンテンツのバック グラウンド更新をトリガーすることができます。 このメソッドを使用してで更新すること*重要な時間を区別する*更新コンテンツ*散発的*します。


次のセクションでは、これらのオプションの基本について説明します。

## <a name="region-monitoring-and-significant-location-changes"></a>領域の監視と重要な場所の変更

iOS では、2 つの位置認識 Api 機能をバック グラウンド処理を提供します。

1.  *領域の監視*の境界を持つ領域を設定し、ユーザーか領域を終了すると、デバイスの起動時のプロセスです。 リージョンは循環型と、さまざまなサイズであることができます。 ユーザーが、領域境界を超えたときに、デバイスが通知を発生させる、またはタスクを開始して、通常は、イベントを処理するでウェイク アップします。 領域の監視は、GPS を必要とし、バッテリとデータの使用状況を高めます。
1.  *重要な場所の変更サービス*移動体通信無線を使用したデバイスの単純化し、電源節約オプションがあります。 重要な場所の変更をリッスンしているアプリケーションは、デバイスは、セルの塔を切り替えたときに通知されます。 このサービスは、中断または終了したアプリケーションをウェイクするために使用でき、新しいコンテンツをバック グラウンドをチェックする機会を提供します。 ペアになっている場合を除き、バック グラウンド アクティビティが約 10 秒に制限されていますが、[バック グラウンド タスク](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md)します。


アプリケーションで、場所が必要ありません`UIBackgroundMode`これら位置認識 Api を使用します。 IOS では、デバイスは、ユーザーの場所の変更内容によってウェイクときに実行できるタスクの種類を追跡しない、ため、これらの Api は、iOS 6 でバック グラウンドでコンテンツの更新の回避提供します。 *注意してください、場所ベースの Api を使用したバック グラウンド更新をトリガーするデバイスのリソース上に描画されますが、アプリケーションがその場所へのアクセスに必要な理由を理解していないユーザーを混乱*します。 バック グラウンド処理でまだロケーション Api を使用していないアプリケーションの領域の監視または場所の大幅な変更を実装する場合は、判断を使用します。

IOS 6 でこの問題を公開する場所がバック グラウンド処理の監視を使用してアプリ: バック グラウンド処理のオプションは制限されている場合、バック グラウンドに必要なカテゴリにアプリケーションのニーズに合わない、します。 2 つの新しい Api の導入に伴い*バック グラウンド フェッチ*と*リモート通知*iOS 7 (以降) がより多くのアプリケーションをバック グラウンド処理の機会を提供します。 次の 2 つのセクションでは、これらの新しい Api を紹介します。

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>バック グラウンド フェッチ (iOS 7 以降)

IOS 6、フォア グラウンドを入力するアプリケーションは必要について簡単にユーザーを表示するが、既に説明したコンテンツを含む、新しいコンテンツの読み込みに時間です。 バック グラウンド フェッチにより、新しいデータを読み込むアプリケーション*する前に*ユーザーがアプリケーションを起動して、最新のコンテンツをユーザーに提供します。

バック グラウンドでフェッチを実装するには編集*Info.plist*を確認し、**バック グラウンド モードを有効にする**と**に Background Fetch**チェック ボックス。

 [![](updating-an-application-in-the-background-images/fetch.png "Info.plist を編集し、バック グラウンド モードを有効にして、バック グラウンド フェッチ チェック ボックスを確認します。")](updating-an-application-in-the-background-images/fetch.png#lightbox)

次に、 `AppDelegate`、オーバーライド、`FinishedLaunching`フェッチの最小間隔を設定します。 この例では、多くの場合、新しいコンテンツをフェッチする方法を決定する OS お知らせ。

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

最後に、オーバーライドすることで、フェッチを実行、`PerformFetch`メソッドで、`AppDelegate`を渡して、*完了ハンドラー*します。 完了ハンドラーはデリゲートを受け取る、 `UIBackgroundFetchResult`:

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

コンテンツの更新が終わって、適切な状態で完了ハンドラーを呼び出している OS を使用できます。 iOS では、完了ハンドラーの状態の 3 つのオプションが用意されています。

1.  `UIBackgroundFetchResult.NewData` -新しいコンテンツがフェッチされ、アプリケーションが更新されているときに呼び出されます。
1.  `UIBackgroundFetchResult.NoData` -新しいコンテンツのフェッチは、問題が発生しましたが、使用可能なコンテンツがないときに呼び出されます。
1.  `UIBackgroundFetchResult.Failed` エラー処理に便利です、これが呼び出されます、フェッチを経由することができませんでした。


バック グラウンド フェッチを使用してアプリケーションには、バック グラウンドから UI を更新する呼び出しを行うことができます。 ユーザーがアプリを開くときに、UI は日付と新しいコンテンツを表示するまでになります。 アプリケーションのスイッチャーのアプリのスナップショットは、アプリケーションが新しいコンテンツを持っている場合、ユーザーが確認できるようにこれも更新されます。

> [!IMPORTANT]
> 1 回`PerformFetch`が呼び出されると、アプリケーションは、約 30 秒間、新しいコンテンツのダウンロードを開始し、完了ハンドラー ブロックを呼び出します。 この時間がかかりすぎる場合は、アプリが終了します。 バック グラウンドのフェッチで使用を検討して、_バック グラウンド転送サービス_メディアまたはその他の大きなファイルをダウンロードするときにします。


### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

上記のサンプル コードで、OS フェッチの最小間隔を設定して新しいコンテンツをフェッチする頻度を決定できるように`BackgroundFetchIntervalMinimum`します。 iOS では、フェッチ間隔の 3 つのオプションが用意されています。

1.  `BackgroundFetchIntervalNever` -しない新しいコンテンツをフェッチするシステムに指示します。 これを使用して、フェッチなど、ユーザーが署名されていない場合、特定の状況でオフにします。 これは、既定値は、フェッチ間隔のです。 
1.  `BackgroundFetchIntervalMinimum` -できるように多くの場合、フェッチする方法を決定する、システム ユーザーのパターン、バッテリの寿命、データの使用状況、およびその他のアプリケーションのニーズに基づいて。
1.  `BackgroundFetchIntervalCustom` -アプリケーションのコンテンツが更新される頻度がわかっている場合を指定できます「スリープ」間隔フェッチごとに後から新しいコンテンツをフェッチしています、アプリケーションを禁止します。 その間隔が、コンテンツをフェッチするときに、システムによって決定されます。


両方`BackgroundFetchIntervalMinimum`と`BackgroundFetchIntervalCustom`フェッチをスケジュールするシステムに依存します。 この間隔は動的で、個々 のユーザーの習慣と同様に、デバイスのニーズに合わせて調整します。 たとえば、1 人のユーザーは、毎朝、アプリケーションをチェックし、もう 1 つチェック 1 時間ごとに、iOS がコンテンツを確認しますが両方のユーザーの最新の状態、アプリケーションを開くたびに

重大でないコンテンツを頻繁に更新するアプリケーションに background Fetch を使用してください。 重要な更新プログラムとアプリケーションでは、リモート通知を使用する必要があります。 リモート通知は、バック グラウンド フェッチ、基づいており、同じ完了ハンドラーを共有します。 次に、リモート通知について説明します。

 <a name="remote_notifications" />


## <a name="remote-notifications-ios-7-and-greater"></a>リモート通知 (iOS 7 以降)

プッシュ通知は、プロバイダーからのデバイスに送信される JSON メッセージ、 *Apple Push Notification service (APNs)* します。

IOS 6、プッシュ通知の受信アプリケーションで興味深いの出来事が発生したユーザーに警告する、システムに指示します。 中断または終了の状態からアプリケーションを取得する通知をクリックし、アプリはコンテンツの更新を開始します。 iOS 7 (以降) は通常のプッシュ通知をアプリケーションに、バック グラウンドでコンテンツを更新する機会を提供することにより拡張*する前に*ユーザーに通知するようにユーザーがアプリケーションを開くことができ、新しいコンテンツの表示すぐにします。

リモート通知を実装するには編集*Info.plist*を確認し、**バック グラウンド モードを有効にする**と**リモート通知**チェック ボックス。

 [![](updating-an-application-in-the-background-images/remote.png "バック グラウンド モードを有効にするバック グラウンド モードとリモート通知に設定")](updating-an-application-in-the-background-images/remote.png#lightbox)

次に、設定、`content-available`を 1 に、プッシュ通知自体のフラグ。 これにより、アプリケーションは、アラートを表示する前に新しいコンテンツをフェッチします。

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

*AppDelegate*、オーバーライド、`DidReceiveRemoteNotification`の利用可能なコンテンツは、通知ペイロードを確認し、適切な完了ハンドラー ブロックを呼び出すメソッド。

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

リモート通知は、アプリケーションの機能するために重要であるコンテンツで頻度の低い更新プログラムとして使用する必要があります。 リモート通知の詳細については、Xamarin を参照してください。 [ios Push Notifications](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)ガイド。

> [!IMPORTANT]
> リモート通知の更新メカニズムは、バック グラウンド フェッチに基づいているため、アプリケーションの新しいコンテンツのダウンロードを開始し、通知の受信の 30 秒以内に、完了ハンドラー ブロックを呼び出す必要があります。 または iOS には、アプリケーションが終了します。 使用したリモート通知を組み合わせることを検討_バック グラウンド転送サービス_メディアまたはバック グラウンドで他の大きなファイルをダウンロードするときにします。


### <a name="silent-remote-notifications"></a>サイレント リモート通知

リモート通知は更新プログラムのアプリケーションに通知し、新しいコンテンツをフェッチを開始する簡単な方法が、何らかの変更がユーザーに通知する必要がある場合があります。 たとえば、ユーザーは、同期中のファイルをフラグ、ファイルを更新するたびに通知するために私たちが不要です。 ファイルの同期は驚くほどイベントではありません。 また、ユーザーの早急な措置は必要。 ユーザーには、状を開くときに最新であるファイルが予想されるだけです。

上記のような場合、iOS でプッシュ通知をアラートなし - 自動的には、送信はできます。 サイレントに定期的な通知を有効にするには、通知ペイロードからアラートを削除だけです。

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>速度の制限

開発者の観点から通知をサイレント モードでの通常の最も大きな違いは、サイレント プッシュ数が制限されています。 プッシュ率が高すぎる場合、APNs は、デバイスへのサイレント プッシュの配信を遅延させます。 これは、アプリケーションがサイレント通知が多すぎると、デバイス リソースをドレインしないようにします。

ただし、APNs では、サイレント通知"piggyback"と共に、リモート正常な通知または応答のキープ アライブをことができます。 定期的な通知は、レート制限ではありません、ために、次の図に示すようは APNs からデバイスへのプッシュをストアドのサイレント通知に使用できます。

 [![](updating-an-application-in-the-background-images/silent.png "この図に示すように、ストアド サイレント通知を APNs から、デバイスにプッシュする定期的な通知を使用できます。")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> Apple では、開発者は、アプリケーションに必要な場合に、サイレント プッシュ通知を送信して、APNs、配信をスケジュールできるようにします。


このセクションでバック グラウンドに必要なカテゴリに適合しないタスクを実行するバック グラウンドでコンテンツを更新するためのさまざまなオプションを取り上げました。 次に、これらの Api の動作の一部を見てみましょう。

 [次: パート 4 - iOS バック グラウンド処理のチュートリアル](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
