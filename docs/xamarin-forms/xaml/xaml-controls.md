---
title: XAML コントロール
description: 「」で定義されているすべてのビューは、 Xamarin.Forms XAML ファイルから参照できます。
ms.topic: article
ms.prod: xamarin
ms.assetid: 639BD392-1496-41BB-BB09-7652273AC9D8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/09/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 94c451305f04294924b3f7115c4f2b3a7a6bad07
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918664"
---
# <a name="xaml-controls"></a>XAML コントロール

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

ビューは、ラベル、ボタン、スライダーなどのユーザーインターフェイスオブジェクトであり、他のグラフィカルプログラミング環境では、*コントロール*や*ウィジェット*と呼ばれることがよくあります。 でサポートされるすべてのビューは、 Xamarin.Forms クラスから派生し [`View`](xref:Xamarin.Forms.View) ます。

「」で定義されているすべてのビューは、 Xamarin.Forms XAML ファイルから参照できます。

## <a name="views-for-presentation"></a>表示用のビュー

| View | 例 |
| --- | --- |
| <h3>BoxView</h3>特定の色の四角形を表示します。<p align="center">![BoxView のスクリーンショット](xaml-controls-images/BoxView.png "BoxView")</p>[API](xref:Xamarin.Forms.BoxView)  / [ガイド](~/xamarin-forms/user-interface/boxview.md) | <p valign="center"><pre>&lt;BoxView Color="Accent"<br />         WidthRequest="150"<br />         HeightRequest="150"<br />         HorizontalOptions="Center"&gt;</pre></p> |
| <h3>Ellipse</h3>楕円または円を表示します。<p align="center">![楕円のスクリーンショット](xaml-controls-images/Ellipse.png "Ellipse")</p>[API](xref:Xamarin.Forms.Shapes.Ellipse)  / [ガイド](~/xamarin-forms/user-interface/shapes/ellipse.md) | <p valign="center"><pre>&lt;Ellipse Fill="Red"<br />         WidthRequest="150"<br />         HeightRequest="50"<br />         HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Expander</h3>コンテンツをホストするための拡張可能なコンテナーを提供します。<p align="center">![展開コントロールのスクリーンショット](xaml-controls-images/Expander.png "Expander")</p>[ガイド](~/xamarin-forms/user-interface/expander.md) | <pre>&lt;Expander&gt;<br />    &lt;Expander.Header&gt;<br />        &lt;Label Text="Baboon" /&gt;<br />    &lt;/Expander.Header&gt;<br />    &lt;Image Source="Baboon.png"<br />           Aspect="AspectFill" /&gt;<br />&lt;/Expander&gt;</pre></p> |
| <h3>Image</h3>ビットマップを表示します。<p align="center">![イメージのスクリーンショット](xaml-controls-images/Image.png "Image")</p>[API](xref:Xamarin.Forms.Image)  / [ガイド](~/xamarin-forms/user-interface/images.md) | <pre>&lt;Image Source="https://aka.ms/campus.jpg"<br />       Aspect="AspectFit"<br />       HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>ラベル</h3>1行以上のテキストを表示します。<p align="center">![ラベルのスクリーンショット](xaml-controls-images/Label.png "ラベル")</p>[API](xref:Xamarin.Forms.Label)  / [ガイド](~/xamarin-forms/user-interface/text/label.md) | <p valign="center"><pre>&lt;Label Text="Hello, Xamarin.Forms!"<br />       FontSize="Large"<br />       FontAttributes="Italic"<br />       HorizontalTextAlignment="Center" /&gt;</pre></p> |
| <h3>行</h3>線を表示します。<p align="center">![線のスクリーンショット](xaml-controls-images/Line.png "行")</p>[API](xref:Xamarin.Forms.Shapes.Line)  / [ガイド](~/xamarin-forms/user-interface/shapes/line.md) | <p valign="center"><pre>&lt;Line X1="40"<br />      Y1="0"<br />      X2="0"<br />      Y2="120"<br />      Stroke="Red"<br />      HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>マップ</h3>マップを表示します。<p align="center">![マップのスクリーンショット](xaml-controls-images/Map.png "マップ")</p>[API](xref:Xamarin.Forms.Maps.Map)  / [ガイド](~/xamarin-forms/user-interface/map/index.md) | <p valign="center"><pre>&lt;maps:Map ItemsSource="{Binding Locations}" /&gt;</pre></p> |
| <h3>MediaElement</h3>ビデオまたはオーディオを再生します。<p align="center">![MediaElement のスクリーンショット](xaml-controls-images/MediaElement.png "MediaELement")</p>[API](xref:Xamarin.Forms.MediaElement)  / [ガイド](~/xamarin-forms/user-interface/mediaelement.md) | <p valign="center"><pre>&lt;MediaElement Source="https://sec.ch9.ms/ch9/XamarinShow_mid.mp4"<br />              AutoPlay="True"<br />              ShowsPlaybackControls="True" /&gt;</pre></p> |
| <h3>パス</h3>曲線と複雑な図形を表示します。<p align="center">![パスのスクリーンショット](xaml-controls-images/Path.png "パス")</p>[API](xref:Xamarin.Forms.Shapes.Path)  / [ガイド](~/xamarin-forms/user-interface/shapes/path.md) | <p valign="center"><pre>&lt;Path Stroke="Black"<br />      Aspect="Uniform"<br />      HorizontalOptions="Center"<br />      HeightRequest="100"<br />      WidthRequest="100"<br />      Data="M13.9,16.2<br />            L32,16.2 32,31.9 13.9,30.1Z<br />            M0,16.2<br />            L11.9,16.2 11.9,29.9 0,28.6Z<br />            M11.9,2<br />            L11.9,14.2 0,14.2 0,3.3Z<br />            M32,0<br />            L32,14.2 13.9,14.2 13.9,1.8Z" /&gt;</pre></p> |
| <h3>多角形</h3>多角形を表示します。<p align="center">![多角形のスクリーンショット](xaml-controls-images/Polygon.png "多角形")</p>[API](xref:Xamarin.Forms.Shapes.Polygon)  / [ガイド](~/xamarin-forms/user-interface/shapes/polygon.md) | <p valign="center"><pre>&lt;Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96,<br/>                 50 96, 48 192, 150 200 144 48"<br />         Fill="Blue"<br />         Stroke="Red"<br />         StrokeThickness="3"<br />         HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>ポリライン</h3>接続された一連の直線を表示します。<p align="center">![ポリラインのスクリーンショット](xaml-controls-images/Polyline.png "ポリライン")</p>[API](xref:Xamarin.Forms.Shapes.Polyline)  / [ガイド](~/xamarin-forms/user-interface/shapes/Polyline.md) | <p valign="center"><pre>&lt;Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0<br />                  43,60 48,30 100,30"<br />          Stroke="Red"<br />          HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Rectangle</h3>四角形または正方形を表示します。<p align="center">![四角形のスクリーンショット](xaml-controls-images/Rectangle.png "Rectangle")</p>[API](xref:Xamarin.Forms.Shapes.Rectangle)  / [ガイド](~/xamarin-forms/user-interface/shapes/rectangle.md) | <p valign="center"><pre>&lt;Rectangle Fill="Red"<br />           WidthRequest="150"<br />           HeightRequest="50"<br />           HorizontalOptions="Center" /&gt;</pre></p> |  
| <h3>WebView</h3>Web ページまたは HTML コンテンツを表示します。<p align="center">![WebView のスクリーンショット](xaml-controls-images/WebView.png "WebView")</p>[API](xref:Xamarin.Forms.WebView)  / [ガイド](~/xamarin-forms/user-interface/webview.md) | <p valign="center"><pre>&lt;WebView Source="https://docs.microsoft.com/xamarin/"<br/>         VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-initiate-commands"></a>コマンドを開始するビュー

