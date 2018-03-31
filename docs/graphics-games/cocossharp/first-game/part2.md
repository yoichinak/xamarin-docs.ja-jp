---
title: 跳ね返りゲームを実装します。
description: このチュートリアルでは、ゲーム ロジックとコンテンツを BouncingGame と呼ばれるフル ゲームを作成する空のテンプレートに追加する方法を示します。
ms.topic: article
ms.prod: xamarin
ms.assetid: AC9FD56F-6E4A-40DA-8168-45A761D869FD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 584ae03a3a773ae7e16fa7a24b9dbfa6c5056342
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2018
---
# <a name="implementing-the-bouncing-game"></a>跳ね返りゲームを実装します。

_このチュートリアルでは、ゲーム ロジックとコンテンツを BouncingGame と呼ばれるフル ゲームを作成する空のテンプレートに追加する方法を示します。_

このチュートリアルの前の部分では、iOS と Android の開発用のプロジェクト含む CocosSharp ソリューションを作成する方法を示しました。 このガイドは、次をカバーするフルのゲームを実装することで続行されます。

 - 解凍、ゲームの内容
 - 一般的な CocosSharp 視覚要素
 - ビジュアル要素を追加します。 `GameLayer`
 - フレームごとのロジックを実装します。

完成したゲームは、次のようになります。

![](part2-images/image1.png "完成したゲームのようになります")


# <a name="unzipping-our-game-content"></a>解凍、ゲームの内容

ゲームの開発者は多くの場合、用語を使用して*コンテンツ*には、通常 visual アーティスト、ゲームのデザイナーやオーディオ デザイナーによって作成されるコード以外のファイルを参照してください。 一般的なコンテンツの種類には、ビジュアルを表示、音を再生または人工知能 (AI) の動作を制御するために使用するファイルが含まれます。 ゲームの開発チームの観点から、コンテンツは通常プログラマ以外が作成されます。 ゲームには、2 種類コンテンツにはが含まれています。

 - PNG ファイル、ボールとパドルの表示方法を定義します。
 - 1 つの XNB ファイル (以下、スコア表示を追加する際は、さらに詳しく説明します)、スコアの表示で使用されるフォントを定義するには

ここで使用されるこのコンテンツは含まれて[zip コンテンツ](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)です。 これらのファイルをダウンロードしてこのチュートリアルの後半にアクセスできる場所に解凍する必要があります。


# <a name="common-cocossharp-visual-elements"></a>共通の CocosSharp Visual 要素

CocosSharp は、さまざまなビジュアルを表示するために使用するクラスを提供します。 いくつかの要素は、組織に使用されるものの中に直接表示されます。 このゲームでは、次を使用します。

 - `CCNode` – CocosSharp 内のすべてのビジュアル オブジェクト基本クラスです。 `CCNode`クラスに含まれる、 `AddChild` 、親子階層を構築するために使用できる関数とも呼ば、*ビジュアル ツリー*または*シーン グラフ*です。 以下に言及するすべてのクラスを継承`CCNode`です。
 - `CCScene` – すべての CocosSharp ゲームのビジュアル ツリーのルートです。 すべてのビジュアル要素のビジュアル ツリーの一部をする必要があります、`CCScene`ルート ディレクトリにあるか、表示されません。
 - `CCLayer` – などのビジュアルのコンテナー オブジェクト`CCSprite`です。 名前が示すように、`CCLayer`クラスは相互にどのようにビジュアルの要素層の制御に使用します。
 - `CCSprite` – イメージまたはイメージの一部を表示します。 `CCSprite` インスタンス配置でき、サイズ変更、さまざまな視覚効果を提供します。
 - `CCLabel` -画面に文字列を表示します。 使用されるフォント`CCLabel`XNB ファイルで定義されています。 詳細については、次に、XNBs について説明します。

