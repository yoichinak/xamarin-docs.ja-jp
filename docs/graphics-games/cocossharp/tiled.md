---
title: "使用して CocosSharp を並べて表示"
description: "並べて表示は強力で柔軟なされ、ゲームの直交と等角 タイルを作成するための完成度の高いアプリケーションがマップされます。 CocosSharp は、並べてのネイティブのファイル形式の組み込みの統合を提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 804C042C-F62A-4E6C-B10F-06528637F0E2
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 5a469a372a9299712be7aef46c51f3d644946535
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="using-tiled-with-cocossharp"></a>使用して CocosSharp を並べて表示

_並べて表示は強力で柔軟なされ、ゲームの直交と等角 タイルを作成するための完成度の高いアプリケーションがマップされます。CocosSharp は、並べてのネイティブのファイル形式の組み込みの統合を提供します。_

*並べて*アプリケーションを作成するための標準*マップのタイル*ゲーム開発で使用するためです。 このガイドは、既存の .tmx ファイル (並べてによって作成されたファイル) を行い、CocosSharp ゲームで使用する方法を説明します。 具体的には、このガイドを取り上げます。

 - マップのタイルの目的は、
 - .Tmx ファイルの操作
 - ピクセルの画像を表示するための考慮事項
 - 実行時にタイルのプロパティを使用します。

ときに終了が次のデモ。

![](tiled-images/image1.png "このガイドの手順に従って作成され、デモ アプリ")


# <a name="the-purpose-of-tile-maps"></a>マップのタイルの目的は、

マップのタイル 10 年間、ゲームの開発に存在していたけれども、引き続き一般的に使用され 2D ゲームで、効率と esthetics タイルのマップは非常に高レベルのマップのタイルで使用されるソース イメージ タイル セットの使用により、効率を実現することです。 タイルのセットは、1 つのファイルに結合するイメージのコレクションです。 タイルのセットは、マップのタイルで使用されるイメージを参照してください、複数の小さい画像が含まれているファイルとも呼ばれますスプライト シートまたはスプライトがゲームの開発にマップします。 このデモで使用するタイルのセットに、グリッドの追加によるタイルのセットを使用する方法を視覚化できます。

![](tiled-images/image2.png "デモで使用されるタイルのセットをグリッドの追加によるタイルのセットを使用する方法の視覚化されたビュー")

