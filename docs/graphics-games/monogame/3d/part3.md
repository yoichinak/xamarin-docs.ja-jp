---
title: MonoGame の 3D 座標
description: 3D ゲームの開発における重要な手順は、3 D 座標系を理解することです。 MonoGame の位置、向き、およびスケーリングの 3D 空間でオブジェクトのクラスを提供します。
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: dc21228f1daa74a90ff8f0ea346bc01b109f0987
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107644"
---
# <a name="3d-coordinates-in-monogame"></a>MonoGame の 3D 座標

_3D ゲームの開発における重要な手順は、3 D 座標系を理解することです。MonoGame の位置、向き、およびスケーリングの 3D 空間でオブジェクトのクラスを提供します。_

3D ゲームの開発には、3 D 座標系でオブジェクトを操作する方法を理解する必要があります。 このチュートリアルでは、ビジュアル オブジェクト (具体的にはモデル) を操作する方法について説明します。 3D カメラ クラスを作成するモデルを制御するための概念をビルドします。

線形代数、由来の概念が提示されますが、厳密な数値演算の背景の任意のユーザーが独自のゲームでこれらの概念を適用できるように、実践的なアプローチを移動します。

テーマは、次のトピック。

- Visual C++ プロジェクト
- ロボット エンティティを作成します。
- ロボット エンティティの移動
- 行列乗算
- カメラのエンティティを作成します。
- 入力でカメラの移動

完了するとは、プロジェクト、円、タッチ入力によって制御できるカメラの移動ロボットなります。

![](part3-images/image1.gif "アプリが、円、タッチ入力によって制御できるカメラの移動ロボット プロジェクトを含める完了すると、")


## <a name="creating-a-project"></a>Visual C++ プロジェクト

このチュートリアルでは、3 D 空間でオブジェクトの移動について説明します。 まず、プロジェクトのレンダリング モデルおよび頂点配列[ここで含まれている](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)します。 ダウンロードされると、解凍し、実行し、次のことが表示されていることを確認するプロジェクトを開きます。

![](part3-images/image2.png "ダウンロードされたら、解凍を実行して、このビューを表示するかを確認するプロジェクトを開く")


## <a name="creating-a-robot-entity"></a>ロボット エンティティを作成します。

作成、ロボットの周囲の移行を始める前に、`Robot`描画と移動するためのロジックを格納するクラス。 ゲーム開発者は、このカプセル化のロジックとデータを参照してください、*エンティティ*します。

新しい空のクラス ファイルを追加、 **MonoGame3D**ポータブル クラス ライブラリ (プラットフォーム固有の ModelAndVerts.Android とは異なる)。 名前を付けます**ロボット**クリック**新規**:

![](part3-images/image3.png "ロボットに名前を指定し、[新規] をクリックしてください")

変更、`Robot`クラスの次のようにします。

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Content;

namespace MonoGame3D
{
    public class Robot
    {
        Model model;

        public void Initialize(ContentManager contentManager)
        {
            model = contentManager.Load<Model> ("robot");

        }

        // For now we'll take these values in, eventually we'll
        // take a Camera object
        public void Draw(Vector3 cameraPosition, float aspectRatio)
        {
            foreach (var mesh in model.Meshes)
            {
                foreach (BasicEffect effect in mesh.Effects)
                {
                    effect.EnableDefaultLighting ();
                    effect.PreferPerPixelLighting = true;

                    effect.World = Matrix.Identity; 
                    var cameraLookAtVector = Vector3.Zero;
                    var cameraUpVector = Vector3.UnitZ;

                    effect.View = Matrix.CreateLookAt (
                        cameraPosition, cameraLookAtVector, cameraUpVector);

                    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                    float nearClipPlane = 1;
                    float farClipPlane = 200;

                    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);


                }

                // Now that we've assigned our properties on the effects we can
                // draw the entire mesh
                mesh.Draw ();
            }
        }
    }
}
```

`Robot`コードは基本的に同じコード`Game1`描画のため、`Model`します。 レビューの`Model`を読み込むと、描画を参照してください。[モデルの操作では、このガイド](~/graphics-games/monogame/3d/part1.md)。 削除すべてのようになりましたことができます、`Model`の読み込みとからコードをレンダリング`Game1`、置き換え、それを`Robot`インスタンス。

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        Vector3 cameraPosition = new Vector3(15, 10, 10);

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

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

            robot = new Robot ();
            robot.Initialize (Content);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            robot.Draw (cameraPosition, aspectRatio);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
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
    }
}
```

