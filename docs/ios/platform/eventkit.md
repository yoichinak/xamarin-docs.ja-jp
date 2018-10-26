---
title: Xamarin.iOS で EventKit
description: このドキュメントでは、EventKit と Xamarin.iOS で使用する方法について説明します。 EventKit などを使用したプログラミング時によく使われるクラス調べる予定表、予定表イベント、およびアラームがについて説明します。
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: ea03c8b382e2de29bd20ab1d696d7abb7733e182
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120230"
---
# <a name="eventkit-in-xamarinios"></a>Xamarin.iOS で EventKit

iOS が 2 つのカレンダーに関連のアプリケーション組み込み: カレンダー アプリケーション、およびアラーム アプリケーション。 カレンダー アプリケーションが、予定表のデータを管理する方法を理解するのに十分なと簡単ですが、アラーム アプリケーションはあまり知られていません。 アラームが完了すると、期限になったら、それらの条件に関連する日付を持つことができますなど。IOS が予定表のすべてのデータを格納するカレンダーのイベントまたは通知と呼ばれる 1 つの場所であるかどうかなど、*カレンダー データベース*します。

EventKit フレームワークにアクセスする方法を提供する、*カレンダー*、*カレンダー イベント*と*アラーム*カレンダー データベースに保存されるデータ。 アクセス カレンダーと予定表へのイベントは以来、使用可能な iOS 4 がアラームへのアクセスは iOS 6 の新機能です。

このガイドに対応するいきます。

-   **EventKit 基本**– この EventKit の主要なクラスを使用して基本の部分では紹介し、その使用方法について説明します。 このセクションでは、ドキュメントの次のパートに取り組む前に読む必要です。 
-   **一般的なタスク**– 一般的なタスク セクションが共通のような方法のクイック リファレンスを意図したものは、予定表の列挙、作成、保存および取得するカレンダーのイベントと通知、ほかの組み込みのコント ローラーを使用作成して、カレンダー イベントを変更します。 このセクションが特定のタスクへの参照の意味は、に対して前から後ろは読み取られません必要があります。 


