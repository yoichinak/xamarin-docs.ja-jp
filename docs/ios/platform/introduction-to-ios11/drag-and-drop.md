---
title: Xamarin.iOS にドラッグ アンド ドロップ
description: このドキュメントでは、ドラッグを実装して、iOS 11 で導入された Api を使用して Xamarin.iOS アプリを削除する方法について説明します。 有効化について説明します、具体的には、UITableView をドラッグ アンド ドロップします。
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/05/2017
ms.openlocfilehash: aa93e015a399e733a2bb52f087a1e482bc23a00a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112079"
---
# <a name="drag-and-drop-in-xamarinios"></a>Xamarin.iOS にドラッグ アンド ドロップ

_ドラッグ アンド ドロップの iOS 11 を実装します。_

iOS 11 含むドラッグ アンド ドロップのサポート、iPad 上のアプリケーション間のデータをコピーします。 ユーザーを選択し、配置されているアプリのサイドからまたはを開くし、削除するデータを許可するアプリをトリガーするアプリ アイコンの上にドラッグして、すべての種類のコンテンツをドラッグします。

![Notes アプリケーションのカスタム アプリからのドラッグ アンド ドロップの例](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> ドラッグ アンド ドロップでは、iPhone で同じアプリ内で使用できるのみです。

ドラッグのサポートを検討およびドロップ操作を任意の場所にコンテンツを作成または編集できます。

- テキスト コントロールは、iOS 11 の追加作業なしに対して作成されたすべてのアプリのドラッグ アンド ドロップをサポートします。
- テーブルのビューとコレクション ビューは、追加のドラッグを簡素化し、ドロップ動作を iOS 11 の機能強化が含まれます。
- その他のビューは、ドラッグをサポートし、さらにカスタマイズを削除するにできます。

追加のドラッグ アンド ドロップをサポートして、アプリをときに、さまざまなレベルのコンテンツの忠実性; を行うことができます。たとえばをドラッグ先に最適に適合する受信側のアプリを選択できます書式設定されたテキストとプレーン テキスト バージョンのデータの両方を提供可能性があります。 ドラッグの視覚エフェクトをカスタマイズし、一度に複数の項目をドラッグできるようにすることもできます。

## <a name="drag-and-drop-with-text-controls"></a>テキスト コントロールによるドラッグ アンド ドロップ

`UITextView` `UITextField`自動的に、選択したテキストをドラッグして、テキスト コンテンツの削除をサポートします。

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>UITableView によるドラッグ アンド ドロップ

`UITableView` 組み込みドラッグ アンド ドロップの既定の動作を有効にするいくつかのメソッドのみを必要とするテーブルの行との対話を処理しています

関連するは 2 つのインターフェイスがあります。

- `IUITableViewDragDelegate` – テーブル ビューで、ドラッグが開始される情報のパッケージ。
- `IUITableViewDropDelegate` –、ドロップダウンがされているときに情報を処理しようとして完了します。

[DragAndDropTableView サンプル](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)これら 2 つのインターフェイスは両方の実装、`UITableViewController`クラスをデリゲートとデータ ソースと共に使用します。 割り当てられている、`ViewDidLoad`メソッド。

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

これら 2 つのインターフェイスに必要な最小限のコードは以下について説明します。

### <a name="table-view-drag-delegate"></a>テーブル ビューのドラッグ デリゲート

唯一の方法_必要_テーブル ビューから行をドラッグすることをサポートするためには、`GetItemsForBeginningDragSession`します。 ユーザーは、行のドラッグを開始して、このメソッドが呼び出されます。

実装は、以下に示します。 ドラッグ中の行に関連付けられたデータを取得、エンコード、およびを構成します、`NSItemProvider`アプリケーションが操作の"drop"の一部を処理する方法を決定する (たとえば、かどうか処理できるデータ型、`PlainText`の例)。

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

多くの省略可能なメソッドが対象のアプリケーションでの利点を実行できる複数のデータ表現を提供するなどのドラッグ動作をカスタマイズする実装できるドラッグ デリゲートでは (なども、プレーン テキスト、またはベクターとして書式設定されたテキストとビットマップのバージョンの描画)。 ドラッグ アンド ドロップ、同じアプリ内に使用するカスタム データ表現を指定することもできます。

### <a name="table-view-drop-delegate"></a>テーブル ビューのドロップ デリゲート

ドラッグ操作またはテーブル ビューでは、上に発生しますが、その上が完了すると、ドロップ デリゲートにメソッドが呼び出されます。 必要なメソッドは、データを削除するには、許可するかどうかと、削除が完了した場合に実行するアクションを決定します。

- `CanHandleDropSession` – ドラッグが進行状況、およびアプリケーションにドロップされる可能性がありますのある中、このメソッドは、ドラッグされているデータが削除されるを許可されているかどうかを決定します。
- `DropSessionDidUpdate` – ドラッグの進行中は、このメソッドが呼び出され、どのようなアクションを決定します。 動作と、ユーザーに提供される視覚的なフィードバックを確認する、上にドラッグしたテーブルのビュー、ドラッグ セッション、および使用可能なインデックスのパスからの情報をすべてに使用することができます。
- `PerformDrop` – ときに、ユーザーには、(指を持ち上げる) で、削除が完了すると、このメソッドはドラッグされているデータを抽出し、新しい行 (または行) のデータを追加するテーブルのビューを変更します。

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` テーブル ビューがドラッグされているデータを受け入れるかどうかを示します。 このコード スニペットで`CanLoadObjects`このテーブルのビューが文字列データを受け入れることを確認するために使用します。

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate`ドラッグ操作の進行中は、ユーザーに視覚的な手掛かりを提供する、メソッドが繰り返し呼び出されます。

次のコードで`HasActiveDrag`操作が現在のテーブル ビューで発生したかどうかを判断するために使用します。 その場合は、単一行のみが移動する許可されます。
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

ドロップの操作には、いずれかを指定できます`Cancel`、 `Move`、または`Copy`します。

ドロップ目的は、新しい行を挿入または既存の行にデータを追加/追加できます。

#### <a name="performdrop"></a>PerformDrop

`PerformDrop`ユーザーは、操作が完了して、ドロップしたデータを反映するように、テーブル ビューとデータ ソースを変更します。 メソッドが呼び出されます。

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

IPad を使用してテストする必要があります、[サンプル](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)します。
(注を参照) などの別のアプリと共にサンプルを開き、それらの間の行とテキストをドラッグします。

![実行中のドラッグ操作のスクリーン ショット](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>関連リンク

- [ドラッグ アンド ドロップのヒューマン インターフェイス ガイドライン (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [ドラッグ アンド ドロップ テーブルのビューのサンプル](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [ドラッグ アンド ドロップ コレクション ビューのサンプル](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [ドラッグ アンド ドロップ (WWDC) (ビデオ) の概要](https://developer.apple.com/videos/play/wwdc2017/203/)
- [コレクションおよびテーブル ビュー (WWDC) (ビデオ) によるドラッグ アンド ドロップ](https://developer.apple.com/videos/play/wwdc2017/223/)
