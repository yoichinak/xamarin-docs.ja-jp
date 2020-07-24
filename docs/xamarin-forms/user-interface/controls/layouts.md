---
title: :::no-loc(Xamarin.Forms):::レイアウト
description: ':::no-loc(Xamarin.Forms):::レイアウトは、ビジュアル構造にユーザーインターフェイスコントロールを作成するために使用されます。 この記事では、に含まれているレイアウトの一覧を示し :::no-loc(Xamarin.Forms)::: ます。'
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: 9b75ab8d3778770b37fee33c3b3f87d46d9ca5f0
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997307"
---
# <a name="no-locxamarinforms-layouts"></a>:::no-loc(Xamarin.Forms):::レイアウト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_:::no-loc(Xamarin.Forms):::レイアウトは、ユーザーインターフェイスコントロールをビジュアル構造に作成するために使用されます。_

[`Layout`](xref::::no-loc(Xamarin.Forms):::.Layout) [`Layout<T>`](xref::::no-loc(Xamarin.Forms):::.Layout`1) のクラスとクラス :::no-loc(Xamarin.Forms)::: は、ビューおよびその他のレイアウトのコンテナーとして機能するビューの特殊なサブタイプです。 `Layout`クラス自体は、から派生 [`View`](views.md) します。 通常、派生には、 `Layout` アプリケーション内の子要素の位置とサイズを設定するロジックが含まれ :::no-loc(Xamarin.Forms)::: ます。

[![::: no-loc (Xamarin. Forms)::: Layout 型](layouts-images/layouts-sml.png "::: no-loc (Xamarin. Forms)::: Layout 型")](layouts-images/layouts.png#lightbox "::: no-loc (Xamarin. Forms)::: Layout 型")

から派生するクラスは、 `Layout` 次の2つのカテゴリに分けることができます。

## <a name="layouts-with-single-content"></a>コンテンツが1つのレイアウト

これらのクラス [`Layout`](xref::::no-loc(Xamarin.Forms):::.Layout) [`Padding`](xref::::no-loc(Xamarin.Forms):::.Layout.Padding) は、プロパティとプロパティを定義するから派生し [`IsClippedToBounds`](xref::::no-loc(Xamarin.Forms):::.Layout.IsClippedToBounds) ます。

| 種類 | 説明 | 外観 |
| --- | --- | --- |
| `ContentView` | [`ContentView`](xref::::no-loc(Xamarin.Forms):::.ContentView)プロパティで設定された1つの子が含まれ [`Content`](xref::::no-loc(Xamarin.Forms):::.ContentView.Content) ます。 プロパティは、 `Content` `View` 他の派生を含む任意の派生物に設定でき `Layout` ます。 `ContentView`は、主に構造的要素として使用され、の基本クラスとして機能し [`Frame`](xref::::no-loc(Xamarin.Forms):::.Frame) ます。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.ContentView)  / [ガイド](~/xamarin-forms/user-interface/layouts/contentview.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/) | [![ContentView の例](layouts-images/ContentView.png "ContentView の例")](layouts-images/ContentView-Large.png#lightbox "ContentView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
| `Frame` | [`Frame`](xref::::no-loc(Xamarin.Forms):::.Frame)クラスはから派生 [`ContentView`](xref::::no-loc(Xamarin.Forms):::.ContentView) し、その子の周りに境界線またはフレームを表示します。 `Frame`クラスの既定値は [`Padding`](xref::::no-loc(Xamarin.Forms):::.Layout.Padding) 20 で、、、およびの各プロパティも定義し [`BorderColor`](xref::::no-loc(Xamarin.Forms):::.Frame.BorderColor) [`CornerRadius`](xref::::no-loc(Xamarin.Forms):::.Frame.CornerRadius) [`HasShadow`](xref::::no-loc(Xamarin.Forms):::.Frame.HasShadow) ます。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.Frame)  / [ガイド](~/xamarin-forms/user-interface/layouts/frame.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/) | [![フレームの例](layouts-images/Frame.png "フレームの例")](layouts-images/Frame-Large.png#lightbox "フレームの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
| `ScrollView` | [`ScrollView`](xref::::no-loc(Xamarin.Forms):::.ScrollView)はその内容をスクロールできます。 プロパティを [`Content`](xref::::no-loc(Xamarin.Forms):::.ScrollView.Content) ビューまたはレイアウトが大きすぎて画面に収まりません。 (のコンテンツは `ScrollView` 非常によくあり [`StackLayout`](xref::::no-loc(Xamarin.Forms):::.StackLayout) ます)。[`Orientation`](xref::::no-loc(Xamarin.Forms):::.ScrollView.Orientation)垂直方向、水平方向、または両方のスクロールを使用するかどうかを示すプロパティを設定します。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.ScrollView)  / [ガイド](~/xamarin-forms/user-interface/layouts/scrollview.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![ScrollView の例](layouts-images/ScrollView.png "ScrollView の例")](layouts-images/ScrollView-Large.png#lightbox "ScrollView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
| `TemplatedView` | [`TemplatedView`](xref::::no-loc(Xamarin.Forms):::.TemplatedView)コントロールテンプレートを使用してコンテンツを表示します。これはの基本クラスです [`ContentView`](xref::::no-loc(Xamarin.Forms):::.ContentView) 。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.TemplatedView)  / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![TemplatedView の例](layouts-images/TemplatedView.png "TemplatedView の例")](layouts-images/TemplatedView.png#lightbox "TemplatedView の例") |
| `ContentPresenter` | [`ContentPresenter`](xref::::no-loc(Xamarin.Forms):::.ContentPresenter)は、テンプレートビューのレイアウトマネージャーであり、内で使用され、表示 [`ControlTemplate`](xref::::no-loc(Xamarin.Forms):::.ControlTemplate) されるコンテンツの場所をマークします。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.ContentPresenter)  / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![ContentPresenter の例](layouts-images/ContentPresenter.png "ContentPresenter の例")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter の例") |
|     |     |     |

## <a name="layouts-with-multiple-children"></a>複数の子を持つレイアウト

これらのクラスはから派生し [`Layout<View>`](xref::::no-loc(Xamarin.Forms):::.Layout`1) ます。

