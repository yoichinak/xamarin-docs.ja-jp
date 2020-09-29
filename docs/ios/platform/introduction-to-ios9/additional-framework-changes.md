---
title: IOS 9 フレームワークのその他の変更
description: このドキュメントでは、iOS 9 で導入された追加のフレームワーク変更について説明します。 AVFoundation、Avfoundation、および CloudKit について説明します。
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 8a4b1b5ed3089cb7044850cf19629fa063e43703
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436347"
---
# <a name="additional-ios-9-frameworks-changes"></a>IOS 9 フレームワークのその他の変更

_この記事では、iOS 9 用の既存のフレームワークの追加、軽微な変更、強化された機能について説明します。_

[![iOS 9 ロゴ](additional-framework-changes-images/ios9-sml.png)](additional-framework-changes-images/ios9.png#lightbox)

IOS に加えられた主な変更に加えて、Apple は、iOS 9 の既存の複数のフレームワークに対して変更や機能強化を加えました。

## <a name="avfoundation-framework-additions"></a>AVFoundation フレームワークの追加

AVFoundation framework では、 [AVSpeechSynthesisVoice](xref:AVFoundation.AVSpeechSynthesisVoice) クラスを使用して、言語に加えて、識別子で音声を指定できるようになりました。

たとえば、次のコードは、使用可能なすべての音声の一覧を取得します。

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

次に、 `Voice` [AVSpeachUtterance](xref:AVFoundation.AVSpeechUtterance) クラスのインスタンスのプロパティとして設定することで、一覧のいずれかの音声を使用できます。

[Avqueueplayer](xref:AVFoundation.AVQueuePlayer)クラスは、キュー内のインターネットストリーミングとファイルベースのメディアを組み合わせてサポートするようになりました。 以前のバージョンでは、同じ種類のメディアのみをキューに置いていました。

詳細については、Apple の [AVSpeechSynthesisVoice リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice)を参照してください。

## <a name="avkit-framework-additions"></a>AVKit フレームワークの追加

新しい画像形式 (PIP) 機能を使用するために、AVKit フレームワークには、new `AVPictureInPictureController` クラスと [AVPlayerViewController](xref:AVKit.AVPlayerViewController) クラスが含まれています。

- **Avピクチャ Inピクチャコントローラー** -このクラスを使用すると、iOS 9 アプリはユーザーに応答して、iPad 上のサイズ変更可能なフローティング状態の PIP ウィンドウでビデオの再生を開始することができます。
- **AVPlayerViewController** - `AVPlayer` iPad 上のサイズ変更可能なフローティング状態の PIP ウィンドウにビデオを表示するために使用されるコントローラーを管理します。

詳細については、 [iPad のドキュメント用のマルチタスキングに](~/ios/platform/introduction-to-ios9/index.md#multitasking) 関するドキュメントと Apple の [Av画像 inピクチャコントローラーのリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) と [AVPlayerViewController リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController)を参照してください。

## <a name="introducing-cloudkit-web-services"></a>CloudKit Web サービスの概要

CloudKit フレームワークを使用すると、iCloud にアクセスするアプリケーションの開発が効率化されます。 これには、アプリケーションのデータと資産の権限の取得だけでなく、アプリケーションの情報を安全に格納できることも含まれます。 このキットは、個人情報を共有せずに iCloud Id を使用してアプリケーションにアクセスできるようにすることで、ユーザーに対して匿名のレイヤーを提供します。

新しい _Cloudkit Web サービス_ フレームワークには、web サイトに組み込むことができる JavaScript ライブラリ (CLOUDKIT JS) が用意されています。これを使用すると、Xamarin iOS アプリと同じ cloudkit ベースのデータとコンテンツにアクセスできます。

> [!IMPORTANT]
> CloudKit JS を使用して CloudKit データベースのコンテンツにアクセスしたり、表示したり、更新したりするには、そのデータベースのスキーマを事前に定義しておく必要があります。

詳細については、次のドキュメントを参照してください。

- [CloudKit の概要](~/ios/data-cloud/intro-to-cloudkit.md) -Xamarin iOS アプリでの cloudkit の使用について説明します。
- [Cloudkit クイックスタート](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) -Apple が cloudkit を紹介します。
- [CLOUDKIT Js リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) -Apple の cloudkit js ドキュメント。
- [Cloudkit カタログ: cloudkit (Cocoa および JavaScript) の概要](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) -CloudKit と CLOUDKIT JS を使用した Apple のサンプルアプリ。

> [!IMPORTANT]
> Apple からは、開発者が欧州連合の一般データ保護規則 (GDPR) を適切に処理するための[ツールが提供](https://developer.apple.com/support/allowing-users-to-manage-data/)されています。

## <a name="foundation-framework-additions"></a>Foundation フレームワークの追加

Apple では、iOS 9 の Foundation framework に次の変更が加えられています。

### <a name="changes-to-nsbundle"></a>NSBundle の変更

IOS 9 の [Nsbundle](xref:Foundation.NSBundle) クラスには、次の変更が加えられました。

- `GetPreservationPriorityForTag (NSString tag)` -指定されたタグを持つリソースの現在の保存優先度を取得します。 有効な値はからの範囲内で `0.0` `1.0` 、最も低い優先順位のリソースが最初に削除されます。
- `SetPreservationPriorityForTag (double priority, NSSet tags)` -指定されたタグを持つリソースの現在の保存優先度を設定します。 有効な値はからの範囲内で `0.0` `1.0` 、最も低い優先順位のリソースが最初に削除されます。

詳細については、「Apple の [Nsbundle リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle)」を参照してください。

### <a name="changes-to-nsprocessinfo"></a>NSProcessInfo の変更

IOS デバイスで実行されている各プロセスには、 _プロセス情報エージェント_ (PIA) が1つあります。 [Nsprocessinfo](xref:Foundation.NSProcessInfo)クラスを使用して、特定のプロセスの現在の PIA に関する情報を提供し、電源と温度の管理を制御します。

たとえば、プロセスの自動終了を制御するには、次のコードを使用します。

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

詳細については、Apple の [Nsprocessinfo リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo)を参照してください。

### <a name="reacting-to-low-power-mode"></a>低電力モードへの対応

`LowPowerModeEnabled` [Nsprocessinfo](xref:Foundation.NSProcessInfo)クラスのプロパティを使用して、アプリが実行されている IOS デバイスで低電力モードが有効になっているかどうかを確認します。 次に例を示します。

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>HealthKit Framework の変更

Apple では、iOS 9 の [HealthKit](xref:HealthKit) フレームワークに次の変更が加えられています。

- HealthKit データベース内のエントリの一括削除と削除の追跡をサポートします。 詳細については、Apple の [Hkdeletedobject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject)、 [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) 、および [HKHealthStore クラスリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) を参照してください。
- `HKQuantityTypeIdentifier`クラス (など) `UVExposure` や `HKCategoryTypeIdentifier` クラス (など) に、新しい追跡カテゴリと特性が追加されました `OvulationTestResult` 。 

Xamarin. iOS での HealthKit の使用方法の詳細については、 [HealthKit の概要に](~/ios/platform/healthkit.md) 関するドキュメントを参照してください。

## <a name="local-authentication-framework-changes"></a>ローカル認証フレームワークの変更

Apple では、iOS 9 の [ローカル認証](xref:LocalAuthentication) フレームワークに次の変更が加えられています。

- `EvaluateAccessControl` `EvaluatePolicy` [LAContext](xref:LocalAuthentication.LAContext)クラスのメソッドとメソッドを使用して、前回成功したロック解除試行とのタッチ ID 一致を再利用できるようになりました。
- 現在登録されている指の一覧を取得する機能。
- 指が認証に追加または削除されたときの追跡のサポート。
- キーチェーンの呼び出しと、キーチェーンアクセス制御リストの評価をサポートする _認証コンテキスト_ を使用する機能。
- コードからユーザープロンプトをキャンセルする権限。

詳細については、「 [Xamarin. iOS でのタッチ id と顔 id](~/ios/platform/touch-id-face-id.md)」を参照してください。

### <a name="lacontext-changes"></a>LAContext の変更

IOS 9 の [LAContext](xref:LocalAuthentication.LAContext) クラスには、次の変更が加えられました。

- **TouchIdAuthenticationMaximumAllowableReuseDuration** -タッチ ID 認証を再利用できる最長時間を返します。
- **EvaluatedPolicyDomainState** -評価されたポリシーの状態を取得または設定します。
- **MaxBiometryFailures** -iOS 9 では、減価償却が行われています。
- **TouchIdAuthenticationAllowableReuseDuration** Touch ID 認証を再利用できる時間を取得します。値の設定も可能です。
- **EvaluateAccessControl** -認証ポリシーを非同期に評価します。
- **無効** -特定のタッチ ID 認証を無効にします。
- **Iscredentialset** - `true` 資格情報が現在設定されている場合は、を返します。
- **Setcredentialtype** 指定された資格情報の種類を設定します。

詳細については、Apple の [LAContext のリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) を参照してください。

## <a name="mapkit-framework-changes"></a>MapKit フレームワークの変更点

Apple では、iOS 9 の [Mapkit](xref:MapKit) フレームワークに次の変更が加えられています。

- MapKit は、転送方向にマップアプリを直接起動し、 [Mklaunchoptions](xref:MapKit.MKLaunchOptions) および [mk道順](xref:MapKit.MKLaunchOptions) クラスを使用して到着の推定所要時間 (ETA) を照会するためのサポートを提供するようになりました。
- MapKit と [Clgeocoder](xref:CoreLocation.CLGeocoder) クラスによって返される検索結果では、結果のタイムゾーンを指定することもできます。
- `DetailCalloutAccessoryView` [MKAnnotationView](xref:MapKit.MKAnnotationView)クラスのプロパティを使用して、iOS アプリによって提示されたマップの注釈を完全にカスタマイズできるようになりました。

Xamarin でマップと注釈を操作する方法の詳細については、「 [Ios マップ](~/ios/user-interface/controls/ios-maps/index.md) と [チュートリアル-mapkit での注釈とオーバーレイの調査](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) 」を参照してください。詳細については、Ios と Apple の [clgeocoder リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) を参照してください。

## <a name="passkit-framework-additions"></a>Pass Kit フレームワークの追加

Apple では、iOS 9 の [Pass kit](xref:PassKit) フレームワークに次の変更が加えられています。

- Apple Pay では、店舗のデビットカードとクレジットカード、および検出カードの両方がサポートされるようになりました。 詳細については、Apple の[PKPaymentRequest クラスリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832)の「**支払いネットワーク**」セクションを参照してください。
- Xamarin. iOS アプリ内で直接、支払いネットワークとカード発行者を Apple Pay に追加できるようになりました。 詳細については、「Apple の [PKAddPaymentPassViewController クラスリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) 」を参照してください。

Xamarin. iOS での Pass Kit の使用方法の詳細については、「 [Pass kit](~/ios/platform/passkit.md) のドキュメントの概要」を参照してください。

## <a name="safari-services-framework-additions"></a>Safari サービスフレームワークの追加

Apple では、iOS 9 の [Safari サービス](xref:SafariServices) フレームワークに次の変更が加えられています。

- 新しい [SFSafariViewController](xref:SafariServices.SFSafariViewController) クラスを使用して、Xamarin. iOS アプリ内に web コンテンツを表示できるようになりました。 Safari アプリで web サイトのデータと cookie を共有でき、Safari の機能 (閲覧者やオートフィルなど) がいくつか含まれています。 [SFSafariViewController](xref:SafariServices.SFSafariViewController) は、web コンテンツの表示が終了したときにユーザーをアプリに返す [ **完了** ] ボタンを備えています。

[SFSafariViewController](xref:SafariServices.SFSafariViewController)クラスは、web コンテンツの1ページを表示するように調整されているため、既存の Xamarin. iOS アプリ内の[WKWebKit](xref:WebKit.WKWebView)または[uiwebview](xref:UIKit.UIWebView)コントロールを置き換えるために使用することを検討してください。

### <a name="displaying-a-website"></a>Web サイトの表示

次のコードは、別のビューコントローラー内から [SFSafariViewController](xref:SafariServices.SFSafariViewController) を呼び出す例です。

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>UIKit フレームワークの変更

Apple には、iOS 9 用の [Uikit](xref:UIKit) フレームワークのいくつかの要素に対する多くの機能強化が含まれています。 以下のセクションでは、これらの変更について詳しく説明します。

### <a name="3d-touch-events"></a>3D タッチイベント

IOS 9 および iPhone 6s と iPhone 6s Plus の新機能である3D タッチにより、負荷の高いジェスチャが iOS アプリに追加されます。 その結果、アプリが iOS 9 (またはそれ以降) で実行されていて、iOS デバイスが3D タッチをサポートできる場合、負荷の変化によって `TouchesMoved` イベントが発生します。

この動作の変更のため、 `TouchesMoved` X/Y 座標が変更されていない場合でも、イベントがより頻繁に呼び出されるように iOS アプリを準備する必要があります。

詳細については、「 [3D タッチ](~/ios/platform/3d-touch.md) ガイドの概要」を参照してください。

### <a name="document-open-in-place-functionality"></a>ドキュメントのオープンインプレース機能

`FinishedLaunching (application, launchOptions)` `WillFinishLaunching (Application, launchOptions)` [Uiapplicationdelegate](xref:UIKit.UIApplicationDelegate)クラスのメソッドまたはメソッドを使用することにより、ドキュメントを開いて (コピーで作業するのではなく) 変更できるようになりました。

新しいオープンインプレース機能をサポートするには、 `LSSupportsOpeningDocumentsInPlace` Xamarin. iOS アプリの**情報**ファイルにキーを追加します。このファイルには、という値を指定します。 `YES`

詳細については、Apple の [Uiapplicationdelegate のリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) を参照してください。

### <a name="enhanced-touch-events"></a>強化されたタッチイベント

Apple では、iOS 9 のタッチイベントに対していくつかの機能強化が行われています。 これには、タッチ予測を使用したり、表示の更新間の中間のタッチにアクセスしたりする機能が含まれます。

詳細については、 [iOS 用の Apple のイベント処理ガイド](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) を参照してください。

### <a name="fetching-tailored-content"></a>調整したコンテンツを取得する

新しいクラスを使用すると、 `NSDataAsset` Xamarin ios アプリは、現在実行されている iOS デバイスのメモリおよびグラフィック機能に合わせて調整されたコンテンツを取得できます。

### <a name="new-layout-anchors"></a>新しいレイアウトアンカー

新しい `NSLayoutAnchor` と `NSLayoutDimension` レイアウトのアンカークラスは、 [uiview](xref:UIKit.UIView) クラス (やなど) の新しいアンカープロパティと連携して、 `LeadingAnchor` `WidthAnchor` iOS 9 でのレイアウトを簡単にすることができます。

詳細については、統合された [ストーリーボードの概要に](~/ios/user-interface/storyboards/unified-storyboards.md) 関するドキュメントを参照してください。 iOS アプリと Apple の [nslayoutanchor のリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor)、 [nslayoutanchor リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) 、および [uiview リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) の詳細については、「統合されたストーリーボードの概要」を参照してください。

### <a name="new-readable-content-margins"></a>新しい読み取り可能なコンテンツの余白

新しい `UILayoutGuide` クラスを使用すると、読み取り可能なコンテンツの余白を提供し、ビュー内のコンテンツの描画領域を定義できます。 詳細については、「Apple の [UILayoutGuide リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) 」を参照してください。

### <a name="text-input-in-notifications-modifications"></a>通知の変更時のテキスト入力

[Uiusernotificationaction](xref:UIKit.UIUserNotificationAction)クラスには、 `Behavior` 通知からのテキスト入力をサポートするために使用できる新しいプロパティがあります。

### <a name="uiapplicationdelegate-changes"></a>UIApplicationDelegate の変更

Apple では正式に非推奨とされていますが、 `FinishedLaunching (UIApplication application)` [Uiapplicationdelegate](xref:UIKit.UIApplicationDelegate) クラスのメソッドへのすべての呼び出しを `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` メソッドまたはメソッドに置き換えることをお勧めし `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` ます。

詳細については、Apple の [Uiapplicationdelegate のリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) を参照してください。

### <a name="uikit-dynamics-changes"></a>UIKit Dynamics の変更

Apple では、iOS 9 の UIKit Dynamics に次の変更が加えられています。

- Dynamics では、四角形を使用しない衝突境界がサポートされるようになりました。
- 新しいカスタマイズ可能 `UIFieldBehavior` なクラスは、さまざまなフィールドの種類をサポートするために使用されます。
- 追加の添付ファイルの種類がクラスに追加されました `UIAttachmentBehavior` 。

詳細については、Apple の [Uiattachment のリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) を参照してください。

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView と UIDatePicker の変更

IOS 9 より前では、 [UIPickerView](xref:UIKit.UIPickerView) および [UIDatePicker](xref:UIKit.UIDatePicker) コントロールのサイズは変更できませんでした。コンテナーの幅に合わせて自動的にサイズが変更されます (通常、アプリが実行されていた iOS デバイスの幅)。

IOS 9 では、この自動サイズ変更は発生せず、画面のサイズと向きに関係なく、すべての iOS デバイスで、コントロールは320ポイントの幅でレンダリングされます。

このような状況を修正するには、Auto Layout クラスと Size クラスを使用して、コントロールの幅を親コンテナー (ビュー) の端に固定し、必要な高さを指定します。 Xamarin iOS アプリでの自動レイアウトクラスとサイズクラスの使用の詳細については、「統合された [ストーリーボードの概要」を](~/ios/user-interface/storyboards/unified-storyboards.md) 参照してください。

### <a name="new-uitextinputassistantitem-class"></a>New UITextInputAssistantItem クラス

新しいクラスを使用して、 `UITextInputAssistantItem` _ショートカットバー_のレイアウトバーボタングループにします。 ショートカットバーは、ソフトキーボードで入力ショートカットを提供するために使用できる新しい領域です。

## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS9)
- [iOS 9 の概要](~/ios/platform/introduction-to-ios9/index.md)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 の新機能](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)