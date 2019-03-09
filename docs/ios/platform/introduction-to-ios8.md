---
title: IOS 8 の概要
description: IOS 8、Apple は多くの新しいフレームワークと開発者を魅了し、エキサイティングに Api を提供しました。 このガイドはこれらの新しい Api を導入し、iOS 8 が開発者とユーザーの両方を利用する方法を参照してください。
ms.prod: xamarin
ms.assetid: 33AD66C0-3743-49FE-9DCE-88ED3A16BA63
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 9299322eb20561444262c2b2ba87191d2bddcde4
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668752"
---
# <a name="introduction-to-ios-8"></a>IOS 8 の概要

_IOS 8、Apple は多くの新しいフレームワークと開発者を魅了し、エキサイティングに Api を提供しました。このガイドはこれらの新しい Api を導入し、iOS 8 が開発者とユーザーの両方を利用する方法を参照してください。_

iOS 7 は、どのようなユーザーと開発者は、最初の iPhone OS から直接、期待する取り込まれたから全体の iOS のユーザー インターフェイスを視覚的に変更します。 IOS 8 では、ユーザーが、iPhone から直接、ライフ サイクルのほぼすべての側面を制御する開発者は、多くのフレームワークを提供することによってこれを続行します。 正常性および適合性を使用して分析するたとえば*HealthKit*、パスコード、生体認証を使用してで使用されなくなった*LocalAuthentication*、 *App エクス テンション*サード パーティのアプリ間の通信チャネルを開くと*HomeKit*将来のホームに家を有効にすることができます。 

IOS 7 のユーザーを喜ばせる場合は、iOS 8 は開発者おいしい新ツールの範囲全体に喜んでいただけるについて説明します。 

このガイドでは、Xamarin.iOS の開発者のため、新しい Api が導入されています。  

詳細については、このドキュメントの最後に iOS 8 で非推奨とされましたが、一部の Api もあります。

## <a name="requirements"></a>必要条件

次が Visual studio for Mac iOS 8 のアプリを作成する必要です。

- **Xcode 7 および iOS 8 以降**– Apple の最新の Xcode と iOS Api がインストールされ、開発者のコンピューターに構成する必要があります。
- **Visual Studio for Mac** – Visual Studio for Mac の最新バージョンをインストールして、ユーザーのデバイスで構成する必要があります。
- **iOS 8 デバイスまたはシミュレーター** – iOS デバイスをテストするための iOS 8 の最新バージョンを実行します。

## <a name="home-and-leisure"></a>ホーム ページと都合のよいタイミング

iOS 8 をしっかりと HomeKit や HealthKit を使用して、自宅の中核にまっすぐに Apple、および iOS デバイスを工場に役立ちました。 このセクションでは、これら両方の新しいフレームワークは、次の作業、およびそれらを Xamarin.iOS アプリケーションに統合する方法に注目します。

## <a name="homekit"></a>HomeKit

IPhone から、アプライアンスを制御するテクノロジです。 新しいアプリケーションではありません。接続されているホーム製品の多くは、iOS アプリを使用して制御できます。 ただし HomeKit ここでは、この手順さらにホーム オートメーション デバイスは、一般的なプロトコルを昇格させることにより、特定の製造元など、iHome Philips と Honeywell をパブリック API を使用できるようにします。 シームレスに自分のホームのほぼすべての側面を制御できることを意味これをユーザーに 1 つのアプリケーション内で。 Philips Hue 電球、または入れ子のアラームを使用していることを知らせに関連はありません。 ユーザーには、多数のスマート ホーム プロセスが「シーン」にまとめてチェーンもことができます。

HomeKit、サード パーティのアプリと Siri できますアクセサリを検出し、個人のホームの構成データベースに追加、編集し、このデータに対して操作を実行およびアクセサリとアクションを実行するには、そのサービスと通信します。

### <a name="configuration"></a>構成

次の図は、HomeKit アクセサリの構成の基本的な階層を示します。

![](introduction-to-ios8-images/image1.png "この図では、HomeKit アクセサリの構成の基本的な階層を示しています。")
 
