---
title: "バック グラウンドでのアプリケーションの更新"
ms.topic: article
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: d878f922b74ea3e95fd0e1ebce9e7445063a2946
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="updating-an-application-in-the-background"></a>バック グラウンドでのアプリケーションの更新

バック グラウンド更新は、中断されているアプリケーションのウェイク アップまたはが実行されていないと新しいコンテンツで更新のプロセスです。 iOS は、バック グラウンドでコンテンツを更新するための 3 つのオプションを提供します。

1.  *領域の監視*と*重要な場所の変更サービス*-トリガー バック グラウンドの場所に対応する Api がユーザーの所在地にに基づいて変更を更新します。 これらの Api 使用できます慎重にアプリケーションではない場所ベースの iOS 6、コンテンツを更新するその他のオプションは使用できません。
1.  *フェッチ (iOS 7 以降) のバック グラウンド*-に対するテンポラル アプローチを更新する*重大でない*コンテンツを更新する*頻繁に*です。
1.  *リモート通知 (iOS 7 以降)* -プッシュ通知を受信するアプリケーションでは、通知を使用してコンテンツのバック グラウンド更新を開始することができます。 このメソッドで更新するために使用できます*重要な時間を区別*コンテンツを更新する*散発*です。


次のセクションでは、これらのオプションの基礎を説明します。

## <a name="region-monitoring-and-significant-location-changes"></a>領域の監視との重要な位置の変更

iOS では、機能を backgrounding で 2 つの場所に対応する Api を提供します。

1.  *領域の監視*プロセス境界を持つ領域を設定して、ユーザーか領域を終了すると、デバイスのウェイク アップです。 領域は、循環をさまざまなサイズを指定できます。 ユーザーが、領域境界を超えるデバイスが通知を発生させるか、作業を始める際、通常、イベントを処理するをウェイク アップします。 領域の監視は、GPS を必要とし、バッテリとデータの使用状況を強化します。
1.  *重要な場所の変更サービス*移動体通信無線を使用したデバイスの単純化し、電源節約オプションがあります。 重要な場所の変更をリッスンしているアプリケーションは、デバイスがセル塔を切り替えたときに通知されます。 このサービスは、中断または終了したアプリケーションの起動に使用できるし、バック グラウンドで新しいコンテンツを確認する機会を提供します。 組み合わせて使用しない限り、バック グラウンド アクティビティが約 10 秒間に制限されます、[バック グラウンド タスク](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md)です。


アプリケーションでは、場所は必要はありません`UIBackgroundMode`これらの場所に対応する Api を使用します。 IOS デバイスは、ユーザーの場所の変更によるウェイク状態がときに実行できるタスクの種類を追跡しない、ためこれらの Api は、iOS 6 でバック グラウンドでコンテンツの更新の回避提供します。 *注意してください、場所ベースの Api を使用したバック グラウンド更新をトリガーする、デバイス リソース上に描画されますアプリケーションがその場所へのアクセスを必要とする理由を理解していないユーザーが混乱*です。 バック グラウンド処理は既に場所 Api を使用しているアプリケーションでの領域を監視または場所の大幅な変更を実装する場合は、判断を使用します。

バック グラウンド処理の場所の監視を使用してアプリを公開に問題がある iOS 6: backgrounding オプションは制限されている場合は、アプリケーションのニーズは、バック グラウンドに必要なカテゴリに収まらない、します。 2 つの新しい Api の導入に伴い*バック グラウンドでフェッチ*と*リモート通知*iOS 7 (以降) がより多くのアプリケーションを backgrounding 機会を提供します。 次の 2 つのセクションでは、これらの新しい Api を紹介します。

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>バック グラウンドでフェッチ (iOS 7 以降)

IOS 6、フォア グラウンドを入力するアプリケーションでは新しいコンテンツを簡単に提示してきたユーザーが既に見たようコンテンツの読み込みに時間が必要です。 バック グラウンドでフェッチにより、新しいデータを読み込むアプリケーション*する前に*ユーザーがアプリケーションを起動して、最新のコンテンツをユーザーに提供します。

バック グラウンドでフェッチを実装するのには、編集*Info.plist*を確認し、**バック グラウンド モードを有効にする**と**バック グラウンドでフェッチ**のチェック ボックス。

 [ ![](updating-an-application-in-the-background-images/fetch.png "Info.plist を編集し、バック グラウンド モードを有効にして、バック グラウンドでフェッチ チェック ボックスを確認します。")](updating-an-application-in-the-background-images/fetch.png)

