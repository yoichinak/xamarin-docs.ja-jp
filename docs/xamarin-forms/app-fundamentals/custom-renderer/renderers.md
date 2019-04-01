---
title: レンダラーの基本クラスおよびネイティブ コントロール
description: すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 この記事では、Xamarin.Forms の各ページ、レイアウト、ビュー、およびセルを実装する、レンダラーおよびネイティブ コントロールのクラスの一覧を示します。
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: cdca5294ea12bf8907ea5f6242efea00f384e77e
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329339"
---
# <a name="renderer-base-classes-and-native-controls"></a>レンダラーの基本クラスおよびネイティブ コントロール

_すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。この記事では、Xamarin.Forms の各ページ、レイアウト、ビュー、およびセルを実装する、レンダラーおよびネイティブ コントロールのクラスの一覧を示します。_

`MapRenderer` クラスを除いて、プラットフォーム固有のレンダラーは、次の名前空間にあります。

- **iOS** - Xamarin.Forms.Platform.iOS
- **Android** - Xamarin.Forms.Platform.Android
- **Android (AppCompat)** - Xamarin.Forms.Platform.Android.AppCompat
- **ユニバーサル Windows プラットフォーム (UWP)** - Xamarin.Forms.Platform.UWP

`MapRenderer`クラスは、次の名前空間にあります。

- **iOS** - Xamarin.Forms.Maps.iOS
- **Android** - Xamarin.Forms.Maps.Android
- **ユニバーサル Windows プラットフォーム (UWP)** - Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Pages

次の表は、Xamarin.Forms の各[ページ](~/xamarin-forms/user-interface/controls/pages.md)型を実装する、レンダラーおよびネイティブ コントロールのクラスの一覧を示します。

|ページ|レンダラー|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)|PhoneMasterDetailRenderer (iOS - 電話)、TabletMasterDetailPageRenderer (iOS - タブレット)、MasterDetailRenderer (Android)、MasterDetailPageRenderer (Android AppCompat)、MasterDetailPageRenderer (UWP)|UIViewController (電話)、UISplitViewController (タブレット)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (カスタム コントロール)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|NavigationRenderer (iOS および Android)、NavigationPageRenderer (Android AppCompat)、NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (カスタム コントロール)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|TabbedRenderer (iOS および Android)、TabbedPageRenderer (Android AppCompat)、TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (ピボット)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>レイアウト

次の表は、Xamarin.Forms の各[レイアウト](~/xamarin-forms/user-interface/controls/layouts.md)の種類を実装する、レンダラーおよびネイティブ コントロールのクラスの一覧を示します。

|レイアウト|レンダラー|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)|ViewRenderer|UIView|表示|FrameworkElement|
|[`ContentView`](xref:Xamarin.Forms.ContentView)|ViewRenderer|UIView|表示|FrameworkElement|
|[`FlexLayout`](xref:Xamarin.Forms.FlexLayout)|ViewRenderer|UIView|表示|FrameworkElement|
|[`Frame`](xref:Xamarin.Forms.Frame)|FrameRenderer|UIView|ViewGroup|境界線|
|[`ScrollView`](xref:Xamarin.Forms.ScrollView)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)|ViewRenderer|UIView|表示|FrameworkElement|
|[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)|ViewRenderer|UIView|表示|FrameworkElement|
|[`Grid`](xref:Xamarin.Forms.Grid)|ViewRenderer|UIView|表示|FrameworkElement|
|[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)|ViewRenderer|UIView|表示|FrameworkElement|
|[`StackLayout`](xref:Xamarin.Forms.StackLayout)|ViewRenderer|UIView|表示|FrameworkElement|

## <a name="views"></a>Views

次の表は、Xamarin.Forms の各[ビュー](~/xamarin-forms/user-interface/controls/views.md)の種類を実装する、レンダラーおよびネイティブ コントロールのクラスの一覧を示します。

|Views|レンダラー|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer (iOS および Android)、BoxViewRenderer (UWP)|UIView|ViewGroup||四角形|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|UIButton|ボタン|AppCompatButton|ボタン|
|`CollectionView`|CollectionViewRenderer|UICollectionView||RecyclerView||
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||イメージ|
|[`ImageButton`](xref:Xamarin.Forms.ImageButton)|ImageButtonRenderer|UIButton||AppCompatImageButton|ボタン|
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|SeekBar||スライダー|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||コントロール|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|切り替え|SwitchCompat|ToggleSwitch|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](xref:Xamarin.Forms.WebView)|WebViewRenderer|UIWebView|WebView||WebView|

## <a name="cells"></a>セル

次の表は、Xamarin.Forms の各[セル](~/xamarin-forms/user-interface/controls/cells.md)の種類を実装する、レンダラーおよびネイティブ コントロールのクラスの一覧を示します。

|セル|レンダラー|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|UITextField がある UITableViewCell|TextView および EditText がある LinearLayout|TextBox がある DataTemplate|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|UISwitch がある UITableViewCell|切り替え|TextBlock と ToggleSwitch を含む Grid がある DataTemplate|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|2 つの TextViews がある LinearLayout|2 つの TextBlock を含む StackPanel がある DataTemplate|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|UIImage がある UITableViewCell|2 つの TextView と ImageView がある LinearLayout|Image と 2 つの TextBlock がある DataTemplate|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|表示|ContentPresenter がある DataTemplate|

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms の各ページ、レイアウト、ビュー、およびセルを実装する、レンダラーおよびネイティブ コントロールのクラスの一覧を示しました。 すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。

## <a name="related-links"></a>関連リンク

- [カスタム レンダラー (Xamarin University のビデオ)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
