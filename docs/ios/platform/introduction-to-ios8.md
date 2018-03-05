---
title: "IOS 8 の概要"
description: "IOS 8、Apple が多種多様な新しいフレームワークと励起開発者を魅了し、するための Api が提供されます。 このガイドでは、これらの新しい Api を紹介し、iOS 8 が開発者とユーザーの両方を利用する方法を参照してください。 おされます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 3a77d1a3b597667d8944156b040c2819a5c79ca2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-8"></a>IOS 8 の概要

_IOS 8、Apple が多種多様な新しいフレームワークと励起開発者を魅了し、するための Api が提供されます。このガイドでは、これらの新しい Api を紹介し、iOS 8 が開発者とユーザーの両方を利用する方法を参照してください。 おされます。_

iOS 7 では視覚的に、どのようなユーザーおよび開発者渡されたもので OS の最初の iPhone から直接、期待できることから、全体の iOS のユーザー インターフェイスが変更されました。 IOS 8 は、このユーザーが、iPhone から直接その有効期間のほぼすべての側面を制御する開発者は、多くのフレームワークを提供する、続行します。 正常性および適合性を使用して分析するたとえば*HealthKit*、パスコード、生体認証を使用すると使用されなくなった*LocalAuthentication*、 *App extension*サード パーティ製のアプリ間の通信チャネルを開くと*HomeKit*今後のホームに自宅を有効にすることができます。 

IOS 7 のユーザーを満足させる場合は、iOS 8 は、これらおいしいの新しいツールの全体範囲によって、開発者は成長について説明します。 

このガイドでは、Xamarin.iOS 開発者向けの新しい Api が導入されています。  

詳細については、このドキュメントの最後に iOS 8 で廃止されたいくつかの Api もあります。

## <a name="requirements"></a>必要条件

要件は次の Mac を Visual Studio での iOS 8 のアプリを作成します。

- **Xcode 7 および iOS 8 以降**– Apple の最新の Xcode と iOS Api をインストールして、開発者のコンピューター上に構成する必要があります。
- **Visual Studio for Mac** – Visual Studio for Mac の最新バージョンをインストールし、ユーザーのデバイスで構成します。
- **iOS 8 デバイスまたはシミュレーター** : テスト用の iOS 8 の最新バージョンを実行している iOS デバイス。

## <a name="home-and-leisure"></a>ホーム組織とレジャー

iOS 8 がしっかりとできて HomeKit および HealthKit を使用して、自宅の中心となるまっすぐに Apple、および iOS デバイスを工場に役に立ちます。 このセクションでは、これら両方の新しいフレームワークは、次の作業、およびそれらを Xamarin.iOS アプリに統合する方法になります。

## <a name="homekit"></a>HomeKit

IPhone から、アプライアンスの制御は、そのテクノロジ; の新しいアプリケーションではありません。自宅で接続されている製品の多くは、iOS アプリを使用して制御できます。 ただし HomeKit 今すぐはこの手順をさらにホーム オートメーション デバイスの一般的なプロトコルを昇格して特定の製造元、iHome など Philips と Honeywell をパブリック API を使用できるようにします。 シームレスに家中のほぼすべての側面を制御できますこの意味をユーザーに 1 つのアプリケーションの内部です。 Philips 色合い電球、またはネスト アラームを使用することは無効です。 ユーザーは、「シーン」に同時に多数のスマート ホーム プロセスを連鎖することもできます。

HomeKit、サード パーティのアプリと Siri ことができますアクセサリを検出し自分個人のホーム構成データベースに追加、編集し、このデータに対して操作を実行およびとアクセサリと操作を実行するには、そのサービスと通信します。

### <a name="configuration"></a>構成

次の図は、HomeKit アクセサリの構成の基本的な階層を示しています。

![](introduction-to-ios8-images/image1.png "この図では、HomeKit アクセサリの構成の基本的な階層を示しています。")
 
