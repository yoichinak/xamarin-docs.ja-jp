---
title: 日付選択
description: DatePickerDialog およびビューフラグメントを使用したカレンダーの日付の選択
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/22/2018
ms.openlocfilehash: b54a0795dee0bd7a02b419da497f9a1b3f3ee7ff
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029212"
---
# <a name="android-date-picker"></a>Android 日付の選択

## <a name="overview"></a>概要

ユーザーが Android アプリケーションにデータを入力する必要がある場合があります。 これを支援するために、Android framework には[`DatePicker`](xref:Android.Widget.DatePicker)ウィジェットと[`DatePickerDialog`](xref:Android.App.DatePickerDialog)が用意されています。 `DatePicker` を使用すると、ユーザーはデバイスとアプリケーションの間で一貫したインターフェイスで年、月、日を選択できます。 `DatePickerDialog` は、ダイアログで `DatePicker` をカプセル化するヘルパークラスです。

最新の Android アプリケーションでは、 [`DialogFragment`](xref:Android.App.DialogFragment)に `DatePickerDialog` が表示されます。 これにより、アプリケーションでポップアップダイアログとして DatePicker を表示したり、アクティビティに埋め込んだりすることができます。 さらに、`DialogFragment` は、ダイアログのライフサイクルと表示を管理し、実装する必要があるコードの量を減らします。

このガイドでは、`DialogFragment`でラップされた `DatePickerDialog`の使用方法について説明します。 このサンプルアプリケーションでは、ユーザーがアクティビティのボタンをクリックすると、`DatePickerDialog` がモーダルダイアログとして表示されます。 ユーザーが日付を設定した場合、`TextView` は選択された日付で更新されます。

