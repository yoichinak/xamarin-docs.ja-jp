---
title: Xamarin.Forms のビュー
description: Xamarin.Forms のビューとは、クロス プラットフォーム モバイルのユーザー インターフェイスの構成要素です。 この記事では、Xamarin.Forms に含まれているビューを一覧表示します。
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/14/2020
ms.openlocfilehash: 09bcb49db7f257a415518b259672ca8e776cdbc4
ms.sourcegitcommit: 5d22f37dfc358678df52a4d17c57261056a72cb7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77674565"
---
# <a name="xamarinforms-views"></a>Xamarin.Forms のビュー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin 形式のビューは、クロスプラットフォームモバイルユーザーインターフェイスの構成要素です。_

ビューは、ラベル、ボタン、スライダーなどのユーザーインターフェイスオブジェクトであり、他のグラフィカルプログラミング環境では、*コントロール*や*ウィジェット*と呼ばれることがよくあります。 Xamarin. Forms でサポートされているビューはすべて、 [`View`](xref:Xamarin.Forms.View)クラスから派生します。 これらは、いくつかのカテゴリに分類できます。

## <a name="views-for-presentation"></a>表示用のビュー

### <a name="label"></a>[ラベル]

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label)には、定数または変数の書式設定を使用して、単一行のテキスト文字列またはテキストの複数行のブロックが表示されます。 [`Text`](xref:Xamarin.Forms.Label.Text)プロパティを定数形式の文字列に設定するか、 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText)プロパティを変数の書式設定のために[`FormattedString`](xref:Xamarin.Forms.FormattedString)オブジェクトに設定します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Label) / [ガイド](~/xamarin-forms/user-interface/text/label.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![ラベルの例](views-images/Label.png "ラベルの例")](views-images/Label-Large.png#lightbox "ラベルの例")<br /> このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) |
|     |     |

### <a name="image"></a>イメージ

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image)ビットマップを表示します。 ビットマップは、Web 経由でダウンロードしたり、共通プロジェクトまたはプラットフォームプロジェクトにリソースとして埋め込んだり、.NET `Stream` オブジェクトを使用して作成したりすることができます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Image) / [ガイド](~/xamarin-forms/user-interface/images.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![画像の例](views-images/Image.png "画像の例")](views-images/Image-Large.png#lightbox "画像の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) [`Color`](xref:Xamarin.Forms.BoxView.Color)プロパティで色分けされた四角形を表示します。 `BoxView` には、既定のサイズ要求である40x40 があります。 その他のサイズについては、 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)と[`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)のプロパティを割り当てます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.BoxView) / [ガイド](~/xamarin-forms/user-interface/boxview.md) / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)、 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration)、 [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/)、 [4](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)、 [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)、および[6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![BoxView の例](views-images/BoxView.png "BoxView の例")](views-images/BoxView-Large.png#lightbox "BoxView の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) [`Source`](xref:Xamarin.Forms.WebView.Source)プロパティが[`UriWebViewSource`](xref:Xamarin.Forms.UrlWebViewSource)または[`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource)オブジェクトのどちらに設定されているかに基づいて、Web ページまたは HTML コンテンツを表示します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.WebView) / [ガイド](~/xamarin-forms/user-interface/webview.md) / [サンプル 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)および[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![WebView の例](views-images/WebView.png "WebView の例")](views-images/WebView-Large.png#lightbox "WebView の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView)には、IOS および Android プロジェクトの OpenGL グラフィックスが表示されます。 ユニバーサル Windows プラットフォームのサポートはありません。 IOS および Android プロジェクトには、 **OpenTK-1.0**アセンブリまたは**OpenTK** version 1.0.0.0 アセンブリへの参照が必要です。 共有プロジェクトでは、`OpenGLView` を使いやすくすることができます.NET Standard ライブラリで使用されている場合は、(サンプルコードに示すように) 依存関係サービスも必要になります。<br /><br />これは、Xamarin. フォームに組み込まれている唯一のグラフィックス機能ですが、 [`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)、または[`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md)を使用してグラフィックスをレンダリングすることもできます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![OpenGLView の例](views-images/OpenGLView.png "OpenGLView の例")](views-images/OpenGLView-Large.png#lightbox "OpenGLView の例")<br />このページのコード / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml)と[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) |
|     |     |

### <a name="map"></a>マップ

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map)マップを表示します。 **Xamarin. Forms. map** NuGet パッケージがインストールされている必要があります。 Android およびユニバーサル Windows プラットフォームがマップの承認キーが必要です。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Maps.Map) / [ガイド](~/xamarin-forms/user-interface/map/index.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![マップの例](views-images/Map.png "マップの例")](views-images/Map-Large.png#lightbox "マップの例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) |
|     |     |

### <a name="mediaelement"></a>MediaElement

|     |     |
| --- | --- |
| [`MediaElement`](xref:Xamarin.Forms.MediaElement)はビデオまたはオーディオを再生します。 メディアは、 [`Source`](xref:Xamarin.Forms.MediaElement.Source)プロパティが[`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource)または[`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)に設定されているかどうかに基づいて、URL またはローカルファイルから再生できます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.MediaElement) / [ガイド](~/xamarin-forms/user-interface/mediaelement.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos) | [![MediaElement の例](views-images/MediaElement.png "MediaElement の例")](views-images/MediaElement-Large.png#lightbox "MediaElement の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MediaElementDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MediaElementDemoPage.cs) |
|     |     |

## <a name="views-that-initiate-commands"></a>コマンドを開始するビュー

### <a name="button"></a>ボタン

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button)は、テキストを表示し、押されたときに[`Clicked`](xref:Xamarin.Forms.Button.Clicked)イベントを発生させる四角形のオブジェクトです。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Button) / [ガイド](~/xamarin-forms/user-interface/button.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![ボタンの例](views-images/Button.png "ボタンの例")](views-images/Button-Large.png#lightbox "ボタンの例")<br /> このページのコード / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml)と[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton` は、イメージを表示する四角形オブジェクトであり、押されたときに `Clicked` イベントを発生させます。<br /><br />  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)の[ガイド](~/xamarin-forms/user-interface/imagebutton.md) | [![ImageButton の例](views-images/ImageButton.png "ImageButton の例")](views-images/ImageButton-Large.png#lightbox "ImageButton の例")<br /> このページのコード / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml)と[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs) |
|     |     |

### <a name="refreshview"></a>RefreshView

|     |     |
| --- | --- |
| `RefreshView` は、スクロール可能なコンテンツのプルから更新機能を提供するコンテナーコントロールです。 `Command` プロパティによって定義された `ICommand` は、更新がトリガーされたときに実行され、`IsRefreshing` プロパティはコントロールの現在の状態を示します。<br /><br />  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)の[ガイド](~/xamarin-forms/user-interface/refreshview.md) | [![RefreshView の例](views-images/RefreshView.png "RefreshView の例")](views-images/RefreshView-Large.png#lightbox "RefreshView の例")<br /> このページのコード / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml)と[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RefreshViewDemoPage.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar)には、ユーザーがテキスト文字列を入力するための領域と、アプリケーションに検索を実行するように通知するボタン (またはキーボードキー) が表示されます。 [`Text`](xref:Xamarin.Forms.InputView.Text)プロパティはテキストへのアクセスを提供し、 [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)イベントはボタンが押されたことを示します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.SearchBar) / [ガイド](~/xamarin-forms/user-interface/searchbar.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/) | [![SearchBar の例](views-images/SearchBar.png "SearchBar の例")](views-images/SearchBar-Large.png#lightbox "SearchBar の例")<br /> このページのコード / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml)と[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) |
|     |     |

### <a name="swipeview"></a>SwipeView

|     |     |
| --- | --- |
| `SwipeView` は、コンテンツの項目をラップするコンテナーコントロールであり、スワイプジェスチャによって表示されるコンテキストメニュー項目を提供します。 各メニュー項目は、`SwipeItem`によって表されます。これには、項目がタップされたときに `ICommand` を実行する `Command` プロパティがあります。<br /><br />  / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)の[ガイド](~/xamarin-forms/user-interface/swipeview.md) | [![SwipeView の例](views-images/SwipeView.png "SwipeView の例")](views-images/SwipeView-Large.png#lightbox "SwipeView の例")<br /> このページのコード / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml)と[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwipeViewDemoPage.cs) |
|     |     |

## <a name="views-for-setting-values"></a>値を設定するためのビュー

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| `CheckBox` を使用すると、ユーザーは、チェックボックスをオンまたは空にすることができるボタンの種類を使用してブール値を選択できます。 `IsChecked` プロパティは `CheckBox`の状態であり、`CheckedChanged` イベントは、状態が変化したときに発生します。<br /><br />API ドキュメント/[ガイド](~/xamarin-forms/user-interface/checkbox.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) | [![CheckBox の例](views-images/CheckBox.png "CheckBox の例")](views-images/CheckBox-Large.png#lightbox "CheckBox の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxPage.cs) |
|     |     |

### <a name="slider"></a>[スライダー]

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider)を使用すると、ユーザーは[`Minimum`](xref:Xamarin.Forms.Slider.Minimum)および[`Maximum`](xref:Xamarin.Forms.Slider.Maximum)プロパティで指定された連続する範囲から `double` 値を選択できます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Slider) / [ガイド](~/xamarin-forms/user-interface/slider.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![スライダーの例](views-images/Slider.png "スライダーの例")](views-images/Slider-Large.png#lightbox "スライダーの例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) |
|     |     |

### <a name="stepper"></a>ステッパ

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper)を使用すると、ユーザーは、 [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum)、 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum)、および[`Increment`](xref:Xamarin.Forms.Stepper.Increment)の各プロパティで指定された増分値の範囲から `double` 値を選択できます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Stepper)  / [ガイド](~/xamarin-forms/user-interface/stepper.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![ステッパの例](views-images/Stepper.png "ステッパの例")](views-images/Stepper-Large.png#lightbox "ステッパの例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) |
|     |     |

### <a name="switch"></a>Switch

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch)は、ユーザーがブール値を選択できるように、オン/オフスイッチの形式を使用します。 [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)プロパティはスイッチの状態であり、状態が変化したときに[`Toggled`](xref:Xamarin.Forms.Switch.Toggled)イベントが発生します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Switch) / [ガイド](~/xamarin-forms/user-interface/switch.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/) | [![スイッチの例](views-images/Switch.png "スイッチの例")](views-images/Switch-Large.png#lightbox "スイッチの例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker)を使用すると、ユーザーはプラットフォームの日付の選択で日付を選択できます。 許容される日付の範囲を[`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate)と[`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate)のプロパティで設定します。 [`Date`](xref:Xamarin.Forms.DatePicker.Date)プロパティは選択された日付であり、そのプロパティが変更されたときに[`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected)イベントが発生します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.DatePicker) / [ガイド](~/xamarin-forms/user-interface/datepicker.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![DatePicker の例](views-images/DatePicker.png "DatePicker の例")](views-images/DatePicker-Large.png#lightbox "DatePicker の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker)を使用すると、ユーザーは、プラットフォームのタイムピッカーを使用して時刻を選択できます。 [`Time`](xref:Xamarin.Forms.TimePicker.Time)プロパティは、選択された時刻です。 アプリケーションでは、 [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged)イベントのハンドラーをインストールすることによって、`Time` プロパティの変更を監視できます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TimePicker) / [ガイド](~/xamarin-forms/user-interface/timepicker.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![TimePicker の例](views-images/TimePicker.png "TimePicker の例")](views-images/TimePicker-Large.png#lightbox "TimePicker の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) |
|     |     |

## <a name="views-for-editing-text"></a>テキストを編集するためのビュー

これらの2つのクラスは、 [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard)プロパティを定義する[`InputView`](xref:Xamarin.Forms.InputView)クラスから派生します。

### <a name="entry"></a>エントリ

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry)を使用すると、ユーザーは1行のテキストを入力して編集できます。 テキストは[`Text`](xref:Xamarin.Forms.InputView.Text)プロパティとして使用でき、テキストが変更されたとき、またはユーザーが enter キーをタップして完了を通知するときに、 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)イベントと[`Completed`](xref:Xamarin.Forms.Entry.Completed)イベントが発生します。<br /><br />複数行のテキストを入力および編集するには、 [`Editor`](#editor)を使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Entry) / [ガイド](~/xamarin-forms/user-interface/text/entry.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![エントリの例](views-images/Entry.png "エントリの例")](views-images/Entry-Large.png#lightbox "エントリの例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) |
|     |     |

### <a name="editor"></a>エディター

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor)を使用すると、複数行のテキストを入力および編集できます。 テキストは[`Text`](xref:Xamarin.Forms.InputView.Text)プロパティとして使用でき、テキストが変更されたとき、またはユーザーが入力を完了したときに、 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)イベントと[`Completed`](xref:Xamarin.Forms.Editor.Completed)イベントが発生します。<br /><br />1行のテキストを入力および編集するには、 [`Entry`](#entry)ビューを使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Editor) / [ガイド](~/xamarin-forms/user-interface/text/editor.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![エントリの例](views-images/Editor.png "エディターの例")](views-images/Editor-Large.png#lightbox "エディターの例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) |
|     |     |

## <a name="views-to-indicate-activity"></a>アクティビティを示すためのビュー

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)は、アニメーションを使用して、進行状況を示すことなく、アプリケーションが長いアクティビティに関与していることを示します。 [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)プロパティは、アニメーションを制御します。<br /><br />アクティビティの進行状況がわかっている場合は、代わりに[`ProgressBar`](#progressbar)を使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ActivityIndicator) / [ガイド](~/xamarin-forms/user-interface/activityindicator.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/) | [![ActivityIndicator の例](views-images/ActivityIndicator.png "ActivityIndicator の例")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) |
|     |     |

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)はアニメーションを使用して、アプリケーションが時間のかかるアクティビティを経て進行していることを示します。 進行状況を示すには、 [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress)プロパティを 0 ~ 1 の範囲の値に設定します。<br /><br />アクティビティの進行状況が不明な場合は、代わりに[`ActivityIndicator`](#activityindicator)を使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ProgressBar) / [ガイド](~/xamarin-forms/user-interface/progressbar.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/) | [![ProgressBar の例](views-images/ProgressBar.png "ProgressBar の例")](views-images/ProgressBar-Large.png#lightbox "ProgressBar の例")<br />このページのコード / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml)と[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) |
|     |     |

## <a name="views-that-display-collections"></a>コレクションを表示するビュー

### <a name="carouselview"></a>CarouselView

|     |     |
| --- | --- |
| [`CarouselView`](xref:Xamarin.Forms.CarouselView)データ項目のスクロール可能な一覧が表示されます。 `ItemsSource` プロパティをオブジェクトのコレクションに設定し、`ItemTemplate` プロパティに、項目の書式設定を記述する[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトを設定します。 `CurrentItemChanged` イベントは、現在表示されている項目が変更されたことを通知します。これは、`CurrentItem` プロパティとして使用できます。<br /><br /> / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)の[ガイド](~/xamarin-forms/user-interface/carouselview/index.md) | [![CarouselView の例](views-images/CarouselView.png "CarouselView の例")](views-images/CarouselView-Large.png#lightbox "CarouselView の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselViewDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselViewDemoPage.cs) |
|     |     |

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView)異なるレイアウト指定を使用して、選択可能なデータ項目のスクロール可能な一覧を表示します。 これは、 [`ListView`](xref:Xamarin.Forms.ListView)に代わる、より柔軟性が高く、パフォーマンスに優れた方法を提供することを目的としています。 `ItemsSource` プロパティをオブジェクトのコレクションに設定し、`ItemTemplate` プロパティに、項目の書式設定を記述する[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトを設定します。 `SelectionChanged` イベントは、選択が行われたことを通知します。これは `SelectedItem` プロパティとして使用できます。<br /><br /> / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)の[ガイド](~/xamarin-forms/user-interface/collectionview/index.md) | [![CollectionView の例](views-images/CollectionView.png "CollectionView の例")](views-images/CollectionView-Large.png#lightbox "CollectionView の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs) |
|     |     |

### <a name="indicatorview"></a>IndicatorView

|     |     |
| --- | --- |
| `IndicatorView` `CarouselView`内の項目数を表すインジケーターを表示します。 `CarouselView`のインジケーターを表示するには、`CarouselView.IndicatorView` プロパティを `IndicatorView` オブジェクトに設定します。 <br /><br /> / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)の[ガイド](~/xamarin-forms/user-interface/indicatorview.md) | [![IndicatorView の例](views-images/IndicatorView.png "IndicatorView の例")](views-images/IndicatorView-Large.png#lightbox "IndicatorView の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/IndicatorViewDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/IndicatorViewDemoPage.cs) |
|     |     |

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView)は[`ItemsView`](xref:Xamarin.Forms.ItemsView`1)から派生し、選択可能なデータ項目のスクロール可能な一覧を表示します。 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource)プロパティをオブジェクトのコレクションに設定し、 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)プロパティに、項目の書式設定を記述する[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトを設定します。 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)イベントは、選択が行われたことを通知します。これは[`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem)プロパティとして使用できます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ListView) / [ガイド](~/xamarin-forms/user-interface/listview/index.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![ListView の例](views-images/ListView.png "ListView の例")](views-images/ListView-Large.png#lightbox "ListView の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) |
|     |     |

### <a name="picker"></a>ピッカー

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker)テキスト文字列の一覧から選択した項目を表示し、ビューをタップしたときにその項目を選択できるようにします。 [`Items`](xref:Xamarin.Forms.Picker.Items)プロパティを文字列のリスト、またはオブジェクトのコレクションに[`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource)プロパティを設定します。 [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged)イベントは、項目が選択されたときに発生します。<br /><br />`Picker` には、項目が選択されている場合にのみ項目の一覧が表示されます。 ページに残っているスクロール可能な一覧には、 [`ListView`](#listview)または[`TableView`](#tableview)を使用します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.Picker) / [ガイド](~/xamarin-forms/user-interface/picker/index.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![ピッカーの例](views-images/Picker.png "ピッカーの例")](views-images/Picker-Large.png#lightbox "ピッカーの例")<br />このページのコード / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml)と[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) |
|     |     |

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView)は、 [`Cell`](xref:Xamarin.Forms.Cell)型の行の一覧をオプションのヘッダーおよびサブヘッダーと共に表示します。 [`Root`](xref:Xamarin.Forms.TableView.Root)プロパティを[`TableRoot`](xref:Xamarin.Forms.TableRoot)型のオブジェクトに設定し、その `TableRoot`に[`TableSection`](xref:Xamarin.Forms.TableSection)オブジェクトを追加します。 各 `TableSection` は `Cell` オブジェクトのコレクションです。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TableView) / [ガイド](~/xamarin-forms/user-interface/tableview.md) / [サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![TableView の例](views-images/TableView.png "TableView の例")](views-images/TableView-Large.png#lightbox "TableView の例")<br />このページ / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewFormDemoPage.xaml)のコード[ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewFormDemoPage.cs) |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin フォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
