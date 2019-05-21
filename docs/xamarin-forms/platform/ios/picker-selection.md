---
title: IOS での選択項目の選択
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、iOS プラットフォームに固有の選択での項目の選択のタイミングを制御するを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 26B0604A-BD30-49FD-83A6-F0EDFBB0524B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: aeefa22c8a17611579c56b3105be860c12a8711c
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65925377"
---
# <a name="picker-item-selection-on-ios"></a>IOS での選択項目の選択

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

この iOS プラットフォームに固有のコントロールで項目の選択が発生する、 [ `Picker` ](xref:Xamarin.Forms.Picker)、ユーザーが項目の選択が、コントロール内の項目を参照するときに、または 1 回だけ発生することを指定できるように、**完了**ボタンが押されました。 これは、 XAML で `Picker.UpdateMode` 添付プロパティを `UpdateMode` 列挙型の値に設定して使用します。

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

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 `Picker.SetUpdateMode`メソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間に存在し、 `UpdateMode` 列挙型が提供する次の 2 つの値を使って、アイテム選択が発生するタイミングを制御するために使用されます。

- `Immediately` – [`Picker`](xref:Xamarin.Forms.Picker)でユーザーがアイテムを動かしているときにアイテム選択が発生します。 これは Xamarin.Forms の既定の動作です。
- `WhenFinished` – [`Picker`](xref:Xamarin.Forms.Picker) でユーザーが**完了**ボタンを押した時に 1 回だけアイテムの選択が発生します。

さらに、`SetUpdateMode` メソッドは、現在の`UpdateMode`を返す`UpdateMode`メソッドを呼んで列挙型の値を切り替えることに使用できます。

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

その結果、[`Picker`](xref:Xamarin.Forms.Picker)に指定された`UpdateMode`が適用され、アイテム選択が発生するタイミングを制御します。

[![](picker-selection-images/picker-updatemode.png "ピッカー UpdateMode プラットフォーム固有")](picker-selection-images/picker-updatemode-large.png#lightbox "ピッカー UpdateMode プラットフォームに固有")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
