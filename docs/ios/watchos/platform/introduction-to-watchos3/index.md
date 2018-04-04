---
title: WatchOS 3 の概要
description: この記事では、Xamarin の開発者向けのすべての新しいまたは変更された Api と watchOS 3 で使用できる機能を紹介します。
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/07/2017
ms.openlocfilehash: 506e3795538ceffc77301a608c504fc6ec2045a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-watchos-3"></a>WatchOS 3 の概要

_この記事では、Xamarin の開発者向けのすべての新しいまたは変更された Api と watchOS 3 で使用できる機能を紹介します。_

このドキュメントは、次のトピックを取り上げます。

- [新機能 watchOS 3](#Whats-New-in-watchOS-3)
    - [Apple の料金を支払う機能強化](#Apple-Pay-Enhancements)Apple Watch でのアプリ内の支払いのサポートを追加します。
    - [バック グラウンド タスク](#Background-Tasks)できるように、ユーザーが必要になったときに、バック グラウンドで情報を更新する機能をアプリに提供します。
    - [複雑さの一部の機能強化](#Complications-Enhancements)watchOS 3、アプリの新機能を提供するのに加えられました。
    - [使用可能なフレームワーク新しく](#Newly-Available-Frameworks)watchOS アプリを公開しています。
    - [プロアクティブな提案](#Proactive-Suggestions)アプリ事前対応的に情報をユーザーに表示を許可します。
    * いくつか[セキュリティとプライバシーの機能強化](#Security-and-Privacy-Enhancements)watchOS 3 に加えられました。
    - [スナップショットとドッキング](#Snapshots-and-Dock)app watchOS アプリへのクイック アクセスをユーザーに提供します。
    - [ユーザーへの通知](#User-Notifications)ローカルとリモートの両方の通知をユーザーに提供します。
    * いくつか[ウォッチ接続フレームワークの機能強化](#Watch-Connectivity-Framework-Enhancements)watchOS 3 に加えられました。
    * いくつか[WatchKit フレームワークの機能強化](#WatchKit-Framework-Enhancements)watchOS 3 に加えられました。
    - [トレーニングのアプリの機能強化](#Workout-App-Enhancements)トレーニングする新しい機能を示しています。 関連する Apple Watch アプリ。
- [その他のフレームワークの変更](#Additional-Framework-Changes)watchOS 3 全体が加えられました。
- [非推奨 Api](#Deprecated-APIs) watchOS 3 にします。

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>新機能 watchOS 3

Apple には、多くの機能強化を含め、既存の機能にと共に watchOS 3 でいくつかの新しい Api とサービスが追加します。

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>Apple 給与の機能強化

WatchOS 3、PassKit フレームワークを Apple Watch で実行されているアプリの (物理的な商品およびサービスの両方) のセキュリティで保護された、アプリ内の支払いをサポートするために展開されています。

使用して、新しい[PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller)と[PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)クラスを提示し、ユーザーが支払い要求を承認できるインターフェイスに応答します。

詳細については、次を参照してください、 [Apple 料金を支払う強化](~/ios/watchos/platform/apple-pay.md)ガイドです。

<a name="Background-Tasks" />

## <a name="background-tasks"></a>バックグラウンド タスク

watchOS 3 には、これを開く前に、ユーザーが必要なコンテンツがあることを確認情報を更新するアプリが使用できるいくつかのバック グラウンド タスクが導入されています。

次の新しいバック グラウンド タスクを使用できます。

- **アプリの更新をバック グラウンド**- [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask)タスクがバック グラウンドでその状態を更新するアプリを許可します。 通常これを使用して、インターネットから新しいコンテンツのダウンロードなどの別のタスクが含まれます、 [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession)です。
- **スナップショットの更新をバック グラウンド**- [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask)タスクは、システムはドッキング ステーションの設定に使用されるスナップショットを取得する前に、そのコンテンツと UI の両方を更新するアプリを許可します。
- **バック グラウンドのウォッチ接続**- [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask)ペア iPhone からバック グラウンド データを受け取ると、アプリのタスクが開始します。
- **URL のセッションをバック グラウンド**- [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask)バック グラウンド転送は、承認を必要とまたは (正常またはエラー) を完了すると、アプリのタスクが開始します。

詳細については、次を参照してください、[バック グラウンド タスク](~/ios/watchos/platform/background-tasks.md)ガイドです。

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>複雑さの一部の機能強化

複雑さの一部は、一目で有用な情報を提供する小規模のビジュアル要素です。 選択されているウォッチの文字盤、によっては、そのユーザーは、1 つまたは複数のコンプリケーションとウォッチの文字盤をカスタマイズする機能を持ちます。

watchOS 3 は、アプリにユーザーがその情報の概要をウォッチの文字盤からアクセスできるように、watch アプリの 1 つまたは複数のコンプリケーションを作成する機能を示します。

さらに、複雑さの一部では、次の利点があります。

- ユーザーは、ウォッチの文字盤から直接コンプリケーションの順にタップして、アプリを迅速に起動できます。
- ウォッチ面が原因である、アプリの複雑な問題のいずれかのバック グラウンドでアプリを起動しようとする場所の準備完了の起動状態でアプリを保持するシステムに保管してなり、更新に余分なメモリです。
- 複雑さの一部には、1 日あたりの少なくとも 50 のプッシュ更新プログラムが保証されます。
- Apple Watch フェイス ギャラリーで紹介されるアプリには、複雑さの一部が含まれている場合 (Apple を参照してください[ギャラリーに追加する複雑さ](https://developer.apple.com/documentation/clockkit/adding_complications_to_the_gallery)詳細についてはドキュメント)。

WatchOS 3 で ClockKit framework 今すぐなどが含まれる特大の複雑な問題のいくつかの新しいテンプレート[CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext)と[CLKComplicationTemplateExtraLargeRingImage](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). さらに、ローカライズ可能なテキストを作成する新しい方法を使用して、 [CLKTextProvider](https://developer.apple.com/reference/clockkit/clktextprovider)クラスです。

詳細については、次を参照してください、 [watchOS 3 を相互作用テクニックをクイック](~/ios/watchos/platform/quick-interaction-techniques.md)ガイドです。

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>新しく利用可能なフレームワーク

watchOS 3 には、いくつかの既存 Apple フレームワークなど、以前使用できなかったが含まれています。

- **SceneKit** -ほとんどの光源の方向、網かけ、アニメーション、物理的な特性と同様に他のプラットフォームで使用できる機能とパーティクル システムを含む watch アプリの UI に 3D を使用して SceneKit をモデル化します。 3D 空間オーディオ、カスタム金属または OpenGL シェーダー、Core イメージ フィルターおよび物理的に基づくマテリアルはサポートされていません。
- **SpriteKit** -使用 SpriteKit を表示したり、アクション、物理的な特性、照明およびパーティクルのシステムと同様に他のプラットフォームで使用できる機能のほとんどを含むアプリ watch アプリの UI でスプライトをアニメーション化します。 3D 空間オーディオ、ビデオの再生、およびコア イメージ フィルターはサポートされていません。
- **AVFoundation** - を管理およびオーディオを再生します。
- **CloudKit** - watch アプリと iCloud のコンテナー間でデータを移動します。
- **オーディオのコア**- オーディオ ストリーム、複雑な buffers、および時刻の値を表すためのデータ型を管理します。
- **GameKit** - ソーシャル ゲームを作成します。

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>プロアクティブなご提案

watchOS 3 内でのユーザーに情報を表示する事前にアプリを許可するコンテキストを指定します。 この機能をサポートするために、 [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity)が含まれています、`MapItem`アプリに他のアプリで後で使用できる場所の情報を提供するプロパティです。

詳細については、次を参照してください、[プロアクティブな推奨事項の概要](~/ios/watchos/platform/proactive-suggestions.md)ガイドです。

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>セキュリティとプライバシーの機能強化

Apple には、両方のセキュリティとプライバシー、開発者がアプリのセキュリティを強化し、エンドユーザーのプライバシーを確保に役立つ watchOS 3 へのいくつかの機能強化が確立します。

その結果、watchOS 3 (以降) で実行されている必要があります静的に宣言の 1 つまたは複数のプライバシーに関する特定キーを入力して特定の機能、またはユーザー情報にアクセスしようとすると、その`Info.plist`ファイルをアプリがアクセスしようとした理由をユーザーに説明します。

WatchOS 3 は、iOS 10 で、これらの変更を共有するため、iOS 10 を参照してください[セキュリティとプライバシーの機能強化](~/ios/app-fundamentals/security-privacy.md)についてガイドします。

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>スナップショットとドッキング

WatchOS 3 で Apple がドッキング ステーション、ユーザーが自分のお気に入りのアプリをピン留めし、すばやくアクセスを追加します。 ユーザーは、Apple Watch での横のボタンを押すと、固定されたアプリのスナップショットのギャラリーが表示されます。 ユーザーは、左または右に目的のアプリを検索し、スナップショットを置き換えて実行中のアプリのインターフェイスを起動するアプリをタップにスワイプします。

システムは定期的に、アプリの UI のスナップショットし、それらのスナップショットを使用してドキュメントを作成します。watchOS 正しければ、アプリをこのスナップショットが作成される前に、そのコンテンツと UI を更新します。

詳細についてを参照してください、[バック グラウンド タスク](~/ios/watchos/platform/background-tasks.md)ガイドと Apple の[WKSnapshotRefreshBackgroundTask 参照](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask)です。

<a name="User-Notifications" />

## <a name="user-notifications"></a>ユーザーへの通知

WatchOS 3 で導入されたユーザーの通知フレームワークでは、Apple Watch にローカルとリモートの両方の通知の配信をサポートします。 このフレームワークを使用して、日または場所の時間などの特定の条件に基づく通知のスケジュールを設定してを受け取り、通知を処理します。

詳細については、次を参照してください、 [watchOS 3 を相互作用テクニックをクイック](~/ios/watchos/platform/quick-interaction-techniques.md)ガイドです。

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>接続フレームワークの拡張機能を見る

新しい`HasContentPending`のプロパティ、 [WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession)クラスは、セッションが処理する必要があるバック グラウンドでデータを受信したことを示します。 および`RemainingComplicationUserInfoTransfers`プロパティは、その watchOS コンプリケーションを更新できる残りの時間を iOS アプリを返します。

詳細については、次を参照してください、[バック グラウンド タスク](~/ios/watchos/platform/background-tasks.md)ガイドです。

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>WatchKit フレームワークの機能強化

watchOS 3 には、次を含む WatchKit フレームワークには拡張機能が含まれています。

- アプリはデジタル クラウンの状態を取得できます、new を使用して[WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer)クラウンを使用して、ユーザーを回転させると、更新プログラムを受信し、 [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate)クラスです。
- [WKExtension](https://developer.apple.com/reference/watchkit/wkextension)クラスが含まれています、`ApplicationState`メソッドおよび[WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate)定数をアプリは、アプリのランタイム状態を追跡することができます。 `WKExtension` バック グラウンド タスクのスケジュール設定に使用できる 2 つの新しいメソッドを提供します。
- [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate)が含まれています、新しい`ApplicationWillEnterForeground`、`ApplicationDidEnterBackground`と`HandleBackgroundTasks`をアプリの状態の変化を監視し、バック グラウンド タスクの更新を処理するメソッド。
- 新しい[WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer)ジェスチャ認識 watch アプリへの次の種類を提供するクラスが追加されました: [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer)、 [WKPanGestureRecognizer](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer)、 [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer)と[WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer)です。
- 新しい[WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera)任意 HomeKit が IP のカメラを接続クラスのインターフェイスを提供します。
- 新しい[WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie)クラスは、ムービーが、ユーザーがタップすると、実行中のビデオで置き換えられる「ポスター」を表示するアプリを使用できます。
- 新しい[WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton)クラスがタップ支払い要求を開始する UI に、Apple Pay ボタンを表示するアプリを使用できます。
- 新しい[WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene)クラスは、Apple Watch で SceneKit シーンを表示するためのインターフェイスを表示します。
- 新しい[WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene)クラスは、Apple Watch で SpriteKit シーンを表示するためのインターフェイスを表示します。

詳細については、次を参照してください、 [watchOS 3 を相互作用テクニックをクイック](~/ios/watchos/platform/quick-interaction-techniques.md)ガイドです。

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>トレーニングのアプリの機能強化

新しい watchOS 3、トレーニング関連するアプリには、Apple Watch でバック グラウンドで実行する機能が設定されています。 この機能を有効にする (および HealthKit データにアクセスできる)、アプリを含める必要があります、`WKBackgroundModes`キー、 `Info.plist` 、値を持つファイル`workout-processing`です。

さらに、開発者は、今すぐペア iPhone 上の iOS アプリのバージョンから watchOS トレーニング アプリを起動する権限を持ちます。

詳細については、次を参照してください、[トレーニング アプリの機能強化](~/ios/watchos/platform/workout-apps.md)ガイドです。

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>その他のフレームワークの変更

に加えて、主要なフレームワークの変更と追加機能の上に示した Apple 追加マイナー フレームワークの多くの変更に向けた watchOS 3 です。

詳細については、次を参照してください、 [Framework の変更の追加](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md)ガイドです。

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>非推奨 API

WatchOS 3 では、次の Api が廃止されました。

- `UILocalNotification` UIKit のクラスは廃止されており、ユーザー通知のために、フレームワークで置き換える必要があります。

Apple を参照してください[watchOS 3.0 API の相違点を watchOS 2.2](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html)機能廃止と変更の完全な一覧についてはドキュメントです。


## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://developer.xamarin.com/samples/watchos/all/)
- [WatchOS 3 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
