---
title: "データで ListView を設定します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: 74d8533d0a757a307d88125701a482dfefd5eec2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="populating-a-listview-with-data"></a>データで ListView を設定します。

<a name="overview" />

## <a name="overview"></a>概要

行を追加する、`ListView`レイアウトや実装に追加する必要があります、`IListAdapter`メソッドを使用する、`ListView`呼び出し自体を設定します。 Android には、組み込みが含まれています。`ListActivity`と`ArrayAdapter`クラスを、XML のカスタム レイアウトやコードを定義することなく使用できます。 `ListActivity`クラスが自動的に作成、`ListView`を公開し、`ListAdapter`アダプターを経由して表示する行のビューを指定するプロパティです。

組み込みアダプターは、行ごとに使用されるパラメーターとしてビュー リソース ID を取得します。 などの組み込みのリソースを使用できる`Android.Resource.Layout`なくてできるように書き込む独自です。

<a name="Using_ListActivity_and_ArrayAdapterString" />

## <a name="using-listactivity-and-arrayadapterltstringgt"></a>ListActivity および ArrayAdapter を使用して&lt;文字列&gt;

例では、 **BasicTable/HomeScreen.cs**をこれらのクラスを使用して表示する方法を示します、`ListView`のみ数行のコードで。

```csharp
[Activity(Label = "BasicTable", MainLauncher = true, Icon = "@drawable/icon")]
public class HomeScreen : ListActivity {
   string[] items;
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       items = new string[] { "Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers" };
       ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItem1, items);
   }
   protected override void OnListItemClick(ListView l, View v, int position, long id)
}
```

<a name="Handling_Row_Clicks" />

### <a name="handling-row-clicks"></a>クリックした行の処理

通常、`ListView`も (、曲の再生の問い合わせ、または別の画面を表示) などのいくつかの操作を実行する行をタッチするユーザーに許可します。 応答するユーザー仕上げのいずれかを指定する必要が複数のメソッドとして実装された、 `ListActivity` &ndash; `OnListItemClick` &ndash;次のようにします。

