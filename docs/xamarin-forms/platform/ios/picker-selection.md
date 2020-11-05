---
title: IOS でのピッカー項目の選択
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ピッカーで項目の選択が行われるタイミングを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 26B0604A-BD30-49FD-83A6-F0EDFBB0524B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5b7315fce6a51c570c4486c54fdfc81932a4b8f8
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372688"
---
# <a name="picker-item-selection-on-ios"></a>IOS でのピッカー項目の選択

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のコントロールでは、で項目の選択が発生したときに、 [`Picker`](xref:Xamarin.Forms.Picker) ユーザーがコントロールの項目を参照するときに項目の選択を行うように指定できます。または、[ **完了** ] ボタンをクリックしたときにのみ、項目の選択が行われます。 これは、 `Picker.UpdateMode` 添付プロパティを列挙体の値に設定することによって XAML で使用され `UpdateMode` ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

メソッドは、 `Picker.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `Picker.SetUpdateMode`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 項目の選択が行われるタイミングを制御するために使用され `UpdateMode` ます。列挙体では、次の2つの値を指定できます。

- `Immediately` –項目の選択は、ユーザーが内の項目を参照したときに発生し [`Picker`](xref:Xamarin.Forms.Picker) ます。 これは、の既定の動作です Xamarin.Forms 。
- `WhenFinished` –項目の選択は、ユーザーがの [ **完了** ] ボタンをクリックしたときにのみ発生し [`Picker`](xref:Xamarin.Forms.Picker) ます。

また、メソッドを `SetUpdateMode` 使用して、現在のを返すメソッドを呼び出すことにより、列挙値を切り替えることもでき `UpdateMode` `UpdateMode` ます。

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

結果として、指定された `UpdateMode` がに適用され [`Picker`](xref:Xamarin.Forms.Picker) ます。これは、項目の選択が行われるタイミングを制御します。

[![ピッカー UpdateMode プラットフォーム固有](picker-selection-images/picker-updatemode.png)](picker-selection-images/picker-updatemode-large.png#lightbox "選択 UpdateMode Platform-Specific")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)