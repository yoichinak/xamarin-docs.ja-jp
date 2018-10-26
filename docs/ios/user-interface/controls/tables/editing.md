---
title: Xamarin.iOS でのテーブルの編集
description: このドキュメントでは、Xamarin.iOS でのテーブルを編集する方法について説明します。 削除、編集モード、および行を挿入するにはスワイプがについて説明します。
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 1267de341a88130c18254f414d2fbb1c42595a0c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104820"
---
# <a name="editing-tables-with-xamarinios"></a>Xamarin.iOS でのテーブルの編集

内のメソッドをオーバーライドすることでテーブルの編集機能が有効になっている、`UITableViewSource`サブクラスです。 最も簡単な編集の動作には、1 つのメソッドのオーバーライドを実装できる、スワイプ、削除するジェスチャです。
編集モードのテーブルより複雑な編集 (移動行を含む) を実行できます。

## <a name="swipe-to-delete"></a>スワイプに削除する

機能を削除するスワイプは、iOS がユーザーの期待に自然なジェスチャです。 

 [![](editing-images/image10.png "削除するスワイプの例")](editing-images/image10.png#lightbox)

スワイプ ジェスチャを表示するには影響を次の 3 つのメソッドのオーバーライドがある、**削除**セルのボタン。

-   **CommitEditingStyle** – テーブル ソース検出のかどうか、このメソッドはオーバーライドされ、自動的に削除するスワイプ ジェスチャを有効にします。 メソッドの実装を呼び出す必要があります`DeleteRows`上、`UITableView`に表示されなくなり、(たとえば、配列、ディクショナリまたはデータベース) モデルから基になるデータを削除するセルがあります。 
-   **CanEditRow** – CommitEditingStyle の場合は、オーバーライド、すべての行が編集可能にすると見なされます。 このメソッドが実装され、(いくつかの特定の行またはすべての行) は false を返す場合、削除するスワイプ ジェスチャできなくそのセルにします。 
-   **TitleForDeleteConfirmation** – 必要に応じてテキストを指定、**削除**ボタンをクリックします。 このメソッドが実装されていない場合、ボタンのテキストが"Delete"になります。 


これらのメソッドの実装は、`TableSource`クラスの次のとおりです。

```csharp
public override void CommitEditingStyle (UITableView tableView, UITableViewCellEditingStyle editingStyle, Foundation.NSIndexPath indexPath)
{
    switch (editingStyle) {
        case UITableViewCellEditingStyle.Delete:
            // remove the item from the underlying data source
            tableItems.RemoveAt(indexPath.Row);
            // delete the row from the table
            tableView.DeleteRows (new NSIndexPath[] { indexPath }, UITableViewRowAnimation.Fade);
            break;
        case UITableViewCellEditingStyle.None:
            Console.WriteLine ("CommitEditingStyle:None called");
            break;
    }
}
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override string TitleForDeleteConfirmation (UITableView tableView, NSIndexPath indexPath)
{   // Optional - default text is 'Delete'
    return "Trash (" + tableItems[indexPath.Row].SubHeading + ")";
}
```

この例では、`UITableViewSource`を使用するが更新されました、 `List<TableItem>` (文字列配列) ではなく、データ ソースがサポートされているため、追加および削除から項目をコレクション。


## <a name="edit-mode"></a>編集モード

テーブルが編集モードの場合、ユーザーが各の行では、操作されたときに Delete ボタンが表示されます赤の 'stop' ウィジェットが表示されます。 テーブルには、順序を変更する行をドラッグできることを示す 'ハンドル' アイコンも表示されます。
**TableEditMode**サンプルが示すように、これらの機能を実装します。

 [![](editing-images/image11.png "TableEditMode サンプルに示すようにこれらの機能を実装します。")](editing-images/image11.png#lightbox)

多数の異なるメソッドがある`UITableViewSource`テーブルの編集モードの動作に影響します。

-   **CanEditRow** – 各行を編集できるかどうか。 両方のスワイプ-削除と編集モードでは削除しないようにする場合は false を返します。 
-   **CanMoveRow** true を戻り値: '移動 'ハンドルまたは移動を回避する場合は false を有効にします。 
-   **EditingStyleForRow** – 編集モードでのテーブルがこのメソッドからの戻り値がセルに赤い削除アイコンを表示または緑色のアイコンを追加するのかどうかを決定します。 返す`UITableViewCellEditingStyle.None`場合は、行を編集することはできません。 
-   **MoveRow** – テーブルに表示されるデータを一致するように、基になるデータ構造を変更できるように行が移動したときに呼び出されます。 


最初の 3 つのメソッドの実装比較的単純ですが – を使用する場合を除き、`indexPath`特定の行の動作を変更するだけでハードコーディング戻り値は、テーブル全体の。

```csharp
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you don't allow re-ordering
}
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    return UITableViewCellEditingStyle.Delete; // this example doesn't support Insert
}
```

`MoveRow`実装では新しい順序と一致する基になるデータ構造を変更する必要があるため、もう少し複雑になります。 データとして実装されるため、`List`次のコードは元の場所にあるデータ項目を削除し、新しい場所に挿入します。 場合は、データが (たとえば) の 'order' 列を持つ SQLite データベース テーブルに格納された、このメソッドはその列の数値の順序を変更するには、いくつか SQL 操作を実行する代わりに必要。

```csharp
public override void MoveRow (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    var item = tableItems[sourceIndexPath.Row];
    var deleteAt = sourceIndexPath.Row;
    var insertAt = destinationIndexPath.Row;
    
    // are we inserting 
    if (destinationIndexPath.Row < sourceIndexPath.Row) {
        // add one to where we delete, because we're increasing the index by inserting
        deleteAt += 1;
    } else {
        // add one to where we insert, because we haven't deleted the original yet
        insertAt += 1;
    }
    tableItems.Insert (insertAt, item);
    tableItems.RemoveAt (deleteAt);
}
```

テーブル編集モードを取得する最後に、**編集**ボタンを呼び出す必要がある`SetEditing`このような

```csharp
table.SetEditing (true, true);
```

ユーザーが終了すると、編集、**完了**ボタンは編集モードをオフにする必要があります。

```csharp
table.SetEditing (false, true);
```


## <a name="row-insertion-editing-style"></a>行の編集スタイルを挿入

テーブル内の行の挿入は一般的でないユーザー インターフェイス: 標準の iOS アプリのメインの例は、 **Edit Contact**画面。 このスクリーン ショットは、行の挿入機能のしくみを示します – 編集モードは、追加の行を (クリックされた) 場合は、データに追加行を挿入します。 編集が完了すると、一時的な **(新規追加)** 行は削除されます。

 [![](editing-images/image12.png "一時的なが新規に追加の編集が完了したら、行の削除")](editing-images/image12.png#lightbox)

多数の異なるメソッドがある`UITableViewSource`テーブルの編集モードの動作に影響します。 これらのメソッドがコード例では、次のように実装されました。

-   **EditingStyleForRow** – 返します`UITableViewCellEditingStyle.Delete`データ、および返しますを含む行の`UITableViewCellEditingStyle.Insert`最後の行 (そのデータの insert ボタンとして動作するには、具体的には追加されます)。 
-   **CustomizeMoveTarget** – ユーザーがセルこの省略可能なメソッドからの戻り値は、場所の選択を上書きできますを移動します。 つまり、'削除' 防ぐことができます、セルなどの任意の行が後に移動されないように次の使用例 – 特定の位置に、 **(新規追加)** 行。 
-   **CanMoveRow** true を戻り値: '移動 'ハンドルまたは移動を回避する場合は false を有効にします。 例では、最後の行は、非表示にのみの insert ボタンとしてサーバーに意図されているため移動 'ハンドル' を持っています。 


'Insert' の行を追加し、必要なくなった場合でもう一度削除する 2 つのカスタム メソッドを追加することもできます。 呼び出された、**編集**と**完了**ボタン。

-   **WillBeginTableEditing** –、**編集**ボタンは、呼び出し-タッチされた`SetEditing`テーブルを編集モードにします。 これは、場合、トリガー、WillBeginTableEditing メソッドを表示します、 **(新規追加)** 'insert 'ボタンとして機能するテーブルの最後の行。 
-   **DidFinishTableEditing** – 完了ボタンをタッチすると`SetEditing`が編集モードをオフに再度呼び出されます。 削除するコード例、 **(新規追加)** を編集するときに、テーブルから行は必要なくなりました。 


サンプル ファイルでこれらのメソッド オーバーライドが実装されている**TableEditModeAdd/Code/TableSource.cs**:

```csharp
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    if (tableView.Editing) {
        if (indexPath.Row == tableView.NumberOfRowsInSection (0) - 1)
            return UITableViewCellEditingStyle.Insert;
        else
            return UITableViewCellEditingStyle.Delete;
    } else // not in editing mode, enable swipe-to-delete for all rows
        return UITableViewCellEditingStyle.Delete;
}
public override NSIndexPath CustomizeMoveTarget (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath proposedIndexPath)
{
    var numRows = tableView.NumberOfRowsInSection (0) - 1; // less the (add new) one
    if (proposedIndexPath.Row >= numRows)
        return NSIndexPath.FromRowSection(numRows - 1, 0);
    else
        return proposedIndexPath;
} 
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return indexPath.Row < tableView.NumberOfRowsInSection (0) - 1;
}
```

これら 2 つのカスタム メソッドを追加および削除に使用、 **(新規追加)** 行とテーブルの編集モードを有効または無効にします。

```csharp
public void WillBeginTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // insert the 'ADD NEW' row at the end of table display
    tableView.InsertRows (new NSIndexPath[] { 
            NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0), 0) 
        }, UITableViewRowAnimation.Fade);
    // create a new item and add it to our underlying data (it is not intended to be permanent)
    tableItems.Add (new TableItem ("(add new)"));
    tableView.EndUpdates (); // applies the changes
}
public void DidFinishTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // remove our 'ADD NEW' row from the underlying data
    tableItems.RemoveAt ((int)tableView.NumberOfRowsInSection (0) - 1); // zero based :)
    // remove the row from the table display
    tableView.DeleteRows (new NSIndexPath[] { NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0) - 1, 0) }, UITableViewRowAnimation.Fade);
    tableView.EndUpdates (); // applies the changes
}
```

最後に、このコードをインスタンス化、**編集**と**完了**ボタンは、ラムダを有効にするか、影響を受ける場合は、編集モードを無効にします。

```csharp
done = new UIBarButtonItem(UIBarButtonSystemItem.Done, (s,e)=>{
    table.SetEditing (false, true);
    NavigationItem.RightBarButtonItem = edit;
    tableSource.DidFinishTableEditing(table);
});
edit = new UIBarButtonItem(UIBarButtonSystemItem.Edit, (s,e)=>{
    if (table.Editing)
        table.SetEditing (false, true); // if we've half-swiped a row
    tableSource.WillBeginTableEditing(table);
    table.SetEditing (true, true);
    NavigationItem.LeftBarButtonItem = null;
    NavigationItem.RightBarButtonItem = done;
});
```

使用することもできますが、この行の挿入 UI パターンは非常に多くの場合、使用されませんが、`UITableView.BeginUpdates`と`EndUpdates`挿入、または任意のテーブル内のセルの削除をアニメーション化するメソッド。 これらのメソッドを使用するためのルールは、値の差がによって返される`RowsInSection`間、`BeginUpdates`と`EndUpdates`呼び出しは net の追加/削除されたセル数と一致する必要があります、`InsertRows`と`DeleteRows`メソッド。 基になるデータ ソースは、テーブル ビューでの挿入/削除を一致するように変更されていない場合は、エラーが発生します。


## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
