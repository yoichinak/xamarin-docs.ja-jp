---
title: BouncingGame の詳細
description: このドキュメントでは、BouncingGame、CocosSharp で構築された簡単な弾むボール ゲームを作成するため、チュートリアルを提供します。
ms.prod: xamarin
ms.assetid: AC9FD56F-6E4A-40DA-8168-45A761D869FD
author: conceptdev
ms.author: crdun
ms.date: 03/29/2018
ms.openlocfilehash: 9fd6c9108695f58bd69a1c4aa307ca2e4be6dede
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61357528"
---
# <a name="bouncinggame-details"></a>BouncingGame の詳細

> [!TIP]
> BouncingGame のコードが記載されて、[の Xamarin サンプル](https://developer.xamarin.com/samples/mobile/BouncingGame/)します。 このガイドの手順に従う最適なダウンロードしてコードを調べてみると、このガイドでは、それを参照します。

このチュートリアルでは、CocosSharp を使用して、単純な弾むボール ゲームを実装する方法を示します。 

 - ゲームの内容を解凍して
 - CocosSharp の一般的なビジュアル要素
 - ビジュアル要素を追加します。 `GameLayer`
 - フレームごとのロジックを実装します。

完成したゲームは、次のようになります。

![完了した BouncingGame](bouncing-game-images/image1.png "BouncingGame、完了")

## <a name="unzipping-game-content"></a>ゲームの内容を解凍して

ゲーム開発者は多くの場合、用語を使用して*コンテンツ*には、通常 visual アーティスト、ゲームのデザイナー、またはオーディオ デザイナーによって作成される非コード ファイルを参照してください。 一般的な種類コンテンツにはでは、ビジュアルを表示、サウンドを再生または人工知能 (AI) の動作を制御するために使用するファイルが含まれます。 ゲーム開発チームの観点から、通常プログラマ以外にコンテンツが作成されます。 ゲームには、2 種類コンテンツにはが含まれています。

 - パドルとボールを表示する方法を定義する png ファイル。
 - (下のスコアの表示を追加する際は、さらに詳しく説明します) スコア表示で使用されるフォントを定義する 1 つ xnb ファイル

ここで使用してこのコンテンツが記載[zip コンテンツ](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)します。 これらのファイルがダウンロードされ、このチュートリアルの後半にアクセスできる場所に解凍する必要があります。

## <a name="common-cocossharp-visual-elements"></a>CocosSharp の一般的なビジュアル要素

CocosSharp では、多数のビジュアルを表示するために使用するクラスを提供します。 組織に使用されるものが、一部の要素が直接表示します。 ゲームでは、次を使用します。

 - `CCNode` – CocosSharp のすべてのビジュアル オブジェクトの基本クラス。 `CCNode`クラスが含まれています、 `AddChild` 、親子階層を構築するために使用できる関数とも呼ば、*ビジュアル ツリー*または*シーン グラフ*します。 以下に説明するすべてのクラスを継承`CCNode`します。
 - `CCScene` – すべての CocosSharp ゲームのビジュアル ツリーのルートです。 すべてのビジュアル要素は使用したビジュアル ツリーの一部である必要があります、`CCScene`ルートの表示が行われています。
 - `CCLayer` – などのビジュアル コンテナー オブジェクト`CCSprite`します。 名前が示すとおり、`CCLayer`クラスは、相互にどのようにビジュアルの要素層の制御に使用されます。
 - `CCSprite` – イメージまたはイメージの一部を表示します。 `CCSprite` インスタンスは、配置できるサイズを変更し、さまざまな視覚効果を提供します。
 - `CCLabel` -文字列を画面に表示します。 使用されるフォント`CCLabel`xnb ファイルで定義されます。 以下で詳しく xnbs をについて説明します。

さまざまな種類の使用方法について、1 つを検討させていただきます`CCSprite`します。 ビジュアル要素を追加する必要があります、 `CCLayer`、ビジュアル ツリーがあります、`CCScene`のルートにします。 そのため、親/子リレーションシップを 1 つの`CCSprite`なります`CCScene`  >  `CCLayer`  > `CCSprite`します。

## <a name="adding-visual-elements-to-gamelayer"></a>GameLayer ビジュアル要素を追加します。

### <a name="adding-the-paddle-sprite"></a>パドル スプライトを追加します。

最初にゲームのバック グラウンドで黒し、1 つの追加も設定します`CCSprite`画面にレンダリングします。 変更、`GameLayer`を追加するクラス、`CCSprite`次のようにします。

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
            // Use the bounds to layout the positioning of the drawable assets
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

上記のコードは 1 つ作成`CCSprite`の子として追加し、`GameLayer`します。 `CCSprite`コンス トラクターでは、文字列として使用するイメージ ファイルを定義できます。 コードに指示をという名前のファイルを探して CocosSharp `paddle` (拡張機能は省略すると、このガイドで後述)。 CocosSharp は任意のファイル名を検索`paddle`ルート コンテンツ フォルダーで (これは**コンテンツ**) に追加された任意のフォルダーと、 `gameView.ContentManager.SearchPaths` (前のセクションで説明します)。

ルートへの直接ファイルを追加します**コンテンツ**iOS と Android の両方のフォルダー。 右クリックするかをコントロール クリック、**コンテンツ**フォルダーをクリックし、iOS プロジェクトで**追加** > **ファイルを追加しています.** 以前でコンテンツを解凍しましたに移動して選択**paddle.png**します。 場合は、ファイルをフォルダーに追加する方法についてよく寄せられるを選択する必要があります、**コピー**オプション。

![フォルダーのダイアログ ボックスにファイルを追加](bouncing-game-images/image2.png "フォルダ ダイアログ ボックスに、追加のファイル")

次に、Android プロジェクトにファイルを追加します。 右クリックするか、コンテンツのフォルダーをコントロール クリック (これは、**資産**Android プロジェクトのフォルダー) を選択し、**追加** > **ファイルを追加しています.**.今回は、移動、iOS プロジェクトの**コンテンツ**フォルダー。 ファイルを追加する方法について確認するメッセージが表示されたら、選択、**リンクを追加する**オプション。

![フォルダーのダイアログ ボックスにファイルを追加](bouncing-game-images/addalink.png "フォルダ ダイアログ ボックスに、追加ファイル")

ファイルが次の両方のプロジェクトに追加する必要がある理由について説明します。 各プロジェクトの**コンテンツ**フォルダーが含まれています、 **paddle.png**ファイル。

![コンテンツのフォルダーの内容](bouncing-game-images/image3.png "コンテンツ フォルダーの内容")

私たちが表示されます、ゲームを実行した場合、`CCSprite`描画されています。

![画面上に描画、パドル](bouncing-game-images/image4.png "パドル、画面上に描画")

### <a name="file-details"></a>ファイルの詳細

これまでに 1 つのファイルをプロジェクトに追加されましたし、プロセスが非常に簡単です。 追加するだけです、 **paddle.png**ファイルを**コンテンツ**フォルダーと、コード内で参照します。 CocosSharp でファイルを使用する場合、いくつかの考慮事項を見てみましょう。

#### <a name="capitalization"></a>[大文字/小文字の設定]

ファイル名とファイルにアクセスするコードで使用した文字列が小文字の両方に注意してください。 一部のプラットフォーム (Windows デスクトップおよび iOS シミュレーター) などでは大文字と小文字を区別しない、他のプラットフォーム (Android および iOS デバイスなど) は大文字小文字を区別するためです。 クロス プラットフォームとして、ファイルとコードが維持されるよう、小文字のすべてのファイルを使用して、このチュートリアルの残りの部分はできるだけです。

#### <a name="extensions"></a>拡張機能

スプライトを作成するためのコンス トラクターでは、パドル ファイルを参照するときに".png"拡張機能は含まれません。 プラットフォーム間で異なる場合があります同じ資産の種類のファイル拡張子として CocosSharp プロジェクトでの作業時にファイルの拡張子は通常省略されます。 たとえば、オーディオ ファイルについては、プラットフォームに応じて .aiff、.mp3、.wma のファイル形式があります。 拡張機能をオフのままでは、ファイル拡張子に関係なくすべてのプラットフォームで動作する同じコード。

#### <a name="content-in-platform-specific-projects"></a>プラットフォーム固有のプロジェクト内のコンテンツします。

PCL のあるコード ファイルのほとんどとは異なりコンテンツ ファイルはプラットフォーム固有の各プロジェクトに追加する必要があります。 CocosSharp の 2 つの理由からこれが必要です。

1. プラットフォームごとに異なる**ビルド アクション**します。 IOS プロジェクトに追加するコンテンツを使用する必要があります、 **BundleResource**ビルド アクション。 Android プロジェクトに追加するコンテンツを使用する必要があります、 **AndroidAsset**ビルド アクション。
2. 一部のプラットフォームでは、プラットフォーム固有のファイル形式が必要です。 たとえば、このチュートリアルの後半で表示されるよう、フォント .xnb ファイルは iOS と Android で異なります。

ファイル形式は (.png) などのプラットフォーム間で同じ場合、各プラットフォームは 1 つまたは複数のプラットフォームが同じ場所からファイルをリンクが、同じファイルを使用できます。

## <a name="orientation"></a>[方向]

その他のすべてのアプリと同様、CocosSharp アプリは、縦または横の向きで実行できます。 縦モードで実行するゲームが調整されます。 まず、縦向きの縦横比を処理するために、ゲームで解決コードを変更します。 これを行うには、次のように変更します。、`width`と`height`値、`LoadGame`メソッドで、 `ViewController` iOS 上のクラスと`MainActivity`Android 上のクラス。

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
    // ...
```

次に縦モードで実行するには、各プラットフォーム固有プロジェクトを変更する必要があります。

### <a name="ios-orientation"></a>iOS の向き

IOS プロジェクトの向きを変更するには、選択、 **Info.plist**ファイル、 **BouncingGame.iOS**プロジェクト、および変更**展開情報の Iphone/ipod**と、 **iPad 展開情報**縦向きのみを含める。

![向きのオプション](bouncing-game-images/image5.png "向きのオプション")

### <a name="android-orientation"></a>Android の向き

Android プロジェクトの向きを変更するには、BouncingGame.Android プロジェクトの MainActivity.cs ファイルを開きます。 アクティビティの属性の定義を変更ため、次のように縦向きのみを指定します。

```csharp
[Activity (
    Label = "BouncingGame.Android",
    AlwaysRetainTaskState = true,
    Icon = "@drawable/icon",
    Theme = "@android:style/Theme.NoTitleBar",
    ScreenOrientation = ScreenOrientation.Portrait,
    LaunchMode = LaunchMode.SingleInstance,
    MainLauncher = true,
    ConfigurationChanges = ConfigChanges.Orientation | ConfigChanges.ScreenSize | ConfigChanges.Keyboard | ConfigChanges.KeyboardHidden)
]
public class MainActivity : Activity
{ 
...
```

## <a name="default-coordinate-system"></a>既定の座標系

コードでは、インスタンス化、 `CCSprite`、設定、`PositionX`と`PositionY`100 の値。 既定では、つまり、 `CCSprite` center は、最大、および画面の下部にある左から右に 100 ピクセルに配置されます。 座標系をアタッチすることで変更できる、`CCCamera`を`CCLayer`します。 取り組んでしません`CCCamera`の詳細について、このプロジェクトで`CCCamera`で見つかる、 [CocosSharp API ドキュメント](https://developer.xamarin.com/api/type/CocosSharp.CCLayer/)。

ハードウェアでピクセルではなく"game"ピクセル上記で説明した 100 ピクセルです。 これは、中に位置し、物理画面を基準と適切なサイズのオブジェクトで同じゲーム (iPhone と iPad) などの別の解像度のデバイス上で実行が発生することを意味します。 具体的には、ゲームの可視領域では、必ず 1024 ピクセルの高さと幅が 768 ピクセル、前述の指定した解像度は、`LoadGame`メソッド。

## <a name="adding-the-ball-sprite"></a>ボールのスプライトを追加します。

私たちは操作の基本を理解できた`CCSprite`、もう 1 つ追加します`CCSprite`– ボールです。 私たちパドルを作成する方法とよく似ている手順が次を`CCSprite`します。 

最初に、追加、 **ball.png**に iOS プロジェクトの解凍したフォルダーからイメージ**コンテンツ**フォルダー。 選択する**コピー**ファイルを**コンテンツ**ディレクトリ。 リンクを追加するのには、上と同じ手順に従って、 **ball.png** Android プロジェクトのファイル。

次に作成、`CCSprite`と呼ばれるメンバーを追加することでこのボール`ballSprite`を`GameScene`をインスタンス化コードと同様に、クラス、`ballSprite`します。 完了したら、`GameLayer`クラスのようになります。

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

## <a name="adding-the-score-label"></a>スコア付けラベルを追加します。

ゲームを追加して、最後の visual 要素は、`CCLabel`回数の合計を表示するユーザーが正常にボールが浮いています。 `CCLabel`コンス トラクターで指定したフォントを使用して、画面に文字列を表示します。

作成する次のコードを追加、`CCLabel`インスタンス`GameLayer`します。 完了したらコードは、次のようになります。

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
    // ...
```

通知、scoreLabel を使用している、`AnchorPoint`の`CCPoint.AnchorUpperLeft`、です。 つまり、`PositionX`と`PositionY`値は、左上隅の位置を定義します。 これにより、位置、`scoreLabel`ラベルの大きさを考慮することがなく、画面の左上の基準としました。

## <a name="implementing-every-frame-logic"></a>フレームごとのロジックを実装します。

ここまでは、ゲームは、静的なシーンを表示します。 高頻度でオブジェクトの位置を更新するコードを追加することで、シーン内のオブジェクトの動作を制御するためのロジックを追加する予定です。 この場合、コードは 60 を 1 回実行秒 - 60 とも呼ば*フレーム*1 秒あたり (ない場合、ハードウェアでは、これを頻繁に更新を処理できません)。 具体的には、分類され、入力に従ってパドルを移動して、ボールがパドルに発生するたびに、プレーヤーのスコアを更新する、パドルに対してバウンド ボールを作成するロジックを追加する予定です。

`Schedule`メソッドによって提供される、`CCNode`クラス、ゲームをフレームごとのロジックを追加できます。 後のコードを追加します`// New code`GameLayer コンス トラクターに。

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

浮動小数点パラメーターでは、フレームが秒単位で時間を定義します。 使用するこの値はボールの動きを実装する場合。

### <a name="making-the-ball-fall"></a>分類、ボールの作成

重力のどちらか手動で実装するコードによって、または CocosSharp で組み込み Box2D 機能を使用して、ボールを実行できます。 Box2D 物理運動のシミュレーション エンジンは、CocosSharp ゲームで使用可能です。 非常に強力で、効率的なのですが、セットアップ コードを書く必要があります。 物理運動のシミュレーションが非常に単純なので、ここで実装するか手動でします。

重力を実装するには、ストアの現在の X と Y の速度、ボールの必要があります。 2 つのメンバーを追加します、`GameLayer`クラス。

```csharp
float ballXVelocity;
float ballYVelocity;
// How much to modify the ball's y velocity per second:
const float gravity = 140;
```

次に落とすのロジックを実装できる`RunGameLogic`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
}
```

### <a name="moving-the-paddle-with-touch-input"></a>タッチ入力に関するパドルを移動

これで、ボールを以下にある水平方向移動に追加パドルを使用して、`CCEventListenerTouchAllAtOnce`オブジェクト。 このオブジェクトは、さまざまな入力に関連するイベントを提供します。 ここでは、すべてのタッチ ポイントを移動、パドルの位置を調整できるようにユーザーに通知します。 `GameLayer`既にインスタンス化、`CCEventListenerTouchAllAtOnce`だけを割り当てる必要がありますので、`OnTouchesMoved`を委任します。 これを行うには、次のよう AddedToScene メソッドを変更します。

```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();
    // Use the bounds to layout the positioning of the drawable assets
    CCRect bounds = VisibleBoundsWorldspace;
    // Register for touch events
    var touchListener = new CCEventListenerTouchAllAtOnce ();
    touchListener.OnTouchesEnded = OnTouchesEnded;
    // new code:
    touchListener.OnTouchesMoved = HandleTouchesMoved;
    AddEventListener (touchListener, this);
}
```

次に、実装します`HandleTouchesMoved`:

```csharp
void HandleTouchesMoved (System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    // we only care about the first touch:
    var locationOnScreen = touches [0].Location;
    paddleSprite.PositionX = locationOnScreen.X;
}
```

### <a name="implementing-ball-collision"></a>ボールの衝突を実装します。

場合は、ゲームを実行しますので、パドルでボールがあることに気付くでしょう。 実装します*衝突*(重複のゲーム オブジェクトに対応するためのロジック) フレームごとのコードにします。 移動オブジェクトの変更がすべてのフレームを配置するため、通常は競合のチェックもすべてのフレームを実行します。 追加されていきます速度 X 軸のボールがパドル ゲームにいくつかのチャレンジを追加する、つまり、ボールが過去の画面の端を移動する必要があります。 これでは、`RunGameLogic`コードに velocity を適用した後、`ballSprite`後`// New Code`:

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

