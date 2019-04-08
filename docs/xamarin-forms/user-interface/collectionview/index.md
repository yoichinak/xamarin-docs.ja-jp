---
title: Xamarin.Forms CollectionView
description: CollectionView は、異なるレイアウト仕様を使ってデータのリストを表示するための柔軟で高パフォーマンスなビューです。
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: b22449659d2d4b7791328d53ed2d2b29d405ffc8
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329945"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

![[プレビュー]](~/media/shared/preview.png)

> [!IMPORTANT]
> `CollectionView`は現在プレビュー段階で、計画されている機能の一部が不足しています。 さらに、実装が完了すると、API が変更されることがあります。

`CollectionView` は、異なるレイアウト仕様を使用してデータのリストを表示するためのビューです。 これは [ `ListView`](xref:Xamarin.Forms.ListView) の代わりとして、より柔軟でより高パフォーマンスを提供することを目的にしています。 `CollectionView` と `ListView` の API は似ていますが、注意すべきいくつかの違いがあります。

- `CollectionView` は柔軟なレイアウトモデルを持ち、データをリストまたはグリッドに、垂直方向または水平方向に表示することが可能です。
- `CollectionView` にはセルの概念はありません。 代わりに、データテンプレートを使用して、リストのデータの各アイテムの外観を定義します。
- `CollectionView` は基になるネイティブ コントロールによって提供される仮想化を自動的に利用します。
- `CollectionView` API サーフェスを減少[ `ListView`](xref:Xamarin.Forms.ListView)します。 多くのプロパティおよびイベントから`ListView`が存在しない`CollectionView`します。
- `CollectionView` 組み込みの区切り記号は含まれません。

`CollectionView` Xamarin.Forms 4.0 より前のリリースで使用できます。 ただし、現在、試験段階は、し、次のコードの行を追加することでのみ使用できます、`AppDelegate`クラスでは、iOS、または、 `MainActivity` android では、クラスを呼び出す前に`Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!NOTE]
> `CollectionView` iOS と Android ではできるだけです。

## <a name="populate-collectionview-with-datapopulate-datamd"></a>[データ CollectionView を設定します。](populate-data.md)

A`CollectionView`設定によってデータが読み込まれて、`ItemsSource`プロパティを実装するコレクションを`IEnumerable`します。 設定して、リストの各項目の外観を定義することができます、`ItemTemplate`プロパティを[ `DataTemplate`](xref:Xamarin.Forms.DataTemplate)します。

## <a name="specify-collectionview-layoutlayoutmd"></a>[CollectionView レイアウトを指定します](layout.md)

既定で、`CollectionView`を垂直方向に一覧の項目が表示されます。 ただし、垂直および水平方向のリストとグリッドを指定することができます。

## <a name="set-collectionview-selection-modeselectionmd"></a>[CollectionView 選択モードを設定します。](selection.md)

既定では、`CollectionView`選択が無効になっています。 ただし、1 つまたは複数選択を有効にすることができます。

## <a name="display-an-emptyview-when-data-is-unavailableemptyviewmd"></a>[データが利用できない場合に、EmptyView を表示します。](emptyview.md)

`CollectionView`、表示できるデータがない場合、ユーザーにフィードバックを提供する空のビューを指定することができます。 空のビューには、文字列、ビュー、または複数のビューができます。

## <a name="scroll-an-item-into-viewscrollingmd"></a>[項目をビューにスクロールします。](scrolling.md)

ユーザー カードのスクロールを開始するには、ときに項目が完全に表示されるように、スクロールの終了位置を制御できます。 さらに、 `CollectionView` 2 つ定義する`ScrollTo`メソッドは、プログラムで項目をビューにスクロールします。 ビューに指定したインデックス位置にある項目を指定した項目をスクロール、表示中に、オーバー ロードの 1 つスクロールします。
