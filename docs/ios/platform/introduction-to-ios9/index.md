---
title: IOS 9 の概要
description: この記事では、Xamarin.iOS の開発者向けのすべての新規および変更した Api と iOS 9 で使用できる機能を紹介します。
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: becba36655a5247a11decb7dc54334f9397ecdfc
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233537"
---
# <a name="introduction-to-ios-9"></a>IOS 9 の概要

_この記事では、Xamarin.iOS の開発者向けのすべての新規および変更した Api と iOS 9 で使用できる機能を紹介します。_

![](images/ios9-sml.png "IOS 9 のロゴ")

Apple には、既存の機能の多くの機能強化と共に iOS 9 でいくつかの新しい Api やサービスが追加されます。

## <a name="3d-touch"></a>3D Touch

IOS 9 と iPhone 6 s および iPhone 6 s 新しい 3D Touch さらに、iOS アプリに負荷の機密性の高いジェスチャを追加します。 3D を使用したタッチ、iPhone アプリがようになりましただけでなく、ユーザーがデバイスの画面での手を加えることことを確認することができます、ユーザーが exerting 量不足を検出し、異なる圧力レベルに対応します。

3D Touch は、アプリには、次の機能を提供します。

- **感度への負荷が**- アプリがどの程度難しい測定できるようになりましたか光、ユーザーがその情報の画面を利用触れることができます。 たとえば、描画アプリでは、太くや薄型にユーザーがどの程度難しいが画面に触れるに基づいて行を作成できます。
- **ここに表示し、ポップ**-アプリが、ユーザーが、現在のコンテキストから離れることがなく、データと対話できますようになりました。 キーを押して、画面に、できる*ピーク*が (メッセージのプレビュー) のように興味を持つ項目にします。 キーを押して難しく、できる*ポップ*項目にします。
- **クイック アクション**-と考えるのクイック アクションをするポップ アップできる、ユーザーがデスクトップ アプリ内の項目を右クリックすると、コンテキスト メニューのようにします。 クイック アクションを使用して追加できます一般的な迅速かつ簡単には、関数へのアクセスのショートカット、アプリで iOS デバイスのホーム画面アイコンから。

詳細については、次を参照してください、 [3D Touch 概要](~/ios/platform/3d-touch.md)ガイド。

## <a name="app-transport-security"></a>アプリケーション トランスポート セキュリティ

新しい App Transport Security (ATS) を iOS 9、インターネット リソース (アプリのバック エンド サーバーなど) と、アプリの間のセキュリティで保護された接続を強制します。 ATS により、すべてのインターネット通信がセキュリティで保護された接続のベスト プラクティスに準拠しているアプリまたはライブラリを消費するを通じて直接に機密情報の偶発的漏えいを防ぐします。

ATS が iOS 9 および OS X 10.11 (El Capitan) を使用してすべての接続に開発されたアプリの既定で有効になっているため[NSUrlConnection](xref:Foundation.NSUrlConnection)、 [CFUrl](xref:CoreFoundation.CFUrl)または[NSUrlSession](xref:Foundation.NSUrlSession)対象になります。ATS セキュリティ要件です。 接続はこれらの要件を満たしていない場合は、例外で失敗します。

ATS に関する詳細については、次を参照してください、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)ガイド。

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>IPad のマルチタス キング

IOS 9 の場合は、Apple iPad の特定のハードウェア上で同時に 2 つのアプリを実行するためのマルチタス キングのサポートが追加されます。 その結果、任意の時点で実行されている唯一のアプリであるか、全画面表示や、デバイスのリソースへのアクセスである、Xamarin.iOS アプリは想定できなくできます。

IPad のマルチタス キングは、次の機能を使用してサポートされています。

- **スライド上**-現在実行中のメイン アプリケーションの約 25% をカバーする (言語の方向に基づいた画面の右端または左端にあるいずれか) パネルをスライドに一時的に 2 つ目の iOS アプリを実行するユーザーを許可します。 スライドは iPad Pro、iPad 航空、iPad 空気 2、iPad Mini 2、iPad Mini 3、または iPad Mini 4 でのみ使用できます。
- **分割ビュー** -iPad のサポートされているハードウェア (iPad 空気 2、iPad Mini 4 および iPad Pro のみ)、ユーザーは 2 つ目のアプリを選択し、分割の画面表示モードで現在実行中のアプリをサイド バイ サイドでを実行します。 ユーザーは、各アプリを占有するメイン画面の割合を制御できます。
- **ピクチャインピクチャ**- アプリ、iOS デバイスで現在実行中の他のアプリから浮遊した移動とサイズ変更可能なウィンドウで再生ビデオ コンテンツを再生、ビデオできるようになりました。 ユーザーは、このウィンドウの位置とサイズを完全に制御を持ちます。 図の画像は iPad Pro、iPad 航空、iPad 空気 2、iPad Mini 2、iPad Mini 3、または iPad Mini 4 でのみ使用できます。

