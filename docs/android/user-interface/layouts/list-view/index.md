---
title: Xamarin.Android で、ListView を使用します。
description: ListView は、Android アプリケーションの重要な UI コンポーネント使用されます everywhere メニュー オプションの短いリストから連絡先またはインターネットのお気に入りの長い一覧です。 いずれかを組み込みスタイルで書式設定したりできる広範なカスタマイズが行のスクロール リストに表示する簡単な方法を提供します。
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 2560a451f3a6e7dd09b687f9db8c0c070598def6
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/06/2019
ms.locfileid: "66740659"
---
# <a name="listview"></a>ListView

_ListView は、Android アプリケーションの重要な UI コンポーネント使用されます everywhere メニュー オプションの短いリストから連絡先またはインターネットのお気に入りの長い一覧です。いずれかを組み込みスタイルで書式設定したりできる広範なカスタマイズが行のスクロール リストに表示する簡単な方法を提供します。_


## <a name="overview"></a>概要

Android アプリケーションの最も基本的な構成要素には、リスト ビュー、アダプターが含まれます。 `ListView`クラスは短いメニューまたは時間の長いスクロール リストかどうかに存在するデータを柔軟な方法を提供します。 スクロール、高速のインデックスとモバイル フレンドリ ユーザー インターフェイスをアプリケーションを構築するための 1 つまたは複数選択のような操作性機能を提供します。 `ListView` インスタンスでは、行ビューに含まれているデータをフィードするための *Adapter* が必要です。

このガイドを実装する方法について説明`ListView`とさまざまな`Adapter`Xamarin.Android でのクラス。 外観をカスタマイズする方法も示します、`ListView`行は、メモリ消費量を削減する再利用の重要性について説明しています。 アクティビティのライフ サイクルのしくみについていくつかはも`ListView`と`Adapter`を使用します。 Xamarin.iOS でクロスプラット フォーム対応のアプリケーションで作業している場合、`ListView`コントロールが ios の構造が似て`UITableView`(と、Android`Adapter`に似ていますが、 `UITableViewSource`)。

まず、簡単なチュートリアルが導入されています、`ListView`の基本的なコード例とします。 高度なトピックへのリンクを提供するを使用するための次に、`ListView`現実世界のアプリでします。


> [!NOTE]
> `RecyclerView`ウィジェットより高度で柔軟性のあるバージョンの`ListView`します。 `RecyclerView`後継は、 `ListView` (と`GridView`)、使用することをお勧めします。`RecyclerView`なく`ListView`新しいアプリの開発。 詳細については、次を参照してください。 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)します。



## <a name="listview-tutorial"></a>ListView のチュートリアル

