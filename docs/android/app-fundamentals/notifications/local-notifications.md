---
title: ローカル通知
description: このセクションでは、Xamarin.Android でローカル通知を実装する方法を示します。 Android の通知のさまざまな UI 要素について説明し、API について説明しますが、作成して、通知を表示する関連の。
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 221fa9b70eeba2c4ca08433c627e5648470a7fac
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514532"
---
# <a name="local-notifications"></a>ローカル通知

_このセクションでは、Xamarin.Android でローカル通知を実装する方法を示します。Android の通知のさまざまな UI 要素について説明し、API について説明しますが、作成して、通知を表示する関連の。_

## <a name="local-notifications-overview"></a>ローカル通知の概要

このトピックでは、Xamarin.Android アプリケーションでローカル通知を実装する方法について説明します。 Android の通知のさまざまな部分について説明します、アプリの開発者に提供されるさまざまな通知スタイルについて説明し、作成および公開通知に使用される Api の一部が導入されています。

Android は、ユーザーに通知アイコンと通知情報を表示するための 2 つのシステム管理の対象領域を提供します。 そのアイコンが表示されます、通知が最初に発行されたときに、*通知領域*の次のスクリーン ショットに示しますように。

![デバイスで通知領域の例](local-notifications-images/01-notification-shade.png)

ユーザー通知の詳細を取得する (通知の内容を表示するには、各通知アイコンを展開) される通知ドロアーを開くことができ、通知に関連付けられているすべてのアクションを実行します。 次のスクリーン ショットは、*通知ドロアー*上に表示される通知領域に対応します。

