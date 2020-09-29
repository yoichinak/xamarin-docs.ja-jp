---
title: Xamarin. Android での ListView の使用
description: ListView は、Android アプリケーションの重要な UI コンポーネントです。これは、メニューオプションの短い一覧から、連絡先またはインターネットお気に入りの長い一覧に至るまで、あらゆる場所で使用されます。 この機能を使用すると、組み込みスタイルで書式設定したり、広範にカスタマイズしたりできる行のスクロールリストを簡単に表示できます。
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 50784844d35e2f04436b05d9491149a3e0282bdc
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457189"
---
# <a name="xamarinandroid-listview"></a>Xamarin Android ListView

_ListView は、Android アプリケーションの重要な UI コンポーネントです。これは、メニューオプションの短い一覧から、連絡先またはインターネットお気に入りの長い一覧に至るまで、あらゆる場所で使用されます。この機能を使用すると、組み込みスタイルで書式設定したり、広範にカスタマイズしたりできる行のスクロールリストを簡単に表示できます。_

## <a name="overview"></a>概要

リストビューとアダプターは、Android アプリケーションの最も基本的な構成要素に含まれています。 クラスは、 `ListView` 短いメニューでも長いスクロールリストでも、データを表示するための柔軟な方法を提供します。 高速スクロール、インデックス、1つまたは複数の選択などの使いやすさ機能を提供し、アプリケーションのためのモバイル対応ユーザーインターフェイスを作成するのに役立ちます。 `ListView` インスタンスでは、行ビューに含まれているデータをフィードするための *Adapter* が必要です。

このガイド `ListView` では、および Xamarin のさまざまなクラスを実装する方法について説明します `Adapter` 。 また、の外観をカスタマイズする方法についても説明 `ListView` します。また、メモリ使用量を減らすために行を再利用することの重要性についても説明します。 また、アクティビティのライフサイクルがどのように影響し、使用するかについても説明 `ListView` `Adapter` します。 Xamarin を使用したクロスプラットフォームアプリケーションで作業している場合、 `ListView` コントロールは ios と構造的に似 `UITableView` ています (および Android はと似てい `Adapter` `UITableViewSource` ます)。

まず、簡単なチュートリアルでは、 `ListView` 基本的なコード例でを紹介します。 次に、より高度なトピックへのリンクを使用して、実際のアプリでを使用できるようにし `ListView` ます。

> [!NOTE]
> `RecyclerView`ウィジェットは、より高度で柔軟なバージョン `ListView` です。 `RecyclerView`は (および) の後継となるように設計されているので `ListView` `GridView` 、 `RecyclerView` `ListView` 新しいアプリ開発ではなくを使用することをお勧めします。 詳細については、「 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)」を参照してください。

## <a name="listview-tutorial"></a>ListView のチュートリアル

[`ListView`](xref:Android.Widget.ListView) はです。 [`ViewGroup`](xref:Android.Views.ViewGroup)
スクロール可能な項目の一覧を作成する。 リスト項目は、を使用してリストに自動的に挿入され [`IListAdapter`](xref:Android.Widget.IListAdapter) ます。

このチュートリアルでは、文字列配列から読み取られた国名のスクロール可能な一覧を作成します。 リスト項目を選択すると、リスト内の項目の位置がトーストメッセージに表示されます。

**HelloListView**という名前の新しいプロジェクトを開始します。

