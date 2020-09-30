---
title: Android での VisualElement レガシカラーモード
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、従来の色モードを無効にする Android プラットフォーム固有のを使用する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ca8fdf101d521bf5cc58edecc523a035577be09b
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563368"
---
# <a name="visualelement-legacy-color-mode-on-android"></a>Android での VisualElement レガシカラーモード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

一部の Xamarin.Forms ビューでは、従来のカラーモードが機能します。 このモードで [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) は、ビューのプロパティがに設定されている場合、 `false` ユーザーによって設定された色が無効状態の既定のネイティブ色で上書きされます。 旧バージョンとの互換性を維持するために、このレガシカラーモードはサポートされているビューの既定の動作のままです。

この Android プラットフォーム固有の設定により、この従来の色のモードが無効になるため、ビューが無効になっている場合でも、ユーザーによって表示される色は変わりません。 添付プロパティをに設定することにより、XAML で使用 [`VisualElement.IsLegacyColorModeEnabled`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) され `false` ます。

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

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

メソッドは、 `VisualElement.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 [ `VisualElement.SetIsLegacyColorModeEnabled` ] (Xref: Xamarin.FormsPlatformConfiguration. AndroidSpecific Xamarin.Forms ()IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。VisualElement}, system.string) メソッドは、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) レガシカラーモードが無効になっているかどうかを制御するために、名前空間で使用されます。 また、[ `VisualElement.GetIsLegacyColorModeEnabled` ] (xref: Xamarin.FormsPlatformConfiguration. AndroidSpecific Xamarin.Forms ()IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。VisualElement})) メソッドを使用して、レガシカラーモードが無効になっているかどうかを返すことができます。

その結果、レガシ色モードを無効にして、ビューが無効になっている場合でも、ユーザーがビューに設定した色をそのまま使用できるようになります。

![レガシカラーモードが無効です](legacy-color-mode-images/legacy-color-mode-disabled.png)

> [!NOTE]
> ビューにを設定すると、 [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) 従来の色モードは完全に無視されます。 表示状態の詳細については、「 [ Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)