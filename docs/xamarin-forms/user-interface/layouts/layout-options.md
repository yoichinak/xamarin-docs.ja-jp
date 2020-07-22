---
title: レイアウトオプションXamarin.Forms
description: すべて Xamarin.Forms のビューには、LayoutOptions 型の水平オプションと垂直オプションのプロパティがあります。 この記事では、各 LayoutOptions 値がビューの配置と展開に与える影響について説明します。
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6fa03cf5c18e21ce5ca9e7ea907f50c8de6f3e6c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573262"
---
# <a name="layout-options-in-xamarinforms"></a>レイアウトオプションXamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)

_すべて Xamarin.Forms のビューには、LayoutOptions 型の水平オプションと垂直オプションのプロパティがあります。この記事では、各 LayoutOptions 値がビューの配置と展開に与える影響について説明します。_

## <a name="overview"></a>概要

[`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions)構造体は、次の2つのレイアウト設定をカプセル化します。

- **Alignment** –ビューの優先配置で、親レイアウト内の位置とサイズを決定します。
- **拡張**–によってのみ使用され、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 使用可能な場合はビューが余分な領域を使用する必要があるかどうかを示します。

これらのレイアウト設定は、の [`View`](xref:Xamarin.Forms.View) [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティまたは [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティ `View` を構造体のいずれかのパブリックフィールドに設定することにより、親を基準としてに適用できます [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 。 パブリックフィールドは次のとおりです。

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`Start`、、 `Center` `End` 、およびの `Fill` 各フィールドは、親レイアウト内のビューの配置を定義するために使用されます。

- 水平方向の配置で [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) は、が [`View`](xref:Xamarin.Forms.View) 親レイアウトの左側に配置され、垂直方向の配置では、が `View` 親レイアウトの先頭に配置されます。
- 水平方向および垂直方向の配置で [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) は、を水平方向または垂直方向に中央揃えで配置し [`View`](xref:Xamarin.Forms.View) ます。
- 水平方向の配置で [`End`](xref:Xamarin.Forms.LayoutOptions.End) は、が [`View`](xref:Xamarin.Forms.View) 親レイアウトの右側に配置され、垂直方向の配置では、が `View` 親レイアウトの下部に配置されます。
- 水平方向の配置で [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) は、が [`View`](xref:Xamarin.Forms.View) 親レイアウトの幅を塗りつぶすようにします。また、垂直方向の配置では、が `View` 親レイアウトの高さを満たすことを保証します。

`StartAndExpand`、、 `CenterAndExpand` 、およびの各 `EndAndExpand` `FillAndExpand` 値は、配置設定を定義するために使用されます。また、親内で使用可能な場合は、ビューでより多くの領域を使用するかどうかを指定し [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。

> [!NOTE]
> ビューの [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティと [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティの既定値は[`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) です。

## <a name="alignment"></a>アラインメント

配置では、親レイアウトに未使用の領域が含まれている場合 (つまり、親のレイアウトがすべての子の合計サイズを超える場合)、親レイアウト内でビューがどのように配置されるかを制御します。