| View | 例 |
| --- | --- |
| <h3>Button</h3>四角形のオブジェクトにテキストを表示します。<p align="center">![ボタンのスクリーンショット](xaml-controls-images/Button.png "Button")</p>[API](xref:Xamarin.Forms.Button)  / [ガイド](~/xamarin-forms/user-interface/button.md) | <p valign="center"><pre>&lt;Button Text="Click Me!"<br />        Font="Large"<br />        BorderWidth="1"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand"<br />        Clicked="OnButtonClicked" /&gt;</pre></p> |
| <h3>ImageButton</h3>四角形のオブジェクトに画像を表示します。<p align="center">![ImageButton のスクリーンショット](xaml-controls-images/ImageButton.png "ImageButton")</p>[API](xref:Xamarin.Forms.ImageButton)  / [ガイド](~/xamarin-forms/user-interface/imagebutton.md) | <p valign="center"><pre>&lt;ImageButton Source="XamarinLogo.png"<br />             HorizontalOptions="Center"<br />             VerticalOptions="CenterAndExpand"<br />             Clicked="OnImageButtonClicked" /&gt;</pre></p> |
| <h3>RadioButton</h3>セットから1つのオプションを選択できるようにします。<p align="center">![RadioButton のスクリーンショット](xaml-controls-images/RadioButton.png "RadioButton")</p>[ガイド](~/xamarin-forms/user-interface/radiobutton.md) | <p valign="center"><pre>&lt;RadioButton Text="Pineapple"<br/>             CheckedChanged="OnRadioButtonCheckedChanged" /&gt;</pre></p> |
| <h3>RefreshView</h3>スクロール可能なコンテンツのプルから更新機能を提供します。<p align="center">![RefreshView のスクリーンショット](xaml-controls-images/RefreshView.png "RefreshView")</p>[ガイド](~/xamarin-forms/user-interface/refreshview.md) | <p valign="center"><pre>&lt;RefreshView IsRefreshing="{Binding IsRefreshing}"<br />             Command="{Binding RefreshCommand}" &gt;<br />    &lt;!-- Scrollable control goes here --&gt;<br />&lt;/RefreshView&gt;</pre></p> |
| <h3>SearchBar</h3> 検索を実行するために使用するユーザー入力を受け入れます。<p align="center">![SearchBar のスクリーンショット](xaml-controls-images/SearchBar.png "SearchBar")</p>[ガイド](~/xamarin-forms/user-interface/searchbar.md) | <p valign="center"><pre>&lt;SearchBar Placeholder="Enter search term"<br />           SearchButtonPressed="OnSearchBarButtonPressed" /&gt;</pre></p> |
| <h3>SwipeView</h3> スワイプジェスチャによって表示されるコンテキストメニュー項目を提供します。<p align="center">![SwipeView のスクリーンショット](xaml-controls-images/SwipeView.png "SwipeView")</p>[ガイド](~/xamarin-forms/user-interface/swipeview.md) | <p valign="center"><pre>&lt;SwipeView&gt;<br />    &lt;SwipeView.LeftItems&gt;<br />        &lt;SwipeItems&gt;<br />            &lt;SwipeItem Text="Delete"<br />                       IconImageSource="delete.png"<br />                       BackgroundColor="LightPink"<br />                       Invoked="OnDeleteInvoked" /&gt;<br />        &lt;/SwipeItems&gt;<br />    &lt;/SwipeView.LeftItems&gt;<br />    &lt;!-- Content --&gt;<br />&lt;/SwipeView&gt;</pre></p> |
|     |     |

