---
title: watchOS の概要
description: このドキュメントでは、アプリケーションのライフサイクル、ユーザーインターフェイスの種類、画面のサイズ、制限などを説明する watchOS の概要について説明します。
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 1b71ff60ea0e23ce9d631286aec624a84f163ce5
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937505"
---
# <a name="introduction-to-watchos"></a>watchOS の概要

> [!NOTE]
> 最新の機能の概要については、 [watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)の概要に関するトピックをご覧ください。

## <a name="about-watchos"></a>WatchOS について

WatchOS app ソリューションには、次の3つのプロジェクトがあります。

- **Watch Extension** – watch アプリのコードを含むプロジェクトです。
- **Watch アプリ**–ユーザーインターフェイスのストーリーボードとリソースが含まれています。
- **iOS の親アプリ**-このアプリは、通常の iPhone アプリです。 Watch アプリと拡張機能は、ユーザーの監視に配信するために iPhone アプリにバンドルされています。

WatchOS 1 アプリでは、拡張機能のコードは iPhone で実行されます。この Apple Watch は、事実上外部表示です。 watchOS 2 と3のアプリは、Apple Watch で完全に実行されます。 この違いを次の図に示します。

[![次の図は、watchOS 1 と watchOS 2 (およびそれ以降) の違いを示しています。](intro-to-watchos-images/arch-sml.png)](intro-to-watchos-images/arch.png#lightbox)

対象となる watchOS のバージョンに関係なく、Visual Studio for Mac の Solution Pad では、完全なソリューションは次のようになります。

[![Solution Pad](intro-to-watchos-images/projectstructure-sml.png)](intro-to-watchos-images/projectstructure.png#lightbox)

WatchOS ソリューションの*親アプリ*は、通常の iOS アプリです。 これは、**電話に**表示されるソリューション内の唯一のプロジェクトです。 このアプリのユースケースには、チュートリアル、管理画面、中間層のフィルター処理、キャッシュなどが含まれます。ただし、ユーザーは親アプリを開いたまま、watch アプリ/拡張機能をインストールして実行**することが**できます。そのため、1回限りの初期化または管理のために親アプリを実行する必要がある場合は、watch アプリ/拡張機能をプログラムして、ユーザーに通知する必要があります。

親アプリは watch アプリと拡張機能を提供しますが、さまざまなサンドボックスで実行されます。

WatchOS 1 では、共有アプリグループまたは静的関数を介してデータを共有できます `WKInterfaceController.OpenParentApplication` 。これにより、 `UIApplicationDelegate.HandleWatchKitExtensionRequest` 親アプリのメソッドがトリガーされ `AppDelegate` ます (「[親アプリの操作](~/ios/watchos/app-fundamentals/parent-app.md)」を参照してください)。

WatchOS 2 以降では、クラスを使用して、監視接続フレームワークを使用して親アプリと通信し `WCSession` ます。

## <a name="application-lifecycle"></a>アプリケーションのライフ サイクル

Watch 拡張機能で `WKInterfaceController` は、各ストーリーボードシーンに対してクラスのサブクラスが作成されます。

これらの `WKInterfaceController` クラスは、 `UIViewController` iOS プログラミングのオブジェクトに似ていますが、ビューへのアクセスレベルは同じではありません。
たとえば、UI に対してコントロールを動的に追加したり、UI を再構築したりすることはできません。
ただし、コントロールの表示と非表示を切り替えたり、コントロールのサイズ、透明度、外観のオプションを変更したりできます。

オブジェクトのライフサイクルには `WKInterfaceController` 、次の呼び出しが含まれます。

- 起動中: このメソッドでは、ほとんどの初期化を実行する必要が[あります。](xref:WatchKit.WKInterfaceController.Awake*)
- が[activate](xref:WatchKit.WKInterfaceController.WillActivate) : Watch アプリがユーザーに表示される直前に呼び出されます。 このメソッドは、最後の初期化、アニメーションの開始などを実行するために使用します。
- この時点で、ウォッチアプリが表示され、拡張機能はユーザー入力に応答し、アプリケーションロジックごとに Watch アプリの表示を更新します。
- [DidDeactivate](xref:WatchKit.WKInterfaceController.DidDeactivate)ウォッチアプリがユーザーによって破棄されると、このメソッドが呼び出されます。 このメソッドから制御が戻った後、次のが呼び出されるまでユーザーインターフェイスコントロールを変更することはできません `WillActivate` 。 このメソッドは、iPhone への接続が切断された場合にも呼び出されます。
- 拡張機能が非アクティブになると、プログラムからアクセスできなくなります。 保留中の非同期関数は呼び出され**ません**。 Watch Kit の拡張機能では、バックグラウンド処理モードを使用できません。 プログラムがユーザーによって再アクティブ化されても、アプリがオペレーティングシステムによって終了されていない場合、最初に呼び出されるメソッドはになり `WillActivate` ます。

![アプリケーションのライフサイクルの概要](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png)

## <a name="types-of-user-interface"></a>ユーザーインターフェイスの種類

Watch アプリでは、ユーザーが持つことができる相互作用には3種類あります。
すべてのはのカスタムサブクラスを使用してプログラミングされている `WKInterfaceController` ため、前に説明したライフサイクルシーケンスは汎用的に適用されます (これ自体がのサブクラスを使用して、通知がプログラミングされ `WKUserNotificationController` `WKInterfaceController` ます)。

### <a name="normal-interaction"></a>通常の対話

Watch アプリと拡張機能の対話の大半は、 `WKInterfaceController` ウォッチアプリの**インターフェイス**のシーンに対応するために記述するのサブクラスを使用します。 詳細については、の[インストール](~/ios/watchos/get-started/installation.md)と[はじめに](~/ios/watchos/get-started/index.md)に関する記事をご覧ください。
次の図は、 [Watch Kit カタログ](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルのストーリーボードの一部を示しています。 ここに示されているシーンごとに、 `WKInterfaceController` `LabelDetailController` `ButtonDetailController` `SwitchDetailController` 拡張プロジェクトに対応するカスタム (、、など) があります。

![通常の相互作用の例](intro-to-watchos-images/scenes.png)

### <a name="notifications"></a>通知

[通知](~/ios/watchos/platform/notifications.md)は、Apple Watch の主要なユースケースです。 ローカル通知とリモート通知の両方がサポートされています。 通知との相互作用は、短い形式と長い形式の2つの段階で行われます。

短い外観が表示され、[ウォッチ] アプリアイコン、その名前、およびタイトル (で指定) が表示され `WKInterfaceController.SetTitle` ます。

長い外観では、システムに用意されている**サッシ**領域と、カスタムのストーリーボードベースのコンテンツを組み合わせたボタンが表示されます。

`WKUserNotificationInterfaceController`は、およびのメソッドを使用して拡張さ `WKInterfaceController` `DidReceiveLocalNotification` `DidReceiveRemoteNotification` れます。
これらのメソッドをオーバーライドして、通知イベントに応答します。

通知 UI のデザインの詳細については、 [Apple Watch ヒューマンインターフェイスのガイドライン](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)を参照してください。

![サンプル通知](intro-to-watchos-images/notifications.png)

## <a name="screen-sizes"></a>画面サイズ

Apple Watch には、2つの面のサイズがあります。38mm と 42 mm で、5:4 の表示比率と Retina 表示。 有効なサイズは次のとおりです。

- 38mm: 136 x 170 論理ピクセル (272 x 340 物理ピクセル)
- 42 mm: 156 x 195 論理ピクセル (312 x 390 物理ピクセル)。

を使用 `WKInterfaceDevice.ScreenBounds` して、ウォッチアプリが実行されている表示を決定します。

一般に、より制約された38mm ディスプレイを使用してテキストとレイアウトのデザインを簡単に開発し、スケールアップすることができます。
大規模な環境から始めると、スケールダウンによって、見づらい重複やテキストの切り捨てが生じる可能性があります。

詳細については[、画面サイズの操作に関するページを](~/ios/watchos/app-fundamentals/screen-sizes.md)参照してください。

## <a name="limitations-of-watchos"></a>WatchOS の制限事項

WatchOS アプリの開発時に注意すべき watchOS の制限がいくつかあります。

- Apple Watch デバイスのストレージが制限されている-大きなファイルをダウンロードする前に、使用可能な領域に注意する必要があります (例: オーディオまたはムービーファイル)。

- 多くの watchOS[コントロール](~/ios/watchos/user-interface/index.md)には、uikit にアナログがありますが、(などではなく) 異なるクラスがあり、それ `WKInterfaceButton` `UIButton` `WKInterfaceSwitch` `UISwitch` に相当する uikit と比較して、限られた一連のメソッドが用意されています。 また、watchOS には、 `WKInterfaceDate` UIKit に含まれていないコントロール (日付と時刻を表示する場合) などがあります。

  - 監視のみに通知をルーティングすることはできません (ユーザーがルーティングを介して持っているコントロールの種類は Apple によって発表されていません)。

その他の既知の制限事項とよく寄せられる質問:

- Apple では、サードパーティのカスタムウォッチの顔は許可されません。

- 接続された電話の iTunes を監視するための Api はプライベートです。

## <a name="further-reading"></a>もっと読む

Apple のドキュメントを確認してください。

- [Watch Kit 用の開発](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

- [Watch Kit のプログラミングガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

- [Apple Watch ヒューマンインターフェイスのガイドライン](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)

## <a name="related-links"></a>関連リンク

- [watchOS 3 カタログ (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [watchOS 1 カタログ (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [セットアップとインストール](~/ios/watchos/get-started/installation.md)
- [最初のアプリの視聴に関するビデオ](https://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple の開発用 Watch キットガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Apple の WatchKit のヒント](https://developer.apple.com/watchkit/tips/)
- [watchOS 3 の概要](~/ios/watchos/platform/introduction-to-watchos3/index.md)
