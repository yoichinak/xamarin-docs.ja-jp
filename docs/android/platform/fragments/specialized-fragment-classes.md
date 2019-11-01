---
title: 特殊なフラグメント クラス
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: b004fbf121374a2bb3bf5d85f45d8cae293573bf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027314"
---
# <a name="specialized-fragment-classes"></a>特殊なフラグメント クラス

Fragment API は、アプリケーションで検出されたより一般的な機能の一部をカプセル化する他のサブクラスを提供します。 これらのサブクラスは次のとおりです。

- **Listfragment** &ndash; このフラグメントは、配列やカーソルなどのデータソースにバインドされた項目の一覧を表示するために使用されます。

- ダイアログのラッパーとして使用さ**れる &ndash; このフラグメントを**示します。 フラグメントによって、アクティビティの上にダイアログが表示されます。

- **PreferenceFragment** &ndash; このフラグメントは、ユーザー設定オブジェクトを一覧として表示するために使用されます。

## <a name="the-listfragment"></a>ListFragment

`ListFragment` は、`ListActivity`との概念と機能によく似ています。これは、フラグメント内の `ListView` をホストするラッパーです。 次の図は、タブレットと電話で実行されている `ListFragment` を示しています。

[タブレットとスマートフォンで ListFragment のスクリーンショットを![](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)

### <a name="binding-data-with-the-listadapter"></a>ListAdapter を使用してデータをバインドする

`ListFragment` クラスは既に既定のレイアウトを提供しているため、`ListFragment`の内容を表示するために `OnCreateView` をオーバーライドする必要はありません。 `ListView` は、`ListAdapter` の実装を使用してデータにバインドされます。 次の例では、文字列の単純な配列を使用してこれを行う方法を示しています。

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    string[] values = new[] { "Android", "iPhone", "WindowsMobile",
                "Blackberry", "WebOS", "Ubuntu", "Windows7", "Max OS X",
                "Linux", "OS/2" };
    this.ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleExpandableListItem1, values);
}
```

`ListAdapter`を設定するときは、`ListView.ListAdapter` プロパティではなく、`ListFragment.ListAdapter` プロパティを使用することが重要です。 `ListView.ListAdapter` を使用すると、重要な初期化コードがスキップされます。

### <a name="responding-to-user-selection"></a>ユーザー選択への応答

ユーザーの選択に応答するには、アプリケーションで `OnListItemClick` メソッドをオーバーライドする必要があります。 次の例は、このような可能性を示しています。

```csharp
public override void OnListItemClick(ListView l, View v, int index, long id)
{
    // We can display everything in place with fragments.
    // Have the list highlight this item and show the data.
    ListView.SetItemChecked(index, true);

    // Check what fragment is shown, replace if needed.
    var details = FragmentManager.FindFragmentById<DetailsFragment>(Resource.Id.details);
    if (details == null || details.ShownIndex != index)
    {
        // Make new fragment to show this selection.
        details = DetailsFragment.NewInstance(index);

        // Execute a transaction, replacing any existing
        // fragment with this one inside the frame.
        var ft = FragmentManager.BeginTransaction();
        ft.Replace(Resource.Id.details, details);
        ft.SetTransition(FragmentTransit.FragmentFade);
        ft.Commit();
    }
}
```

上のコードでは、ユーザーが `ListFragment`内の項目を選択すると、ホストアクティビティに新しいフラグメントが表示され、選択された項目の詳細が表示されます。

## <a name="dialogfragment"></a>"コードフラグメント"

表示*フラグメント*は、アクティビティのウィンドウの上でフローティングするフラグメント内のダイアログオブジェクトを表示するために使用されるフラグメントです。 これは、(Android 3.0 以降で) マネージドダイアログ Api を置き換えることを意図しています。 次のスクリーンショットは、`DialogFragment`の例を示しています。

[![新しい車両の追加エディットボックスを表示している表示フラグメントのスクリーンショット](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

`DialogFragment` によって、フラグメントとダイアログの間の状態の一貫性が維持されます。 ダイアログオブジェクトのすべての対話とコントロールは、`DialogFragment` API を介して行われる必要があります。ダイアログオブジェクトの直接呼び出しでは実行されません。 `DialogFragment` API は、フラグメントを表示するために使用される `Show()` メソッドを各インスタンスに提供します。 フラグメントを削除するには、次の2つの方法があります。

- `DialogFragment` インスタンスで `DialogFragment.Dismiss()` を呼び出します。 

- 別の `DialogFragment`を表示します。

`DialogFragment`を作成するために、クラスは `Android.App.DialogFragment,` から継承し、次の2つのメソッドのいずれかをオーバーライドします。

- **OnCreateView** &ndash; はビューを作成して返します。

- **Oncreatedialog** &ndash; カスタムダイアログを作成します。 これは通常、 *Alertdialog*を示すために使用されます。 このメソッドをオーバーライドする場合、`OnCreateView` をオーバーライドする必要はありません。

### <a name="a-simple-dialogfragment"></a>単純なコードフラグメント

次のスクリーンショットは、`TextView` があり、`Button`が2つの単純な `DialogFragment` を示しています。

[TextView と2つのボタンを使用した![の例](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

`TextView` には、ユーザーが `DialogFragment`の1つのボタンをクリックした回数が表示されます。 [その他] ボタンをクリックすると、フラグメントが閉じられます。 `DialogFragment` のコードは次のとおりです。

```csharp
public class MyDialogFragment : DialogFragment
{
    private int _clickCount;
    public override void OnCreate(Bundle savedInstanceState)
    {
        _clickCount = 0;
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState)
        
