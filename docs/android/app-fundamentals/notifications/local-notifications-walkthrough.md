---
title: チュートリアル - Xamarin.Android でローカルの通知の使用
description: このチュートリアルでは、Xamarin.Android アプリケーションでローカルの通知を使用する方法を示します。 作成して、ローカルの通知の発行の基礎を示しています。 ユーザーは、通知領域に通知をクリックすると、2 番目のアクティビティを開始します。
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/30/2018
ms.openlocfilehash: a2ca3755e3201263584447ba47ec36d2096386da
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>チュートリアル - Xamarin.Android でローカルの通知の使用

_このチュートリアルでは、Xamarin.Android アプリケーションでローカルの通知を使用する方法を示します。作成して、ローカルの通知の発行の基礎を示しています。ユーザーは、通知領域に通知をクリックすると、2 番目のアクティビティを開始します。_


## <a name="overview"></a>概要

このチュートリアルでは、ユーザー アクティビティ内のボタンをクリックしたときに通知を発生させる Android アプリケーションを作成おです。 ユーザーは、通知をクリックすると、最初のアクティビティのボタンをクリックした回数を表示する 2 つ目のアクティビティを起動します。

次のスクリーン ショットは、このアプリケーションの例をいくつかを示しています。

[![通知の使用例のスクリーン ショット](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)



## <a name="walkthrough"></a>チュートリアル

最初に、作成しましょう、Android プロジェクトを使用して新しい、 **Android アプリ**テンプレート。 このプロジェクトをとしましょう**LocalNotifications**です。 (Xamarin.Android プロジェクトの作成に慣れていない場合は、次を参照してください[Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)。)。


### <a name="add-the-androidsupportv4app-component"></a>Android.Support.V4.App コンポーネントを追加します。

このチュートリアルでは使用`NotificationCompat.Builder`をローカルの通知を作成します。 説明したよう[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)、私たちを含める必要があります、 [Android のサポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)を使用するプロジェクト内の NuGet`NotificationCompat.Builder`です。

次に、編集しましょう**MainActivity.cs**し、以下の追加`using`ステートメントように内の型`Android.Support.V4.App`コードを利用できます。

```csharp
using Android.Support.V4.App;
```

また、コンパイラを使用して、オフにするようにお必要があります、`Android.Support.V4.App`バージョンの`TaskStackBuilder`ではなく、`Android.App`バージョン。 次の追加`using`ステートメントあいまいさを解決するのには。

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```


### <a name="define-the-notification-id"></a>通知 ID を定義します

一意の ID は、この通知お必要があります。 編集しましょう**MainActivity.cs**次の静的インスタンス変数を追加し、`MainActivity`クラス。

```csharp
private static readonly int ButtonClickNotificationId = 1000;
```


### <a name="add-code-to-generate-the-notification"></a>通知を生成するコードを追加します。

次に、ボタンの新しいイベント ハンドラーを作成する必要があります`Click`イベント。 次のメソッドを追加`MainActivity`:

```csharp
private void ButtonOnClick (object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    Bundle valuesForActivity = new Bundle();
    valuesForActivity.PutInt ("count", count);

    // When the user clicks the notification, SecondActivity will start up.
    Intent resultIntent = new Intent(this, typeof (SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras (valuesForActivity);

    // Construct a back stack for cross-task navigation:
    TaskStackBuilder stackBuilder = TaskStackBuilder.Create (this);
    stackBuilder.AddParentStack (Java.Lang.Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent (resultIntent);

    // Create the PendingIntent with the back stack:            
    PendingIntent resultPendingIntent =
        stackBuilder.GetPendingIntent (0, (int)PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
        .SetAutoCancel (true)                    // Dismiss from the notif. area when clicked
        .SetContentIntent (resultPendingIntent)  // Start 2nd activity when the intent is clicked.
        .SetContentTitle ("Button Clicked")      // Set its title
        .SetNumber (count)                       // Display the count in the Content Info
        .SetSmallIcon(Resource.Drawable.ic_stat_button_click)  // Display this icon
        .SetContentText (String.Format(
            "The button has been clicked {0} times.", count)); // The message to display.

    // Finally, publish the notification:
    NotificationManager notificationManager =
        (NotificationManager)GetSystemService(Context.NotificationService);
    notificationManager.Notify(ButtonClickNotificationId, builder.Build());

    // Increment the button press count:
    count++;
}
```

`OnCreate`メソッド、これを割り当てる`ButtonOnClick`メソッドを`Click`ボタン (置き換えるテンプレートによって提供されるデリゲートのイベント ハンドラー) のイベント。

```csharp
button.Click += ButtonOnClick;
```


### <a name="create-a-second-activity"></a>2 番目のアクティビティを作成します。

ユーザーが、通知をクリックしたときに Android を表示する別のアクティビティを作成する必要があります。 Android の別のアクティビティをプロジェクトに追加と呼ばれる**SecondActivity**です。 開いている**SecondActivity.cs**しその内容をこのコードに置き換えます。

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
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Get the count value passed to us from MainActivity:
            int count = Intent.Extras.GetInt ("count", -1);

            // No count was passed? Then just return.
            if (count <= 0)
                return;

            // Display the count sent from the first activity:
            SetContentView (Resource.Layout.Second);
            TextView textView = FindViewById<TextView>(Resource.Id.textView);
            textView.Text = String.Format (
                "You clicked the button {0} times in the previous activity.", count);
        }
    }
}
```

リソースのレイアウトを作成する必要がありますもお**SecondActivity**です。 新しい**Android レイアウト**ファイルをプロジェクトと呼ばれる**Second.axml**です。 編集**Second.axml**レイアウトの次のコードに貼り付けます。

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
        android:id="@+id/textView" />
</LinearLayout>
```


### <a name="add-a-notification-icon"></a>通知アイコンを追加します。

最後に、この通知が起動されたときに、通知領域に表示される小さいアイコンを追加してみましょう。 コピーすることができます[このアイコン](local-notifications-walkthrough-images/ic-stat-button-click.png)をプロジェクトにするか、独自のカスタム アイコンを作成します。 という名前をアイコン ファイル**ic\_stat\_ボタン\_click.png**にコピーし、**リソース/描画**フォルダーです。 使用してください**追加 > 既存アイテム.**にこのアイコン ファイルをプロジェクトに含めます。


### <a name="run-the-application"></a>アプリケーションを実行する

ビルドして、アプリケーションを実行してみましょう。 次のスクリーン ショットに類似の最初のアクティビティが表示する必要があります。

[![最初のアクティビティのスクリーン ショット](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

ボタンをクリックすると、通知領域に表示される通知の小さいアイコンのことを確認する必要があります。

[![通知アイコンが表示されます。](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

ダウン スワイプして通知ドロワーを公開した場合、通知が表示されます。

[![通知メッセージ](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

通知をクリックして、表示されなくなりますされ、その他のアクティビティを起動する必要があります&ndash;次のスクリーン ショットのようなものを検索します。

[![2 つ目のアクティビティのスクリーン ショット](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

おめでとうございます!  Android のローカル通知チュートリアルを完了するこの時点でを使用して実際のサンプルを参照することができます。 通知により、ここで紹介したよりもため詳細については、する場合を見て多くは[通知に Google のドキュメント](http://developer.android.com/guide/topics/ui/notifiers/notifications.html)と Android[通知](http://developer.android.com/design/patterns/notifications.html)設計のガイドです。



## <a name="summary"></a>まとめ

このチュートリアルでは使用`NotificationCompat.Builder`を作成し、表示通知を表示します。 通知とユーザーの対話に応答する方法として 2 番目のアクティビティを開始する方法の基本的な例を説明しましたと 2 番目のアクティビティを最初のアクティビティからのデータの転送を紹介しました。


## <a name="related-links"></a>関連リンク

- [LocalNotifications (サンプル)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [通知のデザイン ガイド パターン](http://developer.android.com/design/patterns/notifications.html)
- [通知](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
