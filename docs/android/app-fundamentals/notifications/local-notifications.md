---
title: Android でのローカル通知
description: このセクションでは、Xamarin. Android でローカル通知を実装する方法について説明します。 Android の通知のさまざまな UI 要素について説明し、通知の作成と表示に関連する API について説明します。
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/16/2018
ms.openlocfilehash: f56e02a89474ebf5f89800b1f96318db7965159d
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91456302"
---
# <a name="local-notifications-on-android"></a>Android でのローカル通知

_このセクションでは、Xamarin. Android でローカル通知を実装する方法について説明します。Android の通知のさまざまな UI 要素について説明し、通知の作成と表示に関連する API について説明します。_

## <a name="local-notifications-overview"></a>ローカル通知の概要

Android には、ユーザーに通知アイコンと通知情報を表示するための、システムによって管理される2つの領域が用意されています。 通知が最初に発行されると、次のスクリーンショットに示すように、通知 *領域*にアイコンが表示されます。

![デバイスの通知領域の例](local-notifications-images/01-notification-shade.png)

通知の詳細を取得するために、ユーザーは通知ドロワーを開き (各通知アイコンを展開して通知コンテンツを表示)、通知に関連付けられているアクションを実行できます。 次のスクリーンショットは、上に表示されている通知領域に対応する *通知ドロワー* を示しています。

