---
title: UrhoSharp を使用して 3D ゲームをビルドするには
description: このドキュメントでは、シーン、コンポーネント、図形、カメラ、アクション、ユーザー入力、サウンドなどを記述する、UrhoSharp の概要を示します。
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 5e5c4f1545d39befde6574338ec4c1ca4037ad8b
ms.sourcegitcommit: a7170494e1975f0f1be547a45444752fd8e57819
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/27/2019
ms.locfileid: "58507163"
---
# <a name="using-urhosharp-to-build-a-3d-game"></a>UrhoSharp を使用して 3D ゲームをビルドするには

初めてのゲームを記述する前に、基礎を詳しくなる: シーンをセットアップする方法、リソース (アートワークを含む) を読み込む方法、およびゲームの単純な相互作用を作成する方法。

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>シーン、ノード、コンポーネント、およびカメラ

シーンのモデルは、コンポーネント ベースのシーン グラフとして記述できます。 シーンは、シーン全体を表すもルート ノードからシーンのノードの階層で構成されます。 各[ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/)は 3D 変換 (位置、回転、およびスケール)、名前、ID、およびコンポーネントの任意の数。  コンポーネントをノードを実現する、視覚的表現の追加を行えるように ([`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel))、サウンドを出力することができます ([`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource))、これに衝突境界を提供できます。

