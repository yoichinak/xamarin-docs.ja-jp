---
title: IOS のセルの背景色
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ios のセルの既定の背景色を設定する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2A3FDACF-5AE2-40DE-8488-6FE41733712F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 24276dce97e4935ba41d7012cf6a9aa8fa2658a8
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "68651377"
---
# <a name="cell-background-color-on-ios"></a>IOS のセルの背景色

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の設定により、 [`Cell`](xref:Xamarin.Forms.Cell)インスタンスの既定の背景色が設定されます。 XAML では、`Cell.DefaultBackgroundColor` バインド可能なプロパティを[`Color`](xref:Xamarin.Forms.Color)に設定することによって使用されます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  IsGroupingEnabled="true">
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <ViewCell ios:Cell.DefaultBackgroundColor="Teal">
                        <Label Margin="10,10"
                               Text="{Binding Key}"
                               FontAttributes="Bold" />
                    </ViewCell>
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
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

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

@No__t_0 メソッドは、このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 @No__t_0 メソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間で、セルの背景色を指定された[`Color`](xref:Xamarin.Forms.Color)に設定します。 また、`Cell.DefaultBackgroundColor` メソッドを使用して、現在のセルの背景色を取得することもできます。

結果として、 [`Cell`](xref:Xamarin.Forms.Cell)の背景色を特定の[`Color`](xref:Xamarin.Forms.Color)に設定できることがわかります。

[![IOS 上の青緑のグループヘッダーセルのスクリーンショット](cell-background-color-images/group-header-cell-color.png "緑色のグループヘッダーセルを含む ListView")](cell-background-color-images/group-header-cell-color-large.png#lightbox "緑色のグループヘッダーセルを含む ListView")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
