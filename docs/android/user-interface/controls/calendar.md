---
title: Xamarin Android カレンダー
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 027b526815c56e5a0092b68b550129d9ffbf1493
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643027"
---
# <a name="xamarinandroid-calendar"></a>Xamarin Android カレンダー


## <a name="calendar-api"></a>Calendar API

Android 4 で導入された新しい一連のカレンダー Api は、カレンダープロバイダーに対してデータの読み取りまたは書き込みを行うように設計されたアプリケーションをサポートしています。 これらの Api は、イベント、出席者、リマインダーの読み取りと書き込みなど、カレンダーデータを使用した豊富な相互作用オプションをサポートしています。 アプリケーションで calendar プロバイダーを使用すると、API を通じて追加したデータが、Android 4 に付属する組み込みの予定表アプリに表示されます。


## <a name="adding-permissions"></a>アクセス許可の追加

アプリケーションで新しい calendar Api を使用する場合は、まず、Android マニフェストに適切なアクセス許可を追加する必要があります。 追加する必要のあるアクセス許可`android.permisson.READ_CALENDAR`は`android.permission.WRITE_CALENDAR`、予定表データの読み取りと書き込みのどちらであるかによって異なります。


## <a name="using-the-calendar-contract"></a>カレンダーコントラクトの使用

アクセス許可を設定すると、 `CalendarContract`クラスを使用してカレンダーデータを操作できます。 このクラスは、アプリケーションがカレンダープロバイダーと対話するときに使用できるデータモデルを提供します。 を`CalendarContract`使用すると、アプリケーションは、カレンダーやイベントなどの予定表エンティティの uri を解決できます。 また、カレンダーの名前や ID、イベントの開始日と終了日など、各エンティティのさまざまなフィールドを操作する方法も提供されます。

Calendar API の使用例を見てみましょう。 この例では、カレンダーとそのイベントを列挙する方法と、カレンダーに新しいイベントを追加する方法について説明します。


## <a name="listing-calendars"></a>カレンダーの一覧表示

まず、カレンダーアプリに登録されているカレンダーを列挙する方法について説明します。 これを行うには、を`CursorLoader`インスタンス化します。 Android 3.0 (API 11) `CursorLoader`で導入された`ContentProvider`は、を使用するためのお勧めの方法です。 少なくとも、カレンダーのコンテンツ Uri と返される列を指定する必要があります。この列の指定は_射影_と呼ばれます。

`CursorLoader.LoadInBackground`メソッドを呼び出すと、カレンダープロバイダーなどのデータのコンテンツプロバイダーに対してクエリを実行できます。
`LoadInBackground`実際の読み込み操作を実行し、 `Cursor`クエリの結果と共にを返します。

は`CalendarContract` 、コンテンツ`Uri`と投影の両方を指定するのに役立ちます。 カレンダーのクエリ用`Uri`にコンテンツを取得するには、次`CalendarContract.Calendars.ContentUri`のようにプロパティを使用します。

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

を使用`CalendarContract`して、必要な予定表の列を均等に単純に指定します。 `CalendarContract.Calendars.InterfaceConsts`クラスのフィールドを配列に追加するだけです。 たとえば、次のコードには、カレンダーの ID、表示名、アカウント名が含まれています。

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

は`Id` 、後で説明するように、を`SimpleCursorAdapter`使用してデータを UI にバインドする場合に、を含めることが重要です。 コンテンツ Uri とプロジェクションが配置されたので、 `CursorLoader`次に示す`CursorLoader.LoadInBackground`ように、をインスタンス化し、メソッドを呼び出して、カレンダーデータを含むカーソルを返します。

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

この例の UI には`ListView`が含まれており、リスト内の各項目は1つの暦を表します。 次の XML は、を`ListView`含むマークアップを示しています。

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

また、次のように、個別の XML ファイルに配置する各リスト項目の UI を指定する必要があります。

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

この時点から、カーソルから UI にデータをバインドするのは、通常の Android コードにすぎません。 次のように`SimpleCursorAdapter`を使用します。

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

上のコードでは、アダプターは`sourceColumns`配列に指定された列を受け取り、カーソル内の各カレンダーエントリの`targetResources`配列内のユーザーインターフェイス要素に書き込みます。 ここで使用されるアクティビティは、 `ListActivity`のサブクラスです`ListAdapter` 。アダプターを設定するプロパティが含まれています。

