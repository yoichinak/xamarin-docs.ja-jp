---
title: watchOS の概要
description: このドキュメントでは、アプリケーションのライフサイクル、ユーザーインターフェイスの種類、画面のサイズ、制限などを説明する watchOS の概要について説明します。
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: b3c2908d8ae9a68189fbff4d47afa49da21b88a5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030185"
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

[![](intro-to-watchos-images/arch-sml.png "The difference between watchOS 1 and watchOS 2 (and greater) is shown in this diagram")](intro-to-watchos-images/arch.png#lightbox)

対象となる watchOS のバージョンに関係なく、Visual Studio for Mac の Solution Pad では、完全なソリューションは次のようになります。

[![](intro-to-watchos-images/projectstructure-sml.png "The Solution Pad")](intro-to-watchos-images/projectstructure.png#lightbox)

WatchOS ソリューションの*親アプリ*は、通常の iOS アプリです。 これは、**電話に**表示されるソリューション内の唯一のプロジェクトです。 このアプリのユースケースには、チュートリアル、管理画面、中間層のフィルター処理、キャッシュなどが含まれます。ただし、ユーザーは親アプリを開いたまま、watch アプリ/拡張機能をインストールして実行**することが**できます。そのため、1回限りの初期化または管理のために親アプリを実行する必要がある場合は、watch アプリ/拡張機能をプログラミングして、ユーザー。

親アプリは watch アプリと拡張機能を提供しますが、さまざまなサンドボックスで実行されます。

WatchOS 1 では、共有アプリグループまたは静的関数 `WKInterfaceController.OpenParentApplication`を介してデータを共有できます。これにより、親アプリの `AppDelegate` の `UIApplicationDelegate.HandleWatchKitExtensionRequest` メソッドがトリガーされます (「[親アプリの操作](~/ios/watchos/app-fundamentals/parent-app.md)」を参照してください)。

WatchOS 2 以降では、監視接続フレームワークを使用して、親アプリとの通信に `WCSession` クラスを使用します。

## <a name="application-lifecycle"></a>アプリケーションのライフ サイクル

Watch 拡張機能では、ストーリーボードシーンごとに `WKInterfaceController` クラスのサブクラスが作成されます。

これらの `WKInterfaceController` クラスは、iOS プログラミングの `UIViewController` オブジェクトに似ていますが、ビューへのアクセスレベルは同じではありません。
たとえば、UI に対してコントロールを動的に追加したり、UI を再構築したりすることはできません。
ただし、コントロールの表示と非表示を切り替えたり、コントロールのサイズ、透明度、外観のオプションを変更したりできます。

`WKInterfaceController` オブジェクトのライフサイクルには、次の呼び出しが含まれます。

- 起動中: このメソッドでは、ほとんどの初期化を実行する必要が[あります。](xref:WatchKit.WKInterfaceController.Awake*)
- が[activate](xref:WatchKit.WKInterfaceController.WillActivate) : Watch アプリがユーザーに表示される直前に呼び出されます。 このメソッドは、最後の初期化、アニメーションの開始などを実行するために使用します。
- この時点で、ウォッチアプリが表示され、拡張機能はユーザー入力に応答し、アプリケーションロジックごとに Watch アプリの表示を更新します。
- [DidDeactivate](xref:WatchKit.WKInterfaceController.DidDeactivate)ウォッチアプリがユーザーによって破棄されると、このメソッドが呼び出されます。 このメソッドから制御が戻った後、次に `WillActivate` が呼び出されるまで、ユーザーインターフェイスコントロールを変更することはできません。 このメソッドは、iPhone への接続が切断された場合にも呼び出されます。
- 拡張機能が非アクティブになると、プログラムからアクセスできなくなります。 保留中の非同期関数は呼び出され**ません**。 Watch Kit の拡張機能では、バックグラウンド処理モードを使用できません。 プログラムがユーザーによって再アクティブ化されても、アプリがオペレーティングシステムによって終了されていない場合、最初に呼び出されたメソッドが `WillActivate`されます。

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "Application Lifecycle overview")

## <a name="types-of-user-interface"></a>ユーザーインターフェイスの種類

Watch アプリでは、ユーザーが持つことができる相互作用には3種類あります。
すべては `WKInterfaceController`のカスタムサブクラスを使用してプログラミングされているため、前に説明したライフサイクルシーケンスは、汎用的に適用されます (通知は `WKUserNotificationController`のサブクラスを使用してプログラミングされ、それ自体が `WKInterfaceController`のサブクラスになります)。

### <a name="normal-interaction"></a>通常の対話

Watch アプリと拡張機能の対話の大部分は、watch アプリの**インターフェイス**のシーンに対応するために記述する `WKInterfaceController` のサブクラスを使用して行われます。 詳細については、の[インストール](~/ios/watchos/get-started/installation.md)と[はじめに](~/ios/watchos/get-started/index.md)に関する記事をご覧ください。
次の図は、 [Watch Kit カタログ](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルのストーリーボードの一部を示しています。 ここに示されているシーンごとに、拡張機能プロジェクトに対応するカスタム `WKInterfaceController` (`LabelDetailController`、`ButtonDetailController`、`SwitchDetailController`など) があります。

![](intro-to-watchos-images/scenes.png "Normal Interaction examples")

### <a name="notifications"></a>通知

[通知](~/ios/watchos/platform/notifications.md)は、Apple Watch の主要なユースケースです。 ローカル通知とリモート通知の両方がサポートされています。 通知との相互作用は、短い形式と長い形式の2つの段階で行われます。

短い外観が表示され、[ウォッチ] アプリのアイコン、名前、および (`WKInterfaceController.SetTitle`で指定されている) タイトルが表示されます。

長い外観では、システムに用意されている**サッシ**領域と、カスタムのストーリーボードベースのコンテンツを組み合わせたボタンが表示されます。

`WKUserNotificationInterfaceController` は、`DidReceiveLocalNotification` および `DidReceiveRemoteNotification`メソッドを使用して `WKInterfaceController` を拡張します。
これらのメソッドをオーバーライドして、通知イベントに応答します。

通知 UI のデザインの詳細については、 [Apple Watch ヒューマンインターフェイスのガイドライン](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)を参照してください。

![](intro-to-watchos-images/notifications.png "Sample notifications")

## <a name="screen-sizes"></a>画面サイズ

Apple Watch には、2つの面のサイズがあります。38mm と 42 mm で、5:4 の表示比率と Retina 表示。 有効なサイズは次のとおりです。

- 38mm: 136 x 170 論理ピクセル (272 x 340 物理ピクセル)
- 42 mm: 156 x 195 論理ピクセル (312 x 390 物理ピクセル)。

`WKInterfaceDevice.ScreenBounds` を使用して、Watch アプリが実行されている表示を決定します。

一般に、より制約された38mm ディスプレイを使用してテキストとレイアウトのデザインを簡単に開発し、スケールアップすることができます。
大規模な環境から始めると、スケールダウンによって、見づらい重複やテキストの切り捨てが生じる可能性があります。

詳細については[、画面サイズの操作に関するページを](~/ios/watchos/app-fundamentals/screen-sizes.md)参照してください。

## <a name="limitations-of-watchos"></a>WatchOS の制限事項

WatchOS アプリの開発時に注意すべき watchOS の制限がいくつかあります。

- Apple Watch デバイスのストレージが制限されている-大きなファイルをダウンロードする前に、使用可能な領域に注意する必要があります (例: オーディオまたはムービーファイル)。

- 多くの watchOS[コントロール](~/ios/watchos/user-interface/index.md)には、uikit にアナログがありますが、異なるクラス (`UIButton``WKInterfaceSwitch`、`UISwitch`などではなく`WKInterfaceButton`) と、それらに対応する uikit と比較したメソッドの限られたセットがあります。 また、watchOS には、UIKit に含まれていない `WKInterfaceDate` (日付と時刻の表示用) などのコントロールがいくつかあります。

  - 監視のみに通知をルーティングすることはできません (ユーザーがルーティングを介して持っているコントロールの種類は Apple によって発表されていません)。

その他の既知の制限事項とよく寄せられる質問:

- Apple では、サードパーティのカスタムウォッチの顔は許可されません。

- 接続された電話の iTunes を監視するための Api はプライベートです。

## <a name="further-reading"></a>関連項目

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
