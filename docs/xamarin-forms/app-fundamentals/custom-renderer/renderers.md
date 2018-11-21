---
title: レンダラーの基本クラスおよびネイティブ コントロール
description: すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 この記事では、レンダラー、および各 Xamarin.Forms ページ、レイアウト、ビュー、およびセルを実装するネイティブ コントロール クラスを示します。
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: 56df2f7e6b83ddd4a5780506471cbd32a3aced40
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171951"
---
# <a name="renderer-base-classes-and-native-controls"></a>レンダラーの基本クラスおよびネイティブ コントロール

_すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。この記事では、レンダラー、および各 Xamarin.Forms ページ、レイアウト、ビュー、およびセルを実装するネイティブ コントロール クラスを示します。_

例外として、`MapRenderer`クラス、プラットフォーム固有のレンダラーは、次の名前空間で見つかんだことができます。

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **ユニバーサル Windows プラットフォーム (UWP)** – Xamarin.Forms.Platform.UWP

`MapRenderer`クラスは次の名前空間にあります。

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **ユニバーサル Windows プラットフォーム (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>ページ数

次の表は、レンダラー、および各 Xamarin.Forms を実装するネイティブ コントロール クラス[ページ](~/xamarin-forms/user-interface/controls/pages.md)型。

|ページ|レンダラー|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)|PhoneMasterDetailRenderer (iOS – Phone)、TabletMasterDetailPageRenderer (iOS – タブレット)、MasterDetailRenderer (Android)、MasterDetailPageRenderer (Android AppCompat)、MasterDetailPageRenderer (UWP)|UIViewController (電話) UISplitViewController (タブレット)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (カスタム コントロール)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|NavigationRenderer (iOS と Android)、NavigationPageRenderer (Android AppCompat)、NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (カスタム コントロール)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|TabbedRenderer (iOS と Android)、TabbedPageRenderer (Android AppCompat)、TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (ピボット)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>レイアウト

次の表は、レンダラー、および各 Xamarin.Forms を実装するネイティブ コントロール クラス[レイアウト](~/xamarin-forms/user-interface/controls/layouts.md)型。

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

次の表は、レンダラー、および各 Xamarin.Forms を実装するネイティブ コントロール クラス[ビュー](~/xamarin-forms/user-interface/controls/views.md)型。

|Views|レンダラー|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer (iOS と Android)、BoxViewRenderer (UWP)|UIView|ViewGroup||四角形|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|UIButton|ボタン|AppCompatButton|ボタン|
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||イメージ|
|`ImageButton`|ImageButtonRenderer|UIButton||AppCompatImageButton|ボタン|
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|検索ビュー||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|あり||スライダー|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||コントロール|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|切り替え|SwitchCompat|ToggleSwitch|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](xref:Xamarin.Forms.WebView)|WebViewRenderer|UIWebView|WebView||WebView|

## <a name="cells"></a>セル

次の表は、レンダラー、および各 Xamarin.Forms を実装するネイティブ コントロール クラス[セル](~/xamarin-forms/user-interface/controls/cells.md)型。

|セル|レンダラー|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|UITextField で UITableViewCell|TextView と EditText LinearLayout|TextBox で DataTemplate|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|UISwitch UITableViewCell|切り替え|DataTemplate の TextBlock と ToggleSwitch を含むグリッド|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|2 つするテキスト ビューで LinearLayout|2 つの Textblock を含む StackPanel と DataTemplate|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|UIImage UITableViewCell|2 つするテキスト ビューと、ImageView LinearLayout|イメージと 2 つの Textblock を含む Grid と DataTemplate|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|表示|Contentpresenter DataTemplate|

## <a name="summary"></a>まとめ

この記事には、レンダラー、および各 Xamarin.Forms ページ、レイアウト、ビュー、およびセルを実装するネイティブ コントロールのクラスが一覧表示されます。 すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。

## <a name="related-links"></a>関連リンク

- [カスタム レンダラー (Xamarin University のビデオ)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
