---
title: iPad モーダル ページの表示スタイル
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、iOS プラットフォーム固有のを使用して、iPad 上のモーダルページのプレゼンテーションスタイルを設定する方法について説明します。
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: c8962c46e0b496844bd3fc00346a117b6753f818
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651946"
---
# <a name="ipad-modal-page-presentation-style"></a>iPad モーダル ページの表示スタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、iPad のモーダルページのプレゼンテーションスタイルを設定するために使用されます。 XAML で設定して使用される、`Page.ModalPresentationStyle`バインド可能なプロパティを`UIModalPresentationStyle`列挙値。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="FormSheet">
    ...
</ContentPage>
```

代わりに、fluent API を使用して C# から使用できます。

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

`Page.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 `Page.SetModalPresentationStyle`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)でモーダル プレゼンテーションのスタイルを設定する名前空間が使用される、 [ `Page` ](xref:Xamarin.Forms.Page) 、次のいずれかを指定することによって`UIModalPresentationStyle`列挙型値:

- `FullScreen`、画面全体を網羅するモーダル表示スタイルを設定します。 既定では、このプレゼンテーションのスタイルを使用してモーダル ページが表示されます。
- `FormSheet`、モーダル プレゼンテーションのスタイルを中心とした、画面より小さいを設定します。

さらに、`GetModalPresentationStyle`の現在の値を取得するメソッドを使用することができます、`UIModalPresentationStyle`列挙型に適用される、 [ `Page`](xref:Xamarin.Forms.Page)します。

その結果にモーダル プレゼンテーション スタイル、 [ `Page` ](xref:Xamarin.Forms.Page)設定できます。

[![](page-presentation-style-images/modal-presentation-style-small.png "IPad でのモーダル スタイル")](page-presentation-style-images/modal-presentation-style-large.png#lightbox "iPad でモーダルの表示スタイル")

> [!NOTE]
> モーダルの表示スタイルを設定するプラットフォームに固有のこのを使用するページには、モーダル ナビゲーションを使用する必要があります。 詳細については、[Xamarin.Forms のモーダル ページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)を参照してください。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
