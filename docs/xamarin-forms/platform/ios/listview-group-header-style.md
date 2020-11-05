---
title: IOS 上の ListView グループヘッダーのスタイル
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、スクロール中に ListView ヘッダーセルをフローティングにするかどうかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8a856dd74bb319436dbe1c8506d34dfcdb268834
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374174"
---
# <a name="listview-group-header-style-on-ios"></a>IOS 上の ListView グループヘッダーのスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の設定は、 [`ListView`](xref:Xamarin.Forms.ListView) スクロール中にヘッダーセルをフローティングにするかどうかを制御します。 このメソッドは、バインド可能な `ListView.GroupHeaderStyle` プロパティを列挙体の値に設定することにより、XAML で使用され `GroupHeaderStyle` ます。

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

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

メソッドは、 `ListView.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `ListView.SetGroupHeaderStyle` [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) スクロール中にヘッダーセルをフローティングするかどうかを制御するには、名前空間のメソッドを使用し [`ListView`](xref:Xamarin.Forms.ListView) ます。 `GroupHeaderStyle`列挙体には、次の2つの値があります。

- `Plain` – [`ListView`](xref:Xamarin.Forms.ListView) がスクロールされたとき (既定値)、ヘッダーセルがフローティングすることを示します。
- `Grouped` –がスクロールされるときに、ヘッダーセルがフローティングしないことを示し [`ListView`](xref:Xamarin.Forms.ListView) ます。

また、メソッドを `ListView.GetGroupHeaderStyle` 使用して、に適用されたを返すこともでき `GroupHeaderStyle` [`ListView`](xref:Xamarin.Forms.ListView) ます。

結果として、指定された `GroupHeaderStyle` 値がに適用され [`ListView`](xref:Xamarin.Forms.ListView) ます。これは、スクロール中にヘッダーセルをフローティングするかどうかを制御します。

[![IOS 上のフローティングおよび非フローティング ListView ヘッダーセルのスクリーンショット](listview-group-header-style-images/group-header-styles.png "フローティングおよび非フローティングヘッダーセルを含む ListView")](listview-group-header-style-images/group-header-styles-large.png#lightbox "フローティングおよび非フローティングヘッダーセルを含む ListView")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)