---
title: "ドラッグ アンド ドロップ"
description: "ドラッグ アンド ドロップ iOS 11 用の実装"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/05/2016
ms.openlocfilehash: c24e46ca43e9efdf20e38a78410cc21054645ec7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="drag-and-drop"></a>ドラッグ アンド ドロップ

_ドラッグ アンド ドロップ iOS 11 用の実装_

iOS 11 を含むドラッグ アンド ドロップのサポートを iPad でのアプリケーション間でデータをコピーします。 ユーザーは、選択し、アプリが配置されているサイド バイ サイドからを開くには、削除するデータを許可するアプリをトリガーするアプリ アイコンの上にドラッグするか、すべての種類のコンテンツをドラッグします。

![メモ アプリにカスタム アプリからのドラッグ アンド ドロップの例](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> ドラッグ アンド ドロップでは、iPhone 上の同じアプリケーション内で使用できるのみです。

ドラッグをサポートし、ドロップ操作の任意の場所にコンテンツを作成または編集することができます。

- テキスト コントロールは、iOS 11 で、追加の作業なしに対してビルドされたすべてのアプリのドラッグ アンド ドロップをサポートします。
- テーブルのビューとコレクション ビューは、追加のドラッグを簡略化し、ドロップ動作する iOS 11 に拡張機能を含めます。
- ドラッグをサポートし、さらにカスタマイズを削除し、その他のビューを作成できます。

追加のドラッグ アンド ドロップ サポートされる場合、アプリに、さまざまなレベルのコンテンツの忠実性; を使用できます。たとえば、受信側のアプリが収まる最適なドラッグ先を選択できるように書式設定されたテキストとデータのプレーン テキスト バージョンの両方を提供可能性があります。 ドラッグの視覚エフェクトをカスタマイズし、一度に複数の項目をドラッグすることを有効にすることもできます。

## <a name="drag-and-drop-with-text-controls"></a>テキスト コントロールによるドラッグ アンド ドロップ

`UITextView` および`UITextField`自動的に、選択したテキストをドラッグして、テキスト コンテンツの削除をサポートします。

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>UITableView によるドラッグ アンド ドロップ

`UITableView` 組み込みの処理をドラッグ アンド ドロップを既定の動作を有効にするいくつかのメソッドだけを必要とするテーブルの行とのやり取りがあります。

2 つのインターフェイスを必要があります。

- `IUITableViewDragDelegate` – テーブル ビューで、ドラッグが開始される情報のパッケージです。
- `IUITableViewDropDelegate` – ドロップされているときに情報を処理しようし、して、完了します。

[DragAndDropTableView サンプル](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)これら 2 つのインターフェイスが両方で実装されている、`UITableViewController`クラス、デリゲートとデータ ソースと共にします。 割り当てられた、`ViewDidLoad`メソッド。

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

これら 2 つのインターフェイスの最低限必要なコードは、以下について説明します。

### <a name="table-view-drag-delegate"></a>テーブル ビュー ドラッグ デリゲート

唯一の方法_必要_テーブル ビューから行をドラッグすることをサポートするためには`GetItemsForBeginningDragSession`します。 ユーザーは、行のドラッグを開始して、このメソッドが呼び出されます。

実装は、以下に示します。 ドラッグした行に関連付けられているデータを取得、それには、エンコードされ、構成、`NSItemProvider`アプリケーションが操作の"drop"の一部を処理する方法を決定する (たとえば、かどうか、処理できるデータ型`PlainText`の例)。

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

対象アプリでの利点を実行することができますが、複数のデータ表現を提供するなどのドラッグ動作のカスタマイズに実装できるドラッグ デリゲートでは多くの省略可能な方法 (プレーン テキストとして well またはベクターとして書式設定テキストなど、ビットマップのバージョンの描画)。 ドラッグ アンド ドロップして同じアプリケーション内に使用するカスタム データ表現を指定することもできます。

### <a name="table-view-drop-delegate"></a>テーブル ビュー ドロップ デリゲート

ドロップ デリゲートのメソッドは、ドラッグ操作が表形式ビューを経由して行われますまたはその上に終了したときと呼ばれます。 必要なメソッドは、データが削除されるを許可されているかどうかと、削除が完了した場合に実行する動作を決定します。

- `CanHandleDropSession` – の中に、ドラッグ進行状況、およびアプリケーションにドロップされる可能性のある、このメソッドは、ドラッグされているデータが削除されるを許可されているかどうかを判断します。
- `DropSessionDidUpdate` – 中に、ドラッグが実行中のものではどのようなアクションを決定するこのメソッドが呼び出されます。 テーブル ビューの上にドラッグされている、ドラッグ セッションと使用可能なインデックスのパスの情報をすべて使用できますの動作と、ユーザーに提供される視覚的なフィードバックを確認します。
- `PerformDrop` – ときにユーザーには、(その指を持ち上げる) によって、削除が完了すると、このメソッドはドラッグされるデータを抽出し、新しい行または行のデータを追加するテーブルのビューを変更します。

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` テーブル ビューがドラッグされるデータを受け入れるかどうかを示します。 このコード スニペットで`CanLoadObjects`このテーブルのビューが文字列データをそのまま使用できることを確認するために使用します。

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate`ドラッグ操作の進行状況を視覚的な手掛かりをユーザーに提供するためには、メソッドが繰り返し呼び出されます。

下のコードで`HasActiveDrag`操作が現在のテーブル ビューで生成されたかどうかを決定するために使用します。 単一行のみが移動する許可されます。
別のソースからドラッグする場合は、コピー操作が示されます。

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

Drop 操作には、いずれかを指定できる`Cancel`、 `Move`、または`Copy`です。

ドロップの目的は、新しい行を挿入または既存の行にデータを追加/追加可能性があります。

#### <a name="performdrop"></a>PerformDrop

`PerformDrop`メソッドは、ユーザーは、操作を完了して、ドロップされたデータを反映するようにテーブルのビューとデータ ソースを変更します。

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

大規模なデータ オブジェクトを非同期的に読み込むために、追加のコードを追加できます。

### <a name="testing-drag-and-drop"></a>テストのドラッグ アンド ドロップ

IPad を使用してテストする必要があります、[サンプル](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)です。
(注を参照) などの別のアプリと共にサンプルを開き、それらの間の行とテキストをドラッグします。

![ドラッグ操作が進行中のスクリーン ショット](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>関連リンク

- [ドラッグ アンド ドロップのヒューマン インターフェイス ガイドライン (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [ドラッグ アンド ドロップ テーブルのビューのサンプル](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [ドラッグ アンド ドロップ コレクション ビューのサンプル](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [ドラッグ アンド ドロップ (WWDC) (ビデオ) の概要](https://developer.apple.com/videos/play/wwdc2017/203/)
- [コレクションとテーブル ビュー (WWDC) (ビデオ) によるドラッグ アンド ドロップ](https://developer.apple.com/videos/play/wwdc2017/223/)
