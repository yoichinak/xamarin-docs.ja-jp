---
title: Xamarin.Formsブラシ
description: Xamarin.FormsBrush クラスは、出力を使用して領域を塗りつぶす抽象クラスです。
ms.prod: xamarin
ms.assetid: 44420FC2-304C-4175-8654-76769F79A813
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4f1f56103b20eac84ce6106c0955acebf974cfe3
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919108"
---
# <a name="no-locxamarinforms-brushes"></a>Xamarin.Formsブラシ

![プレビュー API](~/media/shared/preview.png "この API は現在プレリリースです")

ブラシを使用すると、さまざまな方法を使用して、コントロールの背景などの領域を塗りつぶすことができます。 でのブラシのサポート Xamarin.Forms は、 `Xamarin.Forms` IOS、Android、macOS、ユニバーサル WINDOWS プラットフォーム (UWP)、および WINDOWS PRESENTATION FOUNDATION (WPF) の名前空間で利用できます。

> [!IMPORTANT]
> でのブラシのサポート Xamarin.Forms は現在試験段階であり、フラグを設定することによってのみ使用でき `Brush_Experimental` ます。 詳細については、「試験的な[フラグ](~/xamarin-forms/internals/experimental-flags.md)」を参照してください。

クラスは、 `Brush` 出力を使用して領域を塗りつぶす抽象クラスです。 から派生するクラスは `Brush` 、領域を塗りつぶすさまざまな方法を記述します。 で使用できるさまざまなブラシの種類を次の一覧に示し Xamarin.Forms ます。

- `SolidColorBrush`。純色で領域を塗りつぶします。 詳細については、「 [ Xamarin.Forms ブラシ: 純色](solidcolor.md)」を参照してください。
- `LinearGradientBrush`。線状グラデーションで領域を塗りつぶします。 詳細については、「 [ Xamarin.Forms ブラシ: 線状グラデーション](lineargradient.md)」を参照してください。
- `RadialGradientBrush`。放射状グラデーションで領域を塗りつぶします。 詳細については、「 [ Xamarin.Forms ブラシ: 放射状グラデーション](radialgradient.md)」を参照してください。

これらのブラシの種類のインスタンスは、の `Stroke` `Fill` プロパティとプロパティ、 `Shape` および `Background` のプロパティに割り当てることができ [`VisualElement`](xref:Xamarin.Forms.VisualElement) ます。

> [!NOTE]
> `VisualElement.Background`プロパティを使用すると、ブラシをコントロールの背景として使用できます。

クラスには、ブラシが定義されている `Brush` `IsNullOrEmpty` `bool` かどうかを表すを返すメソッドもあります。
