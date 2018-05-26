---
title: プラットフォーム仕様
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォーム上でのみ利用できる機能の使用を可能にします。
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
ms.openlocfilehash: 0d536c2c9c98ba65f80bf95810bf45d395ee785d
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/25/2018
---
# <a name="platform-specifics"></a>プラットフォーム仕様

_プラットフォーム仕様は、 カスタム レンダラーや特殊効果を実装することなく、特定のプラットフォーム上でのみ利用できる機能の使用を可能にします。_

次のようなプラットフォーム仕様の機能が、Xamarin.Forms に組み込まれています。

|iOS|Android|Windows|
|--- |--- |--- |
|[VisualElement.BlurEffect](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur)|[Application.WindowSoftInputModeAdjust](~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode)|[Page.ToolbarPlacement](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement)|
|[NavigationPage.PrefersLargeTitles](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title)|[ListView.IsFastScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll)|[MasterDetailPage.CollapsedPaneWidth と MasterDetailPage.CollapseStyle](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar)|
|[Page.UseSafeArea](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout)|[TabbedPage.IsSwipePagingEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging)|[WebView.IsJavaScriptAlertEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#webview-javascript-alert)
|[NavigationPage.IsNavigationBarTranslucent](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar)|[VisualElement.Elevation](~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation)|
|[NavigationPage.StatusBarTextColorMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode)|[Application.SendDisappearingEventOnPause、Application.SendAppearingEventOnResume、および Application.ShouldPreserveKeyboardOnResume](~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events)|
|[Entry.AdjustsFontSizeToFitWidth](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size)|[WebView.MixedContentMode](~/xamarin-forms/platform/platform-specifics/consuming/android.md#webview-mixed-content)
|[Picker.UpdateMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)|[Entry.ImeOptions](~/xamarin-forms/platform/platform-specifics/consuming/android.md#entry-imeoptions)
|[Page.PrefersStatusBarHidden と Page.PreferredStatusBarUpdateAnimation](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility)|
|[ScrollView.ShouldDelayContentTouches](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches)|
|[ListView.SeparatorStyle](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#listview-separatorstyle)|

XAML または fluent API のコードを使ったプラットフォーム仕様の使用手順は次のとおりです。

1. [`Xamarin.Forms.PlatformConfiguration`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/)名前空間に`xmlns`宣言または`using`ディレクティブを追加します。
1. プラットフォーム仕様の機能が含まれる名前空間に`xmlns`宣言または`using`ディレクティブを追加します
    1. iOS の場合、[`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)名前空間です。
    1. Android の場合は、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)名前空間です。 Android AppCompat の場合は、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)名前空間です。
    1. ユニバーサル Windows プラットフォームの場合は、[`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)名前空間です。
1. XAML から、または `On<T>` fluent API を含むコードからプラットフォーム仕様を適用します。 `T`の値には、[`Xamarin.Forms.PlatformConfiguration`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/)名前空間から [`iOS`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOS/)、[`Android`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Android/)、または [`Windows`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Windows/)の型を指定できます。

> [!NOTE]
> プラットフォーム仕様を利用できないプラットフォーム上で、プラットフォーム仕様の使おうとしてもエラーにはならないことに注意してください。 そのかわり、そのコードはプラットフォーム仕様の適用なしで実行されます。

プラットフォーム仕様は、[`IPlatformElementConfiguration`](https://developer.xamarin.com/api/type/Xamarin.Forms.IPlatformElementConfiguration%3CTPlatform,TElement%3E/)オブジェクトを返すコードの `On<T>` fluent API を通じて使用します。 これにより、メソッドの連鎖で同じオブジェクトで呼び出される複数のプラットフォーム固有です。

プラットフォーム仕様の詳細については、「[Platform-Specific の使用](~/xamarin-forms/platform/platform-specifics/consuming/index.md)」や「[Platform-Specific の作成](~/xamarin-forms/platform/platform-specifics/creating.md)」を参照してください。


## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様の使用](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/)
