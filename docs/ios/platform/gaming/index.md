---
title: iOS ゲーム Api Xamarin.iOS で
description: この記事では、Xamarin.iOS ゲームのグラフィック、オーディオ機能を向上させるために使用できる iOS 9 によって提供される、新しいゲームの機能強化について説明します。
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: d8a531e495a19be7437d4a600e758028594248ab
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60953325"
---
# <a name="ios-gaming-apis-in-xamarinios"></a>iOS ゲーム Api Xamarin.iOS で

_この記事では、Xamarin.iOS ゲームのグラフィック、オーディオ機能を向上させるために使用できる iOS 9 によって提供される、新しいゲームの機能強化について説明します。_

Apple は iOS 9 での Api のゲーム プレイするゲームのグラフィックとオーディオを Xamarin.iOS アプリに実装するより簡単にするいくつかの技術的な強化されています。
高レベルのフレームワークの速度の向上とグラフィック機能、iOS デバイスの GPU の力を活用して開発の両方の容易さが含まれます。

[![](images/flocking01.png "群雄実行中のアプリの例")](images/flocking01.png#lightbox)

金属、SceneKit SpriteKit の新しい高度な機能と共に GameplayKit、ReplayKit、モデルの I/O、MetalKit および金属パフォーマンス シェーダーが含まれます。

この記事ではすべて iOS 9 の新しいゲームが強化された Xamarin.iOS ゲームを向上させる方法の紹介します。

## <a name="introducing-gameplaykit"></a>GameplayKit の概要

新しい GameplayKit の Apple のフレームワークを実装に必要な繰り返し発生する、共通のコードの量を減らすことで、iOS デバイス用のゲームを作成するが容易にする一連のテクノロジを提供します。 GameplayKit (SceneKit SpriteKit など) のグラフィック エンジンと簡単に結合できますゲームのメカニズムについてを迅速に開発用のツールが完了したゲームを配信を提供します。

ゲーム プレイ アルゴリズムのなど、GameplayKit には、複数、一般的なが含まれます。

- 動作に基づくエージェント シミュレーションの動きと AI の追求が自動的に目標を定義することができます。
- ターンベースのゲーム プレイの minmax 人工知能します。
- 緊急の動作を提供するあいまいな裏付けとなるとゲーム ロジックをデータに基づくルール システム。

さらに、GameplayKit は、次の機能を提供するモジュール式アーキテクチャを使用してゲーム開発へビルディング ブロック アプローチを採用します。

- 複雑な手続き型コードを処理するためのステート マシン ベースのゲーム プレイ システム。
- 提供するためのツールは、ゲーム プレイ、デバッグの問題を発生させることがなく予測不能性をランダム化します。
- 再利用可能なコンポーネント化されたエンティティ ベースのアーキテクチャ。

GameplayKit の詳細については、Apple を参照してください[Gameplaykit プログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172)と[GameplayKit フレームワーク参照](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199)します。

## <a name="gameplaykit-examples"></a>GameplayKit 例

ゲームのプレイ kit を使用して Xamarin.iOS アプリでいくつかの単純なゲーム プレイ メカニズムの実装の概要を見てみましょう。

### <a name="pathfinding"></a>Pathfinding

Pathfinding そのゲーム ボードの回避策を探すには、ゲームの AI 要素の機能があります。
たとえば、迷路の手段や最初パーソン-シューティング ゲームの世界の地形を 3D 文字 2D の敵です。

次のマップを検討してください。

[![](images/gkpathfindpath.png "Pathfinding マップの例")](images/gkpathfindpath.png#lightbox)

Pathfinding を使用してこのC#コードには、マップを使用する方法があります。

```csharp
var a = GKGraphNode2D.FromPoint (new Vector2 (0, 5));
var b = GKGraphNode2D.FromPoint (new Vector2 (3, 0));
var c = GKGraphNode2D.FromPoint (new Vector2 (2, 6));
var d = GKGraphNode2D.FromPoint (new Vector2 (4, 6));
var e = GKGraphNode2D.FromPoint (new Vector2 (6, 5));
var f = GKGraphNode2D.FromPoint (new Vector2 (6, 0));

a.AddConnections (new [] { b, c }, false);
b.AddConnections (new [] { e, f }, false);
c.AddConnections (new [] { d }, false);
d.AddConnections (new [] { e, f }, false);

var graph = GKGraph.FromNodes(new [] { a, b, c, d, e, f });

var a2e = graph.FindPath (a, e); // [ a, c, d, e ]
var a2f = graph.FindPath (a, f); // [ a, b, f ]

Console.WriteLine(String.Join ("->", (object[]) a2e));
Console.WriteLine(String.Join ("->", (object[]) a2f));
```

### <a name="classical-expert-system"></a>従来のエキスパート システム

次のスニペットC#GameplayKit を使用して、従来のエキスパート システムを実装する方法のコードに示します。

```csharp
string output = "";
bool reset = false;
int input = 15;

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    /*
    If reset is true, clear the output and set reset to false
    */
    var clearRule = GKRule.FromPredicate ((rules) => reset, rules => {
        output = "";
        reset = false;
    });
    clearRule.Salience = 1;

    var fizzRule = GKRule.FromPredicate (mod (3), rules => {
        output += "fizz";
    });
    fizzRule.Salience = 2;

    var buzzRule = GKRule.FromPredicate (mod (5), rules => {
        output += "buzz";
    });
    buzzRule.Salience = 2;

    /*
    This *always* evaluates to true, but is higher Salience, so evaluates after lower-salience items
    (which is counter-intuitive). Print the output, and reset (thus triggering "ResetRule" next time)
    */
    var outputRule = GKRule.FromPredicate (rules => true, rules => {
        System.Console.WriteLine(output == "" ? input.ToString() : output);
        reset = true;
    });
    outputRule.Salience = 3;

    var rs = new GKRuleSystem ();
    rs.AddRules (new [] {
        clearRule,
        fizzRule,
        buzzRule,
        outputRule
    });

    for (input = 1; input < 16; input++) {
        rs.Evaluate ();
        rs.Reset ();
    }
}

protected Func<GKRuleSystem, bool> mod(int m)
{
    Func<GKRuleSystem,bool> partiallyApplied = (rs) => input % m == 0;
    return partiallyApplied;
}
```

指定された一連のルールに基づいて (`GKRule`) と既知の一連の入力、エキスパート システム (`GKRuleSystem`) 予測可能な出力が作成されます (`fizzbuzz`上記の例)。

### <a name="flocking"></a>群雄

群雄、AI のグループがゲームのエンティティをグループを移動およびフライトで鳥の群れや泳ぐ魚の学校のような潜在顧客エンティティの操作に応答する、群れのように動作を制御できます。

次のスニペットC#コードが GameplayKit と SpriteKit を使用して、グラフィックスの表示の群れの動作を実装します。

```csharp
using System;
using SpriteKit;
using CoreGraphics;
using UIKit;
using GameplayKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
using OpenTK;

namespace FieldBehaviorExplorer
{
    public static class FlockRandom
    {
        private static GKARC4RandomSource rand = new GKARC4RandomSource ();

        static FlockRandom ()
        {
            rand.DropValues (769);
        }

        public static float NextUniform ()
        {
            return rand.GetNextUniform ();
        }
    }

    public class FlockingScene : SKScene
    {
        List<Boid> boids = new List<Boid> ();
        GKComponentSystem componentSystem;
        GKAgent2D trackingAgent; //Tracks finger on screen
        double lastUpdateTime = Double.NaN;
        //Hold on to behavior so it doesn't get GC'ed
        static GKBehavior flockingBehavior;
        static GKGoal seekGoal;


        public FlockingScene (CGSize size) : base (size)
        {
            AddRandomBoids (20);

            var scale = 0.4f;
            //Flocking system
            componentSystem = new GKComponentSystem (typeof(GKAgent2D));
            var behavior = DefineFlockingBehavior (boids.Select (boid => boid.Agent).ToArray<GKAgent2D>(), scale);
            boids.ForEach (boid => {
                boid.Agent.Behavior = behavior;
                componentSystem.AddComponent(boid.Agent);
            });

            trackingAgent = new GKAgent2D ();
            trackingAgent.Position = new Vector2 ((float) size.Width / 2.0f, (float) size.Height / 2.0f);
            seekGoal = GKGoal.GetGoalToSeekAgent (trackingAgent);
        }

        public override void TouchesBegan (NSSet touches, UIEvent evt)
        {
            boids.ForEach(boid => boid.Agent.Behavior.SetWeight(1.0f, seekGoal));
        }

        public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            boids.ForEach (boid => boid.Agent.Behavior.SetWeight (0.0f, seekGoal));
        }

        public override void TouchesMoved (NSSet touches, UIEvent evt)
        {
            var touch = (UITouch) touches.First();
            var loc = touch.LocationInNode (this);
            trackingAgent.Position = new Vector2((float) loc.X, (float) loc.Y);
        }


        private void AddRandomBoids (int count)
        {
            var scale = 0.4f;
            for (var i = 0; i < count; i++) {
                var b = new Boid (UIColor.Red, this.Size, scale);
                boids.Add (b);
                this.AddChild (b);
            }
        }

        internal static GKBehavior DefineFlockingBehavior(GKAgent2D[] boidBrains, float scale)
        {
            if (flockingBehavior == null) {
                var flockingGoals = new GKGoal[3];
                flockingGoals [0] = GKGoal.GetGoalToSeparate (boidBrains, 100.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [1] = GKGoal.GetGoalToAlign (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [2] = GKGoal.GetGoalToCohere (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);

                flockingBehavior = new GKBehavior ();
                flockingBehavior.SetWeight (25.0f, flockingGoals [0]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [1]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [2]);
            }
            return flockingBehavior;
        }

        public override void Update (double currentTime)
        {
            base.Update (currentTime);
            if (Double.IsNaN(lastUpdateTime)) {
                lastUpdateTime = currentTime;
            }
            var delta = currentTime - lastUpdateTime;
            componentSystem.Update (delta);
        }
    }

    public class Boid : SKNode, IGKAgentDelegate
    {
        public GKAgent2D Agent { get { return brains; } }
        public SKShapeNode Sprite { get { return sprite; } }

        class BoidSprite : SKShapeNode
        {
            public BoidSprite (UIColor color, float scale)
            {
                var rot = CGAffineTransform.MakeRotation((float) (Math.PI / 2.0f));
                var path = new CGPath ();
                path.MoveToPoint (rot, new CGPoint (10.0, 0.0));
                path.AddLineToPoint (rot, new CGPoint (0.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 20.0));
                path.AddLineToPoint (rot, new CGPoint (20.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 0.0));
                path.CloseSubpath ();

                this.SetScale (scale);
                this.Path = path;
                this.FillColor = color;
                this.StrokeColor = UIColor.White;

            }
        }

        private GKAgent2D brains;
        private BoidSprite sprite;
        private static int boidId = 0;

        public Boid (UIColor color, CGSize size, float scale)
        {
            brains = BoidBrains (size, scale);
            sprite = new BoidSprite (color, scale);
            sprite.Position = new CGPoint(brains.Position.X, brains.Position.Y);
            sprite.ZRotation = brains.Rotation;
            sprite.Name = boidId++.ToString ();

            brains.Delegate = this;

            this.AddChild (sprite);
        }

        private GKAgent2D BoidBrains(CGSize size, float scale)
        {
            var brains = new GKAgent2D ();
            var x = (float) (FlockRandom.NextUniform () * size.Width);
            var y = (float) (FlockRandom.NextUniform () * size.Height);
            brains.Position = new Vector2 (x, y);

            brains.Rotation = (float)(FlockRandom.NextUniform () * Math.PI * 2.0);
            brains.Radius = 30.0f * scale;
            brains.MaxSpeed = 0.5f;
            return brains;
        }

        [Export ("agentDidUpdate:")]
        public void AgentDidUpdate (GameplayKit.GKAgent agent)
        {
        }

        [Export ("agentWillUpdate:")]
        public void AgentWillUpdate (GameplayKit.GKAgent agent)
        {
            var brainsIn = (GKAgent2D) agent;
            sprite.Position = new CGPoint(brainsIn.Position.X, brainsIn.Position.Y);
            sprite.ZRotation = brainsIn.Rotation;
            Console.WriteLine ($"{sprite.Name} -> [{sprite.Position}], {sprite.ZRotation}");
        }
    }
}
```

次に、ビュー コント ローラーでこのシーンを実装します。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.
        this.View = new SKView {
        ShowsFPS = true,
        ShowsNodeCount = true,
        ShowsDrawCount = true
    };
}

public override void ViewWillLayoutSubviews ()
{
    base.ViewWillLayoutSubviews ();

    var v = (SKView)View;
    if (v.Scene == null) {
        var scene = new FlockingScene (View.Bounds.Size);
        scene.ScaleMode = SKSceneScaleMode.AspectFill;
        v.PresentScene (scene);
    }
}
```

実行すると、少しアニメーション _"Boids"_ 本の指のタップ周囲 flock されます。

[![](images/flocking01.png "もう少しアニメーション Boids は本の指のタップ周囲 flock します。")](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>Apple の他の例

上記のサンプルに加えて、Apple にトランス コードは、次のサンプル アプリが提供されるC#と Xamarin.iOS:

- [FourInARow:対戦相手の AI の GameplayKit Minmax ストラテジストを使用します。](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog:GameplayKit で、エージェント システムの使用](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots:SpriteKit と GameplayKit クロス プラットフォーム ゲームの作成](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>メタル

Ios 9 で Apple が行われたいくつかの変更と追加メタル GPU にオーバーヘッドの少ないアクセスを提供します。 Metal を使用したグラフィックと、iOS アプリのコンピューティングの可能性を最大限ことができます。

金属製のフレームワークには、次の新機能が含まれています。

- 新しいプライベートと深度ステンシル テクスチャ OS X 用です。
- 奥行の固定されると、別の前面と背面ステンシル値による品質の向上シャドウします。
- 金属製の網掛けの言語と標準ライブラリのメタル向上があります。
- 計算シェーダーは、広範なピクセル形式をサポートします。

### <a name="the-metalkit-framework"></a>MetalKit フレームワーク

MetalKit フレームワークでは、一連のユーティリティ クラスとメタルを使用して、iOS アプリケーションに必要な作業の量を減らす機能を提供します。 MetalKit は、次の 3 つの主要分野におけるサポートを提供します。

1. 非同期のテクスチャのさまざまな PNG、JPEG、KTX PVR などの一般的な形式を含むソースからの読み込みです。
2. 金属製の特定のモデルの処理のために資産を簡単にモデルの I/O のアクセスに基づいています。 これらの機能は、モデルの I/O のメッシュと金属バッファー間で効率的なデータ転送を提供する高い最適化されています。
3. 金属製の定義済みのビューとビューの管理が、iOS アプリ内でのグラフィックのレンダリングを表示するために必要なコードの量を大幅に軽減します。

MetalKit の詳細については、Apple を参照してください[MetalKit フレームワーク参照](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356)、[メタル プログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)、[金属製のフレームワーク参照](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161)と[メタルシェーディング言語ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)します。

### <a name="metal-performance-shaders-framework"></a>Metal シェーダー フレームワーク

金属パフォーマンス シェーダー フレームワークは、高度に最適化された一連のグラフィックを提供し、ベースのコンピューティング、金属で使用するためのシェーダー ベースの iOS アプリ。 メタルの高パフォーマンスを提供するフレームワークが具体的には調整されてメタル パフォーマンス シェーダーの各シェーダーには、iOS の Gpu がサポートされています。

金属パフォーマンス シェーダー クラスを使用すると、対象として、個々 のコード ベースを管理することがなく各特定の iOS GPU で考えられる最高のパフォーマンスを実現できます。 金属製のパフォーマンスのシェーダーは、バッファー、テクスチャなど、金属製のリソースで使用できます。

金属パフォーマンス シェーダー framework などの一般的なシェーダーのセットが用意されています。

- **ガウスぼかし**(`MPSImageGaussianBlur`)
- **Sobel エッジ検出**(`MPSImageSobel`)
- **イメージのヒストグラム**(`MPSImageHistogram`)

詳細については、Apple を参照してください[メタル シェーディング言語ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)します。

## <a name="introducing-model-io"></a>モデルの I/O の概要

Apple のモデルの I/O のフレームワークでは、(モデルとその関連リソース) などの 3D アセットの深い理解を提供します。 モデルの I/O は、モデルとライティング GameplayKit、金属製および SceneKit を使用できる物理ベース資料で iOS ゲームを提供します。

モデルの I/O では、次の種類のタスクをサポートできます。

- ライト、マテリアルをインポート、データ、カメラの設定、およびさまざまな人気のあるソフトウェアとゲーム エンジン形式からの他のシーンに基づく情報をメッシュです。
- 処理または sky ドームまたはメッシュに照明作成テクスチャ手続きの作成など、シーンに基づく情報を生成します。
- レンダリングのバッファーを GPU に効率的にゲーム アセットを読み込むには、MetalKit、SceneKit および GLKit で動作します。
- シーンに基づく情報をさまざまな人気のあるソフトウェアとゲーム エンジンの形式にエクスポートします。

モデルの I/O の詳細については、Apple を参照してください[モデル I/O のフレームワーク参照](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)

## <a name="introducing-replaykit"></a>ReplayKit の概要

新しい ReplayKit の Apple のフレームワークを使用すると、簡単に、iOS ゲームをゲーム プレイの記録を追加し、迅速かつ簡単に編集し、アプリ内からこの動画を共有するユーザーを許可することができます。

詳細については、Apple を参照してください[ReplayKit と Game Center のビデオでソーシャル機能と](https://developer.apple.com/videos/wwdc/2015/?id=605)とその[DemoBots:SpriteKit と GameplayKit とクロス プラットフォーム ゲームを構築](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)サンプル アプリです。

## <a name="scenekit"></a>SceneKit

シーンのキットは、3 D シーン グラフの 3D グラフィックスの操作を簡素化する API です。 OS X 10.8 で初めて導入し、iOS 8 に来たようになりました。 シーンのキットで没入型の 3D 視覚化およびカジュアルの 3D ゲームの作成は必要ありません OpenGL の専門知識。 シーン グラフの一般的な概念に基づき、シーンのキットを抽象 OpenGL やことが非常に簡単に追加する 3D コンテンツをアプリケーションに、OpenGL ES の複雑さです。 ただし、OpenGL の専門家の場合は、シーン キットも OpenGL と直接リンク付けするための優れたサポートしています。 また、物理学などの 3D グラフィックスを補完する多数の機能が含まれていて、いくつかその他の Apple などのフレームワーク コア アニメーション、Core イメージおよび Sprite Kit と非常にうまく連携します。

詳細についてを参照してください、 [SceneKit](~/ios/platform/gaming/scenekit.md)ドキュメント。

### <a name="scenekit-changes"></a>SceneKit 変更

Apple が ios 9、SceneKit に次の新機能を追加します。

- Xcode は、シーン エディターから直接 Xcode 内でのシーンを編集することによって、ゲームや対話型の 3D アプリをすばやく構築することができますを提供します。
- `SCNView`と`SCNSceneRenderer`(サポートされている iOS デバイス) 上でのベアメタルのレンダリングを有効にするクラスを使用できます。
- `SCNAudioPlayer`と`SCNNode`プレイヤーの位置を iOS アプリに自動的に追跡する空間のオーディオ効果を追加するクラスを使用できます。

詳細についてを参照してください、 [SceneKit ドキュメント](~/ios/platform/introduction-to-ios8.md#scenekit)と Apple の[SceneKit フレームワーク参照](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283)と[Fox:SceneKit ゲーム Xcode シーン エディターでビルド](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154)サンプル プロジェクト。

## <a name="spritekit"></a>SpriteKit

Sprite Kit、apple の 2D ゲーム フレームワークでは、いくつかの興味深い新機能では、iOS 8 および OS X Yosemite が。 シーンのキット、シェーダーのサポート、照明、影、制約、法線マップの生成、および物理学の機能強化との統合が含まれます。 具体的には、物理運動の新機能を使用すると、非常に簡単にゲームにリアルな効果を追加します。

詳細についてを参照してください、 [SpriteKit](~/ios/platform/gaming/spritekit.md)ドキュメント。

### <a name="spritekit-changes"></a>SpriteKit の変更

Apple が iOS 9 の SpriteKit に次の新機能を追加します。

- プレイヤーの位置を自動的に追跡する空間のオーディオ エフェクト、`SKAudioNode`クラス。
- Xcode のシーン エディターとを簡単に 2D ゲームとアプリ作成アクション エディター機能ようになりました。
- 新しいカメラのノードを持つゲームのサポートを簡単にスクロール (`SKCameraNode`) オブジェクト。
- 金属をサポートする iOS デバイスで SpriteKit が自動的に使用が、表示用 OpenGL ES のカスタムのシェーダーを既に使用していた場合でもです。

詳細についてを参照してください、 [SpriteKit ドキュメント](~/ios/platform/introduction-to-ios8.md#spritekit)Apple の[SpriteKit フレームワーク参照](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041)とその[DemoBots:SpriteKit と GameplayKit とクロス プラットフォーム ゲームを構築](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)サンプル アプリです。

## <a name="summary"></a>まとめ

この記事では、新しいでカバーされて、Xamarin.iOS アプリをゲーム機能を iOS 9 を提供します。
GameplayKit とモデルの I/O; がで導入されました金属の主な機能強化である SceneKit と SpriteKit の新機能です。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
