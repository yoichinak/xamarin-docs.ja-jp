---
title: "編集"
ms.topic: article
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 1ea4489cd6f9839d5d32c97aa7ded41e4f15538a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="editing"></a>編集

内のメソッドをオーバーライドすることでテーブルの編集機能が有効になっている、`UITableViewSource`サブクラスです。 最も簡単な編集の動作は、1 つのメソッドのオーバーライドで実装できるスワイプ-delete ジェスチャです。
複雑な編集 (移動行を含む) は、編集モードでテーブルを実行できます。

このガイドは、次になります。

- [削除へスワイプし](#Swipe_to_Delete)
- [編集モード](#Edit_Mode)
- [行挿入の編集スタイル](#row_insertion_editing_style)

<a name="Swipe_to_delete" />

## <a name="swipe-to-delete"></a>削除へスワイプし

スワイプして機能を削除するのには、ユーザーが期待 iOS で自然なジェスチャです。 

 [ ![](editing-images/image10.png "Delete にスワイプしての例")](editing-images/image10.png)

スワイプ ジェスチャを表示するには影響が 3 つのメソッドのオーバーライドがある、**削除**セルのボタン。

-   **CommitEditingStyle** – テーブル ソースは、このメソッドがオーバーライドされていて、自動的に有効にスワイプして-delete ジェスチャかどうかを検出します。 メソッドの実装を呼び出す必要があります`DeleteRows`上、`UITableView`に表示されなくなり、(たとえば、配列、ディクショナリまたはデータベース) モデルから基になるデータを削除するセルがあります。 
-   **CanEditRow** – 場合 CommitEditingStyle がオーバーライドされる、すべての行は編集可能であると見なされます。 このメソッドが実装され、(いくつか特定の行のまたはすべての行) は false を返す場合、スワイプ-delete ジェスチャできなくなりますそのセルにします。 
-   **TitleForDeleteConfirmation** – 必要に応じてテキストを指定、**削除**ボタンをクリックします。 このメソッドが実装されていない場合、ボタン テキストは"Delete"になります。 


これらのメソッドの実装は、`TableSource`クラスの次のようにします。

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

この例については、`UITableViewSource`を使用するが更新されて、 `List<TableItem>` (配列の代わりに、文字列) をサポートするために、データがソースとしての追加と削除の項目コレクションからです。

<a name="Edit_mode" />

## <a name="edit-mode"></a>編集モード

テーブルが編集モードの場合、ユーザーは、行ごとに、触れると削除 ボタンを明らかに赤の 'stop' ウィジェットを表示できます。 テーブルには、順序を変更する行をドラッグできることを示すために 'ハンドル' アイコンも表示されます。
**TableEditMode**サンプルが示すように、これらの機能を実装します。

 [ ![](editing-images/image11.png "TableEditMode サンプルが示すように、これらの機能を実装します。")](editing-images/image11.png)

さまざまな方法の数がある`UITableViewSource`テーブルの編集モードの動作に影響を与えます。

-   **CanEditRow** – 各行を編集できるかどうか。 スワイプ-削除と編集モードでの削除の両方を防ぐために false を返します。 
-   **CanMoveRow** true を戻り値 – 移動 'ハンドル' または移動を回避するには false を有効にします。 
-   **EditingStyleForRow** – このテーブルは編集モードにするときこのメソッドからの戻り値は、セルに赤い削除アイコンが表示されますか、緑色のアイコンを追加するかどうかを決定します。 返す`UITableViewCellEditingStyle.None`場合は、行を編集することはできません。 
-   **MoveRow** – テーブルに表示されるデータを一致するように、基になるデータ構造を変更できるように行が移動したときに呼び出されます。 


最初の 3 つのメソッドの実装比較的単純ですが – を使用する場合を除き、`indexPath`特定の行の動作を変更するだけでハードコーディング戻り値は、テーブル全体にします。

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

`MoveRow`実装では新しい順序と一致する基になるデータ構造を変更する必要があるため、もう少し複雑になります。 データとして実装されるため、`List`次のコードは元の場所にあるデータ項目を削除し、新しい場所に挿入します。 場合は、データは、(たとえば) 'order' 列を持つ SQLite データベース テーブルに格納された、このメソッドはその列の数値の順序を変更する SQL 操作を実行する代わりに必要。

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

最後に、取得するテーブルを編集モードでは、**編集** ボタンを呼び出す必要がある`SetEditing`次のように

```csharp
table.SetEditing (true, true);
```

ユーザーが終了すると、編集、**実行**ボタンは編集モードをオフにする必要があります。

```csharp
table.SetEditing (false, true);
```

<a name="Edit_mode_–_row_insertion_editing_style" />

## <a name="row-insertion-editing-style"></a>行挿入編集スタイル

テーブル内の行の挿入は珍しいことでユーザー インターフェイス: 標準的な iOS アプリの主要な例は、 **Edit Contact**画面。 このスクリーン ショットは、行の挿入機能のしくみを示しています: 編集モードは、追加の行を (がクリックされたとき) は、データに追加行を挿入します。 編集が完了すると、一時的な**(新規追加)**行は削除されます。

 [ ![](editing-images/image12.png "一時的なが新規に追加の編集が完了したら、行が削除されます。")](editing-images/image12.png)

さまざまな方法の数がある`UITableViewSource`テーブルの編集モードの動作に影響を与えます。 これらのメソッドは、コード例に次のように実装されています。

-   **EditingStyleForRow** – 返します`UITableViewCellEditingStyle.Delete`データ、および返しますを含む行の`UITableViewCellEditingStyle.Insert`(具体的には、挿入ボタンとして動作に追加されます) を最後の行にします。 
-   **CustomizeMoveTarget** – ユーザーがセルこの省略可能なメソッドからの戻り値は、好みの場所を上書きできますを移動します。 つまり、'ドロップ' 防ぐことができます、セルなどの任意の行が後に移動されていることを防止する次の使用例 – 特定の位置に、 **(新規追加)**行です。 
-   **CanMoveRow** true を戻り値 – 移動 'ハンドル' または移動を回避するには false を有効にします。 例では、最後の行は、非表示に必要なのでサーバーにのみ挿入ボタンとして移動 'ハンドル' をいます。 


'Insert' の行を追加し、必要なくなった場合でもう一度削除する 2 つのカスタム メソッドを追加することもできます。 呼び出される、**編集**と**完了**ボタン。

-   **WillBeginTableEditing** –、**編集**ボタンは、呼び出しに操作された`SetEditing`テーブルを編集モードにします。 これは、場合、トリガー、WillBeginTableEditing メソッドの場所を表示、 **(新規追加)** 'insert button' として機能するテーブルの最後の行。 
-   **DidFinishTableEditing** – [終了] ボタンの影響を受けると`SetEditing`編集モードをオフにするためにもう一度呼び出されます。 削除するコード例、 **(新規追加)**テーブルの行を編集する場合は必要なくなりました。 


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

これら 2 つのカスタム メソッドを使用して追加および削除、 **(新規追加)**行とテーブルの編集モードを有効または無効にします。

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

このコードの最後に、インスタンスを作成、**編集**と**完了**ラムダが有効にするにまたはに接しているときに編集モードを無効にすると、ボタン。

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

使用することもできますが、この行挿入 UI パターンは非常に多くの場合、使用できません、`UITableView.BeginUpdates`と`EndUpdates`挿入、または任意のテーブル内のセルの削除をアニメーション化するメソッド。 これらのメソッドを使用するためのルールは、値の差がによって返される`RowsInSection`間、`BeginUpdates`と`EndUpdates`呼び出しは、net セルの数を追加/削除と一致する必要があります、`InsertRows`と`DeleteRows`メソッドです。 基になるデータ ソースは、テーブル ビューでの挿入/削除を一致するように変更されていない場合、エラーが発生します。


## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
