---
title: IOS での名前付きフォントサイズのアクセシビリティスケーリング
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、名前付きフォントサイズのアクセシビリティスケーリングを無効にする iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/28/2019
ms.openlocfilehash: 678ea2210389eb33bc3a184d758f2bfa07c60bd9
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656732"
---
# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>IOS での名前付きフォントサイズのアクセシビリティスケーリング

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の設定により、名前付きフォントサイズのアクセシビリティスケーリングが無効になります。 XAML で設定して使用される、`Application.EnableAccessibilityScalingForNamedFontSizes`バインド可能なプロパティを`false`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.EnableAccessibilityScalingForNamedFontSizes="false">
    ...
</Application>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetEnableAccessibilityScalingForNamedFontSizes(false);
```

`Application.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間のメソッドは`Application.SetEnableAccessibilityScalingForNamedFontSizes` 、iOS アクセシビリティ設定によってスケーリングされる名前付きフォントサイズを無効にするために使用されます。 また、 `Application.GetEnableAccessibilityScalingForNamedFontSizes`メソッドを使用して、名前付きフォントサイズが iOS アクセシビリティ設定によってスケーリングされているかどうかを返すことができます。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
