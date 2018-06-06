---
title: 追加 tvOS 10 フレームワークの変更
description: このドキュメントでは、軽微な変更と 10、iOS での既存のフレームワークの拡張機能について説明します。 更新プログラム AVFoundation AVKit、基本データ、コア グラフィックス、Foundation、GameKit、GameplayKit、詳細を説明します。
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 326fb6a23048ba3d3e1d33f42c8da2fff8544c25
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788876"
---
# <a name="additional-tvos-10-frameworks-changes"></a>追加 tvOS 10 フレームワークの変更

TvOS に主要な変更だけでなく Apple 修正といくつかの既存のフレームワークの機能強化に向けた tvOS 10 です。

<a name="AV-Foundation-Framework" />

## <a name="avfoundation-framework-additions"></a>AVFoundation フレームワークの追加

AVFoundation フレームワークには、次の機能強化が含まれています。

 - TvOS 10 で、アプリは実装されなく異なる[AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem)コンテンツの種類に基づいて動作します。 設定するだけ、`Rate`プロパティと AVFoundation が決まります十分なコンテンツの再生に使用できる失速なし。
 - 新しい`AVPlayerLooper`クラスでは、再生中に特定のメディアをループしやすくします。

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>AVKit フレームワークの機能強化

AVKit フレームワークには、次の機能強化が含まれています。

 - アプリがのスキップの動作を制御、 [AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller)ジェスチャをスキップしていますが、再生リストまたは現在の項目内で事前に次の項目に移動可能性があります。

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>コア データ拡張機能

tvOS 10 には、データのコア フレームワークに次の機能強化が含まれています。

 - ルート[NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext)オブジェクトには、同時実行エラーが発生とシリアル化せずフェッチがサポートしています。
 - [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator)クラス SQLite データ ストアのプールが保持されます。
 - [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) WAL ジャーナル モードのサポート、新しいクエリの生成に SQLite データ ストア オブジェクトの機能で管理されているオブジェクトのコンテキスト (MOC) ピン留めできます将来をフェッチするための特定のデータベース バージョンにし、。トランザクション エラーが発生します。
 - 使用して、高度な`NSPersistenceContainer`参照に、 `NSPersistentStoreCoordinator`、 [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel)およびその他のコア データの構成リソース。
 - いくつかの新しい便利なメソッドに追加された`NSManagedObject`フェッチを実行し、サブクラスを作成しやすきます。

