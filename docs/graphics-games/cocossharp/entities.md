---
title: CocosSharp 内のエンティティ
description: エンティティのパターンは、ゲーム コードを整理する強力な方法です。 読みやすさを向上、維持するため、コードを容易にし、組み込みの親/子の機能を活用します。
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D3261CE-AC96-4296-8A53-A76A42B927A8
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: bb4af0f76f6b266cad4eb969d987a346b7396aa9
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="entities-in-cocossharp"></a>CocosSharp 内のエンティティ

_エンティティのパターンは、ゲーム コードを整理する強力な方法です。読みやすさを向上、維持するため、コードを容易にし、組み込みの親/子の機能を活用します。_

エンティティ パターンでは、強化されたコードの組織で CocosSharp では、開発者の作業を向上できます。 このチュートリアルは、2 つのエンティティ: 出荷エンティティとの行頭文字エンティティを作成する方法を示す実践的な例になります。 これらのエンティティは、自己完結型のオブジェクトをする、1 回インスタンス化することを示します自動的にレンダリングされる、その型の適切な移動ロジックを実行します。 

このガイドでは、次のトピックについて説明します。

 - ゲームのエンティティの概要
 - 型と特定のエンティティ型には、[全般]
 - プロジェクトのセットアップ
 - エンティティ クラスを作成します。
 - エンティティ インスタンスを追加します `GameLayer`
 - 内のエンティティのロジックに反応します `GameLayer`

完成したゲームは、次のようになります。

![](entities-images/image1.png "完成したゲームのようになります")


## <a name="introduction-to-game-entities"></a>ゲームのエンティティの概要

ゲームのエンティティは、レンダリング、衝突、物理的、または人工知能のロジックを必要とするオブジェクトを定義するクラスです。 さいわい、ゲームのコード ベースであるエンティティには、ゲームの概念オブジェクト多くの場合と一致します。 これが true の場合、ゲームで必要となるエンティティを識別するより簡単に実現できます。 

