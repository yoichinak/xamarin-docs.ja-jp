---
title: IOS 10 フレームワークのその他の変更
description: このドキュメントでは、iOS 10 の既存のフレームワークに加えられた軽微な変更と拡張について説明し、これらの更新プログラムを Xamarin iOS で使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: b5dbfe4de21cd359e89d84db310a1e1905ddbb1a
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434596"
---
# <a name="additional-ios-10-frameworks-changes"></a>IOS 10 フレームワークのその他の変更

_この記事では、iOS 10 の既存のフレームワークに対する追加、軽微な変更、または拡張機能について説明します。_

## <a name="av-foundation-framework-additions"></a>AV Foundation フレームワークの追加

AVFoundation framework には、次の機能強化が含まれています。

- IOS 10 では、開発者はコンテンツの種類に基づいてさまざまな [AVPlayerItem](xref:AVFoundation.AVPlayerItem) 動作を実装する必要がなくなりました。 プロパティを設定するだけ `Rate` で、停止することなく再生できるコンテンツが十分にあるかどうかが AVFoundation によって判断されます。
- 新しい [AVCapturePhotoOutput](xref:AVFoundation.AVCaptureFileOutput) クラスは、非推奨のクラスを置き換え、 `AVCaptureStillImageOutput` キャプチャプロセスの高度な制御と監視、およびライブ写真や生のキャプチャ形式などの新機能のサポートを提供することで、すべての写真ワークフローを処理するための統一された方法を提供します。
- 新しい `AVPlayerLooper` クラスを使用すると、再生中に特定のメディアを簡単にループできます。
- クラスでは、 `AVAssetDownloadURLSession` FairPlay の暗号化された HLS ストリームをダウンロードし、後で再生することができます。
- 既定では、 [AVCaptureSession](xref:AVFoundation.AVCaptureSession) クラスは、デバイスのハードウェアでサポートされている場合に、ワイド色のワイド色のキャプチャを自動的にサポートします。 詳細については、Apple の [IOS デバイス互換性のリファレンス](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) を参照してください。

## <a name="avkit-additions"></a>AVKit の追加

AVKit フレームワークに新しいプロパティが含まれるようになりました `UpdatesNowPlayingInfoCenter` 。これにより、現在プレイ中の情報センターを更新する必要があることが示されます。

## <a name="core-data-enhancements"></a>コアデータの機能強化

iOS 10 には、コアデータフレームワークに対する次の機能強化が含まれています。

- WAL ジャーナルモードで SQLite データストアを使用する [NSManagedObjectContext](xref:CoreData.NSManagedObjectContext) オブジェクトでは、新しいクエリ生成機能がサポートされています。この機能を使用すると、マネージオブジェクトコンテキスト (MOC) を特定のデータベースバージョンに固定して、将来のフェッチやエラー処理を行うことができます。
- ルート [NSManagedObjectContext](xref:CoreData.NSManagedObjectContext) オブジェクトは、シリアル化を使用しない同時エラーおよびフェッチをサポートしています。
- [NSPersistentStoreCoordinator](xref:CoreData.NSPersistentStoreCoordinator)クラスは、SQLite データストアのプールを保持します。
- `NSManagedObject`フェッチの実行とサブクラスの作成を容易にするために、いくつかの新しい便利なメソッドが追加されました。
- 高レベルを使用して `NSPersistenceContainer` `NSPersistentStoreCoordinator` 、、 [NSManagedObjectModel](xref:CoreData.NSManagedObjectModel) 、その他のコアデータ構成リソースを参照します。

