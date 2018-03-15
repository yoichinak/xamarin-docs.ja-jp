---
title: "MonoGame で 3D 座標"
description: "3D ゲーム開発における重要な手順は、3 D の座標系を理解することです。 MonoGame は、さまざまな位置、向きの調整、およびスケーリングの 3D 空間内のオブジェクトのクラスを提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: e930004a91133f391f68221473f212b7caaf1b07
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="3d-coordinates-in-monogame"></a>MonoGame で 3D 座標

_3D ゲーム開発における重要な手順は、3 D の座標系を理解することです。MonoGame は、さまざまな位置、向きの調整、およびスケーリングの 3D 空間内のオブジェクトのクラスを提供します。_

3D ゲーム開発には、3 D 座標系でオブジェクトを操作する方法を理解する必要があります。 このチュートリアルでは、visual オブジェクト (具体的にはモデル) を操作する方法について説明します。 3D カメラ クラスを作成するモデルを制御するという概念に基づいておビルドします。

線形代数由来の概念が提示されますが、強力な数式の背景の任意のユーザーが独自のゲームでこれらの概念を適用できるように、実践的なアプローチを移動します。

テーマは、次のトピック。

 - Visual C++ プロジェクト
 - ロボット エンティティを作成します。
 - ロボット エンティティの移動
 - 行列乗算
 - カメラのエンティティを作成します。
 - 入力でカメラの移動

完了後、必要があるプロジェクト、円でタッチ入力によって制御できるカメラ移動ロボット。

![](part3-images/image1.gif "完了後、アプリは、プロジェクト、円でタッチ入力によって制御できるカメラ移動ロボット")


# <a name="creating-a-project"></a>Visual C++ プロジェクト

このチュートリアルは、3 D 空間にオブジェクトの移動について説明します。 まず、プロジェクトのレンダリング モデル、および頂点の配列の[に含まれているここ](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)です。 ダウンロードされると、解凍し、実行して、次おが確認されていることを確認するプロジェクトを開きます。

![](part3-images/image2.png "ダウンロードされると、解凍し、実行して、このビューを表示するかを確認するプロジェクトを開く")


# <a name="creating-a-robot-entity"></a>ロボット エンティティを作成します。

作成、ロボットの周囲の移動を始める前に、`Robot`描画と移動のためのロジックを格納するクラス。 ゲームの開発者は、ロジックとデータとしてのこのカプセル化を参照してください、*エンティティ*です。

新しい空のクラス ファイルを追加、 **MonoGame3D**ポータブル クラス ライブラリ (プラットフォーム固有の ModelAndVerts.Android とは異なる)。 名前を付けます**ロボット** をクリック**新規**:

![](part3-images/image3.png "ロボットに名前を指定し、[新規] をクリックしてください")

変更、`Robot`クラスの次のようにします。


```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Content;

namespace MonoGame3D
{
    public class Robot
    {
        Model model;

        public void Initialize(ContentManager contentManager)
        {
            model = contentManager.Load<Model> ("robot");

        }

        // For now we'll take these values in, eventually we'll
        // take a Camera object
        public void Draw(Vector3 cameraPosition, float aspectRatio)
        {
            foreach (var mesh in model.Meshes)
            {
                foreach (BasicEffect effect in mesh.Effects)
                {
                    effect.EnableDefaultLighting ();
                    effect.PreferPerPixelLighting = true;

                    effect.World = Matrix.Identity; 
                    var cameraLookAtVector = Vector3.Zero;
                    var cameraUpVector = Vector3.UnitZ;

                    effect.View = Matrix.CreateLookAt (
                        cameraPosition, cameraLookAtVector, cameraUpVector);
                        
                    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                    float nearClipPlane = 1;
                    float farClipPlane = 200;

                    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);


                }

                // Now that we've assigned our properties on the effects we can
                // draw the entire mesh
                mesh.Draw ();
            }
        }
    }
}
```

