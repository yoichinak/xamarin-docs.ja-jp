---
title: "iOS ゲーム Api"
description: "この記事では、iOS 9 Xamarin.iOS ゲームのグラフィックスとオーディオ機能を向上させるために使用できるによって提供される新しいゲーム機能強化について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: d0a66d4cfdb3050c7ad791d24e24d6917a031ee1
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="ios-gaming-apis"></a>iOS ゲーム Api

_この記事では、iOS 9 Xamarin.iOS ゲームのグラフィックスとオーディオ機能を向上させるために使用できるによって提供される新しいゲーム機能強化について説明します。_

Apple には、技術的な改良がいくつか iOS 9 のゲームの Api に容易にする Xamarin.iOS アプリでは、ゲーム グラフィックスおよびオーディオを実装するが行われます。
これらには、高度なフレームワークと速度の向上やグラフィック機能について、iOS デバイスの GPU の電源を利用することによる開発の両方の簡単操作が含まれます。

[![](images/flocking01.png "群雄を実行しているアプリの例")](images/flocking01.png#lightbox)

金属、SceneKit SpriteKit の新しい、拡張機能と共に GameplayKit、ReplayKit、モデルの I/O、MetalKit およびメタル パフォーマンス シェーダーが含まれます。

この記事ではすべての iOS 9 の新しいゲームが強化された、Xamarin.iOS ゲームを向上させる方法の説明します。

## <a name="introducing-gameplaykit"></a>GameplayKit の概要

Apple の新しい GameplayKit フレームワークでは、簡単に実装に必要な共通の反復的なコードの量を減らすと、iOS デバイス用のゲームを作成する一連のテクノロジを提供します。 GameplayKit をすばやく、簡単と組み合わせて使用できます (SceneKit や SpriteKit) などのグラフィック エンジン ゲームのしくみを開発するためのツールが完了したゲームを配信を提供します。

ゲーム プレイ アルゴリズムのように、GameplayKit には、いくつかは、一般的なが含まれます。

- 動作に基づくエージェント シミュレーションの動きや AI を追求は自動的に目標を定義することができます。
- ゲームのプレイを有効にするベースの minmax 人工知能です。
- 緊急の動作を提供するあいまい理由でのゲームのロジックをデータ ドリブンのルール システムです。

さらに、GameplayKit は、次の機能を提供するモジュール式アーキテクチャを使用してゲームの開発にビルディング ブロック アプローチを受け取る。

- 複雑な手続き型コードを処理するためのステート マシン ベースのゲームのプレイでのシステム。
- ツールを提供するランダム化されたゲーム プレイと予測不可能な動作のデバッグの問題を引き起こすことがなくです。
- 再利用可能なコンポーネント化されたエンティティ ベースのアーキテクチャです。

GameplayKit に関する詳細についてを参照してください Apple の[Gameplaykit プログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172)と[GameplayKit フレームワーク参照](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199)です。

## <a name="gameplaykit-examples"></a>GameplayKit 例

ゲーム プレイ キットを使用して、Xamarin.iOS アプリでのいくつかの単純なゲーム プレイ機構の実装の概要を見てみましょう。

### <a name="pathfinding"></a>経路

経路は、その逆はゲーム ボードを検索するゲームの AI 要素の機能です。
たとえば、2 D 敵迷路を通じてまたは最初のユーザー-シューティング world 地形を 3D 文字を検索します。

次のマップを考慮してください。

[![](images/gkpathfindpath.png "経路マップの例")](images/gkpathfindpath.png#lightbox)

経路を使用してこの c# コードのマップを使用する方法があります。

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

### <a name="classical-expert-system"></a>古典エキスパート システム

C# コードの次のスニペットは、GameplayKit を使用して、古典エキスパート システムを実装する方法を示しています。

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

指定された一連のルールに基づく (`GKRule`) と既知の一連の入力、エキスパート システム (`GKRuleSystem`) 予測可能な出力が作成されます (`fizzbuzz`上記の例)。

### <a name="flocking"></a>群雄

群雄 AI のグループにグループを移動および鳥インフライトの群れや泳ぐ魚の学校のような潜在顧客エンティティの操作に応答する、群れとして動作するゲームのエンティティの制御を使用できます。

C# コードの次のスニペットでは、GameplayKit と SpriteKit グラフィック表示を使用した群れの動作を実装します。

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

実行すると、少しアニメーション_"Boids"_指タップ周囲 flock されます。

[![](images/flocking01.png "少しアニメーション Boids は指タップ周囲 flock します。")](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>その他の Apple 例

上記のサンプルに加えて Apple が c# と Xamarin.iOS にトランス コードは、次のサンプル アプリを指定します。

- [FourInARow: GameplayKit Minmax ストラテジストを使用して相手 AI](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog: GameplayKit エージェント システムを使用します。](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots: SpriteKit と GameplayKit クロスプラット フォーム ゲームの作成](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>メタル

IOS 9、Apple がいくつかの変更と追加するように行わメタル GPU にオーバーヘッドの少ないアクセスを提供します。 金属を使用することができますを最大化する、グラフィックスに iOS アプリのコンピューティング可能性です。

金属製のフレームワークには、次の新機能が含まれています。

- 新しいプライベートと深度ステンシル テクスチャ OS X 用です。
- 深さを分けてそれぞれの固定されるフロントとバック ステンシル値による品質の向上シャドウします。
- 金属製の網かけの言語および標準ライブラリのメタル向上があります。
- 計算シェーダーは、広範なピクセル形式をサポートします。

### <a name="the-metalkit-framework"></a>MetalKit フレームワーク

MetalKit フレームワークでは、一連のユーティリティ クラスとメタルを iOS アプリで使用するために必要な作業量を減らす機能を提供します。 MetalKit では、次の 3 つの主要領域でサポートされています。

1. 非同期のテクスチャがさまざまな PNG、JPEG、KTX PVR などの一般的な形式を含むソースからの読み込みです。
2. 金属製の特定のモデルを処理するための資産を簡単にモデル I/O アクセスに基づいています。 これらの機能は、モデルの I/O メッシュとメタル バッファー間で効率的なデータ転送を実現する高最適化されています。
3. 定義済みのメタル ビューと iOS アプリ内でのグラフィックのレンダリングを表示するために必要なコードの量を大幅に削減するビューの管理。

MetalKit に関する詳細についてを参照してください Apple の[MetalKit フレームワーク参照](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356)、[メタル プログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)、[メタル フレームワーク参照](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161)と[メタル言語のガイドを網掛け](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)です。

### <a name="metal-performance-shaders-framework"></a>金属パフォーマンス シェーダー フレームワーク

金属パフォーマンス シェーダー framework グラフィックスの高度に最適化セットを提供し、計算のベース シェーダー、金属で使用する iOS アプリのベースします。 金属で高いパフォーマンスを提供するフレームワークを具体的には調整されている、金属パフォーマンス シェーダーの各シェーダーでは、iOS の Gpu がサポートされています。

金属パフォーマンス シェーダー クラスを使用すると、対象として、個々 のコード ベースを管理することがなく各特定 iOS GPU で可能な最高のパフォーマンスを実現できます。 金属製のパフォーマンス シェーダーは、バッファー、テクスチャなどの任意のメタル リソースで使用できます。

金属パフォーマンス シェーダー フレームワークでは、共通のシェーダーなどのセットを提供します。

- **ガウスのぼかし**(`MPSImageGaussianBlur`)
- **Sobel エッジの検知**(`MPSImageSobel`)
- **画像のヒストグラム**(`MPSImageHistogram`)

詳細については、Apple を参照してください[メタル網掛け言語ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)です。

## <a name="introducing-model-io"></a>モデルの I/O の概要

Apple のモデルの I/O のフレームワークでは、モデルとその関連リソース) などの 3D アセットの深い理解を提供します。 モデルの I/O は、モデルおよび照明 GameplayKit、金属 SceneKit と使用できる物理ベース品に iOS ゲームを提供します。

モデルの I/O で、次の種類のタスクをサポートできます。

- 照明を有するマテリアルをインポート、データ、カメラの設定、およびさまざまな一般的なソフトウェアおよびゲーム エンジン形式から他のシーン ベースの情報をメッシュです。
- プロシージャの作成などのテクスチャ sky ドームまたはメッシュに照明を組み込むことがシーン ベースの情報を生成または処理します。
- MetalKit SceneKit、GLKit に効率的にゲーム アセットを表示するためのバッファーを GPU に読み込むと連動します。
- シーン ベースの情報をさまざまな一般的なソフトウェアおよびゲーム エンジン形式にエクスポートします。

モデルの I/O の詳細については、Apple を参照してください[モデル I/O Framework リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)

## <a name="introducing-replaykit"></a>ReplayKit の概要

Apple の新しい ReplayKit フレームワークを使用すると、簡単に iOS ゲームにゲームの記録を追加して、ユーザーが迅速かつ簡単に編集し、アプリ内からこのビデオを共有できるようにできます。

詳細については、Apple を参照してください[ReplayKit とゲーム センターのビデオでは、ソーシャル移動](https://developer.apple.com/videos/wwdc/2015/?id=605)され、その[DemoBots: 構築、クロス プラットフォームと使用したゲーム SpriteKit GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)サンプル アプリ。

## <a name="scenekit"></a>SceneKit

シーン キットは 3D シーン グラフを 3D 画像の操作を簡略化する API です。 OS X 10.8 で初めて導入されましたし、iOS 8 に来たようになりました。 シーン キット イマーシブ 3D 視覚エフェクトとカジュアル 3D ゲームを作成する必要はありません OpenGL の専門知識。 シーン graph の一般的な概念を構築、シーン キットを抽象 OpenGL と非常に簡単に行う追加 3D コンテンツをアプリケーションに OpenGL ES の複雑な作業です。 ただし、OpenGL エキスパートの場合は、シーン キットによっても OpenGL と直接に結び付けることのサポートがあります。 また、物理学など、3 D グラフィックスを補足する多数の機能が含まれていて、いくつか他の Apple などのフレームワーク、コア アニメーション、Core のイメージおよびスプライト キット予算との統合します。

詳細についてを参照してください、 [SceneKit](~/ios/platform/gaming/scenekit.md)ドキュメント。

### <a name="scenekit-changes"></a>SceneKit 変更

Apple が iOS 9 の SceneKit に次の新機能を追加します。

- Xcode では、Xcode 内から直接シーンを編集することによって、ゲームや対話型の 3D アプリを迅速に構築できるようにするシーン エディターが用意されています。
- `SCNView`と`SCNSceneRenderer`クラスを使用して (サポートされている iOS デバイス上) の金属製のレンダリングを有効にすることができます。
- `SCNAudioPlayer`と`SCNNode`クラスを使用して、自動的に iOS アプリにプレーヤーの位置を追跡する空間のオーディオ エフェクトを追加することです。

詳細についてを参照してください、 [SceneKit ドキュメント](~/ios/platform/introduction-to-ios8.md#scenekit)と Apple の[SceneKit フレームワーク参照](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283)と[Fox: SceneKit ゲームの Xcode シーン エディターを使用して構築](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154)サンプル プロジェクト。

## <a name="spritekit"></a>SpriteKit

スプライト キット、Apple から 2D ゲーム フレームワークには、iOS 8 および OS X Yosemite のいくつか興味深いの新機能があります。 これらには、シーン キット、シェーダーのサポート、照明、影、制約、法線マップの生成、および物理学の機能強化との統合が含まれます。 具体的には、物理的な特性の新機能を使用すると、非常に簡単にゲームに現実的な効果を追加します。

詳細についてを参照してください、 [SpriteKit](~/ios/platform/gaming/spritekit.md)ドキュメント。

### <a name="spritekit-changes"></a>SpriteKit 変更

Apple が iOS 9 の SpriteKit に次の新機能を追加します。

- プレーヤーの位置を自動的に追跡空間のオーディオ エフェクト、`SKAudioNode`クラスです。
- Xcode は、シーンのエディターとを簡単に 2D ゲームとアプリ作成アクション エディターに今すぐ機能します。
- カメラの新しいノードのサポートのゲームを簡単にスクロール (`SKCameraNode`) オブジェクト。
- 金属をサポートする iOS デバイスで SpriteKit は自動的の使用レンダリングでは、カスタムの OpenGL ES シェーダーを使用している場合でもです。

詳細についてを参照してください、 [SpriteKit ドキュメント](~/ios/platform/introduction-to-ios8.md#spritekit)Apple の[SpriteKit フレームワーク参照](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041)され、その[DemoBots: SpriteKit とクロス プラットフォーム ゲームを作成し、GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)サンプル アプリケーションです。

## <a name="summary"></a>まとめ

この記事は、新しいカバーされて、Xamarin.iOS アプリを iOS 9 で提供されるゲーム機能します。
これを導入 GameplayKit しモデル I/O です。金属; する主な機能強化および SceneKit と SpriteKit の新機能です。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
