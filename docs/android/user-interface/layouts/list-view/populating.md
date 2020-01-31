---
title: Xamarin. Android ListView にデータを読み込む
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2017
ms.openlocfilehash: 7ec5537345536884e2dc3da02ab54a3ca00f760e
ms.sourcegitcommit: 6f09bc2b760e76a61a854f55d6a87c4f421ac6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/02/2020
ms.locfileid: "75607972"
---
# <a name="populating-a-xamarinandroid-listview-with-data"></a>Xamarin. Android ListView にデータを読み込む

`ListView` に行を追加するには、その行をレイアウトに追加し、`ListView` が呼び出すメソッドを使用して `IListAdapter` を実装し、それ自体にデータを設定する必要があります。 Android には、カスタムレイアウト XML やコードを定義せずに使用できる組み込みの `ListActivity` および `ArrayAdapter` クラスが含まれています。 `ListActivity` クラスは、`ListView` を自動的に作成し、`ListAdapter` プロパティを公開して、アダプターを介して表示する行ビューを提供します。

組み込みアダプターは、行ごとに使用されるパラメーターとして、ビューリソース ID を取得します。 `Android.Resource.Layout` などの組み込みリソースを使用して、独自のリソースを作成する必要はありません。

## <a name="using-listactivity-and-arrayadapterltstringgt"></a>ListActivity および ArrayAdapter&lt;文字列&gt; の使用

