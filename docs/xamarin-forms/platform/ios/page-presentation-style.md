---
title: IOS のモーダルページプレゼンテーションスタイル
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、モーダルページのプレゼンテーションスタイルを設定する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c522b1e9c191a4f030355f0cfcca1240d64e09df
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373706"
---
# <a name="modal-page-presentation-style-on-ios"></a>IOS のモーダルページプレゼンテーションスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、モーダルページのプレゼンテーションスタイルを設定するために使用され、さらに、透明な背景を持つモーダルページを表示するために使用できます。 これは、 `Page.ModalPresentationStyle` バインド可能なプロパティを列挙値に設定することによって XAML で使用され `UIModalPresentationStyle` ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="OverFullScreen">
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
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.OverFullScreen);
        ...
    }
}
```

メソッドは、 `Page.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `Page.SetModalPresentationStyle`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) [`Page`](xref:Xamarin.Forms.Page) 次の列挙値のいずれかを指定することによって、のモーダルプレゼンテーションスタイルを設定するために使用され `UIModalPresentationStyle` ます。

- `FullScreen`。画面全体を囲むようにモーダルプレゼンテーションスタイルを設定します。 既定では、このプレゼンテーションスタイルを使用してモーダルページが表示されます。
- `FormSheet`。画面の中央に配置されるモーダルプレゼンテーションスタイルを設定します。
- `Automatic`。モーダルプレゼンテーションスタイルを、システムによって選択された既定値に設定します。 ほとんどのビューコントローラーでは、が `UIKit` にマップさ `UIModalPresentationStyle.PageSheet` れますが、システムビューコントローラーによっては別のスタイルにマップされる場合があります。
- `OverFullScreen`。画面を覆うようにモーダルプレゼンテーションスタイルを設定します。
- `PageSheet`。基になるコンテンツに対応するモーダルプレゼンテーションスタイルを設定します。

また、メソッドを `GetModalPresentationStyle` 使用して `UIModalPresentationStyle` 、に適用されている列挙体の現在の値を取得することもでき [`Page`](xref:Xamarin.Forms.Page) ます。

結果として、のモーダルプレゼンテーションスタイルを [`Page`](xref:Xamarin.Forms.Page) 設定できます。

[![モーダルプレゼンテーションスタイル](page-presentation-style-images/modal-presentation-style-small.png)](page-presentation-style-images/modal-presentation-style-large.png#lightbox "モーダルプレゼンテーションスタイル")

> [!NOTE]
> このプラットフォーム固有のを使用してモーダル表示スタイルを設定するページでは、モーダルナビゲーションを使用する必要があります。 詳細については、「 [ Xamarin.Forms モーダルページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)