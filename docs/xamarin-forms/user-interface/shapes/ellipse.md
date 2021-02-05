---
title: 'Xamarin.Forms 図形: 楕円'
description: Xamarin.Forms楕円クラスを使用して、楕円と円を描画できます。
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/05/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b3130fd4cb054799ca9e0a61d13cacb2629320b7
ms.sourcegitcommit: 06701714021545eb5e932847829b876082194ffc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2021
ms.locfileid: "99585833"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.Forms 図形: 楕円

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Ellipse`クラスはクラスから派生 `Shape` し、楕円と円を描画するために使用できます。 クラスから継承されるプロパティの詳細につい `Ellipse` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

クラスは、 `Ellipse` クラスから継承されたプロパティをに設定し `Aspect` `Shape` `Stretch.Fill` ます。 プロパティの詳細については `Aspect` 、「 [Stretch shapes](index.md#stretch-shapes)」を参照してください。

## <a name="create-an-ellipse"></a>楕円を作成する

楕円を描画するには、 `Ellipse` オブジェクトを作成し、その `WidthRequest` プロパティとプロパティを設定し `HeightRequest` ます。 楕円の内部を描画するには、その `Fill` プロパティをから派生したオブジェクトに設定し [`Brush`](xref:Xamarin.Forms.Brush) ます。 楕円に輪郭を付けるには、その `Stroke` プロパティをから派生したオブジェクトに設定し [`Brush`](xref:Xamarin.Forms.Brush) ます。 `StrokeThickness` プロパティは、楕円の輪郭の太さを指定します。 オブジェクトの詳細について `Brush` は、「 [ Xamarin.Forms ブラシ](~/xamarin-forms/user-interface/brushes/index.md)」を参照してください。

円を描画するには、 `WidthRequest` `HeightRequest` オブジェクトのプロパティとプロパティを同じにし `Ellipse` ます。

次の XAML の例は、塗りつぶされた楕円を描画する方法を示しています。

```xaml
<Ellipse Fill="Red"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

この例では、ディメンション 150x50 (デバイスに依存しない単位) の赤い塗りつぶされた楕円が描画されます。

![塗りつぶし楕円](ellipse-images/filled.png "塗りつぶし楕円")

次の XAML の例は、円を描画する方法を示しています。

```xaml
<Ellipse Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="150"
         HorizontalOptions="Start" />
```

この例では、150x150 正方形 (デバイスに依存しない単位) という寸法を含む赤い円が描画されます。

![塗りつぶさない円](ellipse-images/circle.png "Circle")

破線の楕円を描画する方法の詳細については、「 [破線の図形を描画](index.md#draw-dashed-shapes)する」を参照してください。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms 図形](index.md)
- [Xamarin.Forms ブラシ](~/xamarin-forms/user-interface/brushes/index.md)
