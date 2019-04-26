---
title: CocosSharp によるタイル表示
description: 並べて表示された、強力な柔軟性が高くは、ゲームのマップを直交と等角 タイルを作成するための完成度の高いアプリケーション。 CocosSharp では、並べて表示のネイティブなファイル形式の組み込みの統合を提供します。
ms.prod: xamarin
ms.assetid: 804C042C-F62A-4E6C-B10F-06528637F0E2
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 8e7ef890af264bb08827d86c635d555184f1ec00
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61235960"
---
# <a name="using-tiled-with-cocossharp"></a>CocosSharp によるタイル表示

_並べて表示された、強力な柔軟性が高くは、ゲームのマップを直交と等角 タイルを作成するための完成度の高いアプリケーション。CocosSharp では、並べて表示のネイティブなファイル形式の組み込みの統合を提供します。_

*並べて*アプリケーションを作成するための標準*maps のタイル*ゲーム開発で使用します。 このガイドは .tmx ファイル (並べて表示によって作成されたファイル) を受け取り、CocosSharp ゲームで使用する方法について説明します。 具体的には、このガイドを取り上げます。

 - マップのタイルの目的は、
 - .Tmx ファイルの使用
 - アートのピクセルをレンダリングするための考慮事項
 - 実行時にタイルのプロパティを使用します。

ときに完了に、次のデモ。

![](tiled-images/image1.png "このガイドの手順に従って作成したデモ アプリ")


## <a name="the-purpose-of-tile-maps"></a>マップのタイルの目的は、

マップのタイルは、数十年、ゲームの開発に存在しているが、効率性と esthetics の 2D ゲームでもよく使用されます。 マップのタイルは、マップのタイルで使用されるソース イメージ – タイルのセットの使用による効率改善の非常に高いレベルを実現するためにできます。 タイルのセットは、1 つのファイルに結合するイメージのコレクションです。 タイルのセットは、マップのタイルで使用されるイメージを参照してください、複数の小さい画像が含まれているファイルとも呼ばれますスプライト シートまたはスプライトがゲーム開発にマップされます。 このデモで使用するタイルのセットをグリッドを追加することでタイルのセットを使用する方法を視覚化できます。

![](tiled-images/image2.png "今回のデモで使用されるタイルのセットをグリッドを追加することでタイルのセットを使用する方法の視覚化されたビュー")

