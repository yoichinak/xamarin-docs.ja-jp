---
title: 時刻の選択
description: TimePickerDialog と DialogFragment を使用して時間を選択します。
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: faf2c35b49b0b02b9f3b16e19494d2e447361d84
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61283666"
---
# <a name="time-picker"></a>時刻の選択

ユーザーが時刻を選択するための方法を提供する使用[TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)します。 Android アプリを使用して、通常`TimePicker`で[TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)時刻値を選択するため&ndash;デバイスやアプリケーション全体で一貫性のあるインターフェイスを確認することになります。 `TimePicker` 12 時間または 24 時間のいずれかの午前/午後モードの 1 日の時刻を選択することができます。
`TimePickerDialog` カプセル化するヘルパー クラスには、`TimePicker`ダイアログにします。

[![動作時間ピッカー ダイアログのスクリーン ショットの例](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>概要

最新の Android アプリケーションの表示、`TimePickerDialog`で、 [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)します。 アプリケーションの表示にできるようになります、`TimePicker`ポップアップ ダイアログ ボックスとしてアクティビティに埋め込む。 さらに、`DialogFragment`ライフ サイクルおよび実装する必要があるコードの量を減らす ダイアログ ボックスの表示を管理します。

このガイドは、使用する方法を示します、`TimePickerDialog`でラップされた、`DialogFragment`します。 サンプル アプリケーションが表示されます、`TimePickerDialog`として、ユーザーがアクティビティ上のボタンをクリックすると、モーダル ダイアログ。 ダイアログ ボックスを終了し、ハンドラーが更新の時間が、ユーザーが設定されている場合、`TextView`アクティビティ画面で選択された時間。

## <a name="requirements"></a>必要条件

このガイドのサンプル アプリケーションの対象が Android 4.1 (API レベル
16) 以降では Android 3.0 (API レベル 11 以上) で使用できます。 古いバージョンの Android プロジェクトとコードを変更する Android サポート ライブラリ v4 の追加をサポートできるようになります。

## <a name="using-the-timepicker"></a>TimePicker の使用

この例では拡張`DialogFragment`; のサブクラス実装`DialogFragment`(と呼ばれる`TimePickerFragment`の下) をホストし、表示、`TimePickerDialog`します。 サンプル アプリを最初に起動すると表示されます、**選択時間**上のボタン、`TextView`選択した時刻を表示する使用します。

[![最初のサンプル アプリの画面](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

クリックすると、**選択時間**ボタンの例のアプリの起動、`TimePickerDialog`このスクリーン ショットで。

[![アプリで表示される、既定時刻の選択 ダイアログ ボックスのスクリーン ショット](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

`TimePickerDialog`、時刻を選択およびクリックすると、 **[ok]** 原因のボタン、`TimePickerDialog`メソッドを呼び出す[IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/)します。
このインターフェイスは、ホストによって実装`DialogFragment`(`TimePickerFragment`以下を参照)。 クリックすると、**キャンセル**ボタンをクリックすると、フラグメントとダイアログ ボックスを破棄します。

`DialogFragment` 3 つの方法の 1 つのホスティングのアクティビティには、選択した時間を返します。

1. **メソッドの呼び出しまたはプロパティを設定する**&ndash;アクティビティは、プロパティまたは具体的には、この値を設定するためのメソッドを提供できます。

2. **イベントを発生させる** &ndash; 、`DialogFragment`となるイベントを定義できる場合に発生します`OnTimeSet`が呼び出されます。

3. **使用して、 `Action`**  &ndash; 、`DialogFragment`呼び出すことができます、`Action<DateTime>`アクティビティで時刻を表示します。 アクティビティを提供、`Action<DateTime`インスタンス化するとき、`DialogFragment`します。 

このサンプルは、アクティビティの提供に 3 番目の手法は、する必要がありますには使用、`Action<DateTime>`ハンドラーを`DialogFragment`します。



## <a name="start-an-app-project"></a>アプリ プロジェクトを開始します。

という名前の新しい Android プロジェクトを開始**TimePickerDemo** (Xamarin.Android プロジェクトの作成に慣れていない場合は、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)に新しいプロジェクトを作成する方法について説明します)。

編集**Resources/layout/Main.axml**内容を次の XML に置き換えます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_gravity="center_horizontal"
    android:padding="16dp">
    <Button
        android:id="@+id/select_button"
        android:paddingLeft="24dp"
        android:paddingRight="24dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="PICK TIME"
        android:textSize="20dp" />
    <TextView
        android:id="@+id/time_display"
        android:layout_height="wrap_content"
        android:layout_width="match_parent"
        android:paddingTop="22dp"
        android:text="Picked time will be displayed here"
        android:textSize="24dp" />
</LinearLayout>
```

これは、基本的な[LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)で、 [TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/)時間を表示して、[ボタン](https://developer.xamarin.com/api/type/Android.Widget.Button/)を開く、`TimePickerDialog`します。 このレイアウトがハードコーディングされた文字列とディメンションを使用してアプリをより簡単かつ理解しやすいようにすることに注意してください&ndash;運用アプリは、これらの値のリソースを使用する通常 (でわかるように、 [DatePicker](https://github.com/xamarin/recipes/blob/master/Recipes/android/controls/datepicker/select_a_date/Resources/layout/Main.axml)のコード例)。

編集**MainActivity.cs**し、その内容を次のコードに置き換えます。

```csharp
using Android.App;
using Android.Widget;
using Android.OS;
using System;
using Android.Util;
using Android.Text.Format;

namespace TimePickerDemo
{
    [Activity(Label = "TimePickerDemo", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        TextView timeDisplay;
        Button timeSelectButton;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            SetContentView(Resource.Layout.Main);
            timeDisplay = FindViewById<TextView>(Resource.Id.time_display);
            timeSelectButton = FindViewById<Button>(Resource.Id.select_button);
        }
    }
}
```

ビルドし、この例を実行すると、次のスクリーン ショットのような最初の画面が表示されます。

[![アプリの最初の画面](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

クリックすると、**選択時間**ボタンは、何も行われませんので、`DialogFragment`表示に実装されていません、 `TimePicker`。
次の手順がこれを作成するには`DialogFragment`します。



## <a name="extending-dialogfragment"></a>DialogFragment を拡張します。

拡張する`DialogFragment`で使用するため`TimePicker`から派生するサブクラスを作成する必要がある`DialogFragment`実装と`TimePickerDialog.IOnTimeSetListener`します。 次のクラスを追加**MainActivity.cs**:

```csharp
public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
{
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };

    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }

    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }

    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
        Log.Debug(TAG, selectedTime.ToLongTimeString());
        timeSelectedHandler (selectedTime);
    }
}
```

これは、`TimePickerFragment`クラスが小さな部分に分割され、次のセクションで説明します。


### <a name="dialogfragment-implementation"></a>DialogFragment 実装

`TimePickerFragment` いくつかのメソッドの実装: ファクトリ メソッド、ダイアログのインスタンス化方法では、および`OnTimeSet`ハンドラーのメソッドで必要な`TimePickerDialog.IOnTimeSetListener`します。

-   `TimePickerFragment` サブクラスは`DialogFragment`します。 実装も、`TimePickerDialog.IOnTimeSetListener`インターフェイス (提供に必要な`OnTimeSet`メソッド)。

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` ログ記録のために初期化されます (*MyTimePickerFragment*は任意の文字列を使用する)。 `timeSelectedHandler`操作が null 参照例外を回避する空のデリゲートを初期化します。

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   `NewInstance`ファクトリ メソッドを呼び出して、新しいインスタンスを作成する`TimePickerFragment`します。 このメソッドは、 `Action<DateTime>` 、ユーザーがクリックしたときに呼び出されるハンドラー、 **OK**ボタン、 `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   フラグメントが表示される、Android が呼び出し、`DialogFragment`メソッド[OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/)します。 
    このメソッドは、新しい作成`TimePickerDialog`オブジェクトし、アクティビティ、コールバック オブジェクトで初期化 (の現在のインスタンスである、 `TimePickerFragment`)、および現在の時刻。

    ```csharp
    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }
    ```

-   ユーザーが時刻の設定を変更するときに、`TimePicker`ダイアログ ボックスで、`OnTimeSet`メソッドが呼び出されます。 `OnTimeSet` 作成、`DateTime`オブジェクトの現在の日付と、ユーザーが選択した時間 (時間および分) でのマージを使用します。

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   これは、`DateTime`にオブジェクトが渡される、`timeSelectedHandler`で登録されている、`TimePickerFragment`オブジェクトの作成時にします。 `OnTimeSet` 選択した時間 (このハンドラーは、次のセクションで実装) に、アクティビティの時刻の表示を更新するには、このハンドラーを呼び出します。

    ```csharp
    timeSelectedHandler (selectedTime);
    ```



## <a name="displaying-the-timepickerfragment"></a>TimePickerFragment を表示します。

これで、`DialogFragment`されましたをインスタンス化する時間が実装されると、`DialogFragment`を使用して、`NewInstance`ファクトリ メソッドを呼び出すことによって表示[DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

次のメソッドを追加`MainActivity`:

```csharp
void TimeSelectOnClick (object sender, EventArgs eventArgs)
{
    TimePickerFragment frag = TimePickerFragment.NewInstance (
        delegate (DateTime time)
        {
            timeDisplay.Text = time.ToShortTimeString();
        });

    frag.Show(FragmentManager, TimePickerFragment.TAG);
}
```

後`TimeSelectOnClick`をインスタンス化、 `TimePickerFragment`、作成し、渡された時刻の値で、アクティビティの時刻の表示を更新する匿名メソッドのデリゲートに渡します。 最後に、起動、`TimePicker`ダイアログ フラグメント (を使用して`DialogFragment.Show`) を表示する、`TimePicker`をユーザーにします。

最後に、`OnCreate`メソッド、イベント ハンドラーを結び付けるには、次の行を追加、**選択時間**ダイアログを起動するボタン。

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

ときに、**選択時間**ボタンがクリックされた`TimeSelectOnClick`表示に呼び出される、`TimePicker`をユーザーにダイアログのフラグメント。



## <a name="try-it"></a>手順を次に示します。

アプリケーションをビルドし、実行します。 クリックすると、**選択時間**ボタン、 `TimePickerDialog` (この場合、12 時間の午前/午後モード) では、アクティビティの既定の時刻形式で表示されます。

[![午前/午後モードで時間のダイアログが表示されます。](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
クリックすると**OK**で、`TimePicker`ダイアログ ボックスで、ハンドラーの更新プログラムのアクティビティの`TextView`で選択した時間とし、終了。

[![アクティビティの TextView に A/分の時間が表示されます。](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

次に、次のコードの行を追加`OnCreateDialog`直後後`is24HourFormat`宣言および初期化します。

```csharp
is24HourFormat = true;
```

この変更によりに渡されるフラグ、`TimePickerDialog`コンス トラクターを`true`のため、ホスティングのアクティビティの時刻の形式ではなく、24 時間制を使用します。 ビルドし、再度アプリを実行すると、クリックして、**選択時間** ボタン、`TimePicker`ダイアログが 24 時間形式で表示されるようになりました。

[![24 時間形式で TimePicker ダイアログ](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

ハンドラーが呼び出すため[DateTime.ToShortTimeString](xref:System.DateTime.ToShortDateString*)アクティビティの時間を印刷する`TextView`時間はまだ既定の 12 時間 AM/PM 形式で印刷します。



## <a name="summary"></a>まとめ

この記事の説明を表示する方法、 `TimePicker` Android アクティビティからモーダル ポップアップ ダイアログとしてウィジェット。 サンプルが提供されている`DialogFragment`実装について説明し、`IOnTimeSetListener`インターフェイス。 このサンプルにも示されていますが、どのように`DialogFragment`ホストを選択した時間を表示するアクティビティと対話できます。


## <a name="related-links"></a>関連リンク

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (sample)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