さまざまな種類の使用方法を理解しておく 1 つを検討してみましょう`CCSprite`です。 ビジュアル要素を追加する必要があります、 `CCLayer`、ビジュアル ツリーである必要があります、`CCScene`ルートの位置。 そのため、親/子リレーションシップを 1 つの`CCSprite`なります`CCScene`  >  `CCLayer`  > `CCSprite`です。


# <a name="adding-visual-elements-to-gamelayer"></a>GameLayer ビジュアル要素を追加します。


## <a name="adding-the-paddlesprite"></a>PaddleSprite を追加します。

最初に黒を 1 つ追加するゲームの背景設定`CCSprite`画面に表示します。 変更、`GameLayer`を追加するクラス、`CCSprite`次のようにします。


```csharp
using System;
using System.Collections.Generic;
using CocosSharp;
namespace BouncingGame
{
    public class GameLayer : CCLayer
    {
        CCSprite paddleSprite;
        public GameLayer () : base(CCColor4B.Black)
        {
            // "paddle" refers to the paddle.png image
            paddleSprite = new CCSprite ("paddle");
            paddleSprite.PositionX = 100;
            paddleSprite.PositionY = 100;
            AddChild (paddleSprite);
        }
        protected override void AddedToScene ()
        {
            base.AddedToScene ();
            // Use the bounds to layout the positioning of our drawable assets
            CCRect bounds = VisibleBoundsWorldspace;
            // Register for touch events
            var touchListener = new CCEventListenerTouchAllAtOnce ();
            touchListener.OnTouchesEnded = OnTouchesEnded;
            AddEventListener (touchListener, this);
        }
        void OnTouchesEnded (List<CCTouch> touches, CCEvent touchEvent)
        {
            if (touches.Count > 0)
            {
                // Perform touch handling here
            }
        }
    }
}
```

上記のコードは、1 つを作成`CCSprite`の子として追加し、`GameLayer`です。 `CCSprite`コンス トラクターでは、文字列として使用するイメージ ファイルを定義します。 コードに指示をという名前のファイルを探して CocosSharp `paddle` (拡張機能は省略すると、このガイドの後半で説明する)。 CocosSharp はそのファイル名を探します`paddle`ルート コンテンツ フォルダー内 (これは**コンテンツ**) に追加された任意のフォルダーだけでなく、 `gameView.ContentManager.SearchPaths` (前のセクションで説明しています)。

ファイルのルートに直接追加**コンテンツ**iOS および Android の両方のフォルダーです。 右クリックまたはコントロールをクリック、**コンテンツ**フォルダーをクリックし、iOS プロジェクトで**追加** > **ファイルを追加しています.**以前でコンテンツを解凍おに移動して選択**paddle.png**です。 フォルダーにファイルを追加する方法の場合を選択する必要があります、**コピー**オプション。

![](part2-images/image2.png "フォルダーにファイルを追加する方法の場合、[コピー] オプションを選択します。")

次に、Android のプロジェクトにファイルを追加します。 右クリックまたはコントロールの [コンテンツのフォルダー (である、**資産**Android プロジェクトのフォルダー)] を選択して、**追加** > **ファイルを追加しています.**.このとき、' éˆú"iOS プロジェクトの**コンテンツ**フォルダーです。 ファイルを追加する方法について確認するメッセージが表示されたら、選択、**リンクを追加する**オプション。

![](part2-images/addalink.png "ファイルを追加する方法について確認するメッセージが表示されたら、追加のリンクのオプション を選択します。")

ファイルの次の 2 つのプロジェクトに追加する必要がある理由について説明します。 各プロジェクトの**コンテンツ**フォルダーが含まれています、 **paddle.png**ファイル。

![](part2-images/image3.png "各プロジェクトのコンテンツ フォルダーには、paddle.png ファイルが含まれています")

場合は、ゲームを実行お会いしましょう、`CCSprite`描画されます。

![](part2-images/image4.png "CCSprite が描画される場合は、ゲームを実行すると、")


## <a name="file-details"></a>ファイルの詳細