このガイドのすべてのタスクは付属のサンプル アプリケーションで使用できます。

 [![](eventkit-images/01.png "付属のサンプル アプリケーションの画面")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>必要条件

EventKit は iOS 4.0 で導入されましたが、アラーム データへのアクセスは iOS 6.0 で導入されました。 そのため、EventKit の全般的な開発を行う必要がありますを少なくともターゲット バージョン 4.0、および通知の 6.0。

さらに、アラームのアプリケーションでは、アラーム データも使用できないことを最初に追加しない限り、つまり、シミュレーターでは使用できません。 さらに、アクセス要求は、実際のデバイスのユーザーにのみ表示されます。 そのため、EventKit 開発は、デバイスで最適なテストです。

## <a name="event-kit-basics"></a>イベント キットの基礎

EventKit を使用する場合は、一般的なクラスとその使用状況の把握しておく重要です。 検出されたすべてのクラス、`EventKit`と`EventKitUI`(用、 `EKEventEditController`)。

### <a name="eventstore"></a>EventStore

*EventStore* EventKit で操作を実行することが必要なため、クラスは EventKit で最も重要なクラスです。 永続的なストレージ、またはすべての EventKit データ用のデータベース エンジンと考えることができます。 `EventStore`予定表とアプリケーションでは、予定表、予定表イベントおよび通知アラーム アプリケーションへのアクセスがあります。

`EventStore`は、データベース エンジンのような有効期間が長い、つまりことにする必要があります作成し、破棄をできるだけ少なくアプリケーション インスタンスの有効期間中に場合があります。 実際には、ことをお勧めする 1 つのインスタンスを作成すると、`EventStore`アプリケーションでは、保持の周りを参照する、アプリケーションの有効期間にわたってもう一度が必要ない場合を除いて、します。 さらに、すべての呼び出しを行う、1 つに`EventStore`インスタンス。 このため、使用可能な 1 つのインスタンスを保持するシングルトン パターンはお勧めします。

#### <a name="creating-an-event-store"></a>イベント ストアを作成します。

次のコードは、効率的な方法の 1 つのインスタンスを作成する、`EventStore`クラスし、アプリケーション内から静的に使用できるようにします。

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

上記のコードでは、シングルトン パターンを使用してのインスタンスをインスタンス化、`EventStore`アプリケーションが読み込まれた場合。 `EventStore`しからアクセスできるグローバルに、アプリケーション内で次のようにします。

```csharp
App.Current.EventStore;
```

参照するため、すべての例では、ここでは、このパターンを使用します。、`EventStore`を介して`App.Current.EventStore`します。

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>予定表、アラーム データへのアクセスを要求します。

EventStore 経由で任意のデータにアクセスする許可をアプリケーションはまず、予定表イベントのデータまたは必要なものに応じて、アラーム データへのアクセスを要求する必要があります。 これを容易にする、`EventStore`という名前のメソッドを公開します`RequestAccess`を-呼び出されたときに、アプリケーションが、予定表のデータ、またはどのによっては、アラームデータへのアクセスを要求していることを示すユーザーにアラートをアラートビューが表示されます`EKEntityType`渡されます。 呼び出しは非同期であり、として渡された完了ハンドラーを呼び出しますが、アラートの表示を生成するため、 `NSAction` (またはラムダ) に 2 つを受信するパラメーターは、アクセスが許可されたかどうかのブール値と`NSError`であり、not null は場合要求でエラー情報が含まれてください。 など、次のコード化されたは、予定表イベントのデータと表示、要求が許可されていなかった場合にアラートが表示へのアクセスが要求されます。

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

要求が許可されていると、アプリケーションがデバイスにインストールされており、ユーザーに警告されませんがポップアップ記憶されます。
ただし、アクセスがカレンダー イベントまたは付与アラームのいずれかのリソースの種類にのみ与えられます。 アプリケーションでは、両方へのアクセスを必要とする場合両方要求すべきです。

アクセス許可を記憶すると、ために、要求を行うたびに、操作を実行する前に常にアクセスを要求するので、比較的低コストです。

さらに、個別の (非 UI) スレッドでは、完了ハンドラーが呼び出される、ため、完了ハンドラーでは、UI の更新する必要があります経由で呼び出し、 `InvokeOnMainThread`、それ以外の場合は、例外がスローされます、および、キャッチされない場合、アプリケーションがクラッシュします。

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` 型を記述する列挙体は、`EventKit`項目またはデータ。 2 つの値がある:`Event`とアラームを設定します。 さまざまな方法としてで使用される`EventStore.RequestAccess`への通知`EventKit`へのアクセスを取得または取得するデータの種類。

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar*カレンダー イベントのグループを含む予定表を表します。 カレンダーをなど、多くの別の場所に格納できるでローカルに*iCloud*で、サード パーティ製プロバイダーの場所など、 *Exchange Server*または*Google*など。何度も`EKCalendar`を指示する`EventKit`イベントを検索する場所またはそれらを保存する場所。

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController*で見つかる、`EventKitUI`名前空間が組み込みのコント ローラーの編集やカレンダー イベントを作成するために使用できます。 よく組み込みカメラ コント ローラーに似た`EKEventEditController`によって UI を表示して、保存の処理のため、複雑です。

### <a name="ekevent"></a>EKEvent

 *EKEvent*カレンダー イベントを表します。 両方`EKEvent`と`EKReminder`継承`EKCalendarItem`などのフィールドがあると`Title`、`Notes`など。

### <a name="ekreminder"></a>EKReminder

 *EKReminder*アラームの項目を表します。

### <a name="ekspan"></a>EKSpan

*EKSpan*イベントを繰り返すことができますがあり、2 つの値を変更するときにイベントの期間を表す列挙体は、:*含めた*と*FutureEvents*します。 `ThisEvent` すべての変更のみ行われる、参照される系列の特定のイベントには意味`FutureEvents`はそのイベントとすべての将来の定期的なアイテムに影響します。

## <a name="tasks"></a>[タスク]

使いやすく、EventKit 使用量を次のセクションで説明されている、一般的なタスクに分割されています。

### <a name="enumerate-calendars"></a>予定表を列挙します。

ユーザーがデバイスで構成されているカレンダーを列挙するために呼び出す`GetCalendars`上、`EventStore`受信を希望するカレンダー (通知またはイベント) の型を渡します。

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>追加または変更を組み込み、コント ローラーを使用してイベント

*EKEventEditViewController*カレンダー アプリケーションを使用する場合、ユーザーに提示される同じ UI を使用したイベントを作成または更新する場合、面倒な作業の多くをするは。

 [![](eventkit-images/02.png "カレンダー アプリケーションを使用する場合、ユーザーに表示される UI")](eventkit-images/02.png#lightbox)

これを使用するには、それがようにガベージ コレクションがメソッド内で宣言されている場合は、クラス レベル変数として宣言する必要があります。

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

その後を起動する: インスタンス化への参照を指定、`EventStore`を接続する、 *EKEventEditViewDelegate*に委任し、それを使用して表示`PresentViewController`:

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

必要に応じて、イベントを事前に設定する場合は、(下図のように)、新しいイベントを作成するかまたは保存されているイベントを取得することができます。

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

UI を事前に設定する場合は、コント ローラーのイベント プロパティを設定することを確認してください。

```csharp
eventController.Event = newEvent;
```

既存のイベントを使用する、次を参照してください。、 *ID でイベントを取得*後でセクション。

デリゲートをオーバーライドする必要があります、`Completed`メソッドで、ユーザーがダイアログを完了すると、コント ローラーによって呼び出されます。

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

必要に応じて、デリゲートで確認できます、*アクション*で、`Completed`変更イベントを保存し直します、または取り消された場合は、他のものを実行する方法など。

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

コードでイベントを作成するには、使用、 *FromStore*ファクトリ メソッドを`EKEvent`クラス、および任意のデータの設定。

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

保存されているイベントのカレンダーを設定する必要がありますが、基本設定がない場合は、既定値を使用できます。

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

コードで通知の作成は、予定表イベントの作成とほぼ同じ。

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

ID は、そのイベントを取得するために、使用、 *EventFromIdentifier*メソッドを`EventStore`を渡して、`EventIdentifier`イベントから取得します。

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

イベントの場合はその他の 2 つの識別子プロパティしますが、`EventIdentifier`はこの機能を 1 つだけです。

### <a name="retrieving-a-reminder-by-id"></a>ID でアラームを取得します。

アラームを取得する、 *GetCalendarItem*メソッドを`EventStore`を渡して、 *CalendarItemIdentifier*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

`GetCalendarItem`を返します、`EKCalendarItem`にキャストする必要があります、`EKReminder`アラーム データにアクセスしたり、としてインスタンスを使用する必要があるかどうか、`EKReminder`後。

使用しない`GetCalendarItem`の予定表イベント、記事の執筆時点は機能しません。

### <a name="deleting-an-event"></a>イベントの削除

カレンダー イベントを削除する*RemoveEvent*上、`EventStore`イベント、および適切なへの参照を渡すと`EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

注ただし、イベントが削除されると、イベントの参照になります`null`します。

### <a name="deleting-a-reminder"></a>アラームを削除します。

アラームを削除する*RemoveReminder*上、`EventStore`アラームへの参照を渡すとします。

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

上記のコードがあることへのキャストに注意してください`EKReminder`ため、`GetCalendarItem`取得に使用された。

### <a name="searching-for-events"></a>イベントの検索

カレンダー イベントを検索するに作成する必要があります、 *NSPredicate*オブジェクトを使用して、 *PredicateForEvents*メソッドを`EventStore`します。 `NSPredicate`クエリは、一致の検索にその iOS を使用してデータのオブジェクトします。

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

作成した後、`NSPredicate`を使用して、 *EventsMatching*メソッドを`EventStore`:

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

クエリは、同期 (ブロック) と、新しいスレッドまたはタスクを行う、速度を上げたい場合がありますので、クエリによっては、時間をかかる場合がありますに注意してください。

### <a name="searching-for-reminders"></a>アラームの検索

アラームの検索はイベントに似ています、述語が必要ですが、呼び出しは非同期で既にスレッドをブロックについて心配する必要はありませんので。

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

このドキュメントでは、EventKit framework の両方、重要な部分と最も一般的なタスクの数の概要を説明しました。 ただし、EventKit フレームワークの大規模かつ強力ですとされているが生じていないここなどの機能が含まれていますバッチ更新、アラーム、イベントを登録すると、予定表のデータベースに対する変更のリッスンに定期実行の構成を構成する。ジオフェンスと詳細を設定します。  詳細については、Apple を参照してください[カレンダーとアラームのプログラミング ガイド](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)します。


## <a name="related-links"></a>関連リンク

- [カレンダー (サンプル)](https://developer.xamarin.com/samples/monotouch/Calendars/)
- [iOS 6 の概要](~/ios/platform/introduction-to-ios6/index.md)
- [予定表とアラームの概要](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
