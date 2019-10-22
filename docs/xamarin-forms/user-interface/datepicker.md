---
title: DatePicker
description: DatePicker は、ユーザーが日付を選択できるようにする Xamarin 形式のビューです。 この記事では、Xamarin. フォームアプリケーションで DatePicker を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 15d4159d89a463c0335d9c91b24b55151c91de8c
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695908"
---
# <a name="xamarinforms-datepicker"></a>DatePicker

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)

_ユーザーが日付を選択できるようにする Xamarin 形式のビュー。_

Xamarin [`DatePicker`](xref:Xamarin.Forms.DatePicker)は、プラットフォームの日付の選択コントロールを呼び出し、ユーザーが日付を選択できるようにします。 `DatePicker` は、次の8つのプロパティを定義します。

- [`DateTime`](xref:System.DateTime)型の[`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) 。既定では、1900年の最初の日になります。
- `DateTime` 型の[`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) 。既定では、2100年の最後の日付になります。
- `DateTime` 型の[`Date`](xref:Xamarin.Forms.DatePicker.Date) 。選択された日付は、既定で[`DateTime.Today`](xref:System.DateTime.Today)値になります。
- [標準](/dotnet/standard/base-types/standard-date-and-time-format-strings/)または[カスタム](/dotnet/standard/base-types/custom-date-and-time-format-strings/)の .net 書式指定文字列である型 `string` の[`Format`](xref:Xamarin.Forms.DatePicker.Format) 。既定値は "D" で、長い形式の日付パターンです。
- [`Color`](xref:Xamarin.Forms.Color)型の[`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor) 、選択した日付の表示に使用される色。既定では[`Color.Default`](xref:Xamarin.Forms.Color.Default)になります。
- [`FontAttributes`](xref:Xamarin.Forms.FontAttributes)型の[`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) 。既定では[`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)になります。
- `string` 型の[`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) 。既定では `null` になります。
- `double` 型の[`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) 。既定値は-1.0 です。
- `double` 型の `CharacterSpacing` は `DatePicker` テキストの文字間隔です。

@No__t_0 は、ユーザーが日付を選択したときに[`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected)イベントを発生させます。

> [!WARNING]
> @No__t_0 と `MaximumDate` を設定するときは、`MinimumDate` が常に `MaximumDate` 以下であることを確認してください。 それ以外の場合、`DatePicker` は例外を発生させます。

内部的には、`DatePicker` により、`Date` が `MinimumDate` と `MaximumDate` の間にあることが保証されます。 @No__t_0 または `MaximumDate` が設定されて `Date` がこれらの間にない場合は、`DatePicker` によって `Date` の値が調整されます。

