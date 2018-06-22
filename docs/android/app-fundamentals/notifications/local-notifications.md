---
title: ローカルの通知
description: このセクションでは、Xamarin.Android でローカルの通知を実装する方法を示します。 Android の通知のさまざまな UI 要素について説明し、API について説明します、通知が表示の作成と関係しているのです。
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 97c8372656f0cbfa5b8f7bb12d15b00feac4b5c3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773993"
---
# <a name="local-notifications"></a>ローカルの通知

_このセクションでは、Xamarin.Android でローカルの通知を実装する方法を示します。Android の通知のさまざまな UI 要素について説明し、API について説明します、通知が表示の作成と関係しているのです。_

## <a name="local-notifications-overview"></a>ローカルの通知の概要

このトピックでは、Xamarin.Android アプリケーションでローカルの通知を実装する方法について説明します。 Android の通知のさまざまな部分についても説明し、アプリ開発者が利用できるさまざまな通知スタイルについて説明します作成および通知の発行に使用される Api の一部が導入されています。

Android は、ユーザーに通知アイコンおよび通知情報を表示するための 2 つのシステム管理の対象領域を提供します。 そのアイコンを表示、通知が最初に発行されたときに、*通知領域*の次のスクリーン ショットに示すように、します。

![デバイスの通知領域の例](local-notifications-images/01-notification-shade.png)

通知の詳細を取得するには、ユーザーが (通知の内容を表示するには、各通知アイコンを展開) を通知ドロワーを開くことができ、通知に関連付けられている任意のアクションを実行します。 次のスクリーン ショット、*通知ドロワー*上に表示される通知領域に対応します。

