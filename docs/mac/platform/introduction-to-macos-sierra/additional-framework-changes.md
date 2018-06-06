---
title: 追加 macOS Sierra フレームワークの変更
description: このドキュメントでは、軽微な変更と macOS Sierra で導入された既存のフレームワークの機能強化について説明します。 加速 framework、AppKit、AVFoundation、基本データ、Core のイメージ、Foundation を変更を確認します。
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3cfa2e9bcb0be4d65462914215045c9c7f01da5b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792594"
---
# <a name="additional-macos-sierra-framework-changes"></a>追加 macOS Sierra フレームワークの変更

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>フレームワークの機能強化を高速化します。

MacOS Sierra の高速化フレームワークには、次の機能強化が加えられました。

- 信号 (整数微積分) を追加します。
- ニューラル ネットワークを構築するための基本的な機能を追加します。
- 追加した幾何学的述語関数を 2 つの幾何オブジェクトの交差部分のようなものをテストします。

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>AppKit フレームワークの機能強化

AppKit framework macOS Sierra は、次の機能強化が施されました。

- いくつかの機能強化`NSCollectionView`など。
    - **折りたたみ可能なセクション**-を水平方向の単一行に、コレクション ビューのセクションを折りたたむことができます。
    - **ヘッダーを浮動**-ヘッダーとフッターできるようになりましたフロートするか (、フロー レイアウト内で) と同じ API を使用して[UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) iOS にします。
    - **スクロール可能なバック グラウンド ビュー** -コレクション ビューの背景、コンテンツと共にスクロールを設定できます。
- 遅延ビュー レイアウト パスが最適化され、拡張されています。
- ドラッグ アンド ドロップ API が含まれています、新しい`NSFilePromiseProvider`と`NSFilePromiseReceiver`群雄をサポートするクラスにドラッグします。
- いくつかの便利なコンス トラクターは、既存のコントロールに追加されました。
    -  `NSButton` プッシュ ボタンを作成するための新しいコンス トラクターが含まれていますチェック ボックスやラジオ ボタンです。
    -  `NSTextField` ラッピングと非の折り返しのラベル、属性付きラベルの編集可能なテキスト フィールドを作成するための新しいコンス トラクターが含まれます。
    -  `NSSegmentedControl` グループのラベルまたはイメージからセグメント化されたコントロールを作成するための新しいコンス トラクターが含まれます。
    -  `NSSlider` 水平方向の線形スライダーを作成するための新しいコンス トラクターが含まれます。
    -  `NSImageView` 編集不可能なイメージのビューを作成するための新しいコンス トラクターを含む、指定された`NSImage`です。
-  新しい`NSGridView`sub ビューのコレクション変数を持つグリッドにサイズが行と列を動的に非表示または表示できる自動レイアウトに追加されました。

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>AVFoundation フレームワークの機能強化

AVFoundation framework macOS Sierra は、次の機能強化が施されました。

- MacOS などで、アプリがなくなった異なるを実装する[AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem)コンテンツの種類に基づいて動作します。 設定するだけ、`Rate`プロパティと AVFoundation が決まります十分なコンテンツの再生に使用できる失速なし。
- 新しい`AVPlayerLooper`クラスでは、再生中に特定のメディアをループしやすくします。
- `AVAssetDownloadURLSession`クラスでは、ダウンロード、および暗号化された HLS ストリームを FairPlay の後で再生します。

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>コア データ フレームワークの機能強化

MacOS Sierra のコア データ フレームワークには、次の機能強化が加えられました。

- ルート[NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext)オブジェクトには、同時実行エラーが発生とシリアル化せずフェッチがサポートしています。
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator)クラス SQLite データ ストアのプールが保持されます。
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) WAL ジャーナル モードのサポート、新しいクエリの生成に SQLite データ ストア オブジェクトの機能で管理されているオブジェクトのコンテキスト (MOC) ピン留めできます将来をフェッチするための特定のデータベース バージョンにし、。トランザクション エラーが発生します。
- 使用して、高度な`NSPersistenceContainer`参照に、 `NSPersistentStoreCoordinator`、 [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel)およびその他のコア データの構成リソース。
- いくつかの新しい便利なメソッドに追加された`NSManagedObject`フェッチを実行し、サブクラスを作成しやすきます。

