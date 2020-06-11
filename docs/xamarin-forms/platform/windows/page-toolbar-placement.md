---
title: "Windows 上のページツールバーの配置" 説明: "プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ページ上のツールバーの配置を変更する Windows プラットフォーム固有のを使用する方法について説明します。
ms. 製品: xamarin ms. assetid: 99F29E95-0C36-4A3B-BDE8-7E9F119E844E: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 10/24/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="page-toolbar-placement-on-windows"></a>Windows 上のページツールバーの配置

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

このユニバーサル Windows プラットフォーム、のツールバーの配置を変更するために使用され [`Page`](xref:Xamarin.Forms.Page) ます。 XAML では、 [`Page.ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty) 添付プロパティを列挙体の値に設定することによって使用され [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) ます。

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

メソッドは、 `Page.On<Windows>` このプラットフォーム固有のが Windows 上でのみ実行されることを指定します。 [ `Page.SetToolbarPlacement` ] (Xref: Xamarin.FormsPlatformConfiguration. WindowsSpecific SetToolbarPlacement ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。ページ}、 Xamarin.Forms 。[`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) 、、およびの3つの値を指定する列挙体を使用して、ツールバーの配置を設定するには、名前空間のメソッドを使用 [`Default`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default) し [`Top`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top) ます [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom) 。

結果として、指定されたツールバーの配置がインスタンスに適用され [`Page`](xref:Xamarin.Forms.Page) ます。

[![](page-toolbar-placement-images/toolbar-placement.png "Toolbar Placement Platform-Specific")](page-toolbar-placement-images/toolbar-placement-large.png#lightbox "Toolbar Placement Platform-Specific")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
