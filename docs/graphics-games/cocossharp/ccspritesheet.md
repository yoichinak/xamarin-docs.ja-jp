---
title: CCSpriteSheet フレーム レートの向上
description: CCSpriteSheet は、結合および 1 つのテクスチャで多数のイメージ ファイルを使用するための機能を提供します。 テクスチャ数を減らすと、ゲームの読み込み時間とフレーム レートを向上させることができます。
ms.prod: xamarin
ms.assetid: A1334030-750C-4C60-8B84-1A8A54B0D00E
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 9b0f58554b26b1a5334970b8c1288234acbf8db7
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="improving-frame-rate-with-ccspritesheet"></a>CCSpriteSheet フレーム レートの向上

_CCSpriteSheet は、結合および 1 つのテクスチャで多数のイメージ ファイルを使用するための機能を提供します。テクスチャ数を減らすと、ゲームの読み込み時間とフレーム レートを向上させることができます。_

多くのゲームでは、スムーズに実行し、モバイル ハードウェアでのロードは速かった最適化作業が必要です。 `CCSpriteSheet`クラスの CocosSharp ゲームで発生した多くの一般的なパフォーマンス問題の解決に役立つことができます。 このガイドは、一般的なパフォーマンス問題とその対処方法について説明します。 を使用して、`CCSpriteSheet`クラスです。


## <a name="what-is-a-sprite-sheet"></a>スプライト シートとは何ですか。

A*スプライト シート*、することがあるとも呼びます、*テクスチャ地図*、イメージを 1 つのファイルに複数のイメージを組み合わせたものです。 これは、コンテンツの読み込み時間と実行時のパフォーマンスを向上させることができます。

たとえば、次の図は、3 つの独立したイメージによって作成された単純なスプライト シートはします。 個々 のイメージは、任意のサイズを指定でき、結果として得られるスプライト シートが完全に入力する必要はありません。

![](ccspritesheet-images/image1.png "個々 のイメージは、任意のサイズを指定でき、結果として得られるスプライト シートが完全に入力する必要はありません。")


### <a name="render-states"></a>状態を表示します。

ビジュアルの CocosSharp オブジェクト (など`CCSprite`)、頂点バッファーの作成を必要とする従来のグラフィカル API レンダリング コード MonoGame など OpenGL、経由でレンダリング コードを簡略化 (」の説明に従って、[描画と 3D グラフィックスMonoGame の頂点](~/graphics-games/monogame/3d/part2.md)ガイド)。 その簡潔さに関係なく CocosSharp 消去しません設定のコスト*レンダリング ステート*、レンダリング コードでテクスチャまたは他のレンダリングに関連する状態を切り替える必要がある回数を超えるはします。

CocosSharp の内部コード要素をレンダリング各 visual 順番に、現在のビジュアル ツリーの先頭を走査して`CCScene`です。 たとえば、次のシーンがあるとします。

![](ccspritesheet-images/image2.png "このシーンを検討してください。")

CocosSharp は、シーケンスで 4 個の星をレンダリングは。

![](ccspritesheet-images/image3.png "シーケンスで 4 個の星をレンダリングは CocosSharp")

以降の各`CCSprite`同じテクスチャを使用して CocosSharp に同時に 4 つの星をすべてをグループ化します。 このコードでは、1 つだけ表示状態リソース割り当て星型のテクスチャのフレームごとに必要です。 このシナリオでは、非常に効率的です。

もちろん、ほとんどのゲームは、1 つだけのイメージを使用します。 次のシーンでは、地球グラフィックが導入されています。

![](ccspritesheet-images/image4.png "このシーン惑星グラフィックが導入されています")

理想的には CocosSharp では、最初 (など)、星 1 つのイメージの他のイメージ (地球) を使用してスプライトの残りの部分を使用してすべてのスプライトを描画する必要があります。

![](ccspritesheet-images/image5.png "理想的には CocosSharp が最初に 1 つのイメージを使用し、その他のイメージは、地球を使用してスプライトの残りの部分、星などすべてスプライトを描画する必要があります。")

2 つのレンダリング ステートが必要です上記の順序付け: 最初の地球上スター、最初のいずれか。

すべて`CCSprite`インスタンスが同じ子`CCNode`CocosSharp がレンダリング状態の変更を削減するように描画順序を最適化し、します。 場合、その一方で、`CCSprite`インスタンスは構成により CocosSharp のレンダリングを最適化することはありません (別のエンティティの一部である場合など`CCNode`インスタンス)、順序が最適でない、します。 5 つの表示状態の結果として得られる、最悪の可能な描画順序を次に示します。

