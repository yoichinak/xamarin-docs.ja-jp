---
title: Xamarin.Forms CarouselView
description: CarouselView は、スクロール可能なレイアウトでデータを表示するためのビューであり、ユーザーはスワイプして項目のコレクション内を移動できます。
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 958a7c292ba636368a016894e98fe8aaff0d0f60
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940565"
---
# <a name="no-locxamarinforms-carouselview"></a>Xamarin.Forms CarouselView

## <a name="introduction"></a>[はじめに](introduction.md)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、スクロール可能なレイアウトでデータを表示するためのビューです。ユーザーは、スワイプして項目のコレクション内を移動できます。

## <a name="data"></a>[データ](populate-data.md)

には、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) プロパティをを実装する任意のコレクションに設定することにより、データが設定され `IEnumerable` ます。 各項目の外観は、プロパティをに設定することによって定義でき [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。

## <a name="layout"></a>[レイアウト](layout.md)

既定では、の [`CarouselView`](xref:Xamarin.Forms.CarouselView) 項目が横方向の一覧に表示されます。 ただし、垂直方向を含む CollectionView と同じレイアウトにもアクセスできます。

## <a name="interaction"></a>[相互作用](interaction.md)

で現在表示されている項目には [`CarouselView`](xref:Xamarin.Forms.CarouselView) 、プロパティおよびプロパティを使用してアクセスでき `CurrentItem` `Position` ます。

## <a name="empty-views"></a>[空のビュー](emptyview.md)

で [`CarouselView`](xref:Xamarin.Forms.CarouselView) は、表示可能なデータがない場合にユーザーにフィードバックを提供する空のビューを指定できます。 空のビューには、文字列、ビュー、または複数のビューを指定できます。

## <a name="scrolling"></a>[スクロール](scrolling.md)

ユーザーがスクロールを開始しようとしたときに、項目が完全に表示されるように、スクロールの終了位置を制御できます。 さらに、では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 項目をプログラムによってビューにスクロールする2つのメソッドが定義されています。 オーバーロードの1つは、指定されたインデックス位置にある項目をビューにスクロールし、もう一方は指定された項目をビューにスクロールします。
