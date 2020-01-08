---
title: IOS の SwipeView スワイプ切り替えモード
description: Platform-specifics は custom renderers や effects を実装することなく、特定のプラットフォーム上でのみ利用できる機能の使用を可能にします。 この記事では、SwipeView を開くときに使用される遷移を制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: C667F24C-BAD8-47E0-9285-D3546BEF703B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: d5ba92d008cf3431bce2c197aca45c894eb3d5c7
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490452"
---
# <a name="swipeview-swipe-transition-mode-on-ios"></a>IOS の SwipeView スワイプ切り替えモード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の設定は、`SwipeView`を開くときに使用される遷移を制御します。 XAML では、`SwipeView.SwipeTransitionMode` バインド可能なプロパティを `SwipeTransitionMode` 列挙値に設定することによって使用されます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SwipeView ios:SwipeView.SwipeTransitionMode="Drag">
            <SwipeView.LeftItems>
                <SwipeItems>
                    <SwipeItem Text="Delete"
                               IconImageSource="delete.png"
                               BackgroundColor="LightPink"
                               Invoked="OnDeleteSwipeItemInvoked" />
                </SwipeItems>
            </SwipeView.LeftItems>
            <!-- Content -->
        </SwipeView>
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<iOS>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

`SwipeView.On<iOS>`メソッドは、このプラットフォーム仕様が iOS でのみ動作することを指定します。 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間の `SwipeView.SetSwipeTransitionMode` メソッドを使用して、`SwipeView`を開くときに使用される遷移を制御します。 `SwipeTransitionMode` 列挙体には、次の2つの値があります。

- `Reveal` は、`SwipeView` コンテンツがスワイプされるとスワイプ項目が表示されることを示し、は `SwipeView.SwipeTransitionMode` プロパティの既定値です。
- `Drag` は、`SwipeView` コンテンツがスワイプとして、スワイプ項目がビューにドラッグされることを示します。

さらに、`SwipeView.GetSwipeTransitionMode` メソッドを使用して、`SwipeView`に適用される `SwipeTransitionMode` を返すことができます。

結果として、指定された `SwipeTransitionMode` 値が `SwipeView`に適用され、`SwipeView`を開くときに使用される遷移を制御します。

[![IOS 上の SwipeView SwipeTransitionModes のスクリーンショット](swipeview-swipetransitionmode-images/swipetransitionmode.png "IOS での SwipeTransitionModes")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "IOS での SwipeTransitionModes")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
