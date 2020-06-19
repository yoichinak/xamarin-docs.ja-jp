---
title: Xamarin.FormsCarouselView の概要
description: CarouselView は、スクロール可能なレイアウトでデータを表示するためのビューであり、ユーザーはスワイプして項目のコレクション内を移動できます。
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2e67acd0188e1147481005502ad9ccdaada645d9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140271"
---
# <a name="xamarinforms-carouselview-introduction"></a>Xamarin.FormsCarouselView の概要

![](~/media/shared/preview.png "This API is currently pre-release")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、スクロール可能なレイアウトでデータを表示するためのビューであり、ユーザーはスワイプして項目のコレクション内を移動できます。 既定では、の `CarouselView` 項目は水平方向に表示されます。 1つの項目が画面に表示されます。スワイプジェスチャを使用すると、項目のコレクションの前後に移動します。 さらに、の各項目を表すインジケーターを表示でき `CarouselView` ます。

[![IOS と Android での CarouselView と IndicatorView のスクリーンショット](populate-data-images/indicators.png "IndicatorView の円")](populate-data-images/indicators-large.png#lightbox "IndicatorView の円")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は4.3 で使用でき Xamarin.Forms ます。 ただし、現在は実験的であり、を呼び出す前に、次のコード行を `AppDelegate` iOS のクラスまたは Android のクラスに追加することによってのみ使用でき `MainActivity` `Forms.Init` ます。

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)は iOS と Android で利用できますが、一部の機能はユニバーサル Windows プラットフォームでのみ使用できます。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、の実装の多くをと共有 [`CollectionView`](xref:Xamarin.Forms.CollectionView) します。 ただし、2つのコントロールのユースケースは異なります。 `CollectionView`は、通常、任意の長さのデータの一覧を表示するために使用されます。一方、は、制限された `CarouselView` 長さの一覧の情報を強調表示するために使用されます。