[![3つの通知を表示している通知ドロワーの例](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Android の通知では、次の2種類のレイアウトを使用します。

- ***基本レイアウト*** &ndash; コンパクトで固定のプレゼンテーション形式。

- ***展開***されたレイアウト &ndash; より大きなサイズに拡張して詳細情報を表示できるプレゼンテーション形式。

これらの各レイアウトの種類 (およびそれらを作成する方法) については、次のセクションで説明します。

> [!NOTE]
> このガイドでは、 [Android サポートライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)の[notificationcompat api](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html)に焦点を当てています。 これらの Api によって、Android 4.0 (API レベル 14) との下位互換性が最大になります。

### <a name="base-layout"></a>基本レイアウト

すべての Android 通知は基本レイアウト形式に基づいて構築されます。これには、少なくとも次の要素が含まれます。

1. *通知アイコン*。アプリがさまざまな種類の通知をサポートしている場合、発信元アプリまたは通知の種類を表します。

2. 通知の *タイトル*。通知が個人のメッセージの場合は、送信者の名前。

3. 通知メッセージ。

4. *タイムスタンプ*。

次の図に示すように、これらの要素が表示されます。

[![通知要素の場所](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

基本レイアウトの高さは、64の密度に依存しないピクセル (dp) に制限されています。 Android では、この基本通知スタイルが既定で作成されます。

必要に応じて、通知には、アプリケーションまたは送信者の写真を表す大きいアイコンを表示できます。 Android 5.0 以降の通知で大きいアイコンが使用されている場合、小さい通知アイコンは、大きいアイコンのバッジとして表示されます。

![単純な通知写真](local-notifications-images/04-simple-notification-photo.png)

Android 5.0 以降では、通知はロック画面にも表示されます。

[![ロック画面の通知例](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

ユーザーは、ロック画面の通知をダブルタップしてデバイスのロックを解除し、その通知を開始したアプリに移動するか、スワイプして通知を破棄することができます。 アプリでは、ロック画面に表示される内容を制御する通知の表示レベルを設定できます。また、ユーザーは、機密性の高いコンテンツをロック画面の通知に表示するかどうかを選択できます。

Android 5.0 では、 *ヘッドアップ*と呼ばれる優先順位の高い通知プレゼンテーション形式が導入されました。 ヘッドアップ通知は、画面の上部から数秒後にスライドダウンし、通知領域に戻るようになります。

[![ヘッドアップ通知の例](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

ヘッドアップ通知を使用すると、システム UI は、現在実行中のアクティビティの状態を中断することなく、ユーザーの前に重要な情報を格納できます。

Android では、通知の並べ替えと表示をインテリジェントに行えるように、通知メタデータのサポートが追加されています。 通知メタデータは、ロック画面およびヘッドアップ形式で通知を表示する方法も制御します。 アプリケーションでは、次の種類の通知メタデータを設定できます。

- **優先順位** &ndash; 通知を表示する方法とタイミングは、優先度レベルによって決まります。 たとえば、Android 5.0 では、優先度の高い通知はヘッドアップ通知として表示されます。

- **可視性** &ndash; 通知がロック画面に表示されたときに表示される通知コンテンツの量を指定します。

- **カテゴリ** &ndash; デバイスが " *応答不可* " モードの場合など、さまざまな状況で通知を処理する方法をシステムに通知します。

> [!NOTE]
> **可視性** と **カテゴリ** は、android 5.0 で導入されたものであり、以前のバージョンの android では使用できません。 Android 8.0 以降では、 [通知チャネル](#notif-chan) を使用して、ユーザーに通知を表示する方法を制御します。

### <a name="expanded-layouts"></a>展開されたレイアウト

Android 4.1 以降では、展開されたレイアウトスタイルを使用して通知を構成し、ユーザーがより多くのコンテンツを表示できるように通知の高さを広げることができます。 たとえば、次の例は、コントラクトモードでの展開されたレイアウト通知を示しています。

![契約の通知](local-notifications-images/07-contracted-notification.png)

この通知が展開されると、メッセージ全体が表示されます。

![拡張された通知](local-notifications-images/08-expanded-notification.png)

Android では、単一イベント通知用に展開された3つのレイアウトスタイルがサポートされています。

- ***大きいテキスト*** &ndash; [契約] モードでは、メッセージの最初の行の抜粋の後に2つの期間が表示されます。 展開モードでは、メッセージ全体が表示されます (上の例を参照)。

- ***受信トレイ*** &ndash; [契約] モードでは、新しいメッセージの数が表示されます。 展開モードでは、最初の電子メールメッセージ、または受信トレイ内のメッセージの一覧が表示されます。

- ***イメージ*** &ndash; [契約] モードでは、メッセージテキストのみが表示されます。 展開モードでは、テキストとイメージが表示されます。

[基本の通知以外](#beyond-the-basic-notification) (この記事の後半) では、 *テキスト*、 *受信トレイ*、および *イメージ* の通知を作成する方法について説明します。

<a name="notif-chan"></a>
<a name="notification-channels"></a>
## <a name="notification-channels"></a>通知チャネル

Android 8.0 (Oreo) 以降では、 *通知チャネル* 機能を使用して、表示する通知の種類ごとにユーザーがカスタマイズ可能なチャネルを作成できます。 通知チャネルを使用すると、チャネルにポストされたすべての通知の動作が同じになるように、通知をグループ化することができます。 たとえば、すぐに対処する必要がある通知用の通知チャネル、および情報メッセージに使用される別の "静かな" チャネルがあるとします。

Android Oreo と共にインストールされる **YouTube** アプリには、次の2つの通知カテゴリ ( **ダウンロード通知** と **一般的な通知**) が一覧表示されます。

[![Android Oreo の YouTube の通知画面](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

これらの各カテゴリは、通知チャネルに対応します。 YouTube アプリでは、 **ダウンロード通知**  チャネルと **一般通知** チャネルが実装されています。 ユーザーは [ **ダウンロード通知**] をタップすると、アプリのダウンロード通知チャネルの [設定] 画面が表示されます。

[![YouTube アプリの [ダウンロード通知] 画面](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

この画面では、ユーザーは次の手順に従って、 **ダウンロード** 通知チャネルの動作を変更できます。

- [重要度] レベルを [ **緊急**]、[ **高**]、[ **中**]、または [   **低**] に設定します。これにより、サウンドとビジュアルの中断のレベルが構成されます。

- 通知ドットをオンまたはオフにします。

- 点滅する光をオンまたはオフにします。

- ロック画面で通知を表示または非表示にします。

- [ **応答不可** ] 設定をオーバーライドします。

**一般的な通知**チャネルには、次のような設定があります。

[![YouTube アプリの全般通知画面](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

通知チャネルがユーザーとどのように対話するかを絶対に制御できないことに注意してください &ndash; 。上のスクリーンショットに示されているように、ユーザーはデバイスで通知チャネルの設定を変更できます。 ただし、次に示すように、既定値を構成することができます。 これらの例で示すように、新しい通知チャネル機能を使用すると、ユーザーがさまざまな種類の通知をきめ細かく制御できるようになります。

## <a name="notification-creation"></a>通知の作成

Android で通知を作成するには、 [Xamarin. Android.](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)の NuGet パッケージの[notificationcompat ビルダー](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder)クラスを使用します。 このクラスを使用すると、以前のバージョンの Android で通知を作成して発行することができます。
`NotificationCompat.Builder` についても説明します。

`NotificationCompat.Builder` 通知のさまざまなオプションを設定するためのメソッドを提供します。次に例を示します。

- コンテンツ (タイトル、メッセージテキスト、通知アイコンなど)。

- *ビッグテキスト*、*受信トレイ*、*画像*のスタイルなど、通知のスタイル。

- 通知の優先度。 [最小]、[低]、[既定]、[高]、または [最大] を設定します。 Android 8.0 以降では、 [_通知チャネル_](#notification-channels)を使用して優先度が設定されます。

- ロック画面での通知の可視性 (パブリック、プライベート、またはシークレット)。

- Android が通知を分類してフィルター処理するのに役立つカテゴリメタデータ。

- 通知がタップされたときに起動するアクティビティを示す、省略可能なインテント。

- 通知が発行される通知チャネルの ID (Android 8.0 以降)。

ビルダーでこれらのオプションを設定すると、設定を含む通知オブジェクトが生成されます。 通知を発行するには、この通知オブジェクトを *通知マネージャー*に渡します。 Android には、 [Notificationmanager](xref:Android.App.NotificationManager) クラスが用意されています。このクラスは、通知を発行してユーザーに表示します。 このクラスへの参照は、アクティビティやサービスなど、任意のコンテキストから取得できます。

### <a name="creating-a-notification-channel"></a>通知チャネルの作成

Android 8.0 で実行されているアプリでは、通知用の通知チャネルを作成する必要があります。 通知チャネルには、次の3つの情報が必要です。

- チャネルを識別するパッケージに固有の ID 文字列。
- ユーザーに表示されるチャネルの名前。  名前は 1 ~ 40 文字にする必要があります。
- チャネルの重要度。

アプリは、実行されている Android のバージョンを確認する必要があります。
Android 8.0 よりも前のバージョンを実行しているデバイスでは、通知チャネルを作成できません。 次のメソッドは、アクティビティで通知チャネルを作成する方法の一例です。

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var channelName = Resources.GetString(Resource.String.channel_name);
    var channelDescription = GetString(Resource.String.channel_description);
    var channel = new NotificationChannel(CHANNEL_ID, channelName, NotificationImportance.Default)
                  {
                      Description = channelDescription
                  };

    var notificationManager = (NotificationManager) GetSystemService(NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

通知チャネルは、アクティビティが作成されるたびに作成される必要があります。 メソッドの場合は `CreateNotificationChannel` 、アクティビティのメソッドで呼び出す必要があり `OnCreate` ます。

### <a name="creating-and-publishing-a-notification"></a>通知の作成と公開

Android で通知を生成するには、次の手順を実行します。

1. `NotificationCompat.Builder` オブジェクトをインスタンス化します。

2. オブジェクトのさまざまなメソッドを呼び出して、 `NotificationCompat.Builder` 通知オプションを設定します。

3. オブジェクトの [ビルド](xref:Android.App.Notification.Builder.Build) メソッドを呼び出して、 `NotificationCompat.Builder` 通知オブジェクトをインスタンス化します。

4. 通知マネージャーの [Notify](xref:Android.App.NotificationManager.Notify*) メソッドを呼び出して、通知を発行します。

通知ごとに、少なくとも次の情報を指定する必要があります。

- 小さいアイコン (24 x 24 dp のサイズ)

- 短いタイトル

- 通知のテキスト

次のコード例は、を使用して基本通知を生成する方法を示してい `NotificationCompat.Builder` ます。 メソッドで `NotificationCompat.Builder` は [メソッドチェーン](https://en.wikipedia.org/wiki/Method_chaining)がサポートされていることに注意してください。つまり、各メソッドはビルダーオブジェクトを返します。これにより、最後のメソッド呼び出しの結果を使用して次のメソッド呼び出しを呼び出すことができます。

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
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

この例で `NotificationCompat.Builder` は、という新しいオブジェクト `builder` が、使用される通知チャネルの ID と共にインスタンス化されます。 通知のタイトルとテキストが設定され、通知アイコンが **Resources/合成/ic_notification.png**から読み込まれます。 通知ビルダーのメソッドを呼び出すと、 `Build` これらの設定を使用して通知オブジェクトが作成されます。 次の手順では、 `Notify` 通知マネージャーのメソッドを呼び出します。 通知マネージャーを検索するには、上記のように `GetSystemService` を呼び出します。

メソッドは、 `Notify` 通知 id と通知オブジェクトという2つのパラメーターを受け取ります。 通知識別子は、アプリケーションへの通知を識別する一意の整数です。 この例では、通知識別子がゼロ (0) に設定されています。ただし、実稼働アプリケーションでは、各通知に一意の識別子を付けることをお勧めします。 の呼び出しで前の識別子の値を再利用する `Notify` と、最後の通知が上書きされます。

このコードを Android 5.0 デバイスで実行すると、次の例のような通知が生成されます。

![サンプルコードの通知結果](local-notifications-images/09-hello-world.png)

通知アイコンは、通知の左側に表示され &ndash; ます。この画像には、円の &ldquo; 背後にグレーの円形の &rdquo; 背景を描画できるようにアルファチャネルがあります。 また、アルファチャネルなしでアイコンを指定することもできます。 写真画像をアイコンとして表示するには、このトピックで後述する「 [大きいアイコン形式](#large-icon-format) 」を参照してください。

タイムスタンプは自動的に設定されますが、通知ビルダーの [Setwhen](xref:Android.App.Notification.Builder.SetWhen*) メソッドを呼び出すことにより、この設定をオーバーライドできます。 たとえば、次のコード例では、タイムスタンプを現在の時刻に設定しています。

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>サウンドと振動の有効化

通知でもサウンドを再生する場合は、通知ビルダーの [Setdefaults](xref:Android.App.Notification.Builder.SetDefaults*) メソッドを呼び出して、フラグを渡すことができ `NotificationDefaults.Sound` ます。

```csharp
// Instantiate the notification builder and enable sound:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

この呼び出しを行うと、 `SetDefaults` 通知が発行されるときにデバイスがサウンドを再生します。 サウンドを再生するのではなく、デバイスをバイブレーションにする場合は、にを渡すことができます。デバイスで `NotificationDefaults.Vibrate` `SetDefaults.` サウンドを再生し、デバイスをバイブレーションするには、両方のフラグをに渡すことができ `SetDefaults` ます。

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

再生するサウンドを指定せずにサウンドを有効にすると、Android では既定のシステム通知音が使用されます。 ただし、通知ビルダーの [setsound](xref:Android.App.Notification.Builder.SetSound*) メソッドを呼び出すことによって再生されるサウンドを変更することができます。 たとえば、(既定の通知音ではなく) 通知でアラームサウンドを再生するには、 [RingtoneManager](xref:Android.Media.RingtoneManager) からアラーム音の URI を取得して、次のように渡します `SetSound` 。

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

または、通知に対してシステムの既定の着信音音を使用することもできます。

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

通知オブジェクトを作成した後は、通知オブジェクトの通知プロパティを設定できます (メソッドを使用して事前に構成するのではなく `NotificationCompat.Builder` )。 たとえば、 `SetDefaults` 通知で振動を有効にするためにメソッドを呼び出す代わりに、通知の [Defaults](xref:Android.App.Notification.Defaults) プロパティのビットフラグを直接変更できます。

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

この例では、通知が発行されるときにデバイスのバイブレーションが発生します。

### <a name="updating-a-notification"></a>通知を更新しています

発行後に通知の内容を更新する場合は、既存のオブジェクトを再利用して `NotificationCompat.Builder` 新しい通知オブジェクトを作成し、最後の通知の識別子を使用してこの通知を発行することができます。 次に例を示します。

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

この例では、既存のオブジェクトを使用して、 `NotificationCompat.Builder` 別のタイトルとメッセージを含む新しい通知オブジェクトを作成します。
新しい通知オブジェクトは、前の通知の識別子を使用して公開されます。これにより、以前に発行された通知の内容が更新されます。

![更新された通知](local-notifications-images/12-updated-notification.png)

前の通知の本文はタイトルだけを再利用し、通知が通知された &ndash; ときに通知のテキストが変更されます。 タイトルのテキストが "サンプル通知" から "更新通知" に変更され、メッセージテキストが "Hello World! これは最初の通知です。 " "このメッセージに変更しました。"

通知は、次の3つのうちのいずれかが発生するまで表示されたままになります。

- ユーザーが通知を破棄する (または *クリアオール*をタップする)。

- アプリケーションは、 `NotificationManager.Cancel` 通知が発行されたときに割り当てられた一意の通知 ID を渡して、への呼び出しを行います。

- アプリケーションが `NotificationManager.CancelAll` を呼び出します。

Android の通知を更新する方法の詳細については、「 [通知を変更する](https://developer.android.com/training/notify-user/managing.html#Updating)」を参照してください。

### <a name="starting-an-activity-from-a-notification"></a>通知からのアクティビティの開始

Android では、通知は、 *action* &ndash; ユーザーが通知をタップしたときに起動されるアクティビティに関連付けられるのが一般的です。 このアクティビティは、別のアプリケーションまたは別のタスクに配置できます。 通知にアクションを追加するには、 [Pendingintent](xref:Android.App.PendingIntent) オブジェクトを作成し、を `PendingIntent` 通知に関連付けます。 は、 `PendingIntent` 受信者アプリケーションが送信側アプリケーションのアクセス許可を持つ定義済みのコードを実行できるようにする特殊なインテントです。 ユーザーが通知をタップすると、Android はによって指定されたアクティビティを開始し `PendingIntent` ます。

次のコードスニペットは、を使用して通知を作成し、 `PendingIntent` 元のアプリのアクティビティを起動する方法を示してい `MainActivity` ます。

```csharp
// Set up an intent so that tapping the notifications returns to this app:
Intent intent = new Intent (this, typeof(MainActivity));

// Create a PendingIntent; we're only using one PendingIntent (ID = 0):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    PendingIntent.GetActivity (this, pendingIntentId, intent, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including pending intent:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
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

このコードは、が通知オブジェクトに追加される点を除いて、前のセクションの通知コードによく似てい `PendingIntent` ます。 この例では、は、 `PendingIntent` 通知ビルダーの [Setcontentintent](xref:Android.App.Notification.Builder.SetContentIntent*) メソッドに渡される前に、元のアプリのアクティビティに関連付けられています。 `PendingIntentFlags.OneShot`フラグは `PendingIntent.GetActivity` 、 `PendingIntent` が1回だけ使用されるようにメソッドに渡されます。 このコードを実行すると、次の通知が表示されます。

![最初のアクション通知](local-notifications-images/10-first-action-notification.png)

この通知をタップすると、ユーザーは元のアクティビティに戻ります。

運用アプリでは、ユーザーが通知アクティビティ内で [**戻る**] ボタンを押したときに、アプリケーションが*バックスタック*を処理する必要があります (Android タスクとバックスタックに慣れていない場合は、「[タスクとバックスタック](https://developer.android.com/guide/components/tasks-and-back-stack.html)」を参照してください)。
ほとんどの場合、通知アクティビティからさかのぼって移動すると、アプリからユーザーが返され、ホーム画面に戻ります。 バックスタックを管理するために、アプリは [Taskstackbuilder](xref:Android.App.TaskStackBuilder) クラスを使用し `PendingIntent` て、バックスタックを持つを作成します。

もう1つの実際の考慮事項は、元のアクティビティが通知アクティビティにデータを送信する必要があることです。 たとえば、通知はテキストメッセージが届いたことを示し、通知アクティビティ (メッセージ表示画面) では、メッセージをユーザーに表示するためにメッセージの ID が必要です。 を作成するアクティビティは、 `PendingIntent` インテントを使用してデータ (文字列など) をインテントに追加することで、このデータが通知アクティビティに渡されるようにすることができ[ます。](xref:Android.Content.Intent.PutExtra*)

次のコードサンプルは、を使用してバックスタックを管理する方法を示してい `TaskStackBuilder` ます。これには、という通知アクティビティに1つのメッセージ文字列を送信する方法の例が含まれてい `SecondActivity` ます。

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
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
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

このコード例では、アプリは `MainActivity` (上の通知コードを含む) 2 つのアクティビティと `SecondActivity` 、通知をタップした後にユーザーに表示される画面で構成されています。 このコードを実行すると、単純な通知 (前の例に似ています) が表示されます。 通知をタップすると、ユーザーが画面に移動し `SecondActivity` ます。

![2番目のアクティビティのスクリーンショット](local-notifications-images/11-second-activity.png)

(インテントのメソッドに渡された) 文字列メッセージ `PutExtra` は、 `SecondActivity` 次のコード行を使用してで取得します。

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

上記のスクリーンショットに示されているように、この取得したメッセージ (MainActivity! からの応答) が画面に表示され `SecondActivity` ます。 の間にユーザーが [ **戻る** ] ボタンを押すと、アプリの `SecondActivity` 外部に移動し、アプリの起動前の画面に戻ります。

保留中のインテントの作成の詳細については、「 [Pendingintent](xref:Android.App.PendingIntent)」を参照してください。

<a name="beyond-the-basic-notification"></a>

## <a name="beyond-the-basic-notification"></a>基本通知以外

通知は Android の単純な基本レイアウト形式に既定で設定されていますが、追加のメソッド呼び出しを行うことで、この基本形式を拡張でき `NotificationCompat.Builder` ます。 このセクションでは、大きな写真アイコンを通知に追加する方法について説明し、展開されたレイアウト通知を作成する方法の例を示します。

<a name="large-icon-format"></a>

### <a name="large-icon-format"></a>大きいアイコン形式

通常、Android の通知には、(通知の左側にある) 元のアプリのアイコンが表示されます。 ただし、通知では、標準の小さいアイコンの代わりに画像や写真 ( *大きいアイコン*) を表示できます。 たとえば、メッセージングアプリでは、アプリアイコンではなく、送信者の写真を表示できます。

基本的な Android 5.0 通知の例を次に示し &ndash; ます。小さいアプリアイコンのみが表示されます。

![通常の通知の例](local-notifications-images/13-sample-notification.png)

次に示すのは、サイズの大きいアイコンを表示するように変更した後の通知のスクリーンショットです &ndash; 。これは、Xamarin のコードのサルのイメージから作成されたアイコンを使用します。

![大きいアイコン通知の例](local-notifications-images/14-large-icon-sample.png)

通知が大きいアイコン形式で表示されると、小さいアプリのアイコンが大きいアイコンの右下隅にバッジとして表示されることに注意してください。

画像を通知で大きいアイコンとして使用するには、通知ビルダーの [SetLargeIcon](xref:Android.App.Notification.Builder.SetLargeIcon*) メソッドを呼び出し、イメージのビットマップを渡します。 とは異なり `SetSmallIcon` 、は `SetLargeIcon` ビットマップのみを受け入れます。 イメージファイルをビットマップに変換するには、 [Bitmapfactory](xref:Android.Graphics.BitmapFactory) クラスを使用します。 次に例を示します。

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

このコード例では、イメージ、描画、 **または monkey_icon.png**でイメージファイルを開き、ビットマップに変換して、結果のビットマップをに渡し `NotificationCompat.Builder` ます。 通常、ソースイメージの解像度は小さいアイコンよりも大きくなりますが、それほど &ndash; 大きくはありません。 画像が大きすぎると、通知の投稿を遅らせてしまう可能性のある、不要なサイズ変更操作が発生する可能性があります。

### <a name="big-text-style"></a>ビッグテキストスタイル

*ビッグテキスト*スタイルは、通知に長いメッセージを表示するために使用する、展開されたレイアウトテンプレートです。 展開されたすべてのレイアウト通知と同様に、ビッグテキスト通知は最初にコンパクトなプレゼンテーション形式で表示されます。

![ビッグテキスト通知の例](local-notifications-images/15-big-text-notification.png)

この形式では、メッセージの抜粋のみが2つの期間で終了します。 ユーザーが通知を下にドラッグすると、通知メッセージ全体が表示されます。

![拡張されたビッグテキスト通知](local-notifications-images/16-big-text-expanded.png)

この展開されたレイアウト形式には、通知の下部に概要テキストも含まれています。 *大きなテキスト*通知の最大の高さは 256 dp です。

*ビッグテキスト*通知を作成するには、前と同様にオブジェクトをインスタンス化して `NotificationCompat.Builder` から、オブジェクトに[bigtextstyle](xref:Android.App.Notification.BigTextStyle)オブジェクトをインスタンス化して追加し `NotificationCompat.Builder` ます。 たとえば次のようになります。

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

この例では、に渡される前に、メッセージテキストと概要テキストがオブジェクト () に格納されています `BigTextStyle` `textStyle` 。 `NotificationCompat.Builder.`

### <a name="image-style"></a>画像のスタイル

*画像*のスタイル (*大きな画像*のスタイルとも呼ばれます) は、通知の本文に画像を表示するために使用できる、拡張された通知形式です。 たとえば、スクリーンショットアプリや写真アプリでは、 *イメージ* 通知スタイルを使用して、キャプチャされた最後のイメージの通知をユーザーに提供できます。 *イメージ*通知の最大の高さは256であることに注意してください。 dp Android では、 &ndash; 使用可能なメモリの制限内で、この最大の高さの制限に収まるようにイメージのサイズが変更されます。

展開されたすべてのレイアウト通知と同様に、 *イメージ* 通知は最初にコンパクト形式で表示され、付随するメッセージテキストの抜粋が表示されます。

![イメージが表示されないことを示すイメージの通知](local-notifications-images/17-image-compact.png)

ユーザーが *イメージ* の通知を下にドラッグすると、画像が表示されます。 たとえば、前の通知の拡張バージョンを次に示します。

![展開されたイメージの通知画像を目にする](local-notifications-images/18-image-expanded.png)

通知がコンパクト形式で表示されると、通知テキスト (前述のように通知ビルダーのメソッドに渡されるテキスト) が表示され `SetContentText` ます。 ただし、通知を展開して画像を表示すると、画像の上に概要テキストが表示されます。

*イメージ*通知を作成するには、 `NotificationCompat.Builder` 前と同じようにオブジェクトをインスタンス化してから、オブジェクトに[big画像スタイル](xref:Android.App.Notification.BigPictureStyle)オブジェクトを作成して挿入し `NotificationCompat.Builder` ます。 次に例を示します。

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

`SetLargeIcon`のメソッドと同様 `NotificationCompat.Builder` に、の[bigpicture](xref:Android.App.Notification.BigPictureStyle.BigPicture*)メソッドでは、 `BigPictureStyle` 通知の本文に表示するイメージのビットマップが必要です。 この例では、の [DecodeResource](xref:Android.Graphics.BitmapFactory.DecodeResource*) メソッドは、 `BitmapFactory` **Resources//x_bldg.png** にあるイメージファイルを読み取り、それをビットマップに変換します。

リソースとしてパッケージ化されていないイメージを表示することもできます。 たとえば、次のサンプルコードは、ローカル SD カードからイメージを読み込み、 *イメージ* 通知に表示します。

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

この例では、 **/sdcard/Pictures/my-tshirt.jpg** にあるイメージファイルが読み込まれ、元のサイズの半分にサイズ変更された後、通知で使用するビットマップに変換されます。

![通知での T シャツ画像の例](local-notifications-images/19-tshirt-notification.png)

イメージファイルのサイズが事前にわからない場合は、例外ハンドラーで DecodeFile への呼び出しをラップすることをお勧めします [。](xref:Android.Graphics.BitmapFactory.DecodeFile*) これは &ndash; `OutOfMemoryError` 、画像が Android のサイズ変更に対して大きすぎる場合に例外がスローされる可能性があるためです。

大きなビットマップイメージの読み込みとデコードの詳細については、「大きなビットマップを [効率的に読み込む](https://github.com/xamarin/recipes/tree/master/Recipes/android/resources/general/load_large_bitmaps_efficiently)」を参照してください。

### <a name="inbox-style"></a>受信トレイのスタイル

*受信トレイ*の形式は、通知の本文に別のテキスト行 (電子メールの受信トレイの概要など) を表示するための展開されたレイアウトテンプレートです。 *受信トレイ*形式の通知は、まずコンパクトな形式で表示されます。

![コンパクトな受信トレイ通知の例](local-notifications-images/20-inbox-compact.png)

ユーザーが通知を下にドラッグすると、次のスクリーンショットに示すように展開され、電子メールの概要が表示されます。

![展開された受信トレイ通知の例](local-notifications-images/21-inbox-expanded.png)

*受信トレイ*通知を作成するには、 `NotificationCompat.Builder` 前と同様にオブジェクトをインスタンス化し、に Inbox[スタイル](xref:Android.App.Notification.InboxStyle)オブジェクトを追加し `NotificationCompat.Builder` ます。 たとえば次のようになります。

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

通知本文に新しいテキスト行を追加するには、オブジェクトの [Addline](xref:Android.App.Notification.InboxStyle.AddLine*) メソッドを呼び出し `InboxStyle` ます ( *受信トレイ* 通知の最大の高さは 256 dp です)。 *ビッグテキスト*スタイルとは異なり、*受信トレイ*スタイルでは、通知本文内の個々のテキスト行がサポートされることに注意してください。

また、拡張された形式で個々のテキスト行を表示する必要があるすべての通知に対して、 *受信トレイ* スタイルを使用することもできます。 たとえば、 *受信トレイ* の通知スタイルを使用して、複数の保留中の通知をまとめて概要通知を作成できます &ndash; 。1つの *受信トレイ* スタイルの通知を新しい通知コンテンツの行で更新できます (上記の「 [通知の更新](#updating-a-notification) 」を参照してください)。これは、ほぼ同様の新しい通知の連続ストリームを生成することです。

## <a name="configuring-metadata"></a>メタデータの構成

`NotificationCompat.Builder` には、優先度、可視性、カテゴリなど、通知に関するメタデータを設定するために呼び出すことができるメソッドが用意されています。 Android では、この情報をユーザー設定と共に使用して &mdash; &mdash; 、通知を表示する方法とタイミングを決定します。

### <a name="priority-settings"></a>優先順位の設定

Android 7.1 以降で実行されているアプリでは、通知自体に優先順位を直接設定する必要があります。 通知が発行されると、通知の優先度設定によって2つの結果が決定されます。

- 通知は、他の通知に関連して表示されます。
    たとえば、優先度の高い通知は、各通知が公開されたタイミングに関係なく、通知ドロワーの優先度の低い通知の上に表示されます。

- 通知をヘッドアップ通知形式 (Android 5.0 以降) で表示するかどうかを指定します。 優先度が *高* および *最大* の通知だけが、ヘッドアップ通知として表示されます。

Xamarin Android では、通知の優先度を設定するために次の列挙が定義されています。

- `NotificationPriority.Max`&ndash;緊急時または重大な状態 (着信呼び出し、ターン順、緊急通知など) に対してユーザーに警告します。 Android 5.0 以降のデバイスでは、最大優先順位通知は、ヘッドアップ形式で表示されます。

- `NotificationPriority.High`重要な &ndash; イベント (重要なメールやリアルタイムチャットメッセージの到着など) をユーザーに通知します。 Android 5.0 以降のデバイスでは、優先度の高い通知がヘッドアップ形式で表示されます。

- `NotificationPriority.Default`&ndash;中レベルの重要度を持つ条件をユーザーに通知します。

- `NotificationPriority.Low`&ndash;ソフトウェア更新プログラムの通知やソーシャルネットワークの更新プログラムなど、ユーザーに通知する必要がある非緊急情報。

- `NotificationPriority.Min`&ndash;通知を表示する場合にのみユーザーに通知される背景情報 (場所や天気情報など)。

通知の優先度を設定するには、オブジェクトの [Setpriority](xref:Android.App.Notification.Builder.SetPriority*) メソッドを呼び出して `NotificationCompat.Builder` 、優先度レベルで渡します。 次に例を示します。

```csharp
builder.SetPriority (NotificationPriority.High);
```

次の例では、優先度の高い通知である "重要なメッセージ!" 通知ドロワーの上部に表示されます。

![高優先度通知の例](local-notifications-images/22-hi-priority-drawer.png)

これは優先度の高い通知であるため、Android 5.0 のユーザーの現在のアクティビティ画面の上に、ヘッドアップ通知としても表示されます。

![ヘッドアップ通知の例](local-notifications-images/23-heads-up-example.png)

次の例では、低優先度の "日に考えられる" という通知が、優先度の高いバッテリレベルの通知に表示されます。

![低優先度の通知の例](local-notifications-images/24-lo-priority-drawer.png)

"1 日のうちに考えられる" 通知は優先度の低い通知であるため、Android ではヘッドアップ形式で表示されません。

> [!NOTE]
> Android 8.0 以降では、通知チャネルとユーザー設定の優先度によって通知の優先度が決まります。

### <a name="visibility-settings"></a>可視性の設定

Android 5.0 以降では、[ *可視性* ] 設定を使用して、セキュリティで保護されたロック画面に表示される通知コンテンツの量を制御できます。
Xamarin Android では、通知の可視性を設定するために次の列挙が定義されています。

- `NotificationVisibility.Public`&ndash;通知の完全な内容が、セキュリティで保護されたロック画面に表示されます。

- `NotificationVisibility.Private`&ndash;セキュリティで保護されたロック画面 (通知アイコンや、通知を送信したアプリの名前など) には重要な情報のみが表示されますが、通知の詳細の残りは非表示になります。 すべての通知の既定値は `NotificationVisibility.Private` です。

- `NotificationVisibility.Secret`&ndash;通知アイコンだけでなく、セキュリティで保護されたロック画面には何も表示されません。 通知コンテンツは、ユーザーがデバイスのロックを解除した後にのみ使用できます。

通知の表示を設定するために、アプリは `SetVisibility` オブジェクトのメソッドを呼び出し `NotificationCompat.Builder` 、可視性の設定を渡します。 たとえば、この呼び出しに `SetVisibility` よって通知が行われ `Private` ます。

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

`Private`通知が投稿されると、セキュリティで保護されたロック画面にアプリの名前とアイコンのみが表示されます。 ユーザーは、通知メッセージの代わりに、"この通知を表示するためにデバイスのロックを解除しています" と表示されます。

![デバイスの通知メッセージのロック解除](local-notifications-images/25-lockscreen-private.png)

この例では、 **Notificationslab** は、発信元アプリの名前です。 この最終バージョンの通知は、ロック画面がセキュリティで保護されていない場合 (つまり、PIN、パターン、またはパスワードを使用してセキュリティで保護されている場合) にのみ表示されます。ロック画面では、 &ndash; 通知の完全な内容を利用できます。

### <a name="category-settings"></a>カテゴリの設定

Android 5.0 以降では、通知の順位付けとフィルター処理に定義済みのカテゴリを使用できます。 Xamarin Android では、これらのカテゴリに対して次の列挙が提供されます。

- `Notification.CategoryCall`&ndash;着信通話。

- `Notification.CategoryMessage`&ndash;テキストメッセージを受信します。

- `Notification.CategoryAlarm`&ndash;アラーム条件またはタイマーの有効期限。

- `Notification.CategoryEmail`&ndash;受信電子メールメッセージ。

- `Notification.CategoryEvent`&ndash;カレンダーイベント。

- `Notification.CategoryPromo`&ndash;プロモーションメッセージまたは広告。

- `Notification.CategoryProgress`&ndash;バックグラウンド操作の進行状況。

- `Notification.CategorySocial`&ndash;ソーシャルネットワークの更新。

- `Notification.CategoryError`&ndash;バックグラウンド操作または認証プロセスの失敗。

- `Notification.CategoryTransport`&ndash;メディア再生の更新。

- `Notification.CategorySystem`&ndash;システム使用 (システムまたはデバイスの状態) 用に予約されています。

- `Notification.CategoryService`&ndash;バックグラウンドサービスが実行されていることを示します。

- `Notification.CategoryRecommendation`&ndash;現在実行中のアプリに関連する推奨メッセージ。

- `Notification.CategoryStatus`&ndash;デバイスに関する情報。

通知が並べ替えられると、通知の優先度はそのカテゴリの設定よりも優先されます。 たとえば、優先度の高い通知は、カテゴリに属している場合でも、ヘッドアップとして表示され `Promo` ます。 通知のカテゴリを設定するには、オブジェクトのメソッドを呼び出して、 `SetCategory` `NotificationCompat.Builder` カテゴリの設定を渡します。 次に例を示します。

```csharp
builder.SetCategory (Notification.CategoryCall);
```

" *応答不可* " 機能 (Android 5.0 の新機能) は、カテゴリに基づいて通知をフィルター処理します。 たとえば、[**設定**] の [*応答不可*] 画面では、ユーザーは通話とメッセージの通知を除外できます。

![スクリーンスイッチに応答しない](local-notifications-images/26-do-not-disturb.png)

ユーザーが電話以外のすべての割り込みをブロックするように設定 *され* ている場合 (上のスクリーンショットを例として)、Android で `Notification.CategoryCall` は、デバイスが [ *応答不可* ] モードのときに、カテゴリ設定がの通知を表示できます。 `Notification.CategoryAlarm`通知は、[*応答不可*モードではブロックされないことに注意してください。

[Localnotifications](/samples/xamarin/monodroid-samples/localnotifications)サンプルでは、を使用して、 `NotificationCompat.Builder` 通知から2番目のアクティビティを起動する方法を示します。 このサンプルコードについては、 [「Xamarin. Android でのローカル通知の使用](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) 」チュートリアルで説明しています。

### <a name="notification-styles"></a>通知スタイル

で *テキスト*、 *画像*、または *受信トレイ* のスタイルに関する通知を作成するには `NotificationCompat.Builder` 、アプリでこれらのスタイルの互換性バージョンを使用する必要があります。 たとえば、 *ビッグテキスト* スタイルを使用するには、次のようにインスタンス化し `NotificationCompat.BigTextstyle` ます。

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

同様に、アプリではとを使用して、 `NotificationCompat.InboxStyle` `NotificationCompat.BigPictureStyle` *受信トレイ* と *画像* のスタイルをそれぞれ使用できます。

### <a name="notification-priority-and-category"></a>通知の優先度とカテゴリ

`NotificationCompat.Builder``SetPriority`(Android 4.1 以降で使用可能) メソッドをサポートしています。 ただし、では、 `SetCategory` Android 5.0 で導入された新しい通知メタデータシステムにカテゴリが含まれているため、このメソッドはサポートされて *いません* `NotificationCompat.Builder` 。

以前のバージョンの Android (を使用できない) をサポートするには、 `SetCategory` `SetCategory` api レベルが Android 5.0 (api レベル 21) 以上であるときに、実行時に api レベルを条件付きで呼び出すことができます。

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

この例では、アプリの **ターゲットフレームワーク** は android 5.0 に設定されており、 **Android の最小バージョン** は **android 4.1 (API レベル 16)** に設定されています。 `SetCategory`は api レベル21以降で使用可能であるため、このコード例は、 `SetCategory` 使用可能な場合にのみを呼び出します &ndash; `SetCategory` 。 api レベルが21より小さい場合には呼び出されません。

### <a name="lock-screen-visibility"></a>ロック画面の表示

Android では、Android 5.0 (API レベル 21) より前のロック画面通知をサポートしていなかったため、 `NotificationCompat.Builder` はメソッドをサポートしていません `SetVisibility` 。 について前述したように `SetCategory` 、コードは実行時に API レベルを確認し、 `SetVisiblity` 使用可能な場合にのみを呼び出すことができます。

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop) {
    builder.SetVisibility (Notification.Public);
}
```

## <a name="summary"></a>まとめ

この記事では、Android でローカル通知を作成する方法について説明しました。 ここでは、通知の構造について説明し、を使用して通知を作成する方法 `NotificationCompat.Builder` 、大きいアイコンで通知をスタイル設定する方法、 *大きなテキスト*、 *画像* と *受信トレイ* の形式、通知メタデータの設定 (優先度、可視性、カテゴリなど)、通知からアクティビティを起動する方法について説明しました。 この記事では、これらの通知設定が、Android 5.0 で導入された新しいヘッドアップ、ロック画面、および *応答不可* 機能とどのように連携するかについても説明しました。 最後に、を使用し `NotificationCompat.Builder` て、以前のバージョンの Android との通知の互換性を維持する方法を学習しました。

Android 用の通知の設計に関するガイドラインについては、「 [通知](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)」を参照してください。

## <a name="related-links"></a>関連リンク

- [NotificationsLab (サンプル)](/samples/xamarin/monodroid-samples/android50-notificationslab)
- [LocalNotifications (サンプル)](/samples/xamarin/monodroid-samples/localnotifications)
- [Android でのローカル通知のチュートリアル](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [ユーザーへの通知](https://developer.android.com/training/notify-user/index.html)
- [通知](xref:Android.App.Notification)
- [NotificationManager](xref:Android.App.NotificationManager)
- [NotificationCompat。ビルダー](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](xref:Android.App.PendingIntent)