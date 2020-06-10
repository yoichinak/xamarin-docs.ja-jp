---
title: "iOS での名前付きフォントサイズのアクセシビリティスケーリング" 説明: "プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装しなくても、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、名前付きフォントサイズのアクセシビリティスケーリングを無効にする iOS プラットフォーム固有のを使用する方法について説明します。
ms. 製品: xamarin ms. assetid: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 06/28/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>IOS での名前付きフォントサイズのアクセシビリティスケーリング

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の設定により、名前付きフォントサイズのアクセシビリティスケーリングが無効になります。 これは、バインド可能なプロパティをに設定することによって XAML で使用され `Application.EnableAccessibilityScalingForNamedFontSizes` `false` ます。

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.EnableAccessibilityScalingForNamedFontSizes="false">
    ...
</Application>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetEnableAccessibilityScalingForNamedFontSizes(false);
```

メソッドは、 `Application.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `Application.SetEnableAccessibilityScalingForNamedFontSizes`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) iOS アクセシビリティ設定によってスケーリングされる名前付きフォントサイズを無効にするために使用されます。 また、メソッドを `Application.GetEnableAccessibilityScalingForNamedFontSizes` 使用して、名前付きフォントサイズが iOS アクセシビリティ設定によってスケーリングされているかどうかを返すことができます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
