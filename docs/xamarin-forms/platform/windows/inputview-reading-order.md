---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f5f0bcdc2d2c8eb1b51ad8dcd1014c649af80c90
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137762"
---
# <a name="inputview-reading-order-on-windows"></a>Windows での InputView の読み取り順序

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

このユニバーサル Windows プラットフォームプラットフォーム固有であるため、、、およびの各インスタンスで双方向テキストの読み取り順序 (左から右または右から左) [`Entry`](xref:Xamarin.Forms.Entry) [`Editor`](xref:Xamarin.Forms.Editor) [`Label`](xref:Xamarin.Forms.Label) を動的に検出できます。 XAML で [`InputView.DetectReadingOrderFromContent`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) は、( `Entry` および `Editor` インスタンス) または添付プロパティを値に設定することによって使用 [`Label.DetectReadingOrderFromContent`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) され `boolean` ます。

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>`メソッドは、このプラットフォーム固有のがユニバーサル Windows プラットフォームでのみ実行されることを指定します。 [ `InputView.SetDetectReadingOrderFromContent` ] (Xref: Xamarin.FormsPlatformConfiguration. SetDetectReadingOrderFromContent (を参照してください。 Xamarin.FormsIPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。InputView}, System. Boolean) メソッドは、の [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) コンテンツから読み取り順序が検出されるかどうかを制御するために、名前空間で使用され [`InputView`](xref:Xamarin.Forms.InputView) ます。 また、メソッドを使用すると、 `InputView.SetDetectReadingOrderFromContent` [ `InputView.GetDetectReadingOrderFromContent` ] (xref: を呼び出すことによって、コンテンツから読み取り順序が検出されるかどうかを切り替えることができます Xamarin.Forms 。PlatformConfiguration. GetDetectReadingOrderFromContent (を参照してください。 Xamarin.FormsIPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。InputView})) メソッドを取得し、現在の値を返します。

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

結果として [`Entry`](xref:Xamarin.Forms.Entry) 、、 [`Editor`](xref:Xamarin.Forms.Editor) 、およびの [`Label`](xref:Xamarin.Forms.Label) 各インスタンスは、コンテンツの読み取り順序を動的に検出できます。

[![コンテンツプラットフォーム固有の読み取り順序を検出する InputView](inputview-reading-order-images/editor-readingorder.png "コンテンツプラットフォーム固有の読み取り順序を検出する InputView")](inputview-reading-order-images/editor-readingorder-large.png#lightbox "コンテンツプラットフォーム固有の読み取り順序を検出する InputView")

> [!NOTE]
> プロパティの設定とは異なり [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 、テキストコンテンツからの読み取り順序を検出するビューのロジックは、ビュー内のテキストの配置には影響しません。 代わりに、双方向テキストのブロックのレイアウト順序を調整します。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
