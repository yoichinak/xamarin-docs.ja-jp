---
title: IOS 上の VisualElement の最初のレスポンダー
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、VisualElement オブジェクトを使用してイベントをタッチする最初のレスポンダーにする、iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 3A77BA02-B87A-44EC-AC51-9D3130EF314C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: be6c233b63d172d2fcacb1cea7f5e9aeeb7faed1
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646714"
---
# <a name="visualelement-first-responder-on-ios"></a>IOS 上の VisualElement の最初のレスポンダー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の機能により、 [`VisualElement`](xref:Xamarin.Forms.VisualElement)オブジェクトは、要素を含むページではなく、最初の応答側の応答者になることができます。 XAML では、`VisualElement.CanBecomeFirstResponder` バインド可能なプロパティを `true`に設定することによって使用されます。

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

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Entry entry = new Entry { Placeholder = "Enter text" };
Button button = new Button { Text = "OK" };
button.On<iOS>().SetCanBecomeFirstResponder(true);
```

`VisualElement.On<iOS>` メソッドは、このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間の `VisualElement.SetCanBecomeFirstResponder` メソッドを使用して、タッチイベントの最初の応答側になるように `VisualElement` を設定します。 さらに、`VisualElement.CanBecomeFirstResponder` メソッドを使用して、`VisualElement` がイベントをタッチする最初の応答側であるかどうかを返すことができます。

結果として、要素を含むページではなく、 [`VisualElement`](xref:Xamarin.Forms.VisualElement)がタッチイベントの最初の応答側になることがあります。 これにより、 [`Button`](xref:Xamarin.Forms.Button)がタップされたときに、チャットアプリケーションなどのシナリオでキーボードが終了しないようにすることができます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
