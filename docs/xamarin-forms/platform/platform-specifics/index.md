---
title: "プラットフォーム固有"
description: "プラットフォーム固有では、カスタム レンダラーや特殊効果を実装することがなく、特定のプラットフォームで利用可能なだけの機能を使用できます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
ms.openlocfilehash: e68b13cc7f673d607022751a1f1038fd057a90d8
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="platform-specifics"></a>プラットフォーム固有

_プラットフォーム固有では、カスタム レンダラーや特殊効果を実装することがなく、特定のプラットフォームで利用可能なだけの機能を使用できます。_

次のプラットフォーム固有の機能は、Xamarin.Forms に構成されています。

|iOS|Android|Windows|
|--- |--- |--- |
|[VisualElement.BlurEffect](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur)|[Application.WindowSoftInputModeAdjust](~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode)|[Page.ToolbarPlacement](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement)|
|[NavigationPage.PrefersLargeTitles](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title)|[ListView.IsFastScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll)|[MasterDetailPage.CollapsedPaneWidth と MasterDetailPage.CollapseStyle](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar)|
|[Page.UseSafeArea](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout)|[TabbedPage.IsSwipePagingEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging)|
|[NavigationPage.IsNavigationBarTranslucent](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar)|[Elevation.Elevation](~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation)|
|[NavigationPage.StatusBarTextColorMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode)|[Application.SendDisappearingEventOnPause、Application.SendAppearingEventOnResume、および Application.ShouldPreserveKeyboardOnResume](~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events)|
|[Entry.AdjustsFontSizeToFitWidth](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size)|
|[Picker.UpdateMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)|
|[Page.PrefersStatusBarHidden と Page.PreferredStatusBarUpdateAnimation](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility)|
|[ScrollView.ShouldDelayContentTouches](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches)|

XAML による fluent コード API によるかプラットフォームに固有の使用に関して、プロセスは次のとおりです。

1. 追加、`xmlns`宣言または`using`ディレクティブを[ `Xamarin.Forms.PlatformConfiguration` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/)名前空間。
1. 追加、`xmlns`宣言または`using`プラットフォーム固有の機能を含む名前空間のディレクティブ。
    1. これは、ios の場合、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)名前空間。
    1. これは、android で、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)名前空間。 これは Android の AppCompat、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)名前空間。
    1. これは、ユニバーサル Windows プラットフォームに、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)名前空間。
1. プラットフォーム固有の適用、XAML またはコードから、 `On<T>` fluent API です。 値`T`できます、 [ `iOS` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOS/)、 [ `Android` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Android/)、または[ `Windows` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Windows/)から型の[ `Xamarin.Forms.PlatformConfiguration`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/)名前空間。

> [!NOTE]
> ここでは使用できませんのプラットフォームでは、プラットフォームに固有の使用を試みたりは発生しないこと、エラーに注意してください。 代わりに、コードが適用されているプラットフォームに応じたせず実行されます。

プラットフォーム固有の使用を通じて、 `On<T>` fluent コード API の戻り値[ `IPlatformElementConfiguration` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IPlatformElementConfiguration%3CTPlatform,TElement%3E/)オブジェクト。 これにより、メソッドの連鎖で同じオブジェクトで呼び出される複数のプラットフォーム固有です。

プラットフォーム固有の詳細については、次を参照してください。[消費してプラットフォーム固有](~/xamarin-forms/platform/platform-specifics/consuming/index.md)と[を作成するプラットフォーム固有](~/xamarin-forms/platform/platform-specifics/creating.md)です。


## <a name="related-links"></a>関連リンク

- [プラットフォーム固有設定の使用](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/)
