---
title: Xamarin. Android スピンボタン
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: ba4a83eb997b879e8a2398f9857e2fd80221f8ef
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029158"
---
# <a name="xamarinandroid-spinner"></a>Xamarin. Android スピンボタン

[`Spinner`](xref:Android.Widget.Spinner)は、項目を選択するためのドロップダウンリストを表示するウィジェットです。 このガイドでは、スピンボタンの選択肢の一覧を表示する簡単なアプリを作成する方法について説明し、その後に、選択した選択に関連付けられている他の値を表示する変更を示します。

## <a name="basic-spinner"></a>基本スピンボタン

このチュートリアルの最初の部分では、惑星の一覧を表示する単純なスピンボタンウィジェットを作成します。 地球を選択すると、選択した項目がトーストメッセージに表示されます。

[HelloSpinner アプリの![スクリーンショットの例](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png#lightbox)

**HelloSpinner**という名前の新しいプロジェクトを開始します。

**Resources/Layout/Main**を開き、次の xml を挿入します。

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

[`TextView`](xref:Android.Widget.TextView)の `android:text` 属性と[`Spinner`](xref:Android.Widget.Spinner)の `android:prompt` 属性が同じ文字列リソースを参照していることに注意してください。 このテキストは、ウィジェットのタイトルとして動作します。 [`Spinner`](xref:Android.Widget.Spinner)に適用すると、ウィジェットを選択したときに表示される選択ダイアログにタイトルのテキストが表示されます。

**Resources/Values/Strings .xml**を編集し、次のようにファイルを変更します。

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

2番目の `<string>` 要素は、上のレイアウトで[`TextView`](xref:Android.Widget.TextView)と[`Spinner`](xref:Android.Widget.Spinner)によって参照されるタイトル文字列を定義します。
`<string-array>` 要素は、 [`Spinner`](xref:Android.Widget.Spinner)ウィジェットでリストとして表示される文字列の一覧を定義します。

ここで、 **MainActivity.cs**を開き、次の `using` ステートメントを追加します。

```csharp
using System;
```

次に、 [`OnCreate()`](xref:Android.App.Activity.OnCreate*)) メソッドの次のコードを挿入します。

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

`Main.axml` レイアウトがコンテンツビューとして設定されると、 [`Spinner`](xref:Android.Widget.Spinner)ウィジェットが[`FindViewById<>(int)`](xref:Android.App.Activity.FindViewById*)でレイアウトからキャプチャされます。
[`CreateFromResource()`](xref:Android.Widget.ArrayAdapter.CreateFromResource*)
次に、メソッドは新しい[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)を作成します。これにより、文字列配列内の各項目が[`Spinner`](xref:Android.Widget.Spinner)の初期外観にバインドされます (選択したときに各項目がスピンボタンに表示されます)。 `Resource.Array.planets_array` ID は、上で定義された `string-array` を参照し、`Android.Resource.Layout.SimpleSpinnerItem` ID は、プラットフォームで定義されている標準のスピンボタンの外観のレイアウトを参照します。
[`SetDropDownViewResource`](xref:Android.Widget.ArrayAdapter.SetDropDownViewResource*)
は、ウィジェットが開かれたときに各項目の外観を定義するために呼び出されます。 最後に、 [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)は[`Adapter`](xref:Android.Widget.ArrayAdapter)プロパティを設定して、すべての項目を[`Spinner`](xref:Android.Widget.Spinner)に関連付けるように設定されています。

[`Spinner`](xref:Android.Widget.Spinner)から項目が選択されたときにアプリケーションを notifys するコールバックメソッドを提供するようになりました。 このメソッドは次のようになります。

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

項目を選択すると、その項目にアクセスできるように、送信側が[`Spinner`](xref:Android.Widget.Spinner)にキャストされます。 `ItemEventArgs`の `Position` プロパティを使用して、選択したオブジェクトのテキストを確認し、それを使用して[`Toast`](xref:Android.Widget.Toast)を表示できます。

アプリケーションを実行します。次のようになります。

[地球として選択された Mars を使用したスピンボタンの例の![スクリーンショット](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>キー/値ペアを使用したスピンボタン

多くの場合、アプリで使用される何らかの種類のデータに関連付けられているキー値を表示するには、`Spinner` を使用する必要があります。 `Spinner` はキーと値のペアを直接操作しないため、キーと値のペアを個別に格納し、`Spinner` にキー値を設定してから、スピンボタンで選択したキーの位置を使用して、関連付けられているデータ値を検索する必要があります。 

次の手順では、選択した地球の平均温度を表示するように**HelloSpinner**アプリを変更します。

次の `using` ステートメントを**MainActivity.cs**に追加します。

```csharp
using System.Collections.Generic;
```

次のインスタンス変数を `MainActivity` クラスに追加します。
この一覧には、惑星のキーと値のペアと、その平均温度が保持されます。

```csharp
private List<KeyValuePair<string, string>> planets;
```

`OnCreate` メソッドで、`adapter` が宣言される前に次のコードを追加します。

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

このコードでは、惑星とそれに関連付けられた平均気温の単純なストアを作成します。 (実際のアプリケーションでは、通常、データベースはキーとそれに関連付けられたデータを格納するために使用されます)。

上記のコードの直後に、キーを抽出してリストに配置するために、次の行を追加します。

```csharp
List<string> planetNames = new List<string>();
foreach (var item in planets)
    planetNames.Add (item.Key);
```

このリストを `ArrayAdapter` コンストラクター (`planets_array` リソースではなく) に渡します。

```csharp
var adapter = new ArrayAdapter<string>(this,
    Android.Resource.Layout.SimpleSpinnerItem, planetNames);
```

選択した位置を使用して、選択した地球に関連付けられている値 (気温) を検索するように `spinner_ItemSelected` を変更します。

```csharp
private void spinner_ItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format("The mean temperature for planet {0} is {1}",
        spinner.GetItemAtPosition(e.Position), planets[e.Position].Value);
    Toast.MakeText(this, toast, ToastLength.Long).Show();
}
```

アプリケーションを実行します。トーストは次のようになります。

[地球選択の表示温度の![例](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)

## <a name="resources"></a>リソース

- [`Resource.Layout`](xref:Android.Resource.Layout)
- [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
- [`Spinner`](xref:Android.Widget.Spinner)

*このページの一部は、Android オープンソースプロジェクトによって作成および共有*され、
[*Creative Commons 2.5 属性のライセンス*](https://creativecommons.org/licenses/by/2.5/)に記載されている条項に従って使用される作業に基づいて変更されます。
