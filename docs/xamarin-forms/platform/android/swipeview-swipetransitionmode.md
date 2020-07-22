---
title: Android での SwipeView スワイプ切り替えモード
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、SwipeView を開くときに使用される遷移を制御する Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 6B1F8213-9D62-4C40-9F04-881F1371B5AA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c420fe65b020067169230dd06dbcd5ce65c036ab
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128623"
---
# <a name="swipeview-swipe-transition-mode-on-android"></a>Android での SwipeView スワイプ切り替えモード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有のは、を開くときに使用される遷移を制御し `SwipeView` ます。 このメソッドは、バインド可能な `SwipeView.SwipeTransitionMode` プロパティを列挙体の値に設定することにより、XAML で使用され `SwipeTransitionMode` ます。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core" >
    <StackLayout>
        <SwipeView android:SwipeView.SwipeTransitionMode="Drag">
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
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<Android>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

メソッドは、 `SwipeView.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 `SwipeView.SetSwipeTransitionMode`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) を開くときに使用される遷移を制御するために使用され `SwipeView` ます。 `SwipeTransitionMode`列挙体には、次の2つの値があります。

- `Reveal`コンテンツがスワイプされたときにスワイプ項目が表示されることを示し `SwipeView` ます。は、プロパティの既定値です `SwipeView.SwipeTransitionMode` 。
- `Drag`コンテンツがスワイプされると、スワイプ項目がビューにドラッグされることを示し `SwipeView` ます。

また、メソッドを `SwipeView.GetSwipeTransitionMode` 使用して、に適用されたを返すこともでき `SwipeTransitionMode` `SwipeView` ます。

結果として、指定された `SwipeTransitionMode` 値がに適用され `SwipeView` ます。これは、を開くときに使用される遷移を制御し `SwipeView` ます。

[![Android 上の SwipeView SwipeTransitionModes のスクリーンショット](swipeview-swipetransitionmode-images/swipetransitionmode.png "Android での SwipeTransitionModes")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "Android での SwipeTransitionModes")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
