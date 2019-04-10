---
title: XAML コントロール
description: すべての Xamarin.Forms で定義されているビューは、XAML ファイルから参照できます。
ms.topic: article
ms.prod: xamarin
ms.assetid: 639BD392-1496-41BB-BB09-7652273AC9D8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/03/2019
ms.openlocfilehash: a8a61ac505eab8c458c49bde9184d6e96583d37f
ms.sourcegitcommit: be51b459a0a148ae3adca31d7599f53f7b2c3a68
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2019
ms.locfileid: "59020270"
---
# <a name="xaml-controls"></a>XAML コントロール

[![Download サンプル](~/media/shared/download.png) サンプルのダウンロード](https://developer.xamarin.com/samples/FormsGallery/)

ビューは、ラベル、ボタン、およびとよく呼ばれるスライダーなどのユーザー インターフェイス オブジェクト*コントロール*または*ウィジェット*他のグラフィカルなプログラミング環境でします。 すべての派生を Xamarin.Forms でサポートされるビュー、 [ `View` ](xref:Xamarin.Forms.View)クラス。

すべての Xamarin.Forms で定義されているビューは、XAML ファイルから参照できます。

## <a name="views-for-presentation"></a>プレゼンテーションのビュー

|     |     |
| --- | --- |
| <h3>BoxView</h3>特定の色の四角形が表示されます。<p align="center">![スクリーン ショット、BoxView の](xaml-controls-images/BoxView.png "BoxView")</p>[API](xref:Xamarin.Forms.BoxView) / [ガイド](https://developer.xamarin.com/guides/xamarin-forms/user-interface/boxview/) | <pre valign="center">&lt;BoxView Color="Accent"<br />         WidthRequest="150"<br />         HeightRequest="150"<br />         HorizontalOptions="Center"&gt;</pre></p> |
| <h3>イメージ</h3>ビットマップが表示されます。<p align="center">![イメージのスクリーン ショット](xaml-controls-images/Image.png "イメージ")</p>[API](xref:Xamarin.Forms.Image) / [ガイド](~/xamarin-forms/user-interface/images.md) | <pre>&lt;Image Source="https://aka.ms/campus.jpg"<br />       Aspect="AspectFit"<br />       HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>group1</h3>1 つまたは複数の行のテキストを表示します。<p align="center">![ラベルのスクリーン ショット](xaml-controls-images/Label.png "ラベル")</p>[API](xref:Xamarin.Forms.Label) / [ガイド](~/xamarin-forms/user-interface/text/label.md) | <p valign="center"><pre>&lt;Label Text="Hello, Xamarin.Forms!"<br />       FontSize="Large"<br />       FontAttributes="Italic"<br />       HorizontalTextAlignment="Center" /&gt;</pre></p> |
| <h3>マップ</h3>マップを表示します。<p align="center">![マップのスクリーン ショット](xaml-controls-images/Map.png "マップ")</p>[API](xref:Xamarin.Forms.Maps.Map) / [ガイド](~/xamarin-forms/user-interface/map.md) | <p valign="center"><pre>&lt;maps:Map ItemsSource="{Binding Locations}" /&gt;</pre></p> |
| <h3>WebView</h3>Web ページまたは HTML コンテンツを表示します。<p align="center">![Web ビューのスクリーン ショット](xaml-controls-images/WebView.png "WebView")</p>[API](xref:Xamarin.Forms.WebView) / [ガイド](~/xamarin-forms/user-interface/webview.md) | <p valign="center"><pre>&lt;WebView Source="https://docs.microsoft.com/xamarin/"<br/>         VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-initiate-commands"></a>コマンドの開始ビュー

|     |     |
| --- | --- |
| <h3>ボタン</h3>四角形のオブジェクトにテキストを表示します。<p align="center">![ボタンのスクリーン ショット](xaml-controls-images/Button.png "ボタン")</p>[API](xref:Xamarin.Forms.Button) / [ガイド](~/xamarin-forms/user-interface/button.md) | <p valign="center"><pre>&lt;Button Text="Click Me!"<br />        Font="Large"<br />        BorderWidth="1"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand"<br />        Clicked="OnButtonClicked" /&gt;</pre></p> |
| <h3>ImageButton</h3>四角形のオブジェクトでは、イメージを表示します。<p align="center">![ImageButton のスクリーン ショット](xaml-controls-images/ImageButton.png "ImageButton")</p>[API](xref:Xamarin.Forms.ImageButton) / [ガイド](~/xamarin-forms/user-interface/imagebutton.md) | <p valign="center"><pre>&lt;ImageButton Source="XamarinLogo.png"<br />             HorizontalOptions="Center"<br />             VerticalOptions="CenterAndExpand"<br />             Clicked="OnImageButtonClicked" /&gt;</pre></p> |
| <h3>SearchBar</h3>検索を実行するための検索バーを表示します。<p align="center">![スクリーン ショット、SearchBar の](xaml-controls-images/SearchBar.png "SearchBar")</p>[API](xref:Xamarin.Forms.SearchBar) | <p valign="center"><pre>&lt;SearchBar Placeholder="Xamarin.Forms Property"<br />           SearchButtonPressed="OnSearchBarButtonPressed" /&gt;</pre></p> |
|     |     |

## <a name="views-for-setting-values"></a>値の設定のビュー

|     |     |
| --- | --- |
| <h3>スライダー</h3>選択できるように、`double`を連続する範囲の値。<p align="center">![スライダーのスクリーン ショット](xaml-controls-images/Slider.png "スライダー")</p>[API](xref:Xamarin.Forms.Slider) / [ガイド](~/xamarin-forms/user-interface/slider.md) | <p valign="center"><pre>&lt;Slider Minimum="0"<br />        Maximum="100"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ステッパ</h3>選択できるように、`double`増分の範囲からの値。<p align="center">![スクリーン ショット、ステッパの](xaml-controls-images/Stepper.png "ステッパ")</p>[API](xref:Xamarin.Forms.Stepper) / [ガイド](~/xamarin-forms/user-interface/stepper.md) | <p valign="center"><pre>&lt;Stepper Minimum="0"<br />         Maximum="10"<br />         Increment="0.1"<br />         HorizontalOptions="Center"<br />         VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>切り替え</h3>選択できるように、`boolean`値。<p align="center">![スイッチのスクリーン ショット](xaml-controls-images/Switch.png "スイッチ")</p>[API](xref:Xamarin.Forms.Switch) | <p valign="center"><pre>&lt;Switch IsToggled="false"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>DatePicker</h3>日付を選択をできます。<p align="center">![スクリーン ショット、DatePicker の](xaml-controls-images/DatePicker.png "DatePicker")</p>[API](xref:Xamarin.Forms.DatePicker) / [ガイド](~/xamarin-forms/user-interface/datepicker.md) | <p valign="center"><pre>&lt;DatePicker Format="D"<br/>            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>TimePicker</h3>一度に選択できます。<p align="center">![スクリーン ショット、TimePicker の](xaml-controls-images/TimePicker.png "TimePicker")</p>[API](xref:Xamarin.Forms.TimePicker) / [ガイド](~/xamarin-forms/user-interface/timepicker.md) | <p valign="center"><pre>&lt;TimePicker Format="T"<br />            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-for-editing-text"></a>テキストを編集するためのビュー

|     |     |
| --- | --- |
| <h3>入力</h3>1 行のテキストを入力して編集を使用できます。<p align="center">![エントリのスクリーン ショット](xaml-controls-images/Entry.png "エントリ")</p>[API](xref:Xamarin.Forms.Entry) / [ガイド](~/xamarin-forms/user-interface/text/entry.md) | <p valign="center"><pre>&lt;Entry Keyboard="Email"<br />       Placeholder="Enter email address"<br />       VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>エディター</h3>複数行のテキストを入力して編集を許可します。<p align="center">![エディターのスクリーン ショット](xaml-controls-images/Editor.png "ラベル")</p>[API](xref:Xamarin.Forms.Editor) / [ガイド](~/xamarin-forms/user-interface/text/editor.md) | <p valign="center"><pre>&lt;Editor VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-to-indicate-activity"></a>アクティビティを示すビュー

|     |     |
| --- | --- |
| <h3>ActivityIndicator</h3>進行状況の情報を与えることがなく時間がかかる作業では、アプリケーションが進行中、表示するアニメーションを表示します。<p align="center">![スクリーン ショット、ActivityIndicator の](xaml-controls-images/ActivityIndicator.png "ActivityIndicator")</p>[API](xref:Xamarin.Forms.ActivityIndicator) | <p valign="center"><pre>&lt;ActivityIndicator IsRunning="True"<br />                   VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ProgressBar</h3>時間のかかるアクティビティを通じて、アプリケーションの進行状況を表示するアニメーションを表示します。<p align="center">![ProgressBar のスクリーン ショット](xaml-controls-images/ProgressBar.png "ProgressBar")</p>[API](xref:Xamarin.Forms.ProgressBar) | <p valign="center"><pre>&lt;ProgressBar Progress=".5"<br />             VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-display-collections"></a>コレクションを表示するビュー

|     |     |
| --- | --- |
| <h3>CollectionView</h3>を別のレイアウトの仕様を使用して、選択可能なデータ項目のスクロール可能な一覧が表示されます。<p align="center">![スクリーン ショット、CollectionView の](xaml-controls-images/CollectionView.png "CollectionView")</p>[ガイド](~/xamarin-forms/user-interface/collectionview/index.md) | <p valign="center"><pre>&lt;CollectionView ItemsSource="{Binding Monkeys}"&gt;<br/>                ItemTemplate="{StaticResource MonkeyTemplate}"<br />    &lt;CollectionView.ItemsLayout&gt;<br />       &lt;GridItemsLayout Orientation="Vertical"<br />                        Span="2" /&gt;<br />    &lt;/CollectionView.ItemsLayout&gt;<br />&lt;/CollectionView/&gt;</pre></p> |
| <h3>ListView</h3>選択可能なデータ項目のスクロール可能な一覧が表示されます。<p align="center">![スクリーン ショット、ListView の](xaml-controls-images/ListView.png "ListView")</p>[API](xref:Xamarin.Forms.ListView) / [ガイド](~/xamarin-forms/user-interface/listview/index.md) | <p valign="center"><pre>&lt;ListView ItemsSource="{Binding Monkeys}"&gt;<br />          ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p> |
| <h3>ピッカー</h3>テキスト文字列の一覧から選択項目を表示します。<p align="center">![ピッカーのスクリーン ショット](xaml-controls-images/Picker.png "ピッカー")</p>[API](xref:Xamarin.Forms.Picker) / [ガイド](~/xamarin-forms/user-interface/picker/index.md) | <p valign="center"><pre>&lt;Picker Title="Select a monkey"<br />        TitleColor="Red"&gt;<br />  &lt;Picker.ItemsSource&lt;<br />    &lt;x:Array Type="{x:Type x:String}"&gt;<br />      &lt;x:String&gt;Baboon&lt;/x:String&gt;<br />      &lt;x:String&gt;Capuchin Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Blue Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Squirrel Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Golden Lion Tamarin&lt;/x:String&gt;<br />      &lt;x:String&gt;Howler Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Japanese Macaque&lt;/x:String&gt;<br />    &lt;/x:Array&gt;<br />  &lt;/Picker.ItemsSource&gt;<br />&lt;/Picker&gt;</pre></p> |
| <h3>TableView</h3>対話形式の行の一覧を表示します。<p align="center">![テーブルのスクリーン ショット](xaml-controls-images/TableView.png "テーブル")</p>[API](xref:Xamarin.Forms.TableView) / [ガイド](~/xamarin-forms/user-interface/tableview.md) | <p valign="center"><pre>&lt;TableView Intent="Settings"&gt;<br />    &lt;TableRoot&gt;<br />        &lt;TableSection Title="Ring"&gt;<br />            &lt;SwitchCell Text="New Voice Mail" /&gt;<br />            &lt;SwitchCell Text="New Mail" On="true" /&gt;<br />        &lt;/TableSection&gt;<br />    &lt;/TableRoot&gt;<br />&lt;/TableView&gt;</pre></p> |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms FormsGallery サンプル](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