コードを実行した場合今すぐに、ほとんどの場合、floor で描画されるロボットが 1 つだけを使用したシーン。

![](part3-images/image4.png "アプリがほとんどの場合、floor で描画されるロボットが 1 つだけでシーンを表示、コードを今すぐ実行する場合")

## <a name="moving-the-robot"></a>ロボットを移動

あるので、`Robot`クラス、ロボットに移動ロジックを追加できます。 この場合は、単にしましょうロボットのゲーム時間に基づいてを円状に移動します。 これは、文字が入力に応答が通常、人工知能, が 3D の位置と回転を探索するための環境を提供します。 または、実際のゲームをある程度実用的でない実装です。

唯一の情報の外部から必要があります、`Robot`クラスは、現在のゲーム時間。 追加、`Update`メソッドは、`GameTime`パラメーター。 これは、`GameTime`を使用して、ロボットの最終的な位置を決定する角度変数をインクリメントに使用されるパラメーター。

最初に、角度のフィールドを追加します、`Robot`クラスの下で、`model`フィールド。

```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ...
```

 これで、この値を増やし、`Update`関数。

```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

確認する必要があります、`Update`メソッドの呼び出し元`Game1.Update`:

```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
}
```

もちろん、この時点で、[角度] フィールドでは何も行いません –、それを使用するコードを記述する必要があります。 変更します。、`Draw`メソッド世界を計算できるように`Matrix`専用のメソッドで。 

```csharp
public void Draw(Vector3 cameraPosition, float aspectRatio)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            // We’ll be doing our calculations here...
            effect.World = GetWorldMatrix();

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }

        mesh.Draw ();
    }
}
```

次に、実装、`GetWorldMatrix`メソッドで、`Robot`クラス。

```csharp
Matrix GetWorldMatrix()
{
    const float circleRadius = 8;
    const float heightOffGround = 3;

    // this matrix moves the model "out" from the origin
    Matrix translationMatrix = Matrix.CreateTranslation (
        circleRadius, 0, heightOffGround);

    // this matrix rotates everything around the origin
    Matrix rotationMatrix = Matrix.CreateRotationZ (angle);

    // We combine the two to have the model move in a circle:
    Matrix combined = translationMatrix * rotationMatrix;

    return combined;
}
```

このコードを実行した結果は、円の中を移動するロボットが得られます。

![](part3-images/image5.gif "このコードの結果を円の中を移動するロボットで実行されています。")

## <a name="matrix-multiplication"></a>行列乗算

上記のコードを作成して、ロボットの回転を`Matrix`で、`GetWorldMatrix`メソッド。 `Matrix`構造体には (位置を設定する) を変換、回転、およびスケーリングするために使用される 16 の float 値が含まれています (サイズの設定)。 代入すると、`effect.World`プロパティを指示することに、基になるシステムの配置、サイズ、および任意の場所を確認する方法を表示を描画するため (、`Model`またはジオメトリ頂点から)。 

さいわい、`Matrix`構造体には、いくつかの一般的な種類のマトリックスの作成を簡略化するメソッドが含まれています。 上記のコードで使用される 1 つ目は`Matrix.CreateTranslation`します。 数学用語*翻訳*参照ポイント (またはモデルの今回の場合) の結果を操作する (回転、サイズ変更など)、その他の変更を加えない間で 1 つの場所から移動します。 この関数は、変換の X、Y、および Z 値を受け取ります。 場合は、トップダウンから現実のシーンを見て、 `CreateTranslation` (分離) 内のメソッドは、次を実行します。

![](part3-images/image6.png "分離で CreateTranslation メソッドは、このアクションを実行します。")

作成する 2 番目の行列は回転行列を使用して、`CreateRotationZ`行列。 これは、回転を作成するために使用できる 3 つの方法のいずれかです。

- `CreateRotationX`
- `CreateRoationY`
- `CreateRotationZ`

各メソッドでは、特定の軸を中心に回転して回転行列を作成します。 ここでは「を」指す Z 軸の周りで回転していること。 次は、回転の軸ベースを視覚化できます works:

![](part3-images/image7.png "回転の軸ベースを視覚化しますこうすると、動作。")

使用している、`CreateRotationZ`角度フィールドでは、期限を時間の経過と共に増加メソッド、`Update`メソッドが呼び出されます。 その結果、`CreateRotationZ`メソッドにより、ロボットへの時間の経過に伴って原点の周囲に軌道にします。

コードの最後の行は、2 つの行列を 1 つに結合します。

```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

