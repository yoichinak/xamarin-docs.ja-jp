---
title: Xamarin を使用したテーブルの編集
description: このドキュメントでは、Xamarin. iOS でテーブルを編集する方法について説明します。 ここでは、スワイプによる削除、編集モード、および行の挿入について説明します。
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: f82057957e76ee683e2a649fdf6c2350bf282c18
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528640"
---
# <a name="editing-tables-with-xamarinios"></a>Xamarin を使用したテーブルの編集

テーブル編集機能は、サブクラスのメソッドを`UITableViewSource`オーバーライドすることによって有効になります。 最も簡単な編集動作は、1回のメソッドオーバーライドで実装できる、削除時のスワイプジェスチャです。
編集モードのテーブルを使用して、より複雑な編集 (行の移動を含む) を行うことができます。

## <a name="swipe-to-delete"></a>スワイプして削除

削除のためにスワイプする機能は、ユーザーが期待する iOS の自然なジェスチャです。 

 [![](editing-images/image10.png "スワイプして削除する例")](editing-images/image10.png#lightbox)

セルに**Delete**ボタンを表示するには、スワイプジェスチャに影響するメソッドオーバーライドが3つあります。

- **Commit編集スタイル**–テーブルソースは、このメソッドがオーバーライドされたかどうかを検出し、スワイプして自動的に削除するジェスチャを有効にします。 メソッドの実装では、 `DeleteRows` `UITableView`でを呼び出して、セルを非表示にしたり、モデルから基になるデータ (配列、ディクショナリ、データベースなど) を削除したりする必要があります。 
- **Caneditrow** – Commit編集スタイルがオーバーライドされると、すべての行が編集可能であると見なされます。 このメソッドが実装されていて、false (特定の行またはすべての行) が返された場合、そのセルでは、[削除] ジェスチャを使用できません。 
- **TitleForDeleteConfirmation** –必要に応じて、 **[削除]** ボタンのテキストを指定します。 このメソッドが実装されていない場合、ボタンのテキストは "Delete" になります。 


これらのメソッドは、 `TableSource`クラスで次のように実装されます。

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

この例では`UITableViewSource` 、はコレクションの項目の`List<TableItem>`追加と削除をサポートしているため、(文字列配列ではなく) をデータソースとして使用するようにが更新されています。


## <a name="edit-mode"></a>編集モード

テーブルが編集モードの場合、行ごとに赤い "stop" ウィジェットが表示され、操作すると [削除] ボタンが表示されます。 また、このテーブルには、行をドラッグして順序を変更できることを示す "ハンドル" アイコンも表示されます。
**Tableeditmode**サンプルは、次のようにこれらの機能を実装します。

 [![](editing-images/image11.png "TableEditMode サンプルは、次のようにこれらの機能を実装します。")](editing-images/image11.png#lightbox)

に`UITableViewSource`は、テーブルの編集モードの動作に影響するさまざまなメソッドがいくつかあります。

- **Caneditrow** –各行を編集できるかどうかを指定します。 編集モードでは、スワイプと削除の両方が行われないようにするには false を返します。 
- **CanMoveRow** – move ' handle ' または false を有効にして移動を回避するには true を返します。 
- 編集**スタイルの行**–テーブルが編集モードの場合、このメソッドからの戻り値によって、そのセルに赤い削除アイコンと緑色の [追加] アイコンのどちらが表示されるかが決まります。 行`UITableViewCellEditingStyle.None`を編集可能にしない場合は、を返します。 
- **MoveRow** – テーブルに表示されるデータを一致するように、基になるデータ構造を変更できるように行が移動したときに呼び出されます。 


最初の3つのメソッドの実装は比較的単純です。を使用`indexPath`して特定の行の動作を変更する場合を除き、テーブル全体の戻り値をハードコーディングするだけです。

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

この`MoveRow`実装は、基になるデータ構造を新しい順序と一致するように変更する必要があるため、少し複雑になります。 データはとして実装さ`List`れているので、次のコードでは、古い場所のデータ項目を削除し、新しい場所に挿入します。 データが ' order ' 列を含む SQLite データベーステーブルに格納されている場合 (たとえば)、このメソッドでは、その列の数値の順序を変更するために、いくつかの SQL 操作を実行する必要があります。

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

最後に、テーブルを編集モードにするには、 **[編集]** ボタン`SetEditing`が次のようにを呼び出す必要があります。

```csharp
table.SetEditing (true, true);
```

また、ユーザーの編集が完了したら、 **[完了]** ボタンをクリックして編集モードをオフにする必要があります。

```csharp
table.SetEditing (false, true);
```


## <a name="row-insertion-editing-style"></a>行の挿入の編集スタイル

テーブル内からの行の挿入は、一般的ではないユーザーインターフェイスです。標準的な iOS アプリの主な例は、 **[連絡先の編集]** 画面です。 このスクリーンショットは、行の挿入機能のしくみを示しています。編集モードでは、追加の行が追加されています (クリックすると、追加の行がデータに挿入されます)。 編集が完了すると、一時的な **([新規追加])** 行が削除されます。

 [![](editing-images/image12.png "編集が完了すると、一時的な [新しい行の追加] が削除されます。")](editing-images/image12.png#lightbox)

に`UITableViewSource`は、テーブルの編集モードの動作に影響するさまざまなメソッドがいくつかあります。 これらのメソッドは、コード例で次のように実装されています。

- **編集スタイル forrow** –データ`UITableViewCellEditingStyle.Delete`を含む行に対してを返し`UITableViewCellEditingStyle.Insert` 、最後の行に対してを返します (挿入ボタンとして動作するように特別に追加されます)。 
- **カスタマイズの Emovetarget** –ユーザーがセルを移動するときに、この省略可能なメソッドから戻り値を使用して、場所の選択をオーバーライドできます。 つまり、この例では、 **(新しい行の追加)** 行の後に行が移動されないようにするために、特定の位置にあるセルが "ドロップ" されるのを防ぐことができます。 
- **CanMoveRow** – move ' handle ' または false を有効にして移動を回避するには true を返します。 この例では、最後の行の move ' handle ' が非表示になっています。これは、サーバーを insert ボタンとして使用するためです。 


また、"insert" 行を追加する2つのカスタムメソッドを追加し、不要になったときにもう一度削除します。 これらは、 **[編集]** ボタンと **[完了]** ボタンから呼び出されます。

- **Begintableedit** – **[編集]** ボタンに触れると、が`SetEditing`呼び出され、テーブルが編集モードになります。 これにより、"挿入ボタン" として機能するために、テーブルの末尾に **(新しい行の追加)** 行が表示されている場合に、このメソッドがトリガーされます。 
- **DidFinishTableEditing** – [完了] ボタンがタッチ`SetEditing`されたときに、編集モードを無効にするためにもう一度呼び出されます。 このコード例では、編集が不要になったときに、テーブルから **(add new)** 行を削除します。 


これらのメソッドのオーバーライドは、 **Tableeditmodeadd/Code/Tableource**というサンプルファイルに実装されています。

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

この2つのカスタムメソッドは、テーブルの編集モードが有効または無効になっている場合に、 **(新しい行の追加)** 行を追加および削除するために使用されます。

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

最後に、このコードでは、編集モードと完了時に編集モードを有効または無効にするラムダを使用して、 **edit**および**Done**ボタンをインスタンス化しています。

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

この行挿入 UI パターンはあまり頻繁には使用されませんが、 `UITableView.BeginUpdates`メソッド`EndUpdates`とメソッドを使用して、テーブル内のセルの挿入や削除をアニメーション化することもできます。 これらのメソッドを使用するためのルールでは、と`RowsInSection` `EndUpdates`の`BeginUpdates`呼び出しの間で返される値の差が、メソッド`InsertRows`および`DeleteRows`メソッドで追加または削除されたセルの数と一致する必要があります。 基になるデータソースが、テーブルビューでの挿入または削除に合わせて変更されていない場合は、エラーが発生します。


## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
