---
title: CarouselView の概要
description: CarouselView は、スクロール可能なレイアウトでデータを表示するためのビューであり、ユーザーはスワイプして項目のコレクション内を移動できます。
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
ms.openlocfilehash: 2fe4d984f36880493a9a04d99b63876551366477
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696970"
---
# <a name="xamarinforms-carouselview-introduction"></a>CarouselView の概要

![](~/media/shared/preview.png "This API is currently pre-release")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、スクロール可能なレイアウトでデータを表示するためのビューであり、ユーザーはスワイプして項目のコレクション内を移動できます。 既定では、`CarouselView` の項目は水平方向に表示されます。 1つの項目が画面に表示されます。スワイプジェスチャを使用すると、項目のコレクションの前後に移動します。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、Xamarin. Forms 4.3 で使用できます。 ただし、現在は実験的で、`Forms.Init` を呼び出す前に、次のコード行を iOS の `AppDelegate` クラス、または Android の `MainActivity` クラスに追加することによってのみ使用できます。

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)は IOS と Android で使用できますが、一部の機能はユニバーサル Windows プラットフォームでのみ使用できます。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、その実装の多くを[`CollectionView`](xref:Xamarin.Forms.CollectionView)と共有します。 ただし、2つのコントロールのユースケースは異なります。 `CollectionView` は通常、任意の長さのデータの一覧を表示するために使用されます。一方、`CarouselView` は通常、制限された長さの一覧の情報を強調表示するために使用されます。