`Robot`コードは基本的に同じコード`Game1`描画、`Model`です。 確認のため`Model`を読み込むと、描画を参照してください[モデルでの作業では、このガイド](~/graphics-games/monogame/3d/part1.md)です。 すべてを削除ようになりました、`Model`の読み込みとからコードをレンダリング`Game1`、置き換える、`Robot`インスタンス。


```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        Vector3 cameraPosition = new Vector3(15, 10, 10);

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;
                        
            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

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

            robot = new Robot ();
            robot.Initialize (Content);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            robot.Draw (cameraPosition, aspectRatio);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
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
    }                                          
}
```

場合はコードが実行されますがあるためシーンは描画されたほとんどの場合、floor 下にある 1 つだけのロボットで。

![](part3-images/image4.png "場合は、コードは、今すぐ実行は、アプリが表示されますシーンは描画されたほとんどの場合、floor 下にある 1 つだけのロボット")


# <a name="moving-the-robot"></a>ロボットを移動

これでが、`Robot`クラス、ロボットを移動ロジックを追加できます。 ここでは、単にしましょうロボットのゲームの時間に基づく円の中を移動します。 これは、文字が入力に応答が通常または人工知能ですが、3 D の位置と回転を調査するうえで環境を提供します。 以降の実際のゲームの多少不可能な場合の実装です。

唯一の情報の外部から必要があります、`Robot`クラスは、現在のゲームの時間。 追加、`Update`メソッドは、`GameTime`パラメーター。 これは、`GameTime`を使用して、ロボットの最後の位置を決定する角度変数をインクリメントするパラメーターが使用されます。

まず追加の角度にフィールドを`Robot`クラスの下にある、`model`フィールド。


```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ... 
```

 これでこの値を増やし、`Update`関数。


```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

確認する必要があります、`Update`からメソッドを呼び出した`Game1.Update`:


```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
} 
```

もちろん、この時点で、[角度] フィールドが何も行われません: を使用するコードを記述する必要があります。 変更して、`Draw`メソッド世界計算できるように`Matrix`専用メソッドで。 


```csharp
public void Draw(Vector3 cameraPosition, float aspectRatio)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            // We’ll be doing our calculations here...
            effect.World = GetWorldMatrix();

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);
                
            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }

        mesh.Draw ();
    }
} 
```

次に、実装、`GetWorldMatrix`メソッドで、`Robot`クラス。


```csharp
Matrix GetWorldMatrix()
{
    const float circleRadius = 8;
    const float heightOffGround = 3;
    
    // this matrix moves the model "out" from the origin
    Matrix translationMatrix = Matrix.CreateTranslation (
        circleRadius, 0, heightOffGround);

    // this matrix rotates everything around the origin
    Matrix rotationMatrix = Matrix.CreateRotationZ (angle);

    // We combine the two to have the model move in a circle:
    Matrix combined = translationMatrix * rotationMatrix;

    return combined;
} 
```

このコードを実行した結果は、円で囲んだ移動ロボットが得られます。

![](part3-images/image5.gif "このコードの結果を円で囲まれた移動ロボットで実行されています。")


# <a name="matrix-multiplication"></a>行列乗算

上記のコードは、作成することで、ロボットを回転、`Matrix`で、`GetWorldMatrix`メソッドです。 `Matrix`構造体には (位置を設定する) を変換、回転、拡大/縮小するために使用する 16 の浮動小数点値が含まれています (サイズの設定)。 代入すると、`effect.World`プロパティを指示することに、基になるシステムの位置、サイズ、および任意の方向を設定する方法を表示を描くことに発生する可能性 (、`Model`または頂点からジオメトリ)。 

さいわい、`Matrix`構造体には、いくつかの一般的な種類のマトリックスの作成を簡素化するメソッドが含まれています。 上記のコードで使用される 1 つ目は`Matrix.CreateTranslation`します。 数学用語*翻訳*参照ポイント (またはモデルの今回の場合) の結果を操作するその他のすべての変更 (回転、サイズ変更など) することがなく 1 つの場所から移動します。 この関数は、変換用の X、Y、Z 値を受け取ります。 場合は、トップダウンから、シーンを表示して、 `CreateTranslation` (分離) 内のメソッドは、次を実行します。

![](part3-images/image6.png "分離 CreateTranslation メソッドは、この操作を実行します。")

2 番目の行列を作成するには、回転行列を使用して、`CreateRotationZ`行列。 これは、回転の作成に使用できる 3 つのメソッドのいずれかです。

 - `CreateRotationX`
 - `CreateRoationY`
 - `CreateRotationZ`

各メソッドは、特定の軸を中心に回転して回転行列を作成します。 ここではおを「を」指し示します Z 軸を中心に回転します。 次は、軸ベースの回転を視覚化することができますの動作。

![](part3-images/image7.png "軸ベースの回転を視覚化に役立ちます works")

使用しても、`CreateRotationZ`角度フィールドでは、期限を時間の経過と共に増加メソッド、`Update`メソッドが呼び出されます。 結果は、`CreateRotationZ`メソッドは、時間の経過に伴って原点の周囲 orbit、ロボットをによりします。

コードの最後の行は、2 つの行列を 1 つに結合します。


```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

