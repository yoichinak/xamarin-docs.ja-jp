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
ms.openlocfilehash: 0db20620870340386ccd0cedf7f98cb2975527ba
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128046"
---
# <a name="large-page-titles-on-ios"></a>IOS 上の大きなページタイトル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Ios 11 以上を使用するデバイスでは、この iOS プラットフォーム固有のを使用して、のナビゲーションバーにページタイトルを大きなタイトルとして表示し [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ます。 大きなタイトルが左揃えになり、大きなフォントが使用され、ユーザーがコンテンツのスクロールを開始したときに標準のタイトルに移行して、画面の実際の使用量が効率的に使用されるようにします。 ただし、横方向では、タイトルはナビゲーションバーの中央に戻り、コンテンツレイアウトを最適化します。 添付プロパティを値に設定することにより、XAML で使用 `NavigationPage.PrefersLargeTitles` され `boolean` ます。

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

メソッドは、 `NavigationPage.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `NavigationPage.SetPrefersLargeTitle`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 大きなタイトルが有効かどうかを制御します。

で大きなタイトルが有効になっていると [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 、ナビゲーションスタック内のすべてのページに大きなタイトルが表示されます。 この動作は、 `Page.LargeTitleDisplay` 添付プロパティを列挙体の値に設定することにより、ページでオーバーライドでき `LargeTitleDisplayMode` ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

または、fluent API を使用して C# からページの動作をオーバーライドすることもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

メソッドは、 `Page.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `Page.SetLargeTitleDisplay`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) の大規模なタイトルの動作を制御し [`Page`](xref:Xamarin.Forms.Page) `LargeTitleDisplayMode` ます。列挙体では、次の3つの値を指定できます。

- `Always`–ナビゲーションバーとフォントサイズが大きい形式を使用するように強制します。
- `Automatic`–ナビゲーションスタックの前の項目と同じスタイル (large または small) を使用します。
- `Never`–標準の小さい形式のナビゲーションバーを強制的に使用します。

また、メソッドを `SetLargeTitleDisplay` 使用して、現在のを返すメソッドを呼び出すことにより、列挙値を切り替えることもでき `LargeTitleDisplay` `LargeTitleDisplayMode` ます。

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

結果として、指定された `LargeTitleDisplayMode` がに適用され [`Page`](xref:Xamarin.Forms.Page) 、大きなタイトルの動作を制御します。

![](page-large-title-images/large-title.png "Blur Effect Platform-Specific")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
