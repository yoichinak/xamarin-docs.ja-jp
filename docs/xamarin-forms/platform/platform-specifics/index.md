---
title: プラットフォーム仕様
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォーム上でのみ利用できる機能の使用を可能にします。
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
ms.openlocfilehash: 0b062a99954d4200a343d502f7577208e5e5dd8d
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171301"
---
# <a name="platform-specifics"></a>プラットフォーム仕様

_プラットフォーム仕様は、 カスタム レンダラーや特殊効果を実装することなく、特定のプラットフォーム上でのみ利用できる機能の使用を可能にします。_

Xamarin.Forms のビュー、ページ、およびレイアウトの次のプラットフォーム固有の機能が提供されます。

|iOS|Android|Windows|
|--- |--- |--- |
|[VisualElement.BlurEffect](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur)|[VisualElement.Elevation](~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation)|[VisualElement.AccessKey、VisualElement.AccessKeyPlacement、VisualElement.AccessKeyHorizontalOffset、および VisualElement.AccessKeyVerticalOffset](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#visualelement-accesskeys)|
|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#legacy-color-mode)|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#legacy-color-mode)|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#legacy-color-mode)|
|[VisualElement.IsShadowEnabled](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#drop-shadow)|

Xamarin.Forms のビューでは、次のプラットフォームに固有の機能が提供されます。

|iOS|Android|Windows|
|--- |--- |--- |
|[Entry.AdjustsFontSizeToFitWidth](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size)|[Button.UseDefaultPadding と Button.UseDefaultShadow](~/xamarin-forms/platform/platform-specifics/consuming/android.md#button-padding-shadow)|[InputView.DetectReadingOrderFromContent、Label.DetectReadingOrderFromContent](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#inputview-readingorder)|
|[Entry.CursorColor](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#entry-cursorcolor)|[Entry.ImeOptions](~/xamarin-forms/platform/platform-specifics/consuming/android.md#entry-imeoptions)|[ListView.SelectionMode](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#listview-selectionmode)|
|[ListView.SeparatorStyle](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#listview-separatorstyle)|[ImageButton.IsShadowEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#imagebutton-drop-shadow)|[SearchBar.IsSpellCheckEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#searchbar-spellcheck)|
|[Picker.UpdateMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)|[ListView.IsFastScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll)|[WebView.IsJavaScriptAlertEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#webview-javascript-alert)|
|[Slider.UpdateOnTap](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#slider-updateontap)|[WebView.MixedContentMode](~/xamarin-forms/platform/platform-specifics/consuming/android.md#webview-mixed-content)|

Xamarin.Forms のページでは、次のプラットフォームに固有の機能が提供されます。

|iOS|Android|Windows|
|--- |--- |--- |
|[NavigationPage.HideSeparatorBar](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#navigationpage-hideseparatorbar)|[NavigationPage.BarHeight](~/xamarin-forms/platform/platform-specifics/consuming/android.md#navigationpage-barheight)|[MasterDetailPage.CollapsedPaneWidth と MasterDetailPage.CollapseStyle](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar)|
|[NavigationPage.IsNavigationBarTranslucent](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar)|[TabbedPage.IsSmoothScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-transition-animations)|[Page.ToolbarPlacement](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement)|
|[NavigationPage.StatusBarTextColorMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode)|[TabbedPage.IsSwipePagingEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging)|[TabbedPage.HeaderIconsEnabled と TabbedPage.HeaderIconsSize](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#tabbedpage-icons)|
|[NavigationPage.PrefersLargeTitles](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title)|[TabbedPage.ToolbarPlacement、TabbedPage.BarItemColor、および TabbedPage.BarSelectedItemColor](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar)|
|[Page.ModalPresentationStyle](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#modal-page-presentation-style)|
|[Page.PrefersStatusBarHidden と Page.PreferredStatusBarUpdateAnimation](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility)|
|[Page.UseSafeArea](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout)|

Xamarin.Forms のレイアウトでは、次のプラットフォームに固有の機能が提供されます。

|iOS|
|--- |
|[ScrollView.ShouldDelayContentTouches](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches)|

Xamarin.Forms のプラットフォーム固有の次の機能が提供される[ `Application` ](xref:Xamarin.Forms.Application)クラス。

|iOS|Android|
|--- |--- |
|[Application.HandleControlUpdatesOnMainThread](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#update-on-main-thread)|[Application.WindowSoftInputModeAdjust](~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode)|
|[Application.PanGestureRecognizerShouldRecognizeSimultaneously](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#simultaneous-pan-gesture)|[Application.SendDisappearingEventOnPause、Application.SendAppearingEventOnResume、および Application.ShouldPreserveKeyboardOnResume](~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events)|

## <a name="consuming-platform-specifics"></a>プラットフォーム固有設定の使用

XAML、またはコードの fluent API を通じてプラットフォームに固有の使用するためのプロセスは次のとおりです。

1. [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration)名前空間に`xmlns`宣言または`using`ディレクティブを追加します。
1. プラットフォーム仕様の機能が含まれる名前空間に`xmlns`宣言または`using`ディレクティブを追加します
    1. iOS の場合、[`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間です。
    1. Android の場合は、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間です。 Android AppCompat の場合は、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)名前空間です。
    1. ユニバーサル Windows プラットフォームの場合は、[`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間です。
1. XAML から、または `On<T>` fluent API を含むコードからプラットフォーム仕様を適用します。 `T`の値には、[`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration)名前空間から [`iOS`](xref:Xamarin.Forms.PlatformConfiguration.iOS)、[`Android`](xref:Xamarin.Forms.PlatformConfiguration.Android)、または [`Windows`](xref:Xamarin.Forms.PlatformConfiguration.Windows)の型を指定できます。

> [!NOTE]
> プラットフォーム仕様を利用できないプラットフォーム上で、プラットフォーム仕様の使おうとしてもエラーにはならないことに注意してください。 そのかわり、そのコードはプラットフォーム仕様の適用なしで実行されます。

プラットフォーム仕様は、[`IPlatformElementConfiguration`](xref:Xamarin.Forms.IPlatformElementConfiguration`2)オブジェクトを返すコードの `On<T>` fluent API を通じて使用します。 これにより、メソッドのカスケードを同じオブジェクトで呼び出される複数のプラットフォーム固有です。

プラットフォーム仕様の詳細については、「[Platform-Specific の使用](~/xamarin-forms/platform/platform-specifics/consuming/index.md)」や「[Platform-Specific の作成](~/xamarin-forms/platform/platform-specifics/creating.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様の使用](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](xref:Xamarin.Forms.PlatformConfiguration)
