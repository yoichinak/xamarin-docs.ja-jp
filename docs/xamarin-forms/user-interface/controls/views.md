---
title: Xamarin. フォームビュー
description: Xamarin 形式のビューは、クロスプラットフォームモバイルユーザーインターフェイスの構成要素です。 この記事では、Xamarin. フォームに含まれるビューの一覧を示します。
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/16/2020
ms.openlocfilehash: 4164941e4ed8d484699e52eece86085f1c761c91
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516935"
---
# <a name="xamarinforms-views"></a>Xamarin. フォームビュー

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin 形式のビューは、クロスプラットフォームモバイルユーザーインターフェイスの構成要素です。_

ビューは、ラベル、ボタン、スライダーなどのユーザーインターフェイスオブジェクトであり、他のグラフィカルプログラミング環境では、*コントロール*や*ウィジェット*と呼ばれることがよくあります。 Xamarin. Forms でサポートされているビューは[`View`](xref:Xamarin.Forms.View)すべて、クラスから派生します。 複数のカテゴリに分けることができます。

## <a name="views-for-presentation"></a>表示用のビュー

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView)[`Color`](xref:Xamarin.Forms.BoxView.Color)プロパティで色分けされた四角形を表示します。 `BoxView`の既定サイズ要求は40x40 です。 その他のサイズについ[`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)て[`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)は、プロパティとプロパティを割り当てます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.BoxView) / [ガイド](~/xamarin-forms/user-interface/boxview.md) / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)、 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration)、 [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/)、 [4](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)、 [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)、および[6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![BoxView の例](views-images/BoxView.png "BoxView の例")](views-images/BoxView-Large.png#lightbox "BoxView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml)の C# コード |
|     |     |

### <a name="expander"></a>Expander

|     |     |
| --- | --- |
| `Expander`コンテンツをホストするための拡張可能なコンテナーを提供し、ヘッダーとコンテンツで構成されます。 `Header`プロパティ[`View`](xref:Xamarin.Forms.View)をヘッダーとして表示されるに設定し、 `Content`プロパティを、タップに[`View`](xref:Xamarin.Forms.View)よってヘッダーが展開されるときに表示されるに設定します。<br /><br />[ガイド](~/xamarin-forms/user-interface/expander.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos) | [![エキスパンダーの例](views-images/Expander.png "エキスパンダーの例")](views-images/Expander-Large.png#lightbox "エキスパンダーの例")<br /> [このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ExpanderDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ExpanderDemoPage.xaml)の C# コード |
|     |     |

### <a name="label"></a>ラベル

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label)単一行のテキスト文字列またはテキストの複数行のブロックを、定数または変数の書式設定を使用して表示します。 [`Text`](xref:Xamarin.Forms.Label.Text)プロパティを定数形式の文字列に設定するか、 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText)プロパティを変数の書式[`FormattedString`](xref:Xamarin.Forms.FormattedString)設定のオブジェクトに設定します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Label) / [ガイド](~/xamarin-forms/user-interface/text/label.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![ラベルの例](views-images/Label.png "ラベルの例")](views-images/Label-Large.png#lightbox "ラベルの例")<br /> [このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml)の C# コード |
|     |     |

### <a name="image"></a>Image

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image)ビットマップを表示します。 ビットマップは、Web 経由でダウンロードすることも、共通プロジェクトまたはプラットフォームプロジェクトにリソースとして埋め込む`Stream`ことも、.net オブジェクトを使用して作成することもできます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Image) / [ガイド](~/xamarin-forms/user-interface/images.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![画像の例](views-images/Image.png "画像の例")](views-images/Image-Large.png#lightbox "画像の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml)の C# コード |
|     |     |

### <a name="map"></a>マップ

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map)マップを表示します。 **Xamarin. Forms. map** NuGet パッケージがインストールされている必要があります。 Android とユニバーサル Windows プラットフォームには、マップ承認キーが必要です。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Maps.Map) / [ガイド](~/xamarin-forms/user-interface/map/index.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![マップの例](views-images/Map.png "マップの例")](views-images/Map-Large.png#lightbox "マップの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml)の C# コード |
|     |     |

### <a name="mediaelement"></a>MediaElement

|     |     |
| --- | --- |
| [`MediaElement`](xref:Xamarin.Forms.MediaElement)ビデオまたはオーディオを再生します。 メディアは、 [`Source`](xref:Xamarin.Forms.MediaElement.Source)プロパティが[`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource)またはに設定されているかどうかに基づいて、URL またはローカル[`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)ファイルから再生できます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.MediaElement) / [ガイド](~/xamarin-forms/user-interface/mediaelement.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos) | [![MediaElement の例](views-images/MediaElement.png "MediaElement の例")](views-images/MediaElement-Large.png#lightbox "MediaElement の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MediaElementDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MediaElementDemoPage.xaml)の C# コード |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView)iOS および Android プロジェクトの OpenGL グラフィックスを表示します。 ユニバーサル Windows プラットフォームはサポートされていません。 IOS および Android プロジェクトには、 **OpenTK-1.0**アセンブリまたは**OpenTK** version 1.0.0.0 アセンブリへの参照が必要です。 `OpenGLView`共有プロジェクトでの使用が簡単です。.NET Standard ライブラリで使用されている場合は、(サンプルコードに示すように) 依存関係サービスも必要になります。<br /><br />これは、Xamarin. フォームに組み込まれている唯一のグラフィックス機能ですが、、または[`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) [`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md)を使用してグラフィックスをレンダリングすることもできます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![OpenGLView の例](views-images/OpenGLView.png "OpenGLView の例")](views-images/OpenGLView-Large.png#lightbox "OpenGLView の例")<br />[C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs)を使用したこのページ[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml)の C# コード |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView)[`Source`](xref:Xamarin.Forms.WebView.Source)プロパティが[`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource)オブジェクトとオブジェクトのどちらに[`UriWebViewSource`](xref:Xamarin.Forms.UrlWebViewSource)設定されているかに基づいて、Web ページまたは HTML コンテンツを表示します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.WebView) / [ガイド](~/xamarin-forms/user-interface/webview.md) / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)および[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![WebView の例](views-images/WebView.png "WebView の例")](views-images/WebView-Large.png#lightbox "WebView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml)の C# コード |
|     |     |

## <a name="views-that-initiate-commands"></a>コマンドを開始するビュー

### <a name="button"></a>Button

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button)は、テキストを表示し、押されたときに[`Clicked`](xref:Xamarin.Forms.Button.Clicked)イベントを発生させる四角形のオブジェクトです。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Button) / [ガイド](~/xamarin-forms/user-interface/button.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![ボタンの例](views-images/Button.png "ボタンの例")](views-images/Button-Large.png#lightbox "ボタンの例")<br /> [C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs)を使用したこのページ[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml)の C# コード |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton`は、イメージを表示し、押されたときにイベント`Clicked`を発生させる四角形のオブジェクトです。<br /><br /> [ガイド](~/xamarin-forms/user-interface/imagebutton.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![ImageButton の例](views-images/ImageButton.png "ImageButton の例")](views-images/ImageButton-Large.png#lightbox "ImageButton の例")<br /> [C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs) / [分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs)を使用したこのページ[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml)の C# コード |
|     |     |

### <a name="radiobutton"></a>RadioButton

|     |     |
| --- | --- |
| `RadioButton`セットから1つのオプションを選択できるようにします`CheckedChanged` 。選択が発生したときにイベントを発生させます。<br /><br />[ガイド](~/xamarin-forms/user-interface/radiobutton.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/) | [![RadioButton の例](views-images/RadioButton.png "RadioButton の例")](views-images/RadioButton-Large.png#lightbox "RadioButton の例")<br /> [C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RadioButtonDemoPage.cs) / [分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RadioButtonDemoPage.xaml.cs)を使用したこのページ[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RadioButtonDemoPage.xaml)の C# コード |
|     |     |

### <a name="refreshview"></a>RefreshView

|     |     |
| --- | --- |
| `RefreshView`は、スクロール可能なコンテンツのプルから更新機能を提供するコンテナーコントロールです。 更新`ICommand`がトリガーさ`Command`れたときに、プロパティによって定義さ`IsRefreshing`れたが実行され、プロパティはコントロールの現在の状態を示します。<br /><br /> [ガイド](~/xamarin-forms/user-interface/refreshview.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![RefreshView の例](views-images/RefreshView.png "RefreshView の例")](views-images/RefreshView-Large.png#lightbox "RefreshView の例")<br /> [C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RefreshViewDemoPage.cs) / [分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml.cs)を使用したこのページ[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml)の C# コード |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar)ユーザーがテキスト文字列を入力するための領域、および検索を実行するようにアプリケーションに通知するボタン (またはキーボードキー) を表示します。 プロパティ[`Text`](xref:Xamarin.Forms.InputView.Text)はテキストへのアクセスを提供し、 [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)イベントはボタンが押されたことを示します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.SearchBar) / [ガイド](~/xamarin-forms/user-interface/searchbar.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/) | [![SearchBar の例](views-images/SearchBar.png "SearchBar の例")](views-images/SearchBar-Large.png#lightbox "SearchBar の例")<br /> [C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs)を使用したこのページ[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml)の C# コード |
|     |     |

### <a name="swipeview"></a>SwipeView

|     |     |
| --- | --- |
| `SwipeView`は、コンテンツの項目をラップするコンテナーコントロールであり、スワイプジェスチャによって表示されるコンテキストメニュー項目を提供します。 各メニュー項目は、項目が`SwipeItem`タップされ`ICommand`た`Command`ときにを実行するプロパティを持つによって表されます。<br /><br /> [ガイド](~/xamarin-forms/user-interface/swipeview.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![SwipeView の例](views-images/SwipeView.png "SwipeView の例")](views-images/SwipeView-Large.png#lightbox "SwipeView の例")<br /> [C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwipeViewDemoPage.cs) / [分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml.cs)を使用したこのページ[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml)の C# コード |
|     |     |

## <a name="views-for-setting-values"></a>値を設定するためのビュー

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| `CheckBox`ユーザーは、チェックボックスをオンまたはオフにできるボタンの種類を使用してブール値を選択できます。 `IsChecked`プロパティはの状態`CheckBox`です。 `CheckedChanged`イベントは、状態が変化したときに発生します。<br /><br />API ドキュメント/[ガイド](~/xamarin-forms/user-interface/checkbox.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) | [![CheckBox の例](views-images/CheckBox.png "CheckBox の例")](views-images/CheckBox-Large.png#lightbox "CheckBox の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxPage.xaml)の C# コード |
|     |     |

### <a name="slider"></a>Slider

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider)ユーザーは、 `double` [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) [`Maximum`](xref:Xamarin.Forms.Slider.Maximum)プロパティとプロパティで指定された連続する範囲から値を選択できます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Slider) / [ガイド](~/xamarin-forms/user-interface/slider.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![スライダーの例](views-images/Slider.png "スライダーの例")](views-images/Slider-Large.png#lightbox "スライダーの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml)の C# コード |
|     |     |

### <a name="stepper"></a>ステッパ

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper)`double`ユーザーは[`Minimum`](xref:Xamarin.Forms.Stepper.Minimum)、 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum)、、および[`Increment`](xref:Xamarin.Forms.Stepper.Increment)の各プロパティで指定された増分値の範囲から値を選択できます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Stepper)  / [ガイド](~/xamarin-forms/user-interface/stepper.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![ステッパの例](views-images/Stepper.png "ステッパの例")](views-images/Stepper-Large.png#lightbox "ステッパの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml)の C# コード |
|     |     |

### <a name="switch"></a>Switch

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch)ユーザーがブール値を選択できるようにするには、オン/オフスイッチの形式を使用します。 [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)プロパティはスイッチの状態であり、状態が変化[`Toggled`](xref:Xamarin.Forms.Switch.Toggled)するとイベントが発生します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Switch) / [ガイド](~/xamarin-forms/user-interface/switch.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/) | [![スイッチの例](views-images/Switch.png "スイッチの例")](views-images/Switch-Large.png#lightbox "スイッチの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml)の C# コード |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker)プラットフォームの日付の選択で日付を選択することをユーザーに許可します。 プロパティと[`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate)プロパティを使用して、 [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate)許容される日付の範囲を設定します。 [`Date`](xref:Xamarin.Forms.DatePicker.Date)プロパティは選択された日付であり[`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) 、そのプロパティが変更されるとイベントが発生します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.DatePicker) / [ガイド](~/xamarin-forms/user-interface/datepicker.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![DatePicker の例](views-images/DatePicker.png "DatePicker の例")](views-images/DatePicker-Large.png#lightbox "DatePicker の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml)の C# コード |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker)ユーザーがプラットフォームのタイムピッカーを使用して時刻を選択できるようにします。 [`Time`](xref:Xamarin.Forms.TimePicker.Time)プロパティは、選択された時刻です。 アプリケーションでは、 `Time` [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged)イベントのハンドラーをインストールすることによって、プロパティの変更を監視できます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TimePicker) / [ガイド](~/xamarin-forms/user-interface/timepicker.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![TimePicker の例](views-images/TimePicker.png "TimePicker の例")](views-images/TimePicker-Large.png#lightbox "TimePicker の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml)の C# コード |
|     |     |

## <a name="views-for-editing-text"></a>テキストを編集するためのビュー

これらの2つのクラス[`InputView`](xref:Xamarin.Forms.InputView)は、 [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard)プロパティを定義するクラスから派生します。

### <a name="entry"></a>エントリ

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry)ユーザーが1行のテキストを入力および編集できるようにします。 テキストは[`Text`](xref:Xamarin.Forms.InputView.Text)プロパティとして使用でき、テキスト[`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)が[`Completed`](xref:Xamarin.Forms.Entry.Completed)変更されたとき、またはユーザーが enter キーをタップして完了を通知すると、イベントとイベントが発生します。<br /><br />複数行[`Editor`](#editor)のテキストを入力および編集するには、を使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Entry) / [ガイド](~/xamarin-forms/user-interface/text/entry.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![エントリの例](views-images/Entry.png "エントリの例")](views-images/Entry-Large.png#lightbox "エントリの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml)の C# コード |
|     |     |

### <a name="editor"></a>エディター

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor)ユーザーが複数行のテキストを入力および編集できるようにします。 テキストは[`Text`](xref:Xamarin.Forms.InputView.Text)プロパティとして使用でき、テキスト[`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)が[`Completed`](xref:Xamarin.Forms.Editor.Completed)変更されたとき、またはユーザーが入力を完了すると、イベントとイベントが発生します。<br /><br />1行[`Entry`](#entry)のテキストを入力および編集するには、ビューを使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Editor) / [ガイド](~/xamarin-forms/user-interface/text/editor.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![エントリの例](views-images/Editor.png "エディターの例")](views-images/Editor-Large.png#lightbox "エディターの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml)の C# コード |
|     |     |

## <a name="views-to-indicate-activity"></a>アクティビティを示すためのビュー

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)アニメーションを使用して、進行状況を示すことなく、アプリケーションが長いアクティビティに関与していることを示します。 プロパティ[`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)は、アニメーションを制御します。<br /><br />アクティビティの進行状況がわかっている場合は[`ProgressBar`](#progressbar) 、代わりにを使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ActivityIndicator) / [ガイド](~/xamarin-forms/user-interface/activityindicator.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/) | [![ActivityIndicator の例](views-images/ActivityIndicator.png "ActivityIndicator の例")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml)の C# コード |
|     |     |

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)アニメーションを使用して、アプリケーションが長いアクティビティを経て進行していることを示します。 進行状況[`Progress`](xref:Xamarin.Forms.ProgressBar.Progress)を示すには、プロパティを 0 ~ 1 の範囲の値に設定します。<br /><br />アクティビティの進行状況が不明な場合は、 [`ActivityIndicator`](#activityindicator)代わりにを使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ProgressBar) / [ガイド](~/xamarin-forms/user-interface/progressbar.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/) | [![ProgressBar の例](views-images/ProgressBar.png "ProgressBar の例")](views-images/ProgressBar-Large.png#lightbox "ProgressBar の例")<br />[C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs)を使用したこのページ[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml)の C# コード |
|     |     |

## <a name="views-that-display-collections"></a>コレクションを表示するビュー

### <a name="carouselview"></a>CarouselView

|     |     |
| --- | --- |
| [`CarouselView`](xref:Xamarin.Forms.CarouselView)データ項目のスクロール可能な一覧を表示します。 `ItemsSource`プロパティをオブジェクトのコレクションに設定し、 `ItemTemplate`プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトに設定して、項目の書式設定方法を記述します。 イベント`CurrentItemChanged`は、現在表示されている項目が変更されたこと`CurrentItem`を通知します。これは、プロパティとして使用できます。<br /><br />[ガイド](~/xamarin-forms/user-interface/carouselview/index.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/) | [![CarouselView の例](views-images/CarouselView.png "CarouselView の例")](views-images/CarouselView-Large.png#lightbox "CarouselView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselViewDemoPage.xaml)の C# コード |
|     |     |

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView)さまざまなレイアウト仕様を使用して、選択可能なデータ項目のスクロール可能な一覧を表示します。 これは、より柔軟でパフォーマンスの高い代替手段を[`ListView`](xref:Xamarin.Forms.ListView)提供することを目的としています。 `ItemsSource`プロパティをオブジェクトのコレクションに設定し、 `ItemTemplate`プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトに設定して、項目の書式設定方法を記述します。 イベント`SelectionChanged`は、選択が行われたことを通知します。これ`SelectedItem`はプロパティとして使用できます。<br /><br />[ガイド](~/xamarin-forms/user-interface/collectionview/index.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/) | [![CollectionView の例](views-images/CollectionView.png "CollectionView の例")](views-images/CollectionView-Large.png#lightbox "CollectionView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml)の C# コード |
|     |     |

### <a name="indicatorview"></a>IndicatorView

|     |     |
| --- | --- |
| `IndicatorView`内の項目の数を表すインジケーターを表示`CarouselView`します。 `CarouselView.IndicatorView`プロパティを`IndicatorView`オブジェクトに設定して、 `CarouselView`のインジケーターを表示します。 <br /><br />[ガイド](~/xamarin-forms/user-interface/indicatorview.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/) | [![IndicatorView の例](views-images/IndicatorView.png "IndicatorView の例")](views-images/IndicatorView-Large.png#lightbox "IndicatorView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/IndicatorViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/IndicatorViewDemoPage.xaml)の C# コード |
|     |     |

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView)から[`ItemsView`](xref:Xamarin.Forms.ItemsView`1)派生し、選択可能なデータ項目のスクロール可能なリストを表示します。 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource)プロパティをオブジェクトのコレクションに設定し、 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトに設定して、項目の書式設定方法を記述します。 イベント[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)は、選択が行われたことを通知します。これ[`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem)はプロパティとして使用できます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ListView) / [ガイド](~/xamarin-forms/user-interface/listview/index.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![ListView の例](views-images/ListView.png "ListView の例")](views-images/ListView-Large.png#lightbox "ListView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml)の C# コード |
|     |     |

### <a name="picker"></a>ピッカー

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker)テキスト文字列の一覧から選択した項目を表示し、ビューをタップしたときにその項目を選択できるようにします。 [`Items`](xref:Xamarin.Forms.Picker.Items)プロパティを文字列のリスト、または[`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource)プロパティをオブジェクトのコレクションに設定します。 イベント[`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged)は、項目が選択されたときに発生します。<br /><br />で`Picker`は、項目が選択されている場合にのみ、項目の一覧が表示されます。 ページに[`ListView`](#listview)残っ[`TableView`](#tableview)ているスクロール可能な一覧には、またはを使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Picker) / [ガイド](~/xamarin-forms/user-interface/picker/index.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![ピッカーの例](views-images/Picker.png "ピッカーの例")](views-images/Picker-Large.png#lightbox "ピッカーの例")<br />[C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs)を使用したこのページ[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml)の C# コード |
|     |     |

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView)省略可能なヘッダーとサブヘッダー [`Cell`](xref:Xamarin.Forms.Cell)を持つ型の行の一覧を表示します。 [`Root`](xref:Xamarin.Forms.TableView.Root)プロパティを型[`TableRoot`](xref:Xamarin.Forms.TableRoot)のオブジェクトに設定し、オブジェクトを[`TableSection`](xref:Xamarin.Forms.TableSection)その`TableRoot`に追加します。 各`TableSection`はオブジェクトの`Cell`コレクションです。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TableView) / [ガイド](~/xamarin-forms/user-interface/tableview.md) / の[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![TableView の例](views-images/TableView.png "TableView の例")](views-images/TableView-Large.png#lightbox "TableView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewFormDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewFormDemoPage.xaml)の C# コード |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin フォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