マップのタイルはタイルのセットから個々 のタイルを配置します。 各タイルのマップが、タイルの独自のコピーを格納する必要がないことに注意してください設定 – 代わりに、複数のマップのタイルを参照できます同じタイルのセット。 これは、タイルのセットとは別のマップのタイルが必要はほとんどのメモリを意味します。 これにより、多数のタイルのマップの作成など、大規模なゲーム プレイ領域の作成に使用している場合でも、[スクロール platformer](https://en.wikipedia.org/wiki/Platform_game)環境。 同じタイルのセットを使用可能な配置を次に示します。

![](tiled-images/image3.png "このイメージは、同じタイルのセットを使用可能な配置を示しています。")

![](tiled-images/image4.png "このイメージは、同じタイルのセットを使用可能な配置を示しています。")


## <a name="working-with-tmx-files"></a>.Tmx ファイルの使用

.Tmx ファイル形式は、可能性がある並べてアプリケーションによって作成された XML ファイルを[並べて web サイトで無料でダウンロード](http://www.mapeditor.org/)します。 .Tmx ファイルの形式では、マップのタイル情報を格納します。 通常、ゲームでは、レベル、または別の領域ごとに 1 つの .tmx ファイルがあります。

このガイドは CocosSharp; の既存の .tmx ファイルを使用する方法について重点的に取り上げます。ただし、追加のチュートリアルを参照して、含む[並べてマップ エディターのこの概要](http://gamedevelopment.tutsplus.com/tutorials/introduction-to-tiled-map-editor--gamedev-2838)します。

解凍する必要があります、[コンテンツの zip ファイル](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true)ゲームで使用します。 最初に注目するは、タイルのマップ ファイルを使用して両方 .tmx (dungeon.tmx) と 1 つまたはタイルを定義する複数のイメージ ファイル (dungeon_1.png) のデータを設定することができます。 これらのファイルを読み込み、実行時に .tmx の両方を含めるゲームのニーズがこれをプロジェクトの両方を追加**コンテンツ**フォルダー (に含まれている、**資産**Android プロジェクトのフォルダー)。 具体的には、という名前のフォルダーにファイルを追加**tilemaps**内で、**コンテンツ**フォルダー。

![](tiled-images/image5.png "コンテンツのフォルダー内に tilemaps をという名前のフォルダーにファイルを追加します。")

**Dungeon.tmx**に実行時にファイルを読み込むようになりましたことが、`CCTileMap`オブジェクト。 次に、変更、 `GameLayer` (または同等のルート コンテナー オブジェクト) を格納する、`CCTileMap`インスタンスおよび子として追加します。


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");
        this.AddChild (tileMap);
    }
} 
```

ゲームを実行した場合、画面の左下隅にタイルのマップが表示されます。

![](tiled-images/image6.png "ゲームを実行すると場合、タイルのマップは、画面の左下隅に表示されます。")


## <a name="considerations-for-rendering-pixel-art"></a>アートのピクセルをレンダリングするための考慮事項

ピクセルのアート、ビデオ ゲームの開発のコンテキストでは、通常は、手動で、作成し、低解像度は、多くの場合、2 D visual アートを指します。 ピクセル アートは広い範囲で、時間がかかるためピクセル アートのタイルのセット、多くの場合、16 バイトまたは 32 ピクセル幅や高さなどの低解像度のタイルを作成します。 実行時に何も掛けません場合、ピクセル アートは小さすぎるため、ほとんどの最新のスマート フォンやタブレットで、多くの場合。

表示されているディメンションへの呼び出しを追加します、ゲームの GameAppDelegate.cs ファイルで調整できる`CCScene.SetDefaultDesignResolution`:


```csharp
 public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");

    CCSize windowSize = mainWindow.WindowSizeInPixels;

    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to be (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDefaultDesignResolution (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    
    // Determine whether to use the high or low def versions of our images
    // Make sure the default texel to content size ratio is set correctly
    // Of course you're free to have a finer set of image resolutions e.g (ld, hd, super-hd)
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }

    // New code:
    CCScene.SetDefaultDesignResolution (380, 240, CCSceneResolutionPolicy.ShowAll);

    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);

    mainWindow.RunWithScene (scene);
} 
```

詳細については`CCSceneResolutionPolicy`のガイドを参照してください[CocosSharp で解像度処理](~/graphics-games/cocossharp/resolutions.md)します。

ゲームを今すぐ実行はデバイスの全画面表示をゲームにとると、私たちがわかります。

![](tiled-images/image7.png "デバイスの全画面表示をゲームにとると、")

最後に、タイルのマップのアンチエイリアシングを無効にするされます。 `Antialiased`ズームインするオブジェクトを表示するときに、プロパティにはブラー効果が適用されます。 アンチエイリアシングはのグラフィカル オブジェクトは、ピクセル ファイルの場所を削減するのに便利ですが、独自のレンダリング アーティファクトが生じる可能性がありますも。 具体的には、アンチ エイリアス処理は、各タイルの内容をぼかします。 ただし、各タイルの端がぼかされません、これにより、隣接するタイルを使用して描画ではなく目立た個々 のタイル。 ピクセルのアートのゲームに維持するために、ピクセルの外観が保持する多くの場合、注意してくださいも、*レトロ*感じです。

設定`Antialiased`に`false`構築した後、 `tileMap`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();

    tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

    // new code:
    tileMap.Antialiased = false;

    this.AddChild (tileMap);
} 
```

今すぐ、タイルのマップは表示されませんがぼやけて。

![](tiled-images/image8.png "これで、タイルのマップは表示されませんがぼやけて")


## <a name="using-tile-properties-at-runtime"></a>実行時にタイルのプロパティを使用します。

これまでにある、 `CCTileMap` .tmx ファイルの読み込みおよび表示するには、対話する方法はありません。 具体的には、特定のタイル (など、宝箱) は、カスタム ロジックが必要があります。 カスタム タイルのプロパティ、および実行時に 1 回識別されるこれらのプロパティに対応するためのさまざまな方法を検出する方法を説明します。

すべてのコードを記述前にプロパティを並べて表示から、タイルのマップに追加する必要になります。 これを行うには、並べて表示プログラムで dungeon.tmx ファイルを開きます。 必ず、ゲームのプロジェクトで使用されているファイルを開きます。

一度開いて、設定プロパティを表示するタイルで宝箱を選択します。

