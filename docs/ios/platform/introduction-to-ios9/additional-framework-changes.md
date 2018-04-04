---
title: その他の iOS 9 のフレームワークの変更
description: この記事では、追加、マイナーの変更や iOS 9 の既存のフレームワークの機能強化について説明します。
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 5561cccfd0968c309526aae1e5dc90831ca681b4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="additional-ios-9-frameworks-changes"></a>その他の iOS 9 のフレームワークの変更

_この記事では、追加、マイナーの変更や iOS 9 の既存のフレームワークの機能強化について説明します。_

[![](additional-framework-changes-images/ios9-sml.png "iOS 9 のロゴ")](additional-framework-changes-images/ios9.png#lightbox)

IOS に主要な変更だけでなく Apple 修正といくつかの既存のフレームワークの機能強化に向けた iOS 9 です。

## <a name="av-foundation-framework-additions"></a>AV Foundation フレームワークの追加

AV Foundation framework、 [AVSpeechSynthesisVoice](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechSynthesisVoice/)クラスだけでなく言語識別子によって、音声を指定できるようになりました。

たとえば、次のコードはすべて利用できる音声の一覧を取得します。

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

行うこともできますし、一覧から音声のいずれかのように設定して、`Voice`のインスタンスのプロパティ、 [AVSpeachUtterance](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechUtterance/)クラスです。

[AVQueuePlayer](https://developer.xamarin.com/api/type/AVFoundation.AVQueuePlayer/)クラスは、キューにインターネットのストリーミングとファイル ベースのメディアの組み合わせをサポートするようになりました。 以前のバージョンでは、同じ種類のキュー メディアのみ可能性があります。

詳細については、Apple を参照してください[AVSpeechSynthesisVoice 参照](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice)です。

## <a name="avkit-framework-additions"></a>AVKit フレームワークの追加

画像に画像 (PIP) の新しい機能を使用する AVKit フレームワークが含まれています、新しい`AVPictureInPictureController`と[AVPlayerViewController](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/)クラス。

- **AVPictureInPictureController** -このクラスは、iOS 9 iPad で浮動小数点、サイズ変更可能な PIP ウィンドウでビデオの再生を実行するユーザーに応答するアプリを使用します。
- **AVPlayerViewController** -管理、 `AVPlayer` iPad で浮動小数点、サイズ変更可能な PIP ウィンドウでビデオを表示するためにコント ローラー。

詳細についてを参照してください、 [iPad のマルチタスク](~/ios/platform/introduction-to-ios9/index.md#multitasking)ドキュメントと Apple の[AVPictureInPictureController 参照](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController)と[AVPlayerViewController 参照](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>CloudKit Web サービスの概要

CloudKit フレームワークは、そのアクセス iCloud アプリケーションの開発を簡略化します。 これには、アプリケーション データと資産の権限だけでなくアプリケーションの情報を安全に保管することの取得が含まれます。 このキットは、個人情報を共有せず、iCloud Id を持つアプリケーションへのアクセスを許可することで、匿名のレイヤーにユーザーを表示します。

新しい_CloudKit Web Services_ framework には、同じベース CloudKit データと、Xamarin.iOS アプリとのコンテンツへのアクセスを提供する web サイトに組み込むことができる JavaScript ライブラリ (CloudKit JS) が用意されています。

> [!IMPORTANT]
> アクセスまたは提示 CloudKit を CloudKit JS を使用してデータベースからコンテンツを更新することができます、前にする必要がありますが以前に定義したそのデータベースのスキーマです。




詳細については、次のドキュメントを参照してください。

- [CloudKit 概要](~/ios/data-cloud/intro-to-cloudkit.md)-Xamarin.iOS アプリで CloudKit を使用して導入します。
- [クイック スタートの CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) -CloudKit の Apple の概要です。
- [CloudKit JS 参照](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359)-Apple の CloudKit JS ドキュメントを参照します。
- [Web サービス参照の CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240) -CloudKit HTTP インターフェイスを記述する Apple の参照。
- [: CloudKit カタログの概要 (Cocoa および JavaScript) CloudKit](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) -CloudKit および CloudKit JS を使用して、Apple のサンプル アプリ。

## <a name="foundation-framework-additions"></a>Foundation フレームワークの追加

Apple には、iOS 9 に Foundation フレームワークには、次の変更が含まれます。

### <a name="changes-to-nsbundle"></a>NSBundle への変更

次の変更が加えられた、 [NSBundle](https://developer.xamarin.com/api/type/Foundation.NSBundle/) iOS 9 のクラス。

* `GetPreservationPriorityForTag (NSString tag)` -特定のタグを使用してリソースの現在の保持の優先順位を取得します。 有効な値が範囲内にある`0.0`に`1.0`、優先度の低いリソースが最初に消去されます。
* `SetPreservationPriorityForTag (double priority, NSSet tags)` -特定のタグを使用してリソースの現在の保持の優先順位を設定します。 有効な値が範囲内にある`0.0`に`1.0`、優先度の低いリソースが最初に消去されます。

詳細については、Apple を参照してください[NSBundle 参照](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle)です。

### <a name="changes-to-nsprocessinfo"></a>NSProcessInfo への変更

IOS デバイスで実行されている各プロセスが 1 つの_プロセス情報エージェント_(PIA)。 使用して、 [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) PIA とコントロールの現在の電力と、特定のプロセスの温度管理に関する情報を提供するクラス。

たとえば、プロセスの自動終了を制御するには、次のコードを使用することができます。

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

詳細については、Apple を参照してください[NSProcessInfo 参照](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo)です。

### <a name="reacting-to-low-power-mode"></a>省電力モードへの応答

使用して、`LowPowerModeEnabled`のプロパティ、 [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/)クラスで、アプリが実行されている iOS デバイスで低電力モードが有効にされているかどうかを決定します。 例えば:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>HealthKit フレームワークの変更

Apple には、次の変更が含まれている、 [HealthKit](https://developer.xamarin.com/api/namespace/HealthKit/) iOS 9 のフレームワーク。

- 一括削除と HealthKit データベース内のエントリの削除の追跡をサポートします。 Apple を参照してください[HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject)、 [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery)と[HKHealthStore クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708)詳細についてはします。
- 新しい追跡カテゴリと特性が追加されました、`HKQuantityTypeIdentifier`クラス (など`UVExposure`) および、`HKCategoryTypeIdentifier`クラス (など`OvulationTestResult`)。 Apple を参照してください[HealthKit 定数参照](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html#//apple_ref/doc/uid/TP40014710)詳細についてはします。

参照してください、 [HealthKit 概要](~/ios/platform/healthkit.md)HealthKit Xamarin.iOS での操作方法の詳細についてはドキュメントです。

## <a name="local-authentication-framework-changes"></a>ローカル認証フレームワークの変更

Apple には、次の変更が含まれている、[ローカル認証](https://developer.xamarin.com/api/namespace/LocalAuthentication/)iOS 9 のフレームワーク。

- 使用して、`EvaluateAccessControl`と`EvaluatePolicy`のメソッド、 [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/)クラスできるようになりましたロック解除に成功した前から Touch ID と一致する再利用しようとします。
- 現在登録されている本の指の一覧を取得する権限です。
- 指が追加または認証から削除されたときの追跡をサポートします。
- 使用できる_認証コンテキスト_キーチェーン呼び出しとキーチェーン アクセス制御を評価するためのサポートを一覧表示します。
- ユーザーをキャンセルする機能は、コードから求められます。

参照してください、 [Touch ID の概要](~/ios/platform/touchid.md)Xamarin.iOS で Touch ID の操作方法の詳細についてはドキュメントです。

### <a name="lacontext-changes"></a>LAContext 変更

次の変更が加えられた、 [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) iOS 9 のクラス。

- **TouchIdAuthenticationMaximumAllowableReuseDuration** - Returns the maximum amount of time that a touch ID authentication can be reused.
- **EvaluatedPolicyDomainState** - を取得または評価されたポリシーの状態を設定します。
- **MaxBiometryFailures** -iOS 9 で非推奨になっています。
- **TouchIdAuthenticationAllowableReuseDuration** Gets or sets the amount of time that a touch ID authentication can be reused.
- **EvaluateAccessControl** - 非同期的に認証ポリシーを評価します。
- **無効になる**-特定のタッチ ID の認証を無効にします。
- **IsCredentialSet** -返します`true`資格情報が現在設定されている場合。
- **SetCredentialType**指定された資格情報の種類を設定します。

Apple を参照してください[LAContext 参照](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:)詳細についてはします。

## <a name="mapkit-framework-changes"></a>MapKit フレームワークの変更

Apple には、次の変更が含まれている、 [MapKit](https://developer.xamarin.com/api/namespace/MapKit/) iOS 9 のフレームワーク。

- MapKit は、転送中の方向に直接マップ アプリを起動する、転送中 (予定) の到着を使用する予定の時間のクエリを実行するようになりましたサポートを提供、 [MKLaunchOptions](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/)と[MKDirections](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/)クラスです。
- MapKit によって返される検索結果と[CLGeocoder](https://developer.xamarin.com/api/type/CoreLocation.CLGeocoder/)クラスは、結果のタイム ゾーンも提供します。
- IOS アプリを使用して、プレゼンター マップ注釈を完全にカスタマイズできるようになりました、`DetailCalloutAccessoryView`のプロパティ、 [MKAnnotationView](https://developer.xamarin.com/api/type/MapKit.MKAnnotationView/)クラスです。

参照してください、 [iOS マップ](~/ios/user-interface/controls/ios-maps/index.md)と[チュートリアル - 注釈と MapKit のオーバーレイを探索](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md)マップおよび、Xamarin.iOS および Apple内の注釈を操作する方法の詳細についてはドキュメント[CLGeocoder 参照](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder)詳細についてはします。

## <a name="passkit-framework-additions"></a>PassKit フレームワークの追加

Apple には、次の変更が含まれている、 [PassKit](https://developer.xamarin.com/api/namespace/PassKit/) iOS 9 のフレームワーク。

- ストア借方と Discover カードと共にクレジット カードの両方に Apple Pay なりました。 参照してください、**支払ネットワーク**Apple のセクション[PKPaymentRequest クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832)詳細についてはします。
- Xamarin.iOS アプリ内で直接今すぐ追加できます支払ネットワークとカードの発行元 Apple Pay にします。 Apple を参照してください[PKAddPaymentPassViewController クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116)詳細についてはします。

参照してください、 [PassKit 概要](~/ios/platform/passkit.md)PassKit Xamarin.iOS での操作方法の詳細についてはドキュメントです。

## <a name="safari-services-framework-additions"></a>Safari サービス フレームワークの追加

Apple には、次の変更が含まれている、 [Safari サービス](https://developer.xamarin.com/api/namespace/SafariServices/)iOS 9 のフレームワーク。

- 使用できるように、新しい[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) Xamarin.iOS アプリ内で web コンテンツを表示するクラス。 Safari アプリと web サイトのデータと cookie を共有する機能を提供し、Safari の機能 (リーダーやオートコンプリートを使用) などのいくつか含まれています。 [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/)機能、**完了**が web コンテンツの表示が完了したら、アプリにユーザーを返すボタンをクリックします。

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/)クラス向けに、web コンテンツの単一のページを表示する、いずれかの置換に使用する必要があります[WKWebKit](https://developer.xamarin.com/api/type/WebKit.WKWebView/)または[UIWebView](https://developer.xamarin.com/api/type/UIKit.UIWebView/)既存 Xamarin.iOS アプリ内のコントロールです。

### <a name="displaying-a-website"></a>Web サイトを表示します。

次のコードは、呼び出し元の例、 [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/)から別のビュー コント ローラー内。

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>UIKit フレームワークの変更

Apple での複数の要素に多くの機能強化が含まれている、 [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) iOS 9 のフレームワークです。 次のセクションではこれらの変更の詳細に説明します。

### <a name="3d-touch-events"></a>3D タッチ イベント

IOS 9 iPhone 6 s と iPhone 6 s を新しいさらに、3 D Touch は、iOS アプリに圧力機密性の高いジェスチャを追加します。 その結果、9 (またはそれ以上)、iOS でアプリが実行されている iOS デバイスが 3 D Touch をサポートできる場合は、負荷の変更により、`TouchesMoved`イベントが発生します。 

この動作の変更のために iOS アプリを準備する必要があります、`TouchesMoved`より多くの場合、呼び出されるイベント場合でも、X Y 座標値が変更されていない/です。 

詳細についてを参照してください、 [3 D Touch をはじめ](~/ios/platform/3d-touch.md)ガイドです。

### <a name="document-open-in-place-functionality"></a>ドキュメント開くインプレース機能

いずれかを使用して、`FinishedLaunching (application, launchOptions)`または`WillFinishLaunching (Application, launchOptions)`のメソッド、 [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/)クラス、今すぐドキュメントを開くし、(コピーで作業) ではなく位置で変更できます。

新しいオープン インプレース機能をサポートする追加、`LSSupportsOpeningDocumentsInPlace`を Xamarin.iOS アプリの主要**Info.plist**の値を持つファイル`YES`です。

Apple を参照してください[UIApplicationDelegate 参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate)詳細についてはします。

### <a name="enhanced-touch-events"></a>強化されたタッチ イベント

IOS 9 でのタッチ イベントのいくつかの機能強化が Apple で用意しています。 これらには、予測のタッチを使用して表示の更新間隔の中間調整へのアクセスを取得する機能が含まれます。

Apple を参照してください[iOS 用のイベント処理ガイド](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html)詳細についてはします。

### <a name="fetching-tailored-content"></a>調整されたコンテンツのフェッチ

新しい`NSDataAsset`クラスは、メモリと、iOS デバイスで現在実行中のグラフィックスの機能に合わせてカスタマイズ内容をフェッチする Xamarin.iOS アプリを使用できます。

### <a name="new-layout-anchors"></a>新しいレイアウト アンカー

新しい`NSLayoutAnchor`と`NSLayoutDimension`レイアウト アンカー クラスの新しいアンカーのプロパティを操作、 [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/)クラス (など`LeadingAnchor`と`WidthAnchor`) iOS 9 のレイアウトを容易にします。

参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)Xamarin.iOS アプリと Apple の自動レイアウトとサイズのクラスの使用方法の詳細についてはドキュメント[NSLayoutAnchor 参照](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor)、 [NSLayoutDimension 参照](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension)と[UIView 参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView)詳細についてはします。

### <a name="new-readable-content-margins"></a>新しい読み取り可能なコンテンツの余白

新しい`UILayoutGuide`読み取り可能なコンテンツの余白を提供し、ビュー内でコンテンツの描画領域を定義するクラスを使用できます。 Apple を参照してください[UILayoutGuide 参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide)詳細についてはします。

### <a name="text-input-in-notifications-modifications"></a>変更の通知のテキスト入力

[UIUserNotificationAction](https://developer.xamarin.com/api/type/UIKit.UIUserNotificationAction/)クラスには、新しい`Behavior`通知からのテキスト入力をサポートするために使用できるプロパティです。

### <a name="uiapplicationdelegate-changes"></a>UIApplicationDelegate 変更

すべての呼び出しを置き換えるとして推奨されていない正式に廃止され、使用 Apple 中、`FinishedLaunching (UIApplication application)`のメソッド、 [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/)クラスを`FinishedLaunching (UIApplication application, NSDictionary launchOptions)`または`WillFinishLaunching (UIApplication application, NSDictionary launchOptions)`メソッドです。

Apple を参照してください[UIApplicationDelegate 参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate)詳細についてはします。

### <a name="uikit-dynamics-changes"></a>UIKit Dynamics 変更

Apple には、iOS 9 に UIKit Dynamics に対する次の変更が含まれます。

- Dynamics は今すぐ、四角形以外の競合の境界のサポートを提供します。
- 新しいカスタマイズ可能な`UIFieldBehavior`クラスは、さまざまなフィールドの種類をサポートするために使用します。
- 追加された添付ファイルの種類、`UIAttachmentBehavior`クラスです。

Apple を参照してください[UIAttachment 参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior)詳細についてはします。

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView と UIDatePicker の変更

IOS 9 が前に、 [UIPickerView](https://developer.xamarin.com/api/type/UIKit.UIPickerView/)と[UIDatePicker](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/)コントロール、サイズを変更したし、(通常、幅、アプリケーションが iOS デバイスのコンテナーの幅に合わせて自動的にサイズを変更実行されている)。

IOS 9 の場合は、この自動サイズ変更は発生しなくなりますでコントロールを画面のサイズや向きに関係なく、すべての iOS デバイスで 320 ポイントの幅にレンダリングされます。

このような状況を解決するには、親コンテナー (ビュー) の端にコントロールの幅に固定し、必要な高さを指定する自動レイアウトとサイズ クラスを使用します。 参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)Xamarin.iOS アプリでの自動レイアウトとサイズ クラス操作する方法の詳細についてはドキュメントです。

### <a name="new-uitextinputassistantitem-class"></a>新しい UITextInputAssistantItem クラス

使用して、新しい`UITextInputAssistantItem`クラスのレイアウト バー ボタンのグループに、_ショートカット バー_です。 ショートカットのバーは、入力のショートカットを提供するソフト キーボードで使用できる新しい領域です。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 の概要](~/ios/platform/introduction-to-ios9/index.md)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [新機能 iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