詳細については、Apple を参照してください[コア データ フレームワーク参照](https://developer.apple.com/reference/coredata)です。

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>Core イメージ フレームワークの機能強化

Core イメージ framework macOS Sierra は、次の機能強化が施されました。

- `ImageWithExtent`のメソッド、 [CIFilter](https://developer.apple.com/reference/coreimage/cifilter)クラスは、フィルター操作にカスタム処理を挿入するために使用できます。 Core のイメージは出力用のイメージを処理するときに、フィルターの間で指定されたコールバックを呼び出すか、表示されます。
- アプリは前に、と処理後の色領域との間に変換することで、Core のイメージのコンテキストの作業の色空間の外部の色空間内のイメージを今すぐ処理できます。
- Core イメージ カーネルは、特定のピクセル出力形式を要求できますようになりました。
- 次の新しいイメージ フィルターが追加されました: `CINinePartTitled`、 `CINinePartStretched`、 `CIHueSaturationValueGradient`、`CIEdgePreserveUpsampleFilter`と`CIClamp`です。

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Foundation フレームワークの機能強化

Foundation framework macOS Sierra は、次の機能強化が施されました。

- 使用して、 [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension)質量、長さ、速度、期間、および温度を表す、変換およびなど最も一般的な物理的な単位の多くを表示するための API です。
- 使用して、 [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter)書式設定された日付を解析し、ISO 8601 形式を生成するためのクラスです。
- 使用して、新しい[NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval)の間隔を比較して、間隔の交差部分のテストの期間などの日付と時刻の間隔の計算を作成するクラス。
- 使用して、 [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter)文字列からユーザーの名前の要素を解析するクラス。
- 使用して、新しい[NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics)セッションのネットワークは URL のメトリックを取得するクラス。

詳細については、Apple を参照してください[OS X v10.12 および iOS 10 Foundation のリリース ノート](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html)です。

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>GameKit フレームワークの機能強化

GameKit framework macOS Sierra は、次の機能強化が施されました。

- **Game Center アプリ**は廃止され macOS から削除します。 アプリが GameKit を使用している場合、_必要があります_スコアボードなど GameKit 機能の表示に独自のインターフェイスを提供します。 
- 新しい iCloud 専用のアカウントの種類が実装されて、 [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer)クラスです。
- 新しい[GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession)クラスがゲーム センターで永続的なデータ ストレージを管理するために一般化されたソリューションを提供します。 `GKGameSession` プレーヤーの一覧を保持し、アプリが担当するフォームを取得またはプレーヤーの間で交換する方法とタイミングは、参加者の日付が格納されているの実装です。 さまざまな状況では、ゲーム セッションはターンに基づく既存の一致、リアルタイム一致または永続的なゲーム メソッド保存を置き換えることができます。

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>GamePlayKit フレームワークの機能強化

GamePlayKit framework macOS Sierra は、次の機能強化が施されました。

- 手続き型のノイズの生成が追加されましたし、自然なテクスチャのリアルさを向上させる、カメラの動きをよりリアルを追加、および豊富なゲームの世界を生成するために使用できます。
- 効率的な検索のゲームの世界のデータをパーティション分割は、空間をパーティション分割を使用します。
- 新しいモンテカルロ ストラテジスト ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) 可能な移動の排他的な計算が追加されました。
- デシジョン ツリーの新しい API が追加されました ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree)と[GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) ゲーム構築 AI を強化するためにします。
- 既存のエージェントと、new を使用してパス検索の動作に 3D のサポートが追加されました[GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d)と[GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d)クラスです。
- 使用して、新しい[GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph)高パフォーマンスで自然なパスを提供するクラス。
- 新しい[GKScene](https://developer.apple.com/reference/gameplaykit/gkscene)と[GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent)クラス make GameplayKit と SpriteKit より簡単に結合します。

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>金属製のフレームワークの機能強化

MacOS Sierra のメタル フレームワークには、次の機能強化が加えられました。

- 3D アプリやゲームを使用できるよう_テセレーション_効率的に複雑なシーンと GPU を使用してジオメトリをレンダリングします。
- 高度に最適化材料とシーンのライト組み合わせ関数のコレクションを作成するのにには、関数の特殊化を使用します。
- 金属のパフォーマンスを最適化するためにリソース割り当ての粒度の細かい制御ベースのリソースをヒープを使用してアプリと Memoryless レンダー ターゲットを提供します。

詳細については、Apple を参照してください[メタル プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)です。

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>モデル I/O フレームワークの機能強化

モデル I/O framework macOS Sierra は、次の機能強化が施されました。

- USD ファイル形式はサポートされています。
- 使用して、新しい`MDLMaterialPropertyGraph`を簡単にランタイムをサポートするクラスがモデルに変更します。
- 署名にサポートが追加されました距離フィールド、 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray)クラスです。
- 使用して、新しい`MDLLightProbeIrradianceDataSource`ライト プローブの配置を支援するクラス。

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>写真のフレームワークの機能強化

