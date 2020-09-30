---
title: IOS でのスライダーの Thumb タップ
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、スライダーバーをタップして Slider. Value プロパティを設定できるようにする iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: D0915D37-9A59-4728-BB6A-FE094A661275
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c3e74d52eee5da11dffc3c8a778fa2f691f1ca02
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91560937"
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

![タップが有効になっているスライダーの更新](slider-thumb-images/slider-updateontap.png)

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)