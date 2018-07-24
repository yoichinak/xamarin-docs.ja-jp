---
title: Spinner
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 039c3f3a177d62a43679facba821675c6d716a91
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774344"
---
# <a name="spinner"></a>Spinner

[`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) 項目を選択するためのドロップダウン リストに表示するウィジェットがします。 このガイドでは、選択された項目に関連付けられているその他の値を表示するための変更後に、スピン ボタンの選択肢の一覧を表示する簡単なアプリを作成する方法について説明します。

## <a name="basic-spinner"></a>基本的なスピン ボタン

このチュートリアルの最初の部分では、惑星の一覧を表示する単純なスピン ウィジェットを作成します。 惑星を選択すると、通知メッセージには、選択した項目が表示されます。

[![HelloSpinner アプリの例のスクリーン ショット](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png#lightbox)

という名前の新しいプロジェクトを開始**HelloSpinner**です。

開いている**Resources/Layout/Main.axml**し、次の XML を挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:padding="10dip"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dip"
        android:text="@string/planet_prompt"
    />
    <Spinner
        android:id="@+id/spinner"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:prompt="@string/planet_prompt"
    />
</LinearLayout>
```

注意して、 [ `TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)の`android:text`属性および[ `Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)の`android:prompt`両方の属性が同じ文字列リソースを参照します。 このテキストは、ウィジェットのタイトルとして動作します。 適用すると、 [ `Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)ウィジェットを選択すると表示される [選択] ダイアログ ボックスのタイトルのテキストが表示されます。

編集**Resources/Values/Strings.xml**次のようにファイルを変更します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloSpinner</string>
  <string name="planet_prompt">Choose a planet</string>
  <string-array name="planets_array">
    <item>Mercury</item>
    <item>Venus</item>
    <item>Earth</item>
    <item>Mars</item>
    <item>Jupiter</item>
    <item>Saturn</item>
    <item>Uranus</item>
    <item>Neptune</item>
  </string-array>
</resources>
```

2 番目`<string>`要素によって参照されるタイトル文字列の定義、 [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/)と[ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)上記のレイアウトでします。
`<string-array>`要素のリストとして表示される文字列のリストを定義する、 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)ウィジェット。

これで開く**MainActivity.cs**し、以下の追加`using`ステートメント。

```csharp
using System;
```

次のコードを次に、挿入、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))メソッド。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    Spinner spinner = FindViewById<Spinner> (Resource.Id.spinner);

    spinner.ItemSelected += new EventHandler<AdapterView.ItemSelectedEventArgs> (spinner_ItemSelected);
    var adapter = ArrayAdapter.CreateFromResource (
            this, Resource.Array.planets_array, Android.Resource.Layout.SimpleSpinnerItem);

    adapter.SetDropDownViewResource (Android.Resource.Layout.SimpleSpinnerDropDownItem);
    spinner.Adapter = adapter;
}
```

後に、`Main.axml`レイアウトは、コンテンツ ビューとして設定されて、 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)ウィジェットが持つレイアウトからキャプチャされた[ `FindViewById<>(int)`](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/)です。
[ `CreateFromResource()` ](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.CreateFromResource/p/Android.Content.Context/System.Int32/System.Int32/)メソッドは、新しい作成[ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)、文字列配列の各項目の初期の外観にバインド、 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)(これは各項目が選択するスピン ボタンでどのように表示されるか) です。 `Resource.Array.planets_array` ID 参照、`string-array`上記で定義された、 `Android.Resource.Layout.SimpleSpinnerItem` ID は、プラットフォームによって定義された、標準のスピン ボタンの外観のレイアウトを参照します。
[`SetDropDownViewResource`](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.SetDropDownViewResource/p/System.Int32/) ウィジェットを開いたときに、各項目の外観を定義すると呼びます。 最後に、 [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)に関連付けるすべての項目で設定されている、 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)を設定して、 [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter)プロパティです。

項目が選択された場合に、アプリケーションを通知によってコールバック メソッドを提供するようになりました、 [ `Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)です。 このメソッドがどのようにを次に示します。

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

送信者にキャストされる項目を選択すると、 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)項目にアクセスできるようにします。 使用して、`Position`プロパティを`ItemEventArgs`、選択されたオブジェクトのテキストを検索して表示するため使用、 [ `Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)です。

アプリケーションを実行します。次のようになります。

[![地球として選択された mars スピン ボタンのスクリーン ショットの例](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>キー/値ペアを使用するスピン ボタン

多くの場合に使用する必要は`Spinner`をいくつかの種類のアプリで使用されるデータに関連付けられているキーの値を表示します。 `Spinner`は機能しませんキー/値ペアを直接には、キー/値ペアを個別に保存、設定する必要があります、 `Spinner` 、キーの値を使用して、スピン ボタンの選択したキーの位置に関連付けられているデータ値を検索します。 

次の手順で、 **HelloSpinner**アプリが選択された地球の平均気温を表示するように変更します。

次の追加`using`ステートメントを**MainActivity.cs**:

```csharp
using System.Collections.Generic;
```

次のインスタンス変数を追加、`MainActivity`クラスです。
このリストには、惑星と、平均の気温のキー/値ペアを保持します。

```csharp
private List<KeyValuePair<string, string>> planets;
```

`OnCreate`メソッド、前に、次のコードを追加`adapter`が宣言されています。

```csharp
planets = new List<KeyValuePair<string, string>>
{
    new KeyValuePair<string, string>("Mercury", "167 degrees C"),
    new KeyValuePair<string, string>("Venus", "464 degrees C"),
    new KeyValuePair<string, string>("Earth", "15 degrees C"),
    new KeyValuePair<string, string>("Mars", "-65 degrees C"),
    new KeyValuePair<string, string>("Jupiter" , "-110 degrees C"),
    new KeyValuePair<string, string>("Saturn", "-140 degrees C"),
    new KeyValuePair<string, string>("Uranus", "-195 degrees C"),
    new KeyValuePair<string, string>("Neptune", "-200 degrees C")
};
```

このコードでは、惑星と、関連する平均の気温の単純なストアを作成します。 (実際のアプリケーションでは、データベースは通常使用をキーとその関連データを格納します。)

上記のコードの直後に、キーを抽出し、順に一覧に配置するには、以下の行を追加します。

```csharp
List<string> planetNames = new List<string>();
foreach (var item in planets)
    planetNames.Add (item.Key);
```

この一覧に渡す、`ArrayAdapter`コンス トラクター (の代わりに、`planets_array`リソース)。

```csharp
var adapter = new ArrayAdapter<string>(this,
    Android.Resource.Layout.SimpleSpinnerItem, planetNames);
```

変更`spinner_ItemSelected`選択した位置があり、選択された世界に関連付けられている値 (温度) の検索に使用できるようにします。

```csharp
private void spinner_ItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format("The mean temperature for planet {0} is {1}",
        spinner.GetItemAtPosition(e.Position), planets[e.Position].Value);
    Toast.MakeText(this, toast, ToastLength.Long).Show();
}
```

アプリケーションを実行します。トーストは、次のようになります。

[![温度を表示する惑星選択例](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)
   
  

## <a name="resources"></a>リソース

-   [`Resource.Layout`](https://developer.xamarin.com/api/type/Android.Resource+Layout/) 
-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) 
-   [`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) 

*このページの部分は変更を作成し、Android のオープン ソース プロジェクトで共有しての条項に従って使用作業に基づく、*
[*クリエイティブ コモンズ 2.5 Attribution ライセンス*](http://creativecommons.org/licenses/by/2.5/).
