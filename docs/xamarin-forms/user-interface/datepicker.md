---
title: "DatePicker を使用します。"
description: "ユーザーが日付を選択できる Xamarin.Forms ビュー"
ms.topic: article
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/12/2018
ms.openlocfilehash: d47499c1e309fbc67c85b55cacbbba3942188f54
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/16/2018
---
# <a name="using-datepicker"></a>DatePicker を使用します。

_ユーザーが日付を選択できる Xamarin.Forms ビュー_

Xamarin.Forms [ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)プラットフォームの日付選択カレンダー コントロールを呼び出し、日付を選択することができます。 `DatePicker` 5 つのプロパティを定義します。

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) 型の[ `DateTime` ](https://developer.xamarin.com/api/type/System.DateTime/)1900 年の最初の日に既定値です。
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) 型の`DateTime`、どの既定値は、今年の最終日、2100 です。
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) 型の`DateTime`、値の既定値は、選択した日付[ `DateTime.Today`](https://developer.xamarin.com/api/property/System.DateTime.Today/)です。
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) 型の`string`、[標準](/dotnet/standard/base-types/standard-date-and-time-format-strings/)または[カスタム](/dotnet/standard/base-types/custom-date-and-time-format-strings/).NET の既定値は"D"、文字列の書式設定、時間の長い日付パターン。
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.TextColor/) 型の[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)、既定値は、選択した日付の表示に使用する色[ `Color.Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/)です。

`DatePicker`発生、 [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/)イベント、ユーザーが日付を選択します。

> [!WARNING]
> 設定するときに`MinimumDate`と`MaximumDate`、ことを確認して`MinimumDate`は常に以下を`MaximumDate`です。 それ以外の場合、`DatePicker`で例外が発生します。

内部的には、`DatePicker`確実`Date`間`MinimumDate`と`MaximumDate`、包括的です。 場合`MinimumDate`または`MaximumDate`設定されているように`Date`、それらの間ではない`DatePicker`の値を調整`Date`です。

5 つすべてのプロパティが裏付け[ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)オブジェクト、つまり、スタイルを設定できる、し、プロパティがデータ バインドの対象になることができます。 `Date`プロパティがの既定のバインド モード[ `BindingMode.TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/)、つまり、使用するアプリケーションでデータ バインディングのターゲットができること、[モデル View-viewmodel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャです。

## <a name="initializing-the-datetime-properties"></a>DateTime プロパティの初期化

コードでは、初期化することができます、 `MinimumDate`、 `MaximumDate`、および`Date`プロパティ型の値を`DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

ときに、 `DateTime` 、XAML パーサーを使用して XAML では、指定の値、`DateTime.Parse`メソッドを`CultureInfo.InvariantCulture`引数を文字列に変換する、`DateTime`値。 正確な形式で日付を指定する必要があります。 2 桁の月、2 桁の日、およびスラッシュで区切られた 4 桁の年。

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

場合、`BindingContext`プロパティの`DatePicker`型のプロパティを含んでいる ViewModel のインスタンスに設定されている`DateTime`という名前`MinDate`、 `MaxDate`、および`SelectedDate`をインスタンス化 (など)、`DatePicker`次のように:

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

この例では、3 つすべてのプロパティは、ViewModel で対応するプロパティに初期化されます。 `Date`プロパティは、バインディング モードの`TwoWay`ユーザーの選択は、ViewModel に自動的に反映される任意の新しい日付。

場合、`DatePicker`でバインドが含まれていないその`Date`プロパティ、アプリケーションにハンドラーをアタッチする必要があります、`DateSelected`するイベントを通知、ユーザーが新しい日付を選択するとします。

## <a name="datepicker-and-layout"></a>DatePicker とレイアウト

制約なしの水平レイアウト オプションを使用することは`Center`、 `Start`、または`End`で`DatePicker`:

```xaml
<DatePicker ··· 
            HorizontalOptions="Center" 
            ··· />
```

ただし、これは推奨されません。 設定に応じて、`Format`プロパティを選択した日付は、別のディスプレイの幅を必要があります。 たとえば、"D"書式指定文字列が`DateTime`長い形式の場合は、および"水曜日、2018年年 9 月 12日"の日付を表示するには、"金曜日、2018年 5 月 4日"より大きい値の表示幅が必要です。 、プラットフォームによっては、この違いが生じる、`DateTime`レイアウトでは、またはが切り捨てられるディスプレイの幅を変更するビュー。

> [!TIP]
> 既定値を使用することをお勧め`HorizontalOptions`の設定`Fill`で`DatePicker`との幅を使用しないように`Auto`設定時に`DatePicker`で、`Grid`セル。

## <a name="datepicker-in-an-application"></a>アプリケーションで DatePicker

[ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)サンプルでは、2 つ含まれています`DatePicker`そのページ上のビューです。 これらは、2 つの日付を選択に使用することができ、プログラムは、それらの日付までの日数の数を計算します。 プログラムの設定を変更しない、`MinimumDate`と`MaximumDate`プロパティ、2 つの日付は 1900 ~ 2100 でなければなりません。

XAML ファイルを次に示します。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DaysBetweenDates"
             x:Class="DaysBetweenDates.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <StackLayout Margin="10">
        <Label Text="Days Between Dates"
               Style="{DynamicResource TitleStyle}"
               Margin="0, 20"
               HorizontalTextAlignment="Center" />

        <Label Text="Start Date:" />

        <DatePicker x:Name="startDatePicker"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <Label Text="End Date:" />

        <DatePicker x:Name="endDatePicker"
                    MinimumDate="{Binding Source={x:Reference startDatePicker},
                                          Path=Date}"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <StackLayout Orientation="Horizontal"
                     Margin="0, 0, 0, 30">
            <Label Text="Include both days in total: "
                   VerticalOptions="Center" />
            <Switch x:Name="includeSwitch"
                    Toggled="OnSwitchToggled" />
        </StackLayout>

        <Label x:Name="resultLabel"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

各`DatePicker`が割り当てられている、`Format`長い日付形式の"D"のプロパティです。 また、`endDatePicker`オブジェクトを対象とするバインディングにがその`MinimumDate`プロパティです。 バインディング ソースは、選択した`Date`のプロパティ、`startDatePicker`オブジェクト。 これによりを終了日が常に後で、開始日と等しいまたはそれよりもします。 2 つに加えて`DatePicker`、オブジェクト、 `Switch` "Include 両方日の合計"というラベルが付いたです。 

2 つ`DatePicker`ビューにアタッチされているハンドラーがある、`DateSelected`イベント、および`Switch`にハンドラーをアタッチがその`Toggled`イベント。 これらのイベント ハンドラーは、分離コード ファイル内にあるし、2 つの日付間の日数の計算を新しいトリガーします。

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    void OnDateSelected(object sender, DateChangedEventArgs args)
    {
        Recalculate();
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        Recalculate();
    }

    void Recalculate()
    {
        TimeSpan timeSpan = endDatePicker.Date - startDatePicker.Date +
            (includeSwitch.IsToggled ? TimeSpan.FromDays(1) : TimeSpan.Zero);

        resultLabel.Text = String.Format("{0} day{1} between dates",
                                            timeSpan.Days, timeSpan.Days == 1 ? "" : "s");
    }
}
```

このサンプルは、最初の実行時、どちらも`DatePicker`ビューは、今日の日付を初期化します。 次のスクリーン ショットは、iOS、Android、およびユニバーサル Windows プラットフォームで実行されているプログラムを示しています。

[![開始の日付の間の日数](datepicker-images/DaysBetweenDatesStart.png "開始の日付の間の日数")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "開始の日付の間の日数")

いずれかをタップすると、`DatePicker`表示がプラットフォーム日付選択カレンダーを呼び出します。 3 つのプラットフォームでは、この日付の選択を実装、非常に異なる方法では、それぞれの方法は、そのプラットフォームのユーザーになじみ。

[![日付の間の日数を選択](datepicker-images/DaysBetweenDatesSelect.png "の日付の間の日数を選択")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "の日付の間の日数を選択")

2 つの日付を選択した後、アプリケーションは、それらの日付までの日数の数を表示します。

[![日付の結果の間の日数](datepicker-images/DaysBetweenDatesResult.png "日付結果の間の日数")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "日付結果の間の日数")

## <a name="related-links"></a>関連リンク

- [DaysBetweenDates サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [DatePicker API](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)
