---
title: IOS の安全な領域レイアウトガイド
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ios プラットフォーム固有のを使用する方法について説明します。これにより、iOS 11 以上を使用するすべてのデバイスで安全なページコンテンツが画面の領域に配置されるようになります。
ms.prod: xamarin
ms.assetid: 2B6789C1-39B4-4C16-ADE1-3ED3378EAC63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f5907e5493ff43c6a69dc4a8a8e5d2b4fc78210e
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374876"
---
# <a name="safe-area-layout-guide-on-ios"></a>IOS の安全な領域レイアウトガイド

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、iOS 11 以上を使用するすべてのデバイスで安全なページコンテンツが画面の領域に配置されるようにするために使用されます。 具体的には、デバイスの角の丸み、ホームインジケーター、または iPhone X のセンサーハウジングによってコンテンツがクリップされないようにすることができます。添付プロパティを値に設定することにより、XAML で使用 `Page.UseSafeArea` され `boolean` ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

メソッドは、 `Page.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `Page.SetUseSafeArea`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) セーフ領域レイアウトガイドを有効にするかどうかを制御します。

結果として、ページコンテンツは、すべての iphone で安全な画面の領域に配置できるようになります。

[![安全領域レイアウト ガイド](page-safe-area-images/safe-area-layout.png)](page-safe-area-images/safe-area-layout-large.png#lightbox "安全領域レイアウト ガイド")

> [!NOTE]
> Apple によって定義された安全な領域は、プロパティを設定するためにで使用され、 Xamarin.Forms [`Page.Padding`](xref:Xamarin.Forms.Page.Padding) このプロパティに設定されている以前の値をすべてオーバーライドします。

セーフ領域をカスタマイズするには、 [`Thickness`](xref:Xamarin.Forms.Thickness) `Page.SafeAreaInsets` 名前空間からメソッドを使用して値を取得し [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ます。 次に、必要に応じて変更し、オーバーライドのプロパティに再割り当てでき `Padding` [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) ます。

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)