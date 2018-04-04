---
title: その他の iOS 10 フレームワークの変更
description: この記事では、追加、マイナーの変更や iOS 10 用の既存のフレームワークの機能強化について説明します。
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 33852ef62bd00368ef6544d07e60dd6de4c3b7d3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="additional-ios-10-frameworks-changes"></a>その他の iOS 10 フレームワークの変更

_この記事では、追加、マイナーの変更や iOS 10 用の既存のフレームワークの機能強化について説明します。_

## <a name="av-foundation-framework-additions"></a>AV Foundation フレームワークの追加

AVFoundation フレームワークには、次の機能強化が含まれています。

- 10、iOS の開発者がなくなった異なるを実装する[AVPlayerItem](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/)コンテンツの種類に基づいて動作します。 設定するだけ、`Rate`プロパティと AVFoundation が決まります十分なコンテンツの再生に使用できる失速なし。
- 新しい[AVCapturePhotoOutput](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureFileOutput/)クラスは、非推奨置換`AVCaptureStillImageOutput`クラスし、高度な制御を提供して、キャプチャ プロセスの監視で写真のすべてのワークフローを処理するための統一されたメソッドを提供し、ライブの写真と生のキャプチャ形式などの新機能のサポートします。
- 新しい`AVPlayerLooper`クラスでは、再生中に特定のメディアをループしやすくします。
- `AVAssetDownloadURLSession`クラスでは、ダウンロード、および暗号化された HLS ストリームを FairPlay の後で再生します。
- 既定では、 [AVCaptureSession](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureSession/)クラスは、デバイスのハードウェアでサポートされるときに自動的にワイド色、wide 域キャプチャをサポートします。 Apple を参照してください[iOS デバイスの互換性リファレンス](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599)詳細についてはします。

## <a name="avkit-additions"></a>AVKit の追加

AVKit フレームワークが含まれています、新しい`UpdatesNowPlayingInfoCenter`現在再生中のインフォ センターの更新された日時を示すプロパティです。

## <a name="core-data-enhancements"></a>コア データ拡張機能

iOS 10 には、データのコア フレームワークに次の機能強化が含まれています。

- [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) WAL ジャーナル モードのサポート、新しいクエリの生成に SQLite データ ストア オブジェクトの機能で管理されているオブジェクトのコンテキスト (MOC) ピン留めできます将来をフェッチするための特定のデータベース バージョンにし、。トランザクション エラーが発生します。
- ルート[NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/)オブジェクトには、同時実行エラーが発生とシリアル化せずフェッチがサポートしています。
- [NSPersistentStoreCoordinator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/)クラス SQLite データ ストアのプールが保持されます。
- いくつかの新しい便利なメソッドに追加された`NSManagedObject`フェッチを実行し、サブクラスを作成しやすきます。
- 使用して、高度な`NSPersistenceContainer`参照に、 `NSPersistentStoreCoordinator`、 [NSManagedObjectModel](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectModel/)およびその他のコア データの構成リソース。

