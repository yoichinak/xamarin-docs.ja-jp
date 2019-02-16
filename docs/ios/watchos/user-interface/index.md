---
title: ユーザー インターフェイス コントロールは、Xamarin で watchOS
description: このドキュメントでは、watchOS ユーザー インターフェイスで使用するために使用できるさまざまなコントロールについて説明します。 これは、ラベル、ボタン、スイッチ、スライダー、イメージ、区切り記号、マップ、および詳細の説明を提供します。
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/19/2016
ms.openlocfilehash: c8ef76f24b017f5e3e6bec9d39534f3626e79147
ms.sourcegitcommit: 2713f2c1d74e3582704c3d0ca65b6651119ed489
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/15/2019
ms.locfileid: "56321104"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>ユーザー インターフェイス コントロールは、Xamarin で watchOS

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) watchOS のさまざまなコントロールを示します。 アプリのストーリー ボードは、次に示します (クリックすると拡大します)。

[![](images/storyboard-sml.png "サンプルの watchOS レイアウト")](images/storyboard.png#lightbox)

すべてのコントロールのプログラミング可能な名前が付いた`WKInterface`(例。 `WKInterfaceLabel`, `WKInterfaceButton`).

|コントロール|説明|スクリーン ショット|
|---|---|---|
|group1|使用`SetText`とラベル コントロールのテキストの外観を制御するには、その他のプロパティ。 `NSAttributedString` サポートされます。<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|ボタン|作成し、ストーリー ボードのプロパティを設定します。 Ctrl キーを押しながらドラッグして、追加、`Action`がクリックされたときのハンドラーを実装します。<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|切り替え|使用`SetOn`スイッチの状態を制御します。<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|スライダー|さまざまなスタイルがあります。<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|イメージ|使用して、 `myImage.SetImage("MyWatchImage")` 、watch でのイメージを読み込むまたは`WKInterfaceDevice.CurrentDevice.AddCachedImage`watch で再利用するためにキャッシュします。<br />[イメージ コントロールのドキュメント](~/ios/watchos/user-interface/image.md)<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|区切り記号|区切り記号を使用して、魅力的なウォッチ Ui を作成するのに役立ちます。<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|マップ|マップ画像が watch で静的に表示されますが、外観、ピンを追加するなどの多くの側面を制御できます。<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|ムービー & InlineMove|映画では、いずれかのことができます自身で、オープンまたはインライン<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|グループ化|グループを使用すると、魅力的なウォッチ Ui を作成するのに役立ちます。<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|テーブル|IOS 上のテーブルの簡略化されたバージョン。 実装`DidSelectRow`ユーザー選択への対処 (または、セグエを使用)。<br />[テーブル コントロールのドキュメント](~/ios/watchos/user-interface/table.md)<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|デバイス|`WKInterfaceDevice.CurrentDevice` などのプロパティが含まれている`ScreenBounds`、 `ScreenScale`、および`PreferredContentSizeCategory`します。<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menu](~/ios/watchos/user-interface/menu.md)|ストーリー ボードで force のキーを押してメニューを定義し、コードの各ボタンの操作を実装します。<br />[メニュー (Force Touch) コントロールのドキュメント](~/ios/watchos/user-interface/menu.md)<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|テキスト入力|使用`PresentTextInputController`と`WKTextInputMode`列挙体。<br />[テキスト入力のドキュメント](~/ios/watchos/user-interface/text-input.md)<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|デジタル クラウン|デジタル クラウンは、ピッカーでは、ドライブに使用できるまたはコードでの回転を追跡することができます。<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|ジェスチャ|シーンに追加できるジェスチャ認識の 4 つの種類があります。Tap、スワイプ、パン、および LongPress します。<br />[コードをカタログします。](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [キットの API リファレンスをご覧ください。](xref:WatchKit)
