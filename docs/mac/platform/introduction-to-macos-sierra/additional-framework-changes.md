---
title: 追加の macOS Sierra フレームワークの変更
description: このドキュメントでは、軽微な変更と macOS Sierra で導入された既存のフレームワークの機能強化について説明します。 加速フレームワーク、AppKit、AVFoundation、Core Data、Core のイメージ、Foundation を変更を確認します。
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 6ed827c018931e5b79887dc355f136e2a84623d6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61031628"
---
# <a name="additional-macos-sierra-framework-changes"></a>追加の macOS Sierra フレームワークの変更

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>フレームワークの機能強化を高速化します。

MacOS Sierra の高速化フレームワークには、次の機能強化が施されました。

- 信号 (整数微積分) を追加します。
- ニューラル ネットワークを構築するための基本的な機能を追加します。
- 追加した幾何学的述語関数を 2 つの幾何オブジェクトの交差部分のようなものをテストします。

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>AppKit フレームワークの機能強化

MacOS Sierra の AppKit フレームワークに、次の機能強化が加えられました。

- いくつかの機能強化`NSCollectionView`など。
    - **折りたたみ可能なセクション**-を水平方向の単一行にコレクション ビューのセクションを折りたたんだりできます。
    - **ヘッダーに floating** -ヘッダーとフッターこれで、フロー レイアウト) の「フロートすると同じ API を使用して[UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) iOS でします。
    - **スクロール可能なバック グラウンド ビュー** -コンテンツと共にスクロールするコレクション ビューの背景を設定することができますようになりました。
- 遅延のビューのレイアウト パスが最適化されており、拡張されています。
- ドラッグ アンド ドロップ API が含まれています、新しい`NSFilePromiseProvider`と`NSFilePromiseReceiver`群雄をサポートするクラスがドラッグします。
- 既存のコントロールには、いくつかの便利なコンス トラクターが追加されました。
    -  `NSButton` プッシュ ボタンを作成するための新しいコンス トラクターを含むチェック ボックスやラジオ ボタン。
    -  `NSTextField` 折り返しと折り返しのないラベル、属性付きのラベルおよびテキストの編集可能なフィールドを作成するための新しいコンス トラクターが含まれています。
    -  `NSSegmentedControl` グループのラベルまたはイメージからセグメント付きコントロールを作成するための新しいコンス トラクターが含まれています。
    -  `NSSlider` 水平方向の線形のスライダーを作成するための新しいコンス トラクターが含まれています。
    -  `NSImageView` イメージが編集可能なビューを作成するための新しいコンス トラクターが含まれています、指定された`NSImage`します。
-  新しい`NSGridView`変数を持つグリッドにサブ ビューのコレクション サイズの行と列を動的に非表示または表示できる自動レイアウトに追加されました。

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>AVFoundation フレームワークの機能強化

MacOS Sierra の AVFoundation フレームワークに、次の機能強化が加えられました。

- 、MacOS で、アプリがなくなった別実装するために[AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem)コンテンツの種類に基づいて動作します。 設定するだけです、`Rate`プロパティと AVFoundation の十分なコンテンツはなく失速再生に使用できる、ときに決まります。
- 新しい`AVPlayerLooper`クラスでは、再生中に、特定のメディアをループ処理しやすくします。
- `AVAssetDownloadURLSession`クラスでは、ダウンロードし、暗号化された HLS ストリームの FairPlay の後で再生します。

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>Core Data Framework の拡張

MacOS Sierra のコア データ フレームワークには、次の機能強化が施されました。

- ルート[NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext)オブジェクトは、同時実行のエラーとシリアル化せずフェッチをサポートしています。
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator)クラスには、SQLite のデータ ストアのプールが管理されます。
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) WAL ジャーナルのモードのサポート、新しいクエリの生成にデータ ストアを SQLite オブジェクト機能の管理オブジェクトのコンテキスト (MOC) を将来をフェッチするための特定のデータベース バージョンに固定でき、トランザクションのエラーが発生します。
- 高レベルを使用して`NSPersistenceContainer`参照に、 `NSPersistentStoreCoordinator`、 [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel)およびその他のコア データの構成リソース。
- いくつかの新しい便利なメソッドに追加された`NSManagedObject`フェッチを実行し、サブクラスを作成しやすきます。

