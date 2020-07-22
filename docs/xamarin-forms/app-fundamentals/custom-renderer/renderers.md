---
title: レンダラーの基本クラスおよびネイティブ コントロール
description: すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 この記事では、Xamarin.Forms のページ、レイアウト、ビュー、およびセルのそれぞれを実装するレンダラーとネイティブ コントロールのクラスの一覧を示します。
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/09/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 612200a23c198cbb1127119548c0a1dcc2928645
ms.sourcegitcommit: cd0c0999b53e825b60471bfbfd4144cfcd783587
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2020
ms.locfileid: "86225469"
---
# <a name="renderer-base-classes-and-native-controls"></a>レンダラーの基本クラスおよびネイティブ コントロール

_すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。この記事では、Xamarin.Forms のページ、レイアウト、ビュー、およびセルのそれぞれを実装するレンダラーとネイティブ コントロールのクラスの一覧を示します。_

`MapRenderer` クラスを除いて、プラットフォーム固有のレンダラーは、次の名前空間にあります。

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **Android (FastRenderers)**  - Xamarin.Forms.Platform.Android.FastRenderers
- **ユニバーサル Windows プラットフォーム (UWP)** – Xamarin.Forms.Platform.UWP

高速レンダラーの詳細については、「[Xamarin.Forms 高速レンダラー](~/xamarin-forms/internals/fast-renderers.md)」を参照してください。

`MapRenderer` クラスは、次の名前空間にあります。

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **ユニバーサル Windows プラットフォーム (UWP)** – Xamarin.Forms.Maps.UWP

> [!NOTE]
> シェル アプリケーション用のカスタム レンダラーの作成について詳しくは、「[Xamarin.Forms シェルのカスタム レンダラー](~/xamarin-forms/app-fundamentals/shell/customrenderers.md)」を参照してください。

## <a name="pages"></a>Pages

次の表に、Xamarin.Forms の各[ページ](~/xamarin-forms/user-interface/controls/pages.md)型を実装する、レンダラーおよびネイティブ コントロールのクラスの一覧を示します。

|ページ|レンダラー|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)|PhoneMasterDetailRenderer (iOS - 電話)、TabletMasterDetailPageRenderer (iOS - タブレット)、MasterDetailRenderer (Android)、MasterDetailPageRenderer (Android AppCompat)、MasterDetailPageRenderer (UWP)|UIViewController (電話)、UISplitViewController (タブレット)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (カスタム コントロール)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|NavigationRenderer (iOS および Android)、NavigationPageRenderer (Android AppCompat)、NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (カスタム コントロール)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|TabbedRenderer (iOS および Android)、TabbedPageRenderer (Android AppCompat)、TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (Pivot)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>レイアウト

次の表に、Xamarin.Forms の各[レイアウト](~/xamarin-forms/user-interface/controls/layouts.md)型を実装する、レンダラーおよびネイティブ コントロールのクラスの一覧を示します。

|レイアウト|レンダラー|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`ContentView`](xref:Xamarin.Forms.ContentView)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`FlexLayout`](xref:Xamarin.Forms.FlexLayout)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`Frame`](xref:Xamarin.Forms.Frame)|FrameRenderer|UIView|ViewGroup|CardView|Border|
|[`ScrollView`](xref:Xamarin.Forms.ScrollView)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`Grid`](xref:Xamarin.Forms.Grid)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`StackLayout`](xref:Xamarin.Forms.StackLayout)|ViewRenderer|UIView|View|View|FrameworkElement|

## <a name="views"></a>Views

次の表に、Xamarin.Forms の各[ビュー](~/xamarin-forms/user-interface/controls/views.md)型を実装する、レンダラーおよびネイティブ コントロールのクラスの一覧を示します。

|Views|レンダラー|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer (iOS および Android)、BoxViewRenderer (UWP)|UIView|ViewGroup||Rectangle|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|UIButton|Button|AppCompatButton|Button|
|[`CarouselView`](xref:Xamarin.Forms.CarouselView)|CarouselViewRenderer|UICollectionView||RecyclerView|ListViewBase|
|[`CheckBox`](xref:Xamarin.Forms.CheckBox)|CheckBoxRenderer|UIButton||AppCompatCheckBox|CheckBox|
|[`CollectionView`](xref:Xamarin.Forms.CollectionView)|CollectionViewRenderer|UICollectionView||RecyclerView|ListViewBase|
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|EditText||TextBox|
|[`Ellipse`](xref:Xamarin.Forms.Shapes.Ellipse)|EllipseRenderer|CALayer|View||Ellipse|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||Image|
|[`ImageButton`](xref:Xamarin.Forms.ImageButton)|ImageButtonRenderer|UIButton||AppCompatImageButton|Button|
|[`IndicatorView`](xref:Xamarin.Forms.IndicatorView)|IndicatorViewRenderer|UIPageControl||LinearLayout||
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|TextView||TextBlock|
|[`Line`](xref:Xamarin.Forms.Shapes.Line)|LineRenderer|CALayer|View||行|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)|MKMapView|MapView||MapControl|
|[`MediaElement`](xref:Xamarin.Forms.MediaElement)|MediaElementRenderer|UIView||VideoView|MediaElement|
|[`Path`](xref:Xamarin.Forms.Shapes.Path)|PathRenderer|CALayer|View||パス|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`Polygon`](xref:Xamarin.Forms.Shapes.Polygon)|PolygonRenderer|CALayer|View||多角形|
|[`Polyline`](xref:Xamarin.Forms.Shapes.Polyline)|PolylineRenderer|CALayer|View||ポリライン|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`RadioButton`](xref:Xamarin.Forms.RadioButton)|RadioButtonRenderer|UIButton||AppCompatRadioButton|RadioButton|
|[`Rectangle`](xref:Xamarin.Forms.Shapes.Rectangle)|RectangleRenderer|CALayer|View||Rectangle|
|[`RefreshView`](xref:Xamarin.Forms.RefreshView)|RefreshViewRenderer|UIView||SwipeRefreshLayout|RefreshContainer|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|SeekBar||Slider|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||Control|
|[`SwipeView`](xref:Xamarin.Forms.SwipeView)|SwipeViewRenderer|UIView||View|SwipeControl|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|Switch|SwitchCompat|ToggleSwitch|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](xref:Xamarin.Forms.WebView)|WkWebViewRenderer (iOS)、WebViewRenderer (Android および UWP)|WkWebView|WebView||WebView|

> [!NOTE]
> `Expander` コントロールは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) でアニメーションを使用して実装されます。 そのため、プラットフォーム レンダラーはありません。

## <a name="cells"></a>セル

次の表に、Xamarin.Forms の各[セル](~/xamarin-forms/user-interface/controls/cells.md)型を実装する、レンダラーおよびネイティブ コントロールのクラスの一覧を示します。

|セル|レンダラー|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|UITextField がある UITableViewCell|TextView および EditText がある LinearLayout|TextBox がある DataTemplate|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|UISwitch がある UITableViewCell|Switch|TextBlock と ToggleSwitch を含む Grid がある DataTemplate|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|2 つの TextViews がある LinearLayout|2 つの TextBlock を含む StackPanel がある DataTemplate|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|UIImage がある UITableViewCell|2 つの TextView と ImageView がある LinearLayout|Image と 2 つの TextBlock がある DataTemplate|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|View|ContentPresenter がある DataTemplate|

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms 高速レンダラー](~/xamarin-forms/internals/fast-renderers.md)
- [Xamarin.Forms シェルのカスタム レンダラー](~/xamarin-forms/app-fundamentals/shell/customrenderers.md)
