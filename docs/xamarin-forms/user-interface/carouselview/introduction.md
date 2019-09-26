---
title: CarouselView の概要
description: CarouselView は、カルーセルに似たレイアウトでデータを表示するためのビューです。
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 7979f6ed362c580d9cf80f19b3bc0ea7550ca70c
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "70985971"
---
# <a name="xamarinforms-carouselview-introduction"></a>CarouselView の概要

![](~/media/shared/preview.png "この API は現在プレリリースです")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、カルーセルに似た方法で情報を提示するためのビューです。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、Xamarin. Forms 4.3 で使用できます。 ただし、現在試験段階で、`Forms.Init` を呼ぶ前に、Android では `MainActivity` クラス、iOS では `AppDelegate` クラスに以下の1行を加えることによってのみ使用できます:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

_注:このドキュメントの作成時点では、CarouselView は引き続き CollectionView フラグを使用して機能を有効にします。_

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)は iOS と Android で利用できますが、一部の機能はユニバーサル Windows プラットフォームでのみ使用できます。

## <a name="when-to-use-carouselview"></a>CarouselView を使用する場合

CarouselView は、実装の大部分を CollectionView からのものではありませんが、その目的はより重視されています。 CollectionView と CarouselView は自由に使用できますが、手荷物のアプリは通常、情報を強調表示するために使用され、使用される項目の合計数に制限されます。