**list_item.xml**という名前の XML ファイルを作成し、 **Resources/Layout/** フォルダー内に保存します。 次のように挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

このファイルは、に配置される各項目のレイアウトを定義し [`ListView`](xref:Android.Widget.ListView) ます。

を開い `MainActivity.cs` て、(ではなく) 拡張するクラスを変更し [`ListActivity`](xref:Android.App.ListActivity) [`Activity`](xref:Android.App.Activity) ます。

```csharp
public class MainActivity : ListActivity
{
```

) メソッドに次のコードを挿入し [`OnCreate()`](xref:Android.App.Activity.OnCreate*) ます。

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

この操作では、アクティビティのレイアウトファイル (通常はを使用) は読み込まれないことに注意してください [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*) 。
代わりに、 [`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)
プロパティは、を自動的に追加します。 [`ListView`](xref:Android.Widget.ListView)
を画面全体に表示する場合は [`ListActivity`](xref:Android.App.ListActivity) 。
このメソッドは、に [`ArrayAdapter<T>`](xref:Android.Widget.ArrayAdapter`1) 格納されるリスト項目の配列を管理するを受け取り [`ListView`](xref:Android.Widget.ListView) ます。
[`ArrayAdapter<T>`](xref:Android.Widget.ArrayAdapter`1)
コンストラクターは、アプリケーション、 [`Context`](xref:Android.Content.Context) (前の手順で作成した) 各リスト項目のレイアウトの説明、 `T[]` またはを受け取ります。 [`Java.Util.IList<T>`](xref:Java.Util.IList)
に挿入するオブジェクトの配列。 [`ListView`](xref:Android.Widget.ListView)
(次の定義)。

[`TextFilterEnabled`](xref:Android.Widget.AbsListView.TextFilterEnabled)
プロパティは、のテキストフィルター処理を有効にし [`ListView`](xref:Android.Widget.ListView) ます。これにより、ユーザーが入力を開始すると、リストがフィルター処理されます。

[`ItemClick`](xref:Android.Widget.AdapterView.ItemClick)
イベントを使用して、クリックするハンドラーをサブスクライブできます。 の項目が [`ListView`](xref:Android.Widget.ListView)
がクリックされ、ハンドラーが呼び出され、 [`Toast`](xref:Android.Widget.Toast)
クリックした項目のテキストを使用して、メッセージが表示されます。

に独自のレイアウトファイルを定義するのではなく、プラットフォームによって提供されるリスト項目のデザインを使用でき [`ListAdapter`](xref:Android.App.ListActivity.ListAdapter) ます。
たとえば、の代わりにを使用してみてください `Android.Resource.Layout.SimpleListItem1` `Resource.Layout.list_item` 。

次の `using` ステートメントを追加します。

```csharp
using System;
```

次に、次の文字列配列をのメンバーとして追加し `MainActivity` ます。

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

これは、に格納される文字列の配列です [`ListView`](xref:Android.Widget.ListView) 。

アプリケーションを実行します。 一覧をスクロールするか、「」と入力してフィルター処理し、項目をクリックしてメッセージを表示することができます。 次のような結果が表示されます。

[![国名を使用した ListView の例のスクリーンショット](images/01-listview-example-sml.png)](images/01-listview-example.png#lightbox)

ハードコーディングされた文字列配列を使用することは、最適なデザイン方法ではないことに注意してください。 このチュートリアルでは、わかりやすくするために、 [`ListView`](xref:Android.Widget.ListView)
ウィジェット. `string-array`プロジェクト**リソース/値/Strings.xml**ファイルのリソースを使用して、などの外部リソースによって定義された文字列配列を参照することをお勧めします。 次に例を示します。

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

これらのリソース文字列をに使用するには、 [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter`1) 元の [`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)
次の行を使用します。

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```

アプリケーションを実行します。 次のような結果が表示されます。

[![名前の小さいリストを使用した ListView の例のスクリーンショット](images/02-smaller-example-sml.png)](images/02-smaller-example.png#lightbox)

## <a name="going-further-with-listview"></a>ListView をさらに進める

その他のトピック (下のリンク) では、 `ListView` クラスと共に使用できるさまざまな種類のアダプターについて、包括的に説明しています。 構造は次のとおりです。

- **視覚的外観** &ndash; コントロールの一部 `ListView` とその動作方法。

- **クラス** &ndash; の表示に使用されるクラスの概要   `ListView` 。

- ListView での**データの表示** &ndash;単純なデータの一覧を表示する方法ユーザビリティ機能を実装する方法、 `ListView's` さまざまな組み込みの行レイアウトを使用する方法、およびアダプターで行ビューを再利用してメモリを節約する方法について説明します。

- **カスタムの外観** &ndash;`ListView`カスタムレイアウト、フォント、および色を使用してのスタイルを変更する。

- **SQLite** &ndash; の使用で SQLite データベースのデータを表示する方法について説明 `CursorAdapter` します。

- **アクティビティのライフサイクル** &ndash; アクティビティを実装する際の設計に関する考慮事項 `ListView` (ライフサイクルのどこにあるかを含む)。データを設定し、リソースを解放するタイミングを含みます。

(6 つの部分に分かれている) 説明は、クラス自体の概要から始めて、 `ListView` その使用方法についてのより複雑な例を紹介します。

- [ListView のパーツと機能](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
- [ListView にデータを読み込む](~/android/user-interface/layouts/list-view/populating.md)
- [ListView の外観のカスタマイズ](~/android/user-interface/layouts/list-view/customizing-appearance.md)
- [CursorAdapters の使用](~/android/user-interface/layouts/list-view/cursor-adapters.md)
- [ContentProvider の使用](~/android/user-interface/layouts/list-view/content-provider.md)
- [ListView とアクティビティのライフサイクル](~/android/user-interface/layouts/list-view/activity-lifecycle.md)

## <a name="summary"></a>まとめ

この一連のトピックで `ListView` は、の組み込み機能を使用する方法の例をいくつか紹介しました `ListActivity` 。 ここでは、カラフルなレイアウトと SQLite データベースの使用を許可するのカスタム実装について説明 `ListView` し、実装におけるアクティビティのライフサイクルの関連性について簡単に触れました `ListView` 。

## <a name="related-links"></a>関連リンク

- [AccessoryViews (サンプル)](/samples/xamarin/monodroid-samples/accessoryviews)
- [BasicTableAndroid (サンプル)](/samples/xamarin/monodroid-samples/basictableandroid)
- [BasicTableAdapter (サンプル)](/samples/xamarin/monodroid-samples/basictableadapter)
- [BuiltInViews (サンプル)](/samples/xamarin/monodroid-samples/builtinviews)
- [CustomRowView (サンプル)](/samples/xamarin/monodroid-samples/customrowview)
- [FastScroll (サンプル)](/samples/xamarin/monodroid-samples/fastscroll)
- [SectionIndex (サンプル)](/samples/xamarin/monodroid-samples/sectionindex)
- [SimpleCursorTableAdapter (サンプル)](/samples/xamarin/monodroid-samples/simplecursortableadapter)
- [カーソル Tableadapter (サンプル)](/samples/xamarin/monodroid-samples/cursortableadapter)
- [アクティビティライフサイクルのチュートリアル](~/android/app-fundamentals/activity-lifecycle/index.md)
- [テーブルとセルの操作 (Xamarin. iOS)](~/ios/user-interface/controls/tables/index.md)
- [ListView クラスの参照](xref:Android.Widget.ListView)
- [ListActivity クラスリファレンス](xref:Android.App.ListActivity)
- [BaseAdapter クラスリファレンス](xref:Android.Widget.BaseAdapter)
- [ArrayAdapter クラスリファレンス](xref:Android.Widget.ArrayAdapter)
- [カーソルクラス参照のカーソル](xref:Android.Widget.CursorAdapter)