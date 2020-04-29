---
title: IOS 上の Masterのページシャドウ
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、マスターページが明らかになったときに、Masterdetail ページの詳細ページに影が適用されているかどうかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: FB907EA2-C00A-43A5-84B1-A43584C867A5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
ms.openlocfilehash: eaa82b5accae73e4c6e8eb8d3fbcc873fc111be9
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82532959"
---
# <a name="masterdetailpage-shadow-on-ios"></a>IOS 上の Masterのページシャドウ

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

このプラットフォーム固有の設定により、 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)の詳細ページに影が適用されているかどうかを制御します (マスターページが公開されている場合)。 これは、バインド可能なプロパティを`MasterDetailPage.ApplyShadow`に設定する`true`ことによって XAML で使用されます。

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

メソッド`MasterDetailPage.On<iOS>`は、このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 名前空間のメソッドは`MasterDetailPage.SetApplyShadow` 、マスターページを公開するときに、 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)の詳細ページに影が適用されているかどうかを制御するために使用されます。 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) また、 `GetApplyShadow`メソッドを使用して、 `MasterDetailPage`の詳細ページに影が適用されているかどうかを判断することもできます。

結果として、マスターページが明らか[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)になったときに、の [詳細] ページに影が適用される可能性があります。

[![シャドウなしの Masterのページのスクリーンショット](masterdetailpage-shadow-images/shadow.png "シャドウの有無に関係なく Masterのページ")](masterdetailpage-shadow-images/shadow-large.png#lightbox "シャドウの有無に関係なく Masterのページ")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
