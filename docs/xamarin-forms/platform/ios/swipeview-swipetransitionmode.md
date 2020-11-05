---
title: IOS の SwipeView スワイプ切り替えモード
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、SwipeView を開くときに使用される遷移を制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: C667F24C-BAD8-47E0-9285-D3546BEF703B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 01e3d98fe92a69337d97f724d6027e33a9684515
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93371405"
---
# <a name="swipeview-swipe-transition-mode-on-ios"></a>IOS の SwipeView スワイプ切り替えモード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、を開くときに使用される遷移を制御し `SwipeView` ます。 このメソッドは、バインド可能な `SwipeView.SwipeTransitionMode` プロパティを列挙体の値に設定することにより、XAML で使用され `SwipeTransitionMode` ます。

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

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<iOS>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

メソッドは、 `SwipeView.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `SwipeView.SetSwipeTransitionMode`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) を開くときに使用される遷移を制御するために使用され `SwipeView` ます。 `SwipeTransitionMode`列挙体には、次の2つの値があります。

- `Reveal` コンテンツがスワイプされたときにスワイプ項目が表示されることを示し `SwipeView` ます。は、プロパティの既定値です `SwipeView.SwipeTransitionMode` 。
- `Drag` コンテンツがスワイプされると、スワイプ項目がビューにドラッグされることを示し `SwipeView` ます。

また、メソッドを `SwipeView.GetSwipeTransitionMode` 使用して、に適用されたを返すこともでき `SwipeTransitionMode` `SwipeView` ます。

結果として、指定された `SwipeTransitionMode` 値がに適用され `SwipeView` ます。これは、を開くときに使用される遷移を制御し `SwipeView` ます。

[![IOS 上の SwipeView SwipeTransitionModes のスクリーンショット](swipeview-swipetransitionmode-images/swipetransitionmode.png "IOS での SwipeTransitionModes")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "IOS での SwipeTransitionModes")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)