---
title: モノゲームでの頂点を使用した3D グラフィックスの描画
description: モノゲームでは、頂点の配列を使用して、3D オブジェクトをポイント単位でどのようにレンダリングするかを定義できます。 ユーザーは、頂点配列を利用して動的なジオメトリを作成し、特殊効果を実装し、カリングによってレンダリングの効率を向上させることができます。
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: dfb03815f8642519cecf49ab7b626b9575821af1
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937632"
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>モノゲームでの頂点を使用した3D グラフィックスの描画

_モノゲームでは、頂点の配列を使用して、3D オブジェクトをポイント単位でどのようにレンダリングするかを定義できます。ユーザーは、頂点配列を利用して動的なジオメトリを作成し、特殊効果を実装し、カリングによってレンダリングの効率を向上させることができます。_

[レンダリングモデルのガイド](~/graphics-games/monogame/3d/part1.md)に目を通しているユーザーは、モノゲームで3d モデルをレンダリングすることに慣れています。 クラスは、 `Model` ファイルで定義されているデータ (fbx など) を操作するときや、静的データを処理するときに、3d グラフィックスを効果的にレンダリングするための方法です。 一部のゲームでは、実行時に動的に3D ジオメトリを定義または操作する必要があります。 このような場合は、*頂点*の配列を使用して、ジオメトリを定義およびレンダリングできます。 頂点は、ジオメトリを定義するために使用される順序付けられたリストの一部である、3D 空間内のポイントの一般的な用語です。 通常、頂点は、一連の三角形を定義するように並べられています。

頂点を使用して3D オブジェクトを作成する方法を視覚化するには、次の球体を考えてみましょう。

![頂点を使用して3D オブジェクトを作成する方法を視覚化するには、次の球体を検討してください。](part2-images/image1.png)

上に示すように、球は、複数の三角形で明確に構成されています。 球のワイヤーフレームを表示して、頂点がどのように接続して三角形を形成するかを確認できます。

![球のワイヤーフレームを表示して、頂点がどのようにつながるかを確認します。](part2-images/image2.png)

このチュートリアルでは、次のトピックについて説明します。

- プロジェクトの作成
- 頂点の作成
- 追加 (描画コードを)
- テクスチャを使用したレンダリング
- テクスチャ座標の変更
- モデルを使用した頂点のレンダリング

完成したプロジェクトには、頂点配列を使用して描画されるチェッカーが含まれます。

![完成したプロジェクトには、頂点配列を使用して描画されるチェッカーが含まれます](part2-images/image3.png)

## <a name="creating-a-project"></a>プロジェクトの作成

まず、開始点として機能するプロジェクトをダウンロードします。 [ここで](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelrenderingmg/)は、モデルプロジェクトを使用します。

ダウンロードして解凍したら、プロジェクトを開いて実行します。 画面上に6つのロボットモデルが描画されていることが予想されます。

![画面上に描画されている6つのロボットモデル](part2-images/image4.png)

このプロジェクトの終わりまでに、独自のカスタム頂点レンダリングをロボットと組み合わせます `Model` 。そのため、ロボットレンダリングコードは削除しません。 代わりに、 `Game1.Draw` 6 つのロボットの描画を削除するための呼び出しをクリアするだけです。 これを行うには、 **Game1.cs**ファイルを開き、 `Draw` メソッドを見つけます。 次のコードが含まれるように変更します。

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

これにより、ゲームに空のブルースクリーンが表示されます。

![これにより、ゲームに空のブルースクリーンが表示されます。](part2-images/image5.png)

## <a name="creating-the-vertices"></a>頂点の作成

ここでは、ジオメトリを定義する頂点の配列を作成します。 このチュートリアルでは、3D 平面 (航空機ではなく、3D 空間の正方形) を作成します。 平面には4つの辺と4つのコーナーがありますが、それぞれが3つの頂点を必要とする2つの三角形で構成されます。 そのため、合計6つのポイントを定義します。

ここまでは頂点について一般的に説明してきましたが、モノゲームには頂点に使用できる標準の構造体が用意されています。

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

各型の名前は、それに含まれるコンポーネントを示します。 たとえば、には `VertexPositionColor` 位置と色の値が含まれます。 各コンポーネントについて見てみましょう。

- Position –すべての頂点型には、コンポーネントが含ま `Position` れます。 値は、 `Position` 頂点が3d 空間 (X、Y、Z) に配置される場所を定義します。
- Color –頂点は、必要に応じて、 `Color` カスタムの色合いを実行する値を指定できます。
- Normal –オブジェクトのサーフェイスがどのように接しているかを法線で定義します。 光源は、光を使用してオブジェクトをレンダリングする場合に必要です。これは、表面が接している方向が、受け取ったライトの量に影響するからです。 通常、法線は、長さが1の3D ベクトルである、通常は*単位ベクター*として指定されます。
- テクスチャ–テクスチャはテクスチャ座標を表します。つまり、テクスチャのどの部分を特定の頂点に表示するかを指定します。 テクスチャを使用して3D オブジェクトをレンダリングする場合は、テクスチャ値が必要です。 テクスチャ座標は正規化された座標で、値は 0 ~ 1 の範囲になります。 テクスチャ座標については、このガイドの後半で詳しく説明します。

