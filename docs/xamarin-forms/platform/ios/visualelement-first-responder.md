---
title: IOS 上の VisualElement の最初のレスポンダー
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、VisualElement オブジェクトを使用してイベントをタッチする最初のレスポンダーにする、iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 3A77BA02-B87A-44EC-AC51-9D3130EF314C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d8bd539c2bb0e8963afae3392b6f8e99d79af9af
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136970"
---
# <a name="visualelement-first-responder-on-ios"></a>IOS 上の VisualElement の最初のレスポンダー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の機能を使用すると、 [`VisualElement`](xref:Xamarin.Forms.VisualElement) オブジェクトは、要素を含むページではなく、最初の応答側の応答者になることができます。 これは、バインド可能なプロパティをに設定することによって XAML で使用され `VisualElement.CanBecomeFirstResponder` `true` ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry Placeholder="Enter text" />
        <Button ios:VisualElement.CanBecomeFirstResponder="True"
                Text="OK" />
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Entry entry = new Entry { Placeholder = "Enter text" };
Button button = new Button { Text = "OK" };
button.On<iOS>().SetCanBecomeFirstResponder(true);
```

メソッドは、 `VisualElement.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `VisualElement.SetCanBecomeFirstResponder`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) `VisualElement` タッチイベントの最初の応答側になるようにを設定するために使用されます。 また、メソッドを `VisualElement.CanBecomeFirstResponder` 使用して、 `VisualElement` がイベントをタッチする最初の応答側であるかどうかを返すことができます。

結果として、は、 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 要素を含むページではなく、タッチイベントの最初の応答側になることができます。 これにより、がタップされたときに、チャットアプリケーションなどのシナリオでキーボードが終了しないようにすること [`Button`](xref:Xamarin.Forms.Button) ができます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
