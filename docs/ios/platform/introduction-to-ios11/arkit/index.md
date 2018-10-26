---
title: Xamarin.iOS で ARKit の概要
description: このドキュメントでは、augmented reality ARKit と iOS 11 で説明します。 これには、アプリへの 3D モデルを追加、構成、ビュー、セッション デリゲートを実装、世界では、3 D モデルを配置および augmented reality セッションを一時停止する方法について説明します。
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/30/2017
ms.openlocfilehash: 348d2f2090105ed693da7be5a44c82ef18bd2a89
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119742"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Xamarin.iOS で ARKit の概要

_IOS 11 の拡張現実_

ARKit では、さまざまな拡張現実のアプリケーションやゲームを使用できます。 ここでは、次のトピックについて説明します。

- [ARKit の概要](#gettingstarted)
- [ARKit UrhoSharp の併用](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>ARKit の概要

単純なアプリケーションを通じて augmented reality で開始するに次の手順を説明します。 3 d モデルの配置と ARKit と追跡の機能に、モデルを維持できるようにすることです。

![カメラの画像を浮動小数点 jet 3D モデル](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1.3D モデルを追加します。

資産をプロジェクトに追加する必要があります、 **SceneKitAsset**ビルド アクション。

![プロジェクトである SceneKit アセット](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2.ビューを構成します。

ビュー コント ローラーで`ViewDidLoad`メソッド、シーンの資産を読み込むし、設定、`Scene`ビューのプロパティ。

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3.必要に応じてセッション デリゲートを実装します。

単純な場合は必要ありませんがセッション デリゲートの実装は (と、ユーザーにフィードバックを提供する実際のアプリケーションで)、ARKit セッションの状態をデバッグするために利用できます。 次のコードを使用して単純なデリゲートを作成します。

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

### <a name="4-position-the-3d-model-in-the-world"></a>4.世界での 3D モデルを配置します。

`ViewWillAppear`、次のコードは ARKit セッションを確立し、デバイスのカメラの基準とした領域の 3D モデルの位置を設定します。

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

アプリケーションが実行または再開するには、3 D モデルは、カメラの前に配置されます。 モデルが配置されているし、カメラの移動 ARKit が配置されているモデルをご覧ください。

### <a name="5-pause-the-augmented-reality-session"></a>5.拡張現実のセッションを一時停止します。

ときに ビュー コント ローラーが表示されていない ARKit セッションを一時停止することをお勧めは (で、`ViewWillDisappear`メソッド。

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>まとめ

単純な ARKit アプリケーションで上記のコードの結果。 複雑な例については、ビュー コント ローラーを実装するために、拡張現実のセッションをホストを期待どおり`IARSCNViewDelegate`、追加のメソッドを実装するとします。

ARKit では、多数のサーフェスの追跡、およびユーザーの操作などのより高度な機能を提供します。 参照してください、 [UrhoSharp デモ](urhosharp.md)ARKit UrhoSharp の追跡を組み合わせる例についてはします。


## <a name="related-links"></a>関連リンク

- [Augmented Reality (Apple)](https://developer.apple.com/arkit/)
- [ARKit UrhoSharp の併用](urhosharp.md)
- [単純な ARKit (Jet) のサンプル](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [ARKit を配置するオブジェクト (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [ARKit - Augmented Reality ios (WWDC) (ビデオ) の概要](https://developer.apple.com/videos/play/wwdc2017/602/)
