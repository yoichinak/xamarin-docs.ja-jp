---
title: Xamarin.Forms TimePicker
description: TimePicker は、ユーザーが時刻を選択できるようにする Xamarin.Forms ビューです。 この記事では、アプリケーションで TimePicker を使用する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 2E99FB23-B82D-4EB4-AFB3-5002E736E7B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2f8452ea36d6fe376880b8882652c59fcb2ed23b
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93371886"
---
# <a name="no-locxamarinforms-timepicker"></a>Xamarin.Forms TimePicker

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)

_Xamarin.Formsユーザーが時刻を選択できるビュー。_

は、 Xamarin.Forms [`TimePicker`](xref:Xamarin.Forms.TimePicker) プラットフォームのタイムピッカーコントロールを呼び出し、ユーザーが時刻を選択できるようにします。 `TimePicker` は次の特性を定義します。

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) 型の `TimeSpan` 場合は、選択された時刻。既定値は `TimeSpan` 0 です。 この `TimeSpan` 型は、午前0時からの期間を示します。
- [`Format`](xref:Xamarin.Forms.TimePicker.Format)`string`[標準](/dotnet/standard/base-types/standard-date-and-time-format-strings/)または[カスタム](/dotnet/standard/base-types/custom-date-and-time-format-strings/)の .net 書式指定文字列。既定では、短い時刻パターンが "t" になります。
- [`TextColor`](xref:Xamarin.Forms.TimePicker.TextColor) 型の [`Color`](xref:Xamarin.Forms.Color) 場合は、選択された時間を表示するために使用される色。既定値は [`Color.Default`](xref:Xamarin.Forms.Color.Default) です。
- [`FontAttributes`](xref:Xamarin.Forms.TimePicker.FontAttributes) 型の [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) 。既定値は [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None) です。
- [`FontFamily`](xref:Xamarin.Forms.TimePicker.FontFamily) 型の `string` 。既定値は `null` です。
- [`FontSize`](xref:Xamarin.Forms.TimePicker.FontSize) 型の `double` 。既定値は-1.0 です。
- `CharacterSpacing`: `double` 型、`TimePicker` テキストの文字間の間隔。

これらのプロパティはすべてオブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これはスタイルを設定でき、プロパティはデータバインディングのターゲットにすることができることを意味します。 [`Time`](xref:Xamarin.Forms.TimePicker.Time)プロパティには、の既定のバインディングモードがあります。これは、 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) [モデルビュービューモデル (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャを使用するアプリケーションで、データバインディングのターゲットにできることを意味します。

には、 [`TimePicker`](xref:Xamarin.Forms.TimePicker) 選択された新しい値を示すイベントは含まれていません [`Time`](xref:Xamarin.Forms.TimePicker.Time) 。 この通知を受け取る必要がある場合は、イベントのハンドラーを追加でき [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) ます。

## <a name="initializing-the-time-property"></a>Time プロパティの初期化

コードでは、 [`Time`](xref:Xamarin.Forms.TimePicker.Time) プロパティを次の型の値に初期化でき `TimeSpan` ます。

```csharp
TimePicker timePicker = new TimePicker
{
  Time = new TimeSpan(4, 15, 26) // Time set to "04:15:26"
};
```

[`Time`](xref:Xamarin.Forms.TimePicker.Time)XAML でプロパティが指定されている場合、値はに変換され、ミリ秒数が0以上であること、 `TimeSpan` および時間数が24未満であることを確認するために検証されます。 時刻部分は、コロンで区切る必要があります。

```xaml
<TimePicker Time="4:15:26" />
```

[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)のプロパティが、 [`TimePicker`](xref:Xamarin.Forms.TimePicker) という名前の型のプロパティを含むビューモデルのインスタンスに設定されている場合 `TimeSpan` `SelectedTime` (たとえば)、次のようにをインスタンス化でき `TimePicker` ます。

```xaml
<TimePicker Time="{Binding SelectedTime}" />
```

この例では、 [`Time`](xref:Xamarin.Forms.TimePicker.Time) プロパティは、ビューモデルのプロパティに初期化され `SelectedTime` ます。 プロパティには `Time` バインドモードがあるため [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) 、ユーザーが選択した新しい時間は、自動的にビューモデルに反映されます。

に [`TimePicker`](xref:Xamarin.Forms.TimePicker) プロパティのバインディングが含まれていない場合 [`Time`](xref:Xamarin.Forms.TimePicker.Time) 、アプリケーションは、 [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) ユーザーが新しい時刻を選択したときに通知されるように、イベントにハンドラーをアタッチする必要があります。

フォントプロパティの設定の詳細については、「 [フォント](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

## <a name="timepicker-and-layout"></a>TimePicker とレイアウト

次のように、、、などの制約のない水平レイアウトオプションを使用することができ `Center` `Start` `End` [`TimePicker`](xref:Xamarin.Forms.TimePicker) ます。

```xaml
<TimePicker ···
            HorizontalOptions="Center"
            ··· />
```

ただし、これは推奨されません。 プロパティの設定によって [`Format`](xref:Xamarin.Forms.TimePicker.Format) は、選択した時間の表示幅が異なる場合があります。 たとえば、"T" 書式指定文字列を使用 [`TimePicker`](xref:Xamarin.Forms.TimePicker) すると、ビューは長い形式で時刻を表示します。 "4:15:26 am" は、"4:15 am" の短い時刻形式 ("T") よりも高い表示幅を必要とします。 プラットフォームによっては、この違いにより、 `TimePicker` ビューがレイアウトの幅を変更したり、表示が切り捨てられたりする可能性があります。

> [!TIP]
> `HorizontalOptions` `Fill` [`TimePicker`](xref:Xamarin.Forms.TimePicker) `Auto` セルにを配置するときは、の `TimePicker` 既定の設定であるを使用し、を使用しないことをお勧めし `Grid` ます。

## <a name="timepicker-in-an-application"></a>アプリケーションでの TimePicker

[**SetTimer**](/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)サンプルには [`TimePicker`](xref:Xamarin.Forms.TimePicker) 、、 [`Entry`](xref:Xamarin.Forms.Entry) 、およびの各ビューがページに含まれてい [`Switch`](xref:Xamarin.Forms.Switch) ます。 は、 `TimePicker` 時間を選択するために使用でき `Entry` ます。また、がオンになっている場合は、のテキストをユーザーに通知する警告ダイアログが表示され `Switch` ます。 XAML ファイルを次に示します。

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

を [`Entry`](xref:Xamarin.Forms.Entry) 使用すると、選択した時間が経過したときに表示されるリマインダーテキストを入力できます。 に [`TimePicker`](xref:Xamarin.Forms.TimePicker) は、 [`Format`](xref:Xamarin.Forms.TimePicker.Format) 長い時刻形式のプロパティ "T" が割り当てられています。 イベントにはイベントハンドラーがアタッチされて [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) おり、には [`Switch`](xref:Xamarin.Forms.Switch) イベントにアタッチされたハンドラーがあり [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) ます。 これらのイベントハンドラーは分離コードファイル内にあり、メソッドを呼び出し `SetTriggerTime` ます。

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

メソッドは、 `SetTriggerTime` `DateTime.Today` プロパティ値と `TimeSpan` から返された値に基づいて、タイマー時間を計算し [`TimePicker`](xref:Xamarin.Forms.TimePicker) ます。 これが必要なのは、 `DateTime.Today` プロパティは現在の日付を示すを返し `DateTime` ますが、時間は午前0時です。 タイマー時刻が既に過ぎている場合は、明日であると見なされます。

タイマー刻みは1秒ごとに、 `OnTimerTick` がオンになっているかどうか、 [`Switch`](xref:Xamarin.Forms.Switch) および現在の時刻がタイマー時刻以上かどうかをチェックするメソッドを実行します。 タイマー時間が経過すると、メソッドは、通知 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) としてユーザーに警告ダイアログを表示します。

このサンプルを最初に実行すると、 [`TimePicker`](xref:Xamarin.Forms.TimePicker) ビューは11am に初期化されます。 をタップすると `TimePicker` 、プラットフォーム時間の選択が呼び出されます。 このプラットフォームでは、さまざまな方法でタイムピッカーが実装されていますが、各アプローチは、そのプラットフォームのユーザーにとってはよく知られています。

[![時間の選択](timepicker-images/timepicker-open.png "時間の選択")](timepicker-images/timepicker-open-large.png#lightbox "時間の選択")

> [!TIP]
> Android では、 [`TimePicker`](xref:Xamarin.Forms.TimePicker) カスタムレンダラーでメソッドをオーバーライドすることによって、ダイアログをカスタマイズでき `CreateTimePickerDialog` ます。 これにより、たとえば、追加のボタンをダイアログに追加することができます。

時刻を選択すると、選択した時刻がに表示され [`TimePicker`](xref:Xamarin.Forms.TimePicker) ます。

[![選択された時刻](timepicker-images/timepicker-selected.png "選択された時刻")](timepicker-images/timepicker-selected-large.png#lightbox "選択された時刻")

を on の [`Switch`](xref:Xamarin.Forms.Switch) 位置に切り替えた場合、アプリケーションは、選択した時間が経過したときにのテキストをユーザーに通知する警告ダイアログを表示し [`Entry`](xref:Xamarin.Forms.Entry) ます。

[![タイマーポップアップ](timepicker-images/timer-test.png "タイマーポップアップ")](timepicker-images/timer-test-large.png#lightbox "タイマーポップアップ")

アラートダイアログが表示されるとすぐに、は [ [`Switch`](xref:Xamarin.Forms.Switch) オフ] の位置に切り替わります。

## <a name="related-links"></a>関連リンク

- [SetTimer サンプル](/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)
- [TimePicker API](xref:Xamarin.Forms.TimePicker)