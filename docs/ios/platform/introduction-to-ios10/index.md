---
title: IOS 10 の概要
description: この記事では、Xamarin.iOS 開発者向けのすべての新しいまたは変更された Api と iOS 10 で使用できる機能を紹介します。
ms.prod: xamarin
ms.assetid: FB91DFFE-CF5E-4253-92CB-78A6371259D9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: c3bee0f15016394005a67e98cd8435e6d63b3ac6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30786475"
---
# <a name="introduction-to-ios-10"></a>IOS 10 の概要

_この記事では、Xamarin.iOS 開発者向けのすべての新しいまたは変更された Api と iOS 10 で使用できる機能を紹介します。_

## <a name="introducing-ios-10"></a>IOS 10 の概要

新しい ios 10 SDK、Apple が含まれている新しい Api と、開発者はアプリおよび機能の新しいカテゴリの作成を有効にするサービスです。 IOS アプリでは、エンドユーザーが使用されていた使用可能な豊富な魅力的な機能を提供するメッセージ、Siri、Phone、およびマップのアプリを拡張できますようになりました。

IOS 10 の詳細については、Apple を参照してください[iOS + アプリ](https://developer.apple.com/ios/)ドキュメント。

## <a name="whats-new-in-ios-10"></a>新機能 iOS 10

Apple は iOS 10 と共になど、既存の機能に多くの機能強化にいくつかの新しい Api とサービスが追加します。


## <a name="adapting-to-the-true-tone-display"></a>True トーン ディスプレイに適応させる

Apple の True トーン表示テクノロジは、色と現在の照明条件に一致するのにディスプレイの輝度を動的に調整するのに、iOS デバイスで環境の照明センサーを使用します。 iOS 10 は、新しい[UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31)キー、アプリに追加できる`Info.plist`ファイルし、True トーンが標準的な色ずれを適用する方法を制御します。 

次の値を使用できます。

- `UIWhitePointAdaptivityStyleStandard` **既定の**-標準の空白ポイント adaptivity を使用します。
- `UIWhitePointAdaptivityStyleReading` -アプリの読み取りに重点を置いたに使用されます。
- `UIWhitePointAdaptivityStyleGame` -アプリのゲームに重点を置いたに使用されます。
- `UIWhitePointAdaptivityStyleVideo` -ビデオに重点を置いたアプリに使用されます。
- `UIWhitePointAdaptivityStylePhoto` -アプリに使用される写真に重点を置いた色の忠実性が環境ホワイト ポイント調整より重要です。

<a name="app-extensions" />

## <a name="app-extensions"></a>アプリの拡張機能

Apple は iOS 10 で新しいアプリケーション拡張ポイントがいくつか用意されています。

- ディレクトリを呼び出す
- インテントとインテント UI
- メッセージ
- 通知の内容
- Notification Services
- ステッカー パック

また、サード パーティ製キーボード アプリ拡張機能では、次の機能強化があります。

- 新しい`DocumentInputMode`のプロパティ、`UITextDocumentProxy`クラスは、ドキュメントの入力言語を決定、その言語に合うようにキーボード拡張子を許可できます。
- 新しい`HandleInputModeList`メソッドにより、キーボード拡張機能がタップ球キーへの応答で、システムのキーボード ピッカー メニューを表示します。

詳細についてを参照してください、 [Introduction to Extensions](~/ios/platform/extensions.md)、[メッセージ アプリ統合](~/ios/platform/message-app-integration/index.md)、[プロアクティブな推奨事項の概要](~/ios/platform/search/proactive-suggestions.md)、 [Introduction to SiriKit](~/ios/platform/sirikit/index.md)、[ユーザーへの通知の概要](~/ios/platform/user-notifications/index.md)と Apple の[アプリ拡張機能プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)です。

## <a name="app-search-enhancements"></a>アプリの検索の機能強化

IOS 10 の主要なメディアは、次のアプリを検索するいくつかの拡張機能を提供します。

- **(差分プライバシー) の Crowdsourced ディープ リンク人気**-検索結果にコンテンツをディープ リンク アプリを昇格する方法を提供します。
- **アプリ内検索**-新しい`CSSearchQuery`メール、Messages や Notes アプリの作業と同様に、アプリ内・ Spotlight 検索機能を提供するクラス。
- **継続タスクを検索**- により、ユーザーはアプリを開き、メディアまたは Safari、検索を開始して、その検索を続行します。
- **検証結果の視覚化**-Apple の[アプリの検索 API 検証ツール](https://search.developer.apple.com/appsearch-validation-tool)preforming テストと web サイトのマークアップとディープ リンクの視覚的表現が表示されます。
- **アプリ画像の共有をメッセージ**-Spotlight 検索で表示するメッセージ (メッセージ アプリ拡張機能) 経由で共有するために提供される一般的なアプリでイメージを使用します。

詳細については、次を参照してください、[アプリ検索の機能強化](~/ios/platform/search/app-search-enhancements.md)ガイドです。

## <a name="apple-pay-enhancements"></a>Apple 給与の機能強化

Apple がに対していくつかの機能強化 Apple Pay の web サイトから Siri とマップとの対話によっても、セキュリティで保護された支払にユーザーに許可する ios 10 です。

10、iOS でいくつかの新しい Api が追加されました iOS プラットフォームと watchOS 動的支払ネットワークと新しいサンド ボックスのテスト環境をサポートするために両方を操作します。

さらに、PassKit フレームワークの外部の Apple Pay をサポートするために拡大されて`UIKit`とそれぞれのアプリ内から、カードを提示するカードの発行元を許可します。

詳細については、次を参照してください、 [Apple 料金を支払う強化](~/ios/platform/apple-pay.md)ガイドです。

## <a name="alternate-app-icons"></a>代替アプリのアイコン

Apple では、そのアイコンを管理するアプリを許可する iOS 10.3 にいくつかの機能強化が追加します。

 - `ApplicationIconBadgeNumber` -を取得またはのスプリング ボードで、アプリ アイコンのバッジを設定します。
 - `SupportsAlternateIcons` If`true`アプリは別のアイコンのセット。
 - `AlternateIconName` 現在選択されている別のアイコンの名前を返しますまたは`null`プライマリ アイコンを使用する場合。
 - `SetAlternameIconName` -指定した代替アイコンに、アプリのアイコンを切り替えるには、このメソッドを使用します。

詳細については、次を参照してください、[代替アプリ アイコン](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)ガイドです。


## <a name="introduction-to-callkit"></a>CallKit の概要

IOS 10 で新しい CallKit API では、VOIP アプリ iPhone UI と統合し際に使い慣れたインターフェイスを提供し、エンドユーザーに発生するための手段を提供します。 この API を使用してユーザーが表示し、iOS デバイスのロック画面からの VOIP 呼び出しとの対話および連絡先の電話アプリの使用を管理**お気に入り**と**最近**ビュー。

さらに、CallKit API は、ことができます (呼び出し元の ID) の名前と電話番号を関連付けるか (呼び出しをブロックして)、番号がなるべきときに、システムがブロックされているアプリ拡張機能を作成する機能を提供します。

詳細については、次を参照してください、 [Callkit 概要](~/ios/platform/callkit.md)ガイドです。

## <a name="message-app-integration"></a>メッセージのアプリケーションの統合

iOS 10 と連携し、Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を含めることを許可する、**メッセージ**をユーザーにアプリと表示の新機能です。 拡張機能には、テキスト、ステッカー、メディア ファイルおよび対話型のメッセージを送信できます。 2 つの種類のメッセージ アプリ拡張機能を使用できます。

- **ステッカー パック**-ユーザーがメッセージに追加できるステッカーのコレクションを格納します。 すべてのコードを記述することがなくステッカー パックを作成できます。
- **iMessage アプリ**-ステッカーを選択すると、テキストを入力する、(省略可能な型変換) とメディア ファイルを含む、作成、編集、および操作メッセージを送信する用アプリのメッセージのカスタム ユーザー インターフェイスを提供します。

詳細については、次を参照してください、[メッセージ アプリ統合](~/ios/platform/message-app-integration/index.md)ガイドです。

## <a name="news-publisher-enhancements"></a>ニュースのパブリッシャーの機能強化

10、iOS では、Apple は主要なマガジンと新しい組織ブロガー、サインアップする独立した発行元および製品からのすべてのユーザーおよび Apple ニュース アプリにコンテンツを配信します。 詳細については、Apple を参照してください[ニュース リソース](https://newsresources.apple.com/)ドキュメント。

## <a name="providing-haptic-feedback"></a>Haptic フィードバックを提供します。

IPhone 7 と iPhone で 7 さらに、Apple が含まれる、ユーザーが物理的に参加するその他の方法を提供する新しい haptics 応答します。 ユーザーの注意を取得し、それらのアクションを強化する新しい触るフィードバックのオプションを使用します。

いくつかの組み込み UI 要素は、既にピッカー、スイッチおよびスライダーなど haptic フィードバックを提供します。 iOS 10 には、今すぐプログラムでの具体的なサブクラスを使用して haptics をトリガーする機能が追加されて、`UIFeedbackGenerator`クラスです。

詳細については、次を参照してください、 [Haptic フィードバックの提供](~/ios/user-interface/ios-ui/haptic-feedback.md)ガイドです。

## <a name="proactive-suggestions"></a>プロアクティブなご提案

iOS 10 では、事前対応的に提示する役に立つ情報自動的にユーザーに適切な時点ではシステムによって推進エンゲージメント アプリへの新しい方法を表示します。 同様に iOS 9 には、ios 10 アプリは、次の場所内で、システムにより、ユーザーに表示できる機能を公開できますスポット ライト、ハンドオフおよび Siri の推奨事項を使用してアプリに詳細な検索を追加する機能が用意されています。

- アプリのスイッチャー
- ロック画面
- CarPlay
- マップ
- Siri の相互作用
- QuickType 提案 

アプリがなど、テクノロジのコレクションを使用して、システムには、この機能を公開[NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/)コアのスポット ライト、MapKit、Media Player および UIKit web マークアップ。

詳細については、次を参照してください、[プロアクティブな推奨事項の概要](~/ios/platform/search/proactive-suggestions.md)ガイドです。

## <a name="request-app-review"></a>アプリのレビューを要求します。

IOS 10.3 に新しい、`RequestReview()`メソッドにより、iOS アプリに転送率のまたは確認をユーザーに確認してください。 このメソッドは、合理的であること、ユーザー エクスペリエンスにどの時点でも呼び出すことができる、ときにレビュー プロセスの管理し、App Store のポリシーによって処理します。 その結果、このメソッド可能性がありますまたは警告が表示されない場合があり、ボタンをタップするなどのユーザー アクションへの応答で呼び出さないでです。

詳細については、次を参照してください、[アプリ レビューの要求](~/ios/platform/request-app-review.md)ガイドです。

## <a name="security-and-privacy-enhancements"></a>セキュリティとプライバシーの機能強化

Apple では、両方のセキュリティとプライバシー、開発者がアプリのセキュリティを強化し、エンドユーザーのプライバシーを確保に役立つ iOS 10 にいくつかの機能強化が行わします。

結果として、iOS 10 (またはそれ以降) で実行されている必要があります静的に宣言で 1 つまたは複数のプライバシーに関する特定キーを入力して特定の機能、またはユーザー情報にアクセスしようとすると、その`Info.plist`ファイルをアプリがアクセスしようとした理由をユーザーに説明します。

詳細については、次を参照してください、[セキュリティとプライバシーの機能強化](~/ios/app-fundamentals/security-privacy.md)ガイドです。

## <a name="sirikit"></a>SiriKit

新しい 10、iOS に SiriKit が iOS デバイスで Siri を使用してユーザーがアクセスできるサービスを提供する Xamarin.iOS アプリを許可します。 この機能は、new を使用して 1 つまたは複数のアプリ拡張機能で提供**インテント**と**インテント UI**フレームワークです。

SiriKit には、次のサービスのドメインがサポートされています。

- オーディオまたはビデオ通話です。
- 素敵を予約します。
- ワークアウトを管理します。
- メッセージングです。
- 写真を検索しています。
- 送信または受信支払いを処理します。

ユーザーは、1 つのアプリ拡張機能のサービスに関連する Siri の要求を行う、SiriKit 送信、拡張機能、**インテント**サポート データと共に、ユーザーの要求を記述するオブジェクト。 アプリ拡張機能は、適切な生成**応答**オブジェクトを指定された**インテント**、拡張機能が、要求を処理する方法の詳細を示すです。

Siri は通常、すべてのユーザー操作を処理するときにアプリ拡張機能を使用できます、**インテント UI**豊富なカスタム ユーザー インターフェイスを機能させると、アプリのブランド化を提供するフレームワークと追加情報。

詳細については、次を参照してください、 [SiriKit 概要](~/ios/platform/sirikit/index.md)ガイドです。

## <a name="speech-recognition"></a>音声認識

iOS 10 には、テキストにアプリを継続的な音声認識をサポートし、(生または録画のオーディオ ストリーム) からの音声議事録の作成を許可する新しい音声 API が含まれています。

音声認識は、転送および Apple のサーバー、アプリケーションのデータの一時的なストレージが必要なため_必要があります_を含めることによって認識を実行するユーザーのアクセス許可を要求、`NSSpeechRecognitionUsageDescription`キーをその`Info.plist`ファイル サービスおよび通話、`SFSpeechRecognizer.RequestAutorization`メソッドです。

詳細については、次を参照してください、[音声認識機能の概要](~/ios/platform/speech.md)ガイドです。

## <a name="user-notifications"></a>ユーザーへの通知

初めて使用する iOS 10 では、ユーザー通知の配信とローカルおよびリモートの通知の処理のフレームワークを利用します。 このフレームワークを使用して、アプリまたはアプリ拡張機能をスケジュールできますローカルに関する通知の配信場所などの条件のセットまたは 1 日の時刻を指定することで。

さらに、アプリまたは拡張機能が表示される (および可能性のある変更できます) ローカルとリモートの両方の通知、ユーザーの iOS デバイスに配信されるようにします。

新しいユーザー通知の UI フレームワークが使用するは、アプリまたはアプリの拡張機能をユーザーに表示するときに、ローカルおよびリモートの両方の通知の外観をカスタマイズします。

詳細については、次を参照してください、[ユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)ガイドです。

## <a name="video-subscriber-account"></a>ビデオのサブスクライバーのアカウント

新しい ios 10 の場合、ビデオ サブスクライバー アカウント フレームワークにより、アプリそのストリーミング認証サポートまたはそのケーブルまたは衛星放送テレビ プロバイダーと、エンドユーザーの単一のサインイン エクスペリエンスを使用して認証するビデオ オンデマンドです。

## <a name="wide-color"></a>広色

iOS 10 では、拡張範囲のピクセル形式とコア グラフィックス、Core のイメージ、金属および AVFoundation などのフレームワークを含む、システム全体で全体の色域スペースのサポートを拡張します。 ワイド カラー ディスプレイを使用したデバイスのサポートはさらに、グラフィック スタック全体におけるこの動作を提供することにより容易になります。

さらに、 [UIKit](https://developer.xamarin.com/api/namespace/UIKit/)が変更された新しい機能を拡張**sRGB**大幅なパフォーマンスを失うことがなくワイド色域内の色を混在させるをやすくするための色空間です。

幅の色を扱うときに、Apple は次のベスト プラクティスを提供します。

- [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/)これで、使用、sRGB 領域の色およびしなくなります固定値を`0.0`に`1.0`範囲です。 アプリは、以前のクランプ動作に依存する場合は、iOS 10 に変更する必要があります。
- カスタムの実行時に、sRGB 色空間の描画の環境を構成するは`UIView`iPad Pro で描画します。
- アプリのカスタムの表示を実行する場合`UIImages`、使用して、新しい[UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer)拡張範囲または標準範囲の形式の使用を指定するクラス。
- コア グラフィックや金属などの低レベルの API を使用して、イメージ処理を提供する、開発者は、16 ビット浮動小数点値をサポートする拡張範囲色領域とピクセル形式が使用する必要があります。 必要に応じて、開発者がコンポーネントの色の値を手動で固定する必要があります。
- コア グラフィックス、Core のイメージとメタル パフォーマンス シェーダーは、2 つのカラー スペース間で変換するための新しいメソッドを提供します。

詳細については、次を参照してください、[広色の概要](~/ios/platform/wide-color.md)ガイドです。

## <a name="widget-enhancements"></a>ウィジェットの機能強化

Apple はウィジェットが優れた上にある新しい iOS 10 個のロック画面、色の背景に表示されるように、ウィジェットのシステムには拡張機能を導入しました。 [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect)プロパティは廃止されており、新しいに置き換えられました[WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect)または[WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect)プロパティです。 さらに、ウィジェットが含まれるよう、 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode)プロパティにより、開発者は、コンテンツの量が使用可能な説明を入力しを展開して、コンテンツを折りたたむことができます。

詳細については、次を参照してください、[検索とホーム画面ウィジェット機能強化](~/ios/platform/search/widgets.md)ガイドです。

## <a name="additional-framework-changes"></a>その他のフレームワークの変更

に加えて、主要なフレームワークの変更と追加機能の上に示した Apple 追加マイナー フレームワークの多くの変更に向けた iOS 10 です。

詳細については、次を参照してください、 [Framework の変更の追加](~/ios/platform/introduction-to-ios10/additional-framework-changes.md)ガイドです。

## <a name="deprecated-apis"></a>非推奨 API

次の Api が iOS 10 で廃止されました。

- `CKDiscoverAllContactsOperation`、 `CKDiscoveredUserInfo`、`CKDiscoverUserInfosOperation`と`CKFetchRecordChangesOperation`ios 10 CloudKit でクラスが削除されています。 使用して、 [CKDiscoverAllUserIdentitiesOperation](https://developer.xamarin.com/api/type/CloudKit.CKDiscoverUserIdentitiesOperation/)、 [CKUserIdentity](https://developer.xamarin.com/api/type/CloudKit.CKUserIdentity/)と[CKFetchRecordZoneChangesOperation](https://developer.xamarin.com/api/type/CloudKit.CKFetchRecordZoneChangesOperation/)クラス (レコードの共有をサポート) を代わりにします。
- いくつか[CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) (ゾーンおよびクエリ ベース サブスクリプション) などの Api は推奨されません。 使用して、 [CKRecordZoneSubscription](https://developer.xamarin.com/api/type/CloudKit.CKRecordZoneSubscription/)と[CKQuerySubscription](https://developer.xamarin.com/api/type/CloudKit.CKQuerySubscription/) Api 代わりにします。
- [NSPersistentStoreCoordnator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/)ユビキタス コンテンツに関連するシンボルが使用されなくなりました。
- `ADBannerView`、`ADInterstitialAd`内のシンボルを関連して、 [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/)クラスが廃止されました。
- [SKUniform](https://developer.apple.com/reference/spritekit/skuniform)浮動小数点値に関連付けられた記号が使用されなくなりました。
- `UILocalNotification`、 `UIMutableUserNotificationAction`、 `UIMutableUserNotificationCategory`、 `UIUserNotificationAction`、`UIUserNotificationCategory`と`UIUserNotificationSettings`UIKit のクラスが廃止されました。 使用して、[ユーザーへの通知](#User-Notifications)framework 代わりにします。
- `HandleActionForLocalNotification`、 `HandleActionForRemoteNotification`、`DidReceiveLocalNotification`と`DidReceiveRemoteNotification`WatchKit メソッドは廃止されます。 使用して、`HandleActionForNotification`と`DidReceiveNotification`メソッド代わりにします。
- `DidReceiveLocalNotification`と`DidReceiveRemoteNotification`のメソッド、 [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate)使用されなくなりました。 インスタンスを作成する[UNUserNotificationCenterDelegate](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate)を適切なメソッドを実装して、それを割り当てる、`Delegate`のプロパティ、 [UNUserNotificationCenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter)オブジェクト。
- **Game Center アプリ**は廃止され iOS から削除します。 アプリが GameKit を使用している場合、_必要があります_スコアボードなど GameKit 機能の表示に独自のインターフェイスを提供します。

Apple を参照してください[iOS 10.0 API の相違点を iOS 9.3](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html)機能廃止の完全な一覧についてはドキュメントです。



## <a name="related-links"></a>関連リンク

- [iOS 10 サンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [IOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
