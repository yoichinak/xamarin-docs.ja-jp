---
title: Xamarin.iOS である SceneKit
description: このドキュメントでは、OpenGL の複雑さを取り除くことによって 3D グラフィックスの操作を簡素化する 3D シーン グラフ API である SceneKit を説明します。
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 0944978b34c8e164acd6e829db177bf4fd72dea9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61376292"
---
# <a name="scenekit-in-xamarinios"></a>Xamarin.iOS である SceneKit

SceneKit とは、3 D シーン グラフを 3D グラフィックスの操作を簡略化する API です。 OS X 10.8 で初めて導入し、iOS 8 に来たようになりました。 SceneKit で没入型の 3D 視覚化およびカジュアルの 3D ゲームの作成は必要ありません OpenGL の専門知識。 シーン グラフの一般的な概念に基づき、SceneKit 抽象 OpenGL やことが非常に簡単に追加する 3D コンテンツをアプリケーションに、OpenGL ES の複雑な作業です。 ただし、OpenGL の専門家の場合は、SceneKit も OpenGL と直接リンク付けするための優れたサポートしています。 また、物理学などの 3D グラフィックスを補完する多数の機能が含まれていて、いくつかその他の Apple などのフレームワーク コア アニメーション、Core イメージおよび Sprite Kit と非常にうまく連携します。

SceneKit は非常に簡単に使用します。 レンダリングを処理する宣言型の API になります。 設定するシーンを簡単に、シーンのレンダリング、および SceneKit ハンドルにプロパティを追加します。

使用してシーン グラフを作成するである SceneKit を使用する、`SCNScene`クラス。 シーンにはインスタンスによって表されるノードの階層が含まれています`SCNNode`、3 D 空間の場所を定義します。 各ノードは次の図に示すようにジオメトリ、ライティングとマテリアルの外観に影響するなどのプロパティがあります。

![](scenekit-images/image7.png "SceneKit 階層") 

## <a name="create-a-scene"></a>シーンを作成します。

シーンを画面に表示するために、追加する、`SCNView`ビューのシーンのプロパティに割り当てるとします。 さらに、シーンを変更する場合は、`SCNView`表示、変更を自動的に更新されます。

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

3d モデリング ツールを使用してエクスポート ファイルまたはプログラムでジオメトリ プリミティブから、シーンを設定できます。 たとえば、これは球体を作成して、シーンに追加する方法を示します。

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>ライトを追加します。

この時点で、球体何も表示されません、シーン内のライトがないためです。 アタッチ`SCNLight`ノードにインスタンスである SceneKit でライトを作成します。 いくつかの種類の光の方向にライトのさまざまな形式からアンビエント照明に至るまでがあります。 たとえば、次のコードでは、球体の横にある全方向性の光が作成されます。

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

全方向照明は、拡散反射懐中電灯を当てたなどの偶数の照明の結果を生成します。 方向として、すべての方向に均等に当たりますが、アンビエント ライトを作成すると同様に、しません。 気分が:) 照明のように考える

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

インプレース ライト、球体では、シーンに表示されるようになりました。

![](scenekit-images/image8.png "球が点灯している場合は、シーンに表示されます。")
 
## <a name="adding-a-camera"></a>カメラの追加

カメラ (SCNCamera) をシーンに追加の観点を変更します。 カメラを追加するパターンは似ています。 カメラを作成、ノードにアタッチし、シーンにノードを追加します。

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

SceneKit コンス トラクターを使用してオブジェクトを作成する上記のコードまたは Create ファクトリ メソッドからがわかるように 前者は、使用してC#初期化子の構文が、大部分の好みでは、どれを使用します。

インプレース カメラ、球体全体は、ユーザーに表示されます。

![](scenekit-images/image9.png "球体全体は、ユーザーに表示されます。")
 
シーンに追加のライトを追加できます。 どのように見えるのいくつかの詳細全方向ライトを次に示します。

![](scenekit-images/image10.png "球のいくつかの詳細全方向性ライト")
 
設定することによってさらに、`sceneView.AllowsCameraControl = true`ユーザーがタッチ ジェスチャの観点を変更できます。

### <a name="materials"></a>素材

マテリアルは SCNMaterial クラスを使用して作成されます。 球のサーフェイスにイメージを追加する例に、イメージを素材の設定*拡散*内容。

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

これは、層の次のようにノード イメージ。

![](scenekit-images/image11.png "球体にイメージをレイヤー化")
 
他の種類の光のすぎるに応答する素材を設定できます。 たとえば、オブジェクトを光沢のあるできるし、反射の内容を画面で、明るいスポットを次に示すようにその結果、スペキュラ反射を表示する設定。

![](scenekit-images/image12.png "光沢のあるサーフェイスに明るいスポットで反射のリフレクションを使用して行われたオブジェクト")
 
資料は非常に柔軟なごくわずかなコードの多くを達成することができます。 たとえば、設定ではなく拡散の内容をイメージに設定反射内容代わりにします。

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

内の観点の独立した、球体の視覚的にとどまるように、monkey が表示されます。

### <a name="animation"></a>アニメーション

SceneKit は、アニメーションで適切に動作設計されています。 明示的または暗黙的のアニメーションを作成したり、コア アニメーションのレイヤーのツリーからシーンのレンダリングもできます。 SceneKit 独自 transition クラスは、暗黙のアニメーションを作成するときに`SCNTransaction`します。

球を回転する例を次に示します。

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

アニメーション化できます回転よりはるかに多くただしします。 SceneKit の多数のプロパティはアニメーション化可能です。 たとえば、次のコードがマテリアルをアニメーション化`Shininess`スペキュラ反射を向上させる。

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

SceneKit は非常に簡単に使用します。 豊富な制約、物理学、宣言型のアクション、3D テキスト フィールドのサポート、Sprite Kit 統合と少しだけ名前を Core イメージ統合の深さを含む追加機能を提供します。