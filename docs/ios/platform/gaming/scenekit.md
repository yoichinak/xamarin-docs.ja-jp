---
title: Xamarin. iOS の SceneKit
description: このドキュメントでは、SceneKit について説明します。これは、OpenGL の複雑さを解消することで3D グラフィックスの操作を簡略化する3D シーングラフ API です。
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: 5279effa83a8784f6d475188e67a535f7b5e1262
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032496"
---
# <a name="scenekit-in-xamarinios"></a>Xamarin. iOS の SceneKit

SceneKit は、3d グラフィックスを簡単に操作できる3D シーングラフ API です。 これは OS X 10.8 で初めて導入されましたが、現在は iOS 8 に付属しています。 SceneKit では、イマーシブ3D 視覚エフェクトとカジュアル3D ゲームを作成する際に、OpenGL に関する専門知識は必要ありません。 一般的なシーングラフの概念を基にして構築された SceneKit では、OpenGL と OpenGL ES の複雑さが解消されるため、アプリケーションに3D コンテンツを簡単に追加できます。 しかし、OpenGL の専門家であれば、SceneKit は OpenGL と直接結び付けることをサポートしています。 また、物理などの3D グラフィックスを補完する多くの機能が含まれており、コアアニメーション、コアイメージ、スプライトキットなど、他のいくつかの Apple framework と非常によく統合されています。

SceneKit は、非常に簡単に操作できます。 レンダリングを処理する宣言型 API です。 シーンを設定し、プロパティを追加するだけで、SceneKit はシーンのレンダリングを処理します。

SceneKit を操作するには、`SCNScene` クラスを使用してシーングラフを作成します。 シーンには、`SCNNode`のインスタンスによって表されるノードの階層が含まれており、3D 空間内の位置を定義します。 各ノードには、次の図に示すように、ジオメトリ、光源、外観に影響を与える素材などのプロパティがあります。

![](scenekit-images/image7.png "The SceneKit hierarchy")

## <a name="create-a-scene"></a>シーンを作成する

シーンを画面に表示するには、そのシーンをビューのシーンプロパティに割り当てることによって、`SCNView` に追加します。 また、シーンに変更を加えた場合は、`SCNView` によって変更が表示されます。

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

シーンは、3d モデリングツールを使用してエクスポートされたファイルから、またはプログラムでジオメトリックプリミティブから設定できます。 たとえば、球を作成してシーンに追加する方法を次に示します。

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>ライトの追加

この時点では、シーンにライトがないため、球には何も表示されません。 `SCNLight` インスタンスをノードにアタッチすると、SceneKit にライトが作成されます。 さまざまな形式の光源からアンビエント照明まで、いくつかの種類のライトがあります。 たとえば、次のコードでは、球の横に全方向光が作成されます。

```csharp
// omnidirectional light
var light = SCNLight.Create ();
var lightNode = SCNNode.Create ();
light.LightType = SCNLightType.Omni;
light.Color = UIColor.Blue;
lightNode.Light = light;
lightNode.Position = new SCNVector3 (-40, 40, 60);
scene.RootNode.AddChildNode (lightNode);
```

全方向照明は拡散反射を生成するため、光が発生した場合と同様に、光が生じます。 アンビエントライトの作成も似ていますが、すべての方向に均等に光がかかるという点はありません。 これは、ムード照明のように考え:)

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

ライトが配置された状態で、球がシーンに表示されるようになりました。

![](scenekit-images/image8.png "The sphere is visible in the scene when lit")

## <a name="adding-a-camera"></a>カメラの追加

シーンにカメラ (SCNCamera) を追加すると、視点が変わります。 カメラを追加するパターンも似ています。 カメラを作成し、それをノードにアタッチして、そのノードをシーンに追加します。

```csharp
// camera
camera = new SCNCamera {
    XFov = 80,
    YFov = 80
};
cameraNode = new SCNNode {
    Camera = camera,
    Position = new SCNVector3 (0, 0, 40)
};
scene.RootNode.AddChildNode (cameraNode);
```

上記のコードからわかるように、SceneKit オブジェクトは、コンストラクターを使用して作成することも、Create factory メソッドから作成することもできます。 前者では初期化C#子構文を使用できますが、どちらを使用するかは基本的に優先されます。

カメラを配置すると、ユーザーには球全体が表示されます。

![](scenekit-images/image9.png "The entire sphere is visible to the user")

シーンにも他のライトを追加できます。 ここでは、次のように、複数の無指向性ライトを使用します。

![](scenekit-images/image10.png "The sphere with a few more omnidirectional lights")

さらに、`sceneView.AllowsCameraControl = true`を設定することによって、タッチジェスチャを使用してビューの点を変更できます。

### <a name="materials"></a>素材

素材は SCNMaterial クラスを使用して作成されます。 たとえば、球の表面に画像を追加するには、イメージを素材の*拡散*コンテンツに設定します。

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

次に示すように、イメージをノード上にレイヤーします。

![](scenekit-images/image11.png "Layering the image onto the sphere")

素材は、他の種類の照明にも応答するように設定できます。 たとえば、次に示すように、オブジェクトは光沢を設定し、反射の反射を表示するように設定することができます。

![](scenekit-images/image12.png "The object made shiny with specular reflection, resulting in a bright spot on the surface")

素材は非常に柔軟であり、非常に少ないコードで非常に多くのことを実現できます。 たとえば、画像を拡散コンテンツに設定するのではなく、反射コンテンツに設定します。

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

これで、視点は、視点に関係なく、球内に視覚的に見えるようになりました。

### <a name="animation"></a>アニメーション

SceneKit は、アニメーションで適切に動作するように設計されています。 暗黙的または明示的なアニメーションの両方を作成できます。また、コアアニメーションレイヤーツリーからシーンをレンダリングすることもできます。 暗黙的なアニメーションを作成する場合、SceneKit には独自の遷移クラス `SCNTransaction`が用意されています。

球を回転させる例を次に示します。

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

ただし、回転よりもはるかに多くのアニメーション化を行うことができます。 SceneKit の多くのプロパティは system.windows.media.animation.animatable> です。 たとえば、次のコードは、素材の `Shininess` をアニメーション化して、反射反射を増やします。

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

SceneKit は、非常に簡単に使用できます。 制約、物理、宣言型アクション、3D テキスト、フィールドサポートの深さ、スプライトキットの統合、コアイメージの統合など、いくつかの追加機能が用意されています。