次に、 `AppDelegate`、オーバーライド、`FinishedLaunching`フェッチの最小間隔を設定します。 この例では、多くの場合、新しいコンテンツをフェッチする方法を決定する、OS ができます。

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

最後に、オーバーライドすることで、フェッチを実行、`PerformFetch`メソッドで、`AppDelegate`とを渡して、*完了ハンドラー*です。 受け取るデリゲートは、完了ハンドラーが、 `UIBackgroundFetchResult`:

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

更新コンテンツ完了したら、適切な状態で完了ハンドラーを呼び出すことで、OS できるようにします。 iOS では、完了ハンドラーの状態の 3 つのオプションを提供します。

1.  `UIBackgroundFetchResult.NewData` -新しいコンテンツがフェッチされ、アプリケーションが更新されているときに呼び出されます。
1.  `UIBackgroundFetchResult.NoData` -新しいコンテンツのフェッチが経てが使用できるコンテンツがないときに呼び出されます。
1.  `UIBackgroundFetchResult.Failed` エラー処理のために便利です、このときに呼び出される、フェッチを経由することができませんでした。


バック グラウンドでフェッチを使用してアプリケーションには、バック グラウンドから UI を更新する呼び出しを行うことができます。 ユーザーがアプリを開いたには、UI が最新であり新しいコンテンツの表示になります。 アプリケーションのアプリケーションのスイッチャーのスナップショットは、アプリケーションが新しいコンテンツを持つ場合に、ユーザーが確認できるようにこのも更新されます。

> [!IMPORTANT]
> **注**: 1 回`PerformFetch`が呼び出されると、アプリケーションは、約 30 秒、新しいコンテンツのダウンロードを開始し、完了ハンドラー ブロックを呼び出します。 これは時間がかかりすぎる、アプリが終了されます。 バック グラウンドのフェッチで使用を検討して、_バック グラウンド転送サービス_メディアまたはその他の大きなファイルをダウンロードするときにします。


### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

上記のサンプル コードでお知らせフェッチの最小間隔を設定して新しいコンテンツを取得する頻度を決定する OS`BackgroundFetchIntervalMinimum`です。 iOS では、フェッチ間隔の 3 つのオプションを提供しています。

1.  `BackgroundFetchIntervalNever` -しない新しいコンテンツをフェッチするシステムに通知します。 ときに、ユーザーがサインインしていないなど、特定の状況でフェッチをオフにするのには、これを使用します。 これは、フェッチ間隔の既定値です。 
1.  `BackgroundFetchIntervalMinimum` -できるように多くの場合、フェッチする方法を決定する、システム ユーザーのパターン、バッテリの寿命、データの使用状況、およびその他のアプリケーションのニーズに基づきます。
1.  `BackgroundFetchIntervalCustom` 場合は、アプリケーションのコンテンツが更新を取得する頻度、する間隔を指定できます、「スリープ」すべてフェッチ後にするアプリケーションは実行されません新しいコンテンツをフェッチします。 その間隔は、セットアップが、コンテンツをフェッチするときに、システムによって決定されます。


両方`BackgroundFetchIntervalMinimum`と`BackgroundFetchIntervalCustom`フェッチのスケジュールを設定するシステムに依存します。 この間隔は動的で、個々 のユーザーの傾向と同様に、デバイスのニーズに合わせて調整します。 たとえば、1 人のユーザーがアプリケーション、毎朝をチェックおよび別チェックを 1 時間ごとに、iOS は、コンテンツを確認してくださいが最新の状態両方のユーザーのアプリケーションを開くたびにします。

重大でないコンテンツを持つを頻繁に更新するアプリケーションのバック グラウンドでフェッチを使用してください。 重要な更新プログラムとアプリケーションの場合は、リモートの通知を使用してください。 リモートの通知は、バック グラウンドのフェッチに基づいており、同じの完了ハンドラーを共有します。 次にリモートの通知に掘り下げるします。

 <a name="remote_notifications" />


## <a name="remote-notifications-ios-7-and-greater"></a>リモート通知 (iOS 7 以降)

