---
title: 追加の macOS Sierra フレームワークの変更
description: このドキュメントでは、macOS Sierra で導入された既存のフレームワークの軽微な変更点と機能強化について説明します。 加速フレームワーク、AppKit、AVFoundation、Core Data、Core Image、Foundation などの変更について考察しています。
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 9bee1cb5108053dac74fcde4f4f5171c24c7ce1a
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91437218"
---
# <a name="additional-macos-sierra-framework-changes"></a>追加の macOS Sierra フレームワークの変更

<a name="Accelerate-Framework-Enhancements"></a>

## <a name="accelerate-framework-enhancements"></a>フレームワークの機能強化の高速化

MacOS Sierra の高速化フレームワークには、次の機能強化が行われています。

- Quadrature (整数微積分) が追加されました。
- ニューラルネットワークを構築するための基本的な機能が追加されました。
- 2つのジオメトリックオブジェクトの交差部分などをテストするジオメトリック述語関数を追加しました。

<a name="AppKit-Framework-Enhancements"></a>

## <a name="appkit-framework-enhancements"></a>AppKit フレームワークの機能強化

MacOS Sierra の AppKit フレームワークには、次の機能強化が行われています。

- 次のようないくつかの機能強化が行われ `NSCollectionView` ています。
  - [折りたたみ可能な**セクション**]: ユーザーがコレクションビューセクションを1つの水平行に折りたたむことを許可します。
  - **フローティングヘッダー** -ヘッダーとフッターは、IOS の [UICOLLECTIONVIEW](https://developer.apple.com/reference/uikit/uicollectionview) と同じ API を使用して (フローレイアウトで) フローティングできるようになりました。
  - **スクロール可能な背景ビュー** -コレクションビューの背景を、コンテンツと共にスクロールするように設定できるようになりました。
- 遅延ビューレイアウトパスは、最適化および拡張されています。
- ドラッグアンドドロップ API `NSFilePromiseProvider` `NSFilePromiseReceiver` に、ドラッグ flocking をサポートする新しいクラスとクラスが追加されました。
- いくつかの便利なコンストラクターが既存のコントロールに追加されました。
  - `NSButton` プッシュボタン、チェックボックス、およびオプションボタンを作成するための新しいコンストラクターが含まれています。
  - `NSTextField` 折り返しと折り返しのないラベル、属性付きラベル、および編集可能なテキストフィールドを作成するための新しいコンストラクターが含まれています。
  - `NSSegmentedControl` には、ラベルまたは画像のグループからセグメント化されたコントロールを作成するための新しいコンストラクターが含まれています。
  - `NSSlider` 水平方向の線形スライダーを作成するための新しいコンストラクターが含まれています。
  - `NSImageView` 指定されたから編集できないイメージビューを作成するための新しいコンストラクターを追加 `NSImage` します。
- 新しいは、 `NSGridView` サブビューのコレクションを、動的に非表示にしたり表示したりできる可変サイズの行と列を持つグリッドに自動レイアウトするために追加されました。

<a name="AVFoundation-Framework-Enhancements"></a>

## <a name="avfoundation-framework-enhancements"></a>AVFoundation Framework の機能強化

MacOS Sierra の AVFoundation Framework には、次の機能強化が加えられています。

- MacOS では、アプリはコンテンツの種類に基づいてさまざまな [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) 動作を実装する必要がなくなりました。 プロパティを設定するだけ `Rate` で、停止することなく再生できるコンテンツが十分にあるかどうかが AVFoundation によって判断されます。
- 新しい `AVPlayerLooper` クラスを使用すると、再生中に特定のメディアを簡単にループできます。
- クラスでは、 `AVAssetDownloadURLSession` FairPlay の暗号化された HLS ストリームをダウンロードし、後で再生することができます。

<a name="Core-Data-Framework-Enhancements"></a>

## <a name="core-data-framework-enhancements"></a>コアデータフレームワークの機能強化

MacOS Sierra のコアデータフレームワークには、次の機能強化が加えられています。

- ルート [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) オブジェクトは、シリアル化を使用しない同時エラーおよびフェッチをサポートしています。
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator)クラスは、SQLite データストアのプールを保持します。
- WAL ジャーナルモードで SQLite データストアを使用する [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) オブジェクトでは、新しいクエリ生成機能がサポートされています。この機能を使用すると、マネージオブジェクトコンテキスト (MOC) を特定のデータベースバージョンに固定して、将来のフェッチやエラー処理を行うことができます。
- 高レベルを使用して `NSPersistenceContainer` `NSPersistentStoreCoordinator` 、、 [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) 、その他のコアデータ構成リソースを参照します。
- `NSManagedObject`フェッチの実行とサブクラスの作成を容易にするために、いくつかの新しい便利なメソッドが追加されました。

