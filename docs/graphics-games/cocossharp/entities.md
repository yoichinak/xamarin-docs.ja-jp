---
title: CocosSharp のエンティティ
description: エンティティのパターンは、ゲームのコードを整理する強力な方法です。 読みやすく、コードを簡単に維持するには、親/子の組み込み機能を活用しているとします。
ms.prod: xamarin
ms.assetid: 1D3261CE-AC96-4296-8A53-A76A42B927A8
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 6445d595c9d8ca47e187fdcd158cd5a801a96407
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103206"
---
# <a name="entities-in-cocossharp"></a>CocosSharp のエンティティ

_エンティティのパターンは、ゲームのコードを整理する強力な方法です。読みやすく、コードを簡単に維持するには、親/子の組み込み機能を活用しているとします。_

エンティティのパターンでは、改善されたコードの組織で CocosSharp による開発者の作業を向上できます。 このチュートリアルは、– 出荷エンティティと箇条書きエンティティの 2 つのエンティティを作成する方法を示す実践的な例になります。 これらのエンティティは自己完結型のオブジェクトをあるは 1 回インスタンス化です。 つまり、自動的に表示されると、型の適切な移動ロジックを実行します。 

このガイドでは、次のトピックについて説明します。

 - ゲームのエンティティの概要
 - 型と特定のエンティティ型には、[全般]
 - プロジェクトのセットアップ
 - エンティティ クラスを作成します。
 - エンティティ インスタンスを追加します `GameLayer`
 - エンティティのロジックへの対応、 `GameLayer`

完成したゲームは、次のようになります。

![](entities-images/image1.png "完成したゲームのようになります")


## <a name="introduction-to-game-entities"></a>ゲームのエンティティの概要

ゲームのエンティティは、レンダリング、衝突、物理学や人工知能のロジックを必要とするオブジェクトを定義するクラスです。 さいわい、ゲームのコード ベースに存在するエンティティには、ゲームの概念オブジェクト多くの場合と一致します。 これが true の場合、ゲームに必要なエンティティを識別するより簡単に実現できます。 

