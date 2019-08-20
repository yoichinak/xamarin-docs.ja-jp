---
title: iOS 9 の概要
description: この記事では、iOS 9 for Xamarin の開発者が利用できる、新しい Api と変更された Api と機能について説明します。
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 69a599b6a7534cb77dd9e023d937f10a6b0523fb
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/19/2019
ms.locfileid: "69620670"
---
# <a name="introduction-to-ios-9"></a>iOS 9 の概要

_この記事では、iOS 9 for Xamarin の開発者が利用できる、新しい Api と変更された Api と機能について説明します。_

![](images/ios9-sml.png "IOS 9 ロゴ")

Apple では、iOS 9 に新しい Api とサービスがいくつか追加されており、既存の機能に多くの機能強化が加えられています。

## <a name="3d-touch"></a>3D Touch

IOS 9 および iPhone 6s と iPhone 6s Plus の新機能である3D タッチにより、負荷の高いジェスチャが iOS アプリに追加されます。 3D タッチを使用すると、iPhone アプリは、ユーザーがデバイスの画面に接していることを示すだけでなく、ユーザーがどの程度の圧力を exerting、さまざまなプレッシャーレベルに応答しているかを判断することもできます。

3D Touch は、アプリに次の機能を提供します。

- **筆圧の感度**-アプリは、ユーザーが画面に接しているハードまたはライトを測定し、その情報を活用できるようになりました。 たとえば、描画アプリを使用すると、ユーザーがどのくらいの画面にタッチしているかに基づいて、行を太くまたは細くすることができます。
- **ピークとポップ**-アプリは現在のコンテキストから移動することなく、ユーザーがデータを操作できるようになりました。 画面上で [ハード] を押すと、関心のある項目 (メッセージのプレビューなど) を*見る*ことができます。 より複雑にすると、項目に*ポップ*できます。
- **クイックアクション**-ユーザーがデスクトップアプリの項目を右クリックしたときにポップアップできるコンテキストメニューなどのクイックアクションを考えてみましょう。 クイックアクションを使用すると、iOS デバイスのホーム画面のアイコンから、アプリ内の関数に共通の、迅速かつ簡単にアクセスできるショートカットを追加できます。

詳細については、「 [3D タッチ](~/ios/platform/3d-touch.md)ガイドの概要」を参照してください。

## <a name="app-transport-security"></a>アプリケーション トランスポート セキュリティ

IOS 9 を初めて使用するアプリトランスポートセキュリティ (ATS) は、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続を適用します。 ATS は、すべてのインターネット通信がセキュリティで保護された接続のベストプラクティスに準拠していることを保証します。これにより、アプリまたは使用しているライブラリを通じて、機密情報が誤って開示されるのを防ぎます。

IOS 9 と OS X 10.11 (El Capitan) 用に構築されたアプリでは、ATS が既定で有効になっているため、 [n Lconnection](xref:Foundation.NSUrlConnection)、 [cfurl](xref:CoreFoundation.CFUrl) 、または[nを](xref:Foundation.NSUrlSession)使用した接続はすべて、ats セキュリティ要件の対象となります。 接続がこれらの要件を満たしていない場合、例外が発生して失敗します。

ATS の詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)ガイド」を参照してください。

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>iPad のマルチタスキング

IOS 9 では、Apple は、特定の iPad ハードウェアで同時に2つのアプリを実行するためのマルチタスクサポートを追加しました。 その結果、Xamarin iOS アプリは、特定の時点で実行されている唯一のアプリであることや、デバイスの全画面またはリソースにアクセスできることを前提にできなくなります。

IPad のマルチタスキングは、次の機能によってサポートされています。

- **スライドオーバー** -ユーザーが、現在実行されているメインアプリの約 25% をカバーするスライドアウトパネル (言語の方向に基づいて、画面の右側または左側) で2番目の iOS アプリを一時的に実行できるようにします。 スライドショーは、iPad Pro、iPad Air、iPad Air 2、iPad ミニ2、iPad ミニ3、iPad ミニ4でのみ利用できます。
- **分割ビュー** -サポートされている ipad ハードウェア (ipad Air 2、ipad ミニ4、ipad Pro のみ) では、ユーザーは2つ目のアプリを選択して、現在実行中のアプリと並べて分割画面モードで実行できます。 ユーザーは、各アプリが占めるメイン画面の割合を制御できます。
- 画像の画像-ビデオコンテンツを再生するアプリでは、iOS デバイスで現在実行されている他のアプリの周囲にある、移動してサイズ変更が可能なウィンドウでビデオを再生できるようになりました。 ユーザーは、このウィンドウのサイズと位置を完全に制御できます。 画像の画像は、iPad Pro、iPad Air、iPad Air 2、iPad ミニ2、iPad ミニ3、iPad ミニ4でのみ使用できます。

