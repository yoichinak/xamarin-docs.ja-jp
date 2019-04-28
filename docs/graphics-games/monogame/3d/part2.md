---
title: MonoGame の頂点を 3D グラフィックスの描画
description: 頂点の配列を使用してポイントごとに、3 D オブジェクトを表示する方法を定義する MonoGame をサポートします。 ユーザーでは、動的なジオメトリを作成、特殊効果を実装およびカリングを通じて、レンダリングの効率を向上する頂点配列を利用できます。
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: df36c149e98e8c0cbb16de4c2cf52def5713ec13
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61178587"
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>MonoGame の頂点を 3D グラフィックスの描画

_頂点の配列を使用してポイントごとに、3 D オブジェクトを表示する方法を定義する MonoGame をサポートします。ユーザーでは、動的なジオメトリを作成、特殊効果を実装およびカリングを通じて、レンダリングの効率を向上する頂点配列を利用できます。_

読んでいるユーザー、[モデルのレンダリングのガイド](~/graphics-games/monogame/3d/part1.md)MonoGame の 3D モデルを表示するのには慣れています。 `Model`クラスは、(.fbx) などのファイルで定義されているデータを扱うとき、および静的データを処理する 3D グラフィックスをレンダリングする効果的な方法です。 いくつかのゲームでは、3 D ジオメトリを定義または実行時に動的に処理する必要があります。 このような場合は、配列を使用できます*頂点*を定義し、ジオメトリをレンダリングします。 頂点は、ジオメトリを定義するために使用順序付きリストの一部である 3D 空間でのポイントの一般的な用語です。 通常の頂点は、一連の三角形を定義するように並べ替えられています。

頂点を使用して 3D オブジェクトを作成する方法を視覚化するために、次の球体を考えてみましょう。

![](part2-images/image1.png "頂点を使用して 3D オブジェクトを作成する方法を視覚化するため、この球体を検討してください。")

前述のように、球が明確に複数の三角形で構成されます。 フォームの三角形の頂点の接続方法を確認する球のワイヤー フレームを表示できます。

![](part2-images/image2.png "フォームの三角形の頂点の接続方法を確認する球のワイヤー フレームを表示します。")

このチュートリアルは、次のトピックを取り上げます。

- プロジェクトの作成
- 頂点を作成します。
- 描画コードを追加します。
- テクスチャを使用したレンダリング
- テクスチャ座標を変更します。
- モデルを使用した頂点の表示

完成したプロジェクトは、頂点配列を使用して描画されます格子模様の床面が表示されます。

![](part2-images/image3.png "完成したプロジェクトには、頂点配列を使用して描画されます格子模様フロアにが含まれます")

## <a name="creating-a-project"></a>Visual C++ プロジェクト

最初に、私たちは、開始点として機能するプロジェクトをダウンロードします。 モデル プロジェクトを使用します[ここで含まれている](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)します。

ダウンロードして解凍した後、開き、プロジェクトを実行します。 画面上に描画されている 6 つのロボット モデルを参照してください期待される結果します。

![](part2-images/image4.png "画面上に描画されている 6 つのロボット モデル")

このプロジェクトの末尾で私たちを結合する独自のカスタムの頂点のレンダリング ロボット`Model`ロボットのレンダリング コードを削除しません。 代わりに、クリアだけですが、`Game1.Draw`ここでは 6 ロボットの描画を削除する要求。 これを行うには、開く、 **Game1.cs**ファイルし、検索、`Draw`メソッド。 次のコードが含まれているように変更します。

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

これにより、空のブルー スクリーンを表示する、ゲームが発生します。

![](part2-images/image5.png "これは、結果、空のブルー スクリーンを表示するゲーム")

## <a name="creating-the-vertices"></a>頂点を作成します。

このジオメトリを定義する頂点の配列を作成します。 このチュートリアルで作成します (3 D 空間内の正方形、飛行機ではない) の 3D の平面。 平面は 4 つの側面と 4 つの角は、3 つの頂点が必要、2 つの三角形で構成されます。 そのため、合計で 6 つのポイントを定義していますが。

これまでに説明してきましたに関する一般的な意味での頂点が、MonoGame がいくつかの頂点に使用できる標準構造体を提供します。

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

各型の名前には、コンポーネントが含まれていることを示します。 たとえば、`VertexPositionColor`位置と色の値が含まれています。 各コンポーネントを見てみましょう。

