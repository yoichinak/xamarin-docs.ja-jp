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
ms.openlocfilehash: 725f892a667b4e89d55266abcf69e5394b09b6a5
ms.sourcegitcommit: ef3d4a70e70927c4f231b763842c5355f1571d15
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243827"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.Forms図形: 楕円

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Ellipse`クラスはクラスから派生 `Shape` し、楕円と円を描画するために使用できます。 クラスから継承されるプロパティの詳細につい `Ellipse` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

クラスは、 `Ellipse` クラスから継承されたプロパティをに設定し `Aspect` `Shape` `Stretch.Fill` ます。 プロパティの詳細については `Aspect` 、「 [Stretch shapes](index.md#stretch-shapes)」を参照してください。

## <a name="create-an-ellipse"></a>楕円を作成する

楕円を描画するには、 `Ellipse` オブジェクトを作成し、その `WidthRequest` プロパティとプロパティを設定し `HeightRequest` ます。 楕円の内部を描画するには、そのプロパティをに設定 `Fill` [`Color`](xref:Xamarin.Forms.Color) します。 楕円に輪郭を付けるには、その `Stroke` プロパティをに設定し [`Color`](xref:Xamarin.Forms.Color) ます。 `StrokeThickness` プロパティは、楕円の輪郭の太さを指定します。

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

破線の楕円を描画する方法の詳細については、「[破線の図形を描画](index.md#draw-dashed-shapes)する」を参照してください。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形](index.md)