テーマが適用されたスペースなど、[ゲームを撮影実践](http://en.wikipedia.org/wiki/Shoot_%27em_up)次のエンティティを含めることがあります。

 - `PlayerShip`
 - `EnemyShip`
 - `PlayerBullet`
 - `EnemyBullet`
 - `CollectableItem`
 - `HUD`
 - `Environment`

これらのエンティティは、ゲームの独自のクラスは、各インスタンスのインスタンス化を超えるまったくまたはほとんどのセットアップが必要になります。


## <a name="general-vs-specific-entity-types"></a>型と特定のエンティティ型には、[全般]

どの程度のエンティティを一般化するエンティティ システムを使用してゲームの開発者が直面している最初の質問の 1 つです。 実装の中で最も固有では、いくつかの特性が異なる場合でも、すべての種類、エンティティのためのクラスが定義します。 一般的なシステムはエンティティのグループを 1 つのクラスに結合しをカスタマイズできますのインスタンスを許可します。

たとえば、次のクラスを定義する空間ゲームを想像します。

 - `PlayerDogfighter`
 - `PlayerBomber`
 - `EnemyMissileShip`
 - `EnemyLaserShip`

一般的な方法は、player 付属の 1 つのクラスと別の出荷の種類をサポートするように構成できなかった敵船、1 つのクラスを作成することです。 カスタマイズは、どのイメージを読み込むには、移動の係数を撮影するときに作成する箇条書きエンティティの種類を含めることができ、敵 AI ロジックは付属しています。 ここでは、エンティティの一覧を縮小することがあります。

 - `PlayerShip`
 - `EnemyShip`

もちろん、これらのエンティティ型は、移動を制御するためのインスタンスごとにカスタマイズすることによってさらに一般化することができます。 Player 出荷インスタンスは、敵出荷インスタンス可能性があります AI ロジックの実行中に、入力から読み取らとします。 つまり、エンティティを一般化することが 1 つのクラスに、さらに。

 - `Ship`

汎化は、ゲームはすべてのエンティティの単一の基本クラスを使用することがあります、さらに – 続行できます。 この 1 つのクラス、呼び出される`GameEntity`箇条書きと収集可能な項目 (電源 ups) などの付属されていないエンティティを含む、すべてのエンティティ インスタンス全体のゲームで使用されるクラスになります。

これは、システムのほとんどの一般的な多くの場合と呼びます、*コンポーネント システム*です。 このようなシステムでは、ゲームのエンティティは、物理学、AI などの個々 のコンポーネントを持つことができます。 または動作と外観をカスタマイズする追加のコンポーネントを表示します。 純粋なコンポーネント ベースのシステムでは、究極の柔軟性を有効にして、複雑な継承チェーンなどの継承の使用によって引き起こされる問題を回避できます。 その他の汎化効果的なコンポーネントのシステムを設定するが困難にすることができ、同様のコードの表現力を減らすことができます。

使用汎化のレベルは、さまざまな考慮事項を含むによって異なります。

 - ゲーム サイズ – より小さいゲーム余裕がある大規模なゲームがクラスの数が多いと管理が難しい可能性がありますのときに、特定のクラスを作成します。
 - データ駆動型開発 – データ (イメージ、3 D モデル、および JSON または XML などのデータ ファイル) に依存するゲーム パフォーマンスが向上、エンティティ型を持つ一般化されたデータに基づいて詳細を構成するとします。 これは、開発中またはゲームがリリースされた後に、新しいコンテンツを追加する予定のゲームのインポートに特にです。
 - ゲーム エンジン パターン – 一部のゲーム エンジンを強くお勧めコンポーネントのシステムの使用法のエンティティを整理する方法を決定する開発者は、他のユーザーです。 開発者は、エンティティの任意の型を実装するため、CocosSharp によるコンポーネントのシステムでは、使用法は不要です。 

わかりやすくするため、使用するクラス ベースのアプローチを特定出荷] および [箇条書きの単一のエンティティとこのチュートリアルでは。


## <a name="project-setup"></a>プロジェクトのセットアップ

エンティティの実装を始める前にプロジェクトを作成する必要があります。 使用する CocosSharp プロジェクト テンプレートをプロジェクトの作成を簡略化します。 [この投稿の確認](http://forums.xamarin.com/discussion/26822/cocossharp-project-templates-for-xamarin-studio)については、Visual Studio for Mac テンプレートから CocosSharp プロジェクトを作成します。 このガイドの残りの部分が、プロジェクト名を使用して**EntityProject**です。

プロジェクトを作成した後は、320 x 480 で実行するゲームの解像度を設定します。 これを行うには、呼び出す`CCScene.SetDefaultDesignResolution`で、`GameAppDelegate.ApplicationDidFinishLaunching`メソッドを次のようにします。


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...

    // New code for resolution setting:
    CCScene.SetDefaultDesignResolution(480, 320, CCSceneResolutionPolicy.ShowAll);
    
    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);
    mainWindow.RunWithScene (scene);
} 
```

CocosSharp 解像度を処理する場合の詳細については、次を参照してください。 この[CocosSharp 内の複数の解決を処理ガイド](~/graphics-games/cocossharp/resolutions.md)です。


## <a name="adding-content-to-the-project"></a>プロジェクトにコンテンツを追加します。

含まれるファイルを追加おは、プロジェクトが作成されたら、[このコンテンツの zip ファイル](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)です。 これを行うには、zip ファイルをダウンロードして解凍します。 両方を追加**ship.png**と**bullet.png**を**コンテンツ**フォルダーです。 **コンテンツ**フォルダー内に生成されます、**資産**フォルダーには、Android、iOS にプロジェクトのルートになります。 追加されると、両方のファイルに表示する必要があります、**コンテンツ**フォルダー。

![](entities-images/image2.png "追加されると、両方のファイルがコンテンツのフォルダーにする必要があります。")


## <a name="creating-the-ship-entity"></a>出荷エンティティを作成します。

`Ship`クラスは、ゲームの最初のエンティティになります。 追加する、`Ship`クラス、という名前のフォルダーをまず作成**エンティティ**プロジェクトのルート レベルにします。 新しいクラスを追加、**エンティティ**という名前のフォルダー `Ship`:

![](entities-images/image3.png "出荷をという名前のエンティティ フォルダーで新しいクラスを追加します。")

最初の変更は、`Ship`クラスは継承を知らせるためには、`CCNode`クラスです。 `CCNode` などの一般的な CocosSharp クラスの基底クラスとして機能`CCSprite`と`CCLayer`、次の機能を提供しています。

 - `Position` 画面の周りの出荷を移動するためのプロパティです。
 - `Children` プロパティを追加するため、 `CCSprite.`
 - `Parent` プロパティで、接続に使用できる`Ship`を他のインスタンス`CCNodes`です。 このチュートリアルではこの機能を使用することはありません大規模なゲーム多くの場合、利用を他のエンティティのアタッチの`CCNodes`します。 
 - `AddEventListener` 出荷を移動するための入力に応答する方法。
 - `Schedule` 箇条書きを撮影する方法。

追加しても、`CCSprite`インスタンス化を画面に、出荷を表示できるようにします。


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Ship : CCNode
    {
        CCSprite sprite;

        public Ship () : base()
        {
            sprite = new CCSprite ("ship.png");
            // Center the Sprite in this entity to simplify
            // centering the Ship when it is instantiated
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);
        }
    }
}
```

