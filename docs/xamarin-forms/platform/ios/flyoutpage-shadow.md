---
title: IOS 上の FlyoutPage シャドウ
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、FlyoutPage の詳細ページに影が適用されているかどうかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: FB907EA2-C00A-43A5-84B1-A43584C867A5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0b05cb37a2399f438b9003e39341feb138bce490
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940803"
---
# <a name="flyoutpage-shadow-on-ios"></a>IOS 上の FlyoutPage シャドウ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

このプラットフォーム固有の設定は、の詳細ページに影が適用されているかどうかを制御します。このページには、 [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) フライアウトページがあります。 これは、バインド可能なプロパティをに設定することによって XAML で使用され `FlyoutPage.ApplyShadow` `true` ます。

```xaml
<FlyoutPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                  ios:FlyoutPage.ApplyShadow="true">
    ...
</FlyoutPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSFlyoutPageCS : FlyoutPage
{
    public iOSFlyoutPageCS(ICommand restore)
    {
        On<iOS>().SetApplyShadow(true);
        // ...
    }
}
```

メソッドは、 `FlyoutPage.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 名前空間のメソッドを使用して、 `FlyoutPage.SetApplyShadow` [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) の詳細ページに影が適用されているかどうかを制御し [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) ます (フライアウトページが公開されている場合)。 また、メソッドを `GetApplyShadow` 使用して、の詳細ページに影が適用されているかどうかを判断することもでき `FlyoutPage` ます。

その結果、の [詳細] ページに [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) 影が適用され、フライアウトページが消えます。

[![影付きおよび影なしの FlyoutPage のスクリーンショット](flyoutpage-shadow-images/shadow.png "シャドウなしの FlyoutPage")](flyoutpage-shadow-images/shadow-large.png#lightbox "シャドウなしの FlyoutPage")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