HomeKit で開始するには、開発者は、プロビジョニング プロファイルに選択した HomeKit サービスがあることを確認する必要があります。 Apple では、Xcode の開発者向けの HomeKit シミュレーター アドインが用意されています。 これは含まれて、 [Apple Developer Center](https://developer.apple.com/downloads/index.action)`Hardware IO Tools for Xcode`です。 

詳細についてを参照してください、 [HomeKit](~/ios/platform/homekit.md)ガイドです。

## <a name="healthkit"></a>HealthKit

HealthKit は、正常性関連の情報を一元的な調整、およびセキュリティで保護されたデータ ストアを提供する iOS 8 で導入されたフレームワークです。 オペレーティング システムにより、プライバシーとセキュリティの正常性に関する情報と、ヘルス アプリケーションで、ユーザーのダッシュ ボード。 ユーザーのアクセス許可を持つアプリケーションはできるさまざまな正常性に関する情報を読み書きします。

Xamarin.iOS アプリでこれを使用する方法についてを参照してください、 [HealthKit 概要](~/ios/platform/healthkit.md)ガイドです。

## <a name="extending-iphone-functionality"></a>IPhone の機能を拡張します。
IOS8、開発者はされている指定されたでサード パーティ製アプリ間でより多くの通信用のアプリ、および高度な機能を使用できるをより細かく制御します。 アプリの拡張機能とドキュメントのピッカーなどの機能は、Apple のエコシステムでアプリケーションを使用する方法の可能性の世界を開きます。

### <a name="app-extensions"></a>アプリの拡張機能
、簡単に説明する、アプリの拡張機能は、サード パーティ製アプリを相互に通信する方法です。 高度なセキュリティ標準を維持し、セキュリティで保護されたアプリの整合性を維持するためには、この通信はアプリケーション間で直接発生しません。 代わりに、これで実行される、*拡張子*の中間にします。

アプリの拡張機能の作成の最初の手順は、正しい拡張ポイントを定義する — これは、動作と、正しい Api の可用性を確認するために重要です。 For Mac を Visual Studio でアプリの拡張機能を作成するには、既存のアプリケーションに新しいプロジェクトをソリューションに追加することで追加します。

**新しいプロジェクト**ダイアログ ' éˆú" **c#** > **iOS** > **Unified API**  >  **拡張機能**、次のスクリーン ショットに示すようにします。

![](introduction-to-ios8-images/image2.png "新しい拡張機能の作成")
 
新しいプロジェクト ダイアログでは、7 つの新しいプロジェクト テンプレートは、アプリの拡張機能を作成してを次に説明します。 ドキュメントの選択など、iOS で新しい api の他の関連の拡張機能の多くに注意してください。

- **アクション**– これにより、ユーザーが特定のタスクを実行できるように一意のカスタム アクション ボタンを作成する開発者
- **カスタムのキーボード**– これにより、開発者の範囲に追加する独自のカスタムの 1 つを追加することで Apple キーボードに組み込まれています。 一般的なキーボード、Swype はこの関数を使用して、iOS にキーボードを表示します。
- **ピッカーを文書化**– アプリケーションのサンド ボックスの外部のファイルにアクセスするユーザーができるようにドキュメント ピッカー ビューのコント ローラーが含まれます。
- **ピッカー ファイル プロバイダーを文書化**– これにより、ドキュメント ピッカーを使用してファイルを安全に保管します。
- **写真編集**– このフィルターを拡張し、ユーザーに提供する複数のコントロールとその他のオプション、写真の編集時に、写真のアプリケーションで Apple によって既に提供されているツールを編集します。
- **今日**– これにより、アプリケーション、通知センターの [今日] セクションでウィジェットを表示します。

Xamarin でアプリの拡張機能の使用に関する詳細についてを参照してください、[アプリ拡張機能の概要](~/ios/platform/extensions.md)ガイドです。

### <a name="touch-id"></a>タッチ ID

タッチ ID は、ユーザーを認証するための手段として iOS 7 で導入されました: パスコードに似ています。 デバイスのロックを解除する、アプリ ストアを使用して、iTunes を使用して、iCloud キーチェーンのみの認証に制限されていましたが、 

ローカルの認証 API を使用してアプリケーションを iOS 8 での認証メカニズムとして Touch ID を使用する 2 つの方法はようになりました。 現在ありませんをリモートでの認証にローカルの認証を使用すること。

まず、新しいキーチェーン アクセス制御リスト (Acl) を使用して既存のキーチェーン サービスを支援します。 キーチェーン データは、ユーザーの指紋の成功した認証を使用してロックできないことができます。

次に、LocalAuthentication は、アプリケーションをローカルでの認証に 2 つのメソッドを提供します。 開発者が使用する必要があります`CanEvaluatePolicy`デバイスが Touch ID を受け入れられるかを判断し、`EvaluatePolicy`認証操作を開始します。

Touch ID と Xamarin.iOS アプリケーションに統合する方法についての詳細についてを参照してください、[の概要に touch Id](~/ios/platform/touchid.md)ガイドです。

### <a name="document-picker"></a>ドキュメントの選択

別のアプリで作成された、インポートおよびそれらの操作およびそれらをエクスポートするファイルを開くアクセス許可をユーザーの iCloud ドライブでドキュメントの選択の動作は、もう一度バックアップします。 これは、直感的なワークフローでは、およびしたがって程度エクスペリエンスを向上させる、ユーザーを作成します。 この 1 つの手順をさらに受け取る iCloud の同期: 1 つのアプリケーションで行った変更がすべてのデバイス間で一貫して反映するもされます。

詳しくは、ドキュメントの選択について説明し、Xamarin.iOS アプリケーションに統合する方法についてを参照してください。、 [Introduction to The ドキュメント ピッカー](~/ios/platform/document-picker.md)ガイドです。

### <a name="handoff"></a>ハンドオフ

大きい、継続性機能の一部である、ハンドオフには、OS X、iOS を統合する方向にさらに一歩がかかります。 これには、クロスプラット フォーム AirDrop、呼び出しの iPhone、iPad、Mac、および、iPhone からテザリングの機能強化で SMS を実行する機能が含まれます。

ハンドオフは iOS 8 および Yosemite、連携して、使用するすべての異なるデバイスにログインして iCloud アカウントが必要です。 Safari、iWork、マップ、予定表、連絡先など、Apple の事前インストールされているアプリを使用して処理が必要です。

詳細についてを参照してください、[ハンドオフ](~/ios/platform/handoff.md)ガイドです。

## <a name="unified-storyboards"></a>統一されたストーリー ボード
iOS 8 が含まれていますが、新しいユーザー インターフェイスを作成するためのメカニズムを使用する方が簡単、統一されたストーリー ボードです。 すべてのハードウェアのさまざまな画面サイズに対応する 1 つにストーリー ボードは、true「一度設計すれば、多くの使用」スタイルで高速と応答性のビューを作成できます。

IOS8、前に開発者が使用される`UIInterfaceOrientation`縦長と横長のモード間で区別するために、 `UIInterfaceIdiom` iOS デバイスを区別するためにします。 IOS8 では不要になった iPhone および iPad デバイス用の個別のストーリー ボードを作成するために必要な — 向きとデバイスを使用してによって決定されます*サイズ クラス*です。

すべてのデバイスは、垂直および水平の軸の両方で、サイズ クラスによって定義され、iOS 8 のサイズ クラスの 2 種類があります。

- **正規**-これは、(iPad) などの大きな画面サイズ、または (など、UIScrollView 大きなサイズの印象を提供するガジェット
- **Compact** -これは、小さいデバイス (iPhone) などです。 このサイズでは、デバイスの向きが考慮されます。

2 つの概念が一緒に使用されている場合、結果は次の図に示すように、両方のさまざまな向きで使用できるさまざまな可能なサイズを定義する 2 x 2 グリッドには。

![](introduction-to-ios8-images/image3.png "両方のさまざまな向きで使用できるさまざまな可能なサイズを定義する 2 x 2 グリッドを表すダイアグラム")
 
サイズのクラスの詳細についてを参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)です。

## <a name="photo-kit"></a>フォト キット
フォト キットは、イメージ ライブラリがシステムのクエリを表示およびその内容を変更するカスタム ユーザー インターフェイスを作成するアプリケーションを使用する新しいフレームワークです。 画像とビデオの資産だけでなくアルバムやフォルダーなどの資産のコレクションを表すクラスの数が含まれています。

詳細についてを参照してください、 [PhotoKit](~/ios/platform/photokit.md)ガイドです。

## <a name="games"></a>ゲーム

<a name="scenekit" />

### <a name="scene-kit"></a>シーン キット

シーン キットは 3D シーン グラフを 3D 画像の操作を簡略化する API です。 OS X 10.8 で初めて導入されましたし、iOS 8 に来たようになりました。 シーン キット イマーシブ 3D 視覚エフェクトとカジュアル 3D ゲームを作成する必要はありません OpenGL の専門知識。 シーン graph の一般的な概念を構築、シーン キットを抽象 OpenGL と非常に簡単に行う追加 3D コンテンツをアプリケーションに OpenGL ES の複雑な作業です。 ただし、OpenGL エキスパートの場合は、シーン キットによっても OpenGL と直接に結び付けることのサポートがあります。 また、物理学など、3 D グラフィックスを補足する多数の機能が含まれていて、いくつか他の Apple などのフレームワーク、コア アニメーション、Core のイメージおよびスプライト キット予算との統合します。

詳細についてを参照してください、 [SceneKit](~/ios/platform/gaming/scenekit.md)ドキュメント。

<a name="spritekit" />

### <a name="sprite-kit"></a>スプライト キット

スプライト キット、Apple から 2D ゲーム フレームワークには、iOS 8 および OS X Yosemite のいくつか興味深いの新機能があります。 これらには、シーン キット、シェーダーのサポート、照明、影、制約、法線マップの生成、および物理学の機能強化との統合が含まれます。 具体的には、物理的な特性の新機能を使用すると、非常に簡単にゲームに現実的な効果を追加します。

詳細についてを参照してください、 [SpriteKit](~/ios/platform/gaming/spritekit.md)ドキュメント。

## <a name="other-changes"></a>その他の変更
前述の iOS 8 の主要な変更だけでなく、Apple は多くの既存のフレームワークをさらに更新が。 これら以下で詳しく説明します。

- **[イメージのコア](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)** – Apple が四角形の領域の検出サポートの向上を追加することで、イメージ処理のフレームワーク上に拡張し、イメージ内の QR コードします。 Mike Bluestein の彼のブログ投稿というタイトルでこれが検討[iOS 8 でのイメージの検出](http://blog.xamarin.com/image-detection-in-ios-8/)

## <a name="deprecated-apis"></a>非推奨 API
IOS 8 で行われたすべての機能の Api が使用されなくなりました。 以下のとおりです。

- **[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)**  – メソッドとリモートの通知を登録するために使用されるプロパティが推奨されていません。 これらは、registerForRemoteNotificationTypes enabledRemoteNotificationTypes です。
- **[UIViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)**  – メソッドとインターフェイスの向きを記述するためのプロパティに特徴 (traits) クラスとサイズのクラスが置き換えられます。 参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)これらを使用する方法の詳細についてはします。

- **[UISearchDisplayController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)**  – これは置き換えられました UISearchController iOS8 にします。

## <a name="summary"></a>まとめ
この記事では、apple iOS 8 で導入された新機能のいくつか説明しました。



## <a name="related-links"></a>関連リンク

- [UIKitEnhancements (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/UIKitEnhancements)
- [アプリの拡張機能の概要](~/ios/platform/extensions.md)
- [CloudKit の概要](~/ios/data-cloud/intro-to-cloudkit.md)
- [ドキュメントの選択の概要](~/ios/platform/document-picker.md)
- [HealthKit の概要](~/ios/platform/healthkit.md)
- [手動のカメラのコントロールの概要](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [Touch Id の概要](~/ios/platform/touchid.md)
- [統一されたストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)