IOS 9 のマルチタス キングの新機能に関する詳細については、次を参照してください、 [iPad のマルチタス キング](~/ios/platform/multitasking.md)ガイド。

## <a name="new-contacts-and-contacts-ui-frameworks"></a>新しいアドレス帳と連絡先の UI フレームワーク

Apple iOS 9 の導入に伴い、2 つの新しいフレームワークをリリースしました[連絡先](https://developer.xamarin.com/api/namespace/Contacts/)と[ContactsUI](https://developer.xamarin.com/api/namespace/ContactsUI/)8 以降、iOS でアドレス帳の UI フレームワークが使用される、既存のアドレス帳を交換します。

これらの新しい、オブジェクト指向フレームワークは、以下を説明します。

- **連絡先**– ユーザーの連絡先情報への Xamarin.iOS を提供します。 ほとんどのアプリには、読み取り専用アクセスのみ必要とするため、このフレームワークは、スレッド セーフの読み取り専用アクセスの最適化されています。
- **ContactsUI** – を表示するには、Xamarin.iOS の UI 要素の編集、選択、および iOS デバイスでの連絡先を作成します。

詳細については、次を参照してください。 この[連絡先と連絡先 UI](~/ios/platform/contacts.md)ドキュメント。


## <a name="new-search-apis"></a>新しい Search Api

検索は、iOS、xamarin ios アプリ内の情報にアクセスする新しい方法を提供する 9 で拡張されています。 新しい検索 Api を使用して行うことができます、アプリのコンテンツと Siri アラームと提案など、スポット ライトと Safari の検索結果を検索可能です。 これにより、ユーザーのアクティビティと、アプリ内で詳細情報へのクイック アクセスできます。

さらに、新しい検索 Api を使用すると、以前の検索の実装機能せず、アプリで検索を統合しやすくします。 このため、Apple は iOS 9 アプリのコンテンツをアプリの検索を使用して検索可能な汎用的に数時間を要するそのことを要求します。

詳細についてを参照してください、[検索の機能強化](~/ios/platform/search/index.md)ドキュメント。

## <a name="new-stack-view"></a>新しいスタック ビュー

スタック ビュー コントロール ([UIStackView](xref:UIKit.UIStackView) iOS デバイスの向きや画面サイズに動的に応答する (水平方向または垂直方向に)、サブビューのスタックを管理するには、自動レイアウトとサイズ クラスの機能を利用しています。

スタック ビュー コントロールを使用すると、作業量は、ユーザー インターフェイスが大幅に減少のレイアウトに必要です。 スタック ビューにアタッチされているすべてのサブビューのレイアウトは、軸、配布、配置、および間隔などの開発者は定義されたプロパティに基づいて自動的に管理されます。

詳細についてを参照してください、[スタック ビューの概要](~/ios/user-interface/controls/uistackview.md)ドキュメント。


## <a name="collection-view-changes"></a>コレクションの変更の表示

IOS 9、コレクション ビューで ([UICollectionView](xref:UIKit.UICollectionView)サポート ドラッグ、新しい既定ジェスチャ レコグナイザーおよびいくつかの新しいサポート メソッドを追加することで、ボックスから項目を並べ替えできるようになりました。

これらの新しいメソッドを使用して、簡単に実装ドラッグの順序を変更する、コレクション ビューにして、順序変更プロセスのいずれかの段階中にアイテムの外観のカスタマイズのオプションがあります。

IOS 9 のコレクション ビューの変更に関する詳細については、次を参照してください、[コレクションの変更の表示](~/ios/user-interface/controls/uicollectionview.md)ガイド。

## <a name="game-enhancements"></a>ゲームの機能強化

Ios 9、Apple は、Xamarin.iOS アプリでゲームのグラフィックとオーディオを実装しやすくゲーム Api へいくつかの技術的な機能強化が確立します。 高レベルのフレームワークの速度の向上と低レベルが強化されたグラフィックの機能、iOS デバイスの GPU の力を活用して開発の両方の容易さが含まれます。

金属、SceneKit SpriteKit の新しい高度な機能と共に GameplayKit、ReplayKit、モデルの I/O、MetalKit および金属パフォーマンス シェーダーが含まれます。

詳細についてを参照してください、[ゲームの機能強化](~/ios/platform/gaming/index.md)ドキュメント。

## <a name="homekit-framework-changes"></a>HomeKit フレームワークの変更

[HomeKit](https://developer.xamarin.com/api/namespace/HomeKit/) iOS 8 で導入されたフレームワークをセットアップして Xamarin.iOS アプリから (自動化されたライト、ドアのロック ガレージ ドアを開ける装置など) の各種の有効になっている HomeKit アクセサリを制御する機能を提供します。 簡単にセットアップして構成することに加え、Siri の音声コマンドを使用して HomeKit アクセサリを制御できます。

Ios 9 では Apple がセットアップを簡単に行う、サポートされており (アクセサリ iCloud を使用してリモートで制御する) など、複数のアクセサリ相互作用を提供する、[アクセサリ] の種類を展開します。

詳細については、次を参照してください。 この[HomeKit の概要](~/ios/platform/homekit.md)、 [HomeKitIntro iOS サンプル アプリ](https://developer.xamarin.com/samples/monotouch/HomeKit/HomeKitIntro/)と Apple の[HomeKit](https://developer.apple.com/homekit/)ドキュメント。

## <a name="handoff-framework-changes"></a>ハンドオフ フレームワークの変更

ハンドオフ (継続性とも呼ばれます) は、Apple ios 8 および OS X Yosemite (10.10) でユーザーが自分のデバイス (iOS または Mac) のいずれかのアクティビティを開始して、同じアクティビティに続行 (ユーザーの iClou で識別されるデバイスのもう 1 つの方法として導入されました。d アカウント)。

ハンドオフは、強化された検索機能を iOS 9 でも、新しいサポートで拡張されました。 詳細についてを参照してください、[検索の機能強化](~/ios/platform/search/index.md)ドキュメント。 ハンドオフの使用に関する詳細についてを参照してください、[ハンドオフの概要](~/ios/platform/handoff.md)ドキュメント。

## <a name="new-extension-points"></a>新しい拡張ポイント

IOS 8、Apple が拡張機能を導入しました。-ライブラリが示されている標準のコンテキストで、オペレーティング システムによってなど、通知センター内のユーザーの要求、キーボード、または写真を編集するときにします。

Ios 9、Apple は、いくつかの新しい提供することで拡張機能のサポートを拡張は_拡張ポイント_使用ポリシーを定義し、次のように指定された領域内で操作するための Api を提供します。

- **オーディオ ユニットの新しい拡張機能ポイント**– この拡張機能ポイントを使用して (GarageBand) などの他のオーディオ ユニット ホスト アプリ内で使用するなど、オーディオ効果、楽器、サウンドのジェネレーターを提供します。 この拡張機能ポイントでは販売することもできます_オーディオ ユニット_(オーディオ プラグインを)、App Store でします。
- **新しいインデックスのメンテナンス拡張機能ポイント**: この拡張機能ポイントを使用して、アプリの再起動を必要とせず、アプリ データのインデックス再構築をサポートします。
- **新しいネットワークの拡張機能ポイント**(Apple から特別なアクセスを許可する必要があります)。
    - **アプリ プロキシ プロバイダーの拡張機能**: この拡張機能ポイントを使用して、カスタムの透過的なクライアント側のネットワーク プロキシを実装します。
    - **データ プロバイダーのフィルター/フィルター コントロール プロバイダーの拡張機能**-これらの拡張ポイントを使用して、デバイス上のフィルター処理ネットワークの動的なコンテンツを実装します。
    - **パケット トンネル プロバイダーの拡張機能**: この拡張機能ポイントを使用してカスタム VPN トンネリング プロトコル クライアント側を実装します。
- **Safari の新しい拡張機能ポイント**:
    - **コンテンツの拡張機能をブロックしている**: この拡張機能ポイントを使用して、ユーザーが web を参照するときに表示されないブロックのコンテンツの一覧を定義します。
    - **共有リンク拡張機能**: この拡張機能ポイントを使用して、Safari の共有リンクで、アプリのコンテンツの表示を有効にします。

詳細についてを参照してください、[拡張機能の概要](~/ios/platform/extensions.md)と Apple の[アプリ拡張機能のプログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)ドキュメント。

## <a name="keychain-enhancements"></a>キーチェーンの機能強化

Ios 9 で Apple は、次のように、セキュリティで保護されたエンクレーブとその他の項目の保護オプションの新しい暗号化キーの種類を提供するキーチェーンを強化します。

- 指紋データベースが変更されたときに、キーチェーンの項目を無効にする新しい Touch ID 制約。
- パスコードの Touch ID とアクセス制御リスト エントリを作成すると、のみを許可する新しい制約。
- 新しい認証コンテキストを別の認証を呼び出すことができる`SecItem`呼び出し。
- キーチェーンをアプリに用意されている項目の暗号化コントロール一覧のエントロピ (アプリケーションのパスワード オプションを使用) にアクセスします。
- 生成して、セキュリティで保護されたエンクレーブ内のキーを使用するためのサポート (を使用して、`kSecAttrTokenIDSecureEnclave`属性)。

詳細についてを参照してください、 [Touch ID の概要](~/ios/platform/touchid.md)ドキュメント。


## <a name="right-to-left-language-support"></a>右から左へ記述する言語のサポート

Ios 9 の場合は、Apple が提示反転したユーザー インターフェイスよりも簡単にこれまでの右から左の言語の完全なサポートを提供することで行われます。 次に例を示します。

- 標準[UIKit](xref:UIKit)コントロールを左右に - iOS デバイスのロケールと言語の設定に基づいて自動的に反転します。
- [UIView](xref:UIKit.UIView)クラスは、指定されたビューの表示時期方法を定義するための属性は、右から左を反転します。
- 使用してプログラムでイメージを反転する機能、 [FlipsForRightToLeftLayoutDirection](xref:UIKit.UIImage.FlipsForRightToLeftLayoutDirection)のプロパティ、 [UIImage](xref:UIKit.UIImage)クラス。

詳細については、Apple を参照してください[言語のサポートを左右](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17)ドキュメント。



## <a name="additional-framework-changes"></a>その他のフレームワークの変更

上を説明する主な変更を加え Apple が行った変更と iOS 9 の次のように既存のフレームワークをいくつかの機能強化。

- AV Foundation Framework
- AVKit Framework
- CloudKit Framework
- Foundation Framework
- ハンドオフ フレームワーク
- HealthKit のフレームワーク
- HomeKit フレームワーク
- ローカル認証フレームワーク
- MapKit フレームワーク
- PassKit Framework
- サービス フレームワークの safari
- UIKit フレームワーク

詳細についてを参照してください、[追加 iOS 9 フレームワークの変更点](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)ドキュメント。

## <a name="deprecated-apis-and-functions"></a>非推奨の Api と機能

Apple には、次の Api と iOS 9 の関数が非推奨とされます。

- **アドレス帳とアドレス帳 UI** -これらの Api は、連絡先と連絡先の UI フレームワーク置き換えられています。 詳細については、次を参照してください。 この[連絡先と連絡先 UI](~/ios/platform/contacts.md)ドキュメント。
- **CBCentralManager** -`RetrievePeripherals`と`RetrieveConnectedPeripherals`のメソッド、 `CBCentralManager` iOS 9 でクラスが削除されました。 これらのメソッドを呼び出すと、クラッシュ アクセサリをペアリングする場合、またはアプリの起動時にアプリが発生します。
- **FetchAllChanges** -`FetchAllChanges`の`CKFetchRecordChangesOperation`クラスが非推奨になって、iOS 9 では削除されます。
- **Media Player** -iOS 9 で、Media Player フレームワークが非推奨とされました。 代わりに、AVKit または AV Foundation Api のいずれかを使用します。

特定の API の廃止された機能の一覧については、Apple を参照してください。 [iOS 9.0 API の差分を](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222)ドキュメント。

## <a name="ios-9-sample-apps"></a>iOS 9 のサンプル アプリ

ある[iOS 9 に固有のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)を開始します。

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://developer.xamarin.com/samples/monotouch/ios9/MetalPerformanceShadersHelloWorld/)
- [MusicMotion](https://developer.xamarin.com/samples/monotouch/ios9/MusicMotion/)
- [PhotoProgress](https://developer.xamarin.com/samples/monotouch/ios9/PhotoProgress/)
- [SegueCatalog](https://developer.xamarin.com/samples/monotouch/ios9/SegueCatalog/)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

これらのサンプル (コンパニオン Mac OS X バージョン予定!) の iOS の部分を確認します。

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [3D Touch の概要](~/ios/platform/3d-touch.md)
- [アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)
- [iPad のマルチタスキング](~/ios/platform/multitasking.md)
- [連絡先と連絡先 UI](~/ios/platform/contacts.md)
- [新しい Search Api](~/ios/platform/search/index.md)
- [スタック ビューの概要](~/ios/user-interface/controls/uistackview.md)
- [コレクションの変更の表示](~/ios/user-interface/controls/uicollectionview.md)
- [ゲームの機能強化](~/ios/platform/gaming/index.md)
- [HomeKit の概要](~/ios/platform/homekit.md)
- [ハンドオフの概要](~/ios/platform/handoff.md)
- [iOS 9 フレームワークのその他の変更点](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [トラブルシューティング](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 を新します。](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Ios 9 (ビデオ) に、Xamarin.iOS アプリを更新しています](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
