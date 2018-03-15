---
title: "レンダラーの基本クラスおよびネイティブ コントロール"
description: "各 Xamarin.Forms コントロールには、ネイティブなコントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 この記事では、レンダラーと各 Xamarin.Forms ページ、レイアウト、ビュー、およびセルを実装するコントロールのネイティブ クラスを示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 06887e6c1a39dd695fdaddb2fade8a463d9d4580
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="renderer-base-classes-and-native-controls"></a>レンダラーの基本クラスおよびネイティブ コントロール

_各 Xamarin.Forms コントロールには、ネイティブなコントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。この記事では、レンダラーと各 Xamarin.Forms ページ、レイアウト、ビュー、およびセルを実装するコントロールのネイティブ クラスを示します。_

例外を除いて、`MapRenderer`クラス、プラットフォーム固有のレンダラーが含まれる次の名前空間内。

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **ユニバーサル Windows プラットフォーム (UWP)** – Xamarin.Forms.Platform.UWP

`MapRenderer`次の名前空間のクラスが見つかりません。

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **ユニバーサル Windows プラットフォーム (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>ページ数

次の表は、レンダラーと各 Xamarin.Forms を実装するコントロールのネイティブ クラス[ページ](~/xamarin-forms/user-interface/controls/pages.md)型。

|ページ|レンダラー|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)|PhoneMasterDetailRenderer (– 電話)、iOS TabletMasterDetailPageRenderer タブレット (iOS –)、MasterDetailRenderer (Android)、MasterDetailPageRenderer (Android AppCompat)、MasterDetailPageRenderer (UWP)|UIViewController (Phone) UISplitViewController (タブレット)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (カスタム制御)|
|[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)|(IOS および Android) NavigationRenderer、NavigationPageRenderer (Android AppCompat)、NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (カスタム制御)|
|[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)|(IOS および Android) TabbedRenderer、TabbedPageRenderer (Android AppCompat)、TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (Pivot)|
|[`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>レイアウト

次の表は、レンダラーと各 Xamarin.Forms を実装するコントロールのネイティブ クラス[レイアウト](~/xamarin-forms/user-interface/controls/layouts.md)型。

|レイアウト|レンダラー|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)|ViewRenderer|UIView|表示|FrameworkElement|
|[`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)|ViewRenderer|UIView|表示|FrameworkElement|
|[`Frame`](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/)|FrameRenderer|UIView|ViewGroup|境界線|
|[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)|ViewRenderer|UIView|表示|FrameworkElement|
|[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)|ViewRenderer|UIView|表示|FrameworkElement|
|[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)|ViewRenderer|UIView|表示|FrameworkElement|
|[`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)|ViewRenderer|UIView|表示|FrameworkElement|
|[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)|ViewRenderer|UIView|表示|FrameworkElement|

## <a name="views"></a>ビュー

次の表は、レンダラーと各 Xamarin.Forms を実装するコントロールのネイティブ クラス[ビュー](~/xamarin-forms/user-interface/controls/views.md)型。

|ビュー|レンダラー|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)|(IOS および Android) BoxRenderer、BoxViewRenderer (Windows Phone および WinRT)|UIView|ViewGroup||四角形|
|[`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)|ButtonRenderer|UIButton|ボタン|AppCompatButton|ボタン|
|[`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/)|CarouselViewRenderer|UIScrollView|RecyclerView||flipView|
|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)|ImageRenderer|UIImageView|ImageView||イメージ|
|[`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)|SliderRenderer|UISlider|SeekBar||スライダー|
|[`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|StepperRenderer|UIStepper|LinearLayout||コントロール|
|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|SwitchRenderer|UISwitch|切り替え|SwitchCompat|ToggleSwitch|
|[`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)|WebViewRenderer|UIWebView|WebView||WebView|

## <a name="cells"></a>セル

次の表は、レンダラーと各 Xamarin.Forms を実装するコントロールのネイティブ クラス[セル](~/xamarin-forms/user-interface/controls/cells.md)型。

|セル|レンダラー|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/)|EntryCellRenderer|UITextField UITableViewCell|TextView EditText と LinearLayout|データ テンプレートのテキスト ボックス|
|[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/)|SwitchCellRenderer|UISwitch UITableViewCell|切り替え|DataTemplate を TextBlock と ToggleSwitch を含むグリッド|
|[`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/)|TextCellRenderer|UITableViewCell|2 つするテキスト ビューで LinearLayout|2 つのテキスト ブロックを含む StackPanel で DataTemplate|
|[`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/)|ImageCellRenderer|UIImage UITableViewCell|2 つするテキスト ビューと、ImageView LinearLayout|DataTemplate をイメージと 2 つのテキスト ブロックを含むグリッド|
|[`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|表示|DataTemplate、ContentPresenter と|

## <a name="summary"></a>まとめ

この記事には、レンダラーと各 Xamarin.Forms ページ、レイアウト、ビュー、およびセルを実装するコントロールのネイティブ クラスが一覧表示されます。 各 Xamarin.Forms コントロールには、ネイティブなコントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。

## <a name="related-links"></a>関連リンク

- [カスタム レンダラー (Xamarin 大学ビデオ)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
