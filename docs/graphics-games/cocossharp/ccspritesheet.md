---
title: CCSpriteSheet でフレーム レートの向上
description: CCSpriteSheet では、結合して 1 つのテクスチャで多くのイメージ ファイルを使用するための機能を提供します。 テクスチャの数を減らすと、ゲームの読み込み時間とフレーム レートを改善できます。
ms.prod: xamarin
ms.assetid: A1334030-750C-4C60-8B84-1A8A54B0D00E
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: eab6153653a8c8df2068aaaf879d84d35473c541
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123566"
---
# <a name="improving-frame-rate-with-ccspritesheet"></a>CCSpriteSheet でフレーム レートの向上

_CCSpriteSheet では、結合して 1 つのテクスチャで多くのイメージ ファイルを使用するための機能を提供します。テクスチャの数を減らすと、ゲームの読み込み時間とフレーム レートを改善できます。_

多くのゲームをスムーズに実行し、モバイル ハードウェアですばやく読み込みの最適化が必要です。 `CCSpriteSheet`クラスは CocosSharp ゲームで発生した多くの一般的なパフォーマンス問題に対処できます。 このガイドは、一般的なパフォーマンス問題とその対処方法について説明します。 を使用して、`CCSpriteSheet`クラス。


## <a name="what-is-a-sprite-sheet"></a>スプライト シートとは何ですか。

A*スプライト シート*、これも参照できますとして、*テクスチャ アトラス*、1 つのファイルに複数のイメージを結合するイメージします。 これは、コンテンツの読み込み時間と実行時のパフォーマンスを向上させることができます。

たとえば、次の図は、3 つの独立したイメージによって作成された単純なスプライト シートは。 個々 のイメージは、任意のサイズを指定でき、結果のスプライト シートは完全に入力する必要はありません。

![](ccspritesheet-images/image1.png "個々 のイメージは、任意のサイズを指定でき、結果のスプライト シートは完全に入力する必要はありません。")


### <a name="render-states"></a>状態を表示します。

CocosSharp のビジュアル オブジェクト (など`CCSprite`) を頂点バッファーの作成を必要とするグラフィカル MonoGame など、OpenGL API レンダリング コードを従来よりもレンダリング コードを簡略化 (」の説明に従って、[で 3D グラフィックスの描画MonoGame の頂点](~/graphics-games/monogame/3d/part2.md)ガイド)。 、その単純さに関係なく CocosSharp なくなるわけではない設定のコスト*レンダリング ステート*、レンダリング コードでテクスチャまたはその他のレンダリングに関連する状態を切り替える必要がありますを時間数であります。

CocosSharp の内部のコード要素をレンダリング各 visual 順番に、現在の以降のビジュアル ツリーを走査して`CCScene`します。 たとえば、次のシーンがあるとします。

![](ccspritesheet-images/image2.png "このシーンを検討してください。")

CocosSharp では、シーケンスで 4 つの星をレンダリングは。

![](ccspritesheet-images/image3.png "CocosSharp はシーケンスで星 4 つを表示")

以降の各`CCSprite`同じのテクスチャを使用して CocosSharp では、次の 4 つの星をすべてをまとめてグループします。 このコードでは、フレームごとに 1 つのみのレンダリング状態割り当て (スターのテクスチャの割り当て) が必要です。 このシナリオでは、非常に効率的です。

もちろん、ほとんどのゲームは、1 つのイメージを使用します。 次のシーンには、地球のグラフィックが導入されています。

![](ccspritesheet-images/image4.png "このシーンに地球グラフィックが導入されています")

理想的には CocosSharp では、まず (星) などの 1 つのイメージの他のイメージ (惑星) を使用してスプライトの残りの部分を使用してすべてのスプライトを描画する必要があります。

![](ccspritesheet-images/image5.png "CocosSharp によるなど、他のイメージ、地球を使用して、sprite の残りの部分、星 1 つのイメージを使用して最初にすべてのスプライトの描画が理想的です。")

2 つの表示状態が必要です上記の順序: 最初の世界でスター、最初の 1 つ。

すべて`CCSprite`インスタンスの同じ子である`CCNode`CocosSharp はレンダリング状態の変化を削減する描画順序を最適化します。 一方で、表示されている場合、 `CCSprite` CocosSharp はレンダリングを最適化することができるように、インスタンスが構成されていますが (別のエンティティの一部である場合など`CCNode`インスタンス)、順序が最適なできない可能性があります。 5 つの描画状態で、最悪の可能な描画順序を次に示します。