これまでに、プロジェクトに 1 つのファイルが追加されましたし、プロセスの実行がきわめて簡単です。 単純に追加される、 **paddle.png**ファイルの名前を**コンテンツ**フォルダーで、コードで参照します。 CocosSharp でファイルを操作するときに、いくつかの考慮事項を見てみましょう。

### <a name="capitalization"></a>[大文字/小文字の設定]

ファイル名およびファイルへのアクセス コードで使用した文字列は、小文字の両方でことに注意してください。 Windows デスクトップおよび iOS simulator) などの一部のプラットフォームでは大文字小文字を区別しない、他のプラットフォーム (Android および iOS デバイス) などは大文字小文字を区別するためです。 クロス プラットフォームとして、ファイルとコードが維持されるよう、小文字のすべてのファイルを使用して、このチュートリアルの残りの部分がおできるだけです。

### <a name="extensions"></a>拡張機能

スプライトを作成するためのコンス トラクターでは、パドル ファイルを参照するときに、".png"の拡張子は含まれません。 プラットフォーム間で異なる場合があります、同じアセットの種類のファイル拡張子として CocosSharp プロジェクトで作業場合、通常はファイルの拡張子を省略します。 たとえば、オーディオ ファイルについては、プラットフォームに応じて .aiff、.mp3、または .wma ファイル形式があります。 拡張機能をオフに残しておくと、同じコードをファイル拡張子に関係なくすべてのプラットフォームで動作ができます。

### <a name="content-in-platform-specific-projects"></a>プラットフォーム固有のプロジェクト内のコンテンツ

ほとんどのコード ファイルに含めることがあるのとは異なり、PCL、コンテンツ ファイルは、各プラットフォームに固有のプロジェクトに追加する必要があります。 CocosSharp には、2 つの理由これが必要です。

1. 各プラットフォームには異なる**ビルド アクション**です。 IOS のプロジェクトに追加されたコンテンツを使用する必要があります、 **BundleResource**ビルド アクション。 Android プロジェクトに追加されたコンテンツを使用する必要があります、 **AndroidAsset**ビルド アクション。
2. 一部のプラットフォームでは、プラットフォーム固有のファイル形式が必要です。 たとえば、フォント .xnb ファイルは、このチュートリアルで後ほどおわかりのよう iOS および Android での間で異なります。

ファイル形式は、(.png) などのプラットフォーム間で違いはない場合、各プラットフォームは 1 つまたは複数のプラットフォームが同じ場所からファイルをリンクは、同じファイルを使用できます。

# <a name="orientation"></a>[方向]

その他のすべてのアプリと同じように、CocosSharp アプリは、縦または横の向きで実行できます。 縦専用モードで実行するゲームを調整します。 最初に、縦縦横比を処理するゲーム解決コードを変更しています。 これを行うには、次のように変更します。、`width`と`height`の値が、`LoadGame`メソッドで、 `ViewController` iOS でのクラスと`MainActivity`Android でのクラス。


```csharp
void LoadGame (object sender, EventArgs e)
{
    CCGameView gameView = sender as CCGameView;

    if (gameView != null)
    {
        var contentSearchPaths = new List<string> () { "Fonts", "Sounds" };
        CCSizeI viewSize = gameView.ViewSize;

        int width = 768;
        int height = 1027;
    ...
```

次に縦モードで実行する各プラットフォームに固有のプロジェクトを変更する必要があります。


## <a name="ios-orientation"></a>iOS の向き

IOS プロジェクトの向きを変更するには、選択、 **Info.plist**ファイルで、 **BouncingGame.iOS**プロジェクト、および変更**展開情報の Iphone/ipod**と、 **iPad 展開情報**縦向きだけを。

![](part2-images/image5.png "展開情報の Iphone/ipod および iPad だけを縦向きの展開情報を変更します。")


## <a name="android-orientation"></a>Android の向き

