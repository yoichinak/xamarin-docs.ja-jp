---
title: Xamarin.Formsレイアウト
description: Xamarin.Formsレイアウトは、ビジュアル構造にユーザーインターフェイスコントロールを作成するために使用されます。 この記事では、に含まれているレイアウトの一覧を示し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 167ac8ffc7416facb41083e97a69cb1c981ba0a3
ms.sourcegitcommit: c3329ab25d377907d8804cdd5e26dc84a274f39c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/12/2020
ms.locfileid: "88130956"
---
# <a name="no-locxamarinforms-layouts"></a>Xamarin.Formsレイアウト

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin.Formsレイアウトは、ユーザーインターフェイスコントロールをビジュアル構造に作成するために使用されます。_

[`Layout`](xref:Xamarin.Forms.Layout) [`Layout<T>`](xref:Xamarin.Forms.Layout`1) のクラスとクラス Xamarin.Forms は、ビューおよびその他のレイアウトのコンテナーとして機能するビューの特殊なサブタイプです。 `Layout`クラス自体は、から派生 [`View`](views.md) します。 通常、派生には、 `Layout` アプリケーション内の子要素の位置とサイズを設定するロジックが含まれ Xamarin.Forms ます。

[![::: no-loc (Xamarin. Forms)::: Layout 型](layouts-images/layouts-sml.png "::: no-loc (Xamarin. Forms)::: Layout 型")](layouts-images/layouts.png#lightbox "::: no-loc (Xamarin. Forms)::: Layout 型")

から派生するクラスは、 `Layout` 次の2つのカテゴリに分けることができます。

## <a name="layouts-with-single-content"></a>コンテンツが1つのレイアウト

これらのクラス [`Layout`](xref:Xamarin.Forms.Layout) [`Padding`](xref:Xamarin.Forms.Layout.Padding) は、プロパティとプロパティを定義するから派生し [`IsClippedToBounds`](xref:Xamarin.Forms.Layout.IsClippedToBounds) ます。

| Type | 説明 | 外観 |
| --- | --- | --- |
| `ContentView` | [`ContentView`](xref:Xamarin.Forms.ContentView)プロパティで設定された1つの子が含まれ [`Content`](xref:Xamarin.Forms.ContentView.Content) ます。 プロパティは、 `Content` `View` 他の派生を含む任意の派生物に設定でき `Layout` ます。 `ContentView`は、主に構造的要素として使用され、の基本クラスとして機能し [`Frame`](xref:Xamarin.Forms.Frame) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ContentView)  / [ガイド](~/xamarin-forms/user-interface/layouts/contentview.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/) | [![ContentView の例](layouts-images/ContentView.png "ContentView の例")](layouts-images/ContentView-Large.png#lightbox "ContentView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
| `Frame` | [`Frame`](xref:Xamarin.Forms.Frame)クラスはから派生 [`ContentView`](xref:Xamarin.Forms.ContentView) し、その子の周りに境界線またはフレームを表示します。 `Frame`クラスの既定値は [`Padding`](xref:Xamarin.Forms.Layout.Padding) 20 で、、、およびの各プロパティも定義し [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor) [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius) [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Frame)  / [ガイド](~/xamarin-forms/user-interface/layouts/frame.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/) | [![フレームの例](layouts-images/Frame.png "フレームの例")](layouts-images/Frame-Large.png#lightbox "フレームの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
| `ScrollView` | [`ScrollView`](xref:Xamarin.Forms.ScrollView)はその内容をスクロールできます。 プロパティを [`Content`](xref:Xamarin.Forms.ScrollView.Content) ビューまたはレイアウトが大きすぎて画面に収まりません。 (のコンテンツは `ScrollView` 非常によくあり [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます)。[`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation)垂直方向、水平方向、または両方のスクロールを使用するかどうかを示すプロパティを設定します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ScrollView)  / [ガイド](~/xamarin-forms/user-interface/layouts/scrollview.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![ScrollView の例](layouts-images/ScrollView.png "ScrollView の例")](layouts-images/ScrollView-Large.png#lightbox "ScrollView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
| `TemplatedView` | [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)コントロールテンプレートを使用してコンテンツを表示します。これはの基本クラスです [`ContentView`](xref:Xamarin.Forms.ContentView) 。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TemplatedView)  / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![TemplatedView の例](layouts-images/TemplatedView.png "TemplatedView の例")](layouts-images/TemplatedView.png#lightbox "TemplatedView の例") |
| `ContentPresenter` | [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)は、テンプレートビューのレイアウトマネージャーであり、内で使用され、表示 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) されるコンテンツの場所をマークします。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ContentPresenter)  / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![ContentPresenter の例](layouts-images/ContentPresenter.png "ContentPresenter の例")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter の例") |
|     |     |     |

