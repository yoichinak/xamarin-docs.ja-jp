---
title: 'Xamarin.Forms図形: 四角形'
description: Xamarin.Forms四角形クラスは、四角形の描画に使用できます。
ms.prod: xamarin
ms.assetid: 2DD663D3-DAEC-495C-AB6D-8A143FC97637
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 91960c08640a412e02298ed051469a86c1a9f368
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930573"
---
# <a name="xamarinforms-shapes-rectangle"></a>Xamarin.Forms図形: 四角形

![プレリリース API](~/media/shared/preview.png "この API は現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Rectangle`クラスはクラスから派生 `Shape` し、四角形と正方形を描画するために使用できます。 クラスから継承されるプロパティの詳細につい `Rectangle` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

`Rectangle` は次の特性を定義します。

- `RadiusX`型の。 `double` これは、四角形の角を丸めるために使用される x 軸半径です。 このプロパティの既定値は0.0 です。
- `RadiusY`型の。 `double` これは、四角形の角を丸めるために使用される y 軸半径です。 このプロパティの既定値は0.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

クラスは、 `Rectangle` クラスから継承されたプロパティをに設定し `Aspect` `Shape` `Stretch.Fill` ます。 プロパティの詳細については `Aspect` 、「 [Stretch shapes](index.md#stretch-shapes)」を参照してください。

## <a name="create-a-rectangle"></a>四角形を作成する

四角形を描画するには、 `Rectangle` オブジェクトを作成し、その `WidthRequest` プロパティとプロパティを設定し `HeightRequest` ます。 四角形の内部を描画するには、そのプロパティをに設定 `Fill` [`Color`](xref:Xamarin.Forms.Color) します。 四角形に輪郭を付けるには、その `Stroke` プロパティをに設定し [`Color`](xref:Xamarin.Forms.Color) ます。 プロパティは、 `StrokeThickness` 四角形の輪郭の太さを指定します。

四角形の角を丸くするには、 `RadiusX` プロパティとプロパティを設定し `RadiusY` ます。 これらのプロパティは、四角形の角を丸めるために使用される x 軸と y 軸の半径を設定します。

正方形を描画するには、 `WidthRequest` `HeightRequest` オブジェクトのプロパティとプロパティを同じにし `Rectangle` ます。

次の XAML の例は、塗りつぶされた四角形を描画する方法を示しています。

```xaml
<Rectangle Fill="Red"
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

この例では、ディメンション 150x50 (デバイスに依存しない単位) の赤い塗りつぶされた四角形が描画されます。

![塗りつぶされた四角形](rectangle-images/filled.png "塗りつぶされた四角形")

次の XAML の例は、角が丸い塗りつぶし四角形を描画する方法を示しています。

```xaml
<Rectangle Fill="Blue"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10"
           WidthRequest="200"
           HeightRequest="100"
           HorizontalOptions="Start" />
```

この例では、角が丸い青い塗りつぶされた四角形が描画されます。

![角が丸い四角形](rectangle-images/rounded.png "角が丸い四角形")

破線の四角形を描画する方法については、「[破線の図形を描画](index.md#draw-dashed-shapes)する」を参照してください。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形](index.md)
