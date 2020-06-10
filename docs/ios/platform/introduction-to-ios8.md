---
title: iOS 8 の概要
description: IOS 8 では、Apple は楽しみな気持ちと満足させるの開発者に多数の新しいフレームワークと Api を提供しています。 このガイドでは、これらの新しい Api を紹介し、iOS 8 が開発者とユーザーの両方にもたらすメリットについて説明します。
ms.prod: xamarin
ms.assetid: 33AD66C0-3743-49FE-9DCE-88ED3A16BA63
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: 1fae83f60f819da9767e14612a7f778dc49ddf52
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84564631"
---
# <a name="introduction-to-ios-8"></a>iOS 8 の概要

_IOS 8 では、Apple は楽しみな気持ちと満足させるの開発者に多数の新しいフレームワークと Api を提供しています。このガイドでは、これらの新しい Api を紹介し、iOS 8 が開発者とユーザーの両方にもたらすメリットについて説明します。_

iOS 7 では、最初の iPhone OS から直接、ユーザーと開発者が想定していた iOS ユーザーインターフェイス全体が視覚的に変更されました。 IOS 8 では、開発者向けに多くのフレームワークを提供することで、これを引き続き使用します。これにより、ユーザーは自分の iPhone から直接、ほぼすべての機能を制御できます。 たとえば、正常性と適合性は*HealthKit*を使用して分析できます。パスコードは、 *localauthentication*を使用する生体認証によって obsolescent されます。*アプリ拡張*機能では、サードパーティ製アプリ間の通信チャネルが開かれます。また、ホーム*キット*を使用すると、家を将来の自宅に変えることができます。 

IOS 7 がユーザーのために使用されていた場合、iOS 8 では、おいしいの新しいツールのすべての範囲を備えた delighting 開発者に重点を置いています。 

このガイドでは、Xamarin の開発者向けの新しい Api について説明します。  

IOS 8 では非推奨とされているいくつかの Api もあります。これについては、このドキュメントの最後で説明します。

## <a name="requirements"></a>必要条件

Visual Studio for Mac で iOS 8 アプリを作成するには、次のものが必要です。

- **Xcode 7 と ios 8 以降**– Apple の最新の Xcode と ios api は、開発者のコンピューターにインストールして構成する必要があります。
- **Visual Studio for Mac** –ユーザーデバイスで Visual Studio for Mac の最新バージョンをインストールして構成する必要があります。
- **ios 8 デバイスまたはシミュレーター** –テスト用に ios 8 の最新バージョンを実行している ios デバイスです。

## <a name="home-and-leisure"></a>自宅とレジャー

iOS 8 では、ホームキットと HealthKit を使用して、Apple と iOS デバイスを自宅の中核に直接送り込むことができました。 このセクションでは、これらの新しいフレームワークのしくみと、それらを Xamarin の iOS アプリケーションに統合する方法について説明します。

## <a name="homekit"></a>HomeKit

IPhone からアプライアンスを制御することは、新しいテクノロジのアプリケーションではありません。多くの接続されたホーム製品は、iOS アプリを使用して制御できます。 しかし、home Kit は、ホームオートメーションデバイス用の共通プロトコルを昇格させ、特定の製造元が iHome、Honeywell、およびなどのパブリック API を利用できるようにすることで、一歩進んでいます。 ユーザーにとって、これは、1つのアプリケーション内から、自宅のほとんどすべての側面をシームレスに制御できることを意味します。 これは、お客様が、、または入れ子になった警報を使用していることを認識しているとは無関係です。 ユーザーは、多数のスマートホームプロセスを1つにまとめて、"シーン" にすることもできます。

ホームキットを使用すると、サードパーティ製アプリおよび Siri は、アクセサリを検出し、個人のホーム構成データベースに追加し、このデータを編集して操作し、アクセサリとそのサービスと通信してアクションを実行することができます。

### <a name="configuration"></a>構成

次の図は、ホームキットのアクセサリの構成の基本階層を示しています。

![](introduction-to-ios8-images/image1.png "This diagram shows the basic hierarchy of the configuration of HomeKit accessories")

