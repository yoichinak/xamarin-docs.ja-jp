---
title: CarouselView の概要
description: CarouselView は、スクロール可能なレイアウトでデータを表示するためのビューであり、ユーザーはスワイプして項目のコレクション内を移動できます。
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
ms.openlocfilehash: 2c3e15ce68ad1507318a1d8155f9ab03095ea409
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635581"
---
# <a name="xamarinforms-carouselview-introduction"></a>CarouselView の概要

![](~/media/shared/preview.png "This API is currently pre-release")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、スクロール可能なレイアウトでデータを表示するためのビューであり、ユーザーはスワイプして項目のコレクション内を移動できます。 既定では、`CarouselView` の項目は水平方向に表示されます。 1つの項目が画面に表示されます。スワイプジェスチャを使用すると、項目のコレクションの前後に移動します。 さらに、`CarouselView`内の各項目を表すインジケーターを表示できます。

[![IOS と Android での CarouselView と IndicatorView のスクリーンショット](populate-data-images/indicators.png "IndicatorView の円")](populate-data-images/indicators-large.png#lightbox "IndicatorView の円")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、Xamarin. Forms 4.3 で使用できます。 ただし、現在は実験的で、`Forms.Init`を呼び出す前に、次のコード行を iOS の `AppDelegate` クラス、または Android の `MainActivity` クラスに追加することによってのみ使用できます。

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)は IOS と Android で使用できますが、一部の機能はユニバーサル Windows プラットフォームでのみ使用できます。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、その実装の多くを[`CollectionView`](xref:Xamarin.Forms.CollectionView)と共有します。 ただし、2つのコントロールのユースケースは異なります。 `CollectionView` は通常、任意の長さのデータの一覧を表示するために使用されます。一方、`CarouselView` は通常、制限された長さの一覧の情報を強調表示するために使用されます。
