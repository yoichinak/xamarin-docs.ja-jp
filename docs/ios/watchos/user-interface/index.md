---
title: Xamarin の watchOS ユーザーインターフェイスコントロール
description: このドキュメントでは、watchOS ユーザーインターフェイスで使用できるさまざまなコントロールについて説明します。 ラベル、ボタン、スイッチ、スライダー、画像、区切り記号、マップなどの説明が表示されます。
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/19/2016
ms.openlocfilehash: b8f33be80851815f00b0460a8e31f01733c425e4
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939720"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>Xamarin の watchOS ユーザーインターフェイスコントロール

[**WatchKitCatalog**](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog)サンプルでは、さまざまな watchOS コントロールを示します。 アプリのストーリーボードがここに表示されます (クリックしてズームします):

[![WatchOS レイアウトのサンプル](images/storyboard-sml.png)](images/storyboard.png#lightbox)

すべてのコントロールのプログラム名にプレフィックスが付き `WKInterface` ます (例として、 `WKInterfaceLabel`, `WKInterfaceButton`).

|Control|説明|Screenshot|
|---|---|---|
|ラベル|`SetText`およびその他のプロパティを使用して、ラベルコントロールのテキストの外観を制御します。 `NSAttributedString`もサポートされています。<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|ボタン|ストーリーボードでプロパティを作成および設定します。 Ctrl キーを押しながらドラッグしてを追加し、を `Action` クリックしたときのハンドラーを実装します。<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Switch|`SetOn`スイッチの状態を制御するには、を使用します。<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|スライダー|さまざまなスタイルを使用できます。<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|イメージ|を使用して、 `myImage.SetImage("MyWatchImage")` ウォッチに画像を読み込んだり、 `WKInterfaceDevice.CurrentDevice.AddCachedImage` ウォッチで使用するためにそれらをキャッシュしたりします。<br />[イメージコントロールのドキュメント](~/ios/watchos/user-interface/image.md)<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|区切り記号|記号を使用すると、魅力的なウォッチ Ui を作成できます。<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|マップ|マップ画像は、モニターに静的に表示されますが、ピンの追加など、外観のさまざまな側面を制御できます。<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|ムービー & の無効化|ムービーは自分で開くことも、インラインで開くこともできます。<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|グループ|グループを使用すると、魅力的なウォッチ Ui を作成できます。<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|テーブル|IOS 上のテーブルの簡略化されたバージョン。 `DidSelectRow`ユーザー選択に応答するために実装します (またはセグエを使用します)。<br />[テーブルコントロールのドキュメント](~/ios/watchos/user-interface/table.md)<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Device|`WKInterfaceDevice.CurrentDevice`、、などのプロパティが含まれ `ScreenBounds` `ScreenScale` `PreferredContentSizeCategory` ます。<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menu](~/ios/watchos/user-interface/menu.md)|ストーリーボードで強制押しメニューを定義し、コード内の各ボタンのアクションを実装します。<br />[メニューコントロール (Force Touch) のドキュメント](~/ios/watchos/user-interface/menu.md)<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|テキスト入力|`PresentTextInputController`と列挙体を使用し `WKTextInputMode` ます。<br />[テキスト入力のドキュメント](~/ios/watchos/user-interface/text-input.md)<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Digital Crown|Digital Crown は、ピッカーを駆動するために使用できます。また、コード内でローテーションを追跡することもできます。<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|ジェスチャ|シーンには、タップ、スワイプ、パン、LongPress の4種類のジェスチャ認識を追加できます。<br />[カタログコード](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|

## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Watch Kit API リファレンス](xref:WatchKit)
