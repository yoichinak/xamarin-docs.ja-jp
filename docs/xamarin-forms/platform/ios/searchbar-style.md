---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e02ef600af761915d05c912b586e409dd6f46b85
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137086"
---
# <a name="searchbar-style-on-ios"></a>IOS の SearchBar スタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、に背景があるかどうかを制御 [`SearchBar`](xref:Xamarin.Forms.SearchBar) します。 このメソッドは、バインド可能な `SearchBar.SearchBarStyle` プロパティを列挙体の値に設定することにより、XAML で使用され `UISearchBarStyle` ます。

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

メソッドは、 `SearchBar.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `SearchBar.SetSearchBarStyle`名前空間のメソッドは [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 、に背景があるかどうかを制御するために使用され [`SearchBar`](xref:Xamarin.Forms.SearchBar) ます。 `UISearchBarStyle`列挙体には、次の3つの値があります。

- `Default`に既定のスタイルが設定されていることを示し [`SearchBar`](xref:Xamarin.Forms.SearchBar) ます。 これは、バインド可能なプロパティの既定値です `SearchBar.SearchBarStyle` 。
- `Prominent`の背景が [`SearchBar`](xref:Xamarin.Forms.SearchBar) 半透明で、検索フィールドが不透明であることを示します。
- `Minimal`に背景が [`SearchBar`](xref:Xamarin.Forms.SearchBar) なく、検索フィールドが半透明であることを示します。

また、メソッドを `SearchBar.GetSearchBarStyle` 使用して、に適用されたを返すこともでき `UISearchBarStyle` `SearchBar` ます。

結果として、指定された `UISearchBarStyle` メンバーがに適用され、に背景がある [`SearchBar`](xref:Xamarin.Forms.SearchBar) かどうかを制御し `SearchBar` ます。

![IOS 上の SearchBar スタイルのスクリーンショット](searchbar-style-images/searchbar-styles.png "IOS での SearchBar スタイル")

次のスクリーンショットは、 `UISearchBarStyle` プロパティが設定されているオブジェクトに適用されるメンバーを示して [`SearchBar`](xref:Xamarin.Forms.SearchBar) い `BackgroundColor` ます。

![バックグラウンド色を使用した SearchBar スタイルのスクリーンショット (iOS)](searchbar-style-images/searchbar-background-styles.png "IOS の背景色を持つ SearchBar スタイル")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
