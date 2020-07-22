---
title: IOS 上の Masterのページシャドウ
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、マスターページが明らかになったときに、Masterdetail ページの詳細ページに影が適用されているかどうかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: FB907EA2-C00A-43A5-84B1-A43584C867A5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: aaf94536d41da47aec10fc655f9d053b753da5a2
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84135968"
---
# <a name="masterdetailpage-shadow-on-ios"></a>IOS 上の Masterのページシャドウ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

このプラットフォーム固有の設定により、の詳細ページに影が適用されているかどうかを制御し [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) ます (マスターページが公開されている場合)。 これは、バインド可能なプロパティをに設定することによって XAML で使用され `MasterDetailPage.ApplyShadow` `true` ます。

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                  ios:MasterDetailPage.ApplyShadow="true">
    ...
</MasterDetailPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSMasterDetailPageCS : MasterDetailPage
{
    public iOSMasterDetailPageCS(ICommand restore)
    {
        On<iOS>().SetApplyShadow(true);
        // ...
    }
}
```

メソッドは、 `MasterDetailPage.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `MasterDetailPage.SetApplyShadow`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) マスターページを公開するときに、の詳細ページ [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) に影が適用されているかどうかを制御するために使用されます。 また、メソッドを `GetApplyShadow` 使用して、の詳細ページに影が適用されているかどうかを判断することもでき `MasterDetailPage` ます。

結果として、マスターページが明らかになったときに、の [詳細] ページ [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) に影が適用される可能性があります。

[![シャドウなしの Masterのページのスクリーンショット](masterdetailpage-shadow-images/shadow.png "シャドウの有無に関係なく Masterのページ")](masterdetailpage-shadow-images/shadow-large.png#lightbox "シャドウの有無に関係なく Masterのページ")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
