---
title: 追加の iOS 10 フレームワークの変更
description: このドキュメントは、軽微な変更と iOS 10 での既存のフレームワークに加えられた機能強化について説明し、させる方法について説明します Xamarin.iOS でこれらの更新プログラムを使用します。
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: ac255baf44951518b29112d2903950039a80ee53
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831222"
---
# <a name="additional-ios-10-frameworks-changes"></a>追加の iOS 10 フレームワークの変更

_この記事では、追加、マイナー変更や iOS 10 用の既存のフレームワークの機能強化について説明します。_

## <a name="av-foundation-framework-additions"></a>AV Foundation フレームワークの追加

AVFoundation フレームワークには、次の機能強化が含まれています。

- Ios 10 で、開発者がなくなった異なるを実装する[AVPlayerItem](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/)コンテンツの種類に基づいて動作します。 設定するだけです、`Rate`プロパティと AVFoundation の十分なコンテンツはなく失速再生に使用できる、ときに決まります。
- 新しい[AVCapturePhotoOutput](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureFileOutput/)クラスは、非推奨と置き換えられます`AVCaptureStillImageOutput`クラスし、高度な制御を提供し、キャプチャ プロセスの監視によって写真のすべてのワークフローを処理するための統一されたメソッドを提供し、ライブの写真と生のキャプチャ形式などの新機能のサポートします。
- 新しい`AVPlayerLooper`クラスでは、再生中に、特定のメディアをループ処理しやすくします。
- `AVAssetDownloadURLSession`クラスでは、ダウンロードし、暗号化された HLS ストリームの FairPlay の後で再生します。
- 既定で、 [AVCaptureSession](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureSession/)クラスは、デバイスのハードウェアをサポートしているときに自動的に全体色、全体にわたるすべてのキャプチャをサポートします。 Apple を参照してください。 [iOS デバイスの互換性のリファレンス](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599)の詳細。

## <a name="avkit-additions"></a>AVKit の追加

AVKit フレームワークが含まれています、新しい`UpdatesNowPlayingInfoCenter`プロパティを指定すると、現在再生中のインフォ センターを更新する必要があります。

## <a name="core-data-enhancements"></a>コア データ拡張機能

iOS 10 には、Core Data framework に、次の機能強化が含まれています。

- [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) WAL ジャーナルのモードのサポート、新しいクエリの生成にデータ ストアを SQLite オブジェクト機能の管理オブジェクトのコンテキスト (MOC) を将来をフェッチするための特定のデータベース バージョンに固定でき、トランザクションのエラーが発生します。
- ルート[NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/)オブジェクトは、同時実行のエラーとシリアル化せずフェッチをサポートしています。
- [NSPersistentStoreCoordinator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/)クラスには、SQLite のデータ ストアのプールが管理されます。
- いくつかの新しい便利なメソッドに追加された`NSManagedObject`フェッチを実行し、サブクラスを作成しやすきます。
- 高レベルを使用して`NSPersistenceContainer`参照に、 `NSPersistentStoreCoordinator`、 [NSManagedObjectModel](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectModel/)およびその他のコア データの構成リソース。

