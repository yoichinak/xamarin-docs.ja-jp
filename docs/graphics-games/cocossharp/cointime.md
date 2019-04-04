---
title: Coin Time game の詳細
description: このガイドでは、マップのタイルの操作、エンティティの作成、スプライトをアニメーション化、効率的な衝突を実装するなど、コインにゲームの実装の詳細について説明します。
ms.prod: xamarin
ms.assetid: 5D285684-0417-4E16-BD14-2D1F6DEFBB8B
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: af914e10eb93aa0668743a113ffe647a939fea75
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112625"
---
# <a name="coin-time-game-details"></a>Coin Time game の詳細

_このガイドでは、マップのタイルの操作、エンティティの作成、スプライトをアニメーション化、効率的な衝突を実装するなど、コインにゲームの実装の詳細について説明します。_

コインは完全なプラットフォーム向けの iOS および Android 用のゲームです。 ゲームの目的をすべてのレベルで収集して敵や障害物を回避しながら出口のドアに到達してです。

![](cointime-images/image1.png "ゲームの目的はすべてのレベルで収集し、敵や障害物を回避しながら出口のドアを到達")

このガイドでは、実装の詳細を次のトピックをカバーする、コイン時間について説明します。

- [Tmx ファイルの使用](#working-with-tmx-files)
- [レベルの読み込み](#level-loading)
- [新しいエンティティを追加します。](#adding-new-entities)
- [アニメーション化されたエンティティ](#animated-entities)


## <a name="content-in-coin-time"></a>コインのコンテンツ

コインには、完全な CocosSharp プロジェクトの編成方法を表すサンプル プロジェクトです。 Coin Time の追加とコンテンツの保守を簡単に目的の構造体します。 使用して **.tmx**によって作成されたファイル[並べて](http://www.mapeditor.org)レベルおよびアニメーションを定義する XML ファイル。 変更または新しいコンテンツの追加は、最小限の労力で実現できます。 

どの本格的なゲームを反映してもこの方法によりコイン時間学習、実験の効果的なプロジェクトは、中には行われます。 このガイドを追加して、コンテンツの変更を簡単に実行する方法について説明します。


## <a name="working-with-tmx-files"></a>Tmx ファイルの使用

コイン時間レベルが、出力で .tmx ファイル形式を使用して定義されている、[並べて](http://www.mapeditor.org)タイル マップ エディター。 並べて表示の操作の詳細については、、 [Cocos シャープ ガイド」(並べて表示) を使用して](~/graphics-games/cocossharp/tiled.md)を参照してください。 

各レベルに含まれる、独自の .tmx ファイルで定義されている、 **CoinTime/資産/コンテンツ/レベル**フォルダー。 すべての硬貨時間レベルで定義されている 1 つのタイルセット ファイルの共有、 **mastersheet.tsx**ファイル。 このファイルは、タイルの solid の衝突がかどうかや、エンティティのインスタンスで、タイルを置き換える必要があるかどうかなど、各タイルのカスタム プロパティを定義します。 Mastersheet.tsx ファイルは、1 回だけ定義でき、すべてのレベルで使用されるプロパティを使用できます。 


### <a name="editing-a-tile-map"></a>タイルのマップの編集

タイルのマップを編集するには、.tmx ファイルをダブルクリックするか、並べて表示の [ファイル] メニューから開くことで並べて表示 .tmx ファイルを開きます。 Coin Time のレベルのタイルのマップには、3 つの層にはが含まれます。 

- **エンティティ**– このレイヤーには、実行時のエンティティのインスタンスに置き換えられるタイルが含まれています。 例には、プレイヤー、コイン、敵、および終了のレベルのドアが含まれます。
- **地形**– このレイヤーには、solid 衝突が通常のタイルが含まれています。 Solid の衝突、落下遅延することがなく、これらのタイルの方法について説明します。 Solid の衝突を含むタイルは、壁、シーリングとしても機能できます。
- **バック グラウンド**– 背景レイヤーには、静的な背景として使用されているタイルが含まれています。 視差効果の深さの外観を作成、レベル全体でカメラを移動すると、このレイヤーはスクロールされません。

後で調査は、レベル読み込みコードはこれら 3 つの層ですべての硬貨時間レベルが必要です。

#### <a name="editing-terrain"></a>地形の編集

タイルをクリックして配置できる、 **mastersheet**タイルセット タイルをクリックしてマップします。 たとえば、レベル内の新しい地形を描画します。

1. 地形のレイヤーを選択します。
1. 描画するためにタイルをクリックします
1. クリックしてまたはプッシュし、タイルを描画するマップ上にドラッグ

    ![](cointime-images/image2.png "1 を描画するためにタイルをクリックします")

タイルセットの左上には、コインの地形のすべてが含まれます。 地形が含まれています、 **SolidCollision**プロパティ、画面の左側のタイルのプロパティで示した。

![](cointime-images/image3.png "Solid は、地形、画面の左側のタイルのプロパティで示すように、SolidCollision プロパティが含まれます")

#### <a name="editing-entities"></a>エンティティの編集

エンティティを追加または地形と同じように – レベルから削除できます。 **Mastersheet**が表示されないあります右にスクロールすることがなく、タイルセットは配置の途中で水平方向にすべてのエンティティがあります。

![](cointime-images/image4.png "いないできるように表示される右にスクロールすることがなく、mastersheet タイルセットが配置の途中で水平方向にすべてのエンティティ")

新しいエンティティを配置する必要があります、**エンティティ**レイヤー。

![](cointime-images/image5.png "新しいエンティティをエンティティのレイヤーに配置する必要があります。")

CoinTime のコードは、 **EntityType**エンティティによって置き換える必要がありますのタイルを識別するために、レベルが読み込まれるときに。 

![](cointime-images/image6.png "エンティティで置き換える必要がありますのタイルを識別するために、レベルが読み込まれるときに、entitytype CoinTime コードは次")

ファイルを変更して保存すると、変更が表示されます、プロジェクトがビルドし、実行します。

![](cointime-images/image7.png "ファイルを変更して保存すると、変更が表示されるかどうか、プロジェクトのビルドし、実行")


### <a name="adding-new-levels"></a>新しいレベルを追加します。

コインにレベルを追加するプロセスでは、コードを変更せず、およびプロジェクトにいくつか小さな変更のみが必要です。 新しいレベルを追加します。

1. 上位のフォルダーにあるを開く <**CoinTime ルート > \CoinTime\Assets\Content\levels**
1. コピーして、レベルのいずれかなどに貼り付ける**level0.tmx**
1. 新しい .tmx ファイル名の変更など、既存のレベルを持つレベル番号のシーケンスを続けます**level8.tmx**
1. Visual Studio または Visual Studio for Mac では、Android レベル フォルダーに新しい .tmx ファイルを追加します。 ファイルで使用されることを確認、 **AndroidAsset**ビルド アクション。

    ![](cointime-images/image8.png "ファイルで、AndroidAsset ビルド アクションを使用することを確認します。")

1. IOS レベル フォルダーに新しい .tmx ファイルを追加します。 元の場所からファイルをリンクし、使用していることを確認してください、 **BundleResource**ビルド アクション。

    ![](cointime-images/image9.png "元の場所からファイルをリンクし、BundleResource ビルド アクションを使用していることを確認してください。")

新しいレベルはレベル 9 としてレベル選択画面に表示されます (ファイル名のレベルは、0 から始まるが、レベルのボタンを 1 から開始)。

![](cointime-images/image10.png "0 レベルのファイル名の最初のレベル 9 としてレベル選択画面に新しいレベルは表示する必要がありますが、レベルのボタンを 1 から開始")


## <a name="level-loading"></a>レベルの読み込み

前述のように、新しいレベルにコードの変更が必要としない – ゲームは、レベルを自動的に検出、という名前が正しくに追加された場合、**レベル**適切なビルド アクションを持つフォルダー (**BundleResource**または**AndroidAsset**)。

レベルの数を決定するためのロジックが含まれている、`LevelManager`クラス。 インスタンス、 `LevelManager` (シングルトン パターンを使用して) 構築される、`DetermineAvailbleLevels`メソッドが呼び出されます。


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

CocosSharp では、クロスプラット フォーム対応の方法に依存していますが、ファイルが存在する場合の検出を提供しません、`TitleContainer`ストリームを開こうとするクラス。 例外、ファイル ストリームを開くためのコードをスローした場合は存在しないと、while ループを中断します。 ループの完了後、`NumberOfLevels`プロパティは、プロジェクトの一部である有効なレベルの数を報告します。

`LevelSelectScene`クラスで使用、`LevelManager.NumberOfLevels`で作成するボタンの数を決定する、`CreateLevelButtons`メソッド。


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

`NumberOflevels`プロパティを使用して、決定ボタンを作成する必要があります。 このコードでは、ページ、ユーザーが現在表示していると、1 ページあたり 6 つのボタンの最大値を作成するだけが考慮されます。 クリックすると、ボタンのインスタンスの呼び出し、`HandleButtonClicked`メソッド。


```csharp
private void HandleButtonClicked(object sender, EventArgs args)
{
    // levelNumber is 1-based, so subtract 1:
    var levelIndex = (sender as Button).LevelNumber - 1;
    LevelManager.Self.CurrentLevel = levelIndex;
    CoinTime.GameAppDelegate.GoToGameScene ();
}
```

このメソッドを割り当てます、`CurrentLevel`プロパティで使用される、`GameScene`レベルの読み込み時にします。 設定した後、 `CurrentLevel`、`GoToGameScene`メソッドを発生するからシーンの切り替え`LevelSelectScene`に`GameScene`します。

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

呼び出されるメソッドを見てをみましょう次`GoToLevel`します。


### <a name="loadlevel"></a>LoadLevel

`LoadLevel`メソッドは .tmx ファイルの読み込みとに追加することを`GameScene`します。 このメソッドが衝突やエンティティなど、対話型のオブジェクトを作成できません-とも呼ばれます、レベルのビジュアルが作成されるだけ、*環境*します。


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

`CCTileMap`コンス トラクターに渡されたレベル番号を使用して作成されるファイル名を受け取る`LoadLevel`します。 作成と操作の詳細については`CCTileMap`インスタンスを参照してください、 [CocosSharp ガイド (並べて表示) を使用して](~/graphics-games/cocossharp/tiled.md)します。

現時点では、CocosSharp によりレイヤーの順序が変更を削除して、それぞれの親を追加し直してせず`CCScene`(これは、`GameScene`ここで) でメソッドの最後の数行は、レイヤーの順序を変更する必要があるため、します。


### <a name="createcollision"></a>CreateCollision

`CreateCollision`メソッド コンストラクトを`LevelCollision`インスタンスを実行するために使用*solid 衝突*プレーヤーと環境の間。


```csharp
private void CreateCollision()
{
    levelCollision = new LevelCollision();
    levelCollision.PopulateFrom(currentLevel);
}
```

この競合の発生せず、player は、レベルを該当して、ゲームを再生できなくなります。 Solid の競合は、地上方法について説明します。 player を使用し、壁にたどるまたはシーリングを通じてをジャンプからプレーヤーを防止します。

コインの衝突は、– 追加のコードはタイル化されたファイルへの変更のみを追加できます。 


### <a name="processtileproperties"></a>ProcessTileProperties

レベルが読み込まれ、競合を作成すると、一度`ProcessTileProperties`タイルのプロパティに基づいてロジックを実行すると呼びます。 コインの時間が含まれます、`PropertyLocation`プロパティと、これらのプロパティ タイルの座標を定義する構造体。


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

この構造体が構築に使用されるエンティティ インスタンスを作成し、不要なタイルの削除、`ProcessTileProperties`メソッド。


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

Foreach ループの評価チェック、キーがいずれかである場合、各タイル プロパティ`EntityType`または`RemoveMe`します。 `EntityType` エンティティ インスタンスを作成する必要があることを示します。 `RemoveMe` タイルを完全に実行時に削除する必要がありますを示します。

場合のプロパティ、 `EntityType` 、し、キーが見つかった`TryCreateEntity`を呼び出すと、一致するプロパティを使用してエンティティの作成が試行されます、`EntityType`キー。


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


## <a name="adding-new-entities"></a>新しいエンティティを追加します。

コイン時間、ゲーム オブジェクトに対して、エンティティのパターンを使用して (これについては、 [CocosSharp のエンティティのガイド](~/graphics-games/cocossharp/entities.md))。 すべてのエンティティが継承`CCNode`の子として追加できることを意味する、`gameplayLayer`します。

各エンティティの種類は、リストまたは 1 つのインスタンスから直接参照もされます。 たとえば、`Player`によって参照されている、`player`フィールド、およびすべて`Coin`でインスタンスが参照されている、`coins`一覧。 エンティティへの直接参照を保持する (参照することではなく、`gameLayer.Children`リスト) コードを読みやすくするこれらのエンティティにアクセスし、可能性のある高価な型キャストを排除します。

既存のコードでは、新しいエンティティを作成する方法の例としてエンティティ型の数を提供します。 新しいエンティティを作成する、次の手順を使用できます。


### <a name="1---define-a-new-class-using-the-entity-pattern"></a>1 - エンティティ パターンを使用して新しいクラスを定義します。

エンティティを作成するための唯一の要件が継承するクラスを作成するには`CCNode`します。 ほとんどのエンティティは、いくつかのビジュアルをなどがある、 `CCSprite`、そのコンス トラクター内のエンティティの子として追加する必要があります。

CoinTime の提供、`AnimatedSpriteEntity`クラスがアニメーション化されたエンティティの作成を簡略化されます。 詳しく取り上げるアニメーション、[アニメーション エンティティ セクション](#animated-entities)します。


### <a name="2--add-a-new-entry-to-the-trycreateentity-switch-statement"></a>2 – TryCreateEntity switch ステートメントに新しいエントリを追加します。

新しいエンティティのインスタンスをインスタンス化する必要があります、`TryCreateEntity`します。 エンティティが衝突、AI、または、入力を読み取るようにロジックをフレームごとに必要な場合、`GameScene`オブジェクトへの参照を保持する必要があります。 複数のインスタンスが必要な場合 (など`Coin`または`Enemy`インスタンス)、し、新しい`List`に追加する必要があります、`GameScene`クラス。


### <a name="3--modify-tile-properties-for-the-new-entity"></a>3 – 新しいエンティティのタイルのプロパティを変更します。

コードでは、新しいエンティティの作成をサポートすると、新しいエンティティをタイルセットに追加する必要があります。 任意のレベルを開いて編集できる、タイルセット`.tmx`ファイル。 

タイルセットがで .tmx 別に保存されて、 **mastersheet.tsx**ファイルを編集する前にインポートする必要があります。

![](cointime-images/image11.png "編集できる前にインポートする必要がありますので、タイルセットを .tsx ファイルから個別に格納されます。")

インポートされると、選択したタイルのプロパティは編集可能なと EntityType を追加することができます。

![](cointime-images/image12.png "インポートされると、選択したタイルのプロパティは編集可能なと EntityType を追加します。")

新しい一致するように、その値を設定することができます、プロパティが作成されると、`case`で`TryCreateEntity`:

![](cointime-images/image13.png "TryCreateEntity で、新しいケースに一致するように、その値を設定することができます、プロパティが作成した後")

タイルセットが変更された後は、エクスポートする必要があります: これにより、変更は、その他のすべてのレベルの使用を示します。

![](cointime-images/image14.png "エクスポートする必要があります、タイルセットが変更された後")

既存のタイルセットを上書きする**mastersheet.tsx**タイルセット。

![](cointime-images/image15.png "彼はタイルセット既存 mastersheet.tsx タイルセットを上書きする必要があります。")


## <a name="entity-tile-removal"></a>エンティティのタイルの削除

タイルのマップが、ゲームに読み込まれると、個々 のタイルは、静的オブジェクトです。 エンティティには、移動などのカスタム動作が必要があるために、コイン時のコードは、エンティティが作成されたときにタイルを削除します。

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

タイルの自動削除は、コイン、敵など、タイルセットで 1 つだけのタイルを占有するエンティティに対して十分です。 大きなエンティティでは、プロパティと追加のロジックが必要です。

ドアは、2 つのタイルを完全に描画する必要があります。

![](cointime-images/image16.png "ドアが 2 つのタイルを完全に描画する必要があります。")

ドアの下部にあるタイルには、エンティティを作成するためのプロパティが含まれています (**EntityType**設定**ドア**)。

![](cointime-images/image17.png "ドアの下部にあるタイルには、EntityType にドア設定エンティティを作成するためのプロパティが含まれています")

ドアのインスタンスが作成されたときに、ドアの下部にあるタイルのみが削除されると、ために、実行時に、上部のタイルを削除する追加のロジックは必要です。 上部のタイルが、 **RemoveMe**プロパティに設定**true**:

![](cointime-images/image18.png "上部のタイルに設定された RemoveMe プロパティは true")



このプロパティを使用してタイルの削除を`ProcessTileProperties`:


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


## <a name="entity-offsets"></a>エンティティのオフセット

タイルから作成されたエンティティは、目盛りに合わせてタイルの中心とエンティティの中央に配置されます。 などのより大きなエンティティは、`Door`を正しく配置する追加のプロパティとロジックを使用します。 

下部にあるドアのタイルを定義する、`Door`エンティティの配置を指定します、 **y オフセット**4 の値。 このプロパティを使用せず、`Door`インスタンスは、タイルの中央に配置されます。

![](cointime-images/image19.png "タイルの中心に、このプロパティを使用しないでドア インスタンスの配置します。")

 

適用することでこの問題が修正、 **y オフセット**値`ProcessTileProperties`:


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


## <a name="animated-entities"></a>アニメーション化されたエンティティ

コイン時間には、いくつかのアニメーション化されたエンティティが含まれます。 `Player`と`Enemy`エンティティ ウォーク アニメーションを再生して、`Door`すべてコインを収集したら、エンティティは、アニメーションを再生します。


### <a name="achx-files"></a>.achx ファイル

コイン時間アニメーションは、.achx ファイルで定義されます。 間でそれぞれのアニメーションが定義されている`AnimationChain`で定義されている次のアニメーションで示すように、タグ**propanimations.achx**:


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

このアニメーションには、静的な画像を表示するスパイク エンティティの結果として、1 つのフレームにはのみが含まれています。 エンティティは、1 つまたは複数のフレームのアニメーションを表示するかどうか、.achx ファイルを使用できます。 追加のフレームは、コードで、変更しなくても .achx ファイルに追加できます。 

フレームに表示するイメージの定義、`TextureName`パラメーター、および表示の座標、 `LeftCoordinate`、 `RightCoordinate`、 `TopCoordinate`、および`BottomCoordinate`タグ。 – 使用されているイメージのアニメーションのフレームのピクセル座標を表すこれら**mastersheet.png**ここでします。

![](cointime-images/image20.png "イメージのアニメーションのフレームのピクセル座標を表します")

`FrameLength`プロパティ アニメーションのフレームを表示する秒数を定義します。 単一フレームのアニメーションは、この値を無視します。

その他のすべての AnimationChain プロパティ .achx ファイルでは、コイン時間によって無視されます。


### <a name="animatedspriteentity"></a>AnimatedSpriteEntity

アニメーション処理のロジックが含まれている、`AnimatedSpriteEntity`で使用されるほとんどのエンティティの基本クラスとして機能するには、クラス、`GameScene`します。 次の機能を提供します。

 - 読み込み`.achx`ファイル
 - 読み込まれたアニメーションのアニメーションのキャッシュ
 - アニメーションを表示するための CCSprite インスタンス
 - 現在のフレームを変更するためのロジック

スパイクのコンス トラクターは、読み込みアニメーションを使用する方法の簡単な例を提供します。


```csharp
public Spikes ()
{
    LoadAnimations ("Content/animations/propanimations.achx");
    CurrentAnimation = animations [0];
}
```

**PropAnimations.achx**コンス トラクターは、インデックスを使用してこのアニメーションにアクセスするため、1 つのアニメーションにはのみが含まれています。 .Achx ファイルには、複数のアニメーションが含まれるかどうかは、アニメーションは、のように、名前で参照できる、`Enemy`コンス トラクター。


```csharp
walkLeftAnimation = animations.Find (item => item.Name == "WalkLeft");
walkRightAnimation = animations.Find (item => item.Name == "WalkRight");
```


## <a name="summary"></a>まとめ

このガイドでは、コイン時間の実装の詳細について説明します。 コインには、完全版のゲームに作成されますも簡単に変更および拡張可能なプロジェクトには。 リーダーは、新しいレベルを追加して、コインの時間を実装する方法をさらに理解する新しいエンティティの作成時間レベル、修正を費やすことをお勧めします。

## <a name="related-links"></a>関連リンク

- [ゲーム プロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/CoinTime/)
