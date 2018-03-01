---
title: SpriteKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 79f7fbf52b544416223d225457d3148d68bc29e2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="spritekit"></a>SpriteKit

スプライト キット、Apple から 2D ゲーム フレームワークには、iOS 8 および OS X Yosemite のいくつか興味深いの新機能があります。 これらには、シーン キット、シェーダーのサポート、照明、影、制約、法線マップの生成、および物理学の機能強化との統合が含まれます。 具体的には、物理的な特性の新機能を使用すると、非常に簡単にゲームに現実的な効果を追加します。

## <a name="physics-bodies"></a>物理本文

スプライト キットには、2 D、固定された本文物理 API が含まれています。 各スプライトが関連付けられている物理本文 (`SKPhysicsBody`) 物理世界で大容量摩擦と本文の geometry などの物理プロパティを定義します。

## <a name="creating-a-physics-body-from-a-texture"></a>テクスチャから物理本文を作成します。
スプライト キットでは、そのテクスチャからスプライトの物理学の本の派生をサポートしています。 これにより、簡単に競合がより自然な検索を実装します。

たとえばに注意してください以下の競合の各イメージの表面にほぼバナナとサルの競合します。
 
![](spritekit-images/image13.png "各イメージの表面にほぼバナナとサルが競合します。")

スプライト キットにより、このような物理本文を作成する 1 行のコードで実行できます。 呼び出すだけ`SKPhysicsBody.Create`テクスチャ サイズと: スプライトします。PhysicsBody = SKPhysicsBody.Create (スプライトです。テクスチャ、スプライトします。サイズ)。

## <a name="alpha-threshold"></a>アルファのしきい値

だけを設定するだけでなく、`PhysicsBody`プロパティ テクスチャから派生したジオメトリを直接、アプリケーションに設定できるとアルファのしきい値をジオメトリの派生方法を制御します。 

アルファのしきい値は、ピクセルが、結果として得られる物理本文に含まれる必要最低限のアルファ値を定義します。 たとえば、次のコードは、若干異なる物理本文が得られます。

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

次のようなアルファのしきい値の調整による効果は、バナナと重複しているときに、サルが経由で分類するように、前の競合を fine-tunes:

![](spritekit-images/image14.png "バナナと重複しているときに、サルが経由で分類します。")
 
## <a name="physics-fields"></a>物理フィールド

別優れたスプライト キットには、値が、新しい物理計算フィールドをサポートします。 Vortex フィールドなどを追加することはこれら放射状重力フィールドといくつかの名前を付けるスプリング フィールドです。

同じように、他のシーンに追加されている SKFieldNode クラスを使用して物理フィールドが作成された`SKNode`です。 さまざまなファクトリ メソッドがある`SKFieldNode`異なる物理計算フィールドを作成します。 スプリング フィールドを作成するには呼び出すことによって`SKFieldNode.CreateSpringField()`、呼び出すことによって放射状重力フィールド`SKFieldNode.CreateRadialGravityField()`のようにします。

`SKFieldNode` フィールドの強度、フィールド領域、およびフィールド フォースの減衰などのフィールドの属性を制御するプロパティもあります。

## <a name="spring-field"></a>スプリング フィールド

たとえば、次のコードはスプリング フィールドを作成し、シーンに追加します。

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

スプライトを追加し、設定できます、`PhysicsBody`プロパティとして、次のコードは、ユーザーが画面をタッチすると、物理計算フィールドのスプライトが影響するように、します。

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    var touch = touches.AnyObject as UITouch;
    var pt = touch.LocationInNode (this);
    var node = SKSpriteNode.FromImageNamed ("TinyBanana");
    node.PhysicsBody = SKPhysicsBody.Create (node.Texture, node.Size);
    node.PhysicsBody.AffectedByGravity = false;
    node.PhysicsBody.AllowsRotation = true;
    node.PhysicsBody.Mass = 0.03f;
    node.Position = pt;
    AddChild (node);
}
```

これにより、フィールド ノードの周囲のスプリングなどが変化するバナナをします。

![](spritekit-images/image15.png "フィールド ノードの周囲のスプリングと同様に、バナナが揺れる")
 
## <a name="radial-gravity-field"></a>放射状重力フィールド

別のフィールドを追加することは似ています。 たとえば、次のコードでは、放射状重力フィールドを作成します。

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

これは、結果は、ここで、バナナ引き出されます放射状フィールドに関する別の force フィールドになります。

![](spritekit-images/image16.png "バナナは、フィールドの周囲放射状プルします。")