これは、これは通常の乗算と若干異なる行列乗算と呼ばれます。 *乗算の可換プロパティ*乗算演算での数値の順序に結果が変わらないことを示します。 つまり、3 * 4 は 4 * 3 に相当します。 行列乗算はという点では可換です。 つまり、「モデルを移動する translationMatrix を適用し、すべてのものを rotationMatrix を適用することで回転」は、上記の行として読み取ることができます。 上記の行に影響する位置と回転とおりの方法を視覚化して可能性があります。

![](part3-images/image8.png "視覚エフェクト pf 位置と回転上記の行に影響する方法")

行列乗算の順序が結果に影響を与える理解に役立つ、行列乗算が反転させて、次を考慮してください。


```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

上記のコードは最初、モデル、インプレースを回転し、変換します。

![](part3-images/image9.png "まず、モデル、インプレースを回転し、変換は上記のコード")

コードを反対側の乗算を実行お場合お気づきの回転が最初に適用されるため、モデルの向きにだけに影響し、モデルの位置は変わりません。 つまり、モデルは、場所に回転します。

![](part3-images/image10.gif "モデルが配置に回転します。")


# <a name="creating-the-camera-entity"></a>カメラのエンティティを作成します。

`Camera`エンティティにはすべての入力ベースの移動を実行してプロパティの割り当てのプロパティを指定するために必要なロジックが含まれます、`BasicEffect`クラスです。

まずおを静的カメラ (入力に基づく動かさず) を実装し、既存のプロジェクトに統合します。 新しいクラスを追加、 **MonoGame3D**ポータブル クラス ライブラリ (と同じプロジェクト`Robot.cs`) し、名前**カメラ**です。 このファイルの内容を次のコードに置き換えます。


```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Camera
    {
        // We need this to calculate the aspectRatio
        // in the ProjectionMatrix property.
        GraphicsDevice graphicsDevice;

        Vector3 position = new Vector3(15, 10, 10);

        public Matrix ViewMatrix
        {
            get
            {
                var lookAtVector = Vector3.Zero;
                var upVector = Vector3.UnitZ;

                return Matrix.CreateLookAt (
                    position, lookAtVector, upVector);
            }
        }

        public Matrix ProjectionMatrix
        {
            get
            {
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                float nearClipPlane = 1;
                float farClipPlane = 200;
                float aspectRatio = graphicsDevice.Viewport.Width / (float)graphicsDevice.Viewport.Height;
                
                return Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
            }
        }

        public Camera(GraphicsDevice graphicsDevice)
        {
            this.graphicsDevice = graphicsDevice;
        }

        public void Update(GameTime gameTime)
        {
            // We'll be doing some input-based movement here
        }
    }
}
```

上記のコードはからコードによく似た`Game1`と`Robot`上、マトリックスに代入する`BasicEffect`です。 

これで、新しいを統合できます`Camera`既存のプロジェクトにクラスです。 おを最初に、変更を`Robot`をクラス、`Camera`インスタンスその` Draw `メソッドで、重複したコードの多くを排除します。 置換、`Robot.Draw`を次のメソッド。


```csharp
public void Draw(Camera camera)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = GetWorldMatrix();
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;
        }

        mesh.Draw ();
    }
} 
```

次に、変更、`Game1.cs`ファイル。


```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        // New camera code
        Camera camera;

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;
                        
            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

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

            robot = new Robot ();
            robot.Initialize (Content);

            // New camera code
            camera = new Camera (graphics.GraphicsDevice);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            robot.Update (gameTime);
            // New camera code
            camera.Update (gameTime);
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            // New camera code
            robot.Draw (camera);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            // New camera code
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;

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
    }
} 
```

変更、`Game1`以前のバージョン (で識別される`// New camera code`) は。

 - `Camera` 内のフィールド `Game1`
 - `Camera` インスタンス化 `Game1.Initialize`
 - `Camera.Update` を呼び出し `Game1.Update`
 - `Robot.Draw` ここでは、`Camera`パラメーター
 - `Game1.Draw` ここでは使用して`Camera.ViewMatrix`と `Camera.ProjectionMatrix`


