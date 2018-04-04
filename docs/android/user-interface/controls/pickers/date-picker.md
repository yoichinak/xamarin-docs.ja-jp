---
title: 日付の選択
description: DatePickerDialog および DialogFragment を使用して、カレンダーの日付を選択します。
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 916a9c74fa28b99e799eef80db822e86cfda617d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="date-picker"></a>日付の選択

## <a name="overview"></a>概要

ときに、ユーザーは、Android のアプリケーションへのデータを入力する必要がありますの機会があります。 この作業に役立つ、Android のフレームワークを提供、 [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)ウィジェットと[ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)です。 `DatePicker`デバイスやアプリケーション間で一貫性のあるインターフェイスの年、月、および日を選択することができます。 `DatePickerDialog`をカプセル化するヘルパー クラスには、`DatePicker`ダイアログでします。

最新の Android アプリケーションを表示する必要があります、`DatePickerDialog`で、 [ `DialogFragment`](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)です。 これを DatePicker をポップアップ ダイアログ ボックスとして表示するアプリケーションを許可またはアクティビティに埋め込まれます。 さらに、`DialogFragment`ライフ サイクル、および実装する必要があるコードの量を減らすと、ダイアログの表示を管理します。

このガイドは、使用する方法をデモンストレーションします、`DatePickerDialog`でラップされた、`DialogFragment`です。 サンプル アプリケーションが表示されます、`DatePickerDialog`モーダル ダイアログ アクティビティ上のボタンをクリックするとします。 日付が、ユーザーが設定されている場合、`TextView`選択された日付が更新されます。

[![日付の選択 ダイアログ ボックスでその後に日付の選択のスクリーン ショット ボタン](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>要件

このガイドのサンプル アプリケーションは、Android 4.1 (API レベルをターゲットします。
16) またはそれ以降、Android 3.0 (API レベル 11 以上) に適用されますが、します。 Android の古いバージョンの追加により、プロジェクトとコードを変更する Android のサポート ライブラリ v4 のサポートを行うことができます。

## <a name="using-the-datepicker"></a>DatePicker を使用します。

このサンプルを拡張`DialogFragment`です。 サブクラスがホストされ、表示、 `DatePickerDialog`:

![クローズ アップの日付の選択 ダイアログ](date-picker-images/image-02.png)

ユーザーが日付を選択し、クリックして、 **OK**  ボタン、`DatePickerDialog`メソッドを呼び出して[ `IOnDateSetListener.OnDateSet`](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/)です。
このインターフェイスは、ホストによって実装`DialogFragment`です。 ユーザーがクリックした場合、**キャンセル** ボタン、フラグメントおよび ダイアログ ボックスはそれ自体で終了されます。

いくつかの方法、`DialogFragment`ホスティングのアクティビティを選択した日付を返すことができます。

1. **メソッドの呼び出しまたはプロパティを設定**&ndash;アクティビティは、プロパティまたは具体的には、この値を設定するためのメソッドを提供できます。

2. **イベントを発生させる** &ndash; 、`DialogFragment`となるイベントを定義できる場合に発生`OnDateSet`が呼び出されます。

3. **使用して、 `Action`**  &ndash; 、`DialogFragment`呼び出すことができます、`Action<DateTime>`アクティビティで日付を表示します。 活動が提供する、`Action<DateTime`インスタンス化するとき、`DialogFragment`です。 このサンプルは 3 番目の手法を使用して、アクティビティを指定する必要があります、`Action<DateTime>`を`DialogFragment`です。



### <a name="extending-dialogfragment"></a>DialogFragment を拡張します。

表示するために、まず、`DatePickerDialog`をサブクラス化`DialogFragment`を実装して、`IOnDateSetListener`インターフェイス。

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

`NewInstance`新しいのインスタンスを作成するメソッドが呼び出される`DatePickerFragment`です。 このメソッドは、`Action<DateTime>`をユーザーがクリックしたときに呼び出される、 **OK**ボタンをクリックして、`DatePickerDialog`です。

Android がメソッドを呼び出して、フラグメントは、表示されるが、`OnCreateDialog`です。 このメソッドが、新しい作成`DatePickerDialog`オブジェクトを現在の日付と、コールバック オブジェクトの初期化 (の現在のインスタンスは、 `DatePickerFragment`)。


> [!NOTE]
> 注意してくださいを月の値と`IOnDateSetListener.OnDateSet`が呼び出されますが 0 ~ 11 しない 1 ~ 12 の範囲内です。 月の日は、1 ~ 31 (に応じて月が選択されている) の範囲になります。



### <a name="showing-the-datepickerfragment"></a>表示、DatePickerFragment

これで、`DialogFragment`が実装されると、このセクションの内容は検査してフラグメントをアクティビティで使用する方法です。 このガイドに付属するサンプル アプリで、アクティビティがインスタンス化され、`DialogFragment`を使用して、`NewInstance`ファクトリ メソッドを呼び出すことを表示`DialogFragment.Show`です。 インスタンス化の一環として、 `DialogFragment`、アクティビティから渡さ、 `Action<DateTime>`、日付に表示される、`TextView`アクティビティによってホストされています。

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

このサンプルを表示する方法を説明した、 `DatePicker` Android アクティビティの一部としてポップアップのモーダル ダイアログ ボックスとしてウィジェット。 DialogFragment 実装のサンプルを提供し、説明、`IOnDateSetListener`インターフェイスです。 このサンプルでは、DialogFragment 可能性があります、ホストを選択した日付を表示するアクティビティと対話する方法も示されています。


## <a name="related-links"></a>関連リンク

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [日付を選択します。](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/android/controls/datepicker/select_a_date)
