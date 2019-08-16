---
title: 特殊なフラグメント クラス
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/08/2018
ms.openlocfilehash: e49f12dd656d5e07feccd34e231a00124d81048a
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524275"
---
# <a name="specialized-fragment-classes"></a>特殊なフラグメント クラス

Fragment API は、アプリケーションで検出されたより一般的な機能の一部をカプセル化する他のサブクラスを提供します。 これらのサブクラスは次のとおりです。

- **Listfragment**&ndash;このフラグメントは、配列やカーソルなどのデータソースにバインドされた項目の一覧を表示するために使用されます。

- "**コードフラグメント**"&ndash;このフラグメントは、ダイアログのラッパーとして使用されます。 フラグメントによって、アクティビティの上にダイアログが表示されます。

- **PreferenceFragment**&ndash;このフラグメントは、ユーザー設定オブジェクトをリストとして表示するために使用されます。



## <a name="the-listfragment"></a>ListFragment

は、の概念と機能`ListActivity`とよく似て`ListView` います。これは、フラグメント内のをホストするラッパーです。`ListFragment` 次の図は、 `ListFragment`タブレットと電話で実行されているを示しています。

[![タブレットと電話における ListFragment のスクリーンショット](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)


### <a name="binding-data-with-the-listadapter"></a>ListAdapter を使用してデータをバインドする

クラス`ListFragment`は既に既定のレイアウトを提供しているため、 `ListFragment`の`OnCreateView`内容を表示するためにをオーバーライドする必要はありません。 は`ListView` 、 `ListAdapter`実装を使用してデータにバインドされます。 次の例では、文字列の単純な配列を使用してこれを行う方法を示しています。

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

`ListAdapter`を設定するときは、プロパティではなく`ListFragment.ListAdapter` `ListView.ListAdapter` 、プロパティを使用することが重要です。 を`ListView.ListAdapter`使用すると、重要な初期化コードがスキップされます。



### <a name="responding-to-user-selection"></a>ユーザー選択への応答

ユーザーの選択に応答するには、アプリケーションで`OnListItemClick`メソッドをオーバーライドする必要があります。 次の例は、このような可能性を示しています。

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

上のコードでは、 `ListFragment`ユーザーが内の項目を選択すると、ホストアクティビティに新しいフラグメントが表示され、選択された項目に関する詳細が表示されます。



## <a name="dialogfragment"></a>DialogFragment

表示*フラグメント*は、アクティビティのウィンドウの上でフローティングするフラグメント内のダイアログオブジェクトを表示するために使用されるフラグメントです。 これは、(Android 3.0 以降で) マネージドダイアログ Api を置き換えることを意図しています。 次のスクリーンショットは、 `DialogFragment`の例を示しています。

[![新しい車両の追加エディットボックスを表示している表示のスクリーンショット](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

は`DialogFragment` 、フラグメントとダイアログの間の状態の一貫性を保ちます。 ダイアログオブジェクトのすべての対話とコントロールは、 `DialogFragment` API を介して行われる必要があります。ダイアログオブジェクトの直接呼び出しでは実行されません。 API `DialogFragment`は、フラグメントを表示する`Show()`ために使用されるメソッドを各インスタンスに提供します。 フラグメントを削除するには、次の2つの方法があります。

- インスタンスでを呼び出し`DialogFragment.Dismiss()`ます。 `DialogFragment` 

- 別`DialogFragment`の表示。

を作成`DialogFragment`するために、クラスは`Android.App.DialogFragment,`から継承した後、次の2つのメソッドのいずれかをオーバーライドします。

- **OnCreateView**&ndash;これにより、ビューが作成されて返されます。

- **Oncreatedialog**&ndash;これにより、カスタムダイアログが作成されます。 これは通常、 *Alertdialog*を示すために使用されます。 このメソッドをオーバーライドする場合は、をオーバーライド`OnCreateView`する必要はありません。



### <a name="a-simple-dialogfragment"></a>単純なコードフラグメント

次のスクリーンショットは、 `DialogFragment` `TextView`とが2つ`Button`ある単純なを示しています。

[![TextView と2つのボタンを使用したサンプルのサンプル](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

には、ユーザーが`DialogFragment`内の1つのボタンをクリックした回数が表示されます。[その他]ボタンをクリックすると、フラグメントが閉じられます。`TextView` の`DialogFragment`コードは次のとおりです。

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

`DialogFragment` すべて`FragmentTransaction`のフラグメントと同様に、はのコンテキストで表示されます。

の`Show()`メソッドは、`FragmentTransaction`と`DialogFragment` を入力とし`string`て受け取ります。 ダイアログがアクティビティに追加され、が`FragmentTransaction`コミットされます。

次のコードは、アクティビティがメソッドを使用して`Show()`を`DialogFragment`表示する方法の1つを示しています。

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```


### <a name="dismissing-a-fragment"></a>フラグメントを終了する

のインスタンスでを呼び出す`Dismiss()`と、フラグメントがアクティビティから削除され、そのトランザクションがコミットされます。 `DialogFragment`
フラグメントの破棄に関連する標準的なフラグメントライフサイクルメソッドが呼び出されます。


### <a name="alert-dialog"></a>警告ダイアログ

をオーバーライドする代わりに`DialogFragment` 、をオーバーライド`OnCreateDialog`することもできます。 `OnCreateView` これにより、アプリケーションは、フラグメントによって管理される[Alertdialog](xref:Android.App.AlertDialog)を作成できます。 次のコードは、を使用`AlertDialog.Builder`してを`Dialog`作成する例です。

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

基本設定を管理するために、fragment API `PreferenceFragment`はサブクラスを提供します。 は[PreferenceActivity に似](xref:Android.Preferences.PreferenceActivity)ていますが、フラグメント内のユーザーに対する基本設定の階層が表示されます。 `PreferenceFragment` &ndash; ユーザーが設定を操作すると、自動的に[Sharedpreferences](https://developer.android.com/reference/android/content/SharedPreferences.html)に保存されます。
Android 3.0 以降のアプリケーションでは、 `PreferenceFragment`を使用してアプリケーションの基本設定を処理します。 次の図は、 `PreferenceFragment`の例を示しています。

[![インライン、ダイアログ、起動の設定を使用した PreferencesFragment の例](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)


### <a name="create-a-preference-fragment-from-a-resource"></a>リソースから基本設定フラグメントを作成する

[AddPreferencesFromResource](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromResource*)メソッドを使用して、XML リソースファイルから設定フラグメントを大きくすることができます。 フラグメントのライフサイクル内でこのメソッドを呼び出す論理位置は、 `OnCreate`メソッド内にあります。

上`PreferenceFragment`の図は、XML からリソースを読み込むことによって作成されました。 リソースファイルは次のとおりです。

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

を作成するための`PreferenceFragment`もう1つの方法は、アクティビティのクエリを実行することです。 各アクティビティでは、XML リソースファイルをポイントする[メタデータ\_\_キーの基本設定](xref:Android.Preferences.PreferenceManager.MetadataKeyPreferences)属性を使用できます。 Xamarin Android では、 `MetaDataAttribute`を使用してアクティビティを装飾し、使用するリソースファイルを指定することによってこれを行います。 クラス`PreferenceFragment`には、 [AddPreferenceFromIntent](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromIntent*)メソッドが用意されています。このメソッドを使用して、アクティビティを照会し、この XML リソースを検索し、そのリソースの優先順位の階層を拡張することができます。

このプロセスの例を次のコードスニペットで示します`AddPreferencesFromIntent` `PreferenceFragment`。これは、を使用してを作成します。

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

Android はクラス`MyActivityWithPreference`を確認します。 クラスは、次のコードスニペット`MetaDataAttribute,`に示すように、によって装飾される必要があります。

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

は`MetaDataAttribute` 、が基本設定階層の膨張`PreferenceFragment`に使用する XML リソースファイルを宣言します。 `MetatDataAttribute`が指定されていない場合は、実行時に例外がスローされます。 このコードを実行すると`PreferenceFragment` 、は次のスクリーンショットのように表示されます。

[![PreferenceFragment が表示されているサンプルアプリのスクリーンショット](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
