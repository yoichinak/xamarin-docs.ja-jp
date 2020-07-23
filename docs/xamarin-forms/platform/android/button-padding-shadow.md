---
title: Android でのボタンの余白と影
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、android のボタンの既定の埋め込み値とシャドウ値を使用する Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: BD2B60D1-DE6E-4691-A777-8EC5F560A4E9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3efcd5cd7c2d00e9d646c937863fd7e55d17f240
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929884"
---
# <a name="button-padding-and-shadows-on-android"></a>Android でのボタンの余白と影

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有 Xamarin.Forms の設定では、ボタンが android ボタンの既定の埋め込み値とシャドウ値を使用するかどうかを制御します。 [`Button.UseDefaultPadding`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty)プロパティと添付プロパティを値に設定することにより、XAML で使用 [`Button.UseDefaultShadow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) され `boolean` ます。

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

メソッドは、 `Button.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 [ `Button.SetUseDefaultPadding` ] (Xref: Xamarin.FormsPlatformConfiguration. AndroidSpecific の. Button. SetUseDefaultPadding ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。Button}、System. Boolean)、および [ `Button.SetUseDefaultShadow` ] (xref: Xamarin.Forms .PlatformConfiguration. AndroidSpecific の. Button. SetUseDefaultShadow ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。Button}, System. Boolean) メソッドを使用して、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Xamarin.Forms ボタンが Android ボタンの既定の埋め込み値とシャドウ値を使用するかどうかを制御します。 また、[ `Button.UseDefaultPadding` ] (xref: Xamarin.FormsPlatformConfiguration. AndroidSpecific の...................... Xamarin.Forms ..IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。ボタン}) と [ `Button.UseDefaultShadow` ] (xref: Xamarin.Forms .PlatformConfiguration. AndroidSpecific の...................... Xamarin.Forms ..IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。Button})) メソッドを使用して、ボタンが既定の埋め込み値と既定の影値をそれぞれ使用するかどうかを返すことができます。

その結果、 Xamarin.Forms ボタンは Android ボタンの既定の埋め込み値とシャドウ値を使用できるようになります。

![Android ボタンの既定の埋め込みと影の値](button-padding-shadow-images/button-padding-and-shadow.png)

上のスクリーンショットでは、 [`Button`](xref:Xamarin.Forms.Button) `Button` Android ボタンの既定の埋め込み値とシャドウ値を使用している点を除いて、それぞれの定義が同じであることに注意してください。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