は [`StackLayout`](xref:Xamarin.Forms.StackLayout) `Start` 、 `Center` 逆方向の子ビューの、、 `End` 、およびの各フィールドに対してのみ、向きを尊重し `Fill` [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) `StackLayout` ます。 したがって、垂直方向の子ビューでは `StackLayout` [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) `Start` 、、、 `Center` `End` 、またはの各フィールドのいずれかにプロパティを設定でき `Fill` ます。 同様に、水平方向の子ビューでは `StackLayout` [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 、、、、のいずれかのフィールドにプロパティを設定でき `Start` `Center` `End` `Fill` ます。

は、方向 [`StackLayout`](xref:Xamarin.Forms.StackLayout) `Start` `Center` `End` `Fill` [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) と同じ方向にある子ビューの、、、およびの各フィールド `StackLayout` を考慮しません。 そのため、 `StackLayout` `Start` `Center` `End` `Fill` 子ビューのプロパティにが設定されている場合、垂直方向のでは、、、、の各フィールドが無視され [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) ます。 同様に、 `StackLayout` `Start` `Center` `End` `Fill` 子ビューのプロパティに設定されている場合は、、、、の各フィールドは、水平方向によって無視され [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) ます。

> [!NOTE]
> [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)一般に、プロパティとプロパティを使用して指定されたサイズ要求を上書きし [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) ます。

次の XAML コード例は、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 各子が [`Label`](xref:Xamarin.Forms.Label) その [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティを構造体の4つのアラインメントフィールドのいずれかに設定する垂直方向のを示してい [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) ます。

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

同等の C# コードを次に示します。

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
    new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
    new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
    new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
  }
};
```

コードを実行すると、次のスクリーンショットに示されているレイアウトが表示されます。

[![](layout-options-images/alignment.png "Alignment Layout Options")](layout-options-images/alignment-large.png#lightbox "Alignment Layout Options")

## <a name="expansion"></a>正規の表記

展開では、内でビューが使用可能な領域を占有するかどうかを制御し [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。 に `StackLayout` 未使用の領域が含まれている場合 (つまり、が `StackLayout` そのすべての子の合計サイズを超える場合)、その [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティまたは [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティをサフィックスを使用するフィールドに設定することによって、展開を要求するすべての子ビューによって未使用領域が均等に共有され [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) `AndExpand` ます。 内のすべての領域が使用されている場合、 `StackLayout` 展開オプションの効果はありません。

[`StackLayout`](xref:Xamarin.Forms.StackLayout) は子ビューをその方向にのみ展開できます。 このため、に `StackLayout` [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) `StartAndExpand` `CenterAndExpand` `EndAndExpand` `FillAndExpand` `StackLayout` 使用されていない領域がある場合、垂直方向には、プロパティを、、、またはフィールドのいずれかに設定する子ビューを展開できます。 同様に、 `StackLayout` [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) `StartAndExpand` `CenterAndExpand` `EndAndExpand` `FillAndExpand` が `StackLayout` 使用されていない領域が含まれている場合、水平方向のは、プロパティを、、、またはフィールドのいずれかに設定する子ビューを展開できます。

では、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 子ビューを向きの反対方向に展開することはできません。 したがって、垂直方向に、 `StackLayout` [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 子ビューのプロパティをに設定する [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand) と、プロパティをに設定するのと同じ効果があり [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) ます。

> [!NOTE]
> 拡張を有効にしても、を使用しない場合はビューのサイズが変更されないことに注意 [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) してください。

次の XAML コード例は、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 各子が [`Label`](xref:Xamarin.Forms.Label) その [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティを、構造体の4つの拡張フィールドのいずれかに設定する垂直方向のを示してい [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) ます。

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Start" BackgroundColor="Gray" VerticalOptions="StartAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Center" BackgroundColor="Gray" VerticalOptions="CenterAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="End" BackgroundColor="Gray" VerticalOptions="EndAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Fill" BackgroundColor="Gray" VerticalOptions="FillAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
</StackLayout>
```

同等の C# コードを次に示します。

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
  }
};
```

コードを実行すると、次のスクリーンショットに示されているレイアウトが表示されます。

[![](layout-options-images/expansion.png "Expansion Layout Options")](layout-options-images/expansion-large.png#lightbox "Expansion Layout Options")

各 [`Label`](xref:Xamarin.Forms.Label) は、内で同じ量の領域を占有し [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。 ただし、[`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティを [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) に設定する最後の `Label` のみ、サイズが異なります。 さらに、各 `Label` は小さい赤で区切られ [`BoxView`](xref:Xamarin.Forms.BoxView) ます。これにより、 `Label` 占有領域を簡単に表示できます。

## <a name="summary"></a>まとめ

この記事では、各 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 構造体の値が、親を基準としたビューの配置と展開に与える影響について説明しました。 `Start`、、 `Center` `End` 、およびの `Fill` 各フィールドは、親レイアウト内のビューの配置を定義するために使用されます。また、、、 `StartAndExpand` `CenterAndExpand` `EndAndExpand` 、およびの各フィールドは、配置設定を定義するために使用されます。また、使用 `FillAndExpand` 可能な場合は、内でビューが使用できる領域を占有するかどうかを決定し [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。

## <a name="related-links"></a>関連リンク

- [LayoutOptions (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)
- [LayoutOptions](xref:Xamarin.Forms.LayoutOptions)