![](ccspritesheet-images/image6.png "これは 5 つの描画状態で、最悪の可能な描画順序を示しています")

描画状態の描画順序はのビジュアル ツリーに従う必要があるために、最適化には難しい`CCNode`インスタンス。 このツリーは多くの場合、(エンティティなど、ビジュアルな子を含む) を使用しやすくする構造化または、アーティストで定義されているため、必要な視覚的なレイアウトに編成します。

もちろん、理想的な状態をであっても複数のイメージの 1 つのレンダー状態です。 CocosSharp のゲームは 1 つのファイルにすべてのイメージを結合し、その 1 つのファイルの読み込みでこれを実現できます (とそれに付随する **.plist**ファイル) に、`CCSpriteSheet`します。 使用して、`CCSpriteSheet`クラスがさらにゲーム、イメージの数が多いであるか、これが非常に重要なレイアウトが複雑です。 

### <a name="load-times"></a>読み込み時間

1 つのファイルに複数のイメージを結合することと、さまざまな理由から、ゲームの読み込み時間も改善されます。

 - 1 つのファイルに複数のイメージを組み合わせることで効率的にパッキングを通じて使用されるピクセルの全体的な数を削減できます。
 - .Png ヘッダーを解析するなど、ファイルごとのオーバーヘッドを減らすことを意味が少ないファイルの読み込み
 - 少ないファイルの読み込みには、ディスク ベースのメディアの Dvd と従来のコンピューターのハード ドライブなどの重要なは、時刻をシーク以下が必要です。

## <a name="using-ccspritesheet-in-code"></a>CCSpriteSheet をコードで使用します。

作成する、`CCSpriteSheet`インスタンス、コードは、イメージおよび各フレームを使用するイメージの領域を定義するファイルを指定する必要があります。 イメージとして読み込むことができます、 **.png**または **.xnb**ファイル (を使用する場合、[コンテンツ パイプライン](~/graphics-games/cocossharp/content-pipeline/index.md))。 フレームを定義するファイルは、 **.plist**ファイルを手動で作成できるまたは*TexturePacker* (これは、私たちは、以下について説明します)。

サンプル アプリでを[でダウンロードできます](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)、作成、`CCSpriteSheet`から、 **.png**と **.plist**を使用した次のコード ファイルします。

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

読み込まれた後、`CCSpriteSheet`が含まれています、`List`の`CCSpriteFrame`– シート全体を作成するために使用するソース イメージのいずれかに対応する各インスタンスのインスタンスします。 場合、 **SpriteSheetDemo**プロジェクト、 `CCSpriteSheet` 3 つのイメージが含まれています。 **.Plist** Visual Studio for Mac で、またはどのイメージが使用可能な任意のテキスト エディターでファイルを検査することができます。 マイクロソフトを参照する場合、 **.plist** 3 つのフレーム (セクションのキー名を強調するために省略) を参照してください、テキスト エディターでファイル。


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

使用して、`Find`フレームを次のように、名前で検索するメソッド (コードを強調するために省略すると`CCSpriteFrame`使用状況)。


```csharp
CCSpriteFrame frame;
...
frame = sheet.Frames.Find(item=>item.TextureFilename == "farBackground.png"); 
CCSprite sprite = new CCSprite (frame); 
...
```

以降、`CCSprite`コンス トラクターがかかることができます、`CCSpriteFrame`パラメーターのコードは決しての詳細を調査する必要が、 `CCSpriteFrame`、どのテクスチャなどを使用するか、マスターのスプライト シート内のイメージのリージョン。


## <a name="creating-a-sprite-sheet-plist"></a>スプライト シートの .plist を作成します。

形式の .plist ファイルは、作成して手動で編集できる xml ベース ファイル、です。 同様に、イメージ編集プログラムは、1 つの大きなファイルに複数のファイルの結合を使用できます。 作成し、スプライト シートは非常に時間がかかる、保守、以降は、CocosSharp 形式のファイルをエクスポートすることができる TexturePacker プログラムに注目します。 TexturePacker は無料と、"Pro"のバージョンを提供し、Windows および Mac OS。 このガイドの残りの部分は、無料のバージョンを使用して実行できます。 

