---
title: Xamarin のデータを使用してテーブルにデータを読み込む
description: このドキュメントでは、Xamarin iOS アプリケーションのデータをテーブルに設定する方法について説明します。 ここでは、UITableViewSource、セルの再利用、インデックスの追加、ヘッダーとフッターについて説明します。
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: d700aa8c0744d08e6536614b8cb8b024a99c84e1
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69889789"
---
# <a name="populating-a-table-with-data-in-xamarinios"></a>Xamarin のデータを使用してテーブルにデータを読み込む

に`UITableView`行を追加するには、サブクラス`UITableViewSource`を実装し、テーブルビューがそれ自体を設定するために呼び出すメソッドをオーバーライドする必要があります。

このガイドの内容は次のとおりです。

- UITableViewSource のサブクラス化
- セルの再利用
- インデックスの追加
- ヘッダーとフッターの追加


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>UITableViewSource のサブクラス化

サブ`UITableViewSource`クラスは、すべて`UITableView`のに割り当てられます。 テーブルビューでは、ソースクラスに対してクエリを行い、それ自体をどのようにレンダリングするかを決定します (たとえば、必要な行の数と、既定と異なる場合の各行の高さ)。 最も重要なのは、データが設定された各セルビューがソースによって提供されることです。

テーブルにデータを表示するには、次の2つの必須メソッドが必要です。

- **Rowsinsection** –テーブルに[`nint`](~/cross-platform/macios/nativetypes.md)表示されるデータ行の合計数を返します。
- **Getcell** –メソッドに`UITableCellView`渡される対応する行インデックスのデータが設定されたを返します。


BasicTable サンプルファイル**TableSource.cs**には、の`UITableViewSource`最も簡単な実装があります。 次のコードスニペットでは、テーブルに表示する文字列の配列を受け取り、各文字列を含む既定のセルスタイルを返しています。

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

は`UITableViewSource` 、単純な文字列配列 (この例で示されているように) の任意のデータ構造を、> またはその他のコレクション < リストに使用できます。 メソッドの`UITableViewSource`実装により、基になるデータ構造からテーブルが分離されます。

このサブクラスを使用するには、ソースを構築する文字列配列を作成して、 `UITableView`のインスタンスに割り当てます。

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

