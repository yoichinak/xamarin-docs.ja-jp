---
title: Fruity Falls game の詳細
description: このガイドでは、一般的な CocosSharp と物理学、コンテンツ管理、ゲームの状態、およびゲームの設計などのゲーム開発の概念を説明、Fruity がゲームを確認します。
ms.prod: xamarin
ms.assetid: A5664930-F9F0-4A08-965D-26EF266FED24
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 959f5eb149ad375d686b17a85eb3d3b8fbdf3659
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114250"
---
# <a name="fruity-falls-game-details"></a>Fruity Falls game の詳細

_このガイドでは、一般的な CocosSharp と物理学、コンテンツ管理、ゲームの状態、およびゲームの設計などのゲーム開発の概念を説明、Fruity がゲームを確認します。_

Fruity Falls は、プレーヤーがポイントを獲得できる色付きのバケットに赤色と黄色の果物を並べ替えますが、物理運動ベースの単純なゲームです。 ゲームの目的では、せず成果物ドロップ間違ったビンにゲームを終了、できるだけ多くのポイントを獲得できます。

![](fruity-falls-images/image1.png "ゲームの目標は、せず成果物ドロップ間違ったビンにゲームを終了できるだけ多くのポイントを獲得するには")

Fruity Falls で導入された概念を拡張し、 [BouncingGame ガイド](~/graphics-games/cocossharp/bouncing-game.md)次を追加することで。

 - Png の形式でコンテンツします。
 - 高度な物理学
 - ゲームの状態 (シーン間の移行)
 - 1 つのクラスに含まれる変数を通じてゲームの係数を調整する機能
 - 組み込みのビジュアル デバッグのサポート
 - ゲームのエンティティを使用してコードの編成
 - 趣味と再生の値に重点を置いてゲームの設計

中に、 [BouncingGame ガイド](~/graphics-games/cocossharp/bouncing-game.md)Fruity が core CocosSharp の概念を導入することに重点をゲームの完成した製品にまとめてさせる方法を示します。 このガイドは、BouncingGame を参照するためリーダーがまず理解している、 [BouncingGame ガイド](~/graphics-games/cocossharp/bouncing-game.md)このガイドを読む前にします。

このガイドでは、Fruity には該当の独自のゲームを作成するための洞察を提供するための設計と実装について説明します。 これには、次のトピックについて説明します。


