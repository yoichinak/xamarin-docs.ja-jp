---
title: Xamarin.iOS での選択コントロール
description: このドキュメントでは、設計および Xamarin.iOS アプリでピッカー コントロールを操作する方法について説明します。 これには、コード内と iOS Designer での選択を実装する方法について説明します。
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/14/2018
ms.openlocfilehash: 525ddf3c8cfc457738099c3afbb162fd3fb9239b
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233576"
---
# <a name="picker-control-in-xamarinios"></a>Xamarin.iOS での選択コントロール

A [ `UIPickerView` ](xref:UIKit.UIPickerView)ホイールに似たインターフェイスの個々 のコンポーネントをスクロールして一覧から値を選択することになります。

ピッカーが頻繁に使用する日付と時刻を選択しますApple から提供されて、 [`UIDatePicker`](xref:UIKit.UIDatePicker)
この目的のクラスです。

実装して使用する方法について説明します、`UIPickerView`と`UIDatePicker`コントロール。

## <a name="uipickerview"></a>UIPickerView

### <a name="implementing-a-picker"></a>ピッカーを実装します。

新しいをインスタンス化して、ピッカーを実装`UIPickerView`:

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width, 
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
    )
);
```

### <a name="pickers-and-storyboards"></a>ピッカーとストーリー ボード

選択を作成する、 **iOS Designer**、ドラッグ、**ピッカー ビュー**から、**ツールボックス**デザイン サーフェイスにします。

![ピッカーの表示をデザイン画面にドラッグ](picker-images/image1.png "ピッカー ビューをデザイン画面にドラッグします")

### <a name="working-with-a-picker-control"></a>ピッカー コントロールの操作

ピッカーを使用して、_モデル_データと対話します。

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    var pickerModel = new PeopleModel(personLabel);
    personPicker.Model = pickerModel;
}
```

[ `UIPickerViewModel` ](xref:UIKit.UIPickerViewModel)基本クラスは、2 つのインターフェイスを実装します。 [`IUIPickerDataSource`](xref:UIKit.IUIPickerViewDataSource)
[ `IUIPickerViewDelegate`](xref:UIKit.IUIPickerViewDelegate)ピッカーのデータを指定するさまざまなメソッドを宣言して、対話を処理する方法。

```csharp
public class PeopleModel : UIPickerViewModel
{
    public string[] names = new string[] {
            "Amy Burns",
            "Kevin Mullins",
            "Craig Dunn",
            "Joel Martinez",
            "Charles Petzold",
            "David Britch",
            "Mark McLemore",
            "Tom Opegenorth",
            "Joseph Hill",
            "Miguel De Icaza"
        };

    private UILabel personLabel;

    public PeopleModel(UILabel personLabel)
    {
        this.personLabel = personLabel;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 2;
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return names.Length;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        if (component == 0)
            return names[row];
        else
            return row.ToString();
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        personLabel.Text = $"This person is: {names[pickerView.SelectedRowInComponent(0)]},\n they are number {pickerView.SelectedRowInComponent(1)}";
    }

    public override nfloat GetComponentWidth(UIPickerView picker, nint component)
    {
        if (component == 0)
            return 240f;
        else
            return 40f;
    }

    public override nfloat GetRowHeight(UIPickerView picker, nint component)
    {
        return 40f;
    }
```

複数の列を持つことができます、ピッカーまたは_コンポーネント_します。 コンポーネントは、簡単かつより特定のデータを選択できる場合の複数のセクションに、ピッカーをパーティション分割します。

![2 つのコンポーネントでピッカー](picker-images/image3.png "2 つのコンポーネントの選択")

選択では、コンポーネントの数を指定するには、使用、 [`GetComponentCount`](xref:UIKit.UIPickerViewModel.GetComponentCount(UIKit.UIPickerView)) 
メソッドをオーバーライドします。

### <a name="customizing-a-pickers-appearance"></a>選択コントロールの外観のカスタマイズ

