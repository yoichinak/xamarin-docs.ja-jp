---
title: Xamarin. iOS にドラッグアンドドロップ
description: このドキュメントでは、iOS 11 で導入された Api を使用して、Xamarin iOS アプリにドラッグアンドドロップを実装する方法について説明します。 具体的には、UITableView でドラッグアンドドロップを有効にする方法について説明します。
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/05/2017
ms.openlocfilehash: bbdfe9682474352fab7c0ae3b1cd7d949cbca0b4
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91437173"
---
# <a name="drag-and-drop-in-xamarinios"></a>Xamarin. iOS にドラッグアンドドロップ

_IOS 11 用のドラッグアンドドロップの実装_

iOS 11 には、iPad 上のアプリケーション間でデータをコピーするためのドラッグアンドドロップのサポートが含まれています。 ユーザーは、アプリを左右に並べて表示したり、アプリのアイコンをドラッグして、アプリを開いたり、データを削除したりできるようにすることで、すべての種類のコンテンツを選択してドラッグすることができます。

![カスタムアプリからメモアプリへのドラッグアンドドロップの例](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> ドラッグアンドドロップは、iPhone の同じアプリ内でのみ使用できます。

コンテンツを作成または編集できる場所でドラッグアンドドロップ操作をサポートすることを検討してください。

- テキストコントロールは、iOS 11 に対してビルドされたすべてのアプリのドラッグアンドドロップをサポートします。追加の作業は必要ありません。
- テーブルビューとコレクションビューには、iOS 11 の機能強化が含まれています。これにより、ドラッグアンドドロップ動作の追加が簡単になります。
- その他のビューは、追加のカスタマイズでドラッグアンドドロップをサポートするために作成できます。

アプリにドラッグアンドドロップのサポートを追加すると、さまざまなレベルのコンテンツの忠実性を提供できます。たとえば、書式設定されたテキストとプレーンテキスト形式のデータの両方を提供して、受信側のアプリがドラッグ先に最適なものを選択できるようにすることができます。 ドラッグ視覚エフェクトをカスタマイズしたり、一度に複数の項目をドラッグしたりすることもできます。

## <a name="drag-and-drop-with-text-controls"></a>テキストコントロールを含むドラッグアンドドロップ

`UITextView` と `UITextField` は、選択したテキストのドラッグ、およびのテキストコンテンツのドロップを自動的にサポートします。

<a name="uitableview"></a>

## <a name="drag-and-drop-with-uitableview"></a>UITableView を使用したドラッグアンドドロップ

`UITableView` には、テーブル行とのドラッグアンドドロップ操作の処理が組み込まれています。既定の動作を有効にするには、いくつかのメソッドを使用する必要があります。

次の2つのインターフェイスが関係します。

- `IUITableViewDragDelegate` –テーブルビューでドラッグが開始されたときに情報をパッケージ化します。
- `IUITableViewDropDelegate` –ドロップが試行されて完了したときに情報を処理します。

[DragAndDropTableView サンプル](/samples/xamarin/ios-samples/ios11-draganddroptableview)では、この2つのインターフェイスは両方とも、デリゲートとデータソースと共にクラスに実装されてい `UITableViewController` ます。 これらは、メソッドで割り当てられてい `ViewDidLoad` ます。

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

以下では、これら2つのインターフェイスに最低限必要なコードについて説明します。

### <a name="table-view-drag-delegate"></a>テーブルビューのドラッグデリゲート

テーブルビューからの行のドラッグをサポートするために _必要な_ メソッドは、だけです `GetItemsForBeginningDragSession` 。 ユーザーが行のドラッグを開始すると、このメソッドが呼び出されます。

実装の例を次に示します。 このメソッドは、ドラッグされた行に関連付けられたデータを取得し、それをエンコードして、 `NSItemProvider` アプリケーションが操作の "削除" 部分を処理する方法 (たとえば、データ型を処理できるかどうかなど) を決定するを構成し `PlainText` ます。

```csharp
public UIDragItem[] GetItemsForBeginningDragSession (UITableView tableView,
  IUIDragSession session, NSIndexPath indexPath)
{
  // gets the 'information' to be dragged
  var placeName = model.PlaceNames[indexPath.Row];
  // convert to NSData representation
  var data = NSData.FromString(placeName, NSStringEncoding.UTF8);
  // create an NSItemProvider to describe the data
  var itemProvider = new NSItemProvider();
  itemProvider.RegisterDataRepresentation(UTType.PlainText,
                                NSItemProviderRepresentationVisibility.All,
                                (completion) =>
  {
    completion(data, null);
    return null;
  });
  // wrap in a UIDragItem
  return new UIDragItem[] { new UIDragItem(itemProvider) };
}
```

ドラッグの動作をカスタマイズするために実装できるオプションメソッドは多数あります。たとえば、対象のアプリ (書式設定されたテキストやプレーンテキストなど、描画のベクターおよびビットマップバージョン) で利用できる複数のデータ表現を用意することができます。 また、同じアプリ内でドラッグアンドドロップするときに使用するカスタムデータ表現を指定することもできます。

### <a name="table-view-drop-delegate"></a>テーブルビューのドロップデリゲート

Drop デリゲートのメソッドは、ドラッグ操作がテーブルビューを介して実行されたとき、またはその上に完了したときに呼び出されます。 必須のメソッドは、データの削除が許可されているかどうか、および削除が完了した場合に実行されるアクションを決定します。

- `CanHandleDropSession` –ドラッグが進行中で、アプリケーションで削除される可能性がありますが、このメソッドは、ドラッグされているデータを削除できるかどうかを判断します。
- `DropSessionDidUpdate` –ドラッグの進行中は、このメソッドを呼び出して、目的のアクションを決定します。 ドラッグしているテーブルビューからの情報、ドラッグセッション、および使用可能なインデックスパスを使用して、ユーザーに提供される動作と視覚的フィードバックを特定できます。
- `PerformDrop` –ユーザーが (指を離すことによって) ドロップを完了すると、このメソッドはドラッグされているデータを抽出し、テーブルビューを変更して、新しい行 (または行) にデータを追加します。

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` ドラッグされているデータをテーブルビューが受け入れることができるかどうかを示します。 このコードスニペットでは、を `CanLoadObjects` 使用して、このテーブルビューが文字列データを受け入れることを確認します。

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate`ドラッグ操作の実行中にメソッドが繰り返し呼び出され、ユーザーに視覚的な手掛かりを提供します。

次のコードでは、を使用して、 `HasActiveDrag` 操作が現在のテーブルビューで開始されたかどうかを判断します。 その場合は、1つの行だけを移動できます。
ドラッグが別のソースからのものである場合、コピー操作が示されます。

```csharp
public UITableViewDropProposal DropSessionDidUpdate(UITableView tableView, IUIDropSession session, NSIndexPath destinationIndexPath)
{
  // The UIDropOperation.Move operation is available only for dragging within a single app.
  if (tableView.HasActiveDrag)
  {
    if (session.Items.Length > 1)
    {
        return new UITableViewDropProposal(UIDropOperation.Cancel);
    } else {
        return new UITableViewDropProposal(UIDropOperation.Move, UITableViewDropIntent.InsertAtDestinationIndexPath);
    }
  } else {
    return new UITableViewDropProposal(UIDropOperation.Copy, UITableViewDropIntent.InsertAtDestinationIndexPath);
  }
}
```

Drop 操作には、、、またはのいずれかを指定でき `Cancel` `Move` `Copy` ます。

ドロップの目的は、新しい行を挿入したり、既存の行にデータを追加/追加したりすることです。

#### <a name="performdrop"></a>パフォーマンスの低下

メソッドは、 `PerformDrop` ユーザーが操作を完了したときに呼び出され、テーブルビューとデータソースを変更して、削除されたデータを反映します。

```csharp
public void PerformDrop(UITableView tableView, IUITableViewDropCoordinator coordinator)
{
  NSIndexPath indexPath, destinationIndexPath;
  if (coordinator.DestinationIndexPath != null)
  {
    indexPath = coordinator.DestinationIndexPath;
    destinationIndexPath = indexPath;
  }
  else
  {
    // Get last index path of table view
    var section = tableView.NumberOfSections() - 1;
    var row = tableView.NumberOfRowsInSection(section);
    destinationIndexPath = NSIndexPath.FromRowSection(row, section);
  }
  coordinator.Session.LoadObjects(typeof(NSString), (items) =>
  {
    // Consume drag items
    List<string> stringItems = new List<string>();
    foreach (var i in items)
    {
      var q = NSString.FromHandle(i.Handle);
      stringItems.Add(q.ToString());
    }
    var indexPaths = new List<NSIndexPath>();
    for (var j = 0; j < stringItems.Count; j++)
    {
      var indexPath1 = NSIndexPath.FromRowSection(destinationIndexPath.Row + j, destinationIndexPath.Section);
      model.AddItem(stringItems[j], indexPath1.Row);
      indexPaths.Add(indexPath1);
    }
    tableView.InsertRows(indexPaths.ToArray(), UITableViewRowAnimation.Automatic);
  });
}
```

追加のコードを追加して、大きなデータオブジェクトを非同期に読み込むことができます。

### <a name="testing-drag-and-drop"></a>テスト (ドラッグアンドドロップを)

[サンプル](/samples/xamarin/ios-samples/ios11-draganddroptableview)をテストするには、iPad を使用する必要があります。
別のアプリ (メモなど) と共にサンプルを開き、行とテキストをドラッグします。

![ドラッグ操作のスクリーンショットが進行中です](drag-and-drop-images/01-sml.png)

## <a name="related-links"></a>関連リンク

- [ヒューマンインターフェイスのガイドライン (Apple) をドラッグアンドドロップする](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [テーブルビューサンプルのドラッグアンドドロップ](/samples/xamarin/ios-samples/ios11-draganddroptableview)
- [コレクションビューのサンプルのドラッグアンドドロップ](/samples/xamarin/ios-samples/ios11-draganddropcollectionview)
- [ドラッグアンドドロップ (WWDC) の概要 (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/203/)
- [コレクションとテーブルビュー (WWDC) を使用したドラッグアンドドロップ (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/223/)