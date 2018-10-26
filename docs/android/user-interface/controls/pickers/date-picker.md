---
title: 日付の選択
description: DatePickerDialog と DialogFragment を使用して予定表の日付を選択します。
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: 9f82317f6041de3952d11b391afffafe6fbd8761
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122973"
---
# <a name="date-picker"></a>日付の選択

## <a name="overview"></a>概要

ときに、ユーザーは、Android のアプリケーションへのデータを入力する必要がありますの状況があります。 これを支援する Android のフレームワークを提供します、 [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)ウィジェットと[ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)します。 `DatePicker`デバイスやアプリケーション間で一貫性のあるインターフェイスの年、月、日を選択することができます。 `DatePickerDialog`をカプセル化するヘルパー クラスには、`DatePicker`ダイアログでします。

最新の Android アプリケーションを表示する必要があります、`DatePickerDialog`で、 [ `DialogFragment`](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)します。 これにより、ポップアップ ダイアログ ボックスとして、DatePicker を表示するアプリケーションまたはアクティビティに埋め込まれています。 さらに、`DialogFragment`ライフ サイクルおよび実装する必要があるコードの量を減らす ダイアログ ボックスの表示を管理します。

このガイドは、使用する方法を示しますが、`DatePickerDialog`でラップされた、`DialogFragment`します。 サンプル アプリケーションが表示されます、`DatePickerDialog`として、ユーザーがアクティビティ上のボタンをクリックすると、モーダル ダイアログ。 日付が、ユーザーが設定されている場合、`TextView`選択された日付が更新されます。

[![日付の選択 ダイアログ ボックスでその後に日付の選択のスクリーン ショットのボタン](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>必要条件

このガイドのサンプル アプリケーションの対象が Android 4.1 (API レベル
16) またはそれ以降、Android 3.0 (API レベル 11 以上) に適用されますが、します。 古いバージョンの Android プロジェクトとコードを変更する Android サポート ライブラリ v4 の追加をサポートできるようになります。

## <a name="using-the-datepicker"></a>DatePicker の使用

このサンプルは拡張`DialogFragment`します。 サブクラスがホストされ、表示、 `DatePickerDialog`:

![クローズ アップの日付の選択 ダイアログ](date-picker-images/image-02.png)

ユーザーが日付を選択し、クリックして、 **OK**  ボタン、`DatePickerDialog`メソッドを呼び出す[ `IOnDateSetListener.OnDateSet` ](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/)。
このインターフェイスは、ホストによって実装`DialogFragment`します。 ユーザーがクリックした場合、**キャンセル**ボタンをクリック、フラグメントおよび ダイアログ ボックスは自体で無視されます。

いくつかの方法がある、`DialogFragment`ホスティングのアクティビティを選択した日付を返すことができます。

1. **メソッドの呼び出しやプロパティを設定する**&ndash;アクティビティは、プロパティまたは具体的には、この値を設定するためのメソッドを提供できます。

2. **イベントを発生させる** &ndash; 、`DialogFragment`となるイベントを定義できる場合に発生します`OnDateSet`が呼び出されます。

3. **使用して、 `Action`**  &ndash; 、`DialogFragment`呼び出すことができます、`Action<DateTime>`アクティビティの日付を表示します。 アクティビティを提供、`Action<DateTime`インスタンス化するとき、`DialogFragment`します。 このサンプルは、3 番目の手法を使用し、アクティビティを指定する必要があります、`Action<DateTime>`を`DialogFragment`します。



### <a name="extending-dialogfragment"></a>DialogFragment を拡張します。

表示するために、まず、`DatePickerDialog`をサブクラス化`DialogFragment`を実装すること、`IOnDateSetListener`インターフェイス。

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

`NewInstance`新しいインスタンスを作成するメソッドが呼び出される`DatePickerFragment`します。 このメソッドは、`Action<DateTime>`をユーザーがクリックしたときに呼び出される、 **OK**ボタン、 `DatePickerDialog`。

Android でメソッドを呼び出して、フラグメントは、表示されるが、`OnCreateDialog`します。 このメソッドが、新しい作成`DatePickerDialog`オブジェクトし、現在の日付とコールバック オブジェクトで初期化 (の現在のインスタンスである、 `DatePickerFragment`)。


> [!NOTE]
> 注意してくださいを月の値と`IOnDateSetListener.OnDateSet`が呼び出されますが 0 ~ 11 としない 1 ~ 12 の範囲内です。 月の日は、1 ~ 31 (に応じて月が選択されている) の範囲になります。



### <a name="showing-the-datepickerfragment"></a>表示、DatePickerFragment

これで、`DialogFragment`されました実装されると、このセクションではアクティビティで、フラグメントを使用する方法が確認されます。 このガイドに付属しているサンプル アプリで、アクティビティがインスタンスを作成、`DialogFragment`を使用して、`NewInstance`ファクトリ メソッドを呼び出すことを表示`DialogFragment.Show`します。 インスタンス化の一部として、 `DialogFragment`、アクティビティに渡します、`Action<DateTime>`で日付が表示されます、`TextView`アクティビティによってホストされています。

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

このサンプルを表示する方法を説明した、 `DatePicker` Android アクティビティの一部としてモーダル ポップアップ ダイアログとしてウィジェット。 DialogFragment 実装のサンプルを提供し、説明、`IOnDateSetListener`インターフェイス。 このサンプルでは、ホストを選択した日付を表示するアクティビティに、DialogFragment が対話する方法も示されています。


## <a name="related-links"></a>関連リンク

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [日付を選択します。](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
