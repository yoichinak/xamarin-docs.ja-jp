---
title: "IOS 9 の概要"
description: "この記事では、Xamarin.iOS 開発者向けのすべての新しいまたは変更された Api と iOS 9 で使用できる機能を紹介します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 5b26989603695cfb309fba5a5318f7ef4d2460e2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-9"></a>IOS 9 の概要

_この記事では、Xamarin.iOS 開発者向けのすべての新しいまたは変更された Api と iOS 9 で使用できる機能を紹介します。_

![](images/ios9-sml.png "IOS 9 のロゴ")

Apple には、iOS 9 と共に既存の機能の多くの機能強化でいくつかの新しい Api とサービスが追加されます。

## <a name="3d-touch"></a>3 D Touch

IOS 9 iPhone 6 s と iPhone 6 s を新しいさらに、3 D Touch は、iOS アプリに圧力機密性の高いジェスチャを追加します。 3D を使用したタッチ、iPhone アプリだけでなく、ユーザーがデバイスの画面と接触しているかどうか、ことができますも、ユーザーが exerting 量不足の意味し、異なる圧迫度への応答します。

3 D Touch は、アプリには、次の機能を提供します。

- **小文字の区別を圧迫**- アプリがどの程度厳密測定できるようになりましたかライト、ユーザーはその情報の画面と take 利点に触れることです。 たとえば、ペイント アプリは太くまたは薄型画面をユーザーがどの程度厳密に触れることに基づいて線になります。
- **ここに表示およびポップ**-アプリが、現在のコンテキスト外に移動することがなく、データとやり取りするようになりました。 画面にハードを押すと、これらのことができます*ピーク*(メッセージのプレビュー) と同様の関心のある項目にします。 困難を押すと、これらのことができます*Pop*を項目にします。
- **クイック アクション**-と考えるのクイック操作をするポップ アップできる、ユーザーがデスクトップ アプリ内の項目を右クリックすると、コンテキスト メニューのようにします。 クイック操作を使用して、追加できます共通、迅速かつ簡単な関数へのアクセスのショートカットをアプリで、ホーム画面アイコンから iOS デバイスで。

詳細については、次を参照してください、 [3 D Touch をはじめ](~/ios/platform/3d-touch.md)ガイドです。

## <a name="app-transport-security"></a>アプリのトランスポート セキュリティ

新しい iOS 9 の場合は、アプリのトランスポート セキュリティ (ATS) を設けて (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリ間でセキュリティで保護された接続。 ATS により、すべてのインターネット通信がセキュリティで保護された接続のベスト プラクティスに準拠しているアプリまたはそれが消費するためのライブラリを通じて直接の機密情報の誤った情報開示を回避します。

使用するすべての接続を iOS 9 および OS X 10.11 (許可されて El) 用に開発されたアプリの既定で有効になって ATS のため[NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)、 [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/)または[NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/)対象になります。ATS セキュリティ要件です。 接続にはこれらの要件を満たしていない場合は、例外で失敗します。

ATS の詳細についてを参照してください、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)ガイドです。

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>IPad のマルチタスク

Ios 9、Apple に特定 iPad ハードウェアで同時に 2 個のアプリを実行するためのマルチタスク サポートが追加されました。 その結果がだけに特定の時点で実行されているアプリ、またはを全画面表示またはデバイスのリソースへのアクセスがあること、Xamarin.iOS アプリは想定できなくできます。

IPad のマルチタス キングは、次の機能を使用してサポートされます。

- **スライド上**-現在実行されているメインのアプリの約 25% がカバーする (言語方向に基づいて、画面の右側または左側にいずれか) パネルをスライドに一時的に 2 つ目の iOS アプリを実行することができます。 スライドは iPad Pro、iPad 空気、iPad 空気 2、iPad ミニ 2、iPad ミニ 3、または iPad ミニ 4 でのみ使用できます。
- **分割ビュー** -iPad のサポートされているハードウェア (iPad 空気 2、iPad ミニ 4 および iPad Pro のみ)、ユーザーは、2 番目のアプリを選択し、分割画面表示モードで現在実行中のアプリとサイド バイ サイドを実行します。 ユーザーは、各アプリを占有するメイン画面の割合を制御できます。
- **画像に画像**- アプリの移動とサイズを変更できる ウィンドウで、iOS デバイスで現在実行されている他のアプリで再生するビデオ コンテンツの再生、ビデオできるようになりました。 ユーザーは、このウィンドウの位置とサイズを完全に制御を持ちます。 図の画像は iPad Pro、iPad 空気、iPad 空気 2、iPad ミニ 2、iPad ミニ 3、または iPad ミニ 4 でのみ使用できます。