HomeKit を開始するには、開発者は、プロビジョニング プロファイルに選択した HomeKit サービスがあることを確認する必要があります。 Apple は、Xcode の開発者向けの HomeKit シミュレーター アドインをも提供しました。 確認できます、 [Apple Developer Center](https://developer.apple.com/downloads/index.action)`Hardware IO Tools for Xcode`します。 

詳細についてを参照してください、 [HomeKit](~/ios/platform/homekit.md)ガイド。

## <a name="healthkit"></a>HealthKit

HealthKit は、正常性関連の情報を一元的な調整、およびセキュリティで保護されたデータ ストアを提供する iOS 8 で導入されたフレームワークです。 オペレーティング システムにより、プライバシーとセキュリティの正常性に関する情報と、正常性のアプリ、ユーザーのダッシュ ボード。 ユーザーのアクセス許可を持つアプリケーションは、読み取りし、さまざまな正常性に関する情報を記述します。

Xamarin.iOS アプリの使用方法に関する詳細についてを参照してください、 [HealthKit の概要](~/ios/platform/healthkit.md)ガイド。

## <a name="extending-iphone-functionality"></a>IPhone の機能を拡張します。
IOS8 で開発者がサード パーティのアプリ間通信をよりオープンに、アプリ、および高度な機能を使用できるユーザーをより細かく制御提供されているされます。 アプリの拡張機能とドキュメント ピッカーなどの機能は、Apple のエコシステムでのアプリケーションの使用方法の可能性を開きます。

### <a name="app-extensions"></a>アプリ拡張機能
、簡単に説明する、アプリの拡張機能は、サード パーティ製アプリで互いと通信する方法です。 高度なセキュリティ標準を維持し、サンド ボックス化されたアプリの整合性を維持するためには、この通信はアプリケーション間で直接発生しません。 代わりに、これによって実行される、*拡張子*中央にします。

アプリ拡張機能の作成の最初の手順が適切な拡張ポイントを定義するには、これは、動作と、適切な Api の可用性を保証するために重要です。 Visual studio for Mac アプリの拡張機能を作成するをソリューションに新しいプロジェクトを追加することで既存のアプリケーションに追加します。

**新しいプロジェクト**に移動します ダイアログ**C#**  >  **iOS** > **Unified API**  > **拡張**、次のスクリーン ショットに示すようにします。

![](introduction-to-ios8-images/image2.png "新しい拡張機能の作成")
 
新しいプロジェクト ダイアログでは、7 つの新しいプロジェクト テンプレートは、アプリ拡張機能を作成して、次に説明します。 ドキュメント ピッカーなどの iOS の他の新しい Api に関連する拡張機能の多くに注意してください。

- **アクション**– これにより、開発者がユーザーには、特定のタスクを実行できるように一意のカスタム アクション ボタンを作成するには
- **カスタム キーボード**– これにより、開発者の範囲に追加する独自のカスタムの 1 つを追加することで Apple キーボード組み込まれています。 人気のあるキーボード、Swype はこの関数を使用して、iOS にキーボードを表示します。
- **Document Picker** – アプリケーションのサンド ボックスの外部ファイルにアクセスするユーザーがドキュメント ピッカー ビューのコント ローラーが含まれます。
- **ドキュメント ピッカー ファイル プロバイダー** – これにより、ドキュメント ピッカーを使用してファイルをセキュリティで保護されたストレージ。
- **写真編集**– このフィルターを拡張し、写真を編集するときにユーザーの詳細に制御し、その他のオプションに付与する写真アプリケーションで Apple によって既に提供されているツールを編集します。
- **今すぐ**– これにより、アプリケーション、通知センターの [今日] セクションにウィジェットを表示します。

Xamarin でのアプリの拡張機能の使用の詳細についてを参照してください、[アプリ拡張機能の概要](~/ios/platform/extensions.md)ガイド。

### <a name="touch-id"></a>タッチ ID

Touch ID が、ユーザーの認証の手段として iOS 7 で導入された: パスコードに似ています。 ただし、デバイスのロックを解除、App Store を使用して、iTunes を使用して、iCloud キーチェーンのみの認証に制限されていますが 

ローカル認証 API を使用して iOS 8 アプリケーションの認証メカニズムとして Touch ID を使用する 2 つの方法はようになりました。 現在これは、ローカルの認証を使用して、リモートで認証することはできません。

まず、新しいキーチェーン アクセス制御リスト (Acl) を使用して既存の Keychain サービスを支援します。 キーチェーンのデータは、ユーザーの指紋認証が成功したロックされていることができます。

次に、LocalAuthentication は、アプリケーションをローカルで認証する 2 つのメソッドを提供します。 開発者が使用する必要があります`CanEvaluatePolicy`デバイスがタッチの ID を受信できるかどうかが決定し、`EvaluatePolicy`認証操作を開始します。

Touch ID と Xamarin.iOS アプリケーションに統合する方法についての詳細についてを参照してください、[概要に touch Id](~/ios/platform/touchid.md)ガイド。

### <a name="document-picker"></a>ドキュメント ピッカー

ドキュメント ピッカーの連携が別のアプリで作成された、インポートし操作、およびそれらをエクスポートするファイルを開くユーザーを許可するユーザーの iCloud ドライブは、もう一度バックアップします。 これには、直感的なワークフロー、およびそのためより優れたエクスペリエンスが大幅に、ユーザーが作成されます。 さらにこの 1 つの手順は、iCloud の同期: 1 つのアプリケーションで行われた変更がすべてのデバイスで一貫して反映するもされます。

ドキュメント ピッカーの詳細にについて説明し、Xamarin.iOS アプリケーションに統合する方法についてを参照して、 [、ドキュメント ピッカーの概要](~/ios/platform/document-picker.md)ガイド。

### <a name="handoff"></a>ハンドオフ

大規模な継続性機能の一部である、ハンドオフは OS X、iOS の統合に向けてさらに一歩を受け取ります。 これには、クロス プラットフォーム AirDrop、呼び出しの iPhone、iPad、Mac、および iPhone からテザリングの向上で SMS を実行する機能が含まれます。

Handoff を使用して、iOS 8 および Yosemite で動作、使用する別のすべてのデバイスにログインして iCloud アカウントが必要です。 Apple の事前インストールされているアプリ、Safari、iWork、マップ、予定表、連絡先など、使用する必要があります。

詳細についてを参照してください、[ハンドオフ](~/ios/platform/handoff.md)ガイド。

## <a name="unified-storyboards"></a>統合ストーリー ボード
iOS 8 が含まれていますが、新しいユーザー インターフェイスを作成するためのメカニズムを使用する方が簡単-統合されたストーリー ボード。 別のハードウェアの画面サイズのすべてをカバーする 1 つにストーリー ボードは、true「一度設計すれば、多くを使用して、」スタイルの高速で応答性の高いビューを作成できます。

IOS8、前に開発者が使用される`UIInterfaceOrientation`縦長と横長のモードを区別する、 `UIInterfaceIdiom` iOS デバイスを区別します。 IOS8 でできなくなった iPhone および iPad デバイスの個別のストーリー ボードを作成するために必要な印刷の向きとデバイスを使用してによって決定されます*サイズ クラス*。

すべてのデバイスは垂直と水平方向の軸の両方で、サイズ クラスによって定義され、iOS 8 でのサイズ クラスの 2 種類があります。

- **通常**-これは、(iPad) などの大きな画面サイズ、または (など、UIScrollView 大きなサイズのという印象を与えるガジェット
- **Compact** -これは比較的小さなデバイス (iPhone) などの。 このサイズでは、デバイスの向きが考慮されます。

2 つの概念をまとめて使用する場合、結果は次の図に示すように、両方、異なる向きで使用できるさまざまな可能なサイズを定義する 2 x 2 グリッドに。

![](introduction-to-ios8-images/image3.png "両方の異なる向きで使用できるさまざまな可能なサイズを定義する 2 x 2 グリッドを表すダイアグラム")
 
サイズ クラスの詳細についてを参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)します。

## <a name="photo-kit"></a>フォト キット
フォト キットは、アプリケーションをシステムのイメージのライブラリを照会し、その内容を表示したり、カスタム ユーザー インターフェイスを作成できる新しいフレームワークです。 これには、多数アルバムやフォルダーなどの資産のコレクションだけでなく、イメージおよびビデオ資産を表すクラスにはが含まれます。

詳細についてを参照してください、 [PhotoKit](~/ios/platform/photokit.md)ガイド。

## <a name="games"></a>ゲーム

<a name="scenekit" />

### <a name="scene-kit"></a>シーンのキット

シーンのキットは、3 D シーン グラフの 3D グラフィックスの操作を簡素化する API です。 OS X 10.8 で初めて導入し、iOS 8 に来たようになりました。 シーンのキットで没入型の 3D 視覚化およびカジュアルの 3D ゲームの作成は必要ありません OpenGL の専門知識。 シーン グラフの一般的な概念に基づき、シーンのキットを抽象 OpenGL やことが非常に簡単に追加する 3D コンテンツをアプリケーションに、OpenGL ES の複雑さです。 ただし、OpenGL の専門家の場合は、シーン キットも OpenGL と直接リンク付けするための優れたサポートしています。 また、物理学などの 3D グラフィックスを補完する多数の機能が含まれていて、いくつかその他の Apple などのフレームワーク コア アニメーション、Core イメージおよび Sprite Kit と非常にうまく連携します。

詳細についてを参照してください、 [SceneKit](~/ios/platform/gaming/scenekit.md)ドキュメント。

<a name="spritekit" />

### <a name="sprite-kit"></a>Sprite Kit

Sprite Kit、apple の 2D ゲーム フレームワークでは、いくつかの興味深い新機能では、iOS 8 および OS X Yosemite が。 シーンのキット、シェーダーのサポート、照明、影、制約、法線マップの生成、および物理学の機能強化との統合が含まれます。 具体的には、物理運動の新機能を使用すると、非常に簡単にゲームにリアルな効果を追加します。

詳細についてを参照してください、 [SpriteKit](~/ios/platform/gaming/spritekit.md)ドキュメント。

## <a name="other-changes"></a>その他の変更
上記で説明した iOS 8 の主要な変更、および Apple は、既存の多くのフレームワークをさらに更新が。 これらを次に説明します。

- **[Core イメージ](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)** : Apple が四角形の領域の検出に対する優れたサポートを追加することで、イメージ処理のフレームワーク上に拡張し、イメージ内での QR コードします。 Mike Bluestein では、彼のブログ投稿の対象に、についての説明をします[iOS 8 でのイメージの検出](https://blog.xamarin.com/image-detection-in-ios-8/)

## <a name="deprecated-apis"></a>非推奨の API
IOS 8 で行われたすべての強化された多数の Api が非推奨とされます。 これらのいくつか記載しています。

- **[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)**  – メソッドとリモート通知を登録するために使用されるプロパティで非推奨とされます。 これらは registerForRemoteNotificationTypes と enabledRemoteNotificationTypes です。
- **[UIViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)**  – メソッドとインターフェイスの向きを記述するためのプロパティに特徴とサイズ クラスは置き換えられています。 参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)これらを使用する方法についての詳細。

- **[UISearchDisplayController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)**  – これは置き換えられました UISearchController iOS8 でします。

## <a name="summary"></a>まとめ
この記事では、apple iOS 8 で導入された新機能のいくつか説明しました。



## <a name="related-links"></a>関連リンク

- [UIKitEnhancements (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/UIKitEnhancements)
- [アプリ拡張機能の概要](~/ios/platform/extensions.md)
- [CloudKit の概要](~/ios/data-cloud/intro-to-cloudkit.md)
- [ドキュメント ピッカーの概要](~/ios/platform/document-picker.md)
- [HealthKit の概要](~/ios/platform/healthkit.md)
- [手動カメラ コントロールの概要](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [Touch Id の概要](~/ios/platform/touchid.md)
- [統合ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)
