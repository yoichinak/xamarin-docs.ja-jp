---
title: Xamarin の ARKit の概要
description: このドキュメントでは、ARKit を使用した iOS 11 の強化された現実について説明します。 ここでは、3D モデルをアプリに追加する方法、ビューを構成する方法、セッションデリゲートを実装する方法、世界で3D モデルを配置する方法、および拡張された現実のセッションを一時停止する方法について説明します。
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/30/2017
ms.openlocfilehash: 6803ecf2303ff2c91265f3ac8352a7aa15e74d40
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436210"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Xamarin の ARKit の概要

_IOS 11 の拡張現実_

ARKit は、拡張されたさまざまな現実のアプリケーションやゲームを可能にします。 このセクションは、次のトピックで構成されています。

- [ARKit を使用したはじめに](#gettingstarted)
- [UrhoSharp で ARKit を使用する](urhosharp.md)

<a name="gettingstarted"></a>

## <a name="getting-started-with-arkit"></a>ARKit を使用したはじめに

次の手順では、強化された現実を開始するために、単純なアプリケーションについて説明します。3D モデルを配置し、ARKit によってモデルが追跡機能によって維持されるようにします。

![カメライメージでの Jet 3D モデルフローティング](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. 3D モデルを追加する

アセットは、 **SceneKitAsset** ビルドアクションを使用してプロジェクトに追加する必要があります。

![プロジェクト内の SceneKit アセット](images/scene-assets.png)

### <a name="2-configure-the-view"></a>2. ビューを構成する

ビューコントローラーのメソッドで `ViewDidLoad` 、シーンアセットを読み込み、ビューのプロパティを設定し `Scene` ます。

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. 必要に応じてセッションデリゲートを実装する

単純なケースでは必須ではありませんが、セッションデリゲートを実装すると、ARKit セッション (および実際のアプリケーションではユーザーにフィードバックを提供する) の状態をデバッグするのに役立ちます。 次のコードを使用して、単純なデリゲートを作成します。

```csharp
public class SessionDelegate : ARSessionDelegate
{
  public SessionDelegate() {}
  public override void CameraDidChangeTrackingState(ARSession session, ARCamera camera)
  {
    Console.WriteLine("{0} {1}", camera.TrackingState, camera.TrackingStateReason);
  }
}
```

メソッドで、デリゲートをに割り当て `ViewDidLoad` ます。

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. 3D モデルを世界中に配置する

では `ViewWillAppear` 、次のコードで ARKit セッションを確立し、デバイスのカメラに対して相対的な空間で3d モデルの位置を設定します。

```csharp
// Create a session configuration
var configuration = new ARWorldTrackingConfiguration {
  PlaneDetection = ARPlaneDetection.Horizontal,
  LightEstimationEnabled = true
};

// Run the view's session
SceneView.Session.Run(configuration, ARSessionRunOptions.ResetTracking);

// Find the ship and position it just in front of the camera
var ship = SceneView.Scene.RootNode.FindChildNode("ship", true);

ship.Position = new SCNVector3(2f, -2f, -9f);
```

アプリケーションが実行または再開されるたびに、3D モデルがカメラの前面に配置されます。 モデルが配置されたら、カメラを移動し、ARKit によってモデルが配置されたままになるようにします。

### <a name="5-pause-the-augmented-reality-session"></a>5. 拡張された現実のセッションを一時停止する

ビューコントローラーが表示されていない場合 (メソッドの場合) は、ARKit セッションを一時停止することをお勧めし `ViewWillDisappear` ます。

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>まとめ

上記のコードでは、単純な ARKit アプリケーションが生成されます。 さらに複雑な例としては、拡張された現実セッションをホストするビューコントローラーがを実装し、追加のメソッドを実装することが想定 `IARSCNViewDelegate` されています。

ARKit は、surface tracking やユーザー操作など、より高度な機能を備えています。 ARKit の追跡と UrhoSharp の組み合わせの例については、 [urhosharp デモ](urhosharp.md) を参照してください。

## <a name="related-links"></a>関連リンク

- [拡張現実 (Apple)](https://developer.apple.com/arkit/)
- [UrhoSharp で ARKit を使用する](urhosharp.md)
- [簡易 ARKit (Jet) のサンプル](/samples/xamarin/ios-samples/ios11-arkitsample)
- [ARKit 配置 (オブジェクトを) (サンプル)](/samples/xamarin/ios-samples/ios11-arkitplacingobjects)
- [ARKit の導入-iOS 向けの拡張現実 (WWDC) (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/602/)