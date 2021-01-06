---
title: Xamarin.Forms ブラシ
description: Xamarin.FormsBrush クラスは、出力を使用して領域を塗りつぶす抽象クラスです。
ms.prod: xamarin
ms.assetid: 44420FC2-304C-4175-8654-76769F79A813
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e0bb8b5e1668e683fa8b15f752b77d865cb317b5
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940396"
---
# <a name="no-locxamarinforms-brushes"></a>Xamarin.Forms ブラシ

ブラシを使用すると、さまざまな方法を使用して、コントロールの背景などの領域を塗りつぶすことができます。 でのブラシのサポート Xamarin.Forms は、 `Xamarin.Forms` IOS、Android、macOS、ユニバーサル WINDOWS プラットフォーム (UWP)、および WINDOWS PRESENTATION FOUNDATION (WPF) の名前空間で利用できます。

クラスは、 `Brush` 出力を使用して領域を塗りつぶす抽象クラスです。 から派生するクラスは `Brush` 、領域を塗りつぶすさまざまな方法を記述します。 で使用できるさまざまなブラシの種類を次の一覧に示し Xamarin.Forms ます。

- `SolidColorBrush`。純色で領域を塗りつぶします。 詳細については、「 [ Xamarin.Forms ブラシ: 純色](solidcolor.md)」を参照してください。
- `LinearGradientBrush`。線状グラデーションで領域を塗りつぶします。 詳細については、「 [ Xamarin.Forms ブラシ: 線状グラデーション](lineargradient.md)」を参照してください。
- `RadialGradientBrush`。放射状グラデーションで領域を塗りつぶします。 詳細については、「 [ Xamarin.Forms ブラシ: 放射状グラデーション](radialgradient.md)」を参照してください。

これらのブラシの種類のインスタンスは、の `Stroke` `Fill` プロパティとプロパティ、 `Shape` および `Background` のプロパティに割り当てることができ [`VisualElement`](xref:Xamarin.Forms.VisualElement) ます。

> [!NOTE]
> `VisualElement.Background`プロパティを使用すると、ブラシをコントロールの背景として使用できます。

クラスには、ブラシが定義されている `Brush` `IsNullOrEmpty` `bool` かどうかを表すを返すメソッドもあります。
