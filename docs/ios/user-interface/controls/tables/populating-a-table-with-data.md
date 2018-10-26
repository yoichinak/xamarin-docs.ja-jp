---
title: Xamarin.iOS でのデータのテーブルの作成
description: このドキュメントでは、Xamarin.iOS アプリケーションでデータを含むテーブルを作成する方法について説明します。 UITableViewSource、セルの再利用、インデックス、およびヘッダーとフッターの追加がについて説明します。
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 859afcf6ab9f3acfb56104fa68683ba28d913ce4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117139"
---
# <a name="populating-a-table-with-data-in-xamarinios"></a>Xamarin.iOS でのデータのテーブルの作成

行を追加する、`UITableView`を実装する必要がある、`UITableViewSource`サブクラス化とオーバーライド自体を設定するテーブルを表示するメソッドを呼び出します。

このガイドを内容します。

- UITableViewSource をサブクラス化
- セルの再利用
- インデックスを追加します。
- ヘッダーとフッターの追加


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>UITableViewSource をサブクラス化

A`UITableViewSource`サブクラスが割り当てられたにすべて`UITableView`します。 テーブル ビューでは、(例、行の数が必要なと、既定と異なる場合、各行の高さ) の自身をレンダリングする方法については、ソース クラスを照会します。 最も重要なは、ソースでは、データが格納されて各セルのビューが用意されています。

データを表示するテーブルの作成に必要な 2 つの必須の方法はあります。

-   **RowsInSection**戻り値 –、 [ `nint` ](http://developer.xamarin.com/guides/cross-platform/macios/nativetypes/)合計数の行のデータ テーブルを表示する必要があります。
-   **GetCell**戻り値 –、`UITableCellView`メソッドに渡される対応する行のインデックスのデータが設定されます。


BasicTable サンプル ファイル**TableSource.cs**の最もシンプルな実装を持つ`UITableViewSource`します。 テーブルに表示する文字列の配列を受け取るし、各文字列を含む既定のセル スタイルを返しますが、次のコード スニペットで参照できます。

```csharp
public class TableSource : UITableViewSource {

        string[] TableItems;
        string CellIdentifier = "TableCell";

        public TableSource (string[] items)
        {
            TableItems = items;
        }

        public override nint RowsInSection (UITableView tableview, nint section)
        {
            return TableItems.Length;
        }

        public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewCell cell = tableView.DequeueReusableCell (CellIdentifier);
            string item = TableItems[indexPath.Row];

            //---- if there are no cells to reuse, create a new one
            if (cell == null)
            { cell = new UITableViewCell (UITableViewCellStyle.Default, CellIdentifier); }

            cell.TextLabel.Text = item;

            return cell;
        }
}
```

A`UITableViewSource`ことができますを使用して、すべてのデータ構造では、単純な文字列の配列から (この例で説明) と一覧 <> またはその他のコレクションをします。 実装`UITableViewSource`メソッドは、基になるデータ構造からテーブルを分離します。

このサブクラスを使用するには、ソースを構築しのインスタンスに割り当てる文字列の配列を作成`UITableView`:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    table = new UITableView(View.Bounds); // defaults to Plain style
    string[] tableItems = new string[] {"Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers"};
    table.Source = new TableSource(tableItems);
    Add (table);
}
```

結果のテーブルのようになります。

 [![](populating-a-table-with-data-images/image3.png "実行しているサンプル テーブル")](populating-a-table-with-data-images/image3.png#lightbox)

ほとんどのテーブルには、タッチ、行を選択し、その他のアクション (、曲の再生または連絡先を呼び出すことや、別の画面を表示) を実行するユーザーが可能です。 これを実現するには、は、いくつかを行う必要があります。 最初に、作成、ユーザーに次のコードを追加することで行をクリックするときに、メッセージを表示する、AlertController、`RowSelected`メソッド。

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

次に、ビュー コント ローラーのインスタンスを作成します。

```csharp
HomeScreen owner;
```
パラメーターとしてビュー コント ローラーを取得し、フィールドに保存します UITableViewSource クラスにコンス トラクターを追加します。

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```
ViewDidLoad メソッドに渡す UITableViewSource クラスが作成される場所を変更、`this`参照。

```csharp
table.Source = new TableSource(tableItems, this);
```
最後に、もう一度、`RowSelected`メソッドを呼び出します`PresentViewController`でキャッシュされたフィールド。

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


