---
title: チュートリアル - Xamarin.Android でローカル通知の使用
description: このチュートリアルでは、Xamarin.Android アプリケーションでローカル通知を使用する方法を示します。 作成して、ローカル通知の発行の基礎を示します。 ユーザーには、通知領域に通知がクリックすると、2 番目のアクティビティを開始します。
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/16/2018
ms.openlocfilehash: a2ca3755e3201263584447ba47ec36d2096386da
ms.sourcegitcommit: f9fb316344fc4dce2fdce0dde3c5f4c2e4a42d08
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/30/2018
ms.locfileid: "30766583"
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>チュートリアル - Xamarin.Android でローカル通知の使用

_このチュートリアルでは、Xamarin.Android アプリケーションでローカル通知を使用する方法を示します。作成して、ローカル通知の発行の基礎を示します。ユーザーには、通知領域に通知がクリックすると、2 番目のアクティビティを開始します。_


## <a name="overview"></a>概要

このチュートリアルでは、Android アプリケーションをユーザーがアクティビティ内のボタンをクリックしたときに通知を発生させることは作成します。 ユーザーには、通知がクリックすると、最初のアクティビティでボタンをクリックした回数の合計を表示する 2 番目のアクティビティを起動します。

次のスクリーン ショットは、このアプリケーションのいくつかの例を示しています。

[![通知の例のスクリーン ショット](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)

> [!NOTE]
> このガイドの説明で、 [NotificationCompat Api](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html)から、 [Android サポート ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)します。 これらの Api は互換性を確保最大内を後方に向かって Android 4.0 (API レベル 14)。

## <a name="creating-the-project"></a>プロジェクトの作成

最初に、みましょう新しい Android プロジェクトを作成、 **Android アプリ**テンプレート。 このプロジェクトをましょう**LocalNotifications**します。 (Xamarin.Android プロジェクトの作成に慣れていない場合は、次を参照してください[Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)。)。

リソース ファイルを編集**values/Strings.xml**通知チャネルの作成に時間がある場合に使用される 2 つの余分な文字列リソースを格納できるようにします。


```xml
<?xml version="1.0" encoding="utf-8"?>

<resources>
  <string name="Hello">Hello World, Click Me!</string>
  <string name="ApplicationName">Notifications</string>

  <string name="channel_name">Local Notifications</string>
  <string name="channel_description">The count from MainActivity.</string>
</resources>
```

### <a name="add-the-androidsupportv4-nuget-package"></a>Android.Support.V4 NuGet パッケージを追加します。

このチュートリアルでは使用`NotificationCompat.Builder`ローカルの通知を作成します。 説明したよう[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)、私たちに含める必要があります、 [Android サポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)を使用するプロジェクトで NuGet`NotificationCompat.Builder`します。

次に、編集しましょう**MainActivity.cs**し、以下の追加`using`ステートメントように内の型`Android.Support.V4.App`コードを利用できます。

```csharp
using Android.Support.V4.App;
```

また、オフにすると、コンパイラが使用していることする必要があります、`Android.Support.V4.App`版`TaskStackBuilder`なく`Android.App`バージョン。 次の追加`using`ステートメントあいまいさを解決するのには。

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```

### <a name="create-the-notification-channel"></a>通知チャネルを作成します。

メソッドを次に、追加`MainActivity`(必要に応じて) 通知チャネルを作成するされます。

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

    var name = Resources.GetString(Resource.String.channel_name);
    var description = GetString(Resource.String.channel_description);
    var channel = new NotificationChannel(CHANNEL_ID, name, NotificationImportance.Default)
                  {
                      Description = description
                  };

    var notificationManager = (NotificationManager) GetSystemService(NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

更新プログラム、`OnCreate`この新しいメソッドを呼び出すメソッド。

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();
}
```


### <a name="define-the-notification-id"></a>通知 ID を定義します

当社の通知と通知チャネルの一意の ID を必要になります。 編集しましょう**MainActivity.cs**次の静的インスタンス変数を追加し、`MainActivity`クラス。

```csharp
static readonly int NOTIFICATION_ID = 1000;
static readonly string CHANNEL_ID = "location_notification";
internal static readonly string COUNT_KEY = "count";
```

### <a name="add-code-to-generate-the-notification"></a>通知を生成するコードを追加します。

次に、ボタンの新しいイベント ハンドラーを作成する必要があります`Click`イベント。 次のメソッドを追加`MainActivity`:

```csharp
void ButtonOnClick(object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    var valuesForActivity = new Bundle();
    valuesForActivity.PutInt(COUNT_KEY, count);

    // When the user clicks the notification, SecondActivity will start up.
    var resultIntent = new Intent(this, typeof(SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras(valuesForActivity);

    // Construct a back stack for cross-task navigation:
    var stackBuilder = TaskStackBuilder.Create(this);
    stackBuilder.AddParentStack(Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent(resultIntent);

    // Create the PendingIntent with the back stack:
    var resultPendingIntent = stackBuilder.GetPendingIntent(0, (int) PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    var builder = new NotificationCompat.Builder(this, CHANNEL_ID)
                  .SetAutoCancel(true) // Dismiss the notification from the notification area when the user clicks on it
                  .SetContentIntent(resultPendingIntent) // Start up this activity when the user clicks the intent.
                  .SetContentTitle("Button Clicked") // Set the title
                  .SetNumber(count) // Display the count in the Content Info
                  .SetSmallIcon(Resource.Drawable.ic_stat_button_click) // This is the icon to display
                  .SetContentText($"The button has been clicked {count} times."); // the message to display.

    // Finally, publish the notification:
    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(NOTIFICATION_ID, builder.Build());

    // Increment the button press count:
    count++;
}
```

`OnCreate` MainActivity のメソッドは、通知チャネルを作成し、割り当てるへの呼び出しを行う必要があります、`ButtonOnClick`メソッドを`Click`ボタン (テンプレートによって提供されるデリゲート イベント ハンドラーを置き換えてください) のイベント。

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();

    // Display the "Hello World, Click Me!" button and register its event handler:
    var button = FindViewById<Button>(Resource.Id.MyButton);
    button.Click += ButtonOnClick;
}
```


### <a name="create-a-second-activity"></a>2 番目のアクティビティを作成します。

Android は、ユーザーは、通知をクリックしたときに表示される別のアクティビティを作成する必要があります。 プロジェクトに別の Android アクティビティを追加**SecondActivity**します。 開いている**SecondActivity.cs**内容を次のコードで置き換えます。

```csharp
using System;
using Android.App;
using Android.OS;
using Android.Widget;