ホームキットの使用を開始するには、開発者は、プロビジョニングプロファイルに [ホームキット] サービスが選択されていることを確認する必要があります。 Apple では、Xcode 用のホームキットシミュレーターアドインも開発者に提供しています。 これは、 [Apple Developer Center](https://developer.apple.com/downloads/index.action)のの下にあり `Hardware IO Tools for Xcode` ます。 

詳細については、「[ホームキット](~/ios/platform/homekit.md)ガイド」を参照してください。

## <a name="healthkit"></a>HealthKit

HealthKit は、iOS 8 で導入されたフレームワークであり、正常性に関する情報を提供する一元化された、連携し、セキュリティで保護されたデータストアを提供します。 オペレーティングシステムによって、正常性情報のプライバシーとセキュリティが確保され、ヘルスアプリがユーザーのダッシュボードになります。 アプリケーションでは、ユーザーのアクセス許可を使用して、さまざまな正常性情報の読み取りと書き込みを行うことができます。

Xamarin. iOS アプリでこれを使用する方法の詳細については、 [HealthKit の概要に](~/ios/platform/healthkit.md)関するガイドを参照してください。

## <a name="extending-iphone-functionality"></a>IPhone 機能の拡張
IOS8 を使用すると、開発者は、アプリを使用できるユーザーをより細かく制御できるようになり、サードパーティのアプリ間のよりオープンな通信のための機能が強化されます。 アプリ拡張機能やドキュメントピッカーなどの機能では、Apple のエコシステムでアプリケーションを使用する方法について、さまざまな可能性があります。

### <a name="app-extensions"></a>アプリの拡張機能
アプリの拡張機能は、サードパーティのアプリが相互に通信するための方法です。 高いセキュリティ標準を維持し、サンドボックス化されたアプリの整合性を維持するために、この通信はアプリケーション間で直接行われません。 代わりに、中央の*拡張機能*によって実行されます。

アプリ拡張機能を作成するための最初の手順は、正しい拡張ポイントを定義することです。これは、適切な Api の動作と可用性を確保するために重要です。 Visual Studio for Mac でアプリ拡張機能を作成するには、ソリューションに新しいプロジェクトを追加して、既存のアプリケーションに追加します。

[**新しいプロジェクト**] ダイアログで、 **C#**  >  **iOS**  >  **Unified API**  >  次のスクリーンショットに示すように、C# iOS Unified API**拡張機能**に移動します。

![](introduction-to-ios8-images/image2.png "Creating a new extension")

[新しいプロジェクト] ダイアログには、アプリ拡張機能を作成するための7つの新しいプロジェクトテンプレートが用意されています。次に説明します。 拡張機能の多くは、iOS の他の新しい Api (ドキュメントピッカーなど) に関連しています。

- **アクション**–開発者は、ユーザーが特定のタスクを実行できるように、独自のカスタムアクションボタンを作成できます。
- **カスタムキーボード**–開発者は、独自のカスタムのキーボードを追加することにより、組み込みの Apple キーボードの範囲にを追加できます。 一般的なキーボードである Swype は、キーボードを iOS に持ち込むために使用されます。
- **ドキュメントピッカー** –これには、ユーザーがアプリケーションのサンドボックスの外部にあるファイルにアクセスできるようにする、ドキュメントピッカービューコントローラーが含まれています。
- **ドキュメントピッカーファイルプロバイダー** –これにより、ドキュメントピッカーを使用してファイルの安全なストレージが提供されます。
- **写真の編集**–写真アプリケーションで既に Apple によって提供されているフィルターと編集ツールを拡張し、ユーザーが写真を編集するときにより多くのオプションを制御できるようにします。
- **今日**–アプリケーションでは、通知センターの今日のセクションにウィジェットを表示する機能が提供されています。

Xamarin でのアプリ拡張機能の使用の詳細については、[アプリ拡張機能の概要](~/ios/platform/extensions.md)に関するガイドを参照してください。

### <a name="touch-id"></a>Touch ID

タッチ ID は、ユーザーを認証する手段として iOS 7 で導入されました。パスコードに似ています。 ただし、デバイスのロック解除、アプリストアの使用、iTunes の使用、iCloud キーチェーンの認証のみに制限されていました。 

ローカル認証 API を使用した iOS 8 アプリケーションでは、タッチ ID を認証メカニズムとして使用する2つの方法があります。 現在、ローカル認証を使用してリモートで認証することはできません。

まず、新しいキーチェーン Access Control リスト (Acl) を使用して、既存のキーチェーンサービスを支援します。 キーチェーンデータは、ユーザーの指紋の認証が成功したときにロックを解除することができます。

次に、LocalAuthentication は、アプリケーションをローカルで認証する2つの方法を提供します。 開発者は、を使用して `CanEvaluatePolicy` 、デバイスがタッチ ID を受け入れることができるかどうかを判断し、次に認証操作を開始する必要があり `EvaluatePolicy` ます。

タッチ ID の詳細と、それを Xamarin. iOS アプリケーションに統合する方法については、 [xamarin の「TOUCH id と FACE id](~/ios/platform/touch-id-face-id.md) 」を参照してください。

### <a name="document-picker"></a>ドキュメント ピッカー

ドキュメントピッカーはユーザーの iCloud ドライブと連携して、ユーザーが別のアプリで作成されたファイルを開いて、それらをインポートして操作し、再びエクスポートできるようにします。 これにより、直感的なワークフローが作成され、ユーザーにとってはるかに優れたエクスペリエンスが得られます。 iCloud の同期ではこれをさらに一歩進めます。1つのアプリケーションで行った変更もすべてのデバイスで一貫して反映されます。

ドキュメントピッカーの詳細を確認し、それを Xamarin iOS アプリケーションに統合する方法については、[ドキュメントピッカーガイドの概要](~/ios/platform/document-picker.md)に関するページを参照してください。

### <a name="handoff"></a>Handoff

より大規模な継続性機能の一部であるハンドオフは、OS X と iOS の統合についてさらに一歩進んでいます。 これには、クロスプラットフォームのエアドロップ、iPhone 呼び出しを行う機能、iPad と Mac での SMS、iPhone からのテザリングの機能強化が含まれます。

ハンドオフは iOS 8 と、ユーザーが使用するすべてのデバイスにログインするための iCloud アカウントを必要とします。 Safari、iWork、Maps、カレンダー、連絡先など、プレインストールされているほとんどの Apple アプリで動作します。

詳細については、[ハンドオフ](~/ios/platform/handoff.md)ガイドを参照してください。

## <a name="unified-storyboards"></a>統合ストーリーボード
iOS 8 には、ユーザーインターフェイスを作成するための、統合されたストーリーボードという新しい簡単な使用機構が含まれています。 1つのストーリーボードを使用してさまざまなハードウェア画面サイズをすべてカバーすることで、高速および応答性の高いビューを、真の "デザイン1、多くの使用" スタイルで作成できます。

IOS8 より前の開発者は、 `UIInterfaceOrientation` 縦モードと横モードを区別し、iOS デバイスを区別するために使用されていました `UIInterfaceIdiom` 。 IOS8 では、iPhone と iPad デバイス用に個別のストーリーボードを作成する必要がなくなりました。向きとデバイスは、*サイズクラス*を使用して決定されます。

すべてのデバイスはサイズクラスによって、垂直軸と水平軸の両方で定義されます。また、iOS 8 のサイズクラスには次の2種類があります。

- [**標準**]-大きなサイズ (iPad など) または大きなサイズの印象を与えるガジェット (UIScrollView など) の場合は、
- **Compact** -これは、より小さなデバイス (iPhone など) を対象としています。 このサイズでは、デバイスの向きが考慮されます。

2つの概念が一緒に使用されている場合は、次の図に示すように、異なる向きの両方で使用できるさまざまなサイズを定義する2つの x 2 グリッドが生成されます。

![](introduction-to-ios8-images/image3.png "A diagram representing the 2 x 2 grid that defines the different possible sizes that can be used in both the differing orientations")

サイズクラスの詳細については、「[統合ストーリーボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)」を参照してください。

## <a name="photo-kit"></a>フォトキット
Photo Kit は、アプリケーションがシステムイメージライブラリに対してクエリを実行し、その内容を表示および変更するためのカスタムユーザーインターフェイスを作成できるようにする新しいフレームワークです。 これには、イメージとビデオ資産を表す多数のクラスと、アルバムやフォルダーなどの資産のコレクションが含まれます。

詳細については、 [Photokit](~/ios/platform/photokit.md)のガイドを参照してください。

## <a name="games"></a>ゲーム

<a name="scenekit"></a>

### <a name="scene-kit"></a>シーンキット

シーンキットは、3d グラフィックスを簡単に操作できる3D シーングラフ API です。 これは OS X 10.8 で初めて導入されましたが、現在は iOS 8 に付属しています。 シーンキットでは、イマーシブ3D 視覚エフェクトとカジュアル3D ゲームを作成する際に、OpenGL に関する専門知識は必要ありません。 シーンキットでは、一般的なシーングラフの概念を基にして、OpenGL と OpenGL の複雑さが解消されるため、アプリケーションに3D コンテンツを簡単に追加できます。 しかし、OpenGL の専門家であれば、シーンキットは OpenGL と直接結び付けることをサポートしています。 また、物理などの3D グラフィックスを補完する多くの機能が含まれており、コアアニメーション、コアイメージ、スプライトキットなど、他のいくつかの Apple framework と非常によく統合されています。

詳細については、 [SceneKit](~/ios/platform/gaming/scenekit.md)のドキュメントを参照してください。

<a name="spritekit"></a>

### <a name="sprite-kit"></a>スプライトキット

Apple の 2D game framework であるスプライトキットには、iOS 8 と OS X の新しい機能がいくつかあります。 これには、シーンキット、シェーダーサポート、照明、影、制約、法線マップの生成、および物理的な機能強化との統合が含まれます。 特に、新しい物理機能により、現実的な効果をゲームに追加することが非常に簡単になります。

詳細については、 [SpriteKit](~/ios/platform/gaming/spritekit.md)のドキュメントを参照してください。

## <a name="other-changes"></a>その他の変更
また、上記の iOS 8 の主な変更点に加えて、Apple では、多くの既存のフレームワークをさらに更新しています。 これらの詳細を以下に示します。

- **[コアイメージ](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)**– Apple は、イメージ処理フレームワークで拡張されています。これは、四角形領域の検出のサポートを強化し、画像内に QR コードを追加することによって行われます。 Mike Bluestein は、 [iOS 8 でのイメージの検出](https://blog.xamarin.com/image-detection-in-ios-8/)に関するブログ投稿でこれを考察しています

## <a name="deprecated-apis"></a>非推奨の API
IOS 8 で加えられたすべての機能強化により、いくつかの Api が非推奨となりました。 これらの一部を以下に示します。

- **[Uiapplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)** –リモート通知の登録に使用されるメソッドとプロパティは非推奨となりました。 これらは registerForRemoteNotificationTypes と enabledRemoteNotificationTypes です。
- **[Uiviewcontroller](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)** –特徴クラスとサイズクラスは、インターフェイスの向きを示すために使用されるメソッドとプロパティに置き換えられました。 これらの使用方法の詳細については、「[統合ストーリーボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)」を参照してください。

- **[Uisearchdisplaycontroller](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)** – IOS8 の uisearchcontroller に置き換えられました。

## <a name="summary"></a>まとめ
この記事では、iOS 8 で Apple によって導入された新機能について説明しました。

## <a name="related-links"></a>関連リンク

- [UIKitEnhancements (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-uikitenhancements)
- [アプリ拡張機能の概要](~/ios/platform/extensions.md)
- [CloudKit の概要](~/ios/data-cloud/intro-to-cloudkit.md)
- [ドキュメントピッカーの概要](~/ios/platform/document-picker.md)
- [HealthKit の概要](~/ios/platform/healthkit.md)
- [手動カメラコントロールの概要](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [Xamarin でタッチ ID と顔 ID を使用する](~/ios/platform/touch-id-face-id.md)
- [統合されたストーリーボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)
