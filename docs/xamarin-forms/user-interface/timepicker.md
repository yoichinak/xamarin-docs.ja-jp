---
title: Xamarin.Forms TimePicker
description: TimePicker は、ユーザーが時刻を選択できるようにする Xamarin 形式のビューです。 この記事では、Xamarin.Forms アプリケーションでの TimePicker を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2E99FB23-B82D-4EB4-AFB3-5002E736E7B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2018
ms.openlocfilehash: d5c4cc6600c8192718257abf4ef1cbec49c12eee
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656485"
---
# <a name="xamarinforms-timepicker"></a>Xamarin.Forms TimePicker

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)

_時刻を選択するユーザーを許可する Xamarin.Forms のビュー。_

Xamarin.Forms [ `TimePicker` ](xref:Xamarin.Forms.TimePicker)プラットフォームの時間の選択コントロールを呼び出すし、時刻を選択できます。 `TimePicker` 次のプロパティを定義します。

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) 型の`TimeSpan`、既定値は、選択した時間、 `TimeSpan` 0 です。 `TimeSpan`型が午前 0 時からの経過時間の期間を示します。
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) 型の`string`、[標準](/dotnet/standard/base-types/standard-date-and-time-format-strings/)または[カスタム](/dotnet/standard/base-types/custom-date-and-time-format-strings/).NET の文字列で、既定値は"t"、短い形式の時刻パターンの書式設定します。
- [`TextColor`](xref:Xamarin.Forms.TimePicker.TextColor) 型の[ `Color` ](xref:Xamarin.Forms.Color)、既定値は、選択した時間を表示するために使用する色[ `Color.Default`](xref:Xamarin.Forms.Color.Default)します。
- [`FontAttributes`](xref:Xamarin.Forms.TimePicker.FontAttributes) 型の[ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes)、既定では[ `FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)します。
- [`FontFamily`](xref:Xamarin.Forms.TimePicker.FontFamily) 型の`string`、既定では`null`します。
- [`FontSize`](xref:Xamarin.Forms.TimePicker.FontSize) 型の`double`、既定では-1.0。

これらすべてのプロパティに支えは[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクト、つまりスタイルして、プロパティがデータ バインドの対象になることができます。 [ `Time` ](xref:Xamarin.Forms.TimePicker.Time)プロパティの既定のバインド モードは、 [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay)、つまり、使用するアプリケーションでのデータ バインディングのターゲットにできる、 [モデル-ビュー-ビューモデル (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャ。

[ `TimePicker` ](xref:Xamarin.Forms.TimePicker)を新しい選択を示すイベントを含まない[ `Time` ](xref:Xamarin.Forms.TimePicker.Time)値。 この通知を受ける必要がある場合は、ハンドラーを追加することができます、 [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged)イベント。

## <a name="initializing-the-time-property"></a>時間プロパティの初期化

コードでは、初期化することができます、 [ `Time` ](xref:Xamarin.Forms.TimePicker.Time)プロパティ型の値を`TimeSpan`:

```csharp
TimePicker timePicker = new TimePicker
{
  Time = new TimeSpan(4, 15, 26) // Time set to "04:15:26"
};
```

ときに、 [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) XAML でプロパティを指定するとの値に変換されます、 `TimeSpan` (ミリ秒) の数が、0 以上であること、時間数が 24 時間未満であることを確認する検証済みとします。 時間コンポーネントは、コロンで区切る必要があります。

```xaml
<TimePicker Time="4:15:26" />
```

場合、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)プロパティの[ `TimePicker` ](xref:Xamarin.Forms.TimePicker)型のプロパティを格納している ViewModel のインスタンスに設定されている`TimeSpan`という`SelectedTime`インスタンス化できます (例)。`TimePicker`次のようにします。

```xaml
<TimePicker Time="{Binding SelectedTime}" />
```

この例で、 [ `Time` ](xref:Xamarin.Forms.TimePicker.Time)プロパティに初期化されます、`SelectedTime`ビューモデルのプロパティ。 `Time`プロパティのバインド モードは、 [ `TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay)、新しい時間をユーザーが選択は ViewModel に自動的に反映されます。

場合、 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker)でバインドが含まれていないその[ `Time` ](xref:Xamarin.Forms.TimePicker.Time)プロパティ、アプリケーションはハンドラーをアタッチする必要があります、 [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged)イベント通知されず、ユーザーが新しい時刻を選択します。

フォントのプロパティの設定については、[フォント](~/xamarin-forms/user-interface/text/fonts.md)を参照してください。

## <a name="timepicker-and-layout"></a>TimePicker とレイアウト

制約のない水平レイアウト オプションを使用することは`Center`、 `Start`、または`End`で[ `TimePicker` ](xref:Xamarin.Forms.TimePicker):

