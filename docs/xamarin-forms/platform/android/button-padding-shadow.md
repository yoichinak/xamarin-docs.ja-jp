---
title: ボタンの余白と Android でのシャドウ
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、Android プラットフォーム固有既定のパディングと Android のボタンの影の値を使用するを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: BD2B60D1-DE6E-4691-A777-8EC5F560A4E9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: bcdea14ba6a4a9a09a7b35db67794709d6d36c4c
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/06/2019
ms.locfileid: "66741049"
---
# <a name="button-padding-and-shadows-on-android"></a>ボタンの余白と Android でのシャドウ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

この Android プラットフォームに固有では、Xamarin.Forms のボタンを使用して、既定のパディングと Android のボタンの影の値かどうかを制御します。 XAML で設定して使用される、 [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty)と[ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty)添付プロパティを`boolean`値。

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

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

`Button.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean))と[ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間には、Xamarin.Forms のボタンが既定値を使用するかどうか制御に使用されますパディングと Android のボタンの影の値。 さらに、 [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button}))と[ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button}))ボタンが既定の値と既定シャドウの値をそれぞれ埋め込みを使用するかどうかを返すメソッドを使用できます。

結果は、既定の埋め込みと Android のボタンの影の値は、Xamarin.Forms のボタンが使用できます。

![](button-padding-shadow-images/button-padding-and-shadow.png "既定のパディングと Android のボタンの影の値")

各上のスクリーン ショットで[ `Button` ](xref:Xamarin.Forms.Button)点を除いて同一の定義が含まれて、右側`Button`既定のパディングと Android のボタンの影の値を使用します。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
