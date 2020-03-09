---
title: iOS 10 の概要
description: この記事では、iOS 10 for Xamarin の開発者向けの新しい Api と変更された Api と機能について説明します。
ms.prod: xamarin
ms.assetid: FB91DFFE-CF5E-4253-92CB-78A6371259D9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: ce262faf2d79e6a2cc969df582446fdc2ec29bde
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78910873"
---
# <a name="introduction-to-ios-10"></a>iOS 10 の概要

新しい iOS 10 SDK を使用すると、Apple に新しい Api とサービスが追加されました。これにより、開発者は新しいカテゴリのアプリと機能を作成できます。 IOS アプリでは、メッセージ、Siri、電話、Maps アプリを拡張して、以前は利用できなかったエンドユーザーに豊富で魅力的な機能を提供できるようになりました。

IOS 10 の詳細については、Apple の[ios + アプリ](https://developer.apple.com/ios/)に関するドキュメントを参照してください。

## <a name="whats-new-in-ios-10"></a>IOS 10 の新機能

Apple では、iOS 10 に新しい Api とサービスがいくつか追加され、既存の機能に対する多くの機能強化が加えられています。

## <a name="adapting-to-the-true-tone-display"></a>実際のトーン表示への適応

Apple の真の雰囲気ディスプレイテクノロジでは、iOS デバイスのアンビエント光センサーを使用して、現在の照明条件に合わせてディスプレイの色と輝度を動的に調整します。 iOS 10 は、アプリの `Info.plist` ファイルに追加できる新しい[UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31)キーを提供し、標準のカラーシフトを適用する方法を制御します。 

次の値を指定できます。

- `UIWhitePointAdaptivityStyleStandard`**既定値**: 標準のホワイトポイント adaptivity を使用します。
- `UIWhitePointAdaptivityStyleReading`-集中アプリに使用されます。
- `UIWhitePointAdaptivityStyleGame`-ゲーム中心のアプリに使用されます。
- `UIWhitePointAdaptivityStyleVideo`-ビデオに焦点を絞ったアプリに使用します。
- `UIWhitePointAdaptivityStylePhoto`-写真に重点を置いたアプリに使用されます。色の忠実性は、環境の白い点の調整よりも重要です。

## <a name="app-extensions"></a>アプリの拡張機能

Apple では、iOS 10 に新しいアプリの拡張ポイントがいくつか提供されています。

- 呼び出しディレクトリ
- インテントとインテント UI
- メッセージ
- 通知の内容
- Notification Services
- ステッカーパック

さらに、サードパーティ製のキーボードアプリ拡張機能には、次のような点があります。

- `UITextDocumentProxy` クラスの新しい `DocumentInputMode` プロパティを使用すると、ドキュメントの入力言語を決定し、その言語に合わせてキーボード拡張を配置できます。
- 新しい `HandleInputModeList` メソッドを使用すると、タップされている地球キーに応答して、キーボード拡張機能でシステムのキーボードピッカーメニューを表示できます。

詳細については、「[拡張機能の概要](~/ios/platform/extensions.md)」、「[メッセージアプリの統合](~/ios/platform/message-app-integration/index.md)」、「[プロアクティブな提案](~/ios/platform/search/proactive-suggestions.md)の概要」、「 [Sirikit](~/ios/platform/sirikit/index.md)の概要」、「ユーザーへの[通知](~/ios/platform/user-notifications/index.md)の概要」、および「Apple の[アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)」を参照してください。

## <a name="app-search-enhancements"></a>アプリ検索の機能強化

IOS 10 のコアスポットライトは、次のようなアプリ検索に対していくつかの機能強化を提供します。

- **引き出しディープリンクの人気度 (差分プライバシー)** -検索結果でディープリンクアプリのコンテンツを昇格する方法を提供します。
- **アプリ内検索**-新しい `CSSearchQuery` クラスを使用して、メール、メッセージ、ノートアプリの動作と同様に、アプリ内スポットライト検索機能を提供します。
- **[検索の継続]** -ユーザーがスポットライトまたは Safari で検索を開始し、アプリを開いて検索を続行できるようにします。
- **検証結果の視覚化**-Apple の[App Search API 検証ツール](https://search.developer.apple.com/appsearch-validation-tool)では、テストを事前に形成するときに、web サイトのマークアップとディープリンクが視覚的に表示されるようになりました。
- **メッセージアプリイメージの共有**-メッセージ (メッセージアプリ拡張機能を使用) での共有用に提供された、人気のあるアプリ内イメージがスポットライト検索に表示されます。

詳細については、[アプリ検索の拡張機能](~/ios/platform/search/app-search-enhancements.md)に関するガイドを参照してください。

## <a name="apple-pay-enhancements"></a>Apple Pay の機能強化

Apple は iOS 10 の Apple Pay に対していくつかの機能強化を行いました。これにより、ユーザーは、web サイトからのセキュリティで保護された支払いを行うことができ、Siri および Maps との対話が可能に

IOS 10 では、iOS と watchOS の両方を使用して、動的な支払いネットワークと新しいサンドボックステスト環境をサポートする新しい Api がいくつか追加されています。

また、`UIKit` の外部に Apple Pay をサポートし、カードの発行者がアプリ内からカードを提示できるように、Pass Kit フレームワークが拡張されました。

詳細については、Apple Pay の[拡張機能](~/ios/platform/apple-pay.md)に関するガイドを参照してください。

## <a name="alternate-app-icons"></a>代替アプリ アイコン

Apple では、アプリによるアイコンの管理を可能にする iOS 10.3 の機能強化がいくつか追加されています。

- `ApplicationIconBadgeNumber`-スプリングボードのアプリアイコンのバッジを取得または設定します。
- `SupportsAlternateIcons`-アプリに別のアイコンのセットがある場合 `true`。
- `AlternateIconName`-プライマリアイコンを使用している場合に、現在選択さ `null` れている代替アイコンの名前を返します。
- `SetAlternameIconName`-アプリのアイコンを指定した代替アイコンに切り替えるには、このメソッドを使用します。

詳細については、アプリの[代替アイコン](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)ガイドを参照してください。

## <a name="introduction-to-callkit"></a>CallKit の概要

IOS 10 の新しい CallKit API は、VOIP アプリを iPhone UI と統合し、使い慣れたインターフェイスとエクスペリエンスをエンドユーザーに提供するための手段を提供します。 この API を使用すると、ユーザーは iOS デバイスのロック画面から VOIP 通話を表示および操作し、電話アプリの **[お気に入り]** ビューと **[受信者]** ビューを使用して連絡先を管理できます。

また、CallKit API では、電話番号を名前 (発信者 ID) に関連付けたり、番号をブロックする必要がある場合にシステムに通知したりできるアプリ拡張機能を作成することができます (呼び出しブロック)。

詳細については、 [Callkit の概要に](~/ios/platform/callkit.md)関するガイドを参照してください。

## <a name="message-app-integration"></a>メッセージ アプリの統合

iOS 10 では、**メッセージアプリと**統合し、新しい機能をユーザーに提供する Xamarin. iOS ソリューションにメッセージアプリ拡張機能を含めることができます。 拡張機能は、テキスト、ステッカー、メディアファイル、および対話型メッセージを送信できます。 次の2種類のメッセージアプリ拡張機能を使用できます。

- **ステッカーパック**-ユーザーがメッセージに追加できるステッカーのコレクションが含まれています。 ステッカーパックは、コードを記述せずに作成できます。
- **IMessage アプリ**-メッセージアプリ内にカスタムユーザーインターフェイスを提供して、ステッカーの選択、テキストの入力 (オプションの型変換を含む)、および操作メッセージの作成、編集、および送信を行うことができます。

詳細については、「 [Message App 統合](~/ios/platform/message-app-integration/index.md)ガイド」を参照してください。

## <a name="news-publisher-enhancements"></a>ニュースパブリッシャーの機能強化

IOS 10 を使用すると、Apple は主要な雑誌や新しい組織のブロガーが、独立した出版社によるサインアップと製品の登録を行い、コンテンツを Apple News アプリに配信できるようにします。 詳細については、Apple の[ニュースリソース](https://newsresources.apple.com/)のドキュメントを参照してください。

## <a name="providing-haptic-feedback"></a>Haptic フィードバックの提供

IPhone 7 と iPhone 7 に加えて、Apple には、ユーザーに物理的に参加するための追加の手段を提供する新しい haptics 応答が含まれています。 新しい tactile フィードバックオプションを使用して、ユーザーの注意を促し、行動を補強します。

いくつかの組み込み UI 要素には、ピッカー、スイッチ、スライダーなどの haptic フィードバックが既に用意されています。 iOS 10 では、`UIFeedbackGenerator` クラスの具象サブクラスを使用して、プログラムによって haptics をトリガーする機能が追加されました。

詳細については、「 [Haptic フィードバック](~/ios/user-interface/ios-ui/haptic-feedback.md)ガイドの提供」を参照してください。

## <a name="proactive-suggestions"></a>プロアクティブな候補

iOS 10 は、システムが適切なタイミングで有益な情報をユーザーに事前に自動的に提示できるようにすることで、アプリに対するエンゲージメントを促進する新しい方法を提供します。 Ios 9 では、スポットライト、ハンドオフ、Siri の提案を使用してアプリにディープ検索を追加できるようになったのと同じように、iOS 10 では、アプリは次の場所からシステムによってユーザーに提示される機能を公開することができます。

- アプリスイッチャー
- ロック画面
- CarPlay
- マップ
- Siri の相互作用
- QuickType 候補

アプリは、 [Nsuseractivity](xref:Foundation.NSUserActivity)、web マークアップ、コアスポットライト、mapkit、Media Player、uikit などのテクノロジのコレクションを使用して、この機能をシステムに公開します。

詳細については、「[プロアクティブな提案](~/ios/platform/search/proactive-suggestions.md)ガイドの概要」を参照してください。

## <a name="request-app-review"></a>アプリ レビューの要求

IOS 10.3 の新機能である `RequestReview()` 方法では、iOS アプリはユーザーに対して評価またはレビューを要求できます。 このメソッドは、ユーザーエクスペリエンスが理にかなっている任意の時点で呼び出すことができますが、レビュープロセスは App Store ポリシーによって管理および処理されます。 結果として、このメソッドは警告を表示したり、表示したりすることはできません。また、ボタンをタップするなど、ユーザーの操作に応答して呼び出さないでください。

詳細については、「[要求アプリレビュー](~/ios/platform/request-app-review.md)ガイド」を参照してください。

## <a name="security-and-privacy-enhancements"></a>セキュリティとプライバシーの強化

Apple は、iOS 10 のセキュリティとプライバシーの両方に対していくつかの機能強化を行っています。これは、開発者がアプリのセキュリティを向上させ、エンドユーザーのプライバシーを確保するのに役立ちます。

そのため、iOS 10 (またはそれ以降) で実行されるアプリは、特定の機能またはユーザー情報にアクセスする目的を静的に宣言する必要があります。そのためには、`Info.plist` ファイルにプライバシー固有のキーを1つ以上入力して、アプリがアクセスする理由をユーザーに説明します。

詳細については、[セキュリティとプライバシーの強化](~/ios/app-fundamentals/security-privacy.md)に関するガイドを参照してください。

## <a name="sirikit"></a>SiriKit

IOS 10 の新機能である SiriKit を使用すると、Xamarin iOS アプリは、iOS デバイスで Siri を使用してユーザーがアクセスできるサービスを提供できます。 この機能は、新しい**インテント**および**インテント UI**フレームワークを使用して1つ以上のアプリ拡張機能に用意されています。

SiriKit では、次のサービスドメインがサポートされています。

- オーディオまたはビデオの呼び出し。
- 乗り物を予約します。
- ワークスペースの管理。
- メッセージング.
- 写真を検索しています。
- 支払いの送信または受信。

ユーザーがアプリ拡張機能のいずれかのサービスに関連する Siri の要求を行うと、SiriKit は、ユーザーの要求とサポートデータを記述する**インテント**オブジェクトを拡張機能に送信します。 その後、アプリ拡張機能は、指定された**インテント**に対して適切な**応答**オブジェクトを生成し、拡張機能が要求を処理する方法を詳述します。

Siri は通常、すべてのユーザーの操作を処理しますが、アプリの拡張機能は**インテント UI**フレームワークを使用して、アプリのブランド化と追加情報を示す、豊富なカスタムユーザーインターフェイスを提供できます。

詳細については、 [SiriKit の概要に](~/ios/platform/sirikit/index.md)関するガイドを参照してください。

## <a name="speech-recognition"></a>音声認識

iOS 10 には新しい Speech API が含まれています。これにより、アプリは、音声の音声認識と議事録 (ライブまたは録音されたオーディオストリーム) をテキストにすることができます。

音声認識では、Apple のサーバー上のデータの転送と一時的な保存が必要であるため、アプリは、`Info.plist` ファイルに `NSSpeechRecognitionUsageDescription` キーを含め、`SFSpeechRecognizer.RequestAutorization` メソッドを呼び出すことによって、ユーザーが認識を実行するためのアクセス許可を要求_する必要があり_ます。

詳細については、「[音声認識ガイドの概要](~/ios/platform/speech.md)」を参照してください。

## <a name="user-notifications"></a>ユーザーへの通知

IOS 10 の新機能であるユーザー通知フレームワークを使用すると、ローカルおよびリモートの通知を配信および処理することができます。 このフレームワークを使用すると、アプリまたはアプリ拡張機能は、場所や時間などの条件のセットを指定することによって、ローカル通知の配信をスケジュールすることができます。

また、アプリまたは拡張機能は、ローカルとリモートの両方の通知をユーザーの iOS デバイスに配信するときに、それらを受信 (および変更する可能性があります) することができます。

新しいユーザー通知 UI フレームワークでは、アプリまたはアプリ拡張機能を使用して、ユーザーに表示されるときに、ローカルとリモートの両方の通知の外観をカスタマイズできます。

詳細については、「[ユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)ガイド」を参照してください。

## <a name="video-subscriber-account"></a>ビデオ購読者アカウント

IOS 10 の新機能であるビデオサブスクライバーアカウントフレームワークを使用すると、認証されたストリーミングまたはビデオオンデマンドをサポートするアプリは、エンドユーザーにシングルサインインエクスペリエンスを使用して、ケーブルまたは衛星放送会社による認証を行うことができます。

## <a name="wide-color"></a>広色域

iOS 10 は、コアグラフィックス、コアイメージ、メタル、AVFoundation などのフレームワークを含む、システム全体の拡張範囲のピクセル形式と広い範囲の色空間のサポートを拡張します。 グラフィックススタック全体でこの動作を提供することにより、さまざまな色で表示されるデバイスのサポートがさらに緩和さます。

さらに、 [Uikit](xref:UIKit)は新しい extended **sRGB** colorspace で動作するように変更されているため、パフォーマンスが大幅に低下することなく、色域の色をより簡単に混在させることができます。

Apple では、広範囲にわたる色を使用するときに、次のベストプラクティスを提供しています。

- [UIColor](xref:UIKit.UIColor)は sRGB 色空間を使用するようになり、`1.0` 範囲の `0.0` に値がクランプされることはなくなりました。 アプリが以前のクランプ動作に依存している場合は、iOS 10 用に変更する必要があります。
- IPad Pro でカスタム `UIView` 描画を実行するときに、描画環境が sRGB 色空間用に構成されます。
- アプリで `UIImages`のカスタムレンダリングを実行する場合は、新しい[UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer)クラスを使用して、拡張範囲または標準の範囲形式の使用を指定します。
- コアグラフィックスや金属などの低レベルの API を使用してイメージ処理を行う場合、開発者は、16ビット浮動小数点値をサポートする拡張範囲の色空間とピクセル形式を使用する必要があります。 必要に応じて、開発者は色コンポーネントの値を手動で固定する必要があります。
- コアグラフィックス、コアイメージ、および金属パフォーマンスシェーダーはすべて、2つの色空間間で変換を行うための新しいメソッドを提供します。

詳細については、「 [Wide カラー](~/ios/platform/wide-color.md)ガイドの概要」を参照してください。

## <a name="widget-enhancements"></a>ウィジェットの機能強化

Apple では、ウィジェットシステムにいくつかの機能強化が導入され、新しい iOS 10 ロック画面に存在する背景でウィジェットが見栄えよく見えるようになりました。 [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect)プロパティは非推奨とされており、新しい[WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect)または[WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect)プロパティに置き換えられています。 さらに、ウィジェットには[NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode)プロパティが含まれるようになりました。これにより、開発者は、使用可能なコンテンツの量を説明し、ユーザーがコンテンツを展開したり折りたたんだりできるようになります。

詳細については、[検索とホーム画面のウィジェットの機能強化](~/ios/platform/search/widgets.md)に関するガイドを参照してください。

## <a name="additional-framework-changes"></a>追加のフレームワークの変更

Apple では、上に示したフレームワークの主な変更点と追加機能に加えて、iOS 10 で多くのマイナーフレームワーク変更が加えられています。

詳細については、追加の[フレームワーク変更](~/ios/platform/introduction-to-ios10/additional-framework-changes.md)ガイドを参照してください。

## <a name="deprecated-apis"></a>非推奨の API

次の Api は、iOS 10 では非推奨となりました。

- `CKDiscoverAllContactsOperation`、`CKDiscoveredUserInfo`、`CKDiscoverUserInfosOperation`、および `CKFetchRecordChangesOperation` クラスは、iOS 10 用の CloudKit では非推奨とされました。 代わりに、レコードの共有をサポートする[CKDiscoverAllUserIdentitiesOperation](xref:CloudKit.CKDiscoverUserIdentitiesOperation)、 [CKUserIdentity](xref:CloudKit.CKUserIdentity) 、 [CKFetchRecordZoneChangesOperation](xref:CloudKit.CKFetchRecordZoneChangesOperation)の各クラスを使用してください。
- いくつかの[CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) api (ゾーンベースおよびクエリベースのサブスクリプションなど) は非推奨となりました。 代わりに、 [CKRecordZoneSubscription](xref:CloudKit.CKRecordZoneSubscription) Api と[CKQuerySubscription](xref:CloudKit.CKQuerySubscription) api を使用してください。
- ユビキタスコンテンツに関連する[NSPersistentStoreCoordinator](xref:CoreData.NSPersistentStoreCoordinator)シンボルは非推奨となりました。
- [Uiviewcontroller](xref:UIKit.UIViewController)クラスの `ADBannerView`、`ADInterstitialAd` および関連するシンボルの使用は非推奨とされました。
- 浮動小数点値に関連する[Skuniform](https://developer.apple.com/reference/spritekit/skuniform)シンボルは、非推奨とされます。
- UIKit の `UILocalNotification`、`UIMutableUserNotificationAction`、`UIMutableUserNotificationCategory`、`UIUserNotificationAction`、`UIUserNotificationCategory`、および `UIUserNotificationSettings` クラスは非推奨とされました。 代わりに、[ユーザー通知](#user-notifications)フレームワークを使用してください。
- `HandleActionForLocalNotification`、`HandleActionForRemoteNotification`、`DidReceiveLocalNotification`、および `DidReceiveRemoteNotification` の WatchKit メソッドの使用は非推奨とされました。 代わりに、`HandleActionForNotification` メソッドと `DidReceiveNotification` メソッドを使用してください。
- [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate)の `DidReceiveLocalNotification` および `DidReceiveRemoteNotification` メソッドの使用は非推奨とされました。 適切なメソッドを実装し、 [Unusernotificationcenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter)オブジェクトの `Delegate` プロパティに割り当てる[Unusernotificationcenter デリゲート](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate)のインスタンスを作成します。
- **Game Center アプリ**は非推奨となり、iOS から削除されました。 アプリで使用する場合は、独自のインターフェイスを提示して、スコアボードなどのユーザーキット機能を表示_する必要があり_ます。

廃止の完全な一覧については、Apple の[ios 9.3 To ios 10.0 API の相違点](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html)に関するドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
