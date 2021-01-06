---
title: Xamarin.Forms CarouselView の概要
description: CarouselView は、スクロール可能なレイアウトでデータを表示するためのビューであり、ユーザーはスワイプして項目のコレクション内を移動できます。
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2aa33b0bd2e11d854f4f4dcfe03258a621301395
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97939027"
---
# <a name="no-locxamarinforms-carouselview-introduction"></a>Xamarin.Forms CarouselView の概要

[`CarouselView`](xref:Xamarin.Forms.CarouselView) は、スクロール可能なレイアウトでデータを表示するためのビューであり、ユーザーはスワイプして項目のコレクション内を移動できます。 既定では、の `CarouselView` 項目は水平方向に表示されます。 1つの項目が画面に表示されます。スワイプジェスチャを使用すると、項目のコレクションの前後に移動します。 さらに、の各項目を表すインジケーターを表示でき `CarouselView` ます。

[![IOS と Android での CarouselView と IndicatorView のスクリーンショット](populate-data-images/indicators.png "IndicatorView の円")](populate-data-images/indicators-large.png#lightbox "IndicatorView の円")

既定では、は、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 項目のコレクションへのループアクセスを提供します。 したがって、コレクション内の最初の項目から後ろにスワイプすると、コレクションの最後の項目が表示されます。 同様に、コレクション内の最後の項目からのスワイプ転送は、コレクションの最初の項目に戻ります。

[`CarouselView`](xref:Xamarin.Forms.CarouselView) は、の実装の多くをと共有 [`CollectionView`](xref:Xamarin.Forms.CollectionView) します。 ただし、2つのコントロールのユースケースは異なります。 `CollectionView` は、通常、任意の長さのデータの一覧を表示するために使用されます。一方、は、制限された `CarouselView` 長さの一覧の情報を強調表示するために使用されます。
