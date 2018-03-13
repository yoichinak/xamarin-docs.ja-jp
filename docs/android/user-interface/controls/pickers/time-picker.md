---
title: "時刻の選択"
description: "TimePickerDialog および DialogFragment を使用して時刻を選択します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 93a2effd42432d13767dad05a47548aebc9a0b93
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="time-picker"></a>時刻の選択

ユーザーが時刻を選択するための方法を提供するを使用できます[TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)です。 Android アプリを使用して通常`TimePicker`で[TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)時刻の値を選択するため&ndash;これにより、デバイスとアプリケーション間で一貫性のあるインターフェイスを確認してください。 `TimePicker` 12 時間制または 24 時間のいずれかの午前/午後モードの 1 日の時刻を選択することができます。
`TimePickerDialog` カプセル化するヘルパー クラスには、`TimePicker`ダイアログでします。

[![アクションで時間の選択 ダイアログの例のスクリーン ショット](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>概要

最新の Android アプリケーションの表示、`TimePickerDialog`で、 [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)です。 これにより、アプリケーションを表示する、`TimePicker`ポップアップ ダイアログ ボックスとしてアクティビティに埋め込む。 さらに、`DialogFragment`ライフ サイクル、および実装する必要があるコードの量を減らすと、ダイアログの表示を管理します。

このガイドを使用する方法を示しています、`TimePickerDialog`でラップされた、`DialogFragment`です。 サンプル アプリケーションが表示されます、`TimePickerDialog`モーダル ダイアログ アクティビティ上のボタンをクリックするとします。 ダイアログ ボックスを終了し、ハンドラーが更新の時間が、ユーザーが設定されている場合、 `TextView` [アクティビティ] 画面で選択された時刻を使用します。

## <a name="requirements"></a>必要条件

このガイドのサンプル アプリケーションは、Android 4.1 (API レベルをターゲットします。
16) 以降では、またはが Android 3.0 (API レベル 11 以上) で使用できます。 Android の古いバージョンの追加により、プロジェクトとコードを変更する Android のサポート ライブラリ v4 のサポートを行うことができます。

## <a name="using-the-timepicker"></a>使用して、TimePicker

この例では拡張`DialogFragment`; のサブクラス実装`DialogFragment`(と呼ばれる`TimePickerFragment`下) をホストし、表示、`TimePickerDialog`です。 表示するサンプル アプリを最初に起動するときに、**ピック タイム**上のボタン、`TextView`選択した時刻を表示する使用します。

[![初期のサンプル アプリの画面](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

クリックすると、**ピック タイム** ボタン、例のアプリが起動、`TimePickerDialog`このスクリーン ショットに示すよう。

[![アプリで表示される既定の時刻の選択ダイアログのスクリーン ショット](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

`TimePickerDialog`、時刻を選択しをクリックすると、 **[ok]**原因のボタンをクリックして、`TimePickerDialog`メソッドを呼び出す[IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/)です。
このインターフェイスは、ホストによって実装`DialogFragment`(`TimePickerFragment`、以下の説明)。 クリックすると、**キャンセル**ボタンをクリックすると、フラグメントとダイアログ ボックスを破棄できます。

`DialogFragment` 3 つの方法のいずれかでホスティング Actvity に、選択された時刻を返します。

1. **メソッドの呼び出しまたはプロパティを設定**&ndash;アクティビティは、プロパティまたは具体的には、この値を設定するためのメソッドを提供できます。

2. **イベントを発生させる** &ndash; 、`DialogFragment`となるイベントを定義できる場合に発生`OnTimeSet`が呼び出されます。

3. **使用して、 `Action`**  &ndash; 、`DialogFragment`呼び出すことができます、`Action<DateTime>`アクティビティで時刻を表示します。 活動が提供する、`Action<DateTime`インスタンス化するとき、`DialogFragment`です。 

このサンプルは手法を使用して、3 番目を必要とアクティビティ供給、`Action<DateTime>`ハンドラー、`DialogFragment`です。



## <a name="start-an-app-project"></a>アプリ プロジェクトを開始します。

いう新しい Android プロジェクトを開始**TimePickerDemo** (Xamarin.Android プロジェクトの作成に慣れていない場合は、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)を新しいプロジェクトを作成する方法を参照してください)。

編集**Resources/layout/Main.axml**しその内容を次の XML に置き換えます。

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

これは、基本的な[LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)で、 [TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/)時間を表示して、[ボタン](https://developer.xamarin.com/api/type/Android.Widget.Button/)を開く、`TimePickerDialog`です。 このレイアウトが、アプリを簡素化され、理解しやすいようにする、ハードコーディングされた文字列とディメンションを使用することに注意してください&ndash;、運用アプリケーションは通常、これらの値のリソースを使用 (で示されているように、 [DatePicker](https://github.com/xamarin/recipes/blob/master/android/controls/datepicker/select_a_date/Resources/layout/Main.axml)のコード例)。

編集**MainActivity.cs**しその内容を次のコードに置き換えます。

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

[![最初のアプリの画面](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

クリックすると、**ピック タイム**ボタンは、何も行われませんので、`DialogFragment`表示に実装されていない、`TimePicker`です。
次の手順がこれを作成するには`DialogFragment`します。



## <a name="extending-dialogfragment"></a>DialogFragment を拡張します。

拡張する`DialogFragment`で使用するため`TimePicker`から派生したサブクラスを作成する必要がある`DialogFragment`を実装して`TimePickerDialog.IOnTimeSetListener`です。 次のクラスを追加**MainActivity.cs**:

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

これは、`TimePickerFragment`クラスが細かく分割して、次のセクションで説明します。


### <a name="dialogfragment-implementation"></a>DialogFragment 実装

`TimePickerFragment` いくつかのメソッドを実装する: 工場出荷時のメソッド、ダイアログのインスタンス化方法では、および`OnTimeSet`ハンドラー メソッドで必要な`TimePickerDialog.IOnTimeSetListener`します。

-   `TimePickerFragment` サブクラスは、`DialogFragment`です。 実装して、`TimePickerDialog.IOnTimeSetListener`インターフェイス (つまり、提供、必要な`OnTimeSet`メソッド)。

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` ログ記録の目的で初期化 (*MyTimePickerFragment*を変更できる任意の文字列に使用する)。 `timeSelectedHandler`操作は null 参照の例外を防ぐために空のデリゲートを初期化します。

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   `NewInstance`新しいのインスタンスを作成するファクトリ メソッドが呼び出された`TimePickerFragment`です。 このメソッドは、 `Action<DateTime>` 、ユーザーがクリックしたときに呼び出されるハンドラー、 **OK**ボタンをクリックして、 `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   フラグメントは、表示するのには、Android 呼び出し、`DialogFragment`メソッド[OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/)です。 
    このメソッドは、新しい作成`TimePickerDialog`オブジェクトは、アクティビティ、コールバック オブジェクトで初期化 (の現在のインスタンスは、 `TimePickerFragment`)、および現在の時刻。

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

-   ユーザーが時間の設定を変更するときに、`TimePicker`ダイアログ ボックスで、`OnTimeSet`メソッドが呼び出されます。 `OnTimeSet` 作成、`DateTime`オブジェクトの現在の日付と、ユーザーが選択した時間 (時間および分) で結合を使用します。

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   これは、`DateTime`にオブジェクトが渡される、`timeSelectedHandler`で登録されている、`TimePickerFragment`オブジェクトの作成時にします。 `OnTimeSet` (このハンドラーは実装されています、次のセクションで) 選択した時間に、アクティビティの時刻の表示を更新するには、このハンドラーを呼び出します。

    ```csharp
    timeSelectedHandler (selectedTime);
    ```



## <a name="displaying-the-timepickerfragment"></a>TimePickerFragment を表示します。

これで、`DialogFragment`が実装した場合はインスタンスを作成する、`DialogFragment`を使用して、`NewInstance`ファクトリ メソッドを呼び出すことによって表示および[DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

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

後に`TimeSelectOnClick`をインスタンス化、 `TimePickerFragment`、作成し、渡された時刻の値で、アクティビティの時刻の表示を更新する匿名メソッドのデリゲートで渡します。 最後が起動され、`TimePicker`ダイアログ フラグメント (を介して`DialogFragment.Show`) を表示する、`TimePicker`をユーザーにします。

最後に、`OnCreate`メソッド、イベント ハンドラーをアタッチする次の行を追加、**ピック タイム**ダイアログを起動するボタン。

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

ときに、**ピック タイム**ボタンをクリックすると、`TimeSelectOnClick`が呼び出され表示が、`TimePicker`をユーザーにダイアログ フラグメント。



## <a name="try-it"></a>手順を次に示します。

アプリケーションをビルドし、実行します。 クリックすると、**ピック タイム** ボタン、 `TimePickerDialog` (この場合、12 時間の午前/午後モード) では、アクティビティの既定の時刻形式で表示されます。

[![午前/午後モードで時間のダイアログ ボックスが表示されます。](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
クリックすると**OK**で、`TimePicker`ダイアログ ボックスで、ハンドラーは、更新、アクティビティの`TextView`と、選択した時間とし、終了。

[![アクティビティ TextView の A/M 時刻が表示されます。](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

次に、次のコードの行を追加`OnCreateDialog`後すぐに`is24HourFormat`宣言および初期化します。

```csharp
is24HourFormat = true;
```

この変更を強制的に渡すフラグ、`TimePickerDialog`コンス トラクターを`true`のため、ホスティングのアクティビティの時刻の形式ではなく、24 時間制を使用します。 ビルド アプリをもう一度実行すると、クリックして、**ピック タイム** ボタン、 `TimePicker` 24 時間形式でダイアログが表示されるようになりました。

[![24 時間形式で TimePicker ダイアログ](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

ハンドラーを呼び出すため[DateTime.ToShortTimeString](https://msdn.microsoft.com/en-us/library/system.datetime.toshortdatestring%28v=vs.110%29.aspx) 、アクティビティの時間を印刷する`TextView`時間は既定の 12 時間 AM/PM 形式で出力もします。



## <a name="summary"></a>まとめ

この記事の内容を表示する方法を説明した、 `TimePicker` Android アクティビティからポップアップのモーダル ダイアログ ボックスとしてウィジェット。 ファイルを提供して、サンプル`DialogFragment`実装について説明し、`IOnTimeSetListener`インターフェイスです。 このサンプルも示されている方法、`DialogFragment`ホストを選択した時間を表示するアクティビティとやり取りできます。


## <a name="related-links"></a>関連リンク

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (sample)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
