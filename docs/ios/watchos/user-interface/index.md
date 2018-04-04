---
title: watchOS ユーザー インターフェイス
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: 73099768d876cad08571c3d0bf8340535eb1307b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="watchos-user-interface"></a>watchOS ユーザー インターフェイス

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog)サンプルでは、さまざまな watchOS コントロール。 アプリのストーリー ボードが表示されます (を拡大表示をクリックします)。

[![](images/storyboard-sml.png "サンプル watchOS レイアウト")](images/storyboard.png#lightbox)

すべてのコントロールのプログラミング可能な名前が付いて`WKInterface`などです。 `WKInterfaceLabel`, `WKInterfaceButton`).

|コントロール|説明|スクリーン ショット|
|---|---|---|
|group1|使用して`SetText`およびラベル コントロール内のテキストの外観を制御するには、他のプロパティです。 `NSAttributedString` サポートされます。<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|ボタン|作成し、ストーリー ボードのプロパティを設定します。 Ctrl キーを押しながらドラッグして、追加、`Action`がクリックされたときのハンドラーを実装します。<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|切り替え|使用して`SetOn`スイッチの状態を制御します。<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|スライダー|さまざまなスタイルが可能です。<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|イメージ|使用して`myImage.SetImage("MyWatchImage")`、ウォッチ上のイメージを読み込むまたは`WKInterfaceDevice.CurrentDevice.AddCachedImage`watch で繰り返し使用してキャッシュします。<br />[イメージ コントロールのドキュメント](~/ios/watchos/user-interface/image.md)<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|区切り記号|ウォッチ式の魅力的な Ui を作成するために区切り記号を使用します。<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|マップ|Watch でマップの画像が表示される静的には、外観、pin を追加するなどの多くの側面を制御することができます。<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|ムービー & InlineMove|ビデオでは、いずれかのことができますが、自分で開く] または [インライン<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|グループ化|ウォッチ式の魅力的な Ui を作成するためにグループを使用します。<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|テーブル|IOS でのテーブルの簡略化されたバージョンです。 実装`DidSelectRow`をユーザーの選択に応答 (または、segue を使用)。<br />[テーブル コントロールのドキュメント](~/ios/watchos/user-interface/table.md)<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|デバイス|`WKInterfaceDevice.CurrentDevice` プロパティをなどが含まれる`ScreenBounds`、 `ScreenScale`、および`PreferredContentSizeCategory`です。<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menu](~/ios/watchos/user-interface/menu.md)|ストーリー ボードで force キーを押してメニューを定義し、コード内の各ボタンのアクションを実装します。<br />[メニュー コントロール (Force タッチ) ドキュメント](~/ios/watchos/user-interface/menu.md)<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|テキスト入力|使用して`PresentTextInputController`と`WKTextInputMode`列挙します。<br />[テキスト入力ドキュメント](~/ios/watchos/user-interface/text-input.md)<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|デジタル クラウン|ピッカーをドライブに使用できるデジタル クラウンまたはコードでの回転を追跡できます。<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|ジェスチャ|シーンに追加できるジェスチャ認識の 4 つの種類があります。 Tap、スワイプして、パン、および LongPress です。<br />[カタログのコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [ウォッチ キット API リファレンス](https://developer.xamarin.com/api/namespace/WatchKit/)
