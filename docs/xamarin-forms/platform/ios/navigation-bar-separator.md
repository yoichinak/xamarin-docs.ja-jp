---
title: IOS 上の NavigationPage バーの区切り記号
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、NavigationPage のナビゲーションバーの下部にある区切り線と影を非表示にする iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 5A45748A-6779-4441-82F2-415BD68473B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 44bd1ca75220d0aefe2f26bf0e3ba1afcde93150
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372354"
---
# <a name="navigationpage-bar-separator-on-ios"></a>IOS 上の NavigationPage バーの区切り記号

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、のナビゲーションバーの下部にある区切り線と影を非表示にし [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ます。 これは、バインド可能なプロパティをに設定することによって XAML で使用され [`NavigationPage.HideNavigationBarSeparator`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) `false` ます。

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ios:NavigationPage.HideNavigationBarSeparator="true">

</NavigationPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;

public class iOSTitleViewNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public iOSTitleViewNavigationPageCS()
    {
        On<iOS>().SetHideNavigationBarSeparator(true);
    }
}
```

メソッドは、 `NavigationPage.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [ `NavigationPage.SetHideNavigationBarSeparator` ] (Xref: Xamarin.FormsPlatformConfiguration. Xamarin.Forms SetHideNavigationBarSeparator () を指定します。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。NavigationPage}, Boolean) メソッドを [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間で使用して、ナビゲーションバーの区切り記号を非表示にするかどうかを制御します。 また、[ `NavigationPage.HideNavigationBarSeparator` ] (xref: Xamarin.FormsPlatformConfiguration. Xamarin.Forms HideNavigationBarSeparator () を指定します。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。NavigationPage})) メソッドを使用して、ナビゲーションバーの区切り記号が非表示になっているかどうかを返すことができます。

結果として、のナビゲーションバーの区切り記号を [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 非表示にすることができます。

![NavigationPage ナビゲーションバーが非表示](navigation-bar-separator-images/navigationpage-hideseparatorbar.png)

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)