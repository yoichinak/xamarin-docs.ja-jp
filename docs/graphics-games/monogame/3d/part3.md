---
title: モノゲームの3D 座標
description: 3d の座標系を理解することは、3D ゲームを開発する上で重要な手順です。 モノゲームには、3D 空間におけるオブジェクトの配置、回転、およびスケーリングを行うためのさまざまなクラスが用意されています。
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 2f5e66ae58f471be62839de17fe4edd12ccef457
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70763995"
---
# <a name="3d-coordinates-in-monogame"></a>モノゲームの3D 座標

_3d の座標系を理解することは、3D ゲームを開発する上で重要な手順です。モノゲームには、3D 空間におけるオブジェクトの配置、回転、およびスケーリングを行うためのさまざまなクラスが用意されています。_

3D ゲームを開発するには、3D 座標系でオブジェクトを操作する方法を理解する必要があります。 このチュートリアルでは、ビジュアルオブジェクト (具体的にはモデル) を操作する方法について説明します。 ここでは、3D カメラクラスを作成するためのモデルの制御の概念について説明します。

ここで説明する概念は、線形代数から生成されますが、強力な数値演算の背景を持たないユーザーが独自のゲームでこれらの概念を適用できるように、実用的なアプローチを採用します。

ここでは、次のトピックについて説明します。

- Visual C++ プロジェクト
- ロボットエンティティの作成
- ロボットエンティティの移動
- 行列乗算
- カメラエンティティの作成
- 入力によるカメラの移動

完了すると、ロボットが円で移動するプロジェクトと、タッチ入力で制御できるカメラが作成されます。

![](part3-images/image1.gif "完了すると、アプリにはロボットが円で移動するプロジェクトと、タッチ入力で制御できるカメラが含まれます。")

## <a name="creating-a-project"></a>Visual C++ プロジェクト

このチュートリアルでは、3D 空間でオブジェクトを移動する方法について説明します。 [ここ](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelsandvertsmg/)では、モデルと頂点配列をレンダリングするためのプロジェクトについて説明します。 ダウンロードが完了したら、プロジェクトを解凍して開き、実行されていることを確認します。次のように表示されます。

![](part3-images/image2.png "ダウンロードが完了したら、プロジェクトを解凍して開いて、実行されていることを確認します。このビューが表示されます。")

## <a name="creating-a-robot-entity"></a>ロボットエンティティの作成

ロボットの移動を始める前に、描画と移動の`Robot`ロジックを含むクラスを作成します。 ゲーム開発者は、このロジックとデータを*エンティティ*としてカプセル化します。

新しい空のクラスファイルを、プラットフォーム固有の ModelAndVerts ではなく、 **MonoGame3D**ポータブルクラスライブラリに追加します。 **ロボット**に名前を指定し、 **[新規]** をクリックします。

![](part3-images/image3.png "ロボットに名前を指定し、[新規] をクリックします。")

クラスを`Robot`次のように変更します。

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

このコードは、基本的にを描画`Game1`する`Model`ためののコードと同じです。 `Robot` 読み込みと描画の詳細については、[モデルの使用に関するこのガイド](~/graphics-games/monogame/3d/part1.md)を参照してください。 `Model` から`Model` `Robot`読み込みとレンダリングのコードをすべて削除し、インスタンスに置き換えることができるようになりました。 `Game1`

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

ここでコードを実行すると、1つのロボットだけを含むシーンが作成されます。これは、主にフロアの下に描画されます。

![](part3-images/image4.png "コードがすぐに実行された場合、アプリには、主にフロアの下に描画されるロボットを1つだけ持つシーンが表示されます。")

## <a name="moving-the-robot"></a>ロボットの移動

`Robot`クラスを用意したので、移動ロジックをロボットに追加できます。 この例では、ゲーム時間に従ってロボットを円形に移動させるだけです。 これは、通常、文字が入力または人工知能に応答する可能性があるため、実際のゲームでは実用的ではない実装ですが、3D の位置と回転を調べるための環境を提供します。

`Robot`クラスの外部から必要な情報は、現在のゲーム時刻だけです。 ここでは、 `Update`パラメーターを`GameTime`受け取るメソッドを追加します。 この`GameTime`パラメーターは、ロボットの最終的な位置を決定するために使用する角度変数をインクリメントするために使用されます。

まず、フィールドの`Robot` `model`下にあるクラスに angle フィールドを追加します。

```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ...
```

 `Update`関数内でこの値をインクリメントできるようになりました。

