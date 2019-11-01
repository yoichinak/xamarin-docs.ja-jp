---
title: Xamarin Android カレンダー
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 3d74e2db541e1f30c7626cd1b08228c1e8f57a42
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029312"
---
# <a name="xamarinandroid-calendar"></a>Xamarin Android カレンダー

## <a name="calendar-api"></a>Calendar API

Android 4 で導入された新しい一連のカレンダー Api は、カレンダープロバイダーに対してデータの読み取りまたは書き込みを行うように設計されたアプリケーションをサポートしています。 これらの Api は、イベント、出席者、リマインダーの読み取りと書き込みなど、カレンダーデータを使用した豊富な相互作用オプションをサポートしています。 アプリケーションで calendar プロバイダーを使用すると、API を通じて追加したデータが、Android 4 に付属する組み込みの予定表アプリに表示されます。

## <a name="adding-permissions"></a>アクセス許可の追加

アプリケーションで新しい calendar Api を使用する場合は、まず、Android マニフェストに適切なアクセス許可を追加する必要があります。 追加する必要のある権限は、カレンダーデータの読み取りと書き込みのどちらを行っているかによって `android.permisson.READ_CALENDAR` および `android.permission.WRITE_CALENDAR`ます。

## <a name="using-the-calendar-contract"></a>カレンダーコントラクトの使用

アクセス許可を設定すると、`CalendarContract` クラスを使用してカレンダーデータを操作できます。 このクラスは、アプリケーションがカレンダープロバイダーと対話するときに使用できるデータモデルを提供します。 `CalendarContract` を使用すると、アプリケーションは、カレンダーやイベントなどの予定表エンティティの Uri を解決できます。 また、カレンダーの名前や ID、イベントの開始日と終了日など、各エンティティのさまざまなフィールドを操作する方法も提供されます。

Calendar API の使用例を見てみましょう。 この例では、カレンダーとそのイベントを列挙する方法と、カレンダーに新しいイベントを追加する方法について説明します。

## <a name="listing-calendars"></a>カレンダーの一覧表示

まず、カレンダーアプリに登録されているカレンダーを列挙する方法について説明します。 これを行うには、`CursorLoader`をインスタンス化します。 Android 3.0 (API 11) で導入された `CursorLoader` は、`ContentProvider`を使用する場合に推奨される方法です。 少なくとも、カレンダーのコンテンツ Uri と返される列を指定する必要があります。この列の指定は_射影_と呼ばれます。

`CursorLoader.LoadInBackground` メソッドを呼び出すことで、カレンダープロバイダーなどのデータのコンテンツプロバイダーに対してクエリを実行できます。
`LoadInBackground` は実際の読み込み操作を実行し、クエリの結果と共に `Cursor` を返します。

`CalendarContract` は、コンテンツ `Uri` と投影の両方を指定するのに役立ちます。 カレンダーを照会するためのコンテンツ `Uri` を取得するには、次のように `CalendarContract.Calendars.ContentUri` プロパティを使用します。

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

`CalendarContract` を使用して、必要な予定表の列を同じように簡単に指定できます。 `CalendarContract.Calendars.InterfaceConsts` クラスのフィールドを配列に追加するだけです。 たとえば、次のコードには、カレンダーの ID、表示名、アカウント名が含まれています。

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

`SimpleCursorAdapter` を使用してデータを UI にバインドする場合は、`Id` を含めることが重要です。これについては、この後すぐに説明します。 コンテンツ Uri と射影が配置されたので、次に示すように、`CursorLoader` をインスタンス化し、`CursorLoader.LoadInBackground` メソッドを呼び出して、カレンダーデータを含むカーソルを返します。

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

この例の UI には `ListView`が含まれており、リスト内の各項目は1つのカレンダーを表します。 次の XML は、`ListView`を含むマークアップを示しています。

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

この時点から、カーソルから UI にデータをバインドするのは、通常の Android コードにすぎません。 次のように `SimpleCursorAdapter` を使用します。

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

