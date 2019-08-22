---
title: CollectionView
description: CollectionView は、異なるレイアウト仕様を使ってデータのリストを表示するための柔軟で高パフォーマンスなビューです。
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: b3841ed1287c980212ce37078f38f4984393c414
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888593"
---
# <a name="xamarinforms-collectionview"></a>CollectionView

![](~/media/shared/preview.png "この API は、現在プレリリースです")

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

は[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、さまざまなレイアウト仕様を使用してデータの一覧を表示するための、柔軟でパフォーマンスの高いビューです。

## <a name="datapopulate-datamd"></a>[データ](populate-data.md)

に[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティをを実装`IEnumerable`する任意のコレクションに設定することにより、データが設定されます。 リスト内の各項目の外観は、 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)をに設定することによって定義できます。

## <a name="layoutlayoutmd"></a>[レイアウト](layout.md)

既定では、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)の項目が縦の一覧に表示されます。 垂直および水平方向のリストとグリッドを指定することができます。

## <a name="selectionselectionmd"></a>[選択内容](selection.md)

既定では[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、選択は無効になっています。 単一または複数選択を有効にすることができます。

## <a name="empty-viewsemptyviewmd"></a>[空のビュー](emptyview.md)

で[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、表示可能なデータがない場合にユーザーにフィードバックを提供する空のビューを指定できます。 `EmptyView` には、文字列、ビュー、または複数のビューが使用できます。

## <a name="scrollingscrollingmd"></a>[スクロール](scrolling.md)

ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 さらに、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)では[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 、項目をプログラムによってビューにスクロールする2つのメソッドが定義されています。 オーバーロードの 1 つは、ビュー内の指定したインデックスにある項目にスクロールし、もう一つは、ビュー内の指定した項目にスクロールします。

## <a name="groupinggroupingmd"></a>[グループ化](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、プロパティをに設定する`IsGrouped` `true`ことによって、正しくグループ化されたデータを表示できます。
