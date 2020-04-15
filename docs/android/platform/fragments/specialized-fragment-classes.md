---
title: 特殊なフラグメント クラス
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: b004fbf121374a2bb3bf5d85f45d8cae293573bf
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027314"
---
# <a name="specialized-fragment-classes"></a>特殊なフラグメント クラス

Fragments API では、アプリケーションに含まれるより一般的な機能の一部をカプセル化する他のサブクラスが提供されています。 次のようなサブクラスです。

- **ListFragment** &ndash; このフラグメントは、配列やカーソルなどのデータソースにバインドされた項目の一覧を表示するために使用されます。

- **DialogFragment** &ndash; このフラグメントは、ダイアログのラッパーとして使用されます。 フラグメントでは、アクティビティの上にダイアログが表示されます。

- **PreferenceFragment** &ndash; このフラグメントは、Preference オブジェクトを一覧として表示するために使用されます。

## <a name="the-listfragment"></a>ListFragment

`ListFragment` の概念と機能は、`ListActivity` とよく似ています。これは、フラグメント内で `ListView` をホストするラッパーです。 次の図は、タブレットとスマートフォンで実行されている `ListFragment` を示したものです。

[![タブレットとスマートフォンでの ListFragment のスクリーンショット](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)

### <a name="binding-data-with-the-listadapter"></a>ListAdapter を使用してデータをバインドする

`ListFragment` クラスでは既に既定のレイアウトが提供されているため、`ListFragment` の内容を表示するために `OnCreateView` をオーバーライドする必要はありません。 `ListView` をデータにバインドするには、`ListAdapter` の実装を使用します。 次の例では、簡単な文字列の配列を使用してこれを行う方法を示します。

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

`ListAdapter` を設定するときは、`ListView.ListAdapter` プロパティではなく、`ListFragment.ListAdapter` プロパティを使用することが重要です。 `ListView.ListAdapter` を使用すると、重要な初期化コードがスキップされます。

### <a name="responding-to-user-selection"></a>ユーザーによる選択に応答する

アプリケーションでユーザーの選択に応答するには、`OnListItemClick` メソッドをオーバーライドする必要があります。 考えられる 1 つの例を次に示します。

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

上のコードでは、ユーザーが `ListFragment` で項目を選択すると、ホストしているアクティビティに新しいフラグメントが表示され、選択された項目の詳細が表示されます。

## <a name="dialogfragment"></a>DialogFragment

*DialogFragment* は、アクティビティのウィンドウ上を浮動するダイアログ オブジェクトをフラグメント内の表示するために使用されるフラグメントです。 これは、(Android 3.0 以降で) マネージド ダイアログ API を置き換えることが意図されています。 次のスクリーンショットでは、`DialogFragment` の例を示します。

[![Add New Vehicle エディット ボックスを表示している DialogFragment のスクリーンショット](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

`DialogFragment` により、フラグメントとダイアログの間で状態の整合性が維持されます。 ダイアログ オブジェクトのすべての対話と制御は、ダイアログ オブジェクトでの直接呼び出しで行われるのではなく、`DialogFragment` API を介して行われる必要があります。 `DialogFragment` API では、フラグメントを表示するために使用される `Show()` メソッドが、各インスタンスに提供されます。 フラグメントを削除するには、2 つの方法があります。

- `DialogFragment` のインスタンスで `DialogFragment.Dismiss()` を呼び出します。 

- 別の `DialogFragment` を表示します。

`DialogFragment` を作成するには、クラスで `Android.App.DialogFragment,` を継承した後、次の 2 つのメソッドのいずれかをオーバーライドします。

- **OnCreateView** &ndash; ビューを作成して返します。

- **OnCreateDialog** &ndash; カスタム ダイアログを作成します。 通常は、*AlertDialog* を表示するために使用されます。 このメソッドをオーバーライドする場合は、`OnCreateView` をオーバーライドする必要はありません。

### <a name="a-simple-dialogfragment"></a>簡単な DialogFragment

次のスクリーンショットでは、1 つの `TextView` と 2 つの `Button` が含まれる簡単な `DialogFragment` が示されています。

[![TextView と 2 つのボタンが含まれる DialogFragment の例](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

`TextView` には、ユーザーが `DialogFragment` の 1 つのボタンをクリックした回数が表示されます。他のボタンをクリックすると、フラグメントが閉じられます。 `DialogFragment` のコードは次のとおりです。

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

### <a name="displaying-a-fragment"></a>フラグメントを表示する

すべてのフラグメントと同様に、`DialogFragment` は `FragmentTransaction` のコンテキストで表示されます。

`DialogFragment` の `Show()` メソッドは、`FragmentTransaction` と `string` を入力として受け取ります。 ダイアログがアクティビティに追加されて、`FragmentTransaction` がコミットされます。

次のコードでは、アクティビティで `Show()` メソッドを使用して `DialogFragment` を表示する方法の 1 つを示します。

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
フラグメントの破棄に関連する標準のフラグメント ライフサイクル メソッドが呼び出されます。

### <a name="alert-dialog"></a>アラート ダイアログ

`OnCreateView` をオーバーライドする代わりに、`DialogFragment` では `OnCreateDialog` をオーバーライドできます。 これにより、アプリケーションでは、フラグメントによって管理される [AlertDialog](xref:Android.App.AlertDialog) を作成できます。 次のコードは、`AlertDialog.Builder` を使用して `Dialog` を作成する例です。

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

基本設定を管理するため、Fragmentst API には `PreferenceFragment` サブクラスが用意されています。 `PreferenceFragment` は [PreferenceActivity](xref:Android.Preferences.PreferenceActivity) に似ており、フラグメントでのユーザーに対する基本設定の階層が表示されます。 ユーザーが基本設定を操作すると、[SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html) に自動的に保存されます。
Android 3.0 以降のアプリケーションで基本設定を処理するには、`PreferenceFragment` を使用します。 次の図は、`PreferenceFragment` の例を示したものです。

[![インライン、ダイアログ、起動の基本設定が含まれる PreferencesFragment の例](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)

### <a name="create-a-preference-fragment-from-a-resource"></a>リソースから基本設定フラグメントを作成する

[PreferenceFragment.AddPreferencesFromResource](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromResource*) メソッドを使用して、XML リソース ファイルから基本設定フラグメントを拡張できる場合があります。 フラグメントのライフサイクル内でこのメソッドを呼び出す論理的な場所は、`OnCreate` メソッドです。

上の図に示した `PreferenceFragment` は、XML からリソースを読み込むことによって作成されました。 リソース ファイルは次のとおりです。

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

### <a name="querying-activities-to-create-a-preference-fragment"></a>アクティビティをクエリして基本設定フラグメントを作成する

`PreferenceFragment` を作成するもう 1 つの方法には、アクティビティのクエリが含まれます。 各アクティビティでは、XML リソース ファイルを指し示す [METADATA\_KEY\_PREFERENCE](xref:Android.Preferences.PreferenceManager.MetadataKeyPreferences) 属性を使用できます。 Xamarin.Android では、`MetaDataAttribute` を使用してアクティビティを装飾し、使用するリソース ファイルを指定することによって、これを行います。 `PreferenceFragment` クラスで提供される [AddPreferenceFromIntent](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromIntent*) メソッドを使用して、アクティビティのクエリを行い、この XML リソースを見つけて、その基本設定階層を拡張できます。

このプロセスの例を次のコード スニペットに示します。このコードでは、`AddPreferencesFromIntent` を使用して `PreferenceFragment` を作成しています。

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

Android では、`MyActivityWithPreference` クラスが調べられます。 クラスは、次のコード スニペットで示すように、`MetaDataAttribute,` で装飾されている必要があります。

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

`MetaDataAttribute` では、`PreferenceFragment` によって基本設定階層の膨張に使用される XML リソース ファイルが宣言されています。 `MetatDataAttribute` が提供されていない場合は、実行時に例外がスローされます。 このコードを実行すると、`PreferenceFragment` が次のスクリーンショットのように表示されます。

[![PreferenceFragment が表示されているアプリの例のスクリーンショット](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