IOS 9 の新しいマルチタスク機能の詳細についてを参照してください、 [iPad のマルチタスク](~/ios/platform/multitasking.md)ガイドです。

## <a name="new-contacts-and-contacts-ui-frameworks"></a>新しいアドレス帳と連絡先の UI フレームワーク

Apple iOS 9 の導入に伴い、2 つの新しいフレームワークをリリースしました[連絡先](https://developer.xamarin.com/api/namespace/Contacts/)と[ContactsUI](https://developer.xamarin.com/api/namespace/ContactsUI/)、既存のアドレス帳を交換して、8 と前に、iOS でアドレス帳の UI フレームワークが使用されています。

これらの新しい、オブジェクト指向フレームワークは、次を提供します。

- **連絡先**– ユーザーの連絡先情報には、Xamarin.iOS アクセスします。 ほとんどのアプリには、読み取り専用アクセスのみが必要なために、このフレームワークは、スレッド セーフである、読み取り専用アクセスに最適化されています。
- **ContactsUI** – 表示するには、Xamarin.iOS UI 要素の編集を選択、および iOS デバイスで連絡先を作成します。

詳細については、次を参照してください。 この[アドレス帳と連絡先 UI](~/ios/platform/contacts.md)ドキュメント。


## <a name="new-search-apis"></a>新しい検索 Api

検索は、iOS、Xamarin.iOS アプリ内の情報にアクセスする新しい方法を提供する 9 で展開されています。 新しい検索 Api を使用することができます、アプリのコンテンツ ハンドオフと Siri アラームと推奨事項など、メディア、および Safari の検索結果を検索可能です。 これにより、アクティビティと、アプリ内で詳細情報へのユーザーのクイック アクセスできます。

さらに、新しい Search Api を使用すると、以前の検索の実装機能せず、アプリで検索を統合しやすくします。 このため、Apple は、通常して、iOS 9 アプリのコンテンツを検索可能ユニバーサル アプリの検索を使用してほんの数時間を要することを要求します。

詳細についてを参照してください、[検索の機能強化](~/ios/platform/search/index.md)ドキュメント。

## <a name="new-stack-view"></a>新しいスタック ビュー

スタック ビュー コントロール ([UIStackView](https://developer.xamarin.com/api/type/UIKit.UIStackView/))、iOS デバイスの向きや画面のサイズを動的に応答する (水平方向または垂直方向に) サブビューのスタックを管理するには、自動レイアウトおよびサイズ クラスの機能を利用しています。

スタック ビュー コントロールを使用すると、作業の量は、ユーザー インターフェイスが大幅に減少レイアウトに必要です。 スタック ビューにアタッチされているすべてのサブビューのレイアウトは、軸、配布、調整間隔などの開発者が定義されているプロパティに基づいて自動的に管理されます。

詳細についてを参照してください、[スタック ビューの概要](~/ios/user-interface/controls/uistackview.md)ドキュメント。


## <a name="collection-view-changes"></a>コレクションの変更の表示

IOS 9、コレクション ビュー内で ([UICollectionView](https://developer.xamarin.com/api/type/UIKit.UICollectionView/)) ここではサポートしていますが新しい既定のジェスチャ レコグナイザーといくつかの新しいサポート メソッドを追加することで、ボックスから項目の並べ替えをドラッグします。

これらの新しいメソッドを使用して、簡単にコレクション ビューにドラッグの並べ替えを実装して、並べ替え処理の段階で、項目の外観のカスタマイズのオプションがあります。

IOS 9 のコレクション ビューの変更に関する詳細については、次を参照してください、[コレクションの変更の表示](~/ios/user-interface/controls/uicollectionview.md)ガイドです。

## <a name="game-enhancements"></a>ゲームの機能強化

Ios 9、Apple がに対していくつかの技術的な機能強化 Xamarin.iOS アプリで、ゲーム グラフィックスおよびオーディオを実装するが簡単にするゲーム用の Api です。 これらには、高度なフレームワークと速度の向上や低レベルの強化機能を備えたグラフィック機能について、iOS デバイスの GPU の電源を利用することによる開発の両方の簡単操作が含まれます。

金属、SceneKit SpriteKit の新しい、拡張機能と共に GameplayKit、ReplayKit、モデルの I/O、MetalKit およびメタル パフォーマンス シェーダーが含まれます。

詳細についてを参照してください、[ゲームの機能強化](~/ios/platform/gaming/index.md)ドキュメント。

## <a name="homekit-framework-changes"></a>HomeKit フレームワークの変更

[HomeKit](https://developer.xamarin.com/api/namespace/HomeKit/) iOS 8 で導入された、framework には、セットアップおよび、Xamarin.iOS アプリから (自動ライト、ドアのロック ガレージ ドアを開ける装置など) さまざまな有効になっている HomeKit アクセサリを制御する機能が用意されています。 だけでなく簡単にセットアップおよび構成、HomeKit アクセサリ Siri の話したコマンドを使用して制御できます。

Ios 9、Apple とセットアップが容易になりますが、アクセサリ サポートされており (アクセサリ iCloud を使用してリモートで制御する) など、複数の付属品相互作用を指定の種類を展開します。

詳細については、次を参照してください。、 [HomeKit 概要](~/ios/platform/homekit.md)、 [HomeKitIntro iOS サンプル アプリ](https://developer.xamarin.com/samples/monotouch/HomeKit/HomeKitIntro/)と Apple の[HomeKit](https://developer.apple.com/homekit/)ドキュメント。

## <a name="handoff-framework-changes"></a>ハンドオフ フレームワークの変更

ハンドオフ (継続性とも呼ばれます) は、ユーザーが自分のデバイス (iOS または Mac) のいずれかでアクティビティを起動し、自分のデバイス (ユーザーの iClou で認識される形式の他の場所でその同じアクティビティを続行するための手段として Apple ios 8 および OS X Yosemite (10.10) で導入されました。d アカウント)。

Handoff が強化された検索機能で iOS 9 サポートすることも、新しい展開されました。 詳細についてを参照してください、[検索の機能強化](~/ios/platform/search/index.md)ドキュメント。 ハンドオフの使用に関する詳細についてを参照してください、[ハンドオフの概要](~/ios/platform/handoff.md)ドキュメント。

## <a name="new-extension-points"></a>新しい拡張ポイント

IOS 8、Apple に拡張機能が導入されました。: ライブラリが提示する、標準のコンテキストで、オペレーティング システムなど、通知センター内で、ユーザーは、キーボードを要求するとき、または写真を編集するときにします。

Ios 9、Apple がサポートを拡張する拡張機能を提供するいくつかの新しいによって_拡張ポイント_使用ポリシーを定義し、次のように指定された領域内で操作するための Api を提供します。

- **新しいオーディオ単位の拡張点**– この拡張機能ポイントを使用して (GarageBand) など他のオーディオの単位のホスト アプリケーション内で使用するオーディオ特殊効果、楽器、サウンド ジェネレーターなどを提供します。 この拡張機能ポイントでは販売することもできます_オーディオ ユニット_(オーディオ プラグインを)、アプリ ストアにします。
- **新しいインデックスのメンテナンスの拡張点**— この拡張機能ポイントを使用して、アプリの再起動を必要とせず、アプリ データのインデックス再構築をサポートします。
- **新しいネットワーク拡張機能ポイント**(これら Apple からの特別なアクセス許可が必要)。
    - **アプリケーション プロキシ プロバイダー拡張**— この拡張機能ポイントを使用して、カスタムの透過的なクライアント側ネットワーク プロキシの実装です。
    - **データ プロバイダーのフィルター/フィルター コントロール プロバイダーの拡張機能**-これらの拡張ポイントを使用して動的ネットワークのコンテンツがデバイス上のフィルターを実装します。
    - **パケット トンネル プロバイダー拡張**— この拡張機能ポイントを使用してカスタム VPN トンネリング プロトコル クライアント側を実装します。
- **Safari の新しい拡張機能ポイント**:
    - **コンテンツの拡張機能のブロック**— この拡張機能ポイントを使用して、ユーザーが、web を参照するときに表示されないブロックのコンテンツの一覧を定義します。
    - **リンクの拡張機能を共有**— この拡張機能ポイントを使用して、Safari の共有へのリンクで、アプリのコンテンツの表示を有効にします。

詳細についてを参照してください、 [Introduction to Extensions](~/ios/platform/extensions.md)と Apple の[アプリ拡張機能プログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)ドキュメント。

## <a name="keychain-enhancements"></a>キーチェーンの機能強化

Ios 9、Apple が強化され、新しい暗号化キーの種類を次のようにセキュリティで保護された Enclave と他の項目の保護オプションを提供するキーチェーン。

- 指紋データベースが変更されたときに、キーチェーンの項目を無効にする新しい Touch ID 制約。
- Touch ID とパスコード アクセス制御リスト エントリを作成すると、のみを許可する新しい制約です。
- 新しい認証コンテキストを別の認証を呼び出すことができる`SecItem`呼び出しです。
- キーチェーンのアプリで用意されている項目の暗号化 (アプリケーションのパスワード オプションを使用) コントロール リストのエントロピーにアクセスします。
- 生成して、セキュリティで保護された enclave 内のキーを使用するためのサポート (を使用して、`kSecAttrTokenIDSecureEnclave`属性)。

詳細についてを参照してください、 [Touch ID の概要](~/ios/platform/touchid.md)ドキュメント。


## <a name="right-to-left-language-support"></a>右から左へ記述する言語のサポート

IOS 9、Apple が行わ提示反転したユーザー インターフェイスより簡単にこれまで右から左へ記述する言語の完全なサポートを提供しています。 次に例を示します。

- 標準的な[UIKit](https://developer.xamarin.com/api/namespace/UIKit/)コントロールは右から左 iOS デバイスのロケールと言語の設定に基づいて自動的に反転します。
- [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/)クラスを提供できるようにする特定のビューの表示時に方法を定義する属性が右から左へを反転します。
- 使用してプログラムからイメージを反転する機能、 [FlipsForRightToLeftLayoutDirection](https://developer.xamarin.com/api/property/UIKit.UIImage.FlipsForRightToLeftLayoutDirection/)のプロパティ、 [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/)クラスです。

詳細については、Apple を参照してください[右から左へのサポート言語](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17)ドキュメント。



## <a name="additional-framework-changes"></a>その他のフレームワークの変更

上記を説明する主な変更を加え Apple が行った変更を行うと iOS 9 に、次のいくつかの既存のフレームワークの機能強化。

- AV Foundation フレームワーク
- AVKit フレームワーク
- CloudKit フレームワーク
- Foundation フレームワーク
- ハンドオフ フレームワーク
- HealthKit フレームワーク
- HomeKit フレームワーク
- ローカル認証フレームワーク
- MapKit フレームワーク
- PassKit フレームワーク
- Safari サービス フレームワーク
- UIKit フレームワーク

詳細についてを参照してください、 [Framework の変更の他の iOS 9](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)ドキュメント。

## <a name="deprecated-apis-and-functions"></a>非推奨 Api および関数

Apple には、次の Api と iOS 9 の関数が非推奨します。

- **アドレス帳とアドレス帳 UI** -これらの Api は、取引先担当者および連絡先の UI フレームワーク置き換えられています。 詳細については、次を参照してください。 この[アドレス帳と連絡先 UI](~/ios/platform/contacts.md)ドキュメント。
- **CBCentralManager** -`RetrievePeripherals`と`RetrieveConnectedPeripherals`のメソッド、 `CBCentralManager` iOS 9 でクラスが削除されました。 これらのメソッドを呼び出すと、アプリの起動時またはクラッシュ アクセサリをペアリングするときにアプリが発生します。
- **FetchAllChanges** -`FetchAllChanges`の`CKFetchRecordChangesOperation`クラスが減価償却がされ、iOS 9 では削除されます。
- **Media Player** -iOS 9 の「Media Player framework は廃止されています。 代わりに、AVKit または AV Foundation Api のいずれかを使用します。

特定の API 機能廃止の完全な一覧は、Apple を参照してください。 [iOS 9.0 API の Diff](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222)ドキュメント。

## <a name="ios-9-sample-apps"></a>iOS 9 のサンプル アプリ

ある[iOS 9 に固有のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)作業を開始します。

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://developer.xamarin.com/samples/monotouch/ios9/MetalPerformanceShadersHelloWorld/)
- [MusicMotion](https://developer.xamarin.com/samples/monotouch/ios9/MusicMotion/)
- [PhotoProgress](https://developer.xamarin.com/samples/monotouch/ios9/PhotoProgress/)
- [SegueCatalog](https://developer.xamarin.com/samples/monotouch/ios9/SegueCatalog/)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

これらのサンプル (コンパニオン Mac OS X バージョンが!) の iOS 部分も確認します。

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [3 D Touch の概要](~/ios/platform/3d-touch.md)
- [アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)
- [iPad のマルチタスキング](~/ios/platform/multitasking.md)
- [アドレス帳と連絡先 UI](~/ios/platform/contacts.md)
- [新しい検索 Api](~/ios/platform/search/index.md)
- [スタック ビューの概要](~/ios/user-interface/controls/uistackview.md)
- [コレクションの変更の表示](~/ios/user-interface/controls/uicollectionview.md)
- [ゲームの機能強化](~/ios/platform/gaming/index.md)
- [HomeKit の概要](~/ios/platform/homekit.md)
- [ハンドオフの概要](~/ios/platform/handoff.md)
- [iOS 9 フレームワークのその他の変更点](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [トラブルシューティング](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [新機能 iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IOS9 (ビデオ) を Xamarin.iOS アプリの更新](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
