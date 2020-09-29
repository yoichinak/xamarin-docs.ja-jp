---
title: Xamarin の iOS ゲーム Api
description: この記事では、iOS 9 によって提供される新しいゲーム拡張機能について説明します。これを使用すると、Xamarin のゲームのグラフィックス機能とオーディオ機能を向上させることができます。
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 20efcf1af10b7c1d3d36e570bc838e396241ffee
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436247"
---
# <a name="ios-gaming-apis-in-xamarinios"></a>Xamarin の iOS ゲーム Api

_この記事では、iOS 9 によって提供される新しいゲーム拡張機能について説明します。これを使用すると、Xamarin のゲームのグラフィックス機能とオーディオ機能を向上させることができます。_

Apple は、iOS 9 のゲーム Api に技術的にいくつかの機能強化を行っています。これにより、Xamarin iOS アプリでゲームグラフィックスとオーディオを簡単に実装できるようになりました。
これには、高レベルのフレームワークを使用した簡単な開発と、iOS デバイスの GPU 機能を活用して速度とグラフィック能力を向上させることが含まれます。

[![Flocking を実行するアプリの例](images/flocking01.png)](images/flocking01.png#lightbox)

これには、MetalKit、SceneKit、および SpriteKit の新機能として、再生キット、ReplayKit、モデル i/o、、金属パフォーマンスシェーダーが含まれます。

この記事では、Xamarin を改善するためのすべての方法について説明します。 ios ゲームでは、iOS 9 の新しいゲーム拡張機能が追加されています。

## <a name="introducing-gameplaykit"></a>お持ちのおプレイキットの紹介

Apple の新しいゲームプレイキットフレームワークは、実装に必要な反復的な共通コードの量を減らすことで、iOS デバイス用のゲームを簡単に作成できるようにする一連のテクノロジを提供します。 SceneKit Playkit は、ゲーム機構を開発するためのツールを提供します。このメカニズムをグラフィックエンジン (や SpriteKit など) と簡単に組み合わせて、完成したゲームを迅速に提供できます。

ゲームプレイキットには、次のような一般的なゲーム再生アルゴリズムがいくつか含まれています。

- 動作ベースのエージェントシミュレーション。 AI が自動的に追求する動きと目標を定義できます。
- ターンベースのゲームプレイのための、minmax 人工知能。
- 差し迫っの動作を提供するあいまいな推論を持つデータドリブンゲームロジックのルールシステム。

さらに、次の機能を備えたモジュール型アーキテクチャを使用して、ゲーム開発に対するビルディングブロックアプローチを採用しています。

- ゲームプレイで複雑な手続き型のコードベースシステムを処理するためのステートマシン。
- デバッグの問題を発生させずに、ランダム化されたゲームの play と困難を提供するツール。
- 再利用可能なコンポーネント化エンティティベースのアーキテクチャ。

強化されたプレイキットの詳細については、Apple の「開発者向けのプレイキットの [プログラミングガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) 」と「説明」を [参照して](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199)ください。

## <a name="gameplaykit-examples"></a>お持ちのおプレイキットの例

ゲームプレイキットを使用した Xamarin iOS アプリでの簡単なゲームプレイ機構の実装について簡単に見てみましょう。

### <a name="pathfinding"></a>Pathfinding

Pathfinding はゲームの AI 要素がゲームボードを囲む方法を見つけることができる機能です。
たとえば、shooter 世界の地形を通じて、迷路や3D 文字を使用して、2番目の敵を見つけることができます。

次のマップについて考えてみましょう。

[![Pathfinding マップの例](images/gkpathfindpath.png)](images/gkpathfindpath.png#lightbox)

この C# コードを使用して pathfinding を実行すると、マップを通じて次のような結果が得られます。

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

### <a name="classical-expert-system"></a>古典エキスパートシステム

次の C# コードのスニペットでは、次の C# コードを使用して、従来のエキスパートシステムを実装する方法を示しています。

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

特定のルールセット ( `GKRule` ) と既知の入力セットに基づいて、上級システム () によって `GKRuleSystem` 予測可能な出力が作成され `fizzbuzz` ます (上記の例をご覧ください)。

### <a name="flocking"></a>Flocking

Flocking を使用すると、AI 制御ゲームエンティティのグループを flock として動作させることができます。この場合、グループは、フライト中の鳥の flock や魚の頭などの潜在顧客エンティティの動きとアクションに応答します。

次の C# コードスニペットでは、flocking Playkit と SpriteKit を使用したグラフィックスディスプレイの動作を実装しています。

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

次に、ビューコントローラーにこのシーンを実装します。

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

実行すると、わずかなアニメーションの _"Boids"_ が指タップに flock ます。

[![小さなアニメーション化された Boids は、指タップを flock します。](images/flocking01.png)](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>その他の Apple の例

上記のサンプルに加えて、Apple には、C# および Xamarin にトランスコードできる次のサンプルアプリが用意されています。

- [4番の Inaro: 対戦相手 AI 用のストラテジスト Playkit Minmax の使用](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog: Agents システムの使用](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots: SpriteKit とゲームプレイキットを使用したクロスプラットフォームゲームの構築](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>金属

IOS 9 では、Apple によっていくつかの変更が加えられ、GPU へのオーバーヘッドの少ないアクセスが提供されるようになりました。 金属を使用すると、iOS アプリのグラフィックスとコンピューティングの可能性を最大限に引き出すことができます。

金属フレームワークには、次の新機能が含まれています。

- OS X 用の新しいプライベートおよび深度のステンシルテクスチャ。
- 深さのクランプによるシャドウ品質の向上と、個別の front および back ステンシル値の使用。
- メタルシェーディング言語と金属標準ライブラリの機能強化。
- 計算シェーダーは、より広い範囲のピクセル形式をサポートしています。

### <a name="the-metalkit-framework"></a>MetalKit フレームワーク

MetalKit フレームワークには、iOS アプリで金属を使用するために必要な作業量を減らすための一連のユーティリティクラスと機能が用意されています。 MetalKit では、3つの主要な領域がサポートされています。

1. PNG、JPEG、KTX、PVR などの一般的な形式を含むさまざまなソースからの非同期テクスチャ読み込み。
2. 金属固有のモデル処理のために、モデル i/o ベースのアセットに簡単にアクセスできます。 これらの機能は、モデル i/o メッシュとメタルバッファーの間で効率的なデータ転送を提供するように高度に最適化されています。
3. IOS アプリ内でグラフィックのレンダリングを表示するために必要なコードの量を大幅に削減する、事前に定義されたメタルビューおよびビュー管理。

MetalKit の詳細については、Apple の [MetalKit フレームワークリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356)、 [金属プログラミングガイド](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)、金属の [フレームワークリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) 、 [メタルシェーディング言語ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)を参照してください。

### <a name="metal-performance-shaders-framework"></a>メタルパフォーマンスシェーダーフレームワーク

金属製のパフォーマンスシェーダーフレームワークは、金属ベースの iOS アプリで使用する、高度に最適化された一連のグラフィックスと計算ベースのシェーダーを提供します。 金属製のパフォーマンスシェーダーフレームワークの各シェーダーは、金属がサポートされている iOS Gpu の高パフォーマンスを実現するように特別にチューニングされています。

金属製のパフォーマンスシェーダークラスを使用することにより、個々のコードベースをターゲットにして維持しなくても、各 iOS GPU で可能な限り最高のパフォーマンスを実現できます。 メタルパフォーマンスシェーダーは、テクスチャやバッファーなどの任意の金属リソースで使用できます。

金属パフォーマンスシェーダーフレームワークには、次のような一般的なシェーダーのセットが用意されています。

- **ブラー (ガウス)** ( `MPSImageGaussianBlur` )
- **Sobel エッジ検出** ( `MPSImageSobel` )
- **イメージのヒストグラム** ( `MPSImageHistogram` )

詳細については、Apple の [メタルシェーディング言語ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)を参照してください。

## <a name="introducing-model-io"></a>モデル i/o の概要

Apple のモデル i/o フレームワークを使用すると、3D アセット (モデルやその関連リソースなど) を深く理解することができます。 モデル i/o では、お客様の iOS ゲームに、SceneKit Playkit、金属、およびと共に使用できる、物理的なマテリアル、モデル、および照明が用意されています。

モデル i/o では、次の種類のタスクをサポートできます。

- さまざまな一般的なソフトウェアおよびゲームエンジン形式から、照明、マテリアル、メッシュデータ、カメラの設定、およびその他のシーンベースの情報をインポートします。
- メッシュに procedurally テクスチャを作成するなどのシーンベースの情報を処理または生成します。
- MetalKit、SceneKit、GLKit と連携して、レンダリングのためにゲームアセットを GPU バッファーに効率的に読み込みます。
- さまざまな一般的なソフトウェアおよびゲームエンジン形式にシーンベースの情報をエクスポートします。

モデル i/o の詳細については、「Apple の[モデル I/o フレームワークリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)」を参照してください。

## <a name="introducing-replaykit"></a>ReplayKit の概要

Apple の新しい ReplayKit フレームワークを使用すると、iOS ゲームにゲームプレイの記録を簡単に追加し、ユーザーがアプリ内からこのビデオをすばやく簡単に編集および共有できるようにすることができます。

詳細については、「 [ReplayKit を使用した](https://developer.apple.com/videos/wwdc/2015/?id=605) Apple の継続的なソーシャル」と Game Center ビデオとその [Demobots: SpriteKit とゲームプレイキットの](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) サンプルアプリを使用したクロスプラットフォームゲームの構築に関するビデオを参照してください。

## <a name="scenekit"></a>SceneKit

シーンキットは、3d グラフィックスを簡単に操作できる3D シーングラフ API です。 これは OS X 10.8 で初めて導入されましたが、現在は iOS 8 に付属しています。 シーンキットでは、イマーシブ3D 視覚エフェクトとカジュアル3D ゲームを作成する際に、OpenGL に関する専門知識は必要ありません。 シーンキットでは、一般的なシーングラフの概念を基にして、OpenGL と OpenGL の複雑さが解消されるため、アプリケーションに3D コンテンツを簡単に追加できます。 しかし、OpenGL の専門家であれば、シーンキットは OpenGL と直接結び付けることをサポートしています。 また、物理などの3D グラフィックスを補完する多くの機能が含まれており、コアアニメーション、コアイメージ、スプライトキットなど、他のいくつかの Apple framework と非常によく統合されています。

詳細については、 [SceneKit](~/ios/platform/gaming/scenekit.md) のドキュメントを参照してください。

### <a name="scenekit-changes"></a>SceneKit の変更

Apple では、iOS 9 の SceneKit に次の新機能が追加されました。

- Xcode では、Xcode 内から直接シーンを編集することで、ゲームや対話型の3D アプリをすばやく作成できるシーンエディターが提供されるようになりました。
- `SCNView`クラスと `SCNSceneRenderer` クラスを使用して、(サポートされている iOS デバイスで) 金属のレンダリングを有効にすることができます。
- `SCNAudioPlayer`クラスと `SCNNode` クラスを使用すると、iOS アプリに対してプレーヤーの位置を自動的に追跡する空間オーディオ効果を追加できます。

詳細については、 [SceneKit のドキュメント](~/ios/platform/introduction-to-ios8.md#scenekit) と Apple の [SceneKit Framework リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) および「 [Fox: Xcode シーンエディターのサンプルプロジェクトを使用した SceneKit ゲームの構築](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154) 」を参照してください。

## <a name="spritekit"></a>SpriteKit

Apple の 2D game framework であるスプライトキットには、iOS 8 と OS X の新しい機能がいくつかあります。 これには、シーンキット、シェーダーサポート、照明、影、制約、法線マップの生成、および物理的な機能強化との統合が含まれます。 特に、新しい物理機能により、現実的な効果をゲームに追加することが非常に簡単になります。

詳細については、 [SpriteKit](~/ios/platform/gaming/spritekit.md) のドキュメントを参照してください。

### <a name="spritekit-changes"></a>SpriteKit の変更

Apple では、iOS 9 の SpriteKit に次の新機能が追加されました。

- プレーヤーの位置をクラスで自動的に追跡する空間オーディオ効果。 `SKAudioNode`
- Xcode では、2D ゲームとアプリの作成を容易にするシーンエディターとアクションエディターが機能するようになりました。
- 新しいカメラノード () オブジェクトによるゲームのサポートが簡単に `SKCameraNode` なります。
- 金属をサポートする iOS デバイスでは、カスタム OpenGL ES シェーダーを既に使用している場合でも、SpriteKit はレンダリングに自動的にそれを使用します。

詳細については、 [SpriteKit のドキュメント](~/ios/platform/introduction-to-ios8.md#spritekit) 「Apple の [SpriteKit Framework リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) 」および「SpriteKit and のサンプルアプリ [を使用したクロスプラットフォームゲームの構築](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) 」を参照してください。

## <a name="summary"></a>まとめ

この記事では、iOS 9 が Xamarin の iOS アプリ用に提供する新しいゲーム機能について説明しました。
この記事では、お勧めのプレイキットとモデル i/o を導入しました。金属の主な機能強化SceneKit と SpriteKit の新機能。

## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS9)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)