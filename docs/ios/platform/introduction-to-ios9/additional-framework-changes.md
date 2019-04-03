---
title: 追加の iOS 9 フレームワークの変更
description: このドキュメントでは、iOS 9 で導入された追加のフレームワークの変更について説明します。 AVFoundation、AVKit、および CloudKit がについて説明します。
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 5156259f8178da69595464f75a10cd8f41965519
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870327"
---
# <a name="additional-ios-9-frameworks-changes"></a>追加の iOS 9 フレームワークの変更

_この記事では、追加、マイナー変更や iOS 9 の既存のフレームワークの機能強化について説明します。_

[![](additional-framework-changes-images/ios9-sml.png "iOS 9 のロゴ")](additional-framework-changes-images/ios9.png#lightbox)

IOS の大幅な変更を加え Apple は、iOS 9 の修正といくつかの既存のフレームワークの機能強化を行ったが。

## <a name="avfoundation-framework-additions"></a>AVFoundation フレームワークの追加

AVFoundation フレームワークで、 [AVSpeechSynthesisVoice](xref:AVFoundation.AVSpeechSynthesisVoice)クラスの言語に加えて言語識別子を使用して、音声を指定できるようになりました。

たとえば、次のコードは、すべての利用できる音声の一覧を取得します。

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

として設定して、一覧から、音声のいずれか、使用できます、`Voice`のインスタンスのプロパティ、 [AVSpeachUtterance](xref:AVFoundation.AVSpeechUtterance)クラス。

[AVQueuePlayer](xref:AVFoundation.AVQueuePlayer)クラスは、キュー内のインターネットのストリーミングとファイル ベースのメディアの混在をサポートするようになりました。 以前のバージョンは、同じ種類のキュー メディアのみでした。

詳細については、Apple を参照してください[AVSpeechSynthesisVoice 参照](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice)します。

## <a name="avkit-framework-additions"></a>AVKit フレームワークの追加

AVKit フレームワークを含む、新しい画像画像 (PIP) の新機能を使用する`AVPictureInPictureController`と[AVPlayerViewController](xref:AVKit.AVPlayerViewController)クラス。

- **AVPictureInPictureController** -このクラスは、iPad で浮動小数点、サイズ変更可能な PIP ウィンドウで、ビデオの再生を実行するユーザーに対応するアプリを iOS 9 を使用できます。
- **AVPlayerViewController** -管理、`AVPlayer`コント ローラーが iPad で浮動小数点、サイズ変更可能な PIP ウィンドウでビデオを再生するために使用します。

詳細についてを参照してください、 [iPad のマルチタス キング](~/ios/platform/introduction-to-ios9/index.md#multitasking)ドキュメントと Apple の[AVPictureInPictureController 参照](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController)と[AVPlayerViewController 参照](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>CloudKit の Web サービスの概要

CloudKit フレームワークでは、アプリケーションの開発に、そのアクセス iCloud が効率化します。 これには、アプリケーション データと資産の権利とアプリケーションの情報を安全に保管することの取得が含まれます。 このキットでは、個人情報を共有することがなく、iCloud Id とアプリケーションへのアクセスを許可することでユーザーの匿名性レイヤーを提供します。

新しい_CloudKit Web Services_フレームワークには、同じ CloudKit ベースのデータと、Xamarin.iOS アプリとコンテンツへのアクセスを提供する web サイトに組み込むことができる JavaScript ライブラリ (CloudKit JS) が用意されています。

> [!IMPORTANT]
> アクセスまたは提示 CloudKit を CloudKit JS を使用してデータベースからコンテンツを更新することができます、前にする必要がありますが以前に定義データベースのスキーマ。




詳細については、次のドキュメントを参照してください。

- [CloudKit の概要](~/ios/data-cloud/intro-to-cloudkit.md)-CloudKit を使用して Xamarin.iOS アプリに導入します。
- [CloudKit のクイック スタート](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987)-Apple の CloudKit の概要。
- [CloudKit JS 参照](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359)-Apple の CloudKit JS ドキュメント。
- [Web サービス参照の CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240) -CloudKit の HTTP インターフェイスを記述する Apple の参照。
- [CloudKit カタログ:(Cocoa および JavaScript) CloudKit の概要](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599)-CloudKit と CloudKit JS を使用して Apple のサンプル アプリです。

> [!IMPORTANT]
> Apple からは、開発者が欧州連合の一般データ保護規則 (GDPR) を適切に処理するための[ツールが提供](https://developer.apple.com/support/allowing-users-to-manage-data/)されています。

## <a name="foundation-framework-additions"></a>Foundation フレームワークの追加

Apple には、iOS 9 に Foundation フレームワークには、次の変更が含まれます。

### <a name="changes-to-nsbundle"></a>NSBundle への変更

次の変更を加え、 [NSBundle](xref:Foundation.NSBundle) iOS 9 のクラス。

* `GetPreservationPriorityForTag (NSString tag)` -特定のタグを持つリソースの現在の保持の優先順位を取得します。 有効な値が、範囲内にある`0.0`に`1.0`、優先度の低いリソースが最初に削除されます。
* `SetPreservationPriorityForTag (double priority, NSSet tags)` -特定のタグによるリソースの現在の保持の優先順位を設定します。 有効な値が、範囲内にある`0.0`に`1.0`、優先度の低いリソースが最初に削除されます。

詳細については、Apple を参照してください[NSBundle 参照](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle)します。

### <a name="changes-to-nsprocessinfo"></a>NSProcessInfo への変更

IOS デバイスで実行されている各プロセスが 1 つの_プロセス情報エージェント_(PIA)。 使用して、 [NSProcessInfo](xref:Foundation.NSProcessInfo) PIA とコントロールの現在の能力と、特定のプロセスの温度管理に関する情報を提供するクラス。

たとえば、プロセスの自動終了を制御するには、次のコードを使用することができます。

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

詳細については、Apple を参照してください[NSProcessInfo 参照](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo)します。

### <a name="reacting-to-low-power-mode"></a>省電力モードへの対応

使用して、`LowPowerModeEnabled`のプロパティ、 [NSProcessInfo](xref:Foundation.NSProcessInfo)クラスで、アプリが実行されている iOS デバイスで、低電力モードが有効にされているかどうかを決定します。 例:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>HealthKit フレームワークの変更

Apple には、次の変更が含まれている、 [HealthKit](xref:HealthKit) iOS 9 フレームワーク。

- 一括削除と HealthKit データベース内のエントリの削除の追跡をサポートします。 Apple を参照してください。 [HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject)、 [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery)と[HKHealthStore クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708)詳細についてはします。
- 新しい追跡カテゴリと特性が追加されました、`HKQuantityTypeIdentifier`クラス (など`UVExposure`) と、`HKCategoryTypeIdentifier`クラス (など`OvulationTestResult`)。 Apple を参照してください。 [HealthKit の定数参照](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html#//apple_ref/doc/uid/TP40014710)詳細についてはします。

参照してください、 [HealthKit の概要](~/ios/platform/healthkit.md)Xamarin.iOS で HealthKit の使用の詳細についてはドキュメントです。

## <a name="local-authentication-framework-changes"></a>ローカル認証フレームワークの変更

Apple には、次の変更が含まれている、[ローカル認証](xref:LocalAuthentication)iOS 9 フレームワーク。

- 使用して、`EvaluateAccessControl`と`EvaluatePolicy`のメソッド、 [LAContext](xref:LocalAuthentication.LAContext)クラスできるようになりましたロック解除に成功した前から Touch ID と一致する再利用しようとします。
- 現在登録されている本の指の一覧を取得する機能。
- 指を追加または認証から削除されたときの追跡をサポートします。
- 使用できる_認証コンテキスト_キーチェーンの呼び出しでキーチェーン アクセス制御を評価するためのサポートを一覧表示します。
- コードからユーザー プロンプトをキャンセルする機能。

参照してください、 [Touch ID の概要](~/ios/platform/touchid.md)Xamarin.iOS で Touch ID を操作する方法の詳細についてはドキュメントです。

### <a name="lacontext-changes"></a>LAContext 変更

次の変更を加え、 [LAContext](xref:LocalAuthentication.LAContext) iOS 9 のクラス。

- **TouchIdAuthenticationMaximumAllowableReuseDuration** -最大タッチ ID の認証を再利用可能な時間を返します。
- **EvaluatedPolicyDomainState** - を取得または評価のポリシーの状態を設定します。
- **MaxBiometryFailures** -iOS 9 の償却されています。
- **TouchIdAuthenticationAllowableReuseDuration** を取得またはタッチ ID の認証を再利用可能な時間を設定します。
- **EvaluateAccessControl** - 非同期的に認証ポリシーを評価します。
- **無効になる**-特定のタッチ ID の認証を無効になります。
- **IsCredentialSet** -返します`true`資格情報が現在設定されている場合。
- **SetCredentialType**指定された資格情報の種類を設定します。

Apple を参照してください[LAContext 参照](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:)の詳細。

## <a name="mapkit-framework-changes"></a>MapKit フレームワークの変更

Apple には、次の変更が含まれている、 [MapKit](xref:MapKit) iOS 9 フレームワーク。

- MapKit が今すぐ転送の方向に直接マップ アプリを起動するため、転送中の到着 (予定) を使用して推定時間を照会するためにサポートが提供、 [MKLaunchOptions](xref:MapKit.MKLaunchOptions)と[MKDirections](xref:MapKit.MKLaunchOptions)クラス。
- MapKit によって返される検索結果と[CLGeocoder](xref:CoreLocation.CLGeocoder)クラスは、結果のタイム ゾーンも提供できます。
- マップの注釈を使用して iOS アプリによって提示されるを完全にカスタマイズできるようになりました、`DetailCalloutAccessoryView`のプロパティ、 [MKAnnotationView](xref:MapKit.MKAnnotationView)クラス。

参照してください、 [iOS マップ](~/ios/user-interface/controls/ios-maps/index.md)と[チュートリアル - 注釈とオーバーレイ MapKit で探索](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md)マップと Xamarin.iOS および Appleで注釈を操作する方法の詳細についてはマニュアル[CLGeocoder 参照](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder)詳細についてはします。

## <a name="passkit-framework-additions"></a>PassKit Framework の追加

Apple には、次の変更が含まれている、 [PassKit](xref:PassKit) iOS 9 フレームワーク。

- ストア借方と Discover カードと共にクレジット カードの両方に Apple Pay なりました。 参照してください、**支払ネットワーク**の Apple の「 [PKPaymentRequest クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832)詳細についてはします。
- Xamarin.iOS アプリ内で直接追加できますカード発行者と支払いネットワーク Apple Pay にします。 Apple を参照してください。 [PKAddPaymentPassViewController クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116)の詳細。

参照してください、 [PassKit の概要](~/ios/platform/passkit.md)PassKit Xamarin.iOS での操作の詳細についてはドキュメントです。

## <a name="safari-services-framework-additions"></a>Safari Services Framework の追加

Apple には、次の変更が含まれている、 [Safari サービス](xref:SafariServices)iOS 9 フレームワーク。

- 使用できるように、新しい[SFSafariViewController](xref:SafariServices.SFSafariViewController) Xamarin.iOS アプリ内で web コンテンツを表示するクラス。 Safari のアプリと web サイト データと cookie を共有する機能を提供し、いくつかのリーダーとオートコンプリート) などの Safari の機能が含まれています。 [SFSafariViewController](xref:SafariServices.SFSafariViewController)機能、**完了**web コンテンツの表示が完了したら、アプリにユーザーが戻るボタン。

[SFSafariViewController](xref:SafariServices.SFSafariViewController)テーラード フィット クラスの 1 つの web コンテンツのページを表示する、いずれかを置換するために使用する必要があります[WKWebKit](xref:WebKit.WKWebView)または[UIWebView](xref:UIKit.UIWebView)、既存の Xamarin.iOS アプリ内のコントロール。

### <a name="displaying-a-website"></a>Web サイトを表示します。

次のコードは、呼び出し元の例を[SFSafariViewController](xref:SafariServices.SFSafariViewController)から別のビュー コント ローラーの内部。

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>UIKit フレームワークの変更

Apple での複数の要素に多くの機能強化が含まれている、 [UIKit](xref:UIKit) ios 9 フレームワーク。 次のセクションでは、これらの変更について詳しく説明します。

### <a name="3d-touch-events"></a>3D タッチ イベント

IOS 9 と iPhone 6 s および iPhone 6 s 新しい 3D Touch さらに、iOS アプリに負荷の機密性の高いジェスチャを追加します。 その結果、アプリが iOS 9 (またはそれ以上) で実行されている iOS デバイスが 3D タッチをサポートできる場合は、負荷の変更により、`TouchesMoved`イベントが発生します。

この動作の変更のための iOS アプリを準備する必要があります、`TouchesMoved`より多くの場合、呼び出されるイベント場合でも、X Y 座標が変更されていない/。

詳細についてを参照してください、 [3D Touch 概要](~/ios/platform/3d-touch.md)ガイド。

### <a name="document-open-in-place-functionality"></a>ドキュメントを開くインプレース機能

いずれかを使用して、`FinishedLaunching (application, launchOptions)`または`WillFinishLaunching (Application, launchOptions)`のメソッド、 [UIApplicationDelegate](xref:UIKit.UIApplicationDelegate)クラス、ドキュメントを開くしてインプレース (作業コピー) ではなく変更することができますようになりました。

新しいオープン インプレース機能をサポートするために追加、 `LSSupportsOpeningDocumentsInPlace` Xamarin.iOS アプリのキー **Info.plist**の値を持つファイル`YES`します。

Apple を参照してください[UIApplicationDelegate 参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate)の詳細。

### <a name="enhanced-touch-events"></a>強化されたタッチ イベント

Apple では、iOS 9 にタッチ イベントにいくつかの機能強化を用意います。 タッチ予測を使用して表示の更新間隔の中間調整へのアクセスを取得する機能が含まれます。

Apple を参照してください[iOS 用のイベント処理ガイド](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html)の詳細。

### <a name="fetching-tailored-content"></a>調整されたコンテンツをフェッチしています

新しい`NSDataAsset`クラスは、メモリおよび iOS デバイスで現在実行中のグラフィックス機能へのコンテンツをフェッチする Xamarin.iOS アプリを使用できます。

### <a name="new-layout-anchors"></a>新しいレイアウトのアンカー

新しい`NSLayoutAnchor`と`NSLayoutDimension`レイアウト アンカー クラスの新しいアンカーのプロパティを使用、 [UIView](xref:UIKit.UIView)クラス (など`LeadingAnchor`と`WidthAnchor`) iOS 9 のレイアウトを容易にします。

参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)オートとサイズ クラスで Xamarin.iOS アプリと Apple の操作の詳細についてはドキュメント[NSLayoutAnchor 参照](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor)、 [NSLayoutDimension 参照](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension)と[UIView 参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView)詳細についてはします。

### <a name="new-readable-content-margins"></a>新しい読み取り可能なコンテンツの余白

新しい`UILayoutGuide`読み取り可能なコンテンツの余白を提供し、ビュー内のコンテンツの描画領域を定義するクラスを使用できます。 Apple を参照してください。 [UILayoutGuide 参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide)詳細についてはします。

### <a name="text-input-in-notifications-modifications"></a>変更の通知のテキスト入力

[UIUserNotificationAction](xref:UIKit.UIUserNotificationAction)クラスには、新しい`Behavior`通知からのテキスト入力をサポートするために使用できるプロパティです。

### <a name="uiapplicationdelegate-changes"></a>UIApplicationDelegate 変更

すべての呼び出しを置き換える提案、Apple によっていない正式に非推奨、中に、`FinishedLaunching (UIApplication application)`のメソッド、 [UIApplicationDelegate](xref:UIKit.UIApplicationDelegate)クラスを`FinishedLaunching (UIApplication application, NSDictionary launchOptions)`または`WillFinishLaunching (UIApplication application, NSDictionary launchOptions)`メソッド。

Apple を参照してください[UIApplicationDelegate 参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate)の詳細。

### <a name="uikit-dynamics-changes"></a>UIKit Dynamics の変更

Apple には、iOS 9 の UIKit Dynamics に次の変更が含まれています。

- これで、dynamics は、四角形以外の衝突の境界のサポートをします。
- 新しい、カスタマイズ可能な`UIFieldBehavior`クラスは、さまざまなフィールドの種類をサポートするために使用します。
- その他の添付ファイルの種類に追加された、`UIAttachmentBehavior`クラス。

Apple を参照してください[UIAttachment 参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior)の詳細。

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView と UIDatePicker の変更

IOS 9 の前に、 [UIPickerView](xref:UIKit.UIPickerView)と[UIDatePicker](xref:UIKit.UIDatePicker)コントロール、サイズを変更され、(通常は、幅、アプリが iOS デバイスのコンテナーの幅に合わせて自動的にサイズを変更実行されている)。

Ios 9 でこの自動サイズ変更が発生しないことと、コントロールを画面のサイズと向きに関係なく、すべての iOS デバイスで 320 のポイントの幅にレンダリングされます。

この問題を解決するには、親コンテナー (ビュー) の端にコントロールの幅をピン留めし、必要な高さを指定する自動レイアウトとサイズ クラスを使用します。 参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)Xamarin.iOS アプリで自動レイアウトとサイズ クラスの使用の詳細についてはドキュメントです。

### <a name="new-uitextinputassistantitem-class"></a>新しい UITextInputAssistantItem クラス

使用して、新しい`UITextInputAssistantItem`クラスのレイアウト バー ボタンのグループに、_ショートカット バー_します。 ショートカット バーは、入力のショートカットを提供するソフト キーボードで使用できる新しい領域です。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [IOS 9 の概要](~/ios/platform/introduction-to-ios9/index.md)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 を新します。](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