次に、出荷先を追加、`GameLayer`ゲーム表示します。


```csharp
public class GameLayer : CCLayer
{
    Ship ship;

    public GameLayer ()
    {
        ship = new Ship ();
        ship.PositionX = 240;
        ship.PositionY = 50;
        this.AddChild (ship);
    } 
    ...
```

場合は、出荷エンティティおが表示されます、ゲームを実行します。

![](entities-images/image4.png "ゲームを実行するには、出荷エンティティが表示されます。")


### <a name="why-inherit-from-ccnode-instead-of-ccsprite"></a>なぜ CCSprite ではなく CCNode から継承しますか。

この時点で、`Ship`クラスは、簡単なラッパーで、`CCSprite`インスタンス。 `CCSprite`からも継承`CCNode`から直接継承されたでしたお`CCSprite`、内のコードを削減するよう`Ship.cs`です。 さらに、直接継承`CCSprite`メモリ内のオブジェクトの数を削減し、依存関係ツリーを小さくすることによってパフォーマンスを向上させることができます。

継承したこれらの利点に関係なく`CCNode`いくつかの非表示にする、`CCSprite`各インスタンスのプロパティです。 たとえば、`Texture`以外のプロパティは変更しないで、`Ship`クラス、および継承`CCNode`このプロパティを非表示にすることができます。 エンティティのパブリック メンバーは、ゲームが大きくとその他の開発者がチームに追加されると、特に重要になります。


## <a name="adding-input-to-the-ship"></a>船への入力を追加します。

これで、出荷を画面に表示されている入力を追加する予定です。 当社のアプローチはアプローチで実行されるようになります、 [BouncingGame ガイド](~/graphics-games/cocossharp/bouncing-game.md)、コード内の動きを配置おする点を除いて、`Ship`クラスの親ではなく`CCLayer`または`CCScene`です。

コードを追加して`Ship`ユーザーが画面に触れては任意の場所に移動することをサポートするためにします。


```csharp
public class Ship : CCNode
{
    CCSprite sprite;

    CCEventListenerTouchAllAtOnce touchListener;

    public Ship () : base()
    {
        sprite = new CCSprite ("ship.png");
        // Center the Sprite in this entity to simplify
        // centering the Ship on screen
        sprite.AnchorPoint = CCPoint.AnchorMiddle;
        this.AddChild(sprite);

        touchListener = new CCEventListenerTouchAllAtOnce();
        touchListener.OnTouchesMoved = HandleInput;
        AddEventListener(touchListener, this);

    }

    private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
    {
        if(touches.Count > 0)
        {
            CCTouch firstTouch = touches[0];

            this.PositionX = firstTouch.Location.X;
            this.PositionY = firstTouch.Location.Y;
        } 
    }
} 
```

多く撮影実践ゲームを実装する最大速度になっているため、コント ローラー ベースの従来の動作を模倣します。 ただし、単に短いコードを保持するイミディ エイトの動きを実装します。


## <a name="creating-the-bullet-entity"></a>行頭文字エンティティを作成します。

単純なゲームでは、2 番目のエンティティは、エンティティの行頭文字を表示するためです。 同じように、 `Ship` 、エンティティ、`Bullet`エンティティが含まれます、`CCSprite`画面に表示されないようにします。 移動のためのロジックはという点では、移動; のユーザー入力には依存しません代わりに、`Bullet`インスタンスが速度プロパティを使用して直線的に移動します。