詳細については、Apple を参照してください[コア データ フレームワーク参照](https://developer.apple.com/reference/coredata)です。

## <a name="core-image-enhancements"></a>Core イメージの機能強化

iOS 10 では、Core のイメージのフレームワークに次の機能強化を加えます。

- 開発者は前に、と処理後の色領域との間に変換することで、Core のイメージのコンテキストの作業の色空間の外部の色空間内のイメージを今すぐ処理できます。
- A8 または A9 の Cpu を使用して iOS デバイスの場合は、生のイメージの形式がサポートされています。 Core のイメージは、今すぐには組み込み iSight カメラからの生のイメージをデコードまたはサード パーティ製カメラからのサポートの機能を提供します。 使用して、`FilterWithImageData`または`FilterWithImageURL`のメソッド、 [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) RAW 画像を処理するクラス。
- いくつかのレンダリング パフォーマンス強化が加えられた`UIImage`で (Core のイメージのイメージ ストアに支えられて) ときにレンダリング`UIImageView`オブジェクト。 
- `UIImage` オブジェクト タグが付けられた wide 域で、全体にわたる域色をレンダリングする`UIImageView`広色をサポートする iOS デバイス上のオブジェクト。
- コア イメージ カーネル コードでは、特定のピクセル出力形式を要求できますようになりました。
- `ImageWithExtent`のメソッド、 [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/)クラスは、フィルター操作にカスタム処理を挿入するために使用できます。 Core のイメージは出力用のイメージを処理するときに、フィルターの間で指定されたコールバックを呼び出すか、表示されます。

さらに、以下の新しい Core イメージ フィルターが追加されました。

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>コア アニメーションの追加

新しい iOS 10、コア モーション フレームワークが含まれています pedometer イベント通知を受信する高速、リアルタイムの一時停止と再開の実行中の追跡のユーザーのアプリを有効にします。 使用して、 [CMPedometer](https://developer.xamarin.com/api/type/CoreMotion.CMPedometer/)フォア グラウンドまたはバック グラウンドの pedometer イベントを登録します。

## <a name="foundation-enhancements"></a>Foundation の機能強化

Ios 10 Foundation フレームワークには、次の機能強化が加えられました。

- 使用して、新しい[NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter)クラスをエンドユーザーに表示するローカライズされた測定値の書式を設定します。
- 使用して、新しい[NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval)の間隔を比較して、間隔の交差部分のテストの期間などの日付と時刻の間隔の計算を作成するクラス。
- 使用して、新しい[NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement)間でさまざまなユニットのメジャー (出荷単位) を変換するか、異なる UOMs 内の値に対して計算を実行するクラス。

- 使用して、新しい[NSUnit](https://developer.apple.com/reference/foundation/nsunit)と[NSDimension](https://developer.apple.com/reference/foundation/nsdimension)特定 UOMs を表すクラス。
- いくつかの新しいプロパティが追加されて、 [NSLocal](https://developer.apple.com/reference/foundation/nslocale)クラスをローカルの情報と使用可能な表示形式を取得します。

## <a name="gamekit-enhancements"></a>GameKit の機能強化

Ios 10 GameKit フレームワークには、次の機能強化が施されました。

- **Game Center アプリ**は廃止され iOS から削除します。 アプリが GameKit を使用している場合、_必要があります_スコアボードなど GameKit 機能の表示に独自のインターフェイスを提供します。 
- 新しい iCloud 専用のアカウントの種類が実装されて、 [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer)クラスです。
- 新しい[GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession)クラスがゲーム センターで永続的なデータ ストレージを管理するために一般化されたソリューションを提供します。 `GKGameSession` プレーヤーの一覧を保持し、アプリが実装を担当する参加要素の日付が格納されているになっていること、取得またはプレーヤーの間で交換する方法とタイミング。 さまざまな状況では、ゲーム セッションはターンに基づく既存の一致、リアルタイム一致または永続的なゲーム メソッド保存を置き換えることができます。

## <a name="gameplaykit-enhancements"></a>GameplayKit の機能強化

Ios 10 GameplayKit フレームワークには、次の機能強化が施されました。

- 使用して、新しい[GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph)高パフォーマンスで自然なパスを提供するクラス。
- 手続き型のノイズの生成が追加されましたし、自然なテクスチャのリアルさを向上させる、カメラの動きをよりリアルを追加、および豊富なゲームの世界を生成するために使用できます。
- 効率的な検索のゲームの世界のデータをパーティション分割は、空間をパーティション分割を使用します。
- 新しいモンテカルロ ストラテジスト ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) 可能な移動の排他的な計算が追加されました。
- 既存のエージェントと、new を使用してパス検索の動作に 3D のサポートが追加されました[GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d)と[GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d)クラスです。
- 新しい[GKScene](https://developer.apple.com/reference/gameplaykit/gkscene)と[GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent)クラス make GameplayKit と SpriteKit より簡単に結合します。
- デシジョン ツリーの新しい API が追加されました ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree)と[GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) ゲーム構築 AI を強化するためにします。

## <a name="healthkit-enhancements"></a>HealthKit の機能強化

Ios 10 HealthKit フレームワークには、次の機能強化が施されました。

- 天気の種類の新しいメタデータ キーが追加されました (など`HKWeatherConditionClear`と`HKWeatherConditionCloudy`) とトレーニング型 (など`HKWorkoutActivityTypeFlexibility`と`HKWorkoutActivityTypeWheelchairRunPace`) が追加されました。
- 新しい`HKCDADocument`形式の文書を治療ドキュメント アーキテクチャ (CDA) を表すクラスが追加されました。
- 使用して、新しい[HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration)クラスを指定する、`ActivityType`と`LocationType`トレーニングのです。
- 新しい[HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject)と`WheelchairUse`のメソッド、 [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore)手押し車を操作するためのクラスが追加されましたに関連したヘルス データ。

## <a name="homekit-enhancements"></a>HomeKit の機能強化

Ios 10 HomeKit フレームワークには、次の機能強化が施されました。

- 新しいサービスと特性が追加されました。
- 共有ユーザーのアクセス許可の付属品のリモート アクセスを提供し、オートメーションのトリガーを実行し、有効にする HomeKit ハブとして機能する iPad を構成することができます。
- カメラとドアベル アクセサリのサポートが追加されました。
- 複数のコンテキストと構成は、accessories の用意されています。

参照してください、 [HomeKit 概要](~/ios/platform/homekit.md)詳細についてはドキュメントです。

## <a name="metal-enhancements"></a>金属製の拡張機能

Ios 10 金属製のフレームワークには、次の機能強化が施されました。

- 3D アプリやゲームを使用できるよう_テセレーション_効率的に複雑なシーンと GPU を使用してジオメトリをレンダリングします。
- 金属のパフォーマンスを最適化するためにリソース割り当ての粒度の細かい制御ベースのリソースをヒープを使用してアプリと Memoryless レンダー ターゲットを提供します。
- 高度に最適化材料とシーンのライト組み合わせ関数のコレクションを作成するのにには、関数の特殊化を使用します。

詳細については、Apple を参照してください[メタル プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)です。

## <a name="modelio-enhancements"></a>ModelIO の機能強化

Ios 10 ModelIO フレームワークには、次の機能強化が施されました。

 - USD ファイル形式はサポートされています。
 - 署名にサポートが追加されました距離フィールド、 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray)クラスです。
 - 使用して、新しい`MDLLightProbeIrradianceDataSource`ライト プローブの配置を支援するクラス。
 - 使用して、新しい`MDLMaterialPropertyGraph`を簡単にランタイムをサポートするクラスがモデルに変更します。

## <a name="photos-enhancements"></a>写真の機能強化

Ios 10 写真フレームワークには、次の機能強化が施されました。

- 使用して、 [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput)と[CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput)編集を実行して新しい Core のイメージのプロセッサ機能を利用するクラス。
- ライブの写真の編集が、写真のフレームワークをサポートするアプリ、および写真編集の拡張機能を利用できます (写真やカメラの内部で使用するためのアプリ)。
- 使用して、新しい[PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext)ビデオとの両方に引き続き Live 写真のコンテンツの編集を適用するクラス。

## <a name="replaykit-enhancements"></a>ReplayKit の機能強化

Ios 10 ReplayKit フレームワークには、次の機能強化が施されました。

- 使用して、 [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder)、 [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller)と[RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller)のブロードキャストをサポートするクラスは、サード パーティでメディアを記録サイトです。
- アプリで ReplayKit サード パーティの放送サービスをサポートするために、ブロードキャスト UI およびブロードキャストのアップロードの拡張機能が必要です。

## <a name="scenekit-enhancements"></a>SceneKit の機能強化

Ios 10 SceneKit フレームワークには、次の機能強化が施されました。

- [SCNCamera](https://developer.xamarin.com/api/type/SceneKit.SCNCamera/)クラスは HDR 機能と効果を使用して大きいよりリアルを提供することができます。 アダプティブ露出を使用すると、自動効果または使用周辺、カラーの縁取りおよびゲームに fillmatic 効果を追加するのに成績評価の色を作成できます。
- SceneKit には、単純な資産の作成をより現実的な結果を新しい物理的に基づくレンダリング (PBR) システムが含まれています。
- 使用して、新しい[SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased)網掛けをモデルの製品に現実的な網かけのさまざまな 3 つの基本的なプロパティを必要とするときに (`Diffuse`、`Metalness`と`Roughness`)。
- 環境に基づく光源の動作を最適な網掛け PBR、以降を使用して、`LightingEnvironment`シーン全体にイメージ ベースの照明を割り当てるプロパティをします。
- 使用して、 `IESProfileURL` (ルーメン) の強度と色温度 (ケルビン) などの実際の値に基づいてプロパティを付けて光を定義する実際の電灯をインポートします。
- PBR と HDR の両方のカメラ機能は、従来のレンダリング テクニックよりも良い結果を提供するをその結果、SceneKit 今すぐ計算を実行すべて色 (wide カラー デバイス ディスプレイで P3 色域を使用して) 線形のカラー スペースです。
- SceneKit 色カラー プロファイル情報を読み取ることですべての色を一致するようになりました。
- SceneKit は、カラー シェーダーの種類のすべての線形 RGB 色空間に値を解釈します。
- 領域のレンダリングを色線形の両方を無効状態でのワイド色を指定して、`SCNDisableLinearSpaceRendering`と`SCNDisableWideGamut`アプリの内のキー`Info.plist`です。
- 任意の多角形霊長 (またはのいずれかのファイルから読み込まれたをプログラムで生成) をビルドに新しい geometry を指定する[SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon)クラスです。
- SceneKit を読み取り、テクスチャのイメージのカラー プロファイル情報を調整するため、すべてのイメージの資産カタログを使用して、この情報が提供されることを確認します。

## <a name="spritekit-enhancements"></a>SpriteKit の機能強化

Ios 10 SpriteKit フレームワークには、次の機能強化が施されました。

- カスタムのシェーダーは、属性を指定できます (`SKAttribute`) 属性値を指定することによって、シェーダーを使用する各ノードで個別に構成できます (`SKAttributeValue`)。
- Tilemaps は、2 D、2.5 D およびを使用する側のスクロールのゲームの正方形、六角および等角タイル図形をサポートするようになりました、 `SKTileMapMode`、 `SKTileGroup`、`SKTileGroupRule`と`SKTileSet`クラスです。
- 使用して、新しい`SKWarpGeometry`を拡大または変形クラス[SKSpriteNode](https://developer.xamarin.com/api/type/SpriteKit.SKSpriteNode/)または[SKEffectNode](https://developer.xamarin.com/api/type/SpriteKit.SKEffectNode/)レンダリングします。 新しい[SKAction](https://developer.xamarin.com/api/type/SpriteKit.SKAction/) warp エフェクト間の切り替え効果をアニメーション化するクラスを使用できます。
- [SKView](https://developer.xamarin.com/api/type/SpriteKit.SKView/)クラス シーンが表示されるタイミングと方法を詳細に制御するためのいくつかの新しいメソッドを提供します。

## <a name="scrollview-enhancements"></a>ScrollView の機能強化

IOS 10.3 ScrollView コントロールには、次の機能強化が施されました。

- `UIScrollView` 追加されました、`IndexDisplayMode`プロパティとして、ユーザーがスクロール中にインデックスを表示する方法を制御する、`UIScrollViewIndexDisplayMode`の。
    - `Automatic` でインデックスの表示は、オペレーティング システムによって制御されます。
    - `AlwaysHidden` -インデックスが常に非表示しますの表示。

参照してください、 [iOSTenThree サンプル](https://developer.xamarin.com/samples/monotouch/iOS10/iOSTenThree)使用法をします。

## <a name="uikit-enhancements"></a>UIKit の機能強化

Ios 10 UIKit フレームワークには、次の機能強化が施されました。

- 新しい[UIPasteboard](https://developer.xamarin.com/api/type/UIKit.UIPasteboard/) API (有効期間の制限事項) などの新しいオプションを提供し、一般的なクラス型に互換性のあるコンテンツの種類を自動的に宣言されます。
- 新しいアニメーションを完全に対話型、オブジェクトに基づく、割り込み可能なサポートが追加され、ジェスチャにリンクすることができます。 Pleas を参照してください Apple の[UIViewAnimating Protocol Reference](https://developer.apple.com/reference/uikit/uiviewanimating)、 [UIViewPropertyAnimator クラス参照](https://developer.apple.com/reference/uikit/uiviewpropertyanimator)、 [UITimingCurveProvider Protocol Reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider)、 [UICubicTimingParameters クラス参照](https://developer.apple.com/reference/uikit/uicubictimingparameters)と[UISpringTimingParameter クラス参照](https://developer.apple.com/reference/uikit/uispringtimingparameters)詳細についてはします。
- 新しい`UIPreviewInteraction`と`UIPreviewInteractionDelegate`ピークおよびポップ操作にカスタムのインターフェイスを提供する developer アプリを許可します。
- 新しい`UIAccessibilityCustomRotor`クラス経由で音声などの支援技術にカスタムのコンテキストに固有の機能を提供するアプリを使用できます。
- 使用して、`UIAccessibilityIsAssistiveTouchRunning`と`UIAccessibilityAssistiveTouchStatusDidChangeNotification`AssistiveTouch が有効になっているかどうかを決定する記号。
- 使用して、`UIAccessibilityHearingDevicePairedEar`と`UIAccessibilityHearingDevicePairedEarDidChangeNotification`いずれかの状態を取得するシンボル ペア MFi 聴覚を支援します。
- ラベルの動的な型をサポートするためにテキスト フィールドとテキスト ボックスを使用して、新しい`PreferredFontForTextStyle`のメソッド、`UIFont`クラスです。
- かどうか、要素は、そのフォントを更新する必要がありますを決定するときに、デバイスの`UIContentSizeCategory`、変更を使用して、`AdjustsFontForContentSizeCategory`のプロパティ、`UIContentSizeCategoryAdjusting`を委任します。
- `OpenURL`のメソッド、`UIApplication`クラスは非同期的に呼び出すし、完了ハンドラー [開く] アクションが完了した後に呼び出されるようになりました。
- CloudKit の共有を開始し、new を使用してプロパティを変更`UICloudSharingController`と`UICloudSharingControllerDelegate`クラスです。
- プリフェッチされたセルのスクロールのエクスペリエンスを向上させるの活用`UICollectionViews`に新しい`UICollectionViewDataSourcePrefetching`を委任します。
- 開発者は、タブ バー アイテム (テキストと背景色) などのバッジの外観を制御ようになりましたことができます。
- コントロールの更新 は、すべてのスクロール ビューとスクロールのビューのサブクラスとしてサポートされています (など`UICollectionView`)。

## <a name="webkit-enhancements"></a>WebKit の機能強化

Ios 10 WebKit フレームワークには、次の機能強化が施されました。

- ピークと pop サポートが追加されましたが、`WKWebView`クラスです。 使用して、`ShouldPreviewElement`かどうかは、指定された web ビューがプレビューを表示する必要がありますを調べます。


## <a name="related-links"></a>関連リンク

- [iOS 10 サンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [IOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
