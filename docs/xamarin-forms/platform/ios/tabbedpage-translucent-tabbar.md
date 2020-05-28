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
ms.openlocfilehash: 8127191aab80d81fc2e532e3d5e14931b834eeae
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137034"
---
# <a name="tabbedpage-translucent-tab-bar-on-ios"></a>IOS の TabbedPage 半透明タブバー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、のタブバーの透明度モードを設定するために使用され [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ます。 これは、 `TabbedPage.TranslucencyMode` バインド可能なプロパティを列挙値に設定することによって XAML で使用され `TranslucencyMode` ます。

```xaml
<TabbedPage ...
            xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
            ios:TabbedPage.TranslucencyMode="Opaque">
    ...
</TabbedPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetTranslucencyMode(TranslucencyMode.Opaque);
```

メソッドは、 `TabbedPage.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `TabbedPage.SetTranslucencyMode`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 次の列挙値のいずれかを指定することによって、のタブバーの透明度モードを設定するために使用され `TranslucencyMode` ます。

- `Default`。タブバーを既定の透明度モードに設定します。 これは、`TabbedPage.TranslucencyMode` プロパティの既定値です。
- `Translucent`。タブバーが半透明になるように設定します。
- `Opaque`。タブバーが不透明になるように設定します。

また、メソッドを `GetTranslucencyMode` 使用して `TranslucencyMode` 、に適用されている列挙体の現在の値を取得することもでき [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ます。

結果として、のタブバーの透明度モードは次のように [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 設定できます。

![IOS の半透明および不透明なタブバーのスクリーンショット](tabbedpage-translucent-tabbar-images/translucencymodes.png "透明および不透明なタブバー")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
