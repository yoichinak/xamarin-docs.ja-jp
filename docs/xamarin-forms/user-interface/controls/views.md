---
title: Xamarin.Forms のビュー
description: Xamarin.Forms のビューとは、クロス プラットフォーム モバイルのユーザー インターフェイスの構成要素です。 この記事では、Xamarin.Forms に含まれているビューを一覧表示します。
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: 0094fbc73e88dc4e84d8bf415db30c17f955ddcc
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "69976600"
---
# <a name="xamarinforms-views"></a>Xamarin.Forms のビュー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin.Forms のビューとは、クロス プラットフォーム モバイルのユーザー インターフェイスの構成要素です。_

ビューは、ラベル、ボタン、およびとよく呼ばれるスライダーなどのユーザー インターフェイス オブジェクト*コントロール*または*ウィジェット*他のグラフィカルなプログラミング環境でします。 すべての派生を Xamarin.Forms でサポートされるビュー、 [ `View` ](xref:Xamarin.Forms.View)クラス。 これらは、いくつかのカテゴリに分類できます。

## <a name="views-for-presentation"></a>プレゼンテーションのビュー

### <a name="label"></a>group1

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label) 単一行のテキスト文字列または文字列定数または変数の書式設定のいずれかの複数行ブロックが表示されます。 設定、 [ `Text` ](xref:Xamarin.Forms.Label.Text)定数の書式設定、またはセット用の文字列にプロパティ、 [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText)プロパティを[ `FormattedString` ](xref:Xamarin.Forms.FormattedString)変数のオブジェクト書式設定します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Label) / [ガイド](~/xamarin-forms/user-interface/text/label.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![ラベルの例](views-images/Label.png "ラベルの例")](views-images/Label-Large.png#lightbox "ラベルの例")<br /> [このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>イメージ

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image) ビットマップが表示されます。 ビットマップは、一般的なプロジェクトまたはプラットフォームのプロジェクトにリソースとして埋め込まれてまたは .NET を使用して作成、Web 経由でダウンロードできます`Stream`オブジェクト。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Image) / [ガイド](~/xamarin-forms/user-interface/images.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![画像の例](views-images/Image.png "画像の例")](views-images/Image-Large.png#lightbox "画像の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) ごとに色分けされて実線の四角形が表示されます、 [ `Color` ](xref:Xamarin.Forms.BoxView.Color)プロパティ。 `BoxView` 40 x 40 の既定サイズの要求があります。 他のサイズを割り当てる、 [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest)と[ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest)プロパティ。<br /><br />[API ドキュメント](xref:Xamarin.Forms.BoxView) / [ガイド](~/xamarin-forms/user-interface/boxview.md) / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)、 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration)、 [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/)、 [4](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)、 [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)、および[6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![Boxview の例](views-images/BoxView.png "Boxview の例")](views-images/BoxView-Large.png#lightbox "BoxView の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) かどうかに基づいて Web ページまたは HTML コンテンツを表示、 [ `Source` ](xref:Xamarin.Forms.WebView.Source)プロパティに設定されて、 [ `UriWebViewSource` ](xref:Xamarin.Forms.UrlWebViewSource)または[ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource)オブジェクト。<br /><br />[API ドキュメント](xref:Xamarin.Forms.WebView) / [ガイド](~/xamarin-forms/user-interface/webview.md) / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)と[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![WebView の例](views-images/WebView.png "WebView の例")](views-images/WebView-Large.png#lightbox "WebView の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView) iOS と Android プロジェクトでは、OpenGL のグラフィックスを表示します。 ユニバーサル Windows プラットフォームのサポートはありません。 IOS と Android プロジェクトへの参照が必要、 **OpenTK 1.0**アセンブリまたは**OpenTK**バージョン 1.0.0.0 アセンブリ。 `OpenGLView` 共有プロジェクトで使用する方が簡単です.NET Standard ライブラリで使用されている場合は、依存関係サービスが (サンプル コードで示す) のように必要にもなります。<br /><br />これは、xamarin. フォームに組み込まれている唯一のグラフィックス機能ですが、、または[`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) [`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md)を使用してグラフィックスをレンダリングすることもできます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![OpenGLView の例](views-images/OpenGLView.png "OpenGLView の例")](views-images/OpenGLView-Large.png#lightbox "OpenGLView の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>マップ

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map) マップを表示します。 **Xamarin.Forms.Maps** Nuget パッケージをインストールする必要があります。 Android およびユニバーサル Windows プラットフォームがマップの承認キーが必要です。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Maps.Map) / [ガイド](~/xamarin-forms/user-interface/map.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![マップの例](views-images/Map.png "マップの例")](views-images/Map-Large.png#lightbox "マップの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>コマンドの開始ビュー

### <a name="button"></a>ボタン

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button) テキストを表示する四角形のオブジェクトは、発生して、 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked)が押されたときにイベント。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Button) / [ガイド](~/xamarin-forms/user-interface/button.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![ボタンの例](views-images/Button.png "ボタンの例")](views-images/Button-Large.png#lightbox "ボタンの例")<br /> [このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton` 四角形オブジェクトのイメージを表示して起動される、`Clicked`が押されたときにイベント。<br /><br /> [ガイド](~/xamarin-forms/user-interface/imagebutton.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![ImageButton の例](views-images/ImageButton.png "ImageButton の例")](views-images/ImageButton-Large.png#lightbox "ImageButton の例")<br /> [このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar) 検索を実行するアプリケーションを通知する型、文字列とボタン (または、キーボードのキー) をユーザーの領域が表示されます。 [ `Text` ](xref:Xamarin.Forms.SearchBar.Text)プロパティは、テキストへのアクセスを提供し、 [ `SearchButtonPressed` ](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)イベントは、ボタンが押されたことを示します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.SearchBar) / [ガイド](~/xamarin-forms/user-interface/searchbar.md) / [サンプル](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SearchBar) | [![Searchbar の例](views-images/SearchBar.png "Searchbar の例")](views-images/SearchBar-Large.png#lightbox "SearchBar の例")<br /> [このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>値の設定のビュー

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| `CheckBox`ユーザーは、チェックボックスをオンまたはオフにできるボタンの種類を使用してブール値を選択できます。 プロパティはの状態`CheckBox` `CheckedChanged`です。イベントは、状態が変化したときに発生します。 `IsChecked`<br /><br />API ドキュメント/[ガイド](~/xamarin-forms/user-interface/checkbox.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) | [![CheckBox の例](views-images/CheckBox.png "CheckBox の例")](views-images/CheckBox-Large.png#lightbox "CheckBox の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxDemoPage.xaml) |
|     |     |

### <a name="slider"></a>Slider

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider) ユーザーが選択、`double`から連続する範囲で指定された値、 [ `Minimum` ](xref:Xamarin.Forms.Slider.Minimum)と[ `Maximum` ](xref:Xamarin.Forms.Slider.Maximum)プロパティ。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Slider) / [ガイド](~/xamarin-forms/user-interface/slider.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![スライダーの例](views-images/Slider.png "スライダーの例")](views-images/Slider-Large.png#lightbox "スライダーの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>ステッパ

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper) ユーザーが選択、`double`値で指定された増分値の範囲から、 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum)、 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum)、および[ `Increment` ](xref:Xamarin.Forms.Stepper.Increment)プロパティ。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Stepper)  / [ガイド](~/xamarin-forms/user-interface/stepper.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![ステッパの例](views-images/Stepper.png "ステッパの例")](views-images/Stepper-Large.png#lightbox "ステッパの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>切り替え

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch) ブール値を選択できるようにするオン/オフ スイッチの形式をとります。 [ `IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled)プロパティは、スイッチの状態と[ `Toggled` ](xref:Xamarin.Forms.Switch.Toggled)状態が変更されたときに発生します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Switch) / [ガイド](~/xamarin-forms/user-interface/switch.md) / [サンプル](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SwitchDemos) | [![スイッチの例](views-images/Switch.png "スイッチの例")](views-images/Switch-Large.png#lightbox "スイッチの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker) プラットフォームの日付選択カレンダーで日付を選択できます。 許容される日付の範囲を設定、 [ `MinimumDate` ](xref:Xamarin.Forms.DatePicker.MinimumDate)と[ `MaximumDate` ](xref:Xamarin.Forms.DatePicker.MaximumDate)プロパティ。 [ `Date` ](xref:Xamarin.Forms.DatePicker.Date)プロパティは、選択した日付と[ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected)そのプロパティを変更するときに発生します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.DatePicker) / [ガイド](~/xamarin-forms/user-interface/datepicker.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![DatePicker の例](views-images/DatePicker.png "DatePicker の例")](views-images/DatePicker-Large.png#lightbox "DatePicker の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker) プラットフォームの時刻の選択と時刻を選択できます。 [ `Time` ](xref:Xamarin.Forms.TimePicker.Time)プロパティは、選択した時間。 アプリケーションの変更を監視できます、`Time`プロパティのハンドラーをインストールすることによって、 [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged)イベント。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TimePicker) / [ガイド](~/xamarin-forms/user-interface/timepicker.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![Timepicker の例](views-images/TimePicker.png "Timepicker の例")](views-images/TimePicker-Large.png#lightbox "TimePicker の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>テキストを編集するためのビュー

これら 2 つのクラスから派生し、 [ `InputView` ](xref:Xamarin.Forms.InputView)クラスを定義する、 [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard)プロパティ。

<a name="entry" />

### <a name="entry"></a>入力

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry) 入力し、1 行のテキストを編集できます。 テキストは、 [ `Text` ](xref:Xamarin.Forms.Entry.Text)プロパティ、および[ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged)と[ `Completed` ](xref:Xamarin.Forms.Entry.Completed)イベントが発生したときにテキストの変更またはユーザー完了を通知するには、enter キーをタップします。<br /><br />使用して、 [ `Editor` ](#editor)の入力と複数行のテキストを編集します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Entry) / [ガイド](~/xamarin-forms/user-interface/text/entry.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![エントリの例](views-images/Entry.png "エントリの例")](views-images/Entry-Large.png#lightbox "エントリの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>エディター

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor) 入力し、複数行のテキストを編集できます。 テキストは、 [ `Text` ](xref:Xamarin.Forms.Editor.Text)プロパティ、および[ `TextChanged` ](xref:Xamarin.Forms.Editor.TextChanged)と[ `Completed` ](xref:Xamarin.Forms.Editor.Completed)イベントが発生したときにテキストの変更またはユーザー完了を通知します。<br /><br />使用して、 [ `Entry` ](#entry)ビューを入力すると、1 行のテキストを編集します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Editor) / [ガイド](~/xamarin-forms/user-interface/text/editor.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![エントリの例](views-images/Editor.png "エディターの例")](views-images/Editor-Large.png#lightbox "エディターの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>アクティビティを示すビュー

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) アニメーションを使用して、進行状況の情報を与えることがなく、時間がかかる作業で、アプリケーションが進行中ことを示します。 [ `IsRunning` ](xref:Xamarin.Forms.ActivityIndicator.IsRunning)プロパティは、アニメーションを制御します。<br /><br />アクティビティの進行状況がわかっている場合は、使用、 [ `ProgressBar` ](#progressbar)代わりにします。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ActivityIndicator) / [ガイド](~/xamarin-forms/user-interface/activityindicator.md) / [サンプル](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ActivityIndicatorDemos) | [![Activityindicator の例](views-images/ActivityIndicator.png "Activityindicator の例")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) アニメーションを使用して、時間がかかる作業を通じて、アプリケーションの進行状況を表示します。 設定、 [ `Progress` ](xref:Xamarin.Forms.ProgressBar.Progress)プロパティを 0 ~ 1 に、進行状況を示す値。<br /><br />アクティビティの進行状況が不明である場合は、使用、 [ `ActivityIndicator` ](#activityindicator)代わりにします。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ProgressBar) / [ガイド](~/xamarin-forms/user-interface/progressbar.md) / [サンプル](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ProgressBarDemos) | [![ProgressBar の例](views-images/ProgressBar.png "ProgressBar の例")](views-images/ProgressBar-Large.png#lightbox "ProgressBar の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>コレクションを表示するビュー

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView)さまざまなレイアウト仕様を使用して、選択可能なデータ項目のスクロール可能な一覧を表示します。 これは [ `ListView`](xref:Xamarin.Forms.ListView) の代わりとして、より柔軟でより高パフォーマンスを提供することを目的にしています。 プロパティをオブジェクトのコレクションに設定し`ItemTemplate` 、プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトに設定して、項目の書式設定方法を記述します。 `ItemsSource` イベント`SelectionChanged`は、選択が行われたことを通知します。これ`SelectedItem`はプロパティとして使用できます。<br /><br />[ガイド](~/xamarin-forms/user-interface/collectionview/index.md) / [サンプル](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/) | [![CollectionView の例](views-images/CollectionView.png "CollectionView の例")](views-images/CollectionView-Large.png#lightbox "CollectionView の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView) 派生した[ `ItemsView` ](xref:Xamarin.Forms.ItemsView`1)選択可能なデータ項目のスクロール可能な一覧が表示されます。 設定、 [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource)プロパティ オブジェクト、およびセットのコレクションを[ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)プロパティを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)アイテムがどのように記述するオブジェクト書式設定されます。 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected)選択が行われたこと、として利用できるイベントの通知、 [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem)プロパティ。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ListView) / [ガイド](~/xamarin-forms/user-interface/listview/index.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![ListView の例](views-images/ListView.png "ListView の例")](views-images/ListView-Large.png#lightbox "ListView の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

### <a name="picker"></a>ピッカー

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker) テキストの文字列の一覧から選択した項目を表示し、ビューがタップされたときに、その項目を選択できます。 設定、 [ `Items` ](xref:Xamarin.Forms.Picker.Items)プロパティの文字列のリストをまたは[ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource)プロパティをオブジェクトのコレクション。 [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged)項目が選択されているときに発生します。<br /><br />`Picker`が選択されている場合にのみ、項目の一覧を表示します。 使用して、 [ `ListView` ](#listView)または[ `TableView` ](#tableView)ページに残っているスクロール可能な一覧についてはします。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Picker) / [ガイド](~/xamarin-forms/user-interface/picker/index.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![ピッカーの例](views-images/Picker.png "ピッカーの例")](views-images/Picker-Large.png#lightbox "ピッカーの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView) 型の行の一覧を表示します[ `Cell` ](xref:Xamarin.Forms.Cell)省略可能なヘッダー、サブヘッダーとします。 設定、 [ `Root` ](xref:Xamarin.Forms.TableView.Root)プロパティ型のオブジェクトを[ `TableRoot` ](xref:Xamarin.Forms.TableRoot)、追加[ `TableSection` ](xref:Xamarin.Forms.TableSection)オブジェクトに`TableRoot`します。 各`TableSection`のコレクションである`Cell`オブジェクト。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TableView) / [ガイド](~/xamarin-forms/user-interface/tableview.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![TableView の例](views-images/TableView.png "TableView の例")](views-images/TableView-Large.png#lightbox "TableView の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms FormsGallery サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