# <a name="moving-the-camera-with-input"></a>入力でカメラの移動

これまでに追加されました。、`Camera`エンティティが、実行時の動作を変更することで何も行っていません。 ユーザーを許可する動作は、追加します。

 - タッチ左側に向かってカメラを有効にする画面の左側にあります。
 - 右にカメラを有効にする画面の右側にあるをタッチします。
 - タッチ カメラを前方に移動する画面の中央


## <a name="making-lookat-relative"></a>相対パスを見せる場合を行う

まずが更新されます、`Camera`クラスを含める、`angle`フィールドに使用される方向を設定、`Camera`が直面しています。 現時点では、当社`Camera`ローカルを通じて向く方向を決定`lookAtVector`に割り当てられる`Vector3.Zero`です。 つまり、当社`Camera`原点を常に検索します。 カメラの移動し、カメラが直面している角度も変更されます。

![](part3-images/image11.gif "カメラの移動し、カメラが直面している角度も変更されます。")

たい、`Camera`少なくとも – の位置に関係なく同じ方向が直面するを回転させるためのロジックを実装してまで、` Camera `の入力を使用します。 最初の変更を調整する、`lookAtVector`絶対位置に探し求めているのではなく、変数、現在の場所から基にします。


```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    // Let's start at X = 0 so we're looking at things head-on
    Vector3 position = new Vector3(0, 20, 10);

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    } 
    ...
```

これは、結果、`Camera`直線の世界を表示します。 注意して、初期`position`に値が変更された`(0, 20, 10)`ため、`Camera`は X 軸の中央に配置します。 ゲームの表示を実行しています。

![](part3-images/image12.png "ゲームを実行するには、このビューが表示されます。")


## <a name="creating-an-angle-variable"></a>角度変数を作成します。

`lookAtVector`変数は、カメラが表示される角度を制御します。 負の値、Y 軸を表示するように固定現在と若干ダウン傾いた (から、 `-.5f` Z 値)。 ここで作成、`angle`を調整するために使用する変数、`lookAtVector`プロパティです。 

このチュートリアルの前のセクションでは、オブジェクトの描画方法を回転するマトリックスを使用できることを紹介しました。 マトリックスなどのベクトルを回転にも使用できます、`lookAtVector`を使用して、`Vector3.Transform`メソッドです。 

追加、`angle`フィールドし、変更、`ViewMatrix`次のようにプロパティ。