[`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/) は、 [`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
スクロール可能な項目の一覧を作成するとします。 リスト項目が自動的に挿入を使用して、一覧に、 [ `IListAdapter`](https://developer.xamarin.com/api/type/Android.Widget.IListAdapter/)します。

このチュートリアルでは、文字列の配列から読み取られる国名のスクロール可能なリストを作成します。 リスト項目を選択すると、トースト メッセージは、一覧で項目の位置に表示されます。


という名前の新しいプロジェクトを開始**HelloListView**します。

という名前の XML ファイルを作成**list_item.xml**内で保存し、 **リソース/レイアウト/** フォルダー。 次を挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

このファイル内に配置される各項目のレイアウトを定義する、 [ `ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)します。

開いている`MainActivity.cs`を拡張するクラスを変更および[ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/) (の代わりに[ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/))。

```csharp
public class MainActivity : ListActivity
{
```

次のコードを挿入します [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))
方法:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);

    ListView.TextFilterEnabled = true;

    ListView.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args)
    {
        Toast.MakeText(Application, ((TextView)args.View).Text, ToastLength.Short).Show();
    };
}
```

これを読み込みませんレイアウト ファイルをアクティビティに注目してください (通常の扱いが[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)))。
代わりに、設定、 [`ListAdapter`](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)
プロパティが自動的に追加します。 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
画面全体を[ `ListActivity`](https://developer.xamarin.com/api/type/Android.App.ListActivity/)します。
このメソッドは、 [ `ArrayAdapter<T>`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/)には、リスト項目の配列を管理、 [ `ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)します。
、 [`ArrayAdapter<T>`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/)
コンス トラクターは、アプリケーション[ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/)、(前の手順で作成された) の各リスト項目のレイアウトの説明と`T[]`または [`Java.Util.IList<T>`](https://developer.xamarin.com/api/type/Java.Util.IList/)
挿入するオブジェクトの配列、 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
(定義されている次)。

、 [`TextFilterEnabled`](https://developer.xamarin.com/api/property/Android.Widget.AbsListView.TextFilterEnabled/)
プロパティは、テキストのフィルタ リング、 [ `ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)一覧をフィルター処理は、ユーザーの入力開始時にするようにします。

、 [`ItemClick`](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/)
イベントを使用して、数回のクリック ハンドラーをサブスクライブします。 内の項目のときに、 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
クリックされると、ハンドラーが呼び出されると、 [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
クリックされた項目からテキストを使用して、メッセージが表示されます。

独自のレイアウト ファイルを定義する代わりに、プラットフォームによって提供されるリスト項目の設計を使用することができます、 [ `ListAdapter`](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)します。
たとえば、を使用してお試しください`Android.Resource.Layout.SimpleListItem1`の代わりに`Resource.Layout.list_item`します。

次の追加`using`ステートメント。

```csharp
using System;
```
次に、次の文字列配列を追加のメンバーとして`MainActivity`:

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

これには、文字列の配列、 [ `ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)します。

アプリケーションを実行します。 一覧をスクロールまたは、フィルター処理する入力し、メッセージを表示する項目をクリックできます。 次のように表示されます。

[![国の名前を持つ ListView の例のスクリーン ショット](images/01-listview-example-sml.png)](images/01-listview-example.png#lightbox)

ただし、ハード コーディングされた文字列の配列を使用して、デザインのベスト プラクティスではありません。 示すためにわかりやすくするため、このチュートリアルで使用される 1 つ、 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
ウィジェット。 によって定義されている外部のリソースなどの文字列配列を参照することをお勧め、`string-array`プロジェクトのリソース**Resources/Values/Strings.xml**ファイル。 例えば:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloListView</string>
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

これらのリソースの文字列を使用する、 [ `ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/)元の置換 [`ListAdapter`](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)
次のコマンドライン:

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```
アプリケーションを実行します。 次のように表示されます。

[![小さいリスト名の ListView の例のスクリーン ショット](images/02-smaller-example-sml.png)](images/02-smaller-example.png#lightbox)


## <a name="going-further-with-listview"></a>ListView を進める

(下記のリンク) の残りのトピックについて見て包括的な操作、`ListView`クラスと、さまざまな種類のアダプターの種類と使用することができます。 構造は、次のとおりです。

-   **視覚的な外観**&ndash;の部分、`ListView`コントロールとどのように動作します。

-   **クラス**&ndash;表示に使用されるクラスの概要、`ListView`します。

-   **データ、ListView で表示する**&ndash;データの単純なリストを表示する方法を実装する方法`ListView's`便利な機能はさまざまな組み込みの行のレイアウトを使用する方法と再行ビューを使用して、アダプターがメモリを節約する方法。

-   **カスタムの外観**&ndash;のスタイルを変更する、`ListView`カスタム レイアウト、フォントおよび色。

-   **SQLite を使った**&ndash;と SQLite データベースからデータを表示する方法、`CursorAdapter`します。

-   **アクティビティのライフ サイクル**&ndash;設計に関する考慮事項を実装するときに`ListView`アクティビティ、場所、ライフ サイクルにする必要がありますしてデータを入力、リソースを解放する場合などです。

(6 つの部分に分割) についての概要から始まり、`ListView`の使用方法の段階的に複雑な例を紹介する前にクラス自体。

-   [ListView のパーツと機能](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
-   [データの ListView の設定](~/android/user-interface/layouts/list-view/populating.md)
-   [ListView の外観のカスタマイズ](~/android/user-interface/layouts/list-view/customizing-appearance.md)
-   [CursorAdapters の使用](~/android/user-interface/layouts/list-view/cursor-adapters.md)
-   [ContentProvider の使用](~/android/user-interface/layouts/list-view/content-provider.md)
-   [ListView とアクティビティのライフサイクル](~/android/user-interface/layouts/list-view/activity-lifecycle.md)


## <a name="summary"></a>まとめ

この一連の導入トピック`ListView`の組み込み機能を使用する方法の例をいくつか提供されていると、`ListActivity`します。 カスタム実装が説明されている`ListView`カラフルなレイアウトの許可されているし、SQLite データベースを使用して、アクティビティのライフ サイクルの関連性のに触れること、`ListView`実装します。


## <a name="related-links"></a>関連リンク

- [AccessoryViews (サンプル)](https://developer.xamarin.com/samples/monodroid/AccessoryViews/)
- [BasicTableAndroid (サンプル)](https://developer.xamarin.com/samples/monodroid/BasicTableAndroid/)
- [BasicTableAdapter (サンプル)](https://developer.xamarin.com/samples/monodroid/BasicTableAdapter/)
- [BuiltInViews (サンプル)](https://developer.xamarin.com/samples/monodroid/BuiltInViews/)
- [CustomRowView (サンプル)](https://developer.xamarin.com/samples/monodroid/CustomRowView/)
- [FastScroll (サンプル)](https://developer.xamarin.com/samples/monodroid/FastScroll/)
- [SectionIndex (サンプル)](https://developer.xamarin.com/samples/monodroid/SectionIndex/)
- [SimpleCursorTableAdapter (サンプル)](https://developer.xamarin.com/samples/monodroid/SimpleCursorTableAdapter/)
- [CursorTableAdapter (サンプル)](https://developer.xamarin.com/samples/monodroid/CursorTableAdapter/)
- [アクティビティ ライフ サイクルのチュートリアル](~/android/app-fundamentals/activity-lifecycle/index.md)
- [テーブルおよびセル (Xamarin.iOS) の使用](~/ios/user-interface/controls/tables/index.md)
- [ListView クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [ListActivity クラスのリファレンス](https://developer.xamarin.com/api/type/Android.App.ListActivity/)
- [BaseAdapter クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
- [ArrayAdapter クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
- [CursorAdapter クラスのリファレンス](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