詳細については、Apple の [Core Data Framework リファレンス](https://developer.apple.com/reference/coredata)を参照してください。

## <a name="core-image-enhancements"></a>コアイメージの機能強化

iOS 10 では、コアイメージフレームワークが次のように強化されています。

- 開発者は、処理の前後に色空間を変換することによって、コアイメージコンテキストの作業色空間の外にある色空間のイメージを処理できるようになりました。
- A8 または A9 の Cpu を使用する iOS デバイスでは、RAW イメージ形式がサポートされるようになりました。 コアイメージでは、組み込みの iSight カメラから、またはサードパーティ製カメラから生画像をデコードできるようになりました。 `FilterWithImageData` `FilterWithImageURL` [Cifilter](xref:CoreImage.CIFilter)クラスのメソッドまたはメソッドを使用して、未加工のイメージを処理します。
- `UIImage`オブジェクトのレンダリング (コアイメージイメージストアによってサポートされている場合) に対して、いくつかのレンダリングパフォーマンスが向上してい `UIImageView` ます。
- `UIImage` ワイド色のタグが付けられたオブジェクトは、ワイド `UIImageView` 色をサポートする iOS デバイス上のオブジェクトで、幅域外色として表示されます。
- コアイメージカーネルコードは、特定のピクセル出力形式を要求できるようになりました。
- `ImageWithExtent` [Cifilter](xref:CoreImage.CIFilter)クラスのメソッドを使用して、フィルター操作にカスタム処理を挿入できます。 コアイメージは、出力または表示のためにイメージを処理するときに、フィルター間で指定されたコールバックを呼び出します。

さらに、次の新しいコアイメージフィルターが追加されました。

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>コアモーションの追加

IOS 10 の新機能であるコアモーションフレームワークには pedometer イベントが含まれています。これにより、アプリは、実行中に追跡を一時停止および再開するためのリアルタイムのリアルタイム通知を受け取ることができます。 [CMPedometer](xref:CoreMotion.CMPedometer)を使用して、フォアグラウンドまたはバックグラウンドの pedometer イベントに登録します。

## <a name="foundation-enhancements"></a>Foundation の機能強化

IOS 10 の Foundation framework には、次の機能強化が加えられています。

- 新しい [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) クラスを使用して、ローカライズされた測定形式をエンドユーザーに表示するように書式設定します。
- 新しい [Nsdateinterval](https://developer.apple.com/reference/foundation/nsdateinterval) クラスを使用して、間隔の比較と間隔の交差部分のテストを行うために、期間などの日付と時間の間隔を計算します。
- 新しい [Nsmeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) クラスを使用して異なる測定単位 (UOM) 間で変換するか、異なる uoms の値に対して計算を実行します。

- 特定の UOMs を表すために、新しい [Nsunit](https://developer.apple.com/reference/foundation/nsunit) クラスと [nsunit](https://developer.apple.com/reference/foundation/nsdimension) クラスを使用します。
- ローカル情報と使用可能な表示形式を取得するために、いくつかの新しいプロパティが [nslocal](https://developer.apple.com/reference/foundation/nslocale) クラスに追加されました。

## <a name="gamekit-enhancements"></a>お持ちのキットの機能強化

IOS 10 の作成キットフレームワークには、次の機能強化が行われています。

- **Game Center アプリ**は非推奨となり、iOS から削除されました。 アプリで使用する場合は、独自のインターフェイスを提示して、スコアボードなどのユーザーキット機能を表示 _する必要があり_ ます。
- 新しい iCloud 専用のアカウントの種類は、 [Gkcloudplayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) クラスによって実装されています。
- 新しい [Gkq&a session](https://developer.apple.com/reference/gamekit/gkgamesession) クラスは、Game Center で永続的なデータストレージを管理するための一般化されたソリューションを提供します。 `GKGameSession` プレーヤーのリストを保持します。アプリは、参加者の日付の格納、取得、交換の方法とタイミングを実装する役割を担います。 多くの場合、ゲームセッションでは、既存のターンベースの一致、リアルタイムの一致、または永続的なゲーム保存方法を置き換えることができます。

## <a name="gameplaykit-enhancements"></a>お持ちのおプレイキットの機能強化

IOS 10 では、次のような機能強化が行われています。

- 新しい [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) クラスを使用して、高パフォーマンスの自然なパスを提供します。
- 手続き型のノイズ生成が追加され、自然なテクスチャのリアリティを向上させるために使用できます。また、カメラの動きにリアリティを加え、豊富なゲームワールドを生み出します。
- 空間パーティション分割を使用して、効率的な検索のためにゲームのワールドデータを分割します。
- すべての移動計算を可能にするために、新しいモンテカルロストラテジスト ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) が追加されました。
- 新しい [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) クラスと [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) クラスを使用して、既存のエージェントおよびパス検索動作に3d サポートが追加されました。
- 新しい [Gkscene](https://developer.apple.com/reference/gameplaykit/gkscene) クラスと [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) クラスを使用すると、これまでにないようにすることができます。
- ゲームを構築する AI を強化するために、新しいデシジョンツリー API が追加されました ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) と [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode))。

## <a name="healthkit-enhancements"></a>HealthKit の機能強化

IOS 10 の HealthKit フレームワークには、次の機能強化が行われています。

- 気象の種類 (やなど) に新しいメタデータキーが追加され `HKWeatherConditionClear` `HKWeatherConditionCloudy` 、トレーニングの種類 ( `HKWorkoutActivityTypeFlexibility` やなど `HKWorkoutActivityTypeWheelchairRunPace` ) が追加されました。
- 新しい `HKCDADocument` クラスが追加され、臨床ドキュメントアーキテクチャ (CDA) 形式のドキュメントを表します。
- 新しい [Hkworkspace Outconfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) クラスを使用して、 `ActivityType` トレーニングのおよびを指定し `LocationType` ます。
- 車椅子に[HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) `WheelchairUse` 関連する正常性データを操作するために、新しい HKWheelchairUseObject と[HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore)クラスのメソッドが追加されました。

## <a name="homekit-enhancements"></a>ホームキットの機能強化

IOS 10 のホームキットフレームワークには、次の機能強化が行われています。

- 新しいサービスと特性が追加されました。
- IPad は、リモートのアクセサリアクセスを提供するホームキットハブとして機能し、automation トリガーを実行し、共有ユーザーのアクセス許可を有効にするように構成できます。
- カメラと doorbell のアクセサリのサポートが追加されました。
- アクセサリ向けに、より多くのコンテキストと構成が用意されています。

詳細については、「 [ホームキット](~/ios/platform/homekit.md) のドキュメントの概要」を参照してください。

## <a name="metal-enhancements"></a>金属の機能強化

IOS 10 の金属フレームワークには、次のような機能強化が加えられています。

- 3D アプリやゲームでは、 _テセレーション_ を使用して、複雑なシーンとジオメトリを GPU 経由で効率的にレンダリングできるようになりました。
- リソースの割り当てをきめ細かく制御することにより、リソースヒープと Memoryless レンダーターゲットを使用して金属ベースのアプリのパフォーマンスを最適化できます。
- 関数の特殊化を使用して、シーンに対して高度に最適化されたマテリアルおよびライトの組み合わせ関数のコレクションを作成します。

詳細については、Apple の [金属のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)を参照してください。

## <a name="modelio-enhancements"></a>ModelIO の機能強化

IOS 10 の ModelIO フレームワークには、次の機能強化が行われています。

- 米国ドルのファイル形式がサポートされるようになりました。
- 符号付き距離フィールドのサポートが [Mdlvoxelarray](https://developer.apple.com/reference/modelio/mdlvoxelarray) クラスに追加されました。
- 新しいクラスを使用すると、 `MDLLightProbeIrradianceDataSource` プローブの配置を簡単に行うことができます。
- 新しい `MDLMaterialPropertyGraph` クラスを使用すると、モデルに対する実行時の変更を簡単にサポートできます。

## <a name="photos-enhancements"></a>写真の機能強化

IOS 10 の Photos フレームワークには、次のような機能強化が加えられています。

- [Ciimageprocessorinput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput)クラスと[Ciimageprocessorinput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput)クラスを使用して、新しいコアイメージプロセッサ機能を利用して編集を行います。
- 写真フレームワークと写真編集拡張機能をサポートするアプリ (写真およびカメラアプリ内で使用) でライブフォト編集ができるようになりました。
- 新しい [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) クラスを使用して、ライブ写真のビデオと静止コンテンツの両方に編集を適用します。

## <a name="replaykit-enhancements"></a>ReplayKit の機能強化

IOS 10 の ReplayKit フレームワークには、次の機能強化が行われています。

- [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller)クラスと[RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller)クラスを使用し[て、サード](https://developer.apple.com/reference/replaykit/rpscreenrecorder)パーティのサイトからの記録されたメディアのブロードキャストをサポートします。
- アプリケーションで ReplayKit サードパーティのブロードキャストサービスをサポートするには、ブロードキャスト UI と Broadcast Upload の拡張機能が必要です。

## <a name="scenekit-enhancements"></a>SceneKit の機能強化

IOS 10 の SceneKit フレームワークには、次の機能強化が行われています。

- [Scncamera](xref:SceneKit.SCNCamera)クラスは、HDR の特徴と効果を使用して、よりリアリティを高めることができます。 アダプティブ露出を使用して自動効果を作成するか、vignetting、color fringing、色の調整を使用して、ゲームに fillmatic 効果を追加します。
- SceneKit には、より簡単なアセット作成により、より現実的な結果を得るために、新しい物理的に基づく表示 (.PBR) システムが追加されました。
- 新しい [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) シェーディングモデルを使用して、さまざまな現実的な網掛け効果を製品に追加し、3つの基本プロパティ ( `Diffuse` 、および) のみを必要 `Metalness` と `Roughness` します。
- .PBR シェーディングは環境ベースの光源で最適に機能するため、プロパティを使用して、 `LightingEnvironment` イメージベースの光源をシーン全体に割り当てます。
- プロパティを使用して、 `IESProfileURL` 照度 (ルーメン) や色温度 (ケルビン単位) などの実際の値に基づいて光源を定義する実際の照明器具をインポートします。
- SceneKit と HDR の両方のカメラ機能は、従来の表示手法よりも優れた結果を提供します。結果として、では、すべての色の計算を線形の色空間で実行するようになりました (ワイド色のデバイスディスプレイで P3 の色域外を使用)。
- SceneKit color プロファイル情報を読み取って、すべての色に一致するようになりました。
- SceneKit は、すべてのシェーダーの種類の線形 RGB カラー空間で色コンポーネントの値を解釈します。
- `SCNDisableLinearSpaceRendering` `SCNDisableWideGamut` アプリのでキーとキーを指定することにより、線形色空間のレンダリングとワイド色の両方を無効にできます `Info.plist` 。
- 新しい [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/documentation/scenekit/scngeometryprimitivetype/scngeometryprimitivetypepolygon) クラスで geometry を指定するために、任意の多角形の霊長類 (ファイルから読み込まれるか、プログラムによって生成される) を構築します。
- テクスチャイメージでカラープロファイル情報の読み取りと調整を SceneKit するため、すべてのイメージに対してアセットカタログを使用して、この情報が確実に提供されるようにします。

## <a name="spritekit-enhancements"></a>SpriteKit の機能強化

IOS 10 の SpriteKit フレームワークには、次の機能強化が行われています。

- カスタムシェーダーは `SKAttribute` 、属性値 () を指定することによって、シェーダーを使用する各ノードで個別に構成できる属性 () を提供でき `SKAttributeValue` ます。
- Tilemaps は、、、およびの各クラスを使用して、2d、2.5 d、およびサイドスクロールゲーム用の正方形、六角、および等角投影のタイル図形をサポートするようになりました `SKTileMapMode` `SKTileGroup` `SKTileGroupRule` `SKTileSet` 。
- 新しいクラスを使用して、 `SKWarpGeometry` [SKSpriteNode](xref:SpriteKit.SKSpriteNode) または [SKEffectNode](xref:SpriteKit.SKEffectNode) のレンダリングを伸縮またはデフォルメします。 新しい [Skaction](xref:SpriteKit.SKAction) クラスは、ワープ効果間の遷移をアニメーション化するために使用できます。
- [Skview](xref:SpriteKit.SKView)クラスには、シーンをレンダリングするタイミングと方法をきめ細かく制御できる新しいメソッドがいくつか用意されています。

## <a name="scrollview-enhancements"></a>ScrollView の機能強化

IOS 10.3 の ScrollView コントロールには、次の機能強化が行われています。

- `UIScrollView` 次に、 `IndexDisplayMode` のようにユーザーがスクロールしているときのインデックスの表示方法を制御するプロパティを含め `UIScrollViewIndexDisplayMode` ます。
  - `Automatic` -インデックスの表示は OS によって制御されます。
  - `AlwaysHidden` -インデックスの表示は常に非表示になります。

使用方法については、 [IOSTenThree サンプル](/samples/xamarin/ios-samples/ios10-iostenthree) を参照してください。

## <a name="uikit-enhancements"></a>UIKit の機能強化

IOS 10 の UIKit フレームワークには、次のような機能強化が加えられています。

- 新しい [Uipasteboard](xref:UIKit.UIPasteboard) API は、新しいオプション (有効期間の制限など) を提供し、共通クラス型に対して互換性のあるコンテンツの種類を自動的に宣言します。
- 新しい完全対話型のオブジェクトベースの中断可能なアニメーションのサポートが追加され、ジェスチャにリンクできるようになりました。 詳細については、Apple の [Uiviewanimating プロトコルのリファレンス](https://developer.apple.com/reference/uikit/uiviewanimating)、 [uiviewpropertyアニメータークラスリファレンス](https://developer.apple.com/reference/uikit/uiviewpropertyanimator)、 [UITimingCurveProvider Protocol reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider)、 [UICubicTimingParameters class](https://developer.apple.com/reference/uikit/uicubictimingparameters) reference、および [UISpringTimingParameter クラスリファレンス](https://developer.apple.com/reference/uikit/uispringtimingparameters) を参照してください。
- 新しい `UIPreviewInteraction` とを `UIPreviewInteractionDelegate` 使用すると、開発者アプリはピーク操作とポップ操作のためのカスタムインターフェイスを提供できます。
- 新しい `UIAccessibilityCustomRotor` クラスを使用すると、アプリは、ボイスオーバーなどの補助的なテクノロジに対して、コンテキスト固有のカスタム機能を提供できます。
- `UIAccessibilityIsAssistiveTouchRunning` `UIAccessibilityAssistiveTouchStatusDidChangeNotification` AssistiveTouch が有効になっているかどうかを判断するには、記号と記号を使用します。
- と記号を使用して、 `UIAccessibilityHearingDevicePairedEar` `UIAccessibilityHearingDevicePairedEarDidChangeNotification` ペアになっている mfi 聴覚援助の状態を取得します。
- ラベルの動的な型をサポートするために、テキストフィールドとテキストボックスはクラスの新しいメソッドを使用し `PreferredFontForTextStyle` `UIFont` ます。
- デバイスの変更時に要素のフォントを更新する必要があるかどうかを判断するには `UIContentSizeCategory` 、 `AdjustsFontForContentSizeCategory` デリゲートのプロパティを使用し `UIContentSizeCategoryAdjusting` ます。
- `OpenURL`クラスのメソッド `UIApplication` が非同期に呼び出され、open アクションの完了後に呼び出される完了ハンドラーがサポートされるようになりました。
- CloudKit の共有を開始し、新しいクラスとクラスを使用してそのプロパティを変更し `UICloudSharingController` `UICloudSharingControllerDelegate` ます。
- 新しいデリゲートを使用したのスクロールエクスペリエンスを向上させるために、プリフェッチされたセルを活用し `UICollectionViews` `UICollectionViewDataSourcePrefetching` ます。
- 開発者は、タブバー項目 (テキストや背景色など) のバッジの外観を制御できるようになりました。
- すべてのスクロールビューおよびスクロールビューのサブクラス (など) で更新コントロールがサポートされるようになりました `UICollectionView` 。

## <a name="webkit-enhancements"></a>WebKit の機能強化

IOS 10 の WebKit フレームワークには、次の機能強化が行われています。

- Peek と pop のサポートがクラスに追加されました `WKWebView` 。 メソッドを使用して、 `ShouldPreviewElement` 特定の web ビューでプレビューを表示するかどうかを決定します。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS10)
- [IOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)