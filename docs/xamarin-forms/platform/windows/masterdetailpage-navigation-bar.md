---
title: Windows 上の Masterのページナビゲーションバー
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、Master詳細ページのナビゲーションバーを折りたたむ、Windows プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 0E7436C9-FA3E-40CD-801C-3F7ED95C412D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e39c16b900b89f0f3ba4c55fffc4e360070a0540
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374811"
---
# <a name="masterdetailpage-navigation-bar-on-windows"></a>Windows 上の Masterのページナビゲーションバー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

このユニバーサル Windows プラットフォーム、のナビゲーションバーを折りたたむために使用され [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) ます。 XAML では、 [`MasterDetailPage.CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty) プロパティと添付プロパティを設定することによって使用され [`MasterDetailPage.CollapsedPaneWidth`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty) ます。

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

メソッドは、 `MasterDetailPage.On<Windows>` このプラットフォーム固有のが Windows 上でのみ実行されることを指定します。 [ `Page.SetCollapseStyle` ] (Xref: Xamarin.FormsPlatformConfiguration. WindowsSpecific Xamarin.Forms ()。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。Masterのページ}、 Xamarin.Forms 。CollapseStyle)) メソッドを名前空間で使用して、折りたたみスタイルを指定します。 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 列挙体には、との2つの値を指定し [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) [`Full`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) [`Partial`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial) ます。 [ `MasterDetailPage.CollapsedPaneWidth` ] (Xref: Xamarin.FormsPlatformConfiguration. WindowsSpecific Xamarin.Forms ()。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。Masterのページ}, system.string) メソッドを使用して、部分的に折りたたまれたナビゲーションバーの幅を指定します。

結果として、指定されたが [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) インスタンスに適用され [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 、幅も指定されます。

[![折りたたまれたナビゲーションバープラットフォーム固有](masterdetailpage-navigation-bar-images/collapsed-navigation-bar.png)](masterdetailpage-navigation-bar-images/collapsed-navigation-bar-large.png#lightbox "折りたたまれたナビゲーションバー Platform-Specific")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)