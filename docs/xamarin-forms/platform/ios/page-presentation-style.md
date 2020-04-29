---
title: IOS のモーダルページプレゼンテーションスタイル
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、iOS プラットフォーム固有の設定を使用して、モーダルページのプレゼンテーションスタイルを設定する方法について説明します。
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
ms.openlocfilehash: 5078b280499929e0e2e3691539cf1927b4c79fe7
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517535"
---
# <a name="modal-page-presentation-style-on-ios"></a>IOS のモーダルページプレゼンテーションスタイル

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、モーダルページのプレゼンテーションスタイルを設定するために使用されます。 これは、バインド可能な`Page.ModalPresentationStyle` `UIModalPresentationStyle`プロパティを列挙値に設定することによって XAML で使用されます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="FormSheet">
    ...
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.FormSheet);
        ...
    }
}
```

メソッド`Page.On<iOS>`は、このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 名前空間のメソッド`Page.SetModalPresentationStyle`は、次`UIModalPresentationStyle`の列挙値のいずれかを指定すること[`Page`](xref:Xamarin.Forms.Page)によって、のモーダルプレゼンテーションスタイルを設定するために使用されます。 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)

- `FullScreen`。画面全体を囲むようにモーダルプレゼンテーションスタイルを設定します。 既定では、このプレゼンテーションスタイルを使用してモーダルページが表示されます。
- `FormSheet`。画面の中央に配置されるモーダルプレゼンテーションスタイルを設定します。
- `Automatic`。モーダルプレゼンテーションスタイルを、システムによって選択された既定値に設定します。 ほとんどのビューコントローラーで`UIKit`は、が`UIModalPresentationStyle.PageSheet`にマップされますが、システムビューコントローラーによっては別のスタイルにマップされる場合があります。
- `OverFullScreen`。画面を覆うようにモーダルプレゼンテーションスタイルを設定します。
- `PageSheet`。基になるコンテンツに対応するモーダルプレゼンテーションスタイルを設定します。

また、 `GetModalPresentationStyle`メソッドを使用して、 `UIModalPresentationStyle` [`Page`](xref:Xamarin.Forms.Page)に適用されている列挙体の現在の値を取得することもできます。

結果として、のモーダルプレゼンテーションスタイルを[`Page`](xref:Xamarin.Forms.Page)設定できます。

[![](page-presentation-style-images/modal-presentation-style-small.png "Modal Presentation Styles")](page-presentation-style-images/modal-presentation-style-large.png#lightbox "Modal Presentation Styles")

> [!NOTE]
> このプラットフォーム固有のを使用してモーダル表示スタイルを設定するページでは、モーダルナビゲーションを使用する必要があります。 詳細については、「 [Xamarin. フォームモーダルページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
