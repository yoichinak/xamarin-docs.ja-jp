---
title: 追加の tvOS 10 フレームワークの変更
description: このドキュメントでは、軽微な変更と iOS 10 での既存のフレームワークに加えられた機能強化について説明します。 AVFoundation、AVKit、Core Data、Core Graphics、Foundation、GameKit、GameplayKit などへの更新がについて説明します。
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3eccba01f235382b7969a2f4a122c09ce9b4127b
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832346"
---
# <a name="additional-tvos-10-frameworks-changes"></a>追加の tvOS 10 フレームワークの変更

TvOS の大幅な変更を加え Apple に変更し、既存のフレームワークをいくつかの機能強化 tvOS 10。

<a name="AV-Foundation-Framework" />

## <a name="avfoundation-framework-additions"></a>AVFoundation フレームワークの追加

AVFoundation フレームワークには、次の機能強化が含まれています。

- TvOS 10 で、アプリは実装されなく異なる[AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem)コンテンツの種類に基づいて動作します。 設定するだけです、`Rate`プロパティと AVFoundation の十分なコンテンツはなく失速再生に使用できる、ときに決まります。
 - 新しい`AVPlayerLooper`クラスでは、再生中に、特定のメディアをループ処理しやすくします。

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>AVKit フレームワークの機能強化

AVKit フレームワークには、次の機能強化が含まれています。

- アプリになりましたのスキップの動作を制御、 [AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller)ジェスチャをスキップしていますが、再生リストまたは現在の項目内で事前に次の項目に移動するようにします。

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>コア データ拡張機能

tvOS 10 には、Core Data framework に、次の機能強化が含まれています。

- ルート[NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext)オブジェクトは、同時実行のエラーとシリアル化せずフェッチをサポートしています。
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator)クラスには、SQLite のデータ ストアのプールが管理されます。
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) WAL ジャーナルのモードのサポート、新しいクエリの生成にデータ ストアを SQLite オブジェクト機能の管理オブジェクトのコンテキスト (MOC) を将来をフェッチするための特定のデータベース バージョンに固定でき、トランザクションのエラーが発生します。
- 高レベルを使用して`NSPersistenceContainer`参照に、 `NSPersistentStoreCoordinator`、 [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel)およびその他のコア データの構成リソース。
- いくつかの新しい便利なメソッドに追加された`NSManagedObject`フェッチを実行し、サブクラスを作成しやすきます。

