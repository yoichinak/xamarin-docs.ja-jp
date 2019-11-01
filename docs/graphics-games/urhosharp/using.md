---
title: UrhoSharp を使用した3D ゲームの構築
description: このドキュメントでは、シーン、コンポーネント、図形、カメラ、アクション、ユーザー入力、サウンドなどを説明する UrhoSharp の概要について説明します。
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: cb4e524977b53f1a17552298c509d43ccf460f08
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73011652"
---
# <a name="using-urhosharp-to-build-a-3d-game"></a>UrhoSharp を使用した3D ゲームの構築

最初のゲームを作成する前に、「シーンの設定方法」、「リソースの読み込み方法 (アートワークを含む)」、および「ゲーム用の簡単な対話を作成する方法」の基本に十分理解をお勧めします。

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>シーン、ノード、コンポーネント、およびカメラ

シーンモデルは、コンポーネントベースのシーングラフとして記述できます。 シーンは、シーンノードの階層で構成され、ルートノードから始まります。これはシーン全体も表します。 各 `Node` には、3D 変換 (位置、回転と小数点以下桁数)、名前、ID、および任意の数のコンポーネントがあります。  コンポーネントによってノードが有効になり、視覚表現 (`StaticModel`) を追加したり、サウンド (`SoundSource`) を出力したり、衝突の境界を提供したりすることができます。

