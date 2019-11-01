---
title: 時刻の選択
description: TimePickerDialog と [コードフラグメント] を使用した時間の選択
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 37dd57b0f3264ed25d0a53632f312d30761f747b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029199"
---
# <a name="android-time-picker"></a>Android の時刻の選択

ユーザーが時刻を選択する方法を提供するには、 [Timepicker](xref:Android.Widget.TimePicker)を使用します。 Android アプリでは、通常、時間 &ndash; 値を選択するために[TimePickerDialog](xref:Android.App.TimePickerDialog)と共に `TimePicker` を使用します。これは、デバイスとアプリケーションの間で一貫したインターフェイスを確保するのに役立ちます。 `TimePicker` では、24時間または12時間の AM/PM モードで時刻を選択できます。
`TimePickerDialog` は、ダイアログで `TimePicker` をカプセル化するヘルパークラスです。

[![[時間の選択] ダイアログボックスのスクリーンショット](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>概要

最新の Android アプリケーションでは、`TimePickerDialog` が表示[フラグメント](xref:Android.App.DialogFragment)に表示されます。 これにより、アプリケーションでポップアップダイアログとして `TimePicker` を表示したり、アクティビティに埋め込んだりすることができます。 さらに、`DialogFragment` は、ダイアログのライフサイクルと表示を管理し、実装する必要があるコードの量を減らします。

このガイドでは、`DialogFragment`でラップされた `TimePickerDialog`の使用方法について説明します。 このサンプルアプリケーションでは、ユーザーがアクティビティのボタンをクリックしたときに、`TimePickerDialog` がモーダルダイアログとして表示されます。 ユーザーが時刻を設定すると、ダイアログが終了し、ハンドラーは、選択された時刻を使用してアクティビティ画面の `TextView` を更新します。

## <a name="requirements"></a>［要件］

このガイドのサンプルアプリケーションでは、Android 4.1 (API レベル) を対象としています。
16) 以上。ただし、は Android 3.0 (API レベル11以降) で使用できます。 Android サポートライブラリ v4 をプロジェクトに追加し、一部のコードを変更することで、以前のバージョンの Android をサポートすることができます。

## <a name="using-the-timepicker"></a>TimePicker の使用

この例では `DialogFragment`を拡張します。`DialogFragment` (下 `TimePickerFragment` と呼ばれます) のサブクラス実装は、`TimePickerDialog`をホストして表示します。 最初にサンプルアプリを起動すると、選択した時間を表示するために使用される `TextView` の上に **[選択時間]** ボタンが表示されます。

[![最初のサンプルアプリの画面](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

**[時間の選択]** ボタンをクリックすると、このスクリーンショットに示されているように、サンプルアプリによって `TimePickerDialog` が起動します。

[アプリによって表示される既定のタイムピッカーダイアログのスクリーンショット![](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

`TimePickerDialog`で、時間を選択して **[OK** ] ボタンをクリックすると、`TimePickerDialog` によって[IOnTimeSetListener](xref:Android.App.TimePickerDialog.IOnTimeSetListener.OnTimeSet*)メソッドが呼び出されます。
このインターフェイスは、ホスティング `DialogFragment` (以下で説明する`TimePickerFragment`) によって実装されます。 **[キャンセル**] ボタンをクリックすると、フラグメントとダイアログが破棄されます。

`DialogFragment` は、次の3つの方法のいずれかで、選択した時間をホスティングアクティビティに返します。

1. アクティビティ &ndash;**メソッドを呼び出すか、プロパティを設定**して、この値を設定するためのプロパティまたはメソッドを提供できます。

2. `DialogFragment` &ndash;**イベントを発生**させると、`OnTimeSet` が呼び出されたときに発生するイベントを定義できます。

3. `DialogFragment` &ndash; **`Action`を使用**すると、`Action<DateTime>` を呼び出して、アクティビティの時間を表示できます。 アクティビティは、`DialogFragment`をインスタンス化するときに、`Action<DateTime` を提供します。

このサンプルでは3番目の手法を使用します。そのためには、アクティビティが `DialogFragment`に `Action<DateTime>` ハンドラーを提供する必要があります。

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

これは、時間を表示する[TextView](xref:Android.Widget.TextView)と `TimePickerDialog`を開く[ボタン](xref:Android.Widget.Button)を備えた基本的な[LinearLayout](xref:Android.Widget.LinearLayout)です。 このレイアウトでは、ハードコーディングされた文字列とディメンションを使用してアプリをより簡単に &ndash; 理解できるようにします。実稼働アプリでは通常、これらの値にリソースを使用します ( [DatePicker](https://github.com/xamarin/recipes/blob/master/Recipes/android/controls/datepicker/select_a_date/Resources/layout/Main.axml)コード例を参照)。

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

**[時間の選択]** ボタンをクリックしても、`TimePicker`を表示するための `DialogFragment` がまだ実装されていないため、何も実行されません。
次の手順では、この `DialogFragment`を作成します。

## <a name="extending-dialogfragment"></a>コードフラグメントの拡張

`TimePicker`で使用するために `DialogFragment` を拡張するには `DialogFragment` から派生したサブクラスを作成し、`TimePickerDialog.IOnTimeSetListener`を実装する必要があります。 **MainActivity.cs**に次のクラスを追加します。

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

この `TimePickerFragment` クラスは、次のセクションで説明するように、より小さな部分に分けられています。

### <a name="dialogfragment-implementation"></a>"コード片実装"

`TimePickerFragment` には、ファクトリメソッド、ダイアログのインスタンス化メソッド、`TimePickerDialog.IOnTimeSetListener`で必要な `OnTimeSet` ハンドラーメソッドなど、いくつかのメソッドが実装されています。

- `TimePickerFragment` は `DialogFragment`のサブクラスです。 また、`TimePickerDialog.IOnTimeSetListener` インターフェイスも実装します (つまり、必要な `OnTimeSet` メソッドを提供します)。

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

- `TAG` は、ログ記録の目的で初期化されます (*MyTimePickerFragment*は、使用する任意の文字列に変更できます)。 Null 参照の例外を防ぐために、`timeSelectedHandler` アクションは空のデリゲートに初期化されます。

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

- `NewInstance` ファクトリメソッドが呼び出され、新しい `TimePickerFragment`がインスタンス化されます。 このメソッドは、ユーザーが `TimePickerDialog`の **[OK** ] ボタンをクリックしたときに呼び出される `Action<DateTime>` ハンドラーを受け取ります。

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

- フラグメントを表示すると、Android は `DialogFragment` メソッドの[Oncreatedialog](xref:Android.App.DialogFragment.OnCreateDialog*)を呼び出します。
    このメソッドは、新しい `TimePickerDialog` オブジェクトを作成し、アクティビティ、コールバックオブジェクト (`TimePickerFragment`の現在のインスタンス)、および現在の時刻を使用して初期化します。

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

- ユーザーが [`TimePicker`] ダイアログボックスで時刻の設定を変更すると、`OnTimeSet` メソッドが呼び出されます。 `OnTimeSet` は、ユーザーによって選択された時間 (時間と分) の現在の日付とマージを使用して、`DateTime` オブジェクトを作成します。

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```

- この `DateTime` オブジェクトは、作成時に `TimePickerFragment` オブジェクトに登録されている `timeSelectedHandler` に渡されます。 `OnTimeSet` は、このハンドラーを呼び出して、アクティビティの時間表示を選択された時間に更新します (このハンドラーは次のセクションで実装されています)。

    ```csharp
    timeSelectedHandler (selectedTime);
    ```

## <a name="displaying-the-timepickerfragment"></a>TimePickerFragment の表示

`DialogFragment` が実装されたので、次は、`NewInstance` ファクトリメソッドを使用して `DialogFragment` をインスタンス化し、表示を呼び出して表示し[ます。 Show](xref:Android.App.DialogFragment.Show*):

次のメソッドを `MainActivity` に追加します。

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

`TimePickerFragment`がインスタンス化された後、`TimeSelectOnClick` は、渡された時刻の値を使用してアクティビティの時間表示を更新する匿名メソッドのデリゲートを作成して渡します。 最後に、`TimePicker` ダイアログフラグメント (`DialogFragment.Show`経由) を起動して、ユーザーに `TimePicker` を表示します。

`OnCreate` メソッドの最後に、次の行を追加して、ダイアログを起動する **[選択]** ボタンにイベントハンドラーをアタッチします。

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

**[選択]** ボタンをクリックすると `TimeSelectOnClick` が呼び出され、ユーザーに `TimePicker` ダイアログフラグメントが表示されます。

## <a name="try-it"></a>手順を次に示します。

アプリケーションをビルドし、実行します。 **[選択]** ボタンをクリックすると、アクティビティの既定の時刻形式で `TimePickerDialog` が表示されます (この例では、12時間の AM/PM モード)。

[![時間のダイアログが AM/PM モードで表示される](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
[`TimePicker`] ダイアログで **[OK]** をクリックすると、ハンドラーは、選択した時間でアクティビティの `TextView` を更新してから終了します。

[![A/M 時間がアクティビティ TextView に表示される](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

次に、`is24HourFormat` を宣言して初期化した直後に `OnCreateDialog` に次のコード行を追加します。

```csharp
is24HourFormat = true;
```

この変更により、`TimePickerDialog` コンストラクターに渡されるフラグが `true` され、ホストアクティビティの時刻形式の代わりに24時間モードが使用されるようになります。 アプリを再度ビルドして実行した場合は、 **[選択]** ボタンをクリックすると、`TimePicker` ダイアログが24時間形式で表示されるようになります。

[![TimePicker ダイアログを24時間形式で表示する](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

ハンドラーは[DateTime. To短縮 Timestring](xref:System.DateTime.ToShortDateString*)を呼び出して、アクティビティの `TextView`に時間を出力するため、既定の12時間の AM/PM 形式で時刻が出力されます。

## <a name="summary"></a>まとめ

この記事では、`TimePicker` ウィジェットを Android アクティビティからポップアップモーダルダイアログとして表示する方法について説明しました。 サンプル `DialogFragment` 実装が提供され、`IOnTimeSetListener` インターフェイスについて説明しました。 また、このサンプルでは、`DialogFragment` がホストアクティビティと対話して選択された時間を表示する方法についても説明しました。

## <a name="related-links"></a>関連リンク

- ["コードフラグメント"](xref:Android.App.DialogFragment)
- [TimePicker](xref:Android.Widget.TimePicker)
- [TimePickerDialog](xref:Android.App.TimePickerDialog)
- [TimePickerDialog.IOnTimeSetListener](xref:Android.App.TimePickerDialog.IOnTimeSetListener)
- [TimePickerDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-timepickerdemo)