テーマが適用されたスペースなど、[ゲームをやって撮影](http://en.wikipedia.org/wiki/Shoot_%27em_up)次のエンティティを含めることができます。

 - `PlayerShip`
 - `EnemyShip`
 - `PlayerBullet`
 - `EnemyBullet`
 - `CollectableItem`
 - `HUD`
 - `Environment`

これらのエンティティがゲームでは、独自のクラスになり、各インスタンスのインスタンス化を超えるほとんどまたはまったくのセットアップが必要になります。


## <a name="general-vs-specific-entity-types"></a>型と特定のエンティティ型には、[全般]

システム エンティティを使用してゲームの開発者が直面している最初の質問の 1 つは、エンティティを一般化する量です。 いくつかの特性が異なる場合でも、最も具体的な実装は、エンティティの種類ごとにクラスを定義します。 一般的なシステムは、エンティティのグループを 1 つのクラスに結合をカスタマイズするインスタンスを許可します。

たとえば、次のクラスを定義する領域のゲームを想像します。

 - `PlayerDogfighter`
 - `PlayerBomber`
 - `EnemyMissileShip`
 - `EnemyLaserShip`

一般的なアプローチは、player 船の 1 つのクラスと敵の船が別の出荷の種類をサポートするように構成が 1 つのクラスを作成することです。 カスタマイズは、どのイメージを読み込むには、作成、移動の係数を撮影するときに行頭文字エンティティの種類を含めることができ、敵の AI のロジックが用意されています。 この場合にエンティティの一覧を縮小可能性があります。

 - `PlayerShip`
 - `EnemyShip`

もちろん、これらのエンティティ型は、移動を制御するためのインスタンスごとのカスタマイズを許可することでさらに一般化することができます。 Player 出荷インスタンスは入力から読み取られますが宇宙船が敵のインスタンスは、AI ロジックを実行可能性があります。 つまり、エンティティを一般化することが 1 つのクラスにさらに発展させる。

 - `Ship`

汎化は、ゲームはすべてのエンティティの 1 つの基本クラスを使用することがあります –、さらに続行できます。 呼び出すことはこの 1 つのクラス`GameEntity`箇条書きと収集可能な項目 (パワーアップ) などに同梱されていないエンティティを含む、ゲーム全体ですべてのエンティティ インスタンスに使用されるクラスになります。

これは、システムのほとんどの一般的な多くの場合と呼びます、*コンポーネント システム*します。 このようなシステムでは、ゲーム エンティティは、物理学、AI などの個々 のコンポーネントを持つことができます。 または動作と外観をカスタマイズする追加のコンポーネントをレンダリングします。 純粋なコンポーネント ベースのシステムでは、柔軟性を有効にし、複雑な継承チェーンなどの継承の使用によって発生する問題を回避できます。 同様に他の汎化、有効なコンポーネントのシステムを設定するには難しいコードの表現力を減らすことができます。

使用汎化のレベルなど、さまざまな考慮事項によって異なります。

 - ゲームのサイズ-小さなゲームにクラスの数が多いと管理が難しい大規模なゲームがあります、特定のクラスを作成する余裕があることができます。
 - データ駆動型の開発 – データ (イメージ、3 D モデル、および JSON や XML などのデータ ファイル) に依存するゲームは、エンティティ型が汎用化されてから利用可能性があり、データに基づいて詳細を構成します。 これは、特にゲームを開発中、またはゲームが解放された後に、新しいコンテンツを追加する予定のためにインポートします。
 - ゲーム エンジン パターン – 一部のゲーム エンジンを強くお勧めコンポーネントのシステムの使用状況のエンティティを整理する方法を決定する開発者は、他のユーザー。 CocosSharp では開発者がエンティティの任意の型を実装するため、システム コンポーネントの使用量は必要ありません。 

わかりやすく、使用する特定のクラス ベースのアプローチと船と箇条書きの 1 つのエンティティでこのチュートリアルでは。


## <a name="project-setup"></a>プロジェクトのセットアップ

このエンティティの実装を始める前にプロジェクトを作成する必要があります。 使用する、CocosSharp プロジェクト テンプレート プロジェクトの作成を簡略化します。 [この投稿を確認してください。](http://forums.xamarin.com/discussion/26822/cocossharp-project-templates-for-xamarin-studio)については、Visual Studio for Mac テンプレートから CocosSharp プロジェクトを作成します。 このガイドの残りの部分が、プロジェクト名を使用して**EntityProject**します。

このプロジェクトが作成されたら、480 x 320 で実行する、ゲームの解像度を設定します。 これを行うには、呼び出す`CCScene.SetDefaultDesignResolution`で、`GameAppDelegate.ApplicationDidFinishLaunching`メソッドとして、次のとおりです。


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...

    // New code for resolution setting:
    CCScene.SetDefaultDesignResolution(480, 320, CCSceneResolutionPolicy.ShowAll);
    
    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);
    mainWindow.RunWithScene (scene);
} 
```

CocosSharp の解像度で処理する方法の詳細については、次を参照してください。 この[CocosSharp での複数の解決を処理ガイド](~/graphics-games/cocossharp/resolutions.md)します。


## <a name="adding-content-to-the-project"></a>プロジェクトにコンテンツを追加します。

含まれるファイルは追加、プロジェクトが作成されたら、[このコンテンツの zip ファイル](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)します。 これを行うには、zip ファイルをダウンロードして解凍します。 両方を追加する**ship.png**と**bullet.png**を**コンテンツ**フォルダー。 **コンテンツ**フォルダー内に生成されます、**資産**フォルダーには、Android、ios プロジェクトのルートになります。 追加するには、両方のファイルに表示する必要があります、**コンテンツ**フォルダー。

![](entities-images/image2.png "両方のファイルがコンテンツのフォルダーに配置する必要があります追加されると、")


## <a name="creating-the-ship-entity"></a>出荷のエンティティを作成します。

`Ship`クラスは、ゲームの最初のエンティティになります。 追加する、`Ship`クラスは、まずという名前のフォルダーを作成**エンティティ**プロジェクトのルート レベルにします。 新しいクラスを追加、**エンティティ**という名前のフォルダー `Ship`:

![](entities-images/image3.png "出荷をという名前のエンティティ フォルダーに新しいクラスを追加します。")

最初の変更を行う、`Ship`クラスは継承を知らせるためには、`CCNode`クラス。 `CCNode` などの共通の CocosSharp クラスの基底クラスとして機能`CCSprite`と`CCLayer`、次の機能を提供しています。

 - `Position` 画面の周りの出荷を移動するためのプロパティです。
 - `Children` プロパティを追加するため、 `CCSprite.`
 - `Parent` プロパティをアタッチするために使用できる`Ship`他のインスタンス`CCNodes`します。 このチュートリアルでこの機能を使用することはありません。大規模なゲーム多くの場合、利用を他のエンティティをアタッチした`CCNodes`します。 
 - `AddEventListener` 宇宙船を移動するための入力に応答する方法。
 - `Schedule` 箇条書きを撮影するためのメソッド。

追加します、`CCSprite`インスタンスの画面で、出荷を表示できます。


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Ship : CCNode
    {
        CCSprite sprite;

        public Ship () : base()
        {
            sprite = new CCSprite ("ship.png");
            // Center the Sprite in this entity to simplify
            // centering the Ship when it is instantiated
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);
        }
    }
}
```