タイルのマップ タイルのセットから個々 のタイルを配置します。 各タイルのマップが必要はありません、タイルの独自のコピーを格納することに注意する必要があります – 設定ではなく、タイルに複数のマップできる、同じセットを参照タイルです。 つまり、タイルのセットとは別のマップのタイルが必要メモリがほとんどです。 これにより、多数のタイルのマップの作成など、大規模なゲーム プレイ領域の作成に使用されている場合でも、 [platformer をスクロール](http://en.wikipedia.org/wiki/Platform_game)環境。 同じタイルのセットを使用可能な並べ替え方法を次に示します。

![](tiled-images/image3.png "このイメージは、同じタイルのセットを使用可能な並べ替え方法を示しています。")

![](tiled-images/image4.png "このイメージは、同じタイルのセットを使用可能な並べ替え方法を示しています。")


# <a name="working-with-tmx-files"></a>.Tmx ファイルの操作

.Tmx ファイル形式は、並べて表示のアプリケーションで作成した XML ファイル[並べて web サイトを無料でダウンロード](http://www.mapeditor.org/)です。 .Tmx ファイル形式は、マップのタイル情報を格納します。 通常、ゲームでは、それぞれのレベルまたは独立した領域に 1 つの .tmx ファイルがあります。

このガイドで CocosSharp; 内の既存の .tmx ファイルを使用する方法の説明します。ただし、追加のチュートリアルを参照して、含む[並べてマップ エディターのこの概要](http://gamedevelopment.tutsplus.com/tutorials/introduction-to-tiled-map-editor--gamedev-2838)です。

解凍する必要があります、[コンテンツ zip ファイル](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true)ゲームで使用します。 最初の注目するはタイル マップ ファイルを使用して両方 .tmx (dungeon.tmx) 1 つだけでなく、またはタイルを定義する複数のイメージ ファイル (dungeon_1.png) のデータを設定することです。 これらのファイルを読み込み、実行時に .tmx の両方を含めるにゲームが必要ですので、プロジェクトの両方追加**コンテンツ**フォルダー (に含まれている、**資産**Android プロジェクトのフォルダー)。 具体的には、という名前のフォルダーにファイルを追加**tilemaps**内、**コンテンツ**フォルダー。

![](tiled-images/image5.png "コンテンツのフォルダー内に tilemaps をという名前のフォルダーにファイルを追加します。")

**Dungeon.tmx**に実行時に、ファイルを読み込むようになりましたことができます、`CCTileMap`オブジェクト。 次に、変更、 `GameLayer` (または同等のルート コンテナー オブジェクト) を含む、`CCTileMap`インスタンスと、子として追加します。


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");
        this.AddChild (tileMap);
    }
} 
```

表示されます、ゲームを実行し、画面の左下隅にタイルのマップが表示されます。

![](tiled-images/image6.png "ゲームを実行すると場合、タイル マップ画面の左下隅に表示します。")


# <a name="considerations-for-rendering-pixel-art"></a>ピクセルの画像を表示するための考慮事項

ピクセル アート、ビデオ ゲーム開発のコンテキストでは、手動、通常は作成され、低解像度は、多くの場合、2 D visual アートを指します。 ピクセル アートを特定することができますを作成するため、ピクセル アート タイルのセットが 16 または 32 ピクセル幅と高さなどの低解像度のタイルが含まれる場合に処理を要する時間。 実行時に縮小しない、ピクセル アートが多くの場合、最近のほとんどの携帯電話とタブレットには小さすぎます。

表示されているディメンションへの呼び出しを追加して、ゲームの GameAppDelegate.cs ファイルで調整できます`CCScene.SetDefaultDesignResolution`:


```csharp
 public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");

    CCSize windowSize = mainWindow.WindowSizeInPixels;

    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to be (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDefaultDesignResolution (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    
    // Determine whether to use the high or low def versions of our images
    // Make sure the default texel to content size ratio is set correctly
    // Of course you're free to have a finer set of image resolutions e.g (ld, hd, super-hd)
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }

    // New code:
    CCScene.SetDefaultDesignResolution (380, 240, CCSceneResolutionPolicy.ShowAll);

    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);

    mainWindow.RunWithScene (scene);
} 
```

詳細については`CCSceneResolutionPolicy`、についてこのガイドを参照してください[CocosSharp の解像度を処理](~/graphics-games/cocossharp/resolutions.md)です。

ゲームを今すぐ実行して、デバイスの全画面表示をゲーム take おことがわかります。

![](tiled-images/image7.png "デバイスの全画面表示をゲームの実行")

最後に、タイルのマップのアンチエイリアシングを無効にするされます。 `Antialiased`にズーム インするオブジェクトを表示するときに、プロパティがブラー効果を適用します。 アンチ エイリアスでは、グラフィカル オブジェクトは、のピクセルの外観を削減するのに便利ですできますが、独自のレンダリング成果物を導入も可能性があります。 具体的には、(アンチエイリアシング) は、各タイルの内容をぼかすです。 ただし、各タイルの端をいないぼかす、されるため、個々 のタイルが隣接するタイルを使用して描画するのではなくが目立つようにします。 ピクセル アートのゲームに維持するために、ピクセルの外観が保持する多くの場合、注意してください。 また、*レトロ調*と思われます。

設定`Antialiased`に`false`構築した後、 `tileMap`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();

    tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

    // new code:
    tileMap.Antialiased = false;

    this.AddChild (tileMap);
} 
```

これで、タイルのマップは表示されませんがぼやけて。

![](tiled-images/image8.png "これで、タイルのマップは表示されませんがぼやけて")


# <a name="using-tile-properties-at-runtime"></a>実行時にタイルのプロパティを使用します。

これまで、 `CCTileMap` .tmx ファイルの読み込みと表示がサービスと対話する方法はありません。 具体的には、特定のタイル (など、宝箱) は、カスタム ロジックを用意する必要があります。 おにカスタム タイルのプロパティ、および実行時に 1 回識別されるこれらのプロパティに対応するためにさまざまな方法を検出する方法について説明します。

すべてのコードを記述して前に、プロパティ マップを追加する、並べて表示並べて表示で必要になります。 これを行うには、並べて表示プログラムで dungeon.tmx ファイルを開きます。 必ず、game プロジェクトで使用されているファイルを開きます。

されたら、[開く] タイルで、そのプロパティを表示する設定宝箱を選択します。

