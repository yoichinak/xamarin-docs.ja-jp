---
title: Xamarin.Forms DatePicker
description: DatePicker は、ユーザーが日付を選択できる Xamarin.Forms のビューです。 この記事では、Xamarin.Forms アプリケーションでの DatePicker を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 104985a4221cc6e5e6aa56420c1a41ee4c736b8d
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563694"
---
# <a name="xamarinforms-datepicker"></a>Xamarin.Forms DatePicker

_ユーザーが日付を選択できる Xamarin.Forms のビュー。_

Xamarin.Forms [ `DatePicker` ](xref:Xamarin.Forms.DatePicker)プラットフォームの日付の選択コントロールを呼び出すし、ユーザーは日付を選択できます。 `DatePicker` 8 つのプロパティを定義します。

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) 型の[ `DateTime` ](xref:System.DateTime)、既定では 1900 年の最初の日。
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) 型の`DateTime`2100 年の最終日にどの既定値。
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) 型の`DateTime`、既定値を選択した日付[ `DateTime.Today`](xref:System.DateTime.Today)します。
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) 型の`string`、[標準](/dotnet/standard/base-types/standard-date-and-time-format-strings/)または[カスタム](/dotnet/standard/base-types/custom-date-and-time-format-strings/)書式設定文字列で、既定値は"D"に、.NET、時間の長い日付パターン。
- [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor) 型の[ `Color` ](xref:Xamarin.Forms.Color)、既定値は、選択した日付を表示するために使用する色[ `Color.Default`](xref:Xamarin.Forms.Color.Default)します。
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) 型の[ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes)、既定では[ `FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)します。
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) 型の`string`、既定では`null`します。
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) 型の`double`、既定では-1.0。

`DatePicker`発生、 [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected)イベント、ユーザーが日付を選択します。

> [!WARNING]
> 設定するときに`MinimumDate`と`MaximumDate`、ことを確認します`MinimumDate`が常に等しいまたはそれよりも小さい`MaximumDate`します。 それ以外の場合、`DatePicker`で例外が発生します。

内部的には、`DatePicker`により`Date`間`MinimumDate`と`MaximumDate`までの値。 場合`MinimumDate`または`MaximumDate`設定されているように`Date`、それらの間でない`DatePicker`の値を調節`Date`します。

8 つすべてのプロパティが支え[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクト、つまりスタイルして、プロパティがデータ バインドの対象になることができます。 `Date`プロパティの既定のバインド モードは、 [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay)、つまり、使用するアプリケーションでのデータ バインディングのターゲットにできる、[モデル-ビュー-ビューモデル (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャです。

## <a name="initializing-the-datetime-properties"></a>DateTime プロパティの初期化

コードでは、初期化することができます、 `MinimumDate`、 `MaximumDate`、および`Date`プロパティを型の値に`DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

ときに、 `DateTime` XAML パーサーを使用して、XAML で値が指定された、`DateTime.Parse`メソッドを`CultureInfo.InvariantCulture`を文字列に変換する引数を`DateTime`値。 正確な形式で日付を指定する必要があります。 2 桁の月、2 桁の日、スラッシュで区切られた 4 桁の年。

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

場合、`BindingContext`プロパティの`DatePicker`型のプロパティを含んでいる ViewModel のインスタンスに設定されている`DateTime`という`MinDate`、`MaxDate`と`SelectedDate`インスタンス化できます (たとえば)、`DatePicker`このような:

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

この例では、3 つすべてのプロパティは、ViewModel で対応するプロパティに初期化されます。 `Date`プロパティのバインド モードは、 `TwoWay`、任意の新しい日付をユーザーが選択は、ViewModel で自動的に反映されます。

場合、`DatePicker`でバインドが含まれていないその`Date`プロパティ、アプリケーションはハンドラーをアタッチする必要があります、`DateSelected`するイベント通知、ユーザーが新しい日付を選択するとします。

フォントのプロパティの設定については、次を参照してください。[フォント](~/xamarin-forms/user-interface/text/fonts.md)します。

## <a name="datepicker-and-layout"></a>DatePicker とレイアウト

制約のない水平レイアウト オプションを使用することは`Center`、 `Start`、または`End`で`DatePicker`:

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

ただし、これは推奨されません。 設定に応じて、`Format`プロパティは、選択した日付が別の表示幅を必要があります。 たとえば、"D"書式指定文字列と`DateTime`長い形式の場合は、および「水曜日 2018 年 9 月 12日、日」の日付を表示するには、「Friday、4、2018 年 5 月」より大きい値の表示幅が必要です。 この違いがあります、プラットフォームによって、`DateTime`ビュー レイアウトでは、またはが切り捨てられる表示幅を変更します。

> [!TIP]
> 既定値を使用することをお勧め`HorizontalOptions`の設定`Fill`で`DatePicker`との幅を使用しないように`Auto`設定時に`DatePicker`で、`Grid`セル。

## <a name="datepicker-in-an-application"></a>アプリケーションで DatePicker

[ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)サンプルでは、2 つ含まれています`DatePicker`そのページのビュー。 これら 2 つの日付の選択に使用でき、プログラムは、それらの日付間の日数を計算します。 プログラムの設定を変更しない、`MinimumDate`と`MaximumDate`プロパティ、2 つの日付は 1900 ~ 2100 でなければなりません。

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

各`DatePicker`が割り当てられている、`Format`長い日付形式の"D"のプロパティ。 また、`endDatePicker`オブジェクトを対象とするバインディングにはその`MinimumDate`プロパティ。 バインディング ソースが、選択されている`Date`のプロパティ、`startDatePicker`オブジェクト。 これによりを終了日は常に後で、開始日と等しいまたはそれよりも。 2 つだけでなく`DatePicker`、オブジェクト、 `Switch` 「合計にどちらの日を含める」というラベルの付いたします。

2 つ`DatePicker`ビューにアタッチされたハンドラーがある、`DateSelected`イベント、および`Switch`にハンドラーをアタッチがその`Toggled`イベント。 これらのイベント ハンドラーは、分離コード ファイルには、2 つの日付までの日数の新しい計算をトリガーします。

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

このサンプルは、最初の実行時、両方`DatePicker`ビューは、今日の日付に初期化されます。 次のスクリーン ショットは、iOS、Android、およびユニバーサル Windows プラットフォームで実行されているプログラムを示しています。

[![開始の日付間の日数](datepicker-images/DaysBetweenDatesStart.png "開始の日付間の日数")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "開始の日付間の日数")

いずれかをタップすると、`DatePicker`が表示されますが、プラットフォームの日付の選択を呼び出します。 3 つのプラットフォームで非常にさまざまな方法は、この日付の選択の実装が、それぞれのアプローチは、そのプラットフォームのユーザーにとって馴染み深い。

[![日付間の日数を選択](datepicker-images/DaysBetweenDatesSelect.png "の日付間の日数を選択")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "の日付間の日数を選択します")

> [!TIP]
> Android では、`DatePicker`ダイアログをオーバーライドすることでカスタマイズできる、`CreateDatePickerDialog`カスタム レンダラーのメソッド。 これにより、たとえば、ダイアログ ボックスに追加するその他のボタン。

2 つの日付を選択した後、アプリケーションは、それらの日付間の日数を表示します。

[![結果の日付間の日数](datepicker-images/DaysBetweenDatesResult.png "結果の日付間の日数")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "結果の日付間の日数")

## <a name="related-links"></a>関連リンク

- [DaysBetweenDates サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [DatePicker API](xref:Xamarin.Forms.DatePicker)
