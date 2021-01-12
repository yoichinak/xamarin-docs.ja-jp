---
title: Windows 上の FlyoutPage ナビゲーションバー
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、FlyoutPage のナビゲーションバーを折りたたむ Windows プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 0E7436C9-FA3E-40CD-801C-3F7ED95C412D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e2f6a4c3eaf2668eff7e2123378175f7844a3413
ms.sourcegitcommit: 1decf2c65dc4c36513f7dd459a5df01e170a036f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/12/2021
ms.locfileid: "98115146"
---
# <a name="flyoutpage-navigation-bar-on-windows"></a>Windows 上の FlyoutPage ナビゲーションバー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

このユニバーサル Windows プラットフォーム、のナビゲーションバーを折りたたむために使用され [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) ます。 XAML では、 [`FlyoutPage.CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.FlyoutPage.CollapseStyleProperty) プロパティと添付プロパティを設定することによって使用され [`FlyoutPage.CollapsedPaneWidth`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.FlyoutPage.CollapsedPaneWidthProperty) ます。

```xaml
<FlyoutPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:FlyoutPage.CollapseStyle="Partial"
                  windows:FlyoutPage.CollapsedPaneWidth="48">
  ...
</FlyoutPage>

```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

メソッドは、 `FlyoutPage.On<Windows>` このプラットフォーム固有のが Windows 上でのみ実行されることを指定します。 [ `Page.SetCollapseStyle` ] (Xref: Xamarin.FormsPlatformConfiguration. FlyoutPage. SetCollapseStyle ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。FlyoutPage}, Xamarin.Forms 。CollapseStyle)) メソッドを名前空間で使用して、折りたたみスタイルを指定します。 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 列挙体には、との2つの値を指定し [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) [`Full`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) [`Partial`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial) ます。 [ `FlyoutPage.CollapsedPaneWidth` ] (Xref: Xamarin.FormsPlatformConfiguration. FlyoutPage. CollapsedPaneWidth ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。FlyoutPage}, system.string) メソッドを使用して、部分的に折りたたまれたナビゲーションバーの幅を指定します。

結果として、指定されたが [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) インスタンスに適用され [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) 、幅も指定されます。

[![折りたたまれたナビゲーションバープラットフォーム固有](flyoutpage-navigation-bar-images/collapsed-navigation-bar.png)](flyoutpage-navigation-bar-images/collapsed-navigation-bar-large.png#lightbox "折りたたまれたナビゲーションバー Platform-Specific")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)