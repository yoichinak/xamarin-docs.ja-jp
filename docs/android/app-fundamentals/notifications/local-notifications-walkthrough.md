---
title: チュートリアル-Xamarin. Android でのローカル通知の使用
description: このチュートリアルでは、Xamarin Android アプリケーションでローカル通知を使用する方法について説明します。 ここでは、ローカル通知を作成して発行する方法の基本について説明します。 ユーザーが通知領域の通知をクリックすると、2つ目のアクティビティが開始されます。
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/16/2018
ms.openlocfilehash: 6d48d650b0900e71b7d3d4d5e1ff1ac919dcb948
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025545"
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>チュートリアル-Xamarin. Android でのローカル通知の使用

_このチュートリアルでは、Xamarin Android アプリケーションでローカル通知を使用する方法について説明します。ここでは、ローカル通知を作成して発行する方法の基本について説明します。ユーザーが通知領域の通知をクリックすると、2つ目のアクティビティが開始されます。_

## <a name="overview"></a>概要

このチュートリアルでは、ユーザーがアクティビティのボタンをクリックしたときに通知を生成する Android アプリケーションを作成します。 ユーザーが通知をクリックすると、2番目のアクティビティが起動し、ユーザーが最初のアクティビティのボタンをクリックした回数が表示されます。

次のスクリーンショットは、このアプリケーションの例を示しています。

[通知を含む![のスクリーンショットの例](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)

> [!NOTE]
> このガイドでは、 [Android サポートライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)の[notificationcompat api](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html)に焦点を当てています。 これらの Api によって、Android 4.0 (API レベル 14) との下位互換性が最大になります。

## <a name="creating-the-project"></a>プロジェクトの作成

まず、 **Android アプリ**テンプレートを使用して新しい android プロジェクトを作成してみましょう。 このプロジェクトの**localnotifications**を呼び出しましょう。 (Xamarin Android プロジェクトの作成に慣れていない場合は、「 [Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md)」を参照してください)。

リソースファイルの**値/文字列 .xml**を編集して、通知チャネルの作成時に使用される2つの余分な文字列リソースが含まれるようにします。

```xml
<?xml version="1.0" encoding="utf-8"?>

<resources>
  <string name="Hello">Hello World, Click Me!</string>
  <string name="ApplicationName">Notifications</string>

  <string name="channel_name">Local Notifications</string>
  <string name="channel_description">The count from MainActivity.</string>
</resources>
```

### <a name="add-the-androidsupportv4-nuget-package"></a>Android. サポートの NuGet パッケージを追加する

このチュートリアルでは、`NotificationCompat.Builder` を使用してローカル通知を作成します。 「[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)」で説明されているように、`NotificationCompat.Builder`を使用するには、 [Android サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet をプロジェクトに含める必要があります。

次に、 **MainActivity.cs**を編集し、次の `using` ステートメントを追加して、`Android.Support.V4.App` 内の型がコードで使用できるようにします。

```csharp
using Android.Support.V4.App;
```

また、`Android.App` バージョンではなく、`TaskStackBuilder` の `Android.Support.V4.App` バージョンを使用していることをコンパイラに明確にする必要があります。 あいまいさを解決するには、次の `using` ステートメントを追加します。

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```

### <a name="create-the-notification-channel"></a>通知チャネルを作成する

次に、必要に応じて通知チャネルを作成する `MainActivity` するメソッドを追加します。

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

この新しいメソッドを呼び出すように `OnCreate` メソッドを更新します。

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();
}
```

### <a name="define-the-notification-id"></a>通知 ID を定義する

通知と通知チャネルには一意の ID が必要です。 **MainActivity.cs**を編集し、次の静的インスタンス変数を `MainActivity` クラスに追加してみましょう。

```csharp
static readonly int NOTIFICATION_ID = 1000;
static readonly string CHANNEL_ID = "location_notification";
internal static readonly string COUNT_KEY = "count";
```

### <a name="add-code-to-generate-the-notification"></a>通知を生成するコードを追加する

次に、ボタン `Click` イベントの新しいイベントハンドラーを作成する必要があります。 次のメソッドを `MainActivity` に追加します。

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

MainActivity の `OnCreate` メソッドでは、通知チャネルを作成し、`ButtonOnClick` メソッドをボタンの `Click` イベントに割り当てる必要があります (テンプレートによって提供されるデリゲートイベントハンドラーを置き換える)。

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

### <a name="create-a-second-activity"></a>2番目のアクティビティを作成する

次に、ユーザーが通知をクリックしたときに Android によって表示される別のアクティビティを作成する必要があります。 別の Android アクティビティを **、"という名前のプロジェクト**に追加します。 **SecondActivity.cs**を開き、その内容を次のコードに置き換えます。

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

また、次のように、同じ**アクティビティ**のリソースレイアウトを作成する必要があります。 **2 番目の axml**という名前の新しい**Android レイアウト**ファイルをプロジェクトに追加します。 **2 番目の axml**を編集し、次のレイアウトコードを貼り付けます。

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

### <a name="add-a-notification-icon"></a>通知アイコンの追加

最後に、通知が開始されたときに通知領域に表示される小さいアイコンを追加します。 [このアイコン](local-notifications-walkthrough-images/ic-stat-button-click.png)をプロジェクトにコピーするか、独自のカスタムアイコンを作成することができます。 アイコンファイルに「 **ic\_stat\_」** という名前を付いたボタンをクリックし、[.png] をクリックして、[ **Resources/** \_] フォルダーにコピーします。 このアイコンファイルをプロジェクトに含めるには、 **[既存の項目の追加 >]** を忘れずに使用してください。

### <a name="run-the-application"></a>アプリケーションの実行

アプリケーションをビルドして実行します。 最初のアクティビティが表示されます。次のスクリーンショットのようになります。

[![最初のアクティビティのスクリーンショット](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

このボタンをクリックすると、通知の小さいアイコンが通知領域に表示されます。

[![通知アイコンが表示される](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

下へスワイプして通知ドロワーを公開すると、通知が表示されます。

[![通知メッセージ](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

通知をクリックすると、通知が表示されなくなり、次のスクリーンショットのように、その他のアクティビティが起動 &ndash; ます。

[![2 番目のアクティビティのスクリーンショット](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

おめでとうございます! この時点で、Android のローカル通知チュートリアルが完了し、参照できる実用的なサンプルがあります。 ここでは説明したよりも多くの通知があります。詳細については、[通知に関する Google のドキュメント](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)を参照してください。

## <a name="summary"></a>まとめ

このチュートリアルでは `NotificationCompat.Builder` を使用して通知を作成および表示します。 ここでは、通知によるユーザーの操作に応答する方法として2番目のアクティビティを開始する方法の基本的な例を示し、最初のアクティビティから2番目のアクティビティへのデータの転送について説明しました。

## <a name="related-links"></a>関連リンク

- [LocalNotifications (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/localnotifications)
- [Android Oreo の通知チャネル](https://blog.xamarin.com/android-oreo-notification-channels/)
- [警告](xref:Android.App.Notification)
- [NotificationManager](xref:Android.App.NotificationManager)
- [NotificationCompat。ビルダー](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](xref:Android.App.PendingIntent)