## <a name="layouts-with-multiple-children"></a>複数の子を持つレイアウト

これらのクラスはから派生し [`Layout<View>`](xref:Xamarin.Forms.Layout`1) ます。

| Type | 説明 | 外観 |
| --- | --- | --- |
| `StackLayout` | [`StackLayout`](xref:Xamarin.Forms.StackLayout)プロパティに基づいて、スタック内の子要素を水平方向または垂直方向に配置 [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) します。 プロパティは、 [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) 子間の間隔を制御し、既定値は6です。<br /><br />[API ドキュメント](xref:Xamarin.Forms.StackLayout)  / [ガイド](~/xamarin-forms/user-interface/layouts/stacklayout.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)| [![StackLayout の例](layouts-images/StackLayout.png "StackLayout の例")](layouts-images/StackLayout-Large.png#lightbox "StackLayout の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
| `Grid` | [`Grid`](xref:Xamarin.Forms.Grid)行と列のグリッドに子要素を配置します。 子の位置は、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)、、、およびを使用して示され [`Row`](xref:Xamarin.Forms.Grid.RowProperty) [`Column`](xref:Xamarin.Forms.Grid.ColumnProperty) [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Grid)  / [ガイド](~/xamarin-forms/user-interface/layouts/grid.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![グリッドの例](layouts-images/Grid.png "グリッドの例")](layouts-images/Grid-Large.png#lightbox "グリッドの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
| `AbsoluteLayout` | [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)親を基準として、特定の場所に子要素を配置します。 子の位置は、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)とを使用して示され [`LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) [`LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) ます。 `AbsoluteLayout`は、ビューの位置をアニメーション化する場合に便利です。<br /><br />[API ドキュメント](xref:Xamarin.Forms.AbsoluteLayout)  / [ガイド](~/xamarin-forms/user-interface/layouts/absolutelayout.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![AbsoluteLayout の例](layouts-images/AbsoluteLayout.png "AbsoluteLayout の例")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) |
| `RelativeLayout` | [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)子要素を `RelativeLayout` それ自体またはその兄弟に相対的に配置します。 子の位置は、型および型のオブジェクトに設定されている[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)を使用して示され [`Constraint`](xref:Xamarin.Forms.Constraint) [`BoundsConstraint`](xref:Xamarin.Forms.Constraint) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.RelativeLayout)  / [ガイド](~/xamarin-forms/user-interface/layouts/relative-layout.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![RelativeLayout の例](layouts-images/RelativeLayout.png "RelativeLayout の例")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
| `FlexLayout` | [`FlexLayout`](xref:Xamarin.Forms.FlexLayout)は、CSS[フレキシブルボックスレイアウトモジュール](https://www.w3.org/TR/css-flexbox-1/)に基づいています。これは、通常、_フレックスレイアウト_または_フレックスボックス_と呼ばれます。 `FlexLayout`6つのバインド可能なプロパティと5つのアタッチ可能なバインド可能なプロパティを定義します。これにより、複数の配置および向きのオプションで子を積み上げたりラップしたりできます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.FlexLayout)  / [ガイド](~/xamarin-forms/user-interface/layouts/flex-layout.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos) | [![FlexLayout の例](layouts-images/FlexLayout.png "FlexLayout の例")](layouts-images/FlexLayout-Large.png#lightbox "FlexLayout の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Formsフォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
