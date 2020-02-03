---
title: Xamarin.Forms のレイアウト
description: Xamarin.Forms のレイアウトは、visual 構造にユーザー インターフェイス コントロールの作成に使用されます。 この記事では、Xamarin.Forms のレイアウトを示します。
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 4747ce6555a6440c687dc3d239d75307f68683ca
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724483"
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms のレイアウト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin. フォームレイアウトは、ユーザーインターフェイスコントロールをビジュアル構造に作成するために使用されます。_

Xamarin. Forms の[`Layout`](xref:Xamarin.Forms.Layout)クラスと[`Layout<T>`](xref:Xamarin.Forms.Layout`1)クラスは、ビューおよびその他のレイアウトのコンテナーとして機能するビューの特殊なサブタイプです。 `Layout` クラス自体は、 [`View`](views.md)から派生します。 `Layout` 派生物には通常、Xamarin. Forms アプリケーションで子要素の位置とサイズを設定するロジックが含まれています。

[![Xamarin. Forms レイアウトの種類](layouts-images/layouts-sml.png "Xamarin. Forms レイアウトの種類")](layouts-images/layouts.png#lightbox "Xamarin. Forms レイアウトの種類")

`Layout` から派生するクラスは、次の2つのカテゴリに分けることができます。

## <a name="layouts-with-single-content"></a>単一のコンテンツとレイアウト

これらのクラスは[`Layout`](xref:Xamarin.Forms.Layout)から派生し、 [`Padding`](xref:Xamarin.Forms.Layout.Padding)と[`IsClippedToBounds`](xref:Xamarin.Forms.Layout.IsClippedToBounds)プロパティを定義します。

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView)には、 [`Content`](xref:Xamarin.Forms.ContentView.Content)プロパティで設定された単一の子が含まれています。 `Content` プロパティは、他の `Layout` 派を含む任意の `View` 派生に設定できます。 `ContentView` は、主に構造的要素として使用され、 [`Frame`](#frame)する基底クラスとして機能します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ContentView) / [ガイド](~/xamarin-forms/user-interface/layouts/contentview.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/) | [![ContentView の例](layouts-images/ContentView.png "ContentView の例")](layouts-images/ContentView-Large.png#lightbox "ContentView の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) |
|     |     |

<a named="frame" />

### <a name="frame"></a>フレーム

|     |     |
| --- | --- |
| [`Frame`](xref:Xamarin.Forms.Frame)クラスは[`ContentView`](#contentView)から派生し、その子の周りに境界線またはフレームを表示します。 `Frame` クラスの既定の[`Padding`](xref:Xamarin.Forms.Layout.Padding)値は20であり、 [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor)、 [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)、および[`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow)の各プロパティも定義します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Frame) / [ガイド](~/xamarin-forms/user-interface/layouts/frame.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/) | [![フレームの例](layouts-images/Frame.png "フレームの例")](layouts-images/Frame-Large.png#lightbox "フレームの例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView)はその内容をスクロールできます。 [`Content`](xref:Xamarin.Forms.ScrollView.Content)プロパティをビューまたはレイアウトが大きすぎて画面に収まりません。 (`ScrollView` のコンテンツは、多くの場合[`StackLayout`](#stackLayout)です)。スクロールを垂直、水平、または両方のどちらにするかを示すには、 [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation)プロパティを設定します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ScrollView) / [ガイド](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![ScrollView の例](layouts-images/ScrollView.png "ScrollView の例")](layouts-images/ScrollView-Large.png#lightbox "ScrollView の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)はコントロールテンプレートを使用してコンテンツを表示し、は[`ContentView`](#contentView)の基本クラスです。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TemplatedView) / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![TemplatedView の例](layouts-images/TemplatedView.png "TemplatedView の例")](layouts-images/TemplatedView.png#lightbox "TemplatedView の例") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)は、 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)内で使用されるテンプレートビューのレイアウトマネージャーであり、表示されるコンテンツの場所をマークします。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ContentPresenter) / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![ContentPresenter の例](layouts-images/ContentPresenter.png "ContentPresenter の例")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter の例") |
|     |     |

## <a name="layouts-with-multiple-children"></a>複数の子要素のレイアウト

これらのクラスは[`Layout<View>`](xref:Xamarin.Forms.Layout`1)から派生します。

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout) 、 [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)プロパティに基づいて、スタック内の子要素を水平方向または垂直方向に配置します。 [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)プロパティは、子間の間隔を制御し、既定値は6です。<br /><br />[API ドキュメント](xref:Xamarin.Forms.StackLayout) / [ガイド](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)| [![StackLayout の例](layouts-images/StackLayout.png "StackLayout の例")](layouts-images/StackLayout-Large.png#lightbox "StackLayout の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) |
|     |     |

<a name="grid" />

### <a name="grid"></a>グリッド

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid)は、行と列のグリッドに子要素を配置します。 子の位置は、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md) [`Row`](xref:Xamarin.Forms.Grid.RowProperty)、 [`Column`](xref:Xamarin.Forms.Grid.ColumnProperty)、 [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty)、および[`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty)を使用して示されます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Grid) / [ガイド](~/xamarin-forms/user-interface/layouts/grid.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![グリッドの例](layouts-images/Grid.png "グリッドの例")](layouts-images/Grid-Large.png#lightbox "グリッドの例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)は、親を基準として相対的に子要素を特定の場所に配置します。 子の位置は、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md) [`LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty)および[`LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)を使用して示されます。 `AbsoluteLayout` は、ビューの位置をアニメーション化する場合に便利です。<br /><br />[API ドキュメント](xref:Xamarin.Forms.AbsoluteLayout) / [ガイド](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![AbsoluteLayout の例](layouts-images/AbsoluteLayout.png "AbsoluteLayout の例")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout の例")<br />このページのコード / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml)と[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutDemoPage.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)は、`RelativeLayout` 自体またはその兄弟に相対的に子要素を配置します。 子の位置は、 [`Constraint`](xref:Xamarin.Forms.Constraint)および[`BoundsConstraint`](xref:Xamarin.Forms.Constraint)型のオブジェクトに設定されている[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)を使用して示されます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.RelativeLayout) / [ガイド](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![RelativeLayout の例](layouts-images/RelativeLayout.png "RelativeLayout の例")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout)は、CSS[フレキシブルボックスレイアウトモジュール](https://www.w3.org/TR/css-flexbox-1/)に基づいています。このモジュールは、通常、_フレックスレイアウト_または_フレックスボックス_と呼ばれます。 `FlexLayout` は、6つのバインド可能なプロパティと5つのアタッチ可能なバインド可能なプロパティを定義します。これにより、複数の配置および向きのオプションで子を積み上げたりラップしたりできます<br /><br />[API ドキュメント](xref:Xamarin.Forms.FlexLayout) / [ガイド](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos) | [![FlexLayout の例](layouts-images/FlexLayout.png "FlexLayout の例")](layouts-images/FlexLayout-Large.png#lightbox "FlexLayout の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin フォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
