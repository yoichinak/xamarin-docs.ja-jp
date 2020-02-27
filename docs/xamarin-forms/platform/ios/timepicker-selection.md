---
title: IOS での TimePicker 項目の選択
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、TimePicker で項目の選択が行われるタイミングを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 554AC877-8698-4B8C-A676-80DD64305A06
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: 818f368da8ebb375fbacd97d3d48185ba60470d4
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646678"
---
# <a name="timepicker-item-selection-on-ios"></a>IOS での TimePicker 項目の選択

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のコントロールは、 [`TimePicker`](xref:Xamarin.Forms.TimePicker)で項目の選択が発生したときに、ユーザーがコントロールの項目を参照するときに項目の選択を**行う**ように指定できるようにします。または、[完了] ボタンをクリックしたときにのみ表示されます。 XAML では、`TimePicker.UpdateMode` 添付プロパティを `UpdateMode` 列挙値に設定することによって使用されます。

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

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`TimePicker.On<iOS>` メソッドは、このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間の `TimePicker.SetUpdateMode` メソッドは、項目の選択が行われるタイミングを制御するために使用されます。 `UpdateMode` 列挙体では、次の2つの値を指定できます。

- `Immediately`-ユーザーが[`TimePicker`](xref:Xamarin.Forms.TimePicker)内の項目を参照すると、項目の選択が発生します。 これは Xamarin.Forms の既定の動作です。
- `WhenFinished`-ユーザーが[`TimePicker`](xref:Xamarin.Forms.TimePicker)の **[完了]** ボタンをクリックした後にのみ、項目の選択が発生します。

さらに、`SetUpdateMode` メソッドを使用して、現在の `UpdateMode`を返す `UpdateMode` メソッドを呼び出すことにより、列挙値を切り替えることができます。

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

結果として、指定された `UpdateMode` が[`TimePicker`](xref:Xamarin.Forms.TimePicker)に適用されます。これは、項目の選択が行われるタイミングを制御します。

[![TimePicker 更新モードのスクリーンショット](timepicker-selection-images/timepicker-updatemode.png "TimePicker UpdateMode プラットフォーム固有")](timepicker-selection-images/timepicker-updatemode-large.png#lightbox "TimePicker UpdateMode プラットフォーム固有")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
