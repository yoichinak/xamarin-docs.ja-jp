---
title: Xamarin.Forms のレイアウト
description: Xamarin.Forms のレイアウトは、visual 構造にユーザー インターフェイス コントロールの作成に使用されます。 この記事では、Xamarin.Forms のレイアウトを示します。
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 520a90030200a1371b15331868ec6e325c6d3694
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53053332"
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms のレイアウト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/FormsGallery/)

_Xamarin.Forms のレイアウトは、visual 構造にユーザー インターフェイス コントロールの作成に使用されます。_

[ `Layout` ](xref:Xamarin.Forms.Layout)と[ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) Xamarin.Forms クラスは、ビュー、およびその他のレイアウト コンテナーとして機能するビューの特殊なサブタイプ。 `Layout`自体クラスから派生[ `View`](views.md)します。 A`Layout`派生物には通常 Xamarin.Forms アプリケーションの子要素のサイズと位置を設定するためのロジックが含まれています。

[![Xamarin.Forms のレイアウト型](layouts-images/layouts-sml.png "Xamarin.Forms レイアウト型")](layouts-images/layouts.png#lightbox "Xamarin.Forms のレイアウトの種類")

派生するクラス`Layout`2 つのカテゴリに分類できます。

## <a name="layouts-with-single-content"></a>単一のコンテンツとレイアウト

これらのクラスから派生[ `Layout` ](xref:Xamarin.Forms.Layout)、定義する[ `Padding` ](xref:Xamarin.Forms.Layout.Padding)と[ `IsClippedToBounds` ](xref:Xamarin.Forms.Layout.IsClippedToBounds)プロパティ。

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView) 設定されている 1 つの子が含まれています、 [ `Content` ](xref:Xamarin.Forms.ContentView.Content)プロパティ。 `Content`プロパティを任意に設定できます`View`などその他の派生物`Layout`派生クラス。 `ContentView` 構造体の要素として提供されるほとんどの場合と機能する基底クラスとして[ `Frame`](#frame)します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ContentView) | [![ContentView 例](layouts-images/ContentView.png "ContentView 例")](layouts-images/ContentView-Large.png#lightbox "ContentView 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>フレーム

|     |     |
| --- | --- |
| [ `Frame` ](xref:Xamarin.Forms.Frame)クラスから派生[ `ContentView` ](#contentView)し、その子の周囲に四角形のフレームを表示します。 `Frame` 既定値を持つ[ `Padding` ](xref:Xamarin.Forms.Layout.Padding) 20, の値も定義します[ `OutlineColor` ](xref:Xamarin.Forms.Frame.OutlineColor)、 [ `CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)と[ `HasShadow` ](xref:Xamarin.Forms.Frame.HasShadow)プロパティ。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Frame) | [![例のフレーム](layouts-images/Frame.png "フレーム例")](layouts-images/Frame-Large.png#lightbox "フレームの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView) その内容をスクロールすることができます。 設定、 [ `Content` ](xref:Xamarin.Forms.ScrollView.Content)プロパティを画面に合わせるには、表示またはレイアウトが大きすぎます。 (のコンテンツを`ScrollView`は非常に多くの場合、 [ `StackLayout` ](#stackLayout))。設定、 [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation)プロパティを指定するかどうかはスクロールは垂直、水平、またはその両方です。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ScrollView) / [ガイド](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![ScrollView 例](layouts-images/ScrollView.png "ScrollView 例")](layouts-images/ScrollView-Large.png#lightbox "ScrollView 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) コントロール テンプレートでのコンテンツを表示し、基本クラスです[ `ContentView`](#contentView)します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TemplatedView) / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedView 例](layouts-images/TemplatedView.png "TemplatedView 例")](layouts-images/TemplatedView.png#lightbox "TemplatedView 例") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) 内で使用される、テンプレート化されたビューのレイアウト マネージャー、 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)マークが表示されるコンテンツが表示されます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ContentPresenter) / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![ContentPresenter 例](layouts-images/ContentPresenter.png "ContentPresenter 例")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter の例") |
|     |     |

## <a name="layouts-with-multiple-children"></a>複数の子要素のレイアウト

これらのクラスから派生[ `Layout<View>`](xref:Xamarin.Forms.Layout`1)します。

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout) 水平方向または垂直方向にベースのいずれかのスタックに子要素を配置、 [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation)プロパティ。 [ `Spacing` ](xref:Xamarin.Forms.StackLayout.Spacing)プロパティは、子の間の間隔を制御し、6 の既定値を持ちます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.StackLayout) / [ガイド](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)| [![StackLayout 例](layouts-images/StackLayout.png "StackLayout 例")](layouts-images/StackLayout-Large.png#lightbox "StackLayout 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>グリッド

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid) 行と列のグリッドには、その子要素を配置します。 使用して、子の位置が示される、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](xref:Xamarin.Forms.Grid.RowProperty)、 [ `Column` ](xref:Xamarin.Forms.Grid.ColumnProperty)、 [ `RowSpan` ](xref:Xamarin.Forms.Grid.RowSpanProperty)、および[ `ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty)します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Grid) / [ガイド](~/xamarin-forms/user-interface/layouts/grid.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![グリッドの例](layouts-images/Grid.png "グリッド例")](layouts-images/Grid-Large.png#lightbox "グリッドの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) その親に対する相対的な特定の位置に子要素を配置します。 使用して、子の位置が示される、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty)と[ `LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)します。 `AbsoluteLayout`はビューの位置をアニメーション化するために便利です。<br /><br />[API ドキュメント](xref:Xamarin.Forms.AbsoluteLayout) / [ガイド](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![AbsoluteLayout の使用例](layouts-images/AbsoluteLayout.png "AbsoluteLayout の使用例")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout の使用例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) に対して相対的な子要素を配置、`RelativeLayout`自体または兄弟にします。 使用して、子の位置が示される、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)型のオブジェクトに設定されている[ `Constraint` ](xref:Xamarin.Forms.Constraint)と[ `BoundsConstraint`](xref:Xamarin.Forms.Constraint)します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.RelativeLayout) / [ガイド](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![[相対レイアウト] 例](layouts-images/RelativeLayout.png "[相対レイアウト] 例")](layouts-images/RelativeLayout-Large.png#lightbox "[相対レイアウト] の使用例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) CSS に基づいて[フレキシブル ボックス レイアウト モジュール](http://www.w3.org/TR/css-flexbox-1/)とよく呼ばれる、 _flex レイアウト_または_フレックス ボックス_します。 `FlexLayout` 6 つのバインド可能なプロパティと積み上げまたは多くの配置と向きのオプションを使用してラップする子を許可する 5 つの接続されているバインド可能なプロパティを定義します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.FlexLayout) / [ガイド](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/) | [![FlexLayout 例](layouts-images/FlexLayout.png "FlexLayout 例")](layouts-images/FlexLayout-Large.png#lightbox "FlexLayout 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery サンプル](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
