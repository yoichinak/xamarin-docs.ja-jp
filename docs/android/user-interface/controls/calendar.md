---
title: 予定表
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: d8a6044c47c568c7f1e17d01915e2e5a6e888f95
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61278027"
---
# <a name="calendar"></a>予定表


## <a name="calendar-api"></a>予定表 API

Android 4 で導入された Api のカレンダーの新しいセットは、予定表のプロバイダーにデータを読み書きできるように設計されたアプリケーションをサポートします。 これらの Api は、豊富なイベント、出席者、およびアラームを読み書きする機能など、予定表のデータとの相互作用オプションをサポートします。 予定表のプロバイダーを使用すると、アプリケーションで、API を通じて追加したデータは、Android 4 に付属する組み込みの予定表アプリに表示されます。


## <a name="adding-permissions"></a>アクセス許可を追加

アプリケーションで新しい予定表 Api を使用する場合は、Android マニフェストを適切なアクセス許可の追加はまず行う必要があります。 追加する必要があるアクセス許可は`android.permisson.READ_CALENDAR`と`android.permission.WRITE_CALENDAR`予定表データの書き込みおよび読み取りをかどうかに応じて、します。


## <a name="using-the-calendar-contract"></a>予定表のコントラクトの使用方法

使用して予定表のデータとやり取りすることができます、アクセス許可を設定すると、`CalendarContract`クラス。 このクラスは、アプリケーションは、予定表のプロバイダーとやり取りしているときに使用できるデータ モデルを提供します。 `CalendarContract`イベント カレンダーなど、予定表のエンティティに Uri を解決するのにはアプリケーションを使用します。 また、エンティティごとに、カレンダーの名前と ID、またはイベントの開始および終了日などのさまざまなフィールドと対話することもできます。

予定表 API を使用する例を見てみましょう。 この例では、私たちにカレンダーと、そのイベントを列挙する方法と、カレンダーに新しいイベントを追加する方法について説明します。


## <a name="listing-calendars"></a>予定表を一覧表示します。

最初に、予定表アプリで登録されているカレンダーを列挙する方法を調べてみましょう。 これを行うには、インスタンス化できる、`CursorLoader`します。 Android 3.0 (API 11) で導入された`CursorLoader`が使用することをお勧め、`ContentProvider`します。 少なくとも、予定表と; を返す必要がある列のコンテンツ Uri を指定する必要があります。この列の指定と呼ばれる、_プロジェクション_します。

呼び出す、`CursorLoader.LoadInBackground`メソッドでは、カレンダー プロバイダーなどのデータのコンテンツ プロバイダーのクエリを実行できます。
`LoadInBackground` 実際の読み込み操作を実行し、返します、`Cursor`クエリの結果。

`CalendarContract`私たちを使用するには、両方のコンテンツを指定することで`Uri`と投影します。 コンテンツを取得する`Uri`の予定表のクエリを実行するには、単に使用できます、`CalendarContract.Calendars.ContentUri`このようなプロパティ。

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

使用して、`CalendarContract`カレンダーを指定する必要がある列は実に単純な。 内のフィールドを追加するだけ、`CalendarContract.Calendars.InterfaceConsts`クラスを配列にします。 たとえば、次のコードには、予定表の ID が含まれます。、表示名、アカウント名。

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

`Id`必ず使用している場合、`SimpleCursorAdapter`ようにすぐにわかりますが、UI にデータをバインドします。 コンテンツの Uri と投影があれば、インスタンス化、`CursorLoader`を呼び出すと、`CursorLoader.LoadInBackground`メソッドを次に示すように、予定表データを使用してカーソルを返します。

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

この例では、UI が含まれています、 `ListView`、1 つのカレンダーを表すリストの各項目にします。 次の XML を含むマークアップを示しています、 `ListView`:

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

また、各リスト項目は、次のように個別の XML ファイルに配置するための UI を指定する必要があります。

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

この時点から、カーソルからデータを UI にバインドするごく普通の Android コードです。 使用して、`SimpleCursorAdapter`次のようにします。

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

上記のコードで、アダプターがで指定された列を受け取り、`sourceColumns`配列し、ユーザー インターフェイス要素に書き込む、`targetResources`カーソルでは、各予定表エントリの配列。 ここで使用するアクティビティのサブクラスは、 `ListActivity`; が含まれています、`ListAdapter`プロパティをアダプターを設定します。

