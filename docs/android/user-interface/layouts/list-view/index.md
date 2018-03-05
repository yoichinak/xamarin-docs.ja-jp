---
title: ListView
description: "ListView が Android アプリケーションの重要な UI 要素です。使用されます everywhere メニュー オプションの短い一覧から連絡先またはインターネットのお気に入りの長い一覧です。 いずれかの組み込みスタイルで書式設定したりできる広範なカスタマイズされた行のスクロール ボックスの一覧を表示する簡単な方法を提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 70a7abb186c102fb803c0ab6fa38c7b2d8222292
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="listview"></a>ListView

_ListView が Android アプリケーションの重要な UI 要素です。使用されます everywhere メニュー オプションの短い一覧から連絡先またはインターネットのお気に入りの長い一覧です。いずれかの組み込みスタイルで書式設定したりできる広範なカスタマイズされた行のスクロール ボックスの一覧を表示する簡単な方法を提供します。_

<a name="overview" />

## <a name="overview"></a>概要

Android アプリケーションの最も基本的なビルド ブロックでは、リスト ビューおよびアダプターが含まれています。 `ListView`クラスは、短いメニューや長いスクロール リストであるかどうかに柔軟なデータの表示方法を提供します。 スクロール、高速のインデックスと mobile に適したユーザー インターフェイスをアプリケーションを構築するための 1 つまたは複数選択のような操作性機能を提供します。 `ListView` インスタンスでは、行ビューに含まれているデータをフィードするための *Adapter* が必要です。

このガイドを実装する方法について説明`ListView`と、さまざまな`Adapter`Xamarin.Android 内のクラスです。 外観をカスタマイズする方法も示します、`ListView`行は、メモリ使用量を削減する再利用の重要性について説明しています。 アクティビティのライフ サイクルへの影響についてもあります`ListView`と`Adapter`を使用します。 Xamarin.iOS を使用してクロスプラット フォーム アプリケーションで作業している場合、`ListView`コントロールは、iOS に構造が似ている`UITableView`(および、Android`Adapter`に似ていますが、 `UITableViewSource`)。

最初に、簡単なチュートリアルが導入されています、`ListView`の基本的なコード例とします。 使用するためにより高度なトピックへのリンクを提供する次に、`ListView`現実世界のアプリでします。


> [!NOTE]
> **注**:`RecyclerView`ウィジェットより高度で柔軟なバージョンの`ListView`します。 `RecyclerView`に代わるものであるように設計された`ListView`(および`GridView`) を使用することをお勧め`RecyclerView`なく`ListView`新規のアプリ開発します。 詳細については、次を参照してください。 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)です。


<a name="tutorial" />

## <a name="listview-tutorial"></a>ListView のチュートリアル

[`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/) [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)スクロール可能な項目のリストを作成します。 使用して、一覧に、リスト項目が自動的に挿入、 [ `IListAdapter`](https://developer.xamarin.com/api/type/Android.Widget.IListAdapter/)です。

このチュートリアルでは、文字列の配列から読み取られる国名のスクロール可能な一覧を作成します。 リスト項目を選択すると、トースト メッセージが表示されます、アイテムの位置一覧。


という名前の新しいプロジェクトを開始**HelloListView**です。

という XML ファイルを作成する**list_item.xml**内で保存し、 **リソース/レイアウト/**フォルダーです。 次に挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

このファイルに配置される各項目のレイアウトを定義する、 [ `ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)です。

開く、`HelloListView.cs`拡張クラスおよび[ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/) (の代わりに[ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/))。

```csharp
public class HelloListView : ListActivity
{
```

次のコードを挿入、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))メソッド。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);

    ListView.TextFilterEnabled = true;

    ListView.ItemClick += delegate (object sender, ItemEventArgs args) {
        // When clicked, show a toast with the TextView text
        Toast.MakeText (Application, ((TextView)args.View).Text, ToastLength.Short).Show ();
    };
}
```

これを読み込みませんレイアウト ファイル アクティビティに注意してください (これが通常の[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)))。
代わりに、設定、 [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)プロパティが自動的に追加、 [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/)の画面全体を[ `ListActivity`](https://developer.xamarin.com/api/type/Android.App.ListActivity/)です。
このメソッドは、 [ `ArrayAdapter<T>` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/)、配置先リスト項目の配列を管理する、 [ `ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)です。
[ `ArrayAdapter<T>` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/)コンス トラクターは、アプリケーション[ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/)、(前の手順で作成される) の各リスト項目のレイアウトの説明と`T[]`または[`Java.Util.IList<T>` ](https://developer.xamarin.com/api/type/Java.Util.IList/)に挿入するオブジェクトの配列、 [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) (次に定義されます)。

[ `TextFilterEnabled` ](https://developer.xamarin.com/api/property/Android.Widget.AbsListView.TextFilterEnabled/)プロパティをオンにテキストのフィルタ リング、 [ `ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)ユーザーの入力開始時に、リストはフィルター処理されるようにします。

[ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/)クリック ハンドラーをサブスクライブするイベントを使用できます。 場合内の項目、 [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/)は、ハンドラーが呼び出されるとクリックすると、および[ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)クリックした項目のテキストを使用して、メッセージが表示されます。

独自のレイアウト ファイルを定義する代わりに、プラットフォームによって提供されるリスト項目の設計を使用することができます、 [ `ListAdapter`](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)です。
たとえば、を使用してみてください`Android.Resource.Layout.SimpleListItem1`の代わりに`Resource.Layout.list_item`です。

後に、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))メソッド、文字列の配列を追加します。

