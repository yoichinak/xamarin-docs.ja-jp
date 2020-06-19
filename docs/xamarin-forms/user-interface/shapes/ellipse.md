---
title: 'Xamarin.Forms図形: 楕円'
description: Xamarin.Forms楕円クラスを使用して、楕円と円を描画できます。
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 719713368971b0b278cd4a4a9721e863a17ae16d
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990792"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.Forms図形: 楕円

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Ellipse`クラスはクラスから派生 `Shape` し、楕円と円を描画するために使用できます。 クラスから継承されるプロパティの詳細につい `Ellipse` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

クラスは、 `Ellipse` クラスから継承されたプロパティをに設定し `Aspect` `Shape` `Stretch.Fill` ます。

## <a name="create-an-ellipse"></a>楕円を作成する

次の XAML の例は、塗りつぶされた楕円を描画する方法を示しています。

```xaml
<Ellipse Fill="Red"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

次の XAML の例は、円を描画する方法を示しています。

```xaml
<Ellipse Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="150"
         HorizontalOptions="Start" />
```

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形](index.md)
