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
- `CollectionView` は [ `ListView`](xref:Xamarin.Forms.ListView) の外観に関する API を減らしています。 `ListView` の多くのプロパティおよびイベントは、 `CollectionView` では存在しません。
- `CollectionView` には組み込みの区切り線は含まれません。

`CollectionView` Xamarin.Forms 4.0 より前のリリースで使用できます。 ただし、現在、試験段階は、し、次のコードの行を追加することでのみ使用できます、`AppDelegate`クラスでは、iOS、または、 `MainActivity` android では、クラスを呼び出す前に`Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!NOTE]
> `CollectionView` は iOS と Android でのみ利用できます。

## <a name="populate-collectionview-with-datapopulate-datamd"></a>[ CollectionView にデータを挿入する ](populate-data.md)

`CollectionView` は、`ItemsSource` プロパティに `IEnumerable` を実装した任意のコレクションを設定することで、データが挿入されます。 リストの各項目の外観は、`ItemTemplate` プロパティに[ `DataTemplate`](xref:Xamarin.Forms.DataTemplate) を設定することで定義することができます。

## <a name="specify-collectionview-layoutlayoutmd"></a>[CollectionView にレイアウトを指定する ](layout.md)

既定で、`CollectionView`を垂直方向に一覧の項目が表示されます。 ただし、垂直および水平方向のリストとグリッドを指定することができます。

## <a name="set-collectionview-selection-modeselectionmd"></a>[CollectionView の選択モードを設定する ](selection.md)

既定では、`CollectionView`選択が無効になっています。 ただし、1 つまたは複数選択を有効にすることができます。

## <a name="display-an-emptyview-when-data-is-unavailableemptyviewmd"></a>[データが利用できない場合に、EmptyView を表示する ](emptyview.md)

`CollectionView`、表示できるデータがない場合、ユーザーにフィードバックを提供する空のビューを指定することができます。 空のビューには、文字列、ビュー、または複数のビューができます。

## <a name="scroll-an-item-into-viewscrollingmd"></a>[項目をビューにスクロールします。](scrolling.md)

ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 さらに、 `CollectionView` には、2 つの `ScrollTo` メソッドが定義されており、プログラムでビューの項目にスクロールできます。 オーバーロードの 1 つは、ビュー内の指定したインデックスにある項目にスクロールし、もう一つは、ビュー内の指定した項目にスクロールします。
