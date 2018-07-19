---
title: 予定表
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d48af16f50c5a4482342d323b5c73b9c237cc83a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767977"
---
# <a name="calendar"></a>予定表


## <a name="calendar-api"></a>予定表 API

新しい予定表 Android 4 で導入された Api のセットは、予定表のプロバイダーにデータを読み書きするように設計されたアプリケーションをサポートします。 これらの Api は、さまざまなイベント、出席者とリマインダーを読み書きする機能など、カレンダー データの相互作用オプションをサポートします。 カレンダーのプロバイダーを使用すると、アプリケーションで、API を通じて追加したデータは、Android 4 に付属する組み込みの予定表アプリに表示されます。


## <a name="adding-permissions"></a>アクセス許可を追加します。

アプリケーションで新しい予定表 Api を使用する場合は、Android マニフェストに適切なアクセス許可を追加は、まず行う必要があります。 追加する必要があります。 アクセス許可は`android.permisson.READ_CALENDAR`と`android.permission.WRITE_CALENDAR`かどうかを読み取ることが、予定表データの書き込みやするに応じて、します。


## <a name="using-the-calendar-contract"></a>カレンダーのコントラクトの使用方法

使用して、予定表のデータと対話できるアクセス許可を設定すると、`CalendarContract`クラスです。 このクラスは、アプリケーションがカレンダー プロバイダーとやり取りするときに使用するデータ モデルを提供します。 `CalendarContract`により、アプリケーションは、カレンダーとイベントなどの予定表のエンティティに Uri を解決するのには。 エンティティごとに、カレンダーの名前と ID、またはイベントの開始と終了日などのさまざまなフィールドとやり取りする方法も提供します。

予定表 API を使用する例を見てみましょう。 この例ではあります、カレンダーとそのイベントを列挙する方法だけでなく、予定表に、新しいイベントを追加する方法を確認します。


## <a name="listing-calendars"></a>予定表の一覧を表示します。

最初に、予定表アプリで登録されているカレンダーを列挙する方法を調べてみましょう。 これを行うには、インスタンス化できる、`CursorLoader`です。 Android 3.0 (API 11) で導入された`CursorLoader`が使用することをお勧め、`ContentProvider`です。 少なくとも、予定表と; を返す必要がある列のコンテンツ Uri を指定する必要があります。この列の仕様と呼ばれる、_プロジェクション_です。

呼び出す、`CursorLoader.LoadInBackground`メソッドにより、カレンダー プロバイダーなど、データのコンテンツ プロバイダーを照会します。
`LoadInBackground` 実際の読み込み操作を実行し、返します、`Cursor`クエリの結果。

`CalendarContract`両方のコンテンツを指定するために役立ちます us`Uri`投影とします。 コンテンツを取得する`Uri`予定表のクエリを実行するには、おだけで使用できます、`CalendarContract.Calendars.ContentUri`次のようなプロパティ。

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

使用して、`CalendarContract`カレンダーを指定する必要がある列は均等に単純です。 内のフィールドを追加するだけ、`CalendarContract.Calendars.InterfaceConsts`配列へのクラスです。 たとえば、次のコードには、予定表の ID が含まれます。、表示名、およびアカウント名。

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

`Id`が使用する場合は、重要な`SimpleCursorAdapter`UI ですぐに表示される同じデータをバインドします。 コンテンツの Uri および投影を使用してインスタンス化、`CursorLoader`を呼び出すと、`CursorLoader.LoadInBackground`を次に示すように、予定表のデータを使用してカーソルを返すメソッド。

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

この例の UI が含まれています、 `ListView`、1 つのカレンダーを表すリスト内の各項目にします。 次の XML を含むマークアップを示しています、 `ListView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">
  <ListView
    android:id="@android:id/android:list"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
</LinearLayout>
```

また、別の XML ファイルに次のように配置したリスト項目ごとの UI を指定する必要があります。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="wrap_content">
  <TextView android:id="@+id/calDisplayName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="16dip" />
  <TextView android:id="@+id/calAccountName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="12dip" />