上のコードでは、アダプターは `sourceColumns` 配列に指定された列を受け取り、カーソル内の各カレンダーエントリの `targetResources` 配列内のユーザーインターフェイス要素に書き込みます。 ここで使用されるアクティビティは `ListActivity`のサブクラスです。これには、アダプターを設定する `ListAdapter` プロパティが含まれています。

次のスクリーンショットでは、`ListView`にカレンダー情報が表示され、最終的な結果を示しています。

[![CalendarDemo をエミュレーターで実行し、2つのカレンダーエントリを表示する](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)

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

このコードでは、`EventListActivity`型のアクティビティを開こうとして、意図した暦の ID を渡すインテントを作成しています。 イベントを照会する予定表を把握するには ID が必要です。 `EventListActivity`の `OnCreate` メソッドでは、次に示すように `Intent` から ID を取得できます。

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

ここで、このカレンダー ID のイベントをクエリします。 イベントを照会するプロセスは、先に予定表の一覧を照会したときの方法に似ています。今回は `CalendarContract.Events` クラスを使用します。 次のコードでは、イベントを取得するクエリを作成します。

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

このコードでは、まず、`CalendarContract.Events.ContentUri` プロパティからイベントの `Uri` コンテンツを取得します。 次に、イベントプロジェクション配列で取得するイベント列を指定します。
最後に、この情報を使用して `CursorLoader` をインスタンス化し、ローダーの `LoadInBackground` メソッドを呼び出して、イベントデータと共に `Cursor` を返します。

UI でイベントデータを表示するには、予定表の一覧を表示する前と同じように、マークアップとコードを使用します。 ここでも、次のコードに示すように、`SimpleCursorAdapter` を使用してデータを `ListView` にバインドします。

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

このコードと、カレンダーの一覧を表示するために前に使用したコードの主な違いは、行に設定されている `ViewBinder`を使用することです。

```csharp
adapter.ViewBinder = new ViewBinder ();
```

`ViewBinder` クラスを使用すると、値をビューにバインドする方法をさらに制御できます。 この場合、次の実装に示すように、このメソッドを使用して、イベントの開始時刻をミリ秒から日付文字列に変換します。

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

[![3 つのカレンダーイベントを表示するアプリ例のスクリーンショット](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)

## <a name="adding-a-calendar-event"></a>カレンダーイベントの追加

カレンダーデータを読み取る方法についても説明しました。 次に、カレンダーにイベントを追加する方法を見てみましょう。 これを機能させるには、前に説明した `android.permission.WRITE_CALENDAR` のアクセス許可を必ず含めてください。 カレンダーにイベントを追加するには、次の操作を行います。

1. `ContentValues` インスタンスを作成します。
1. `CalendarContract.Events.InterfaceConsts` クラスのキーを使用して、`ContentValues` インスタンスを設定します。
1. イベントの開始時刻と終了時刻のタイムゾーンを設定します。
1. イベントデータをカレンダーに挿入するには、`ContentResolver` を使用します。

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

タイムゾーンを設定しなかった場合は、`Java.Lang.IllegalArgumentException` 型の例外がスローされることに注意してください。 イベント時刻値は、エポック以降のミリ秒単位で表す必要があるため、`GetDateTimeMS` メソッド (`EventListActivity`) を作成して、日付の仕様をミリ秒形式に変換します。

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

[カレンダーイベントを含むサンプルアプリのスクリーンショット![[サンプルイベントの追加] ボタン](calendar-images/13.png)](calendar-images/13.png#lightbox)

予定表アプリを開くと、イベントも同様に書き込まれます。

[選択したカレンダーイベントを表示している予定表アプリのスクリーンショット![](calendar-images/14.png)](calendar-images/14.png#lightbox)

ご覧のように、Android では、カレンダーデータを取得して保持するための強力で簡単なアクセスが可能になり、アプリケーションが予定表の機能をシームレスに統合できるようになります。

## <a name="related-links"></a>関連リンク

- [カレンダーのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/calendardemo)
- [アイスクリームサンドイッチの導入](https://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
