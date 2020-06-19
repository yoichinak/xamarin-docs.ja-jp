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
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f5a2be4bd61056a42593fc45e1abdd3679795bc0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139985"
---
# <a name="tabbedpage-toolbar-placement-and-color-on-android"></a>TabbedPage Android 上のツールバーの配置と色

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

> [!IMPORTANT]
> では、ツールバーの色を設定するプラットフォームの詳細は互換性のために残されており、 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) [`SelectedTabColor`](xref:Xamarin.Forms.TabbedPage.SelectedTabColor) プロパティとプロパティによって置き換えられました [`UnselectedTabColor`](xref:Xamarin.Forms.TabbedPage.UnselectedTabColor) 。 詳細については、「 [Create a TabbedPage](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md#create-a-tabbedpage)」を参照してください。

これらのプラットフォームの詳細は、のツールバーの配置と色を設定するために使用され [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ます。 これら [`TabbedPage.ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty) のプロパティは、添付プロパティを列挙値に設定し、 [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) [`TabbedPage.BarItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty) [`TabbedPage.BarSelectedItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty) プロパティと添付プロパティをに設定することによって、XAML で使用され [`Color`](xref:Xamarin.Forms.Color) ます。

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

メソッドは、 `TabbedPage.On<Android>` これらのプラットフォーム固有のが Android でのみ実行されることを指定します。 [ `TabbedPage.SetToolbarPlacement` ] (Xref: Xamarin.FormsPlatformConfiguration. TabbedPage ()。 SetToolbarPlacement ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。TabbedPage}, Xamarin.Forms 。PlatformConfiguration. AndroidSpecific の ToolbarPlacement)) メソッドを使用して、の [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ツールバーの配置を設定し [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) ます。列挙体には次の値を指定します。

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default)–ツールバーがページ上の既定の場所に配置されることを示します。 これは、電話のページの上部、およびその他のデバイスの表現上のページの下部です。
- [`Top`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top)–ツールバーがページの上部に配置されることを示します。
- [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom)–ツールバーがページの下部に配置されることを示します。

また、[ `TabbedPage.SetBarItemColor` ] (xref: Xamarin.FormsPlatformConfiguration. AndroidSpecific () を指定し Xamarin.Forms ます。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。TabbedPage}, Xamarin.Forms 。Color)) と [ `TabbedPage.SetBarSelectedItemColor` ] (xref: Xamarin.Forms .PlatformConfiguration. AndroidSpecific. SetBarSelectedItemColor ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。TabbedPage}, Xamarin.Forms 。Color)) メソッドを使用して、ツールバー項目と選択したツールバー項目の色をそれぞれ設定します。

> [!NOTE]
> [ `GetToolbarPlacement` ] (Xref: Xamarin.FormsPlatformConfiguration. TabbedPage Xamarin.Forms placement () を指定します。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。TabbedPage})、[ `GetBarItemColor` ] (xref: Xamarin.Forms 。PlatformConfiguration. TabbedPage. GetBarItemColor ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。TabbedPage})、および [ `GetBarSelectedItemColor` ] (xref: Xamarin.Forms .PlatformConfiguration. AndroidSpecific. GetBarSelectedItemColor ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。TabbedPage})) メソッドを使用して、ツールバーの配置と色を取得でき [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ます。

結果として、ツールバーの配置、ツールバーの項目の色、および選択したツールバー項目の色をに設定でき [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ます。

![](tabbedpage-toolbar-placement-color-images/tabbedpage-toolbar-placement.png)

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