namespace LocalNotifications
{
    [Activity(Label = "Second Activity")]
    public class SecondActivity : Activity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            // Get the count value passed to us from MainActivity:
            var count = Intent.Extras.GetInt(MainActivity.COUNT_KEY, -1);

            // No count was passed? Then just return.
            if (count <= 0)
            {
                return;
            }

            // Display the count sent from the first activity:
            SetContentView(Resource.Layout.Second);
            var txtView = FindViewById<TextView>(Resource.Id.textView1);
            txtView.Text = $"You clicked the button {count} times in the previous activity.";
        }
    }
}
```

リソース レイアウトを作成する必要がありますも**SecondActivity**します。 新しい追加**Android レイアウト**ファイルをプロジェクトと呼ばれる**Second.axml**します。 編集**Second.axml**レイアウトの次のコードに貼り付けます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text=""
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:id="@+id/textView1" />
</LinearLayout>
```


### <a name="add-a-notification-icon"></a>通知アイコンを追加します。

最後に、通知を起動するときに、通知領域に表示される小さいアイコンを追加します。 コピーすることができます[このアイコン](local-notifications-walkthrough-images/ic-stat-button-click.png)をプロジェクトにするか、独自のカスタム アイコンを作成します。 アイコン ファイルの名前を付けます**ic\_stat\_ボタン\_click.png**にコピーし、**リソース/drawable**フォルダー。 使用してください**追加 > 既存の項目.** にこのアイコン ファイルをプロジェクトに含めます。


### <a name="run-the-application"></a>アプリケーションの実行

アプリケーションをビルドして実行します。 次のスクリーン ショットと同様の最初のアクティビティが表示する必要があります。

[![最初のアクティビティのスクリーン ショット](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

ボタンをクリックすると、通知の小さなアイコンが通知領域に表示されることに注意してください必要があります。

[![通知アイコンが表示されます。](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

下にスワイプして通知ドロアーを公開した場合、通知が表示されます。

[![通知メッセージ](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

通知をクリックするは表示されなくなり、他のアクティビティを起動する必要があります&ndash;次のスクリーン ショットのようなものを検索します。

[![2 番目のアクティビティのスクリーン ショット](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

おめでとうございます! ローカル通知を Android チュートリアルを完了する時点でこのと実際のサンプルを参照できます。 通知の詳細はここでは、ここに表示がよりもので詳細については、する場合について見て多く[通知に関する Google のドキュメント](http://developer.android.com/guide/topics/ui/notifiers/notifications.html)します。


## <a name="summary"></a>まとめ

このチュートリアルのために使用`NotificationCompat.Builder`を作成し、通知を表示します。 ユーザー通知のやり取りに応答する方法として 2 番目のアクティビティを開始する方法の基本的な例を示しましたし、2 番目のアクティビティを最初のアクティビティからのデータの転送をについて説明します。


## <a name="related-links"></a>関連リンク

- [LocalNotifications (サンプル)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Android Oreo の通知チャネル](https://blog.xamarin.com/android-oreo-notification-channels/)
- [通知](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
