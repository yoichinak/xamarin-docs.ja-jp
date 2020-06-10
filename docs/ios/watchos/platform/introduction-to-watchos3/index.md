---
title: watchOS 3 の概要
description: この記事では、Xamarin 開発者向けの watchOS 3 で利用できる、新しい Api と変更された Api と機能について説明します。
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/07/2017
ms.openlocfilehash: abe9c95c889aed5258ea3a5367e05368ddb7c77f
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574172"
---
# <a name="introduction-to-watchos-3"></a>watchOS 3 の概要

_この記事では、Xamarin 開発者向けの watchOS 3 で利用できる、新しい Api と変更された Api と機能について説明します。_

このドキュメントでは、次のトピックについて説明します。

- [WatchOS 3 の新機能](#Whats-New-in-watchOS-3)
  - [Apple Pay の拡張機能](#Apple-Pay-Enhancements)により、Apple Watch でのアプリ内支払いのサポートが追加されます。
  - [バックグラウンドタスク](#Background-Tasks)は、ユーザーが必要とするときに、バックグラウンドで情報を更新する機能をアプリに提供します。
  - WatchOS 3 については、アプリの新機能を提供する複雑な機能[強化](#Complications-Enhancements)が行われています。
  - [新しく利用できるフレームワーク](#Newly-Available-Frameworks)は、watchOS アプリ向けに公開されています。
  - [プロアクティブな提案](#Proactive-Suggestions)により、アプリはユーザーに情報を事前に表示できます。
  - WatchOS 3 には[、セキュリティとプライバシー](#Security-and-Privacy-Enhancements)に関するいくつかの機能強化が行われています。
  - [スナップショットとドッキング](#Snapshots-and-Dock)により、ユーザーはアプリ watchOS アプリにすばやくアクセスできます。
  - [ユーザー通知](#User-Notifications)は、ローカル通知とリモート通知の両方をユーザーに提供します。
  - WatchOS 3 では、いくつかの[監視接続フレームワークの機能強化](#Watch-Connectivity-Framework-Enhancements)が行われています。
  - WatchOS 3 では、いくつかの[WatchKit Framework の機能強化](#WatchKit-Framework-Enhancements)が行われています。
  - [トレーニングアプリの機能強化](#Workout-App-Enhancements)により、トレーニング関連の Apple Watch アプリに新しい機能が追加されました。
- WatchOS 3 では、[追加のフレームワークの変更](#Additional-Framework-Changes)が行われています。
- WatchOS 3 の[非推奨の api](#Deprecated-APIs) 。

<a name="Whats-New-in-watchOS-3"></a>

## <a name="whats-new-in-watchos-3"></a>WatchOS 3 の新機能

Apple では、watchOS 3 に新しい Api とサービスがいくつか追加され、既存の機能に対する多くの機能強化が加えられています。

<a name="Apple-Pay-Enhancements"></a>

## <a name="apple-pay-enhancements"></a>Apple Pay の機能強化

WatchOS 3 では、Pass Kit フレームワークが拡張され、Apple Watch で実行されているアプリのセキュリティで保護されたアプリ内支払い (物理的な商品とサービスの両方) をサポートできるようになりました。

新しい[PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller)クラスと[PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)クラスを使用して、ユーザーが支払い要求を承認できるインターフェイスを表示し、応答します。

詳細については、Apple Pay の[拡張機能](~/ios/watchos/platform/apple-pay.md)に関するガイドを参照してください。

<a name="Background-Tasks"></a>

## <a name="background-tasks"></a>バックグラウンド タスク

watchOS 3 では、アプリで情報を更新するために使用できるバックグラウンドタスクがいくつか導入されています。このタスクを開く前に、ユーザーが必要とするコンテンツがあることを確認します。

次の新しいバックグラウンドタスクを使用できます。

- **バックグラウンドアプリの更新**- [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask)タスクを使用すると、アプリの状態をバックグラウンドで更新できます。 通常、これには、 [Nq&a Lsession](https://developer.apple.com/reference/foundation/nsurlsession)を使用したインターネットからの新しいコンテンツのダウンロードなど、別のタスクが含まれます。
- **バックグラウンドスナップショット更新**- [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask)タスクを使用すると、アプリケーションは、ドッキングの設定に使用されるスナップショットを取得する前に、コンテンツと UI の両方を更新できます。
- **バックグラウンドウォッチ接続**-ペアになっている iPhone からバックグラウンドデータを受信すると、アプリの[WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask)タスクが開始されます。
- **バックグラウンド URL セッション**-バックグラウンド転送に承認が必要な場合や、(正常にまたはエラーが発生したときに) 完了した場合に、アプリの[WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask)タスクが開始されます。

詳細については、「[バックグラウンドタスク](~/ios/watchos/platform/background-tasks.md)ガイド」を参照してください。

<a name="Complications-Enhancements"></a>

## <a name="complications-enhancements"></a>複雑な機能強化

複雑さは、役に立つ情報を一目で示す小さな視覚要素です。 選択したウォッチ式に応じて、ユーザーは1つまたは複数の複雑なウォッチ式を使用してウォッチフェイスをカスタマイズできます。

watchOS 3 では、ウォッチアプリに対して1つ以上の複雑な機能を作成することができます。これにより、ユーザーがウォッチ式の情報に目を通してアクセスできるようになります。

さらに、複雑さには次のような利点があります。

- ユーザーは、ウォッチ式を直接タップして、アプリケーションをすばやく起動できます。
- ウォッチ盤でアプリの複雑さの1つを使用すると、システムはアプリを起動可能な状態にして、バックグラウンドでアプリを起動しようとし、メモリ内に保持し、更新に余分な時間を与えるようにします。
- 複雑さは、1日に50以上のプッシュ更新が保証されることを保証します。
- アプリに複雑な点がある場合は、Apple Watch Face ギャラリーに表示されます。

WatchOS 3 では、ClockKit フレームワークには、 [CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext)や[CLKComplicationTemplateExtraLargeRingImage](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage)などの大規模な複雑さに対応するための新しいテンプレートがいくつか追加されました。 さらに、ローカライズ可能なテキストを作成するには、 [Clktextprovider](https://developer.apple.com/reference/clockkit/clktextprovider)クラスの新しいメソッドを使用します。

詳細については、 [watchOS 3 ガイドのクイック対話手法に関する](~/ios/watchos/platform/quick-interaction-techniques.md)記事をご覧ください。

<a name="Newly-Available-Frameworks"></a>

## <a name="newly-available-frameworks"></a>新しく利用可能なフレームワーク

watchOS 3 には、次のような、以前は使用できなかった既存の Apple framework がいくつか含まれています。

- **SceneKit** -SceneKit を使用して、3d モデルを watch アプリの UI に含めます。これには、照明、網掛け、アニメーション、物理システム、およびパーティクルシステムなど、他のプラットフォームで使用できる機能の大部分が含まれます。 3D 空間オーディオ、カスタム金属または OpenGL シェーダー、コアイメージフィルター、物理的ベースのマテリアルはサポートされていません。
- **SpriteKit** -SpriteKit を使用して、アプリウォッチアプリの UI でスプライトをレンダリングし、アニメーション化します。これには、アクション、物理、照明、およびパーティクルシステムなど、他のプラットフォームで利用できる機能の大部分が含まれます。 3D 空間オーディオ、ビデオ再生、およびコアイメージフィルターはサポートされていません。
- **Avfoundation** -オーディオを管理および再生します。
- **Cloudkit** -watch アプリと iCloud コンテナーの間でデータを移動します。
- **コアオーディオ**-オーディオストリーム、複雑なバッファー、時刻値を表すデータ型を管理します。
- ゲーム**キット**-ソーシャルゲームを作成します。

<a name="Proactive-Suggestions"></a>

## <a name="proactive-suggestions"></a>プロアクティブな候補

watchOS 3 を使用すると、アプリは特定のコンテキスト内でユーザーに情報を事前に提示できます。 この機能をサポートするために、 [Nsuseractivity](https://developer.apple.com/reference/foundation/nsuseractivity)には、 `MapItem` アプリが他のアプリで後で使用するための場所情報を提供できるようにするプロパティが含まれるようになりました。

詳細については、「[プロアクティブな提案](~/ios/watchos/platform/proactive-suggestions.md)ガイドの概要」を参照してください。

<a name="Security-and-Privacy-Enhancements"></a>

## <a name="security-and-privacy-enhancements"></a>セキュリティとプライバシーの強化

Apple は、watchOS 3 のセキュリティとプライバシーの両方に対していくつかの機能強化を行っています。これは、開発者がアプリのセキュリティを向上させ、エンドユーザーのプライバシーを確保するのに役立ちます。

結果として、watchOS 3 (またはそれ以降) で実行されるアプリは、特定の機能またはユーザー情報にアクセスする目的を静的に宣言する必要があります。そのためには、アプリがアクセスする必要がある理由を説明する1つまたは複数のプライバシー固有のキーをファイルに入力し `Info.plist` ます。

WatchOS 3 はこれらの変更を iOS 10 で共有するため、詳細については、iOS 10 の[セキュリティとプライバシーの強化](~/ios/app-fundamentals/security-privacy.md)に関するガイドを参照してください。

<a name="Snapshots-and-Dock"></a>

## <a name="snapshots-and-dock"></a>スナップショットとドッキング

WatchOS 3 では、Apple がドッキングを追加し、ユーザーがお気に入りのアプリをピン留めしてすぐにアクセスできるようにしました。 ユーザーが Apple Watch の横にあるボタンを押すと、ピン留めされたアプリスナップショットのギャラリーが表示されます。 ユーザーは、左または右にスワイプして目的のアプリを見つけ、アプリをタップして起動し、スナップショットを実行中のアプリのインターフェイスに置き換えることができます。

システムは、アプリの UI のスナップショットを定期的に取得し、それらのスナップショットを使用してドキュメントを作成します。watchOS は、このスナップショットを取得する前に、アプリにコンテンツと UI を更新する機会を与えます。

詳細については、「[バックグラウンドタスク](~/ios/watchos/platform/background-tasks.md)ガイド」と「Apple の[WKSnapshotRefreshBackgroundTask リファレンス](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask)」を参照してください。

<a name="User-Notifications"></a>

## <a name="user-notifications"></a>ユーザーへの通知

WatchOS 3 で導入されたユーザー通知フレームワークは、ローカルとリモートの両方の通知を Apple Watch に配信することをサポートしています。 このフレームワークを使用して、時刻や場所などの特定の条件に基づいて通知をスケジュールし、通知を受信して処理することができます。

詳細については、 [watchOS 3 ガイドのクイック対話手法に関する](~/ios/watchos/platform/quick-interaction-techniques.md)記事をご覧ください。

<a name="Watch-Connectivity-Framework-Enhancements"></a>

## <a name="watch-connectivity-framework-enhancements"></a>接続フレームワークの機能強化の監視

`HasContentPending` [Wcsession](https://developer.apple.com/reference/watchconnectivity/wcsession)クラスの新しいプロパティは、セッションが処理する必要があるデータをバックグラウンドで受信したことを示します。 プロパティは、 `RemainingComplicationUserInfoTransfers` iOS アプリが watchOS の複雑さを更新できる残りの時間を返します。

詳細については、「[バックグラウンドタスク](~/ios/watchos/platform/background-tasks.md)ガイド」を参照してください。

<a name="WatchKit-Framework-Enhancements"></a>

## <a name="watchkit-framework-enhancements"></a>WatchKit Framework の機能強化

watchOS 3 には、次のような WatchKit フレームワークに対するいくつかの機能強化が含まれています。

- アプリは、新しい[WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer)クラスを使用して Digital Crown の状態を取得し、ユーザーが[WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate)クラスを使用してクラウンを回転したときに更新を受け取ることができます。
- [WKExtension](https://developer.apple.com/reference/watchkit/wkextension)クラスには、アプリが `ApplicationState` アプリのランタイム状態を追跡するために使用できるメソッドと[WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate)定数が含まれるようになりました。 `WKExtension`には、バックグラウンドタスクをスケジュールするために使用できる2つの新しいメソッドも用意されています。
- [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate)には、 `ApplicationWillEnterForeground` `ApplicationDidEnterBackground` `HandleBackgroundTasks` アプリの状態の変化を監視し、バックグラウンドタスクの更新を処理するための新しいメソッドとメソッドが追加されました。
- 次の種類のジェスチャ認識を watch アプリに提供するために、新しい[WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer)クラスが追加されました。 [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer)、 [WKPanGestureRecognizer](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer)、 [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) 、および[WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer)。
- 新しい[WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera)クラスは、すべてのホームキットに接続されている IP カメラ用のインターフェイスを提供します。
- 新しい[WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie)クラスを使用すると、アプリは、ユーザーがタップしたときに実行中のムービーに置き換えられるムービー "ポスター" を表示できます。
- 新しい[WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton)クラスを使用すると、アプリは UI に Apple Pay ボタンを表示し、タップしたときに支払い要求を開始することができます。
- 新しい[WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene)クラスは、Apple Watch に SceneKit シーンを表示するためのインターフェイスを提供します。
- 新しい[WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene)クラスは、Apple Watch に SpriteKit シーンを表示するためのインターフェイスを提供します。

詳細については、 [watchOS 3 ガイドのクイック対話手法に関する](~/ios/watchos/platform/quick-interaction-techniques.md)記事をご覧ください。

<a name="Workout-App-Enhancements"></a>

## <a name="workout-app-enhancements"></a>トレーニング アプリの拡張機能

WatchOS 3 の新機能であるトレーニング関連のアプリは、Apple Watch のバックグラウンドで実行することができます。 この機能を有効にする (および HealthKit データへのアクセス権を取得する) には、アプリ `WKBackgroundModes` で値を持つキーをファイルに含める必要があり `Info.plist` `workout-processing` ます。

また、開発者は、ペアになっている iPhone で iOS アプリのバージョンから watchOS のトレーニングアプリを起動できるようになりました。

詳細については、「[トレーニングアプリの拡張機能](~/ios/watchos/platform/workout-apps.md)」を参照してください。

<a name="Additional-Framework-Changes"></a>

## <a name="additional-framework-changes"></a>追加のフレームワークの変更

Apple では、上に示した主要なフレームワークの変更と追加に加えて、watchOS 3 で多くのマイナーフレームワーク変更が加えられています。

詳細については、追加の[フレームワーク変更](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md)ガイドを参照してください。

<a name="Deprecated-APIs"></a>

## <a name="deprecated-apis"></a>非推奨の API

WatchOS 3 では、次の Api が非推奨とされています。

- `UILocalNotification`UIKit のクラスは非推奨とされており、ユーザー通知フレームワークに置き換える必要があります。

廃止と変更の完全な一覧については、Apple の[watchOS 2.2 To watchOS 3.0 API の相違点](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html)に関するドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
- [WatchOS 3 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
