---
title: IOS 上の ListView グループヘッダーのスタイル
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、スクロール中に ListView ヘッダーセルをフローティングにするかどうかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: f40737799f63c6e0c61fcc6f4f59584222a49d6d
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "68648320"
---
# <a name="listview-group-header-style-on-ios"></a>IOS 上の ListView グループヘッダーのスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の設定では、スクロール中にヘッダーセルを[`ListView`](xref:Xamarin.Forms.ListView)かどうかを制御します。 XAML では、`ListView.GroupHeaderStyle` バインド可能なプロパティを `GroupHeaderStyle` 列挙値に設定することによって使用されます。

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

または、fluent API C#を使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

@No__t_0 メソッドは、このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [@No__t_2](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間の `ListView.SetGroupHeaderStyle` メソッドは、スクロール中に[`ListView`](xref:Xamarin.Forms.ListView)ヘッダーセルをフローティングするかどうかを制御するために使用されます。 @No__t_0 列挙体には、次の2つの値があります。

- `Plain` – [`ListView`](xref:Xamarin.Forms.ListView)がスクロールされたときにヘッダーセルがフローティングすることを示します (既定)。
- `Grouped` – [`ListView`](xref:Xamarin.Forms.ListView)がスクロールされてもヘッダーセルがフローティングしないことを示します。

さらに、`ListView.GetGroupHeaderStyle` メソッドを使用して、 [`ListView`](xref:Xamarin.Forms.ListView)に適用される `GroupHeaderStyle` を返すことができます。

結果として、指定された `GroupHeaderStyle` 値が[`ListView`](xref:Xamarin.Forms.ListView)に適用されます。これは、スクロール中にヘッダーセルをフローティングするかどうかを制御します。

[![IOS 上のフローティングおよび非フローティング ListView ヘッダーセルのスクリーンショット](listview-group-header-style-images/group-header-styles.png "フローティングおよび非フローティングヘッダーセルを含む ListView")](listview-group-header-style-images/group-header-styles-large.png#lightbox "フローティングおよび非フローティングヘッダーセルを含む ListView")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
