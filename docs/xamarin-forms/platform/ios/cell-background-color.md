---
title: IOS のセルの背景色
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォーム上でのみ利用できる機能の使用を可能にします。 この記事では、ios のセルの既定の背景色を設定する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2A3FDACF-5AE2-40DE-8488-6FE41733712F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 24276dce97e4935ba41d7012cf6a9aa8fa2658a8
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "68651377"
---
# <a name="cell-background-color-on-ios"></a>IOS のセルの背景色

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、インスタンスの[`Cell`](xref:Xamarin.Forms.Cell)既定の背景色を設定します。 これは、バインド可能な`Cell.DefaultBackgroundColor` [`Color`](xref:Xamarin.Forms.Color)プロパティをに設定することによって XAML で使用されます。

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

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

`ListView.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 名前空間のメソッド`Cell.SetDefaultBackgroundColor` [`Color`](xref:Xamarin.Forms.Color)は、セルの背景色を指定されたに設定します。 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) また`Cell.DefaultBackgroundColor` 、メソッドを使用して、現在のセルの背景色を取得することもできます。

結果として、の背景色[`Cell`](xref:Xamarin.Forms.Cell)を特定[`Color`](xref:Xamarin.Forms.Color)のに設定できます。

[![IOS 上の青緑のグループヘッダーセルのスクリーンショット](cell-background-color-images/group-header-cell-color.png "緑色のグループヘッダーセルを含む ListView")](cell-background-color-images/group-header-cell-color-large.png#lightbox "緑色のグループヘッダーセルを含む ListView")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
