---
title: 余白とスペース
description: マージンと埋め込みのプロパティは、要素は、ユーザー インターフェイスに表示されるときに、レイアウトの動作を制御します。 この記事では、2 つのプロパティとその設定方法の違いを示します。
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 595e673c59d23a45cbaf923a0d58faff2000c296
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61370940"
---
# <a name="margin-and-padding"></a>余白とスペース

_マージンと埋め込みのプロパティは、要素は、ユーザー インターフェイスに表示されるときに、レイアウトの動作を制御します。この記事では、2 つのプロパティとその設定方法の違いを示します。_

## <a name="overview"></a>概要

余白とスペースは、関連するレイアウトの概念です。

- [ `Margin` ](xref:Xamarin.Forms.View.Margin)プロパティは、要素とその隣接する要素間の距離を表し、要素のレンダリング位置、およびその近隣のレンダリング位置を制御するために使用します。 `Margin` 値を指定できる[レイアウト](~/xamarin-forms/user-interface/controls/layouts.md)と[ビュー](~/xamarin-forms/user-interface/controls/views.md)クラス。
- [ `Padding` ](xref:Xamarin.Forms.Layout.Padding)プロパティは、要素とその子要素の間の距離を表すし、独自のコンテンツからコントロールを分離するために使用します。 `Padding` 値を指定できる[レイアウト](~/xamarin-forms/user-interface/controls/layouts.md)クラス。

次の図は、2 つの概念を示しています。

[![](margin-and-padding-images/margins-and-padding-sml.png "余白とパディング概念")](margin-and-padding-images/margins-and-padding.png#lightbox "余白とパディングの概念")

なお[ `Margin` ](xref:Xamarin.Forms.View.Margin)値は、加法です。 したがって、2 つの隣接する要素は、20 ピクセルの余白を指定する要素間の距離は 40 ピクセルになります。 さらに、余白やパディングが加法の両方が適用されると、要素およびそのコンテンツ間の距離は、余白とパディングとなることです。

## <a name="specifying-a-thickness"></a>太さを指定します。

[ `Margin` ](xref:Xamarin.Forms.View.Margin)と[ `Padding` ](xref:Xamarin.Forms.Layout.Padding)プロパティは、型のどちらも[ `Thickness`](xref:Xamarin.Forms.Thickness)します。 作成するときに次の 3 つの可能性がある、`Thickness`構造体。

- 作成、 [ `Thickness` ](xref:Xamarin.Forms.Thickness)統一された単一の値によって定義された構造体。 1 つの値は、左、上、右、要素の下の辺に適用されます。
- 作成、 [ `Thickness` ](xref:Xamarin.Forms.Thickness)水平および垂直方向の値によって定義された構造体。 水平方向の値は、要素の上端と下端の両側に対称的に適用される垂直方向の値を持つ要素の左側および右側に対称的に適用されます。
- 作成、 [ `Thickness` ](xref:Xamarin.Forms.Thickness)左、上、右、要素の下の辺に適用される 4 つの個別の値によって定義された構造体。

次の XAML コード例では、次の 3 つすべての可能性を示します。

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
> `Thickness` 値は、通常クリップまたは overdraws コンテンツが負の値で指定できます。

## <a name="summary"></a>まとめ

この記事では、間の差を示す、 [ `Margin` ](xref:Xamarin.Forms.View.Margin)と[ `Padding` ](xref:Xamarin.Forms.Layout.Padding)プロパティ、およびそれらを設定する方法。 プロパティは、ユーザー インターフェイスの要素が表示されるときに、レイアウトの動作を制御します。


## <a name="related-links"></a>関連リンク

- [余白](xref:Xamarin.Forms.View.Margin)
- [パディング](xref:Xamarin.Forms.Layout.Padding)
- [太さ](xref:Xamarin.Forms.Thickness)
