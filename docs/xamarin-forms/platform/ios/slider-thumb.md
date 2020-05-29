---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 93b4dba3e8543bd2cc2a4f2187f617aae5daff77
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137073"
---
# <a name="slider-thumb-tap-on-ios"></a>IOS でのスライダーの Thumb タップ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の機能を使用すると、 [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) [`Slider`](xref:Xamarin.Forms.Slider) つまみをドラッグするのではなく、バー上の位置をタップしてプロパティを設定でき `Slider` ます。 これは、バインド可能なプロパティをに設定することによって XAML で使用され [`Slider.UpdateOnTap`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty) `true` ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Slider ... ios:Slider.UpdateOnTap="true" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var slider = new Xamarin.Forms.Slider();
slider.On<iOS>().SetUpdateOnTap(true);
```

メソッドは、 `Slider.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [ `Slider.SetUpdateOnTap` ] (Xref: Xamarin.FormsPlatformConfiguration. iOSSpecific。 SetUpdateOnTap ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。Slider}, system.string) メソッドを使用して、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) バーの tap が `Slider` プロパティを設定するかどうかを制御します。 [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) また、[ `Slider.GetUpdateOnTap` ] (xref: Xamarin.FormsPlatformConfiguration. iOSSpecific の. Slider. GetUpdateOnTap ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。Slider})) メソッドを使用して、バーのタップによってプロパティが設定されるかどうかを返すことができ `Slider` `Slider.Value` ます。

結果として、バーのタップによって [`Slider`](xref:Xamarin.Forms.Slider) つまみが移動し、プロパティが設定され `Slider` [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) ます。

![](slider-thumb-images/slider-updateontap.png "Slider Update on Tap enabled")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