## <a name="views-for-setting-values"></a>値を設定するためのビュー

| View | 例 |
| --- | --- |
| <h3>CheckBox</h3>値の選択を許可し `boolean` ます。<p align="center">![チェックボックスのスクリーンショット](xaml-controls-images/CheckBox.png "CheckBox")</p> [ガイド](~/xamarin-forms/user-interface/checkbox.md) | <p valign="center"><pre>&lt;CheckBox IsChecked="true"<br />          HorizontalOptions="Center"<br />          VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>スライダー</h3>連続した範囲から値を選択できるようにし `double` ます。<p align="center">![スライダーのスクリーンショット](xaml-controls-images/Slider.png "Slider")</p>[API](xref:Xamarin.Forms.Slider)  / [ガイド](~/xamarin-forms/user-interface/slider.md) | <p valign="center"><pre>&lt;Slider Minimum="0"<br />        Maximum="100"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ステッパ</h3>増分範囲から値を選択できるようにし `double` ます。<p align="center">![ステッパのスクリーンショット](xaml-controls-images/Stepper.png "ステッパ")</p>[API](xref:Xamarin.Forms.Stepper)  / [ガイド](~/xamarin-forms/user-interface/stepper.md) | <p valign="center"><pre>&lt;Stepper Minimum="0"<br />         Maximum="10"<br />         Increment="0.1"<br />         HorizontalOptions="Center"<br />         VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Switch</h3>値の選択を許可し `boolean` ます。<p align="center">![スイッチのスクリーンショット](xaml-controls-images/Switch.png "Switch")</p>[API](xref:Xamarin.Forms.Switch)  / [ガイド](~/xamarin-forms/user-interface/switch.md)| <p valign="center"><pre>&lt;Switch IsToggled="false"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>DatePicker</h3>日付の選択を許可します。<p align="center">![DatePicker のスクリーンショット](xaml-controls-images/DatePicker.png "DatePicker")</p>[API](xref:Xamarin.Forms.DatePicker)  / [ガイド](~/xamarin-forms/user-interface/datepicker.md) | <p valign="center"><pre>&lt;DatePicker Format="D"<br/>            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>TimePicker</h3>時間の選択を許可します。<p align="center">![TimePicker のスクリーンショット](xaml-controls-images/TimePicker.png "TimePicker")</p>[API](xref:Xamarin.Forms.TimePicker)  / [ガイド](~/xamarin-forms/user-interface/timepicker.md) | <p valign="center"><pre>&lt;TimePicker Format="T"<br />            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-for-editing-text"></a>テキストを編集するためのビュー

