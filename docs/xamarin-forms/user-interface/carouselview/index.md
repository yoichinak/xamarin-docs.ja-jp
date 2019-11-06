---
title: CarouselView
description: CarouselView は、スクロール可能なレイアウトでデータを表示するためのビューであり、ユーザーはスワイプして項目のコレクション内を移動できます。
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
ms.openlocfilehash: 816c1b6e4ab497d0ada0f80fa3ad4800912587c3
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696988"
---
# <a name="xamarinforms-carouselview"></a>CarouselView

![](~/media/shared/preview.png "This API is currently pre-release")

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

[@No__t_1](xref:Xamarin.Forms.CarouselView)は、スクロール可能なレイアウトでデータを表示するためのビューであり、ユーザーはスワイプして項目のコレクション内を移動できます。

## <a name="datapopulate-datamd"></a>[データ](populate-data.md)

[@No__t_1](xref:Xamarin.Forms.CarouselView)には、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティを `IEnumerable` を実装する任意のコレクションに設定することによってデータが設定されます。 各項目の外観を定義するには、[ [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) ] プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定します。

## <a name="layoutlayoutmd"></a>[レイアウト](layout.md)

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)の項目が横方向の一覧に表示されます。 ただし、垂直方向を含む CollectionView と同じレイアウトにもアクセスできます。

## <a name="interactioninteractionmd"></a>[介入](interaction.md)

[@No__t_1](xref:Xamarin.Forms.CarouselView)に現在表示されているアイテムには、`CurrentItem` プロパティと `Position` プロパティを使用してアクセスできます。

## <a name="empty-viewsemptyviewmd"></a>[空のビュー](emptyview.md)

[@No__t_1](xref:Xamarin.Forms.CarouselView)では、表示可能なデータがない場合にユーザーにフィードバックを提供する空のビューを指定できます。 空のビューには、文字列、ビュー、または複数のビューを指定できます。

## <a name="scrollingscrollingmd"></a>[スクロール](scrolling.md)

ユーザーがスクロールを開始しようとしたときに、項目が完全に表示されるように、スクロールの終了位置を制御できます。 また、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)では、項目をプログラムによってビューにスクロールする2つの[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドが定義されています。 オーバーロードの1つは、指定されたインデックス位置にある項目をビューにスクロールし、もう一方は指定された項目をビューにスクロールします。
