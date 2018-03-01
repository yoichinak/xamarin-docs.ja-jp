---
title: "Fruity がゲームの詳細"
description: "このガイドでは、一般的な CocosSharp および物理学、コンテンツの管理、ゲームの状態とゲームのデザインなどのゲーム開発の概念をカバーする Fruity がゲームを確認します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A5664930-F9F0-4A08-965D-26EF266FED24
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 307fdec697f2b94ddfdfe0c380e02fd69e197132
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="fruity-falls-game-details"></a>Fruity がゲームの詳細

_このガイドでは、一般的な CocosSharp および物理学、コンテンツの管理、ゲームの状態とゲームのデザインなどのゲーム開発の概念をカバーする Fruity がゲームを確認します。_

Fruity なったは、プレーヤーがポイントを取得する色つきのバケットに赤色と黄色の果物を並べ替えます物理ベースの単純なゲームです。 ゲームの目的は、せず果物ドロップ間違ったビンにゲームを終了、できるだけ多くの点を獲得します。

![](fruity-falls-images/image1.png "ゲームの目的は、せず果物ドロップ間違ったビンにゲームを終了、できるだけ多くのポイントを取得するには")

Fruity がで導入された概念を拡張し、 [BouncingGame ガイド](~/graphics-games/cocossharp/first-game/index.md)以下を追加します。

 - Png の形式でコンテンツします。
 - 高度な物理
 - ゲーム状態 (シーンの間の移行)
 - 1 つのクラスに含まれる変数を介して、ゲームの係数を調整する機能
 - 組み込みのビジュアル デバッグ サポート
 - ゲームのエンティティを使用してコード組織
 - 楽しいと再生の値に重点を置いてゲームの設計

BouncingGame は、中核となる CocosSharp 概念の概要に重点を置いて、中に Fruity 滝はさせるすべて同時にゲーム完成する方法を示します。 このガイドは、BouncingGame を参照するためのリーダー必要がありますまず理解している、 [BouncingGame ガイド](~/graphics-games/cocossharp/first-game/index.md)このガイドを読む前にします。

このガイドでは、実装と、独自のゲームを作成するための洞察を提供する Fruity 滝の設計について説明します。 次のトピックがについて説明します。


