---
title: "コイン時の実装の詳細"
description: "このガイドでは、ゲームでは、コイン時間、マップのタイルの操作、エンティティを作成する、スプライト、アニメーション化する、効率的な競合を実装するなどの実装の詳細について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5D285684-0417-4E16-BD14-2D1F6DEFBB8B
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: b3827d05ae9e563ae04dd4ab1e303577f6c9d82a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="coin-time-implementation-details"></a>コイン時の実装の詳細

_このガイドでは、ゲームでは、コイン時間、マップのタイルの操作、エンティティを作成する、スプライト、アニメーション化する、効率的な競合を実装するなどの実装の詳細について説明します。_

コインは iOS および Android 用ゲーム完全 platformer です。 ゲームの目的は、コイン レベル内のすべてを収集し、敵および障害物を回避しながら、終了ドアに到達してです。

![](cointime-images/image1.png "ゲームの目的をコイン、レベル内のすべてを収集し、敵および障害物を回避しながら、終了ドアに到達して")

このガイドでは、コイン時間、次のトピックをカバーする実装の詳細について説明します。

- [TMX ファイルの操作](#Working_with_TMX_Files)
- [レベルの読み込み](#Level_Loading)
- [新しいエンティティを追加します。](#Adding_New_Entities)
- [アニメーション化されたエンティティ](#Animated_Entities)


# <a name="content-in-coin-time"></a>コイン時間でコンテンツ

コイン時間は、サンプル プロジェクトを完全な CocosSharp プロジェクトの構成方法を表すです。 コイン時刻の加算とコンテンツの保守を簡単に目的の構造体します。 使用して**.tmx**によって作成されるファイル[並べて](http://www.mapeditor.org)レベルとアニメーションを定義する XML ファイルです。 変更や新しいコンテンツの追加は、最小限の労力で実現できます。 

プロフェッショナルなどのゲームを反映してもこの方法によりコイン時間の学習、実験の効果的なプロジェクト、中に行われました。 このガイドでは、いくつかの追加および変更内容を簡単に実行される方法について説明します。


# <a name="working-with-tmx-files"></a>TMX ファイルの操作

コイン時間レベルがによって出力された .tmx ファイル形式を使用して定義されている、[並べて](http://www.mapeditor.org)タイル マップ エディターです。 並べて表示の操作の詳細については、次を参照してください。、 [Cocos シャープ ガイドに並べて表示を使用して](~/graphics-games/cocossharp/tiled.md)です。 

各レベルに含まれている、独自の .tmx ファイルで定義されている、 **CoinTime/資産/コンテンツ/レベル**フォルダーです。 コイン時間のすべてのレベルで定義されている 1 つのタイルセット ファイルの共有、 **mastersheet.tsx**ファイル。 このファイルは、タイルが純色の競合を持つかどうかや、エンティティのインスタンスで、タイルを置き換える必要があるかどうかなど、各タイルのカスタム プロパティを定義します。 Mastersheet.tsx ファイルは、1 回だけ定義し、すべてのレベルで使用するプロパティを使用します。 


## <a name="editing-a-tile-map"></a>タイルのマップの編集

タイルのマップを編集するには、.tmx ファイルをダブルクリックするか、並べて表示の [ファイル] メニューから開くことによって並べて表示で .tmx ファイルを開きます。 レベル タイル マップは、次の 3 つのレイヤーを含めるコイン時間: 

- **エンティティ**– このレイヤーには実行時にエンティティのインスタンスに置き換え タイルが含まれています。 プレーヤー、コイン、敵、および終了のレベルのドアをなどです。
- **地形**– このレイヤーには、通常実線の競合を持つタイルが含まれています。 純色の競合は、フォール スルーすることがなくこれらのタイルのすべての要素を player を使用できます。 実線の競合を含むタイル壁、シーリングとして動作することもできます。
- **バック グラウンド**– 背景レイヤーには、静的な背景として使用するタイルが含まれています。 このレイヤーは、カメラが視差経由の深さの外観を作成する、レベルで移動したときにスクロールされません。

後で、学習しましょうはレベル読み込みコードにコイン時間のすべてのレベルでこれらの 3 つのレイヤーが必要です。

### <a name="editing-terrain"></a>地形を編集
クリックしてタイルを配置することができます、 **mastersheet**マップ タイルセットとし、タイルをクリックします。 たとえば、レベル内の新しい地形を描画するには、します。

1. 地形レイヤーを選択します。
1. 描画するタイルをクリックします。
1. をクリックしてまたはプッシュし、タイルを描画するマップ上にドラッグ


    ![](cointime-images/image2.png "1 を描画するタイルをクリックします。")

 

タイルセットの左上には、地形コイン時間内のすべてが含まれます。 純色は、地形が含まれています、 **SolidCollision**プロパティ、画面の左側のタイルのプロパティで示すようにします。

![](cointime-images/image3.png "地形、純色は、画面の左側のタイルのプロパティのように SolidCollision プロパティが含まれます")

### <a name="editing-entities"></a>エンティティの編集
エンティティを追加、地形と同じように – レベルから削除したりすることができます。 **Mastersheet**タイルセットがないできるように表示される右にスクロールしなくても、配置の途中で水平方向に、すべてのエンティティがします。

![](cointime-images/image4.png "いないできるように表示される右にスクロールしなくても、mastersheet タイルセットが配置の途中で水平方向に、すべてのエンティティ")

新しいエンティティを配置する必要があります、**エンティティ**レイヤー。

![](cointime-images/image5.png "新しいエンティティをエンティティ レイヤーに配置する必要があります。")

CoinTime コードは検索、 **EntityType**エンティティによって置き換える必要があるタイルを識別する、レベルが読み込まれるときに。 

![](cointime-images/image6.png "エンティティで置き換える必要があるタイルを識別する、レベルが読み込まれるときに、CoinTime コードが EntityType を探します")

ファイルを変更して保存した後、変更が自動的に表示プロジェクトがビルドされ、実行かどうか。

![](cointime-images/image7.png "ファイルを変更して保存した後、変更が自動的に表示プロジェクトがビルドされ、実行")


## <a name="adding-new-levels"></a>新しいレベルを追加します。

コイン時間にレベルを追加するプロセスは、コードを変更しないでとのみ少しだけ変更をプロジェクトに必要です。 新しいレベルを追加します。

1. 開いているレベルのフォルダーにある <**CoinTime ルート > \CoinTime\Assets\Content\levels**
1. コピーして、レベルのいずれかのように貼り付ける**level0.tmx**
1. ガベージ コレクターは、レベル番号シーケンスが、既存のレベルなど、新しい .tmx ファイルの名前を変更**level8.tmx**
1. Visual Studio または Visual Studio for Mac、Android レベル フォルダーに新しい .tmx ファイルを追加します。 ファイルで使用されることを確認、 **AndroidAsset**ビルド アクション。


    ![](cointime-images/image8.png "ファイルが AndroidAsset ビルド アクションを使用することを確認してください。")


1. レベルの iOS フォルダーに新しい .tmx ファイルを追加します。 元の場所からファイルをリンクし、使用していることを確認してください、 **BundleResource**ビルド アクション。


    ![](cointime-images/image9.png "元の場所からファイルをリンクし、BundleResource ビルド アクションを使用していることを確認してください。")


新しいレベルはレベル 9 としてレベル選択画面に表示する (ファイル名のレベルは、0 から始まりますが、レベルのボタンが 1 から開始)。

![](cointime-images/image10.png "新しいレベルはレベル 9 レベルのファイル名の最初の 0 からとしてレベル選択画面で表示するが、レベルのボタンが 1 で始まる")


# <a name="level-loading"></a>レベルの読み込み

前述のように、新しいレベルにコードの変更が必要としない – ゲームは、正しくという名前に追加されは自動的にレベルを検出、**レベル**適切なビルド アクションを持つフォルダー (**BundleResource**または**AndroidAsset**)。

内のレベル数を決定するためのロジックが含まれている、`LevelManager`クラスです。 インスタンス、 `LevelManager` (シングルトン パターンを使用する) を構築、`DetermineAvailbleLevels`メソッドが呼び出されます。


```csharp
private void DetermineAvailableLevels()
{
    // This game relies on levels being named "levelx.tmx" where x is an integer beginning with
    // 1. We have to rely on MonoGame's TitleContainer which doesn't give us a GetFiles method - we simply
    // have to check if a file exists, and if we get an exception on the call then we know the file doesn't
    // exist. 
    NumberOfLevels = 0;
    while (true)
    {
        bool fileExists = false;
        try
        {
            using(var stream = TitleContainer.OpenStream("Content/levels/level" + NumberOfLevels + ".tmx"))
            {
            }
            // if we got here then the file exists!
            fileExists = true;
        }
        catch
        {
            // do nothing, fileExists will remain false
        }
        if (!fileExists)
        {
            break;
        }
        else
        {
            NumberOfLevels++;
        }
    }
}
```

依存する必要がありますファイルが存在する場合を検出するため、クロスプラット フォームのアプローチを行いません CocosSharp、`TitleContainer`ストリームを開こうとするクラス。 ストリームを開くためのコードは、例外、そのファイルがスローされた場合は存在しないため、while ループを中断します。 ループが終了した、`NumberOfLevels`プロパティは、プロジェクトの一部である有効なレベルの数を報告します。

`LevelSelectScene`クラスの使用、`LevelManager.NumberOfLevels`で作成するボタンの数を決定する、`CreateLevelButtons`メソッド。


```csharp
private void CreateLevelButtons()
{
    const int buttonsPerPage = 6;
    int levelIndex0Based = buttonsPerPage * pageNumber;
    int maxLevelExclusive = System.Math.Min (levelIndex0Based + 6, LevelManager.Self.NumberOfLevels);
    int buttonIndex = 0;
    float centerX = this.ContentSize.Center.X;
    const float topRowOffsetFromCenter = 16;
    float topRowY = this.ContentSize.Center.Y + topRowOffsetFromCenter;
    for (int i = levelIndex0Based; i < maxLevelExclusive; i++)
    {
        ...
    }
}
```

`NumberOflevels`ボタンを作成するかを決定するプロパティを使用します。 このコードでは、ユーザーが現在表示して、1 ページあたりの 6 つのボタンの最大値を作成するだけのどのページと見なします。 ボタン インスタンスの呼び出しをクリックすると、`HandleButtonClicked`メソッド。


```csharp
private void HandleButtonClicked(object sender, EventArgs args)
{
    // levelNumber is 1-based, so subtract 1:
    var levelIndex = (sender as Button).LevelNumber - 1;
    LevelManager.Self.CurrentLevel = levelIndex;
    CoinTime.GameAppDelegate.GoToGameScene ();
}
```

このメソッドは割り当てます、`CurrentLevel`によって使用されるプロパティ、`GameScene`レベルを読み込むときにします。 設定した後、 `CurrentLevel`、`GoToGameScene`メソッドは、切り替えのシーン`LevelSelectScene`に`GameScene`です。

`GameScene`コンス トラクター呼び出し`GoToLevel`レベルの読み込みのロジックを実行します。


```csharp
private void GoToLevel(int levelNumber)
{
    LoadLevel (levelNumber);
    CreateCollision();
    ProcessTileProperties ();
    touchScreen = new TouchScreenInput(gameplayLayer);
    secondsLeft = secondsPerLevel;
}
```

呼び出されるメソッドを見てみましょう次`GoToLevel`です。


## <a name="loadlevel"></a>LoadLevel

`LoadLevel`メソッドは .tmx ファイルの読み込みとに追加することを担当、`GameScene`です。 このメソッドは、衝突やエンティティなど、対話型のオブジェクトを作成しません – とも呼びますレベルのビジュアルをだけを作成、*環境*です。


```csharp
private void LoadLevel(int levelNumber)
{
    currentLevel = new CCTileMap ("level" + levelNumber + ".tmx");
    currentLevel.Antialiased = false;
    backgroundLayer = currentLevel.LayerNamed ("Background");
    // CCTileMap is a CCLayer, so we'll just add it under all entities
    this.AddChild (currentLevel);
    // put the game layer after
    this.RemoveChild(gameplayLayer);
    this.AddChild(gameplayLayer);
    this.RemoveChild (hudLayer);
    this.AddChild (hudLayer);
}
```

`CCTileMap`コンス トラクターに渡されたレベル番号を使用して作成されるファイル名、`LoadLevel`です。 作成と操作について`CCTileMap`インスタンスを参照してください、 [CocosSharp ガイドに並べて表示を使用して](~/graphics-games/cocossharp/tiled.md)です。

現在、CocosSharp できませんを削除して、親に再追加することがなくレイヤーの順序が変更`CCScene`(これは、`GameScene`ここでは)、レイヤーの順序を変更するメソッドの最後の数行が必要なため、します。


## <a name="createcollision"></a>CreateCollision

`CreateCollision`メソッド コンストラクト、`LevelCollision`実行に使用されるインスタンス*ソリッド衝突*プレーヤーと環境の間です。


```csharp
private void CreateCollision()
{
    levelCollision = new LevelCollision();
    levelCollision.PopulateFrom(currentLevel);
}
```

この衝突ないレベルから該当プレーヤーとゲームは再生できません。 純色の衝突を使用して、地上ウォーク プレーヤーをでき、壁にたどるまたはシーリングからのジャンプを有効にします。

コイン時間での競合は、追加のコードを持たない – タイル化されたファイルへの変更のみ追加できます。 


## <a name="processtileproperties"></a>ProcessTileProperties

レベルがロードされ、競合が作成されると、`ProcessTileProperties`タイルのプロパティに基づくロジックを実行すると呼びます。 コイン時間が含まれています、`PropertyLocation`プロパティと、これらのプロパティ タイルの座標を定義するための構造体。


```csharp
public struct PropertyLocation
{
    public CCTileMapLayer Layer;
    public CCTileMapCoordinates TileCoordinates;
    public float WorldX;
    public float WorldY;
    public Dictionary<string, string> Properties;
}
```

この構造体が構築するために使用されるエンティティのインスタンスを作成し、内の不要なタイルの削除、`ProcessTileProperties`メソッド。


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```

Foreach ループの評価チェック キーがいずれかである場合、各タイル プロパティ`EntityType`または`RemoveMe`です。 `EntityType` エンティティのインスタンスを作成する必要があることを示します。 `RemoveMe` タイルが実行時に完全に削除されたことを示します。

場合のプロパティ、 `EntityType` 、し、キーが見つかった`TryCreateEntity`が呼び出されると、一致するプロパティを使用してエンティティを作成しようとしている、`EntityType`キー。


```csharp
private bool TryCreateEntity(string entityType, float worldX, float worldY)
{
    CCNode entityAsNode = null;
    switch (entityType)
    {
    case "Player":
        player = new Player ();
        entityAsNode = player;
        break;
    case "Coin":
        Coin coin = new Coin ();
        entityAsNode = coin;
        coins.Add (coin);
        break;
    case "Door":
        door = new Door ();
        entityAsNode = door;
        break;
    case "Spikes":
        var spikes = new Spikes ();
        this.damageDealers.Add (spikes);
        entityAsNode = spikes;
        break;
    case "Enemy":
        var enemy = new Enemy ();
        this.damageDealers.Add (enemy);
        this.enemies.Add (enemy);
        entityAsNode = enemy;
        break;
    }
    if(entityAsNode != null)
    {
        entityAsNode.PositionX = worldX;
        entityAsNode.PositionY = worldY;
        gameplayLayer.AddChild (entityAsNode);
    }
    return entityAsNode != null;
}
```


# <a name="adding-new-entities"></a>新しいエンティティを追加します。

コイン時間、ゲームのオブジェクトに対してエンティティ パターンを使用して (これは、「、 [CocosSharp 内エンティティのガイド](~/graphics-games/cocossharp/entities.md)). すべてのエンティティを継承`CCNode`の子として追加することを意味する、`gameplayLayer`です。

各エンティティの種類はリストまたは 1 つのインスタンスから直接も参照されます。 たとえば、`Player`によって参照される、`player`フィールド、およびすべて`Coin`でインスタンスが参照されている、 `coins`  ボックスの一覧です。 エンティティへの直接参照を保持 (経由に参照するのではなく、`gameLayer.Children`リスト) を読みやすくするこれらのエンティティにアクセスし、可能性のある高価な型キャストを排除するコードは、します。

既存のコードでは、エンティティ型の例として、新しいエンティティを作成する方法の数を提供します。 次の手順は、新しいエンティティの作成に使用できます。


## <a name="1---define-a-new-class-using-the-entity-pattern"></a>1 - エンティティ パターンを使用して新しいクラスを定義します。

継承されるクラスを作成するエンティティを作成するための唯一の要件は、`CCNode`です。 ほとんどのエンティティがいくつかのビジュアルをなどがある、 `CCSprite`、そのコンス トラクター内のエンティティの子として追加する必要があります。

CoinTime 提供、`AnimatedSpriteEntity`アニメーション化されたエンティティの作成を単純化するクラス。 詳しく取り上げますアニメーション、[エンティティのアニメーション セクションで](#Animated_Entities)です。


## <a name="2--add-a-new-entry-to-the-trycreateentity-switch-statement"></a>2 – TryCreateEntity switch ステートメントに新しいエントリを追加します。

新しいエンティティのインスタンスをインスタンス化する必要があります、`TryCreateEntity`です。 エンティティの競合、AI、入力を読み取るなどのフレームごとのロジックが必要な場合、`GameScene`オブジェクトへの参照を保持する必要があります。 複数のインスタンスが必要な場合 (など`Coin`または`Enemy`インスタンス)、し、新しい`List`に追加する必要があります、`GameScene`クラスです。


## <a name="3--modify-tile-properties-for-the-new-entity"></a>3 – 新しいエンティティのタイルのプロパティを変更します。

コードでは、新しいエンティティの作成をサポートする、タイルセットに追加する新しいエンティティ必要があります。 任意のレベルを開くことによって、タイルセットを編集することができます`.tmx`ファイル。 

タイルセットがで .tmx 別に保存されて、 **mastersheet.tsx**ファイルを編集する前にインポートする必要があります。

![](cointime-images/image11.png "タイルセットは格納 .tsx ファイルから独立したので、編集する前にインポートする必要があります。")

インポートされると、選択したタイルのプロパティは、編集可能なと EntityType を追加することができます。

![](cointime-images/image12.png "インポートされると、選択したタイルのプロパティは、編集可能なと EntityType を追加することができます。")

新しい一致するように、その値を設定することができます、プロパティの作成後、`case`で`TryCreateEntity`:

![](cointime-images/image13.png "TryCreateEntity で、新しいケースに一致するように、その値を設定することができます、プロパティを作成した後")

タイルセットが変更された後は、エクスポートする必要があります-これにより、変更が他のすべてのレベルを使用できます。

![](cointime-images/image14.png "エクスポートする必要があります、タイルセットが変更された後")

既存のタイルセットを上書きする**mastersheet.tsx**タイルセット。

![](cointime-images/image15.png "彼はタイルセット既存 mastersheet.tsx タイルセットを上書きする必要があります。")


# <a name="entity-tile-removal"></a>エンティティのタイルの削除

ゲームにタイルのマップが読み込まれると、個々 のタイルは、静的オブジェクトです。 エンティティには、移動など、カスタム動作が必要があるために、コイン時のコードは、エンティティが作成されたときにタイルを削除します。

`ProcessTileProperties` 使用してエンティティを作成するタイルを削除するためのロジックが含まれています、`RemoveTile`メソッド。


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            ...
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        ...
    }
}
```

タイルの自動削除は、コイン敵など、タイルセットで 1 つだけのタイルを占有するエンティティに対して十分です。 大きなエンティティは、プロパティと追加のロジックが必要です。

ドアは、完全に描画される 2 つの牌が必要です。

![](cointime-images/image16.png "ドアは、完全に描画される 2 つの牌が必要です。")

ドアの下部にあるタイルには、エンティティを作成するためのプロパティが含まれています (**EntityType** 'éý'**ドア**)。

![](cointime-images/image17.png "ドアの下部にあるタイルには、EntityType にドア設定エンティティを作成するためのプロパティが含まれています")

ドアの下部にあるタイルのみが削除されるは、ドア インスタンスが作成されるとき、ので、実行時に、最上位のタイルを削除する追加のロジックが必要です。 最上位のタイルでは、 **RemoveMe**プロパティに設定**true**:

![](cointime-images/image18.png "最上位のタイルに設定された RemoveMe プロパティは true")



内のタイルを削除するこのプロパティは使用`ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        ...
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```


# <a name="entity-offsets"></a>エンティティのオフセット

タイルから作成されたエンティティは、タイルの中央にエンティティの中心を連携させることで配置されます。 などのより大きなエンティティは、`Door`を正しく配置する追加のプロパティとロジックを使用します。 

定義、下部にあるドア タイル、`Door`エンティティの配置を指定します、 **y オフセット**4 の値。 このプロパティは、なし、`Door`インスタンスは、タイルの中央に置かれます。

![](cointime-images/image19.png "このプロパティを持たないドア インスタンスは、タイルの中央に配置します。")

 

これは、適用することによって修正、 **y オフセット**値`ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            ...
        }
...
    }
}
```


# <a name="animated-entities"></a>アニメーション化されたエンティティ

コイン時間には、いくつかのアニメーション化されたエンティティが含まれます。 `Player`と`Enemy`エンティティ ウォーク アニメーションを再生して、`Door`すべてコインが収集された後に、エンティティが開くアニメーションを再生します。


## <a name="achx-files"></a>.achx ファイル

.Achx ファイルでは、コイン時間アニメーションが定義されています。 間で各アニメーションが定義されている`AnimationChain`タグで定義されている次のアニメーションのように**propanimations.achx**:


```xml
<AnimationChain>
  <Name>Spikes</Name>
  <ColorKey>0</ColorKey>
  <Frame>
    <FlipHorizontal>false</FlipHorizontal>
    <FlipVertical>false</FlipVertical>
    <TextureName>..\images\mastersheet.png</TextureName>
    <FrameLength>0.1</FrameLength>
    <LeftCoordinate>1152</LeftCoordinate>
    <RightCoordinate>1168</RightCoordinate>
    <TopCoordinate>128</TopCoordinate>
    <BottomCoordinate>144</BottomCoordinate>
    <RelativeX>0</RelativeX>
    <RelativeY>0</RelativeY>
  </Frame>
</AnimationChain> 
```

このアニメーションには、静的な画像を表示するスパイク エンティティの結果として得られる 1 つのフレームにはのみが含まれています。 エンティティは、1 つまたは複数のフレームのアニメーションを表示するかどうか、.achx ファイルを使用できます。 追加のフレームは、コード内でも変更せず .achx ファイルに追加できます。 

フレームに表示するイメージを定義する、`TextureName`パラメーター、および表示の座標、 `LeftCoordinate`、 `RightCoordinate`、 `TopCoordinate`、および`BottomCoordinate`タグ。 使用されているイメージでのアニメーションのフレームの座標を表すこれら**mastersheet.png**ここでします。

![](cointime-images/image20.png "これらは、イメージでのアニメーションのフレームの座標を表します")

`FrameLength`プロパティをアニメーションのフレームを表示する秒数を定義します。 単一フレームのアニメーションでは、この値を無視します。

.Achx ファイル内の他のすべての AnimationChain プロパティは、コイン時間によって無視されます。


## <a name="animatedspriteentity"></a>AnimatedSpriteEntity

アニメーション処理のロジックが含まれている、`AnimatedSpriteEntity`で使用されるほとんどのエンティティの基本クラスとして機能するクラス、`GameScene`です。 これには、次の機能が提供されます。

 - 読み込み`.achx`ファイル
 - 読み込まれたアニメーションのアニメーションのキャッシュ
 - アニメーションを表示するための CCSprite インスタンス
 - 現在のフレームを変更するためのロジック

スパイク コンス トラクターは、ロードしてアニメーションを使用する方法の簡単な例を提供します。


```csharp
public Spikes ()
{
    LoadAnimations ("Content/animations/propanimations.achx");
    CurrentAnimation = animations [0];
}
```

**PropAnimations.achx**コンス トラクターは、インデックスを使用してこのアニメーションにアクセスするため、1 つのアニメーションにはのみが含まれています。 .Achx ファイルには、複数のアニメーションが含まれるかどうかは、アニメーションは、のように、名前で参照できます、`Enemy`コンス トラクター。


```csharp
walkLeftAnimation = animations.Find (item => item.Name == "WalkLeft");
walkRightAnimation = animations.Find (item => item.Name == "WalkRight");
```


# <a name="summary"></a>まとめ

このガイドでは、コイン時間の実装の詳細について説明します。 コイン時間が完全版のゲームを作成しますが、も簡単に変更および拡張できるプロジェクトです。 リーダーは、新しいレベルを追加して、コイン時間を実装する方法をさらに理解する新しいエンティティを作成する時間の修正レベルを費やすことをお勧めします。

## <a name="related-links"></a>関連リンク

- [ゲーム プロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/CoinTime/)
