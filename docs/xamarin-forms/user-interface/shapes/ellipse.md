---
title: 'Xamarin.Forms図形: 楕円'
description: Xamarin.Forms楕円クラスを使用して、楕円と円を描画できます。
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 053805c14f195ef0dd3ae8f0cfcac2ee7425271d
ms.sourcegitcommit: dc49ba58510eeb52048a866e5d3daf5f1f68fbd2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/22/2020
ms.locfileid: "85130910"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.Forms図形: 楕円

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

`Ellipse`クラスはクラスから派生 `Shape` し、楕円と円を描画するために使用できます。 クラスから継承されるプロパティの詳細につい `Ellipse` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

クラスは、 `Ellipse` クラスから継承されたプロパティをに設定し `Aspect` `Shape` `Stretch.Fill` ます。 プロパティの詳細については `Aspect` 、「 [Stretch shapes](index.md#stretch-shapes)」を参照してください。

## <a name="create-an-ellipse"></a>楕円を作成する

楕円を描画するには、 `Ellipse` オブジェクトを作成し、その `WidthRequest` プロパティとプロパティを設定し `HeightRequest` ます。 プロパティを使用して `Fill` 、 [`Color`](xref:Xamarin.Forms.Color) 楕円の内部を描画するために使用するを指定します。 `Stroke` プロパティを使用して、楕円の輪郭の描画に使用される `Color` を指定します。 `StrokeThickness` プロパティは、楕円の輪郭の太さを指定します。

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

![付い](ellipse-images/circle.png "Circle")

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.Forms図形](index.md)