TexturePacker、 [TexturePacker web サイトからダウンロード](https://www.codeandweb.com/texturepacker)します。 開かれると、TexturePacker はプロジェクトが読み込まれていません。 開始画面のスプライトを追加、最近使ったプロジェクト (他のプロジェクトが作成されている) 場合を開き、スプライト シートを使用する形式を選択することができます。 CocosSharp では、Cocos2D のデータ形式を使用します。

![](ccspritesheet-images/image7.png "CocosSharp は、Cocos2D データ形式を使用します。")

イメージ ファイル (など **.png**) ドラッグ アンド ドロップして Windows エクスプ ローラーから Windows または mac のファインダー、TexturePacker に追加することができます ファイルが追加されるたびに、TexturePacker は自動的にスプライト シートのプレビューを更新します。

![](ccspritesheet-images/image8.png "TexturePacker は、ファイルが追加されるたびに自動的にスプライト シートのプレビューを更新します。")

スプライト シートをエクスポートするには、クリックして、**発行スプライト シート**ボタンをクリックし、スプライト シートの場所を選択します。 TexturePacker は形式の .plist ファイルとイメージ ファイルを保存します。

結果として得られるファイルを使用するには、.png と .plist の両方を CocosSharp プロジェクトに追加します。 CocosSharp プロジェクトにファイルを追加する方法については、次を参照してください。、 [BouncingGame ガイド](~/graphics-games/cocossharp/bouncing-game.md)します。 読み込まれることができますが、ファイルが追加される、`CCSpriteSheet`上記のコードで以前表示されます。

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

### <a name="considerations-for-maintaining-a-texturepacker-sprite-sheet"></a>TexturePacker スプライト シートを維持するための考慮事項

ゲームを開発、アーティスト可能性がありますを追加、削除、またはアートを変更します。 変更には、更新されたスプライト シートが必要です。 次の考慮事項には、スプライト シートのメンテナンスは容易します。

 - 元のファイル (スプライト シートを作成するために使用するファイル) のプロジェクトで、フォルダーに保存し、バージョン管理に追加されるかどうかを確認します。 これらのファイルは、変更が加えられるたびに、スプライト シートを再作成に必要になります。
 - Visual studio for Mac または Visual Studio は、元のファイルを追加または追加されている場合は設定しないでください、**ビルド アクション**に**None**します。 ファイルが追加され、プラットフォーム固有であるかどうかは**ビルド アクション**、結果として得られるアプリのファイル サイズが不必要に長くなります。
 - 使用を検討して*スマート フォルダー* TexturePacker でします。 スマート フォルダーは、スプライト シートに含まれているイメージを自動的に追加します。 この機能は、イメージの数が多い多くのゲームを開発時間を節約できます。 

    ![](ccspritesheet-images/image9.png "この機能は、イメージの数が多い多くのゲームを開発時間を節約できます。")
 - スプライト シートのテクスチャ サイズに注意します。 一部の古い phone ハードウェアは 2048 x 2048 よりも大きいテクスチャ サイズをサポートしていません。 また、2048 x 2048 の 32 ビット イメージは、RAM – 大量のメモリの約 17 (mb) を使用します。
 - TexturePacker は含まれませんフォルダー スプライトの名前に既定では、ため、名前の競合が可能です。 名前に含めるフォルダーをかどうか、または開発の先頭ではなくを決定することをお勧めします。 大規模なゲームでは、競合を避けるためにフォルダー名を使用を検討してください。 フォルダーのパスを含めるをクリックします。**高度な表示**で、**データ**セクションと確認**フォルダー名の先頭**します。 

    ![](ccspritesheet-images/image10.png "フォルダーのパスを含める をクリックがデータ セクションでは高度なを表示し、フォルダー名の先頭を確認します。")

## <a name="summary"></a>まとめ

このガイドを作成して使用する方法を説明します、`CCSpriteSheet`クラス。 ファイルに読み込むことができますを構築する方法についても説明`CCSpriteSheet`TexturePacker プログラムを使用してインスタンスします。

## <a name="related-links"></a>関連リンク

- [CCSpriteSheet](https://developer.xamarin.com/api/type/CocosSharp.CCSpriteSheet/)
- [完全なデモ (サンプル)](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)