[[選択日] ボタンの後に [日付の選択] ダイアログボックスを![スクリーンショット](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>［要件］

このガイドのサンプルアプリケーションでは、Android 4.1 (API レベル) を対象としています。
16) 以上。ただし、Android 3.0 (API レベル11以上) に適用されます。 Android サポートライブラリ v4 をプロジェクトに追加し、一部のコードを変更することで、以前のバージョンの Android をサポートすることができます。

## <a name="using-the-datepicker"></a>DatePicker を使用する

このサンプルでは、`DialogFragment`を拡張します。 サブクラスは、`DatePickerDialog`をホストして表示します。

![クローズアップの日付選択ダイアログ](date-picker-images/image-02.png)

ユーザーが日付を選択して **[OK** ] ボタンをクリックすると、`DatePickerDialog` は[`IOnDateSetListener.OnDateSet`](xref:Android.App.DatePickerDialog.IOnDateSetListener.OnDateSet*)メソッドを呼び出します。
このインターフェイスは、ホスティング `DialogFragment`によって実装されます。 ユーザーが **[キャンセル**] ボタンをクリックすると、フラグメントとダイアログが自動的に破棄されます。

`DialogFragment` では、選択した日付をホスティングアクティビティに返す方法がいくつかあります。

1. **メソッドを呼び出すか、またはプロパティを設定**し &ndash; アクティビティは、この値を設定するためのプロパティまたはメソッドを提供できます。

2. **イベントを発生させる**&ndash; `DialogFragment` は `OnDateSet` が呼び出されたときに発生するイベントを定義できます。

3. `DialogFragment` が `Action<DateTime>` を呼び出して、アクティビティの日付を表示する &ndash; **`Action`を使用**します。 アクティビティは、`DialogFragment`をインスタンス化するときに、`Action<DateTime` を提供します。 このサンプルでは3番目の手法を使用し、アクティビティが `DialogFragment`に `Action<DateTime>` を提供する必要があります。

### <a name="extending-dialogfragment"></a>コードフラグメントの拡張

`DatePickerDialog` を表示するための最初の手順は、`DialogFragment` をサブクラス化し、`IOnDateSetListener` インターフェイスを実装することです。

```csharp
public class DatePickerFragment : DialogFragment, 
                                  DatePickerDialog.IOnDateSetListener
{
    // TAG can be any string of your choice.
    public static readonly string TAG = "X:" + typeof (DatePickerFragment).Name.ToUpper();
    
    // Initialize this value to prevent NullReferenceExceptions.
    Action<DateTime> _dateSelectedHandler = delegate { };
    
    public static DatePickerFragment NewInstance(Action<DateTime> onDateSelected)
    {
        DatePickerFragment frag = new DatePickerFragment();
        frag._dateSelectedHandler = onDateSelected;
        return frag;
    }
    
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        DateTime currently = DateTime.Now;
        DatePickerDialog dialog = new DatePickerDialog(Activity, 
                                                       this, 
                                                       currently.Year, 
                                                       currently.Month - 1,
                                                       currently.Day);
        return dialog;
    }
    
    public void OnDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth)
    {
        // Note: monthOfYear is a value between 0 and 11, not 1 and 12!
        DateTime selectedDate = new DateTime(year, monthOfYear + 1, dayOfMonth);
        Log.Debug(TAG, selectedDate.ToLongDateString());
        _dateSelectedHandler(selectedDate);
    }
}
```

`NewInstance` メソッドは、新しい `DatePickerFragment`をインスタンス化するために呼び出されます。 このメソッドは、ユーザーが `DatePickerDialog`の **[OK** ] ボタンをクリックしたときに呼び出される `Action<DateTime>` を受け取ります。

フラグメントを表示すると、Android は `OnCreateDialog`メソッドを呼び出します。 このメソッドは、新しい `DatePickerDialog` オブジェクトを作成し、現在の日付とコールバックオブジェクト (`DatePickerFragment`の現在のインスタンス) を使用して初期化します。

> [!NOTE]
> `IOnDateSetListener.OnDateSet` が呼び出される月の値は、1 ~ 12 ではなく 0 ~ 11 の範囲内であることに注意してください。 月の日にちは、選択された月に応じて 1 ~ 31 の範囲になります。

### <a name="showing-the-datepickerfragment"></a>DatePickerFragment を表示する

`DialogFragment` が実装されたので、このセクションでは、アクティビティでフラグメントを使用する方法を確認します。 このガイドに付属するサンプルアプリでは、アクティビティは `NewInstance` ファクトリメソッドを使用して `DialogFragment` をインスタンス化してから、`DialogFragment.Show`呼び出しを表示します。 `DialogFragment`のインスタンス化の一環として、アクティビティは `Action<DateTime>`を渡します。これにより、アクティビティによってホストされる `TextView` に日付が表示されます。

```csharp
[Activity(Label = "@string/app_name", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
    TextView _dateDisplay;
    Button _dateSelectButton;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);

        _dateDisplay = FindViewById<TextView>(Resource.Id.date_display);
        _dateSelectButton = FindViewById<Button>(Resource.Id.date_select_button);
        _dateSelectButton.Click += DateSelect_OnClick;
    }

    void DateSelect_OnClick(object sender, EventArgs eventArgs)
    {
        DatePickerFragment frag = DatePickerFragment.NewInstance(delegate(DateTime time)
                                                                 {
                                                                     _dateDisplay.Text = time.ToLongDateString();
                                                                 });
        frag.Show(FragmentManager, DatePickerFragment.TAG);
    }
}
```

## <a name="summary"></a>まとめ

このサンプルでは、Android アクティビティの一部として `DatePicker` ウィジェットをポップアップモーダルダイアログとして表示する方法について説明しました。 ここでは、サンプルの表示フラグメントを実装し、`IOnDateSetListener` インターフェイスについて説明しました。 また、このサンプルでは、選択した日付を表示するために、表示フラグメントがホストアクティビティと対話する方法も示しています。

## <a name="related-links"></a>関連リンク

- ["コードフラグメント"](xref:Android.App.DialogFragment)
- [DatePicker](xref:Android.Widget.DatePicker)
- [DatePickerDialog](xref:Android.App.DatePickerDialog)
- [DatePickerDialog.IOnDateSetListener](xref:Android.App.DatePickerDialog.IOnDateSetListener)
- [日付を選択してください](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