![](tiled-images/image9.png "開いている場合は、選択宝箱 タイルで、そのプロパティを表示する設定")

宝箱を右クリックし、選択宝物箱プロパティが表示されない場合**タイル プロパティ**:

![](tiled-images/image10.png "宝箱を右クリックし、タイルのプロパティを選択して宝物箱プロパティが表示されない場合")

並べて表示されるプロパティは、名前と値で実装されます。 プロパティを追加するをクリックして、  **+** ボタン名を入力します**IsTreasure**、をクリックして**OK**、値を入力**true**: 

![](tiled-images/image11.png "プロパティを追加するには、ボタンをクリックして IsTreasure の名前を入力、[ok] をクリックし、入力値の true")

プロパティを変更した後、.tmx ファイルを保存してください。

最後に、新しく追加されたプロパティを検索するコードを追加します。 各ループが`CCTileMapLayer`(、マップを持つ 2 つのレイヤー) 各の行と列を持つタイルはすべての検索に使用し、`IsTreasure`プロパティ。


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

        // new code:
        tileMap.Antialiased = false;

        this.AddChild (tileMap);

        HandleCustomTileProperties (tileMap);
    }

    void HandleCustomTileProperties(CCTileMap tileMap)
    {
        // Width and Height are equal so we can use either
        int tileDimension = (int)tileMap.TileTexelSize.Width;

        // Find out how many rows and columns are in our tile map
        int numberOfColumns = (int)tileMap.MapDimensions.Size.Width;
        int numberOfRows = (int)tileMap.MapDimensions.Size.Height;

        // Tile maps can have multiple layers, so let's loop through all of them:
        foreach (CCTileMapLayer layer in tileMap.TileLayersContainer.Children)
        {
            // Loop through the columns and rows to find all tiles
            for (int column = 0; column < numberOfColumns; column++)
            {
                // We're going to add tileDimension / 2 to get the position
                // of the center of the tile - this will help us in 
                // positioning entities, and will eliminate the possibility
                // of floating point error when calculating the nearest tile:
                int worldX = tileDimension * column + tileDimension / 2;
                for (int row = 0; row < numberOfRows; row++)
                {
                    // See above on why we add tileDimension / 2
                    int worldY = tileDimension * row + tileDimension / 2;

                    HandleCustomTilePropertyAt (worldX, worldY, layer);
                }
            }
        }
    }

    void HandleCustomTilePropertyAt(int worldX, int worldY, CCTileMapLayer layer)
    {
        CCTileMapCoordinates tileAtXy = layer.ClosestTileCoordAtNodePosition (new CCPoint (worldX, worldY));

        CCTileGidAndFlags info = layer.TileGIDAndFlags (tileAtXy.Column, tileAtXy.Row);

        if (info != null)
        {
            Dictionary<string, string> properties = null;

            try
            {
                properties = tileMap.TilePropertiesForGID (info.Gid);
            }
            catch
            {
                // CocosSharp 
            }

            if (properties != null && properties.ContainsKey ("IsTreasure") && properties["IsTreasure"] == "true" )
            {
                layer.RemoveTile (tileAtXy);

                // todo: Create a treasure chest entity
            }
        }
    }
} 
```

コードのほとんどは自明ですが、宝物タイルの処理について説明する必要があります。 ここでは宝物整理として識別され、タイルを削除する予定です。 これはため宝物整理効果競合とプレーヤーに開かれたときに宝物の内容を獲得する実行時にカスタム コードを必要があります。 さらに、宝物になるように対処する必要があります (そのビジュアルの外観を変更する) を開くし、敵を無効化されているすべて画面に表示されるときに表示されるだけのロジックがあります。

つまり、宝箱を活用できる単純なタイルにされているのではなく、エンティティをされている、`CCTileMap`です。 ゲームのエンティティの詳細については、次を参照してください。、 [CocosSharp 内のエンティティのガイド](~/graphics-games/cocossharp/entities.md)です。


# <a name="summary"></a>まとめ

このチュートリアルでは、CocosSharp アプリケーションに並べて表示によって作成された .tmx ファイルを読み込む方法について説明します。 低解像度ピクセル アートのためにアプリの解像度を変更する方法、およびエンティティのインスタンスを作成するように、カスタム ロジックを実行するには、そのプロパティでタイルを検索する方法を示します。

## <a name="related-links"></a>関連リンク

- [タイル化された web サイト](http://www.mapeditor.org/)
- [Zip コンテンツ](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true)