| 種類 | 説明 | 外観 |
| --- | --- | --- |
| `StackLayout` | [`StackLayout`](xref::::no-loc(Xamarin.Forms):::.StackLayout)プロパティに基づいて、スタック内の子要素を水平方向または垂直方向に配置 [`Orientation`](xref::::no-loc(Xamarin.Forms):::.StackLayout.Orientation) します。 プロパティは、 [`Spacing`](xref::::no-loc(Xamarin.Forms):::.StackLayout.Spacing) 子間の間隔を制御し、既定値は6です。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.StackLayout)  / [ガイド](~/xamarin-forms/user-interface/layouts/stacklayout.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)| [![StackLayout の例](layouts-images/StackLayout.png "StackLayout の例")](layouts-images/StackLayout-Large.png#lightbox "StackLayout の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
| `Grid` | [`Grid`](xref::::no-loc(Xamarin.Forms):::.Grid)行と列のグリッドに子要素を配置します。 子の位置は、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)、、、およびを使用して示され [`Row`](xref::::no-loc(Xamarin.Forms):::.Grid.RowProperty) [`Column`](xref::::no-loc(Xamarin.Forms):::.Grid.ColumnProperty) [`RowSpan`](xref::::no-loc(Xamarin.Forms):::.Grid.RowSpanProperty) [`ColumnSpan`](xref::::no-loc(Xamarin.Forms):::.Grid.ColumnSpanProperty) ます。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.Grid)  / [ガイド](~/xamarin-forms/user-interface/layouts/grid.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![グリッドの例](layouts-images/Grid.png "グリッドの例")](layouts-images/Grid-Large.png#lightbox "グリッドの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
| `AbsoluteLayout` | [`AbsoluteLayout`](xref::::no-loc(Xamarin.Forms):::.AbsoluteLayout)親を基準として、特定の場所に子要素を配置します。 子の位置は、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)とを使用して示され [`LayoutBounds`](xref::::no-loc(Xamarin.Forms):::.AbsoluteLayout.LayoutBoundsProperty) [`LayoutFlags`](xref::::no-loc(Xamarin.Forms):::.AbsoluteLayout.LayoutFlagsProperty) ます。 `AbsoluteLayout`は、ビューの位置をアニメーション化する場合に便利です。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.AbsoluteLayout)  / [ガイド](~/xamarin-forms/user-interface/layouts/absolute-layout.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![AbsoluteLayout の例](layouts-images/AbsoluteLayout.png "AbsoluteLayout の例")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) |
| `RelativeLayout` | [`RelativeLayout`](xref::::no-loc(Xamarin.Forms):::.RelativeLayout)子要素を `RelativeLayout` それ自体またはその兄弟に相対的に配置します。 子の位置は、型および型のオブジェクトに設定されている[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)を使用して示され [`Constraint`](xref::::no-loc(Xamarin.Forms):::.Constraint) [`BoundsConstraint`](xref::::no-loc(Xamarin.Forms):::.Constraint) ます。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.RelativeLayout)  / [ガイド](~/xamarin-forms/user-interface/layouts/relative-layout.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![RelativeLayout の例](layouts-images/RelativeLayout.png "RelativeLayout の例")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
| `FlexLayout` | [`FlexLayout`](xref::::no-loc(Xamarin.Forms):::.FlexLayout)は、CSS[フレキシブルボックスレイアウトモジュール](https://www.w3.org/TR/css-flexbox-1/)に基づいています。これは、通常、_フレックスレイアウト_または_フレックスボックス_と呼ばれます。 `FlexLayout`6つのバインド可能なプロパティと5つのアタッチ可能なバインド可能なプロパティを定義します。これにより、複数の配置および向きのオプションで子を積み上げたりラップしたりできます。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.FlexLayout)  / [ガイド](~/xamarin-forms/user-interface/layouts/flex-layout.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos) | [![FlexLayout の例](layouts-images/FlexLayout.png "FlexLayout の例")](layouts-images/FlexLayout-Large.png#lightbox "FlexLayout の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |     |

## <a name="related-links"></a>関連リンク

- [:::no-loc(Xamarin.Forms):::フォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [:::no-loc(Xamarin.Forms)::: サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=:::no-loc(Xamarin.Forms):::)
- [:::no-loc(Xamarin.Forms):::API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
