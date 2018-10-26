---
title: WatchOS 3 の概要
description: この記事では、Xamarin 開発者向けのすべての新規および変更した Api と watchOS 3 で使用できる機能を紹介します。
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/07/2017
ms.openlocfilehash: 0428a0df157e359ab34a6a71dbba31bdeb6962fa
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121933"
---
# <a name="introduction-to-watchos-3"></a>WatchOS 3 の概要

_この記事では、Xamarin 開発者向けのすべての新規および変更した Api と watchOS 3 で使用できる機能を紹介します。_

このドキュメントは、次のトピックを取り上げます。

- [WatchOS 3 を新します。](#Whats-New-in-watchOS-3)
    - [Apple の支払いの機能強化](#Apple-Pay-Enhancements)Apple Watch のアプリでの支払いのサポートを追加します。
    - [バック グラウンド タスク](#Background-Tasks)できるように、ユーザーが必要なときに、バック グラウンドでは、その情報を更新する機能をアプリに付与します。
    - [コンプリケーションの機能強化](#Complications-Enhancements)アプリの新機能を提供する watchOS 3 に対して行われました。
    - [新しく使用可能なフレームワーク](#Newly-Available-Frameworks)watchOS アプリを公開しています。
    - [プロアクティブな候補](#Proactive-Suggestions)事前に、ユーザーに情報を表示するアプリに許可します。
    * いくつか[セキュリティおよびプライバシーの強化機能](#Security-and-Privacy-Enhancements)watchOS 3 が加えられました。
    - [スナップショットとドッキング](#Snapshots-and-Dock)app watchOS アプリにすばやくアクセスをユーザーに提供します。
    - [ユーザー通知](#User-Notifications)をユーザーにローカルとリモートの両方の通知を提供します。
    * いくつか[ウォッチ接続 Framework 拡張](#Watch-Connectivity-Framework-Enhancements)watchOS 3 で行われました。
    * いくつか[WatchKit Framework 拡張](#WatchKit-Framework-Enhancements)watchOS 3 で行われました。
    - [トレーニング アプリの拡張機能](#Workout-App-Enhancements)関連する Apple Watch アプリをトレーニングに新しい機能を提供します。
- [その他のフレームワークの変更](#Additional-Framework-Changes)watchOS 3 全体が加えられました。
- [非推奨の Api](#Deprecated-APIs) watchOS 3 にします。

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>WatchOS 3 を新します。

Apple は watchOS 3 などの既存の機能に多くの機能強化と共にでいくつかの新しい Api やサービスが追加します。

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>Apple Pay 機能強化

WatchOS 3、PassKit framework は、Apple Watch で実行されるアプリ (物理的な商品およびサービスの両方) のセキュリティで保護された、アプリ内の支払いのサポートを許可する拡張されました。

使用して、新しい[PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller)と[PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)を提示し、ユーザーが支払いの要求を承認できますインターフェイスに対応するクラス。

詳細については、次を参照してください、 [Apple 支払い強化](~/ios/watchos/platform/apple-pay.md)ガイド。

<a name="Background-Tasks" />

## <a name="background-tasks"></a>バックグラウンド タスク

watchOS 3 では、コンテンツがあることを確認状を開く前に、ユーザーが必要な情報を更新するアプリが使用できるいくつかのバック グラウンド タスクについて説明します。

次の新しいバック グラウンド タスクを使用できます。

- **アプリの更新をバック グラウンド**- [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask)タスクがバック グラウンドでは、その状態を更新するアプリを許可します。 通常、これを使用して、インターネットから新しいコンテンツのダウンロードなどの別のタスクが含まれます、 [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession)します。
- **スナップショットの更新をバック グラウンド**- [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask)タスクは、システムは、ドッキング ステーションを設定するために使用するスナップショットを取得する前に、そのコンテンツと UI の両方を更新するアプリを許可します。
- **ウォッチの接続をバック グラウンド**- [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask)ペアになっている iPhone からバック グラウンド データを受信すると、アプリのタスクが開始します。
- **URL のセッションをバック グラウンド**- [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask)バック グラウンド転送が承認を必要があります。 または (正常にまたはエラー) を完了すると、アプリのタスクが開始します。

詳細については、次を参照してください、[バック グラウンド タスク](~/ios/watchos/platform/background-tasks.md)ガイド。

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>コンプリケーションの機能強化

コンプリケーションとは、ひとめで有益な情報を提供する小規模なビジュアル要素です。 選択されている腕時計によっては、ユーザーは、1 つまたは複数のコンプリケーションとウォッチの文字盤をカスタマイズする機能を持ちます。

watchOS 3 は、ユーザーがその情報の概要をウォッチの文字盤からアクセスできるように、watch アプリの 1 つまたは複数の問題を作成する機能をアプリに提供します。

さらに、複雑な問題では、次の利点があります。

- ユーザーは、ウォッチの文字盤から直接コンプリケーションの順にタップしてアプリを迅速に起動できます。
- ウォッチの顔が原因で、アプリの複雑な問題のいずれかが、バック グラウンドでアプリを起動しようとする場所の準備完了の起動状態で、アプリのために、システムに保管して、更新する時間を余分なメモリ。
- 複雑な問題には、1 日あたり少なくとも 50 のプッシュの更新プログラムが保証されます。
- Apple Watch の顔のギャラリーに掲載されるは、アプリには、複雑な問題が含まれている場合 (Apple を参照してください。[複雑な問題をギャラリーに追加する](https://developer.apple.com/documentation/clockkit/adding_complications_to_the_gallery)詳細についてはドキュメント)。

WatchOS 3 で ClockKit フレームワークが含まれています超大規模な複雑な問題のいくつかの新しいテンプレートにはなど[CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext)と[CLKComplicationTemplateExtraLargeRingImage](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). さらに、ローカライズ可能なテキストを作成するには、新しいメソッドを使用して、 [CLKTextProvider](https://developer.apple.com/reference/clockkit/clktextprovider)クラス。

詳細については、次を参照してください、 [watchOS 3 用のクイック操作手法](~/ios/watchos/platform/quick-interaction-techniques.md)ガイド。

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>新しく使用可能なフレームワーク

watchOS 3 では、以前など使用できなかった既存の Apple フレームワークがいくつか含まれています。

- **SceneKit** -ほとんどの網かけ、アニメーション、照明の物理法則などその他のプラットフォームで使用できる機能とパーティクル システムを含む、watch アプリの UI にモデル化に 3D を含める SceneKit を使用します。 3D 空間オーディオ、金属や OpenGL のカスタムのシェーダー、Core イメージ フィルターおよび物理的に基づく資料はサポートされていません。
- **SpriteKit** -をレンダリングし、アクション、物理学、照明、パーティクル システムと同様に他のプラットフォームで使用できる機能のほとんどを含むアプリ watch アプリの UI でスプライトをアニメーション化する SpriteKit を使用します。 3D 空間オーディオ、ビデオの再生、およびコア イメージ フィルターはサポートされていません。
- **AVFoundation** - を管理およびオーディオを再生します。
- **CloudKit** - watch アプリおよび iCloud コンテナー間でデータを移動します。
- **オーディオのコア**- オーディオ ストリーム、複雑なバッファーと時刻の値を表すためのデータ型を管理します。
- **GameKit** - ソーシャル ゲームを作成します。

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>プロアクティブな候補

watchOS 3 内のユーザーに積極的に情報を提示するアプリを許可するコンテキストを指定します。 この機能をサポートするために、 [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity)が含まれています、`MapItem`アプリに他のアプリで後で使用できる場所の情報を提供するプロパティ。

詳細については、次を参照してください、[プロアクティブな候補の概要](~/ios/watchos/platform/proactive-suggestions.md)ガイド。

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>セキュリティとプライバシーの機能強化

Apple は、開発者がアプリのセキュリティを強化し、エンドユーザーのプライバシーを確保に役立つ watchOS 3 のプライバシーとセキュリティの両方にいくつかの機能強化をしました。

その結果、アプリで watchOS 3 (以降) で実行されている必要がありますで 1 つまたは複数のプライバシーに関する特定キーを入力して特定の機能やユーザー情報にアクセスしようとすると、静的に宣言、`Info.plist`ファイルをユーザー、アプリがアクセスしようとした理由を説明します。

WatchOS 3 は、iOS 10 とこれらの変更を共有するため、iOS 10 を参照してください[セキュリティおよびプライバシーの強化機能](~/ios/app-fundamentals/security-privacy.md)詳細情報をガイドします。

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>スナップショットとドッキング

WatchOS 3、Apple ドッキング ステーション ユーザーが、お気に入りのアプリをピン留めしてにすばやくアクセスできますが追加されます。 ユーザーは、Apple Watch の横のボタンを押すと、アプリをピン留めされたスナップショットのギャラリーが表示されます。 ユーザーは、左または右に、目的のアプリを検索し、そのスナップショットを置き換えて、実行中のアプリのインターフェイスを起動するアプリをタップにスワイプします。

システムは定期的には、アプリの UI のスナップショットを取得し、それらのスナップショットを使用してドキュメントを事前設定。watchOS では、アプリに、このスナップショットを作成する前に、そのコンテンツと UI を更新する機会が与えられます。

詳細についてを参照してください、[バック グラウンド タスク](~/ios/watchos/platform/background-tasks.md)ガイドと Apple の[WKSnapshotRefreshBackgroundTask 参照](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask)します。

<a name="User-Notifications" />

## <a name="user-notifications"></a>ユーザー通知

WatchOS 3 で導入されたユーザー通知フレームワークには、Apple Watch に通知をローカルとリモートの両方の配布がサポートしています。 日または場所の時間などの特定の条件に基づく通知のスケジュール設定および受信して、通知を処理するには、このフレームワークを使用します。

詳細については、次を参照してください、 [watchOS 3 用のクイック操作手法](~/ios/watchos/platform/quick-interaction-techniques.md)ガイド。

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>接続フレームワークの拡張機能をご覧ください。

新しい`HasContentPending`のプロパティ、 [WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession)クラスは、セッションが処理する必要があるバック グラウンドでデータを受信したことを示します。 および`RemainingComplicationUserInfoTransfers`プロパティを返します、iOS アプリ日時に残り、watchOS コンプリケーションを更新できます。

詳細については、次を参照してください、[バック グラウンド タスク](~/ios/watchos/platform/background-tasks.md)ガイド。

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>WatchKit フレームワークの機能強化

watchOS 3 には、WatchKit フレームワークを次のようにいくつかの機能強化が含まれています。

- アプリはデジタル クラウンの状態を取得できます、new を使用して[WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer)クラスし、ユーザーを使用して、クラウンを回転したときに、更新プログラムを受信、 [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate)クラス。
- [WKExtension](https://developer.apple.com/reference/watchkit/wkextension)クラスが含まれています、`ApplicationState`メソッドと[WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate)アプリがアプリのランタイム状態を追跡するために使用できる定数。 `WKExtension` バック グラウンド タスクをスケジュールするために使用する 2 つの新しいメソッドを提供します。
- [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate)新しいが含まれています`ApplicationWillEnterForeground`、`ApplicationDidEnterBackground`と`HandleBackgroundTasks`アプリの状態の変化を監視して、バック グラウンド タスクの更新を処理するメソッド。
- 新しい[WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) watch アプリへのジェスチャ認識の次の型を提供するクラスが追加されました: [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer)、 [WKPanGestureRecognizer](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer)、 [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer)と[WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer)します。
- 新しい[WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera)任意 HomeKit IP カメラを接続クラスがインターフェイスを提供します。
- 新しい[WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie)クラスは、ムービーが、ユーザーがタップすると、実行中のムービーで置き換えられる「ポスター」を表示するアプリを使用できます。
- 新しい[WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton)クラスがタップされたときに、支払い要求を開始する UI で、Apple Pay のボタンを表示するアプリを使用できます。
- 新しい[WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene)クラスは、Apple Watch に SceneKit シーンを表示するためのインターフェイスを表示します。
- 新しい[WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene)クラスは、Apple Watch に SpriteKit シーンを表示するためのインターフェイスを表示します。

詳細については、次を参照してください、 [watchOS 3 用のクイック操作手法](~/ios/watchos/platform/quick-interaction-techniques.md)ガイド。

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>トレーニング アプリの拡張機能

新しい watchOS 3、トレーニングに関連するアプリを Apple Watch でバック グラウンドで実行する機能があります。 この機能を有効にする (および、HealthKit のデータにアクセスできる) に、アプリを含める必要があります、`WKBackgroundModes`キー、`Info.plist`値を持つファイル`workout-processing`します。

さらに、開発者は、ペアになっている iPhone の iOS アプリのバージョンから watchOS トレーニング アプリを起動できるようになりましたが。

詳細については、次を参照してください、[トレーニング アプリの拡張機能](~/ios/watchos/platform/workout-apps.md)ガイド。

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>その他のフレームワークの変更

だけでなく、主要なフレームワークの変更と上記に一覧表示されます、Apple は watchOS 3 の多くの他の軽微なフレームワーク変更を加えたが。

詳細については、次を参照してください、 [Framework 変更](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md)ガイド。

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>非推奨の API

WatchOS 3 では、次の Api が廃止されました。

- `UILocalNotification` UIKit のクラスは非推奨し、ユーザー通知フレームワークで置き換える必要があります。

Apple を参照してください。 [watchOS 3.0 API の相違点を watchOS 2.2](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html)廃止された機能と変更の完全な一覧についてはドキュメントです。


## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://developer.xamarin.com/samples/watchos/all/)
- [WatchOS 3 の新機能新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
