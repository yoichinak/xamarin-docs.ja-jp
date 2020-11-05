---
title: Xamarin.Forms DatePicker
description: DatePicker は、ユーザーが日付を選択できるようにする Xamarin.Forms ビューです。 この記事では、アプリケーションで DatePicker を使用する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f7cb79b5b90d586980f2e74cd6d2f04b82a8fdf3
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373693"
---
# <a name="no-locxamarinforms-datepicker"></a>Xamarin.Forms DatePicker

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)

_Xamarin.Formsユーザーが日付を選択できるようにするビュー。_

は、 Xamarin.Forms [`DatePicker`](xref:Xamarin.Forms.DatePicker) プラットフォームの日付の選択コントロールを呼び出し、ユーザーが日付を選択できるようにします。 `DatePicker` 次の8つのプロパティを定義します。

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) 型の [`DateTime`](xref:System.DateTime) 。既定では、1900年の最初の日になります。
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) 型の `DateTime` 。既定では、2100年の最終日になります。
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) 型 `DateTime` (選択された日付)。既定値は [`DateTime.Today`](xref:System.DateTime.Today) です。
- [`Format`](xref:Xamarin.Forms.DatePicker.Format)`string`[標準](/dotnet/standard/base-types/standard-date-and-time-format-strings/)または[カスタム](/dotnet/standard/base-types/custom-date-and-time-format-strings/)の .net 書式指定文字列。既定値は "D" で、長い形式の日付パターンです。
- [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor) 型の [`Color`](xref:Xamarin.Forms.Color) 場合は、選択した日付を表示するために使用する色。既定値は [`Color.Default`](xref:Xamarin.Forms.Color.Default) です。
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) 型の [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) 。既定値は [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None) です。
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) 型の `string` 。既定値は `null` です。
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) 型の `double` 。既定値は-1.0 です。
- `CharacterSpacing`: `double` 型、`DatePicker` テキストの文字間の間隔。

は、 `DatePicker` [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) ユーザーが日付を選択したときにイベントを発生させます。

> [!WARNING]
> とを設定する場合 `MinimumDate` `MaximumDate` は、 `MinimumDate` が常に以下であることを確認し `MaximumDate` ます。 それ以外の場合、 `DatePicker` は例外を発生させます。

内部的には、 `DatePicker` はとの間にあることを保証し `Date` `MinimumDate` `MaximumDate` ます。 またはが設定されていない場合は `MinimumDate` `MaximumDate` 、によって `Date` `DatePicker` の値が調整され `Date` ます。

8つのプロパティはすべてオブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これはスタイルを設定でき、プロパティはデータバインディングのターゲットにすることができることを意味します。 `Date`プロパティには、の既定のバインディングモードがあります。これは、 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) [モデルビュービューモデル (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャを使用するアプリケーションで、データバインディングのターゲットにできることを意味します。

## <a name="initializing-the-datetime-properties"></a>DateTime プロパティの初期化

コードでは `MinimumDate` 、、、およびの各プロパティを、 `MaximumDate` `Date` 型の値に初期化でき `DateTime` ます。

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

`DateTime`Xaml で値を指定すると、xaml パーサーはメソッドを引数と共に使用して、 `DateTime.Parse` 文字列を `CultureInfo.InvariantCulture` 値に変換し `DateTime` ます。 日付は、2桁の月、2桁の日、および4桁の年をスラッシュで区切って正確な形式で指定する必要があります。

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

`BindingContext`のプロパティ `DatePicker` が、、、およびという名前の型のプロパティを含むビューモデルのインスタンスに設定されている場合 (たとえば)、次のようにを `DateTime` `MinDate` `MaxDate` `SelectedDate` インスタンス化でき `DatePicker` ます。

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

この例では、3つのプロパティはすべて、ビューモデルの対応するプロパティに初期化されます。 プロパティには `Date` バインドモードがあるため `TwoWay` 、ユーザーが選択した新しい日付は、自動的にビューモデルに反映されます。

に `DatePicker` プロパティのバインディングが含まれていない場合 `Date` 、アプリケーションは、 `DateSelected` ユーザーが新しい日付を選択したときに通知されるように、イベントにハンドラーをアタッチする必要があります。

フォントプロパティの設定の詳細については、「 [フォント](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

## <a name="datepicker-and-layout"></a>DatePicker とレイアウト

次のように、、、などの制約のない水平レイアウトオプションを使用することができ `Center` `Start` `End` `DatePicker` ます。

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

ただし、これは推奨されません。 プロパティの設定によっては `Format` 、選択した日付に異なる表示幅が必要になる場合があります。 たとえば、"D" 書式指定文字列では、が `DateTime` 長い形式で日付を表示し、"水曜日、2018年9月12日" の場合、"金曜, may 4, 2018" よりも高い表示幅が必要になります。 プラットフォームによっては、この違いにより、 `DateTime` ビューがレイアウトの幅を変更したり、表示が切り捨てられたりする可能性があります。

> [!TIP]
> `HorizontalOptions` `Fill` `DatePicker` `Auto` セルにを配置するときは、の `DatePicker` 既定の設定であるを使用し、を使用しないことをお勧めし `Grid` ます。

## <a name="datepicker-in-an-application"></a>アプリケーションでの DatePicker

[**DaysBetweenDates**](/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)サンプルには、ページに2つのビューが含まれてい `DatePicker` ます。 これらは2つの日付を選択するために使用でき、プログラムはそれらの日付の間の日数を計算します。 プログラムでは、プロパティとプロパティの設定は変更されない `MinimumDate` `MaximumDate` ため、2つの日付は 1900 ~ 2100 の範囲である必要があります。

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

各に `DatePicker` は、 `Format` 長い日付形式の "D" プロパティが割り当てられています。 オブジェクトには、 `endDatePicker` そのプロパティをターゲットとするバインディングがあることにも注意 `MinimumDate` してください。 バインディングソースは、オブジェクトの選択された `Date` プロパティです `startDatePicker` 。 これにより、終了日は常に開始日以降になります。 2つのオブジェクトに加えて、に `DatePicker` `Switch` "両方の日の合計を含める" というラベルが付いています。

2つのビューには、 `DatePicker` イベントに関連付けられたハンドラーがあり、 `DateSelected` には `Switch` イベントにアタッチされたハンドラーがあり `Toggled` ます。 これらのイベントハンドラーは分離コードファイル内にあり、次の2つの日付の間の日数の新しい計算をトリガーします。

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

サンプルを最初に実行すると、両方 `DatePicker` のビューが今日の日付に初期化されます。 次のスクリーンショットは、iOS と Android で実行されているプログラムを示しています。

[![日付の開始日の間の日数](datepicker-images/DaysBetweenDatesStart.png "日付の開始日の間の日数")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "日付の開始日の間の日数")

いずれかの表示をタップすると `DatePicker` 、プラットフォームの日付の選択が呼び出されます。 このようなプラットフォームでは、この日付の選択はまったく異なる方法で実装されていますが、各アプローチは、そのプラットフォームのユーザーにはなじみがあります。

[![日付間の日数選択](datepicker-images/DaysBetweenDatesSelect.png "日付間の日数選択")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "日付間の日数選択")

> [!TIP]
> Android では、 `DatePicker` カスタムレンダラーでメソッドをオーバーライドすることによって、ダイアログをカスタマイズでき `CreateDatePickerDialog` ます。 これにより、たとえば、追加のボタンをダイアログに追加することができます。

2つの日付を選択すると、アプリケーションでは、次の日付の間の日数が表示されます。

[![日付の間の日数の結果](datepicker-images/DaysBetweenDatesResult.png "日付の間の日数の結果")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "日付の間の日数の結果")

## <a name="related-links"></a>関連リンク

- [DaysBetweenDates サンプル](/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)
- [DatePicker API](xref:Xamarin.Forms.DatePicker)