| View | 例 |
| --- | --- |
| <h3>入力</h3>1行のテキストを入力して編集できるようにします。<p align="center">![エントリのスクリーンショット](xaml-controls-images/Entry.png "入力")</p>[API](xref:Xamarin.Forms.Entry)  / [ガイド](~/xamarin-forms/user-interface/text/entry.md) | <p valign="center"><pre>&lt;Entry Keyboard="Email"<br />       Placeholder="Enter email address"<br />       VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>エディター</h3>複数行のテキストを入力および編集できます。<p align="center">![エディターのスクリーンショット](xaml-controls-images/Editor.png "ラベル")</p>[API](xref:Xamarin.Forms.Editor)  / [ガイド](~/xamarin-forms/user-interface/text/editor.md) | <p valign="center"><pre>&lt;Editor VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-to-indicate-activity"></a>アクティビティを示すためのビュー

| View | 例 |
| --- | --- |
| <h3>ActivityIndicator</h3>アプリケーションが時間のかかるアクティビティに関与していることを示すアニメーションを表示します。進行状況は示されません。<p align="center">![ActivityIndicator のスクリーンショット](xaml-controls-images/ActivityIndicator.png "ActivityIndicator")</p>[API](xref:Xamarin.Forms.ActivityIndicator)  / [ガイド](~/xamarin-forms/user-interface/activityindicator.md) | <p valign="center"><pre>&lt;ActivityIndicator IsRunning="True"<br />                   VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ProgressBar</h3>アプリケーションが時間のかかるアクティビティを経て進行していることを示すアニメーションを表示します。<p align="center">![ProgressBar のスクリーンショット](xaml-controls-images/ProgressBar.png "ProgressBar")</p>[API](xref:Xamarin.Forms.ProgressBar)  / [ガイド](~/xamarin-forms/user-interface/progressbar.md) | <p valign="center"><pre>&lt;ProgressBar Progress=".5"<br />             VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-display-collections"></a>コレクションを表示するビュー