Android プロジェクトの向きを変更するには、BouncingGame.Android プロジェクトで MainActivity.cs ファイルを開きます。 次のように縦向きのみを指定するため、アクティビティの属性の定義を変更します。


```csharp
[Activity (
    Label = "BouncingGame.Android",
    AlwaysRetainTaskState = true,
    Icon = "@drawable/icon",
    Theme = "@android:style/Theme.NoTitleBar",
    ScreenOrientation = ScreenOrientation.Portrait,
    LaunchMode = LaunchMode.SingleInstance,
    MainLauncher = true,
    ConfigurationChanges = ConfigChanges.Orientation | ConfigChanges.ScreenSize | ConfigChanges.Keyboard | ConfigChanges.KeyboardHidden)
]
public class MainActivity : Activity
{ 
...
```


# <a name="default-coordinate-system"></a>既定の座標系

このコードは、インスタンス化、 `CCSprite`、設定、`PositionX`と`PositionY`値を 100 です。 既定では、つまり、`CCSprite`センターは、起動して、画面の下部左から右に 100 ピクセルに配置されます。 アタッチすることによって、座標系を変更できる、`CCCamera`を`CCLayer`です。 取り組んでされません`CCCamera`でこのプロジェクトの詳細について`CCCamera`は含まれて、 [CocosSharp API ドキュメント](https://developer.xamarin.com/api/type/CocosSharp.CCLayer/)です。

ハードウェア上のピクセル単位ではなく「ゲーム」ピクセル上記 100 ピクセルです。 これは、(iPhone と iPad) などの別の解像度のデバイスで同じゲームを実行している原因になるオブジェクトに位置し、物理的な画面に対する相対サイズが正しく構成されていることを意味します。 具体的には、ゲームの表示領域は常に 1024 ピクセルの高さと幅 768 ピクセル、これは、解像度を指定しました前述の「、`LoadGame`メソッドです。


# <a name="adding-the-ball-ccsprite"></a>ボール CCSprite を追加します。

これでの操作の基礎に慣れているお`CCSprite`は追加して、1 秒`CCSprite`– ボールです。 おをする手順はパドルの作成方法に非常に似ており`CCSprite`です。 

まず追加、 **ball.png** 、iOS プロジェクトの解凍したフォルダーからイメージ**コンテンツ**フォルダーです。 選択する**コピー**ファイルを**コンテンツ**ディレクトリ。 リンクを追加する上記と同じ手順に従って、 **ball.png** Android プロジェクトでのファイルです。

次に作成、`CCSprite`メンバーを追加することによってこのボールと呼ばれる`ballSprite`を`GameScene`をインスタンス化コードと同様に、クラス、`ballSprite`です。 完了したら、`GameLayer`クラスに次のようになります。


```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
    }
```


# <a name="adding-the-score-cclabel"></a>表すスコア ローカライズされたを追加します。

ゲームを追加して、最後のビジュアル要素は、`CCLabel`回数を表示するユーザーが正常にボールが浮いてです。 `CCLabel`コンス トラクターで指定したフォントを使用して、画面に文字列を表示します。

次のコード作成を追加、`CCLabel`インスタンス`GameLayer`です。 終了したら、コードは次のようになります。


```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    CCLabel scoreLabel;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "Arial", 20, CCLabelFormat.SystemFont);
        scoreLabel.PositionX = 50;
        scoreLabel.PositionY = 1000;
        scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
        AddChild (scoreLabel);
    }
    ...
```

ScoreLabel が使用されていることを確認、`AnchorPoint`の`CCPoint.AnchorUpperLeft`、つまり、`PositionX`と`PositionY`値が左上隅の位置を定義します。 これにより、位置、`scoreLabel`ラベルの大きさを考慮しなくても、画面の左上の基準としました。


# <a name="implementing-every-frame-logic"></a>フレームごとのロジックを実装します。

これまでに、ゲームは、静的なシーンを表示します。 高頻度でオブジェクトの位置を更新するコードを追加することで、シーン内のオブジェクトの動作を制御するためのロジックが追加されます。 ここでは、このコードは 60 を 1 回実行秒 - 60 とも呼ば*フレーム*1 秒間 (場合を除く、ハードウェアでは、これを頻繁に更新を処理できません)。 具体的には、分類され、入力に従ってパドルを移動し、ボール襲うパドルたびに、プレイヤーのスコアを更新する、パドルに対してバウンド ボールを作成するためのロジックが追加されます。

`Schedule`によって提供されるメソッド、`CCNode`クラス、により、ゲームにフレームごとのロジックを追加します。 後のコードを追加お`// New code`GameLayer コンス トラクターに。


```csharp
public GameLayer () : base (CCColor4B.Black)
{
    // "paddle" refers to the paddle.png image
    paddleSprite = new CCSprite ("paddle");
    paddleSprite.PositionX = 100;
    paddleSprite.PositionY = 100;
    AddChild (paddleSprite);
    ballSprite = new CCSprite ("ball");
    ballSprite.PositionX = 320;
    ballSprite.PositionY = 600;
    AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "arial", 22, CCLabelFormat.SpriteFont);
    scoreLabel.PositionX = 50;
    scoreLabel.PositionY = 1000;
    scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
    AddChild (scoreLabel);
    // New code:
    Schedule (RunGameLogic);
}
```

ここで作成、`RunGameLogic`メソッドで、`GameLayer`クラスは、すべてのフレームごとのロジックを格納します。


```csharp
void RunGameLogic(float frameTimeInSeconds)
{
}
```

Float 型パラメーターは、秒単位で、フレームは、どのくらいの時間を定義します。 使用するこの値ボールの動きを実装する場合。


## <a name="making-the-ball-fall"></a>ボール秋を行う

手動で実装するか、独自の重力コードまたは CocosSharp で組み込み Box2D 機能を使用してボールお寄せください。 Box2D 物理シミュレーション エンジンは、CocosSharp ゲームを使用できます。 非常に強力かつ効率的なが、セットアップ コードの記述が必要です。 この物理シミュレーションが非常に単純なので、手動で実装おします。

重力を実装するには、ストア現在 X と Y のベロシティ、ボールの必要があります。 2 つのメンバーを追加、`GameLayer`クラス。


```csharp
float ballXVelocity;
float ballYVelocity;
// How much to modify the ball's y velocity per second:
const float gravity = 140;
```

落とすロジックを実装して次に`RunGameLogic`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
}
```

## <a name="moving-the-paddle-with-touch-input"></a>タッチ入力に関するパドルの移動

これで、ボールを以下にあるは水平方向の移動を追加、パドルを使用して、`CCEventListenerTouchAllAtOnce`オブジェクト。 このオブジェクトは、入力に関連するイベントの数を提供します。 ここでは、任意のタッチ ポイント パドルの位置調整できるようにを移動する場合に通知されるようにします。 `GameLayer`既にインスタンス化、`CCEventListenerTouchAllAtOnce`ので、単に割り当てる必要があります、`OnTouchesMoved`を委任します。 これを行うには、次のように AddedToScene メソッドを変更します。


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();
    // Use the bounds to layout the positioning of our drawable assets
    CCRect bounds = VisibleBoundsWorldspace;
    // Register for touch events
    var touchListener = new CCEventListenerTouchAllAtOnce ();
    touchListener.OnTouchesEnded = OnTouchesEnded;
    // new code:
    touchListener.OnTouchesMoved = HandleTouchesMoved;
    AddEventListener (touchListener, this);
}
```

