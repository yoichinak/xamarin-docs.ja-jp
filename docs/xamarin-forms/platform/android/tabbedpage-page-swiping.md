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
ms.openlocfilehash: 61c0137788303363769fdec80a16542e2d8bea5e
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128610"
---
# <a name="tabbedpage-page-swiping-on-android"></a>Android での TabbedPage ページのスワイプ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有のは、内のページ間でスワイプを水平にするために使用され [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ます。 添付プロパティを値に設定することにより、XAML で使用 [`TabbedPage.IsSwipePagingEnabled`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) され `boolean` ます。

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

メソッドは、 `TabbedPage.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 [ `TabbedPage.SetIsSwipePagingEnabled` ] (Xref: Xamarin.FormsPlatformConfiguration. TabbedPage. SetIsSwipePagingEnabled ( Xamarin.Forms .BindableObject, system.string) メソッドを名前空間で使用して、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 内のページ間をスワイプできるように [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) します。 さらに、 `TabbedPage` 名前空間のクラスに `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` も [ `EnableSwipePaging` ] (xref: があり Xamarin.Forms ます。PlatformConfiguration. AndroidSpecific Xamarin.Forms () を指定します。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。TabbedPage})) を使用できるようにするメソッドを指定し `DisableSwipePaging` Xamarin.Forms ます。PlatformConfiguration. TabbedPage. DisableSwipePaging ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。TabbedPage})) メソッドを無効にします。 [`TabbedPage.OffscreenPageLimit`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty)添付プロパティ、および [ `SetOffscreenPageLimit` ] (xref: Xamarin.Forms 。PlatformConfiguration. TabbedPage. SetOffscreenPageLimit ( Xamarin.Forms .BindableObject, system.string) メソッドを使用して、現在のページの両側でアイドル状態に保持する必要があるページ数を設定します。

結果として、によって表示されるページのスワイプページング [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) が有効になります。

![](tabbedpage-page-swiping-images/tabbedpage-swipe.png)

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