```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

`Update`メソッドがから`Game1.Update`呼び出されるようにする必要があります。

```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
}
```

もちろん、この時点では、[角度] フィールドは何も行いません。これを使用するコードを記述する必要があります。 メソッドを変更し`Draw`て、専用のメソッドで世界`Matrix`を計算できるようにします。 

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

次に、 `Robot`クラスにメソッド`GetWorldMatrix`を実装します。

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

このコードを実行した結果、ロボットが円で移動します。

![](part3-images/image5.gif "このコードを実行すると、ロボットが円で移動します。")

## <a name="matrix-multiplication"></a>行列乗算

上記のコードでは、 `Matrix` `GetWorldMatrix`メソッドでを作成することによってロボットを回転しています。 構造`Matrix`体には、変換 (位置の設定)、回転、およびスケール (サイズの設定) に使用できる16の浮動小数点値が含まれています。 この`effect.World`プロパティを割り当てるときに、基になるレンダリングシステムに、描画しようとしているもの (または頂点からの`Model`ジオメトリ) の位置、サイズ、および向きを指示します。 

幸いにも`Matrix` 、構造体には、一般的な種類のマトリックスの作成を簡略化する多くのメソッドが含まれています。 上記のコードで使用されて`Matrix.CreateTranslation`いる最初のは、です。 数学的用語の*変換*とは、(この場合はモデルの) ある場所から別の場所に移動する (回転やサイズ変更など) 操作を指します。 関数は、変換に対して X、Y、および Z の値を受け取ります。 シーンを上から表示すると、 `CreateTranslation`メソッド (分離) によって次の処理が実行されます。

![](part3-images/image6.png "分離における CreateTranslation メソッドは、このアクションを実行します。")

2番目に作成する行列は、 `CreateRotationZ`マトリックスを使用した回転行列です。 これは、ローテーションを作成するために使用できる3つの方法のうちの1つです。

- `CreateRotationX`
- `CreateRoationY`
- `CreateRotationZ`

各メソッドは、指定された軸を中心に回転行列を作成します。 ここでは、Z 軸を中心に回転しています。これは "up" を指します。 以下は、軸ベースの回転のしくみを視覚化するのに役立ちます。

![](part3-images/image7.png "これは、軸ベースの回転のしくみを視覚化するのに役立ちます。")

また、メソッドを angle `CreateRotationZ`フィールドと共に使用しています。これは、 `Update`メソッドが呼び出されているため、時間の経過と共に増加します。 結果として、 `CreateRotationZ`メソッドによって、ロボットが開始元を時間の経過と共に移動することになります。

最後のコード行では、2つの行列を1つに結合しています。

```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

これは、通常の乗算とは少し異なる動作をする行列乗算と呼ばれます。 *乗算の可換プロパティ*は、乗算演算の数値の順序によって結果が変更されないことを表します。 つまり、3 * 4 は 4 * 3 に相当します。 行列乗算は、可換ではないという点で異なります。 つまり、上記の行は、"translationMatrix を適用してモデルを移動し、rotationMatrix を適用してすべてを回転させる" として読み取ることができます。 次のように、上の行が位置と回転にどのように影響するかを視覚化することができます。

![](part3-images/image8.png "上の行が位置と回転に影響する方法を示す、視覚エフェクト")

行列乗算の順序が結果にどのように影響するかを理解するために、次の点を考慮してください。

```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

上記のコードでは、まずモデルをその場で回転させてから、次のように変換します。

![](part3-images/image9.png "上記のコードでは、まずモデルをその場で回転させ、次に変換します。")

反転された乗算でコードを実行すると、最初に回転が適用されるため、モデルの向きにのみ影響し、モデルの位置は変わりません。 言い換えると、モデルは次のように回転します。

![](part3-images/image10.gif "モデルが所定の位置に回転する")

## <a name="creating-the-camera-entity"></a>カメラエンティティの作成

エンティティには、入力ベースの移動を実行するために必要なすべてのロジックが含まれ、 `BasicEffect`クラスにプロパティを割り当てるためのプロパティを提供します。 `Camera`

まず、静的なカメラ (入力ベースの移動は不要) を実装し、それを既存のプロジェクトに統合します。 **MonoGame3D**ポータブルクラスライブラリ (と同じプロジェクト) に新しいクラスを追加し`Robot.cs`、「**カメラ**」という名前を付けます。 このファイルの内容を次のコードに置き換えます。

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

上記のコードは、のコード`Game1`と非常によく似ており`Robot` 、で`BasicEffect`マトリックスを割り当てます。 

これで、新しい`Camera`クラスを既存のプロジェクトに統合できるようになりました。 まず、クラスを変更し`Robot`て、 `Draw`メソッドの`Camera`インスタンスを取得します。これにより、重複するコードが多数削除されます。 メソッドを`Robot.Draw`次のように置き換えます。

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

次に、ファイル`Game1.cs`を変更します。

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

以前のバージョン ( `Game1`で`// New camera code`識別される) のに対する変更は次のとおりです。

- `Camera`のフィールド`Game1`
- `Camera`でのインスタンス化`Game1.Initialize`
- `Camera.Update`での呼び出し`Game1.Update`
- `Robot.Draw`パラメーターを受け取る`Camera`ようになりました。
- `Game1.Draw`はと`Camera.ViewMatrix`を使用するようになりました。`Camera.ProjectionMatrix`