次を実装します`HandleTouchesMoved`:


```csharp
void HandleTouchesMoved (System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    // we only care about the first touch:
    var locationOnScreen = touches [0].Location;
    paddleSprite.PositionX = locationOnScreen.X;
}
```


## <a name="implementing-ball-collision"></a>ボールの衝突を実装します。

これで、ボールがパドルを通じてがあることに気付くでしょうゲームを実行場合します。 実装します*衝突*(重なっているゲームのオブジェクトに反応するためのロジック)、フレームごとのコードにします。 移動するオブジェクトが変更は、すべてのフレームを配置、ために、通常は干渉チェックもすべてのフレームを実行します。 またが追加されます。 ベロシティ X 軸に、ボールが一度ぶつかるパドル、ゲームにいくつかのチャレンジを追加するが、つまり、ボールが過去の画面の端を移動することを防止する必要があります。 これで行います、`RunGameLogic`する速度を適用した後にコード、`ballSprite`後`// New Code`:


```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
    // New Code:
    // Check if the two CCSprites overlap...
    bool doesBallOverlapPaddle = ballSprite.BoundingBoxTransformedToParent.IntersectsRect(
        paddleSprite.BoundingBoxTransformedToParent);
    // ... and if the ball is moving downward.
    bool isMovingDownward = ballYVelocity < 0;
    if (doesBallOverlapPaddle && isMovingDownward)
    {
        // First let's invert the velocity:
        ballYVelocity *= -1;
        // Then let's assign a random value to the ball's x velocity:
        const float minXVelocity = -300;
        const float maxXVelocity = 300;
        ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    }
    // First let’s get the ball position:   
    float ballRight = ballSprite.BoundingBoxTransformedToParent.MaxX;
    float ballLeft = ballSprite.BoundingBoxTransformedToParent.MinX;
    // Then let’s get the screen edges
    float screenRight = VisibleBoundsWorldspace.MaxX;
    float screenLeft = VisibleBoundsWorldspace.MinX;
    // Check if the ball is either too far to the right or left:    
    bool shouldReflectXVelocity = 
        (ballRight > screenRight && ballXVelocity > 0) ||
        (ballLeft < screenLeft && ballXVelocity < 0);
    if (shouldReflectXVelocity)
    {
        ballXVelocity *= -1;
    }
}
```