ユーザーが行をタッチするようになりましたし、アラートが表示されます。



 [![](populating-a-table-with-data-images/image4.png "行の選択したアラート")](populating-a-table-with-data-images/image4.png#lightbox)


## <a name="cell-reuse"></a>セルの再利用

この例では 6 つのみ項目は、セルの再利用する必要はありません。 数百または数千行を表示するとき、ことが数百または何千もを作成するメモリの無駄使い`UITableViewCell`をいくつかは、一度に画面に収まる場合します。

セルがそのビューが再利用のためのキューに格納画面から消えますときは、このような状況を回避します。 ユーザーがスクロールすると、テーブルを呼び出す`GetCell`を表示する – (がない現在表示されている) 既存のセルを再利用する新しいビューを要求を呼び出すだけです、`DequeueReusableCell`メソッド。 セルを再利用が返されます使用できる場合はそれ以外の場合、null が返されます、コード セルの新しいインスタンスを作成する必要があります。

この例のコード部分では、パターンを示しています。

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

`cellIdentifier`効果的にさまざまな種類のセルの別のキューを作成します。 この例のすべてのセルになりますので 1 つだけのハードコーディングされた同じ識別子を使用します。 さまざまな種類のセルがある場合はそれぞれに与えるさまざまな id 文字列では、再利用のキューから要求されたときと、両方がインスタンス化されるときにします。

### <a name="cell-reuse-in-ios-6"></a>IOS 6 以降でのセルの再利用

iOS 6 では、コレクション ビューを 1 つの概要と同様のセルの再利用のパターンを追加します。 ただし、上記の既存の再利用パターンは、互換性のため、この新しいパターンはお勧め、セルの null チェックの必要性が削除されるので、引き続きサポートされます。

新しいパターンで、アプリケーションを登録、セル クラスまたはいずれかを呼び出すことによって使用される xib`RegisterClassForCellReuse`または`RegisterNibForCellReuse`コント ローラーのコンス トラクターでします。 その後、デキュー セルで、`GetCell`メソッドを呼び出すだけです`DequeueReusableCell`セル クラスまたは xib とインデックスのパスに登録した識別子を渡すことです。

たとえば、次のコードは、UITableViewController でカスタム セル クラスを登録します。

```csharp
public class MyTableViewController : UITableViewController
{
  static NSString MyCellId = new NSString ("MyCellId");

  public MyTableViewController ()
  {
    TableView.RegisterClassForCellReuse (typeof(MyCell), MyCellId);
  }
  ...
}
```

上記の MyCell クラスは、登録されているを使用して、セルでデキューできます、`GetCell`のメソッド、`UITableViewSource`以下に示すように、余分な null チェックの必要はありません。

```csharp
class MyTableSource : UITableViewSource
{
  public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
  {
    // if cell is not available in reuse pool, iOS will create one automatically
    // no need to do null check and create cell manually
    var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

    // do whatever you need to with cell, such as assigning properties, etc.

    return cell;
  }
}
```

注意してくださいを受け取るコンス トラクターを実装する必要がありますをカスタム セル クラスを使用して、新しい再利用パターンを使用する場合、 `IntPtr`、次のスニペットに示すように、それ以外の場合 OBJECTIVE-C ことはできません、セル クラスのインスタンスを構築します。

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

」で説明したトピックの例を参照できます、 **BasicTable**サンプルは、この記事にリンクさせます。

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>インデックスを追加します。

インデックスがインデックスを作成できますが、通常はアルファベット順の長いリストをスクロールして、ユーザーを支援するどのような条件でします。 **BasicTableIndex**サンプルは、インデックスを示すために、ファイルから項目の長いリストを読み込みます。 インデックス内の各項目は、テーブルの 'section' に対応します。

 [![](populating-a-table-with-data-images/image5.png "インデックスの表示")](populating-a-table-with-data-images/image5.png#lightbox)

'Sections' BasicTableIndex サンプルを作成するため、グループ化する必要があります、テーブルの背後にあるデータをサポートするために、`Dictionary<>`ディクショナリ キーとして各項目の最初の文字を使用して文字列の配列から。

```csharp
indexedTableItems = new Dictionary<string, List<string>>();
foreach (var t in items) {
    if (indexedTableItems.ContainsKey (t[0].ToString ())) {
        indexedTableItems[t[0].ToString ()].Add(t);
    } else {
        indexedTableItems.Add (t[0].ToString (), new List<string>() {t});
    }
}
keys = indexedTableItems.Keys.ToArray ();
```

`UITableViewSource`サブクラスですが、次のメソッドを追加または使用するように変更し、必要があります、 `Dictionary<>` :

-   **NumberOfSections** – このメソッドは省略可能、既定では、テーブルは、1 つのセクションを想定しています。 インデックスを表示するときに、このメソッドは (たとえば、26 インデックスには、英語のアルファベットのすべての文字が含まれている場合) のインデックスで項目の数を返す必要があります。
-   **RowsInSection** – 特定のセクションでは行の数を返します。
-   **SectionIndexTitles** –、インデックスの表示に使用される文字列の配列を返します。 サンプル コードでは、文字の配列を返します。


サンプル ファイルで更新されたメソッド**BasicTableIndex/TableSource.cs**ようになります。

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return keys.Length;
}
public override nint RowsInSection (UITableView tableview, nint section)
{
    return indexedTableItems[keys[section]].Count;
}
public override string[] SectionIndexTitles (UITableView tableView)
{
    return keys;
}
```

インデックスは通常、プレーンなテーブル スタイルのみ使用されます。


<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>ヘッダーとフッターの追加

ヘッダーとフッターは、視覚的に、テーブル内の行をグループに使用できます。 必要なデータ構造のインデックスを追加するとよく似ています、–`Dictionary<>`本当にうまく機能します。 セルのグループにアルファベットを使用する代わりにこの例は植物の種類によって野菜をグループ化します。
出力は次のようになります。

 [![](populating-a-table-with-data-images/image6.png "サンプルのヘッダーとフッター")](populating-a-table-with-data-images/image6.png#lightbox)

ヘッダーとフッターを表示する、`UITableViewSource`サブクラスには、これらの追加メソッドが必要です。

-   **TitleForHeader** – ヘッダーとして使用されるテキストを返します
-   **TitleForFooter** – フッターとして使用するテキストを返します。


サンプル ファイルで更新されたメソッド**BasicTableHeaderFooter/Code/TableSource.cs**ようになります。

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    return keys[section];
}
public override string TitleForFooter (UITableView tableView, nint section)
{
    return indexedTableItems[keys[section]].Count + " items";
}
```

ビューを使用してフッター、ヘッダーの外観をカスタマイズすることもできます。 オブジェクトを使用して、`GetViewForHeader`と`GetViewForFooter`メソッドのオーバーライドで`UITableViewSource`します。


## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
