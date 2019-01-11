---
title: Android で VisualElement レガシ カラー モード
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、Android プラットフォーム固有 Xamarin.Forms の従来のカラー モードを無効にするを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: c04cb4a48a984ed9854ae791e554d33b665241ea
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2019
ms.locfileid: "54208747"
---
# <a name="visualelement-legacy-color-mode-on-android"></a>Android で VisualElement レガシ カラー モード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Xamarin.Forms のビューのいくつかの機能の色をレガシ モード。 このモードでは、ときに、 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled)ビューのプロパティに設定されて`false`ビューが既定のネイティブ色無効の状態を使用してユーザー設定の色をオーバーライドします。 旧バージョンと互換性のため、この色のレガシ モードでは、サポートされているビューの既定の動作は残ります。

この Android プラットフォームに固有では、ビューが無効になっている場合でも、ユーザーがビューの設定の色が維持されるようにこの色のレガシ モードが無効にします。 XAML で設定して使用される、 [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty)添付プロパティを`false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間を使用してレガシのカラー モードが無効になっているかどうか。 さらに、 [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement}))を従来のカラー モードが無効になっているかどうかを返すメソッドを使用できます。

結果は、ことは、従来のカラー モードを無効にできます、ように、ユーザーがビューの設定の色もビューが無効になっています。

![](legacy-color-mode-images/legacy-color-mode-disabled.png "従来のカラー モードを無効になっています")

> [!NOTE]
> 設定するときに、 [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup)レガシ カラー モードが完全に無視されます、ビューにします。 表示状態の詳細については、次を参照してください。 [、Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)します。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