        var view = inflater.Inflate(Resource.Layout.dialog_fragment_layout, container, false);
        var textView = view.FindViewById<TextView>(Resource.Id.dialog_text_view);
            
        view.FindViewById<Button>(Resource.Id.dialog_button).Click += delegate
            {
                textView.Text = "You clicked the button " + _clickCount++ + " times.";
            };

        // Set up a handler to dismiss this DialogFragment when this button is clicked.
        view.FindViewById<Button>(Resource.Id.dismiss_dialog_button).Click += (sender, args) => Dismiss();
        return view;
        }
    }
}
```

### <a name="displaying-a-fragment"></a>フラグメントの表示

すべてのフラグメントと同様に、`DialogFragment` は `FragmentTransaction`のコンテキストで表示されます。

`DialogFragment` の `Show()` メソッドは、`FragmentTransaction` と `string` を入力として受け取ります。 ダイアログがアクティビティに追加され、`FragmentTransaction` がコミットされます。

次のコードは、アクティビティが `Show()` メソッドを使用して `DialogFragment`を表示する方法の1つを示しています。

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```

### <a name="dismissing-a-fragment"></a>フラグメントを終了する

`DialogFragment` のインスタンスで `Dismiss()` を呼び出すと、フラグメントがアクティビティから削除され、そのトランザクションがコミットされます。
フラグメントの破棄に関連する標準的なフラグメントライフサイクルメソッドが呼び出されます。

### <a name="alert-dialog"></a>警告ダイアログ

`OnCreateView`をオーバーライドする代わりに、`DialogFragment` で `OnCreateDialog`をオーバーライドすることがあります。 これにより、アプリケーションは、フラグメントによって管理される[Alertdialog](xref:Android.App.AlertDialog)を作成できます。 次のコードは、`AlertDialog.Builder` を使用して `Dialog`を作成する例です。

```csharp
public class AlertDialogFragment : DialogFragment
{
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        EventHandler<DialogClickEventArgs> okhandler;
        var builder = new AlertDialog.Builder(Activity)
            .SetMessage("This is my dialog.")
            .SetPositiveButton("Ok", (sender, args) =>
                                         {
                                             // Do something when this button is clicked.
                                         })
            .SetTitle("Custom Dialog");
        return builder.Create();
    }
}
```

## <a name="preferencefragment"></a>PreferenceFragment

