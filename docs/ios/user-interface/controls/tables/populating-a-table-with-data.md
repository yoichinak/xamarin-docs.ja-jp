---
title: "データを含むテーブルを設定します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: fb0e4341d8d8ad0719f35c691add9bad1d3f85a8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="populating-a-table-with-data"></a>データを含むテーブルを設定します。

行を追加する、`UITableView`を実装する必要があります、`UITableViewSource`サブクラスとオーバーライドをそれ自体を設定するテーブルを表示するメソッドを呼び出します。

このガイドについて説明します。

- UITableViewSource をサブクラス化
- セルの再利用
- インデックスを追加します。
- ヘッダーとフッターの追加


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>UITableViewSource をサブクラス化

A`UITableViewSource`サブクラスが割り当てられているすべてに`UITableView`です。 テーブル ビューでは、それ自体の行の数が必要な既定値と異なる場合、各行の高さ) をレンダリングする方法を決定する元のクラスを照会します。 最も重要なは、ソースでは、データが設定される各セルのビューが用意されています。

データを表示するテーブルに必要な 2 つの必須の方法があります。

-   **RowsInSection**戻り値 –、 [ `nint` ](http://developer.xamarin.com/guides/cross-platform/macios/nativetypes/)合計数の行のデータ テーブルを表示する必要があります。
-   **GetCell**戻り値 –、`UITableCellView`のデータ、メソッドに渡される対応する行のインデックスを使用します。


BasicTable サンプル ファイル**TableSource.cs**の最もシンプルな実装を持つ`UITableViewSource`します。 わかります次のコード スニペットで、テーブルに表示される文字列の配列を受け取るし、各文字列を含む既定のセル スタイルを返します。

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

A`UITableViewSource`ように使用すべてのデータ構造では、単純な文字列の配列から (この例では) 一覧 <> またはその他のコレクション。 実装`UITableViewSource`メソッドは、基になるデータ構造からテーブルを分離します。

このサブクラスを使用するには、ソースを作成しのインスタンスに割り当てる文字列の配列を作成`UITableView`:

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

結果のテーブルは、次のようになります。

 [ ![](populating-a-table-with-data-images/image3.png "実行しているサンプル テーブル")](populating-a-table-with-data-images/image3.png)

ほとんどのテーブルは、タッチ、行を選択し、その他のアクション (など、曲の再生の問い合わせ、または別の画面を表示) を実行するユーザーを許可します。 これを達成するのには、いくつかを行う必要があります。 最初に、作成、AlertController ときに表示されるメッセージには、次を追加することによって、ユーザーが行をクリックして、`RowSelected`メソッド。

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

次に、このビューのコント ローラーのインスタンスを作成します。

```csharp
HomeScreen owner;
```
パラメーターとしてビュー コント ローラーを取得し、フィールドに保存 UITableViewSource クラスにコンス トラクターを追加します。

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
最後に、バックアップ、`RowSelected`メソッドを呼び出し`PresentViewController`キャッシュされたフィールドで。

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


ユーザーが行をタッチするようになりましたし、アラートが表示されます。



 [ ![](populating-a-table-with-data-images/image4.png "行の選択したアラート")](populating-a-table-with-data-images/image4.png)


## <a name="cell-reuse"></a>セルの再利用

この例では 6 つのみ項目が、セルの再利用する必要がないようにします。 数百または数千行を表示するときに、ことが数百または数千を作成するメモリの無駄使い`UITableViewCell`オブジェクトを一度に少数が画面に収まる時にします。

セルがそのビューが再利用するためのキューに格納される画面から消えますときは、このような状況を回避します。 テーブルを呼び出すように、ユーザーがスクロール`GetCell`を表示する: (現在が表示されない) を既存のセルを再利用する新しいビューを要求を呼び出すだけで、`DequeueReusableCell`メソッドです。 セルが再利用が返されます使用可能な場合は、それ以外の場合、null が返されます、コードが新しいセル インスタンスを作成する必要があります。

このスニペットの例のコードでは、パターンを示しています。

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

`cellIdentifier`実質的にさまざまな種類のセルの別のキューを作成します。 すべてのセルが同じため 1 つだけのハードコードされたを検索するこの例では、識別子が使用されます。 さまざまな種類のセルがあった場合、それぞれが必要異なる識別子の文字列では、再利用するキューから要求されたときと、両方がインスタンス化されるときです。

### <a name="cell-reuse-in-ios-6"></a>IOS 6 以降でのセルの再利用します。

iOS 6 は、コレクション ビューを 1 つの概要」のようなセルを再利用パターンを追加します。 上に示した既存の再利用パターンが互換性のため、この新しいパターンはお勧めセルに null チェックの必要性が削除されるので、でもサポートされています。

セルのクラスまたは xib を呼び出して、使用する新しいパターンを持つアプリケーションを登録`RegisterClassForCellReuse`または`RegisterNibForCellReuse`コント ローラーのコンス トラクターでします。 行うセルで、`GetCell`メソッド、単に呼び出し`DequeueReusableCell`セル クラスまたは xib とインデックスのパスに登録した識別子を渡します。

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

コンピューターを内デキュー MyCell クラスが登録されていると、セル、`GetCell`のメソッド、`UITableViewSource`余分な null チェックが示すように、以下の必要はありません。

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

対応するを受け取るコンス トラクターを実装する必要がありますをカスタム セル クラスを持つ新しい再利用パターンを使用する場合、 `IntPtr`、次のスニペットに示すように、それ以外の場合 OBJECTIVE-C ことはできません、セル クラスのインスタンスを構築します。

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

上記で説明したトピックの例を確認できます、 **BasicTable**サンプルは、この記事にリンクします。

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>インデックスを追加します。

インデックスにより、インデックスを作成することができますがアルファベット順通常の長い一覧をスクロールして、ユーザーするどのような条件でします。 **BasicTableIndex**サンプルは、インデックスを示すために、ファイルから項目の長いリストを読み込みます。 インデックス内の各項目は、テーブルの 'section' に対応します。

 [ ![](populating-a-table-with-data-images/image5.png "インデックスの表示")](populating-a-table-with-data-images/image5.png)

'Sections' したがって BasicTableIndex サンプルを作成し、グループ化する必要があります、テーブルの背後にあるデータをサポートするために、`Dictionary<>`ディクショナリのキーとして各項目の最初の文字を使用して文字列の配列から。

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

`UITableViewSource`サブクラスには、次のメソッドを追加または使用するように変更し、必要があります、 `Dictionary<>` :

-   **NumberOfSections** – このメソッドは省略可能、既定では、テーブルは、1 つのセクションを想定しています。 インデックスを表示するときに、このメソッドは、インデックス (たとえば、26 インデックスには、英語のアルファベットのすべての文字が含まれている場合) に項目の数を返す必要があります。
-   **RowsInSection** – 指定されたセクションの行の数を返します。
-   **SectionIndexTitles** –、インデックスを表示するために使用する文字列の配列を返します。 サンプル コードでは、文字の配列を返します。


サンプル ファイルの更新されたメソッドは、 **BasicTableIndex/TableSource.cs**ようになります。

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

インデックスは通常、標準のテーブルのスタイルでのみ使用します。


<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>ヘッダーとフッターの追加

ヘッダーとフッターは、視覚的に複数のテーブルの行グループを使用できます。 必要なデータ構造体は、インデックスを追加するとよく似ています、–`Dictionary<>`は本当にも動作します。 セルをグループ化するには、アルファベットを使用して、代わりにこの例は植物の型によって野菜がグループ化します。
出力は次のようになります。

 [ ![](populating-a-table-with-data-images/image6.png "サンプルのヘッダーとフッター")](populating-a-table-with-data-images/image6.png)

ヘッダーとページ フッターを表示する、`UITableViewSource`サブクラスには、これらの追加メソッドが必要です。

-   **TitleForHeader** – ヘッダーとして使用されるテキストを返します
-   **TitleForFooter** – フッターに使用されるテキストを返します。


サンプル ファイルの更新されたメソッドは、 **BasicTableHeaderFooter/Code/TableSource.cs**ようになります。

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

さらに、ヘッダーとフッターをビューの外観をカスタマイズすることができますオブジェクトを使用して、`GetViewForHeader`と`GetViewForFooter`メソッドのオーバーライドで`UITableViewSource`です。


## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