</LinearLayout>
```

この時点から、カーソル位置から、データを UI にバインドするだけの通常の Android コードを勧めします。 使用して、`SimpleCursorAdapter`次のようにします。

```csharp
string[] sourceColumns = {
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName };

int[] targetResources = {
    Resource.Id.calDisplayName, Resource.Id.calAccountName };      

SimpleCursorAdapter adapter = new SimpleCursorAdapter (this,
    Resource.Layout.CalListItem, cursor, sourceColumns, targetResources);

ListAdapter = adapter;
```

上記のコードで、アダプターがで指定された列を受け取り、`sourceColumns`配列し、それらのユーザー インターフェイス要素を書き込みます、`targetResources`カーソル内の各予定表エントリの配列。 ここで使用する、アクティビティのサブクラスは、 `ListActivity`; が含まれている、`ListAdapter`プロパティ、アダプターを設定します。

表示されるカレンダー情報を含む最終結果を示すスクリーン ショットをここでは、 `ListView`:

[![2 つの予定表エントリを表示する、エミュレーターで実行されている CalendarDemo](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)



## <a name="listing-calendar-events"></a>カレンダー イベントの一覧

[次へ] を指定した暦のイベントを列挙する方法を見てみましょう。
上記の例に基づいて構築、予定表のいずれかを選択すると、イベントの一覧を提示おです。 したがって、前のコードで項目の選択を処理する必要があります。

```csharp
ListView.ItemClick += (sender, e) => {
    int i = (e as ItemEventArgs).Position;

    cursor.MoveToPosition(i);
    int calId =
        cursor.GetInt (cursor.GetColumnIndex (calendarsProjection [0]));

    var showEvents = new Intent(this, typeof(EventListActivity));
    showEvents.PutExtra("calId", calId);
    StartActivity(showEvents);
};
```

このコードでは、インテントを開くには型のアクティビティを作成している`EventListActivity`目的の予定表の ID を渡すことです。 イベントのクエリをどのカレンダーを認識している ID は必要があります。 `EventListActivity`の`OnCreate`メソッドから ID を取得できます、`Intent`次のようにします。

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

これで、このクエリのイベントの id カレンダーみましょう。 イベントをクエリするプロセスは、前の予定表の一覧を照会したのと同様の操作を行います。 このときにだけ、`CalendarContract.Events`クラスです。 次のコードでは、イベントを取得するクエリを作成します。

```csharp
var eventsUri = CalendarContract.Events.ContentUri;

string[] eventsProjection = {
    CalendarContract.Events.InterfaceConsts.Id,
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart
};

var loader = new CursorLoader(this, eventsUri, eventsProjection,
                   String.Format ("calendar_id={0}", _calId), null, "dtstart ASC");
var cursor = (ICursor)loader.LoadInBackground();
```

このコードでは最初にコンテンツを取得します`Uri`からのイベントに対して、`CalendarContract.Events.ContentUri`プロパティです。 次で eventsProjection 配列を取得する必要があるイベント列を指定します。
最後に、インスタンス化、 `CursorLoader` 、この情報と、ローダーを呼び出し`LoadInBackground`を返すメソッドを`Cursor`イベント データを使用します。

UI でイベント データを表示するには、マークアップおよびコードの予定表の一覧を表示する前に行ったのと同じように使用できます。 使用して、もう一度`SimpleCursorAdapter`にデータをバインドする、`ListView`次のコードに示すようにします。

```csharp
string[] sourceColumns = {
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart };

int[] targetResources = {
    Resource.Id.eventTitle,
    Resource.Id.eventStartDate };

var adapter = new SimpleCursorAdapter (this, Resource.Layout.EventListItem,
    cursor, sourceColumns, targetResources);

