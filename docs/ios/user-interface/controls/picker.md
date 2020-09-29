---
title: Xamarin のピッカーコントロール
description: このドキュメントでは、Xamarin iOS アプリでピッカーコントロールをデザインおよび操作する方法について説明します。 コードと iOS デザイナーでピッカーを実装する方法について説明します。
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/14/2018
ms.openlocfilehash: 108d7752c51a6dffd28107b87d29198443827ed2
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436855"
---
# <a name="picker-control-in-xamarinios"></a>Xamarin のピッカーコントロール

を [`UIPickerView`](xref:UIKit.UIPickerView) 使用すると、ホイールのようなインターフェイスの個々のコンポーネントをスクロールすることで、リストから値を選択できます。

ピッカーは、日付と時刻を選択するためによく使用されます。Apple は、 [`UIDatePicker`](xref:UIKit.UIDatePicker)
この目的のためのクラスです。

この記事では、コントロールとコントロールを実装して使用する方法について説明し `UIPickerView` `UIDatePicker` ます。

## <a name="uipickerview"></a>UIPickerView

### <a name="implementing-a-picker"></a>ピッカーの実装

新しいをインスタンス化して、ピッカーを実装し `UIPickerView` ます。

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

### <a name="pickers-and-storyboards"></a>ピッカーとストーリーボード

**IOS デザイナー**でピッカーを作成するには、[**ツールボックス**] から [**ピッカー] ビュー**をデザイン画面にドラッグします。

![デザイン画面にピッカービューをドラッグする](picker-images/image1.png "デザイン画面にピッカービューをドラッグする")

### <a name="working-with-a-picker-control"></a>ピッカーコントロールの操作

ピッカーは、 _モデル_ を使用してデータを操作します。

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    var pickerModel = new PeopleModel(personLabel);
    personPicker.Model = pickerModel;
}
```

[`UIPickerViewModel`](xref:UIKit.UIPickerViewModel)基底クラスは2つのインターフェイスを実装します。[`IUIPickerDataSource`](xref:UIKit.IUIPickerViewDataSource)
[`IUIPickerViewDelegate`](xref:UIKit.IUIPickerViewDelegate)ピッカーのデータとその処理方法を指定するさまざまなメソッドを宣言すると。

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

ピッカーには、複数の列または _コンポーネント_を含めることができます。 コンポーネントは、ピッカーを複数のセクションに分割し、より簡単で特定のデータを選択できるようにします。

![2つのコンポーネントを含むピッカー](picker-images/image3.png "2つのコンポーネントを含むピッカー")

ピッカー内のコンポーネントの数を指定するには、 [`GetComponentCount`](xref:UIKit.UIPickerViewModel.GetComponentCount(UIKit.UIPickerView)) 
メソッドをオーバーライドします。

### <a name="customizing-a-pickers-appearance"></a>ピッカーの外観のカスタマイズ

ピッカーの外観をカスタマイズするには、 [`UIPickerView.UIPickerViewAppearance`](xref:UIKit.UIPickerView.UIPickerViewAppearance)
クラスをオーバーライド [`GetView`](xref:UIKit.UIPickerViewModel.GetView(UIKit.UIPickerView,System.nint,System.nint,UIKit.UIView)) するか [`GetRowHeight`](xref:UIKit.UIPickerViewModel.GetRowHeight(UIKit.UIPickerView,System.nint)) 、のメソッドおよびメソッドをオーバーライドし `UIPickerViewModel` ます。

## <a name="uidatepicker"></a>UIDatePicker

### <a name="implementing-a-date-picker"></a>日付の選択を実装する

をインスタンス化して、日付の選択を実装し `UIDatePicker` ます。

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

### <a name="date-pickers-and-storyboards"></a>日付のピッカーとストーリーボード

**IOS Designer**で日付の選択を作成するには、[**ツールボックス**] から**日付の選択**をデザイン画面にドラッグします。

![日付の選択をデザイン画面にドラッグする](picker-images/image2.png "日付の選択をデザイン画面にドラッグする")

### <a name="date-picker-properties"></a>日付の選択のプロパティ

#### <a name="minimum-and-maximum-date"></a>日付の最小値と最大値

[`MinimumDate`](xref:UIKit.UIDatePicker.MinimumDate)[`MaximumDate`](xref:UIKit.UIDatePicker.MaximumDate)日付選択で使用できる日付の範囲を制限します。 たとえば、次のコードでは、日付の選択が現在までの60年に制限されています。

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();
components.Year = -60;
NSDate minDate = calendar.DateByAddingComponents(components, currentDate, NSCalendarOptions.None);
datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = currentDate;
```

> [!TIP]
> を明示的ににキャストすることができ `DateTime` `NSDate` ます。
>
> ```csharp
> DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
> DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
> ```

#### <a name="minute-interval"></a>分間隔

プロパティは、 [`MinuteInterval`](xref:UIKit.UIDatePicker.MinuteInterval) ピッカーが分を表示する間隔を設定します。

```csharp
datePickerView.MinuteInterval = 10;
```

#### <a name="mode"></a>モード

