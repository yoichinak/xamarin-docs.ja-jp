---
title: Xamarin.Forms のレイアウト オプション
description: すべての Xamarin.Forms のビューには、型 LayoutOptions の HorizontalOptions と \-options のプロパティがあります。 この記事では、各 LayoutOptions 値が、ビューの拡張と配置に与える影響について説明します。
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: 2e4fa5f1fb96077b0237dbeac9074006e761bc09
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655306"
---
# <a name="layout-options-in-xamarinforms"></a>Xamarin.Forms のレイアウト オプション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)

_すべての Xamarin.Forms のビューには、型 LayoutOptions の HorizontalOptions と \-options のプロパティがあります。この記事では、各 LayoutOptions 値が、ビューの拡張と配置に与える影響について説明します。_

## <a name="overview"></a>概要

[ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions)構造体が 2 つのレイアウト設定をカプセル化します。

- **配置**– ビューの配置では、その親のレイアウトのサイズと位置を決定するを優先します。
- **拡張**– のみによって使用される、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)、し、使用できる場合、ビューが余分なスペースを使用するかどうかを示します。

これらのレイアウト設定に適用できる、 [ `View`](xref:Xamarin.Forms.View)を設定して、その親に相対、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)または[ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)のプロパティ、`View`からパブリック フィールドのいずれかに、 [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions)構造体。 パブリック フィールドは次のとおりです。

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`Start`、 `Center`、 `End`、および`Fill`親レイアウト内で、ビューの配置を定義するフィールドを使用します。

- 水平方向の配置の[ `Start` ](xref:Xamarin.Forms.LayoutOptions.Start)位置、 [ `View` ](xref:Xamarin.Forms.View)左側にある側の親のレイアウトと垂直方向の配置では、配置、`View`の上部にある、親のレイアウト。
- 水平および垂直方向の配置の[ `Center` ](xref:Xamarin.Forms.LayoutOptions.Center)水平方向または垂直方向に中央揃え、 [ `View`](xref:Xamarin.Forms.View)します。
- 水平方向の配置の[ `End` ](xref:Xamarin.Forms.LayoutOptions.End)位置、 [ `View` ](xref:Xamarin.Forms.View)の右側にある親のレイアウトと垂直方向の配置、配置、`View`下部にあります。親のレイアウト。
- 水平方向の配置の[ `Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill)により、 [ `View` ](xref:Xamarin.Forms.View)幅いっぱいになるように取り計らいますの親のレイアウトと垂直方向の配置は、`View`塗りつぶします、親のレイアウトの高さ。

`StartAndExpand`、 `CenterAndExpand`、 `EndAndExpand`、および`FillAndExpand`値を使用して、配置設定を定義するかどうか、ビューは占有容量、親の中で使用可能な場合と[ `StackLayout`](xref:Xamarin.Forms.StackLayout)します。

> [!NOTE]
> 既定値のビューの[ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)と[ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)プロパティは[ `LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)します。

<a name="alignment" />

## <a name="alignment"></a>アラインメント

配置を制御すると表示をどのように配置の親レイアウト内で親のレイアウトには、未使用領域が含まれている場合 (つまり、親のレイアウトがそのすべての子の合計サイズより大きい)。

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)のみを尊重、 `Start`、 `Center`、 `End`、および`Fill` [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions)方向が逆に、子ビューのフィールド`StackLayout`方向。 そのため、垂直方向内の子ビュー`StackLayout`を設定できます、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)プロパティのいずれかを`Start`、 `Center`、 `End`、または`Fill`フィールド。 同様に、水平方向内の子ビュー`StackLayout`設定できる、 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)プロパティのいずれかを`Start`、 `Center`、 `End`、または`Fill`フィールド。

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)を考慮せず、 `Start`、 `Center`、 `End`、および`Fill` [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions)と同じ方向に含まれる子ビューのフィールド`StackLayout`方向。 したがって、垂直方向`StackLayout`無視、 `Start`、 `Center`、 `End`、または`Fill`フィールドに設定されている場合、 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)子ビューのプロパティ。 同様に、水平方向`StackLayout`無視、 `Start`、 `Center`、 `End`、または`Fill`フィールドに設定されている場合、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)子ビューのプロパティ。

> [!NOTE]
> [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) 上書きを使用して指定された要求のサイズを一般に、 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest)と[ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest)プロパティ。

次の XAML コード例で、垂直方向に[ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 、それぞれの子[ `Label` ](xref:Xamarin.Forms.Label)設定その[ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)プロパティ次の 4 つの配置のフィールドのいずれかに、 [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions)構造体。

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

同等の c# コードは、以下に示します。

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

展開内で、使用可能な場合、ビューはより多くの領域を占有するかどうかを制御する、 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)します。 場合、`StackLayout`未使用領域が含まれています (つまり、`StackLayout`がそのすべての子の合計サイズより大きい)、未使用の領域に設定して展開を要求するすべての子ビューで均等には、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)または[ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)プロパティを[ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions)を使用するフィールド、`AndExpand`サフィックス。 場合にすべての領域、`StackLayout`が使用すると、拡張オプションがある影響しません。

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)の方向の方向で子ビューを展開します。 したがって、垂直方向`StackLayout`設定子ビューを展開することができます、 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)プロパティのいずれかを`StartAndExpand`、 `CenterAndExpand`、 `EndAndExpand`、または`FillAndExpand`場合、フィールド、 `StackLayout`未使用領域が含まれています。 同様に、水平方向`StackLayout`設定子ビューを展開することができます、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)プロパティのいずれかを`StartAndExpand`、 `CenterAndExpand`、 `EndAndExpand`、または`FillAndExpand`場合、フィールド、 `StackLayout`未使用領域が含まれています。

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)の方向とは反対方向に子ビューを展開することはできません。 したがって、垂直方向に`StackLayout`で、設定、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)プロパティで、子ビューに[ `StartAndExpand` ](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)プロパティを設定すると同じ効果[`Start`](xref:Xamarin.Forms.LayoutOptions.Start).

> [!NOTE]
> 注拡張の有効化が変更されないことをビューのサイズを使用しない限り[ `LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)します。

次の XAML コード例で、垂直方向に[ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 、それぞれの子[ `Label` ](xref:Xamarin.Forms.Label)設定その[ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)プロパティ次の 4 つの拡張フィールドのいずれかに、 [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions)構造体。

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

同等の c# コードは、以下に示します。

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

[![](layout-options-images/expansion.png "レイアウト オプションの拡張")](layout-options-images/expansion-large.png#lightbox "拡張レイアウト オプション")

各[ `Label` ](xref:Xamarin.Forms.Label)領域内の同じ量を占める、 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)します。 ただし、最後の文字だけ`Label`、セット、 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)プロパティを[ `FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)が別のサイズ。 さらに、各`Label`小さな赤いで区切られている[ `BoxView` ](xref:Xamarin.Forms.BoxView)、これにより、領域、`Label`占有を簡単に表示します。

## <a name="summary"></a>まとめ

この記事では、効果を説明する各[ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions)構造体の値がその親に対する相対的なビューの拡張と配置にします。 `Start`、 `Center`、`End`と`Fill`フィールドを使用して、親レイアウト内のビューの配置を定義し、 `StartAndExpand`、 `CenterAndExpand`、`EndAndExpand`と`FillAndExpand`定義に使用されるフィールド配置基本設定内で、使用可能な場合に、ビューがより多くの領域を占有するかどうかを判断して、 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)します。



## <a name="related-links"></a>関連リンク

- [LayoutOptions (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)
- [LayoutOptions](xref:Xamarin.Forms.LayoutOptions)
