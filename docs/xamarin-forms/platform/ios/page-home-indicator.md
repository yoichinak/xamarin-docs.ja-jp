---
title: IOS でのホームインジケーターの可視性
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ページ上のホームインジケーターの可視性を設定する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: F81022E0-3C6C-49C0-A000-FAF6574D3FB7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 49fd8c563509d88b80fcb91b5e6298b8a5bb3363
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372302"
---
# <a name="home-indicator-visibility-on-ios"></a>IOS でのホームインジケーターの可視性

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、上のホームインジケーターの可視性を設定し [`Page`](xref:Xamarin.Forms.Page) ます。 これは、バインド可能なプロパティをに設定することによって XAML で使用され [`Page.PrefersHomeIndicatorAutoHidden`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHiddenProperty) `boolean` ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersHomeIndicatorAutoHidden="true">
    ...
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersHomeIndicatorAutoHidden(true);
```

メソッドは、 `Page.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [ `Page.SetPrefersHomeIndicatorAutoHidden` ] (Xref: Xamarin.FormsPlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。Page}, Boolean)) メソッドを名前空間で、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ホームインジケーターの表示を制御します。 また、[ `Page.PrefersHomeIndicatorAutoHidden` ] (xref: Xamarin.FormsPlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。Page})) メソッドを使用して、ホームインジケーターの表示を取得できます。

その結果、上のホームインジケーターの表示を制御できるようになり [`Page`](xref:Xamarin.Forms.Page) ます。

![IOS ページでのホームインジケーターの表示のスクリーンショット](page-home-indicator-images/home-indicator-visibility.png "ページのホーム インジケーターの表示")

> [!NOTE]
> このプラットフォーム固有のは、、、、およびの各オブジェクトに適用でき [`ContentPage`](xref:Xamarin.Forms.ContentPage) [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)