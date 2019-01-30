---
title: IOS 10 の概要
description: この記事では、Xamarin.iOS の開発者向けのすべての新規および変更した Api と iOS 10 で使用できる機能を紹介します。
ms.prod: xamarin
ms.assetid: FB91DFFE-CF5E-4253-92CB-78A6371259D9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: f2a612eea39a3447cae03e2d7b675a46c47aad52
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233745"
---
# <a name="introduction-to-ios-10"></a>IOS 10 の概要

_この記事では、Xamarin.iOS の開発者向けのすべての新規および変更した Api と iOS 10 で使用できる機能を紹介します。_

## <a name="introducing-ios-10"></a>IOS 10 の概要

新しい ios 10 SDK、Apple に含まれている新しい Api とアプリおよび機能の新しいカテゴリを作成する開発者を有効にするサービス。 IOS アプリでは、ユーザーが以前使用したリッチで魅力的な機能を提供するメッセージ、Siri、Phone、およびマップ アプリを拡張できるようにします。

IOS 10 の詳細については、Apple を参照してください[iOS + アプリ](https://developer.apple.com/ios/)ドキュメント。

## <a name="whats-new-in-ios-10"></a>IOS 10 で新します。

Apple は iOS 10 と共になど、既存の機能に多くの機能強化でいくつかの新しい Api やサービスが追加します。


## <a name="adapting-to-the-true-tone-display"></a>True のトーン表示への適応

Apple の True トーン表示テクノロジでは、色と現在の照明条件に一致するためにディスプレイの輝度を動的に調整するのに、iOS デバイスで環境光センサーを使用します。 iOS 10 は、新しい[UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31)アプリの追加できるキー`Info.plist`ファイルし、True トーンが標準の色の shift キーを適用する方法を制御します。 

次の値を使用できます。

- `UIWhitePointAdaptivityStyleStandard` **既定の**-標準的なホワイト ポイント関連した適性を使用します。
- `UIWhitePointAdaptivityStyleReading` -アプリの読み取りに重点を置いたの使用。
- `UIWhitePointAdaptivityStyleGame` -アプリのゲームに重点を置いたの使用。
- `UIWhitePointAdaptivityStyleVideo` -アプリのビデオに重点を置いたの使用。
- `UIWhitePointAdaptivityStylePhoto` -アプリの使用写真に重点を置いた色の忠実性が環境の白点の調整をよりも重要です。

<a name="app-extensions" />

## <a name="app-extensions"></a>アプリ拡張機能

Apple は iOS 10 で新しいアプリ拡張機能ポイントをいくつか用意されています。

- ディレクトリを呼び出す
- Intents および Intents UI
- メッセージ
- 通知の内容
- Notification Services
- ステッカー パック

さらに、サード パーティ製キーボード アプリ拡張機能では、次の拡張機能があります。

- 新しい`DocumentInputMode`のプロパティ、`UITextDocumentProxy`クラスは、ドキュメントの入力言語を決定、その言語の連携をキーボード拡張機能を許可できます。
- 新しい`HandleInputModeList`方法では、キーボード拡張機能がタップされる世界中のキーへの応答で、システムのキーボード ピッカー メニューを表示できます。

詳細についてを参照してください、[拡張機能の概要](~/ios/platform/extensions.md)、[メッセージ アプリ統合](~/ios/platform/message-app-integration/index.md)、[プロアクティブな候補の概要](~/ios/platform/search/proactive-suggestions.md)、 [SiriKit の概要](~/ios/platform/sirikit/index.md)、[ユーザー通知の概要](~/ios/platform/user-notifications/index.md)と Apple の[アプリ拡張機能のプログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)します。

## <a name="app-search-enhancements"></a>アプリの検索の機能強化

IOS 10 でコア スポット ライトは、次のアプリを検索するいくつかの機能強化を提供します。

- **(差分プライバシー) のディープ リンクの支持を引き出します**-検索結果でのディープ リンク アプリのコンテンツを昇格する方法を提供します。
- **アプリ内検索**-使用して、新しい`CSSearchQuery`メール、Messages や Notes アプリの動作方法と同様のアプリ内スポット ライト検索機能を提供するクラス。
- **継続を検索**- ユーザーをスポット ライトや、Safari で検索を開始し、アプリを開くし、その検索を続行します。
- **検証結果の視覚化**-Apple の[アプリ検索 API 検証ツール](https://search.developer.apple.com/appsearch-validation-tool)テストの実行時に web サイトのマークアップとディープ リンクのビジュアル表現が表示されます。
- **アプリの画像の共有をメッセージ**-スポット ライト検索で表示するメッセージ (メッセージ アプリ拡張機能) 経由で共有するために提供される一般的なアプリ内のイメージを使用します。

詳細については、次を参照してください、[アプリ検索の機能強化](~/ios/platform/search/app-search-enhancements.md)ガイド。

## <a name="apple-pay-enhancements"></a>Apple Pay 機能強化

Apple は、Siri とマップとの対話と web サイトから支払いのセキュリティで保護されたユーザーに許可する iOS 10 で、Apple Pay にいくつかの機能強化を行ったが。

IOS 10 では、いくつかの新しい Api が追加されました iOS と動的な支払いネットワークと新しいサンド ボックス テスト環境をサポートする watchOS の両方を操作します。

さらに、PassKit framework の外部の Apple Pay をサポートするために拡大されて`UIKit`とそれぞれのアプリ内から、カードを提示するカードの発行元を許可するようにします。

詳細については、次を参照してください、 [Apple 支払い強化](~/ios/platform/apple-pay.md)ガイド。

## <a name="alternate-app-icons"></a>代替アプリ アイコン

Apple には、そのアイコンを管理するアプリを許可する iOS 10.3 にいくつかの機能強化が追加されます。

 - `ApplicationIconBadgeNumber` -を取得します。 または、スプリング ボードで、アプリ アイコンのバッジを設定します。
 - `SupportsAlternateIcons` If`true`アプリには、別のアイコンのセット。
 - `AlternateIconName` -現在選択されている代替アイコンの名前を返しますまたは`null`プライマリ アイコンを使用する場合。
 - `SetAlternameIconName` -このメソッドを使用してアプリのアイコンを指定した代替アイコンに切り替えます。

詳細については、次を参照してください、[代替アプリ アイコン](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)ガイド。


## <a name="introduction-to-callkit"></a>CallKit の概要

IOS 10 で新しい CallKit API は、VOIP アプリを iPhone UI と統合し使い慣れたインターフェイスを提供し、エンドユーザーに発生するための手段を提供します。 この API を使用してユーザーが表示し、iOS デバイスのロック画面から VOIP 通話との対話および連絡先の Phone アプリの使用を管理**お気に入り**と**も最近使ったもの**ビュー。

さらに、CallKit API は、データベースを名前 (呼び出し元の ID) と電話番号を関連付けることができます、または (呼び出しをブロックして)、番号がなるべきときに、システムがブロックされているに通知するアプリの拡張機能を作成する機能を提供します。

詳細については、次を参照してください、 [Callkit 概要](~/ios/platform/callkit.md)ガイド。

## <a name="message-app-integration"></a>メッセージ アプリの統合

iOS 10 と連携する Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を含めることを許可する、**メッセージ**をユーザーに新しい機能がアプリを提示しています。 拡張機能には、テキスト、ステッカー、メディア ファイル、および対話型メッセージを送信できます。 メッセージ アプリ拡張機能の 2 つの種類があります。

- **ステッカー パック**-ユーザーがメッセージに追加できるステッカーのコレクションが含まれています。 ステッカー パックは、すべてのコードを記述することがなく作成できます。
- **iMessage アプリ**-ステッカーを選択すると、テキストを入力する、(省略可能な型変換) によるメディア ファイルを含む、作成、編集、および相互作用のメッセージを送信するためのメッセージ アプリ内のカスタム ユーザー インターフェイスを表示することができます。

詳細については、次を参照してください、[メッセージ アプリ統合](~/ios/platform/message-app-integration/index.md)ガイド。

## <a name="news-publisher-enhancements"></a>ニュースの発行元の機能強化

10、iOS では、Apple は主要な雑誌やブロガーとサインアップを独立した発行元、製品に新しい組織の任意のユーザーを許可して Apple News アプリにコンテンツを配信します。 詳細については、Apple を参照してください[ニュース リソース](https://newsresources.apple.com/)ドキュメント。

## <a name="providing-haptic-feedback"></a>Haptic フィードバックの提供

IPhone 7 および iPhone では、7 Plus、Apple が物理的にユーザーと関わるための追加方法を提供する新しい haptics 応答を含めるがします。 新しい触るフィードバック オプションを使用して、ユーザーの注意を引くし、そのアクションを補強します。

いくつかの組み込み UI 要素は、既にピッカー、スイッチ、スライダーなどのハプティクス フィードバックを提供します。 iOS 10 では、プログラムでの具体的なサブクラスを使用して haptics をトリガする機能を追加、`UIFeedbackGenerator`クラス。

詳細については、次を参照してください、[ハプティクス フィードバックを提供する](~/ios/user-interface/ios-ui/haptic-feedback.md)ガイド。

## <a name="proactive-suggestions"></a>プロアクティブな候補

iOS 10 では、事前に提示する役に立つ情報に自動的にユーザーに適切なタイミングでシステムを許可することで運転 engagement アプリへの新しい方法を表示します。 同様の iOS 9 には、ディープ検索アプリは、次の場所内で、システムにより、ユーザーに表示することができます機能を公開できる iOS 10 のスポット ライト、ハンドオフおよび Siri の推奨事項を使用してアプリを追加する機能が用意されています。

- アプリケーションのスイッチャー
- ロック画面
- CarPlay
- マップ
- Siri の相互作用
- QuickType 提案 

アプリなどのテクノロジのコレクションを使用して、システムには、この機能を公開[NSUserActivity](xref:Foundation.NSUserActivity)、コア スポット ライト、MapKit、Media Player、UIKit、web マークアップ。

詳細については、次を参照してください、[プロアクティブな候補の概要](~/ios/platform/search/proactive-suggestions.md)ガイド。

## <a name="request-app-review"></a>アプリ レビューを要求します。

Ios 10.3、新しい、`RequestReview()`メソッド iOS アプリに許可することを確認したり評価ユーザーに確認します。 このメソッドは、ユーザー エクスペリエンスの理にかなって任意の時点で呼び出すことが、中にレビュー プロセスの管理し、アプリ ストアのポリシーによって処理されます。 結果として、このメソッド可能性がありますまたはアラートが表示されない場合があり、ボタンをタップするなどのユーザー アクションへの応答では呼び出さないでいます。

詳細については、次を参照してください、[アプリ レビューの要求](~/ios/platform/request-app-review.md)ガイド。

## <a name="security-and-privacy-enhancements"></a>セキュリティとプライバシーの機能強化

Apple は、開発者がアプリのセキュリティを強化し、エンドユーザーのプライバシーを確保に役立つ iOS 10 のプライバシーとセキュリティの両方にいくつかの機能強化をしました。

結果として、アプリが iOS 10 (以降) で実行されている必要がありますで 1 つまたは複数のプライバシーに関する特定キーを入力して特定の機能やユーザー情報にアクセスしようとすると、静的に宣言、`Info.plist`ファイルをユーザー、アプリがアクセスしようとした理由を説明します。

詳細については、次を参照してください、[セキュリティおよびプライバシーの強化機能](~/ios/app-fundamentals/security-privacy.md)ガイド。

## <a name="sirikit"></a>SiriKit

新しい ios 10 では、SiriKit により、Xamarin.iOS アプリ、iOS デバイスで Siri を使用してユーザーがアクセスできるサービスを提供します。 この機能は、new を使用して 1 つまたは複数のアプリの拡張機能で提供**インテント**と**Intents UI**フレームワーク。

SiriKit には、次のサービスのドメインがサポートされています。

- オーディオまたはビデオ通話。
- 素敵を予約します。
- トレーニングを管理します。
- メッセージング。
- 写真を検索します。
- 送信または支払いを受信します。

SiriKit 送信、拡張機能で、ユーザーがアプリの拡張機能のサービスのいずれかに関連する siri 要求を行うと、**インテント**の関連データと共に、ユーザーの要求を記述するオブジェクト。 アプリ拡張機能は、適切な生成**応答**オブジェクトを指定された**インテント**、拡張機能が要求を処理する方法の詳細を示します。

Siri で処理されるは、すべてのユーザー操作は、通常、アプリ拡張機能を使用できる、**インテント UI**豊富なカスタム ユーザー インターフェイスを備えたアプリのブランドを表示するためにフレームワークと追加情報。

詳細については、次を参照してください、 [SiriKit の概要](~/ios/platform/sirikit/index.md)ガイド。

## <a name="speech-recognition"></a>音声認識

iOS 10 には、アプリを継続的な音声認識をサポートし、議事録の作成 (生または録画のオーディオ ストリーム) からの音声をテキストに許可する新しい Speech API が含まれています。

音声認識は、転送と、アプリ、Apple のサーバー上のデータの一時的なストレージが必要なため_する必要があります_などして認識を実行するユーザーのアクセス許可を要求、`NSSpeechRecognitionUsageDescription`キーでその`Info.plist`ファイルと呼び出し、`SFSpeechRecognizer.RequestAutorization`メソッド。

詳細については、次を参照してください、[音声認識の概要](~/ios/platform/speech.md)ガイド。

## <a name="user-notifications"></a>ユーザー通知

新しい ios 10 では、ユーザー通知の配信とローカルとリモート通知の処理のフレームワークを使用します。 このフレームワークを使用して、アプリまたはアプリ拡張機能をスケジュールできますローカル通知の配信の場所などの条件のセットまたは 1 日の時刻を指定することで。

さらに、アプリまたは拡張機能が表示される (および可能性のある変更できます) ローカルとリモートの両方の通知、ユーザーの iOS デバイスに配信されるようにします。

新しいユーザー通知の UI フレームワークは、アプリまたはアプリの拡張機能をユーザーに表示するときに、ローカルとリモートの両方の通知の外観をカスタマイズできます。

詳細については、次を参照してください、[ユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)ガイド。

## <a name="video-subscriber-account"></a>ビデオのサブスクライバー アカウント

新しい ios 10 の場合、ビデオ サブスクライバー アカウント フレームワークにより、アプリその認証のストリーミング サポートまたはビデオ オンデマンドで、ケーブル会社または衛星テレビ放送、エンドユーザーは、単一のサインイン エクスペリエンスを使用して認証します。

## <a name="wide-color"></a>色

iOS 10 では、拡張範囲のピクセル形式とコア グラフィックス、Core のイメージ、金属製および AVFoundation などのフレームワークを含めて、システム全体で全体の色域スペースのサポートを拡張します。 ワイド カラー ディスプレイを使用したデバイスのサポートはさらに、全体のグラフィックス スタック全体でこの動作を提供することで緩和されました。

さらに、 [UIKit](xref:UIKit)が変更されている、新しい機能を拡張**sRGB**大幅なパフォーマンス失わずワイド色域にカラーの混合をやすくするための色空間。

Apple は、さまざまな色を使用する場合、次のベスト プラクティスを提供します。

- [示す UIColor](xref:UIKit.UIColor)これは、sRGB 色空間となることはありませんクランプする値、`0.0`に`1.0`範囲。 アプリは、以前のクランプ動作に依存する場合は、iOS 10 に変更する必要があります。
- 描画の環境は、カスタムを実行するときに、sRGB 色空間の構成は`UIView`iPad Pro で描画します。
- アプリでのカスタム レンダリングを実行する場合`UIImages`、使用して、新しい[UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer)拡張範囲または範囲の標準形式の使用を指定するクラス。
- コア グラフィックスや金属などの低レベルの API を使用して、イメージの処理を提供する、開発者は、拡張の範囲の色領域とピクセル形式 16 ビット浮動小数点値をサポートするを使用する必要があります。 必要に応じて、開発者は、色コンポーネントの値を手動でクランプする必要があります。
- コア グラフィックス、Core イメージおよび金属パフォーマンス シェーダーは、2 つのカラー スペース間で変換するための新しいメソッドを提供します。

詳細については、次を参照してください、[色の概要](~/ios/platform/wide-color.md)ガイド。

## <a name="widget-enhancements"></a>ウィジェットの機能強化

Apple には、ウィジェット システムをウィジェットに美しく表示で新しい iOS 10 個のロック画面に存在する任意の背景にいくつかの機能強化が導入されています。 [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect)プロパティは非推奨し、は、新しいに置き換えられました[WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect)または[WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect)プロパティ。 さらに、ウィジェットが含まれています、 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode)プロパティにより、開発者は、コンテンツの量は、使用可能な説明を展開したり、コンテンツを折りたたんだりできます。

詳細については、次を参照してください、[検索とホーム画面ウィジェット機能強化](~/ios/platform/search/widgets.md)ガイド。

## <a name="additional-framework-changes"></a>その他のフレームワークの変更

だけでなく、主要なフレームワークの変更と上記に一覧表示されます、Apple は iOS 10 の多くのマイナー framework 変更を加えたが。

詳細については、次を参照してください、 [Framework 変更](~/ios/platform/introduction-to-ios10/additional-framework-changes.md)ガイド。

## <a name="deprecated-apis"></a>非推奨の API

IOS 10 では、次の Api が廃止されました。

- `CKDiscoverAllContactsOperation`、 `CKDiscoveredUserInfo`、`CKDiscoverUserInfosOperation`と`CKFetchRecordChangesOperation`クラス CloudKit で iOS 10 の廃止されました。 使用して、 [CKDiscoverAllUserIdentitiesOperation](https://developer.xamarin.com/api/type/CloudKit.CKDiscoverUserIdentitiesOperation/)、 [CKUserIdentity](https://developer.xamarin.com/api/type/CloudKit.CKUserIdentity/)と[CKFetchRecordZoneChangesOperation](https://developer.xamarin.com/api/type/CloudKit.CKFetchRecordZoneChangesOperation/)クラス (レコードの共有をサポート) を代わりにします。
- いくつか[CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) (ゾーンおよびクエリ ベース サブスクリプション) などの Api が非推奨とされました。 使用して、 [CKRecordZoneSubscription](https://developer.xamarin.com/api/type/CloudKit.CKRecordZoneSubscription/)と[CKQuerySubscription](https://developer.xamarin.com/api/type/CloudKit.CKQuerySubscription/) Api 代わりにします。
- [NSPersistentStoreCoordnator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/)ユビキタス コンテンツに関連するシンボルが非推奨とされました。
- `ADBannerView`、`ADInterstitialAd`内のシンボルに関連し、 [UIViewController](xref:UIKit.UIViewController)クラスが推奨されていません。
- [SKUniform](https://developer.apple.com/reference/spritekit/skuniform)浮動小数点値に関連するシンボルが非推奨とされました。
- `UILocalNotification`、 `UIMutableUserNotificationAction`、 `UIMutableUserNotificationCategory`、 `UIUserNotificationAction`、`UIUserNotificationCategory`と`UIUserNotificationSettings`UIKit のクラスが非推奨とされました。 使用して、[ユーザー通知](#User-Notifications)framework 代わりにします。
- `HandleActionForLocalNotification`、 `HandleActionForRemoteNotification`、`DidReceiveLocalNotification`と`DidReceiveRemoteNotification`WatchKit メソッドが非推奨とされました。 使用して、`HandleActionForNotification`と`DidReceiveNotification`メソッド代わりにします。
- `DidReceiveLocalNotification`と`DidReceiveRemoteNotification`のメソッド、 [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate)非推奨とされました。 インスタンスを作成[UNUserNotificationCenterDelegate](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate)適切なメソッドを実装してに割り当てる、`Delegate`のプロパティ、 [UNUserNotificationCenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter)オブジェクト。
- **Game Center アプリ**非推奨し、iOS から削除されています。 アプリが GameKit を使用する場合、_する必要があります_GameKit 機能 (ランキングなど) の表示に独自のインターフェイスを提供します。

Apple を参照してください。 [iOS 10.0 API の相違点を iOS 9.3](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html)廃止された機能の完全な一覧についてはドキュメントです。



## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [IOS 10 の新機能新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
