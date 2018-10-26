---
title: データの ListView の設定
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2017
ms.openlocfilehash: 57c69223a01074ed15714026b7e9ec4e995808e0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103180"
---
# <a name="populating-a-listview-with-data"></a>データの ListView の設定


## <a name="overview"></a>概要

行を追加する、 `ListView` 、レイアウトと実装に追加する必要がある、`IListAdapter`メソッドを`ListView`呼び出し自体を設定します。 Android には、組み込みが含まれています。`ListActivity`と`ArrayAdapter`、カスタム レイアウト XML やコードを定義することなく使用できるクラス。 `ListActivity`クラスが自動的に作成、`ListView`し、公開、`ListAdapter`アダプターを使用して表示する行ビューを指定するプロパティ。

組み込みのアダプターは、行ごとに使用されるパラメーターとして、ビューのリソース ID を受け取ります。 などの組み込みのリソースを使用する`Android.Resource.Layout`ため必要はありません書き込む独自にです。


## <a name="using-listactivity-and-arrayadapterltstringgt"></a>ListActivity と ArrayAdapter を使用して&lt;文字列&gt;

例では、 **BasicTable/HomeScreen.cs**これらのクラスを使用して表示する方法を示します、`ListView`のみ、わずか数行のコードで。

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


### <a name="handling-row-clicks"></a>クリックした行の処理

通常、`ListView`も (、曲の再生または連絡先を呼び出すことや、別の画面を表示) アクションを実行する行をタッチするユーザーに許可します。 対応するユーザーの仕上げのいずれかを指定する必要があります複数メソッドで実装される、 `ListActivity` &ndash; `OnListItemClick` &ndash;次のようにします。

[![SimpleListItem のスクリーン ショット](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

ユーザーが行をタッチし、`Toast`警告が表示されます。

[![スクリーン ショットのトースト行が処理されたときに表示されます。](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)


## <a name="implementing-a-listadapter"></a>ListAdapter を実装します。

`ArrayAdapter<string>` 便利ですが、その単純さがため非常に制限されています。 ただし、多くの場合にバインドする単なる文字列ではなく、ビジネス エンティティのコレクションがあります。
たとえば、単に一覧が必要な場合がありますし、従業員のクラスのコレクションのデータが構成されている場合は、各従業員の名前を表示します。 動作をカスタマイズする、`ListView`どのようなデータの表示を制御するのサブクラスを実装する必要があります`BaseAdapter`次の 4 つの項目をオーバーライドします。

-   **カウント**&ndash;への行の数が、データをコントロールに通知します。

-   **GetView** &ndash;行ごとのビューが返されるデータを設定します。
    このメソッドはパラメーターを持つ、`ListView`を再利用のため、未使用の既存の行を渡します。

-   **GetItemId** &ndash;行識別子を返します (通常、行番号を long 型の値があります)。

-   **この [int]** インデクサー&ndash;特定の行番号に関連付けられているデータを返します。

コード例で**BasicTableAdapter/HomeScreenAdapter.cs**示しますサブクラス`BaseAdapter`:

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


### <a name="using-a-custom-adapter"></a>カスタム アダプターを使用します。

組み込みに似ていますが、カスタム アダプターを使用して`ArrayAdapter`で渡し、 `context` 、`string[]`の値を表示します。

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

この例は、同じ行のレイアウトを使用するため (`SimpleListItem1`)、作成されたアプリケーションは、前の例と同じになります。


### <a name="row-view-re-use"></a>行ビューを再利用

この例では、6 項目だけがあります。 画面は、8 に合わせてから行を再利用は必要ありません。 数百または数千行を表示するとき、ことが数百または何千もを作成するメモリの無駄使い`View`と 8 個のオブジェクトは、一度に画面に収まります。 行がそのビューが再利用のためのキューに格納する画面から消えますときは、このような状況を回避します。 ユーザーがスクロールすると、`ListView`呼び出し`GetView`表示する新しいビューを要求する&ndash;利用が渡されますに未使用のビュー、`convertView`パラメーター。 コードは、ビューの新しいインスタンスを作成する必要がありますし、この値が null 場合は、それ以外の場合、そのオブジェクトのプロパティを再設定できで再利用できます。

`GetView`メソッドは行ビューを再利用するこのパターンに従う必要があります。

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

カスタム アダプター実装する必要があります*常に*再利用、`convertView`長い一覧を表示するときにメモリ不足実行されないことを確認する新しいビューを作成する前にオブジェクト。

一部のアダプター実装 (など、 `CursorAdapter`) がない、`GetView`メソッド、2 つの方法が必要ではなく`NewView`と`BindView`の責任を分離することで行を再使用を強制する`GetView`を 2 つにメソッド。 `CursorAdapter`ドキュメントで後述する例。


## <a name="enabling-fast-scrolling"></a>高速スクロールを有効にします。

高速スクロールにより、ユーザーが、追加 'ハンドル' リストの一部に直接アクセスするスクロール バーとして機能するを提供することで長いリストをスクロールします。 このスクリーン ショットは、高速スクロール ハンドルを示しています。

[![スクロールのハンドルを持つ高速スクロールのスクリーン ショット](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

設定と同じくらい簡単に表示される場合は、高速スクロール ハンドルの原因は、`FastScrollEnabled`プロパティを`true`:

```csharp
ListView.FastScrollEnabled = true;
```


### <a name="adding-a-section-index"></a>セクションのインデックスを追加します。

セクションのインデックスが長いリストを高速スクロールされるときに、ユーザーの追加のフィードバックを提供&ndash;にスクロールして、'' セクションが表示されます。 アダプターのサブクラスを実装する必要がありますを表示するセクションのインデックスが発生する、`ISectionIndexer`によって表示されている行のインデックスのテキストを指定するインターフェイス。

[![H で始まるセクションの上に表示 H のスクリーン ショット](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

実装する`ISectionIndexer`アダプターに次の 3 つのメソッドを追加する必要があります。

-   **GetSections** &ndash;表示可能なインデックスのタイトルのセクションの完全な一覧を提供します。 このメソッドに Java オブジェクトの配列が必要な場合、コードを作成する必要がありますので、 `Java.Lang.Object[]` .NET コレクション。 この例でとしてリストの先頭の文字の一覧を返します`Java.Lang.String`します。

-   **GetPositionForSection** &ndash;指定したセクション インデックスの最初の行位置を返します。

-   **GetSectionForPosition** &ndash;特定の行に対して表示されるセクションのインデックスを返します。


例では、`SectionIndex/HomeScreenAdapter.cs`ファイルは、コンス トラクターで、これらのメソッドと追加のコードを実装します。 コンス トラクターは、すべての行をループし、(項目並べ替える必要が既にありますこれが機能する) のタイトルの最初の文字を抽出してセクション インデックスを構築します。

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

タイトル セクション インデックスは、実際のセクションでは、1 対 1 にマップする必要はありません。 これは、ため、`GetPositionForSection`メソッドが存在します。
`GetPositionForSection` 営業案件にどのようなインデックスは、任意のセクションでは、リスト ビューではインデックス リストにはマップできます。 例:"z"は、インデックスにする必要がありますが、テーブル セクションのすべての文字がない可能性があります、25、または 24、または任意のセクションのインデックス"z"にマップする必要がありますに 26 へのマッピングを"z"、代わりにマップされるようにします。



## <a name="related-links"></a>関連リンク

- [BasicTableAndroid (サンプル)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (サンプル)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [FastScroll (サンプル)](https://developer.xamarin.com/samples/FastScroll/)
