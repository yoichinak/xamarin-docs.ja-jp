---
title: IOS でスライダーつまみをタップします
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、Slider.Value プロパティをスライダー バーをタップして設定できるようにするプラットフォームに固有の iOS を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: D0915D37-9A59-4728-BB6A-FE094A661275
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: b195a277defa04bac88ad65b928957c6efff4601
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65925338"
---
# <a name="slider-thumb-tap-on-ios"></a>IOS でスライダーつまみをタップします

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

この iOS プラットフォームに固有の有効、 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value)の位置をタップして設定されるプロパティを[ `Slider` ](xref:Xamarin.Forms.Slider)をドラッグすることではなく、横棒グラフ、`Slider`つまみ。 XAML で設定して使用される、 [ `Slider.UpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty)バインド可能なプロパティを`true`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Slider ... ios:Slider.UpdateOnTap="true" />
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var slider = new Xamarin.Forms.Slider();
slider.On<iOS>().SetUpdateOnTap(true);
```

`Slider.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  [ `Slider.SetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.SetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)でタップするかどうか、名前空間を使用して、`Slider`バーは設定、 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value)プロパティ。 さらに、 [ `Slider.GetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.GetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider}))を返すメソッドを使用できるかどうかをタップ、`Slider`バーは設定、`Slider.Value`プロパティ。

その結果でタップ、 [ `Slider` ](xref:Xamarin.Forms.Slider)バーが移動することができます、 `Slider` thumb し、設定、 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value)プロパティ。

![](slider-thumb-images/slider-updateontap.png "有効になっている Tap にスライダーの更新プログラム")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
