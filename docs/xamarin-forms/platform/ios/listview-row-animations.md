---
title: IOS での ListView 行アニメーション
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ListView 項目のコレクションを更新するときに行アニメーションを無効にするかどうかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: E8F5103F-4D8E-4A5A-A16C-7FA14EE786AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7f64dcd0bfcd68f7e5145ce8191dcd4a580fb2a6
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372510"
---
# <a name="listview-row-animations-on-ios"></a>IOS での ListView 行アニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の設定では、項目コレクションを更新するときに行アニメーションを無効にするかどうかを制御 [`ListView`](xref:Xamarin.Forms.ListView) します。 これは、バインド可能なプロパティをに設定することによって XAML で使用され `ListView.RowAnimationsEnabled` `false` ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.RowAnimationsEnabled="false">
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

listView.On<iOS>().SetRowAnimationsEnabled(false);
```

メソッドは、 `ListView.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `ListView.SetRowAnimationsEnabled`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 項目のコレクションの更新時に行アニメーションを無効にするかどうかを制御するために使用され [`ListView`](xref:Xamarin.Forms.ListView) ます。 また、メソッドを `ListView.GetRowAnimationsEnabled` 使用して、で行アニメーションが無効になっているかどうかを返すこともでき `ListView` ます。

> [!NOTE]
> [`ListView`](xref:Xamarin.Forms.ListView) 既定では、行アニメーションが有効になっています。 したがって、に新しい行が挿入されると、アニメーションが発生し `ListView` ます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)