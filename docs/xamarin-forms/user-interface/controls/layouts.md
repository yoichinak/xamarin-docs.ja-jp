---
title: Xamarin.Forms レイアウト
description: Xamarin.Forms のレイアウトは visual 構造にユーザー インターフェイス コントロールの作成に使用されます。
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: e9a4a661e694b5a885f202a36f9a2916c6c339fd
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms レイアウト

_Xamarin.Forms のレイアウトは visual 構造にユーザー インターフェイス コントロールの作成に使用されます。_

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout)と[ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) Xamarin.Forms クラスは、ビューおよびその他のレイアウト コンテナーとして機能するビューの特殊なサブタイプです。 `Layout`自体クラスから派生[ `View`](views.md)です。 A`Layout`派生には通常 Xamarin.Forms アプリケーションで、子要素のサイズと位置を設定するためのロジックが含まれています。

 [ ![](layouts-images/layouts-sml.png "Xamarin.Forms レイアウト型")](layouts-images/layouts.png#lightbox "Xamarin.Forms レイアウトの種類")

派生したクラス`Layout`2 つのカテゴリに分類できます。

## <a name="layouts-with-single-content"></a>単一のコンテンツのレイアウト

これらのクラスから派生して[ `Layout`](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)を定義する[ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)と[ `IsClippedToBounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.IsClippedToBounds/)プロパティです。

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) 設定されている 1 つの子が含まれています、 [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/)プロパティです。 `Content`プロパティは、いずれかに設定することができます`View`から派生したその他を含む`Layout`派生します。 `ContentView` ほとんどの場合、構造体の要素として使用されに基底クラスとして機能[ `Frame`](#frame)です。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) | [![ContentView 例](layouts-images/ContentView.png "ContentView 例")](layouts-images/ContentView-Large.png#lightbox "ContentView 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>フレーム

|     |     |
| --- | --- |
| [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/)クラスから派生[ `ContentView` ](#contentView)し、その子の周囲に四角形のフレームを表示します。 `Frame` 既定値を持つ[ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) 20 の値も定義と[ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/)、 [ `CornerRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.CornerRadius/)、および[ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/)プロパティです。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) | [![例をフレーム](layouts-images/Frame.png "例をフレーム")](layouts-images/Frame-Large.png#lightbox "フレームの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) その内容をスクロールできます。 設定、 [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/)プロパティを画面に収まるようにビューまたはレイアウトが大きすぎます。 (の内容、`ScrollView`は非常に多くの場合、 [ `StackLayout` ](#stackLayout))。設定、 [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/)プロパティを指定するかどうかのスクロールは、垂直方向に水平方向、またはその両方です。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) / [ガイド](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![ScrollView 例](layouts-images/ScrollView.png "ScrollView 例")](layouts-images/ScrollView-Large.png#lightbox "ScrollView 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) コントロール テンプレート付きコンテンツを表示しの基本クラスは、 [ `ContentView`](#contentView)です。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedView 例](layouts-images/TemplatedView.png "TemplatedView 例")](layouts-images/TemplatedView.png#lightbox "TemplatedView 例") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) 内で使用されるテンプレートのビューのレイアウト マネージャー、 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)マークが表示される内容が表示されます。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) / [ガイド](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![ContentPresenter 例](layouts-images/ContentPresenter.png "ContentPresenter 例")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter 例") |
|     |     |

## <a name="layouts-with-multiple-children"></a>複数の子でレイアウト

これらのクラスから派生して[ `Layout<View>`](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)です。

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 水平方向または垂直方向にに基づいてスタック内の子要素を移動、 [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/)プロパティです。 [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/)プロパティは、既定値は 6 があり、子の間のスペースを制御します。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) / [ガイド](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)| [![StackLayout 例](layouts-images/StackLayout.png "StackLayout 例")](layouts-images/StackLayout-Large.png#lightbox "StackLayout 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>グリッド

|     |     |
| --- | --- |
| [`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) 行と列のグリッドで、その子要素を配置します。 使用して子の位置が示されます、[アタッチされるプロパティ](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/)、 [ `Column` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/)、 [ `RowSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/)、および[ `ColumnSpan`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/)です。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) / [ガイド](~/xamarin-forms/user-interface/layouts/grid.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![グリッドの例](layouts-images/Grid.png "グリッド例")](layouts-images/Grid-Large.png#lightbox "グリッドの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) 子要素を親に対して特定の位置に配置します。 使用して子の位置が示されます、[アタッチされるプロパティ](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/)と[ `LayoutFlags`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/)です。 `AbsoluteLayout`はビューの位置をアニメーション化するのに便利です。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) / [ガイド](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![AbsoluteLayout 例](layouts-images/AbsoluteLayout.png "AbsoluteLayout 例")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) 基準に子要素を配置、`RelativeLayout`自体またはその兄弟にします。 使用して子の位置が示されます、[アタッチされるプロパティ](~/xamarin-forms/xaml/attached-properties.md)型のオブジェクトに設定されている[ `Constraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/)と[ `BoundsConstraint`](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/)です。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)/ [ガイド](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![[相対レイアウト] 例](layouts-images/RelativeLayout.png "[相対レイアウト] 例")](layouts-images/RelativeLayout-Large.png#lightbox "[相対レイアウト] の使用例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery サンプル](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API ドキュメント](https://developer.xamarin.com/api/root/Xamarin.Forms/)
