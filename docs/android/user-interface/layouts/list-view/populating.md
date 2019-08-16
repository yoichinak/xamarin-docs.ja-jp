---
title: Xamarin. Android ListView にデータを読み込む
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2017
ms.openlocfilehash: e92aada7be8a296baeaa9eebfb18fe906b5c3b63
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522543"
---
# <a name="populating-a-xamarinandroid-listview-with-data"></a>Xamarin. Android ListView にデータを読み込む

に`ListView`行を追加するには、それをレイアウトに追加し、を`IListAdapter` `ListView`呼び出して自身にデータを設定するメソッドを使用してを実装する必要があります。 Android に`ListActivity`は、カスタムレイアウト`ArrayAdapter` XML やコードを定義せずに使用できる組み込みクラスとクラスが含まれています。 クラス`ListActivity`は、を`ListView`自動的に作成し`ListAdapter` 、プロパティを公開して、アダプターを介して表示する行ビューを提供します。

組み込みアダプターは、行ごとに使用されるパラメーターとして、ビューリソース ID を取得します。 などの`Android.Resource.Layout`組み込みリソースを使用して、独自のリソースを作成する必要はありません。

## <a name="using-listactivity-and-arrayadapterltstringgt"></a>Listactivity および arrayadapter&lt;文字列の使用&gt;

**Basictable/HomeScreen**の例では、これらのクラスを使用して、 `ListView`数行のコードでを表示する方法を示しています。

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


### <a name="handling-row-clicks"></a>行のクリックの処理

通常、 `ListView`では、ユーザーが何らかの操作 (楽曲の再生、連絡先の呼び出し、別の画面の表示など) を実行できるようにすることもできます。 ユーザーへの対応には、次`ListActivity`のように、 &ndash; `OnListItemClick` &ndash;に実装するメソッドをもう1つ追加する必要があります。