詳細については、Apple を参照してください[コア データ フレームワーク参照](https://developer.apple.com/reference/coredata)します。

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>コア イメージ フレームワークの機能強化

MacOS Sierra のコア イメージ フレームワークに、次の機能強化が加えられました。

- `ImageWithExtent`のメソッド、 [CIFilter](https://developer.apple.com/reference/coreimage/cifilter)クラスは、フィルター操作にカスタム処理を挿入するために使用できます。 Core のイメージは出力用のイメージを処理するときに、フィルターの間で指定されたコールバックを呼び出すか、表示します。
- アプリでは、処理の前後に色空間との間に変換することで Core イメージ コンテキストの作業の色空間の外部の色空間内のイメージを処理できるようになりました。
- Core のイメージのカーネルでは、特定のピクセルの出力形式を要求できますようになりました。
- 次の新しいイメージ フィルターが追加されました: `CINinePartTitled`、 `CINinePartStretched`、 `CIHueSaturationValueGradient`、`CIEdgePreserveUpsampleFilter`と`CIClamp`します。

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Foundation フレームワークの機能強化

MacOS Sierra の Foundation フレームワークに、次の機能強化が加えられました。

- 使用して、 [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension)質量、長さ、速度、期間、および温度を表す、変換、およびなどの最も一般的な物理単位の多くを表示するための API。
- 使用して、 [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter)書式設定された日付を解析および ISO 8601 形式を生成するクラス。
- 使用して、新しい[NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval)間隔を比較して、間隔の交差部分のテストのための期間などの日付と時刻の間隔の計算を行うクラス。
- 使用して、 [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter)文字列からユーザーの名前の要素を解析するクラス。
- 使用して、新しい[NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics)セッションのネットワークは URL のメトリックを取得するクラス。

詳細については、Apple を参照してください[v10.12 の OS X、iOS 10 のリリース ノートを Foundation](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html)します。

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>GameKit フレームワークの機能強化

MacOS Sierra の GameKit フレームワークに、次の機能強化が加えられました。

- **Game Center アプリ**非推奨し、macOS から削除されています。 アプリが GameKit を使用する場合、_する必要があります_GameKit 機能 (ランキングなど) の表示に独自のインターフェイスを提供します。 
- 新しい iCloud 専用のアカウントの種類によって実装されて、 [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer)クラス。
- 新しい[GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession)クラス Game Center への永続的なデータ ストレージを管理するための汎用化されたソリューションを提供します。 `GKGameSession` プレーヤーの一覧を保持し、アプリが担当するフォームを取得するか、プレイヤーの間で交換される方法とタイミングは、参加者の日付が格納されている、実装します。 多くの場合は、ゲーム セッションはターンに基づく既存の一致、リアルタイムの一致または永続的なゲーム保存メソッドを置き換えることができます。

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>GamePlayKit フレームワークの機能強化

MacOS Sierra の GamePlayKit フレームワークに、次の機能強化が加えられました。

- 手続き型のノイズの生成が追加され、自然に見えるテクスチャのリアリティを強化し、カメラの動きをリアリティを追加し、豊富なゲームの世界を生成するために使用できます。
- 空間パーティション分割を使用して、効率的な検索のゲームの世界のデータをパーティション分割します。
- 新しいモンテカルロ ストラテジスト ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) 可能な移動の徹底的な計算が追加されました。
- デシジョン ツリーの新しい API が追加されました ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree)と[GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) ゲーム構築 AI を強化するためにします。
- 既存のエージェントと、new を使用してパス検索動作を 3D のサポートが追加されました[GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d)と[GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d)クラス。
- 使用して、新しい[GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph)高パフォーマンスで自然なパスを提供するクラス。
- 新しい[GKScene](https://developer.apple.com/reference/gameplaykit/gkscene)と[GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) GameplayKit と SpriteKit をより簡単に組み合わせるクラスの作成。

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>金属製のフレームワークの機能強化

MacOS Sierra のメタル フレームワークに、次の機能強化が加えられました。

- 3D アプリやゲームが使用できるようになりました_テセレーション_複雑なシーンと GPU を使用してジオメトリを効率的に表示するためにします。
- 関数の特殊化を使用すると、シーンにマテリアルとライトの組み合わせの高度に最適化されたコレクションを作成します。
- 金属のパフォーマンスを最適化するためにリソース割り当ての詳細な管理ベースのリソースをヒープを使用してアプリと Memoryless レンダー ターゲットを提供します。

詳細については、Apple を参照してください[メタル プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)します。

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>モデルの I/O のフレームワークの機能強化

MacOS Sierra のモデルの I/O のフレームワークには、次の機能強化が施されました。

- 米国ドルのファイル形式はサポートされています。
- 使用して、新しい`MDLMaterialPropertyGraph`を簡単にランタイムをサポートするクラスがモデルに変更します。
- サポートが追加された距離のフィールドの署名、 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray)クラス。
- 使用して、新しい`MDLLightProbeIrradianceDataSource`光のプローブの配置を支援するクラス。

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>写真のフレームワークの機能強化