詳細については、Apple を参照してください[コア データ フレームワーク参照](https://developer.apple.com/reference/coredata)します。

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>コア グラフィックスの強化

tvOS 10 には、コア グラフィックスのフレームワークを次の機能強化が含まれています。

- 新しい[CGColorConverterRef](https://developer.apple.com/reference/coregraphics/cgcolorconverterref)クラスは、一連の色の変換の実行に使用できます。

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>コア イメージの機能強化

tvOS 10 では、Core のイメージのフレームワークを次の機能強化。

- `ImageWithExtent`のメソッド、 [CIFilter](https://developer.apple.com/reference/coreimage/cifilter)クラスは、フィルター操作にカスタム処理を挿入するために使用できます。 Core のイメージは出力用のイメージを処理するときに、フィルターの間で指定されたコールバックを呼び出すか、表示します。
- アプリでは、処理の前後に色空間との間に変換することで Core イメージ コンテキストの作業の色空間の外部の色空間内のイメージを処理できるようになりました。
- いくつかのレンダリング パフォーマンスの機能強化が加え`UIImage`で (Core イメージをイメージ ストアによってバックアップされて) いる場合にレンダリング`UIImageView`オブジェクト。 
- `UIImage` 全体のすべてのタグが付けられたオブジェクトは全体域では、色でレンダリングされます`UIImageView`ワイド色をサポートする iOS デバイス上のオブジェクト。
- コア イメージのカーネル コードでは、特定のピクセル出力形式を要求できますようになりました。

さらに、以下の新しいコア イメージ フィルターが追加されました。

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Foundation の機能強化

TvOS 10 の Foundation フレームワークには、次の機能強化が施されました。

- 使用して、新しい[NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval)間隔を比較して、間隔の交差部分のテストのための期間などの日付と時刻の間隔の計算を行うクラス。
- いくつかの新しいプロパティが追加されて、 [NSLocal](https://developer.apple.com/reference/foundation/nslocale)ローカル情報と使用可能な表示形式を取得するクラス。
- 使用して、新しい[NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement)間さまざまなユニットの測定 (UOM) を変換または異なる UOMs 内の値に対して計算を実行するクラス。
- 使用して、新しい[NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter)クラスは、エンドユーザーに表示するためのローカライズされた測定値の書式を設定します。
- 使用して、新しい[NSUnit](https://developer.apple.com/reference/foundation/nsunit)と[NSDimension](https://developer.apple.com/reference/foundation/nsdimension)特定 UOMs を表すためのクラス。

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>GameKit の機能強化

TvOS 10 で GameKit フレームワークには、以下の機能強化が施されました。

- 新しい iCloud 専用のアカウントの種類によって実装されて、 [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer)クラス。
- 新しい[GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession)クラス Game Center への永続的なデータ ストレージを管理するための汎用化されたソリューションを提供します。 `GKGameSession` プレーヤーの一覧を保持し、アプリが担当するフォームを取得するか、プレイヤーの間で交換される方法とタイミングは、参加者の日付が格納されている、実装します。 多くの場合は、ゲーム セッションはターンに基づく既存の一致、リアルタイムの一致または永続的なゲーム保存メソッドを置き換えることができます。

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>GameplayKit の機能強化

TvOS 10 で GameplayKit フレームワークには、以下の機能強化が施されました。

- 手続き型のノイズの生成が追加され、自然に見えるテクスチャのリアリティを強化し、カメラの動きをリアリティを追加し、豊富なゲームの世界を生成するために使用できます。
- 空間パーティション分割を使用して、効率的な検索のゲームの世界のデータをパーティション分割します。
- 新しいモンテカルロ ストラテジスト ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) 可能な移動の徹底的な計算が追加されました。
- デシジョン ツリーの新しい API が追加されました ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree)と[GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) ゲーム構築 AI を強化するためにします。
- 既存のエージェントと、new を使用してパス検索動作を 3D のサポートが追加されました[GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d)と[GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d)クラス。
- 使用して、新しい[GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph)高パフォーマンスで自然なパスを提供するクラス。
- 新しい[GKScene](https://developer.apple.com/reference/gameplaykit/gkscene)と[GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) GameplayKit と SpriteKit をより簡単に組み合わせるクラスの作成。

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>金属製の拡張機能

TvOS 10 の金属製のフレームワークには、次の機能強化が施されました。

- 3D アプリやゲームが使用できるようになりました_テセレーション_複雑なシーンと GPU を使用してジオメトリを効率的に表示するためにします。
- 関数の特殊化を使用すると、シーンにマテリアルとライトの組み合わせの高度に最適化されたコレクションを作成します。
- 金属のパフォーマンスを最適化するためにリソース割り当ての詳細な管理ベースのリソースをヒープを使用してアプリと Memoryless レンダー ターゲットを提供します。

詳細については、Apple を参照してください[メタル プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)します。

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>シェーダーの金属製のパフォーマンスの強化

TvOS 10 のメタル パフォーマンス シェーダー フレームワークには、次の機能強化が施されました。

- 色空間の変換、ニューラル ネットワークの操作などの高に最適化され、データ並列計算を活用するためにアプリを許可するために、金属パフォーマンス シェーダー フレームワークには、多くの新しいカーネルが追加されました。

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>ModelIO の機能強化

TvOS 10 で ModelIO フレームワークには、以下の機能強化が施されました。

- 米国ドルのファイル形式はサポートされています。
- 使用して、新しい`MDLMaterialPropertyGraph`を簡単にランタイムをサポートするクラスがモデルに変更します。
- サポートが追加された距離のフィールドの署名、 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray)クラス。
- 使用して、新しい`MDLLightProbeIrradianceDataSource`光のプローブの配置を支援するクラス。

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>SceneKit の機能強化

TvOS 10 である SceneKit フレームワークには、次の機能強化が施されました。

- SceneKit には、単純な資産の作成をより現実的な結果を新しい物理的にベースのレンダリング (PBR) システムが含まれています。
- 使用して、新しい[SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased)網掛けモデルの製品に現実的な網かけのさまざまな 3 つだけの基本的なプロパティを必要とするときに (`Diffuse`、`Metalness`と`Roughness`)。
- 環境ベースのライティング機能を最適なシェーディング PBR、以降を使用して、`LightingEnvironment`シーン全体のベージュのイメージ ベースの照明を割り当てるプロパティをします。
- 使用して、`IESProfileURL`温度 (ケルビン) での色 (ルーメン) で強度などの実際の値の照明の基本を定義する実際の照明設備をインポートするプロパティ。
- [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera)クラスは HDR 機能と効果を使用して大きいリアリティを提供することができます。 アダプティブ露出を使用すると、自動効果または使用の周辺、カラーの縁取りおよび、ゲームに filmatic 効果を追加するのに色が成績評価を作成します。
- PBR と HDR の両方のカメラの機能は、従来のレンダリング テクニックよりも良い結果を提供し、結果として、SceneKit 今すぐ実行色のすべての計算 (wide カラー デバイス ディスプレイで P3 広色域を使用して) 線形のカラー領域でします。
- SceneKit 色カラー プロファイル情報を読み取ることによってすべての色を一致するようになりました。
- SceneKit では、シェーダーの種類のすべての線形の RGB 色空間の色要素の値を解釈します。
- SceneKit を読み取ってテクスチャ画像のカラー プロファイル情報の調整、ために、この情報が指定されていることをすべてのイメージ資産カタログを使用します。
- 両方の線形のカラー領域のレンダリングと wide 色を指定することで無効にできます、`SCNDisableLinearSpaceRendering`と`SCNDisableWideGamut`アプリのキー`Info.plist`します。
- 任意の多角形霊長 (ファイルから読み込まれたまたはプログラムによって生成されるか) を構築する新しい geometry を指定する[SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon)クラス。

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>SpriteKit の機能強化

TvOS 10 である SpriteKit フレームワークには、次の機能強化が施されました。

- Tilemaps は、2 D、2.5 D、および使用する側のスクロール ゲームの正方形と六角等角投影のタイルの図形をサポートするようになりました、 `SKTileMapMode`、 `SKTileGroup`、`SKTileGroupRule`と`SKTileSet`クラス。
- 使用して、新しい`SKWarpGeometry`を拡大または事実を歪曲クラス[SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode)または[SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode)レンダリングします。 新しい[SKAction](https://developer.apple.com/reference/spritekit/skaction) warp エフェクト間の遷移をアニメーション化するクラスを使用できます。
- カスタムのシェーダーは、属性を指定できます (`SKAttribute`) 属性値を指定することで、シェーダーを使用する各ノードで個別に構成できます (`SKAttributeValue`)。
- [SKView](https://developer.apple.com/reference/spritekit/skview)クラスは、シーンをレンダリングするタイミングと方法をきめ細かく制御できるようにいくつかの新しいメソッドを提供します。

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>UIKit の機能強化

TvOS 10 の UIKit フレームワークには、次の機能強化が施されました。

- さらに非表示項目のフォーカスをサポートする API のフォーカスが強化されました`UIViews`します。 フォーカスをサポートしている項目_する必要があります_実装、`IUIFocusItem`インターフェイス。
- 新しい`UIGraphicsRender`クラスは UIKit レンダリングまたは Core Graphics からビットマップまたは Pdf の作成、オブジェクト指向のメソッドを提供し、非推奨が置き換えられます`UIGraphicsBeginImageContext`メソッド。
- `UIUserInterfaceStyle` (暗色または明色) ユーザー インターフェイス テーマが現在アクティブなを判断するクラスが追加されました。
- 新しいアニメーションを完全にインタラクティブなオブジェクトに基づく、割り込み可能なサポートが追加されました van ジェスチャにリンクします。 Pleas を参照してください Apple の[UIViewAnimating プロトコル リファレンス](https://developer.apple.com/reference/uikit/uiviewanimating)、 [UIViewPropertyAnimator クラス参照](https://developer.apple.com/reference/uikit/uiviewpropertyanimator)、 [UITimingCurveProvider プロトコル リファレンス](https://developer.apple.com/reference/uikit/uitimingcurveprovider)、 [UICubicTimingParameters クラス参照](https://developer.apple.com/reference/uikit/uicubictimingparameters)と[UISpringTimingParameter クラス参照](https://developer.apple.com/reference/uikit/uispringtimingparameters)詳細についてはします。
- 新しい`UIPreviewInteraction`と`UIPreviewInteractionDelegate`アプリがピークと pop 操作のためのカスタム インターフェイスを提供できるようにします。
- 新しい`UIAccessibilityCustomRotor`クラス経由で音声などの支援技術にカスタムのコンテキストに固有の機能を提供するアプリを使用できます。
- 使用して、`UIAccessibilityIsAssistiveTouchRunning`と`UIAccessibilityAssistiveTouchStatusDidChangeNotification`AssistiveTouch が有効になっているかどうかを判断するシンボル。
- 使用して、`UIAccessibilityHearingDevicePairedEar`と`UIAccessibilityHearingDevicePairedEarDidChangeNotification`いずれかの状態を取得するシンボル ペア MFi 皆さまの支援になっています。
- 新しい[UIPasteboard](https://developer.apple.com/reference/uikit/uipasteboard) API (有効期間の制限事項) などの新しいオプションを提供し、共通のクラス型の互換性のあるコンテンツの種類を自動的に宣言されます。
- ラベルで動的な型をサポートするためにテキスト フィールドとテキスト ボックスを使用して、新しい`PreferredFontForTextStyle`のメソッド、`UIFont`クラス。
- かどうかは要素を更新してフォントを決定するときに、デバイス`UIContentSizeCategory`、変更を使用して、`AdjustsFontForContentSizeCategory`のプロパティ、`UIContentSizeCategoryAdjusting`を委任します。
- アプリでは、テキストと背景色などのタブ バー項目のバッジの外観を制御できるようになりました。
- 今すぐ更新コントロールがスクロール ビュー、スクロール ビューのすべてのサブクラスをサポート (など`UICollectionView`)。
- `OpenURL`のメソッド、`UIApplication`クラスは非同期に呼び出されます、オープンが完了した後に呼び出される完了ハンドラーをサポートするようになりました。
- CloudKit の共有を開始し、新しいプロパティを変更`UICloudSharingController`と`UICloudSharingControllerDelegate`クラス。
- プリフェッチされたセルのスクロール エクスペリエンスの向上に利用`UICollectionViews`新しい`UICollectionViewDataSourcePrefetching`を委任します。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10 の新機能新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