```csharp
static readonly string[] countries = new String[] {
    "Afghanistan","Albania","Algeria","American Samoa","Andorra",
    "Angola","Anguilla","Antarctica","Antigua and Barbuda","Argentina",
    "Armenia","Aruba","Australia","Austria","Azerbaijan",
    "Bahrain","Bangladesh","Barbados","Belarus","Belgium",
    "Belize","Benin","Bermuda","Bhutan","Bolivia",
    "Bosnia and Herzegovina","Botswana","Bouvet Island","Brazil","British Indian Ocean Territory",
    "British Virgin Islands","Brunei","Bulgaria","Burkina Faso","Burundi",
    "Cote d'Ivoire","Cambodia","Cameroon","Canada","Cape Verde",
    "Cayman Islands","Central African Republic","Chad","Chile","China",
    "Christmas Island","Cocos (Keeling) Islands","Colombia","Comoros","Congo",
    "Cook Islands","Costa Rica","Croatia","Cuba","Cyprus","Czech Republic",
    "Democratic Republic of the Congo","Denmark","Djibouti","Dominica","Dominican Republic",
    "East Timor","Ecuador","Egypt","El Salvador","Equatorial Guinea","Eritrea",
    "Estonia","Ethiopia","Faeroe Islands","Falkland Islands","Fiji","Finland",
    "Former Yugoslav Republic of Macedonia","France","French Guiana","French Polynesia",
    "French Southern Territories","Gabon","Georgia","Germany","Ghana","Gibraltar",
    "Greece","Greenland","Grenada","Guadeloupe","Guam","Guatemala","Guinea","Guinea-Bissau",
    "Guyana","Haiti","Heard Island and McDonald Islands","Honduras","Hong Kong","Hungary",
    "Iceland","India","Indonesia","Iran","Iraq","Ireland","Israel","Italy","Jamaica",
    "Japan","Jordan","Kazakhstan","Kenya","Kiribati","Kuwait","Kyrgyzstan","Laos",
    "Latvia","Lebanon","Lesotho","Liberia","Libya","Liechtenstein","Lithuania","Luxembourg",
    "Macau","Madagascar","Malawi","Malaysia","Maldives","Mali","Malta","Marshall Islands",
    "Martinique","Mauritania","Mauritius","Mayotte","Mexico","Micronesia","Moldova",
    "Monaco","Mongolia","Montserrat","Morocco","Mozambique","Myanmar","Namibia",
    "Nauru","Nepal","Netherlands","Netherlands Antilles","New Caledonia","New Zealand",
    "Nicaragua","Niger","Nigeria","Niue","Norfolk Island","North Korea","Northern Marianas",
    "Norway","Oman","Pakistan","Palau","Panama","Papua New Guinea","Paraguay","Peru",
    "Philippines","Pitcairn Islands","Poland","Portugal","Puerto Rico","Qatar",
    "Reunion","Romania","Russia","Rwanda","Sqo Tome and Principe","Saint Helena",
    "Saint Kitts and Nevis","Saint Lucia","Saint Pierre and Miquelon",
    "Saint Vincent and the Grenadines","Samoa","San Marino","Saudi Arabia","Senegal",
    "Seychelles","Sierra Leone","Singapore","Slovakia","Slovenia","Solomon Islands",
    "Somalia","South Africa","South Georgia and the South Sandwich Islands","South Korea",
    "Spain","Sri Lanka","Sudan","Suriname","Svalbard and Jan Mayen","Swaziland","Sweden",
    "Switzerland","Syria","Taiwan","Tajikistan","Tanzania","Thailand","The Bahamas",
    "The Gambia","Togo","Tokelau","Tonga","Trinidad and Tobago","Tunisia","Turkey",
    "Turkmenistan","Turks and Caicos Islands","Tuvalu","Virgin Islands","Uganda",
    "Ukraine","United Arab Emirates","United Kingdom",
    "United States","United States Minor Outlying Islands","Uruguay","Uzbekistan",
    "Vanuatu","Vatican City","Venezuela","Vietnam","Wallis and Futuna","Western Sahara",
    "Yemen","Yugoslavia","Zambia","Zimbabwe"
  };
```

