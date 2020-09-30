---
title: IOS での TimePicker 項目の選択
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、TimePicker で項目の選択が行われるタイミングを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 554AC877-8698-4B8C-A676-80DD64305A06
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 372c268c13c50719953ac63bcc43cc8d9bff4bdd
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563719"
---
# <a name="timepicker-item-selection-on-ios"></a>IOS での TimePicker 項目の選択

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のコントロールでは、で項目の選択が発生したときに、 [`TimePicker`](xref:Xamarin.Forms.TimePicker) ユーザーがコントロールの項目を参照するときに項目の選択を行うように指定できます。または、[ **完了** ] ボタンをクリックしたときにのみ、項目の選択が行われます。 これは、 `TimePicker.UpdateMode` 添付プロパティを列挙体の値に設定することによって XAML で使用され `UpdateMode` ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <TimePicker Time="14:00:00"
                   ios:TimePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

メソッドは、 `TimePicker.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `TimePicker.SetUpdateMode`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 項目の選択が行われるタイミングを制御するために使用され `UpdateMode` ます。列挙体では、次の2つの値を指定できます。

- `Immediately` –項目の選択は、ユーザーが内の項目を参照したときに発生し [`TimePicker`](xref:Xamarin.Forms.TimePicker) ます。 これは、の既定の動作です Xamarin.Forms 。
- `WhenFinished` –項目の選択は、ユーザーがの [ **完了** ] ボタンをクリックしたときにのみ発生し [`TimePicker`](xref:Xamarin.Forms.TimePicker) ます。

また、メソッドを `SetUpdateMode` 使用して、現在のを返すメソッドを呼び出すことにより、列挙値を切り替えることもでき `UpdateMode` `UpdateMode` ます。

```csharp
switch (timePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

結果として、指定された `UpdateMode` がに適用され [`TimePicker`](xref:Xamarin.Forms.TimePicker) ます。これは、項目の選択が行われるタイミングを制御します。

[![TimePicker 更新モードのスクリーンショット](timepicker-selection-images/timepicker-updatemode.png "TimePicker UpdateMode プラットフォーム固有")](timepicker-selection-images/timepicker-updatemode-large.png#lightbox "TimePicker UpdateMode プラットフォーム固有")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)