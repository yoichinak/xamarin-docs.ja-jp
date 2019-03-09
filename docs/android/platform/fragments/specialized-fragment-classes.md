---
title: 特殊なフラグメント クラス
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/08/2018
ms.openlocfilehash: 75d95d630415cdaa4c0c1ed3b8ddebb32b8e3c4d
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670066"
---
# <a name="specialized-fragment-classes"></a>特殊なフラグメント クラス

フラグメント API では、アプリケーションでより一般的な機能の一部をカプセル化する他のサブクラスを提供します。 これらのサブクラスは次のとおりです。

-   **ListFragment** &ndash;このフラグメントが配列やカーソルなどのデータ ソースにバインドされている項目の一覧を表示するために使用します。

-   **DialogFragment** &ndash;このフラグメントは、ダイアログのラッパーとして使用されます。 フラグメントは、そのアクティビティ上にダイアログが表示されます。

-   **PreferenceFragment** &ndash;このフラグメントを使用して、リストとして設定オブジェクトを表示します。



## <a name="the-listfragment"></a>ListFragment

`ListFragment`概念と機能を非常に似ていますが、 `ListActivity`; をホストするラッパーは、`ListView`フラグメント内。 次のイメージを`ListFragment`電話とタブレットで実行されています。

[![スクリーン ショットの ListFragment タブレットや電話](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)


### <a name="binding-data-with-the-listadapter"></a>データ、ListAdapter とバインド

`ListFragment`クラスは、既定のレイアウトを既に提供をオーバーライドする必要はありませんので`OnCreateView`の内容を表示する、`ListFragment`します。 `ListView`を使用してデータにバインドされた、`ListAdapter`実装します。 単純な文字列の配列を使用して、その方法これ行うことが次の例を示しています。

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

設定するときに、`ListAdapter`を使用することが重要、`ListFragment.ListAdapter`プロパティ、および not、`ListView.ListAdapter`プロパティ。 使用して`ListView.ListAdapter`スキップする重要な初期化コードが発生します。



### <a name="responding-to-user-selection"></a>ユーザーの選択への応答

ユーザー選択への応答をするアプリケーションをオーバーライドする必要があります、`OnListItemClick`メソッド。 次の例は、このような 1 つの可能性を示しています。

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

ユーザーが内の項目を選択すると前のコードで、 `ListFragment`、ホスティングのアクティビティの詳細については、選択した項目の表示で新しいフラグメントが表示されます。



## <a name="dialogfragment"></a>DialogFragment

*DialogFragment*フラグメントは、アクティビティのウィンドウの上にフローティングするフラグメントの内部でダイアログ ボックスのオブジェクトを表示するために使用します。 これは管理されているダイアログ (Android 3.0 以降) の Api を置き換えるためのものです。 次のスクリーン ショットの例を示しています、 `DialogFragment`:

[![新しい車両エディット ボックスの追加を表示する DialogFragment のスクリーン ショット](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

A`DialogFragment`によりフラグメントとダイアログ ボックスの状態が維持されます。 使用してすべての相互作用とダイアログ オブジェクトを制御する必要があります行われる、 `DialogFragment` API、およびダイアログ オブジェクトの直接の呼び出しを確立できません。 `DialogFragment` API では、各インスタンスを提供する、`Show()`フラグメントを表示するために使用するメソッド。 フラグメントを削除する 2 つの方法はあります。

-  呼び出す`DialogFragment.Dismiss()`上、`DialogFragment`インスタンス。 

-  別の表示`DialogFragment`します。

作成する、`DialogFragment`からクラスを継承`Android.App.DialogFragment,`と次の 2 つのメソッドのいずれかを上書きします。

- **OnCreateView** &ndash;これにより作成され、ビューを返します。

- **OnCreateDialog** &ndash;これは、カスタム ダイアログを作成します。 通常表示を使用する*AlertDialog*します。 このメソッドをオーバーライドする場合は、オーバーライドする必要はありません`OnCreateView`します。



### <a name="a-simple-dialogfragment"></a>単純な DialogFragment

次のスクリーン ショット、単純な`DialogFragment`を持つ、`TextView`と 2 つ`Button`: %s

[![例 DialogFragment、TextView と 2 つのボタン](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

`TextView`ユーザーが 1 つのボタンをクリックした回数の合計が表示されます、 `DialogFragment`、その他のボタンをクリックすると、フラグメントが閉じられます。 コードを`DialogFragment`は。

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


### <a name="displaying-a-fragment"></a>フラグメントを表示します。

などのすべてのフラグメントを`DialogFragment`のコンテキストで表示される、`FragmentTransaction`します。

`Show()`メソッドを`DialogFragment`は、`FragmentTransaction`と`string`の入力として。 ダイアログ ボックスは、アクティビティに追加されます、`FragmentTransaction`コミットします。

次の例では、アクティビティを使用方法の 1 つ、`Show()`を表示するメソッド、 `DialogFragment`:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```


### <a name="dismissing-a-fragment"></a>フラグメントの消去

呼び出す`Dismiss()`のインスタンスで、`DialogFragment`アクティビティから削除するフラグメントを原因し、そのトランザクションをコミットします。
フラグメントの破棄に関連する標準的なフラグメントのライフ サイクル メソッドが呼び出されます。


### <a name="alert-dialog"></a>アラート ダイアログ

オーバーライドする代わりに`OnCreateView`、`DialogFragment`は代わりにオーバーライド`OnCreateDialog`します。 これにより、アプリケーションを作成する、 [AlertDialog](https://developer.xamarin.com/api/type/Android.App.AlertDialog/)フラグメントで管理されています。 次のコード例を使用するは、`AlertDialog.Builder`を作成する、 `Dialog`:

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

基本設定を管理するには、フラグメント API を提供します、`PreferenceFragment`サブクラスです。 `PreferenceFragment`に似ていますが、 [PreferenceActivity](https://developer.xamarin.com/api/type/Android.Preferences.PreferenceActivity/) &ndash;フラグメント、ユーザーの基本設定の階層に表示されます。 ユーザーと対話の設定、として自動的に保存されますを[SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html)します。
Android 3.0 またはの高いアプリケーションでは、使用、`PreferenceFragment`アプリケーションの設定を処理します。 次の図の例を示します、 `PreferenceFragment`:

[![インライン、ダイアログ ボックスで、起動設定して例 PreferencesFragment](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)


### <a name="create-a-preference-fragment-from-a-resource"></a>リソースから基本設定のフラグメントを作成します。

フラグメントを使用して、XML リソース ファイルから拡張可能性があります 基本設定、 [PreferenceFragment.AddPreferencesFromResource](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromResource/p/System.Int32/)メソッド。 論理的な場所とフラグメントのライフ サイクルでこのメソッドを呼び出すようになります。、`OnCreate`メソッド。

`PreferenceFragment`図に示す XML からリソースを読み込むことによって作成された以降。 リソース ファイルは次のとおりです。

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

フラグメントの基本設定のコードは次のとおりです。

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



### <a name="querying-activities-to-create-a-preference-fragment"></a>基本設定のフラグメントを作成するアクティビティのクエリを実行します。

作成するための別の手法を`PreferenceFragment`アクティビティのクエリを実行する必要があります。 各アクティビティを使用できます、[メタデータ\_キー\_優先](https://developer.xamarin.com/api/field/Android.Preferences.PreferenceManager.MetadataKeyPreferences/)XML リソース ファイルを指す属性。 Xamarin.Android では、これでのアクティビティを装飾、 `MetaDataAttribute`、し、使用するリソース ファイルを指定します。 `PreferenceFragment`クラス、メソッドを提供[AddPreferenceFromIntent](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromIntent/p/Android.Content.Intent/)) を使用してこの XML リソースを見つけて、その基本設定の階層を展開するためのアクティビティのクエリをことができます。

このプロセスの例には、次のコード スニペットが記載されて`AddPreferencesFromIntent`を作成する、 `PreferenceFragment`:

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

Android は、クラスを見て`MyActivityWithPreference`します。 クラスを修飾する必要があります、`MetaDataAttribute,`次のコード スニペットに示すようにします。

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

`MetaDataAttribute` XML リソース ファイルを宣言、`PreferenceFragment`を使用して、基本設定の階層を展開します。 場合、`MetatDataAttribute`は指定しないと、実行時に例外がスローされます。 このコードを実行すると、`PreferenceFragment`次のスクリーン ショットのように表示されます。

[![表示される PreferenceFragment 例のアプリのスクリーン ショット](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