- [GameController クラス](#GameController_Class)
- [ゲームのエンティティ](#Game_Entities)
- [果物グラフィック](#Fruit_Graphics)
- [物理的な特性](#Physics)
- [ゲームの内容](#Game_Content)
- [GameCoefficients](#GameCoefficients)


# <a name="gamecontroller-class"></a>GameController クラス

Fruity 分類 PCL プロジェクトに含まれる、`GameController`クラスは、ゲームをインスタンス化して、シーンの間での移動を担当します。 このクラスは、重複するコードを削除 iOS と Android プロジェクトの両方で使用します。

コード内に含まれる、`Initialize`メソッドは、コードに似ています、`Initialize`で変更されていない CocosSharp テンプレートではなく、メソッドには、変更の数が含まれています。

既定では、`GameView.ContentManager.SearchPaths`デバイスの解像度に依存します。 Fruity 滝がデバイスの解像度に関係なく同じ内容を使用してさらに詳しく後述するようため、`Initialize`メソッドを追加、 `Images` (サブフォルダーは不要) のパスを`SearchPaths`:


```csharp
contentSearchPaths.Add ("Images");
```

新しい CocosSharp テンプレートから継承するクラスを含める`CCLayer`、ゲームのビジュアルとロジックをこのクラスに追加する必要があることを意味します。 代わりに、複数の使用 Fruity 滝`CCLayer`インスタンス コントロールを描画順序。 これら`CCLayer`インスタンスに含まれる、`GameView`から継承されるクラスが`CCScene`です。 Fruity 代わりには、最初の中の複数のシーンも含まれています、`TitleScene`です。 したがって、`Initialize`メソッドがインスタンス化、`TitleScene`に渡されるインスタンス`RunWithScene`:


```csharp
var scene = new TitleScene (GameView);
GameView.Director.RunWithScene (scene);
```

Fruity 滝のコンテンツは、見た目上の理由から、低解像度ピクセル アートとして作成されました。 `GameView.DesignResolution`設定されているため、ゲームは、ワイド 384 単位、512 の高さのみ。


```csharp
// We use a lower-resolution display to get a pixellated appearance
int width = 384;
int height = 512;
GameView.DesignResolution = new CCSizeI (width, height); 
```

最後に、`GameController`クラスを提供する静的メソッドによって呼び出すことができますを`CCGameScene`別に移行する`CCScene`です。 このメソッドを使用して、間で移動、`TitleScene`と`GameScene`です。


# <a name="game-entities"></a>ゲームのエンティティ

Fruity がゲームのオブジェクトのほとんどのエンティティのパターンでは、使用できます。 このパターンの詳細については含まれて、 [CocosSharp 内のエンティティのガイド](~/graphics-games/cocossharp/entities.md)です。

エンティティ フォルダー内のエンティティを実装するゲーム オブジェクトが見つかりません、 **FruityFalls.Common**プロジェクト。

![](fruity-falls-images/image2.png "FruityFalls.Common プロジェクト内のエンティティ フォルダー")

エンティティから継承されるオブジェクトは、`CCNode`をビジュアル、競合の発生とフレームごとの動作があります。

`Fruit`オブジェクトは、一般的な CocosSharp エンティティを表します。 ビジュアル オブジェクト、競合、およびフレームごとのロジックが含まれています。 コンス トラクターは、果物を初期化します。


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


## <a name="fruit-graphics"></a>果物グラフィック

`CreateFruitGraphic`メソッドを作成、`CCSprite`をインスタンス化し、それを追加、`Fruit`です。 `IsAntialiased`外観、ゲーム、ピクセルにプロパティが false に設定します。 この値は false に設定すべてに`CCSprite`と`CCLabel`ゲーム全体にわたってインスタンス。


```csharp
private void CreateFruitGraphic()
{
    graphic = new CCSprite ("cherry.png");
    graphic.IsAntialiased = false;
    this.AddChild (graphic);
}
```

プレーヤーの接触するたびに、`Fruit`インスタンス、 `Paddle`、その成果物のポイントの値が 1 つずつ増加します。 これは、経験豊富なプレーヤーの余分なポイントの成果物を同時にする追加のチャレンジを提供します。 使用して、果物のポイントの値を表示、`extraPointsLabel`インスタンス。

`CreateExtraPointsLabel`メソッドを作成、`CCLabel`をインスタンス化し、それを追加、 `Fruit`:


```csharp
private void CreateExtraPointsLabel()
{
    extraPointsLabel = new CCLabel("", "Arial", 12, CCLabelFormat.SystemFont);
    extraPointsLabel.IsAntialiased = false;
    extraPointsLabel.Color = CCColor3B.Black;
    this.AddChild(extraPointsLabel);
}
```

Fruity 代わりには、2 つのさまざまな種類果物 – 結婚記念日おめでとうと lemons にはが含まれています。 `CreateFruitGraphic`で変更できますが、デフォルトのビジュアル、果物ビジュアル割り当てます、`FruitColor`プロパティで、さらにこの`UpdateGraphics`:


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

最初の if ステートメント`UpdateGraphics`衝突領域を視覚化に使用されるデバッグ グラフィックを更新します。 ゲームの最終リリースしますが、物理をデバッグするための開発中に上に保持できる前に、これらのビジュアルがオフになります。 2 番目の一部の変更、`graphic.Texture`プロパティを呼び出して`CCTextureCache.SharedTextureCache.AddImage`です。 このメソッドは、ファイル名で、テクスチャへのアクセスを提供します。 詳細については、このメソッドには含まれて、[ガイドのテクスチャ キャッシュ](~/graphics-games/cocossharp/texture-cache.md)です。

`extraPointsLabel`果物のイメージとコントラストを保持する色を調整とその`PositionY`値が調整センターに、`CCLabel`果物の  `CCSprite`:

![](fruity-falls-images/image3.png "果物のイメージとコントラストを保持する extraPointsLabel 色が調整され、果物 CCSprite で表すローカライズされたを中央には、その PositionY 値を調整してください。")

![](fruity-falls-images/image4.png "果物のイメージとコントラストを保持する extraPointsLabel 色が調整され、果物 CCSprite で表すローカライズされたを中央には、その PositionY 値を調整してください。")


## <a name="collision"></a>衝突

Fruity 代わりには、Geometry フォルダー内のオブジェクトを使用してカスタム競合ソリューションが実装されています。

![](fruity-falls-images/image5.png "Fruity 滝 Geometry フォルダー内のオブジェクトを使用してカスタム競合ソリューションを実装します。")

Fruity 滝で競合を実装するか、`Circle`または`Polygon`エンティティの子として追加されるこれらの 2 種類のいずれかの通常のオブジェクト。 たとえば、`Fruit`オブジェクトには、`Circle`と呼ばれる`Collision`で初期化するその`CreateCollision`メソッド。


```csharp
private void CreateCollision()
{
    Collision = new Circle ();
    Collision.Radius = GameCoefficients.FruitRadius;
    this.AddChild (Collision);
}
```

なお、`Circle`クラスから継承、`CCNode`ゲームのエンティティに子として追加できるように、クラスします。


```csharp
public class Circle : CCNode
{
    ...
}
```

`Polygon` 作成がに似ていますが`Circle`作成のように、`Paddle`クラス。


```csharp
private void CreateCollision()
{
    Polygon = Polygon.CreateRectangle(width, height);
    this.AddChild (Polygon);
}
```

衝突ロジックについては説明[このガイドの後半](#Collision)です。


# <a name="physics"></a>物理的な特性

Fruity 代わりに、物理的な特性は、2 つのカテゴリに分けることが: の移動と競合します。 


## <a name="movement-using-velocity-and-acceleration"></a>ベロシティとアクセラレータを使用して移動

Fruity 滝を使用して`Velocity`と`Acceleration`と同様に、そのエンティティの動作を制御する値、 [BouncingGame](~/graphics-games/cocossharp/first-game/index.md)です。 エンティティは、という名前のメソッドで、移動ロジックを実装`Activity`、フレームごとに 1 回呼び出します。 たとえば、上の移動の実装を参照してお、`Fruit`クラス`Activity`メソッド。

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
`Fruit`オブジェクトは、ドラッグの実装で一意 – 速度の成果物に関連して、ベロシティの低下はの値が移動します。 ドラッグのこの実装を提供する*ターミナル ベロシティ*、果物インスタンス範囲を最大速度であります。 ドラッグも遅く、ゲームを再生する若干容易に果物の水平方向へ移動します。

`Paddle`オブジェクトも適用されます`Velocity`でその`Activity`メソッドが、その`Velocity`を使用して計算されます、`desiredLocation`値。


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

通常、使用するゲーム`Velocity`オブジェクトの位置を直接制御するには、初期化後に、少なくともエンティティの位置を設定します。 直接設定ではなく、 `Paddle` 、位置、`Paddle.HandleInput`メソッド割り当てます、`desiredLocation`値。

```csharp
public void HandleInput(CCPoint touchPoint)
{
    desiredLocation = touchPoint;
}
```

## <a name="collision"></a>衝突

Fruity 滝など、果物と collidable 他のオブジェクト間の半現実的な衝突の実装、`Paddle`と`GameScene.Splitter`です。 デバッグする際の競合、Fruity 滝衝突領域表示できる変更することによって、`GameCoefficients.ShowDebugInfo`で、`GameCoefficients.cs`ファイル。


```csharp
public static class GameCoefficients
{
    ... 
    public const bool ShowCollisionAreas = true;
    ...
}
```

この値を設定`true`衝突領域のレンダリングを有効にします。  

![](fruity-falls-images/image6.png "衝突領域のレンダリングを有効にこの値を true に設定します。")

衝突ロジックの開始、`GameScene.Activity`メソッド。


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

`PerformCollision` すべての競合を処理`Fruit`他のオブジェクトとインスタンス。 


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

### <a name="fruitvsborders"></a>FruitVsBorders
`FruitVsBorders` 競合は、別のクラスに含まれるロジックに依存するのではなく、競合を独自のロジックを実行します。 成果物と、画面の境界線の間で競合が完全に純色ではありません-注意パドル移動して、画面の端からプッシュする果物の可能性があるために、この違いが存在します。 成果物は、パドルとヒット時 画面から離れるバウンドしますが、プレーヤーが緩やかに変化果物をプッシュする場合は移動過去の端と画面をオフ。


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

### <a name="fruitvsbins"></a>FruitVsBins
`FruitVsBins`メソッドは任意の成果物に 2 つの区間のいずれかになった場合にチェックを担当します。場合は、プレーヤーが与えられますポイント (果物/bin 色の一致) する場合か、ゲームは終了 (色が一致しない) 場合。


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

### <a name="fruitvspaddle-and-fruitpolygoncollision"></a>FruitVsPaddle と FruitPolygonCollision
パドルおよび (2 つの区間を分離する領域) のスプリッターと果物の果物のどちらも、依存衝突、`FruitPolygonCollision`メソッドです。 このメソッドには、3 つの部分があります。

1. オブジェクトが競合するかどうかをテストします。
1. これらが重複しないように (Fruity 滝の果物だけ) のオブジェクトを移動します。
1. 次の例では、上でポイント バウンドをシミュレートするには、オブジェクト (Fruity 滝の果物だけ) の速度を調整します。


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

Fruity 滝衝突応答が片側 – のみ果物のベロシティと位置を調整します。 その他のゲームで、互いの中に、ボールダーまたはクラッシュした 2 台の車をプッシュする文字など、関連する両方のオブジェクトに影響する競合があることに注目に値します。

含まれる衝突ロジックの背後にある数値演算、`Polygon`と`CollisionResponse`クラスについては、このガイドの範囲外ですとしてに書き込まれた、これらのメソッドできます多くのゲームの種類。 多角形と`CollisionResponse`クラスのサポートもと四角形ではないの凸多角形、多くのゲームの種類は、このコードを使用できるようにします。

 


# <a name="game-content"></a>ゲームの内容

Fruity 滝でアートは、すぐに、BouncingGame とゲームを区別します。 ゲームの設計と似ていますが、プレーヤーはすぐに、2 つのゲームを検索する方法に違いを確認します。 ゲーマーが面では、そのビジュアルでゲームを再試行するかどうか多くの場合、決定します。 したがって、開発者が視覚に訴えるにする際にリソースを投資は非常に重要ですがゲームです。

Fruity 滝のアートは、次の目的で作成されました。

 - 一貫性のあるテーマ
 - 1 文字だけ – Xamarin サルに重点を置いて
 - 明るい色の条件の緩和、楽しいものが発生します。
 - ゲーム プレイ オブジェクトは、視覚的に追跡するために簡単であるため、背景と前景の間でコントラストします。
 - リソースの量が多いアニメーションなしの単純な視覚効果を作成する機能


## <a name="content-location"></a>コンテンツの場所

Fruity 滝には、Android プロジェクトで、Images フォルダーのすべてのコンテンツが含まれます。

![](fruity-falls-images/image7.png "Fruity がそのコンテンツのすべての Android プロジェクトで Images フォルダーに含まれています")

これらの同じファイルには、重複を避けるために iOS プロジェクトでリンクされ、ファイルにその変更が両方のプロジェクトに影響します。

![](fruity-falls-images/image8.png "これらの同じファイルには、重複を避けるために iOS プロジェクトでリンクされ、ファイルにその変更が両方のプロジェクトに影響")

内でコンテンツが含まれていないことに注意、 **%ld**または**Hd**フォルダーで、既定の CocosSharp テンプレートの一部であります。 **%Ld**と**Hd**コンテンツ: 携帯電話、タブレットなどの高解像度のデバイス用の 1 つなどの低解像度のデバイスの 1 つの 2 つのセットが用意されているゲームをするためのフォルダーが目的としています。 Fruity 滝アートは意図的にで作成、ピクセル、見た目のでのさまざまな画面サイズにコンテンツを提供する必要はありません。 したがって、 **%ld**と**Hd**フォルダーがプロジェクトから完全に削除されました。


## <a name="gamescene-layering"></a>GameScene 重ね順

このガイドの前半で説明したように、として、GameScene はすべてのゲームのオブジェクトのインスタンス化、配置、および (競合) などのオブジェクト間のロジックをします。 すべてのオブジェクトは 4 つのいずれかに追加`CCLayer`インスタンス。


```csharp
CCLayer backgroundLayer;
CCLayer gameplayLayer;
CCLayer foregroundLayer;
CCLayer hudLayer;
```

これら 4 つの層で作成、`CreateLayers`メソッドです。 追加されることに注意してください、`GameScene`背面の前面の順に。


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

レイヤーを作成すると、すべてのビジュアル オブジェクトに追加されますを使用してレイヤー、`AddChild`メソッドのように、`CreateBackground`メソッド。


```csharp
private void CreateBackground()
{
    var background = new CCSprite("background.png");
    background.AnchorPoint = new CCPoint(0, 0);
    background.IsAntialiased = false;
    backgroundLayer.AddChild(background);
}
```


## <a name="vine-entity"></a>つるエンティティ

`Vine`エンティティでは、コンテンツのために使用が一意に – ゲーム プレイに影響を与えません。 構成される 20`CCSprite`つる、画面の上部を常に達したかどうかを確認する試行錯誤が選択した数のインスタンスします。


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

最初の`CCSprite`が配置されているので、下端つるの位置に一致します。 これを実現、AnchorPoint を設定して`new CCPoint(.5f, 0)`です。 `AnchorPoint`プロパティ使用*座標を正規化*どの範囲 0 ~ 1 のサイズに関係なくの間、 `CCSprite`:

![](fruity-falls-images/image9.png "0 から 1、CCSprite のサイズに関係なくどの範囲に正規化された座標を使用する AnchorPoint プロパティ")

後続のつるスプライトを使用して、配置、`graphic.ContentSize.Height`値で、画像の高さをピクセル単位で返します。 次の図は、つるグラフィックを上に並べて表示する方法を示しています。

![](fruity-falls-images/image10.png "この図は、つるグラフィックを上に並べて表示する方法を示しています。")

元のため、`Vine`エンティティ、つるの下部には、配置に示すように簡単です。、`Paddle.CreateVines`メソッド。


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

インスタンスにあるつる、`Paddle`エンティティにも注目に値する回転動作があります。 `Paddle`プレーヤーの入力 (このガイドでは、以下の詳細の詳細について説明します)、に従ってエンティティ全体を回転、`Vine`インスタンスがこのローテーションによっても影響します。 ただし、回転、`Vine`インスタンスと共に、`Paddle`次のアニメーションに示すように、不要な視覚効果を生成します。

![](fruity-falls-images/image11.gif "ただし、回転パドルと共につるインスタンスが生成されます不要な視覚効果の次のアニメーションに示すよう")

対処、`Paddle`回転、`leftVine`と`rightVine`インスタンスは、回転のように、パドルの反対の方向、`Paddle.Activity`メソッド。


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

回転の量が少ないがを通じてつるに追加されたことに注意してください、`vineAngle`係数。 この値は、vines の回転量を調整する変更できます。


# <a name="gamecoefficients"></a>GameCoefficients

すべての適切なゲームは、製品のイテレーションの Fruity 滝にはと呼ばれるクラスが含まれています`GameCoefficients`ゲームの再生方法を制御します。 このクラスには、コントロールの物理学、レイアウト、起動、およびスコア付けするゲーム全体で使用される表現力が豊か変数が含まれています。

たとえば、果物の物理的特性は、次の変数によって制御されます。


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

どのように水平速さ果物を調整するこれらの変数を使用できる名前が示すよう移動速度が低下し、パドル バウンス経過します。

(XML) などのコードまたはデータ ファイルに格納されているゲームの係数はプログラマではないゲームのデザイナーでのチームに対して特に有効にすることはできます。

`GameCoefficients`クラスには、オンにする情報または衝突領域の生成などのデバッグ情報の値も含まれています。


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


# <a name="conclusion"></a>まとめ

このガイドでは、Fruity 滝ゲームについて説明します。 コンテンツ、物理的な特性、ゲーム状態管理などの概念を説明します。

## <a name="related-links"></a>関連リンク

- [CocosSharp API ドキュメント](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [完成したプロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/FruityFalls/)
