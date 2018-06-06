---
title: Windows プラットフォーム仕様
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、Xamarin.Forms 組み込みの Windows の Platform-Specifics の使用方法を説明します。
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 7299de658a3491928e9bbeaa4dd192a8e95c435e
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732802"
---
# <a name="windows-platform-specifics"></a>Windows プラットフォーム仕様

_プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。この記事では、Xamarin.Forms に組み込まれている Windows のプラットフォームの仕様を使用する方法を示します。_

ユニバーサル Windows プラットフォーム (UWP) を Xamarin.Forms には、次のプラットフォームの設定が含まれています。

- ツールバーの配置オプションを設定します。 詳細については、次を参照してください。[ツールバーの配置を変更する](#toolbar_placement)です。
- 折りたたみ、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)ナビゲーション バー。 詳細については、次を参照してください。 [MasterDetailPage ナビゲーション バーを折りたたみ](#collapsable_navigation_bar)です。
- 有効にすると、 [ `WebView` ](xref:Xamarin.Forms.WebView)アラートを表示する JavaScript UWP メッセージ ダイアログ ボックスでします。 詳細については、次を参照してください。 [JavaScript アラートを表示する](#webview-javascript-alert)です。
- 有効にすると、 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar)スペル チェック エンジンと対話します。 詳細については、次を参照してください。 [SearchBar スペル チェックを有効にすると](#searchbar-spellcheck)です。
- テキストのコンテンツからの読み取り順序を検出する[ `Entry` ](xref:Xamarin.Forms.Entry)、 [ `Editor` ](xref:Xamarin.Forms.Editor)、および[ `Label` ](xref:Xamarin.Forms.Label)インスタンス。 詳細については、次を参照してください。[コンテンツからの読み取り順序を検出する](#inputview-readingorder)です。
- サポートされているレガシの色のモードを無効にする[ `VisualElement`](xref:Xamarin.Forms.VisualElement)です。 詳細については、次を参照してください。[レガシ色のモードを無効にすると](#legacy-color-mode)です。
- タップ ジェスチャのサポートを有効にすると、 [ `ListView`](xref:Xamarin.Forms.ListView)です。 詳細については、次を参照してください。 [ListView でタップ ジェスチャ サポートの有効化](#listview-selectionmode)です。

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

<a name="searchbar-spellcheck" />

## <a name="enabling-searchbar-spell-check"></a>SearchBar スペル チェックを有効にします。

このプラットフォームに固有の有効な[ `SearchBar` ](xref:Xamarin.Forms.SearchBar)スペル チェック エンジンと対話します。 これは XAML で [`SearchBar.IsSpellCheckEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) 添付プロパティを `boolean` 値を設定して使用します。

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>`メソッドは、このプラットフォームに応じたがユニバーサル Windows プラットフォームでのみ実行されるを指定します。 [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間が、スペル チェック機能をオンおよびオフにします。 さらに、`SearchBar.SetIsSpellCheckEnabled`メソッドを呼び出すことによって、スペル チェックを切り替えを使用することができます、 [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar}))スペル チェック機能が有効になっているかどうかを返します。

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

結果は、その入力したテキストに、 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar)スペル チェックできる、ユーザーに表示されている正しくないスペルで。

![SearchBar スペル チェックのプラットフォームに応じた](windows-images/searchbar-spellcheck.png "SearchBar スペル チェックのプラットフォームに固有")

> [!NOTE]
> `SearchBar`クラス内で、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間も[ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*)と[ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*)有効および無効にするために使用する方法スペル チェック、 `SearchBar`、それぞれします。

<a name="inputview-readingorder" />

## <a name="detecting-reading-order-from-content"></a>コンテンツからの読み取り順序を検出します。

このプラットフォームに固有の双方向のテキストの読み取り順序 (左から右へまたは右から左へ) を有効に[ `Entry` ](xref:Xamarin.Forms.Entry)、 [ `Editor` ](xref:Xamarin.Forms.Editor)、および[ `Label` ](xref:Xamarin.Forms.Label)を動的に検出するインスタンス。 使用される XAML で設定して、 [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (の`Entry`と`Editor`インスタンス) または[ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty)添付プロパティを`boolean`値。

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>`メソッドは、このプラットフォームに応じたがユニバーサル Windows プラットフォームでのみ実行されるを指定します。 [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)のコンテンツから読み取り順序が検出されたかどうか、名前空間を使用して、 [ `InputView` ](xref:Xamarin.Forms.InputView). さらに、`InputView.SetDetectReadingOrderFromContent`をコンテンツから呼び出すことによって、読み取り順序が検出されたかどうかを切り替えるメソッドを使用することができます、 [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView}))を現在の値を返すメソッド。

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

結果を[ `Entry` ](xref:Xamarin.Forms.Entry)、 [ `Editor` ](xref:Xamarin.Forms.Editor)、および[ `Label` ](xref:Xamarin.Forms.Label)インスタンスが動的に検出されたコンテンツの読み取り順序を持つことができます。

[![プラットフォーム固有のコンテンツからの読み取り順序を検出する InputView](windows-images/editor-readingorder.png "プラットフォーム固有のコンテンツからの読み取り順序を検出する InputView")](windows-images/editor-readingorder-large.png#lightbox "InputView からの読み取り順序を検出します。プラットフォーム固有のコンテンツ")

> [!NOTE]
> 設定とは異なり、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ、ビュー、ビュー内のテキストの配置は無効になります、テキスト コンテンツからの読み取り順序を検出するためのロジックです。 代わりでの双方向テキスト ブロックがレイアウトの順序を調整します。

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>レガシ色のモードを無効にします。

Xamarin.Forms ビューの一部の機能、従来の色のモード。 このモードでときに、 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled)ビューのプロパティに設定されて`false`ビューには、既定のネイティブ色無効の状態を使用して、ユーザーが設定した色がよりも優先されます。 旧バージョンと互換性のため、この従来の色のモードはサポートされているビューの既定の動作のままです。

このプラットフォームに固有では、ビューが無効になっている場合でも、ユーザーがビューに設定した色が維持されるよう、この従来の色のモードが無効にします。 使用される XAML で設定して、 [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty)添付プロパティ`false`:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Windows>`メソッドは、このプラットフォームに応じたが Windows でのみ実行されるを指定します。 [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間を使用して、従来の色のモードが無効になっているかどうか。 さらに、 [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement}))を従来の色のモードが無効になっているかどうかを返すメソッドを使用できます。

結果は、レガシ カラー モードを無効にすること、ように、ユーザーがビューに設定した色もビューが無効になっています。

![](windows-images/legacy-color-mode-disabled.png "従来の色のモードが無効になっています")

> [!NOTE]
> 設定するときに、 [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup)レガシ色のモードが完全に無視されますをビューにします。 表示状態の詳細については、次を参照してください。 [、Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)です。

<a name="listview-selectionmode" />

## <a name="enabling-tap-gesture-support-in-a-listview"></a>ListView でタップ ジェスチャのサポートを有効にします。

ユニバーサル Windows プラットフォームで、既定では、Xamarin.Forms で[ `ListView` ](xref:Xamarin.Forms.ListView)ネイティブを使用して`ItemClick`ネイティブではなくとの対話に応答するイベント`Tapped`イベント。 これにより、ナレーターとキーボードに対話できるように、ユーザー補助機能、`ListView`です。 ただし、レンダリング内の任意のタップ ジェスチャ、`ListView`使用できなくなります。

このプラットフォームに固有のコントロールかどうかの項目を[ `ListView` ](xref:Xamarin.Forms.ListView)タップ ジェスチャに応答できるためかどうか、ネイティブ`ListView`発生、`ItemClick`または`Tapped`イベント。 これは、 XAML で[`ListView.SelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty)添付プロパティを[`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)列挙型の値に設定して使用します。

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>`メソッドは、このプラットフォームに応じたがユニバーサル Windows プラットフォームでのみ実行されるを指定します。 [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間を使用しているかどうかの項目を[ `ListView` ](xref:Xamarin.Forms.ListView)でジェスチャをタップする応答できる、[ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)列挙型の 2 つの値を提供します。

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – ことを示します、`ListView`ネイティブは起動`ItemClick`との対話を処理し、そのためユーザー補助機能を提供するイベントです。 そのため、Windows ナレーターとキーボードに対話できる、`ListView`です。 ただし、項目、`ListView`タップ ジェスチャに応答できません。 既定の動作は、この`ListView`ユニバーサル Windows プラットフォーム上のインスタンス。
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – ことを示します、`ListView`ネイティブは起動`Tapped`の対話を処理するイベントです。 そのため、項目、`ListView`タップ ジェスチャに応答できます。 ただし、ユーザー補助機能はありませんし、そのため、ナレーターとキーボード操作はできません、`ListView`です。

> [!NOTE]
> `Accessible`と`Inaccessible`選択モードは相互に排他的と、アクセス可能なのかを選択する必要があります[ `ListView` ](xref:Xamarin.Forms.ListView)または`ListView`タップ ジェスチャに応答することができます。

さらに、 [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView}))を現在を返すメソッドを使用できます[ `ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)です。

結果は、指定した[ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)に適用される、 [ `ListView` ](xref:Xamarin.Forms.ListView)、制御するかどうかの項目を`ListView`タップ ジェスチャに応答できると同じようかどうか、ネイティブ`ListView`発生、`ItemClick`または`Tapped`イベント。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms に組み込まれている Windows のプラットフォームの仕様を使用する方法を示しました。 プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
