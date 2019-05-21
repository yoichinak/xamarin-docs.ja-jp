---
title: Xamarin.Forms CollectionView
description: CollectionView は、異なるレイアウト仕様を使ってデータのリストを表示するための柔軟で高パフォーマンスなビューです。
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 73e68f29c61661019cfc56f8caa26c6a0b1ce4ba
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971010"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

[ `CollectionView` ](xref:Xamarin.Forms.CollectionView)は別のレイアウトの仕様を使用してデータのリストを表示するための柔軟性とパフォーマンスの高いビューです。

## <a name="datapopulate-datamd"></a>[データ](populate-data.md)

A [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)設定によってデータが読み込まれて、 [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティを実装するコレクションを`IEnumerable`します。 設定して、リストの各項目の外観を定義することができます、 [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティを[ `DataTemplate`](xref:Xamarin.Forms.DataTemplate)します。

## <a name="layoutlayoutmd"></a>[レイアウト](layout.md)

既定で、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)を垂直方向に一覧の項目が表示されます。 垂直および水平方向のリストとグリッドを指定することができます。

## <a name="selectionselectionmd"></a>[選択内容](selection.md)

既定では、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)選択が無効になっています。 単一または複数選択を有効にすることができます。

## <a name="empty-viewsemptyviewmd"></a>[空のビュー](emptyview.md)

[ `CollectionView` ](xref:Xamarin.Forms.CollectionView)、表示できるデータがない場合、ユーザーにフィードバックを提供する空のビューを指定することができます。 `EmptyView` には、文字列、ビュー、または複数のビューが使用できます。

## <a name="scrollingscrollingmd"></a>[スクロール](scrolling.md)

ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 さらに、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 2 つ定義する[ `ScrollTo` ](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドは、プログラムで項目をビューにスクロールします。 オーバーロードの 1 つは、ビュー内の指定したインデックスにある項目にスクロールし、もう一つは、ビュー内の指定した項目にスクロールします。
