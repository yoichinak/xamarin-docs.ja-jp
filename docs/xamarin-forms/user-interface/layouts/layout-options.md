---
title: LayoutOptions
description: "Xamarin.Forms のすべてのビューには、型 LayoutOptions、HorizontalOptions と VerticalOptions プロパティがあります。 この記事では、各 LayoutOptions 値が、配置とビューの拡張に与える影響について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: a2aa143d5aeb801cd753dd99718ca9cf6dd72353
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="layoutoptions"></a>LayoutOptions

_Xamarin.Forms のすべてのビューには、型 LayoutOptions、HorizontalOptions と VerticalOptions プロパティがあります。この記事では、各 LayoutOptions 値が、配置とビューの拡張に与える影響について説明します。_

## <a name="overview"></a>概要

[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)構造体は 2 つのレイアウトの基本設定をカプセル化します。

- **配置**– ビューの親レイアウト内でのサイズと位置を決定するアラインメントを優先します。
- **拡張**– でのみ使われる、 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)、し、使用可能になる場合、ビューが余分なスペースを使用するかどうかを示します。

これらのレイアウト設定に適用できる、 [ `View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)を設定して、その親に相対パス、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)または[ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)のプロパティ、`View`からのパブリック フィールドのいずれかに、 [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)構造体。 パブリック フィールドは次のとおりです。

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`Start`、 `Center`、 `End`、および`Fill`親レイアウト内で、ビューのアラインメントを定義するフィールドが使用されます。

- 水平方向の配置の[ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)位置、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)側では、左側にある親のレイアウトおよび垂直方向の配置、配置、`View`の上部にある、親のレイアウトです。
- 水平および垂直方向の配置の[ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)水平方向または垂直方向に中央揃え、 [ `View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)です。
- 水平方向の配置の[ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)位置、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)配置と垂直方向の配置、親のレイアウトの右側にある、上、`View`下部にあります。親のレイアウトです。
- 水平方向の配置の[ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)確実、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)幅いっぱいになる、親のレイアウトと垂直方向の配置のことを確認する、`View`塗りつぶします、親のレイアウトの高さ。

`StartAndExpand`、 `CenterAndExpand`、 `EndAndExpand`、および`FillAndExpand`値を使用して、配置設定を定義するかどうか、ビューは占有容量、親の中で使用可能な場合と[ `StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)です。

> [!NOTE]
> 既定値のビューの[ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)と[ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)プロパティは[ `LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)です。

<a name="alignment" />

## <a name="alignment"></a>アラインメント

配置を制御ビューをどのように配置親レイアウト内で親レイアウトには、未使用領域が含まれている場合 (つまり、親のレイアウトがそのすべての子の合計サイズより大きい)。

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)のみを尊重、 `Start`、 `Center`、 `End`、および`Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)反対方向には、子ビューのフィールド`StackLayout`方向です。 そのため、垂直方向内の子ビュー`StackLayout`を設定できます、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)プロパティのいずれかを`Start`、 `Center`、 `End`、または`Fill`フィールドです。 同様に、水平方向内の子ビュー`StackLayout`を設定できます、 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)プロパティのいずれかを`Start`、 `Center`、 `End`、または`Fill`フィールドです。

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)を考慮せず、 `Start`、 `Center`、 `End`、および`Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)と方向が同じでは、子ビューのフィールド`StackLayout`方向です。 したがって、垂直方向に`StackLayout`無視、 `Start`、 `Center`、 `End`、または`Fill`フィールドに設定されている場合、 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)子ビューのプロパティです。 同様に、水平方向に`StackLayout`無視、 `Start`、 `Center`、 `End`、または`Fill`フィールドに設定されている場合、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)子ビューのプロパティです。

> [!NOTE]
> [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) 上書きを使用して指定された要求のサイズを一般に、 [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/)と[ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)プロパティです。

次の XAML コード例で、垂直方向に[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 、それぞれの子[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)設定、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)プロパティ次の 4 つの配置のフィールドのいずれかに、 [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)構造体。

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

同等の c# コードは、次に示します。

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

コードは、次のスクリーン ショットに示すように、レイアウトが得られます。

[![](layout-options-images/alignment.png "レイアウトの配置オプション")](layout-options-images/alignment-large.png#lightbox "レイアウトの配置オプション")

<a name="expansion" />

## <a name="expansion"></a>拡張

拡張内で、使用可能な場合、ビューは、多くの領域を占有するかどうかを制御する、 [ `StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)です。 場合、`StackLayout`未使用領域が含まれています (つまり、`StackLayout`がそのすべての子の合計サイズより大きい)、未使用の領域は均等に設定して展開を要求するすべての子ビューによって、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)または[ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)プロパティを[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)を使用するフィールド、`AndExpand`サフィックス。 場合内のすべての領域、`StackLayout`が使用される、拡張オプションがある影響しません。

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)その orientation の方向で子ビューを展開します。 したがって、垂直方向に`StackLayout`を設定する子ビューを展開することができます、 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)プロパティのいずれかを`StartAndExpand`、 `CenterAndExpand`、 `EndAndExpand`、または`FillAndExpand`フィールドの場合、 `StackLayout`未使用領域が含まれています。 同様に、水平方向に`StackLayout`を設定する子ビューを展開することができます、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)プロパティのいずれかを`StartAndExpand`、 `CenterAndExpand`、 `EndAndExpand`、または`FillAndExpand`フィールドの場合、 `StackLayout`未使用領域が含まれています。

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)の方向とは反対方向に子ビューを展開することはできません。 したがって、垂直方向に`StackLayout`、設定、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)プロパティを子ビューを[ `StartAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)プロパティを設定すると同じ効果[`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/).

> [!NOTE]
> 注拡張を有効化が変更されないこと、ビューのサイズを使用しない限り[ `LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)です。

次の XAML コード例で、垂直方向に[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 、それぞれの子[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)設定、 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)プロパティ次の 4 つの拡張のフィールドのいずれかに、 [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)構造体。

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

同等の c# コードは、次に示します。

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

コードは、次のスクリーン ショットに示すように、レイアウトが得られます。

[![](layout-options-images/expansion.png "拡張レイアウト オプション")](layout-options-images/expansion-large.png#lightbox "拡張レイアウト オプション")

各[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)同じ量の内の領域を占有、 [ `StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)です。 ただし、最終的な`Label`、設定する、 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)プロパティを[ `FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)別のサイズします。 さらに、各`Label`小さな赤いで区切られた[ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)、領域を有効にする、`Label`を簡単に表示できるを占有します。

## <a name="summary"></a>まとめ

この記事の内容に影響が説明されている各[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)アラインメントとその親ビューの拡張に対する構造体の値を持ちます。 `Start`、 `Center`、 `End`、および`Fill`親レイアウト内で、ビューの配置の定義に使用されるフィールドと`StartAndExpand`、 `CenterAndExpand`、 `EndAndExpand`、および`FillAndExpand`定義に使用されるフィールド配置設定では、内で、使用可能な場合、ビューは、多くの領域を占有するかどうかを決定して、 [ `StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)です。



## <a name="related-links"></a>関連リンク

- [LayoutOptions (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutoptions/)
- [LayoutOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)
