---
title: IOS でのセルの背景色
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、iOS でのセルの既定の背景色の設定 iOS プラットフォームに固有の使用方法について説明します。
ms.prod: xamarin
ms.assetid: 2A3FDACF-5AE2-40DE-8488-6FE41733712F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 6b1e2fe534c8b7d0c3346a18d1b82d797e52dba1
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926765"
---
# <a name="cell-background-color-on-ios"></a>IOS でのセルの背景色

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

この iOS プラットフォーム固有設定の既定の背景色[ `Cell` ](xref:Xamarin.Forms.Cell)インスタンス。 XAML で設定して使用される、`Cell.DefaultBackgroundColor`バインド可能なプロパティを[ `Color` ](xref:Xamarin.Forms.Color):

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

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

`ListView.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  `Cell.SetDefaultBackgroundColor`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間には、指定したセルの背景色を設定する[ `Color`](xref:Xamarin.Forms.Color)します。 さらに、`Cell.DefaultBackgroundColor`メソッドを使用して、現在のセルの背景色を取得できます。

その結果、背景色で、 [ `Cell` ](xref:Xamarin.Forms.Cell)特定に設定することができます[ `Color` ](xref:Xamarin.Forms.Color):

[![青緑グループのヘッダー セルの iOS のスクリーン ショット](cell-background-color-images/group-header-cell-color.png "青緑グループ ヘッダー セルで ListView")](cell-background-color-images/group-header-cell-color-large.png#lightbox "青緑グループ ヘッダー セルで ListView")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
