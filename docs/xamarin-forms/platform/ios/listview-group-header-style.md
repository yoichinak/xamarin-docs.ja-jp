---
title: IOS 上の ListView グループヘッダーのスタイル
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、スクロール中に ListView ヘッダーセルをフローティングにするかどうかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: f40737799f63c6e0c61fcc6f4f59584222a49d6d
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648320"
---
# <a name="listview-group-header-style-on-ios"></a>IOS 上の ListView グループヘッダーのスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の設定は[`ListView`](xref:Xamarin.Forms.ListView) 、スクロール中にヘッダーセルをフローティングにするかどうかを制御します。 このメソッドは、バインド可能なプロパティ`ListView.GroupHeaderStyle`を`GroupHeaderStyle`列挙体の値に設定することにより、XAML で使用されます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.GroupHeaderStyle="Grouped">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

`ListView.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 スクロール中にヘッダーセル[`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)をフローティングするかどうか[`ListView`](xref:Xamarin.Forms.ListView)を制御するには、名前空間のメソッドを使用します。`ListView.SetGroupHeaderStyle` 列挙`GroupHeaderStyle`体には、次の2つの値があります。

- `Plain`–がスクロールされたとき ( [`ListView`](xref:Xamarin.Forms.ListView)既定値)、ヘッダーセルがフローティングすることを示します。
- `Grouped`–がスクロールされるときに、 [`ListView`](xref:Xamarin.Forms.ListView)ヘッダーセルがフローティングしないことを示します。

また`ListView.GetGroupHeaderStyle` 、メソッドを使用して、 [`ListView`](xref:Xamarin.Forms.ListView)に適用さ`GroupHeaderStyle`れたを返すこともできます。

結果として、指定`GroupHeaderStyle`された値が[`ListView`](xref:Xamarin.Forms.ListView)に適用されます。これは、スクロール中にヘッダーセルをフローティングするかどうかを制御します。

フローティングおよび非フローティング[(listview-group-header-style-images/group-header-styles.png "ヘッダーセルを使用した iOS listview") ![でのフローティングおよび非フローティングの ListView ヘッダーセルのスクリーンショット]](listview-group-header-style-images/group-header-styles-large.png#lightbox "フローティングおよび非フローティングヘッダーセルを含む ListView")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