セットアップのノードを使用して、シーンを作成することができます、 [Urho エディター](#urhoeditor)、または C# コードから操作を実行できます。  このドキュメントで紹介設定、コードを使用して作業を画面に表示させるために必要な要素について説明するよう

セットアップに必要なシーンを設定するだけでなく、 [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/)、これは、どのようにして決まります内容は、ユーザーに表示を取得します。

### <a name="setting-up-your-scene"></a>シーンの設定

このフォームは、Start メソッドは、通常作成します。

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

### <a name="components"></a>コンポーネント

3D オブジェクトのレンダリング、サウンドの再生、物理運動およびスクリプト化されたロジックの更新はすべて有効に呼び出すことによって、ノードにさまざまなコンポーネントを作成して[ `CreateComponent<T>()`](https://developer.xamarin.com/api/member/Urho.Node.CreateComponent%3CT%3E/p/Urho.CreateMode/System.UInt32/)します。  たとえば、ノード、次のようにライトのコンポーネントをセットアップします。

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

名前のノードが作成した"`DirectionalLight`"し、それが他に何も、方向を設定します。  ここで変えることができます、上記のノード light 出力のノードでは、アタッチすることにより、 [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) 、コンポーネントで`CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

作成されたコンポーネント、`Scene`自体が特別なロールを持っている。 シーン全体の機能を実装します。 これらは、他のすべてのコンポーネントの前に作成する必要があり、次のとおり。

* [`Octree`](https://developer.xamarin.com/api/type/Urho.Octree/): 空間パーティション分割を実装し、可視性のクエリを高速化します。 この 3D せず、オブジェクトを表示しないことができます。
* [`PhysicsWorld`](https://developer.xamarin.com/api/type/Urho.Physics.PhysicsWorld/): 物理学のシミュレーションを実装します。 などのコンポーネントの物理運動[ `RigidBody` ](https://developer.xamarin.com/api/type/Urho.Physics.RigidBody/)または[ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/)これを行わないは正しく機能しないことができます。
* [`DebugRenderer`](https://developer.xamarin.com/api/type/Urho.DebugRenderer/): 実装が geometry のレンダリングをデバッグします。

などの通常のコンポーネント[ `Light` ](https://developer.xamarin.com/api/type/Urho.Light)、 [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera)または [`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)
直接作成しないように、 [ `Scene`](https://developer.xamarin.com/api/type/Urho.Scene)が子ノードの代わりにします。

ライブラリが付属するさまざまな有効期間に、ノードにアタッチできるコンポーネント: ユーザーに表示される要素 (モデル)、サウンド、剛体、衝突図形、カメラ、光源、パーティクル エミッタおよびなど。

### <a name="shapes"></a>図形

必要に応じて、さまざまな図形が Urho.Shapes 名前空間内の単純なノードとして使用できます。  ボックス、球体、円錐、円柱、平面が含まれます。

### <a name="camera-and-viewport"></a>カメラとビューポート

カメラは、コンポーネントを光と同様、ノードにコンポーネントをアタッチする必要があります、これは、次のように。

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

これにより、カメラを作成したと 3D の世界では、カメラを配置した、次の手順を通知するためには、`Application`を使用するカメラは、次のコードで実行されます。

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

今すぐして作成の結果を表示。

### <a name="identification-and-scene-hierarchy"></a>識別とシーンの階層

ノードとは異なりコンポーネント名がありません。同じノード内のコンポーネントが、種類、および作成順序が格納されるノードのコンポーネント一覧内のインデックスによって識別されるのみで、たとえば、取得、 [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light)コンポーネントのうち、`lightNode`オブジェクト上記の次のようにします。

```csharp
var myLight = lightNode.GetComponent<Light>();
```

取得することによって、すべてのコンポーネントの一覧を取得することもできます、 [ `Components` ](https://developer.xamarin.com/api/property/Urho.Node.Components/)を返すプロパティ、`IList<Component>`使用することができます。

作成されると、ノードとコンポーネントの両方は、シーンにグローバルな整数 Id を取得します。 関数を使用して、シーンからクエリできる[ `GetNode(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetNode/p/System.UInt32/)と[ `GetComponent(uint id)`](https://developer.xamarin.com/api/member/Urho.Scene.GetComponent/p/System.UInt32/)します。 これは、たとえば再帰的な名前に基づくシーン ノードのクエリの実行より高速です。

エンティティまたは; ゲーム オブジェクトの組み込みの概念はありません。はなく、ノード階層を決定するには、プログラマの責任と、スクリプトのロジックを配置するには、どのノードになります。 通常、ルート ノードの子として、3 D ワールドで無料の移動オブジェクトを作成するは。 または名前を使用せずにノードを作成できる[ `CreateChild()`](https://developer.xamarin.com/api/member/Urho.Node.CreateChild/p/System.String/Urho.CreateMode/System.UInt32/)します。 ノード名の一意性は適用されません。

推奨される (および、コンポーネントには、独自の 3D 変換があるないため、実際に必要) がいくつかの階層の構成があるときに子ノードを作成します。

たとえば、オブジェクトが独自のノードでは、文字のハンド ボーンに親が文字では、自分の手でオブジェクトを保持していた場合、(も、 [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/))。  例外は、物理運動[ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape)、offsetted なり、ノードに対して個別に回転した可能性があります。

なお[ `Scene`](https://developer.xamarin.com/api/type/Urho.Node/)の子ノードの派生ワールド変換を計算するときに、変換は、最適化として無視意図的、効果がないので変更すること、および (回転のない、元の位置にあるために残してください、スケール変換はできません。)

[`Scene`](https://developer.xamarin.com/api/type/Urho.Node/) ノードは自由に親を再指定します。 これに対しコンポーネントはに接続されているし、ノード間を移動しないことができますが、ノードに常に属しています。 ノードおよびコンポーネントの両方を提供、 [ `Remove()` ](https://developer.xamarin.com/api/member/Urho.Node.Remove()/)親を経由することがなくこれを実現する関数。 ノードが削除されると、ノードまたは対象のコンポーネントに対する操作がないセーフその関数を呼び出した後です。

作成することも、`Node`シーンに含まれない。 これは便利ですたとえばために読み込まれたまたは保存すると、シーン内の移動カメラで、カメラは実際のシーンと共には保存されませんシーンが読み込まれるときに破棄されません。 ただし、結び付けられていないノードでは、geometry、物理運動またはスクリプト コンポーネントを作成して、後で、シーンを移動によってそれらのコンポーネントが正常に動作しないによってことに注意してください。

### <a name="scene-updates"></a>シーンの更新プログラム

更新が有効になっているシーン (既定値) はメイン ループ反復ごとに自動的に更新されます。  アプリケーションの[ `SceneUpdate` ](https://developer.xamarin.com/api/event/Urho.Scene.SceneUpdate/)にイベント ハンドラーが呼び出されます。

ノードおよびコンポーネントは、無効にすることで、シーンの更新プログラムから除外することができますを参照してください[ `Enabled`](https://developer.xamarin.com/api/member/Urho.Node.Enabled)します。  動作は特定のコンポーネントに依存が、たとえば描画可能なコンポーネントを無効にすることもできます、非表示、ミュート、サウンドの変換元コンポーネントを無効にするときに。 ノードが無効になっている場合は、独自の有効/無効状態に関係なく無効になっているすべてのコンポーネント扱われます。

## <a name="adding-behavior-to-your-components"></a>コンポーネントの動作を追加します。

ゲームを構築する最善の方法は、させる、アクターまたはゲームの要素をカプセル化するコンポーネントです。  これにより、機能に含まれている、その動作にそれを表示するために使用する資産から自動。

コンポーネントの動作を追加する最も簡単な方法では、アクションは、キューし、C# の非同期プログラミングを併用することができますな手順を使用します。  これにより、コンポーネントを非常に高いレベルの動作と何が起こっているかを理解するが簡単です。

または、何が起きているのコンポーネントに (フレーム ベースの動作のセクションで説明) フレームごとに、コンポーネントのプロパティを更新することで制御できます。

### <a name="actions"></a>アクション

動作は、アクションを使用して簡単に非常にノードを追加できます。  アクションはさまざまなノードのプロパティを変更し、時間の期間にわたってそれらを実行または指定したアニメーション曲線と回数をもう一度実行しています。

たとえば、「クラウド」のノード、シーンには、このようなフェードインすることができます。

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

アクションは、不変のオブジェクトが別のオブジェクトを推進するためのアクションを再利用することができます。

一般的な表現形式は、逆の操作を実行するアクションを作成します。

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

次の例は、3 秒の期間にわたってのオブジェクトをフェードインとします。  1 つのアクションを実行することも、後たとえば、最初に、クラウドに移動して非表示にします。

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

両方のアクションを同時に実行する場合は、並列のアクションを使用して、およびすべての操作を並列に実行を提供できます。

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

上記の例では、クラウドは移動し、同時にフェードアウトします。

これらを使用している C# わかります await、直線的に動作を実現するために考慮することができます。

### <a name="basic-actions"></a>基本的な操作

UrhoSharp でサポートされているアクションを次に示します。

* ノードの移動: [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo)、 [ `MoveBy` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveBy)、 [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place)、 [ `BezierTo` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierTo)、 [ `BezierBy` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierBy), [`JumpTo`](https://developer.xamarin.com/api/type/Urho.Actions.JumpTo), [`JumpBy`](https://developer.xamarin.com/api/type/Urho.Actions.JumpBy)
* ノードの回転: [ `RotateTo` ](https://developer.xamarin.com/api/type/Urho.Actions.RotateTo)、 [`RotateBy`](https://developer.xamarin.com/api/type/Urho.Actions.RotateBy)
* ノードのスケール: [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo)、 [`ScaleBy`](https://developer.xamarin.com/api/type/Urho.Actions.ScaleBy)
* ノードのフェード: [ `FadeIn` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeIn)、 [ `FadeTo` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeTo)、 [ `FadeOut` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeOut)、 [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide)、 [`Blink`](https://developer.xamarin.com/api/type/Urho.Actions.Blink)
* 濃淡: [ `TintTo` ](https://developer.xamarin.com/api/type/Urho.Actions.TintTo)、 [`TintBy`](https://developer.xamarin.com/api/type/Urho.Actions.TintBy)
* 時点: [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide)、 [ `Show` ](https://developer.xamarin.com/api/type/Urho.Actions.Show)、 [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place)、 [ `RemoveSelf` ](https://developer.xamarin.com/api/type/Urho.Actions.RemoveSelf)、 [`ToggleVisibility`](https://developer.xamarin.com/api/type/Urho.Actions.ToggleVisibility)
* ループ: [ `Repeat` ](https://developer.xamarin.com/api/type/Urho.Actions.Repeat)、 [ `RepeatForever` ](https://developer.xamarin.com/api/type/Urho.Actions.RepeatForever)、 [`ReverseTime`](https://developer.xamarin.com/api/type/Urho.Actions.ReverseTime)

その他の高度な機能の組み合わせを含める、 [ `Spawn` ](https://developer.xamarin.com/api/type/Urho.Actions.Spawn)と[ `Sequence` ](https://developer.xamarin.com/api/type/Urho.Actions.Sequence)アクション。

### <a name="easing---controlling-the-speed-of-your-actions"></a>イージング - 操作の速度を制御します。

簡略化、アニメーションが展開されより快適なアニメーションをことができますの方法を指示することができます。  既定では、アクションが線形の動作など、 [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo)アクションが非常にロボットの移動が必要があります。  緩やかに変化、移動の開始、加速および緩やかに変化最後に到達する 1 つの動作を変更するイージング アクションに操作をラップすることができます ([`EasyInOut`](https://developer.xamarin.com/api/type/Urho.Actions.EasyInOut))。

たとえばイージング操作では、既存の操作をラップすることによって行います。

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

多くのイージング モードは、次の図は、開始から終了までの時間の期間にわたってで制御する、オブジェクトの値に、さまざまなイージングの種類とその動作を示しています。

![モードのイージング](using-images/easing.png "このグラフは、時間の期間にわたってそれらを管理しているオブジェクトの値に、さまざまなイージングの種類とその動作を示しています。")

### <a name="using-actions-and-async-code"></a>操作と非同期コードを使用してください。

[ `Component` ](https://developer.xamarin.com/api/type/Urho.Component/)サブクラスでは、コンポーネントの動作を準備し、そのドライブの機能を非同期メソッドを導入する必要があります。
C# を使用してこのメソッドを呼び出すよう、 `await` 、プログラムの別の部分からキーワードか、`Application.Start`メソッドや、アプリケーションのユーザーまたはストーリー ポイントへの応答。

例:

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

`Launch`が上記の 3 つのアクション メソッドの開始: ロボットがシーンには、この操作は、0.6 秒の期間にわたって、ノードの場所を変更します。  これが async オプションであるため、これは同時に、呼び出しは、次の命令としてに`MoveRandomly`します。  このメソッドは、任意の場所に並列的に、ロボットの位置に変更されます。  これは 2 つの複合操作を新しい場所への移動を実行することによって実現され、元に戻って位置しロボットが存続する限り、この手順を繰り返します。  おもしろいことにより、ロボットを同時に撮影保持されます。  発砲と、0.1 秒ごとにのみ開始されます。

### <a name="frame-based-behavior-programming"></a>フレーム ベースの動作のプログラミング

オーバーライドすることでアクションを使用する代わりに、フレームごとに、コンポーネントの動作を制御する場合は、このようなです、 [ `OnUpdate` ](https://developer.xamarin.com/api/member/Urho.Component.OnUpdate)のメソッド、 [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component)サブクラスです。  このメソッドは、フレームごとに 1 回呼び出される、ReceiveSceneUpdates プロパティを true に設定する場合にのみ呼び出されます。

作成する方法を次に示します、`Rotator`を回転するノードのノードに、関連付けられているコンポーネント。

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

これは、ノードにこのコンポーネントをアタッチする方法.

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>結合スタイル

プログラミングでは、ファイア アンド フォーゲット スタイルに最適な動作の多くのプログラミングの非同期/アクション ベースのモデルを使用することができますが、フレームごとにもいくつかの更新プログラムのコードを実行するコンポーネントの動作も細かく調整できます。

たとえば、SamplyGame デモでは、これで使用、`Enemy`クラスは、基本的な動作はアクションをエンコードしますが、コンポーネントを持つノードの方向を設定して、ユーザーに向けたにポイントも確実に`Node.LookAt`:

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

## <a name="loading-and-saving-scenes"></a>読み込みとシーンの保存

シーンがロードされ、XML 形式で保存することができます。関数を参照してください。 [ `LoadXml` ](https://developer.xamarin.com/api/member/Urho.Scene.LoadXml)と[ `SaveXML()`](https://developer.xamarin.com/api/member/Urho.Scene.SaveXml)します。 シーンが読み込まれるときに、まず (子ノードおよびコンポーネント) がすべての既存のコンテンツが削除されます。 ノードおよび一時とマークされているコンポーネント、`Temporary`プロパティは保存されません。 シリアライザーは、すべての組み込みのコンポーネントとプロパティを処理するが、カスタム プロパティと、コンポーネントのサブクラスで定義されているフィールドを処理するのに十分なスマートはありません。 ただし、この 2 つの仮想メソッドを提供します。

* [`OnSerialize`](https://developer.xamarin.com/api/member/Urho.Component.OnSerialize) 登録していることができますをカスタムの状態のシリアル化

* [`OnDeserialized`](https://developer.xamarin.com/api/member/Urho.Component.OnDeserialize) 保存された、カスタム状態を入手できます。

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

### <a name="object-prefabs"></a>オブジェクトのプレハブ

だけの読み込みまたはシーン全体を保存していますがゲームの十分な柔軟性の新しいオブジェクトを動的に作成する必要があります。 その一方で、複雑なオブジェクトを作成して、コードでそのプロパティの設定もなります面倒です。 このため、その子ノード、コンポーネント、および属性が含まれるシーン ノードを保存することがもできます。 これらは、グループとして後で簡単にロードできます。  このような保存されているオブジェクトはプレハブとも呼ばれます。 これには、次の 3 つの方法があります。

- 呼び出すコードの中で[ `Node.SaveXml` ](https://developer.xamarin.com/api/member/Urho.Node.SaveXml)ノード
- エディターで、[階層] ウィンドウで、ノードを選択して、「保存ノードとして」[ファイル] メニューから。
- 「ノード」コマンドを使用して`AssetImporter`シーン ノード階層を保存するが、すべてのモデルが (たとえば、入力資産に含まれる Collada ファイル)

シーンには、保存されているノードをインスタンス化するには、呼び出す[ `InstantiateXml()`](https://developer.xamarin.com/api/member/Urho.Scene.InstantiateXml)します。 ノードは、シーンの子として作成されますは自由に親を再指定を後。 位置と、ノードを配置するための回転を指定する必要があります。 次のコードが、プレハブのインスタンスを作成する方法を示して`Ninja.xm`目的の位置と回転をシーンに。

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>イベント

UrhoObjects 多数のイベントを発生させる、これらは、それらを生成するさまざまなクラス (C#) イベントとして表示されます。  だけでなく、C#-イベント ベースのモデルもを使用して、`SubscribeToXXX`メソッドをサブスクライブし、サブスクリプションのトークンを保持することができる後で使用するサブスクリプションを解除します。  違いは、前者はサブスクライブの多くの呼び出し元を許可するより良いラムダ スタイルが使用するアプローチしを簡単に、サブスクリプションの削除を許可する、まだのみ、2 つ目は、1 つは、ですが、中にです。  これらは相互に排他的です。

イベントにサブスクライブするときに、適切なイベント引数を持つ引数を受け取るメソッドを提供する必要があります。

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

イベント、その場合、戻り値を保存への呼び出しからの通知の受信を停止することがありますする`SubscribeTo`メソッドで、Unsubscribe メソッドを呼び出すと。

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

イベント ハンドラーが受信するパラメーターは、厳密に型指定されたイベント引数クラスは各イベントに固有のものをイベントのペイロードが含まれています。

## <a name="responding-to-user-input"></a>ユーザー入力に対応

イベントにサブスクライブし、配信中の入力に応答をキーストロークなどのさまざまなイベントをサブスクライブできます。

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

多くのシナリオでは、シーンの更新ハンドラーが更新されて、それに応じてコードを更新すると、キーの現在の状態を確認するをします。  たとえば、次できますキーボード入力に基づくカメラの位置を更新します。

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

## <a name="resources-assets"></a>リソース (アセット)

リソースには、初期化またはランタイム中に、大容量記憶装置から読み込まれた UrhoSharp のほとんどの機能が含まれます。

- [`Animation`](https://developer.xamarin.com/api/type/Urho.Animation/) -骨格アニメーションの使用
- [`Image`](https://developer.xamarin.com/api/type/Urho.Resources.Image) -さまざまなグラフィック形式で格納されているイメージを表します
- [`Model`](https://developer.xamarin.com/api/type/Urho.Model/) -3D モデル
- [`Material`](https://developer.xamarin.com/api/type/Urho.Material) -モデルを表示するために使用される素材です。
- [`ParticleEffect`](https://developer.xamarin.com/api/type/Urho.ParticleEffect)- [説明](http://urho3d.github.io/documentation/1.4/_particles.html)パーティクル エミッタのしくみを参照してください"[パーティクル](#particles)"の下。
- [`Shader`](https://developer.xamarin.com/api/type/Urho.Shader) -カスタムのシェーダー
- [`Sound`](https://developer.xamarin.com/api/type/Urho.Audio.Sound) サウンドを再生するを参照してください"[サウンド](#sound)"の下。
- [`Technique`](https://developer.xamarin.com/api/type/Urho.Technique/) -素材のレンダリング テクニック
- [`Texture2D`](https://developer.xamarin.com/api/type/Urho.Urho2D.Texture2D/) -2 D テクスチャ
- [`Texture3D`](https://developer.xamarin.com/api/type/Urho.Texture3D/) -3D テクスチャ
- [`TextureCube`](https://developer.xamarin.com/api/type/Urho.TextureCube/) のキューブ テクスチャ
- `XmlFile`

これらの管理し、によって読み込まれる、 [ `ResourceCache` ](https://developer.xamarin.com/api/type/Urho.Resources.ResourceCache/)サブシステム (として利用可能な[ `Application.ResourceCache` ](https://developer.xamarin.com/api/property/Urho.Application.ResourceCache/))。

リソース自体は、登録済みのリソース ディレクトリまたはファイルのパッケージ関連のファイル パスによって識別されます。 既定では、エンジンは、リソース ディレクトリを登録します`Data`と`CoreData`、またはパッケージ`Data.pak`と`CoreData.pak`存在する場合。

リソースの読み込みに失敗した場合は、エラー ログに記録され、null 参照が返されます。

次の例では、リソースのキャッシュからリソースをフェッチする場合の一般的な方法を示します。  ここでは、UI 要素のテクスチャ、これを使用して、`ResourceCache`プロパティから、`Application`クラス。

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

リソースを手動で作成したもとディスクから読み込まれていた場合、リソースのキャッシュに格納されていることができます。

リソースの種類ごとにメモリの予算を設定できます。 リソースは、許可されているより多くのメモリを消費する場合、最も古いリソースが削除されます使用されていない場合、キャッシュからできなくなります。 既定では、メモリの予算は無制限に設定されます。

### <a name="bringing-3d-models-and-images"></a>3D モデルおよびイメージ

可能であれば、既存のファイル形式を使用し、モデルのようにどうしても必要な場合にのみ、カスタム ファイル形式を定義しようと Urho3D (*.mdl) とアニメーションの (*.ani)。 Urho がのコンバーターを提供するこれらの種類の資産の[AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) fbx、dae、3 ds、および obj など、多くの一般的な 3D 形式を使用することができます。

便利なアドインの Blender も[ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) Urho3D に適した形式で、Blender 資産をエクスポートすることができます。

### <a name="background-loading-of-resources"></a>リソースのバック グラウンド読み込み

通常のいずれかを使用してリソースを要求するときに、`ResourceCache`の`Get`メソッドでは、すべての必要な手順については、数ミリ秒かかります、メイン スレッドですぐに読み込まれる (ディスクからファイルを読み込んで、データの解析、GPU にアップロードするために必要な場合) と、その結果、フレーム レートが低下します。

事前にどのようなリソース必要がある場合は、呼び出すことによって、バック グラウンド スレッドに読み込まれることを要求できます`BackgroundLoadResource()`します。 バック グラウンド読み込みイベントを購読するを使用して、`SubscribeToResourceBackgroundLoaded`メソッド。 読み込みが成功または失敗に実際には通知されます。 バック グラウンド スレッドに移動することがあります、読み込みプロセスの一部のみ、リソースによっては、たとえばフィニッシュの GPU のアップロード手順が常にメイン スレッドで発生する必要です。 バック グラウンド読み込みがキューにリソースのメソッドを読み込み、リソースのいずれかを呼び出した場合、メイン スレッドが停止の読み込みが完了するまでに注意してください。

機能を読み込む非同期シーン`LoadAsync()`と`LoadAsyncXML()`シーンの内容を読み込むより前のリソースをバック グラウンド読み込みオプションがあります。 のみを指定して、シーンを変更することがなく、リソースの読み込みにも使用できます、`LoadMode.ResourcesOnly`します。 これにより、高速なインスタンス化用のシーンまたはオブジェクトのプレハブ ファイルを準備します。

最大時間 (ミリ秒) を各フレームに費やされた最後に読み込まれたリソースを設定して構成できるバック グラウンドの終了、`FinishBackgroundResourcesMs`プロパティを`ResourceCache`します。

<a name="sound"/>

## <a name="sound"></a>サウンド

サウンドがゲームのプレイの重要な部分と UrhoSharp フレームワークは、ゲームでサウンドの再生方法を提供します。  アタッチすることでサウンドを再生します。 [`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource/)
コンポーネントを[ `Node` ](https://developer.xamarin.com/api/type/Urho.Node)とし、リソースの名前付きのファイルを再生します。

これは、これをどのようにです。

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>パーティクル

パーティクルは、アプリケーションへのシンプルで低価格. 効果をいくつかの追加の簡単な方法を提供します。  などのツールを使用して、PEX の形式で格納されるパーティクルを処理できます[ http://onebyonedesign.com/flash/particleeditor/](http://onebyonedesign.com/flash/particleeditor/)します。

パーティクルは、ノードに追加できるコンポーネントです。  ノードを呼び出す必要がある`CreateComponent<ParticleEmitter2D>`パーティクルを作成し、2D 効果に Effect プロパティを設定してパーティクルを構成する方法は、リソースのキャッシュから読み込まれます。

たとえば、ヒットした際に、展開としてレンダリングされるいくつかのパーティクルを表示する、コンポーネントで、このメソッドを呼び出すことができます。

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

上記のコードは、現在のコンポーネントにアタッチされている展開ノードを作成、この展開のノード内で 2D パーティクル エミッタを作成し、Effect プロパティを設定して構成します。  小さくすると、ノードを拡大/縮小する 1 つ使用し、いずれかの 0.5 秒間にそのサイズが離れるを使用する、2 つのアクションを実行します。  画面からパーティクル効果も削除されますと、展開を削除します。

球のテクスチャを使用する場合は、次のように上記のパーティクルがレンダリングされます。

![球体テクスチャ パーティクル](using-images/image-1.png "球体テクスチャを使用する場合、次のように上記のパーティクルを表示します")

これは、どの色むらテクスチャを使用する場合。

![ボックス テクスチャ パーティクル](using-images/image-2.png "これは、どの色むらテクスチャを使用する場合")

## <a name="multithreading-support"></a>マルチ スレッドのサポート

UrhoSharp は、シングル スレッド ライブラリです。  つまり、バック グラウンド スレッドから UrhoSharp のメソッドの呼び出しをしないようにするには、またはリスクのアプリケーションの状態が破損し、アプリケーションがクラッシュする可能性があります。

使用することができます、バック グラウンドでいくつかのコードを実行し、メイン UI に Urho コンポーネントを更新する場合、 [`Application.InvokeOnMain(Action)`](https://developer.xamarin.com/api/member/Urho.Application.InvokeOnMain)
メソッドをオーバーライドします。  さらに、await (C#) を使用し、.NET のタスクが、コードを適切なスレッドで実行されることを確認するための Api。

## <a name="urhoeditor"></a>UrhoEditor

Urho エディターをダウンロードするには、プラットフォームから、 [Urho web サイト](http://urho3d.github.io/)ダウンロードに移動し、最新バージョンを選択します。

## <a name="copyrights"></a>著作権

このドキュメントは、Xamarin Inc から元のコンテンツが含まれていますが、Urho3D プロジェクトのオープン ソース ドキュメントから幅広くを描画し、Cocos2D プロジェクトのスクリーン ショットが含まれています。
