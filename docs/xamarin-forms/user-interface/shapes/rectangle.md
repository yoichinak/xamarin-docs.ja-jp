---
title: 'Xamarin.Forms図形: 四角形'
description: Xamarin.Forms四角形クラスは、四角形の描画に使用できます。
ms.prod: xamarin
ms.assetid: 2DD663D3-DAEC-495C-AB6D-8A143FC97637
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a0d528948ae8a57fc7c702d8a4ce3d3ec2eda15a
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990829"
---
# <a name="xamarinforms-shapes-rectangle"></a>Xamarin.Forms図形: 四角形

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Rectangle`クラスはクラスから派生 `Shape` し、四角形の描画に使用できます。 クラスから継承されるプロパティの詳細につい `Rectangle` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

`Rectangle` は次の特性を定義します。

- `RadiusX`型の。 `double` これは、四角形の角を丸めるために使用される x 軸半径です。 このプロパティの既定値は0.0 です。
- `RadiusY`型の。 `double` これは、四角形の角を丸めるために使用される y 軸半径です。 このプロパティの既定値は0.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

クラスは、 `Rectangle` クラスから継承されたプロパティをに設定し `Aspect` `Shape` `Stretch.Fill` ます。

## <a name="create-a-rectangle"></a>四角形を作成する

次の XAML の例は、角が丸い塗りつぶし四角形を描画する方法を示しています。

```xaml
<Rectangle Fill="DarkBlue"
           Stroke="Red"
           StrokeThickness="4"
           RadiusX="12"
           RadiusY="24"           
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形](index.md)
