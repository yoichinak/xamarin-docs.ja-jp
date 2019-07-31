---
title: Windows での InputView の読み取り順序
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、双方向テキストの読み取り順序を動的に検出できるようにする、Windows プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: E61BAEE0-C8B7-4F33-8DDC-FA1B9CA8E81D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: c184424a982aa82712685dbc33ad57422f2f8338
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651422"
---
# <a name="inputview-reading-order-on-windows"></a>Windows での InputView の読み取り順序

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

このユニバーサル Windows プラットフォームプラットフォーム固有であるため、、 [`Entry`](xref:Xamarin.Forms.Entry) [`Editor`](xref:Xamarin.Forms.Editor)、および[`Label`](xref:Xamarin.Forms.Label)の各インスタンスで双方向テキストの読み取り順序 (左から右または右から左) を動的に検出できます。 XAML で設定して使用される、 [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (の`Entry`と`Editor`インスタンス) または[ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty)添付プロパティを`boolean`値。

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>`メソッドは、このプラットフォームに固有はユニバーサル Windows プラットフォームでのみ実行されるを指定します。 [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)のコンテンツからの読み取り順序が検出されたかどうか、名前空間を使用して、 [ `InputView` ](xref:Xamarin.Forms.InputView). さらに、`InputView.SetDetectReadingOrderFromContent`メソッドを使用して呼び出すことによって、コンテンツからの読み取り順序が検出されたかどうかを切り替える、 [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView}))を現在の値を返すメソッド。

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

その結果、 [ `Entry` ](xref:Xamarin.Forms.Entry)、 [ `Editor` ](xref:Xamarin.Forms.Editor)、および[ `Label` ](xref:Xamarin.Forms.Label)インスタンスが動的に検出されたコンテンツの読み取り順序を持つことができます。

[![プラットフォーム固有のコンテンツからの読み取り順序を検出する InputView](inputview-reading-order-images/editor-readingorder.png "InputView プラットフォームに固有のコンテンツからの読み取り順序を検出する")](inputview-reading-order-images/editor-readingorder-large.png#lightbox "InputView からの読み取り順序を検出します。プラットフォーム固有のコンテンツ")

> [!NOTE]
> 設定とは異なり、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティ、ビューのテキスト コンテンツからの読み取り順序では、ビュー内のテキストの配置には影響は検出ロジック。 代わりでの双方向テキスト ブロックはレイアウトの順序を調整します。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