これでゲームをプレイ、最後の手順は、スコア付けのロジックを追加するは。 最初に、スコア メンバーという名前の GameLayer クラスに追加します`score`:

```csharp
int score;
```

プレーヤーのスコア付けを追跡して表示を使用して、この変数を使用します、`scoreLabel`します。 If ステートメント内で次のコードを追加これを行う`RunGameLogic`パドルとボールが重複して。

```csharp
// ...
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
// ...
```

ここで、ゲームを実行し、ゲームにボールがパドルから bounces にインクリメントされるスコアが表示されることを確認できます。

![スコアがインクリメントを完了したゲーム](bouncing-game-images/image1.png "スコアがインクリメントを完了したゲーム")

## <a name="summary"></a>まとめ

このチュートリアルでは、クロス プラットフォーム ゲームを作成するグラフィックス、物理学 CocosSharp を使用して、入力と表示されます。 CocosSharp のゲーム開発を開始する最初の手順になります。 CocosSharp、オブジェクトが適切にレンダリングされるため、ビジュアル ツリーを構築する方法、およびフレームごとのゲーム ロジックを実装する方法の最も一般的なクラスのいくつか説明しました。

このチュートリアルでは、CocosSharp ゲーム エンジンの機能のほんの一部について説明します。 CocosSharp に関するその他のチュートリアルとは、次を参照してください。 [CocosSharp ガイドの残りの部分](~/graphics-games/cocossharp/index.md)します。

## <a name="related-links"></a>関連リンク

- [完成したゲーム (サンプル)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [ゲームの内容 (サンプル)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
