---
title: CollectionView
description: CollectionView は、さまざまなレイアウト仕様を使用してデータの一覧を表示するための、柔軟でパフォーマンスの高いビューです。
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: 8050d952556ce0b55a7ce72bc5f25de903fee6e5
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696997"
---
# <a name="xamarinforms-collectionview"></a>CollectionView

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

[@No__t_1](xref:Xamarin.Forms.CollectionView)は、さまざまなレイアウト仕様を使用してデータの一覧を表示するための、柔軟でパフォーマンスの高いビューです。

## <a name="datapopulate-datamd"></a>[データ](populate-data.md)

[@No__t_1](xref:Xamarin.Forms.CollectionView)には、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティを `IEnumerable` を実装する任意のコレクションに設定することによってデータが設定されます。 リスト内の各項目の外観を定義するには、[ [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) ] プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定します。

## <a name="layoutlayoutmd"></a>[レイアウト](layout.md)

既定では、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)の項目は縦の一覧に表示されます。 ただし、縦と横のリストおよびグリッドを指定することもできます。

## <a name="selectionselectionmd"></a>[選択内容](selection.md)

既定では[`CollectionView`](xref:Xamarin.Forms.CollectionView)選択は無効になっています。 ただし、1つまたは複数の選択を有効にすることができます。

## <a name="empty-viewsemptyviewmd"></a>[空のビュー](emptyview.md)

[@No__t_1](xref:Xamarin.Forms.CollectionView)では、表示可能なデータがない場合にユーザーにフィードバックを提供する空のビューを指定できます。 空のビューには、文字列、ビュー、または複数のビューを指定できます。

## <a name="scrollingscrollingmd"></a>[スクロール](scrolling.md)

ユーザーがスクロールを開始しようとしたときに、項目が完全に表示されるように、スクロールの終了位置を制御できます。 また、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)では、項目をプログラムによってビューにスクロールする2つの[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドが定義されています。 オーバーロードの1つは、指定されたインデックス位置にある項目をビューにスクロールし、もう一方は指定された項目をビューにスクロールします。

## <a name="groupinggroupingmd"></a>[グループ化](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) `IsGrouped` プロパティを `true` に設定することによって、正しくグループ化されたデータを表示できます。
