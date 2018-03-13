---
title: "watchOS ユーザー インターフェイス"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: 0717b8484c6094bb1d9589c44df37745d9e21900
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="watchos-user-interface"></a>watchOS ユーザー インターフェイス

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog)サンプルでは、さまざまな watchOS コントロール。 アプリのストーリー ボードが表示されます (を拡大表示をクリックします)。

[![](images/storyboard-sml.png "サンプル watchOS レイアウト")](images/storyboard.png#lightbox)

すべてのコントロールのプログラミング可能な名前が付いて`WKInterface`などです。 `WKInterfaceLabel`, `WKInterfaceButton`).


<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
        <strong>コントロール</strong>
      </th>
      <th>
        <strong>説明</strong>
      </th>
      <th>
        <strong>スクリーン ショット</strong>
      </th>
    </thead>
    <tbody>
    <tr>
      <td valign="top">
group1 </td>
      <td valign="top">
使用して<code>SetText</code>およびラベル コントロール内のテキストの外観を制御するには、他のプロパティです。 <code>NSAttributedString</code> サポートされます。
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/label.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
ボタン </td>
      <td valign="top">
作成し、ストーリー ボードのプロパティを設定します。 <kbd>Ctrl キーを押しながらドラッグ</kbd>を追加する、<code>Action</code>がクリックされたときのハンドラーを実装します。
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/button.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
切り替え </td>
      <td valign="top">
使用して<code>SetOn</code>スイッチの状態を制御します。
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/switch.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
スライダー </td>
      <td valign="top">
さまざまなスタイルが可能です。
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/slider.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
イメージ </td>
      <td valign="top">
使用して<code>myImage.SetImage("MyWatchImage")</code>、ウォッチ上のイメージを読み込むまたは<code>WKInterfaceDevice.CurrentDevice.AddCachedImage</code>watch で繰り返し使用してキャッシュします。
        <br />
        <a href="~/ios/watchos/user-interface/image.md">イメージ コントロールのドキュメント</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/image.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
区切り記号 </td>
      <td valign="top">
ウォッチ式の魅力的な Ui を作成するために区切り記号を使用します。
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/separator.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
マップ </td>
      <td valign="top">
Watch でマップの画像が表示される静的には、外観、pin を追加するなどの多くの側面を制御することができます。
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/map.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
ムービー & InlineMove </td>
      <td valign="top">
ビデオでは、いずれかのことができますが、自分で開く] または [インライン <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/movie.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
グループ化 </td>
      <td valign="top">
ウォッチ式の魅力的な Ui を作成するためにグループを使用します。
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/group.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
テーブル </td>
      <td valign="top">
IOS でのテーブルの簡略化されたバージョンです。
実装<code>DidSelectRow</code>をユーザーの選択に応答 (または、segue を使用)。
        <br />
        <a href="~/ios/watchos/user-interface/table.md">テーブル コントロールのドキュメント</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TableDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/table.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
デバイス </td>
      <td valign="top">
        <code>WKInterfaceDevice.CurrentDevice</code> プロパティをなどが含まれる<code>ScreenBounds</code>、 <code>ScreenScale</code>、および<code>PreferredContentSizeCategory</code>です。
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/device.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="~/ios/watchos/user-interface/menu.md">メニュー</a>
      </td>
      <td valign="top">
ストーリー ボードで force キーを押してメニューを定義し、コード内の各ボタンのアクションを実装します。
        <br />
        <a href="~/ios/watchos/user-interface/menu.md">メニュー コントロール (Force タッチ) ドキュメント</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/controller.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
テキスト入力 </td>
      <td valign="top">
使用して<code>PresentTextInputController</code>と<code>WKTextInputMode</code>列挙します。
        <br />
        <a href="~/ios/watchos/user-interface/text-input.md">テキスト入力ドキュメント</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/textinput.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
デジタル クラウン </td>
      <td valign="top">
ピッカーをドライブに使用できるデジタル クラウンまたはコードでの回転を追跡できます。
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/digital-crown.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
ジェスチャ </td>
      <td valign="top">
シーンに追加できるジェスチャ認識の 4 つの種類があります。 Tap、スワイプして、パン、および LongPress です。
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs">カタログのコード</a>
      </td>
      <td>
        <img src="Images/gestures.png" class="tableimg">
      </td>
    </tr>
    </tbody>
</table>



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [ウォッチ キット API リファレンス](https://developer.xamarin.com/api/namespace/WatchKit/)
