---
title: TabbedPage Android 上のツールバーの配置と色
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、TabbedPage のツールバーの配置と色を設定する Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: A5C68D6A-9A5F-42EE-845D-1E5B0CB1544E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: e32c516c4a0a8b12bd8a76478557905149890c6a
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997333"
---
# <a name="tabbedpage-toolbar-placement-and-color-on-android"></a>TabbedPage Android 上のツールバーの配置と色

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

> [!IMPORTANT]
> では、ツールバーの色を設定するプラットフォームの詳細は互換性のために残されており、 [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) [`SelectedTabColor`](xref::::no-loc(Xamarin.Forms):::.TabbedPage.SelectedTabColor) プロパティとプロパティによって置き換えられました [`UnselectedTabColor`](xref::::no-loc(Xamarin.Forms):::.TabbedPage.UnselectedTabColor) 。 詳細については、「 [Create a TabbedPage](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md#create-a-tabbedpage)」を参照してください。

これらのプラットフォームの詳細は、のツールバーの配置と色を設定するために使用され [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) ます。 これら [`TabbedPage.ToolbarPlacement`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty) のプロパティは、添付プロパティを列挙値に設定し、 [`ToolbarPlacement`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) [`TabbedPage.BarItemColor`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty) [`TabbedPage.BarSelectedItemColor`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty) プロパティと添付プロパティをに設定することによって、XAML で使用され [`Color`](xref::::no-loc(Xamarin.Forms):::.Color) ます。

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;assembly=:::no-loc(Xamarin.Forms):::.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration;
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

メソッドは、 `TabbedPage.On<Android>` これらのプラットフォーム固有のが Android でのみ実行されることを指定します。 [ `TabbedPage.SetToolbarPlacement` ] (Xref: :::no-loc(Xamarin.Forms):::PlatformConfiguration. TabbedPage ()。 SetToolbarPlacement ( :::no-loc(Xamarin.Forms)::: .IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: .PlatformConfiguration. Android、 :::no-loc(Xamarin.Forms)::: 。TabbedPage}, :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. AndroidSpecific の ToolbarPlacement)) メソッドを使用して、の [`:::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific) ツールバーの配置を設定し [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) [`ToolbarPlacement`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) ます。列挙体には次の値を指定します。

- [`Default`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default)–ツールバーがページ上の既定の場所に配置されることを示します。 これは、電話のページの上部、およびその他のデバイスの表現上のページの下部です。
- [`Top`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top)–ツールバーがページの上部に配置されることを示します。
- [`Bottom`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom)–ツールバーがページの下部に配置されることを示します。

また、[ `TabbedPage.SetBarItemColor` ] (xref: :::no-loc(Xamarin.Forms):::PlatformConfiguration. AndroidSpecific () を指定し :::no-loc(Xamarin.Forms)::: ます。IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: .PlatformConfiguration. Android、 :::no-loc(Xamarin.Forms)::: 。TabbedPage}, :::no-loc(Xamarin.Forms)::: 。Color)) と [ `TabbedPage.SetBarSelectedItemColor` ] (xref: :::no-loc(Xamarin.Forms)::: .PlatformConfiguration. AndroidSpecific. SetBarSelectedItemColor ( :::no-loc(Xamarin.Forms)::: .IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: .PlatformConfiguration. Android、 :::no-loc(Xamarin.Forms)::: 。TabbedPage}, :::no-loc(Xamarin.Forms)::: 。Color)) メソッドを使用して、ツールバー項目と選択したツールバー項目の色をそれぞれ設定します。

> [!NOTE]
> [ `GetToolbarPlacement` ] (Xref: :::no-loc(Xamarin.Forms):::PlatformConfiguration. TabbedPage :::no-loc(Xamarin.Forms)::: placement () を指定します。IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: .PlatformConfiguration. Android、 :::no-loc(Xamarin.Forms)::: 。TabbedPage})、[ `GetBarItemColor` ] (xref: :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. TabbedPage. GetBarItemColor ( :::no-loc(Xamarin.Forms)::: .IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: .PlatformConfiguration. Android、 :::no-loc(Xamarin.Forms)::: 。TabbedPage})、および [ `GetBarSelectedItemColor` ] (xref: :::no-loc(Xamarin.Forms)::: .PlatformConfiguration. AndroidSpecific. GetBarSelectedItemColor ( :::no-loc(Xamarin.Forms)::: .IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: .PlatformConfiguration. Android、 :::no-loc(Xamarin.Forms)::: 。TabbedPage})) メソッドを使用して、ツールバーの配置と色を取得でき [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) ます。

結果として、ツールバーの配置、ツールバーの項目の色、および選択したツールバー項目の色をに設定でき [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) ます。

![TabbedPage ツールバーの構成](tabbedpage-toolbar-placement-color-images/tabbedpage-toolbar-placement.png)

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.AppCompat)