結果のテーブルは次のようになります。

 [![](populating-a-table-with-data-images/image3.png "実行中のサンプルテーブル")](populating-a-table-with-data-images/image3.png#lightbox)

ほとんどのテーブルでは、ユーザーは行を操作して選択し、他のアクション (楽曲の再生、連絡先の呼び出し、別の画面の表示など) を実行できます。 これを実現するには、いくつかの作業を行う必要があります。 まず、次を`RowSelected`メソッドに追加して、ユーザーが行をクリックしたときにメッセージを表示する alertcontroller を作成します。

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

次に、ビューコントローラーのインスタンスを作成します。

```csharp
HomeScreen owner;
```

ビューコントローラーをパラメーターとして受け取り、それをフィールドに保存する、UITableViewSource クラスにコンストラクターを追加します。

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```

Uitableviewsource クラスを作成して`this`参照を渡す ViewDidLoad メソッドを変更します。

```csharp
table.Source = new TableSource(tableItems, this);
```

最後に、 `RowSelected`メソッドに戻り、キャッシュ`PresentViewController`されたフィールドでを呼び出します。

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


これで、ユーザーは行に触れることができ、アラートが表示されます。



 [![](populating-a-table-with-data-images/image4.png "選択された行の警告")](populating-a-table-with-data-images/image4.png#lightbox)


## <a name="cell-reuse"></a>セルの再利用

この例では、項目が6つだけなので、セルの再利用は必要ありません。 ただし、数百または数千の行を表示する場合は、一度に数百または数`UITableViewCell`千のオブジェクトを作成するために、メモリが無駄になることがあります。

このような状況を回避するため、セルが画面から見えなくなったときに、そのビューは再利用のためにキューに配置されます。 ユーザーがスクロールすると、テーブルは`GetCell`を呼び出して、表示する新しいビューを要求します。 (現在表示されていない) 既存`DequeueReusableCell`のセルを再利用するには、メソッドを呼び出します。 再利用できるセルがある場合は、それが返されます。それ以外の場合は null が返され、コードで新しいセルインスタンスを作成する必要があります。

この例のコードスニペットは、次のパターンを示しています。

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

は`cellIdentifier` 、さまざまな種類のセルに対して個別のキューを効果的に作成します。 この例では、すべてのセルが同じであるため、ハードコードされた識別子を1つだけ使用します。 異なる種類のセルがある場合は、インスタンス化されるときと再利用キューから要求されるタイミングの両方で、それぞれ異なる識別子文字列を持つ必要があります。

### <a name="cell-reuse-in-ios-6"></a>IOS 6 以降でのセルの再利用

iOS 6 では、コレクションビューの概要に似たセル再利用パターンを追加しました。 前に示した既存の再利用パターンは下位互換性のために引き続きサポートされていますが、セルの null チェックが不要になるため、この新しいパターンが推奨されます。

新しいパターンで`RegisterClassForCellReuse`は、コントローラーのコンストラクターでまたは`RegisterNibForCellReuse`を呼び出すことによって使用されるように、アプリケーションによってセルクラスまたは xib が登録されます。 次に、 `GetCell`メソッド`DequeueReusableCell`内のセルをデキューするときに、セルクラスまたは xib に登録した識別子を渡すか、インデックスパスを渡します。

たとえば、次のコードは、UITableViewController にカスタムセルクラスを登録します。

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

Mycell クラスが登録されている場合は、次に`GetCell`示すように`UITableViewSource` 、余分な null チェックを必要とせずに、のメソッドでセルをデキューできます。

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

カスタムセルクラスで新しい再利用パターンを使用する場合は、次のスニペットに示すように、を受け取る`IntPtr`コンストラクターを実装する必要があります。それ以外の場合、目的の C は cell クラスのインスタンスを構築できません。

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

上記のトピックの例については、この記事にリンクされている**Basictable**サンプルを参照してください。

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>インデックスの追加

インデックスは、ユーザーが長いリストをスクロールするのに役立ちます。通常はアルファベット順に並べられていますが、必要な条件によってインデックスを作成することもできます。 **Basictableindex**サンプルは、インデックスを示すために、ファイルからより長い項目のリストを読み込みます。 インデックス内の各項目は、テーブルの ' section ' に対応しています。

 [![](populating-a-table-with-data-images/image5.png "インデックス表示")](populating-a-table-with-data-images/image5.png#lightbox)

' Sections ' をサポートするには、テーブルの背後にあるデータをグループ化する必要があるため`Dictionary<>` 、basictableindex サンプルでは、各項目の最初の文字をディクショナリキーとして使用して、文字列の配列からを作成します。

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

サブ`UITableViewSource`クラスでは、 `Dictionary<>`を使用するために、次のメソッドを追加または変更する必要があります。

- **Numberofsections** –このメソッドは省略可能です。既定では、テーブルは1つのセクションを前提としています。 インデックスを表示する場合、このメソッドはインデックス内の項目数を返す必要があります (たとえば、インデックスに英語のアルファベットのすべての文字が含まれている場合は26など)。
- **Rowsinsection** –指定したセクションの行の数を返します。
- **Sectionindextitles** –インデックスを表示するために使用される文字列の配列を返します。 このサンプルコードは、文字の配列を返します。


サンプルファイル**Basictableindex/Tableource**の更新されたメソッドは次のようになります。

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

通常、インデックスは、Plain table スタイルでのみ使用されます。


<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>ヘッダーとフッターの追加

ヘッダーとフッターを使用して、テーブル内の行を視覚的にグループ化することができます。 必要なデータ構造は、インデックスの追加と非常によく`Dictionary<>`似ています。 a は実際にはうまく機能します。 この例では、アルファベットを使用してセルをグループ化するのではなく、植物 type を使用して野菜をグループ化します。
出力は次のようになります。

 [![](populating-a-table-with-data-images/image6.png "サンプルヘッダーとフッター")](populating-a-table-with-data-images/image6.png#lightbox)

ヘッダーとフッターを表示する`UITableViewSource`には、サブクラスに次の追加のメソッドが必要です。

- **タイトル forheader** –ヘッダーとして使用するテキストを返します
- **タイトル forfooter** –フッターとして使用するテキストを返します。


サンプルファイル**Basictableheaderfooter/Code/Tableource**の更新されたメソッドは次のようになります。

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

で`GetViewForFooter` `GetViewForHeader` は、メソッドとメソッドのオーバーライドを使用して、ビューオブジェクトでヘッダーとフッターの外観をカスタマイズできます。`UITableViewSource`


## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