MacOS Sierra の写真のフレームワークに、次の機能強化が加えられました。

- ライブの写真の編集が写真のフレームワークをサポートするアプリ、および写真編集拡張機能を利用できます (写真やカメラの内部で使用するためのアプリ)。
- 使用して、新しい[PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext)ビデオとの両方にまだコンテンツをライブの写真の編集を適用するクラス。
- 使用して、 [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput)と[CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput)クラスの編集を実行する新しいコア イメージ プロセッサ機能を活用するためにします。
- ライブの写真をサポートするために、 [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto)と[PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview)クラスが macOS に iOS から移植されています。

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>SceneKit フレームワークの機能強化

MacOS Sierra 用の SceneKit フレームワークに、次の機能強化が加えられました。

- 単純な資産の作成をより現実的な結果を新しい物理的にベースのレンダリング (PBR) システムに含まれています。
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

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>セキュリティ フレームワークの機能強化

MacOS Sierra のセキュリティ フレームワークには、次の機能強化が施されました。

- `SecKey`インターフェイスを最新化し、すべてのプラットフォーム (iOS、tvOS、watchOS、および macOS) で統合します。

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>SpriteKit フレームワークの機能強化

MacOS Sierra 用の SpriteKit フレームワークに、次の機能強化が加えられました。

- Tilemaps は、2 D、2.5 D、および使用する側のスクロール ゲームの正方形と六角等角投影のタイルの図形をサポートするようになりました、 `SKTileMapMode`、 `SKTileGroup`、`SKTileGroupRule`と`SKTileSet`クラス。
- 使用して、新しい`SKWarpGeometry`を拡大または事実を歪曲クラス[SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode)または[SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode)レンダリングします。 新しい[SKAction](https://developer.apple.com/reference/spritekit/skaction) warp エフェクト間の遷移をアニメーション化するクラスを使用できます。
- カスタムのシェーダーは、属性を指定できます (`SKAttribute`) 属性値を指定することで、シェーダーを使用する各ノードで個別に構成できます (`SKAttributeValue`)。
- [SKView](https://developer.apple.com/reference/spritekit/skview)クラスは、シーンをレンダリングするタイミングと方法をきめ細かく制御できるようにいくつかの新しいメソッドを提供します。

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>新しいフレームワーク

MacOS Sierra には、次のフレームワークが追加されました。

- **インテント Framework** -このフレームワーク (場所やユーザーの操作など)、相互作用を調べ、その情報に基づいてアクションを実行するアプリを許可します。
- **SafariServices Framework** -このフレームワークには、アプリが iOS と macOS の両方の (コンテンツ ブロッカー) などの Safari のアプリの拡張機能の開発ができるようにします。

## <a name="related-links"></a>関連リンク

- [Mac サンプル](https://developer.xamarin.com/samples/mac/)
- [新機能については OS X 10.12 です。](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