航空機はフロアとして機能します。レンダリングの実行時にテクスチャを適用したいので、型を使用して `VertexPositionTexture` 頂点を定義します。

まず、メンバーを Game1 クラスに追加します。

```csharp
VertexPositionTexture[] floorVerts;
```

次に、で頂点を定義 `Game1.Initialize` します。 この記事の前半で参照されているテンプレートにはメソッドが含まれていない `Game1.Initialize` ため、メソッド全体をに追加する必要があり `Game1` ます。

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];
    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);
    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // We’ll be assigning texture values later
    base.Initialize ();
}
```

頂点がどのように見えるかを視覚化するには、次の図を参考にしてください。

![頂点がどのように見えるかを視覚化するには、次の図を参考にしてください。](part2-images/image6.png)

レンダリングコードの実装が終了するまで、図に依存して頂点を視覚化する必要があります。

## <a name="adding-drawing-code"></a>追加 (描画コードを)

これで、geometry の位置が定義されたので、レンダリングコードを記述できます。

まず、 `BasicEffect` 位置や光源などのレンダリング用のパラメーターを保持するインスタンスを定義する必要があります。 これを行うには、 `BasicEffect` `Game1` フィールドが定義されている下のクラスにメンバーを追加し `floorVerts` ます。

```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

次に、メソッドを変更し `Initialize` て効果を定義します。

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // new code:
    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

これで、描画を実行するコードを追加できます。

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);

    float aspectRatio = 
        graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
    float nearClipPlane = 1;
    float farClipPlane = 200;

    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
            // We’ll be rendering two trinalges
            PrimitiveType.TriangleList,
            // The array of verts that we want to render
            floorVerts,
            // The offset, which is 0 since we want to start
            // at the beginning of the floorVerts array
            0,
            // The number of triangles to draw
            2);
    }
}
```

を呼び出す必要があり `DrawGround` `Game1.Draw` ます。

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

アプリでは、実行時に次のものが表示されます。

![実行時にアプリに表示されます](part2-images/image7.png)

上のコードの詳細をいくつか見てみましょう。

### <a name="view-and-projection-properties"></a>ビューとプロジェクションのプロパティ

`View`プロパティと `Projection` プロパティは、シーンを表示する方法を制御します。 このコードは、後でモデルレンダリングコードを再追加するときに変更します。 具体的には、 `View` カメラの位置と向きを制御し、 `Projection` (カメラのズームに使用できる)*ビューのフィールド*を制御します。

### <a name="techniques-and-passes"></a>手法と成功

効果のプロパティを割り当てたら、実際のレンダリングを実行できます。

このチュートリアルではプロパティを変更しません `CurrentTechnique` が、より高度なゲームでは、さまざまな方法 (色の値の適用方法など) で描画を実行できる1つの効果が得られる場合があります。 これらのレンダリングモードは、レンダリングの前に割り当てることができる手法として表すことができます。 さらに、各方法では、適切にレンダリングするために複数のパスが必要になる場合があります。 光る表面や一番などの複雑なビジュアルをレンダリングする場合、効果には複数のパスが必要になることがあります。

注意すべき重要な点は、ループでは、 `foreach` 基になるの複雑さに関係なく、同じ C# コードを使用して効果を表示できることです `BasicEffect` 。

### <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives`頂点がレンダリングされる場所です。 最初のパラメーターは、頂点の編成方法をメソッドに指示します。 各三角形が3つの順序付けられた頂点によって定義されるように、これらの要素を構成しました。そのため、値を使用し `PrimitiveType.TriangleList` ます。

2番目のパラメーターは、前に定義した頂点の配列です。

3番目のパラメーターは、描画する最初のインデックスを指定します。 頂点配列全体がレンダリングされるようにするため、値0を渡します。

最後に、レンダリングする三角形の数を指定します。 頂点配列には2つの三角形が含まれているため、値2を渡します。

## <a name="rendering-with-a-texture"></a>テクスチャを使用したレンダリング

この時点で、アプリは白い平面 (パースペクティブ) をレンダリングします。 次に、平面をレンダリングするときに使用するテクスチャをプロジェクトに追加します。

