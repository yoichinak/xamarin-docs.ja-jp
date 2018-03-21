---
title: "Xamarin.Forms ビュー"
description: "Xamarin.Forms ビューは、クロスプラット フォーム モバイル ユーザー インターフェイスのビルド ブロックです。"
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: ef4de2d544f3bcfb661b29dd90de738ae0442373
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2018
---
# <a name="xamarinforms-views"></a>Xamarin.Forms ビュー

_Xamarin.Forms ビューは、クロスプラット フォーム モバイル ユーザー インターフェイスのビルド ブロックです。_

ビューは、ラベル、ボタン、およびとしてよく知られているスライダーなどのユーザー インターフェイス オブジェクト*コントロール*または*ウィジェット*グラフィカル他のプログラミング環境でします。 すべて派生 Xamarin.Forms でサポートされているビュー、 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)クラスです。 これらは、複数のカテゴリに分類できます。

## <a name="views-for-presentation"></a>プレゼンテーションのビュー

### <a name="label"></a>group1

|     |     |
| --- | --- |
| [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 単一行のテキスト文字列または複数行のテキスト ブロック、定数または変数の書式設定のいずれかが表示されます。 設定、 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)プロパティを定数の書式設定、または一連の文字列に、 [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/)プロパティを[ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/)変数に対するオブジェクト書式設定します。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) / [ガイド](~/xamarin-forms/user-interface/text/label.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![例のラベルを付ける](views-images/Label.png "例ラベルを付ける")](views-images/Label-Large.png#lightbox "例ラベルを付ける")<br /> [このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>イメージ

|     |     |
| --- | --- |
| [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) ビットマップを表示します。 ビットマップは、一般的なプロジェクトまたはプラットフォームのプロジェクトにリソースとして埋め込ままたは .NET を使用して作成された、Web 経由でダウンロードできます`Stream`オブジェクト。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) / [ガイド](~/xamarin-forms/user-interface/images.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![例 画像の](views-images/Image.png "例のイメージ")](views-images/Image-Large.png#lightbox "画像の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) 色を純色の四角形を表示、 [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/)プロパティです。 `BoxView` 40 x 40 の既定のサイズの要求がします。 他のサイズを割り当てる、 [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)と[ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/)プロパティです。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) / [ガイド](~/xamarin-forms/user-interface/boxview.md) / [サンプル 1](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)、 [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)、 [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)、 [4](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)、 [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)、および[6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![BoxView 例](views-images/BoxView.png "BoxView 例")](views-images/BoxView-Large.png#lightbox "BoxView 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) かどうかに基づいて Web ページまたは HTML コンテンツを表示、 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/)プロパティに設定されている、 [ `UriWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UrlWebViewSource/)または[ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/)オブジェクト。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) / [ガイド](~/xamarin-forms/user-interface/webview.md) / [サンプル 1](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)と[2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![WebView 例](views-images/WebView.png "WebView 例")](views-images/WebView-Large.png#lightbox "WebView の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/) iOS および Android のプロジェクトでは、OpenGL のグラフィックを表示します。 ユニバーサル Windows プラットフォームのサポートされていません。 IOS および Android のプロジェクトへの参照が必要、 **OpenTK 1.0**アセンブリまたは**OpenTK**バージョン 1.0.0.0 アセンブリ。 `OpenGLView` 共有プロジェクトで使用する方が簡単です。PCL または .NET 標準ライブラリで使用されている場合、依存関係サービスも必要になります (示すように、サンプル コードで)。<br /><br />これは、Xamarin.Forms に組み込まれているのみグラフィックス機能が Xamarin.Forms アプリケーションは、グラフィックスを使用しても表示できる[ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md)、 [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)、または[ `UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/)<br /><br /> | [![OpenGLView 例](views-images/OpenGLView.png "OpenGLView 例")](views-images/OpenGLView-Large.png#lightbox "OpenGLView 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>マップ

|     |     |
| --- | --- |
| [`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) マップを表示します。 **Xamarin.Forms.Maps** Nuget パッケージをインストールする必要があります。 Android ユニバーサル Windows プラットフォーム マップ承認キーが必要とします。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) / [ガイド](~/xamarin-forms/user-interface/map.md) / [サンプル](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![マップの使用例](views-images/Map.png "例のマップ")](views-images/Map-Large.png#lightbox "例のマップ")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>コマンドを開始するビュー

### <a name="button"></a>ボタン

|     |     |
| --- | --- |
| [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) テキストを表示する四角形のオブジェクトは、発生して、 [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/)イベントが押されるとします。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) | [![ボタンの例](views-images/Button.png "例をボタン")](views-images/Button-Large.png#lightbox "ボタンの例")<br /> [このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) 検索を実行するアプリケーションのことを通知する型、文字列およびボタン (または、キーボードのキー) にユーザーの領域が表示されます。 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.Text/)プロパティは、テキストへのアクセスを提供し、 [ `SearchButtonPressed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/)イベントは、ボタンが押されたことを示します。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) | [![SearchBar 例](views-images/SearchBar.png "SearchBar 例")](views-images/SearchBar-Large.png#lightbox "SearchBar 例")<br /> [このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>値の設定のビュー 

### <a name="slider"></a>スライダー

|     |     |
| --- | --- |
| [`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) 選択することができます、`double`を連続する範囲で指定された値、 [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/)と[ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/)プロパティです。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) | [![スライダーの使用例](views-images/Slider.png "スライダー例")](views-images/Slider-Large.png#lightbox "スライダーの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>ステッパ

|     |     |
| --- | --- |
| [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) 選択することができます、`double`で指定された増分値の範囲から値を[ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Minimum/)、 [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Maximum/)、および[ `Increment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/)プロパティです。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) | [![ステッパ例](views-images/Stepper.png "ステッパ例")](views-images/Stepper-Large.png#lightbox "ステッパ例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>切り替え 

|     |     |
| --- | --- |
| [`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) 使用するブール値を選択するユーザーのオン/オフ スイッチの形式をとります。 [ `IsToggled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/)プロパティは、スイッチの状態と[ `Toggled` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/)状態が変更されたときに発生します。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) | [![例を切り替える](views-images/Switch.png "スイッチの例")](views-images/Stepper-Large.png#lightbox "スイッチの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) プラットフォームの日付選択カレンダーで日付を選択することができます。 許容される日付の範囲を設定、 [ `MinimumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/)と[ `MaximumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/)プロパティです。 [ `Date` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/)プロパティは、選択した日付と[ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/)イベントは、そのプロパティが変更されたときに発生します。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) / [ガイド](~/xamarin-forms/user-interface/datepicker.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![DatePicker 例](views-images/DatePicker.png "DatePicker 例")](views-images/DatePicker-Large.png#lightbox "DatePicker 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) プラットフォーム時間ピッカーを使用して時刻を選択することができます。 [ `Time` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/)プロパティは、選択された時刻。 アプリケーションでの変更を監視できます、`Time`プロパティのハンドラーをインストールすることによって、 [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/)イベント。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) | [![TimePicker 例](views-images/TimePicker.png "TimePicker 例")](views-images/TimePicker-Large.png#lightbox "TimePicker 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>テキストを編集するためのビュー

これら 2 つのクラスから派生し、 [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/)を定義するクラス、 [ `Keyboard` ](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/)プロパティです。

<a name="entry" />

### <a name="entry"></a>入力

|     |     |
| --- | --- |
| [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 入力し、1 行のテキストを編集することができます。 テキストは、 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/)プロパティ、および[ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/)と[ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/)イベントが発生したときにテキストの変更、またはユーザー完了を通知するには、enter キーをタップします。<br /><br />使用して、 [ `Editor` ](#editor)の入力と複数行のテキストを編集します。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) / [ガイド](~/xamarin-forms/user-interface/text/entry.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![エントリの例](views-images/Entry.png "エントリ例")](views-images/Entry-Large.png#lightbox "エントリの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>エディター

|     |     |
| --- | --- |
| [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) 入力し、複数行のテキストを編集することができます。 テキストは、 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Editor.Text/)プロパティ、および[ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/)と[ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/)イベントが発生したときにテキストの変更、またはユーザー完了を通知します。<br /><br />使用して、 [ `Entry` ](#entry)の入力と 1 つの行のテキストを編集用のビューです。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) / [ガイド](~/xamarin-forms/user-interface/text/editor.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![エントリの例](views-images/Editor.png "エディター例")](views-images/Editor-Large.png#lightbox "エディターの使用例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>アクティビティを示すためにビュー

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) アニメーションを使用しているアプリケーションが進行中で時間がかかる作業の進行状況の兆候を与えることがなくを示します。 [ `IsRunning` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ActivityIndicator.IsRunning/)プロパティは、アニメーションを制御します。<br /><br />アクティビティの進行状況がわかっている場合を使用して、 [ `ProgressBar` ](#progressbar)代わりにします。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) | [![ActivityIndicator 例](views-images/ActivityIndicator.png "ActivityIndicator 例")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) アニメーションを使用して、時間がかかる作業を通じて、アプリケーションの進行状況を表示します。 設定、 [ `Progress` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ProgressBar.Progress/)プロパティを 0 ~ 1 に、進行状況を示す値。<br /><br />使用して、アクティビティの進行状況が不明の場合、 [ `ActivityIndicator` ](#activityindicator)代わりにします。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) | [![ProgressBar の使用例](views-images/ProgressBar.png "ProgressBar 例")](views-images/ProgressBar-Large.png#lightbox "ProgressBar の使用例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>コレクションを表示するビュー

### <a name="picker"></a>ピッカー

|     |     |
| --- | --- |
| [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) テキスト文字列の一覧から選択した項目を表示でき、ビューがタップされたときに、その項目を選択できます。 設定、 [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) 、文字列のリストにプロパティまたは[ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/)プロパティ オブジェクトのコレクションをします。 [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/)イベント項目が選択されているときに発生します。<br /><br />`Picker`が選択されている場合にのみ、項目の一覧を表示します。 使用して、 [ `ListView` ](#listView)または[ `TableView` ](#tableView)ページに残っているスクロール可能な一覧についてはします。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) / [ガイド](~/xamarin-forms/user-interface/picker/index.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![ピッカー例](views-images/Picker.png "ピッカー例")](views-images/Picker-Large.png#lightbox "ピッカーの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml)で[分離コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 派生した[ `ItemsView[Cell]` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)選択可能なデータ項目のスクロール可能な一覧が表示されます。 設定、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/)プロパティ オブジェクト、およびセットのコレクションを[ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/)プロパティを[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)アイテムがどのようにを表すオブジェクトです。書式設定します。 [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/)イベントとしては、選択が行われたことを通知する、 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/)プロパティです。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) / [ガイド](~/xamarin-forms/user-interface/listview/index.md) / [サンプル](https://developer.xamarin.com/samples/WorkingWithListview) | [![ListView の例](views-images/ListView.png "ListView 例")](views-images/ListView-Large.png#lightbox "ListView の例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) 型の行の一覧を表示[ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/)省略可能なヘッダーと含むサブ ヘッダーを使用します。 設定、 [ `Root` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/)プロパティ型のオブジェクトを[ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/)、し、追加[ `TableSection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/)にオブジェクト`TableRoot`です。 各`TableSection`のコレクションは、`Cell`オブジェクト。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) / [ガイド](~/xamarin-forms/user-interface/tableview.md) / [サンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![テーブルの使用例](views-images/TableView.png "テーブル例")](views-images/TableView-Large.png#lightbox "テーブルの例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery サンプル](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API ドキュメント](https://developer.xamarin.com/api/root/Xamarin.Forms/)