**Basictable/HomeScreen**の例では、これらのクラスを使用して、数行のコードでのみ `ListView` を表示する方法を示しています。

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
}
```

### <a name="handling-row-clicks"></a>行のクリックの処理

通常、`ListView` では、ユーザーが何らかの操作 (楽曲の再生、連絡先の呼び出し、別の画面の表示など) を実行することもできます。 ユーザーに応答するには、次のように &ndash; `OnListItemClick` `ListActivity` &ndash; に実装するメソッドが1つ以上必要です。

[![SimpleListItem の スクリーンショット](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

これで、ユーザーは行に触れることができ、`Toast` の警告が表示されます。

[![行にタッチしたときに表示されるトーストの スクリーンショット](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)

## <a name="implementing-a-listadapter"></a>ListAdapter の実装

`ArrayAdapter<string>` は単純なので非常に便利ですが、非常に限られています。 ただし、多くの場合、バインドする文字列だけではなく、ビジネスエンティティのコレクションが存在することになります。
たとえば、データが Employee クラスのコレクションで構成されている場合、リストに各従業員の名前のみを表示することができます。 表示されるデータを制御するために `ListView` の動作をカスタマイズするには、次の4つの項目をオーバーライドする `BaseAdapter` のサブクラスを実装する必要があります。

- **カウント**&ndash; によって、データに含まれる行の数を制御に指示します。

- **Getview** &ndash; データで設定された各行のビューを返します。
    このメソッドには、再利用のために既存の未使用の行に渡す `ListView` のパラメーターがあります。

- **GetItemId** &ndash; では、行識別子 (通常は行番号) が返されます。ただし、任意の long 型の値を指定できます。

- **この [int]** インデクサー &ndash;、特定の行番号に関連付けられたデータを返します。

**Basictableadapter/HomeScreenAdapter**のコード例では、`BaseAdapter`をサブクラス化する方法を示しています。

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

カスタムアダプターの使用は組み込みの `ArrayAdapter`と似ており、`context` と表示する値の `string[]` を渡します。

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

この例では同じ行レイアウト (`SimpleListItem1`) を使用しているので、結果として得られるアプリケーションは前の例と同じ外観になります。

### <a name="row-view-re-use"></a>行ビューの再利用

この例では、項目は6つだけです。 画面は8に収まりますが、行を再利用する必要はありません。 ただし、数百または数千の行を表示する場合は、一度に8つの `View` オブジェクトを作成するために、メモリの無駄になることがあります。 このような状況を回避するため、行が画面から見えなくなったときに、再利用のためにビューがキューに配置されます。 ユーザーがスクロールすると、`ListView` は `GetView` を呼び出して、使用可能な場合は &ndash; を表示するように新しいビューを要求します。これにより、`convertView` パラメーターに未使用のビューが渡されます。 この値が null の場合は、コードで新しいビューインスタンスを作成する必要があります。それ以外の場合は、オブジェクトのプロパティを再設定して再利用することができます。

`GetView` メソッドは、次のパターンに従って行ビューを再利用する必要があります。

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

カスタムアダプターの実装では、新しいビューを作成する前に、`convertView` オブジェクトを再利用して、長いリストを表示するときにメモリ不足にならない*ようにする*必要があります。

一部のアダプター実装 (`CursorAdapter`など) には、`GetView` メソッドはありません。その代わりに、`GetView` の役割を2つのメソッドに分離することによって行の再利用を強制する `NewView` と `BindView` の2つのメソッドが必要です。 このドキュメントでは、`CursorAdapter` の例が後で説明されています。

## <a name="enabling-fast-scrolling"></a>高速スクロールを有効にする

高速スクロールを使用すると、スクロールバーとして機能し、リストの一部に直接アクセスする "ハンドル" を追加することで、長い一覧をスクロールできます。 このスクリーンショットは、高速スクロールハンドルを示しています。

[![スクロールハンドルを使用した高速スクロールの スクリーンショット](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

高速スクロールハンドルが表示されるようにするには、`FastScrollEnabled` プロパティを `true`に設定するだけです。

```csharp
ListView.FastScrollEnabled = true;
```

### <a name="adding-a-section-index"></a>セクションインデックスの追加

セクションインデックスは、ユーザーが長い一覧をスクロールしているときにユーザーに追加のフィードバックを提供し &ndash; スクロールした ' セクション ' が表示されます。 セクションインデックスが表示されるようにするには、アダプターサブクラスは、表示されている行に応じてインデックステキストを指定するために `ISectionIndexer` インターフェイスを実装する必要があります。

[![h で始まるセクションの前に表示される H の スクリーンショット](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

`ISectionIndexer` を実装するには、次の3つのメソッドをアダプターに追加する必要があります。

- **Getsections** &ndash; 表示される可能性のあるセクションのインデックスタイトルの完全な一覧を提供します。 このメソッドには Java オブジェクトの配列が必要であるため、コードで .NET コレクションから `Java.Lang.Object[]` を作成する必要があります。 この例では、リスト内の最初の文字の一覧を `Java.Lang.String` として返します。

- **Getpositionforsection** &ndash; は、指定されたセクションインデックスの最初の行の位置を返します。

- **Getsectionforposition** &ndash; 特定の行に対して表示されるセクションインデックスを返します。

`SectionIndex/HomeScreenAdapter.cs` ファイルの例では、これらのメソッドと、コンストラクター内のいくつかの追加コードを実装しています。 コンストラクターは、すべての行をループし、タイトルの最初の文字を抽出することにより、セクションインデックスを作成します (これが機能するには、項目が既に並べ替えられている必要があります)。

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

データ構造を作成した場合、`ISectionIndexer` のメソッドは非常に単純です。

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

セクションのインデックスタイトルでは、1:1 を実際のセクションにマップする必要はありません。 `GetPositionForSection` メソッドが存在するのはこのためです。
`GetPositionForSection` を使用すると、インデックスの一覧にある任意のインデックスを、リストビュー内のどのセクションにもマップできます。 たとえば、インデックスに "z" があるとしても、すべての文字に対応するテーブルセクションがない場合があります。したがって、"z" を26にマッピングするのではなく、25または24にマップしたり、"z" というセクションにマップする必要があるすべてのセクションにマップしたりすることができます。

## <a name="related-links"></a>関連リンク

- [BasicTableAndroid (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/basictableandroid)
- [BasicTableAdapter (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/basictableadapter)
- [FastScroll (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fastscroll)
