---
title: IOS の TabbedPage 半透明タブバー
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、TabbedPage のタブバーの透明度モードを設定する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 9581AE81-9557-47AD-8B07-25A1AC5F055B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: 55fda0be2e260c5aa4a34ab2dcc1ac3cac33b92a
ms.sourcegitcommit: 6c60914b380ff679bbffd7790edd4d5e18005d0a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2020
ms.locfileid: "80070285"
---
# <a name="tabbedpage-translucent-tab-bar-on-ios"></a>IOS の TabbedPage 半透明タブバー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)のタブバーの透明度モードを設定するために使用されます。 `TabbedPage.TranslucencyMode` バインド可能なプロパティを `TranslucencyMode` 列挙値に設定することにより、XAML で使用されます。

```xaml
<TabbedPage ...
            xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
            ios:TabbedPage.TranslucencyMode="Opaque">
    ...
</TabbedPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetTranslucencyMode(TranslucencyMode.Opaque);
```

`TabbedPage.On<iOS>` メソッドは、このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間の `TabbedPage.SetTranslucencyMode` メソッドを使用して、次の `TranslucencyMode` 列挙値のいずれかを指定することによって、 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)のタブバーの透明度モードを設定します。

- `Default`、タブバーを既定の透明度モードに設定します。 これは、`TabbedPage.TranslucencyMode` プロパティの既定値です。
- `Translucent`。タブバーが半透明になるように設定します。
- `Opaque`。タブバーが不透明になるように設定します。

また、`GetTranslucencyMode` メソッドを使用して、 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)に適用されている `TranslucencyMode` 列挙体の現在の値を取得することもできます。

結果として、 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)のタブバーの透明度モードを設定できます。

![IOS の半透明および不透明なタブバーのスクリーンショット](tabbedpage-translucent-tabbar-images/translucencymodes.png "透明および不透明なタブバー")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