[![SimpleListItem のスクリーン ショット](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

これで、ユーザーが行をタッチしたりしても、`Toast`警告が表示されます。

[![スクリーン ショットのトースト行が影響を受けるときに表示されます。](populating-images/basictable2.png)](populating-images/basictable2.png)

<a name="Implementing_a_ListAdapter" />

## <a name="implementing-a-listadapter"></a>ListAdapter を実装します。

`ArrayAdapter<string>` 優れたためその簡潔さが非常に制限します。 ただし、多くの場合があるバインドするだけの文字列ではなく、ビジネス エンティティのコレクション。
たとえば、Employee クラスのコレクションの場合、データは、だけ各従業員の名前を表示するリストをする可能性があります。 動作をカスタマイズする、`ListView`を表示するデータを制御するのサブクラスを実装する必要があります`BaseAdapter`次の 4 つの項目をオーバーライドします。

-   **カウント**&ndash;への行の数が、データをコントロールに通知します。

-   **GetView** &ndash;各行のビューが返されるデータを設定します。
    このメソッドのパラメーターには、`ListView`を再利用のため、未使用の既存の行に渡します。

-   **GetItemId** &ndash;行識別子を返す (通常、行番号を long 型の値ができます)。

-   **この [int]**インデクサー&ndash;特定の行番号に関連付けられているデータを返します。

コード例**BasicTableAdapter/HomeScreenAdapter.cs**示しますサブクラス`BaseAdapter`:

```csharp
public class HomeScreenAdapter : BaseAdapter<string> {
   string[] items;
   Activity context;
   public HomeScreenAdapter(Activity context, string[] items) : base() {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
  {
       return position;
   }
   public override string this[int position] {  
       get { return items[position]; }
   }
   public override int Count {
       get { return items.Length; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       View view = convertView; // re-use an existing view, if one is available
      if (view == null) // otherwise create a new one
           view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
       view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
       return view;
   }
}
```

<a name="Using_a_Custom_Adapter" />

### <a name="using-a-custom-adapter"></a>カスタム アダプターを使用します。

組み込みに似ていますが、カスタム アダプターを使用して`ArrayAdapter`を渡して、`context`と`string[]`の値を表示します。

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

この例は、同じ行のレイアウトを使用しているため (`SimpleListItem1`)、生成されたアプリケーションは、前の例と同じになります。

<a name="Row_View_Re-Use" />

### <a name="row-view-re-use"></a>行ビューを再利用

この例では、6 つのみ項目があります。 画面には、8 を満たすことはできませんから行を再利用は必要ありません。 数百または数千行を表示するときに、ことが数百または数千を作成するメモリの無駄使い`View`8 個の場合、オブジェクトが、時に画面に収まるです。 行がそのビューが再利用するためのキューに格納される、画面から消えますときは、このような状況を回避します。 ユーザーがスクロールしても、`ListView`呼び出し`GetView`を表示する新しいビューを要求する&ndash;使用可能な渡されます未使用のビューで、`convertView`パラメーター。 コードは、新しいビュー インスタンスを作成する必要がありますし、この値が null 場合は、それ以外の場合そのオブジェクトのプロパティを再設定して再利用します。

`GetView`メソッドは行のビューを再利用するこのパターンに従う必要があります。

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

カスタム アダプター実装する必要があります*常に*再利用、`convertView`長い一覧を表示するときにメモリが不足して実行されないことを確認する新しいビューを作成する前にオブジェクト。

一部のアダプター実装 (など、 `CursorAdapter`) がない、`GetView`メソッド、2 つの方法が必要ではなく`NewView`と`BindView`の役割を分離することにより行が再利用を強制する`GetView`を 2 つにメソッド。 `CursorAdapter`ドキュメントの後半の例です。

<a name="Enabling_Fast_Scrolling" />

## <a name="enabling-fast-scrolling"></a>高速スクロールを有効にします。

スクロールの高速では、ユーザーが、追加 'のハンドル' リストの一部に直接アクセスするスクロール バーとして機能することにより長い一覧をスクロールするのに役立ちます。 このスクリーン ショットは、高速スクロール ハンドルを示しています。

[![スクロールのハンドルを高速スクロールのスクリーン ショット](populating-images/fastscroll.png)](populating-images/fastscroll.png)

設定などの単純な原因で、高速スクロールへのハンドルが表示されますが、`FastScrollEnabled`プロパティを`true`:

```csharp
ListView.FastScrollEnabled = true;
```

<a name="Adding_a_Section_Index" />

### <a name="adding-a-section-index"></a>セクションのインデックスを追加します。

セクション インデックスは、長いリストを速くスクロールされるときにユーザーの追加のフィードバックを提供&ndash;どの 'section' にスクロールしてそれらを示しています。 アダプターのサブクラスを実装する必要がありますを表示するセクションのインデックスが発生する、`ISectionIndexer`によっては表示されている行のインデックスのテキストを指定するインターフェイス。

[![H で始まるセクションの上に表示される H のスクリーン ショット](populating-images/sectionindex.png)](populating-images/sectionindex.png)

実装する`ISectionIndexer`アダプターに次の 3 つのメソッドを追加する必要があります。

-   **GetSections** &ndash;セクションの完全な一覧に表示される可能性がありますのあるインデックスのタイトルを提供します。 このメソッドに Java オブジェクトの配列が必要な場合、コードを作成する必要がありますので、 `Java.Lang.Object[]` .NET コレクションから。 この例では、リストの先頭の文字の一覧を返します`Java.Lang.String`です。

-   **GetPositionForSection** &ndash;指定したセクション インデックスの最初の行位置を返します。

-   **GetSectionForPosition** &ndash;特定の行を表示するセクションのインデックスを返します。


例では、`SectionIndex/HomeScreenAdapter.cs`ファイル コンス トラクターで、これらのメソッドと追加のコードを実装します。 コンス トラクターは、すべての行をループし、(アイテム並べ替える必要が既にありますこれを行う)、タイトルの最初の文字を抽出したセクション インデックスを構築します。

```csharp
alphaIndex = new Dictionary<string, int>();
for (int i = 0; i < items.Length; i++) { // loop through items
   var key = items[i][0].ToString();
   if (!alphaIndex.ContainsKey(key))
       alphaIndex.Add(key, i); // add each 'new' letter to the index
}
sections = new string[alphaIndex.Keys.Count];
alphaIndex.Keys.CopyTo(sections, 0); // convert letters list to string[]

// Interface requires a Java.Lang.Object[], so we create one here
sectionsObjects = new Java.Lang.Object[sections.Length];
for (int i = 0; i < sections.Length; i++) {
   sectionsObjects[i] = new Java.Lang.String(sections[i]);
}
```

作成されると、データ構造を持つ、`ISectionIndexer`メソッドは非常に単純な。

```csharp
public Java.Lang.Object[] GetSections()
{
   return sectionsObjects;
}
public int GetPositionForSection(int section)
{
   return alphaIndexer[sections[section]];
}
public int GetSectionForPosition(int position)
{   // this method isn't called in this example, but code is provided for completeness
    int prevSection = 0;
    for (int i = 0; i < sections.Length; i++)
    {
        if (GetPositionForSection(i) > position)
        {
            break;
        }
        prevSection = i;
    }
    return prevSection;
}
```

セクション インデックス タイトル実際のセクションでは、1:1 にマップする必要はありません。 その理由は、`GetPositionForSection`メソッドが存在します。
`GetPositionForSection` 営業案件にどのようなインデックスは、リスト ビューにはどのようなセクションがインデックス リストに、マップできます。 たとえば、"z"は、インデックスにする必要がありますが、すべて状のテーブル セクションがない可能性があります、25、または 24、または任意のセクションのインデックス"z"にマップする必要がありますに 26 へのマッピングを"z"、代わりにマップされるようにします。



## <a name="related-links"></a>関連リンク

- [BasicTableAndroid (sample)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (サンプル)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [FastScroll (サンプル)](https://developer.xamarin.com/samples/FastScroll/)
