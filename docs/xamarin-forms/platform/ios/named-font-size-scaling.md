---
title: IOS でのフォント サイズのという名前のユーザー補助のスケーリング
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、ユーザー補助の名前付きのフォント サイズのスケーリングを無効にします。 iOS プラットフォームに固有の使用方法について説明します。
ms.prod: xamarin
ms.assetid: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/28/2019
ms.openlocfilehash: 1fc6fefe1f9fe48fe2abb367b376a5a6f3484462
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2019
ms.locfileid: "67517951"
---
# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>IOS でのフォント サイズのという名前のユーザー補助のスケーリング

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

この iOS プラットフォームに固有では、ユーザー補助の名前付きのフォント サイズのスケーリングを無効にします。 XAML で設定して使用される、`Application.EnableAccessibilityScalingForNamedFontSizes`バインド可能なプロパティを`false`:

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

`Application.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 `Application.SetEnableAccessibilityScalingForNamedFontSizes`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間は iOS のユーザー補助の設定でスケーリングされている名前付きのフォント サイズを無効にするために使用します。 さらに、`Application.GetEnableAccessibilityScalingForNamedFontSizes`メソッドを使用して、iOS ユーザー補助の設定で名前付きのフォント サイズをスケールするかどうかを返します。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