これは、行列の乗算は、通常の乗算とは少し異なる機能と呼ばれます。 *乗算の可換プロパティ*乗算演算の数値の順序では、結果が変化しないことを示します。 つまり、3 * 4 は、4 * 3 と同じです。 行列の乗算は、可換的でないことには異なります。 つまり、「、モデルを移動する translationMatrix を適用し、rotationMatrix を適用して回転すべて」に上記の行として読み取ることができます。 上記の行に影響する位置と回転とおりの方法を視覚化してでした。

![](part3-images/image8.png "視覚エフェクト pf 上記の行が、位置と回転に影響する方法")

行列乗算の順序が結果に影響を与えるかを理解するには、行列乗算が反転されます、次を考慮してください。

```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

上記のコードはまず、モデルで適切な場所を回転しに変換するため。

![](part3-images/image9.png "まず、モデルで適切な場所を回転しに変換するため、上記のコード")

コードは、逆の乗算を実行している場合回転が最初に適用されるので、モデルの方向をのみに影響し、モデルの位置は変わりませんが表示されます。 つまり、モデルは、場所に回転します。

![](part3-images/image10.gif "モデルが配置に回転します。")

## <a name="creating-the-camera-entity"></a>カメラのエンティティを作成します。

`Camera`エンティティにはすべての入力ベースの移動を実行して、上のプロパティを割り当てるためのプロパティを提供するために必要なロジックが含まれます、`BasicEffect`クラス。

まず静止カメラ (入力に基づく動かさず) の実装がされ、既存のプロジェクトに統合します。 新しいクラスを追加、 **MonoGame3D**ポータブル クラス ライブラリ (と同じプロジェクト`Robot.cs`) し、名前を**カメラ**します。 このファイルの内容を次のコードに置き換えます。

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Camera
    {
        // We need this to calculate the aspectRatio
        // in the ProjectionMatrix property.
        GraphicsDevice graphicsDevice;

        Vector3 position = new Vector3(15, 10, 10);

        public Matrix ViewMatrix
        {
            get
            {
                var lookAtVector = Vector3.Zero;
                var upVector = Vector3.UnitZ;

                return Matrix.CreateLookAt (
                    position, lookAtVector, upVector);
            }
        }

        public Matrix ProjectionMatrix
        {
            get
            {
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                float nearClipPlane = 1;
                float farClipPlane = 200;
                float aspectRatio = graphicsDevice.Viewport.Width / (float)graphicsDevice.Viewport.Height;

                return Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
            }
        }

        public Camera(GraphicsDevice graphicsDevice)
        {
            this.graphicsDevice = graphicsDevice;
        }

        public void Update(GameTime gameTime)
        {
            // We'll be doing some input-based movement here
        }
    }
}
```