詳細については、Apple の [Core Data Framework リファレンス](https://developer.apple.com/reference/coredata)を参照してください。

<a name="Core-Image-Framework-Enhancements"></a>

## <a name="core-image-framework-enhancements"></a>コアイメージフレームワークの機能強化

MacOS Sierra のコアイメージフレームワークには、次の機能強化が行われています。

- `ImageWithExtent` [Cifilter](https://developer.apple.com/reference/coreimage/cifilter)クラスのメソッドを使用して、フィルター操作にカスタム処理を挿入できます。 コアイメージは、出力または表示のためにイメージを処理するときに、フィルター間で指定されたコールバックを呼び出します。
- これで、アプリは、処理の前後に色空間を変換することによって、コアイメージコンテキストの作業色空間の外にある色空間でイメージを処理できるようになりました。
- コアイメージカーネルは、特定のピクセル出力形式を要求できるようになりました。
- `CINinePartTitled`、、 `CINinePartStretched` `CIHueSaturationValueGradient` 、 `CIEdgePreserveUpsampleFilter` および `CIClamp` の新しいイメージフィルターが追加されました。

<a name="Foundation-Framework-Enhancements"></a>

## <a name="foundation-framework-enhancements"></a>Foundation Framework の機能強化

MacOS Sierra の Foundation Framework には、次の機能強化が加えられています。

- [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) API を使用して、大量、長さ、速度、期間、気温など、最も一般的な物理的な単位の多くを表現、変換、および表示します。
- ISO 8601 形式の日付を解析および生成するには、 [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) クラスを使用します。
- 新しい [Nsdateinterval](https://developer.apple.com/reference/foundation/nsdateinterval) クラスを使用して、間隔の比較と間隔の交差部分のテストを行うために、期間などの日付と時間の間隔を計算します。
- [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter)クラスを使用して、ユーザー名の要素を文字列から解析します。
- 新しい [Nq&a Lsessiontaskmetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) クラスを使用して、URL ネットワークセッションのメトリックを取得します。

詳細については、「 [OS X v 10.12 と iOS 10 の Apple の基礎リリースノート](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html)」を参照してください。

<a name="GameKit-Framework-Enhancements"></a>

## <a name="gamekit-framework-enhancements"></a>お持ちのキットフレームワークの機能強化

MacOS Sierra のために、次のような機能強化が行われています。

- **Game Center アプリ**は非推奨とされ、macOS から削除されました。 アプリで使用する場合は、独自のインターフェイスを提示して、スコアボードなどのユーザーキット機能を表示 _する必要があり_ ます。
- 新しい iCloud 専用のアカウントの種類は、 [Gkcloudplayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) クラスによって実装されています。
- 新しい [Gkq&a session](https://developer.apple.com/reference/gamekit/gkgamesession) クラスは、Game Center で永続的なデータストレージを管理するための一般化されたソリューションを提供します。 `GKGameSession` プレーヤーのリストを保持します。アプリは、参加者の日付の格納、取得、または交換の方法とタイミングを実装するフォームです。 多くの場合、ゲームセッションでは、既存のターンベースの一致、リアルタイムの一致、または永続的なゲーム保存方法を置き換えることができます。

<a name="GamePlayKit-Framework-Enhancements"></a>

## <a name="gameplaykit-framework-enhancements"></a>お持ちのおプレイキットフレームワークの機能強化

MacOS Sierra のために、次のような機能強化が行われています。

- 手続き型のノイズ生成が追加され、自然なテクスチャのリアリティを向上させるために使用できます。また、カメラの動きにリアリティを加え、豊富なゲームワールドを生み出します。
- 空間パーティション分割を使用して、効率的な検索のためにゲームのワールドデータを分割します。
- すべての移動計算を可能にするために、新しいモンテカルロストラテジスト ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) が追加されました。
- ゲームを構築する AI を強化するために、新しいデシジョンツリー API が追加されました ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) と [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode))。
- 新しい [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) クラスと [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) クラスを使用して、既存のエージェントおよびパス検索動作に3d サポートが追加されました。
- 新しい [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) クラスを使用して、高パフォーマンスの自然なパスを提供します。
- 新しい [Gkscene](https://developer.apple.com/reference/gameplaykit/gkscene) クラスと [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) クラスを使用すると、これまでにないようにすることができます。

<a name="Metal-Framework-Enhancements"></a>

## <a name="metal-framework-enhancements"></a>金属フレームワークの機能強化

MacOS Sierra のメタルフレームワークには、次の機能強化が行われています。

- 3D アプリやゲームでは、 _テセレーション_ を使用して、複雑なシーンとジオメトリを GPU 経由で効率的にレンダリングできるようになりました。
- 関数の特殊化を使用して、シーンに対して高度に最適化されたマテリアルおよびライトの組み合わせ関数のコレクションを作成します。
- リソースの割り当てをきめ細かく制御することにより、リソースヒープと Memoryless レンダーターゲットを使用して金属ベースのアプリのパフォーマンスを最適化できます。

詳細については、Apple の [金属のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)を参照してください。

<a name="ModelIO-Framework-Enhancements"></a>

## <a name="model-io-framework-enhancements"></a>モデル i/o フレームワークの機能強化

MacOS Sierra のモデル i/o フレームワークには、次の機能強化が行われています。

- 米国ドルのファイル形式がサポートされるようになりました。
- 新しい `MDLMaterialPropertyGraph` クラスを使用すると、モデルに対する実行時の変更を簡単にサポートできます。
- 符号付き距離フィールドのサポートが [Mdlvoxelarray](https://developer.apple.com/reference/modelio/mdlvoxelarray) クラスに追加されました。
- 新しいクラスを使用すると、 `MDLLightProbeIrradianceDataSource` プローブの配置を簡単に行うことができます。

<a name="Photos-Framework-Enhancements"></a>

## <a name="photos-framework-enhancements"></a>Photos フレームワークの機能強化

MacOS Sierra の Photos フレームワークには、次のような機能強化が加えられています。

- 写真フレームワークと写真編集拡張機能をサポートするアプリ (写真およびカメラアプリ内で使用) でライブフォト編集ができるようになりました。
- 新しい [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) クラスを使用して、ライブ写真のビデオと静止コンテンツの両方に編集を適用します。
- [Ciimageprocessorinput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput)クラスと[Ciimageprocessorinput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput)クラスを使用して、新しいコアイメージプロセッサ機能を利用して編集を行います。
- ライブ写真をサポートするために、 [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) クラスと [PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) クラスは iOS から macOS に移植されています。

<a name="SceneKit-Framework-Enhancements"></a>

## <a name="scenekit-framework-enhancements"></a>SceneKit Framework の機能強化

MacOS Sierra の SceneKit Framework には、次の機能強化が行われています。

- には、より簡単なアセット作成により、より現実的な結果を得るために、新しい物理的なベースのレンダリング (.PBR) システムが追加されました。
- 新しい [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) シェーディングモデルを使用して、さまざまな現実的な網掛け効果を製品に追加し、3つの基本プロパティ ( `Diffuse` 、および) のみを必要 `Metalness` と `Roughness` します。
- .PBR の網掛けは環境ベースの光源で最適に機能するため、プロパティを使用して `LightingEnvironment` イメージベースの照明をシーン全体に適用します。
- プロパティを使用して、 `IESProfileURL` 光源 (ルーメン) や色温度 (ケルビン単位) などの実際の値に光源を定義する実際の照明器具をインポートします。
- [Scncamera](https://developer.apple.com/reference/scenekit/scncamera)クラスは、HDR の特徴と効果を使用して、よりリアリティを高めることができます。 アダプティブ露出を使用して自動効果を作成するか、vignetting、color fringing、色の調整を使用して、ゲームに filmatic 効果を追加します。
- SceneKit と HDR の両方のカメラ機能は、従来の表示手法よりも優れた結果を提供します。結果として、では、すべての色の計算を線形の色空間で実行するようになりました (ワイド色のデバイスディスプレイで P3 の色域外を使用)。
- SceneKit color プロファイル情報を読み取って、すべての色に一致するようになりました。
- SceneKit は、すべてのシェーダーの種類の線形 RGB カラー空間で色コンポーネントの値を解釈します。
- テクスチャイメージでカラープロファイル情報の読み取りと調整を SceneKit するため、すべてのイメージに対してアセットカタログを使用して、この情報が確実に提供されるようにします。
- `SCNDisableLinearSpaceRendering` `SCNDisableWideGamut` アプリのでキーとキーを指定することにより、線形色空間のレンダリングとワイド色の両方を無効にできます `Info.plist` 。
- 新しい [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/documentation/scenekit/scngeometryprimitivetype/scngeometryprimitivetypepolygon?language=objc) クラスで geometry を指定するために、任意の多角形の霊長類 (ファイルから読み込まれるか、プログラムによって生成される) を構築します。

<a name="Security-Framework-Enhancements"></a>

## <a name="security-framework-enhancements"></a>セキュリティフレームワークの機能強化

MacOS Sierra のセキュリティフレームワークには、次の機能強化が行われています。

- この `SecKey` インターフェイスは、すべてのプラットフォーム (iOS、tvOS、watchOS、macOS) で最新で統合されています。

<a name="SpriteKit-Framework-Enhancements"></a>

## <a name="spritekit-framework-enhancements"></a>SpriteKit Framework の機能強化

MacOS Sierra の SpriteKit Framework には、次の機能強化が行われています。

- Tilemaps は、、、およびの各クラスを使用して、2d、2.5 d、およびサイドスクロールゲーム用の正方形、六角、および等角投影のタイル図形をサポートするようになりました `SKTileMapMode` `SKTileGroup` `SKTileGroupRule` `SKTileSet` 。
- 新しいクラスを使用して、 `SKWarpGeometry` [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) または [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) のレンダリングを伸縮またはデフォルメします。 新しい [Skaction](https://developer.apple.com/reference/spritekit/skaction) クラスは、ワープ効果間の遷移をアニメーション化するために使用できます。
- カスタムシェーダーは `SKAttribute` 、属性値 () を指定することによって、シェーダーを使用する各ノードで個別に構成できる属性 () を提供でき `SKAttributeValue` ます。
- [Skview](https://developer.apple.com/reference/spritekit/skview)クラスには、シーンをレンダリングするタイミングと方法をきめ細かく制御できる新しいメソッドがいくつか用意されています。

<a name="New-Frameworks"></a>

## <a name="new-frameworks"></a>新しいフレームワーク

次のフレームワークが macOS Sierra に追加されました。

- **インテントフレームワーク** -このフレームワークを使用すると、アプリで相互作用 (場所やユーザーアクションなど) を確認し、その情報に基づいてアクションを実行できます。
- Safari **Ariservices フレームワーク**-このフレームワークにより、アプリは MacOS と iOS の両方に対して Safari 用のアプリ拡張機能 (コンテンツブロッカーなど) を開発できます。

## <a name="related-links"></a>関連リンク

- [Mac サンプル](/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [OS X 10.12 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)