日付のピッカーでは、次に示す4つの [モード](xref:UIKit.UIDatePickerMode)がサポートされています。

##### <a name="uidatepickermodetime"></a>UIDatePickerMode

`UIDatePickerMode.Time` 時間と分セレクター、およびオプションの AM または PM の指定を使用して時刻を表示します。

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

![UIDatePickerMode](picker-images/image8.png "UIDatePickerMode")

##### <a name="uidatepickermodedate"></a>UIDatePickerMode

`UIDatePickerMode.Date` 月、日、および年のセレクターで日付を表示します。

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

![UIDatePickerMode](picker-images/image7.png "UIDatePickerMode")

セレクターの順序は、日付選択のロケールによって異なります。既定では、システムロケールが使用されます。 上の図は、ロケールのセレクターのレイアウトを示して `en_US` いますが、次のようにして注文を Day | に変更しています。Month |比

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![Day |Month |比](picker-images/image9.png "Day |Month |比")

##### <a name="uidatepickermodedateandtime"></a>UIDatePickerMode

`UIDatePickerMode.DateAndTime` 日付、時刻、時間 (分)、およびオプションの AM または PM の指定 (12 または24時間形式のどちらが使用されているかによって異なる) を表示します。

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

![UIDatePickerMode](picker-images/image6.png "UIDatePickerMode")

と同様に、 [`UIDatePickerMode.Date`](#uidatepickermodedate) セレクターの順序と12または24時間の時刻の使用は、日付の選択のロケールによって異なります。

> [!TIP]
> `Date`モード `UIDatePickerMode.Time` 、、またはで日付選択の値を取得するには、プロパティを使用し `UIDatePickerMode.Date` `UIDatePickerMode.DateAndTime` ます。 この値はとして格納され `NSDate` ます。

##### <a name="uidatepickermodecountdowntimer"></a>UIDatePickerMode

`UIDatePickerMode.CountDownTimer` 時間と分の値を表示します。

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

!["UIDatePickerMode"](picker-images/image5.png "UIDatePickerMode")

プロパティは、 `CountDownDuration` モードで日付選択の値をキャプチャし `UIDatePickerMode.CountDownTimer` ます。 たとえば、現在の日付にカウントダウン値を追加するには、次のようにします。

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="nsdateformatter"></a>NSDateFormatter

の書式を設定するには `NSDate` 、を使用 [`NSDateFormatter`](xref:Foundation.NSDateFormatter) します。

を使用するには `NSDateFormatter` 、 [`ToString`](xref:Foundation.NSDateFormatter.ToString(Foundation.NSDate)) メソッドを呼び出します。 次に例を示します。

```csharp
var date = NSDate.Now;
var formatter = new NSDateFormatter();
formatter.DateStyle = NSDateFormatterStyle.Full;
formatter.TimeStyle = NSDateFormatterStyle.Full;
var formattedDate = formatter.ToString(d);
// Tuesday, August 14, 2018 at 11:20:42 PM Mountain Daylight Time
```

##### <a name="dateformat"></a>DateFormat

[`DateFormat`](xref:Foundation.NSDateFormatter.DateFormat)のプロパティ (文字列) を `NSDateFormatter` 使用すると、カスタマイズ可能な日付形式の指定を行うことができます。

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

##### <a name="timestyle"></a>TimeStyle

[`TimeStyle`](xref:Foundation.NSDateFormatter.TimeStyle)プロパティ (の) は、 [`NSDateFormatterStyle`](xref:Foundation.NSDateFormatterStyle) 事前に定義された `NSDateFormatter` スタイルに基づいて時間書式を指定します。

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

さまざまな値は、次のように `NSDateFormatterStyle` 時刻を表示します。

- `NSDateFormatterStyle.Full`: 7:46:00 PM 東部標準時
- `NSDateFormatterStyle.Long`: 7:47:00 PM EDT
- `NSDateFormatterStyle.Medium`: 7:47:00 PM
- `NSDateFormatterSytle.Short`: 7:47 PM

##### <a name="datestyle"></a>DateStyle

[`DateStyle`](xref:Foundation.NSDateFormatter.DateStyle)のプロパティ () は、 `NSDateFormatterStyle` `NSDateFormatter` 事前に定義されたスタイルに基づいて日付の書式を指定します。

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

さまざまな値は、次のように `NSDateFormatterStyle` 日付を表示します。

- `NSDateFormatterStyle.Full`: 水曜日、2017年8月2日 7:48 PM
- `NSDateFormatterStyle.Long`: 2017 年8月2日 7:49 PM
- `NSDateFormatterStyle.Medium`: 2017 年8月2、、7:49 PM
- `NSDateFormatterStyle.Short`: 8/2/17、7:50 PM

> [!NOTE]
> `DateFormat`と `DateStyle` / `TimeStyle` には、日付と時刻の書式設定を指定するさまざまな方法が用意されています。 最後に設定したプロパティによって、日付フォーマッタの出力が決まります。

## <a name="related-links"></a>関連リンク

- [PickerControl (サンプル)](/samples/xamarin/ios-samples/pickercontrol)