次に、送付先を追加、`GameLayer`ゲームに表示を確認します。


```csharp
public class GameLayer : CCLayer
{
    Ship ship;

    public GameLayer ()
    {
        ship = new Ship ();
        ship.PositionX = 240;
        ship.PositionY = 50;
        this.AddChild (ship);
    } 
    ...
```

場合は、出荷エンティティわかりますが、ゲームを実行します。

![](entities-images/image4.png "ゲームを実行するときに、出荷のエンティティが表示されます。")


### <a name="why-inherit-from-ccnode-instead-of-ccsprite"></a>なぜ CCSprite ではなく CCNode から継承しますか。

この時点で、`Ship`クラスは、簡単なラッパーを`CCSprite`インスタンス。 `CCSprite`からも継承`CCNode`から直接継承したでした`CCSprite`、内のコードを削減するは`Ship.cs`します。 さらに、直接継承`CCSprite`メモリ内のオブジェクトの数が減るし、依存関係ツリーを小さくすることによってパフォーマンスを向上させることができます。

継承したこれらの利点に関係なく`CCNode`の一部を非表示にする、`CCSprite`各インスタンスからのプロパティ。 たとえば、`Texture`以外のプロパティを変更しないでください、`Ship`クラスし、継承`CCNode`により、このプロパティを非表示にします。 エンティティのパブリック メンバーは、ゲームの増加に合わせてとその他の開発者がチームに追加されるときに特に重要になります。


## <a name="adding-input-to-the-ship"></a>配送先への入力を追加します。

これで、宇宙船が画面に表示の入力を追加する予定です。 私たちのアプローチで使用される手法ようになります、 [BouncingGame ガイド](~/graphics-games/cocossharp/bouncing-game.md)における移動のコードを配置したことを除いて、`Ship`クラスではなく、次を含む`CCLayer`または`CCScene`します。

コードを追加して`Ship`ユーザーが画面に触れては任意の場所に移動することをサポートするために。


```csharp
public class Ship : CCNode
{
    CCSprite sprite;

    CCEventListenerTouchAllAtOnce touchListener;

    public Ship () : base()
    {
        sprite = new CCSprite ("ship.png");
        // Center the Sprite in this entity to simplify
        // centering the Ship on screen
        sprite.AnchorPoint = CCPoint.AnchorMiddle;
        this.AddChild(sprite);

        touchListener = new CCEventListenerTouchAllAtOnce();
        touchListener.OnTouchesMoved = HandleInput;
        AddEventListener(touchListener, this);

    }

    private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
    {
        if(touches.Count > 0)
        {
            CCTouch firstTouch = touches[0];

            this.PositionX = firstTouch.Location.X;
            this.PositionY = firstTouch.Location.Y;
        } 
    }
} 
```

多く撮影やってゲームの実装を最大速度は、従来のコント ローラー ベースの動きを模倣します。 ただし、即時の移動、コードを短くするために単純に実装します。


## <a name="creating-the-bullet-entity"></a>行頭文字エンティティを作成します。