- [GameController クラス](#gamecontroller-class)
- [ゲームのエンティティ](#game-entities)
- [果物のグラフィック](#fruit-graphics)
- [物理学](#physics)
- [コンテンツのゲーム](#game-content)
- [GameCoefficients](#gamecoefficients)


## <a name="gamecontroller-class"></a>GameController クラス

Fruity は PCL プロジェクトに含まれる、`GameController`ゲームをインスタンス化し、シーンの間での移動を担当するクラス。 このクラスは、重複するコードを削除する、iOS と Android プロジェクトの両方で使用されます。

コード内に含まれる、`Initialize`メソッドは、コードに似ています、`Initialize`が変更されていない CocosSharp テンプレートをメソッドには、変更の数が含まれています。

既定で、`GameView.ContentManager.SearchPaths`デバイスの解像度に依存します。 Fruity がデバイスの解像度に関係なく同じ内容を使用してさらに詳しくは、次に示すように、そのため、`Initialize`メソッドを追加、`Images`へのパス (サブフォルダーは不要) で、 `SearchPaths`:


```csharp
contentSearchPaths.Add ("Images");
```

CocosSharp の新しいテンプレートを継承するクラスを含める`CCLayer`、ゲームのビジュアルとロジックをこのクラスに追加する必要があることを意味します。 代わりに、複数の Fruity 滝`CCLayer`インスタンス コントロールを描画順序。 これら`CCLayer`内に含まれるインスタンス、`GameView`クラスから継承`CCScene`します。 Fruity Falls にも、最初が、複数のシーンが含まれています、`TitleScene`します。 そのため、`Initialize`メソッドをインスタンス化、`TitleScene`インスタンスに渡される`RunWithScene`:


```csharp
var scene = new TitleScene (GameView);
GameView.Director.RunWithScene (scene);
```

Fruity には該当のコンテンツは、見た目上の理由から、低解像度のピクセルのアートとして作成されました。 `GameView.DesignResolution`設定されているため、ゲームは 384 単位の幅と高さを 512、のみ。


```csharp
// We use a lower-resolution display to get a pixellated appearance
int width = 384;
int height = 512;
GameView.DesignResolution = new CCSizeI (width, height); 
```

最後に、`GameController`クラスは、いずれかで呼び出すことができる静的メソッドを提供します。`CCGameScene`別に移行する`CCScene`します。 このメソッドを使用して、間で移動、 `TitleScene` 、`GameScene`します。


## <a name="game-entities"></a>ゲームのエンティティ

Fruity Falls ゲーム オブジェクトのほとんどのエンティティのパターンを利用します。 このパターンの詳細な説明が記載されて、 [CocosSharp のエンティティのガイド](~/graphics-games/cocossharp/entities.md)します。

[エンティティ] フォルダーにエンティティを実装するゲーム オブジェクトが見つかりません、 **FruityFalls.Common**プロジェクト。

![](fruity-falls-images/image2.png "FruityFalls.Common プロジェクトでエンティティ フォルダー")

エンティティから継承するオブジェクトは、 `CCNode`、ビジュアル、競合、およびフレームごとの動作があります。

`Fruit`オブジェクトは、一般的な CocosSharp のエンティティを表します: ビジュアル オブジェクト、競合、およびフレームごとのロジックが含まれています。 コンス トラクターは、フルーツを初期化します。


```csharp
public Fruit ()
{
    CreateFruitGraphic ();
    if (GameCoefficients.ShowCollisionAreas)
    {
        CreateDebugGraphic ();
    }
    CreateCollision ();
    CreateExtraPointsLabel ();
    Acceleration.Y = GameCoefficients.FruitGravity;
}
```


### <a name="fruit-graphics"></a>果物のグラフィック

`CreateFruitGraphic`メソッドを作成、`CCSprite`インスタンスし、それを追加、`Fruit`します。 `IsAntialiased`ゲーム ピクセルの外観を提供するプロパティが false に設定します。 この値がすべて false に設定されて`CCSprite`と`CCLabel`ゲーム全体にわたってインスタンス。


```csharp
private void CreateFruitGraphic()
{
    graphic = new CCSprite ("cherry.png");
    graphic.IsAntialiased = false;
    this.AddChild (graphic);
}
```

プレイヤーが接触するたびに、`Fruit`インスタンス、 `Paddle`、その成果物のポイント値が 1 つずつ増加します。 これは、果物の余分なポイントを同時に、経験豊富なプレイヤーに追加のチャレンジを提供します。 使用して、果物のポイント値を表示、`extraPointsLabel`インスタンス。

`CreateExtraPointsLabel`メソッドを作成、`CCLabel`インスタンスし、それを追加、 `Fruit`:


```csharp
private void CreateExtraPointsLabel()
{
    extraPointsLabel = new CCLabel("", "Arial", 12, CCLabelFormat.SystemFont);
    extraPointsLabel.IsAntialiased = false;
    extraPointsLabel.Color = CCColor3B.Black;
    this.AddChild(extraPointsLabel);
}
```

Fruity Falls には、果物 – さくらんぼと lemons の 2 つのさまざまな種類が含まれています。 `CreateFruitGraphic`で変更できますが、既定のビジュアルを果物ビジュアル割り当てます、`FruitColor`プロパティを呼び出して`UpdateGraphics`:


```csharp
private void UpdateGraphics()
{
if (GameCoefficients.ShowCollisionAreas)
    {
        debugGrahic.Clear ();
        const float borderWidth = 4;
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius,
            CCColor4B.Black);
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius - borderWidth,
            fruitColor.ToCCColor ());
    }
    if (this.FruitColor == FruitColor.Yellow)
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("lemon.png");
        extraPointsLabel.Color = CCColor3B.Black;
        extraPointsLabel.PositionY = 0;
    }
    else
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("cherry.png");
        extraPointsLabel.Color = CCColor3B.White;
        extraPointsLabel.PositionY = -8;
    }
}
```

最初の if ステートメント`UpdateGraphics`衝突の領域を視覚化に使用されるデバッグ グラフィックを更新します。 ゲームの最終リリースを行うと、物理学をデバッグするための開発時に保持できる前に、これらのビジュアルが無効になります。 2 番目の部分の変更、`graphic.Texture`プロパティを呼び出して`CCTextureCache.SharedTextureCache.AddImage`します。 このメソッドは、ファイル名でテクスチャへのアクセスを提供します。 このメソッドの詳細についてで参照できる、[ガイドのテクスチャ キャッシュ](~/graphics-games/cocossharp/texture-cache.md)します。

`extraPointsLabel`果物のイメージとコントラストを維持する色が調整され、その`PositionY`値が調整センターに、`CCLabel`の果物の`CCSprite`:

![](fruity-falls-images/image3.png "果物のイメージとコントラストを保持する extraPointsLabel 色が調整され、その PositionY 値を調整して、センターの果物 CCSprite を表すローカライズされました。")

![](fruity-falls-images/image4.png "果物のイメージとコントラストを保持する extraPointsLabel 色が調整され、その PositionY 値を調整して、センターの果物 CCSprite を表すローカライズされました。")


### <a name="collision"></a>衝突

Fruity Falls Geometry フォルダー内のオブジェクトを使用してカスタムの競合ソリューションを実装します。

![](fruity-falls-images/image5.png "Fruity Falls Geometry フォルダー内のオブジェクトを使用してカスタムの競合ソリューションを実装します。")

いずれかを使用して Fruity 滝で衝突が実装される、`Circle`または`Polygon`エンティティの子として追加されるこれらの 2 種類のいずれかでは、通常のオブジェクト。 たとえば、`Fruit`オブジェクトには、`Circle`と呼ばれる`Collision`でそれを初期化するその`CreateCollision`メソッド。


```csharp
private void CreateCollision()
{
    Collision = new Circle ();
    Collision.Radius = GameCoefficients.FruitRadius;
    this.AddChild (Collision);
}
```

なお、`Circle`クラスから継承、`CCNode`クラス、ゲームのエンティティに子として追加できるように。


```csharp
public class Circle : CCNode
{
    ...
}
```

`Polygon` 作成がのような`Circle`作成のように、`Paddle`クラス。


```csharp
private void CreateCollision()
{
    Polygon = Polygon.CreateRectangle(width, height);
    this.AddChild (Polygon);
}
```

衝突のロジックについては説明[このガイドの後半](#collision)します。


## <a name="physics"></a>物理学

Fruity には該当の物理運動を 2 つのカテゴリに分けることができます: 移動と競合します。 


### <a name="movement-using-velocity-and-acceleration"></a>速度と高速化を使用した移動

Fruity Falls 使用`Velocity`と`Acceleration`と同様に、そのエンティティの動作を制御する値、 [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)します。 エンティティという名前のメソッドで、移動ロジックを実装する`Activity`、フレームごとに 1 回呼び出します。 たとえばでの移動の実装をわかります、`Fruit`クラス`Activity`メソッド。

```csharp
public void Activity(float frameTimeInSeconds)
{
    timeUntilExtraPointsCanBeAdded -= frameTimeInSeconds;
    
    // linear approximation:
    this.Velocity += Acceleration * frameTimeInSeconds;

    // This is a linear approximation to drag. It's used to
    // keep the object from falling too fast, and eventually
    // to slow its horizontal movement. This makes the game easier
    this.Velocity -= Velocity * GameCoefficients.FruitDrag * frameTimeInSeconds;
    
    this.Position += Velocity * frameTimeInSeconds;
}
```
`Fruit`オブジェクトがドラッグの実装内で一意で – どれほど高速の成果物に関連の速度が遅くなります値が移動します。 ドラッグのこの実装を提供、*ターミナル velocity*、果物インスタンス別に分類の最高速度であります。 ドラッグが、ゲームを再生する少し容易に成果物の水平方向移動ダウンも低下します。

`Paddle`オブジェクトも適用されます`Velocity`でその`Activity`メソッドが、その`Velocity`で計算された、`desiredLocation`値。


```csharp
public void Activity(float frameTimeInSeconds)
{
    // This code will cause the cursor to lag behind the touch point
    // Increasing this value reduces how far the paddle lags
    // behind the player’s finger. 
    const float velocityCoefficient = 4;
    // Get the velocity from current location and touch location
    Velocity = (desiredLocation - this.Position) * velocityCoefficient;
    this.Position += Velocity * frameTimeInSeconds;
    ...
}
```

通常、使用するゲーム`Velocity`直接ではなく、オブジェクトの操作の位置を制御するには、初期化後に、少なくともエンティティの位置を設定します。 直接設定することではなく、 `Paddle` 、位置、`Paddle.HandleInput`メソッド割り当てます、`desiredLocation`値。

```csharp
public void HandleInput(CCPoint touchPoint)
{
    desiredLocation = touchPoint;
}
```

### <a name="collision"></a>衝突

Fruity Falls など、果物と collidable 他のオブジェクト間の半現実的な衝突の実装、`Paddle`と`GameScene.Splitter`します。 競合をデバッグするには、Fruity が衝突領域の可視性を変更して、`GameCoefficients.ShowDebugInfo`で、`GameCoefficients.cs`ファイル。


```csharp
public static class GameCoefficients
{
    ... 
    public const bool ShowCollisionAreas = true;
    ...
}
```

この値を設定`true`衝突の領域を描画します。  

![](fruity-falls-images/image6.png "衝突の領域のレンダリングをにより、この値を true に設定します。")

競合のロジックの開始、`GameScene.Activity`メソッド。


```csharp
private void Activity(float frameTimeInSeconds)
{
    if (hasGameEnded == false)
    {
        paddle.Activity(frameTimeInSeconds);
        foreach (var fruit in fruitList)
        {
            fruit.Activity(frameTimeInSeconds);
        }
        spawner.Activity(frameTimeInSeconds);
        DebugActivity();
        PerformCollision();
    }
}
```

`PerformCollision` すべての競合を処理する`Fruit`他のオブジェクトのインスタンス。 


```csharp
private void PerformCollision()
{
    // reverse for loop since fruit may be destroyed:
    for(int i = fruitList.Count - 1; i > -1; i--)
    {
        var fruit = fruitList[i];
        FruitVsPaddle(fruit);
        FruitPolygonCollision(fruit, splitter.Polygon, CCPoint.Zero);
        FruitVsBorders(fruit);
        FruitVsBins(fruit);
    }
}
```

#### <a name="fruitvsborders"></a>FruitVsBorders

`FruitVsBorders` 競合では、別のクラスに含まれるロジックに依存するのではなく、競合の独自のロジックを実行します。 成果物と、画面の境界線の間の競合が solid 完璧ではありません – 果物慎重パドル移動して、画面の端からプッシュする可能性があるために、この違いが存在します。 果物、パドルとヒット時 画面からバウンドさせますが、過去の端と画面の外を移動するが、プレーヤーが緩やかに変化成果物をプッシュする場合。


```csharp
private void FruitVsBorders(Fruit fruit)
{
    if(fruit.PositionX - fruit.Radius < 0 && fruit.Velocity.X < 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionX + fruit.Radius > gameplayLayer.ContentSize.Width && fruit.Velocity.X > 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionY + fruit.Radius > gameplayLayer.ContentSize.Height && fruit.Velocity.Y > 0)
    {
        fruit.Velocity.Y *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
}
```

#### <a name="fruitvsbins"></a>FruitVsBins

`FruitVsBins`メソッドは 2 つの区間のいずれかに任意の果物が低下する場合にチェックします。場合は、し (この果物/箱は、一致を色) 場合、ポイントが授与はプレーヤーまたは (色が一致しない) 場合、ゲームは終了します。


```csharp
private void FruitVsBins(Fruit fruit)
{
    foreach(var bin in fruitBins)
    {
        if(bin.Polygon.CollideAgainst(fruit.Collision))
        {
            if(bin.FruitColor == fruit.FruitColor)
            {
                // award points:
                score += 1 + fruit.ExtraPointValue;
                scoreText.Score = score;
                Destroy(fruit);
            }
            else
            {
                EndGame();
            }
            break;
        }
    }
}
```

#### <a name="fruitvspaddle-and-fruitpolygoncollision"></a>FruitVsPaddle と FruitPolygonCollision

パドルと分割 (領域の 2 つの箱を分離すること) と成果物と成果物に依存して両方の衝突、`FruitPolygonCollision`メソッド。 このメソッドでは、3 つの部分があります。

1. オブジェクトが衝突するかどうかをテストします。
1. これらが重複しないように、オブジェクト (だけ Fruity には該当の果物) を移動します。
1. 次の例では、上記で作成ポイント バウンドをシミュレートするには、オブジェクト (だけ Fruity には該当の果物) の速度を調整します。


```csharp
private static bool FruitPolygonCollision(Fruit fruit, Polygon polygon, CCPoint polygonVelocity)
{
    // Test whether the fruit collides
    bool didCollide = polygon.CollideAgainst(fruit.Collision);
    if (didCollide)
    {
        var circle = fruit.Collision;
        // Get the separation vector to reposition the fruit so it doesn't overlap the polygon
        var separation = CollisionResponse.GetSeparationVector(circle, polygon);
        fruit.Position += separation;
        // Adjust the fruit's Velocity to make it bounce:
        var normal = separation;
        normal.Normalize();
        fruit.Velocity = CollisionResponse.ApplyBounce(
            fruit.Velocity, 
            polygonVelocity, 
            normal, 
            GameCoefficients.FruitCollisionElasticity);
    }
    return didCollide;
}
```

Fruity Falls 衝突の応答が片側 – のみ果物の速度と位置を調整します。 その他のゲームで、互いに、岩またはクラッシュした 2 台の車をプッシュする文字など、関連する両方のオブジェクトに影響を与える競合があることに注目に値します。

含まれる衝突ロジックの背後にある数値演算、`Polygon`と`CollisionResponse`クラスについては、このガイドの範囲外としてに書き込まれた、これらのメソッド使用できますのゲームの多くの種類。 多角形と`CollisionResponse`クラスは、このコードは、さまざまな種類のゲームを使用できるようにもと四角形ではないの凸多角形をサポートします。

 


## <a name="game-content"></a>コンテンツのゲーム

Fruity には該当のアートは、BouncingGame からゲームをすぐに区別します。 ゲームの設計と似ていますが、プレーヤーは 2 つのゲームを検索する方法の違いをすぐに表示されます。 プレイヤーがゲームでは、そのビジュアルをゲームを試してみてくださいするかどうか多くの場合、決定します。 そのため、開発者が行う視覚に訴えるにリソースを投資することが非常に重要はゲームです。

Fruity には該当のアートは、次の目的で作成されました。

 - 一貫性のあるテーマ
 - 1 つだけの文字 – Xamarin monkey に重点を置いた
 - 発生する明るい色を緩和の楽しいものを作成するには
 - ゲームプレイ オブジェクトは、視覚的に追跡するために簡単であるため、バック グラウンドとフォア グラウンド間コントラストします。
 - リソースの量が多いアニメーションなしの単純な視覚効果を作成する機能


### <a name="content-location"></a>コンテンツの場所

Fruity Falls には、Android プロジェクトで、Images フォルダーにそのすべての内容が含まれます。

![](fruity-falls-images/image7.png "Fruity Falls がそのコンテンツのすべての Android プロジェクトで、Images フォルダーに含まれています")

これらの同じファイルは、重複を避けるために iOS プロジェクトにリンクされ、両方のプロジェクト ファイルにその変更に影響を与えます。

![](fruity-falls-images/image8.png "これらの同じファイルは、重複を避けるために iOS プロジェクトでリンクされ、両方のプロジェクト ファイルにその変更に影響を与える")

内でコンテンツが含まれていないことに注意、 **%ld**または**Hd**フォルダーで、既定の CocosSharp テンプレートの一部であります。 **%Ld**と**Hd**電話、タブレットなどの高解像度のデバイス用に 1 つなど、解像度の低いデバイス用に 1 – コンテンツの 2 つのセットを提供するゲームに使用するフォルダーが対象としています。 Fruity がアートは意図的にで作成、ピクセル見た目、のでさまざまな画面サイズにコンテンツを提供する必要はありません。 そのため、 **%ld**と**Hd**フォルダーがプロジェクトから完全に削除されました。


### <a name="gamescene-layering"></a>GameScene レイヤー

このガイドでは、前半で説明したように、`GameScene`すべてのゲーム オブジェクトのインスタンス化、配置、および (競合) などのオブジェクト間のロジックを担当します。 すべてのオブジェクトは 4 つのいずれかに追加`CCLayer`インスタンス。


```csharp
CCLayer backgroundLayer;
CCLayer gameplayLayer;
CCLayer foregroundLayer;
CCLayer hudLayer;
```

これら 4 つの層が作成、`CreateLayers`メソッド。 追加されますが、`GameScene`後ろから前の順序で。


```csharp
private void CreateLayers()
{
    backgroundLayer = new CCLayer();
    this.AddLayer(backgroundLayer);
    gameplayLayer = new CCLayer();
    this.AddLayer(gameplayLayer);
    foregroundLayer = new CCLayer();
    this.AddLayer(foregroundLayer);
    hudLayer = new CCLayer();
    this.AddLayer(hudLayer);
}
```

使用してレイヤーをすべてのビジュアル オブジェクトを追加のレイヤーが作成されたら、`AddChild`メソッドのように、`CreateBackground`メソッド。


```csharp
private void CreateBackground()
{
    var background = new CCSprite("background.png");
    background.AnchorPoint = new CCPoint(0, 0);
    background.IsAntialiased = false;
    backgroundLayer.AddChild(background);
}
```


### <a name="vine-entity"></a>つるエンティティ

`Vine`エンティティは、一意にコンテンツの使用 – ゲームプレイに影響を与えません。 20 個で構成されています`CCSprite`つるが常に、画面の上部に達したかどうかを確認する試行錯誤が選択した数のインスタンスします。


```csharp
public Vine ()
{
    const int numberOfVinesToAdd = 20;
    for (int i = 0; i < numberOfVinesToAdd; i++)
    {
        var graphic = new CCSprite ("vine.png");
        graphic.AnchorPoint = new CCPoint(.5f, 0);
        graphic.PositionY = i * graphic.ContentSize.Height;
        this.AddChild (graphic);
    }
}
```

最初の`CCSprite`下端には、つるの位置が一致するように配置されます。 これは、AnchorPoint 設定`new CCPoint(.5f, 0)`します。 `AnchorPoint`プロパティで使用*座標を正規化*どの範囲 0 ~ 1 のサイズに関係なくの間、 `CCSprite`:

![](fruity-falls-images/image9.png "0 から、CCSprite のサイズに関係なく 1 までを範囲に正規化された座標を使用する AnchorPoint プロパティ")

後続のつるスプライトを使用して、配置、`graphic.ContentSize.Height`値で、イメージの高さをピクセル単位で返します。 次の図は、つるグラフィックを上に並べて表示する方法を示しています。

![](fruity-falls-images/image10.png "この図は、つるグラフィックを上に並べて表示する方法を示しています。")

配信元から、 `Vine` 、つるの下部にあるエンティティは、その後に配置するはのように簡単です。、`Paddle.CreateVines`メソッド。


```csharp
private void CreateVines()
{
    // Increasing this value moves the vines closer to the 
    // center of the paddle.
    const int vineDistanceFromEdge = 4;
    leftVine = new Vine ();
    var leftEdge = -width / 2.0f;
    leftVine.PositionX = leftEdge + vineDistanceFromEdge;
    this.AddChild (leftVine);
    rightVine = new Vine ();
    var rightEdge = width / 2.0f;
    rightVine.PositionX = rightEdge - vineDistanceFromEdge;
    this.AddChild (rightVine);
}
```

インスタンスをつる、`Paddle`エンティティにも注目に値する回転動作があります。 `Paddle`に従ってプレイヤーの入力 (これは、このガイドは、以下で詳しく説明します)、エンティティ全体を回転、`Vine`インスタンスがこの回転によっても影響します。 ただし、回転、`Vine`インスタンスと共に、`Paddle`次のアニメーションに示すように、望ましくない効果が生成されます。

![](fruity-falls-images/image11.gif "ただし、回転パドルと共につるインスタンスが生成されます、望ましくない効果アニメーションを次に示すように")

対抗する、 `Paddle` 、回転、`leftVine`と`rightVine`インスタンスをローテーションのように、パドルの反対の方向、`Paddle.Activity`メソッド。


```csharp
public void Activity(float frameTimeInSeconds)
{
    ...
    // This value adds a slight amount of rotation
    // to the vine. Having a small amount of tilt looks nice.
    float vineAngle = this.Velocity.X / 100.0f;
    leftVine.Rotation = -this.Rotation + vineAngle;
    rightVine.Rotation = -this.Rotation + vineAngle;
}
```

を通じてつるに少量の回転が追加されたことに注意してください、`vineAngle`係数。 この値は、vines の回転量を調整する変更できます。


## <a name="gamecoefficients"></a>GameCoefficients

Fruity 滝にはというクラスが含まれていますので、すべての優れたゲームは、製品のイテレーションの`GameCoefficients`ゲームをプレイする方法を制御します。 このクラスには、コントロールの物理学、レイアウト、起動、およびスコア付けするゲーム全体で使用されている表現力豊かな変数が含まれています。

たとえば、果物の物理運動は、次の変数によって制御されます。


```csharp
public static class GameCoefficients
{
    ...
    
    // The strength of the gravity. Making this a 
    // smaller (bigger negative) number will make the
    // fruit fall faster. A larger (smaller negative) number
    // will make the fruit more floaty.
    public const float FruitGravity = -60;
    // The impact of "air drag" on the fruit, which gives
    // the fruit terminal velocity (max falling speed) and slows
    // the fruit's horizontal movement (makes the game easier)
    public const float FruitDrag = .1f;
    // Controls fruit collision bouncyness. A value of 1
    // means no momentum is lost. A value of 0 means all
    // momentum is lost. Values greater than 1 create a spring-like effect
    public const float FruitCollisionElasticity = .5f;
    
    ...
}
```

高速な成果物が、水平方向の方法を調整するこれらの変数を使用できる名前が示すよう移動は、時刻、およびパドル バウンス経由で、速度が低下します。

(XML) などのコードまたはデータ ファイルに格納されているゲームの係数をプログラマではないゲーム設計者とチームにとって特に重要であることができます。

`GameCoefficients`クラスには、情報または衝突の領域の生成などのデバッグ情報を有効にするための値も含まれています。


```csharp
public static class GameCoefficients
{
    ...
    // This controls whether debug information is displayed on screen.
    public const bool ShowDebugInfo = false;
    public const bool ShowCollisionAreas = false;
    ...
}
```


## <a name="conclusion"></a>まとめ

このガイドでは、Fruity がゲームについて説明しました。 コンテンツ、物理学、ゲームの状態管理などの概念を説明しました。

## <a name="related-links"></a>関連リンク

- [CocosSharp API ドキュメント](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [完成したプロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/FruityFalls/)