```xaml
<TimePicker ···
            HorizontalOptions="Center"
            ··· />
```

ただし、これは推奨されません。 設定に応じて、 [ `Format` ](xref:Xamarin.Forms.TimePicker.Format)プロパティは、選択した時間は、別の表示幅を必要があります。 たとえば、"T"書式指定文字列と、 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker)時間が長い形式で表示を表示し、"4時 15分: 26 AM"が"4時 15分 AM"の短い時刻形式 ("t") よりも大きい値の表示幅が必要です。 この違いがあります、プラットフォームによって、`TimePicker`ビュー レイアウトでは、またはが切り捨てられる表示幅を変更します。

> [!TIP]
> 既定値を使用することをお勧め`HorizontalOptions`の設定`Fill`で[ `TimePicker`](xref:Xamarin.Forms.TimePicker)との幅を使用しないように`Auto`設定時に`TimePicker`で、`Grid`セル。

## <a name="timepicker-in-an-application"></a>アプリケーションで TimePicker

[ **SetTimer** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)サンプルが含まれます[ `TimePicker` ](xref:Xamarin.Forms.TimePicker)、 [ `Entry` ](xref:Xamarin.Forms.Entry)、および[ `Switch` ](xref:Xamarin.Forms.Switch)そのページのビュー。 `TimePicker` 、時刻を選択するために使用して、内のテキストのユーザーに通知する警告ダイアログが表示されますが発生した時刻を時、`Entry`に用意されている、`Switch`がオンにします。 XAML ファイルを次に示します。

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

[ `Entry` ](xref:Xamarin.Forms.Entry)選択した時間が発生したときに表示される通知のテキストを入力することができます。 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker)が割り当てられている、 [ `Format` ](xref:Xamarin.Forms.TimePicker.Format)時間の長い時刻形式の"T"のプロパティ。 アタッチされているイベント ハンドラーを持つ、 [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged)イベント、および[ `Switch` ](xref:Xamarin.Forms.Switch)にハンドラーをアタッチがその[ `Toggled` ](xref:Xamarin.Forms.Switch.Toggled)イベント。 これらのイベント ハンドラーは、分離コード ファイルと呼び出し、`SetTriggerTime`メソッド。

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

`SetTriggerTime`メソッドに基づくタイマーの時間を計算する、`DateTime.Today`プロパティの値と`TimeSpan`から返される値、 [ `TimePicker`](xref:Xamarin.Forms.TimePicker)します。 これは、必要なため、`DateTime.Today`プロパティから返さ、`DateTime`現在の日付を示すが午前 0 時です。 タイマーの時間が既に渡されて今日場合、は、明日に想定が。

タイマー刻みの実行、毎秒、`OnTimerTick`メソッドをチェックするかどうか、 [ `Switch` ](xref:Xamarin.Forms.Switch)がかどうかと、現在の時刻がより大きいまたはタイマーの時間をします。 タイマーの時間が発生したときに、 [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*)メソッドは、アラームとしてユーザーにアラート ダイアログを表示します。

サンプルを最初に実行時に、 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker)ビューは、午前 11 時に初期化されます。 タップすると、`TimePicker`プラットフォーム時刻の選択を呼び出します。 プラットフォームは、非常にさまざまな方法で時刻の選択を実装しますが、それぞれのアプローチは、そのプラットフォームのユーザーにとって馴染み深い。

[![日時選択](timepicker-images/timepicker-open.png "時刻を選択")](timepicker-images/timepicker-open-large.png#lightbox "時刻を選択します")

> [!TIP]
> Android では、 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker)ダイアログをオーバーライドすることでカスタマイズできる、`CreateTimePickerDialog`カスタム レンダラーのメソッド。 これにより、たとえば、ダイアログ ボックスに追加するその他のボタン。

時刻を選択すると、選択した時間が表示されます、 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker):

[![選択された期間](timepicker-images/timepicker-selected.png "選択された期間")](timepicker-images/timepicker-selected-large.png#lightbox "選択された期間")

されるとき、 [ `Switch` ](xref:Xamarin.Forms.Switch)切り替えがオンの位置に、アプリケーションは内のテキストのユーザーを示す警告ダイアログが表示されます、 [ `Entry` ](xref:Xamarin.Forms.Entry)選択した時間が発生したとき。

[![タイマーのポップアップ](timepicker-images/timer-test.png "タイマー ポップアップ")](timepicker-images/timer-test-large.png#lightbox "タイマーのポップアップ")

警告ダイアログが表示されるとすぐに、 [ `Switch` ](xref:Xamarin.Forms.Switch)オフの位置に切り替えられます。

## <a name="related-links"></a>関連リンク

- [SetTimer サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)
- [TimePicker API](xref:Xamarin.Forms.TimePicker)
