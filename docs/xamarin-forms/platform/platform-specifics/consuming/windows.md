---
title: Windows プラットフォーム仕様
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、Xamarin.Forms 組み込みの Windows の Platform-Specifics の使用方法を説明します。
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2018
ms.openlocfilehash: d4ddb662bf167a0c80561cce097104a7f5fc8096
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/25/2018
---
# <a name="windows-platform-specifics"></a>Windows プラットフォーム仕様

_プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。この記事では、Xamarin.Forms に組み込まれている Windows のプラットフォームの仕様を使用する方法を示します。_

ユニバーサル Windows プラットフォーム (UWP) を Xamarin.Forms には、次のプラットフォームの設定が含まれています。

- ツールバーの配置オプションを設定します。 詳細については、次を参照してください。[ツールバーの配置を変更する](#toolbar_placement)です。
- 折りたたみ、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)ナビゲーション バー。 詳細については、次を参照してください。 [MasterDetailPage ナビゲーション バーを折りたたみ](#collapsable_navigation_bar)です。
- 有効にすると、 [ `WebView` ](xref:Xamarin.Forms.WebView)アラートを表示する JavaScript UWP メッセージ ダイアログ ボックスでします。 詳細については、次を参照してください。 [JavaScript アラートを表示する](#webview-javascript-alert)です。

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>ツールバーの配置を変更します。

このプラットフォームに固有を使用して、ツールバーの配置を変更する、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)、設定によって、XAML で使用されると、 [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/)添付プロパティの値を[`ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/)列挙します。

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

`Page.On<Windows>`メソッドは、このプラットフォームに応じたが Windows でのみ実行されるを指定します。 [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)でツールバーの配置を設定する、名前空間が使用される、 [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/)列挙体を提供します。3 つの値: [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default/)、 [ `Top` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top/)、および[ `Bottom`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom/)です。

結果は指定されたツールバーの配置に適用される、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)インスタンス。

[![](windows-images/toolbar-placement.png "ツールバーの配置のプラットフォームに応じた")](windows-images/toolbar-placement-large.png#lightbox "配置のプラットフォームに固有のツールバー")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>MasterDetailPage ナビゲーション バーを折りたたむ

このプラットフォームに固有が上のナビゲーション バーを折りたたむに使用される、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)、設定によって、XAML で使用されると、 [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/)と[ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)アタッチされるプロパティ。

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`MasterDetailPage.On<Windows>`メソッドは、このプラットフォームに応じたが Windows でのみ実行されるを指定します。 [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)を折りたたむスタイルを指定する名前空間が使用される、 [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/)を提供する 2 つの列挙値: [ `Full` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full/)と[ `Partial`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial/)です。 [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/)メソッドを使用して、部分的に折りたたまれたナビゲーション バーの幅を指定します。

結果は、指定した[ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/)に適用される、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)幅を指定されたものインスタンス。

[![](windows-images/collapsed-navigation-bar.png "プラットフォームに固有のナビゲーション バーが折りたたまれている")](windows-images/collapsed-navigation-bar-large.png#lightbox "プラットフォームに固有のナビゲーション バーを折りたたむ")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>JavaScript の警告を表示します。

このプラットフォームに固有の有効な[ `WebView` ](xref:Xamarin.Forms.WebView)アラートを表示する JavaScript UWP メッセージ ダイアログ ボックスでします。 これは XAML で [`WebView.IsJavaScriptAlertEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) 添付プロパティを `boolean` 値を設定して使用します。

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var webView = new Xamarin.Forms.WebView
{
  Source = new HtmlWebViewSource
  {
    Html = @"<html><body><button onclick=""window.alert('Hello World from JavaScript');"">Click Me</button></body></html>"
  }
};
webView.On<Windows>().SetIsJavaScriptAlertEnabled(true);
```

`WebView.On<Windows>`メソッドは、このプラットフォームに応じたがユニバーサル Windows プラットフォームでのみ実行されるを指定します。 [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間を使用して、JavaScript のアラートが有効になっているかどうか。 さらに、`WebView.SetIsJavaScriptAlertEnabled`メソッドを呼び出すことによって JavaScript のアラートを切り替えを使用することができます、 [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*)は有効になっているかどうかを返します。

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

結果は、JavaScript のアラートは、UWP メッセージ ダイアログ ボックスで表示できます。

![WebView JavaScript アラート プラットフォーム固有](windows-images/webview-javascript-alert.png "WebView JavaScript アラート プラットフォーム固有の仕様")

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms に組み込まれている Windows のプラットフォームの仕様を使用する方法を示しました。 プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