[Urho エディター](#urhoeditor)を使用してシーンとセットアップノードを作成したり、 C#コードから操作を実行したりすることができます。  このドキュメントでは、コードを使用して設定を行います。これは、画面に表示するために必要な要素を示すためです。

シーンの設定に加えて、`Camera`を設定する必要があります。これは、ユーザーに表示される内容を決定するものです。

### <a name="setting-up-your-scene"></a>シーンの設定

通常は、次のようなフォームを開始メソッドとして作成します。

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

3D オブジェクトのレンダリング、サウンドの再生、物理およびスクリプト化されたロジックの更新はすべて、`CreateComponent<T>()`を呼び出すことによってノードにさまざまなコンポーネントを作成することで有効になります。  たとえば、次のように node と light コンポーネントを設定します。

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

"`DirectionalLight`" という名前のノードの上に作成し、方向を設定しましたが、他には何もありません。  次に、`CreateComponent`を使用して、上のノードを光出力ノードに変換します。これには、`Light` コンポーネントをアタッチします。

```csharp
var light = lightNode.CreateComponent<Light>();
```

`Scene` 自体に作成されるコンポーネントには、シーン全体の機能を実装するための特別な役割があります。 これらは、他のすべてのコンポーネントの前に作成する必要があり、次のものが含まれます。

 `Octree`: 空間パーティション分割と高速可視性クエリを実装します。 この3D オブジェクトを使用しない場合は、レンダリングできません。
 `PhysicsWorld`: 物理的シミュレーションを実装します。 `RigidBody` や `CollisionShape` などの物理コンポーネントは、この機能がないと正常に機能しません。
 `DebugRenderer`: デバッグジオメトリレンダリングを実装します。

`Light`、`Camera`、`StaticModel` などの通常のコンポーネント
`Scene`に直接作成することはできず、子ノードにする必要があります。

ライブラリには、さまざまなコンポーネントが用意されています。ノードにアタッチすると、ユーザーに表示される要素 (モデル)、サウンド、固定ボディ、衝突図形、カメラ、ライトソース、パーティクル発信器などが有効になります。

### <a name="shapes"></a>図形

便宜上、Urho 名前空間では、さまざまな図形を単純なノードとして使用できます。  これには、ボックス、球体、コーン、円柱、平面が含まれます。

### <a name="camera-and-viewport"></a>カメラとビューポート

電球と同様に、カメラはコンポーネントであるため、次のようにコンポーネントをノードにアタッチする必要があります。

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

これで、カメラが作成され、3D ワールドにカメラが配置されました。次の手順では、これが使用するカメラであることを `Application` に伝えます。これは、次のコードを使用して行います。

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

これで、作成した結果を確認できるようになります。

### <a name="identification-and-scene-hierarchy"></a>識別とシーンの階層

ノードとは異なり、コンポーネントには名前がありません。同じノード内のコンポーネントは、その種類によってのみ識別されます。また、ノードのコンポーネントリストでは、作成順に入力されます。たとえば、次のように、`lightNode` オブジェクトから `Light` コンポーネントを取得できます。

```csharp
var myLight = lightNode.GetComponent<Light>();
```

使用できる `IList<Component>` を返す `Components` プロパティを取得して、すべてのコンポーネントの一覧を取得することもできます。

作成されると、ノードとコンポーネントは両方ともシーングローバルの整数 Id を取得します。 シーンからクエリを実行するには、関数 `GetNode(uint id)` と `GetComponent(uint id)`を使用します。 これは、名前ベースの再帰的なシーンノードクエリの実行よりもはるかに高速です。

エンティティまたは game オブジェクトの概念は組み込まれていません。むしろ、プログラマはノード階層を決定し、任意のノードでスクリプト化されたロジックを配置する必要があります。 通常、3D ワールドのフリー移動オブジェクトは、ルートノードの子として作成されます。 ノードは、`CreateChild`を使用して名前の有無にかかわらず作成できます。 ノード名の一意性は強制されません。

階層化された構成がある場合は、子ノードを作成するために (コンポーネントには独自の3D 変換がないため、実際に必要になります)、これが推奨されます。

たとえば、文字が手の形でオブジェクトを保持していた場合、オブジェクトは独自のノードを持つ必要があります。このノードは、文字の手の形 (`Node`) を親として使用されます。  例外は物理 `CollisionShape`であり、ノードに対して個別に offsetted およびローテーションできます。

`Scene`の独自の変換は、子ノードのワールド派生変換を計算するときに、最適化として意図的に無視されるため、変更を行っても効果はなく、そのままにしておく必要があります (原点での位置、回転なし、スケーリングなし)。

`Scene` ノードは自由に親できます。 対照的に、コンポーネントは、接続先のノードに常に属しており、ノード間で移動することはできません。 ノードとコンポーネントはどちらも、親を経由せずにこれを実現するための `Remove` の機能を提供します。 ノードが削除されると、対象のノードまたはコンポーネントに対する操作は、その関数を呼び出した後も安全です。

また、シーンに属していない `Node` を作成することもできます。 これは、ロードまたは保存できるシーンでカメラを移動する場合などに便利です。そのため、カメラは実際のシーンと一緒に保存されず、シーンが読み込まれるときに破棄されません。 ただし、接続されていないノードに geometry、物理、またはスクリプトコンポーネントを作成し、後でシーンに移動すると、これらのコンポーネントが正常に動作しなくなることに注意してください。

### <a name="scene-updates"></a>シーンの更新

更新が有効になっているシーン (既定) は、各メインループの反復処理で自動的に更新されます。  アプリケーションの `SceneUpdate` イベントハンドラーが呼び出されます。

ノードとコンポーネントを無効にすることによって、シーンの更新から除外することができます。「`Enabled`」を参照してください。  動作は特定のコンポーネントによって異なりますが、たとえば、描画コンポーネントを無効にすると、そのコンポーネントは見えなくなります。一方、サウンドソースコンポーネントを無効にすると、そのコンポーネントがミュートされます。 ノードが無効になっている場合、そのすべてのコンポーネントは、独自の有効/無効状態にかかわらず、無効として扱われます。

## <a name="adding-behavior-to-your-components"></a>コンポーネントに動作を追加する

ゲームを構築する最善の方法は、ゲームでアクターまたは要素をカプセル化する独自のコンポーネントを作成することです。  これにより、機能は、表示に使用されるアセットから動作に自己完結します。

コンポーネントに動作を追加する最も簡単な方法は、アクションを使用することです。これは、非同期プログラミングでC#キューに登録し、結合することができます。  これにより、コンポーネントの動作を非常に高度なものにして、何が起こっているかを簡単に理解できるようになります。

または、各フレームのコンポーネントのプロパティを更新して (「フレームベースの動作」セクションで説明)、コンポーネントの動作を正確に制御できます。

### <a name="actions"></a>アクション

アクションを使用して、ノードに動作を非常に簡単に追加できます。  アクションでは、さまざまなノードプロパティを変更して一定期間にわたって実行したり、指定されたアニメーション曲線で複数回繰り返すことができます。

たとえば、シーンの "クラウド" ノードについて考えてみましょう。次のようにフェードできます。

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

アクションは、異なるオブジェクトを運転するアクションを再利用できるようにする、変更できないオブジェクトです。

一般的な表現方法は、逆の操作を実行するアクションを作成することです。

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

次の例では、3秒間にわたってオブジェクトをフェードします。  また、1つの操作を別のアクションの後に実行することもできます。たとえば、最初にクラウドを移動してから非表示にすることができます。

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

両方のアクションを同時に実行する場合は、並列アクションを使用して、実行するすべてのアクションを並行して指定できます。

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

上の例では、クラウドの移動とフェードアウトが同時に行われます。

これらは`await`を使用してC#いることがわかります。これにより、実行する動作を直線的に考えることができます。

### <a name="basic-actions"></a>基本的な操作

UrhoSharp でサポートされているアクションを次に示します。

- ノードの移動: `MoveTo`、`MoveBy`、`Place`、`BezierTo`、`BezierBy`、`JumpTo`、`JumpBy`
- ノードの回転: `RotateTo`、`RotateBy`
- ノードのスケーリング: `ScaleTo`、`ScaleBy`
- ノードのフェード: `FadeIn`、`FadeTo`、`FadeOut`、`Hide`、`Blink`
- 色合い: `TintTo`、`TintBy`
- Instants: `Hide`、`Show`、`Place`、`RemoveSelf`、`ToggleVisibility`
- ループ: `Repeat`、`RepeatForever`、`ReverseTime`

その他の高度な機能としては、`Spawn` と `Sequence` アクションの組み合わせがあります。

### <a name="easing---controlling-the-speed-of-your-actions"></a>イージング-アクションの速度の制御

イージングは、アニメーションがどのように展開されるかを指示する方法であり、アニメーションをもっと快適にすることができます。  既定では、アクションには線形の動作があります。たとえば、`MoveTo` アクションには、非常にロボットの動きがあります。  アクションをイージングアクションにラップして、動作を変更することができます。たとえば、動きを徐々に開始することができます。また、処理速度を加速し、終了に近づいています (`EasyInOut`)。

これを行うには、既存のアクションをイージングアクションにラップします。次に例を示します。

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

さまざまなイージングモードがあります。次のグラフは、一定期間内に制御しているオブジェクトの値に対するさまざまなイージングの種類とその動作を示しています。

![イージングモード](using-images/easing.png "このグラフは、さまざまなイージングの種類と、それらが期間にわたって制御しているオブジェクトの値に対する動作を示しています。")

### <a name="using-actions-and-async-code"></a>アクションと非同期コードの使用

`Component` サブクラスでは、コンポーネントの動作を準備し、その機能を駆動する非同期メソッドを導入する必要があります。
次に、プログラムの別の部分C#の`await`キーワードを使用して、`Application.Start`メソッド、またはアプリケーションのユーザーまたはストーリーポイントへの応答として、このメソッドを呼び出します。

(例:

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

上記の `Launch` メソッドでは、次の3つのアクションが開始されています。ロボットはシーンに表示されます。この操作により、ノードの位置が0.6 秒の間に変更されます。  これは非同期オプションであるため、次の命令として同時に実行されます。これは `MoveRandomly`の呼び出しです。  このメソッドは、ロボットの位置をランダムな場所に並列的に変更します。  これを実現するには、2つの複合アクションを実行し、新しい場所に移動して元の位置に戻り、ロボットが生きている間はこれを繰り返します。  さらに興味深いものを作成するために、ロボットは同時に学習を続けます。  この撮影は0.1 秒ごとに開始されます。

### <a name="frame-based-behavior-programming"></a>フレームベースの動作プログラミング

アクションを使用するのではなく、フレームごとにコンポーネントの動作を制御する場合は、`Component` サブクラスの `OnUpdate` メソッドをオーバーライドすることをお勧めします。  このメソッドはフレームごとに1回呼び出され、ReceiveSceneUpdates プロパティを true に設定した場合にのみ呼び出されます。

次の例では、ノードにアタッチされる `Rotator` コンポーネントを作成し、ノードを回転させる方法を示します。

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
            RotationSpeed.X  timeStep,
            RotationSpeed.Y  timeStep,
            RotationSpeed.Z  timeStep),
            TransformSpace.Local);
    }
}
```

このコンポーネントをノードにアタッチする方法を次に示します。

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>スタイルの結合

非同期/アクションベースのモデルを使用して、起動と非表示のプログラミングスタイルに適した動作の多くをプログラミングすることができますが、コンポーネントの動作を微調整して、各フレームでいくつかの更新コードを実行することもできます。

たとえば、SamplyGame demo では、これは `Enemy` クラスで使用されます。基本動作はアクションを使用してエンコードされますが、コンポーネントは `Node.LookAt`を使用してノードの方向を設定することによってユーザーに指示することもできます。

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

## <a name="loading-and-saving-scenes"></a>シーンの読み込みと保存

シーンは XML 形式で読み込んで保存することができます。関数 `LoadXml` と `SaveXML`を参照してください。 シーンが読み込まれると、それに含まれるすべての既存のコンテンツ (子ノードおよびコンポーネント) が最初に削除されます。 `Temporary` プロパティで一時とマークされているノードおよびコンポーネントは保存されません。 シリアライザーは、すべての組み込みコンポーネントとプロパティを処理しますが、コンポーネントサブクラスで定義されたカスタムプロパティおよびフィールドを処理するのに十分なスマートではありません。 ただし、これには2つの仮想メソッドが用意されています。

 シリアル化のカスタム状態を登録できる `OnSerialize`

 保存したカスタム状態を取得できる `OnDeserialized` ます。

通常、カスタムコンポーネントは次のようになります。

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

### <a name="object-prefabs"></a>オブジェクト prefabs

シーン全体の読み込みまたは保存だけでは、新しいオブジェクトを動的に作成する必要があるゲームには柔軟性がありません。 一方、複雑なオブジェクトを作成し、そのプロパティをコードで設定するのも面倒です。 このため、子ノード、コンポーネント、および属性を含むシーンノードを保存することもできます。 これらは後でグループとして読み込むことができます。  このような保存されたオブジェクトは、多くの場合、prefab と呼ばれます。 これには、次の 3 つの方法があります。

- コード内で、ノードの `Node.SaveXml` を呼び出します。
- エディターで、[階層] ウィンドウのノードを選択し、[ファイル] メニューから [ノードに名前を付けて保存] を選択します。
- `AssetImporter`で "node" コマンドを使用します。これにより、シーンノード階層と、入力資産に含まれるすべてのモデルが保存されます (例として、 "同一の Ada ファイル"

保存したノードをシーンにインスタンス化するには、`InstantiateXml`を呼び出します。 ノードはシーンの子として作成されますが、それ以降は自由に親ことができます。 ノードを配置するための位置と回転を指定する必要があります。 次のコードは、必要な位置と回転を使用して、prefab `Ninja.xm` をシーンにインスタンス化する方法を示しています。

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>イベント

UrhoObjects は多数のイベントを発生させ、それらC#を生成するさまざまなクラスのイベントとして表示されます。  ベースのイベントモデルC#に加えて、後で購読を解除するために使用できるサブスクリプショントークンをサブスクライブして保持できるようにする`SubscribeToXXX`メソッドを使用することもできます。  違いは、前者では多くの呼び出し元がサブスクライブできるのに対し、2番目の呼び出し側では1つしか許可されていませんが、それでも、より良いラムダ形式のアプローチを使用できます。また、サブスクリプションを簡単に削除できるようにすることもできます。  これらは相互に排他的です。

イベントをサブスクライブするときは、適切なイベント引数を持つ引数を受け取るメソッドを指定する必要があります。

たとえば、次のようにマウスボタンダウンイベントをサブスクライブします。

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

ラムダスタイルを使用する場合:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

場合によっては、イベントに対する通知の受信を停止し、`SubscribeTo` メソッドの呼び出しから戻り値を保存して、そのメソッドでアンサブスクライブメソッドを呼び出す必要があります。

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

イベントハンドラーによって受信されるパラメーターは、厳密に型指定されたイベント引数クラスで、各イベントに固有のもので、イベントペイロードが含まれています。

## <a name="responding-to-user-input"></a>ユーザー入力への応答

イベントをサブスクライブし、配信される入力に応答することによって、キーストロークを押すなど、さまざまなイベントをサブスクライブできます。

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

しかし、多くのシナリオでは、シーン更新ハンドラーが更新中のキーの現在の状態を確認し、それに応じてコードを更新する必要があります。  たとえば、次のコードを使用して、キーボード入力に基づいてカメラの場所を更新できます。

```csharp
protected override void OnUpdate(float timeStep)
{
    Input input = Input;
    // Movement speed as world units per second
    const float moveSpeed = 4.0f;
    // Read WASD keys and move the camera scene node to the
    // corresponding direction if they are pressed
    if (input.GetKeyDown(Key.W))
        CameraNode.Translate(Vector3.UnitY  moveSpeed  timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.S))
        CameraNode.Translate(new Vector3(0.0f, -1.0f, 0.0f)  moveSpeed  timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.A))
        CameraNode.Translate(new Vector3(-1.0f, 0.0f, 0.0f)  moveSpeed  timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.D))
        CameraNode.Translate(Vector3.UnitX  moveSpeed  timeStep, TransformSpace.Local);
}
```

## <a name="resources-assets"></a>リソース (アセット)

リソースには、初期化または実行時に大容量記憶装置から読み込まれた UrhoSharp のほとんどが含まれます。

- `Animation`-スケルトンアニメーションに使用されます
- `Image`-さまざまなグラフィック形式で格納されている画像を表します。
- `Model`-3D モデル
- モデルのレンダリングに使用される `Material` 素材。
- `ParticleEffect`- では、パーティクルエミッタのしくみ[について説明](https://urho3d.github.io/documentation/1.4/_particles.html)します。次の「[パーティクル](#particles)」を参照してください。
- `Shader`-カスタムシェーダー
- `Sound`-再生されるサウンド、下記の「[サウンド](#sound)」を参照してください。
- `Technique` マテリアルレンダリング技法
- `Texture2D` 2D テクスチャ
- `Texture3D`-3D テクスチャ
- `TextureCube`-キューブのテクスチャ
- `XmlFile`

これらは、`ResourceCache` サブシステム (`Application.ResourceCache`として使用可能) によって管理および読み込まれます。

リソース自体は、登録されているリソースディレクトリまたはパッケージファイルを基準として、ファイルパスによって識別されます。 既定では、エンジンは `Data` と `CoreData`のリソースディレクトリを登録します。または、パッケージが存在する場合は、パッケージ `Data.pak` および `CoreData.pak` します。

リソースの読み込みに失敗した場合は、エラーがログに記録され、null 参照が返されます。

次の例は、リソースキャッシュからリソースをフェッチする一般的な方法を示しています。  このケースでは、UI 要素のテクスチャとして、`Application` クラスの `ResourceCache` プロパティを使用します。

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

リソースは手動で作成し、ディスクから読み込まれたかのようにリソースキャッシュに格納することもできます。

リソースの種類ごとにメモリの予算を設定できます。リソースが許可されている量よりも多くのメモリを消費している場合、使用されなくなったリソースはキャッシュから削除されます。 既定では、メモリの予算は無制限に設定されています。

### <a name="bringing-3d-models-and-images"></a>3D モデルとイメージの導入

Urho3D は、可能な限り既存のファイル形式の使用を試み、モデル (mdl) やアニメーション (ani) などの必要がある場合にのみ、カスタムファイル形式を定義します。 これらの種類の資産では、Urho は、fbx、dae、3ds、obj などの多くの一般的な3D 形式を使用できるコンバーター [AssetImporter](https://urho3d.github.io/documentation/1.4/_tools.html)を提供します。

また、Blender [https://github.com/reattiva/Urho3D-Blender](https://github.com/reattiva/Urho3D-Blender)用の便利なアドインも用意されており、Urho3D に適した形式で Blender アセットをエクスポートできます。

### <a name="background-loading-of-resources"></a>リソースのバックグラウンド読み込み

通常、`ResourceCache`の `Get` メソッドのいずれかを使用してリソースを要求する場合は、メインスレッドにすぐに読み込まれます。必要なすべての手順 (ディスクからファイルを読み込む、データを解析し、必要に応じて GPU にアップロード) を実行できます。このため、フレームレートが削除されます。

必要なリソースを事前に把握している場合は、`BackgroundLoadResource`を呼び出すことによって、それらのリソースをバックグラウンドスレッドに読み込むように要求できます。 `SubscribeToResourceBackgroundLoaded` メソッドを使用して、リソースのバックグラウンドで読み込まれたイベントをサブスクライブできます。 読み込みが実際に成功したか失敗したかがわかります。 リソースによっては、読み込みプロセスの一部だけがバックグラウンドスレッドに移動されることがあります。たとえば、完了した GPU アップロード手順は、常にメインスレッドで実行する必要があります。 バックグラウンド読み込みのためにキューに登録されているリソースに対してリソースの読み込みメソッドのいずれかを呼び出すと、その読み込みが完了するまでメインスレッドは停止します。

非同期シーン読み込み機能 `LoadAsync` および `LoadAsyncXML` には、シーンコンテンツの読み込みに進む前に、まずリソースをバックグラウンドで読み込むオプションがあります。 また、`LoadMode.ResourcesOnly`を指定することによって、シーンを変更せずにリソースを読み込むこともできます。 これにより、がインスタンス化を迅速に実行できるようにシーンまたはオブジェクトの prefab ファイルを準備できます。

最後に、`ResourceCache`の [`FinishBackgroundResourcesMs`] プロパティを設定することによって、バックグラウンドで読み込まれたリソースの終了時に各フレームに費やした最大時間 (ミリ秒単位) を構成できます。

<a name="sound"/>

## <a name="sound"></a>鳴らす

サウンドはゲームプレイの重要な部分であり、UrhoSharp フレームワークはゲームでサウンドを再生する手段を提供します。  サウンドを再生するには、`SoundSource` を添付します。
コンポーネントを `Node` し、リソースから名前付きファイルを再生します。

これは、次のように行われます。

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>パーティクル

パーティクルを使用すると、シンプルで安価な効果をアプリケーションに簡単に追加できます。  [http://onebyonedesign.com/flash/particleeditor/](http://onebyonedesign.com/flash/particleeditor/)のようなツールを使用して、PEX 形式で格納されているパーティクルを使用できます。

パーティクルは、ノードに追加できるコンポーネントです。  ノードの `CreateComponent<ParticleEmitter2D>` メソッドを呼び出して、パーティクルを作成した後、リソースキャッシュから読み込まれる2D 効果に Effect プロパティを設定してパーティクルを構成する必要があります。

たとえば、コンポーネントに対してこのメソッドを呼び出して、ヒットしたときに爆発的に表示されるいくつかのパーティクルを表示できます。

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

上のコードでは、現在のコンポーネントに接続されている爆発ノードが作成されます。この爆発ノード内では、2D パーティクルエミッタを作成し、Effect プロパティを設定して構成します。  2つのアクションを実行します。1つはノードを小さくするようにスケーリングするアクションで、もう1つは0.5 秒間そのサイズのままにするアクションです。  次に、爆発を削除します。これにより、画面からパーティクル効果も削除されます。

前のパーティクルは、球のテクスチャを使用するときに、次のようにレンダリングされます。

![球体テクスチャを持つパーティクル](using-images/image-1.png "上のパーティクルは、球のテクスチャを使用すると、このようにレンダリングされます。")

これは、むらのないテクスチャを使用すると、次のようになります。

![ボックステクスチャを持つパーティクル](using-images/image-2.png "これは、むらのないテクスチャを使用した場合に見られます。")

## <a name="multi-threading-support"></a>マルチスレッドサポート

UrhoSharp はシングルスレッドライブラリです。  これは、バックグラウンドスレッドから UrhoSharp でメソッドを呼び出さないようにするか、アプリケーションの状態を壊す可能性があり、アプリケーションをクラッシュさせる可能性があることを意味します。

バックグラウンドでいくつかのコードを実行し、メイン UI で Urho コンポーネントを更新する場合は、を使用でき `Application.InvokeOnMain(Action)`
メソッドをオーバーライドします。  また、await と .NET C#タスク api を使用して、コードが適切なスレッドで実行されるようにすることもできます。

## <a name="urhoeditor"></a>UrhoEditor

プラットフォームの Urho エディターは、 [Urho の Web サイト](http://urho3d.github.io/)からダウンロードできます。 [ダウンロード] にアクセスして、最新バージョンを選択してください。

## <a name="copyrights"></a>著作権

このドキュメントには、Xamarin Inc. の元のコンテンツが含まれていますが、Urho3D プロジェクトのオープンソースドキュメントから広く使用されており、Cocos2D プロジェクトのスクリーンショットが含まれています。
