---
title: Introduction to Xamarin.iOS で ARKit
description: このドキュメントでは、augmented reality ARKit で ios 11 について説明します。 これには、アプリに 3D のモデルを追加、ビューを構成、セッション デリゲートを実装、世界では、3 D のモデルを配置および augmented reality セッションを一時停止する方法について説明します。
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 55ef2004f66cb808f878b2215dfdd59a45015877
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787171"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Introduction to Xamarin.iOS で ARKit

_Ios 11 augmented Reality_

ARKit は、さまざまな augmented reality アプリケーションとゲームを有効にします。 ここでは、次のトピックについて説明します。

- [ARKit の概要](#gettingstarted)
- [UrhoSharp で ARKit の使用](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>ARKit の概要

、Augmented reality で作業を開始する、次の手順を追って簡単なアプリケーション: 3 D のモデルを配置および ARKit にその追跡機能を持つモデルを維持できるようにすることです。

![カメラの画像を浮動小数点 jet 3D モデル](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1.3D モデルを追加します。

資産を使用してプロジェクトに追加する必要があります、 **SceneKitAsset**ビルド アクション。

![プロジェクトで SceneKit 資産](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2.ビューを構成します。

ビュー コント ローラーで`ViewDidLoad`メソッド、シーンの資産を読み込んで、設定、`Scene`ビューのプロパティ。

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3.必要に応じてセッション デリゲートを実装します。

単純な事例は必要ありませんが、セッションのデリゲートを実装することは (および、ユーザーにフィードバックを提供する実際のアプリケーションで) ARKit セッションの状態をデバッグするため役に立ちます。 次のコードを使用して単純なデリゲートを作成します。

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

内のデリゲートを割り当てるので、`ViewDidLoad`メソッド。

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4.3D のモデルでは、世界の位置に移動します。

`ViewWillAppear`、次のコードが ARKit セッションを確立し、デバイスのカメラを基準とした領域の 3D のモデルの位置を設定します。

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

アプリケーションを実行または再開するたびに、3 D のモデルは、カメラの前に配置されます。 モデルが配置されているし、カメラの移動 ARKit 保持配置モデルを観察します。

### <a name="5-pause-the-augmented-reality-session"></a>5.Augmented reality セッションを一時停止します。

ときに ビューのコント ローラーが表示されていない ARKit セッションを一時停止することをお勧めは (で、`ViewWillDisappear`メソッド。

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>まとめ

上記のコードは、単純な ARKit アプリケーションになります。 複雑な例についてを実装する、augmented reality セッションをホストしているビュー コント ローラー、想像`IARSCNViewDelegate`、追加のメソッドを実装するとします。

ARKit は、多数の表面のトラッキング、およびユーザーの操作などのより高度な機能を提供します。 参照してください、 [UrhoSharp デモ](urhosharp.md)例については、追跡 UrhoSharp と ARKit を結合します。


## <a name="related-links"></a>関連リンク

- [Augmented Reality (Apple)](https://developer.apple.com/arkit/)
- [UrhoSharp で ARKit の使用](urhosharp.md)
- [単純な ARKit (Jet) サンプル](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [ARKit を配置するオブジェクト (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [ARKit - Augmented Reality ios (WWDC) (ビデオ) の概要](https://developer.apple.com/videos/play/wwdc2017/602/)
