---
title: 追加の tvOS 10 フレームワークの変更
description: このドキュメントでは、iOS 10 の既存のフレームワークに加えられた軽微な変更点と機能強化について説明します。 AVFoundation、Avfoundation、コアデータ、コアグラフィックス、Foundation、アップデートキット、およびその他の更新について説明します。
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 99ee654b260a3a89b58578d352dd066a41753295
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769138"
---
# <a name="additional-tvos-10-frameworks-changes"></a>追加の tvOS 10 フレームワークの変更

Apple は、tvOS に対する大きな変更に加えて、tvOS 10 の既存の複数のフレームワークに対して変更や機能強化を加えました。

<a name="AV-Foundation-Framework" />

## <a name="avfoundation-framework-additions"></a>AVFoundation フレームワークの追加

AVFoundation framework には、次の機能強化が含まれています。

- TvOS 10 では、アプリはコンテンツの種類に基づいてさまざまな[AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem)の動作を実装しなくなりました。 プロパティを`Rate`設定するだけで、停止することなく再生できるコンテンツが十分にあるかどうかが avfoundation によって判断されます。
- 新しい`AVPlayerLooper`クラスを使用すると、再生中に特定のメディアを簡単にループできます。

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>AVKit フレームワークの機能強化

AVKit フレームワークには、次の機能強化が含まれています。

- これで、アプリは[AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller)のスキップ動作を制御できるようになりました。そのため、スキップするジェスチャは、再生リストの次の項目に移動するか、現在の項目内で進めることができます。

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>コアデータの機能強化

tvOS 10 には、コアデータフレームワークに対する次の機能強化が含まれています。

- ルート[NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext)オブジェクトは、シリアル化を使用しない同時エラーおよびフェッチをサポートしています。
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator)クラスは、SQLite データストアのプールを保持します。
- WAL ジャーナルモードで SQLite データストアを使用する[NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext)オブジェクトでは、新しいクエリ生成機能がサポートされています。この機能を使用すると、マネージオブジェクトコンテキスト (MOC) を特定のデータベースバージョンに固定して、将来のフェッチやエラー処理を行うことができます。
- 高レベル`NSPersistenceContainer`を使用して`NSPersistentStoreCoordinator`、、 [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) 、その他のコアデータ構成リソースを参照します。
- フェッチの実行とサブクラスの作成`NSManagedObject`を容易にするために、いくつかの新しい便利なメソッドが追加されました。

