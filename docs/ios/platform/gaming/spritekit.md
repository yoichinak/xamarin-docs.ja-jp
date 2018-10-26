---
title: Xamarin.iOS である SpriteKit
description: このドキュメントには、ある SpriteKit を Apple の 2D グラフィックス フレームワークである SceneKit を統合、物理運動とアニメーションが組み込まれて、照明と、網掛けのサポートが含まれていますがについて説明します。 2D ゲームを作成するのには、SpriteKit を使用できます。
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: ef1e9a98b76166f4ee5638d1ab9762896d1e3bc8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121647"
---
# <a name="spritekit-in-xamarinios"></a>Xamarin.iOS である SpriteKit

SpriteKit、apple の 2D グラフィックス フレームワークでは、いくつかの興味深い新機能では、iOS 8 および OS X Yosemite が。 SceneKit、シェーダーのサポート、照明、影、制約、法線マップの生成、および物理学の機能強化との統合が含まれます。 具体的には、物理運動の新機能を使用すると、非常に簡単にゲームにリアルな効果を追加します。

## <a name="physics-bodies"></a>物理運動の本文

SpriteKit には、2 D、剛体物理運動 API が含まれています。 すべてのスプライトが関連付けられている物理学の本文を (`SKPhysicsBody`) 物理学の世界で大容量や摩擦、本文のジオメトリなどの物理運動プロパティを定義します。

## <a name="creating-a-physics-body-from-a-texture"></a>テクスチャから物理学の本文を作成します。
SpriteKit は、そのテクスチャからスプライトの物理運動の本文を派生できるようになりました。 これにより、簡単に競合がより自然な検索を実装できます。

たとえばに注意してください、次の競合で monkey、banana の各イメージの表面にほとんどの競合します。
 
![](spritekit-images/image13.png "各イメージの表面にほぼ monkey、banana の競合します。")

SpriteKit により、このような物理学本文を作成する 1 行のコードでできること。 呼び出すだけで`SKPhysicsBody.Create`テクスチャとサイズを使用します。 スプライト。PhysicsBody = SKPhysicsBody.Create (スプライトです。テクスチャ、スプライトします。サイズ)。

## <a name="alpha-threshold"></a>アルファのしきい値

だけを設定するだけでなく、`PhysicsBody`プロパティ、テクスチャから派生したジオメトリを直接、アプリケーションを設定できますと、geometry の派生方法を制御するアルファのしきい値。 

アルファのしきい値は、結果として得られる物理学の本文に含まれるピクセル必要があります最小アルファ値を定義します。 たとえば、次のコードは若干異なる物理学の本文で返さ。

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

このようなアルファのしきい値の調整による効果は、前の競合を fine-tunes、banana 衝突させるときに、monkey が経由であるように。

![](spritekit-images/image14.png "Banana 衝突させるときに、monkey が経由であります。")
 
## <a name="physics-fields"></a>物理運動のフィールド

SpriteKit をもう 1 つの優れた追加は、新しいフィールドの物理運動のサポートです。 など、渦流形フィールドを追加することを許可するこれら放射状重力フィールドと spring フィールドに、いくつかの名前します。

その他の同様のシーンに追加される SKFieldNode クラスを使用して物理学のフィールドが作成された`SKNode`します。 さまざまなファクトリ メソッドがある`SKFieldNode`さまざまな物理運動のフィールドを作成します。 Spring フィールドを作成するには呼び出すことによって`SKFieldNode.CreateSpringField()`、呼び出すことによって放射状重力フィールド`SKFieldNode.CreateRadialGravityField()`など。

`SKFieldNode` フィールドの強度、フィールドの領域、およびフィールド フォースの減衰などのフィールドの属性を制御するプロパティもあります。

## <a name="spring-field"></a>Spring フィールド

たとえば、次のコードでは、spring フィールドを作成し、シーンに追加します。

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

スプライトを追加し、設定、`PhysicsBody`プロパティとして、次のコードは、ユーザーが画面をタッチすると、物理運動のフィールドのスプライトが影響するように。

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

これにより、フィールドのノードの周囲の spring のように変化する bananas をします。

![](spritekit-images/image15.png "フィールド ノードの周囲の spring のように、bananas に振動します。")
 
## <a name="radial-gravity-field"></a>放射状重力フィールド

別のフィールドを追加することは似ています。 たとえば、次のコードでは、放射状重力フィールドが作成されます。

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

これは、結果、異なる force フィールドをフィールドについて、bananas を放射状取得された場所。

![](spritekit-images/image16.png "放射状、フィールドの周囲、bananas はプルします。")
