---
title: 特殊化されたフラグメント クラス
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: ab7045b16d441a3f2ad06c64d08b9b98135b18a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="specialized-fragment-classes"></a>特殊化されたフラグメント クラス

フラグメントの API は、一部のアプリケーションでより一般的な機能をカプセル化されるその他のサブクラスを提供します。 これらのサブクラスは次のとおりです。

-   **ListFragment** &ndash;このフラグメントが配列やカーソルなどのデータ ソースにバインドされている項目の一覧を表示するために使用します。

-   **DialogFragment** &ndash;このフラグメントは、ダイアログをラップするラッパーとして使用します。 フラグメントは、そのアクティビティ上にダイアログが表示されます。

-   **PreferenceFragment** &ndash;リストとしてユーザー設定オブジェクトを表示するこのフラグメントを使用します。



## <a name="the-listfragment"></a>ListFragment

`ListFragment`は概念と機能を非常に似ていますが、 `ListActivity`; をホストするラッパーである、`ListView`フラグメント。 次のイメージ、`ListFragment`電話とタブレットで実行されています。

[![スクリーン ショットの ListFragment、携帯電話とタブレットで](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)


### <a name="binding-data-with-the-listadapter"></a>ListAdapter とデータのバインド

`ListFragment`をオーバーライドする必要はありませんのでにクラスが既定のレイアウトに既に提供されて`OnCreateView`の内容を表示する、`ListFragment`です。 `ListView`を使用してデータにバインドされて、`ListAdapter`実装します。 どのこうことが、単純な文字列の配列を使用して、次の例を示しています。

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

設定するときに、`ListAdapter`を使用することが重要、`ListFragment.ListAdapter`プロパティ、および not、`ListView.ListAdapter`プロパティです。 使用して`ListView.ListAdapter`がスキップされる重要な初期化コードが発生します。



### <a name="responding-to-user-selection"></a>ユーザーが選択した場合の対処

ユーザーの選択を対処するアプリケーションをオーバーライドする必要があります、`OnListItemClick`メソッドです。 次の例は、このような 1 つの方法を示しています。

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

ユーザーが内の項目を選択すると前のコードで、 `ListFragment`、選択された項目に関する詳細情報を示すホスティングの活動に、新しいフラグメントが表示されます。



## <a name="dialogfragment"></a>DialogFragment

*DialogFragment*フラグメント、アクティビティのウィンドウの上にフロートされるフラグメントの内部でダイアログ ボックスのオブジェクトを表示するために使用します。 管理されているダイアログ (3.0 以降では Android) Api を置き換えるものではします。 次のスクリーン ショットの例を示しています、 `DialogFragment`:

[![DialogFragment のスクリーン ショットは新しい自動車エディット ボックスの追加を表示します。](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

A`DialogFragment`によりフラグメントとダイアログ ボックスの状態が一貫します。 すべての相互作用と、ダイアログ オブジェクトのコントロールは、によって行われる必要があります、 `DialogFragment` API、およびダイアログ オブジェクトの直接の呼び出しを確立できません。 `DialogFragment` API では、各インスタンスを提供する、`Show()`フラグメントを表示するために使用するメソッド。 これにはフラグメントを処分する 2 つの方法があります。

-  呼び出す`DialogFragment.Dismiss()`上、`DialogFragment`インスタンス。 

-  別の表示`DialogFragment`です。

作成する、 `DialogFragment`、クラスを継承`Android.App.DialogFragment,`と次の 2 つの方法の 1 つを上書きします。

- **OnCreateView** &ndash;これが作成され、ビューを返します。

- **OnCreateDialog** &ndash;これがカスタム ダイアログを作成します。 通常の表示に使用する*AlertDialog*です。 このメソッドをオーバーライドする場合は、オーバーライドする必要はありません`OnCreateView`です。



### <a name="a-simple-dialogfragment"></a>単純な DialogFragment

次のスクリーン ショットに示しています、単純な`DialogFragment`を持つ、`TextView`と 2 つ`Button`s:

[![例 DialogFragment、TextView と 2 つのボタン](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

`TextView`ユーザーでの 1 つのボタンがクリックされた回数が表示されます、`DialogFragment`フラグメントは、その他のボタンをクリックすると閉じられます。 コードを`DialogFragment`は。

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

すべてのフラグメントと同様に、`DialogFragment`のコンテキストで表示される、`FragmentTransaction`です。

`Show()`メソッドを`DialogFragment`受け取り、`FragmentTransaction`と`string`の入力として。 ダイアログ ボックスは、アクティビティに追加されます、`FragmentTransaction`コミットします。

次の例では、アクティビティを使用方法の 1 つ、`Show()`を表示するメソッド、 `DialogFragment`:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```


### <a name="dismissing-a-fragment"></a>フラグメントを閉じる

呼び出す`Dismiss()`のインスタンスで、`DialogFragment`アクティビティから削除するフラグメントの原因し、そのトランザクションをコミットします。
フラグメントの破棄に関連する標準のフラグメントのライフ サイクル メソッドが呼び出されます。


### <a name="alert-dialog"></a>警告のダイアログ

オーバーライドする代わりに`OnCreateView`、`DialogFragment`代わりによりも優先`OnCreateDialog`です。 これにより、アプリケーションを作成する、 [AlertDialog](https://developer.xamarin.com/api/type/Android.App.AlertDialog/)フラグメントで管理されています。 次のコードが使用する例を`AlertDialog.Builder`を作成する、 `Dialog`:

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

環境設定の管理に役立つ、フラグメント API が用意されています、`PreferenceFragment`サブクラスです。 `PreferenceFragment`に似ていますが、 [PreferenceActivity](https://developer.xamarin.com/api/type/Android.Preferences.PreferenceActivity/) &ndash;フラグメント内のユーザー設定の階層が表示されます。 ユーザーは、環境設定を使用して、として自動的に保存されますを[SharedPreferences](http://developer.android.com/reference/android/content/SharedPreferences.html)です。
Android 3.0 またはの高いアプリケーションを使用して、`PreferenceFragment`アプリケーションの設定に対処します。 次の図の例を示しています、 `PreferenceFragment`:

[![例 PreferencesFragment インライン、ダイアログ ボックスと起動設定を使用](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)


### <a name="create-a-preference-fragment-from-a-resource"></a>リソースから優先フラグメントを作成します。

優先する設定を使用して、XML リソース ファイルからフラグメントが大きく可能性があります、 [PreferenceFragment.AddPreferencesFromResource](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromResource/p/System.Int32/)メソッドです。 このメソッドを呼び出す、フラグメントのライフ サイクルの論理的な場所になります、`OnCreate`メソッドです。

`PreferenceFragment`絵上記の XML からリソースを読み込むことによって作成されました。 リソース ファイルは次のとおりです。

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

基本設定のフラグメントのコードは次のとおりです。

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

作成するためのもう 1 つの方法、`PreferenceFragment`アクティビティをクエリで取得します。 各アクティビティに使用できる、[メタデータ\_キー\_優先](https://developer.xamarin.com/api/field/Android.Preferences.PreferenceManager.MetadataKeyPreferences/)XML リソース ファイルを指す属性。 Xamarin.Android では、これを持つアクティビティを装飾、 `MetaDataAttribute`、し、使用するリソース ファイルを指定します。 `PreferenceFragment`クラス、メソッドを提供[AddPreferenceFromIntent](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromIntent/p/Android.Content.Intent/)) を使用してこの XML リソースを見つけて、その基本設定の階層を展開するためのアクティビティのクエリをことができます。

このプロセスの例を使用して、次のコード スニペットで提供される`AddPreferencesFromIntent`を作成する、 `PreferenceFragment`:

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

Android のクラスを見て`MyActivityWithPreference`です。 クラスを装飾する必要があります、`MetaDataAttribute,`次のコード スニペットで示すようにします。

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

`MetaDataAttribute` XML リソース ファイルの宣言、 `PreferenceFragment`  基本設定の階層を拡大するために使用されます。 場合、`MetatDataAttribute`は指定しないと、実行時に例外がスローされます。 このコードを実行すると、`PreferenceFragment`次のスクリーン ショットのように表示されます。

[![表示される PreferenceFragment 例のアプリのスクリーン ショット](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