詳細については、Apple を参照してください[コア データ フレームワーク参照](https://developer.apple.com/reference/coredata)です。

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>主要なグラフィックス機能強化

tvOS 10 には、コア グラフィックス フレームワークに次の機能強化が含まれています。

 - 新しい[CGColorConverterRef](https://developer.apple.com/reference/coregraphics/cgcolorconverterref)クラスは、一連の色の変換の実行に使用できます。

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>Core イメージの機能強化

tvOS 10 では、Core のイメージのフレームワークに次の機能強化を加えます。

 - `ImageWithExtent`のメソッド、 [CIFilter](https://developer.apple.com/reference/coreimage/cifilter)クラスは、フィルター操作にカスタム処理を挿入するために使用できます。 Core のイメージは出力用のイメージを処理するときに、フィルターの間で指定されたコールバックを呼び出すか、表示されます。
 - アプリは前に、と処理後の色領域との間に変換することで、Core のイメージのコンテキストの作業の色空間の外部の色空間内のイメージを今すぐ処理できます。
 - いくつかのレンダリング パフォーマンス強化が加えられた`UIImage`で (Core のイメージのイメージ ストアに支えられて) ときにレンダリング`UIImageView`オブジェクト。 
 - `UIImage` オブジェクト タグが付けられた wide 域で、全体にわたる域色をレンダリングする`UIImageView`広色をサポートする iOS デバイス上のオブジェクト。
 - コア イメージ カーネル コードでは、特定のピクセル出力形式を要求できますようになりました。

さらに、以下の新しい Core イメージ フィルターが追加されました。

 - `CINinePartTiled`
 - `CINinePartStretched`
 - `CIHueSaturationValueGradient`
 - `CIEdgePreserveUpsampleFilter`
 - `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Foundation の機能強化

Foundation framework tvOS 10 は、次の機能強化が施されました。

 - 使用して、新しい[NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval)の間隔を比較して、間隔の交差部分のテストの期間などの日付と時刻の間隔の計算を作成するクラス。
 - いくつかの新しいプロパティが追加されて、 [NSLocal](https://developer.apple.com/reference/foundation/nslocale)クラスをローカルの情報と使用可能な表示形式を取得します。
 - 使用して、新しい[NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement)間でさまざまなユニットのメジャー (出荷単位) を変換するか、異なる UOMs 内の値に対して計算を実行するクラス。
 - 使用して、新しい[NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter)クラスをエンドユーザーに表示するローカライズされた測定値の書式を設定します。
 - 使用して、新しい[NSUnit](https://developer.apple.com/reference/foundation/nsunit)と[NSDimension](https://developer.apple.com/reference/foundation/nsdimension)特定 UOMs を表すクラス。

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>GameKit の機能強化

TvOS 10 で GameKit フレームワークには、次の機能強化が施されました。

 - 新しい iCloud 専用のアカウントの種類が実装されて、 [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer)クラスです。
 - 新しい[GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession)クラスがゲーム センターで永続的なデータ ストレージを管理するために一般化されたソリューションを提供します。 `GKGameSession` プレーヤーの一覧を保持し、アプリが担当するフォームを取得またはプレーヤーの間で交換する方法とタイミングは、参加者の日付が格納されているの実装です。 さまざまな状況では、ゲーム セッションはターンに基づく既存の一致、リアルタイム一致または永続的なゲーム メソッド保存を置き換えることができます。

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>GameplayKit の機能強化

TvOS 10 で GameplayKit フレームワークには、次の機能強化が施されました。

 - 手続き型のノイズの生成が追加されましたし、自然なテクスチャのリアルさを向上させる、カメラの動きをよりリアルを追加、および豊富なゲームの世界を生成するために使用できます。
 - 効率的な検索のゲームの世界のデータをパーティション分割は、空間をパーティション分割を使用します。
 - 新しいモンテカルロ ストラテジスト ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) 可能な移動の排他的な計算が追加されました。
 - デシジョン ツリーの新しい API が追加されました ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree)と[GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) ゲーム構築 AI を強化するためにします。
 - 既存のエージェントと、new を使用してパス検索の動作に 3D のサポートが追加されました[GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d)と[GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d)クラスです。
 - 使用して、新しい[GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph)高パフォーマンスで自然なパスを提供するクラス。
 - 新しい[GKScene](https://developer.apple.com/reference/gameplaykit/gkscene)と[GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent)クラス make GameplayKit と SpriteKit より簡単に結合します。

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>金属製の拡張機能

TvOS 10 で金属製のフレームワークには、次の機能強化が施されました。

 - 3D アプリやゲームを使用できるよう_テセレーション_効率的に複雑なシーンと GPU を使用してジオメトリをレンダリングします。
 - 高度に最適化材料とシーンのライト組み合わせ関数のコレクションを作成するのにには、関数の特殊化を使用します。
 - 金属のパフォーマンスを最適化するためにリソース割り当ての粒度の細かい制御ベースのリソースをヒープを使用してアプリと Memoryless レンダー ターゲットを提供します。

詳細については、Apple を参照してください[メタル プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)です。

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>金属パフォーマンス シェーダーの機能強化

TvOS 10 のメタル パフォーマンス シェーダー フレームワークには、次の機能強化が施されました。

 - ニューラル ネットワークの操作のカラー スペース変換などの高最適化、データ並列計算を活用するためにアプリを許可するために、金属パフォーマンス シェーダー フレームワークには、多くの新しいカーネルが追加されました。

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>ModelIO の機能強化

TvOS 10 で ModelIO フレームワークには、次の機能強化が施されました。

 - USD ファイル形式はサポートされています。
 - 使用して、新しい`MDLMaterialPropertyGraph`を簡単にランタイムをサポートするクラスがモデルに変更します。
 - 署名にサポートが追加されました距離フィールド、 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray)クラスです。
 - 使用して、新しい`MDLLightProbeIrradianceDataSource`ライト プローブの配置を支援するクラス。

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>SceneKit の機能強化

TvOS 10 で SceneKit フレームワークには、次の機能強化が施されました。

 - SceneKit には、単純な資産の作成をより現実的な結果を新しい物理的に基づくレンダリング (PBR) システムが含まれています。
 - 使用して、新しい[SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased)網掛けをモデルの製品に現実的な網かけのさまざまな 3 つの基本的なプロパティを必要とするときに (`Diffuse`、`Metalness`と`Roughness`)。
 - 環境に基づく光源の動作を最適な網掛け PBR、以降を使用して、`LightingEnvironment`ベージュ シーン全体でイメージ ベースの照明を割り当てるプロパティをします。
 - 使用して、`IESProfileURL`現実世界の電灯ルーメン) の「強度など実際値光源基本を定義し、色温度 (ケルビン) をインポートするプロパティです。
 - [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera)クラスは HDR 機能と効果を使用して大きいよりリアルを提供することができます。 アダプティブ露出を使用すると、自動効果または使用周辺、カラーの縁取りおよびゲームに filmatic 効果を追加するのに成績評価の色を作成できます。
 - PBR と HDR の両方のカメラ機能は、従来のレンダリング テクニックよりも良い結果を提供するをその結果、SceneKit 今すぐ計算を実行すべて色 (wide カラー デバイス ディスプレイで P3 色域を使用して) 線形のカラー スペースです。
 - SceneKit 色カラー プロファイル情報を読み取ることですべての色を一致するようになりました。
 - SceneKit は、カラー シェーダーの種類のすべての線形 RGB 色空間に値を解釈します。
 - SceneKit を読み取り、テクスチャのイメージのカラー プロファイル情報を調整するため、すべてのイメージの資産カタログを使用して、この情報が提供されることを確認します。
 - 領域のレンダリングを色線形の両方を無効状態でのワイド色を指定して、`SCNDisableLinearSpaceRendering`と`SCNDisableWideGamut`アプリの内のキー`Info.plist`です。
 - 任意の多角形霊長 (またはのいずれかのファイルから読み込まれたをプログラムで生成) をビルドに新しい geometry を指定する[SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon)クラスです。

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>SpriteKit の機能強化

TvOS 10 で SpriteKit フレームワークには、次の機能強化が施されました。

 - Tilemaps は、2 D、2.5 D およびを使用する側のスクロールのゲームの正方形、六角および等角タイル図形をサポートするようになりました、 `SKTileMapMode`、 `SKTileGroup`、`SKTileGroupRule`と`SKTileSet`クラスです。
 - 使用して、新しい`SKWarpGeometry`を拡大または変形クラス[SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode)または[SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode)レンダリングします。 新しい[SKAction](https://developer.apple.com/reference/spritekit/skaction) warp エフェクト間の切り替え効果をアニメーション化するクラスを使用できます。
 - カスタムのシェーダーは、属性を指定できます (`SKAttribute`) 属性値を指定することによって、シェーダーを使用する各ノードで個別に構成できます (`SKAttributeValue`)。
 - [SKView](https://developer.apple.com/reference/spritekit/skview)クラス シーンが表示されるタイミングと方法を詳細に制御するためのいくつかの新しいメソッドを提供します。

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>UIKit の機能強化

TvOS 10 で UIKit フレームワークには、次の機能強化が施されました。

 - 非表示項目のフォーカスをさらにサポートするために、フォーカスの API が拡張され`UIViews`です。 フォーカスをサポートしている項目_必要があります_実装、`IUIFocusItem`インターフェイスです。
 - 新しい`UIGraphicsRender`クラスは、オブジェクト指向の作成方法のビットマップや Pdf UIKit レンダリングまたは Core グラフィックスを提供し、非推奨が置き換えられます`UIGraphicsBeginImageContext`メソッドです。
 - `UIUserInterfaceStyle` (暗色または明色) はどのユーザー インターフェイスのテーマが現在アクティブなを判断するクラスが追加されました。
 - 新しいアニメーションを完全に対話型、オブジェクトに基づく、割り込み可能なサポートが追加されました van ジェスチャにリンクします。 Pleas を参照してください Apple の[UIViewAnimating Protocol Reference](https://developer.apple.com/reference/uikit/uiviewanimating)、 [UIViewPropertyAnimator クラス参照](https://developer.apple.com/reference/uikit/uiviewpropertyanimator)、 [UITimingCurveProvider Protocol Reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider)、 [UICubicTimingParameters クラス参照](https://developer.apple.com/reference/uikit/uicubictimingparameters)と[UISpringTimingParameter クラス参照](https://developer.apple.com/reference/uikit/uispringtimingparameters)詳細についてはします。
 - 新しい`UIPreviewInteraction`と`UIPreviewInteractionDelegate`ピークおよびポップ操作にカスタムのインターフェイスを提供するアプリのアプリを許可します。
 - 新しい`UIAccessibilityCustomRotor`クラス経由で音声などの支援技術にカスタムのコンテキストに固有の機能を提供するアプリを使用できます。
 - 使用して、`UIAccessibilityIsAssistiveTouchRunning`と`UIAccessibilityAssistiveTouchStatusDidChangeNotification`AssistiveTouch が有効になっているかどうかを決定する記号。
 - 使用して、`UIAccessibilityHearingDevicePairedEar`と`UIAccessibilityHearingDevicePairedEarDidChangeNotification`いずれかの状態を取得するシンボル ペア MFi 聴覚を支援します。
 - 新しい[UIPasteboard](https://developer.apple.com/reference/uikit/uipasteboard) API (有効期間の制限事項) などの新しいオプションを提供し、一般的なクラス型に互換性のあるコンテンツの種類を自動的に宣言されます。
 - ラベルの動的な型をサポートするためにテキスト フィールドとテキスト ボックスを使用して、新しい`PreferredFontForTextStyle`のメソッド、`UIFont`クラスです。
 - かどうか要素更新フォントを決定するときに、デバイス`UIContentSizeCategory`、変更を使用して、`AdjustsFontForContentSizeCategory`のプロパティ、`UIContentSizeCategoryAdjusting`を委任します。
 - アプリでは、テキストと背景色などのタブ バー項目に対してバッジの外観を制御できますようになりました。
 - サブクラスのすべてのスクロール ビューとスクロールのビューでサポートされて現在のコントロールの更新 (など`UICollectionView`)。
 - `OpenURL`のメソッド、`UIApplication`クラスが非同期に呼び出されます open が完了した後に呼び出される完了ハンドラーをサポートするようになりました。
 - CloudKit の共有を開始し、new を使用してプロパティを変更`UICloudSharingController`と`UICloudSharingControllerDelegate`クラスです。
 - プリフェッチされたセルのスクロールのエクスペリエンスを向上させるの活用`UICollectionViews`に新しい`UICollectionViewDataSourcePrefetching`を委任します。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
