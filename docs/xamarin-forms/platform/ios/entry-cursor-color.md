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
ms.openlocfilehash: 4d934fd2155a6a088dd543658555bf104b38f302
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138542"
---
# <a name="entry-cursor-color-on-ios"></a>IOS のエントリカーソルの色

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、のカーソルの色 [`Entry`](xref:Xamarin.Forms.Entry) を、指定された色に設定します。 これは、バインド可能なプロパティをに設定することによって XAML で使用され [`Entry.CursorColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.CursorColorProperty) [`Color`](xref:Xamarin.Forms.Color) ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry ... ios:Entry.CursorColor="LimeGreen" />
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var entry = new Xamarin.Forms.Entry();
entry.On<iOS>().SetCursorColor(Color.LimeGreen);
```

メソッドは、 `Entry.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [ `Entry.SetCursorColor` ] (Xref: Xamarin.FormsPlatformConfiguration. iOSSpecific の. Setカーソル色 ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。Entry Xamarin.Forms }、Color)) メソッドを [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間で、カーソルの色を指定したに設定 [`Color`](xref:Xamarin.Forms.Color) します。 また、[ `Entry.GetCursorColor` ] (xref: Xamarin.FormsPlatformConfiguration. iOSSpecific の. Getカーソル色 ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。Entry})) メソッドを使用して、現在のカーソルの色を取得できます。

結果として、のカーソルの色を [`Entry`](xref:Xamarin.Forms.Entry) 特定のに設定でき [`Color`](xref:Xamarin.Forms.Color) ます。

![](entry-cursor-color-images/entry-cursorcolor.png "Entry Cursor Color")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
