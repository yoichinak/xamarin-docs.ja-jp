---
title: Spinner
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 90b4755cdb4b8248c2b731d070d720076d4dda40
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102998"
---
# <a name="spinner"></a>Spinner

[`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) 項目を選択するためのドロップダウン リストを表示するウィジェットです。 このガイドでは、選択された項目に関連付けられているその他の値を表示する変更後に、スピン ボタンの選択肢の一覧を表示する簡単なアプリを作成する方法について説明します。

## <a name="basic-spinner"></a>基本的なスピン ボタン

このチュートリアルの最初の部分では、惑星の一覧を表示する単純なスピン ウィジェットを作成します。 惑星を選択すると、トースト メッセージには、選択した項目が表示されます。

[![HelloSpinner アプリのスクリーン ショットの例](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png#lightbox)

という名前の新しいプロジェクトを開始**HelloSpinner**します。

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

注意、 [ `TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)の`android:text`属性と[ `Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)の`android:prompt`両方の属性は、同じ文字列リソースを参照します。 このテキストは、ウィジェットのタイトルとして動作します。 適用すると、 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)、タイトル テキスト ウィジェットを選択すると表示される選択ダイアログ ボックスで表示されます。

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

2 番目の`<string>`要素によって参照されるタイトルの文字列を定義する、 [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/)と[ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)上記のレイアウトで。
`<string-array>`要素のリストとして表示される文字列のリストを定義する、 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)ウィジェット。

今すぐ開きます**MainActivity.cs**し、以下の追加`using`ステートメント。

```csharp
using System;
```

次に、次のコードを挿入します [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))
方法:

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

後に、`Main.axml`レイアウトは、コンテンツのビューとして設定されて、 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)をレイアウトからウィジェットがキャプチャされた[ `FindViewById<>(int)`](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/)します。
、 [`CreateFromResource()`](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.CreateFromResource/p/Android.Content.Context/System.Int32/System.Int32/)
メソッドは、新しい作成[ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)、各項目を文字列配列内の最初の外観にバインドされる、 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) (選択するスピン ボタンで各項目の表示方法では). `Resource.Array.planets_array` ID 参照、`string-array`上記で定義された、 `Android.Resource.Layout.SimpleSpinnerItem` ID は、プラットフォームによって定義されている、標準のスピン ボタンの外観のレイアウトを参照します。
[`SetDropDownViewResource`](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.SetDropDownViewResource/p/System.Int32/)
ウィジェットを開いたときに、各項目の外観を定義すると呼びます。 最後に、 [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)に関連付けるすべての項目に設定されている、 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)を設定して、 [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter)プロパティ。

通知によって、アプリケーションからのアイテムが選択されているときにコールバック メソッドを提供するようになりました、 [ `Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)します。 このメソッドがどのようにを次に示します。

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

送信者にキャスト項目が選択されているときに、 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)項目にアクセスできるようにします。 使用して、`Position`プロパティを`ItemEventArgs`、選択したオブジェクトのテキストを検索し、使用して表示することができます、 [ `Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)します。

は、アプリケーションを実行します。次のように表示する必要があります。

[![地球として選択されている mars のスピン ボタンのスクリーン ショットの例](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>キー/値ペアを使用するスピン ボタン

多くの場合に使用する必要は`Spinner`ある種のアプリで使用されるデータに関連付けられているキーの値を表示します。 `Spinner`がうまくいかないキー/値のペアを直接とは別に、キー/値ペアを格納、入力する必要があります、`Spinner`キーの値を使用して、スピン ボタンの選択されたキーの位置に関連付けられているデータ値を検索します。 

次の手順で、 **HelloSpinner**選択した地球の平均温度を表示するアプリを変更します。

次の追加`using`ステートメント**MainActivity.cs**:

```csharp
using System.Collections.Generic;
```

次のインスタンス変数を追加、`MainActivity`クラス。
この一覧は、惑星と、平均温度のキー/値ペアを保持します。

```csharp
private List<KeyValuePair<string, string>> planets;
```

`OnCreate`メソッドを追加する前に、次のコード`adapter`が宣言されています。

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

このコードは、惑星と関連付けられている、平均温度の単純なストアを作成します。 (実際のアプリケーションでは、データベースが通常使用キーとその関連データを格納します。)

上記のコードの直後後には、キーを抽出し、順に一覧には、次の行を追加します。

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

変更`spinner_ItemSelected`選択されている位置を使用して、選択した地球に関連付けられている値 (温度) を検索できるようにします。

```csharp
private void spinner_ItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format("The mean temperature for planet {0} is {1}",
        spinner.GetItemAtPosition(e.Position), planets[e.Position].Value);
    Toast.MakeText(this, toast, ToastLength.Long).Show();
}
```

は、アプリケーションを実行します。このよう、トーストになります。

[![温度を表示する地球の選択範囲の例](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)
   
  

## <a name="resources"></a>リソース

-   [`Resource.Layout`](https://developer.xamarin.com/api/type/Android.Resource+Layout/) 
-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) 
-   [`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) 

*このページの部分が作成および Android のオープン ソース プロジェクトで共有し、の条項に従って使用作業に基づいた変更、*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).