![](ccspritesheet-images/image6.png "5 つの表示状態の結果として得られる、最悪の可能な描画順序を示します")

レンダリングの状態が困難になる描画順でのビジュアル ツリーを従う必要がありますがあるために最適化`CCNode`インスタンス。 このツリー多くの場合、作業を容易に (など、visual の子を含む)、エンティティである構造化されたか、または、アーティストで定義されているため、必要なビジュアルなレイアウトに構成されています。

もちろん、理想的な状態を複数のイメージでも、1 つのレンダー状態です。 CocosSharp ゲームこれを行うには 1 つのファイルにすべてのイメージを結合し、その 1 つのファイルの読み込み (それに付随すると共に **.plist**ファイル) に、`CCSpriteSheet`です。 使用して、`CCSpriteSheet`クラスがさらにゲームが、イメージの数が多いこともある非常に重要なレイアウトが複雑になりです。 

### <a name="load-times"></a>読み込み時間

1 つのファイルに複数のイメージを結合すると、さまざまな理由から、ゲームの読み込み時間も向上します。

 - 1 つのファイルに複数のイメージを組み合わせることで効率的にパッキングを介して使用されるピクセルの全体的な数を削減できます。
 - .Png ヘッダーを解析など、ファイルごとのオーバーヘッドを減らすことを意味が少ないファイルを読み込んでいます
 - ファイル数を減らしてを読み込むには、ディスク ベースのメディアの Dvd と従来のコンピューターのハード ドライブなどの重要となる時間をシーク小さい必要があります。

## <a name="using-ccspritesheet-in-code"></a>コードで CCSpriteSheet の使用

作成する、`CCSpriteSheet`インスタンス、イメージとフレームごとに使用するイメージの領域を定義するファイル、コードを指定する必要があります。 としてイメージを読み込むことができます、 **.png**または **.xnb**ファイル (を使用する場合、[コンテンツ パイプライン](~/graphics-games/cocossharp/content-pipeline/index.md))。 フレームを定義するファイルが、 **.plist**を手動で作成することがあるファイルまたは*TexturePacker* (これは、以下について説明します)。

サンプル アプリを[でダウンロードできます](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)、作成、`CCSpriteSheet`から、 **.png**と **.plist**ファイルの次のコードを使用します。

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

読み込まれると、`CCSpriteSheet`が含まれています、`List`の`CCSpriteFrame`– シート全体を作成するために使用するソース イメージの 1 つに対応する各インスタンスのインスタンスします。 場合、 **SpriteSheetDemo**プロジェクト、 `CCSpriteSheet` 3 つのイメージが含まれています。 **.Plist**または Mac 用の Visual Studio で使用可能ながある画像を表示する任意のテキスト エディターでファイルを検査することができます。 マイクロソフトを参照する場合、 **.plist**ファイルは次の 3 つのフレーム数 (セクションのキー名を強調するを省略すると) を参照してください、テキスト エディターで。


```csharp
...
<dict>
    <key>frames</key>
    <dict>
        <key>farBackground.png</key>
        ...
        <key>foreground.png</key>
        ...
<key>forestBackground.png</key>
...
```

使用して、`Find`フレームを次のように、名前で検索するメソッド (強調するコードを省略すると`CCSpriteFrame`使用状況)。


```csharp
CCSpriteFrame frame;
...
frame = sheet.Frames.Find(item=>item.TextureFilename == "farBackground.png"); 
CCSprite sprite = new CCSprite (frame); 
...
```

以降、`CCSprite`コンス トラクターがかかることができます、`CCSpriteFrame`パラメーター、コードではことの詳細を調査するために、 `CCSpriteFrame`、使用しているテクスチャなど、またはマスター スプライト シート内のイメージの地域。


## <a name="creating-a-sprite-sheet-plist"></a>スプライト シート .plist を作成します。

形式の .plist ファイルは、xml ベース、ファイル、作成および手動で編集できます。 同様に、編集プログラム イメージを使用して、1 つの大きなファイルに複数のファイルを結合することができます。 以降の作成と保守スプライト シートを非常に時間がかかることができます、見ていきます TexturePacker プログラムが CocosSharp 形式のファイルをエクスポートできます。 TexturePacker で、空きと、"Pro"バージョンには、Windows と Mac OS 使用します。 このガイドの残りの部分は、無料版を使用して実行できます。 

