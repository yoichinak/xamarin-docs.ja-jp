---
title: Xamarin. フォーム TimePicker
description: TimePicker は、ユーザーが時刻を選択できるようにする Xamarin 形式のビューです。 この記事では、Xamarin. フォームアプリケーションで TimePicker を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2E99FB23-B82D-4EB4-AFB3-5002E736E7B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2018
ms.openlocfilehash: aae0791199b0e3042a3c619fcb11e7b877f52012
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695924"
---
# <a name="xamarinforms-timepicker"></a>Xamarin. フォーム TimePicker

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)

_ユーザーが時刻を選択できるようにする Xamarin 形式のビュー。_

Xamarin [`TimePicker`](xref:Xamarin.Forms.TimePicker)は、プラットフォームのタイムピッカーコントロールを呼び出し、ユーザーが時刻を選択できるようにします。 `TimePicker` は、次のプロパティを定義します。

- `TimeSpan` 型の[`Time`](xref:Xamarin.Forms.TimePicker.Time) 、選択された時刻。既定値は 0 `TimeSpan` になります。 @No__t_0 の種類は、深夜からの時間を示します。
- `string` 型の[`Format`](xref:Xamarin.Forms.TimePicker.Format)は、[標準](/dotnet/standard/base-types/standard-date-and-time-format-strings/)または[カスタム](/dotnet/standard/base-types/custom-date-and-time-format-strings/)の .net 書式指定文字列であり、既定では "t" で、短い時刻パターンが使用されます。
- [`Color`](xref:Xamarin.Forms.Color)型の[`TextColor`](xref:Xamarin.Forms.TimePicker.TextColor) 、選択した時間の表示に使用される色。既定では[`Color.Default`](xref:Xamarin.Forms.Color.Default)になります。
- [`FontAttributes`](xref:Xamarin.Forms.FontAttributes)型の[`FontAttributes`](xref:Xamarin.Forms.TimePicker.FontAttributes) 。既定では[`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)になります。
- `string` 型の[`FontFamily`](xref:Xamarin.Forms.TimePicker.FontFamily) 。既定では `null` になります。
- `double` 型の[`FontSize`](xref:Xamarin.Forms.TimePicker.FontSize) 。既定値は-1.0 です。
- `double` 型の `CharacterSpacing` は `TimePicker` テキストの文字間隔です。

これらのプロパティはすべて[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。つまり、スタイル設定が可能であり、プロパティはデータバインディングのターゲットにすることができます。 [@No__t_1](xref:Xamarin.Forms.TimePicker.Time)プロパティには[`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)の既定のバインディングモードがあります。つまり、モデルビュービューモデル[(MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャを使用するアプリケーションでデータバインディングのターゲットにすることができます。

[@No__t_1](xref:Xamarin.Forms.TimePicker)には、選択された新しい[`Time`](xref:Xamarin.Forms.TimePicker.Time)値を示すイベントは含まれていません。 この通知を受け取る必要がある場合は、 [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged)イベントのハンドラーを追加できます。

## <a name="initializing-the-time-property"></a>Time プロパティの初期化

コードでは、 [`Time`](xref:Xamarin.Forms.TimePicker.Time)プロパティを `TimeSpan` 型の値に初期化できます。

```csharp
TimePicker timePicker = new TimePicker
{
  Time = new TimeSpan(4, 15, 26) // Time set to "04:15:26"
};
```

XAML で[`Time`](xref:Xamarin.Forms.TimePicker.Time)プロパティが指定されている場合、値は `TimeSpan` に変換され、ミリ秒数が0以上であること、および時間数が24未満であることを確認するために検証されます。 時刻部分は、コロンで区切る必要があります。

```xaml
<TimePicker Time="4:15:26" />
```

[@No__t_3](xref:Xamarin.Forms.TimePicker)の[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)プロパティが、`SelectedTime` という名前の `TimeSpan` 型のプロパティを含むビューモデルのインスタンスに設定されている場合 (たとえば)、次のように `TimePicker` をインスタンス化できます。

```xaml
<TimePicker Time="{Binding SelectedTime}" />
```

この例では、 [`Time`](xref:Xamarin.Forms.TimePicker.Time)プロパティは、ビューモデルの `SelectedTime` プロパティに初期化されます。 @No__t_0 プロパティには[`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)のバインドモードがあるため、ユーザーが選択した新しい時間は、自動的にビューモデルに反映されます。

[@No__t_1](xref:Xamarin.Forms.TimePicker)に[`Time`](xref:Xamarin.Forms.TimePicker.Time)プロパティのバインドが含まれていない場合、アプリケーションは、ユーザーが新しい時刻を選択したときに通知されるように、ハンドラーを[`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged)イベントにアタッチする必要があります。

フォントプロパティの設定の詳細については、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

## <a name="timepicker-and-layout"></a>TimePicker とレイアウト

[@No__t_4](xref:Xamarin.Forms.TimePicker)では、`Center`、`Start`、`End` などの制約のない水平レイアウトオプションを使用することができます。

```xaml
<TimePicker ···
            HorizontalOptions="Center"
            ··· />
```

ただし、これは推奨されません。 [@No__t_1](xref:Xamarin.Forms.TimePicker.Format)プロパティの設定によっては、選択した時間の表示幅が異なる場合があります。 たとえば、"T" 書式指定文字列を使用すると、 [`TimePicker`](xref:Xamarin.Forms.TimePicker)ビューでは時間が長い形式で表示され、"4:15:26 am" は "4:15 am" の短い時刻形式 ("T") よりも高い表示幅を必要とします。 プラットフォームによっては、この違いにより、`TimePicker` ビューがレイアウトの幅を変更したり、表示が切り捨てられたりする可能性があります。

> [!TIP]
> [@No__t_3](xref:Xamarin.Forms.TimePicker)で `Fill` の既定の `HorizontalOptions` 設定を使用することをお勧めします。 `TimePicker` セルに `Grid` を配置する場合は、`Auto` の幅を使用しないことをお勧めします。

## <a name="timepicker-in-an-application"></a>アプリケーションでの TimePicker

[**SetTimer**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)サンプルには、ページ上の[`TimePicker`](xref:Xamarin.Forms.TimePicker)、 [`Entry`](xref:Xamarin.Forms.Entry)、および[`Switch`](xref:Xamarin.Forms.Switch)ビューが含まれています。 @No__t_0 を使用して時刻を選択することができます。この時間が経過すると、`Switch` がオンになっている場合に、`Entry` 内のテキストをユーザーに通知する警告ダイアログが表示されます。 XAML ファイルを次に示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SetTimer"
             x:Class="SetTimer.MainPage">
    <StackLayout>
        ...
        <Entry x:Name="_entry"
               Placeholder="Enter event to be reminded of" />
        <Label Text="Select the time below to be reminded at." />
        <TimePicker x:Name="_timePicker"
                    Time="11:00:00"
                    Format="T"
                    PropertyChanged="OnTimePickerPropertyChanged" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Enable timer:" />
            <Switch x:Name="_switch"
                    HorizontalOptions="EndAndExpand"
                    Toggled="OnSwitchToggled" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

[@No__t_1](xref:Xamarin.Forms.Entry)を使用すると、選択した時間が経過したときに表示されるリマインダーテキストを入力できます。 [@No__t_1](xref:Xamarin.Forms.TimePicker)には、長い形式の "t" の[`Format`](xref:Xamarin.Forms.TimePicker.Format)プロパティが割り当てられています。 これには[`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged)イベントにアタッチされたイベントハンドラーがあり、 [`Switch`](xref:Xamarin.Forms.Switch)には[`Toggled`](xref:Xamarin.Forms.Switch.Toggled)イベントにアタッチされたハンドラーがあります。 これらのイベントハンドラーは分離コードファイル内にあり、`SetTriggerTime` メソッドを呼び出します。

```csharp
public partial class MainPage : ContentPage
{
    DateTime _triggerTime;

    public MainPage()
    {
        InitializeComponent();

        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimerTick);
    }

    bool OnTimerTick()
    {
        if (_switch.IsToggled && DateTime.Now >= _triggerTime)
        {
            _switch.IsToggled = false;
            DisplayAlert("Timer Alert", "The '" + _entry.Text + "' timer has elapsed", "OK");
        }
        return true;
    }

    void OnTimePickerPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        if (args.PropertyName == "Time")
        {
            SetTriggerTime();
        }
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        SetTriggerTime();
    }

    void SetTriggerTime()
    {
        if (_switch.IsToggled)
        {
            _triggerTime = DateTime.Today + _timePicker.Time;
            if (_triggerTime < DateTime.Now)
            {
                _triggerTime += TimeSpan.FromDays(1);
            }
        }
    }
}
```

@No__t_0 メソッドは、`DateTime.Today` プロパティ値と[`TimePicker`](xref:Xamarin.Forms.TimePicker)から返された `TimeSpan` 値に基づいて、タイマー時間を計算します。 これが必要なのは、`DateTime.Today` プロパティは現在の日付を示す `DateTime` を返しますが、時間は午前0時です。 タイマー時刻が既に過ぎている場合は、明日であると見なされます。

タイマー刻みは、1秒ごとに、 [`Switch`](xref:Xamarin.Forms.Switch)があるかどうか、および現在の時刻がタイマー時間以上であるかどうかをチェックする `OnTimerTick` メソッドを実行します。 タイマー時間が経過すると、 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)メソッドによって、通知としてユーザーに警告ダイアログが表示されます。

このサンプルを最初に実行すると、 [`TimePicker`](xref:Xamarin.Forms.TimePicker)ビューが11am に初期化されます。 @No__t_0 をタップすると、プラットフォーム時間の選択が呼び出されます。 このプラットフォームでは、さまざまな方法でタイムピッカーが実装されていますが、各アプローチは、そのプラットフォームのユーザーにとってはよく知られています。

[![時間の選択](timepicker-images/timepicker-open.png "時間の選択")](timepicker-images/timepicker-open-large.png#lightbox "時間の選択")

> [!TIP]
> Android では、カスタムレンダラーで `CreateTimePickerDialog` メソッドをオーバーライドすることによって、[ [`TimePicker`](xref:Xamarin.Forms.TimePicker) ] ダイアログをカスタマイズできます。 これにより、たとえば、追加のボタンをダイアログに追加することができます。

時刻を選択すると、選択した時刻が[`TimePicker`](xref:Xamarin.Forms.TimePicker)に表示されます。

[![選択された時刻](timepicker-images/timepicker-selected.png "選択された時刻")](timepicker-images/timepicker-selected-large.png#lightbox "選択された時刻")

[@No__t_1](xref:Xamarin.Forms.Switch)がオンの位置に切り替えられると、アプリケーションは、選択された時間が発生したときに[`Entry`](xref:Xamarin.Forms.Entry)内のテキストをユーザーに通知するアラートダイアログを表示します。

[![タイマーポップアップ](timepicker-images/timer-test.png "タイマーポップアップ")](timepicker-images/timer-test-large.png#lightbox "タイマーポップアップ")

アラートダイアログが表示されるとすぐに、 [`Switch`](xref:Xamarin.Forms.Switch)は [オフ] の位置に切り替わります。

## <a name="related-links"></a>関連リンク

- [SetTimer サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)
- [TimePicker API](xref:Xamarin.Forms.TimePicker)