プッシュ通知は、JSON メッセージ プロバイダーからによってデバイスに送信される、 *Apple Push Notification サービス (APNs)*です。

IOS 6、プッシュ通知の受信は、アプリケーションで何か興味深いものが発生したユーザーに警告する、システムを示しています。 通知をクリックすると、中断されたか、または終了状態から、アプリケーションを取得し、アプリのコンテンツの更新が開始します。 iOS 7 (以降) では、通常のプッシュ通知を拡張アプリケーションにコンテンツをバック グラウンドで更新する機会を提供することにより*する前に*ユーザーがアプリケーションを開くことができ、新しいコンテンツの表示できるように、ユーザーに通知します。すぐにします。

リモートの通知を実装するのには、編集*Info.plist*を確認し、**バック グラウンド モードを有効にする**と**リモート通知**のチェック ボックス。

 [ ![](updating-an-application-in-the-background-images/remote.png "バック グラウンド モードを有効にするバック グラウンド モードおよびリモートの通知を設定")](updating-an-application-in-the-background-images/remote.png)

次に、設定、`content-available`を 1 にプッシュ通知自体のフラグ。 これにより、アプリケーションに通知をアラートを表示する前に新しいコンテンツを取得します。

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

*AppDelegate*、オーバーライド、`DidReceiveRemoteNotification`を利用可能なコンテンツ、通知のペイロードを確認し、適切な完了ハンドラー ブロックを呼び出すメソッド。

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

リモートの通知を使用して、非常に重要アプリケーションの機能には、コンテンツで更新プログラムを不定期に起きる必要があります。 リモートの通知の詳細についてを参照してください、Xamarin [iOS でのプッシュ通知](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)ガイドです。

> [!IMPORTANT]
> **注**: ためリモート通知では、更新機構がバック グラウンドでフェッチに基づいて、または iOS は、アプリケーションの新しいコンテンツのダウンロードを開始し、通知の受信の 30 秒以内に完了ハンドラー ブロックを呼び出す必要がありますアプリケーションを終了します。 リモートの通知とを組み合わせることを検討_バック グラウンド転送サービス_メディアや、バック グラウンドで他の大きなファイルをダウンロードするときにします。


### <a name="silent-remote-notifications"></a>サイレント リモート通知

リモートの通知は、更新プログラムのアプリケーションに通知し、新しいコンテンツは、フェッチを開始する簡単な方法が何らかの変更がユーザーに通知する必要はありません。 たとえば場合は、ユーザーは、同期用にファイルをフラグ、ファイルを更新するたびに通知するために必要はありません。 ファイルの同期ではなくことにより意外のイベントも、ユーザーの早急な措置は必要です。 ユーザーを最新の状態にし、それらを開くと、ファイルだけを期待します。

上記のような場合は、iOS せずに送信するサイレントでは、アラートにプッシュ通知が許可されます。 サイレントに定期的な通知を有効にするには、だけで通知のペイロードからアラートを削除します。

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>速度の制限

開発者の観点からの通常およびサイレント通知の最も大きな違いはあるサイレントのプッシュ率が制限されます。 プッシュ率が高すぎるを取得する場合、APNs はサイレント プッシュのデバイスへの配信を遅延させます。 これは、アプリケーションがサイレント通知が多すぎますにデバイス リソースをドレインしないことを確認します。

ただし、APNs は通知をサイレント、通常リモート通知またはキープ アライブ応答と共に"piggyback"です。 正規の通知は、レート制限ではありません、ために、次の図に示すようは、APNs から、デバイスへのプッシュをストアド サイレント通知を使用できます。

 [ ![](updating-an-application-in-the-background-images/silent.png "この図に示すように、APNs から、デバイスをサイレント ストアドの通知をプッシュする通常の通知を使用できます。")](updating-an-application-in-the-background-images/silent.png)

> [!IMPORTANT]
> **注**: Apple によって、開発者は、アプリケーションを必要として、APNs ことができますが、配信をスケジュールするたびに、サイレントのプッシュ通知を送信します。


このセクションでは、バック グラウンドに必要なカテゴリに適合しないタスクを実行するバック グラウンドでコンテンツを更新する場合、さまざまなオプションをきました。 ここで、これらの Api の動作のいくつかを確認してみましょう。

 [次へ: パート 4 - iOS Backgrounding チュートリアル](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
