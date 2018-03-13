---
title: EventKit
description: "このガイドでは、アクセスし、EventKit を介して公開されると、カレンダー データベースに格納されているカレンダー、CalendarEvents、および通知のデータを操作する方法の概要を示します。 主要なクラスと EventKit プログラミングだけでなく、EventKit フレームワークに関連付けられている一般的なタスクの数には、その役割を説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a08bc67a9af653a9a646ad62071df0400ce58c12
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="eventkit"></a>EventKit

_このガイドでは、アクセスし、EventKit を介して公開されると、カレンダー データベースに格納されているカレンダー、CalendarEvents、および通知のデータを操作する方法の概要を示します。主要なクラスと EventKit プログラミングだけでなく、EventKit フレームワークに関連付けられている一般的なタスクの数には、その役割を説明します。_

iOS が 2 つのカレンダー関連のアプリケーション組み込み: カレンダー アプリケーションとアラーム アプリケーションです。 カレンダー アプリケーションが、予定表のデータを管理する方法を理解するのに十分な簡単ですが、アラーム アプリケーションはわかりにくくします。 アラームは、期限、完了するとしている場合は、観点でそれらに関連付けられている日付を持つことができますなどです。カレンダー イベントまたはアラームと呼ばれる 1 つの場所であるかどうかに、iOS がすべての予定表データを格納するような場合、*カレンダー データベース*です。

EventKit フレームワークにアクセスする方法を提供する、*カレンダー*、*カレンダー イベント*、および*アラーム*カレンダー データベースに保存されるデータ。 アクセス カレンダーと予定表へのイベントは以来、使用可能な iOS 4 がアラームへのアクセスは iOS 6 の新機能です。

このガイドに対応すること。

-   **EventKit 基礎**– この EventKit の主要なクラス経由での基本部分を導入してその使用方法について説明します。 このセクションでは、ドキュメントの次の部分を取り組む前に読む必要です。 
-   **一般的なタスク**– 一般的なタスク セクションがなどの一般的な事項を行う方法のクイック リファレンスを意図した以外の暦を列挙する、作成、保存および取得するカレンダーのイベント通知と組み込みのコント ローラーを使用して作成して、カレンダー イベントを変更します。 このセクションで必要があります読み取れません前、後ろ、ように特定のタスクへの参照であることを意味していました。 


