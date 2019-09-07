---
title: 余白とスペース
description: 余白と余白のプロパティは、要素がユーザーインターフェイスに表示されるときのレイアウト動作を制御します。 この記事では、2つのプロパティの違いとその設定方法について説明します。
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 66ac81631466131cf1ef44dde39aa768d31b65a1
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772487"
---
# <a name="margin-and-padding"></a>余白とスペース

_余白と余白のプロパティは、要素がユーザーインターフェイスに表示されるときのレイアウト動作を制御します。この記事では、2つのプロパティの違いとその設定方法について説明します。_

## <a name="overview"></a>概要

余白と埋め込みは、関連するレイアウトの概念です。

- プロパティ[`Margin`](xref:Xamarin.Forms.View.Margin)は、要素とそれに隣接する要素との距離を表し、要素のレンダリング位置と、その隣接する描画位置を制御するために使用されます。 `Margin`[レイアウト](~/xamarin-forms/user-interface/controls/layouts.md)および[ビュー](~/xamarin-forms/user-interface/controls/views.md)クラスに値を指定できます。
- プロパティ[`Padding`](xref:Xamarin.Forms.Layout.Padding)は、要素とその子要素間の距離を表し、コントロールを独自のコンテンツから分離するために使用されます。 `Padding`[レイアウト](~/xamarin-forms/user-interface/controls/layouts.md)クラスで値を指定できます。

次の図は、2つの概念を示しています。

[余白とパディングの概念![(margin-and-padding-images/margins-and-padding-sml.png " ")]](margin-and-padding-images/margins-and-padding.png#lightbox "余白とパディングの概念")

[`Margin`](xref:Xamarin.Forms.View.Margin)値は加法です。 したがって、2つの隣接する要素が20ピクセルの余白を指定すると、要素間の距離は40ピクセルになります。 さらに、余白と余白は両方とも適用されるときに追加されます。これは、要素とコンテンツとの間の距離が余白と埋め込みであることを示します。

## <a name="specifying-a-thickness"></a>太さの指定

プロパティとプロパティは両方と[`Padding`](xref:Xamarin.Forms.Layout.Padding)も[`Thickness`](xref:Xamarin.Forms.Thickness)型です。 [`Margin`](xref:Xamarin.Forms.View.Margin) 構造体を作成する場合、 `Thickness`次の3つの可能性があります。

- 1つ[`Thickness`](xref:Xamarin.Forms.Thickness)の均一な値で定義された構造体を作成します。 要素の左、上、右、および下側に1つの値が適用されます。
- 水平および[`Thickness`](xref:Xamarin.Forms.Thickness)垂直の値で定義された構造体を作成します。 水平値は、要素の左右左右に対称的に適用されます。垂直方向の値は、要素の上下左右に対称的に適用されます。
- 要素の[`Thickness`](xref:Xamarin.Forms.Thickness)左、上、右、および下側に適用される4つの個別の値によって定義される構造体を作成します。

次の XAML コード例は、3つのすべての可能性を示しています。

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

これと同じ C# コードの例は次のとおりです。

```csharp
var stackLayout = new StackLayout {
  Padding = new Thickness(0,20,0,0),
  Children = {
    new Label { Text = "Xamarin.Forms", Margin = new Thickness (20) },
    new Label { Text = "Xamarin.iOS", Margin = new Thickness (10, 25) },
    new Label { Text = "Xamarin.Android", Margin = new Thickness (0, 20, 15, 5) }
  }
};
```

> [!NOTE]
> `Thickness`値には負の値を指定できます。通常は、コンテンツをクリップまたは過剰に描画します。

## <a name="summary"></a>Summary

この記事では、プロパティと[`Margin`](xref:Xamarin.Forms.View.Margin) [`Padding`](xref:Xamarin.Forms.Layout.Padding)プロパティの違いと、プロパティの設定方法について説明します。 プロパティは、ユーザーインターフェイスで要素がレンダリングされるときのレイアウト動作を制御します。

## <a name="related-links"></a>関連リンク

- [余白](xref:Xamarin.Forms.View.Margin)
- [余白](xref:Xamarin.Forms.Layout.Padding)
- [太さ](xref:Xamarin.Forms.Thickness)