まず追加する新しいクラス ファイル、**エンティティ**フォルダーおよび呼び出し**箇条書き**:

![](entities-images/image5.png "エンティティ フォルダーに新しいクラス ファイルを追加して、行頭文字")

追加すると変更してみます、`Bullet.cs`コードの次のようにします。


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Bullet : CCNode
    {
        CCSprite sprite;

        public float VelocityX
        {
            get;
            set;
        }

        public float VelocityY
        {
            get;
            set;
        }

        public Bullet () : base()
        {
            sprite = new CCSprite ("bullet.png");
            // Making the Sprite be centered makes
            // positioning easier.
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);

            this.Schedule (ApplyVelocity);
        }

        void ApplyVelocity(float time)
        {
            PositionX += VelocityX * time;
            PositionY += VelocityY * time;
        }
    }
} 
```

使用するファイルを変更する場合を除いて、`CCSprite`に`bullet.png`、内のコード`ApplyVelocity`は 2 つの係数に基づいて移動ロジックが含まれています:`VelocityX`と`VelocityY`です。

`Schedule`メソッドでは、フレームごとに呼び出されるデリゲートを追加できるようにします。 ここで追加しています、`ApplyVelocity`メソッドのベロシティ値に従って、行頭に移動できるようにします。 `Schedule`メソッドは、 `Action<float>`、浮動小数点数のパラメーターが最後のフレームは、時間ベースの移動の実装を使用してから経過した時間 (単位は秒) の量を指定します。 秒単位で値を測定した時刻以降、当社ベロシティ値での移動を表します*1 秒あたりのピクセル*です。


## <a name="adding-bullets-to-gamelayer"></a>GameLayer に行頭文字を追加します。

いずれかを追加する前に`Bullet`、ゲームにインスタンスおすると、コンテナー、具体的には、`List<Bullet>`です。 変更、`GameLayer`のため、箇条書きの一覧が含まれています。


```csharp
    public class GameLayer : CCLayer
    {
        Ship ship;
        List<Bullet> bullets;

        public GameLayer ()
        {
            ship = new Ship ();
            ship.PositionX = 240;
            ship.PositionY = 50;
            this.AddChild (ship);

            bullets = new List<Bullet> ();
        }
        ... 
```

次に設定する必要があります、 `Bullet`  ボックスの一覧です。 作成する場合のロジックを`Bullet`に含まれている必要があります、`Ship`エンティティ、ですが、`GameLayer`は箇条書きの一覧を格納します。 ファクトリ パターンを使用できるようにして、`Ship`を作成するエンティティ`Bullet`インスタンス。 このファクトリはイベントを公開する、`GameLayer`を処理できます。 

これを行うにまず追加のフォルダーと呼ばれるプロジェクトに**ファクトリ**という新しいクラスを追加および`BulletFactory`:

![](entities-images/image6.png "ファクトリと呼ばれるプロジェクトにフォルダーを追加し、BulletFactory という新しいクラスを追加")

次に、実装、`BulletFactory`シングルトン クラス。


```csharp
using System;

namespace EntityProject
{
    public class BulletFactory
    {
        static Lazy<BulletFactory> self = 
            new Lazy<BulletFactory>(()=>new BulletFactory());

        // simple singleton implementation
        public static BulletFactory Self
        {
            get
            {
                return self.Value;
            }
        }

        public event Action<Bullet> BulletCreated;

        private BulletFactory()
        {

        }

        public Bullet CreateNew()
        {
            Bullet newBullet = new Bullet ();

            if (BulletCreated != null)
            {
                BulletCreated (newBullet);
            }

            return newBullet;
        }
    }
} 
```

`Ship`エンティティを作成する処理`Bullet`インスタンス – 具体的には、処理する頻度`Bullet`(つまり、どのくらいの頻度の項目が発生した)、インスタンスを作成する必要があります、それらの位置と、速度。

変更、`Ship`新しいを追加するエンティティのコンス トラクター`Schedule`を呼び出すには、次のようにこのメソッドを実装します。


```csharp
...
public Ship () : base()
{
    sprite = new CCSprite ("ship.png");
    // Center the Sprite in this entity to simplify
    // centering the Ship on screen
    sprite.AnchorPoint = CCPoint.AnchorMiddle;
    this.AddChild(sprite);

    touchListener = new CCEventListenerTouchAllAtOnce();
    touchListener.OnTouchesMoved = HandleInput;
    AddEventListener(touchListener, this);

    Schedule (FireBullet, interval: 0.5f);

}

void FireBullet(float unusedValue)
{
    Bullet newBullet = BulletFactory.Self.CreateNew ();
    newBullet.Position = this.Position;
    newBullet.VelocityY = 100;
} 
...
```

最後に、新規の作成を処理する`Bullet`のインスタンスにある、`GameLayer`コード。 イベント ハンドラーを追加、`BulletCreated`新しく作成された追加イベント`Bullet`適切なリストに。


```csharp
...
public GameLayer ()
{
    ship = new Ship ();
    ship.PositionX = 240;
    ship.PositionY = 50;
    this.AddChild (ship);

    bullets = new List<Bullet> ();
    BulletFactory.Self.BulletCreated += HandleBulletCreated;
}

void HandleBulletCreated(Bullet newBullet)
{
    AddChild (newBullet);
    bullets.Add (newBullet);
}
... 
```

これで、ゲームを実行して、参照してください、`Ship`撮影`Bullet`インスタンス。

![](entities-images/image1.png "ゲームを実行し、出荷が行頭文字インスタンスを撮影します。")


## <a name="why-gamelayer-has-ship-and-bullets-members"></a>GameLayer が出荷および箇条書きのメンバーを持つ理由

`GameLayer`クラスは、エンティティ インスタンスへの参照を保持するために 2 つのフィールドを定義 (`ship`と`bullets`)、それらに何も行いません。 さらに、エンティティは、移動やトラブルシューティングなど、独自の動作を担当します。 したがって理由でした追加`ship`と`bullets`フィールドを`GameLayer`しますか?

これらのメンバーを追加したためです完全ゲームの実装のロジックが必要になります、`GameLayer`別のエンティティ間の対話にします。 たとえば、プレーヤーで破壊することができます敵を含めるこのゲームをさらに開発可能性があります。 これら敵に含まれている、`List`で、 `GameLayer`、およびロジックをテストするかどうか`Bullet`と競合するインスタンス敵はで実行されます、`GameLayer`もします。 言い換えると、`GameLayer`ルート*所有者*すべてのエンティティのインスタンス、およびそれがエンティティ インスタンス間の相互作用を担当します。


## <a name="bullet-destruction-considerations"></a>箇条書きの破棄に関する考慮事項

ゲームには、コードの破棄を現在がない`Bullet`インスタンス。 各`Bullet`インスタンスには、画面上を移動するためのロジックが、いずれかの画面の範囲外に破棄するためのコードを追加していません`Bullet`インスタンス。

さらに、破棄`Bullet`インスタンス可能性がありますに属していない`GameLayer`です。 画面外となるときに、破棄されるのではなく、たとえば、`Bullet`エンティティ自体を一定の時間後に破棄するためのロジックがあります。 ここで、`Bullet`を破棄する必要がありますを通信する方法が必要、 `GameLayer`、よく似て、`Ship`に伝達されるエンティティ、`GameLayer`を新しい`Bullet`で作成された、`BulletFactory`です。

最も簡単なソリューションでは、破棄をサポートするためにファクトリ クラスの役割を展開します。 破棄されているエンティティのインスタンスなど、他のオブジェクトを処理するの工場出荷時の通知を受け取ることができ、`GameLayer`その一覧からエンティティのインスタンスを削除します。 

## <a name="summary"></a>まとめ

このガイドを継承して CocosSharp エンティティを作成する方法を示しています、`CCNode`クラスです。 これらのエンティティは、自己完結型のオブジェクトの独自のビジュアルとカスタム ロジックの作成を処理します。 このガイドでは、ルート エンティティ コンテナー (衝突およびその他のエンティティの相互作用ロジック) に属しているコードからエンティティ (移動し、その他のエンティティの作成) の内部が所属するコードを指定します。

## <a name="related-links"></a>関連リンク

- [CocosSharp API ドキュメント](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Zip コンテンツ](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)