次に`ListView`、最終的な結果を示すスクリーンショットを示します。カレンダー情報がに表示されます。

[![2つのカレンダーエントリを表示する、エミュレーターで実行されている CalendarDemo](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)



## <a name="listing-calendar-events"></a>カレンダーイベントの一覧表示

次に、特定のカレンダーのイベントを列挙する方法を見てみましょう。
上の例に基づいて、ユーザーがカレンダーの1つを選択したときにイベントの一覧を表示します。 そのため、前のコードで項目の選択を処理する必要があります。

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

このコードでは、型`EventListActivity`のアクティビティを開き、意図した暦の ID を渡すインテントを作成しています。 イベントを照会する予定表を把握するには ID が必要です。 のメソッドでは、次に示すように、 `Intent`から ID を取得できます。 `OnCreate` `EventListActivity`

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

ここで、このカレンダー ID のイベントをクエリします。 イベントを照会するプロセスは、先に予定表の一覧を照会したときの方法に似ていますが、今回`CalendarContract.Events`はクラスを使用します。 次のコードでは、イベントを取得するクエリを作成します。

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

このコードでは、まず`Uri` `CalendarContract.Events.ContentUri`プロパティからイベントのコンテンツを取得します。 次に、イベントプロジェクション配列で取得するイベント列を指定します。
最後に、この情報`CursorLoader`を使用してをインスタンス化し`LoadInBackground` 、ローダーのメソッド`Cursor`を呼び出して、イベントデータと共にを返します。

UI でイベントデータを表示するには、予定表の一覧を表示する前と同じように、マークアップとコードを使用します。 ここで`ListView`も、 `SimpleCursorAdapter`次のコードに示すように、を使用してデータをにバインドします。

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

このコードと、カレンダーの一覧を表示するために前に使用したコードの主な違いは`ViewBinder`、行に設定されているを使用することです。

```csharp
adapter.ViewBinder = new ViewBinder ();
```

クラス`ViewBinder`を使用すると、ビューに値をバインドする方法をさらに制御できます。 この場合、次の実装に示すように、このメソッドを使用して、イベントの開始時刻をミリ秒から日付文字列に変換します。

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

次に示すように、イベントの一覧が表示されます。

[![3つのカレンダーイベントを表示しているアプリの例のスクリーンショット](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)



## <a name="adding-a-calendar-event"></a>カレンダーイベントの追加

カレンダーデータを読み取る方法についても説明しました。 次に、カレンダーにイベントを追加する方法を見てみましょう。 これを機能させるには、前に`android.permission.WRITE_CALENDAR`説明したアクセス許可を必ず含めてください。 カレンダーにイベントを追加するには、次の操作を行います。

1.  インスタンスを`ContentValues`作成します。
1.  クラスの`CalendarContract.Events.InterfaceConsts`キーを使用して、 `ContentValues`インスタンスにデータを設定します。
1.  イベントの開始時刻と終了時刻のタイムゾーンを設定します。
1.  イベントデータ`ContentResolver`をカレンダーに挿入するには、を使用します。


次のコードは、これらの手順を示しています。

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

タイムゾーンを設定しないと、型`Java.Lang.IllegalArgumentException`の例外がスローされることに注意してください。 イベント時間の値は、エポック以降のミリ秒単位で表す必要`GetDateTimeMS`があるため`EventListActivity`、日付の指定をミリ秒形式に変換するメソッド () を作成します。

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

イベント一覧の UI にボタンを追加し、ボタンの click イベントハンドラーで上記のコードを実行すると、イベントがカレンダーに追加され、次のように一覧で更新されます。

[![カレンダーイベントの後に [サンプルイベントの追加] ボタンを含むサンプルアプリのスクリーンショット](calendar-images/13.png)](calendar-images/13.png#lightbox)

予定表アプリを開くと、イベントも同様に書き込まれます。

[![選択したカレンダーイベントを表示している予定表アプリのスクリーンショット](calendar-images/14.png)](calendar-images/14.png#lightbox)

ご覧のように、Android では、カレンダーデータを取得して保持するための強力で簡単なアクセスが可能になり、アプリケーションが予定表の機能をシームレスに統合できるようになります。


## <a name="related-links"></a>関連リンク

- [カレンダーのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/calendardemo)
- [アイスクリームサンドイッチの導入](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
