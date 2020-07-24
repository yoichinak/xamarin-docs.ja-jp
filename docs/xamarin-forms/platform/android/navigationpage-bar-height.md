---
title: Android での NavigationPage バーの高さ
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、NavigationPage のナビゲーションバーの高さを設定する Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: C8A73B64-FE70-408A-A72E-8AF147F0C52C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5f5e6311c79a88a6018526a2e1c0c06065eefb32
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929741"
---
# <a name="navigationpage-bar-height-on-android"></a>Android での NavigationPage バーの高さ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有のは、のナビゲーションバーの高さを設定 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) します。 これは、 [`NavigationPage.BarHeight`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) バインド可能なプロパティを整数値に設定することによって XAML で使用されます。

```xaml
<NavigationPage ...
                xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
                android:NavigationPage.BarHeight="450">
    ...
</NavigationPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

public class AndroidNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public AndroidNavigationPageCS()
    {
        On<Android>().SetBarHeight(450);
    }
}
```

メソッドは、 `NavigationPage.On<Android>` このプラットフォーム固有のがアプリ互換性 Android でのみ実行されることを指定します。 [ `NavigationPage.SetBarHeight` ] (Xref: Xamarin.FormsPlatformConfiguration. AndroidSpecific の...。 SetBarHeight ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。NavigationPage}, system.string) メソッドを [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) 名前空間で使用して、のナビゲーションバーの高さを設定し [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ます。 また、[ `NavigationPage.GetBarHeight` ] (xref: Xamarin.FormsPlatformConfiguration. AndroidSpecific の...//Height ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。NavigationPage})) メソッドを使用して、のナビゲーションバーの高さを返すことができ `NavigationPage` ます。

結果として、のナビゲーションバーの高さを [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 設定できます。

![NavigationPage ナビゲーションバーの高さ](navigationpage-bar-height-images/navigationpage-barheight.png)

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