MacOS Sierra の写真フレームワークには、次の機能強化が加えられました。

- ライブの写真の編集が、写真のフレームワークをサポートするアプリ、および写真編集の拡張機能を利用できます (写真やカメラの内部で使用するためのアプリ)。
- 使用して、新しい[PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext)ビデオとの両方に引き続き Live 写真のコンテンツの編集を適用するクラス。
- 使用して、 [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput)と[CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput)編集を実行して新しい Core のイメージのプロセッサ機能を利用するクラス。
- ライブの写真をサポートするために、 [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto)と[PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) iOS から macOS に移植されたクラスです。

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>SceneKit フレームワークの機能強化

SceneKit framework macOS Sierra は、次の機能強化が施されました。

- 単純な資産の作成をより現実的な結果を新しい物理的に基づくレンダリング (PBR) システムに含まれています。
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

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>セキュリティ フレームワークの機能強化

MacOS Sierra のセキュリティ フレームワークには、次の機能強化が施されました。

- `SecKey`インターフェイスを最新化され、すべてのプラットフォーム (iOS、tvOS、watchOS および macOS) を統合します。

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>SpriteKit フレームワークの機能強化

SpriteKit framework macOS Sierra は、次の機能強化が施されました。

- Tilemaps は、2 D、2.5 D およびを使用する側のスクロールのゲームの正方形、六角および等角タイル図形をサポートするようになりました、 `SKTileMapMode`、 `SKTileGroup`、`SKTileGroupRule`と`SKTileSet`クラスです。
- 使用して、新しい`SKWarpGeometry`を拡大または変形クラス[SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode)または[SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode)レンダリングします。 新しい[SKAction](https://developer.apple.com/reference/spritekit/skaction) warp エフェクト間の切り替え効果をアニメーション化するクラスを使用できます。
- カスタムのシェーダーは、属性を指定できます (`SKAttribute`) 属性値を指定することによって、シェーダーを使用する各ノードで個別に構成できます (`SKAttributeValue`)。
- [SKView](https://developer.apple.com/reference/spritekit/skview)クラス シーンが表示されるタイミングと方法を詳細に制御するためのいくつかの新しいメソッドを提供します。

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>新しいフレームワーク

次のフレームワークが macOS Sierra に追加されました。

- **目的の Framework** -このフレームワーク (場所やユーザーの操作など)、相互作用を確認し、その情報に基づいてアクションを実行するアプリを許可します。
- **SafariServices Framework** -このフレームワークは、(コンテンツ ブロッカー) などの Safari 向け macOS と iOS の両方のアプリの拡張機能を開発するアプリを許可します。

## <a name="related-links"></a>関連リンク

- [Mac サンプル](https://developer.xamarin.com/samples/mac/)
- [OS X 10.12 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
