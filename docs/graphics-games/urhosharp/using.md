---
title: UrhoSharp を使用します。
description: UrhoSharp エンジンの概要
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: cdb32c0fe9aa1a267bda5768b9026667723d694c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="using-urhosharp"></a>UrhoSharp を使用します。

_UrhoSharp エンジンの概要_

基本的な方法を取得する、初めてのゲームを作成する前に: シーンをセットアップする方法、リソース (アートワークを含む) を読み込む方法、および、ゲーム用の単純な相互作用を作成する方法です。

<a name="scenenodescomponentsandcameras"/>

# <a name="scenes-nodes-components-and-cameras"></a>シーン、ノード、コンポーネントおよびカメラ

シーンのモデルは、シーンのコンポーネントに基づくグラフとして記述することができます。 シーンは、またシーン全体を表すルート ノードから始まる、シーン ノードの階層で構成されます。 各[ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/)が 3D 変換 (位置、回転、および小数点以下桁数)、名前、ID、およびコンポーネントの任意の数。  コンポーネント ノードに命を示すビジュアル表現を追加してもらう ([`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel))、サウンドを出力することができます ([`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource))、衝突境界などを提示できます。

シーンとを使用してセットアップ ノードを作成することができます、 [Urho エディター](#UrhoEditor)、または c# コードからの処理を行うことができます。  このドキュメントでについて学びます設定作業のコードを使用するように、画面上に表示する点を取得するために必要な要素を示しています、

セットアップする必要があります、シーンを設定するだけでなく、 [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/)、これは、ユーザーに何が表示の取得が決まります。

## <a name="setting-up-your-scene"></a>シーンを設定します。

このフォームは、Start メソッド通常作成します。

```csharp
var scene = new Scene ();
// Create the Octree component to the scene. This is required before
// adding any drawable components, or else nothing will show up. The
// default octree volume will be from -1000, -1000, -1000) to
//(1000, 1000, 1000) in world coordinates; it is also legal to place
// objects outside the volume but their visibility can then not be
// checked in a hierarchically optimizing manner
scene.CreateComponent<Octree> ();
// Create a child scene node (at world origin) and a StaticModel
// component into it. Set the StaticModel to show a simple plane mesh
// with a "stone" material. Note that naming the scene nodes is
// optional. Scale the scene node larger (100 x 100 world units)
var planeNode = scene.CreateChild("Plane");
planeNode.Scale = new Vector3 (100, 1, 100);
var planeObject = planeNode.CreateComponent<StaticModel> ();
planeObject.Model = ResourceCache.GetModel ("Models/Plane.mdl");
planeObject.SetMaterial(ResourceCache.GetMaterial("Materials/StoneTiled.xml"));
```

## <a name="components"></a>コンポーネント

3D オブジェクトを表示するには、サウンドの再生、物理およびスクリプト化されたロジックの更新がすべて有効に呼び出すことによって、ノードにさまざまなコンポーネントを作成することで[ `CreateComponent<T>()`](https://developer.xamarin.com/api/member/Urho.Node.CreateComponent%3CT%3E/p/Urho.CreateMode/System.UInt32/)です。  たとえば、ノードと次のような明るいコンポーネントをセットアップします。

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

名前のノード上で作成した"`DirectionalLight`"し、それ以外のものが、方向を設定します。  ここで、移ることができます、上記のノード ライト出力のノードにアタッチすることにより、 [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) 、コンポーネントで`CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

作成されたコンポーネント、`Scene`特別なロールを持っている自体: シーン全体の機能を実装します。 他のすべてのコンポーネントの前に作成するか、次に示します。

* [`Octree`](https://developer.xamarin.com/api/type/Urho.Octree/): 空間的なパーティション分割を実装し、可視性のクエリを高速化します。 この 3D なしオブジェクトを表示できません。
* [`PhysicsWorld`](https://developer.xamarin.com/api/type/Urho.Physics.PhysicsWorld/): 物理シミュレーションを実装します。 物理コンポーネントなど[ `RigidBody` ](https://developer.xamarin.com/api/type/Urho.Physics.RigidBody/)または[ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/)指定しない場合は正しく機能しないことができます。
* [`DebugRenderer`](https://developer.xamarin.com/api/type/Urho.DebugRenderer/): geometry レンダリングのデバッグを実装します。

通常のコンポーネントと同様に[ `Light` ](https://developer.xamarin.com/api/type/Urho.Light)、 [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera)または[ `StaticModel` ](https://developer.xamarin.com/api/type/Urho.StaticModel)に直接作成しないように、 [ `Scene`](https://developer.xamarin.com/api/type/Urho.Scene)が子ノードの代わりにします。

ライブラリが付属してさまざまなコンポーネントをそれらを現実に近づけるノードに割り当てることができます。 ユーザーに表示される要素 (モデル)、サウンド、固定本文、衝突図形、カメラ、光源、パーティクル エミッタおよび多くです。

## <a name="shapes"></a>図形

必要に応じて、さまざまな図形は Urho.Shapes 名前空間内の単純なノードとして使用できます。  これらには、ボックス、球、円錐、円柱、平面が含まれます。

## <a name="camera-and-viewport"></a>カメラとビューポート

光と同じようにカメラ コンポーネントはインストールされて、ノードに、コンポーネントをアタッチする必要があります、これは、次のようにします。

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

カメラ、これにより、作成したと 3D の世界にカメラを配置している、次の手順を通知するためには、`Application`これは、カメラを使用する場合、これは、次のコードで。

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

およびようになりましたことができます、作成の結果を確認します。

## <a name="identification-and-scene-hierarchy"></a>Id およびシーンの階層

ノードとは異なりコンポーネント名がありません。同じノード内のコンポーネントが、型、および作成の順序で設定されるノードのコンポーネント一覧内のインデックスによって識別されますのみ、たとえば、取得することができます、 [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light)コンポーネントのうち、`lightNode`オブジェクト上記の図のようにします。

```csharp
var myLight = lightNode.GetComponent<Light>();
```

取得してすべてのコンポーネントの一覧を取得することもできます、 [ `Components` ](https://developer.xamarin.com/api/property/Urho.Node.Components/)プロパティが返されます、`IList<Component>`使用することができます。

作成されると、ノードおよびコンポーネントの両方は、シーン グローバルな整数の Id を取得します。 関数を使用して、シーンから照会できます[ `GetNode(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetNode/p/System.UInt32/)と[ `GetComponent(uint id)`](https://developer.xamarin.com/api/member/Urho.Scene.GetComponent/p/System.UInt32/)です。 これは、たとえば再帰的な名前に基づくシーン ノードのクエリを実行するよりはるかに高速です。

エンティティまたはゲームはの組み込みの概念はありません。はなく、ノード階層を決定するプログラマ最大と任意のスクリプト化されたロジックを配置するには、どのノードでです。 通常、3 D の世界での移動オブジェクトが、ルート ノードの子として作成されます。 またはのいずれかの名前を使用せずにノードを作成することができます[ `CreateChild()`](https://developer.xamarin.com/api/member/Urho.Node.CreateChild/p/System.String/Urho.CreateMode/System.UInt32/)です。 ノード名の一意性は適用されません。

推奨される (および、コンポーネントには、独自の 3D 変換があるないため、実際に必要) がいくつかの階層的な構成があるときに子ノードを作成します。

たとえば場合、文字は、自分の手でオブジェクトを保持していた、オブジェクトは独自のノードでは、親文字の手骨に設定すると (また、 [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/))。  例外は、物理[ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape)、offsetted およびノードに関連して個別に回転できます。

なお[ `Scene`](https://developer.xamarin.com/api/type/Urho.Node/)の子ノードの派生ワールド変換を計算するときに、変換は最適化の手法として無視されます意図的に、それを変更することも何も起こりませんおよび (回転なし、元の位置があるために残してください、スケーリングなし。)

[`Scene`](https://developer.xamarin.com/api/type/Urho.Node/) ノードできますで自由に親を再指定します。 これに対しコンポーネント常にするノードに属するに接続されている、ノード間で移動できません。 ノードおよびコンポーネントの両方を提供、 [ `Remove()` ](https://developer.xamarin.com/api/member/Urho.Node.Remove()/)これを実現する親を経由することがなく機能します。 ノードを削除すると、ノードを対象となるコンポーネントでの操作はありません安全な関数を呼び出した後です。

作成することも、`Node`をシーンに属していません。 これが便利ですたとえばために読み込まれたまたは保存すると、シーン内の移動カメラで、カメラ、実際のシーンと共には保存されませんされシーンが読み込まれるときに破棄されません。 ただし、結び付けられていないノードでは、geometry、物理またはスクリプト コンポーネントを作成し、後で移動シーンに、それらのコンポーネントが正常に動作しないが発生します。

## <a name="scene-updates"></a>シーンの更新プログラム

更新が有効なシーン (既定値) はメイン ループ反復ごとに自動的に更新されます。  アプリケーションの[ `SceneUpdate` ](https://developer.xamarin.com/api/event/Urho.Scene.SceneUpdate/)にイベント ハンドラーが呼び出されます。

ノードおよびコンポーネントは、無効にすることによって、シーンの更新から除外することを参照してください[ `Enabled`](https://developer.xamarin.com/api/member/Urho.Node.Enabled)です。  動作は、特定のコンポーネントによって異なりますがたとえばドロウアブル コンポーネントを無効にもなります非表示、それをミュートにサウンドの変換元コンポーネントの無効化中に。 ノードが無効になっている場合は、独自の有効/無効状態に関係なく無効とそのすべてのコンポーネント扱われます。

# <a name="adding-behavior-to-your-components"></a>動作をコンポーネントに追加します。

ゲームを構築する最善の方法では、アクターまたはゲームの要素をカプセル化する、独自のコンポーネントを作成します。  これにより、機能は、自己完結型、それをその動作を表示するために使用するアセットからです。

コンポーネントの動作を追加する最も簡単な方法で指示キューし、非同期のプログラミング (C#) を併用することがあるアクションを使用を開始します。  これにより、非常に高いレベルを指定するようにコンポーネントの動作と、何が起こっているかを理解するが簡単です。

代わりに、正確に動作し、コンポーネントに (フレーム ベースの動作のセクションで説明) フレームごとに、コンポーネントのプロパティを更新することによって制御できます。

## <a name="actions"></a>アクション

動作は、非常に簡単に操作を使用してノードを追加できます。  アクションは、さまざまなノードのプロパティを変更し、期間内に、それらを実行または指定したアニメーション curve 回数をもう一度実行してできます。

たとえば、シーンの「クラウド」ノード、次のようにフェードすることができます。

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

アクションは、変更できないオブジェクトは、さまざまなオブジェクトを推進するため、操作を再利用することができます。

一般的な表現は、逆の操作を実行するアクションを作成します。

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

次の例は、3 秒の期間にわたってのオブジェクトをフェードです。  1 つのアクションを実行することも、別の後などの可能性があります最初を移動した、クラウド、非表示にします。

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

両方のアクションを同時に実行する場合は、並列のアクションを使用し、すべての操作を並列に実行を入力できます。

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

上記の例では、クラウドは移動し、同時にフェードアウトします。

これらが使用される c# わかります await、直線的に目的の動作を実現するために考慮することができます。

## <a name="basic-actions"></a>基本的な操作

UrhoSharp でサポートされているアクションを次に示します。

* ノードの移動: [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo)、 [ `MoveBy` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveBy)、 [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place)、 [ `BezierTo` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierTo)、 [ `BezierBy` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierBy), [`JumpTo`](https://developer.xamarin.com/api/type/Urho.Actions.JumpTo), [`JumpBy`](https://developer.xamarin.com/api/type/Urho.Actions.JumpBy)
* ノードを回転: [ `RotateTo` ](https://developer.xamarin.com/api/type/Urho.Actions.RotateTo)、 [`RotateBy`](https://developer.xamarin.com/api/type/Urho.Actions.RotateBy)
* ノードのスケーリング: [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo)、 [`ScaleBy`](https://developer.xamarin.com/api/type/Urho.Actions.ScaleBy)
* ノードにフェードする: [ `FadeIn` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeIn)、 [ `FadeTo` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeTo)、 [ `FadeOut` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeOut)、 [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide)、 [`Blink`](https://developer.xamarin.com/api/type/Urho.Actions.Blink)
* 濃淡: [ `TintTo` ](https://developer.xamarin.com/api/type/Urho.Actions.TintTo)、 [`TintBy`](https://developer.xamarin.com/api/type/Urho.Actions.TintBy)
* 時点: [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide)、 [ `Show` ](https://developer.xamarin.com/api/type/Urho.Actions.Show)、 [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place)、 [ `RemoveSelf` ](https://developer.xamarin.com/api/type/Urho.Actions.RemoveSelf)、 [`ToggleVisibility`](https://developer.xamarin.com/api/type/Urho.Actions.ToggleVisibility)
* ループ: [ `Repeat` ](https://developer.xamarin.com/api/type/Urho.Actions.Repeat)、 [ `RepeatForever` ](https://developer.xamarin.com/api/type/Urho.Actions.RepeatForever)、 [`ReverseTime`](https://developer.xamarin.com/api/type/Urho.Actions.ReverseTime)

その他の高度な特徴の組み合わせ、 [ `Spawn` ](https://developer.xamarin.com/api/type/Urho.Actions.Spawn)と[ `Sequence` ](https://developer.xamarin.com/api/type/Urho.Actions.Sequence)アクション。

## <a name="easing---controlling-the-speed-of-your-actions"></a>イージング - 操作の速度を制御します。

イージングは、アニメーションが展開、および作成できるのアニメーションもっと楽しい方法を指示する方法です。  既定では、アクションが線形の動作など、 [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo)アクションには、非常にロボットの移動は必要があります。  イージングなどの動作を変更する操作を緩やかに変化、移動の開始、高速化され、緩やかに変化最後に達するに操作をラップすることができます ([`EasyInOut`](https://developer.xamarin.com/api/type/Urho.Actions.EasyInOut))。

たとえば、イージング アクションに既存の操作をラップすることによって行います。

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

多くのイージング モード、次の表は、開始から終了までの時間の期間にわたってで制御する、オブジェクトの値にイージングのさまざまな型とその動作を示しています。

![モードをイージング](using-images/easing.png "このグラフは、時間の期間にわたるを制御するオブジェクトの値にイージングのさまざまな型とその動作を示しています。")

## <a name="using-actions-and-async-code"></a>アクションおよび非同期コードを使用します。

[ `Component` ](https://developer.xamarin.com/api/type/Urho.Component/)サブクラスは、コンポーネントの動作を準備し、その機能をドライブする非同期メソッドを導入する必要があります。
C# を使用してこのメソッドを呼び出すと、`await`キーワードをプログラムの別の部分からいずれか、`Application.Start`メソッドまたは、アプリケーションのユーザーまたはストーリー ポイントに応答します。

例えば:

```csharp
class Robot : Component {
    public bool IsAlive;
    async void Launch ()
    {
        // Dress up our robot
        var cache = Application.ResourceCache;
        var model = node.CreateComponent<StaticModel>();
        model.Model = cache.GetModel ("robot.mdl"));
        model.SetMaterial (cache.GetMaterial ("robot.xml"));
        Node.SetScale (1);

        // Bring the robot into our scene
        await Node.RunActionsAsync(
            new MoveBy(duration: 0.6f, position: new Vector3(0, -2, 0)));
        // Make him move around to avoid the user fire
        MoveRandomly(minX: 1, maxX: 2, minY: -3, maxY: 3, duration: 1.5f);
        // And simultaneously have him shoot at the user
        StartShooting();
    }

    protected async void MoveRandomly (float minX, float maxX,
                                       float minY, float maxY,
                       float duration)
    {
        while (IsAlive){
            var moveAction = new MoveBy(duration,
                new Vector3(RandomHelper.NextRandom(minX, maxX),
                            RandomHelper.NextRandom(minY, maxY), 0));
            await Node.RunActionsAsync(moveAction, moveAction.Reverse());
        }
    }
    protected async void StartShooting()
    {
        while (IsAlive && Node.Components.Count > 0){
            foreach (var weapon in Node.Components.OfType<Weapon>()){
                await weapon.FireAsync(false);
                if (!IsAlive)
                    return;
            }
            await Node.RunActionsAsync(new DelayTime(0.1f));
        }
    }
}
```

`Launch`上記 3 つのアクション メソッドの開始: ロボットがシーンには、この操作は 0.6 秒の期間にわたってノードの場所を変更します。  これは async オプションであるため、これは同時に、呼び出しは、次の命令としてに`MoveRandomly`です。  このメソッドの任意の場所に並列的に、ロボットの位置が変更されます。  複合操作の 2 つに、新しい場所への移動を実行してこれを実現し、元に戻ると位置ロボットがアライブ限り、この手順を繰り返します。 します。  おもしろいことにより、ロボットを同時に撮影保持されます。  トラブルシューティングと、0.1 秒ごとにのみ開始されます。

## <a name="frame-based-behavior-programming"></a>動作のフレーム ベースのプログラミング

フレームごとに基づいてアクションを使用する代わりに、コンポーネントの動作を制御する場合は、オーバーライドするはするが何を行う、 [ `OnUpdate` ](https://developer.xamarin.com/api/member/Urho.Component.OnUpdate)のメソッド、 [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component)サブクラスです。  このメソッドは、フレーム、ごとに 1 回呼び出される、ReceiveSceneUpdates プロパティを true に設定する場合にのみが呼び出されます。

作成する方法を次に示します、 `Rotator` 1 つのノードでは、ノードが回転し、アタッチされているコンポーネント。

```csharp
class Rotator : Component {
    public Rotator()
    {
        ReceiveSceneUpdates = true;
    }
    public Vector3 RotationSpeed { get; set; }
    protected override void OnUpdate(float timeStep)
    {
        Node.Rotate(new Quaternion(
            RotationSpeed.X * timeStep,
            RotationSpeed.Y * timeStep,
            RotationSpeed.Z * timeStep),
            TransformSpace.Local);
    }
}
```

これとは、ノードにこのコンポーネントをアタッチする方法。

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

## <a name="combining-styles"></a>スタイルの結合

スタイルのプログラミング、火災忘れたに最適な動作の多くのプログラミングの非同期/アクション ベースのモデルを使用できますにもいくつかの更新プログラムのコードをフレームごとに実行するコンポーネントの動作も細かく調整できます。

たとえば、SamplyGame デモではこれが使用で、`Enemy`クラスには、基本的な動作は操作ができるようにエンコードが、コンポーネントを持つノードの方向を設定してユーザーにポイントも確実に`Node.LookAt`:

```csharp
    protected override void OnUpdate(SceneUpdateEventArgs args)
    {
        Node.LookAt(
            new Vector3(0, -3, 0),
            new Vector3(0, 1, -1),
            TransformSpace.World);
        base.OnUpdate(args);
    }
```

# <a name="loading-and-saving-scenes"></a>読み込みと保存のシーン

シーンがロードされ、XML 形式で保存することができます。関数を参照してください[ `LoadXml` ](https://developer.xamarin.com/api/member/Urho.Scene.LoadXml)と[ `SaveXML()`](https://developer.xamarin.com/api/member/Urho.Scene.SaveXml)です。 シーンが読み込まれるときにすべての既存コンテンツ (子ノードおよびコンポーネント) では最初に削除されます。 ノードおよび一時的とマークされているコンポーネント、`Temporary`プロパティは保存されません。 すべての組み込みのコンポーネントとプロパティに、シリアライザ-が処理がカスタム プロパティと、コンポーネントのサブクラスで定義されているフィールドを処理できるほどではありません。 ただしこの 2 つの仮想メソッドを提供します。

* [`OnSerialize`](https://developer.xamarin.com/api/member/Urho.Component.OnSerialize) 登録していることができますが、シリアル化のカスタム状態

* [`OnDeserialized`](https://developer.xamarin.com/api/member/Urho.Component.OnDeserialize) ここで、保存されているカスタム状態を取得できます。

通常、カスタム コンポーネントは、次のようになります。

```csharp
class MyComponent : Component {
    // Constructor needed for deserialization
    public MyComponent(IntPtr handle) : base(handle) { }
    public MyComponent() { }
    // user defined properties (managed state):
    public Quaternion MyRotation { get; set; }
    public string MyName { get; set; }

    public override void OnSerialize(IComponentSerializer ser)
    {
        // register our properties with their names as keys using nameof()
        ser.Serialize(nameof(MyRotation), MyRotation);
        ser.Serialize(nameof(MyName), MyName);
    }

    public override void OnDeserialize(IComponentDeserializer des)
    {
        MyRotation = des.Deserialize<Quaternion>(nameof(MyRotation));
        MyName = des.Deserialize<string>(nameof(MyName));
    }
    // called when the component is attached to some node
    public override void OnAttachedToNode()
    {
        var node = this.Node;
    }
}
```

## <a name="object-prefabs"></a>オブジェクトの Prefabs

読み込みまたはシーン全体を保存するだけの柔軟性が足りないゲームの新しいオブジェクトを動的に作成する必要があります。 その一方で、複雑なオブジェクトを作成して、コードでそのプロパティの設定もなります面倒です。 このため、可能であればもを保存するシーン ノードは、その子ノード、コンポーネントと属性が含まれます。 これらは、グループとして後で簡単に読み込むことができます。  多くの場合にこのような保存したオブジェクトを prefab と呼びます。 これには、次の 3 つの方法があります。

- 呼び出すコードの中で[ `Node.SaveXml` ](https://developer.xamarin.com/api/member/Urho.Node.SaveXml)ノード
- エディターで、階層 ウィンドウでノードを選択 を選択して「保存ノードとして」、"File"メニュー。
- 「ノード」コマンドを使用して`AssetImporter`は、シーン ノード階層を保存して、入力資産のすべてのモデルが (に含まれる ファイル)

シーンに保存されているノードのインスタンスを作成するには、呼び出す[ `InstantiateXml()`](https://developer.xamarin.com/api/member/Urho.Scene.InstantiateXml)です。 ノードは、シーンの子として作成されますが、できますで自由に親を再指定をした後。 位置と、ノードを配置するための回転を指定する必要があります。 次のコードは、prefab をインスタンス化する方法を示して`Ninja.xm`目的の位置と回転シーンに。

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

# <a name="events"></a>イベント

UrhoObjects に複数のイベントを発生させる、これらは、それらを生成するさまざまなクラス (C#) イベントとして表示されました。  だけでなく、c#-ベースのイベント モデルも使用することは、`SubscribeToXXX`をサブスクライブし、サブスクリプションのトークンを処理するメソッドを後で使用できますをアンサブスク ライブします。  違いは、あるが、前者はサブスクライブする多くの呼び出し元が許可されますより良いラムダ スタイルが使用するアプローチし、を簡単に、サブスクリプションの削除を許可する、まだのみ、2 つ目は、1 つですが、中にします。  これらは相互に排他的です。

イベントにサブスクライブするときに、適切なイベント引数を指定して引数を受け取るメソッドを提供する必要があります。

たとえば、これは、マウス ボタンを押すイベントにサブスクライブする方法です。

```csharp
public void override Start ()
{
    UI.MouseButtonDown += HandleMouseButtonDown;
}

void HandleMouseButtonDown(MouseButtonDownEventArgs args)
{
    Console.WriteLine ("button pressed");
}
```

ラムダのスタイル。

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

場合がありますが、イベントのような場合、戻り値を保存への呼び出しからの通知の受信を停止`SubscribeTo`メソッドに購読取り消しメソッドを呼び出すとします。

```csharp
Subscription mouseSub;

public void override Start ()
{
    mouseSub = UI.SubscribeToMouseButtonDown (args => {
    Console.WriteLine ("button pressed");
      mouseSub.Unsubscribe ();
    };
}
```

イベント ハンドラーによって受信されたパラメーターが各イベントに固有のものし、イベント ペイロードを格納する厳密に型指定されたイベント引数クラスです。

# <a name="responding-to-user-input"></a>ユーザー入力に応答してください。

イベントにサブスクライブおよび配信される、入力に応答してでダウン キーストロークのように、さまざまなイベントにサブスクライブできます。

```csharp
Start ()
{
    UI.KeyDown += HandleKeyDown;
}

void HandleKeyDown (KeyDownEventArgs arg)
{
     switch (arg.Key){
     case Key.Esc: Engine.Exit (); return;
}
```

多くのシナリオで、シーン更新ハンドラーが更新されて、それに応じてコードを更新すると、キーの現在の状態を確認します。  たとえば、次使用できます、キーボードの入力に基づくカメラの位置を更新します。

```csharp
protected override void OnUpdate(float timeStep)
{
    Input input = Input;
    // Movement speed as world units per second
    const float moveSpeed = 4.0f;
    // Read WASD keys and move the camera scene node to the
    // corresponding direction if they are pressed
    if (input.GetKeyDown(Key.W))
        CameraNode.Translate(Vector3.UnitY * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.S))
        CameraNode.Translate(new Vector3(0.0f, -1.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.A))
        CameraNode.Translate(new Vector3(-1.0f, 0.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.D))
        CameraNode.Translate(Vector3.UnitX * moveSpeed * timeStep, TransformSpace.Local);
}
```

# <a name="resources-assets"></a>リソース (資産)

リソースには、UrhoSharp 初期化またはランタイム中に、大容量記憶装置から読み込まれる大部分の作業が含まれます。

- [`Animation`](https://developer.xamarin.com/api/type/Urho.Animation/) -スケルトン アニメーション用の使用
- [`Image`](https://developer.xamarin.com/api/type/Urho.Resources.Image) さまざまなグラフィック形式で保存されている画像を表します
- [`Model`](https://developer.xamarin.com/api/type/Urho.Model/) -3D モデル
- [`Material`](https://developer.xamarin.com/api/type/Urho.Material) -モデルを表示するために使用される材料します。
- [`ParticleEffect`](https://developer.xamarin.com/api/type/Urho.ParticleEffect)- [説明](http://urho3d.github.io/documentation/1.4/_particles.html)パーティクル エミッタの動作を参照してください"[パーティクル](#particles)"の下。
- [`Shader`](https://developer.xamarin.com/api/type/Urho.Shader) -カスタム シェーダー
- [`Sound`](https://developer.xamarin.com/api/type/Urho.Audio.Sound) -再生するサウンドを参照してください"[サウンド](#sound)"の下。
- [`Technique`](https://developer.xamarin.com/api/type/Urho.Technique/) -材料レンダリング テクニック
- [`Texture2D`](https://developer.xamarin.com/api/type/Urho.Urho2D.Texture2D/) -2D テクスチャ
- [`Texture3D`](https://developer.xamarin.com/api/type/Urho.Texture3D/) -3D テクスチャ
- [`TextureCube`](https://developer.xamarin.com/api/type/Urho.TextureCube/) のキューブ テクスチャ
- `XmlFile`

これらの管理し、によって読み込まれる、 [ `ResourceCache` ](https://developer.xamarin.com/api/type/Urho.Resources.ResourceCache/)サブシステム (として利用可能な[ `Application.ResourceCache` ](https://developer.xamarin.com/api/property/Urho.Application.ResourceCache/))。

リソース自体は、登録済みのリソース ディレクトリまたはパッケージ ファイルとの比較、そのファイルのパスで識別されます。 既定では、エンジンは、登録のリソース ディレクトリ`Data`と`CoreData`、またはパッケージ`Data.pak`と`CoreData.pak`存在する場合。

リソースの読み込みが失敗すると、エラー ログに記録されます、null 参照が返されます。

次の例は、リソースのキャッシュからのリソースのフェッチの一般的な方法を示しています。  ここで、テクスチャの UI 要素にするには、これを使用して、`ResourceCache`プロパティから、`Application`クラスです。

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

リソースを手動で作成したもとディスクから読み込まれていた場合、リソースのキャッシュに格納されていることができます。

リソースの種類ごとに設定できるメモリ容量がある: リソースは、許可されているより多くのメモリを消費する場合、最も古いリソースが削除されます使用されていない場合、キャッシュからされなくなります。 既定では、メモリの予算は無制限に設定されます。

## <a name="bringing-3d-models-and-images"></a>3D モデルおよびイメージ

可能な限り、既存のファイル形式を使用し、モデルなど、どうしても必要な場合にのみカスタム ファイル形式を定義しようと Urho3D (*.mdl)、アニメーション用 (*.ani)。 これらの種類の資産の Urho が用意されており、コンバーターでは、 [AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) fbx、dae、3 ds、および obj など多くの人気のある 3D 形式を使用することができます。

ミキサーの便利なのアドインによっても[ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) Urho3D に適した形式で、Blender 資産をエクスポートすることができます。

## <a name="background-loading-of-resources"></a>リソースのバック グラウンドで読み込まない

通常のいずれかを使用してリソースを要求するときに、`ResourceCache`の`Get`メソッドを読み込むのでは、すべての必要な手順のいくつか (ミリ秒) かかる場合があります、メイン スレッドですぐに (ディスクからファイルを読み込んで、データの解析、必要な場合に、GPU にアップロード) なり、したがってフレーム レートのドロップします。

事前にどのようなリソース必要がある場合は、それらを呼び出すことによって、バック グラウンド スレッドで読み込むを要求できます`BackgroundLoadResource()`です。 使用してバック グラウンド読み込みイベントにサブスクライブすることができます、`SubscribeToResourceBackgroundLoaded`メソッドです。 読み込みが成功または失敗に実際には、指示されます。 バック グラウンド スレッドに移動する可能性があります、読み込みプロセスの一部にのみ、リソースに応じて、たとえばフィニッシュの GPU アップロード手順が常にメイン スレッドで発生する必要です。 バック グラウンド読み込みのキューにあるリソースの方法を読み込み、リソースのいずれかを呼び出す場合は、メイン スレッドは停止の読み込みが完了するまでに注意してください。

機能を読み込む非同期シーン`LoadAsync()`と`LoadAsyncXML()`シーンの内容を読み込むより前のリソースのバック グラウンド読み込みを行うオプションが存在します。 のみを指定して、シーンを変更することがなく、リソースの読み込みにも使用できます、`LoadMode.ResourcesOnly`です。 これにより、高速なインスタンス化のシーンまたはオブジェクト prefab ファイルを準備します。

最後に、最大ミリ秒単位) に要した時間 (各フレームでフィニッシュの背景を設定して読み込まれたリソースを構成することができます、`FinishBackgroundResourcesMs`プロパティを`ResourceCache`です。

<a name="sound"/>

# <a name="sound"></a>サウンド

サウンドがゲーム プレイは、の重要な部分と UrhoSharp フレームワークは、ゲームのサウンドの再生方法を提供します。  アタッチすることによりサウンドを再生する、 [ `SoundSource` ](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource/)コンポーネントを[ `Node` ](https://developer.xamarin.com/api/type/Urho.Node)し、自分のリソースから名前付きのファイルを再生します。

その方法を示します。

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

# <a name="particles"></a>パーティクル

パーティクルは、アプリケーションをいくつかのシンプルかつ安価な効果を追加する簡単な方法を提供します。  などのツールを使用して、PEX 形式で格納されるパーティクルを利用できる[ http://onebyonedesign.com/flash/particleeditor/](http://onebyonedesign.com/flash/particleeditor/)です。

パーティクルは、ノードに追加できるコンポーネントです。  ノードを呼び出す必要がある`CreateComponent<ParticleEmitter2D>`パーティクルを作成し、2 次元効果にエフェクト プロパティを設定してパーティクルを構成する方法がリソースのキャッシュから読み込まれます。

たとえば、コンポーネントがいくつかに達したときに、爆発としてレンダリングされるパーティクルを表示するには、このメソッドを呼び出すことができます。

```csharp
public async void Explode (Component target)
{
    // show a small explosion when the missile reaches an aircraft.
    var cache = Application.ResourceCache;
    var explosionNode = Scene.CreateChild();
    explosionNode.Position = target.Node.WorldPosition;
    explosionNode.SetScale(1f);
    var particle = explosionNode.CreateComponent<ParticleEmitter2D>();
    particle.Effect = cache.GetParticleEffect2D("explosion.pex");
    var scaleAction = new ScaleTo(0.5f, 0f);
    await explosionNode.RunActionsAsync(
        scaleAction, new DelayTime(0.5f));
    explosionNode.Remove();
}
```

上記のコードは、現在のコンポーネントにアタッチされている展開ノードを作成、この展開ノード内お 2D パーティクル発信機を作成エフェクト プロパティを設定して構成します。  2 つのアクションを小さくすると、ノードのスケールを設定する、0.5 秒間、そのサイズで状態のままを実行します。  画面からパーティクル効果も削除されます。 展開を削除します。

上記のパーティクルは、球テクスチャを使用する場合に、次のように表示します。

![球テクスチャを使用してパーティクル](using-images/image-1.png "球テクスチャを使用する場合に、次のように上記のパーティクル レンダリング")

これとはどのような見た目濃淡のむらがテクスチャを使用する場合。

![ボックス テクスチャを使用してパーティクル](using-images/image-2.png "がどんなもの濃淡のむらがテクスチャを使用する場合は、これと")

# <a name="multithreading-support"></a>マルチ スレッドのサポート

UrhoSharp は、シングル スレッド ライブラリです。  つまり、バック グラウンド スレッドから UrhoSharp でメソッドを呼び出すしないで、またはリスクのアプリケーション状態が破損し、アプリケーションがクラッシュする可能性があります。

バック グラウンドでいくつかのコードを実行し、主要な UI で Urho コンポーネントを更新する場合を使えば、 [ `Application.InvokeOnMain(Action)` ](https://developer.xamarin.com/api/member/Urho.Application.InvokeOnMain)メソッドです。  さらを実行できます (C#) await を使用して、.NET のタスク、コードが適切なスレッドで実行されることを確認するための Api です。


# <a name="urhoeditor"></a>UrhoEditor

Urho エディターをダウンロードするには、プラットフォームからの[Urho web サイト](http://urho3d.github.io/)ダウンロードに移動し、最新バージョンを選択します。

# <a name="copyrights"></a>著作権

このドキュメントは、Xamarin Inc から元のコンテンツが含まれていますが、オープン ソース プロジェクトのドキュメント、Urho3D から広範なを描画し、Cocos2D プロジェクトからスクリーン ショットが含まれています。



## <a name="related-links"></a>関連リンク

- [惑星地球ブック](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [座標のブックの表示](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