単純なゲームでは、2 番目のエンティティは、行頭文字を表示するエンティティです。 同じように、`Ship`エンティティ、`Bullet`エンティティには、`CCSprite`画面に表示されるようにします。 移動; のユーザー入力には依存しませんが、移動のためのロジックとは異なる代わりに、`Bullet`インスタンスは直線速度プロパティを使用して移動します。

最初に新しいクラス ファイルを追加します、**エンティティ**フォルダー、呼び出す**箇条書き**:

![](entities-images/image5.png "エンティティ フォルダーに新しいクラス ファイルを追加し、行頭文字を付けます")

変更します。 追加すると、`Bullet.cs`コードの次のようにします。


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Bullet : CCNode
    {
        CCSprite sprite;

        public float VelocityX
        {
            get;
            set;
        }

        public float VelocityY
        {
            get;
            set;
        }

        public Bullet () : base()
        {
            sprite = new CCSprite ("bullet.png");
            // Making the Sprite be centered makes
            // positioning easier.
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);

            this.Schedule (ApplyVelocity);
        }

        void ApplyVelocity(float time)
        {
            PositionX += VelocityX * time;
            PositionY += VelocityY * time;
        }
    }
} 
```

使用しているファイルを変更するとは別に、`CCSprite`に`bullet.png`、コードでは、`ApplyVelocity`は 2 つの係数に基づいて移動ロジックが含まれています:`VelocityX`と`VelocityY`します。

`Schedule`メソッドでは、フレームごとに呼び出されるデリゲートを追加できるようにします。 ここで追加しています、`ApplyVelocity`メソッドのベロシティ値に従って、行頭に移動できるようにします。 `Schedule`メソッドは、 `Action<float>`、float 型のパラメーターが時間ベースの移動の実装を使用して、最後のフレームからの秒単位で時間の量を指定します。 以来、値は秒単位で測定し、ベロシティ値での移動を表す*1 秒あたりのピクセル*します。


## <a name="adding-bullets-to-gamelayer"></a>箇条書き GameLayer を追加

いずれかを追加する前に`Bullet`、ゲームにインスタンスいたしますコンテナーであり、具体的には、`List<Bullet>`します。 変更、`GameLayer`のため、箇条書きのリストが含まれています。


```csharp
    public class GameLayer : CCLayer
    {
        Ship ship;
        List<Bullet> bullets;

        public GameLayer ()
        {
            ship = new Ship ();
            ship.PositionX = 240;
            ship.PositionY = 50;
            this.AddChild (ship);

            bullets = new List<Bullet> ();
        }
        ... 
```

次に設定する必要があります、`Bullet`一覧。 ロジックを作成する場合は、`Bullet`で含める必要があります、 `Ship` 、エンティティが、`GameLayer`は箇条書きのリストを格納する責任を負います。 ファクトリ パターンは使用できるようにしました、`Ship`を作成するエンティティ`Bullet`インスタンス。 このファクトリはイベントを公開する、`GameLayer`を処理できます。 

これを行うに最初に追加しますフォルダーと呼ばれるプロジェクト**ファクトリ**という新しいクラスを追加し`BulletFactory`:

![](entities-images/image6.png "ファクトリをという名前のプロジェクトにフォルダーを追加し、BulletFactory という新しいクラスを追加")

次に、実装、`BulletFactory`シングルトン クラス。


```csharp
using System;

namespace EntityProject
{
    public class BulletFactory
    {
        static Lazy<BulletFactory> self = 
            new Lazy<BulletFactory>(()=>new BulletFactory());

        // simple singleton implementation
        public static BulletFactory Self
        {
            get
            {
                return self.Value;
            }
        }

        public event Action<Bullet> BulletCreated;

        private BulletFactory()
        {

        }

        public Bullet CreateNew()
        {
            Bullet newBullet = new Bullet ();

            if (BulletCreated != null)
            {
                BulletCreated (newBullet);
            }

            return newBullet;
        }
    }
} 
```

`Ship`エンティティの作成を処理する`Bullet`のインスタンス、具体的には、処理する頻度`Bullet`(つまりどのくらいの頻度、箇条書きが起動される) インスタンスを作成する必要があります、それらの位置とその速度。

変更、`Ship`新しいを追加するエンティティのコンス トラクター`Schedule`を呼び出すし、次のようにこのメソッドを実装します。


```csharp
...
public Ship () : base()
{
    sprite = new CCSprite ("ship.png");
    // Center the Sprite in this entity to simplify
    // centering the Ship on screen
    sprite.AnchorPoint = CCPoint.AnchorMiddle;
    this.AddChild(sprite);

    touchListener = new CCEventListenerTouchAllAtOnce();
    touchListener.OnTouchesMoved = HandleInput;
    AddEventListener(touchListener, this);

    Schedule (FireBullet, interval: 0.5f);

}

void FireBullet(float unusedValue)
{
    Bullet newBullet = BulletFactory.Self.CreateNew ();
    newBullet.Position = this.Position;
    newBullet.VelocityY = 100;
} 
...
```

最後の手順は、新規の作成を処理するためには`Bullet`インスタンス、`GameLayer`コード。 イベント ハンドラーを追加、 `BulletCreated` 、新しく作成された追加イベント`Bullet`に適切なリスト。


```csharp
...
public GameLayer ()
{
    ship = new Ship ();
    ship.PositionX = 240;
    ship.PositionY = 50;
    this.AddChild (ship);

    bullets = new List<Bullet> ();
    BulletFactory.Self.BulletCreated += HandleBulletCreated;
}

void HandleBulletCreated(Bullet newBullet)
{
    AddChild (newBullet);
    bullets.Add (newBullet);
}
... 
```

これで、ゲームを実行して参照してください、`Ship`撮影`Bullet`インスタンス。

![](entities-images/image1.png "ゲームを実行し、出荷を撮影は箇条書きのインスタンス")


## <a name="why-gamelayer-has-ship-and-bullets-members"></a>GameLayer の出荷と箇条書きのメンバーがなぜ

`GameLayer`クラスは、エンティティ インスタンスへの参照を保持するために 2 つのフィールドを定義します (`ship`と`bullets`)、それらに何も行いません。 さらに、エンティティは、移動、シュートなど、独自の動作を担当します。 その理由が追加`ship`と`bullets`フィールドを`GameLayer`でしょうか。

これらのメンバーを追加しました理由は、完全なゲームの実装のロジックが必要になります、`GameLayer`さまざまなエンティティ間の対話します。 たとえば、プレーヤーは破棄敵を含めるこのゲームをさらに開発可能性があります。 これらの敵に格納されて、`List`で、`GameLayer`とロジックをテストするかどうか`Bullet`と衝突するインスタンスで実行される、敵、`GameLayer`も。 つまり、`GameLayer`ルート*所有者*すべてのエンティティのインスタンス、およびその担当エンティティ インスタンス間の相互作用します。


## <a name="bullet-destruction-considerations"></a>箇条書きの破棄に関する考慮事項

ゲームを破棄するためのコードを現在がない`Bullet`インスタンス。 各`Bullet`インスタンスが画面を移動するためのロジックが、いずれかを画面外に破棄するためのコードを追加していない`Bullet`インスタンス。

さらに、破棄`Bullet`でインスタンスが属していない可能性があります`GameLayer`します。 外となるときに、破棄されるのではなく、たとえば、`Bullet`エンティティ自体を一定の時間後に破棄するためのロジックがあります。 ここで、`Bullet`を破棄する必要がありますと通信する手段が必要、`GameLayer`同様、`Ship`にエンティティが伝達、`GameLayer`を新しい`Bullet`使用して作成された、`BulletFactory`します。

最も単純なソリューションでは、破棄をサポートするために、ファクトリ クラスの役割を展開します。 などの他のオブジェクトで処理できますが、破棄されているエンティティ インスタンスの工場出荷時の通知を受け取ることができ、`GameLayer`その一覧からエンティティのインスタンスを削除します。 

## <a name="summary"></a>まとめ

このガイドから継承することで CocosSharp のエンティティを作成する方法を示します、`CCNode`クラス。 これらのエンティティは、自己完結型のオブジェクトの独自のカスタム ロジックとビジュアルの作成を処理します。 このガイドでは、ルート エンティティ コンテナー (衝突とその他のエンティティの相互作用ロジック) に属しているコードからコード内でのエンティティ (移動と、その他のエンティティの作成) が属しているを指定します。

## <a name="related-links"></a>関連リンク

- [CocosSharp API ドキュメント](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [コンテンツの zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)