## <a name="moving-the-camera-with-input"></a>入力によるカメラの移動

ここまでは、エンティティを`Camera`追加しましたが、実行時の動作を変更するための処理は何も行われていませんでした。 次の操作をユーザーに許可する動作を追加します。

- 画面の左側をタッチして、カメラを左に移動します。
- 画面の右側をタッチして、カメラを右に切り替えます。
- 画面の中央にタッチして、カメラを前方に移動する

### <a name="making-lookat-relative"></a>LookAt を相対的に作成する

まず、 `Camera`クラスを更新して、 `Camera`が`angle`接続している方向を設定するために使用されるフィールドを追加します。 現時点で`Camera`は、に`Vector3.Zero`割り当てられているローカル`lookAtVector`のでの方向が決定されます。 言い換えると、 `Camera`は常に配信元を調べます。 カメラが移動すると、カメラが直面している角度も変わります。

![](part3-images/image11.gif "カメラが移動した場合、カメラが直面している角度も変わります。")

は、 `Camera`その位置に関係なく同じ方向に接続するようにします。少なくとも、入力`Camera`を使用してを回転させるロジックを実装する必要があります。 最初の変更は、絶対位置を`lookAtVector`見るのではなく、現在の場所に基づいて変数を調整することです。

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

これにより、 `Camera`世界の表示が簡単になります。 を X 軸の`position`中央に配置するため`(0, 20, 10)`に、 `Camera`初期値がに変更されていることに注意してください。 ゲームを実行すると、次のように表示されます。

![](part3-images/image12.png "ゲームを実行すると、このビューが表示されます")

### <a name="creating-an-angle-variable"></a>Angle 変数の作成

変数`lookAtVector`は、カメラが表示している角度を制御します。 現在は、負の Y 軸を表示し、( `-.5f` Z 値から) 少し傾いていくように修正されています。 プロパティ`lookAtVector`を調整する`angle`ために使用される変数を作成します。 

このチュートリアルの前のセクションでは、マトリックスを使用してオブジェクトの描画方法をローテーションできることを示しました。 また、マトリックスを使用し`lookAtVector` `Vector3.Transform`て、メソッドを使用してのようなベクターを回転させることもできます。 

フィールドを追加し、次`ViewMatrix`のようにプロパティを変更します。 `angle`

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

### <a name="reading-input"></a>読み取り (入力を)

この`Camera`エンティティは、位置と角度の変数を使用して完全に制御できるようになりました。入力に従って変更するだけで済みます。

最初に、ユーザーが画面`TouchPanel`に触れる場所を見つけるための状態を取得します。 クラスの`TouchPanel`使用方法の詳細については、「[タッチパネル API リファレンス](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel)」を参照してください。

ユーザーが3番目の左に触れている場合は、 `angle`が左に`Camera`回転するように値を調整します。ユーザーが右3番目に触れると、他の方法で回転します。 ユーザーが画面の中央上に触れている場合は、 `Camera`先に進みます。

まず、で`TouchPanel` `TouchCollection`クラスとクラスを修飾するusingステートメントを追加します。`Camera.cs`

```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

次に、 `Update`メソッドを変更して、タッチパネルを読み取り、 `position`変数`angle`と変数を適切に調整します。

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

これで`Camera` 、はタッチ入力に応答します。

![](part3-images/image1.gif "カメラがタッチ入力に応答するようになりました")

Update メソッドは、を呼び`TouchPanel.GetState`出すことによって開始されます。これは、のコレクションを返します。 `TouchPanel.GetState`は複数のタッチポイントを返すことができますが、わかりやすくするために最初のタッチポイントだけを気にします。

ユーザーが画面に触れている場合、コードは、画面の左、中央、または右の3番目のタッチがあるかどうかを確認します。 左端と右端では、 `angle` `TotalSeconds`値に従って変数を増減して、カメラを回転させます (ゲームがフレームレートに関係なく同じように動作するようにします)。

ユーザーが画面の中央の3番目に触れると、カメラが先に進みます。 まず、前方ベクトルを取得します。これは、最初は負の Y 軸をポイントするように定義され、次に`Matrix.CreateRotationZ` 、 `angle`と値を使用して作成された行列によって回転されます。 最後に`forwardVector` 、 `unitsPerSecond`係数を`position`使用してにを適用します。

## <a name="summary"></a>Summary

このチュートリアルでは`Models` `BasicEffect.World` 、およびプロパティを使用して、 `Matrices` 3d 空間を移動および回転させる方法について説明します。 この形式の移動は、3D ゲームでオブジェクトを移動するための基礎となります。 このチュートリアルでは、任意の位置`Camera`や角度から世界を表示するためのエンティティを実装する方法についても説明します。

## <a name="related-links"></a>関連リンク

- [モノゲーム API リンク](http://www.monogame.net/documentation/?page=api)
- [完成したプロジェクト (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/monogame3dcamera)