[![例通知ドロアーを 3 つの通知を表示します。](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Android の通知は、2 つの種類のレイアウトを使用します。

-   ***基本のレイアウト*** &ndash; compact、フィックス プレゼンテーション形式。

-   ***展開されたレイアウト***&ndash;詳細情報を表示するサイズを大きくする拡張可能な表示形式。

各タイプのレイアウト (とその作成方法) については、次のセクションで説明します。


### <a name="base-layout"></a>基本のレイアウト

すべての Android の通知は、基本のレイアウト形式には、少なくとも次の要素が含まれています。 これに基づいて構築されます。

1.  A*通知アイコン*アプリは、さまざまな種類の通知をサポートしている場合、元のアプリまたは通知の種類を表します。

2.  通知*タイトル*、または通知は個人のメッセージの場合は、送信者の名前。

3.  通知メッセージです。

4.  A*タイムスタンプ*します。

次の図に示すように、これらの要素が表示されます。

[![通知要素の場所](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

基本のレイアウトは、高さ 64 の密度に依存しないピクセル (dp) に制限されます。 Android は、既定では、この基本的な通知のスタイルを作成します。

必要に応じて、通知は、アプリケーションまたは送信者の写真を表す大規模なアイコンを表示できます。 Android 5.0 以降では、通知で大きいアイコンを使用する小規模の通知アイコンが大きなアイコンの上、バッジとして表示されます。

![単純な通知の写真](local-notifications-images/04-simple-notification-photo.png)

Android 5.0 以降では、通知もに表示できる、ロック画面。

[![ロック画面通知の例](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

ユーザーはダブルタップして、ロック画面通知デバイスのロックを解除し、その通知が送信されたアプリに移動またはスワイプして通知を破棄します。 アプリは、ロック画面に表示される内容を制御するための通知の可視性のレベルを設定でき、ユーザーがロック画面通知に表示される機密性の高いコンテンツを許可するかどうかを選択します。

Android 5.0 と呼ばれる優先度の高い通知の表示形式を導入する*ヘッドアップ*します。 ヘッドアップ通知は、数秒を画面の一番上から下へスライドし、漂い、通知領域へのバックアップ。

[![例のヘッドアップ通知](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

ヘッドアップ通知では、システムの UI を現在実行中のアクティビティの状態を中断することがなく、ユーザーの前に重要な情報を配置できます。

Android には、通知を並べ替えるし、適切に表示されるように、通知のメタデータのサポートが含まれます。 通知のメタデータは、ヘッドアップ形式と、ロック画面で通知を表示する方法も制御します。 アプリケーションでは、次の種類の通知のメタデータを設定できます。

-   **優先順位**&ndash;優先度レベルは、通知が表示される方法とタイミングを決定します。 たとえば、Android 5.0 では、優先度の高い通知はヘッドアップ通知として表示されます。

-   **可視性**&ndash;通知コンテンツの量が、ロック画面に通知が表示されたときに表示されることを指定します。

-   **カテゴリ**&ndash;でデバイスの場合など、さまざまな状況で通知を処理する方法をシステムに通知*不可*モード。

**注:** **可視性**と**カテゴリ**Android 5.0 およびそれ以前のバージョンの Android では使用できないで導入されました。 Android 8.0 以降[通知チャネル](#notif-chan)をユーザーに通知を表示する方法を制御するために使用します。


### <a name="expanded-layouts"></a>展開されたレイアウト

Android 4.1 以降では、通知は、ユーザーがより多くのコンテンツを表示する通知の高さを展開できるように拡張されたレイアウト スタイルを使用して構成できます。 たとえば、次の例は、contracted モードで展開されたレイアウト通知を示しています。

![Contracted の通知](local-notifications-images/07-contracted-notification.png)

この通知が展開されている場合は、メッセージ全体を表示します。

![展開された通知](local-notifications-images/08-expanded-notification.png)

Android では、1 つのイベント通知の 3 つの展開のレイアウト スタイルをサポートしています。

-   ***大きなテキスト*** &ndash; contracted モードは、2 つのピリオドが続くメッセージの最初の行の抜粋が表示されます。 拡張モードでは、(上記の例に示すように)、メッセージ全体を表示します。

-   ***受信トレイ*** &ndash; contracted のモードでは、新しいメッセージの数を表示します。 展開モードでは、最初の電子メール メッセージや受信トレイにメッセージの一覧が表示されます。

-   ***イメージ*** &ndash; contracted モードは、メッセージのテキストのみが表示されます。 展開されたモードで、テキストとイメージを表示します。

[基本的な通知を超える](#beyond-the-basic-notification)を作成する方法について説明します (この記事で後述)*大きなテキスト*、*受信トレイ*、および*イメージ*通知します。


## <a name="notification-creation"></a>通知の作成

使用する Android の通知を作成するには[Notification.Builder](https://developer.xamarin.com/api/type/Android.App.Notification+Builder/)クラス。 `Notification.Builder` 通知オブジェクトの作成を簡略化 Android 3.0 で導入されました。 Android の以前のバージョンと互換性がある通知を作成するには、使用することができます、 [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)クラスの代わりに`Notification.Builder`(を参照してください[互換性](#compatibility)は、このトピックの後半詳細については`NotificationCompat.Builder`)。
`Notification.Builder` など、通知では、さまざまなオプションを設定するための方法があります。

-   タイトル、メッセージ テキスト、および通知アイコンを含むコンテンツ。

-   通知のスタイルなど*大きなテキスト*、*受信トレイ*、または*イメージ*スタイル。

-   通知の優先度: 少なくとも、低、既定、高、または最大値。

-   ロック画面の通知の可視性: パブリック、プライベート、またはシークレット。

-   Android は、カテゴリのメタデータは、分類し、通知をフィルターします。

-   通知がタップされたときに起動するアクティビティを示す省略可能なものです。

ビルダーでこれらのオプションを設定した後は、設定を含む通知オブジェクトを生成します。 通知を発行するには、この通知オブジェクトを渡す、*通知マネージャー*します。 Android に用意されて、 [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)クラスは、通知を発行して、ユーザーに表示することを担当します。 このクラスへの参照は、アクティビティやサービスなどの任意のコンテキストから取得できます。


### <a name="how-to-generate-a-notification"></a>通知を生成する方法

Android で通知を生成するには、次の手順を実行します。

1.  インスタンスを作成、`Notification.Builder`オブジェクト。

2.  さまざまなメソッドを呼び出して、`Notification.Builder`通知オプションを設定するオブジェクト。

3.  呼び出す、[ビルド](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/)のメソッド、`Notification.Builder`通知オブジェクトをインスタンス化するオブジェクト。

4.  呼び出す、[通知](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification))メソッドの通知マネージャーに通知を発行します。

通知ごとに次の情報を少なくとも指定する必要があります。

-   小さいアイコン (24 x 24 サイズ dp)

-   短いタイトル

-   通知のテキスト

次のコード例は、使用する方法を示しています。`Notification.Builder`基本的な通知を生成します。 注意して`Notification.Builder`メソッドをサポート[メソッド チェーン](http://en.wikipedia.org/wiki/Method_chaining); は、各オブジェクトを返します、ビルダーを呼び出す次のメソッド呼び出しの最後のメソッド呼び出しの結果を使用できるようにします。

```csharp
// Instantiate the builder and set notification elements:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

この例で、新しい`Notification.Builder`と呼ばれるオブジェクト`builder`がインスタンス化すると、タイトルと通知のテキストを設定しからの通知アイコンが読み込まれます**Resources/drawable/ic_notification.png**します。 通知のビルダーへの呼び出し`Build`メソッドは、これらの設定で、通知オブジェクトを作成します。 呼び出す次の手順では、`Notify`通知マネージャーのメソッド。 通知マネージャーが見つかりません、呼び出す`GetSystemService`の上に示すようにします。

`Notify`メソッドは 2 つのパラメーターを受け取ります。 通知の id と、通知オブジェクト。 通知 id は、アプリケーションに通知を識別する一意の整数です。 この例をゼロ (0); 通知 id を設定します。ただし、運用環境のアプリケーションで通知ごとに一意の識別子を提供するされます。 以前の識別子の値への呼び出しで再利用`Notify`を上書きできる最後の通知を発生します。

このコードを Android 5.0 デバイスで実行すると、次の例のような通知が生成されます。

![サンプル コードについての通知結果](local-notifications-images/09-hello-world.png)

通知アイコンが通知の左側に表示される&ndash;、丸付きのこのイメージ&ldquo;は&rdquo;アルファ チャネルを持つ Android は、その背後にある灰色の循環背景を描画できるようにします。 アルファ チャネルなしアイコンを指定することもできます。 写真のイメージをアイコンとして表示するを参照してください。[大きなアイコン形式](#large-icon-format)このトピックで後述します。

タイムスタンプが自動的に設定が、この設定を上書きするには、呼び出すことによって、[プールデフォルトプロセッサ](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/)通知ビルダーのメソッド。 たとえば、次のコード例では、現在の時刻にタイムスタンプを設定します。

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>サウンドと振動を有効にします。

通知のビルダーを呼び出すことができます、通知もサウンドを再生する場合は、 [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/)メソッドを渡します、`NotificationDefaults.Sound`フラグ。

```csharp
// Instantiate the notification builder and enable sound:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

この呼び出しを`SetDefaults`通知が発行されたときにサウンドを再生するデバイスになります。 サウンドを再生するのではなく、振動をデバイスにする場合は、渡す`NotificationDefaults.Vibrate`に`SetDefaults.`サウンドを再生して、デバイスを振動をデバイスにする場合は、両方のフラグを渡すことができます`SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

サウンドを再生するサウンドを指定せずを有効にした場合、Android には、既定のシステム通知サウンドが使用されます。 ただし、通知のビルダーを呼び出すことによって再生されるサウンドを変更できます[SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/)メソッド。 たとえば、アラームを再生する (の既定の通知サウンド) ではなく、通知でサウンド表示できます、URI、行動はアラームからサウンド、 [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/)に渡すと`SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

またはを使用できますシステム既定の着信音サウンドに、通知。

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

通知オブジェクトを作成した後、通知オブジェクトの通知プロパティを設定することは (それらから事前に構成するのではなく`Notification.Builder`メソッド)。 呼び出す代わりに、たとえば、`SetDefaults`通知、振動を有効にする方法の通知のビット フラグを直接変更することができます[既定値は](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/)プロパティ。

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

この例では、通知が発行されたときに振動するデバイス。

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>通知を更新しています

公開された後、通知のコンテンツを更新する場合は、既存の再利用できる`Notification.Builder`オブジェクトの新しい通知オブジェクトを作成し、最後の通知の識別子では、この通知を発行します。 例えば:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

この例で、既存`Notification.Builder`オブジェクトが別のタイトルとメッセージで新しい通知オブジェクトを作成するために使用します。
前の通知の識別子を使用して、新しい通知オブジェクトを公開し、公開されていた通知のコンテンツを更新します。

![更新された通知](local-notifications-images/12-updated-notification.png)

前回の通知の本文が再利用&ndash;タイトルのみと、通知が変更通知ドロアーで、通知が表示されている間のテキスト。 タイトルのテキストが「サンプル通知」から「更新通知」に変更され、"Hello World からのメッセージ テキストの変更! これは、最初の通知です。" 「このメッセージに変更します」。

次の 3 つのいずれかの状況になるまで、通知が表示されます。

-   ユーザーが通知を閉じる (をクリックまたはタップ*すべてクリア*)。

-   呼び出しは、アプリケーションは`NotificationManager.Cancel`通知の発行時に割り当てられた一意の通知 ID を渡しています。

-   アプリケーション呼び出し`NotificationManager.CancelAll`します。

Android の通知の更新については、次を参照してください。[変更通知](http://developer.android.com/training/notify-user/managing.html#Updating)します。


### <a name="starting-an-activity-from-a-notification"></a>通知から、アクティビティの開始

Android が関連付けられている通知の一般的な*アクション*&ndash;ユーザー通知をタップしたときに起動するアクティビティ。 このアクティビティは、別のアプリケーションまたは別のタスクであってもかまいません。 作成する通知をアクションを追加する、 [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)オブジェクトし、関連付ける、`PendingIntent`通知を使用します。 A`PendingIntent`により、送信元アプリケーションのアクセス許可を持つ定義済みのコードを実行する受信者のアプリケーション インテントの特別な型です。 Android の起動時で指定されたアクティビティ、ユーザーが通知をタップしたとき、`PendingIntent`します。

次のコード スニペットを含む通知を作成する方法を示しています、`PendingIntent`が元のアプリのアクティビティを起動する`MainActivity`:

```csharp
// Set up an intent so that tapping the notifications returns to this app:
Intent intent = new Intent (this, typeof(MainActivity));

// Create a PendingIntent; we're only using one PendingIntent (ID = 0):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    PendingIntent.GetActivity (this, pendingIntentId, intent, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including pending intent:
Notification.Builder builder = new Notification.Builder(this)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

このコードは、点を除いて前のセクションで通知コードによく似ています、`PendingIntent`通知オブジェクトに追加されます。 この例で、`PendingIntent`通知ビルダーに渡される前に、元のアプリのアクティビティに関連付けられて[SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/)メソッド。 `PendingIntentFlags.OneShot`にフラグが渡される、`PendingIntent.GetActivity`メソッドように、 `PendingIntent` 1 回だけ使用されます。 このコードを実行すると、次の通知が表示されます。

![最初のアクション通知](local-notifications-images/10-first-action-notification.png)

元の活動に戻るユーザーは、この通知をタップします。

実稼働アプリケーションでは、アプリが処理する必要があります、*戻るスタック*押されたとき、**戻る**通知アクティビティ内のボタン (Android のタスクと戻るスタックに慣れていない場合を参照してください[タスクと戻るスタック](http://developer.android.com/guide/components/tasks-and-back-stack.html))。
ほとんどの場合、通知のアクティビティから後方移動をアプリからサインアウトし、ホーム画面に戻り、ユーザーが返す必要があります。 戻るスタックをアプリで使用を管理する、 [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/)クラスを作成する、`PendingIntent`戻るスタックとします。

実際の考慮事項としては、元のアクティビティは、通知のアクティビティにデータを送信する必要があります。 たとえば、通知は、テキスト メッセージが到着すると、その通知のアクティビティ (画面を表示するメッセージ) には、ユーザーにメッセージを表示するメッセージの ID が必要です。 可能性があります。 作成するアクティビティ、`PendingIntent`使用できる、 [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/)インテントにこのデータは、通知のアクティビティに渡されるように、データ (たとえば、文字列) を追加するメソッド。

次のコード サンプルは、使用する方法を示しています`TaskStackBuilder`戻るスタック、およびそれを管理するという通知アクティビティを 1 つのメッセージ文字列を送信する方法の例が含まれています。 `SecondActivity`:。

```csharp
// Setup an intent for SecondActivity:
Intent secondIntent = new Intent (this, typeof(SecondActivity));

// Pass some information to SecondActivity:
secondIntent.PutExtra ("message", "Greetings from MainActivity!");

// Create a task stack builder to manage the back stack:
TaskStackBuilder stackBuilder = TaskStackBuilder.Create(this);

// Add all parents of SecondActivity to the stack:
stackBuilder.AddParentStack (Java.Lang.Class.FromType (typeof (SecondActivity)));

// Push the intent that starts SecondActivity onto the stack:
stackBuilder.AddNextIntent (secondIntent);

// Obtain the PendingIntent for launching the task constructed by
// stackbuilder. The pending intent can be used only once (one shot):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    stackBuilder.GetPendingIntent (pendingIntentId, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including
// the pending intent:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my second action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

このコード例では、アプリは 2 つのアクティビティで構成されます: `MainActivity` (コードを含む通知上)、および`SecondActivity`画面の通知をタップした後、ユーザーに表示されます。 このコードを実行すると、(前の例のような) 単純な通知が表示されます。 通知をタップは、ユーザーを`SecondActivity`画面。

![2 番目のアクティビティのスクリーン ショット](local-notifications-images/11-second-activity.png)

文字列メッセージ (目的に渡される`PutExtra`メソッド) で取得されます`SecondActivity`のコード行を使用して。

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

「Greetings から MainActivity!、」、取得したこのメッセージが表示されます、`SecondActivity`画面で、上記のスクリーン ショットに示すようにします。 押されたとき、**戻る**の中にボタン`SecondActivity`ナビゲーションのリードをアプリからサインアウトし、アプリの起動の前の画面に戻る。

保留中のインテントを作成する詳細については、次を参照してください。 [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)します。


<a name="notif-chan"></a>
<a name="notification-channels"></a>
## <a name="notification-channels"></a>通知チャネル

Android 8.0 (Oreo) 以降を使えば、*通知チャネル*を表示する通知の種類ごとにユーザーがカスタマイズできるチャネルを作成する機能。 通知チャネルでは、チャネルの展示に同じ動作にポストされたすべての通知できるように、グループの通知を使用します。 たとえば、即時の注意が必要な通知が想定されている通知チャネルと情報メッセージに使用される個別の「静か」チャネルがあります。

**YouTube** Android Oreo でインストールされているアプリが通知の 2 つのカテゴリを一覧表示:**ダウンロードの通知**と**一般的な通知**:

[![Android Oreo で YouTube の通知画面](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

これらの各カテゴリは、通知チャネルに対応します。 YouTube のアプリの実装を**ダウンロードの通知**チャネルと**一般的な通知**チャネル。 タップする**ダウンロードの通知**アプリの通知チャネルのダウンロードの設定 画面が表示されます。

[![YouTube のアプリの通知画面をダウンロードします。](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

この画面で、ユーザーがの動作を変更できる、**ダウンロード**通知は、次の手順に従ってチャネルします。

-   重要度をレベルを設定**Urgent**、**高**、 **Medium**、または**低**、サウンド、およびビジュアルの中断のレベルが構成されます。

-   通知ドットをオンまたはオフにします。

-   点滅のライトをオンまたはオフにします。

-   表示またはロック画面で通知を非表示にします。

-   上書き、**不可**設定します。

**一般的な通知**チャネルが同様の設定。

[![YouTube のアプリの通知の [全般] 画面](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

ユーザーと、通知チャネルの対話を完全に制御がないことに注意してください&ndash;上記のスクリーン ショットに示すように、ユーザーがデバイス上のすべての通知チャネルの設定を変更できます。 ただし、(以下に記載されます) には、既定値を構成できます。 これらの例に示すよう、新しい通知チャネル機能によりさまざまな種類の通知をきめ細かく制御できるようにします。

アプリに通知チャネルのサポートを追加する必要がありますか。 Android 8.0 では、アプリを対象としている場合*する必要があります*通知チャネルを実装します。
Oreo デバイスで通知を表示する通知チャネルを使用せず、ユーザーにローカル通知を送信しようとする Oreo を対象とするアプリが失敗します。 Android 8.0 をターゲットにしない場合、アプリは実行されます Android 8.0 でが同じ通知の動作が発生 Android 7.1 以前を実行している場合。


### <a name="creating-a-notification-channel"></a>通知チャネルを作成します。

通知チャネルを作成するには、次の操作を行います。

1. 構築、 [NotificationChannel](https://developer.android.com/reference/android/app/NotificationChannel.html)を次のオブジェクト。

    - ID 文字列は、パッケージ内で一意です。 次の例では、文字列で`com.xamarin.myapp.urgent`使用されます。

    - チャネル (より小さい 40 文字) のユーザーに表示される名前。 次の例では、名前で**Urgent**使用されます。

    - どの割り込み型の通知を制御すると、チャネルの重要性にポストされた、`NotificationChannel`します。 重要度があります`Default`、 `High`、 `Low`、 `Max`、 `Min`、 `None`、または`Unspecified`します。

    これらの値を渡す、[コンス トラクター](https://developer.android.com/reference/android/app/NotificationChannel.html#NotificationChannel%28java.lang.String,%20java.lang.CharSequence,%20int%29) (この例で`Resource.String.noti_chan_urgent`に設定されている**Urgent**)。

    ```csharp
    public const string URGENT_CHANNEL = "com.xamarin.myapp.urgent";
    . . .
    string chanName = GetString (Resource.String.noti_chan_urgent);
    var importance = NotificationImportance.High;
    NotificationChannel chan =
       new NotificationChannel (URGENT_CHANNEL, chanName, importance);
    ```

2.  構成、`NotificationChannel`初期設定を持つオブジェクト。
    たとえば、次のコードでは構成、`NotificationChannel`オブジェクトをこのチャネルに投稿の通知は、デバイスを振動され、既定では、ロック画面に表示されるようします。

    ```csharp
    chan.EnableVibration (true);
    chan.LockscreenVisibility = NotificationVisibility.Public;
    ```

3.  通知マネージャーの通知チャネル オブジェクトを送信します。

    ```csharp
    NotificationManager notificationManager =
        (NotificationManager) GetSystemService (NotificationService);
    notificationManager.CreateNotificationChannel (chan);
    ```


### <a name="posting-to-a-notifications-channel"></a>通知チャネルに投稿します。

通知チャネルに通知を投稿するには、次の操作を行います。

1.  通知を使用して、構成、`Notification.Builder`でチャネル ID を渡し、`SetChannelId`メソッド。 例えば:

    ```csharp
    Notification.Builder builder = new Notification.Builder (this)
        .SetContentTitle ("Attention!")
        .SetContentText ("This is an urgent notification message!")
        .SetChannelId (URGENT_CHANNEL);
    ```

2.  ビルドし、通知マネージャーを使用して通知を発行[通知](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/p/System.Int32/Android.App.Notification/)メソッド。

    ```csharp
    const int notificationId = 0;
    notificationManager.Notify (notificationId, builder.Build());
    ```

情報メッセージ用の別の通知チャネルを作成するには、上記の手順を繰り返すことができます。 この 2 番目のチャネル、既定では、振動を無効にする、既定のロック画面の表示に設定`Private`に通知の重要度を設定および`Default`します。

アクションで Android Oreo の通知チャネルの完全なコード例を参照してください、 [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels)サンプルです。 このサンプル アプリは、2 つのチャネルを管理し、追加の通知オプションを設定します。



<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>基本的な通知を超える

Android では、単純な標準ベースのレイアウト形式を既定の通知が追加を行うことにより、この基本的な形式を強化することができます、`Notification.Builder`メソッドの呼び出し。 このセクションで、通知に大きな写真のアイコンを追加する方法について説明し、レイアウトが展開された通知を作成する方法の例が表示されます。

<a name="large-icon-format" />

### <a name="large-icon-format"></a>大きいアイコン

Android の通知は通常、(通知の左側にある) で元のアプリのアイコンを表示します。 通知がイメージや写真を表示するただし、(、*大きいアイコン*) 標準の小さいアイコンの代わりにします。 たとえば、メッセージング アプリでは、アプリのアイコンではなく、送信者の写真を表示できます。

基本的な Android 5.0 通知の例を次に示します&ndash;小さいアプリ アイコンのみが表示されます。

![正常な通知の例](local-notifications-images/13-sample-notification.png)

ここでは、大きいアイコンを表示する変更を加えた後、通知のスクリーン ショットと&ndash;Xamarin コードの monkey のイメージから作成されたアイコンを使用します。

![大きいアイコン通知の例](local-notifications-images/14-large-icon-sample.png)

大きいアイコンでは、通知が表示されたらに、小さいアプリ アイコンが大きいアイコンの右下隅でバッジとして表示されることに注意してください。

通知のビルダーを呼び出すことを通知では、大きいアイコンとしてイメージを使用する[SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/)メソッドと、イメージのビットマップを渡します。 異なり`SetSmallIcon`、`SetLargeIcon`ビットマップのみを受け入れます。 使用するビットマップにイメージ ファイルを変換する、 [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/)クラス。 例えば:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

このコード例があるイメージ ファイルを開き**Resources/drawable/monkey_icon.png**、ビットマップに変換し、結果のビットマップを渡します`Notification.Builder`します。 通常、ソース イメージの解像度は小さいアイコンよりも大きい&ndash;が大きくありません。 通知の投稿を延期がサイズ変更の操作が不要なイメージが大きすぎますがあります。
詳細については、Android の通知アイコンのサイズは、次を参照してください。[通知アイコン](http://developer.android.com/design/style/iconography.html#notification)します。


### <a name="big-text-style"></a>大きなテキストのスタイル

*大きなテキスト*スタイルは、展開されたレイアウト テンプレートを通知に時間の長いメッセージを表示するために使用します。 すべてのレイアウトが展開された通知のようなプレゼンテーションのコンパクトな形式で大きなテキスト通知が最初に表示されます。

![大きなテキスト通知の例](local-notifications-images/15-big-text-notification.png)

この形式でメッセージの抜粋のみが表示になり、2 つのピリオドで終了されます。 通知方法ユーザーがドラッグしたとき、全体の通知メッセージを表示する展開します。

![展開された大きなテキスト通知](local-notifications-images/16-big-text-expanded.png)

この展開のレイアウト形式には、通知の下部にある概要テキストも含まれています。 最大の高さ、*大きなテキスト*通知は、256 dp します。

作成する、*大きなテキスト*インスタンス化、通知、`Notification.Builder`オブジェクトと同様に、インスタンス化し、し、追加、 [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/)オブジェクトを`Notification.Builder`オブジェクト。 例えば:

```csharp
// Instantiate the Big Text style:
Notification.BigTextStyle textStyle = new Notification.BigTextStyle();

// Fill it with text:
string longTextMessage = "I went up one pair of stairs.";
longTextMessage += " / Just like me. ";
//...
textStyle.BigText (longTextMessage);

// Set the summary text:
textStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (textStyle);

// Create the notification and publish it ...
```

この例で、メッセージ テキストおよび概要のテキストを格納します、`BigTextStyle`オブジェクト (`textStyle`) に渡される前に。 `Notification.Builder.`


### <a name="image-style"></a>画像のスタイル

*イメージ*スタイル (とも呼ばれる、*全体像*スタイル) は、通知の本文にイメージを表示するのに使用できる展開の通知形式です。 スクリーン ショットのアプリまたはフォト アプリを使用できるなど、*イメージ*キャプチャされた最後の通知をユーザーに提供するスタイルのイメージを通知します。 注意の最大の高さ、*イメージ*通知は、256 dp &ndash; Android がこの最大の高さの制限、使用可能なメモリの制限内に収まるようにイメージのサイズを変更します。

すべてのようなレイアウトの通知を展開*イメージ*通知は最初に付随するメッセージのテキストの抜粋を表示する、コンパクトな形式で表示されます。

![コンパクトなイメージの通知にイメージが表示されません。](local-notifications-images/17-image-compact.png)

ユーザーを引っ張るとき、*イメージ*イメージを表示する通知を展開します。 たとえば、前の通知の拡張のバージョンを示します。

![展開イメージの通知が表示の画像](local-notifications-images/18-image-expanded.png)

コンパクトな形式で、通知が表示される場合は、通知のテキスト表示ことに注意してください (通知のビルダーに渡されるテキスト`SetContentText`メソッドは、前述のように)。 ただし、通知を展開するには、イメージを表示すると、画像の上の概要のテキストが表示されます。

作成する、*イメージ*インスタンス化、通知、`Notification.Builder`オブジェクトと同様に、作成し、挿入、 [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/)オブジェクトを`Notification.Builder`オブジェクト。 例えば:

```csharp
// Instantiate the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Convert the image to a bitmap before passing it into the style:
picStyle.BigPicture (BitmapFactory.DecodeResource (Resources, Resource.Drawable.x_bldg));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create the notification and publish it ...
```

ように、`SetLargeIcon`メソッドの`Notification.Builder`、 [BigPicture](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/)メソッドの`BigPictureStyle`通知の本文に表示するイメージのビットマップが必要です。 この例で、 [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32))メソッドの`BitmapFactory`にあるイメージ ファイルの読み取り**Resources/drawable/x_bldg.png**ビットマップに変換します。

リソースとしてパッケージ化されませんイメージを表示することもできます。 次のサンプル コードのローカルの SD カードからイメージを読み込みますなどと表示されます、*イメージ*通知。

```csharp
// Using the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Read an image from the SD card, subsample to half size:
BitmapFactory.Options options = new BitmapFactory.Options();
options.InSampleSize = 2;
string imagePath = "/sdcard/Pictures/my-tshirt.jpg";
picStyle.BigPicture (BitmapFactory.DecodeFile (imagePath, options));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("Check out my new T-Shirt!");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create notification and publish it ...
```

イメージ ファイルがあるこの例で **/sdcard/Pictures/my-tshirt.jpg**は読み込まれ、元のサイズの半分にサイズ変更、通知で使用するためのビットマップに変換されます。

![通知の例の t シャツ イメージ](local-notifications-images/19-tshirt-notification.png)

呼び出しをラップすることをお勧めは場合は、イメージ ファイルのサイズを事前にわかっていない、 [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/)例外ハンドラーで&ndash;、`OutOfMemoryError`に対してイメージが大きすぎる場合、例外がスローされます。Android のサイズを変更します。

詳細については、読み込みと大きいビットマップ イメージのデコードは、次を参照してください。[ロード大きなビットマップ効率的に](https://github.com/xamarin/recipes/tree/master/Recipes/android/resources/general/load_large_bitmaps_efficiently)します。


### <a name="inbox-style"></a>受信トレイのスタイル

*受信トレイ*形式は、展開されたレイアウト テンプレート通知の本文にテキスト (たとえば、電子メールの受信トレイ概要) の別の行を表示する対象としています。 *受信トレイ*形式の通知が最初に、コンパクトな形式で表示されます。

![例のコンパクトな受信トレイの通知](local-notifications-images/20-inbox-compact.png)

通知方法ユーザーがドラッグしたとき、次のスクリーン ショットに示すように、電子メール概要の表示を展開します。

![展開の例の受信トレイの通知](local-notifications-images/21-inbox-expanded.png)

作成する、*受信トレイ*インスタンス化、通知、`Notification.Builder`と同様に、オブジェクトを追加、 [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/)オブジェクトを`Notification.Builder`します。 例えば:

```csharp
// Instantiate the Inbox style:
Notification.InboxStyle inboxStyle = new Notification.InboxStyle();

// Set the title and text of the notification:
builder.SetContentTitle ("5 new messages");
builder.SetContentText ("chimchim@xamarin.com");

// Generate a message summary for the body of the notification:
inboxStyle.AddLine ("Cheeta: Bananas on sale");
inboxStyle.AddLine ("George: Curious about your blog post");
inboxStyle.AddLine ("Nikko: Need a ride to Evolve?");
inboxStyle.SetSummaryText ("+2 more");

// Plug this style into the builder:
builder.SetStyle (inboxStyle);
```

通知の本文には、新しい行のテキストを追加するには、呼び出し、[を](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/)のメソッド、`InboxStyle`オブジェクト (の最大の高さ、*受信トレイ*通知は、256 dp)。 異なりに注意してください*大きなテキスト*、スタイル、*受信トレイ*スタイルは、通知の本文で個々 のテキスト行をサポートしています。

使用することも、*受信トレイ*拡張の形式で個々 のテキスト行を表示する必要がある任意の通知のスタイル。 たとえば、*受信トレイ*通知スタイルは、概要の通知に保留中の通知を複数の結合に使用することができます&ndash;、1 つを更新する*受信トレイ*new で通知をスタイル設定行の通知内容 (を参照してください[更新通知](#updating-a-notification)上) ではなくよりも、ほとんどの場合と同様、新しい通知の連続ストリームを生成します。 この方法の詳細については、次を参照してください。 [、通知を要約](http://developer.android.com/design/patterns/notifications.html#summarize_your_notifications)します。


## <a name="configuring-metadata"></a>メタデータの構成

`Notification.Builder` 優先度、可視性、およびカテゴリなど、通知に関するメタデータの設定を呼び出すことのできるメソッドが含まれています。 Android はこの情報を使用して&mdash;ユーザー設定と共に&mdash;方法とタイミングを決定する通知を表示します。


### <a name="priority-settings"></a>優先順位の設定

通知の優先順位の設定では、通知が発行されたときに、2 つの結果が決定します。

-   通知は、その他の通知に関連が表示されます。
    たとえば、優先度の高い通知が表示通知ドロワーでは低優先度の通知の上に関係なく各通知の発行時にされます。

-   かどうかヘッドアップ通知の形式 (Android 5.0 以降) で、通知が表示されます。 のみ*高*と*最大*ヘッドアップ通知として優先順位の通知が表示されます。

Xamarin.Android では、通知の優先順位を設定するための次の列挙型を定義します。

-   `NotificationPriority.Max` &ndash; 緊急度や重要な条件 (着信通話を有効にする - 案内、または緊急のアラートなど) をユーザーに警告します。 Android 5.0 と以降のデバイスでは、最大の優先順位の通知はヘッドアップ形式で表示されます。

-   `NotificationPriority.High` &ndash; (重要なメールやリアルタイムのチャット メッセージの到着) などの重要なイベントのユーザーに通知します。 Android 5.0 と以降のデバイスでは、優先度の高い通知はヘッドアップ形式で表示されます。

-   `NotificationPriority.Default` &ndash; 重要度のレベルが中程度の条件のユーザーに通知します。

-   `NotificationPriority.Low` &ndash; 不急については、ユーザーが (たとえば、ソフトウェア更新プログラムの通知またはソーシャル ネットワークの更新プログラム) の形式でする必要があること。

-   `NotificationPriority.Min` &ndash; 背景情報については、ユーザー通知の場合にのみ、通知 (たとえば、場所、気象情報) を表示します。

通知の優先順位を設定するには、呼び出し、 [SetPriority](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/)のメソッド、`Notification.Builder`優先度レベルを渡して、オブジェクト。 例えば:

```csharp
builder.SetPriority (NotificationPriority.High);
```

次の例、優先度の高い通知、「重要なメッセージ。」 通知ドロアーの上部に表示されます。

![例 - 優先度の高い通知](local-notifications-images/22-hi-priority-drawer.png)

これは優先度の高い通知であるため、Android 5.0 で、ユーザーの現在のアクティビティ画面上のヘッドアップ通知としても表示されます。

![例のヘッドアップ通知](local-notifications-images/23-heads-up-example.png)

次の例では、優先順位の高いバッテリ レベル通知 優先順位の低い「考え、その日の」通知が表示されます。

![優先順位の低い通知の例](local-notifications-images/24-lo-priority-drawer.png)

「考え、その日の」通知は、優先順位の低い通知であるため Android は表示されませんがヘッドアップ形式。


### <a name="visibility-settings"></a>可視性の設定

Android 5.0 以降、*可視性*設定については、通知コンテンツの量がセキュリティで保護されたロック画面の表示を制御します。
Xamarin.Android では、次の列挙型の可視性の通知の設定を定義します。

-   `NotificationVisibility.Public` &ndash; セキュリティで保護されたロック画面通知の完全なコンテンツが表示されます。

-   `NotificationVisibility.Private` &ndash; (通知アイコンや投稿されたことをアプリの名前)、セキュリティで保護されたロック画面で重要な情報のみが表示されますが、通知の詳細の残りの部分は表示されません。 既定ですべての通知`NotificationVisibility.Private`します。

-   `NotificationVisibility.Secret` &ndash; セキュリティで保護されたロック画面、通知アイコンも表示されないのは何も表示します。 通知の内容は、ユーザー、デバイスのロックを解除した後にのみ使用できます。

アプリの呼び出し、通知の表示を設定する、`SetVisibility`のメソッド、`Notification.Builder`可視性の設定を渡して、オブジェクト。 たとえば、この呼び出しを`SetVisibility`通知は、 `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

ときに、`Private`通知が投稿された、セキュリティで保護されたロック画面で、名前と、アプリのアイコンが表示されます。 通知メッセージではなくユーザーには、「この通知を表示する、デバイス ロック解除」が表示されます。

![デバイスの通知メッセージのロックを解除します。](local-notifications-images/25-lockscreen-private.png)

この例で**NotificationsLab**発信元のアプリの名前を指定します。 通知の修正されたこのバージョンは、ロック画面がセキュリティで保護された場合にのみ表示されます (つまり、PIN、パターン、またはパスワードを使用して保護する)&ndash;通知の完全なコンテンツは、ロック画面で使用できる、ロック画面が安全でない場合。


### <a name="category-settings"></a>カテゴリの設定

Android 5.0 以降では、定義済みのカテゴリは、順位付けおよび通知をフィルター処理で使用できます。 Xamarin.Android では、これらのカテゴリは、次の列挙を提供します。

-   `Notification.CategoryCall` &ndash; 着信呼び出し。

-   `Notification.CategoryMessage` &ndash; 受信したテキスト メッセージ。

-   `Notification.CategoryAlarm` &ndash; アラーム条件またはタイマー期限。

-   `Notification.CategoryEmail` &ndash; 受信電子メール メッセージ。

-   `Notification.CategoryEvent` &ndash; 予定表イベントです。

-   `Notification.CategoryPromo` &ndash; キャンペーンのメッセージまたは提供情報。

-   `Notification.CategoryProgress` &ndash; バック グラウンド操作の進行状況。

-   `Notification.CategorySocial` &ndash; ソーシャル ネットワー キング更新します。

-   `Notification.CategoryError` &ndash; バック グラウンド操作または認証プロセスのエラーです。

-   `Notification.CategoryTransport` &ndash; メディアの再生更新します。

-   `Notification.CategorySystem` &ndash; システムの使用 (システムまたはデバイスの状態) に予約されています。

-   `Notification.CategoryService` &ndash; バック グラウンド サービスが実行されていることを示します。

-   `Notification.CategoryRecommendation` &ndash; 現在実行中のアプリに関連する推奨事項のメッセージ。

-   `Notification.CategoryStatus` &ndash; デバイスに関する情報。

通知が並べ替えられる場合に、通知の優先順位が、そのカテゴリの設定よりも優先されます。 たとえば、優先度の高い通知はヘッドアップとして表示するのに属している場合でも、`Promo`カテゴリ。 呼び出すことの通知のカテゴリを設定する、`SetCategory`のメソッド、`Notification.Builder`カテゴリの設定を渡して、オブジェクト。 例えば:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

*不可*(Android 5.0 の新機能) の機能のカテゴリに基づく通知をフィルター処理します。 たとえば、*不可*画面で**設定**電話の通話やメッセージの通知を除外するユーザーを許可。

![画面のスイッチは不可します。](local-notifications-images/26-do-not-disturb.png)

ユーザーが構成され*不可*のカテゴリの設定を使用した通知を使用している Android (上記のスクリーン ショットに示すよう) に電話を除くすべての割り込みをブロックする`Notification.CategoryCall`中、デバイスに表示します。*不可*モード。 なお`Notification.CategoryAlarm`で通知がブロックされることはありません*不可*モード。


<a name="compatibility" />

## <a name="compatibility"></a>互換性

アプリを作成する場合も (早い段階で API レベル 4)、Android の以前のバージョンで実行を使用する、 [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)クラスの代わりに`Notification.Builder`します。 使用した通知を作成すると`NotificationCompat.Builder`Android では、基本的な通知の内容が古いデバイス上で適切に表示されることにより、します。 ただし、いくつかの高度な通知機能が以前のバージョンの Android で利用できないため、でも、コード展開された通知のスタイル、カテゴリ、および次に示すように、可視性レベルの互換性の問題する必要があります明示的に処理します。

使用する`NotificationCompat.Builder`含める必要があります、アプリで、 [Android サポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)プロジェクトで NuGet です。

次のコード サンプルを使用して基本的な通知を作成する方法を示しています`NotificationCompat.Builder`:。

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();
```

重要な通知のオプションのメソッド呼び出しがのと同じですがこの例に示すように、`Notification.Builder`します。 ただしより複雑な通知が (次のセクションで説明) の下の互換性の問題を処理するために、コードがあります。

[LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications)サンプルを使用する方法を示します`NotificationCompat.Builder`通知から 2 番目のアクティビティを起動します。 このサンプル コードについては、 [Xamarin.Android でのローカルの通知を使用して](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)チュートリアル。


### <a name="notification-styles"></a>通知のスタイル

作成する*大きなテキスト*、*イメージ*、または*受信トレイ*スタイルを使用した通知`NotificationCompat.Builder`アプリはこれらのスタイルの互換バージョンを使用する必要があります。 たとえば、使用するため、*大きなテキスト*スタイルをインスタンス化`NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

同様に、アプリで使用できる`NotificationCompat.InboxStyle`と`NotificationCompat.BigPictureStyle`の*受信トレイ*と*イメージ*スタイル、それぞれします。


### <a name="notification-priority-and-category"></a>通知の優先順位とカテゴリ

`NotificationCompat.Builder` サポート、`SetPriority`メソッド (Android 4.1 で使用可能な開始位置)。 ただし、`SetCategory`メソッドは*いない*でサポートされている`NotificationCompat.Builder`カテゴリ Android 5.0 で導入された新しい通知のメタデータ システムの一部であるためです。

Android、where の以前のバージョンをサポートするために`SetCategory`が利用できない、コードは実行時に条件付きで呼び出す API レベルを確認できます`SetCategory`Android 5.0 (API レベル 21) 以上の API レベルの場合します。

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

この例では、アプリの**ターゲット フレームワーク**Android 5.0 に設定されていると、**最小 Android バージョン**に設定されている**Android 4.1 (API レベル 16)** します。 `SetCategory`は API レベル 21 以降で使用でき、このコード例が呼び出す`SetCategory`が使用可能な&ndash;は呼び出しません`SetCategory`API レベルの場合より小さい
21.


### <a name="lockscreen-visibility"></a>ロック画面の表示

Android では、Android 5.0 (API レベル 21) の前にロック画面通知をサポートしていないため、`NotificationCompat.Builder`はサポートしていません、`SetVisibility`メソッド。 前述のように`SetCategory`、コードが実行時と呼び出し API レベルを確認できます`SetVisiblity`が使用可能な。

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetVisibility (Notification.Public);
}
```


## <a name="summary"></a>まとめ

この記事では、Android でのローカル通知を作成する方法について説明します。 通知の詳細を説明し、使用する方法について説明し`Notification.Builder`、通知を作成する大きなアイコンのスタイルの通知方法*大きなテキスト*、*イメージ*と*受信トレイ*形式、通知の優先度、可視性、およびカテゴリなどのメタデータの設定を設定する方法、および通知からアクティビティを起動する方法。 この記事でこれらの通知設定が新しいヘッドアップ、ロック画面を使用する方法についても説明し、*不可*Android 5.0 で導入された機能です。 最後に、使用する方法を学習しました`NotificationCompat.Builder`以前のバージョンの Android の通知の互換性を維持します。

Android 用の通知の設計に関するガイドラインについては、次を参照してください。[通知](http://developer.android.com/preview/notifications.html)します。


## <a name="related-links"></a>関連リンク

- [NotificationsLab (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (サンプル)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Android のチュートリアルでのローカル通知](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [通知](http://developer.android.com/design/patterns/notifications.html)
- [ユーザーに通知します。](http://developer.android.com/training/notify-user/index.html)
- [通知](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
