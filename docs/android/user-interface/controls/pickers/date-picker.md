---
title: 日付選択
description: DatePickerDialog およびビューフラグメントを使用したカレンダーの日付の選択
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: ef9abbd60fc83622631b916c50f4993c1c848b00
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510252"
---
# <a name="android-date-picker"></a>Android 日付の選択

## <a name="overview"></a>概要

ユーザーが Android アプリケーションにデータを入力する必要がある場合があります。 これを支援するために、Android フレームワークに[`DatePicker`](xref:Android.Widget.DatePicker)はウィジェット[`DatePickerDialog`](xref:Android.App.DatePickerDialog)とが用意されています。 を`DatePicker`使用すると、ユーザーは、デバイスとアプリケーションの間で一貫したインターフェイスで年、月、日を選択できます。 は、ダイアログ`DatePicker`でをカプセル化するヘルパークラスです。`DatePickerDialog`

最新の Android アプリケーションでは`DatePickerDialog` 、を[`DialogFragment`](xref:Android.App.DialogFragment)に表示する必要があります。 これにより、アプリケーションでポップアップダイアログとして DatePicker を表示したり、アクティビティに埋め込んだりすることができます。 さら`DialogFragment`に、は、ダイアログのライフサイクルと表示を管理し、実装する必要があるコードの量を減らします。

このガイドでは、 `DatePickerDialog` `DialogFragment`でラップされたの使用方法について説明します。 このサンプルアプリケーションでは、 `DatePickerDialog`ユーザーがアクティビティのボタンをクリックすると、がモーダルダイアログとして表示されます。 ユーザーが日付を設定すると、が選択`TextView`された日付で更新されます。

[![[選択日] ボタンの後に [日付の選択] ダイアログのスクリーンショット](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>必要条件

このガイドのサンプルアプリケーションでは、Android 4.1 (API レベル) を対象としています。
16) 以上。ただし、Android 3.0 (API レベル11以上) に適用されます。 Android サポートライブラリ v4 をプロジェクトに追加し、一部のコードを変更することで、以前のバージョンの Android をサポートすることができます。

## <a name="using-the-datepicker"></a>DatePicker を使用する

このサンプルは、 `DialogFragment`を拡張します。 サブクラスは、を`DatePickerDialog`ホストして表示します。

![クローズアップの日付選択ダイアログ](date-picker-images/image-02.png)

ユーザーが日付を選択し、 **[OK** ] ボタン`DatePickerDialog`をクリックすると、は[`IOnDateSetListener.OnDateSet`](xref:Android.App.DatePickerDialog.IOnDateSetListener.OnDateSet*)メソッドを呼び出します。
このインターフェイスは、ホスト`DialogFragment`によって実装されます。 ユーザーが **[キャンセル**] ボタンをクリックすると、フラグメントとダイアログが自動的に破棄されます。

では、 `DialogFragment`選択した日付をホスティングアクティビティに返す方法がいくつかあります。

1. **メソッドを呼び出す、またはプロパティを設定する**&ndash;このアクティビティは、この値の設定専用のプロパティまたはメソッドを提供できます。

2. **イベントを発生させる**&ndash;は、`DialogFragment`が呼び出されたとき`OnDateSet`に発生するイベントを定義できます。

3. を使用`DialogFragment`すると、を呼び出し **`Action`** `Action<DateTime>`て、アクティビティの日付を表示できます。 &ndash; アクティビティは、を`Action<DateTime` `DialogFragment`インスタンス化するときに、を提供します。 このサンプルでは3番目の手法を使用し、アクティビティが`Action<DateTime>`に`DialogFragment`を提供することを要求します。

### <a name="extending-dialogfragment"></a>コードフラグメントの拡張

を`DatePickerDialog`表示するには、まずサブクラス`DialogFragment`を作成`IOnDateSetListener`し、次のようにインターフェイスを実装します。

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

メソッドは、新しい`DatePickerFragment`をインスタンス化するために呼び出されます。 `NewInstance` このメソッドは、 `Action<DateTime>`ユーザーがの`DatePickerDialog` **[OK** ] ボタンをクリックしたときに呼び出されるを受け取ります。

フラグメントを表示すると、Android はメソッド`OnCreateDialog`を呼び出します。 このメソッドは、新しい`DatePickerDialog`オブジェクトを作成し、現在`DatePickerFragment`の日付とコールバックオブジェクト (の現在のインスタンス) を使用して初期化します。

> [!NOTE]
> が呼び出されたとき`IOnDateSetListener.OnDateSet`の月の値は、1 ~ 12 ではなく 0 ~ 11 の範囲内であることに注意してください。 月の日にちは、選択された月に応じて 1 ~ 31 の範囲になります。

### <a name="showing-the-datepickerfragment"></a>DatePickerFragment を表示する

`DialogFragment`が実装されたので、このセクションでは、アクティビティでフラグメントを使用する方法を確認します。 このガイドに付属するサンプルアプリでは、アクティビティは`DialogFragment` `NewInstance`ファクトリメソッドを使用してをインスタンス化し、 `DialogFragment.Show`呼び出しを表示します。 の`DialogFragment`インスタンス化の一環として、アクティビティは`Action<DateTime>`を渡します。これにより、 `TextView`アクティビティによってホストされているに日付が表示されます。

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

## <a name="summary"></a>Summary

このサンプルでは、Android アクティビティ`DatePicker`の一部としてウィジェットをポップアップモーダルダイアログとして表示する方法について説明しました。 ここでは、サンプルの表示フラグメントを実装`IOnDateSetListener`し、インターフェイスについて説明しました。 また、このサンプルでは、選択した日付を表示するために、表示フラグメントがホストアクティビティと対話する方法も示しています。

## <a name="related-links"></a>関連リンク

- [DialogFragment](xref:Android.App.DialogFragment)
- [DatePicker](xref:Android.Widget.DatePicker)
- [DatePickerDialog](xref:Android.App.DatePickerDialog)
- [DatePickerDialog.IOnDateSetListener](xref:Android.App.DatePickerDialog.IOnDateSetListener)
- [日付を選択してください](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
