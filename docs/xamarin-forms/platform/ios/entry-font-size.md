---
title: IOS のエントリのフォントサイズ
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、エントリのフォントサイズをスケールする iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: E8881D4E-902B-4397-A43E-916B2885EC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a3f5b920a3717bb70454eb16a5f830e6472413eb
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373186"
---
# <a name="entry-font-size-on-ios"></a>IOS のエントリのフォントサイズ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のを使用して、のフォントサイズを拡大縮小し、 [`Entry`](xref:Xamarin.Forms.Entry) 入力テキストがコントロールに収まるようにします。 添付プロパティを値に設定することにより、XAML で使用 [`Entry.AdjustsFontSizeToFitWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty) され `boolean` ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

メソッドは、 `Entry.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [ `Entry.EnableAdjustsFontSizeToFitWidth` ] (Xref: Xamarin.FormsPlatformConfiguration. iOSSpecific Xamarin.Forms ()。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。Entry})) メソッドを名前空間で使用して、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 入力テキストのフォントサイズをに拡張し、に収まるようにし [`Entry`](xref:Xamarin.Forms.Entry) ます。 さらに、 [`Entry`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry) 名前空間のクラスに `Xamarin.Forms.PlatformConfiguration.iOSSpecific` も [ `DisableAdjustsFontSizeToFitWidth` ] (xref: があり Xamarin.Forms ます。PlatformConfiguration. iOSSpecific Xamarin.Forms ()。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。Entry})) このプラットフォーム固有の、および a [ `SetAdjustsFontSizeToFitWidth` ] (xref: を無効にし Xamarin.Forms ます。PlatformConfiguration. iOSSpecific Xamarin.Forms ()。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。Entry}, Boolean)) メソッドを呼び出して、フォントサイズのスケーリングを切り替えるために使用することができます `AdjustsFontSizeToFitWidth` Xamarin.Forms 。PlatformConfiguration. iOSSpecific Xamarin.Forms ()。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。Entry}) メソッド:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

結果として、のフォントサイズは、 [`Entry`](xref:Xamarin.Forms.Entry) 入力テキストがコントロールに収まるようにスケーリングされます。

![エントリのフォントサイズを調整 Platform-Specific](entry-font-size-images/entry-font-size.png)

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)