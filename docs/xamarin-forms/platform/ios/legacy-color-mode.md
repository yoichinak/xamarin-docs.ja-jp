---
title: "VisualElement レガシカラーモード (iOS)" 説明: "プラットフォーム-詳細" を使用すると、カスタムレンダラーや特殊効果を実装しなくても、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、従来のカラーモードを無効にする iOS プラットフォーム固有のを使用する方法について説明 Xamarin.Forms します。
ms. 製品: xamarin ms. assetid: 60FFBA67-6E06-439B-A5EB-8C808285E2CD: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 10/24/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="visualelement-legacy-color-mode-on-ios"></a>IOS の VisualElement レガシカラーモード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

一部の Xamarin.Forms ビューでは、従来のカラーモードが機能します。 このモードで [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) は、ビューのプロパティがに設定されている場合、 `false` ユーザーによって設定された色が無効状態の既定のネイティブ色で上書きされます。 旧バージョンとの互換性を維持するために、このレガシカラーモードはサポートされているビューの既定の動作のままです。

この iOS プラットフォーム固有のでは、ではこのレガシカラーモードが無効になって [`VisualElement`](xref:Xamarin.Forms.VisualElement) いるため、ビューが無効になっている場合でも、ユーザーがビューに設定した色はそのまま残ります。 添付プロパティをに設定することにより、XAML で使用 [`VisualElement.IsLegacyColorModeEnabled`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsLegacyColorModeEnabledProperty) され `false` ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                ios:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

_legacyColorModeDisabledButton.On<iOS>().SetIsLegacyColorModeEnabled(false);
```

メソッドは、 `VisualElement.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [ `VisualElement.SetIsLegacyColorModeEnabled` ] (Xref: Xamarin.FormsPlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。VisualElement}, system.string) メソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) レガシカラーモードが無効になっているかどうかを制御するために、名前空間で使用されます。 また、[ `VisualElement.GetIsLegacyColorModeEnabled` ] (xref: Xamarin.FormsPlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。VisualElement})) メソッドを使用して、レガシカラーモードが無効になっているかどうかを返すことができます。

その結果、レガシ色モードを無効にして、ビューが無効になっている場合でも、ユーザーがビューに設定した色をそのまま使用できるようになります。

![](legacy-color-mode-images/legacy-color-mode-disabled.png "Legacy color mode disabled")

> [!NOTE]
> ビューにを設定すると、 [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) 従来の色モードは完全に無視されます。 表示状態の詳細については、「 [ Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