上記のコードがからコードによく似ています。`Game1`と`Robot`上、マトリックスに代入する`BasicEffect`します。 

新しい統合できるようになりました`Camera`既存のプロジェクトにクラス。 最初に、変更します。、`Robot`クラスを、`Camera`インスタンスその`Draw`メソッドで、多くの重複するコードが削除されます。 置換、`Robot.Draw`を次のメソッド。

```csharp
public void Draw(Camera camera)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = GetWorldMatrix();
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;
        }

        mesh.Draw ();
    }
}
```

次に、変更、`Game1.cs`ファイル。

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        // New camera code
        Camera camera;

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

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

            robot = new Robot ();
            robot.Initialize (Content);

            // New camera code
            camera = new Camera (graphics.GraphicsDevice);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            robot.Update (gameTime);
            // New camera code
            camera.Update (gameTime);
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            // New camera code
            robot.Draw (camera);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            // New camera code
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;

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
    }
}
```

変更を加えて、`Game1`以前のバージョンから (で識別される`// New camera code`) は。

- `Camera` 内のフィールド `Game1`
- `Camera` インスタンス化 `Game1.Initialize`
- `Camera.Update` を呼び出し `Game1.Update`
- `Robot.Draw` ここでは、`Camera`パラメーター
- `Game1.Draw` では、`Camera.ViewMatrix`と `Camera.ProjectionMatrix`

## <a name="moving-the-camera-with-input"></a>入力でカメラの移動

これまでに追加されました、`Camera`エンティティが、実行時の動作を変更するには、それ以降を実行していません。 これにより、ユーザーの動作に追加します。

- タッチの左端にカメラを有効にするには、画面の左側にあります。
- 右にカメラを有効にするには、画面の右側にあるをタッチします。
- タッチ、カメラを前方に移動する画面の中央

### <a name="making-lookat-relative"></a>LookAt 相対を行う

最初に更新します、`Camera`クラスを`angle`に使用するフィールドの方向を設定、`Camera`発生しています。 現時点では、当社`Camera`を通じて、ローカルに接続するには、方向を決定`lookAtVector`に割り当てられる`Vector3.Zero`します。 つまり、`Camera`は常に、原点を注視します。 カメラの移動し、カメラが向いている角度も変更されます。

![](part3-images/image11.gif "カメラの移動し、カメラが向いている角度も変更されます。")

`Camera`に少なくとも – の位置に関係なく同じ方向が直面する交換ロジックが実装されるまで、`Camera`入力を使用します。 最初の変更を調整する、`lookAtVector`絶対位置を見てではなく、現在の場所からベースする変数。

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    // Let's start at X = 0 so we're looking at things head-on
    Vector3 position = new Vector3(0, 20, 10);

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

これは、結果、`Camera`直線で世界を表示します。 注意して初期`position`に値が変更された`(0, 20, 10)`ため、 `Camera` X 軸の中央に配置します。 実行中のゲームが表示されます。

![](part3-images/image12.png "ゲームを実行するには、このビューが表示されます。")

### <a name="creating-an-angle-variable"></a>角度の変数を作成します。

`lookAtVector`変数は、カメラを表示する角度を制御します。 負の値の Y 軸を表示する固定現在と若干ダウン傾いて (から、 `-.5f` Z 値)。 ここで作成、`angle`を調整するために使用する変数、`lookAtVector`プロパティ。 

このチュートリアルの前のセクションでは、オブジェクトの描画方法を回転行列を使用できることを紹介しました。 回転のようなベクトルにマトリックスを使用することも、`lookAtVector`を使用して、`Vector3.Transform`メソッド。 

