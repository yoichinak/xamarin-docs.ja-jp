---
title: Windows プラットフォーム固有
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、Xamarin.Forms 組み込みの Windows の Platform-Specifics の使用方法を説明します。
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 52895564ef327845940d687a58b007fb1502e62b
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935124"
---
# <a name="windows-platform-specifics"></a>Windows プラットフォーム固有

_プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。この記事では、Xamarin.Forms に組み込まれている Windows プラットフォームの詳細を使用する方法を示します。_

ユニバーサル Windows プラットフォーム (UWP) で Xamarin.Forms には、次のプラットフォームの設定が含まれています。

- ツールバーの配置オプションを設定します。 詳細については、次を参照してください。[ツールバーの配置を変更する](#toolbar_placement)します。
- 折りたたみ、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)ナビゲーション バー。 詳細については、次を参照してください。 [MasterDetailPage のナビゲーション バーを折りたたみ](#collapsable_navigation_bar)します。
- 有効にすると、 [ `WebView` ](xref:Xamarin.Forms.WebView) UWP メッセージ ダイアログ ボックスで、JavaScript のアラートを表示します。 詳細については、次を参照してください。 [JavaScript のアラートを表示する](#webview-javascript-alert)します。
- 有効にすると、 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar)スペル チェック エンジンと対話します。 詳細については、次を参照してください。 [SearchBar スペル チェックを有効にする](#searchbar-spellcheck)します。
- 検出、テキスト コンテンツからの読み取り順序[ `Entry` ](xref:Xamarin.Forms.Entry)、 [ `Editor` ](xref:Xamarin.Forms.Editor)、および[ `Label` ](xref:Xamarin.Forms.Label)インスタンス。 詳細については、次を参照してください。[コンテンツからの読み取り順序を検出する](#inputview-readingorder)します。
- サポートされている従来のカラー モードを無効にする[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 詳細については、次を参照してください。[レガシ カラー モードを無効にすると](#legacy-color-mode)します。
- タップ ジェスチャのサポートを有効にすると、 [ `ListView`](xref:Xamarin.Forms.ListView)します。 詳細については、次を参照してください。[タップ ジェスチャのサポート、ListView で有効にする](#listview-selectionmode)します。

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>ツールバーの配置を変更します。

このプラットフォームに固有の使用のツールバーの配置を変更する、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)、設定によって、XAML で使用されると、 [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/)添付プロパティの値を[`ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/)列挙体。

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

`Page.On<Windows>`メソッドは、このプラットフォームに固有が Windows でのみ実行されるを指定します。 [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)でツールバーの配置を設定するため、名前空間、 [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement)列挙体を提供します。3 つの値: [ `Default` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default)、 [ `Top` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top)、および[ `Bottom`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom)します。

指定されたツールバーの配置が適用されることになります、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)インスタンス。

[![](windows-images/toolbar-placement.png "ツールバーの配置のプラットフォーム固有")](windows-images/toolbar-placement-large.png#lightbox "ツールバーの配置のプラットフォームに固有")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>MasterDetailPage のナビゲーション バーの折りたたみ

このプラットフォームに固有のナビゲーション バーを縮小するために使用する[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)、設定によって、XAML で使用されると、 [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/)と[ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)添付プロパティ。

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`MasterDetailPage.On<Windows>`メソッドは、このプラットフォームに固有が Windows でのみ実行されるを指定します。 [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)と折りたたみのスタイルを指定する名前空間が使用される、 [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/)列挙型の 2 つを提供します。値: [ `Full` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full)と[ `Partial`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial)します。 [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/)メソッドを使用して、部分的に折りたたまれたナビゲーション バーの幅を指定します。

結果は、指定されたを[ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/)に適用される、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)も指定されている幅でのインスタンス。

[![](windows-images/collapsed-navigation-bar.png "プラットフォームに固有のナビゲーション バーが折りたたまれている")](windows-images/collapsed-navigation-bar-large.png#lightbox "プラットフォームに固有のナビゲーション バーが折りたたまれています。")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>JavaScript の警告を表示します。

このプラットフォームに固有の有効、 [ `WebView` ](xref:Xamarin.Forms.WebView) UWP メッセージ ダイアログ ボックスで、JavaScript のアラートを表示します。 これは XAML で [`WebView.IsJavaScriptAlertEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) 添付プロパティを `boolean` 値を設定して使用します。

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

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

`WebView.On<Windows>`メソッドは、このプラットフォームに固有はユニバーサル Windows プラットフォームでのみ実行されるを指定します。 [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間を使用して、JavaScript のアラートが有効になっているかどうか。 さらに、`WebView.SetIsJavaScriptAlertEnabled`メソッドを使用して呼び出すことによって JavaScript のアラートを切り替える、 [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*)を有効にするかどうかを返すメソッド。

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

結果は、JavaScript のアラートを UWP メッセージ ダイアログに表示できます。

![Web ビューの JavaScript アラート プラットフォームに固有](windows-images/webview-javascript-alert.png "WebView JavaScript アラート プラットフォームに固有")

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

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>`メソッドは、このプラットフォームに固有はユニバーサル Windows プラットフォームでのみ実行されるを指定します。 [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)オンとオフ、名前空間が、スペル チェックをオンにします。 さらに、`SearchBar.SetIsSpellCheckEnabled`メソッドを使用して呼び出すことによって、スペル チェックを切り替える、 [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar}))スペル チェックが有効になっているかどうかを返します。

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

結果は、そのテキストを入力、 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar)スペルを確認できます、正しくないスペルがユーザーに示されます。

![SearchBar スペル チェック プラットフォーム固有](windows-images/searchbar-spellcheck.png "SearchBar スペル チェックのプラットフォームに固有")

> [!NOTE]
> `SearchBar`クラス、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間も[ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*)と[ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*)有効および無効にするために使用する方法スペル チェック、 `SearchBar`、それぞれします。

<a name="inputview-readingorder" />

## <a name="detecting-reading-order-from-content"></a>コンテンツからの読み取り順序を検出します。

このプラットフォームに固有の双方向のテキストの読み取り順序 (左から右または右から左) の有効[ `Entry` ](xref:Xamarin.Forms.Entry)、 [ `Editor` ](xref:Xamarin.Forms.Editor)、および[ `Label` ](xref:Xamarin.Forms.Label)インスタンスを動的に検出します。 XAML で設定して使用される、 [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (の`Entry`と`Editor`インスタンス) または[ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty)添付プロパティを`boolean`値。

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>`メソッドは、このプラットフォームに固有はユニバーサル Windows プラットフォームでのみ実行されるを指定します。 [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)のコンテンツからの読み取り順序が検出されたかどうか、名前空間を使用して、 [ `InputView` ](xref:Xamarin.Forms.InputView). さらに、`InputView.SetDetectReadingOrderFromContent`メソッドを使用して呼び出すことによって、コンテンツからの読み取り順序が検出されたかどうかを切り替える、 [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView}))を現在の値を返すメソッド。

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

その結果、 [ `Entry` ](xref:Xamarin.Forms.Entry)、 [ `Editor` ](xref:Xamarin.Forms.Editor)、および[ `Label` ](xref:Xamarin.Forms.Label)インスタンスが動的に検出されたコンテンツの読み取り順序を持つことができます。

[![プラットフォーム固有のコンテンツからの読み取り順序を検出する InputView](windows-images/editor-readingorder.png "InputView プラットフォームに固有のコンテンツからの読み取り順序を検出する")](windows-images/editor-readingorder-large.png#lightbox "InputView からの読み取り順序を検出します。プラットフォーム固有のコンテンツ")

> [!NOTE]
> 設定とは異なり、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ、ビューのテキスト コンテンツからの読み取り順序では、ビュー内のテキストの配置には影響は検出ロジック。 代わりでの双方向テキスト ブロックはレイアウトの順序を調整します。

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>レガシ カラー モードを無効にします。

Xamarin.Forms のビューのいくつかの機能の色をレガシ モード。 このモードでは、ときに、 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled)ビューのプロパティに設定されて`false`ビューが既定のネイティブ色無効の状態を使用してユーザー設定の色をオーバーライドします。 旧バージョンと互換性のため、この色のレガシ モードでは、サポートされているビューの既定の動作は残ります。

このプラットフォームに固有では、ビューが無効になっている場合でも、ユーザーがビューの設定の色が維持されるようにこの色のレガシ モードが無効にします。 XAML で設定して使用される、 [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty)添付プロパティを`false`:

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

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Windows>`メソッドは、このプラットフォームに固有が Windows でのみ実行されるを指定します。 [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間を使用してレガシのカラー モードが無効になっているかどうか。 さらに、 [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement}))を従来のカラー モードが無効になっているかどうかを返すメソッドを使用できます。

結果は、ことは、従来のカラー モードを無効にできます、ように、ユーザーがビューの設定の色もビューが無効になっています。

![](windows-images/legacy-color-mode-disabled.png "従来のカラー モードを無効になっています")

> [!NOTE]
> 設定するときに、 [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup)レガシ カラー モードが完全に無視されます、ビューにします。 表示状態の詳細については、次を参照してください。 [、Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)します。

<a name="listview-selectionmode" />

## <a name="enabling-tap-gesture-support-in-a-listview"></a>ListView のタップ ジェスチャ サポートを有効にします。

既定では、Xamarin.Forms、ユニバーサル Windows プラットフォームで[ `ListView` ](xref:Xamarin.Forms.ListView)ネイティブを使用して`ItemClick`ネイティブではなくとの対話に応答するイベント`Tapped`イベント。 これにより Windows ナレーターとキーボードで操作できるように、ユーザー補助機能、`ListView`します。 ただし、内の任意のタップ ジェスチャ レンダリングも、`ListView`使用できなくなります。

このプラットフォームに固有のコントロールかどうか内の項目を[ `ListView` ](xref:Xamarin.Forms.ListView)タップ ジェスチャに応答できるため、かどうか、ネイティブ`ListView`発生、`ItemClick`または`Tapped`イベント。 これは、 XAML で[`ListView.SelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty)添付プロパティを[`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)列挙型の値に設定して使用します。

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

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>`メソッドは、このプラットフォームに固有はユニバーサル Windows プラットフォームでのみ実行されるを指定します。 [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間を使用しているかどうかの項目を[ `ListView` ](xref:Xamarin.Forms.ListView)でジェスチャをタップに応答できる、[ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)列挙型の 2 つの値を提供します。

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – ことを示します、`ListView`はネイティブに起動`ItemClick`との対話を処理し、そのためユーザー補助機能を提供するイベントです。 そのため、Windows ナレーターおよびキーボードと対話できます、`ListView`します。 ただし、項目を`ListView`タップ ジェスチャに応答できません。 既定の動作は、この`ListView`ユニバーサル Windows プラットフォーム上のインスタンス。
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – ことを示します、`ListView`はネイティブに起動`Tapped`の対話を処理するイベントです。 そのため、項目を`ListView`タップ ジェスチャに応答できます。 ただし、ユーザー補助機能はありませんし、Windows ナレーターとキーボードと対話できませんので、`ListView`します。

> [!NOTE]
> `Accessible`と`Inaccessible`選択モードは相互に排他的と、アクセス可能な間を選択する必要があります[ `ListView` ](xref:Xamarin.Forms.ListView)または`ListView`タップ ジェスチャに応答することができます。

さらに、 [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView}))メソッドを使用して、現在を返す[ `ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)します。

結果はするが、指定された[ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)に適用される、 [ `ListView` ](xref:Xamarin.Forms.ListView)、どのコントロールかどうか内の項目、`ListView`タップ ジェスチャに応答できるため、かどうか、ネイティブ`ListView`発生、`ItemClick`または`Tapped`イベント。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms に組み込まれている Windows プラットフォームの詳細を使用する方法を示しました。 プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