ここに表示されるカレンダー情報を含む結果を示すスクリーン ショット、 `ListView`:

[![予定表の 2 つのエントリを表示するエミュレーターで実行されている CalendarDemo](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)



## <a name="listing-calendar-events"></a>カレンダー イベントの一覧

[次へ] を指定したカレンダーのイベントを列挙する方法を見てみましょう。
上記の例の基に構築、ご予定表のいずれかを選択すると、イベントの一覧を紹介します。 そのため、上記のコードで項目の選択を処理する必要があります。

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

このコードでは、型のアクティビティを開く、インテントを作成しています`EventListActivity`意図の予定表の ID を渡します。 予定表イベントを照会するために ID を必要になります。 `EventListActivity`の`OnCreate`メソッドから ID を取得できます、`Intent`次に示すよう。

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

これで、このクエリのイベントの id カレンダーしましょう。 イベントを照会するプロセスは、前のカレンダーの一覧についてはクエリを実行するように操作を行います。 この時点でのみ、`CalendarContract.Events`クラス。 次のコードでは、イベントを取得するクエリを作成します。

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

このコードで最初に取得コンテンツ`Uri`からのイベント、`CalendarContract.Events.ContentUri`プロパティ。 次で eventsProjection 配列を取得する必要があるイベント列を指定します。
最後に、インスタンス化、`CursorLoader`この情報と、ローダーを呼び出し`LoadInBackground`を返すメソッドを`Cursor`をイベント データ。

UI でイベント データを表示するには、マークアップと予定表の一覧を表示する前に行ったのと同じようにコードを使用できます。 使用して、もう一度`SimpleCursorAdapter`にデータをバインドする、`ListView`次のコードに示すようにします。

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

このコードとカレンダーの一覧を表示する、以前使用したコードの主な違いは、使用、`ViewBinder`行に設定されています。

```csharp
adapter.ViewBinder = new ViewBinder ();
```

`ViewBinder`クラスでは、さらに制御をビューに値をバインドできます。 この場合、使用しますが (ミリ秒) からのイベントの開始時刻を日付の文字列に変換する実装を次に示すように。

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

次に示す、イベントの一覧が表示されます。

[![次の 3 つのカレンダー イベントを表示するアプリの例のスクリーン ショット](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)



## <a name="adding-a-calendar-event"></a>予定表イベントを追加します。

予定表のデータを読み取る方法を説明しました。 これで、カレンダーにイベントを追加する方法を見てみましょう。 これを機能させるには、必ず、`android.permission.WRITE_CALENDAR`アクセス許可は既に説明しました。 カレンダーにイベントを追加することをご紹介します。

1.  作成、`ContentValues`インスタンス。
1.  キーを使用する、`CalendarContract.Events.InterfaceConsts`を設定するクラス、`ContentValues`インスタンス。
1.  イベントの開始のタイム ゾーンを設定して、終了時刻。
1.  使用して、`ContentResolver`カレンダーにイベント データを挿入します。


次のコードは、次の手順を示しています。

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

されている場合、タイム ゾーンを型の例外を設定しないでください`Java.Lang.IllegalArgumentException`がスローされます。 イベント時刻の値は、エポック以降ミリ秒単位で表す必要があります、ため、作成、`GetDateTimeMS`メソッド (で`EventListActivity`)、日付の仕様をミリ秒形式に変換します。

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

イベント一覧の UI にボタンを追加すると、上記のコードを実行ボタンの click イベント ハンドラー、イベントがカレンダーに追加され、次に示すように、一覧で更新します。

[![サンプル イベントの追加 ボタンの後にカレンダー イベントとアプリの例のスクリーン ショット](calendar-images/13.png)](calendar-images/13.png#lightbox)

予定表アプリを開く場合、わかること、イベントは、そこに書き込まれても。

[![選択したカレンダーのイベントを表示する予定表アプリのスクリーン ショット](calendar-images/14.png)](calendar-images/14.png#lightbox)

ご覧のように、Android は、アプリケーションの予定表機能にシームレスに統合可能を取得し、予定表のデータを永続化強力かつ簡単のアクセスを許可します。


## <a name="related-links"></a>関連リンク

- [予定表のデモ (サンプル)](https://developer.xamarin.com/samples/CalendarDemo/)
- [Ice Cream Sandwich の概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