## <a name="adding-scoring"></a>スコア付けの追加

これで、ゲームをプレイ、最後の手順では、スコア付けのロジックを追加します。 最初に、スコア メンバーという名前の GameLayer クラスに追加されます`score`:


```csharp
int score;
```

プレーヤーのスコアを追跡して表示を使用してこの変数を使用、`scoreLabel`です。 If ステートメント内の次のコードを追加これを行うには`RunGameLogic`ボールとパドルがオーバー ラップします。


```csharp
...
if (doesBallOverlapPaddle && isMovingDownward)
{
    // First let's invert the velocity:
    ballYVelocity *= -1;
    // Then let's assign a random to the ball's x velocity:
    const float minXVelocity = -300;
    const float maxXVelocity = 300;
    ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    // New code:
    score++;
    scoreLabel.Text = "Score: " + score;
}
...
```

今すぐゲームを実行したり、ゲームにボールがパドルから bounces とインクリメントされるスコアが表示されることを確認できます。

![](part2-images/image1.png "ゲームを実行してください。 ボールがパドルから bounces とインクリメントされるスコアが表示されます。")


# <a name="summary"></a>まとめ

このチュートリアルでは、クロスプラット フォーム ゲームを作成するグラフィックス、物理的な特性、CocosSharp を使用して、入力と表示されます。 CocosSharp ゲーム開発の概要の最初のステップすることをお勧めします。 最も一般的なクラスは、CocosSharp オブジェクトが正常にレンダリングされるため、ビジュアル ツリーを構築する方法、およびをフレームごとのゲーム ロジックを実装する方法のいくつか説明しました。

このチュートリアルでは、CocosSharp のゲーム エンジンが提供する機能のごく一部のみを説明します。 情報およびチュートリアル CocosSharp 他のトピックでは、次を参照してください。 [、CocosSharp ガイドの残りの部分](~/graphics-games/cocossharp/index.md)です。

## <a name="related-links"></a>関連リンク

- [完了したゲーム (サンプル)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [ゲームの内容 (サンプル)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