![](tiled-images/image9.png "一度開いて、設定プロパティを表示するタイルで宝箱を選択します。")

宝箱を右クリックし、選択宝物箱プロパティが表示されない場合**タイル プロパティ**:

![](tiled-images/image10.png "宝物箱プロパティが表示されない場合宝箱を右クリックし、タイルのプロパティを選択します。")

タイル化されたプロパティは、名前と値が実装されます。 プロパティを追加するをクリックして、 **+** ボタン名を入力します**IsTreasure**、をクリックして**OK**、値を入力**true**: 

![](tiled-images/image11.png "プロパティを追加するには、ボタンをクリックして IsTreasure の名前を入力、[ok] をクリックし、入力値の true")

忘れずにプロパティを変更した後、.tmx ファイルを保存します。

最後に、コードを新しく追加されたプロパティを探して追加します。 各ループは`CCTileMapLayer`(、マップには 2 層)、各行および列にある任意のタイルの表示を使用し、`IsTreasure`プロパティ。


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

        // new code:
        tileMap.Antialiased = false;

        this.AddChild (tileMap);

        HandleCustomTileProperties (tileMap);
    }

    void HandleCustomTileProperties(CCTileMap tileMap)
    {
        // Width and Height are equal so we can use either
        int tileDimension = (int)tileMap.TileTexelSize.Width;

        // Find out how many rows and columns are in our tile map
        int numberOfColumns = (int)tileMap.MapDimensions.Size.Width;
        int numberOfRows = (int)tileMap.MapDimensions.Size.Height;

        // Tile maps can have multiple layers, so let's loop through all of them:
        foreach (CCTileMapLayer layer in tileMap.TileLayersContainer.Children)
        {
            // Loop through the columns and rows to find all tiles
            for (int column = 0; column < numberOfColumns; column++)
            {
                // We're going to add tileDimension / 2 to get the position
                // of the center of the tile - this will help us in 
                // positioning entities, and will eliminate the possibility
                // of floating point error when calculating the nearest tile:
                int worldX = tileDimension * column + tileDimension / 2;
                for (int row = 0; row < numberOfRows; row++)
                {
                    // See above on why we add tileDimension / 2
                    int worldY = tileDimension * row + tileDimension / 2;

                    HandleCustomTilePropertyAt (worldX, worldY, layer);
                }
            }
        }
    }

    void HandleCustomTilePropertyAt(int worldX, int worldY, CCTileMapLayer layer)
    {
        CCTileMapCoordinates tileAtXy = layer.ClosestTileCoordAtNodePosition (new CCPoint (worldX, worldY));

        CCTileGidAndFlags info = layer.TileGIDAndFlags (tileAtXy.Column, tileAtXy.Row);

        if (info != null)
        {
            Dictionary<string, string> properties = null;

            try
            {
                properties = tileMap.TilePropertiesForGID (info.Gid);
            }
            catch
            {
                // CocosSharp 
            }

            if (properties != null && properties.ContainsKey ("IsTreasure") && properties["IsTreasure"] == "true" )
            {
                layer.RemoveTile (tileAtXy);

                // todo: Create a treasure chest entity
            }
        }
    }
} 
```

コードのほとんどは一目瞭然ですが宝物タイルの処理について説明する必要があります。 ここで宝物整理として識別されるタイルを削除する予定です。 宝物整理効果の衝突して、プレーヤーの特典を開いたときに宝の内容を実行時にカスタム コードで必要になるためにです。 中に対応するため、宝物がさらに、必要があります (その視覚的な外観を変更する) を開くし、敵が負けましたすべて画面に表示されるときに表示される専用のロジックがある可能性があります。

宝箱がされるで単純なタイルではなく、エンティティの中から得られる、つまり、`CCTileMap`します。 ゲームのエンティティの詳細については、次を参照してください。、 [CocosSharp のエンティティのガイド](~/graphics-games/cocossharp/entities.md)します。


## <a name="summary"></a>まとめ

このチュートリアルでは、CocosSharp アプリケーションに並べて表示によって作成された .tmx ファイルを読み込む方法について説明します。 ピクセルの解像度の低いアートに対応するアプリの解像度を変更する方法とそれらのプロパティをエンティティ インスタンスの作成などのカスタム ロジックを実行してタイルを見つける方法を示しています。

## <a name="related-links"></a>関連リンク

- [タイル化された web サイト](http://www.mapeditor.org/)
- [コンテンツの zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true)