詳細については、Apple の[Core Data Framework リファレンス](https://developer.apple.com/reference/coredata)を参照してください。

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>コアグラフィックスの機能強化

tvOS 10 には、コアグラフィックスフレームワークに対する次の機能強化が含まれています。

- 新しい[Cgcolorterref](https://developer.apple.com/reference/coregraphics/cgcolorconverterref)クラスを使用して、一連のカラー変換を実行できます。

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>コアイメージの機能強化

tvOS 10 では、コアイメージフレームワークに対して次の機能強化が行われています。

- [Cifilter](https://developer.apple.com/reference/coreimage/cifilter)クラスのメソッドを使用して、フィルター操作にカスタム処理を挿入できます。`ImageWithExtent` コアイメージは、出力または表示のためにイメージを処理するときに、フィルター間で指定されたコールバックを呼び出します。
- これで、アプリは、処理の前後に色空間を変換することによって、コアイメージコンテキストの作業色空間の外にある色空間でイメージを処理できるようになりました。
- オブジェクトのレンダリング (コアイメージイメージストア`UIImage`によってサポートされている場合) に`UIImageView`対して、いくつかのレンダリングパフォーマンスが向上しています。 
- `UIImage`ワイド色のタグが付けられたオブジェクトは、ワイド`UIImageView`色をサポートする iOS デバイス上のオブジェクトで、幅域外色として表示されます。
- コアイメージカーネルコードは、特定のピクセル出力形式を要求できるようになりました。

さらに、次の新しいコアイメージフィルターが追加されました。

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Foundation の機能強化

TvOS 10 の Foundation framework には、次の機能強化が加えられています。

- 新しい[Nsdateinterval](https://developer.apple.com/reference/foundation/nsdateinterval)クラスを使用して、間隔の比較と間隔の交差部分のテストを行うために、期間などの日付と時間の間隔を計算します。
- ローカル情報と使用可能な表示形式を取得するために、いくつかの新しいプロパティが[nslocal](https://developer.apple.com/reference/foundation/nslocale)クラスに追加されました。
- 新しい [Nsmeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) クラスを使用して異なる測定単位 (UOM) 間で変換するか、異なる uoms の値に対して計算を実行します。
- 新しい[NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter)クラスを使用して、ローカライズされた測定形式をエンドユーザーに表示するように書式設定します。
- 特定の UOMs を表すために、新しい[Nsunit](https://developer.apple.com/reference/foundation/nsunit)クラスと[nsunit](https://developer.apple.com/reference/foundation/nsdimension)クラスを使用します。

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>お持ちのキットの機能強化

TvOS 10 の Kit フレームワークには、次の機能強化が行われています。

- 新しい iCloud 専用のアカウントの種類は、 [Gkcloudplayer](https://developer.apple.com/reference/gamekit/gkcloudplayer)クラスによって実装されています。
- 新しい[Gkq&a session](https://developer.apple.com/reference/gamekit/gkgamesession)クラスは、Game Center で永続的なデータストレージを管理するための一般化されたソリューションを提供します。 `GKGameSession`プレーヤーのリストを保持します。アプリは、参加者の日付の格納、取得、または交換の方法とタイミングを実装するフォームです。 多くの場合、ゲームセッションでは、既存のターンベースの一致、リアルタイムの一致、または永続的なゲーム保存方法を置き換えることができます。

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>お持ちのおプレイキットの機能強化

TvOS 10 の Playkit フレームワークには、次の機能強化が行われています。

- 手続き型のノイズ生成が追加され、自然なテクスチャのリアリティを向上させるために使用できます。また、カメラの動きにリアリティを加え、豊富なゲームワールドを生み出します。
- 空間パーティション分割を使用して、効率的な検索のためにゲームのワールドデータを分割します。
- すべての移動計算を可能にするために、新しいモンテカルロストラテジスト ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) が追加されました。
- ゲームを構築する AI を強化するために、新しいデシジョンツリー API が追加されました ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree)と[GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode))。
- 新しい[GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d)クラスと[GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d)クラスを使用して、既存のエージェントおよびパス検索動作に3d サポートが追加されました。
- 新しい[GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph)クラスを使用して、高パフォーマンスの自然なパスを提供します。
- 新しい[Gkscene](https://developer.apple.com/reference/gameplaykit/gkscene)クラスと[GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent)クラスを使用すると、これまでにないようにすることができます。

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>金属の機能強化

TvOS 10 の金属フレームワークには、次の機能強化が行われています。

- 3D アプリやゲームでは、_テセレーション_を使用して、複雑なシーンとジオメトリを GPU 経由で効率的にレンダリングできるようになりました。
- 関数の特殊化を使用して、シーンに対して高度に最適化されたマテリアルおよびライトの組み合わせ関数のコレクションを作成します。
- リソースの割り当てをきめ細かく制御することにより、リソースヒープと Memoryless レンダーターゲットを使用して金属ベースのアプリのパフォーマンスを最適化できます。

詳細については、Apple の[金属のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)を参照してください。

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>金属製のパフォーマンスシェーダーの機能強化

TvOS 10 の金属パフォーマンスシェーダーフレームワークには、次の機能強化が行われています。

- 金属のパフォーマンスシェーダーフレームワークには、多くの新しいカーネルが追加されました。これにより、アプリは、色空間の変換やニューラルネットワーク操作など、高度に最適化されたデータ並列計算を利用できます。

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>ModelIO の機能強化

TvOS 10 の ModelIO フレームワークには、次の機能強化が行われています。

- 米国ドルのファイル形式がサポートされるようになりました。
- 新しい`MDLMaterialPropertyGraph`クラスを使用すると、モデルに対する実行時の変更を簡単にサポートできます。
- 符号付き距離フィールドのサポートが[Mdlvoxelarray](https://developer.apple.com/reference/modelio/mdlvoxelarray)クラスに追加されました。
- 新しい`MDLLightProbeIrradianceDataSource`クラスを使用すると、プローブの配置を簡単に行うことができます。

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>SceneKit の機能強化

TvOS 10 の SceneKit フレームワークには、次の機能強化が行われています。

- SceneKit には、より簡単なアセット作成により、より現実的な結果を得るために、新しい物理的に基づく表示 (.PBR) システムが追加されました。
- 新しい[SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased)シェーディングモデルを使用して、さまざまな現実的な網掛け効果を製品に追加し、`Diffuse`3 `Metalness`つの基本プロパティ (、および) のみを必要と`Roughness`します。
- .Pbr の網掛けは環境ベースの光源で最適に機能`LightingEnvironment`するため、プロパティを使用してイメージベースの照明をシーン全体に適用します。
- プロパティを`IESProfileURL`使用して、光源 (ルーメン) や色温度 (ケルビン単位) などの実際の値に光源を定義する実際の照明器具をインポートします。
- [Scncamera](https://developer.apple.com/reference/scenekit/scncamera)クラスは、HDR の特徴と効果を使用して、よりリアリティを高めることができます。 アダプティブ露出を使用して自動効果を作成するか、vignetting、color fringing、色の調整を使用して、ゲームに filmatic 効果を追加します。
- SceneKit と HDR の両方のカメラ機能は、従来の表示手法よりも優れた結果を提供します。結果として、では、すべての色の計算を線形の色空間で実行するようになりました (ワイド色のデバイスディスプレイで P3 の色域外を使用)。
- SceneKit color プロファイル情報を読み取って、すべての色に一致するようになりました。
- SceneKit は、すべてのシェーダーの種類の線形 RGB カラー空間で色コンポーネントの値を解釈します。
- テクスチャイメージでカラープロファイル情報の読み取りと調整を SceneKit するため、すべてのイメージに対してアセットカタログを使用して、この情報が確実に提供されるようにします。
- アプリのでキー `SCNDisableLinearSpaceRendering`と`SCNDisableWideGamut`キーを指定することにより、線形色空間の`Info.plist`レンダリングとワイド色の両方を無効にできます。
- 新しい[SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon)クラスで geometry を指定するために、任意の多角形の霊長類 (ファイルから読み込まれるか、プログラムによって生成される) を構築します。

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>SpriteKit の機能強化

TvOS 10 の SpriteKit フレームワークには、次の機能強化が行われています。

- Tilemaps は`SKTileMapMode`、 `SKTileGroup`、、 `SKTileGroupRule`および`SKTileSet`の各クラスを使用して、2d、2.5 d、およびサイドスクロールゲーム用の正方形、六角、および等角投影のタイル図形をサポートするようになりました。
- 新しい`SKWarpGeometry`クラスを使用して、 [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode)または[SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode)のレンダリングを伸縮またはデフォルメします。 新しい[Skaction](https://developer.apple.com/reference/spritekit/skaction)クラスは、ワープ効果間の遷移をアニメーション化するために使用できます。
- カスタムシェーダーは、属性値`SKAttribute`(`SKAttributeValue`) を指定することによって、シェーダーを使用する各ノードで個別に構成できる属性 () を提供できます。
- [Skview](https://developer.apple.com/reference/spritekit/skview)クラスには、シーンをレンダリングするタイミングと方法をきめ細かく制御できる新しいメソッドがいくつか用意されています。

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>UIKit の機能強化

TvOS 10 の UIKit フレームワークには、次の機能強化が加えられています。

- に`UIViews`加えて非ビューアイテムのフォーカスをサポートするために、フォーカス API が強化されました。 フォーカスをサポートする項目は、 `IUIFocusItem`インターフェイスを実装_する必要があり_ます。
- 新しい`UIGraphicsRender`クラスは、uikit レンダリングまたはコアグラフィックスからビットマップや pdf を作成するオブジェクト指向のメソッドを提供し`UIGraphicsBeginImageContext` 、非推奨のメソッドを置き換えます。
- クラス`UIUserInterfaceStyle`が追加され、現在アクティブなユーザーインターフェイスのテーマ (ダークまたはライト) が特定されました。
- 新しい完全対話型のオブジェクトベースの中断可能なアニメーションのサポートが追加され、van がジェスチャにリンクされました。 Apple の[Uiviewanimating プロトコルのリファレンス](https://developer.apple.com/reference/uikit/uiviewanimating)、 [uiviewpropertyアニメータークラスリファレンス](https://developer.apple.com/reference/uikit/uiviewpropertyanimator)、 [UITimingCurveProvider Protocol reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider)、 [UICubicTimingParameters クラス参照](https://developer.apple.com/reference/uikit/uicubictimingparameters) [、](https://developer.apple.com/reference/uikit/uispringtimingparameters)詳細については、UISpringTimingParameter クラス参照を参照してください。
- 新しい`UIPreviewInteraction` と`UIPreviewInteractionDelegate`では、アプリがピーク操作とポップ操作のためのカスタムインターフェイスを提供できるようになります。
- 新しい`UIAccessibilityCustomRotor`クラスを使用すると、アプリは、ボイスオーバーなどの補助的なテクノロジに対して、コンテキスト固有のカスタム機能を提供できます。
- AssistiveTouch が`UIAccessibilityIsAssistiveTouchRunning`有効`UIAccessibilityAssistiveTouchStatusDidChangeNotification`になっているかどうかを判断するには、記号と記号を使用します。
- `UIAccessibilityHearingDevicePairedEar` と`UIAccessibilityHearingDevicePairedEarDidChangeNotification`記号を使用して、ペアになっている mfi 聴覚援助の状態を取得します。
- 新しい[Uipasteboard](https://developer.apple.com/reference/uikit/uipasteboard) API は、新しいオプション (有効期間の制限など) を提供し、共通クラス型に対して互換性のあるコンテンツの種類を自動的に宣言します。
- ラベルの動的な型をサポートするために、テキストフィールドとテキスト`PreferredFontForTextStyle`ボックスは`UIFont`クラスの新しいメソッドを使用します。
- デバイス`UIContentSizeCategory`が変更されたときに、要素がそのフォントを更新する必要がある`UIContentSizeCategoryAdjusting`かどうかを判断するには、デリゲートの`AdjustsFontForContentSizeCategory`プロパティを使用します。
- これで、アプリケーションは、テキストや背景色などのタブバー項目のバッジの外観を制御できるようになりました。
- の更新コントロールが、すべてのスクロールビューおよびスクロールビューサブクラス ( `UICollectionView`など) でサポートされるようになりました。
- クラスのメソッドが非同期に呼び出されるようになったため、open の完了後に呼び出される完了ハンドラーがサポートされるようになりました。 `OpenURL` `UIApplication`
- Cloudkit の共有を開始し、新しい`UICloudSharingController`クラスと`UICloudSharingControllerDelegate`クラスを使用してそのプロパティを変更します。
- `UICollectionViews` 新しい`UICollectionViewDataSourcePrefetching`デリゲートを使用したのスクロールエクスペリエンスを向上させるために、プリフェッチされたセルを活用します。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [TvOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
