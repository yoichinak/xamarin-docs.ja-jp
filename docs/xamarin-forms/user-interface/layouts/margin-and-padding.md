---
title: マージンと埋め込み
description: マージンと埋め込みのプロパティは、要素がユーザー インターフェイスに表示されるときに、レイアウトの動作を制御します。 この記事では、2 つのプロパティとその設定方法の違いについて説明します。
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 545468d3b02f9651c45fcaebe159351aafea6432
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30790601"
---
# <a name="margin-and-padding"></a>マージンと埋め込み

_マージンと埋め込みのプロパティは、要素がユーザー インターフェイスに表示されるときに、レイアウトの動作を制御します。この記事では、2 つのプロパティとその設定方法の違いについて説明します。_

## <a name="overview"></a>概要

マージンと埋め込みは関連するレイアウトの概念です。

- [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)プロパティは、要素と、隣接する要素間の距離を表し、要素の描画位置、およびその近隣の描画位置を制御するために使用します。 `Margin` 値を指定できる[レイアウト](~/xamarin-forms/user-interface/controls/layouts.md)と[ビュー](~/xamarin-forms/user-interface/controls/views.md)クラスです。
- [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)プロパティは、要素とその子要素間の距離を表し、コントロールを独自のコンテンツから分離するために使用します。 `Padding` 値を指定できる[レイアウト](~/xamarin-forms/user-interface/controls/layouts.md)クラスです。

次の図は、2 つの概念を示しています。

[![](margin-and-padding-images/margins-and-padding-sml.png "マージンと埋め込み概念")](margin-and-padding-images/margins-and-padding.png#lightbox "マージンと埋め込みの概念")

なお[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)値は加法です。 したがって、2 つの隣接する要素は、20 ピクセルの余白を指定する、要素間の距離は 40 ピクセルになります。 さらに、余白とスペースは加算的で両方が適用されると、要素とそのコンテンツ間の距離は、マージンと埋め込みとなることです。

## <a name="specifying-a-thickness"></a>太さを指定します。

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)と[ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)プロパティが型の両方とも[ `Thickness`](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)です。 作成するときに次の 3 つの可能性がある、`Thickness`構造体。

- 作成、 [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) 1 つの統一された値によって定義された構造です。 1 つの値は、左、上、右、および要素の下の辺に適用されます。
- 作成、 [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)水平および垂直方向の値によって定義された構造です。 水平方向の値は、要素の上端と下端の辺に対称的に適用される垂直方向の値を持つ要素の左右に対称的に適用されます。
- 作成、 [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)左、上、右、および要素の下の辺に適用されている 4 つの個別の値によって定義された構造です。

次の XAML コードの例は、次の 3 つすべての可能性を示しています。

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
> `Thickness` 値は、通常クリップまたはコンテンツを overdraws 負の値を指定できます。

## <a name="summary"></a>まとめ

この記事の内容に違いが示されている、 [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)と[ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)プロパティ、およびその設定方法です。 プロパティは、要素は、ユーザー インターフェイスに表示されるときに、レイアウトの動作を制御します。


## <a name="related-links"></a>関連リンク

- [マージン](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)
- [Padding](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)
- [太さ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)
