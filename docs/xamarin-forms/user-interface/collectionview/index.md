---
title: Xamarin.Forms CollectionView
description: CollectionView は、異なるレイアウト仕様を使ってデータのリストを表示するための柔軟で高パフォーマンスなビューです。
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: a6cb6e695a4c19627b183060a7636320f8083ee2
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65048138"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

![](~/media/shared/preview.png "この API は、現在プレリリースです")

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

`CollectionView`は別のレイアウトの仕様を使用してデータのリストを表示するための柔軟性とパフォーマンスの高いビューです。

## <a name="datapopulate-datamd"></a>[データ](populate-data.md)

`CollectionView` は、`ItemsSource` プロパティに `IEnumerable` を実装した任意のコレクションを設定することで、データが挿入されます。 リストの各項目の外観は、`ItemTemplate` プロパティに[ `DataTemplate`](xref:Xamarin.Forms.DataTemplate) を設定することで定義することができます。

## <a name="layoutlayoutmd"></a>[レイアウト](layout.md)

既定で、 `CollectionView` は、垂直方向のリストでその項目を表示しますが、 垂直および水平方向のリストとグリッドを指定することができます。

## <a name="selectionselectionmd"></a>[選択内容](selection.md)

既定では、`CollectionView` の選択は無効になっていますが、 単一または複数選択を有効にすることができます。

## <a name="empty-viewsemptyviewmd"></a>[空のビュー](emptyview.md)

`CollectionView` では、表示できるデータがない場合、ユーザーにフィードバックを提供するための `EmptyView` を指定することができます。 `EmptyView` には、文字列、ビュー、または複数のビューが使用できます。

## <a name="scrollingscrollingmd"></a>[スクロール](scrolling.md)

ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 さらに、 `CollectionView` には、2 つの `ScrollTo` メソッドが定義されており、プログラムでビューの項目にスクロールできます。 オーバーロードの 1 つは、ビュー内の指定したインデックスにある項目にスクロールし、もう一つは、ビュー内の指定した項目にスクロールします。
