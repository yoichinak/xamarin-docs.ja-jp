---
title: MonoGame におけるの頂点を持つ 3 D グラフィックスの描画
description: 頂点の配列を使用してポイントごとに、3 D のオブジェクトをレンダリングする方法を定義する MonoGame をサポートします。 ユーザーでは、動的 geometry を作成する、特殊効果を実装およびカリングを通じて、レンダリングの効率を向上する頂点の配列を利用できます。
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 25a05bcd094011042b3dc33a1b837460d5893be0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>MonoGame におけるの頂点を持つ 3 D グラフィックスの描画

_頂点の配列を使用してポイントごとに、3 D のオブジェクトをレンダリングする方法を定義する MonoGame をサポートします。ユーザーでは、動的 geometry を作成する、特殊効果を実装およびカリングを通じて、レンダリングの効率を向上する頂点の配列を利用できます。_

目を通してユーザー、[モデルのレンダリングのガイド](~/graphics-games/monogame/3d/part1.md)精通 MonoGame で 3D モデルを表示することができます。 `Model`クラスは、(.fbx) などのファイルで定義されているデータを操作するとき、および静的データを処理する場合は、3 D グラフィックスを表示する効果的な方法です。 一部のゲームでは、3 D のジオメトリを定義または実行時に動的に処理が必要です。 配列を使用このような場合は、*頂点*を定義し、ジオメトリをレンダリングします。 頂点は、ジオメトリを定義するために使用順序付きリストの一部では 3D 空間でのポイントの一般的な用語です。 通常の頂点は、一連の三角形を定義するように並べ替えられています。

頂点を使用して 3D オブジェクトを作成する方法を視覚化するためには、次の球を見てみましょう。

![](part2-images/image1.png "頂点を使用して 3D オブジェクトを作成する方法を視覚化するため、この球を検討してください。")

上記のように、球は複数の三角形で明確に構成します。 フォームの三角形の頂点の接続方法を表示する球のワイヤー フレームを表示できます。

![](part2-images/image2.png "フォームの三角形の頂点の接続方法を表示する球のワイヤー フレームを表示します。")

このチュートリアルは、次のトピックを取り上げます。

 - プロジェクトの作成
 - 頂点を作成します。
 - 描画コードを追加します。
 - テクスチャとレンダリング
 - テクスチャ座標を変更します。
 - モデルの頂点を表示

完成したプロジェクトは、頂点の配列を使用して描画されますが、チェッカー床に含まれます。

![](part2-images/image3.png "完成したプロジェクトには、頂点の配列を使用して描画されますチェッカー floor にが含まれます")


# <a name="creating-a-project"></a>Visual C++ プロジェクト

最初に、プロジェクトは、開始点として機能をダウンロードします。 モデル プロジェクトが使用されます[に含まれているここ](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)です。

ダウンロードして解凍した後は、開き、プロジェクトを実行します。 画面上に描画される 6 つのロボット モデルを参照して期待される結果します。

![](part2-images/image4.png "画面上に描画される 6 つのロボット モデル")