ピッカーの外観をカスタマイズする、 [`UIPickerView.UIPickerViewAppearance`](https://developer.xamarin.com/api/type/UIKit.UIPickerView+UIPickerViewAppearance/)
クラスのオーバーライド、 [ `GetView` ](xref:UIKit.UIPickerViewModel.GetView(UIKit.UIPickerView,System.nint,System.nint,UIKit.UIView))と[ `GetRowHeight` ](xref:UIKit.UIPickerViewModel.GetRowHeight(UIKit.UIPickerView,System.nint))メソッド、`UIPickerViewModel`します。

## <a name="uidatepicker"></a>UIDatePicker

### <a name="implementing-a-date-picker"></a>日付の選択を実装します。

日付の選択をインスタンス化する実装を`UIDatePicker`:

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width,
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
     )
);
```

### <a name="date-pickers-and-storyboards"></a>日付の選択とストーリー ボード

日付の選択を作成する、 **iOS Designer**、ドラッグ、**日付選択カレンダー**から、**ツールボックス**デザイン サーフェイスにします。

![日付の選択をデザイン画面にドラッグ](picker-images/image2.png "日付の選択をデザイン画面にドラッグします")

### <a name="date-picker-properties"></a>日付の選択のプロパティ

#### <a name="minimum-and-maximum-date"></a>最小値と日付の最大値

[`MinimumDate`](xref:UIKit.UIDatePicker.MinimumDate) [ `MaximumDate` ](xref:UIKit.UIDatePicker.MaximumDate)日付選択カレンダーで使用できる日付の範囲を制限します。 たとえば、次のコードは、存在の瞬間に至るまでの 60 年に日付の選択を制限します。

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();
components.Year = -60;
NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);
datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

> [!TIP]
> 明示的にキャストすることができます、`DateTime`を`NSDate`:
> ```csharp
> DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
> DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
> ```

#### <a name="minute-interval"></a>分単位の間隔

[ `MinuteInterval` ](xref:UIKit.UIDatePicker.MinuteInterval)プロパティは、ピッカーが分を表示する間隔を設定します。

```csharp
datePickerView.MinuteInterval = 10;
```

#### <a name="mode"></a>モード

日付の選択をサポートして 4 つ[モード](xref:UIKit.UIDatePickerMode)以下を参照します。

##### <a name="uidatepickermodetime"></a>UIDatePickerMode.Time

`UIDatePickerMode.Time` 時刻、時間と分のセレクター関数および省略可能な AM または PM の指定と表示されます。

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

![UIDatePickerMode.Time](picker-images/image8.png "UIDatePickerMode.Time")

##### <a name="uidatepickermodedate"></a>UIDatePickerMode.Date

`UIDatePickerMode.Date` 1 か月、日、年のセレクターの日付が表示されます。

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

![UIDatePickerMode.Date](picker-images/image7.png "UIDatePickerMode.Date")

セレクターの順序は、既定では、システムのロケールを使用する日付の選択のロケールに依存します。 上の図は、セレクターのレイアウトを示します、`en_US`ロケールでは、次の日に順序を変更 |1 か月 |年。

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![1 日 |1 か月 |年](picker-images/image9.png "日 |1 か月 |1 年")

##### <a name="uidatepickermodedateandtime"></a>UIDatePickerMode.DateAndTime

`UIDatePickerMode.DateAndTime` 日付、時間と分、および (を 12 や 24 時間制を使用するかどうか) に応じて省略可能な AM または PM 指定の時間の短縮されたビューが表示されます。

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

![UIDatePickerMode.DateAndTime](picker-images/image6.png "UIDatePickerMode.DateAndTime")

同様[ `UIDatePickerMode.Date`](#uidatepickermodedate)セレクターの順序と、12 や 24 時間制の使用は、日付の選択のロケールに依存します。

> [!TIP]
> 使用して、`Date`モードでの日付の選択の値をキャプチャするプロパティ`UIDatePickerMode.Time`、 `UIDatePickerMode.Date`、または`UIDatePickerMode.DateAndTime`します。 この値は、`NSDate`します。

##### <a name="uidatepickermodecountdowntimer"></a>UIDatePickerMode.CountDownTimer

`UIDatePickerMode.CountDownTimer` 1 時間と分の値が表示されます。

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

!["UIDatePickerMode.CountDownTimer"](picker-images/image5.png "UIDatePickerMode.CountDownTimer")

`CountDownDuration`プロパティの日付の選択の値をキャプチャする`UIDatePickerMode.CountDownTimer`モード。 たとえば、カウント ダウンの値を現在の日付に追加します。

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="nsdateformatter"></a>NSDateFormatter

書式設定、`NSDate`を使用して、 [ `NSDateFormatter`](xref:Foundation.NSDateFormatter)します。

使用する、`NSDateFormatter`を呼び出してその[ `ToString` ](xref:Foundation.NSDateFormatter.ToString(Foundation.NSDate))メソッド。 例:

```csharp
var date = NSDate.Now;
var formatter = new NSDateFormatter();
formatter.DateStyle = NSDateFormatterStyle.Full;
formatter.TimeStyle = NSDateFormatterStyle.Full;
var formattedDate = formatter.ToString(d);
// Tuesday, August 14, 2018 at 11:20:42 PM Mountain Daylight Time
```

##### <a name="dateformat"></a>DateFormat

[ `DateFormat` ](xref:Foundation.NSDateFormatter.DateFormat)のプロパティ (文字列)、`NSDateFormatter`カスタマイズ可能な日付形式の仕様します。

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

##### <a name="timestyle"></a>TimeStyle

[ `TimeStyle` ](xref:Foundation.NSDateFormatter.TimeStyle)プロパティ (、 [ `NSDateFormatterStyle` ](xref:Foundation.NSDateFormatterStyle)の`NSDateFormatter`事前に定義されたスタイルに基づく時刻の書式設定を指定します。

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

さまざまな`NSDateFormatterStyle`値時間が次のように表示されます。

- `NSDateFormatterStyle.Full`:午後 7時 46分: 00 東部夏時間
- `NSDateFormatterStyle.Long`:7時 47分: 00 PM EDT
- `NSDateFormatterStyle.Medium`:7時 47分: 00 PM
- `NSDateFormatterSytle.Short`:7時 47分 PM

##### <a name="datestyle"></a>DateStyle

[ `DateStyle` ](xref:Foundation.NSDateFormatter.DateStyle)プロパティ (、 `NSDateFormatterStyle`) の`NSDateFormatter`事前に定義されたスタイルに基づく日付の書式を指定します。

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

さまざまな`NSDateFormatterStyle`値が次のように日付を表示します。

- `NSDateFormatterStyle.Full`:水曜日、2017 年 8 月 2日 7時 48分 pm
- `NSDateFormatterStyle.Long`:2017 年 8 月 2 日午後 7時 49分
- `NSDateFormatterStyle.Medium`:2017 年 8 月 2 日午後 7時 49分
- `NSDateFormatterStyle.Short`:8 月 2 日、午後 7時 50分

> [!NOTE]
> `DateFormat` `DateStyle` / `TimeStyle`日付と時刻の書式設定を指定するさまざまな方法を提供します。 最近プロパティを設定する日時フォーマッタの出力を決定します。

## <a name="related-links"></a>関連リンク

- [PickerControl (サンプル)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