- 位置 – すべての頂点の種類には、`Position`コンポーネント。 `Position`値は、3 D 空間 (X、Y、および Z) での頂点の位置を定義します。
- [色]: 頂点を指定できます必要に応じて、`Color`カスタム色合いを付けることを実行する値。
- Normal – 法線は、オブジェクトの表面に接続する方法を定義します。 法線は、サーフェスが受信影響光の量を向いている方向から光を持つオブジェクトをレンダリングしている場合に必要です。 法線は通常、として指定する*単位ベクトル*– 長さ 1 の 3D ベクトル。
- テクスチャ – テクスチャはテクスチャ座標を – は、特定の頂点に表示するテクスチャのどの部分を参照します。 テクスチャの値は、テクスチャを使用して、3 D オブジェクトをレンダリングしている場合に必要です。 テクスチャ座標は正規化された座標は、0 ~ 1 の値が入ります。 テクスチャ座標では、このガイドの後半で詳しく説明します。

平面はフロアとして機能しを使用しますので、レンダリングを実行するときに、テクスチャを適用するが、`VertexPositionTexture`頂点を定義する型。

まず、Game1 クラスにメンバーを追加します。

```csharp
VertexPositionTexture[] floorVerts; 
```

頂点を次に、定義`Game1.Initialize`します。 この記事の前半で参照されている指定されたテンプレートが含まれていませんが、`Game1.Initialize`メソッド、メソッド全体を追加する必要があります`Game1`:

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

頂点がどのように視覚化するために、次の図を考慮してください。

![](part2-images/image6.png "頂点がどのように視覚化するため、この図を検討してください。")

レンダリング コードを実装することが完了するまでに、頂点を視覚化する図に依存する必要があります。

## <a name="adding-drawing-code"></a>描画コードを追加します。

あるので、位置、ジオメトリを定義して、レンダリング コードを記述できます。

最初に、定義する必要あります、`BasicEffect`の位置などのレンダリングや照明パラメーターを格納するインスタンス。 これを行うには、追加、`BasicEffect`メンバーを`Game1`下クラス、`floorVerts`フィールドが定義されています。


```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

次に、変更、`Initialize`効果を定義するメソッド。

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

描画を実行するコードを追加できます。

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

呼び出す必要があります`DrawGround`で、 `Game1.Draw`:

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

アプリの実行時に、次が表示されます。

![](part2-images/image7.png "アプリの実行時に表示これには")

上記のコードの詳細のいくつかを見てみましょう。

### <a name="view-and-projection-properties"></a>プロジェクションのプロパティを表示および

`View`と`Projection`プロパティは、シーンを表示する方法を制御します。 変更コードのこの後でモデルのレンダリング コードを再追加する際。 具体的には、 `View` 、カメラの向きと場所を制御および`Projection`コントロール、*視野*(カメラのズームを使用できます)。

### <a name="techniques-and-passes"></a>手法を渡します

1 回を割り当てたプロパティで、効果を実行できますを実際に表示します。 

私たちは変更されません、`CurrentTechnique`このチュートリアルより高度なゲーム内のプロパティ (色の値を適用する方法) などのさまざまな方法で描画を実行できる単一の効果があります。 それぞれのレンダリング モードは、レンダリングの前に割り当てることのできる手法として表現できます。 さらに、各手法では、複数のパスが正しくレンダリングが必要です。 効果は、発光画面または後ろなどの複雑なビジュアルをレンダリングしている場合、複数のパスにすることがあります。

留意することが重要ですが、`foreach`ループにより、同じC#、基になるの複雑さに関係なく任意の効果を表示するためにコード`BasicEffect`します。

### <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives` 頂点が表示されます。 最初のパラメーターは、頂点を編成方法をメソッドに指示します。 使用するために 3 つの順序付けされた頂点によって各三角形が定義されているように構成しましたが、`PrimitiveType.TriangleList`値。

2 番目のパラメーターは、先ほど定義したこの頂点の配列です。

3 番目のパラメーターには、描画するために最初のインデックスを指定します。 全体の頂点配列をレンダリングするため、値 0 を渡します。

最後に、表示するために三角形の数を指定します。 頂点配列が 2 つの三角形が含まれています、2 の値を渡すようにします。

