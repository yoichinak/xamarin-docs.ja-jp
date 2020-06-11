---
title: " Xamarin.Forms views" description: " Xamarin.Forms views は、クロスプラットフォームモバイルユーザーインターフェイスの構成要素です。 この記事では、に含まれているビューの一覧を示し Xamarin.Forms ます。
ms. 製品: xamarin ms. assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 04/16/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-views"></a>Xamarin.Forms表示モード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin 形式のビューは、クロスプラットフォームモバイルユーザーインターフェイスの構成要素です。_

ビューは、ラベル、ボタン、スライダーなどのユーザーインターフェイスオブジェクトであり、他のグラフィカルプログラミング環境では、*コントロール*や*ウィジェット*と呼ばれることがよくあります。 でサポートされるすべてのビューは、 Xamarin.Forms クラスから派生し [`View`](xref:Xamarin.Forms.View) ます。 複数のカテゴリに分けることができます。

## <a name="views-for-presentation"></a>表示用のビュー

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView)プロパティで色分けされた四角形を表示 [`Color`](xref:Xamarin.Forms.BoxView.Color) します。 `BoxView`の既定サイズ要求は40x40 です。 その他のサイズについては、 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) プロパティとプロパティを割り当て [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.BoxView)  / [ガイド](~/xamarin-forms/user-interface/boxview.md)  / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)、 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration)、 [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/)、 [4](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)、 [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)、および[6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![BoxView の例](views-images/BoxView.png "BoxView の例")](views-images/BoxView-Large.png#lightbox "BoxView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="expander"></a>Expander

|     |     |
| --- | --- |
| `Expander`コンテンツをホストするための拡張可能なコンテナーを提供し、ヘッダーとコンテンツで構成されます。 プロパティをヘッダーとして表示されるに設定し、プロパティを、 `Header` [`View`](xref:Xamarin.Forms.View) タップに `Content` [`View`](xref:Xamarin.Forms.View) よってヘッダーが展開されるときに表示されるに設定します。<br /><br />[ガイド](~/xamarin-forms/user-interface/expander.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos) | [![エキスパンダーの例](views-images/Expander.png "エキスパンダーの例")](views-images/Expander-Large.png#lightbox "エキスパンダーの例")<br /> [このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ExpanderDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ExpanderDemoPage.xaml) |
|     |     |

### <a name="label"></a>Label

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label)単一行のテキスト文字列またはテキストの複数行のブロックを、定数または変数の書式設定を使用して表示します。 プロパティを [`Text`](xref:Xamarin.Forms.Label.Text) 定数形式の文字列に設定するか、プロパティを [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) 変数の書式設定のオブジェクトに設定し [`FormattedString`](xref:Xamarin.Forms.FormattedString) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Label)  / [ガイド](~/xamarin-forms/user-interface/text/label.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![ラベルの例](views-images/Label.png "ラベルの例")](views-images/Label-Large.png#lightbox "ラベルの例")<br /> [このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Image

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image)ビットマップを表示します。 ビットマップは、Web 経由でダウンロードすることも、共通プロジェクトまたはプラットフォームプロジェクトにリソースとして埋め込むことも、.NET オブジェクトを使用して作成することもでき `Stream` ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Image)  / [ガイド](~/xamarin-forms/user-interface/images.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![画像の例](views-images/Image.png "画像の例")](views-images/Image-Large.png#lightbox "画像の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="map"></a>マップ

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map)マップを表示します。 ** Xamarin.Forms 。マップ**NuGet パッケージがインストールされている必要があります。 Android とユニバーサル Windows プラットフォームには、マップ承認キーが必要です。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Maps.Map)  / [ガイド](~/xamarin-forms/user-interface/map/index.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![マップの例](views-images/Map.png "マップの例")](views-images/Map-Large.png#lightbox "マップの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

### <a name="mediaelement"></a>MediaElement

|     |     |
| --- | --- |
| [`MediaElement`](xref:Xamarin.Forms.MediaElement)ビデオまたはオーディオを再生します。 メディアは、 [`Source`](xref:Xamarin.Forms.MediaElement.Source) プロパティがまたはに設定されているかどうかに基づいて、URL またはローカルファイルから再生でき [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource) [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.MediaElement)  / [ガイド](~/xamarin-forms/user-interface/mediaelement.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos) | [![MediaElement の例](views-images/MediaElement.png "MediaElement の例")](views-images/MediaElement-Large.png#lightbox "MediaElement の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MediaElementDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MediaElementDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView)iOS および Android プロジェクトの OpenGL グラフィックスを表示します。 ユニバーサル Windows プラットフォームはサポートされていません。 IOS および Android プロジェクトには、 **OpenTK-1.0**アセンブリまたは**OpenTK** version 1.0.0.0 アセンブリへの参照が必要です。 `OpenGLView`共有プロジェクトでの使用が簡単です。.NET Standard ライブラリで使用されている場合は、(サンプルコードに示すように) 依存関係サービスも必要になります。<br /><br />これはに組み込まれている唯一のグラフィックス機能です Xamarin.Forms が Xamarin.Forms 、アプリケーションは [`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) 、、またはを使用してグラフィックスをレンダリングすることもでき [`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![OpenGLView の例](views-images/OpenGLView.png "OpenGLView の例")](views-images/OpenGLView-Large.png#lightbox "OpenGLView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView)プロパティがオブジェクトとオブジェクトのどちらに設定されているかに基づいて、Web ページまたは HTML コンテンツを表示し [`Source`](xref:Xamarin.Forms.WebView.Source) [`UriWebViewSource`](xref:Xamarin.Forms.UrlWebViewSource) [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.WebView)  / [ガイド](~/xamarin-forms/user-interface/webview.md)  / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)および[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![WebView の例](views-images/WebView.png "WebView の例")](views-images/WebView-Large.png#lightbox "WebView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>コマンドを開始するビュー

### <a name="button"></a>Button

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button)は、テキストを表示し、 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 押されたときにイベントを発生させる四角形のオブジェクトです。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Button)  / [ガイド](~/xamarin-forms/user-interface/button.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![ボタンの例](views-images/Button.png "ボタンの例")](views-images/Button-Large.png#lightbox "ボタンの例")<br /> [このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton`は、イメージを表示し、 `Clicked` 押されたときにイベントを発生させる四角形のオブジェクトです。<br /><br /> [ガイド](~/xamarin-forms/user-interface/imagebutton.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![ImageButton の例](views-images/ImageButton.png "ImageButton の例")](views-images/ImageButton-Large.png#lightbox "ImageButton の例")<br /> [このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) |
|     |     |

### <a name="radiobutton"></a>RadioButton

|     |     |
| --- | --- |
| `RadioButton`セットから1つのオプションを選択できるようにし `CheckedChanged` ます。選択が発生したときにイベントを発生させます。<br /><br />[ガイド](~/xamarin-forms/user-interface/radiobutton.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/) | [![RadioButton の例](views-images/RadioButton.png "RadioButton の例")](views-images/RadioButton-Large.png#lightbox "RadioButton の例")<br /> [このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RadioButtonDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RadioButtonDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RadioButtonDemoPage.xaml) |
|     |     |

### <a name="refreshview"></a>RefreshView

|     |     |
| --- | --- |
| `RefreshView`は、スクロール可能なコンテンツのプルから更新機能を提供するコンテナーコントロールです。 `ICommand`更新がトリガーされたときに、プロパティによって定義されたが `Command` 実行され、 `IsRefreshing` プロパティはコントロールの現在の状態を示します。<br /><br /> [ガイド](~/xamarin-forms/user-interface/refreshview.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![RefreshView の例](views-images/RefreshView.png "RefreshView の例")](views-images/RefreshView-Large.png#lightbox "RefreshView の例")<br /> [このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RefreshViewDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar)ユーザーがテキスト文字列を入力するための領域、および検索を実行するようにアプリケーションに通知するボタン (またはキーボードキー) を表示します。 [`Text`](xref:Xamarin.Forms.InputView.Text)プロパティはテキストへのアクセスを提供し、 [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) イベントはボタンが押されたことを示します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.SearchBar)  / [ガイド](~/xamarin-forms/user-interface/searchbar.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/) | [![SearchBar の例](views-images/SearchBar.png "SearchBar の例")](views-images/SearchBar-Large.png#lightbox "SearchBar の例")<br /> [このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) |
|     |     |

### <a name="swipeview"></a>SwipeView

|     |     |
| --- | --- |
| `SwipeView`は、コンテンツの項目をラップするコンテナーコントロールであり、スワイプジェスチャによって表示されるコンテキストメニュー項目を提供します。 各メニュー項目は、項目が `SwipeItem` タップされたときにを実行するプロパティを持つによって表され `Command` `ICommand` ます。<br /><br /> [ガイド](~/xamarin-forms/user-interface/swipeview.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![SwipeView の例](views-images/SwipeView.png "SwipeView の例")](views-images/SwipeView-Large.png#lightbox "SwipeView の例")<br /> [このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwipeViewDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml) |
|     |     |

## <a name="views-for-setting-values"></a>値を設定するためのビュー

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| `CheckBox`ユーザーは、チェックボックスをオンまたはオフにできるボタンの種類を使用してブール値を選択できます。 プロパティはの `IsChecked` 状態です `CheckBox` 。イベントは、状態が変化し `CheckedChanged` たときに発生します。<br /><br />API ドキュメント/[ガイド](~/xamarin-forms/user-interface/checkbox.md)の  /  [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) | [![CheckBox の例](views-images/CheckBox.png "CheckBox の例")](views-images/CheckBox-Large.png#lightbox "CheckBox の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxPage.xaml) |
|     |     |

### <a name="slider"></a>Slider

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider)ユーザーは、 `double` プロパティとプロパティで指定された連続する範囲から値を選択でき [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Slider)  / [ガイド](~/xamarin-forms/user-interface/slider.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![スライダーの例](views-images/Slider.png "スライダーの例")](views-images/Slider-Large.png#lightbox "スライダーの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>ステッパ

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper)ユーザーは `double` 、、、およびの各プロパティで指定された増分値の範囲から値を選択でき [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) [`Increment`](xref:Xamarin.Forms.Stepper.Increment) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Stepper)   / [ガイド](~/xamarin-forms/user-interface/stepper.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![ステッパの例](views-images/Stepper.png "ステッパの例")](views-images/Stepper-Large.png#lightbox "ステッパの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Switch

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch)ユーザーがブール値を選択できるようにするには、オン/オフスイッチの形式を使用します。 [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)プロパティはスイッチの状態で [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) あり、状態が変化するとイベントが発生します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Switch)  / [ガイド](~/xamarin-forms/user-interface/switch.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/) | [![スイッチの例](views-images/Switch.png "スイッチの例")](views-images/Switch-Large.png#lightbox "スイッチの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker)プラットフォームの日付の選択で日付を選択することをユーザーに許可します。 プロパティとプロパティを使用して、許容される日付の範囲を設定し [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) ます。 [`Date`](xref:Xamarin.Forms.DatePicker.Date)プロパティは選択された日付で [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) あり、そのプロパティが変更されるとイベントが発生します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.DatePicker)  / [ガイド](~/xamarin-forms/user-interface/datepicker.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![DatePicker の例](views-images/DatePicker.png "DatePicker の例")](views-images/DatePicker-Large.png#lightbox "DatePicker の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker)ユーザーがプラットフォームのタイムピッカーを使用して時刻を選択できるようにします。 プロパティは、 [`Time`](xref:Xamarin.Forms.TimePicker.Time) 選択された時刻です。 アプリケーションでは、 `Time` イベントのハンドラーをインストールすることによって、プロパティの変更を監視でき [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TimePicker)  / [ガイド](~/xamarin-forms/user-interface/timepicker.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![TimePicker の例](views-images/TimePicker.png "TimePicker の例")](views-images/TimePicker-Large.png#lightbox "TimePicker の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>テキストを編集するためのビュー

これらの2つのクラスは、 [`InputView`](xref:Xamarin.Forms.InputView) プロパティを定義するクラスから派生 [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) します。

### <a name="entry"></a>入力

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry)ユーザーが1行のテキストを入力および編集できるようにします。 テキストはプロパティとして使用でき、 [`Text`](xref:Xamarin.Forms.InputView.Text) [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) テキストが [`Completed`](xref:Xamarin.Forms.Entry.Completed) 変更されたとき、またはユーザーが enter キーをタップして完了を通知すると、イベントとイベントが発生します。<br /><br />[`Editor`](#editor)複数行のテキストを入力および編集するには、を使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Entry)  / [ガイド](~/xamarin-forms/user-interface/text/entry.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![エントリの例](views-images/Entry.png "エントリの例")](views-images/Entry-Large.png#lightbox "エントリの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

### <a name="editor"></a>エディター

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor)ユーザーが複数行のテキストを入力および編集できるようにします。 テキストはプロパティとして使用でき、 [`Text`](xref:Xamarin.Forms.InputView.Text) [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) テキストが [`Completed`](xref:Xamarin.Forms.Editor.Completed) 変更されたとき、またはユーザーが入力を完了すると、イベントとイベントが発生します。<br /><br />[`Entry`](#entry)1 行のテキストを入力および編集するには、ビューを使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Editor)  / [ガイド](~/xamarin-forms/user-interface/text/editor.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![エントリの例](views-images/Editor.png "エディターの例")](views-images/Editor-Large.png#lightbox "エディターの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>アクティビティを示すためのビュー

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)アニメーションを使用して、進行状況を示すことなく、アプリケーションが長いアクティビティに関与していることを示します。 プロパティは、 [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) アニメーションを制御します。<br /><br />アクティビティの進行状況がわかっている場合は、代わりにを使用 [`ProgressBar`](#progressbar) します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ActivityIndicator)  / [ガイド](~/xamarin-forms/user-interface/activityindicator.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/) | [![ActivityIndicator の例](views-images/ActivityIndicator.png "ActivityIndicator の例")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)アニメーションを使用して、アプリケーションが長いアクティビティを経て進行していることを示します。 [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress)進行状況を示すには、プロパティを 0 ~ 1 の範囲の値に設定します。<br /><br />アクティビティの進行状況が不明な場合は、代わりにを使用 [`ActivityIndicator`](#activityindicator) します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ProgressBar)  / [ガイド](~/xamarin-forms/user-interface/progressbar.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/) | [![ProgressBar の例](views-images/ProgressBar.png "ProgressBar の例")](views-images/ProgressBar-Large.png#lightbox "ProgressBar の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) |
|     |     |

## <a name="views-that-display-collections"></a>コレクションを表示するビュー

### <a name="carouselview"></a>CarouselView

|     |     |
| --- | --- |
| [`CarouselView`](xref:Xamarin.Forms.CarouselView)データ項目のスクロール可能な一覧を表示します。 プロパティを `ItemsSource` オブジェクトのコレクションに設定し、プロパティをオブジェクトに設定して、 `ItemTemplate` 項目の書式設定方法を記述し [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。 イベントは、 `CurrentItemChanged` 現在表示されている項目が変更されたことを通知します。これは、プロパティとして使用でき `CurrentItem` ます。<br /><br />[ガイド](~/xamarin-forms/user-interface/carouselview/index.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/) | [![CarouselView の例](views-images/CarouselView.png "CarouselView の例")](views-images/CarouselView-Large.png#lightbox "CarouselView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselViewDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselViewDemoPage.xaml) |
|     |     |

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView)さまざまなレイアウト仕様を使用して、選択可能なデータ項目のスクロール可能な一覧を表示します。 これは、より柔軟でパフォーマンスの高い代替手段を提供することを目的として [`ListView`](xref:Xamarin.Forms.ListView) います。 プロパティを `ItemsSource` オブジェクトのコレクションに設定し、プロパティをオブジェクトに設定して、 `ItemTemplate` 項目の書式設定方法を記述し [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。 イベントは、 `SelectionChanged` 選択が行われたことを通知します。これはプロパティとして使用でき `SelectedItem` ます。<br /><br />[ガイド](~/xamarin-forms/user-interface/collectionview/index.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/) | [![CollectionView の例](views-images/CollectionView.png "CollectionView の例")](views-images/CollectionView-Large.png#lightbox "CollectionView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) |
|     |     |

### <a name="indicatorview"></a>IndicatorView

|     |     |
| --- | --- |
| `IndicatorView`内の項目の数を表すインジケーターを表示 `CarouselView` します。 プロパティを `CarouselView.IndicatorView` オブジェクトに設定して、の `IndicatorView` インジケーターを表示 `CarouselView` します。 <br /><br />[ガイド](~/xamarin-forms/user-interface/indicatorview.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/) | [![IndicatorView の例](views-images/IndicatorView.png "IndicatorView の例")](views-images/IndicatorView-Large.png#lightbox "IndicatorView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/IndicatorViewDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/IndicatorViewDemoPage.xaml) |
|     |     |

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView)から派生 [`ItemsView`](xref:Xamarin.Forms.ItemsView`1) し、選択可能なデータ項目のスクロール可能なリストを表示します。 プロパティを [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) オブジェクトのコレクションに設定し、プロパティをオブジェクトに設定して、 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 項目の書式設定方法を記述し [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。 イベントは、 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 選択が行われたことを通知します。これはプロパティとして使用でき [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ListView)  / [ガイド](~/xamarin-forms/user-interface/listview/index.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![ListView の例](views-images/ListView.png "ListView の例")](views-images/ListView-Large.png#lightbox "ListView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

### <a name="picker"></a>ピッカー

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker)テキスト文字列の一覧から選択した項目を表示し、ビューをタップしたときにその項目を選択できるようにします。 プロパティを [`Items`](xref:Xamarin.Forms.Picker.Items) 文字列のリスト、または [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) プロパティをオブジェクトのコレクションに設定します。 イベントは、 [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) 項目が選択されたときに発生します。<br /><br />では、 `Picker` 項目が選択されている場合にのみ、項目の一覧が表示されます。 [`ListView`](#listview) [`TableView`](#tableview) ページに残っているスクロール可能な一覧には、またはを使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Picker)  / [ガイド](~/xamarin-forms/user-interface/picker/index.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![ピッカーの例](views-images/Picker.png "ピッカーの例")](views-images/Picker-Large.png#lightbox "ピッカーの例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs)  /  の C# コード[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs)付き[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) |
|     |     |

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView)[`Cell`](xref:Xamarin.Forms.Cell)省略可能なヘッダーとサブヘッダーを持つ型の行の一覧を表示します。 プロパティを [`Root`](xref:Xamarin.Forms.TableView.Root) 型のオブジェクトに設定 [`TableRoot`](xref:Xamarin.Forms.TableRoot) し、 [`TableSection`](xref:Xamarin.Forms.TableSection) オブジェクトをそのに追加し `TableRoot` ます。 各 `TableSection` はオブジェクトのコレクションです `Cell` 。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TableView)  / [ガイド](~/xamarin-forms/user-interface/tableview.md)  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![TableView の例](views-images/TableView.png "TableView の例")](views-images/TableView-Large.png#lightbox "TableView の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewFormDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewFormDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Formsフォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