8つのプロパティはすべて[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これはスタイルを設定でき、プロパティはデータバインディングのターゲットにすることができることを意味します。 @No__t_0 プロパティには[`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)の既定のバインディングモードがあります。つまり、[モデルビュービューモデル (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャを使用するアプリケーションでデータバインディングのターゲットにすることができます。

## <a name="initializing-the-datetime-properties"></a>DateTime プロパティの初期化

コードでは、`MinimumDate`、`MaximumDate`、および `Date` の各プロパティを `DateTime` 型の値に初期化できます。

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

XAML で `DateTime` 値を指定すると、XAML パーサーは、`DateTime.Parse` メソッドと `CultureInfo.InvariantCulture` 引数を使用して、文字列を `DateTime` 値に変換します。 日付は、2桁の月、2桁の日、および4桁の年をスラッシュで区切って正確な形式で指定する必要があります。

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

@No__t_1 の `BindingContext` プロパティが、`MinDate`、`MaxDate`、`SelectedDate` という名前の型 `DateTime` のプロパティを含むビューモデルのインスタンスに設定されている場合 (たとえば)、次のように `DatePicker` をインスタンス化できます。:

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

この例では、3つのプロパティはすべて、ビューモデルの対応するプロパティに初期化されます。 @No__t_0 プロパティには `TwoWay` のバインドモードがあるため、ユーザーが選択した新しい日付は、自動的にビューモデルに反映されます。

@No__t_0 に `Date` プロパティのバインドが含まれていない場合、アプリケーションは、ユーザーが新しい日付を選択したときに通知されるように、ハンドラーを `DateSelected` イベントにアタッチする必要があります。

フォントプロパティの設定の詳細については、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

## <a name="datepicker-and-layout"></a>DatePicker とレイアウト

@No__t_3 では、`Center`、`Start`、`End` などの制約のない水平レイアウトオプションを使用することができます。

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

ただし、これは推奨されません。 @No__t_0 プロパティの設定によっては、選択した日付に異なる表示幅が必要になる場合があります。 たとえば、"D" 書式指定文字列では、`DateTime` が長い形式で日付を表示し、"水曜日、2018年9月12日" の場合、"金曜, may 4, 2018" よりも高い表示幅が必要になります。 プラットフォームによっては、この違いにより、`DateTime` ビューがレイアウトの幅を変更したり、表示が切り捨てられたりする可能性があります。

> [!TIP]
> @No__t_2 で `Fill` の既定の `HorizontalOptions` 設定を使用することをお勧めします。 `DatePicker` セルに `Grid` を配置する場合は、`Auto` の幅を使用しないことをお勧めします。

## <a name="datepicker-in-an-application"></a>アプリケーションでの DatePicker

[**DaysBetweenDates**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)サンプルには、ページに2つの `DatePicker` ビューが含まれています。 これらは2つの日付を選択するために使用でき、プログラムはそれらの日付の間の日数を計算します。 プログラムは `MinimumDate` と `MaximumDate` のプロパティの設定を変更しないため、2つの日付は1900と2100の間である必要があります。

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

各 `DatePicker` には、長い日付形式の "D" の `Format` プロパティが割り当てられています。 @No__t_0 オブジェクトには、`MinimumDate` プロパティをターゲットとするバインディングがあることにも注意してください。 バインドソースは、`startDatePicker` オブジェクトの選択された `Date` プロパティです。 これにより、終了日は常に開始日以降になります。 2つの `DatePicker` オブジェクトに加えて、`Switch` に "合計日数を含める" というラベルが付いています。

2つの `DatePicker` ビューには、`DateSelected` イベントに関連付けられたハンドラーがあり、`Switch` には、その `Toggled` イベントにアタッチされたハンドラーがあります。 これらのイベントハンドラーは分離コードファイル内にあり、次の2つの日付の間の日数の新しい計算をトリガーします。

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

サンプルを最初に実行すると、`DatePicker` ビューの両方が今日の日付に初期化されます。 次のスクリーンショットは、iOS、Android、ユニバーサル Windows プラットフォームで実行されているプログラムを示しています。

[![日付の開始日の間の日数](datepicker-images/DaysBetweenDatesStart.png "日付の開始日の間の日数")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "日付の開始日の間の日数")

@No__t_0 のいずれかの表示をタップすると、プラットフォームの日付の選択が呼び出されます。 このようなプラットフォームでは、この日付の選択はまったく異なる方法で実装されていますが、各アプローチは、そのプラットフォームのユーザーにはなじみがあります。

[![日付間の日数選択](datepicker-images/DaysBetweenDatesSelect.png "日付間の日数選択")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "日付間の日数選択")

> [!TIP]
> Android では、カスタムレンダラーで `CreateDatePickerDialog` メソッドをオーバーライドすることによって、[`DatePicker`] ダイアログをカスタマイズできます。 これにより、たとえば、追加のボタンをダイアログに追加することができます。

2つの日付を選択すると、アプリケーションでは、次の日付の間の日数が表示されます。

[![日付の間の日数の結果](datepicker-images/DaysBetweenDatesResult.png "日付の間の日数の結果")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "日付の間の日数の結果")

## <a name="related-links"></a>関連リンク

- [DaysBetweenDates サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)
- [DatePicker API](xref:Xamarin.Forms.DatePicker)