| View | 例 |
| --- | --- |
| <h3>CarouselView</h3>データ項目のスクロール可能な一覧を表示します。<p align="center">![CarouselView のスクリーンショット](xaml-controls-images/CarouselView.png "CarouselView")</p>[ガイド](~/xamarin-forms/user-interface/carouselview/index.md) | <p valign="center"><pre>&lt;CarouselView ItemsSource="{Binding Monkeys}"&gt;<br/>              ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p>|
| <h3>CollectionView</h3>さまざまなレイアウト仕様を使用して、選択可能なデータ項目のスクロール可能な一覧を表示します。<p align="center">![CollectionView のスクリーンショット](xaml-controls-images/CollectionView.png "CollectionView")</p>[ガイド](~/xamarin-forms/user-interface/collectionview/index.md) | <p valign="center"><pre>&lt;CollectionView ItemsSource="{Binding Monkeys}"&gt;<br/>                ItemTemplate="{StaticResource MonkeyTemplate}"<br />                ItemsLayout="VerticalGrid, 2" /&gt;</pre></p> |
| <h3>IndicatorView</h3>内の項目の数を表すインジケーターを表示 `CarouselView` します。<p align="center">![IndicatorView のスクリーンショット](xaml-controls-images/IndicatorView.png "IndicatorView")</p>[ガイド](~/xamarin-forms/user-interface/indicatorview.md) | <p valign="center"><pre>&lt;IndicatorView x:Name="indicatorView"<br />               IndicatorColor="LightGray"<br />               SelectedIndicatorColor="DarkGray" /&gt;</pre></p> |
| <h3>ListView</h3>選択可能なデータ項目のスクロール可能な一覧を表示します。<p align="center">![ListView のスクリーンショット](xaml-controls-images/ListView.png "ListView")</p>[API](xref:Xamarin.Forms.ListView)  / [ガイド](~/xamarin-forms/user-interface/listview/index.md) | <p valign="center"><pre>&lt;ListView ItemsSource="{Binding Monkeys}"&gt;<br />          ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p> |
| <h3>ピッカー</h3>テキスト文字列の一覧から選択項目を表示します。<p align="center">![ピッカーのスクリーンショット](xaml-controls-images/Picker.png "ピッカー")</p>[API](xref:Xamarin.Forms.Picker)  / [ガイド](~/xamarin-forms/user-interface/picker/index.md) | <p valign="center"><pre>&lt;Picker Title="Select a monkey"<br />        TitleColor="Red"&gt;<br />  &lt;Picker.ItemsSource&gt;<br />    &lt;x:Array Type="{x:Type x:String}"&gt;<br />      &lt;x:String&gt;Baboon&lt;/x:String&gt;<br />      &lt;x:String&gt;Capuchin Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Blue Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Squirrel Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Golden Lion Tamarin&lt;/x:String&gt;<br />      &lt;x:String&gt;Howler Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Japanese Macaque&lt;/x:String&gt;<br />    &lt;/x:Array&gt;<br />  &lt;/Picker.ItemsSource&gt;<br />&lt;/Picker&gt;</pre></p> |
| <h3>TableView</h3>対話型の行の一覧を表示します。<p align="center">![TableView のスクリーンショット](xaml-controls-images/TableView.png "TableView")</p>[API](xref:Xamarin.Forms.TableView)  / [ガイド](~/xamarin-forms/user-interface/tableview.md) | <p valign="center"><pre>&lt;TableView Intent="Settings"&gt;<br />    &lt;TableRoot&gt;<br />        &lt;TableSection Title="Ring"&gt;<br />            &lt;SwitchCell Text="New Voice Mail" /&gt;<br />            &lt;SwitchCell Text="New Mail" On="true" /&gt;<br />        &lt;/TableSection&gt;<br />    &lt;/TableRoot&gt;<br />&lt;/TableView&gt;</pre></p> |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Formsフォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