[![SimpleListItem のスクリーンショット](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

これで、ユーザーは行`Toast`に触れることができ、アラートが表示されます。

[![行にタッチしたときに表示されるトーストのスクリーンショット](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)


## <a name="implementing-a-listadapter"></a>ListAdapter の実装

`ArrayAdapter<string>`非常に簡単ですが、非常に限られています。 ただし、多くの場合、バインドする文字列だけではなく、ビジネスエンティティのコレクションが存在することになります。
たとえば、データが Employee クラスのコレクションで構成されている場合、リストに各従業員の名前のみを表示することができます。 表示されるデータを制御`ListView`するためのの動作をカスタマイズするには、次`BaseAdapter`の4つの項目をオーバーライドするサブクラスを実装する必要があります。

- **カウント**&ndash;データ内の行の数を制御することを指定します。

- **Getview**&ndash;各行のビューを返すには、データを設定します。
    このメソッドには、 `ListView`再利用のために既存の未使用の行を渡すためのパラメーターがあります。

- **GetItemId**&ndash;行識別子を返します (通常は行番号ですが、任意の long 型の値を指定できます)。

- **この [int]** インデクサー &ndash;は、特定の行番号に関連付けられたデータを返します。

**Basictableadapter/HomeScreenAdapter**のコード例では、サブクラス`BaseAdapter`化する方法を示しています。

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


### <a name="using-a-custom-adapter"></a>カスタムアダプターの使用

カスタムアダプターの使用は組み込み`ArrayAdapter`の`context`と似ています。これは、と`string[]`表示する値のを渡します。

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

この例では同じ行レイアウト (`SimpleListItem1`) を使用しているため、結果のアプリケーションは前の例と同じになります。


### <a name="row-view-re-use"></a>行ビューの再利用

この例では、項目は6つだけです。 画面は8に収まりますが、行を再利用する必要はありません。 ただし、数百または数千の行を表示する場合は、一度に1つの画面に`View` 8 つしか表示されないときに数百または数千のオブジェクトを作成するためにメモリの無駄が発生する可能性があります。 このような状況を回避するため、行が画面から見えなくなったときに、再利用のためにビューがキューに配置されます。 ユーザーがスクロール`ListView`すると&ndash; 、 `GetView`使用可能な場合は新しいビューを要求する呼び出しが、未使用のビューをパラメーターに渡します。`convertView` この値が null の場合は、コードで新しいビューインスタンスを作成する必要があります。それ以外の場合は、オブジェクトのプロパティを再設定して再利用することができます。

メソッド`GetView`は、次のパターンに従って行ビューを再利用する必要があります。

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

カスタムアダプターの実装では、新しいビュー `convertView`を作成する前にオブジェクトを再利用して、長いリストを表示するときにメモリが不足しないようにする必要があります。

一部のアダプター実装 ( `CursorAdapter`など) には`GetView`メソッドがないため、2つの異なるメソッド`NewView`が`BindView`必要であり、の役割を2つに分割`GetView`することによって行の再利用を強制します。メソッド. ドキュメントの後半`CursorAdapter`に例があります。


## <a name="enabling-fast-scrolling"></a>高速スクロールを有効にする

高速スクロールを使用すると、スクロールバーとして機能し、リストの一部に直接アクセスする "ハンドル" を追加することで、長い一覧をスクロールできます。 このスクリーンショットは、高速スクロールハンドルを示しています。

[![スクロールハンドルを使用した高速スクロールのスクリーンショット](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

高速スクロールハンドルが表示されるようにするには、 `FastScrollEnabled`プロパティを`true`次のように設定します。

```csharp
ListView.FastScrollEnabled = true;
```


### <a name="adding-a-section-index"></a>セクションインデックスの追加

セクションインデックスは、ユーザーが長い一覧&ndash;を通じて高速スクロールする場合に追加のフィードバックを提供し、スクロールした ' セクション ' を示します。 セクションインデックスが表示されるようにするには、アダプター `ISectionIndexer`サブクラスは、表示されている行に応じて、インデックステキストを指定するためにインターフェイスを実装する必要があります。

[![H で始まる前に表示されている H のスクリーンショット](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

を実装`ISectionIndexer`するには、アダプターに次の3つのメソッドを追加する必要があります。

- **Getsections**&ndash;表示される可能性のあるセクションのインデックスタイトルの完全な一覧を示します。 このメソッドには Java オブジェクトの配列が必要であるため、コード`Java.Lang.Object[]`で .net コレクションからを作成する必要があります。 この例では、リスト内の最初の文字の一覧がと`Java.Lang.String`して返されます。

- **Getpositionforsection**&ndash;指定されたセクションインデックスの最初の行位置を返します。

- **Getsectionforposition**&ndash;指定された行に表示されるセクションインデックスを返します。


この例`SectionIndex/HomeScreenAdapter.cs`のファイルは、これらのメソッドと、コンストラクター内のいくつかの追加コードを実装しています。 コンストラクターは、すべての行をループし、タイトルの最初の文字を抽出することにより、セクションインデックスを作成します (これが機能するには、項目が既に並べ替えられている必要があります)。

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

データ構造を作成した場合`ISectionIndexer` 、メソッドは非常に単純です。

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

セクションのインデックスタイトルでは、1:1 を実際のセクションにマップする必要はありません。 `GetPositionForSection`メソッドが存在する理由は次のようになります。
`GetPositionForSection`では、インデックスリスト内の任意のインデックスを、リストビュー内のセクションにマップすることができます。 たとえば、インデックスに "z" があるとしても、すべての文字に対応するテーブルセクションがない場合があります。したがって、"z" を26にマッピングするのではなく、25または24にマップしたり、"z" というセクションにマップする必要があるすべてのセクションにマップしたりすることができます。



## <a name="related-links"></a>関連リンク

- [BasicTableAndroid (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/basictableandroid)
- [BasicTableAdapter (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/basictableadapter)
- [FastScroll (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fastscroll)
