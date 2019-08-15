---
title: 時刻の選択
description: TimePickerDialog と [コードフラグメント] を使用した時間の選択
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: fea96ab645b2d01b774f691402a5796eec1f1dba
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644967"
---
# <a name="android-time-picker"></a>Android の時刻の選択

ユーザーが時刻を選択する方法を提供するには、 [Timepicker](xref:Android.Widget.TimePicker)を使用します。 Android アプリでは`TimePicker` 、通常、時間の値&ndash;を選択するために[TimePickerDialog](xref:Android.App.TimePickerDialog)を使用します。これは、デバイスとアプリケーション間で一貫したインターフェイスを確保するのに役立ちます。 `TimePicker`24時間または12時間の AM/PM モードで、ユーザーが時刻を選択できるようにします。
`TimePickerDialog`は、 `TimePicker`ダイアログでをカプセル化するヘルパークラスです。

[![アクションの [時間の選択] ダイアログのスクリーンショットの例](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>概要

最新の Android アプリケーションで`TimePickerDialog`は、が表示[フラグメント](xref:Android.App.DialogFragment)に表示されます。 これにより、アプリケーションでをポップアップダイアログと`TimePicker`して表示したり、アクティビティに埋め込んだりすることができます。 さらに、は`DialogFragment` 、ダイアログのライフサイクルと表示を管理し、実装する必要があるコードの量を減らします。

このガイド`TimePickerDialog` `DialogFragment`では、でラップされたを使用する方法について説明します。 サンプルアプリケーションでは、 `TimePickerDialog`ユーザーがアクティビティのボタンをクリックすると、がモーダルダイアログとして表示されます。 ユーザーが時刻を設定した場合、ダイアログは終了し、ハンドラーは選択`TextView`された時刻を使用してアクティビティ画面でを更新します。

## <a name="requirements"></a>必要条件

このガイドのサンプルアプリケーションでは、Android 4.1 (API レベル) を対象としています。
16) 以上。ただし、は Android 3.0 (API レベル11以降) で使用できます。 Android サポートライブラリ v4 をプロジェクトに追加し、一部のコードを変更することで、以前のバージョンの Android をサポートすることができます。

## <a name="using-the-timepicker"></a>TimePicker の使用

この例は`DialogFragment`、を拡張したものです`TimePickerFragment` 。のサブクラス実装 ( `TimePickerDialog`以下を`DialogFragment`参照) をホストし、を表示します。 サンプルアプリが最初に起動されると、選択した時間を表示`TextView`するために使用されるの上に **[選択日時]** ボタンが表示されます。

[![最初のサンプルアプリの画面](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

**[時間の選択]** ボタンをクリックすると、このスクリーン`TimePickerDialog`ショットに示すように、サンプルアプリによってが起動されます。

[![アプリによって表示される既定の時間選択ダイアログのスクリーンショット](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

`TimePickerDialog` で時間を選択し、 **[OK]** ボタンをクリックすると、`TimePickerDialog` によって [IOnTimeSetListener](xref:Android.App.TimePickerDialog.IOnTimeSetListener.OnTimeSet*) メソッドが呼び出されます。
このインターフェイスは、ホスト`DialogFragment` (`TimePickerFragment`以下で説明します) によって実装されます。 **[キャンセル**] ボタンをクリックすると、フラグメントとダイアログが破棄されます。

`DialogFragment`次の3つの方法のいずれかで、選択した時間をホスティングアクティビティに返します。

1. **メソッドの呼び出しまたはプロパティの設定**&ndash;このアクティビティは、この値の設定専用のプロパティまたはメソッドを提供できます。

2. **イベントの発生**&ndash;は、`DialogFragment`が呼び出されたとき`OnTimeSet`に発生するイベントを定義できます。

3. を使用すると、を呼び出し**て、アクティビティの時間を表示できます。 `Action`**  `Action<DateTime>` `DialogFragment` &ndash; アクティビティは、を`Action<DateTime` `DialogFragment`インスタンス化するときに、を提供します。

このサンプルでは3番目の手法を使用します。この`Action<DateTime>`手法では`DialogFragment`、アクティビティがにハンドラーを提供する必要があります。

## <a name="start-an-app-project"></a>アプリプロジェクトを開始する

**TimePickerDemo**という名前の新しい Android プロジェクトを開始します (Xamarin android プロジェクトの作成に慣れていない場合は、「 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) 」を参照して、新しいプロジェクトを作成する方法を学習してください)。

**Resources/layout/Main**を編集し、その内容を次の xml に置き換えます。

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

これは、時間を表示する[TextView](xref:Android.Widget.TextView)と`TimePickerDialog`、を開く[ボタン](xref:Android.Widget.Button)を持つ基本的な[LinearLayout](xref:Android.Widget.LinearLayout)です。 このレイアウトでは、ハードコーディングされた文字列とディメンションを使用して、アプリを&ndash;より簡単に理解できるようにします。通常、このような値にはリソースが使用されます ( [DatePicker](https://github.com/xamarin/recipes/blob/master/Recipes/android/controls/datepicker/select_a_date/Resources/layout/Main.axml)コード例を参照してください)。

**MainActivity.cs**を編集し、その内容を次のコードに置き換えます。

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

この例をビルドして実行すると、次のスクリーンショットのような初期画面が表示されます。

[![最初のアプリの画面](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

を表示するためにがまだ`TimePicker`実装さ`DialogFragment`れていないため、 **[時間の選択]** ボタンをクリックしても何も行われません。
次の手順では、これ`DialogFragment`を作成します。

## <a name="extending-dialogfragment"></a>コードフラグメントの拡張

を`DialogFragment` `DialogFragment` 使用する`TimePickerDialog.IOnTimeSetListener`ためにを拡張するには、から派生したサブクラスを作成し、を実装する必要があります。`TimePicker` **MainActivity.cs**に次のクラスを追加します。

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

この`TimePickerFragment`クラスは、より小さな部分に分割され、次のセクションで説明します。

### <a name="dialogfragment-implementation"></a>"コード片実装"

`TimePickerFragment`には、ファクトリメソッド、ダイアログのインスタンス化メソッド、およびで必要`OnTimeSet`とされるハンドラー `TimePickerDialog.IOnTimeSetListener`メソッドという、いくつかのメソッドが実装されています。

-   `TimePickerFragment`はの`DialogFragment`サブクラスです。 また、インターフェイスも`TimePickerDialog.IOnTimeSetListener`実装します (つまり、必要な`OnTimeSet`メソッドを提供します)。

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG`はログ記録のために初期化されます (*MyTimePickerFragment*は、使用する任意の文字列に変更できます)。 Null `timeSelectedHandler`参照の例外を防ぐために、アクションは空のデリゲートに初期化されます。

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   ファクトリメソッドは、新しい`TimePickerFragment`をインスタンス化するために呼び出されます。 `NewInstance` このメソッドは、 `Action<DateTime>`ユーザーがの`TimePickerDialog` **[OK** ] ボタンをクリックしたときに呼び出されるハンドラーを受け取ります。

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   フラグメントを表示するときに、Android は`DialogFragment`メソッド[oncreatedialog](xref:Android.App.DialogFragment.OnCreateDialog*)を呼び出します。
    このメソッドは、新しい`TimePickerDialog`オブジェクトを作成し、アクティビティ、コールバックオブジェクト (の現在のインスタンス`TimePickerFragment`)、および現在の時刻を使用して初期化します。

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

-   ユーザーが`TimePicker`ダイアログ`OnTimeSet`の時刻設定を変更すると、メソッドが呼び出されます。 `OnTimeSet`現在の`DateTime`日付と、ユーザーによって選択された時間 (時と分) のマージを使用して、オブジェクトを作成します。

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   この`DateTime`オブジェクトは、 `timeSelectedHandler`作成時に`TimePickerFragment`オブジェクトに登録されたに渡されます。 `OnTimeSet`このハンドラーを呼び出して、アクティビティの時間表示を選択された時間に更新します (このハンドラーは次のセクションで実装されています)。

    ```csharp
    timeSelectedHandler (selectedTime);
    ```

## <a name="displaying-the-timepickerfragment"></a>TimePickerFragment の表示

が実装されたので、 `NewInstance`ファクトリメソッド`DialogFragment`を使用してをインスタンス化し、表示フラグメントを呼び出してそれを表示し[ます。 Show](xref:Android.App.DialogFragment.Show*): `DialogFragment`

に次のメソッドを`MainActivity`追加します。

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

は`TimeSelectOnClick` 、を`TimePickerFragment`インスタンス化した後、渡された時刻値を使用してアクティビティの時間表示を更新する匿名メソッドのデリゲートを作成して渡します。 最後に、を使用`TimePicker` `DialogFragment.Show`してダイアログフラグメントを起動し、 `TimePicker`をユーザーに表示します。

`OnCreate`メソッドの最後に、次の行を追加して、ダイアログを起動する **[選択]** ボタンにイベントハンドラーをアタッチします。

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

**[選択時刻]** ボタンをクリックする`TimeSelectOnClick`と、が`TimePicker`呼び出され、ユーザーにダイアログフラグメントが表示されます。

## <a name="try-it"></a>手順を次に示します。

アプリケーションをビルドし、実行します。 **[選択]** ボタンをクリックすると、 `TimePickerDialog`がアクティビティの既定の時刻形式で表示されます (この例では、12時間の AM/PM モード)。

[![時間ダイアログが AM/PM モードで表示される](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
`TimePicker`ダイアログで **[OK]** をクリックすると、ハンドラーは、選択`TextView`した時間でアクティビティのを更新してから終了します。

[![A/M 時間がアクティビティ TextView に表示される](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

次に、が宣言され初期化さ`OnCreateDialog`れた`is24HourFormat`直後に、次のコード行を追加します。

```csharp
is24HourFormat = true;
```

この変更により、 `TimePickerDialog`コンストラクターに渡されるフラグが強制的に設定さ`true`れるため、ホストアクティビティの時刻形式の代わりに24時間モードが使用されます。 アプリを再度ビルドして実行した場合は、[**選択**した`TimePicker`時間] ボタンをクリックすると、ダイアログが24時間形式で表示されます。

[![24時間形式の TimePicker ダイアログ](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

ハンドラーは、時間をアクティビティの`TextView`に出力するために[DateTime. to短縮 timestring](xref:System.DateTime.ToShortDateString*)を呼び出すため、既定の12時間の AM/PM 形式で時刻が出力されます。

## <a name="summary"></a>まとめ

この記事では、Android アクティビティ`TimePicker`からウィジェットをポップアップモーダルダイアログとして表示する方法について説明しました。 ここでは、 `DialogFragment`サンプルの実装を`IOnTimeSetListener`提供し、インターフェイスについて説明しました。 また、このサンプルでは`DialogFragment` 、がホストアクティビティと対話して選択された時間を表示する方法も示しています。

## <a name="related-links"></a>関連リンク

- [DialogFragment](xref:Android.App.DialogFragment)
- [TimePicker](xref:Android.Widget.TimePicker)
- [TimePickerDialog](xref:Android.App.TimePickerDialog)
- [TimePickerDialog.IOnTimeSetListener](xref:Android.App.TimePickerDialog.IOnTimeSetListener)
- [TimePickerDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-timepickerdemo)