TexturePacker を指定できます[TexturePacker web サイトからダウンロード](https://www.codeandweb.com/texturepacker)です。 開かれると、TexturePacker には読み込まれているプロジェクトはありません。 開始画面を使用すると、スプライトを追加、(他のプロジェクトが作成されている) 場合の最近使ったプロジェクトを開き、スプライト シートを使用する形式を選択できます。 CocosSharp は、Cocos2D データ形式を使用します。

![](ccspritesheet-images/image7.png "CocosSharp は、Cocos2D データ形式を使用します。")

イメージ ファイル (など **.png**) ドラッグ アンド ドロップによってに Windows エクスプ ローラーから Windows または mac 上の Finder TexturePacker に追加することができます ファイルが追加されるたびに、TexturePacker は自動的にスプライト シートのプレビューを更新します。

![](ccspritesheet-images/image8.png "ファイルが追加されるたびにスプライト シートのプレビューが TexturePacker によって自動的に更新します。")

スプライト シートをエクスポートする をクリックして、**発行スプライト シート**ボタンをクリックし、スプライト シートの場所を選択します。 TexturePacker は .plist ファイルとイメージ ファイルに保存します。

結果として得られるファイルを使用するには、CocosSharp プロジェクトに .png と .plist の両方を追加します。 CocosSharp プロジェクトにファイルを追加する方法については、次を参照してください。、 [BouncingGame ガイド](~/graphics-games/cocossharp/bouncing-game.md)です。 ファイルが追加されると、それらに読み込まれる、`CCSpriteSheet`上記のコードで前述したとおりでした。

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

### <a name="considerations-for-maintaining-a-texturepacker-sprite-sheet"></a>TexturePacker スプライト シートを維持するための考慮事項

ゲームを開発すると、アーティスト可能性がありますを追加、削除、またはアートを変更します。 すべての変更には、更新されたスプライト シートが必要です。 次の考慮事項がスプライト シート メンテナンス簡単になります。

 - 元のファイル (スプライト シートの作成に使用されるファイル) のプロジェクトのフォルダーに保存し、バージョン管理に追加されるかどうかを確認します。 これらのファイルは、スプライト シートの変更が加えられるたびに再作成に必要になります。
 - Visual studio for Mac または Visual Studio は、元のファイルを追加設定したりしないで追加されている場合、**ビルド アクション**に**None**です。 ファイルが追加され、プラットフォーム固有であるかどうかは**ビルド アクション**、結果のアプリのファイル サイズが不必要にアップし、します。
 - 使用を検討して*フォルダーをスマート*TexturePacker にします。 スマート フォルダーは、スプライト シートに含まれているイメージを自動的に追加します。 この機能は、多くのゲームを開発する時間を多数のイメージに保存できます。 

    ![](ccspritesheet-images/image9.png "この機能はイメージの数が多い、多くのゲームを開発時間を節約することができます。")
 - スプライト シート テクスチャ サイズに注意します。 一部の古い phone ハードウェアは、2048 x 2048 を超えるテクスチャ サイズをサポートしていません。 また、2048 x 2048 の 32 ビットのイメージは、RAM: 大量のメモリのメガバイト数をほぼ 17 を使用します。
 - TexturePacker がフォルダーに含めないスプライト名既定では、名前の競合も有効であるためです。 開発の先頭ではなくまたはフォルダーを含めるには、名前かどうかを決定することをお勧めします。 大規模なゲームでは、競合を避けるためにフォルダー名を使用を検討してください。 フォルダーのパスを含めるには、クリックして**高度な表示**で、**データ**セクションし、確認**Prepend フォルダー名**です。 

    ![](ccspritesheet-images/image10.png "フォルダーのパスを含めるをクリックして表示高度なデータ セクションにして Prepend フォルダー名を確認")

## <a name="summary"></a>まとめ

このガイドの作成し、使用する方法を説明する、`CCSpriteSheet`クラスです。 ファイルに読み込むことができますを作成する方法についても説明`CCSpriteSheet`TexturePacker プログラムを使用してインスタンス化します。

## <a name="related-links"></a>関連リンク

- [CCSpriteSheet](https://developer.xamarin.com/api/type/CocosSharp.CCSpriteSheet/)
- [完全デモ (サンプル)](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)