詳細については、Apple を参照してください[コア データ フレームワーク参照](https://developer.apple.com/reference/coredata)します。

## <a name="core-image-enhancements"></a>コア イメージの機能強化

iOS 10 では、Core のイメージのフレームワークを次の機能強化。

- 開発者は、処理の前後に色空間との間に変換することで、Core のイメージのコンテキストの作業の色空間の外部の色空間内のイメージを処理できますようになりました。
- A8 または A9 の Cpu を使用して iOS デバイスの場合は、RAW イメージ形式がサポートされています。 Core のイメージは、組み込み iSight カメラから画像をデコードまたはサード パーティのカメラから今すぐサポートを提供します。 使用して、`FilterWithImageData`または`FilterWithImageURL`のメソッド、 [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) RAW 画像を処理するクラス。
- いくつかのレンダリング パフォーマンスの機能強化が加え`UIImage`で (Core イメージをイメージ ストアによってバックアップされて) いる場合にレンダリング`UIImageView`オブジェクト。 
- `UIImage` 全体のすべてのタグが付けられたオブジェクトは全体域では、色でレンダリングされます`UIImageView`ワイド色をサポートする iOS デバイス上のオブジェクト。
- コア イメージのカーネル コードでは、特定のピクセル出力形式を要求できますようになりました。
- `ImageWithExtent`のメソッド、 [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/)クラスは、フィルター操作にカスタム処理を挿入するために使用できます。 Core のイメージは出力用のイメージを処理するときに、フィルターの間で指定されたコールバックを呼び出すか、表示します。

さらに、以下の新しいコア イメージ フィルターが追加されました。

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>コア アニメーションの追加

新しいコア アニメーション フレームワークには iOS 10 では、アプリで高速でリアルタイムの通知を受け取るユーザーの一時停止と再開の実行中に追跡できるようにする歩数計イベントが含まれています。 使用して、 [CMPedometer](https://developer.xamarin.com/api/type/CoreMotion.CMPedometer/)フォア グラウンドまたはバック グラウンドの歩数計イベントを登録します。

## <a name="foundation-enhancements"></a>Foundation の機能強化

IOS 10 の Foundation フレームワークには、次の機能強化が施されました。

- 使用して、新しい[NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter)クラスは、エンドユーザーに表示するためのローカライズされた測定値の書式を設定します。
- 使用して、新しい[NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval)間隔を比較して、間隔の交差部分のテストのための期間などの日付と時刻の間隔の計算を行うクラス。
- 使用して、新しい[NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement)間さまざまなユニットの測定 (UOM) を変換または異なる UOMs 内の値に対して計算を実行するクラス。

- 使用して、新しい[NSUnit](https://developer.apple.com/reference/foundation/nsunit)と[NSDimension](https://developer.apple.com/reference/foundation/nsdimension)特定 UOMs を表すためのクラス。
- いくつかの新しいプロパティが追加されて、 [NSLocal](https://developer.apple.com/reference/foundation/nslocale)ローカル情報と使用可能な表示形式を取得するクラス。

## <a name="gamekit-enhancements"></a>GameKit の機能強化

IOS 10 で GameKit フレームワークには、次の機能強化が施されました。

- **Game Center アプリ**非推奨し、iOS から削除されています。 アプリが GameKit を使用する場合、_する必要があります_GameKit 機能 (ランキングなど) の表示に独自のインターフェイスを提供します。 
- 新しい iCloud 専用のアカウントの種類によって実装されて、 [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer)クラス。
- 新しい[GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession)クラス Game Center への永続的なデータ ストレージを管理するための汎用化されたソリューションを提供します。 `GKGameSession` プレーヤーのリストを保持し、アプリが実装を担当する参加要素の日付が格納されているになっていること、取得またはプレーヤー間で交換される方法とタイミングです。 多くの場合は、ゲーム セッションはターンに基づく既存の一致、リアルタイムの一致または永続的なゲーム保存メソッドを置き換えることができます。

## <a name="gameplaykit-enhancements"></a>GameplayKit の機能強化

IOS 10 で GameplayKit フレームワークには、次の機能強化が施されました。

- 使用して、新しい[GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph)高パフォーマンスで自然なパスを提供するクラス。
- 手続き型のノイズの生成が追加され、自然に見えるテクスチャのリアリティを強化し、カメラの動きをリアリティを追加し、豊富なゲームの世界を生成するために使用できます。
- 空間パーティション分割を使用して、効率的な検索のゲームの世界のデータをパーティション分割します。
- 新しいモンテカルロ ストラテジスト ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) 可能な移動の徹底的な計算が追加されました。
- 既存のエージェントと、new を使用してパス検索動作を 3D のサポートが追加されました[GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d)と[GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d)クラス。
- 新しい[GKScene](https://developer.apple.com/reference/gameplaykit/gkscene)と[GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) GameplayKit と SpriteKit をより簡単に組み合わせるクラスの作成。
- デシジョン ツリーの新しい API が追加されました ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree)と[GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) ゲーム構築 AI を強化するためにします。

## <a name="healthkit-enhancements"></a>HealthKit の機能強化

IOS 10 で HealthKit フレームワークには、次の機能強化が施されました。

- 天気の種類の新しいメタデータのキーが追加されています (など`HKWeatherConditionClear`と`HKWeatherConditionCloudy`) とトレーニングの種類 (など`HKWorkoutActivityTypeFlexibility`と`HKWorkoutActivityTypeWheelchairRunPace`) が追加されています。
- 新しい`HKCDADocument`臨床ドキュメント アーキテクチャ (CDA) を表すため、ドキュメントを書式設定されたクラスが追加されました。
- 使用して、新しい[HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration)クラスを指定する、`ActivityType`と`LocationType`トレーニングの。
- 新しい[HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject)と`WheelchairUse`のメソッド、 [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore)手押し車を操作するためのクラスが追加されました正常性データに関連します。

## <a name="homekit-enhancements"></a>HomeKit の機能強化

IOS 10 で HomeKit フレームワークには、次の機能強化が施されました。

- 新しいサービスと特性が追加されました。
- HomeKit アクセサリのリモート アクセスを提供し、automation のトリガーを実行し、有効にするハブの共有ユーザーのアクセス許可として動作する、iPad を構成できます。
- カメラとドアベルのアクセサリ サポートが追加されました。
- [アクセサリ] のコンテキストと構成の詳細が用意されています。

参照してください、 [HomeKit の概要](~/ios/platform/homekit.md)詳細についてはドキュメントです。

## <a name="metal-enhancements"></a>金属製の拡張機能

IOS 10 で金属製のフレームワークには、次の機能強化が施されました。

- 3D アプリやゲームが使用できるようになりました_テセレーション_複雑なシーンと GPU を使用してジオメトリを効率的に表示するためにします。
- 金属のパフォーマンスを最適化するためにリソース割り当ての詳細な管理ベースのリソースをヒープを使用してアプリと Memoryless レンダー ターゲットを提供します。
- 関数の特殊化を使用すると、シーンにマテリアルとライトの組み合わせの高度に最適化されたコレクションを作成します。

詳細については、Apple を参照してください[メタル プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)します。

## <a name="modelio-enhancements"></a>ModelIO の機能強化

IOS 10 で ModelIO フレームワークには、次の機能強化が施されました。

- 米国ドルのファイル形式はサポートされています。
- サポートが追加された距離のフィールドの署名、 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray)クラス。
- 使用して、新しい`MDLLightProbeIrradianceDataSource`光のプローブの配置を支援するクラス。
- 使用して、新しい`MDLMaterialPropertyGraph`を簡単にランタイムをサポートするクラスがモデルに変更します。

## <a name="photos-enhancements"></a>写真の機能強化

IOS 10 で写真のフレームワークには、次の機能強化が施されました。

- 使用して、 [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput)と[CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput)クラスの編集を実行する新しいコア イメージ プロセッサ機能を活用するためにします。
- ライブの写真の編集が写真のフレームワークをサポートするアプリ、および写真編集拡張機能を利用できます (写真やカメラの内部で使用するためのアプリ)。
- 使用して、新しい[PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext)ビデオとの両方にまだコンテンツをライブの写真の編集を適用するクラス。

## <a name="replaykit-enhancements"></a>ReplayKit の機能強化

IOS 10 で ReplayKit フレームワークには、次の機能強化が施されました。

- 使用して、 [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder)、 [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller)と[RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller)のブロードキャストをサポートするクラスをサード パーティによってメディアの記録サイト。
- Broadcast UI およびブロードキャストのアップロードの拡張機能は、アプリで ReplayKit サード パーティのブロードキャスト サービスをサポートするために必要です。

## <a name="scenekit-enhancements"></a>SceneKit の機能強化

IOS 10 である SceneKit フレームワークには、次の機能強化が施されました。

- [SCNCamera](xref:SceneKit.SCNCamera)クラスは HDR 機能と効果を使用して大きいリアリティを提供することができます。 アダプティブ露出を使用すると、自動効果または使用の周辺、カラーの縁取りおよび、ゲームに fillmatic 効果を追加するのに色が成績評価を作成します。
- SceneKit には、単純な資産の作成をより現実的な結果を新しい物理的にベースのレンダリング (PBR) システムが含まれています。
- 使用して、新しい[SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased)網掛けモデルの製品に現実的な網かけのさまざまな 3 つだけの基本的なプロパティを必要とするときに (`Diffuse`、`Metalness`と`Roughness`)。
- 環境ベースのライティング機能を最適なシェーディング PBR、以降を使用して、`LightingEnvironment`イメージ ベースの光源をシーン全体に割り当てるプロパティをします。
- 使用して、`IESProfileURL`照明を定義する実際の照明設備をインポートするプロパティ (ルーメン) の輝度 (ケルビン) での色温度などの実際の値に基づきます。
- PBR と HDR の両方のカメラの機能は、従来のレンダリング テクニックよりも良い結果を提供し、結果として、SceneKit 今すぐ実行色のすべての計算 (wide カラー デバイス ディスプレイで P3 広色域を使用して) 線形のカラー領域でします。
- SceneKit 色カラー プロファイル情報を読み取ることによってすべての色を一致するようになりました。
- SceneKit では、シェーダーの種類のすべての線形の RGB 色空間の色要素の値を解釈します。
- 両方の線形のカラー領域のレンダリングと wide 色を指定することで無効にできます、`SCNDisableLinearSpaceRendering`と`SCNDisableWideGamut`アプリのキー`Info.plist`します。
- 任意の多角形霊長 (ファイルから読み込まれたまたはプログラムによって生成されるか) を構築する新しい geometry を指定する[SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon)クラス。
- SceneKit を読み取ってテクスチャ画像のカラー プロファイル情報の調整、ために、この情報が指定されていることをすべてのイメージ資産カタログを使用します。

## <a name="spritekit-enhancements"></a>SpriteKit の機能強化

IOS 10 である SpriteKit フレームワークには、次の機能強化が施されました。

- カスタムのシェーダーは、属性を指定できます (`SKAttribute`) 属性値を指定することで、シェーダーを使用する各ノードで個別に構成できます (`SKAttributeValue`)。
- Tilemaps は、2 D、2.5 D、および使用する側のスクロール ゲームの正方形と六角等角投影のタイルの図形をサポートするようになりました、 `SKTileMapMode`、 `SKTileGroup`、`SKTileGroupRule`と`SKTileSet`クラス。
- 使用して、新しい`SKWarpGeometry`を拡大または事実を歪曲クラス[SKSpriteNode](https://developer.xamarin.com/api/type/SpriteKit.SKSpriteNode/)または[SKEffectNode](https://developer.xamarin.com/api/type/SpriteKit.SKEffectNode/)レンダリングします。 新しい[SKAction](https://developer.xamarin.com/api/type/SpriteKit.SKAction/) warp エフェクト間の遷移をアニメーション化するクラスを使用できます。
- [SKView](https://developer.xamarin.com/api/type/SpriteKit.SKView/)クラスは、シーンをレンダリングするタイミングと方法をきめ細かく制御できるようにいくつかの新しいメソッドを提供します。

## <a name="scrollview-enhancements"></a>ScrollView の機能強化

IOS 10.3 の ScrollView コントロールには、次の機能強化が施されました。

- `UIScrollView` 追加されました、`IndexDisplayMode`プロパティとして、ユーザーがスクロール中にインデックスを表示する方法を制御するため、`UIScrollViewIndexDisplayMode`の。
    - `Automatic` 表示インデックスは、OS によって制御されます。
    - `AlwaysHidden` 表示インデックスは常に表示されません。

参照してください、 [iOSTenThree サンプル](https://developer.xamarin.com/samples/monotouch/iOS10/iOSTenThree)の使用量。

## <a name="uikit-enhancements"></a>UIKit の機能強化

IOS 10 の UIKit フレームワークには、次の機能強化が施されました。

- 新しい[UIPasteboard](xref:UIKit.UIPasteboard) API (有効期間の制限事項) などの新しいオプションを提供し、共通のクラス型の互換性のあるコンテンツの種類を自動的に宣言されます。
- 新しいアニメーションを完全にインタラクティブなオブジェクトに基づく、割り込み可能なサポートが追加され、ジェスチャにリンクすることができます。 Apple を参照してください[UIViewAnimating プロトコル リファレンス](https://developer.apple.com/reference/uikit/uiviewanimating)、 [UIViewPropertyAnimator クラス参照](https://developer.apple.com/reference/uikit/uiviewpropertyanimator)、 [UITimingCurveProvider プロトコル リファレンス](https://developer.apple.com/reference/uikit/uitimingcurveprovider)、 [UICubicTimingParameters クラス参照](https://developer.apple.com/reference/uikit/uicubictimingparameters)と[UISpringTimingParameter クラス参照](https://developer.apple.com/reference/uikit/uispringtimingparameters)詳細についてはします。
- 新しい`UIPreviewInteraction`と`UIPreviewInteractionDelegate`開発者アプリがピークと pop 操作のためのカスタム インターフェイスを提供できるようにします。
- 新しい`UIAccessibilityCustomRotor`クラス経由で音声などの支援技術にカスタムのコンテキストに固有の機能を提供するアプリを使用できます。
- 使用して、`UIAccessibilityIsAssistiveTouchRunning`と`UIAccessibilityAssistiveTouchStatusDidChangeNotification`AssistiveTouch が有効になっているかどうかを判断するシンボル。
- 使用して、`UIAccessibilityHearingDevicePairedEar`と`UIAccessibilityHearingDevicePairedEarDidChangeNotification`いずれかの状態を取得するシンボル ペア MFi 皆さまの支援になっています。
- ラベルで動的な型をサポートするためにテキスト フィールドとテキスト ボックスを使用して、新しい`PreferredFontForTextStyle`のメソッド、`UIFont`クラス。
- かどうかは、要素がそのフォントを更新する必要がありますを決定するときに、デバイスの`UIContentSizeCategory`、変更を使用して、`AdjustsFontForContentSizeCategory`のプロパティ、`UIContentSizeCategoryAdjusting`デリゲートします。
- `OpenURL`のメソッド、`UIApplication`クラスは、非同期的が呼び出され、完了ハンドラー オープン操作が完了した後に呼び出されるようになりました。
- CloudKit の共有を開始し、新しいプロパティを変更`UICloudSharingController`と`UICloudSharingControllerDelegate`クラス。
- プリフェッチされたセルのスクロール エクスペリエンスの向上に利用`UICollectionViews`新しい`UICollectionViewDataSourcePrefetching`を委任します。
- 開発者は、(テキストと背景の色) などのタブ バー項目のバッジの外観を制御できるようになりました。
- コントロールの更新は、すべてのスクロール ビュー、スクロール ビューのサブクラスではサポートされています (など`UICollectionView`)。

## <a name="webkit-enhancements"></a>WebKit の機能強化

IOS 10 で WebKit フレームワークには、次の機能強化が施されました。

- ピークと pop のサポートが追加されましたが、`WKWebView`クラス。 使用して、`ShouldPreviewElement`メソッドを特定の web ビューがプレビュー表示を決定します。


## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [IOS 10 の新機能新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
