---
title: IOS でのホームのインジケーター可視性
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、ページのホームのインジケーターの可視性を設定します。 iOS プラットフォームに固有の使用方法について説明します。
ms.prod: xamarin
ms.assetid: F81022E0-3C6C-49C0-A000-FAF6574D3FB7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: c0d6717cd8f89344be7df3dc3ec687a5f1b375f4
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971624"
---
# <a name="home-indicator-visibility-on-ios"></a>IOS でのホームのインジケーター可視性

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

この iOS プラットフォームに固有のホームのインジケーターの可視性の設定、 [ `Page`](xref:Xamarin.Forms.Page)します。 XAML で設定して使用される、 [ `Page.PrefersHomeIndicatorAutoHidden` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHiddenProperty)バインド可能なプロパティを`boolean`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersHomeIndicatorAutoHidden="true">
    ...
</ContentPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersHomeIndicatorAutoHidden(true);
```

`Page.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  [ `Page.SetPrefersHomeIndicatorAutoHidden` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.SetPrefersHomeIndicatorAutoHidden(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Page},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間は、ホーム インジケーターの表示を制御します。 さらに、 [ `Page.PrefersHomeIndicatorAutoHidden` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHidden(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Page}))ホームのインジケーターの可視性を取得するメソッドを使用できます。

その結果、自宅のインジケーターの可視性、 [ `Page` ](xref:Xamarin.Forms.Page)制御できます。

![IOS ページ ホームのインジケーターの可視性のスクリーン ショット](page-home-indicator-images/home-indicator-visibility.png "ホーム インジケーターの可視性 ページ")

> [!NOTE]
> このプラットフォームに固有に適用できる[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)、 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)、および[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)オブジェクト。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
