---
title: IOS の TabbedPage 半透明タブバー
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、TabbedPage のタブバーの透明度モードを設定する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 9581AE81-9557-47AD-8B07-25A1AC5F055B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f6431e2cbb187cbf89bdccad0ba5de57eed162e1
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93371327"
---
# <a name="tabbedpage-translucent-tab-bar-on-ios"></a>IOS の TabbedPage 半透明タブバー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

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

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)