このガイドのすべてのタスクをコンパニオン サンプル アプリケーションで使用できます。

 [![](eventkit-images/01.png "コンパニオン サンプル アプリケーションの画面")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>必要条件

EventKit は iOS 4.0 で導入されましたが、アラーム データへのアクセスは iOS 6.0 で導入されました。 そのため、全般的な EventKit 開発には、する必要がありますには、少なくとも対象バージョン 4.0、および通知の 6.0。

さらに、アラーム アプリケーションでは、シミュレーターのアラーム データも使用できないことを最初に追加する場合にのみ意味で使用できません。 さらに、アクセス要求は、実際のデバイスのユーザーにのみ表示されます。 そのため、EventKit 開発がデバイスに最適なテストされます。

## <a name="event-kit-basics"></a>イベントのキットの基礎

EventKit を使用する場合は、一般的なクラスとその使用状況を把握しておく重要です。 これらのクラスのすべては含まれて、`EventKit`と`EventKitUI`(用、 `EKEventEditController`)。

### <a name="eventstore"></a>EventStore

*EventStore*クラスは、最も重要なクラスで EventKit EventKit で何らかの操作を実行するために必要なためです。 永続的な記憶域、またはすべての EventKit データ用のデータベース エンジンと考えることができます。 `EventStore`両方、暦カレンダー アプリケーション内のイベントだけでなくやアラーム アプリケーションでの通知へのアクセスがあります。

`EventStore`は、データベース エンジンのような有効期間が長い、する必要がある作成と破棄のアプリケーション インスタンスの有効期間中にできるだけ場合があります。 実際には、ことをお勧めする 1 つのインスタンスを作成すると、`EventStore`アプリケーションでは、おくの周りを参照する、アプリケーション全体の有効期間にわたってする必要はない場合を除いて、します。 さらに、すべての呼び出しを行うを 1 つの`EventStore`インスタンス。 このため、シングルトン パターンが使用可能な 1 つのインスタンスを維持するためお勧めします。

#### <a name="creating-an-event-store"></a>イベント ストアを作成します。

次のコードの 1 つのインスタンスを作成する、効率的な方法を示しています、`EventStore`クラスおよびアプリケーション内から静的に使用できるようにします。

```csharp
public class App
{
    public static App Current {
            get { return current; }
    }
    private static App current;

    public EKEventStore EventStore {
            get { return eventStore; }
    }
    protected EKEventStore eventStore;

    static App ()
    {
            current = new App();
    }
    protected App () 
    {
            eventStore = new EKEventStore ( );
    }
}
```

上記のコードでは、シングルトン パターンを使用のインスタンスを作成して、`EventStore`アプリケーションが読み込まれた場合。 `EventStore`しからアクセスできるグローバルに、アプリケーション内で次のようにします。

```csharp
App.Current.EventStore;
```

ここにすべての例を参照するために、このパターンが使用している注記、`EventStore`を介して`App.Current.EventStore`です。

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>予定表とアラーム データへのアクセスを要求します。

EventStore を介してデータにアクセスが許可されている、前に場合は、アプリケーションに、カレンダー イベントのデータまたは必要に応じて、どちらのアラーム データへのアクセスを要求してする必要があります。 これを容易にするために、`EventStore`と呼ばれるメソッドを公開`RequestAccess`を — 呼び出されたときに —、アラートの表示を指示する、アプリケーションが、予定表データ、またはどのに応じてのアラームデータへのアクセスを要求しているユーザーに表示されます`EKEntityType`に渡されます。 呼び出しが非同期でととして渡された完了ハンドラーが呼び出されます、アラートの表示を生成、ため、 `NSAction` (ラムダ) を受け取る 2 つのパラメーター、アクセスが許可されたかどうかのブール値と`NSError`であり、not null は場合要求内のすべてのエラー情報が含まれてください。 たとえば、次のコード化されたは、カレンダー イベントのデータと表示、要求が許可されていなかった場合にアラートが表示へのアクセスが要求されます。

```csharp
App.Current.EventStore.RequestAccess (EKEntityType.Event, 
    (bool granted, NSError e) => {
            if (granted)
                    //do something here
            else
                    new UIAlertView ( "Access Denied", 
"User Denied Access to Calendar Data", null,
"ok", null).Show ();
            } );
```

要求が与えられた後は、アプリケーションがデバイスにインストールされているし、ユーザーにアラートがポップアップいない限り記憶されます。
ただし、アクセスは、リソース、カレンダー イベントまたは付与アラームのいずれかの型にのみ与えます。 アプリケーションでは、両方へのアクセスを必要とする場合に両方ともを要求する必要があります。

アクセス許可が記憶されているために、比較的負荷の少ないため、要求ごとに、常に、操作を実行する前にアクセスを要求することをお勧めです。

さらに、完了ハンドラーが呼び出されるため、別の (非 UI) スレッドで、完了ハンドラーで UI に加えた変更呼び出す必要がありますを使用して`InvokeOnMainThread`、それ以外の場合、例外がスローされます、およびキャッチされない場合、アプリケーションがクラッシュします。

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` 型を記述する列挙体は、`EventKit`アイテムまたはデータ。 2 つの値がある:`Event`とアラームを設定します。 さまざまなさまざまな方法で使用されている`EventStore.RequestAccess`への通知`EventKit`へのアクセスを取得または取得するデータの種類。

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar*カレンダー イベントのグループを含んだ予定表を表します。 カレンダーをなど、多数の異なる場所に格納できるでローカルに*iCloud*で、サードパーティのプロバイダーの場所など、 *Exchange Server*または*Google*, などです。何度も`EKCalendar`よう通知するために使用`EventKit`に保存する場所やイベントを検索する場所です。

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController*は含まれて、`EventKitUI`名前空間が組み込みのコント ローラーの編集や予定表イベントを作成するために使用できます。 ほぼ同様に組み込み、カメラのコント ローラーで`EKEventEditController`はたいへんで UI を表示および保存を処理します。

### <a name="ekevent"></a>EKEvent

 *EKEvent*予定表イベントを表します。 両方`EKEvent`と`EKReminder`から継承`EKCalendarItem`などのフィールドであると`Title`、`Notes`のようにします。

### <a name="ekreminder"></a>EKReminder

 *EKReminder*アラーム アイテムを表します。

### <a name="ekspan"></a>EKSpan

*EKSpan*繰り返すことができますを 2 つの値を持つイベントを変更するときのイベントの期間を表す列挙体は、:*含めた*と*FutureEvents*です。 `ThisEvent` 一方、変更が、参照される系列内の特定のイベントを発生のみ手段`FutureEvents`はそのイベントとすべての将来の定期的なアイテムに影響します。

## <a name="tasks"></a>[タスク]

使いやすいように、EventKit 使用状況を次のセクションで説明されている共通のタスクに分割されています。

### <a name="enumerate-calendars"></a>予定表を列挙します。

ユーザーがデバイスで構成されているカレンダーを列挙するには、呼び出す`GetCalendars`上、`EventStore`受信を希望するカレンダー (通知またはイベント) の型を渡します。

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>追加または組み込みのコント ローラーを使用してイベントを変更します。

*EKEventEditViewController*は面倒な作業の多くを作成またはカレンダー アプリケーションを使用する場合、ユーザーに提示される同じ UI を使用したイベントを編集する場合。

 [![](eventkit-images/02.png "カレンダー アプリケーションを使用する場合に、ユーザーに表示する UI")](eventkit-images/02.png#lightbox)

これを使用するには、ようにはガベージ コレクト メソッド内で宣言されている場合は、クラス レベルの変数として宣言するします。

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

その後を起動する: それをインスタンス化、そのへの参照、 `EventStore`、上のネットワーク上で、 *EKEventEditViewDelegate*に委任し、その表示を使用して`PresentViewController`:

```csharp
EventKitUI.EKEventEditViewController eventController = 
        new EventKitUI.EKEventEditViewController ();

// set the controller's event store - it needs to know where/how to save the event
eventController.EventStore = App.Current.EventStore;

// wire up a delegate to handle events from the controller
eventControllerDelegate = new CreateEventEditViewDelegate ( eventController );
eventController.EditViewDelegate = eventControllerDelegate;

// show the event controller
PresentViewController ( eventController, true, null );
```

必要に応じて、イベントの事前入力する場合は、(次のように)、新しいイベントを作成するか、または保存されているイベントを取得することができます。

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and exercise!";
newEvent.Notes = "This is your reminder to go and exercise for 30 minutes.”;
```

UI を事前設定する場合は、コント ローラーのイベントのプロパティを設定することを確認してください。

```csharp
eventController.Event = newEvent;
```

既存のイベントを使用するのを参照してください。、 *ID でイベントを取得*セクションの以降に。

デリゲートをオーバーライドする必要があります、`Completed`ダイアログ ボックスで、ユーザーが終了すると、コント ローラーによって呼び出されるメソッド。

```csharp
protected class CreateEventEditViewDelegate : EventKitUI.EKEventEditViewDelegate
{
        // we need to keep a reference to the controller so we can dismiss it
        protected EventKitUI.EKEventEditViewController eventController;

        public CreateEventEditViewDelegate (EventKitUI.EKEventEditViewController eventController)
        {
                // save our controller reference
                this.eventController = eventController;
        }

        // completed is called when a user eith
        public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
        {
                eventController.DismissViewController (true, null);
                }
        }
}
```

必要に応じて、デリゲートで確認できます、*アクション*で、`Completed`イベントおよび保存し直します、変更またはが取り消された場合、他のものを実行する方法など。

```csharp
public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
{
        eventController.DismissViewController (true, null);

        switch ( action ) {

        case EKEventEditViewAction.Canceled:
                break;
        case EKEventEditViewAction.Deleted:
                break;
        case EKEventEditViewAction.Saved:
                // if you wanted to modify the event you could do so here,
// and then save:
                //App.Current.EventStore.SaveEvent ( controller.Event, )
                break;
        }
}
```

### <a name="creating-an-event-programmatically"></a>プログラムによるイベントの作成

コードでイベントを作成するには、使用、 *FromStore*ファクトリ メソッドを`EKEvent`クラス、およびすべてのデータを設定します。

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and do some exercise!";
newEvent.Notes = "This is your motivational event to go and do 30 minutes of exercise. Super important. Do this.";
```

イベントを保存される暦を設定する必要がありますが、基本設定がない場合は、既定値を使用することができます。

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

イベントを保存する、 *SaveEvent*メソッドを`EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

保存した後、 *EventIdentifier*プロパティは、イベントを取得する後で使用できる一意の識別子で更新されます。

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` GUID 文字列です。

### <a name="create-a-reminder-programmatically"></a>アラームをプログラムで作成します。

コードで通知の作成は、カレンダー イベントの作成とほぼ同じです。

```csharp
EKReminder reminder = EKReminder.Create ( App.Current.EventStore );
reminder.Title = "Do something awesome!";
reminder.Calendar = App.Current.EventStore.DefaultCalendarForNewReminders;
```

を保存する、 *SaveReminder*メソッドを、 `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveReminder ( reminder, true, out e );
```

### <a name="retrieving-an-event-by-id"></a>ID でイベントを取得します。

これによって、イベントを取得するが ID を使用して、 *EventFromIdentifier*メソッドを`EventStore`させ、`EventIdentifier`イベントからプルされています。

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

イベントの場合はその他の 2 つのプロパティの識別子が`EventIdentifier`のみであるこの動作をします。

### <a name="retrieving-a-reminder-by-id"></a>ID で通知を取得します。

アラームを取得するを使用して、 *GetCalendarItem*メソッドを`EventStore`させ、 *CalendarItemIdentifier*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

`GetCalendarItem`を返します、`EKCalendarItem`にキャストする必要があります`EKReminder`アラーム データにアクセスしたり、としてインスタンスを使用する必要があるかどうか、`EKReminder`以降。

使用しない`GetCalendarItem`、カレンダー イベントの書き込み時に、これがうまく機能します。

### <a name="deleting-an-event"></a>イベントの削除

予定表イベントを削除するには、呼び出す*RemoveEvent*上、`EventStore`イベント、および適切なへの参照を渡すと`EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

注なりますが、イベントが削除された後、イベントの参照`null`です。

### <a name="deleting-a-reminder"></a>アラームを削除します。

アラームを削除するには、呼び出す*RemoveReminder*上、`EventStore`アラームへの参照を渡すと。

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

上記のコードではへのキャスト`EKReminder`ので、`GetCalendarItem`取得に使用されました。

### <a name="searching-for-events"></a>イベントの検索

カレンダー イベントを検索する必要がありますを作成する、 *NSPredicate*オブジェクトを介して、 *PredicateForEvents*メソッドを`EventStore`です。 `NSPredicate`クエリは、データ オブジェクトの iOS を使用して一致を見つけます。

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

作成すると、`NSPredicate`を使用して、 *EventsMatching*メソッドを`EventStore`:

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

クエリが、同期 (ブロック) から、新しいスレッドまたはそれを実行するタスクを起動することが、クエリによっては、時間がかかる場合がありますに注意してください。

### <a name="searching-for-reminders"></a>アラームの検索

アラームの検索はのようなイベントです。述語が必要ですが、呼び出しが非同期で既にためのスレッドをブロックについて心配する必要はありません。

```csharp
// create our NSPredicate which we'll use for the query
NSPredicate query = App.Current.EventStore.PredicateForReminders ( null );

// execute the query
App.Current.EventStore.FetchReminders (
        query, ( EKReminder[] items ) => {
                // do someting with the items
        } );
```

## <a name="summary"></a>まとめ

このドキュメントでは、EventKit フレームワークの両方、重要な部分と最も一般的なタスクの数の概要を指定します。 ただし、EventKit framework 非常に大きいと、強力なされましたが生じていないここなどの機能が含まれています一括更新を登録すると、カレンダー データベースでの変更をリッスンしているイベントに定期実行の構成アラームを構成する。GeoFences などを設定します。  詳細については、Apple を参照してください[カレンダーとアラーム プログラミング ガイド](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)です。


## <a name="related-links"></a>関連リンク

- [カレンダー (サンプル)](https://developer.xamarin.com/samples/monotouch/Calendars/)
- [iOS 6 の概要](~/ios/platform/introduction-to-ios6/index.md)
- [予定表と通知の概要](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