adapter.ViewBinder = new ViewBinder ();       
ListAdapter = adapter;
```

このコードと使用して、前に、予定表の一覧を表示するコードの主な違いは、使用、`ViewBinder`行に設定されています。

```csharp
adapter.ViewBinder = new ViewBinder ();
```

`ViewBinder`クラスにより、さらに制御をビューに値をバインドします。 ここを使用してイベントの開始時刻をミリ秒単位から日付の文字列に変換する、次の実装で示すようにします。

```csharp
class ViewBinder : Java.Lang.Object, SimpleCursorAdapter.IViewBinder
{    
    public bool SetViewValue (View view, Android.Database.ICursor cursor,
        int columnIndex)
    {
        if (columnIndex == 2) {
            long ms = cursor.GetLong (columnIndex);

            DateTime date = new DateTime (1970, 1, 1, 0, 0, 0,
                DateTimeKind.Utc).AddMilliseconds (ms).ToLocalTime ();

            TextView textView = (TextView)view;
            textView.Text = date.ToLongDateString ();

            return true;
        }
        return false;
    }    
}
```

次のように、イベントの一覧が表示されます。

[![次の 3 つのカレンダー イベントを表示する例のアプリのスクリーン ショット](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)



## <a name="adding-a-calendar-event"></a>予定表イベントを追加します。

予定表のデータを読み取る方法を説明しました。 これで、カレンダーにイベントを追加する方法について説明します。 これを行うには、必ず、`android.permission.WRITE_CALENDAR`前述のアクセス許可。 カレンダーにイベントを追加するには。

1.  作成、`ContentValues`インスタンス。
1.  キーを使用して、`CalendarContract.Events.InterfaceConsts`を設定するクラス、`ContentValues`インスタンス。
1.  イベントの開始のタイム ゾーンを設定して、終了時刻。
1.  使用して、`ContentResolver`カレンダーにイベント データを挿入します。


次のコードでは、次の手順を示します。

```csharp
ContentValues eventValues = new ContentValues ();

eventValues.Put (CalendarContract.Events.InterfaceConsts.CalendarId,
    _calId);
eventValues.Put (CalendarContract.Events.InterfaceConsts.Title,
    "Test Event from M4A");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Description,
    "This is an event created from Xamarin.Android");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtstart,
    GetDateTimeMS (2011, 12, 15, 10, 0));
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtend,
    GetDateTimeMS (2011, 12, 15, 11, 0));

eventValues.Put(CalendarContract.Events.InterfaceConsts.EventTimezone,
    "UTC");
eventValues.Put(CalendarContract.Events.InterfaceConsts.EventEndTimezone,
    "UTC");

var uri = ContentResolver.Insert (CalendarContract.Events.ContentUri,
    eventValues);
```

されている場合、タイム ゾーン、型の例外を設定しないでくださいお`Java.Lang.IllegalArgumentException`がスローされます。 イベントの時刻の値は、エポック以降のミリ秒単位で表さ必要があります、ためにの作成、`GetDateTimeMS`メソッド (で`EventListActivity`)、日付の仕様をミリ秒形式に変換します。

```csharp
long GetDateTimeMS (int yr, int month, int day, int hr, int min)
{
    Calendar c = Calendar.GetInstance (Java.Util.TimeZone.Default);

    c.Set (Java.Util.CalendarField.DayOfMonth, 15);
    c.Set (Java.Util.CalendarField.HourOfDay, hr);
    c.Set (Java.Util.CalendarField.Minute, min);
    c.Set (Java.Util.CalendarField.Month, Calendar.December);
    c.Set (Java.Util.CalendarField.Year, 2011);

    return c.TimeInMillis;
}
```

イベント一覧 UI にボタンを追加すると、上記のコードを実行ボタンの click イベント ハンドラー、イベントが、予定表に追加し、次に示すように、一覧で更新します。

[![サンプル イベントの追加 ボタンを続けてカレンダー イベントの例のアプリのスクリーン ショット](calendar-images/13.png)](calendar-images/13.png#lightbox)

予定表アプリを開くこと、お、表示、イベントを書き込むことがありますもされます。

[![選択したイベントを表示する予定表アプリのスクリーン ショット](calendar-images/14.png)](calendar-images/14.png#lightbox)

ご覧のように、Android はできるため、予定表の機能にシームレスに統合するアプリケーションを取得し、予定表のデータを保持する強力かつ簡単のアクセスを許可します。


## <a name="related-links"></a>関連リンク

- [カレンダーのデモ (サンプル)](https://developer.xamarin.com/samples/CalendarDemo/)
- [アイスクリーム サンドイッチの概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](http://developer.android.com/sdk/android-4.0.html)