基本設定を管理するために、fragment API には `PreferenceFragment` サブクラスが用意されています。 `PreferenceFragment` は[PreferenceActivity](xref:Android.Preferences.PreferenceActivity) &ndash; に似ていますが、フラグメント内のユーザーに対する基本設定の階層が表示されます。 ユーザーが設定を操作すると、自動的に[Sharedpreferences](https://developer.android.com/reference/android/content/SharedPreferences.html)に保存されます。
Android 3.0 以降のアプリケーションでは、`PreferenceFragment` を使用して、アプリケーションの基本設定を処理します。 次の図は、`PreferenceFragment`の例を示しています。

[インライン、ダイアログ、および起動の設定を使用した PreferencesFragment の![例](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)

### <a name="create-a-preference-fragment-from-a-resource"></a>リソースから基本設定フラグメントを作成する

[AddPreferencesFromResource](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromResource*)メソッドを使用して、XML リソースファイルから設定フラグメントを大きくすることができます。 フラグメントのライフサイクル内でこのメソッドを呼び出す論理的な場所は、`OnCreate` メソッドです。

上の図に示した `PreferenceFragment` は、XML からリソースを読み込むことによって作成されました。 リソースファイルは次のとおりです。

```xml
<?xml version="1.0" encoding="utf-8"?>

<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android">

  <PreferenceCategory android:title="Inline Preferences">
    <CheckBoxPreference android:key="checkbox_preference"
                        android:title="Checkbox Preference Title"
                        android:summary="Checkbox Preference Summary" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Dialog Based Preferences">

    <EditTextPreference android:key="edittext_preference"
                        android:title="EditText Preference Title"
                        android:summary="EditText Preference Summary"
                        android:dialogTitle="Edit Text Preferrence Dialog Title" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Launch Preferences">

    <!-- This PreferenceScreen tag serves as a screen break (similar to page break
             in word processing). Like for other preference types, we assign a key
             here so it is able to save and restore its instance state. -->
    <PreferenceScreen android:key="screen_preference"
                      android:title="Title Screen Preferences"
                      android:summary="Summary Screen Preferences">

      <!-- You can place more preferences here that will be shown on the next screen. -->

      <CheckBoxPreference android:key="next_screen_checkbox_preference"
                          android:title="Next Screen Toggle Preference Title"
                          android:summary="Next Screen Toggle Preference Summary" />

    </PreferenceScreen>

    <PreferenceScreen android:title="Intent Preference Title"
                      android:summary="Intent Preference Summary">

      <intent android:action="android.intent.action.VIEW"
              android:data="http://www.android.com" />

    </PreferenceScreen>

  </PreferenceCategory>

</PreferenceScreen>
```

基本設定フラグメントのコードは次のとおりです。

```csharp
public class PrefFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        AddPreferencesFromResource(Resource.Xml.preferences);
    }
}
```

### <a name="querying-activities-to-create-a-preference-fragment"></a>アクティビティを照会して基本設定フラグメントを作成する

`PreferenceFragment` を作成するためのもう1つの方法として、アクティビティのクエリがあります。 各アクティビティでは、XML リソースファイルを指す[メタデータ\_キー\_優先順位](xref:Android.Preferences.PreferenceManager.MetadataKeyPreferences)属性を使用できます。 Xamarin Android では、`MetaDataAttribute`を使用してアクティビティを装飾し、使用するリソースファイルを指定することによってこれを行います。 `PreferenceFragment` クラスは、 [AddPreferenceFromIntent](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromIntent*)メソッドを提供します。このメソッドを使用して、アクティビティを照会し、この XML リソースを検索し、そのリソースの優先順位の階層を拡張することができます。

このプロセスの例を次のコードスニペットに示します。このコードでは、`AddPreferencesFromIntent` を使用して `PreferenceFragment`を作成します。

```csharp
public class MyPreferenceFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        var intent = new Intent(this.Activity, typeof (MyActivityWithPreferences));
        AddPreferencesFromIntent(intent);
    }
}
```

Android はクラス `MyActivityWithPreference`を確認します。 クラスは、次のコードスニペットに示すように、`MetaDataAttribute,` で装飾する必要があります。

```csharp
[Activity(Label = "My Activity with Preferences")]
[MetaData(PreferenceManager.MetadataKeyPreferences, Resource = "@xml/preference_from_intent")]
public class MyActivityWithPreferences : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        // This is deliberately blank
    }
}
```

`MetaDataAttribute` は、`PreferenceFragment` が基本設定階層の膨張に使用する XML リソースファイルを宣言します。 `MetatDataAttribute` が指定されていない場合は、実行時に例外がスローされます。 このコードを実行すると、次のスクリーンショットに示すように `PreferenceFragment` が表示されます。

[PreferenceFragment が表示されているサンプルアプリのスクリーンショット![](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