```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    Vector3 position = new Vector3(0, 20, 10);

    float angle;

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            // We'll create a rotation matrix using our angle
            var rotationMatrix = Matrix.CreateRotationZ (angle);
            // Then we'll modify the vector using this matrix:
            lookAtVector = Vector3.Transform (lookAtVector, rotationMatrix);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    } 
    ...
```


## <a name="reading-input"></a>入力を読み取る

当社`Camera`エンティティ今すぐ完全に制御できますの位置と角度変数を介して – だけ入力に従ってそれらを変更する必要があります。

最初になると、`TouchPanel`をユーザーが画面に触れてを検索する状態。 使用する方法について、`TouchPanel`クラスを参照してください[タッチパネル API リファレンス](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel)です。

かどうかは、ユーザーが、3 番目の左上に触れること、調整を`angle`値ため、`Camera`左に回転させます。 その逆の方向を回転おを場合は、ユーザーが、3 番目の右上に触れることをします。 画面の中央の 3 番目のユーザーと接触しているかどうかに移動します、`Camera`転送します。

使用して、最初に、追加を修飾するステートメント、`TouchPanel`と`TouchCollection`クラス`Camera.cs`:


```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

次に、変更、`Update`タッチ パネルを読み取ったり、調整する方法、`angle`と`position`変数適切に。


```csharp
public void Update(GameTime gameTime)
{
    TouchCollection touchCollection = TouchPanel.GetState();

    bool isTouchingScreen = touchCollection.Count > 0;
    if (isTouchingScreen)
    {
        var xPosition = touchCollection [0].Position.X;

        float xRatio = xPosition / (float)graphicsDevice.Viewport.Width;

        if (xRatio < 1 / 3.0f)
        {
            angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
        else if (xRatio < 2 / 3.0f)
        {
            var forwardVector = new Vector3 (0, -1, 0);

            var rotationMatrix = Matrix.CreateRotationZ (angle);
            forwardVector = Vector3.Transform (forwardVector, rotationMatrix);

            const float unitsPerSecond = 3;

            this.position += forwardVector * unitsPerSecond *
                (float)gameTime.ElapsedGameTime.TotalSeconds ;
        }
        else
        {
            angle -= (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
    }
} 
```

これで、`Camera`はタッチ入力に応答します。

![](part3-images/image1.gif "カメラがタッチ入力に応答するようになりました")

Update メソッドを呼び出すことによって開始`TouchPanel.GetState`仕上げのコレクションが返されます。 `TouchPanel.GetState`複数のタッチ ポイントを返すことができますのみ心配わかりやすくするための最初の 1 つです。

場合は、ユーザーは、画面に触れること、コードを最初のタッチが左、中央、または画面の右に 3 つ目のかどうかを確認します。 左と右の 3 分が増加または減少により、カメラを回転、`angle`変数によると、 `TotalSeconds` (できるようにするゲームでは、フレーム レートに関係なく同じ動作) の値します。

ユーザーが中央に 3 つ目に触れること場合、画面の カメラが前面に移動します。 これは、情報は最初は、最初に負の値、Y 軸方向として定義し、回転行列を使用して作成を使用して、前方向ベクトルを取得することによって`Matrix.CreateRotationZ`と`angle`値。 最後に、`forwardVector`に適用される`position`を使用して、`unitsPerSecond`係数。


# <a name="summary"></a>まとめ

このチュートリアルは、移動、回転をする方法を説明`Models`を使用して、領域の 3D で`Matrices`と`BasicEffect.World`プロパティです。 移動のこのフォームは、3 D ゲームでオブジェクトの移動の基礎を提供します。 このチュートリアルでは、実装する方法についても説明、`Camera`任意の位置および角度から世界を表示するためのエンティティです。

## <a name="related-links"></a>関連リンク

- [MonoGame API のリンク](http://www.monogame.net/documentation/?page=api)
- [完成したプロジェクト (サンプル)](https://developer.xamarin.com/samples/monodroid/MonoGame3DCamera/)
