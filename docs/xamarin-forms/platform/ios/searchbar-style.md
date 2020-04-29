---
title: IOS の SearchBar スタイル
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、SearchBar に背景があるかどうかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 3D512DD6-078E-4BC6-926E-62BA6F4DE640
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
ms.openlocfilehash: 7d95a90e96f868b6d8368054659f978555bc28ac
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82532953"
---
# <a name="searchbar-style-on-ios"></a>IOS の SearchBar スタイル

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、に[`SearchBar`](xref:Xamarin.Forms.SearchBar)背景があるかどうかを制御します。 このメソッドは、バインド可能なプロパティ`SearchBar.SearchBarStyle`を`UISearchBarStyle`列挙体の値に設定することにより、XAML で使用されます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ios:SearchBar.SearchBarStyle="Minimal"
                   Placeholder="Enter search term" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

SearchBar searchBar = new SearchBar { Placeholder = "Enter search term" };
searchBar.On<iOS>().SetSearchBarStyle(UISearchBarStyle.Minimal);
```

メソッド`SearchBar.On<iOS>`は、このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 名前空間のメソッドは`SearchBar.SetSearchBarStyle` 、 [`SearchBar`](xref:Xamarin.Forms.SearchBar)に背景があるかどうかを制御するために使用されます。 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 列挙`UISearchBarStyle`体には、次の3つの値があります。

- `Default`に既定の[`SearchBar`](xref:Xamarin.Forms.SearchBar)スタイルが設定されていることを示します。 これは、 `SearchBar.SearchBarStyle`バインド可能なプロパティの既定値です。
- `Prominent`の[`SearchBar`](xref:Xamarin.Forms.SearchBar)背景が半透明で、検索フィールドが不透明であることを示します。
- `Minimal`に[`SearchBar`](xref:Xamarin.Forms.SearchBar)背景がなく、検索フィールドが半透明であることを示します。

また、 `SearchBar.GetSearchBarStyle`メソッドを使用して、 `UISearchBarStyle` `SearchBar`に適用されたを返すこともできます。

結果として、指定`UISearchBarStyle`されたメンバーが[`SearchBar`](xref:Xamarin.Forms.SearchBar)に適用され、 `SearchBar`に背景があるかどうかを制御します。

![IOS 上の SearchBar スタイルのスクリーンショット](searchbar-style-images/searchbar-styles.png "IOS での SearchBar スタイル")

次のスクリーンショットは`UISearchBarStyle` 、 `BackgroundColor`プロパティが[`SearchBar`](xref:Xamarin.Forms.SearchBar)設定されているオブジェクトに適用されるメンバーを示しています。

![バックグラウンド色を使用した SearchBar スタイルのスクリーンショット (iOS)](searchbar-style-images/searchbar-background-styles.png "IOS の背景色を持つ SearchBar スタイル")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