[![例通知ドロワーを 3 つの通知を表示します。](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Android の通知は、2 種類のレイアウトを使用します。

-   ***ベース レイアウト*** &ndash; compact、固定のプレゼンテーション形式です。

-   ***展開されたレイアウト***&ndash;詳細情報を表示するサイズを大きくする拡張可能な表示形式。

これらのレイアウト型 (およびそれらを作成する方法) については、次のセクションで説明します。


### <a name="base-layout"></a>ベース レイアウト

これには、少なくとも、次の要素が含まれています。 レイアウトの基本形式には、すべての Android 通知が組み込まれています。

1.  A*通知アイコン*、またはを表す送信元のアプリでは、通知の種類、アプリは、さまざまな種類の通知をサポートしている場合。

2.  通知*タイトル*、または、通知は個人的なメッセージの場合は、送信者の名前。

3.  通知メッセージです。

4.  A*タイムスタンプ*です。

次の図に示すように、これらの要素が表示されます。

[![通知要素の場所](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

基本のレイアウトは、高さ 64 の密度に依存しないピクセル (dp) に制限されます。 Android では、既定ではこの基本的な通知のスタイルを作成します。

必要に応じて、通知は、アプリケーションまたは送信者の写真を表す大規模なアイコンを表示できます。 大きいアイコンが通知 Android 5.0 以降で使用される、小規模の通知アイコンが大きなアイコンの上バッジとして表示されます。

![単純な通知の写真](local-notifications-images/04-simple-notification-photo.png)

Android 5.0 以降では、通知もに表示できる、ロック画面。

[![例のロック画面通知](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

ユーザー ダブルタップがデバイスのロックを解除し、その通知が送信されたアプリにジャンプするには、ロック画面通知またはスワイプして通知を無視します。 ユーザーには、ロック画面通知に表示される機密性の高いコンテンツを許可するかどうかを指定でき、アプリは、ロック画面に表示される内容を制御する通知の参照範囲レベルを設定できます。

Android 5.0 導入と呼ばれる高優先度の通知のプレゼンテーション形式*ヘッドアップ*です。 ヘッドアップ通知は、数秒、画面の上部から下にスライドさせます、通知領域にバックアップし、退却。

[![例ヘッドアップ通知](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

ヘッドアップ通知では、システム UI を現在実行中のアクティビティの状態を中断することがなく、ユーザーの前に重要な情報を格納できます。

Android には、通知を並べ替え、適切に表示されることができます、通知のメタデータのサポートが含まれます。 通知のメタデータとヘッドアップ形式で、ロック画面で通知を表示する方法も制御します。 アプリケーションでは、次の種類の通知のメタデータを設定できます。

-   **優先順位**&ndash;優先度レベルは、通知が表示される方法とタイミングを決定します。 たとえば、Android 5.0 では、優先度の高い通知はヘッドアップ通知として表示されます。

-   **可視性**&ndash;通知コンテンツの量が、ロック画面に通知が表示されたときに表示されることを指定します。

-   **カテゴリ**&ndash;をする場合など、デバイスのさまざまな状況で通知を処理する方法をシステムに通知*不可*モード。

**注:** **可視性**と**カテゴリ**Android 5.0 および Android の以前のバージョンでは使用できないで導入されました。 以降、Android の 8.0 で[通知チャネル](#notif-chan)を使用して、ユーザーに通知を表示する方法を制御します。


### <a name="expanded-layouts"></a>展開されたレイアウト

Android 4.1 以降では、通知は、ユーザーがより多くのコンテンツを表示する通知の高さを展開できるように拡張レイアウト スタイルを使用して構成できます。 たとえば、次の例は、契約したモードで、展開されたレイアウト通知を示しています。

![契約の通知](local-notifications-images/07-contracted-notification.png)

この通知が展開されている場合は、メッセージ全体を表示します。

![展開された通知](local-notifications-images/08-expanded-notification.png)

Android では、単一のイベント通知の 3 つの展開されたレイアウト スタイルをサポートしています。

-   ***大きなテキスト***&ndash;契約モードは、2 個のピリオドの後に、メッセージの最初の行の抜粋が表示されます。 展開されたモードでは、(上記の例で表示) されてメッセージ全体を表示します。

-   ***受信トレイ***&ndash;契約したモードでは、新しいメッセージの数が表示されます。 展開されたモードでは、最初の電子メール メッセージまたは受信トレイ内のメッセージの一覧を表示します。

-   ***イメージ***&ndash;契約モードは、メッセージのテキストのみが表示されます。 展開されたモードで、テキストとイメージを表示します。

[Basic Notification を超えて](#beyond-the-basic-notification)を作成する方法について説明します (この記事で後述)*大きなテキスト*、*受信トレイ*、および*イメージ*通知します。


## <a name="notification-creation"></a>通知の作成

使用する Android の通知を作成するには[Notification.Builder](https://developer.xamarin.com/api/type/Android.App.Notification+Builder/)クラスです。 `Notification.Builder` 通知オブジェクトの作成を簡略化 Android 3.0 で導入されました。 Android の古いバージョンと互換性がある通知を作成するには、使用することができます、 [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)クラスの代わりに`Notification.Builder`(を参照してください[互換性](#compatibility)は、このトピックの後半詳細についてを使用して`NotificationCompat.Builder`)。
`Notification.Builder` 通知など、さまざまなオプションを設定するための方法があります。

-   タイトル、メッセージ テキストと、通知アイコンを含む内容。

-   通知のスタイルなど*大きなテキスト*、*受信トレイ*、または*イメージ*スタイル。

-   通知の優先順位: 最小値、低、デフォルト、高値、または最大です。

-   ロック画面の通知の可視性: パブリック、プライベート、またはシークレット。

-   Android に役立つカテゴリ メタデータは、分類し、通知をフィルターします。

-   通知をタップすると、起動するためのアクティビティを示す省略可能な目的です。

ビルダーでこれらのオプションを設定した後は、設定を含む通知オブジェクトを生成します。 通知を発行するには、するには、この通知オブジェクトを渡します、*通知マネージャー*です。 Android の提供、 [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)クラスは、通知を発行し、ユーザーに表示することを担当します。 このクラスへの参照は、アクティビティやサービスなど、任意のコンテキストから取得できます。


### <a name="how-to-generate-a-notification"></a>通知を生成する方法

Android の通知を生成するには、次の手順を実行します。

1.  インスタンスを作成、`Notification.Builder`オブジェクト。

2.  さまざまなメソッドを呼び出して、`Notification.Builder`通知オプションを設定するオブジェクト。

3.  呼び出す、[ビルド](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/)のメソッド、`Notification.Builder`通知オブジェクトをインスタンス化するオブジェクト。

4.  呼び出す、[通知](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification))通知をパブリッシュする通知マネージャーのメソッドです。

通知ごとに、次の情報を少なくとも指定する必要があります。

-   小さいアイコン (24 x 24 サイズ dp)

-   短いタイトル

-   通知のテキスト

次のコード例は、使用する方法を示しています。`Notification.Builder`基本的な通知を生成します。 注意して`Notification.Builder`メソッドがサポート[メソッド チェーン](http://en.wikipedia.org/wiki/Method_chaining); は、各メソッドはビルダー オブジェクトを返しますの最後のメソッド呼び出しの結果を使用するには次のメソッド呼び出しを起動するようにします。

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

この例では、新しい`Notification.Builder`と呼ばれるオブジェクト`builder`がインスタンス化すると、タイトルと通知のテキストを設定しから通知アイコンが読み込まれます**Resources/drawable/ic_notification.png**です。 通知 builder への呼び出し`Build`メソッドは、これらの設定で通知オブジェクトを作成します。 次の手順を呼び出すには、`Notify`通知マネージャーのメソッドです。 通知マネージャーを見つけるを呼び出す`GetSystemService`、上記のようにします。

`Notify`メソッドは 2 つのパラメーターを受け取ります。 通知 id と、通知オブジェクト。 通知識別子は、アプリケーションに通知を識別する一意の整数です。 この例では、通知 id が (0); を 0 に設定します。ただし、実稼働アプリケーションで通知ごとに一意の識別子を提供するされます。 呼び出しで以前の識別子の値を再利用して`Notify`を上書きする最後の通知を発生します。

Android 5.0 デバイスでこのコードを実行すると、次の例のような通知が生成されます。

![サンプル コードについての通知結果](local-notifications-images/09-hello-world.png)

通知アイコンが通知の左側に表示&ndash;、丸型のこのイメージ&ldquo;すれば&rdquo;Android がその背後にある灰色円形の背景を描画できるようにアルファ チャネルがします。 アルファ チャネルなし アイコンを指定することもできます。 表示するには、写真イメージ、アイコンとして、次を参照してください。[大判アイコン](#large-icon-format)このトピックで後述します。

タイムスタンプが自動的に設定がこの設定をオーバーライドするには呼び出すことによって、[プールデフォルトプロセッサ](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/)通知ビルダーのメソッドです。 たとえば、次のコード例では、現在の時刻にタイムスタンプを設定します。

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>サウンドと振動を有効にします。

通知 builder を呼び出すことができます、通知もサウンドを再生する場合は、 [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/)メソッドを渡します、`NotificationDefaults.Sound`フラグ。

```csharp
// Instantiate the notification builder and enable sound:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

この呼び出しを`SetDefaults`通知が発行されると、サウンドを再生するデバイスになります。 デバイスにサウンドを再生するのではなく、振動させられる場合は、渡す`NotificationDefaults.Vibrate`に`SetDefaults.`に両方のフラグを渡すことができます、デバイスをサウンドを再生し、デバイスを振動させられる場合は、 `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

サウンドを再生するサウンドを指定しないでを有効にする場合、Android は既定のシステム通知のサウンドを使用します。 ただし、通知 builder を呼び出すことによって再生されるサウンドを変更することができます[SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/)メソッドです。 たとえば、アラームを再生する (既定の通知の音)、代わりに、通知でサウンドすることができますの URI を取得、アラームからサウンド、 [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/)に渡すと`SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

または、使用できますシステム既定の着信音サウンド、通知。

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

通知オブジェクトの通知プロパティを設定することが通知オブジェクトを作成した後 (そのから事前に構成するのではなく`Notification.Builder`メソッド)。 など、呼び出す代わりに、`SetDefaults`振動、通知を有効にする方法の通知のビット フラグは、直接変更[既定値は](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/)プロパティ。

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

この例では、デバイスを振動、通知が発行されるとします。

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>通知の更新

公開された後に、通知の内容を更新する場合は、既存の再利用できる`Notification.Builder`新しい通知オブジェクトを作成し、最後の通知の識別子では、この通知をパブリッシュするオブジェクト。 例えば:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

この例で、既存の`Notification.Builder`オブジェクトを使用して、別のタイトルとメッセージの新しい通知オブジェクトを作成します。
以前の通知の識別子を使用して、新しい通知オブジェクトをパブリッシュし、公開されていた通知のコンテンツを更新します。

![更新された通知](local-notifications-images/12-updated-notification.png)

前回の通知の本文が再利用&ndash;タイトルのみおよび通知の変更通知ドロアーに通知が表示されているときのテキスト。 タイトルのテキストが「サンプル通知」から「通知の更新」に変更され、メッセージのテキスト"Hello World からの変更! これは、最初の通知です。" 「このメッセージに変更します」を。

次の 3 つのいずれかが発生するまで、通知が表示されたまま。

-   ユーザーが通知を閉じる (がタップまたは*すべてクリア*)。

-   アプリケーションへの呼び出しは、`NotificationManager.Cancel`通知が発行されたときに割り当てられた一意の通知 ID を渡して、します。

-   アプリケーションの呼び出し`NotificationManager.CancelAll`です。

Android の通知の更新に関する詳細は、次を参照してください。[変更通知](http://developer.android.com/training/notify-user/managing.html#Updating)です。


### <a name="starting-an-activity-from-a-notification"></a>通知から、アクティビティの開始

Android では、通知に関連付けられる一般的な*アクション*&ndash;ユーザーが通知をタップしたときに起動されるアクティビティ。 このアクティビティは、別のアプリケーションまたは別のタスクであってもかまいません。 作成する通知にアクションを追加するには[PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)オブジェクトし、関連付ける、`PendingIntent`通知します。 A`PendingIntent`により、送信元アプリケーションのアクセス許可を持つ定義済みのコードを実行する、受信者のアプリケーション インテントの特殊な型です。 ユーザーが通知をタップしたときに Android が起動で指定されたアクティビティ、`PendingIntent`です。

次のコード スニペットを含む通知を作成する方法を示しています、`PendingIntent`発信元のアプリのアクティビティが起動される`MainActivity`:

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

このコードは点を除いて、前のセクションで、通知コードによく似ています、`PendingIntent`通知オブジェクトに追加されます。 この例では、`PendingIntent`通知のビルダーに渡される前に、元のアプリのアクティビティに関連付けられて[SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/)メソッドです。 `PendingIntentFlags.OneShot`にフラグが渡される、`PendingIntent.GetActivity`メソッドできるように、`PendingIntent`が一度だけ使用します。 このコードを実行すると、次の通知が表示されます。

![最初のアクション通知](local-notifications-images/10-first-action-notification.png)

ユーザーを元の活動は、この通知をタップします。

実稼働アプリケーションでは、アプリを処理する必要があります、*バック スタック*押されたとき、**戻る**通知アクティビティ内でのボタン (Android のタスクと、バック スタックに慣れていない場合を参照してください[タスクとバック スタック](http://developer.android.com/guide/components/tasks-and-back-stack.html))。
ほとんどの場合は、通知アクティビティから後方移動するホーム画面に戻ると、アプリからユーザーが返す必要があります。 バック スタックをアプリが使用を管理する、 [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/)クラスを作成する、`PendingIntent`バック スタックにします。

別の実際の考慮事項は、元の活動は、通知アクティビティにデータを送信する必要があります。 たとえば、通知は、テキスト メッセージが到着しましたし、通知アクティビティ (画面を表示するメッセージ)、ユーザーにメッセージを表示するメッセージの ID を要求することを可能性があります。 作成するアクティビティ、`PendingIntent`使用できる、 [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/)目的にデータ (文字列など) を追加して、このデータは、通知アクティビティに渡されるようにするメソッド。

次のコード サンプルは、使用する方法を示しています`TaskStackBuilder`バック スタック、およびそれを管理するという通知アクティビティを 1 つのメッセージ文字列を送信する方法の例が含まれています。 `SecondActivity`:。

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

このコード例では、アプリから成る 2 つのアクティビティ: `MainActivity` (これが含まれている上記の通知コード) と`SecondActivity`画面の通知をタップした後はユーザーに表示されます。 このコードの実行時に (前の例に似ています) という単純な通知が表示されます。 通知をタップするは、ユーザーを`SecondActivity`画面。

![2 つ目のアクティビティのスクリーン ショット](local-notifications-images/11-second-activity.png)

文字列メッセージ (目的に渡された`PutExtra`メソッド) で取得`SecondActivity`のコード行で。

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

この取得したメッセージ「Greetings から MainActivity!、」が表示される、`SecondActivity`画面で、上記のスクリーン ショットに示すようにします。 押されたとき、**戻る**の中にボタン`SecondActivity`、ナビゲーション リード アプリ アウトし、アプリの起動の前の画面に戻ります。

保留中のインテント作成の詳細については、次を参照してください。 [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)です。


<a name="notif-chan" />

## <a name="notification-channels"></a>通知チャネル

Android 8.0 (Oreo) 以降では、使用して、*通知チャネル*を表示することを示す通知の種類ごとにユーザーがカスタマイズできるチャネルを作成する機能。 通知チャネル可能になりますのグループの通知動作は同じチャネル展示にポストされたすべての通知できるようにします。 たとえば、早急に措置が必要な通知のためのものでは、通知チャネルと情報メッセージに使用される「静かな」個別のチャネルがあります。

**YouTube** Android Oreo にインストールされているアプリに 2 つの通知のカテゴリを一覧表示:**ダウンロードの通知**と**全般通知**:

[![Android Oreo で YouTube の通知の画面](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

これらの各カテゴリは、通知チャネルに対応します。 YouTube アプリを実装して、**ダウンロードの通知**チャネルと**全般通知**チャネル。 ユーザーがタップできる**ダウンロードの通知**アプリのダウンロード通知チャネルの設定 画面が表示されます。

[![YouTube アプリを画面で通知をダウンロードします。](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

ユーザーはこの画面での動作を変更すること、**ダウンロード**通知チャネルの次の手順で。

-   重要度のレベルを設定**緊急**、**高**、**中**、または**低**、サウンド、および visual の中断のレベルが構成されます。

-   通知のドットをオンまたはオフにします。

-   点滅のライトをオンまたはオフにします。

-   表示またはロック画面で通知を非表示にします。

-   上書き、**不可**設定します。

**全般通知**チャネルと同様の設定には。

[![YouTube アプリの通知の [全般] 画面](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

ユーザーと、通知チャネルが対話する方法を完全に制御がないことに注意してください&ndash;上記のスクリーン ショットに示すように、ユーザーがデバイス上のすべての通知チャネルの設定を変更できます。 ただし、(以下に記載します)、既定値を構成することができます。 これらの例に示すよう、新しい通知チャネル機能できるようになりますさまざまな種類の通知に対して、きめ細かい制御をユーザーに付与します。

アプリに通知チャネルのサポートを追加する必要がありますか。 Android 8.0 では、アプリを対象としている場合*必要があります*通知チャネルを実装します。
Oreo デバイスで通知を表示する通知チャネルを使用せず、ユーザーにローカル通知を送信しようとする Oreo の対象となるアプリが失敗します。 Android 8.0 を対象にしない場合とアプリは引き続き動作通知の動作は同じですが、Android の 8.0 で Android 7.1 またはそれ以前を実行している場合が発生します。


### <a name="creating-a-notification-channel"></a>通知チャネルを作成します。

通知チャネルを作成するには、次の操作を行います。

1. 構築、 [NotificationChannel](https://developer.android.com/reference/android/app/NotificationChannel.html)を次のオブジェクト。

    - パッケージ内で一意の ID 文字列です。 次の例では、文字列`com.xamarin.myapp.urgent`を使用します。

    - チャネル (40 文字未満) のユーザーに表示される名前。 次の例では、名前**緊急**を使用します。

    - 中断する方法の通知を制御すると、チャネルの重要性に送信される、`NotificationChannel`です。 重要度を指定できます`Default`、 `High`、 `Low`、 `Max`、 `Min`、 `None`、または`Unspecified`です。

    これらの値を渡す、[コンス トラクター](https://developer.android.com/reference/android/app/NotificationChannel.html#NotificationChannel%28java.lang.String,%20java.lang.CharSequence,%20int%29) (この例では`Resource.String.noti_chan_urgent`に設定されている**緊急**)。

    ```csharp
    public const string URGENT_CHANNEL = "com.xamarin.myapp.urgent";
    . . .
    string chanName = GetString (Resource.String.noti_chan_urgent);
    var importance = NotificationImportance.High;
    NotificationChannel chan =
       new NotificationChannel (URGENT_CHANNEL, chanName, importance);
    ```

2.  構成、`NotificationChannel`初期設定を持つオブジェクト。
    たとえば、次のコードの構成、`NotificationChannel`オブジェクトのこのチャネルにポストされた通知がデバイスを振動させられるし、既定では、ロック画面に表示できるようにします。

    ```csharp
    chan.EnableVibration (true);
    chan.LockscreenVisibility = NotificationVisibility.Public;
    ```

3.  通知マネージャーに通知チャネル オブジェクトを送信するには。

    ```csharp
    NotificationManager notificationManager =
        (NotificationManager) GetSystemService (NotificationService);
    notificationManager.CreateNotificationChannel (chan);
    ```


### <a name="posting-to-a-notifications-channel"></a>通知チャネルに投稿します。

通知チャネルへの通知を投稿するには、次の操作を行います。

1.  使用して通知を構成、 `Notification.Builder`、チャネル ID を渡して、`SetChannelId`メソッドです。 例えば:

    ```csharp
    Notification.Builder builder = new Notification.Builder (this)
        .SetContentTitle ("Attention!")
        .SetContentText ("This is an urgent notification message!")
        .SetChannelId (URGENT_CHANNEL);
    ```

2.  ビルドおよび通知マネージャーを使用して通知を発行する[通知](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/p/System.Int32/Android.App.Notification/)メソッド。

    ```csharp
    const int notificationId = 0;
    notificationManager.Notify (notificationId, builder.Build());
    ```

情報メッセージの別の通知チャネルを作成するのには、上記の手順を繰り返すことができます。 この 2 番目のチャネルことで、既定では、振動を無効にする、既定のロック画面の表示に設定`Private`に通知の重要度を設定および`Default`です。

完全なコード例は Android Oreo 通知チャネルのアクションで、次を参照してください。、 [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels)サンプルです。 このサンプル アプリは、2 つのチャネルを管理し、その他の通知オプションを設定します。



<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>Basic Notification を超える

Android での単純な標準ベースのレイアウト形式に既定で通知が追加することでこの基本的な書式を強化できます`Notification.Builder`メソッドの呼び出しです。 このセクションで、通知に大きな写真アイコンを追加する方法を学習および展開されたレイアウト通知を作成する方法の例が表示されます。

<a name="large-icon-format" />

### <a name="large-icon-format"></a>大きいアイコン

通常、android の通知は、(上の通知の左側にある) 元のアプリのアイコンを表示します。 画像または写真ただし、通知を表示できます (、*大きいアイコン*) 標準の小さいアイコンの代わりにします。 たとえば、メッセージング アプリケーションは、アプリ アイコンではなく、送信者の写真を表示する可能性があります。

基本的な Android 5.0 通知の例を次に示します&ndash;小さなアプリ アイコンのみが表示されます。

![例正常通知](local-notifications-images/13-sample-notification.png)

大きいアイコンを表示する変更を加えた後、通知のスクリーン ショットは、次&ndash;Xamarin コード サルのイメージから作成されたアイコンを使用します。

![大きいアイコン通知の例](local-notifications-images/14-large-icon-sample.png)

大きいアイコンで通知が表示されたらに、小さなアプリ アイコンが大きいアイコンの右下隅にバッジとして表示されていることを確認します。

通知の大きいアイコンとしてイメージを使用するには通知ビルダーを呼び出す[SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/)メソッドとイメージのビットマップを渡します。 異なり`SetSmallIcon`、`SetLargeIcon`のみビットマップを受け入れます。 使用するビットマップにイメージ ファイルを変換する、 [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/)クラスです。 例えば:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

このコード例は、あるイメージ ファイルを開き**Resources/drawable/monkey_icon.png**、ビットマップに変換し、結果のビットマップを渡します`Notification.Builder`です。 通常、ソース イメージの解像度は小さいアイコンよりも大きい&ndash;は大きくありません。 大きすぎてイメージには、通知の送信が遅れをサイズ変更操作が不要な可能性があります。
Android での通知アイコンのサイズの詳細についてを参照してください。[通知アイコン](http://developer.android.com/design/style/iconography.html#notification)です。


### <a name="big-text-style"></a>大きなテキストのスタイル

*大きなテキスト*スタイルは、通知に長いメッセージを表示するために使用する展開のレイアウト テンプレート。 すべての拡張されたレイアウト通知と同様に大きなテキスト通知が最初にコンパクトに表現形式で表示されます。

![大きなテキスト通知の例](local-notifications-images/15-big-text-notification.png)

この形式では、メッセージの抜粋が表示になり、2 つのピリオドで終了されます。 通知方法ユーザーがドラッグしたとき、全体の通知メッセージを表示するために展開します。

![展開された大きなテキスト通知](local-notifications-images/16-big-text-expanded.png)

この拡張されたレイアウト形式には、通知の下部にある概要テキストも含まれています。 最大の高さ、*大きなテキスト*通知は、256 dp です。

作成する、*大きなテキスト*インスタンス化、通知、`Notification.Builder`前とに、、オブジェクトのインスタンスを作成し、追加、 [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/)オブジェクトを`Notification.Builder`オブジェクト。 例えば:

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

この例に、メッセージ テキストおよび概要のテキストを格納します、`BigTextStyle`オブジェクト (`textStyle`) に渡される前に。 `Notification.Builder.`


### <a name="image-style"></a>画像のスタイル

*イメージ*スタイル (とも呼ばれる、*全体像*スタイル) は、イメージを表示する、通知の本文で使用できる展開の通知の形式。 スクリーン ショット アプリまたはフォト アプリを使用できるなど、*イメージ*キャプチャされた最後の通知をユーザーに提供するスタイルのイメージを通知します。 なおの最大の高さ、*イメージ*通知は、256 dp &ndash; Android が利用可能なメモリの制限内で、この制限の最大の高さに収まるようにイメージのサイズを変更します。

すべてのような展開レイアウト通知*イメージ*通知は最初に付随するメッセージのテキストの抜粋を表示できるコンパクトな形式で表示されます。

![Compact のイメージの通知にイメージが表示されません。](local-notifications-images/17-image-compact.png)

ときに、ユーザーがドラッグ、*イメージ*イメージを表示するために、通知を展開します。 たとえば、前回の通知の拡張されたバージョンを次に示します。

![展開されたイメージの通知が表示の画像](local-notifications-images/18-image-expanded.png)

コンパクトな形式で、通知が表示されたら、そのテキストに表示される通知の通知 (通知のビルダーに渡されるテキスト`SetContentText`メソッド、前に示したよう)。 ただし、イメージを表示するために、通知を展開すると、イメージの上の概要のテキストが表示されます。

作成する、*イメージ*インスタンス化、通知、`Notification.Builder`以前と同様、オブジェクトを作成し、挿入、 [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/)オブジェクトに、`Notification.Builder`オブジェクト。 例えば:

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

同様に、`SetLargeIcon`メソッドの`Notification.Builder`、[となる総合的な](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/)メソッドの`BigPictureStyle`通知の本文に表示するイメージのビットマップが必要です。 この例では、 [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32))メソッドの`BitmapFactory`にあるイメージ ファイルの読み取り**Resources/drawable/x_bldg.png**ビットマップに変換します。

リソースとしてパッケージ化されませんイメージを表示することもできます。 たとえば、次のサンプル コードのローカルの SD カードからイメージを読み込みで表示、*イメージ*通知。

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

この例では、イメージ ファイルにある **/sdcard/Pictures/my-tshirt.jpg**は読み込まれ、元のサイズの半分にサイズ変更、通知で使用するビットマップに変換されます。

![通知で例 t シャツの画像](local-notifications-images/19-tshirt-notification.png)

呼び出しをラップすることをお勧めはかわからない場合、イメージ ファイルのサイズを事前に、 [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/)例外ハンドラーで&ndash;、`OutOfMemoryError`のイメージが大きすぎる場合は、例外がスローされますAndroid のサイズを変更します。

詳細については、読み込みと大きいビットマップ イメージをデコードすることは、次を参照してください。[ロード大きなビットマップ効率的に](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently)です。


### <a name="inbox-style"></a>受信トレイのスタイル

*受信トレイ*形式は、展開されたレイアウト テンプレート通知の本文に、電子メールの受信トレイ集計) などのテキストの別々 の行を表示するためのものです。 *受信トレイ*コンパクトな形式で書式設定通知が最初に表示されます。

![例のコンパクトな受信トレイ通知](local-notifications-images/20-inbox-compact.png)

通知方法ユーザーがドラッグしたときは、次のスクリーン ショットに示すように電子メールの概要を表示するために展開します。

![展開の例の受信トレイ通知](local-notifications-images/21-inbox-expanded.png)

作成する、*受信トレイ*インスタンス化、通知、`Notification.Builder`前とに、、オブジェクトを追加、 [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/)オブジェクトを`Notification.Builder`です。 例えば:

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

通知の本文には、新しい行のテキストを追加するには、呼び出し、[を](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/)のメソッド、`InboxStyle`オブジェクト (の最大の高さ、*受信トレイ*通知は、256 dp)。 なおとは異なり*大きなテキスト*スタイル、*受信トレイ*スタイルは、通知の本文で個々 の行のテキストをサポートしています。

使用することも、*受信トレイ*展開された形式で個々 の行のテキストを表示する必要がある任意の通知のスタイル。 たとえば、*受信トレイ*通知スタイルは、概要の通知に複数の保留通知を結合に使用することができます&ndash;、1 つを更新する*受信トレイ*new で通知のスタイルを設定通知の内容の行 (を参照してください[通知の更新](#updating-a-notification)上) ではなく、ほとんどの場合と同様、新しい通知の連続するストリームを生成します。 この方法の詳細については、次を参照してください。 [、通知を要約](http://developer.android.com/design/patterns/notifications.html#summarize_your_notifications)です。


## <a name="configuring-metadata"></a>メタデータの構成

`Notification.Builder` 呼び出すことができる優先順位、可視性、およびカテゴリなど、通知に関するメタデータを設定する方法があります。 この情報を使用する android&mdash;ユーザー設定と共に&mdash;方法とタイミングを決定する通知を表示します。


### <a name="priority-settings"></a>優先順位の設定

通知の優先度設定では、通知が発行されると、2 つの結果が決定します。

-   ここでその他の通知に関連して、通知が表示されます。
    たとえば、優先度の高い通知が表示通知ドロワーの低い優先度の通知の上に関係なく各通知の発行時にされます。

-   かどうか、通知は、ヘッドアップ通知の形式 (Android 5.0 以降) に表示されます。 のみ*高*と*最大*ヘッドアップ通知として優先順位の通知が表示されます。

Xamarin.Android には、通知の優先順位を設定するための次の列挙が定義されています。

-   `NotificationPriority.Max` &ndash; (たとえば、着信呼び出し、によって - 道案内、または緊急警告) 緊急または重要な条件をユーザーに警告します。 Android 5.0 およびそれ以降のデバイスで最大の優先順位の通知はヘッドアップ形式で表示されます。

-   `NotificationPriority.High` &ndash; (重要な電子メールまたはリアルタイムのチャット メッセージの到着の場合) などの重要なイベントのユーザーに通知します。 Android 5.0 およびそれ以降のデバイスでは、優先度の高い通知はヘッドアップ形式で表示されます。

-   `NotificationPriority.Default` &ndash; 重要度のレベルが中程度の条件のユーザーに通知します。

-   `NotificationPriority.Low` &ndash; 不急については、ユーザーが (たとえば、ソフトウェア更新プログラムの通知やソーシャル ネットワークの更新プログラム) の形式を解除する必要があること。

-   `NotificationPriority.Min` &ndash; 背景情報については、ユーザーがされる場合にのみに通知する (場所や気象情報など) の通知を表示します。

通知の優先順位を設定するには、呼び出し、 [SetPriority](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/)のメソッド、`Notification.Builder`優先度レベルを渡して、オブジェクトです。 例えば:

```csharp
builder.SetPriority (NotificationPriority.High);
```

次の例、優先度の高い通知、「重要なメッセージです。」 通知ドロワーの上部に表示されます。

![例 - 優先度の高い通知](local-notifications-images/22-hi-priority-drawer.png)

これは、優先度の高い通知であるため、Android 5.0 では、ユーザーの現在のアクティビティ画面上部ヘッドアップ通知としても表示されます。

![例ヘッドアップ通知](local-notifications-images/23-heads-up-example.png)

次の例では、[優先順位の高いバッテリ レベル通知] で、優先度の低い「の日と考える」通知が表示されます。

![例 - 優先度の低い通知](local-notifications-images/24-lo-priority-drawer.png)

「考え、その日の」通知は、優先度の低い通知であるため、Android は、形式で表示されませんヘッドアップです。


### <a name="visibility-settings"></a>可視性の設定

Android 5.0 から、*可視性*通知コンテンツの量がセキュリティで保護されたロック画面の表示を制御する設定があります。
Xamarin.Android には、通知の可視性の設定の次の列挙が定義されています。

-   `NotificationVisibility.Public` &ndash; 通知の完全な内容は、セキュリティで保護されたロック画面に表示されます。

-   `NotificationVisibility.Private` &ndash; (通知アイコンやなどが掲載されているアプリの名前)、セキュリティで保護されたロック画面で重要な情報のみが表示されますが、通知の詳細の残りの部分が非表示になります。 既定ですべての通知`NotificationVisibility.Private`です。

-   `NotificationVisibility.Secret` &ndash; セキュリティで保護された、ロック、通知アイコンも表示されない画面に何も表示されます。 通知の内容は、ユーザー、デバイスのロックを解除した後でのみ使用できます。

通知、アプリの呼び出しの可視性を設定、`SetVisibility`のメソッド、`Notification.Builder`表示設定を渡して、オブジェクトです。 たとえば、この呼び出しを`SetVisibility`通知は、 `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

ときに、`Private`通知がポストされる、セキュリティで保護されたロック画面で、名前と、アプリのアイコンが表示されます。 通知メッセージではなくユーザーには、「この通知を表示するデバイス ロック解除」が表示されます。

![デバイスの通知メッセージのロックを解除します。](local-notifications-images/25-lockscreen-private.png)

この例では**NotificationsLab**発信元のアプリの名前を指定します。 通知のこの墨消しバージョンは、ロック画面がセキュリティで保護された場合にのみが表示されます (つまり、暗証番号 (pin)、パターン、またはパスワードを使用して保護する)&ndash;通知の完全な内容は、ロック画面で使用できる、ロック画面が安全でない場合。


### <a name="category-settings"></a>カテゴリの設定

Android 5.0 以降では、定義済みのカテゴリは順位付けと通知をフィルター処理できます。 Xamarin.Android には、これらのカテゴリの次の列挙子が用意されています。

-   `Notification.CategoryCall` &ndash; かかってきた電話します。

-   `Notification.CategoryMessage` &ndash; 入力方向のテキスト メッセージです。

-   `Notification.CategoryAlarm` &ndash; アラーム条件またはタイマー期限。

-   `Notification.CategoryEmail` &ndash; 受信電子メール メッセージ。

-   `Notification.CategoryEvent` &ndash; カレンダー イベントです。

-   `Notification.CategoryPromo` &ndash; キャンペーンのメッセージまたは提供情報。

-   `Notification.CategoryProgress` &ndash; バック グラウンド操作の進行状況。

-   `Notification.CategorySocial` &ndash; ソーシャル ネットワー キング更新します。

-   `Notification.CategoryError` &ndash; バック グラウンド操作または認証プロセスのエラーです。

-   `Notification.CategoryTransport` &ndash; メディアの再生更新します。

-   `Notification.CategorySystem` &ndash; システムで使用する (システムまたはデバイスの状態) の予約されています。

-   `Notification.CategoryService` &ndash; バック グラウンド サービスが実行されていることを示します。

-   `Notification.CategoryRecommendation` &ndash; 現在実行中のアプリに関連する推奨設定のメッセージ。

-   `Notification.CategoryStatus` &ndash; デバイスに関する情報です。

通知は、並べ替えられたときに、通知の優先度が、そのカテゴリの設定よりも優先されます。 たとえば、優先度の高い通知はヘッドアップとして表示するのに属している場合でも、`Promo`カテゴリ。 呼び出すの通知のカテゴリを設定するには`SetCategory`のメソッド、`Notification.Builder`カテゴリの設定を渡して、オブジェクトです。 例えば:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

*不可*(Android 5.0 の新機能) の機能カテゴリに基づく通知をフィルター処理します。 たとえば、*不可*画面で**設定**電話の通話とメッセージの通知を除外するユーザーを許可。

![画面のスイッチは不可します。](local-notifications-images/26-do-not-disturb.png)

ユーザーの構成と*不可*(上のスクリーン ショットに示すよう) に電話を除くすべての割り込みをブロックするには、Android により、通知のカテゴリの設定を`Notification.CategoryCall`中、デバイスに表示します。*不可*モード。 なお`Notification.CategoryAlarm`で通知がブロックされることはありません*不可*モード。


<a name="compatibility" />

## <a name="compatibility"></a>互換性

アプリを作成する場合は (できるだけ早く API レベル 4)、Android の以前のバージョンで実行しても使用する、 [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)クラスの代わりに`Notification.Builder`です。 使用して通知を作成するとき`NotificationCompat.Builder`、Android 古いデバイスの基本的な通知の内容が正しく表示されていることを確認します。 ただし、高度な通知の一部の機能が Android の以前のバージョンで利用できないため、コード必要があります明示的に処理して展開された通知のスタイル、カテゴリ、および可視性のレベル以下に示すように、互換性の問題。

使用する`NotificationCompat.Builder`、アプリにする必要があります、 [Android のサポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)プロジェクトの NuGet です。

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

重要な通知のオプションのメソッド呼び出しがのと同じでこの例のように、`Notification.Builder`です。 ただし、(次のセクションで説明) より複雑な通知の下位互換性の問題を処理するコードがあります。

[LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications)サンプルを使用する方法を示します`NotificationCompat.Builder`通知から 2 番目のアクティビティを起動します。 このサンプル コードは」で説明されて、 [Xamarin.Android でのローカルの通知を使用して](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)チュートリアルです。


### <a name="notification-styles"></a>通知のスタイル

作成する*大きなテキスト*、*イメージ*、または*受信トレイ*による通知をスタイル`NotificationCompat.Builder`アプリは、これらのスタイルの互換性バージョンを使用する必要があります。 たとえば、使用するため、*大きなテキスト*スタイルをインスタンス化`NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

同様に、アプリケーションで使用できる`NotificationCompat.InboxStyle`と`NotificationCompat.BigPictureStyle`の*受信トレイ*と*イメージ*スタイル、それぞれします。


### <a name="notification-priority-and-category"></a>通知の優先度とカテゴリ

`NotificationCompat.Builder` サポートしている、`SetPriority`メソッド (以降で使用可能 Android 4.1)。 ただし、`SetCategory`メソッドは*いない*でサポートされている`NotificationCompat.Builder`カテゴリ Android 5.0 で導入された新しい通知のメタデータ システムの一部であるためです。

Android、場所の古いバージョンをサポートするために`SetCategory`が利用できない、コードは実行時に、条件付きで呼び出し API レベルを確認できます`SetCategory`API レベルが Android 5.0 (API レベル 21) 以上の場合。

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

この例では、アプリの**ターゲット フレームワーク**Android 5.0 に設定されていると、**最低限の Android バージョン**に設定されている**Android 4.1 (API レベル 16)** です。 `SetCategory`は API レベル 21 以降で使用でき、このコード例を呼び出す`SetCategory`が場合にのみ、使用可能な&ndash;を呼び出さない`SetCategory`API レベルが場合より小さい
21.


### <a name="lockscreen-visibility"></a>ロック画面の可視性

アプリが Android 5.0 (API レベル 21) 前に、のロック画面通知をサポートしていないため`NotificationCompat.Builder`はサポートしていません、`SetVisibility`メソッドです。 上で説明したよう`SetCategory`、コードは、確認ランタイムと呼び出し API レベル`SetVisiblity`が場合にのみ、使用できます。

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetVisibility (Notification.Public);
}
```


## <a name="summary"></a>まとめ

この記事では、Android でローカルの通知を作成する方法について説明します。 通知の詳細が説明されている、使用する方法が説明されている`Notification.Builder`通知を作成する 大きいアイコン、スタイルの通知方法*大きなテキスト*、*イメージ*と*受信トレイ*形式、優先度、可視性、およびカテゴリなどのメタデータの設定の通知を設定する方法および通知からアクティビティを起動する方法です。 この記事の内容が新しいヘッドアップ、ロック画面のこれらの通知設定を使用する方法を説明しても、*不可*Android 5.0 で導入された機能です。 最後に、使用する方法を学習しました`NotificationCompat.Builder`Android の旧バージョンと通知の互換性を維持します。

Android 用の設計の通知についてのガイドラインについては、次を参照してください。[通知](http://developer.android.com/preview/notifications.html)です。


## <a name="related-links"></a>関連リンク

- [NotificationsLab (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (サンプル)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Android のチュートリアルでのローカルの通知](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [通知](http://developer.android.com/design/patterns/notifications.html)
- [ユーザーに通知します。](http://developer.android.com/training/notify-user/index.html)
- [通知](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
