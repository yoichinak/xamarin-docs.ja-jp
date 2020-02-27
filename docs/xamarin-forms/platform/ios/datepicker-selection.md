---
title: IOS での DatePicker 項目の選択
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、DatePicker で項目の選択が行われるタイミングを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 847E69D1-6AE0-4E82-B9C8-919E009C2014
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: df84cf01909cec564edc9c6c8bb55382a2b9dfe3
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646690"
---
# <a name="datepicker-item-selection-on-ios"></a>IOS での DatePicker 項目の選択

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のコントロールは、 [`DatePicker`](xref:Xamarin.Forms.DatePicker)で項目の選択が発生したときに、ユーザーがコントロールの項目を参照するときに項目の選択を**行う**ように指定できるようにします。または、[完了] ボタンをクリックしたときにのみ表示されます。 XAML では、`DatePicker.UpdateMode` 添付プロパティを `UpdateMode` 列挙値に設定することによって使用されます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <DatePicker MinimumDate="01/01/2020"
                   MaximumDate="12/31/2020"
                   ios:DatePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`DatePicker.On<iOS>` メソッドは、このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間の `DatePicker.SetUpdateMode` メソッドは、項目の選択が行われるタイミングを制御するために使用されます。 `UpdateMode` 列挙体では、次の2つの値を指定できます。

- `Immediately`-ユーザーが[`DatePicker`](xref:Xamarin.Forms.DatePicker)内の項目を参照すると、項目の選択が発生します。 これは Xamarin.Forms の既定の動作です。
- `WhenFinished`-ユーザーが[`DatePicker`](xref:Xamarin.Forms.DatePicker)の **[完了]** ボタンをクリックした後にのみ、項目の選択が発生します。

さらに、`SetUpdateMode` メソッドを使用して、現在の `UpdateMode`を返す `UpdateMode` メソッドを呼び出すことにより、列挙値を切り替えることができます。

```csharp
switch (datePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        datePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

結果として、指定された `UpdateMode` が[`DatePicker`](xref:Xamarin.Forms.DatePicker)に適用されます。これは、項目の選択が行われるタイミングを制御します。

[![DatePicker 更新モードのスクリーンショット](datepicker-selection-images/datepicker-updatemode.png "DatePicker UpdateMode プラットフォーム固有")](datepicker-selection-images/datepicker-updatemode-large.png#lightbox "DatePicker UpdateMode プラットフォーム固有")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