このプロジェクトの最後でおをするとを組み合わせると、独自のカスタムの頂点レンダリング ロボット`Model`、ロボット表示コードを削除することにしましょう。 クリアします代わりに、話ですが、`Game1.Draw`ここでは 6 ロボットの描画を削除する要求。 これを行うには、開く、 **Game1.cs**ファイルし、検索、`Draw`メソッドです。 次のコードが含まれているように変更します。


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);
    base.Draw(gameTime);
}
```

これは、空の青い画面を表示する、ゲームで決定されます。

![](part2-images/image5.png "これは、空の青い画面を表示するゲームになります")


# <a name="creating-the-vertices"></a>頂点を作成します。

このジオメトリを定義する頂点の配列を作成します。 このチュートリアルでを作成します (3 D 空間に正方形、航空機されません)、3 D の平面。 平面は 4 つの辺と 4 つの角がの 3 つの頂点を必要とそれぞれが 2 つの三角形ので構成されます。 そのため、6 個のポイントの合計を定義していますはします。

これまでまでに説明した一般的な意味で頂点についてが MonoGame がいくつかの頂点で使用できる標準的な構造体を提供します。

 - `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
 - `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
 - `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
 - `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

各型の名前には、コンポーネントが含まれていることを示します。 たとえば、`VertexPositionColor`位置と色の値が含まれます。 各コンポーネントを見てみましょう。

 - 位置 – すべての頂点の種類には、`Position`コンポーネントです。 `Position`値では、3 D 空間 (X、Y、および Z) で、頂点の位置を定義します。
 - 色の頂点を指定できます必要に応じて、`Color`カスタム色合いを実行する値。
 - 通常 – 法線は、オブジェクトの表面が直面しているどのような方法を定義します。 法線は、方向から光を持つオブジェクトをレンダリングしている場合、サーフェスが直面して影響量ライトを受け取る必要があります。 法線は通常、として指定する*単位ベクター* – 1 の長さを持つ 3D ベクトル。
 - テクスチャ – テクスチャはテクスチャ座標 – は、特定の頂点に表示するテクスチャのどの部分を参照します。 テクスチャの値は、テクスチャを使用して、3 D のオブジェクトをレンダリングしている場合に必要です。 テクスチャ座標は正規化された座標は、0 ~ 1 の間の値が入ります。 このガイドの後半で詳しくテクスチャ座標を説明します。

平面は、床として動作しを使用しますので、レンダリングの実行時にテクスチャを適用する必要あります、`VertexPositionTexture`頂点を定義する型。

最初に、メンバー、Game1 クラスに追加されます。


```csharp
VertexPositionTexture[] floorVerts; 
```

次に、定義で頂点`Game1.Initialize`です。 この記事の前半で参照されている指定されたテンプレートが含まれていないことを確認、`Game1.Initialize`メソッド、ために全体のメソッドを追加する必要は`Game1`:


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

頂点がどのように視覚化するためには、次の図を考慮してください。

![](part2-images/image6.png "頂点がどのように視覚化するため、この図を検討してください。")

レンダリング コードの実装が完了までに、頂点を視覚化する図で使用する必要があります。


# <a name="adding-drawing-code"></a>描画コードを追加します。

位置、ジオメトリを定義して、あるレンダリング コードを記述できます。

最初に、定義する必要があります、`BasicEffect`位置などのレンダリングと光源のパラメーターを格納するインスタンス。 これを行うには、追加、`BasicEffect`メンバーを`Game1`場所の下のクラス、`floorVerts`フィールドが定義されています。


```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect; 
```

次に、変更、`Initialize`効果を定義するメソッド。


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
    // new code:
    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
} 
```

これで、描画を実行するコードを追加できます。


```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);

    float aspectRatio = 
        graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
    float nearClipPlane = 1;
    float farClipPlane = 200;

    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);


    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
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

呼び出す必要があります`DrawGround`で、 `Game1.Draw`:

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

アプリを実行する場合、次が表示されます。

![](part2-images/image7.png "アプリの実行時に表示これには")

上記のコードで詳細情報の一部を見てみましょう。


## <a name="view-and-projection-properties"></a>プロジェクションのプロパティを表示および

`View`と`Projection`プロパティは、シーンを表示する方法を制御します。 おあるコードを変更するこの後で、モデルの表示コードを再追加する際します。 具体的には、 `View` 、カメラの向きと場所を制御および`Projection`コントロール、*視野*(カメラを拡大するを使用できます)。


## <a name="techniques-and-passes"></a>テクニックとパス

1 回代入したプロパティの効果を実行できます、実際に表示します。 

変更されません、`CurrentTechnique`このチュートリアルより高度なゲーム内のプロパティは 1 つのエフェクト (色の値を適用する方法) などのさまざまな方法で描画を実行できる必要があります。 これらの各表示モードは、レンダリングの前に割り当てることのできる手法として表現できます。 さらに、各手法では、複数のパスを適切に描画が必要です。 発光画面の一番などの複雑なビジュアルをレンダリングしている場合、効果は複数のパスを必要があります。

注意する重要な点は、`foreach`ループにより、基になるの複雑さに関係なく任意の効果を表示するために同じの c# コード`BasicEffect`です。


## <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives` 頂点は表示されます。 最初のパラメーターは、頂点を整理して方法をメソッドに指示します。 個々 の三角形は 3 つの順序付けされた頂点が使用されるよう構成しましたが、`PrimitiveType.TriangleList`値。

2 番目のパラメーターは、前に定義したの頂点の配列です。

3 番目のパラメーターは、描画する最初のインデックスを指定します。 全体の頂点配列レンダリングされるので、値 0 を渡します。

最後に、表示するためにどのくらいの三角形を指定します。 頂点配列には、次の 2 つの三角形が含まれています、2 の値を渡すようにします。


# <a name="rendering-with-a-texture"></a>テクスチャとレンダリング

この時点で、アプリケーションの動作は、白い平面 (パースペクティブ) を表示します。 次に、テクスチャ、平面をレンダリングするときに使用するプロジェクトに追加します。 

煩雑にならないように、.png、MonoGame パイプライン ツールを使用するのではなく、プロジェクトに直接追加します。 これを行うには、ダウンロード[この .png ファイル](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)をコンピューターにします。 ダウンロードされるを右クリックし、**コンテンツ**ソリューション パッドし、フォルダー**追加 > ファイルを追加しています.** . Android での作業をする場合、このフォルダーはの下に、**資産**Android 固有のプロジェクト内のフォルダーです。 Ios の場合、このフォルダーにある場合 iOS プロジェクトのルートです。 場所に移動、 **checkerboard.png**が保存され、このファイルを選択します。 選択すると、ファイルをディレクトリにコピーします。

