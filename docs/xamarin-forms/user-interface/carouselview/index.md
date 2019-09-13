---
title: CarouselView
description: CarouselView は、カルーセルに似たレイアウトでデータの一覧を表示するためのビューです。
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 06d20341053c4f77cb72aac9e1abdc85d9f95fdb
ms.sourcegitcommit: e83035c746f165ee6d03f2e9fd0066ee4f20a9fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/11/2019
ms.locfileid: "70908332"
---
# <a name="xamarinforms-carouselview"></a>CarouselView

![](~/media/shared/preview.png "この API は、現在プレリリースです")

> [!IMPORTANT]
> CarouselView のドキュメントは開発中であり、一部のリンクは CollectionView のドキュメントを参照している場合があります。 ほとんどの場合、CollectionView に基づく CarouselView の性質により、情報が適用されます。

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

は[`CarouselView`](xref:Xamarin.Forms.CarouselView) 、カルーセルに似たレイアウトでデータを表示するためのビューです。 その実装は、の[`CollectionView`](xref:Xamarin.Forms.CollectionView)実装に基づいています。

## <a name="datacollectionviewpopulate-datamd"></a>[データ](../collectionview/populate-data.md)

に[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティをを実装`IEnumerable`する任意のコレクションに設定することにより、データが設定されます。 リスト内の各項目の外観は、 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)をに設定することによって定義できます。

## <a name="layoutlayoutmd"></a>[レイアウト](layout.md)

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)の項目が横方向の一覧に表示されます。 ただし、垂直方向を含む CollectionView と同じレイアウトにもアクセスできます。

## <a name="empty-viewscollectionviewemptyviewmd"></a>[空のビュー](../collectionview/emptyview.md)

で[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、表示可能なデータがない場合にユーザーにフィードバックを提供する空のビューを指定できます。 `EmptyView` には、文字列、ビュー、または複数のビューが使用できます。

## <a name="scrollingcollectionviewscrollingmd"></a>[スクロール](../collectionview/scrolling.md)

ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 さらに、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)では[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 、項目をプログラムによってビューにスクロールする2つのメソッドが定義されています。 オーバーロードの 1 つは、ビュー内の指定したインデックスにある項目にスクロールし、もう一つは、ビュー内の指定した項目にスクロールします。