IOS 9 の新しいマルチタスク機能の詳細については、「 [iPad 用のマルチタスキング](~/ios/platform/multitasking.md)」ガイドを参照してください。

## <a name="new-contacts-and-contacts-ui-frameworks"></a>新しい連絡先と連絡先の UI フレームワーク

IOS 9 の導入により、Apple は2つの新しいフレームワークである[Contacts](xref:Contacts)と[ContactsUI](xref:ContactsUI)をリリースしました。これは、iOS 8 以前で使用されていた既存のアドレス帳とアドレス帳の UI フレームワークを置き換えるものです。

これらの新しいオブジェクト指向フレームワークには、次のものが用意されています。

- **Contacts** –ユーザーの連絡先情報に対する Xamarin. iOS アクセスを提供します。 ほとんどのアプリは読み取り専用アクセスのみを必要とするため、このフレームワークは、スレッドセーフで読み取り専用アクセスに対して最適化されています。
- **ContactsUI** – ios デバイスで連絡先を表示、編集、選択、および作成するための XAMARIN の UI 要素を提供します。

詳細については、[連絡先と連絡先の UI](~/ios/platform/contacts.md)に関するドキュメントを参照してください。


## <a name="new-search-apis"></a>新しい検索 Api

IOS 9 で Search が拡張され、Xamarin iOS アプリ内の情報に新しい方法でアクセスできるようになりました。 新しい検索 Api を使用して、スポットライトおよび Safari の検索結果、ハンドオフおよび Siri のリマインダーと提案を使用して、アプリのコンテンツを検索可能にすることができます。 これにより、ユーザーはアプリ内のアクティビティや情報にすばやくアクセスできるようになります。

また、新しい検索 Api を使用すると、以前に検索を実装することなく、アプリ内の検索を簡単に統合できます。 このため、Apple は、アプリ検索を使用して、iOS 9 アプリのコンテンツを汎用的に検索できるように、通常は数時間かかることを要求しています。

詳細については、[検索の拡張機能](~/ios/platform/search/index.md)に関するドキュメントを参照してください。

## <a name="new-stack-view"></a>新しいスタックビュー

スタックビューコントロール ([Uistackview](xref:UIKit.UIStackView) ) では、iOS デバイスの向きと画面サイズに動的に応答するサブビュー (水平方向または垂直方向) のスタックを管理するために、Auto Layout クラスと Size クラスの機能が活用されています。

スタックビューコントロールを使用すると、ユーザーインターフェイスのレイアウトに必要な作業量が大幅に削減されます。 スタックビューにアタッチされているすべてのサブビューのレイアウトは、軸、分布、配置、間隔など、開発者が定義したプロパティに基づいて自動的に管理されます。

詳細については、スタックビューのドキュメントの[概要を](~/ios/user-interface/controls/uistackview.md)参照してください。


## <a name="collection-view-changes"></a>コレクションビューの変更