追加、`angle`フィールドし、変更、`ViewMatrix`プロパティとして、次のとおりです。

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    Vector3 position = new Vector3(0, 20, 10);

    float angle;

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            // We'll create a rotation matrix using our angle
            var rotationMatrix = Matrix.CreateRotationZ (angle);
            // Then we'll modify the vector using this matrix:
            lookAtVector = Vector3.Transform (lookAtVector, rotationMatrix);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

### <a name="reading-input"></a>入力を読み取る

この`Camera`エンティティ今すぐ完全制御できますの位置と角度変数を通じて – だけ入力に応じてそれらを変更する必要があります。

まず、少し、`TouchPanel`ユーザーが画面に触れてを検索する状態。 使用しての詳細については、`TouchPanel`クラスを参照してください[タッチパネル API 参照](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel)します。

かどうか、ユーザーは、3 番目の左側に手を加えることは、調整されます、`angle`値ため、`Camera`左回転を他の方法を回転させます場合は、ユーザーは、3 番目の右側に触れています。 かどうか、ユーザーが画面の中央の第 3 回目に触れることに移動します、`Camera`転送します。

使用して、最初に、追加ステートメントを修飾するために、`TouchPanel`と`TouchCollection`クラス`Camera.cs`:

```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

次に、変更、`Update`タッチ パネルを読み取って、調整する方法、`angle`と`position`変数適切に。

```csharp
public void Update(GameTime gameTime)
{
    TouchCollection touchCollection = TouchPanel.GetState();

    bool isTouchingScreen = touchCollection.Count > 0;
    if (isTouchingScreen)
    {
        var xPosition = touchCollection [0].Position.X;

        float xRatio = xPosition / (float)graphicsDevice.Viewport.Width;

        if (xRatio < 1 / 3.0f)
        {
            angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
        else if (xRatio < 2 / 3.0f)
        {
            var forwardVector = new Vector3 (0, -1, 0);

            var rotationMatrix = Matrix.CreateRotationZ (angle);
            forwardVector = Vector3.Transform (forwardVector, rotationMatrix);

            const float unitsPerSecond = 3;

            this.position += forwardVector * unitsPerSecond *
                (float)gameTime.ElapsedGameTime.TotalSeconds ;
        }
        else
        {
            angle -= (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
    }
}
```

これで、`Camera`がタッチ入力に応答します。

![](part3-images/image1.gif "カメラがタッチ入力に応答するようになりました")

Update メソッドを呼び出すことによって開始`TouchPanel.GetState`仕上げのコレクションが返されます。 `TouchPanel.GetState`複数のタッチ ポイントを返すことがわかりやすくするための最初の 1 つについて取り上げますのみです。

場合は、ユーザーは、画面に手を加えることは、左、中央、または画面の右に 3 つ目の最初のタッチが、コードがチェックします。 3 分の左端と右端増加または減少により、カメラの回転、`angle`に従って変数、`TotalSeconds`値 (そのゲームでは、フレーム レートに関係なく同じ動作)。

場合は、ユーザーは、センターを第 3 にタッチは画面の カメラが前面に移動します。 これは最初に負の値の Y 軸の方向として定義し、回転行列を使用して作成を使用して、前方向ベクトルを取得することによって最初にこれは`Matrix.CreateRotationZ`と`angle`値。 最後に、`forwardVector`に適用される`position`を使用して、`unitsPerSecond`係数。

## <a name="summary"></a>まとめ

このチュートリアルは、移動および回転する方法を説明します。 `Models` 3D でスペースを使用して`Matrices`と`BasicEffect.World`プロパティ。 移動には、このフォームは、3D ゲーム内のオブジェクトを移動するための基礎を提供します。 このチュートリアルでは、実装する方法についても説明、`Camera`任意の位置や角度から世界を表示するためのエンティティ。

## <a name="related-links"></a>関連リンク

- [MonoGame API のリンク](http://www.monogame.net/documentation/?page=api)
- [完成したプロジェクト (サンプル)](https://developer.xamarin.com/samples/monodroid/MonoGame3DCamera/)
