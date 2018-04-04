---
title: ピッカー
description: このガイドでは、設計とピッカー Xamarin.iOS アプリで作業について説明します。
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/02/2017
ms.openlocfilehash: e213124e870f1cca96a6078fd26bc7eeb1af55a1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="picker"></a>ピッカー

ピッカー コントロールは、スクロール可能な値を強調表示されている選択した値の一覧を含む 'ホイールに似た' コントロールを表示します。 ユーザーは、必要なオプションを選択するホイールを回転します。

1 つの特定のユーザー場合ピッカーを日付または時刻を設定することです。 この Apple の提供には、カスタム UIDatePicker と呼ばれる UIPickerView クラスのサブクラスを作成しました。

実装して使用を取り上げて、[ピッカー](#picker)と[日付選択カレンダー](#datepicker)コントロール。

<a name="picker" />

## <a name="picker"></a>ピッカー

### <a name="implementing-a-picker"></a>ピッカーを実装します。

ピッカーが、新しいをインスタンス化して実装されている[`UIPickerView`](https://developer.xamarin.com/api/type/UIKit.UIPickerView/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="pickers-and-storyboards"></a>ピッカーおよびストーリー ボード

UI を作成する、iOS デザイナーを使用している場合、ピッカーをツールボックスから、レイアウトに追加できます。

![](picker-images/image1.png)


### <a name="working-with-picker"></a>ピッカーを使用して作業

コードにするかどうかをピッカーを作成またはストーリー ボードなど、使用する必要がありますを割り当てると、_モデル_に渡すしたり、データと対話できるように

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    var pickerModel = new PeopleModel(personLabel);

    personPicker.Model = pickerModel;
}
```

次のコードでは、モデルの例を示します。

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

まず、ユーザー選択のさまざまなオプションを提供するいくつかのデータを渡す必要があります。 できるだけできるだけ短く、この一覧を保持するとき、または 'ダイヤルの 1 つ以上を使用しようとしている必要な場合 (と呼ばれる*コンポーネント*)。

![2 つのコンポーネントの選択](picker-images/image3.png)

コンポーネントの数を設定するには、上書き、`GetComponentCount`メソッド。 

```csharp
public override nint GetComponentCount(UIPickerView pickerView)
{
    return 2;
}
```

戻り値は、ピッカーがダイヤル動作の数を示します。

### <a name="customizing-appearance"></a>外観のカスタマイズ
 
外観、`UIPickerView`を使用してカスタマイズすることができます、`UIPickerView.UIPickerViewAppearance`クラスやオーバーライドによって、`UIPickerViewModel.GetView`と`UIPickerViewModel.GetRowHeight`内のメソッド、`UIPickerViewModel`です。


<a name="datepicker" />

## <a name="date-picker"></a>日付の選択

### <a name="implementing-a-date-picker"></a>日付の選択を実装します。

新しいをインスタンス化して日付選択カレンダーが実装されている[ `UIDatePickerView` ](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="date-pickers-and-storyboards"></a>日付の選択とストーリー ボード

IOS デザイナーを使用して、UI を作成する場合、**日付選択カレンダー**ツールボックスから、レイアウトに追加することができます。 次のプロパティは、プロパティ パッドから調整できます。

![](picker-images/image2.png)

* **モード**– 日付と時刻のモード。 日付、時刻、日付と時刻、またはカウント ダウン タイマーを指定できます。 
* **ロケール**– 日付選択カレンダーのロケールです。 選択**既定**システムの既定値に設定または特定のロケールに設定します。
* **間隔**– クロック オプションが表示されます、増分値を示しています。
* **日付、最小の日付、日付の最大値**– 選択可能な日付の最初の日付、ピッカーが表示されますおよび制約を設定します。

### <a name="configuring-the-datepicker"></a>DatePicker を構成します。

使用して、ユーザーが選択できる日付の範囲を制限することができます、`MinimumDate`と`MaximumDate`プロパティです。 次のコード スニペットは、年前今日 60 の間の範囲を設定する方法の例を示します。

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();

components.Year = -60;

NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);

datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

または、最小値と最大の日付範囲を設定するのに .NET コントロールを使用することもできます。 例えば:

```csharp
DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
```

設定することも、`MinuteInterval`ピッカーが分を表示する間隔を設定するプロパティです。 次のコード スニペットは、10 ごとに設定する分セレクターの設定を使用できます。

```csharp
datePickerView.MinuteInterval = 10;
```

使用して日付選択カレンダー設定できる 4 つのモードがある、 [ `UIDatePicker.Mode` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.Mode/)プロパティです。 以下の一覧は、1 つずつ、およびそれを実装する方法の例を示します。

#### <a name="time"></a>時刻

時モードでは、時間、時間と分のセレクターと省略可能な AM または PM の表記が表示されます。 設定されている、`UIDatePickerMode.Time`プロパティです。 例えば:

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

次の図は、この DatePicker モードの例を示します。

![](picker-images/image8.png)



#### <a name="date"></a>日付

日付のモードでは、月、日、年セレクターと日付を表示します。 設定されている、`UIDatePickerMode.Date`プロパティです。 例えば:

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

次の図は、この DatePicker の例を示します。

![](picker-images/image7.png)

セレクターの順序のロケールによって異なります、`UIDatePicker`です。 既定では、これはシステムの既定値に設定されます。 上記の図のセレクターのレイアウトを示しています、`en_US`を 1 日に変更が、ロケール、|月 |年のレイアウト、ロケールを設定するのに次のコードを使用できます。

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![](picker-images/image9.png)


#### <a name="date-and-time"></a>日付と時刻

日付と時刻のモードを 12 や 24 時間制を使用する場合、上の日付、時間と分、および省略可能な AM または PM 指定 dependings で時間の shortend ビューが表示されます。 設定されている、`UIDatePickerMode.DateAndTime`プロパティです。 例えば:

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

次の図は、この DatePicker の例を示します。

![](picker-images/image6.png)

同様に[日付](#Date)、セレクターの順序と、12 や 24 時間制の使用のロケールに依存します、`UIDatePicker`です。

#### <a name="countdown-timer"></a>カウント ダウン タイマー

カウント ダウン タイマー モードでは、時間と分の値を表示します。 設定されている、`UIDatePickerMode.CountDownTimer`プロパティです。 例えば:

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

次の図は、この DatePicker の例を示します。

![](picker-images/image5.png)

使用することができます、`CountDownDuration`カウント ダウンの日付の選択によって値 dispayed をキャプチャするプロパティです。 たとえば、現在の日付にカウント ダウンの値を追加するには、次のコードを使用でした。

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="formatting"></a>書式設定 

使用して、日付、時刻、および DateAndTime モードの値をキャプチャすることができます、 `Date` 、UIDatePicker プロパティ (例: `datePickerView.Date`)、種類 NSDate があります。 何か詳細人間が判読できるにこの日付の書式を設定するには使用[ `NSDateFormatter`](https://developer.xamarin.com/api/type/Foundation.NSDateFormatter/)です。 次の例では、このクラスで使用可能なプロパティの一部を使用する方法を示します。

`DateFormat`日付を表示する方法を表すに文字列として設定されています。

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

`TimeStyle`プロパティ セット、 `NSDateFormatterStyle`:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

フィールドを`NSDateFormatterStyle`次のように表示します。

![](picker-images/timestyle.png)

`DateStyle`プロパティ セット、 `NSDateFormatterStyle`:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

フィールドを`NSDateFormatterStyle`次のように表示します。

![](picker-images/datestyle.png)

次のコードを使用して、文字列に書式設定された NSDate を出力できます。

```csharp
dateLabel.Text = dateTimeformat.ToString(datePickerView.Date);
```

## <a name="related-links"></a>関連リンク

- [PickerControl (サンプル)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