IOS 9 では、コレクションビュー ([UICollectionView](xref:UIKit.UICollectionView)は、新しい既定のジェスチャレコグナイザーといくつかの新しいサポートメソッドを追加することで、すぐに項目をドラッグして並べ替えることができるようになりました。

これらの新しいメソッドを使用すると、コレクションビューでドラッグツーオーダーを簡単に実装できます。また、並べ替え処理のいずれかの段階で、アイテムの外観をカスタマイズすることもできます。

IOS 9 のコレクションビューの変更の詳細については、[コレクションビューの変更](~/ios/user-interface/controls/uicollectionview.md)に関するガイドを参照してください。

## <a name="game-enhancements"></a>ゲームの機能強化

IOS 9 では、Apple はゲーム Api に対していくつかの技術的な改善を行っています。これにより、Xamarin. iOS アプリでゲームグラフィックスとオーディオを簡単に実装できるようになりました。 これには、高レベルのフレームワークを使用した簡単な開発と、低レベルの拡張により速度とグラフィック機能を向上させるための iOS デバイスの GPU の機能の活用が含まれます。

これには、MetalKit、SceneKit、および SpriteKit の新機能として、再生キット、ReplayKit、モデル i/o、、金属パフォーマンスシェーダーが含まれます。

詳細については、[ゲーム拡張機能](~/ios/platform/gaming/index.md)に関するドキュメントを参照してください。

## <a name="homekit-framework-changes"></a>ホームキットフレームワークの変更

IOS 8 で導入された[ホームキット](xref:HomeKit)フレームワークは、Xamarin ios アプリから各種のホームキット対応アクセサリ (自動ライト、ドアロック、ガレージドア openers など) を設定および制御する機能を提供します。 簡単にセットアップして構成できるだけでなく、ホームキットのアクセサリは、話された Siri コマンドを使用して制御できます。

IOS 9 では、Apple のセットアップが簡単になり、サポートされているアクセサリの種類が拡張され、付属のアクセサリの相互作用 (たとえば、iCloud によるアクセサリのリモート制御など) が提供されました。

詳細については、「[ホームキットの概要](~/ios/platform/homekit.md)」、 [HomeKitIntro iOS サンプルアプリ](https://docs.microsoft.com/samples/xamarin/ios-samples/homekit-homekitintro)、および Apple の[ホームキット](https://developer.apple.com/homekit/)のドキュメントを参照してください。

## <a name="handoff-framework-changes"></a>ハンドオフフレームワークの変更

ハンドオフ (継続性とも呼ばれます) は、ユーザーがいずれかのデバイス (iOS または Mac) でアクティビティを開始し、デバイスの別のアクティビティ (ユーザーの iClou によって識別) を続行する方法として、Apple によって iOS 8 および OS X ヨーク Semite (10.10) に導入されました。d アカウント)。

ハンドオフは、新しい高度な検索機能もサポートするために、iOS 9 で拡張されました。 詳細については、[検索の拡張機能](~/ios/platform/search/index.md)に関するドキュメントを参照してください。 ハンドオフの使用の詳細については、「[ハンドオフ](~/ios/platform/handoff.md)ドキュメントの概要」を参照してください。

## <a name="new-extension-points"></a>新しい拡張ポイント

IOS 8 では、Apple は拡張機能を導入しました。これは、通知センター内、ユーザーがキーボードを要求したとき、または写真を編集しているときに、オペレーティングシステムによって標準コンテキストで提示されるライブラリです。

IOS 9 では、使用ポリシーを定義する新しい_拡張ポイント_をいくつか提供し、特定の領域内で動作するための api を提供することで、拡張機能のサポートを拡張しています。

- **新しいオーディオユニット拡張ポイント**-この拡張ポイントを使用して、他のオーディオユニットホストアプリ (GarageBand など) で使用するオーディオ効果、音楽楽器、サウンドジェネレーターなどを提供します。 また、この拡張ポイントを使用すると、_オーディオユニット_(オーディオプラグイン) をアプリストアで販売することもできます。
- **新しいインデックスメンテナンス拡張ポイント**—この拡張ポイントを使用して、アプリの再起動を必要とせずにアプリデータのインデックスの再作成をサポートします。
- **新しいネットワーク拡張ポイント**(Apple からの特別なアクセス許可が必要です):
  - **アプリケーションプロキシプロバイダーの拡張**—この拡張ポイントを使用して、カスタムの透過的なクライアント側ネットワークプロキシを実装します。
  - **フィルター Data Provider/フィルターコントロールプロバイダー拡張機能**-これらの拡張ポイントを使用して、デバイス上で動的なネットワークコンテンツフィルターを実装します。
  - **パケットトンネルプロバイダーの拡張機能**—この拡張ポイントを使用して、カスタム VPN トンネリングプロトコルクライアント側を実装します。
- **新しい Safari 拡張ポイント**:
  - **[コンテンツブロック拡張]** —この拡張ポイントを使用して、ユーザーが web を閲覧しているときに表示されないブロックされたコンテンツの一覧を定義します。
  - **共有リンク拡張機能**—この拡張ポイントを使用して、Safari の共有リンクでアプリのコンテンツを表示できるようにします。

詳細については、[拡張機能の概要](~/ios/platform/extensions.md)と Apple の[アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)に関するドキュメントを参照してください。

## <a name="keychain-enhancements"></a>キーチェーンの機能強化

IOS 9 では、次のように、Secure エンクレーブおよびその他のアイテム保護オプションに新しい暗号化キーの種類を提供するために、キーチェーンが拡張されています。

- フィンガープリントデータベースが変更されたときにキーチェーン項目を無効にする新しい Touch ID 制約。
- タッチ ID またはパスコードのみを含む Access Control リストエントリを作成できる新しい制約。
- `SecItem`呼び出しとは別に認証を呼び出せるようにする新しい認証コンテキスト。
- アプリによって提供されるキーチェーン項目の暗号化に対して、Access Control リストエントロピ (アプリケーションパスワードオプションを使用)。
- (属性を`kSecAttrTokenIDSecureEnclave`使用して) secure エンクレーブ内でのキーの生成と使用のサポート。

詳細については、「 [TOUCH ID](~/ios/platform/touchid.md)ドキュメントの概要」を参照してください。


## <a name="right-to-left-language-support"></a>右から左へ記述する言語サポート

IOS 9 では、右から左へ記述する言語を完全にサポートすることにより、以前よりも簡単にフリップされたユーザーインターフェイスを提供してきました。 これには、次の内容が含まれます。

- 標準の[Uikit](xref:UIKit)コントロールは、iOS デバイスのロケールと言語の設定に基づいて、右から左に自動的に反転されます。
- [Uiview](xref:UIKit.UIView)クラスには、右から左に反転したときに特定のビューを表示する方法を定義できる属性が用意されています。
- [Uiimage](xref:UIKit.UIImage)クラスの[FlipsForRightToLeftLayoutDirection](xref:UIKit.UIImage.FlipsForRightToLeftLayoutDirection)プロパティを使用して、プログラムによってイメージを反転する機能。

詳細については、Apple の[右から左へ記述する言語](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17)のドキュメントを参照してください。



## <a name="additional-framework-changes"></a>追加のフレームワークの変更

Apple は、上記で説明した主な変更に加えて、次のような iOS 9 用の既存のいくつかのフレームワークに対して変更と改善を加えました。

- AV Foundation フレームワーク
- AVKit フレームワーク
- CloudKit フレームワーク
- Foundation フレームワーク
- ハンドオフフレームワーク
- HealthKit Framework
- ホームキットフレームワーク
- ローカル認証フレームワーク
- MapKit フレームワーク
- Pass Kit フレームワーク
- Safari サービスフレームワーク
- UIKit フレームワーク

詳細については、 [iOS 9 フレームワークのその他の変更](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)に関するドキュメントを参照してください。

## <a name="deprecated-apis-and-functions"></a>非推奨の Api と関数

Apple では、iOS 9 の次の Api と関数が非推奨とされています。

- アドレス帳 & アドレス帳**ui** -これらの api は、Contact および contact ui フレームワークに置き換えられました。 詳細については、[連絡先と連絡先の UI](~/ios/platform/contacts.md)に関するドキュメントを参照してください。
- **CBCentralManager** - `RetrievePeripherals` iOS 9 `RetrieveConnectedPeripherals`では、 `CBCentralManager`クラスのメソッドとメソッドが削除されています。 これらのメソッドを呼び出すと、アクセサリをペアリングするとき、またはアプリを起動するときに、アプリがクラッシュします。
- **FetchAllChanges** - `CKFetchRecordChangesOperation`クラス`FetchAllChanges`のは、減価償却されており、iOS 9 では削除されます。
- **Media Player** -Media Player framework は、iOS 9 では非推奨となりました。 代わりに、AVKit または AV Foundation Api を使用してください。

特定の API 廃止の完全な一覧については、Apple の[iOS 9.0 api の相違](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222)点に関するドキュメントを参照してください。

## <a name="ios-9-sample-apps"></a>iOS 9 サンプルアプリ

開始するには、 [iOS 9 固有のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)がいくつか用意されています。

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-metalperformanceshadershelloworld)
- [MusicMotion](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-musicmotion)
- [PhotoProgress](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-photoprogress)
- [SegueCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-seguecatalog)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

また、これらのサンプルの iOS 部分も確認してください (コンパニオン Mac OS X バージョン)。

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [3D タッチの概要](~/ios/platform/3d-touch.md)
- [アプリケーション トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)
- [iPad のマルチタスキング](~/ios/platform/multitasking.md)
- [連絡先と連絡先の UI](~/ios/platform/contacts.md)
- [新しい検索 Api](~/ios/platform/search/index.md)
- [スタックビューの概要](~/ios/user-interface/controls/uistackview.md)
- [コレクションビューの変更](~/ios/user-interface/controls/uicollectionview.md)
- [ゲームの機能強化](~/ios/platform/gaming/index.md)
- [ホームキットの概要](~/ios/platform/homekit.md)
- [ハンドオフの概要](~/ios/platform/handoff.md)
- [iOS 9 フレームワークのその他の変更点](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [トラブルシューティング](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 の新機能](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Xamarin の iOS アプリを iOS9 に更新する (ビデオ)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