次に、作成するコードを追加、`Texture2D`インスタンス。 最初に、追加、`Texture2D`のメンバーとして`Game1`下にある、`BasicEffect`インスタンス。


```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

変更`Game1.LoadContent`次のようにします。


```csharp
protected override void LoadContent()
{
    // Notice that loading a model is very similar
    // to loading any other XNB (like a Texture2D).
    // The only difference is the generic type.
    model = Content.Load<Model> ("robot");

    // We aren't using the content pipeline, so we need
    // to access the stream directly:
    using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
    {
        checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
    }
} 
```

次に、変更、`DrawGround`メソッドです。 割り当てるには、必要な唯一の変更`effect.TextureEnabled`に`true`を設定して、`effect.Texture`に`checkerboardTexture`:


```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);

    float aspectRatio = 
        graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
    float nearClipPlane = 1;
    float farClipPlane = 200;

    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

    // new code:
    effect.TextureEnabled = true;
    effect.Texture = checkerboardTexture;

    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
                    PrimitiveType.TriangleList,
            floorVerts,
            0,
            2);
    }
} 
```

最後に、変更する必要があります、`Game1.Initialize`もテクスチャを割り当てる方法が、頂点の座標します。


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

    // New code:
    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, 1);
    floorVerts [2].TextureCoordinate = new Vector2 (1, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (1, 1);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
} 
```

コードを実行する場合、平面が、チェッカー ボード パターンを今すぐが表示される確認できます。

![](part2-images/image8.png "平面、チェッカー ボード パターンが表示されます。")


# <a name="modifying-texture-coordinates"></a>テクスチャの変更を調整します。

MonoGame 使用は、0 とテクスチャの幅または高さの間ではなく、0 ~ 1 の間の座標は、テクスチャ座標を正規化します。 次の図は、正規化された座標を視覚化することができます。

![](part2-images/image9.png "この図は、正規化された座標を視覚化することができます。")

正規化されたテクスチャ座標は、コードを書き直すか (.fbx ファイル) などのモデルを再作成しなくてもテクスチャ サイズを変更できるようにします。 これには正規化された座標は、特定のピクセルではなく、比率を表すためです。 たとえば、(1, 1) はでは、常にテクスチャのサイズに関係なく右下隅を表します。

繰り返しの数に 1 つの変数を使用する、テクスチャ座標の割り当てを変更することができます。


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

    int repetitions = 20;

    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
    floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
} 
```

これは、結果、テクスチャの 20 回の繰り返しになります。

![](part2-images/image10.png "これは、結果、テクスチャを 20 回の繰り返し")


# <a name="rendering-vertices-with-models"></a>モデルの頂点を表示

これで、平面は正しくレンダリングは、すべてを一緒に表示するモデルを再追加できます。 まず再追加モデル コードを`Game1.Draw`メソッド (変更後の位置)。


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    DrawGround ();

    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));

    DrawModel (new Vector3 (-4, 4, 3));
    DrawModel (new Vector3 ( 0, 4, 3));
    DrawModel (new Vector3 ( 4, 4, 3));

    base.Draw(gameTime);
} 
```

作成、`Vector3`で`Game1`カメラの位置を表すです。 下のフィールドを追加お、`checkerboardTexture`宣言します。


```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10); 
```

次に、ローカルを削除`cameraPosition`から変数、`DrawModel`メソッド。


```csharp
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = Matrix.CreateTranslation (modelPosition);

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector); 
            ...
```

同様に、ローカルを削除`cameraPosition`から変数、`DrawGround`メソッド。


```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);
    ... 
```

今すぐコードが実行わかる、モデルと地上の両方同時に。

![](part2-images/image11.png "モデルと地上の両方が同時に表示されます。")

カメラの位置を変更している場合 (など、X 値を増やすことでをここでは、カメラ左に移動する) 値が地上と、モデルの両方に影響を確認できます。


```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

このコードは、次が得られます。

![](part2-images/image3.png "このコードは、このビューを結果します。")


# <a name="summary"></a>まとめ

このチュートリアルでは、頂点の配列を使用してカスタム描画を実行する方法を示しました。 この場合、テクスチャを使用して、頂点ベースのレンダリングを組み合わせることによってチェッカー floor を作成したと`BasicEffect`コードは、ここで示す機能、3 D レンダリングの基盤として、します。 また、同じシーン内のモデルに基づく頂点のレンダリングを混在させることができますも紹介しました。

## <a name="related-links"></a>関連リンク

- [チェッカー ファイル (サンプル)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [完成したプロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)
