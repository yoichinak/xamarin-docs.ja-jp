---
title: "WatchOS の概要"
description: "WatchOS ソリューションの構造と制限事項の概要"
ms.topic: article
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: fbf430e16506bdcf89ea3e280d42f27b0406f153
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-watchos"></a>WatchOS の概要

> [!NOTE]
> チェック アウト、 [watchOS 3 の概要](~/ios/watchos/platform/introduction-to-watchos3/index.md)最新の機能の概要についてはします。

## <a name="about-watchos"></a>WatchOS について

WatchOS アプリ ソリューションには、3 つのプロジェクトがあります。

- **拡張機能を見る**– watch アプリのコードを含むプロジェクトです。
- **アプリを見る**– ユーザー インターフェイスのストーリー ボードとリソースが含まれています。
- **iOS アプリの親**– このアプリは通常の iPhone アプリ。 Watch アプリと拡張機能は、ユーザーのウォッチへの配信用 iPhone アプリにバンドルされています。

WatchOS 1 アプリで、iPhone で拡張機能のコードが実行されます: Apple Watch、事実上、外付けディスプレイ。 watchOS 2 および 3 のアプリは、Apple Watch で完全に実行されます。 この違いは、次の図で示されます。

[ ![](intro-to-watchos-images/arch-sml.png "この図に示すは watchOS 1 と watchOS 2 (以降) の違い")](intro-to-watchos-images/arch.png#lightbox)

WatchOS のバージョンを対象とするに関係なく Mac のソリューション パッド用の Visual Studio での完全なソリューションは次のよう。

[![](intro-to-watchos-images/projectstructure-sml.png "ソリューションのパッド")](intro-to-watchos-images/projectstructure.png#lightbox)

*親アプリ*watchOS ソリューションが正規の iOS アプリ。 これは、表示されているソリューションでのみプロジェクト**電話**です。 このアプリのユース ケースは、チュートリアル、管理画面、および中間層のフィルター処理、cacheing などが含まれます。ただし、インストールし、ウォッチ アプリ/拡張機能を実行せずに、ユーザーは**れた**親アプリを開いたのでは親アプリの管理、または 1 回限りの初期化を実行する必要がある場合必要があります、ウォッチのプログラミングユーザーに伝えるためアプリ/拡張機能です。

親アプリは、watch アプリや拡張を配信するが、さまざまなサンド ボックスで実行します。

WatchOS 1 上の共有をデータ、または静的関数による共有アプリ グループで`WKInterfaceController.OpenParentApplication`が発生する、`UIApplicationDelegate.HandleWatchKitExtensionRequest`メソッドに親アプリの`AppDelegate`(を参照してください[親アプリの使用](~/ios/watchos/app-fundamentals/parent-app.md))。

WatchOS 2 またはそれ以降がウォッチ接続フレームワークが親アプリとの通信に使用されるを使用して、`WCSession`クラスです。

## <a name="application-lifecycle"></a>アプリケーションのライフ サイクル

ウォッチ拡張機能のサブクラスで、`WKInterfaceController`各ストーリー ボード シーンのクラスを作成します。

これら`WKInterfaceController`クラスに似ています、 `UIViewController` iOS プログラミング内のオブジェクトが、同じレベルのビューへのアクセスはありません。
たとえば、することはできません動的にコントロールを追加するか、UI を再構築できます。
ただし、非表示にし、コントロールを表示でき、いくつかのコントロールのサイズ、透過性、および表示オプションを変更できます。

ライフ サイクル、`WKInterfaceController`オブジェクトには次の呼び出しが含まれます。

- [起動状態](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.Awake/): この方法では、初期化のほとんどを行う必要があります。
- [WillActivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.WillActivate/) : Watch アプリがユーザーに表示する少し前に呼び出されます。 最後の時点の初期化を実行、アニメーションなどを開始するには、このメソッドを使用します。
- この時点では、Watch アプリが表示され、拡張機能がユーザーの入力し、アプリケーション ロジックあたり Watch アプリの表示を更新する応答を開始します。
- [DidDeactivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.DidDeactivate/)ウォッチ式の後のアプリがユーザーによって閉じられた、このメソッドが呼び出されます。 ユーザー インターフェイス コントロールは、次回まで変更できませんこのメソッドから制御が戻た後`WillActivate`と呼びます。 IPhone への接続が切断された場合、このメソッドは呼び出されます。
- 拡張機能が非アクティブ化されて後、は、プログラムにアクセスできません。 保留中の非同期関数**されません**は呼び出せません。 ウォッチ キットの拡張機能は、バック グラウンド処理モードを使用することはできません。 呼ばれる最初のメソッドがありますが、プログラムがユーザーによって再アクティブ化した場合は、オペレーティング システムによって、アプリが終了していません`WillActivate`です。

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "アプリケーション ライフ サイクルの概要")

## <a name="types-of-user-interface"></a>ユーザー インターフェイスの種類

ユーザーは、watch アプリを持つことができますの相互作用の 3 つの種類があります。
カスタムのサブ クラスを使用するようにプログラミングすべてが`WKInterfaceController`までに説明したライフ サイクルのシーケンスが汎用的に適用されるため、(通知のサブクラスでプログラミング`WKUserNotificationController`、自体のサブクラスは、 `WKInterfaceController`)。

### <a name="normal-interaction"></a>標準の相互作用

サブクラスに watch アプリ/拡張機能の対話処理の大部分がられます`WKInterfaceController`watch アプリのシーンに対応するために作成**Interface.storyboard**です。 これについては詳しく説明、[インストール](~/ios/watchos/get-started/installation.md)と[作業の開始](~/ios/watchos/get-started/index.md)アーティクルです。
次の図の一部を示しています、[ウォッチ キット カタログ](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)サンプルのストーリー ボードです。 ここで示した各シーンの場合は、対応するカスタム`WKInterfaceController`(`LabelDetailController`、 `ButtonDetailController`、`SwitchDetailController`など)、拡張機能プロジェクト内です。

![](intro-to-watchos-images/scenes.png "標準の相互作用の例")

### <a name="notifications"></a>通知

[通知](~/ios/watchos/platform/notifications.md)Apple Watch の主要なユース ケースがします。 ローカルおよびリモートの両方の通知がサポートされます。 通知との対話は、短いと時間の長い外観と呼ばれる、2 つの段階的に発生します。

短い外観が簡単に表示され、ウォッチ アプリのアイコン、その名前とタイトルを表示する (使用して、指定した`WKInterfaceController.SetTitle`)。

システム標準を組み合わせて長い検索**枠**領域と、カスタムのストーリー ボード ベースのコンテンツを持つ Dismiss ボタンをクリックします。

`WKUserNotificationInterfaceController` 拡張`WKInterfaceController`メソッドを使って`DidReceiveLocalNotification`と`DidReceiveRemoteNotification`です。
Notification イベントに対応するためにこれらのメソッドをオーバーライドします。

通知の UI の設計の詳細についてを参照してください、 [Apple Watch ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)

![](intro-to-watchos-images/notifications.png "サンプルの通知")

## <a name="screen-sizes"></a>画面サイズ

Apple Watch が 2 つの面サイズ: 38 mm および 42 mm、両方を 5:4 表示の割合と Retina ディスプレイを使用します。 使用可能なサイズは次のとおりです。

- 38 mm: 136 x 170 論理ピクセル (272 x 340 物理的なピクセル)
- 42 mm: 156 x 195 論理ピクセル (312 x 390 物理的なピクセル) です。

使用して`WKInterfaceDevice.ScreenBounds`をどのディスプレイ Watch アプリが実行されているかを判断します。

一般に、限られた 38 mm 表示で、テキストとレイアウトのデザインを開発し、スケール アップに簡単です。
大規模な環境を起動した場合は、厄介重複またはテキスト切り捨てする可能性がありますスケール ダウンします。

詳細について[画面サイズと作業](~/ios/watchos/app-fundamentals/screen-sizes.md)です。


## <a name="limitations-of-watchos"></a>WatchOS の制限事項

WatchOS アプリを開発するときの注意すべき watchOS のいくつかの制限があります。

- Apple Watch デバイスに記憶域が限られている - (サイズの大きなファイルをダウンロードする前に使用可能な領域を認識します。 オーディオまたはビデオ ファイル)。

- 多くの watchOS[コントロール](~/ios/watchos/user-interface/index.md)UIKit に類似のものがあるが、さまざまなクラスは、(`WKInterfaceButton`なく`UIButton`、`WKInterfaceSwitch`の`UISwitch`など) いて、限られたと比較してその UIKit メソッド対応します。 さらに、一部のコントロールには watchOS など`WKInterfaceDate`(日付と時刻を表示する) その UIKit 必要はありません。

  - のみ、ウォッチへの通知または (ルーティング経由でのユーザーの制御の種類は発表されていない Apple によって)、iPhone のみをルーティングすることはできません。

その他のいくつかの既知の制限/よく寄せられる質問。

- Apple では、サード パーティのカスタム ウォッチ面は許可しません。

- 接続されている電話で iTunes を制御するウォッチができるようにする Api は、プライベートです。


## <a name="further-reading"></a>関連項目

Apple のドキュメントをご覧ください。

* [ウォッチ キット用の開発](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

* [キット プログラミング ガイドを見る](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

* [Apple Watch ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)


## <a name="related-links"></a>関連リンク

- [watchOS 3 カタログ (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [watchOS 1 カタログ (サンプル)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [セットアップとインストール](~/ios/watchos/get-started/installation.md)
- [最初の Watch アプリ ビデオ](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple は、Watch キット ガイド用の開発](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Apple の WatchKit ヒント](https://developer.apple.com/watchkit/tips/)
- [watchOS 3 の概要](~/ios/watchos/platform/introduction-to-watchos3/index.md)