## <a name="rendering-with-a-texture"></a>テクスチャを使用したレンダリング

この時点で、アプリは、(パースペクティブ) で白の平面をレンダリングします。 次に、テクスチャ、平面を表示するときに使用するプロジェクトに追加します。 

複雑にならない、.png MonoGame パイプライン ツールを使用するのではなく、このプロジェクトに直接追加します。 これを行うには、ダウンロード[この .png ファイル](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)をコンピューターにします。 右クリックし、ダウンロードした後、**コンテンツ**フォルダーをクリックし、Solution pad で**追加 > ファイルを追加しています.** . Android に取り組んでいる場合、このフォルダーに置かれます、**資産**Android 固有のプロジェクトのフォルダー。 Ios では、し、このフォルダーにある場合、iOS プロジェクトのルート。 場所に移動、 **checkerboard.png**が保存され、このファイルを選択します。 選択すると、ファイルをディレクトリにコピーします。

次に、作成するコードを追加、`Texture2D`インスタンス。 最初に、追加、`Texture2D`のメンバーとして`Game1`下、`BasicEffect`インスタンス。

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

変更`Game1.LoadContent`次のようにします。


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

次に、変更、`DrawGround`メソッド。 割り当てるために必要な唯一の変更は、`effect.TextureEnabled`に`true`を設定して、`effect.Texture`に`checkerboardTexture`:

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

最後に、変更する必要があります、`Game1.Initialize`もテクスチャを割り当てる方法が、頂点の座標します。


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

コードを実行した場合、平面は、チェッカー ボード パターンを今すぐが表示されますがわかります。

![](part2-images/image8.png "平面は、チェッカー ボード パターンが表示されます。")

## <a name="modifying-texture-coordinates"></a>テクスチャの変更を調整します。

MonoGame の使用は、座標 0 とテクスチャの幅または高さの間ではなく、0 ~ 1 の間であるテクスチャ座標を正規化します。 次の図は、正規化された座標の視覚化に役立ちます。

![](part2-images/image9.png "この図は、正規化された座標を視覚化できます。")

正規化されたテクスチャ座標は、テクスチャのサイズを変更するコードを書き直すか (.fbx ファイル) などのモデルを再作成しなくても使用できます。 正規化された座標は、特定のピクセルではなく、比率を表すため可能です。 たとえば、(1, 1) はでは、常に、テクスチャのサイズに関係なく、右下隅を表します。

繰り返しの数に 1 つの変数を使用するテクスチャ座標の割り当てを変更することができます。


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

これは、20 回の繰り返しのテクスチャが得られます。

![](part2-images/image10.png "これは、結果、20 回の繰り返しのテクスチャ")


## <a name="rendering-vertices-with-models"></a>モデルを使用した頂点の表示

これで、平面は正しくレンダリングは、すべてを一緒に表示するモデルを再追加できます。 最初に、再度追加しますモデル コードを`Game1.Draw`(変更後の位置) を持つメソッド。

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

作成も、`Vector3`で`Game1`カメラの位置を表すため。 下のフィールドを追加します、`checkerboardTexture`宣言。

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10); 
```

次に、ローカルを削除`cameraPosition`変数から、`DrawModel`メソッド。

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

同様に、ローカルを削除`cameraPosition`変数から、`DrawGround`メソッド。

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

今すぐコードを実行した場合ことがわかりますモデルと、最初の両方と同時に。

![](part2-images/image11.png "モデルと、最初の両方が同時に表示されます。")

場合は、カメラの位置を変更します (など、X 値を増やすことでをここでは、カメラを左に移動)、値が地面とモデルの両方に影響することがわかります。

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

このコードは、次の結果します。

![](part2-images/image3.png "このコードは、このビューを結果します。")

## <a name="summary"></a>まとめ

このチュートリアルでは、頂点配列を使用して、カスタム レンダリングを実行する方法を示しました。 ここでは、テクスチャを使用して、頂点ベースのレンダリングを組み合わせることで格子模様の床面を作成したと`BasicEffect`コードは、ここでは役割を果たし、3 D レンダリングの基礎として。 同じシーン内のモデルを使用した頂点ベースのレンダリングを混在させることができますも紹介しました。

## <a name="related-links"></a>関連リンク

- [チェッカー ボード ファイル (サンプル)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [完成したプロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)