簡単にするために、モノのゲームパイプラインツールを使用するのではなく、プロジェクトに .png を直接追加します。 これを行うには、[この .png ファイル](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)をコンピューターにダウンロードします。 ダウンロードしたら、Solution pad で**コンテンツ**フォルダーを右クリックし、[**追加]>[ファイルの追加**] の順に選択します。 Android で作業している場合、このフォルダーは Android 固有のプロジェクトの**Assets**フォルダーの下に配置されます。 IOS の場合、このフォルダーは iOS プロジェクトのルートにあります。 **checkerboard.png**が保存されている場所に移動し、このファイルを選択します。 ファイルをディレクトリにコピーする場合に選択します。

次に、インスタンスを作成するためのコードを追加します `Texture2D` 。 最初に、を `Texture2D` インスタンスのメンバーとして追加し `Game1` `BasicEffect` ます。

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

次のように変更 `Game1.LoadContent` します。

```csharp
protected override void LoadContent()
{
    // Notice that loading a model is very similar
    // to loading any other XNB (like a Texture2D).
    // The only difference is the generic type.
    model = Content.Load<Model> ("robot");

    // We aren't using the content pipeline, so we need
    // to access the stream directly:
    using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
    {
        checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
    }
}
```

次に、メソッドを変更し `DrawGround` ます。 必要な変更は、をに割り当て、をに設定することだけです `effect.TextureEnabled` `true` `effect.Texture` `checkerboardTexture` 。

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);

    float aspectRatio = 
        graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
    float nearClipPlane = 1;
    float farClipPlane = 200;

    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

    // new code:
    effect.TextureEnabled = true;
    effect.Texture = checkerboardTexture;

    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
                    PrimitiveType.TriangleList,
            floorVerts,
            0,
            2);
    }
}
```

最後に、 `Game1.Initialize` 頂点のテクスチャ座標も割り当てるようにメソッドを変更する必要があります。

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    // New code:
    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, 1);
    floorVerts [2].TextureCoordinate = new Vector2 (1, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (1, 1);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

コードを実行すると、平面にチェッカーボードパターンが表示されることがわかります。

![プレーンにチェッカーボードパターンが表示されるようになりました](part2-images/image8.png)

## <a name="modifying-texture-coordinates"></a>テクスチャ座標の変更

モノゲームでは、正規化されたテクスチャ座標を使用します。これは、0とテクスチャの幅または高さの間ではなく、0 ~ 1 の座標です。 次の図は、正規化された座標の視覚化に役立ちます。

![この図は、正規化された座標の視覚化に役立ちます](part2-images/image9.png)

コードを書き直したりモデルを再作成したりしなくても、テクスチャのサイズを調整できます (fbx ファイルなど)。 これは、正規化された座標が特定のピクセルではなく比率を表すために発生する可能性があります。 たとえば、(1, 1) は、テクスチャのサイズに関係なく、常に右下隅を表します。

1つの変数を繰り返し回数として使用するように、テクスチャ座標の割り当てを変更できます。

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    int repetitions = 20;

    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
    floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

この結果、テクスチャが20回繰り返されます。

![この結果、テクスチャが20回繰り返されます。](part2-images/image10.png)

## <a name="rendering-vertices-with-models"></a>モデルを使用した頂点のレンダリング

これで、平面が正しくレンダリングされたので、モデルを再度追加してすべてを表示することができます。 まず、 `Game1.Draw` (変更された位置を使用して) メソッドにモデルコードを再追加します。

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    DrawGround ();

    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));

    DrawModel (new Vector3 (-4, 4, 3));
    DrawModel (new Vector3 ( 0, 4, 3));
    DrawModel (new Vector3 ( 4, 4, 3));

    base.Draw(gameTime);
}
```

また、 `Vector3` `Game1` カメラの位置を表すにを作成します。 次に、宣言の下にフィールドを追加し `checkerboardTexture` ます。

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10);
```

次に、メソッドからローカル変数を削除し `cameraPosition` `DrawModel` ます。

```csharp
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = Matrix.CreateTranslation (modelPosition);

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);
            ...
```

同様に、メソッドからローカル変数を削除し `cameraPosition` `DrawGround` ます。

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);
    ...
```

コードを実行すると、モデルとグラウンドの両方が同時に表示されるようになります。

![モデルとグラウンドの両方が同時に表示されます。](part2-images/image11.png)

カメラの位置を変更すると (この例ではカメラを左に移動します)、値が地表とモデルの両方に影響することがわかります。

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

このコードは次のようになります。

![このコードは、このビューになります。](part2-images/image3.png)

## <a name="summary"></a>まとめ

このチュートリアルでは、頂点配列を使用してカスタムレンダリングを実行する方法について説明しました。 この例では、頂点ベースのレンダリングとテクスチャを組み合わせて、チェッカーを作成しました `BasicEffect` が、ここに示すコードは3d レンダリングの基礎として機能します。 また、頂点ベースのレンダリングを同じシーンのモデルと組み合わせることもできます。

## <a name="related-links"></a>関連リンク

- [チェッカーボードファイル (サンプル)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [完成したプロジェクト (サンプル)](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelsandvertsmg/)