これは、配置先の文字列の配列、 [ `ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)です。

アプリケーションを実行します。 一覧をスクロールし、入力をフィルター処理、またはメッセージを表示する項目をクリックすることができます。 次のように表示されます。

[ ![国の名前を持つ ListView の例のスクリーン ショット](images/helloviews6.png)](images/helloviews6.png)

ただし、ハード コーディングされた文字列配列を使用して、デザインのベスト プラクティスではありません。 いずれかを示すためにわかりやすくするため、このチュートリアルを使用は、 [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/)ウィジェット。 参照によって定義されている外部のリソースなどの文字列配列にすることをお勧め、`string-array`プロジェクトのリソース**Resources/Values/Strings.xml**ファイル。 例:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="countries_array">
        <item>Bahrain</item>
        <item>Bangladesh</item>
        <item>Barbados</item>
        <item>Belarus</item>
        <item>Belgium</item>
        <item>Belize</item>
        <item>Benin</item>
    </string-array>
</resources>
```

これらのリソース文字列を使用する、 [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/)、置換元[ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)を次の行。

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```

<a name="going_further" />

## <a name="going-further-with-listview"></a>ListView に進む

(下記のリンク) の残りのトピックを見て包括的な操作、`ListView`クラスとさまざまな種類のアダプターの種類と使用することができます。 構造は、次のとおりです。

-   **視覚的な外観**&ndash;の部分、`ListView`コントロールとその動作します。

-   **クラス**&ndash;表示に使用されるクラスの概要、`ListView`です。

-   **ListView でデータを表示する**&ndash;データの単純なリストを表示する方法以外を実装する方法の場合は`ListView's`操作性機能以外の場合はさまざまな組み込みの行のレイアウトを使用する方法と再行ビューを使用してアダプターがメモリを節約します。

-   **カスタムの外観**&ndash;のスタイルを変更する、`ListView`カスタム レイアウト、フォントおよび色を使用します。

-   **SQLite を使用して** &ndash; SQLite データベースからデータを表示する方法、`CursorAdapter`です。

-   **アクティビティのライフ サイクル**&ndash;を実装する場合、設計に関する考慮事項`ListView`ライフ サイクルにおけるする必要がありますしてデータを入力リソースを解放する時期を含むアクティビティです。

概要が (6 つの部分に分割) ディスカッションを開始、`ListView`クラス自体の使用方法の段階的に複雑な例を導入する前にします。

-   [ListView のパーツと機能](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
-   [データで ListView を設定します。](~/android/user-interface/layouts/list-view/populating.md)
-   [ListView の外観のカスタマイズ](~/android/user-interface/layouts/list-view/customizing-appearance.md)
-   [CursorAdapters の使用](~/android/user-interface/layouts/list-view/cursor-adapters.md)
-   [ContentProvider の使用](~/android/user-interface/layouts/list-view/content-provider.md)
-   [ListView とアクティビティのライフサイクル](~/android/user-interface/layouts/list-view/activity-lifecycle.md)

<a name="summary" />

## <a name="summary"></a>まとめ

この一連の導入トピック`ListView`の組み込み機能を使用する方法の例をいくつかを指定し、`ListActivity`です。 カスタム実装が説明されている`ListView`カラフルなレイアウトで許可されているし、SQLite データベースを使用して、アクティビティのライフ サイクルの関連性のに触れること、`ListView`実装します。


## <a name="related-links"></a>関連リンク

- [AccessoryViews (サンプル)](https://developer.xamarin.com/samples/AccessoryViews/)
- [BasicTableAndroid (sample)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (サンプル)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [BuiltInViews (サンプル)](https://developer.xamarin.com/samples/BuiltInViews/)
- [CustomRowView (サンプル)](https://developer.xamarin.com/samples/CustomRowView/)
- [FastScroll (サンプル)](https://developer.xamarin.com/samples/FastScroll/)
- [SectionIndex (サンプル)](https://developer.xamarin.com/samples/SectionIndex/)
- [SimpleCursorTableAdapter (サンプル)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (サンプル)](https://developer.xamarin.com/samples/CursorTableAdapter/)
- [アクティビティ ライフ サイクルのチュートリアル](~/android/app-fundamentals/activity-lifecycle/index.md)
- [テーブルとのセル (Xamarin.iOS) の操作](~/ios/user-interface/controls/tables/index.md)
- [ListView クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [ListActivity クラスのリファレンス](https://developer.xamarin.com/api/type/Android.App.ListActivity/)
- [BaseAdapter クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
- [ArrayAdapter クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
- [CursorAdapter クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
