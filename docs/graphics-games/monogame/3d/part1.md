---
title: モデルクラスの使用
description: モデルクラスは、従来の3D グラフィックスレンダリングの方法と比較して、複雑な3D オブジェクトのレンダリングを大幅に簡略化します。 モデルオブジェクトはコンテンツファイルから作成されます。これにより、カスタムコードを使用せずにコンテンツを簡単に統合できます。
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: c5702780b6a0f0732d846a2cd4226aec5e49fc21
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "70766832"
---
# <a name="using-the-model-class"></a>モデルクラスの使用

_モデルクラスは、従来の3D グラフィックスレンダリングの方法と比較して、複雑な3D オブジェクトのレンダリングを大幅に簡略化します。モデルオブジェクトはコンテンツファイルから作成されます。これにより、カスタムコードを使用せずにコンテンツを簡単に統合できます。_

モノゲーム API には、コンテンツファイルから読み込まれたデータを格納したり、レンダリングを実行したりするために使用できる `Model` クラスが含まれています。 モデルファイルは非常に単純な場合があります (たとえば、純色の三角形)。または、テクスチャや照明など、複雑なレンダリングのための情報が含まれている場合があります。

このチュートリアルでは、[ロボットの3d モデルを](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)使用して、次の内容について説明します。

- 新しいゲームプロジェクトの開始
- モデルとそのテクスチャの XNBs を作成する
- ゲームプロジェクトに XNBs を含める
- 3D モデルの描画
- 複数のモデルの描画

完了すると、次のようにプロジェクトが表示されます。

![6つのロボットを示す完成したサンプル](part1-images/image1.png)

## <a name="creating-an-empty-game-project"></a>空のゲームプロジェクトを作成する

まず、MonoGame3D という名前のゲームプロジェクトを設定する必要があります。 新しいモノゲームプロジェクトを作成する方法の詳細については、[クロスプラットフォームのモノゲームプロジェクトの作成に関するこのチュートリアル](~/graphics-games/monogame/introduction/part1.md)を参照してください。

先に進む前に、プロジェクトが開いて正しく配置されていることを確認する必要があります。 デプロイされると、空のブルースクリーンが表示されます。

![空の青いゲーム画面](part1-images/image2.png)

## <a name="including-the-xnbs-in-the-game-project"></a>ゲームプロジェクトに XNBs を含める

Xnb ファイル形式は、ビルドされたコンテンツ ([モノゲームパイプラインツール](http://www.monogame.net/documentation/?page=Pipeline)によって作成されたコンテンツ) の標準の拡張機能です。 すべてのビルドされたコンテンツには、ソースファイル (モデルの場合は fbx ファイル) と変換先ファイル (xnb ファイル) があります。 Fbx 形式は、 [Maya](http://www.autodesk.com/products/maya/overview)や[Blender](http://www.blender.org/)などのアプリケーションで作成できる一般的な3d モデル形式です。 

@No__t_0 クラスは、3D geometry データを格納するディスクから、xnb ファイルを読み込むことによって作成できます。   この xnb ファイルは、コンテンツプロジェクトを通じて作成されます。 モノゲームテンプレートでは、コンテンツフォルダーにコンテンツプロジェクトが自動的に含まれます。 モノゲームパイプラインツールの詳細については、「[コンテンツパイプラインガイド](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md)」を参照してください。

このガイドでは、モノゲームパイプラインツールの使用をスキップし、を使用します。ここには XNB ファイルが含まれています。 であることに注意してください。XNB ファイルはプラットフォームごとに異なります。そのため、使用しているプラットフォームごとに正しい XNB ファイルセットを使用してください。

[コンテンツ .zip ファイル](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)を解凍して、含まれている xnb ファイルをゲームで使用できるようにします。 Android プロジェクトで作業している場合は、プレイ中の**android**プロジェクトの**Assets**フォルダーを右クリックします。 IOS プロジェクトで作業している場合は、プレイ中の**ios**プロジェクトを右クリックします。 **> 追加** を選択して ファイルの追加 を選択し、作業しているプラットフォームのフォルダーにある xnb ファイル を選択します。

2つのファイルは、プロジェクトの一部になります。

![Xnb ファイルを含むソリューションエクスプローラーコンテンツフォルダー](part1-images/xnbsinxs.png)

新しく追加された XNBs のビルドアクションは、Visual Studio for Mac によって自動的に設定されない場合があります。 IOS の場合は、各ファイルを右クリックし、 **[ビルドアクション-> BundleResource]** を選択します。 Android の場合は、各ファイルを右クリックし、 **[ビルドアクション-> AndroidAsset]** を選択します。

## <a name="rendering-a-3d-model"></a>3D モデルのレンダリング

モデルを画面上に表示するために必要な最後の手順は、読み込みと描画のコードを追加することです。 具体的には、次のことを行います。

- @No__t_1 クラスでの `Model` インスタンスの定義
- @No__t_1 に `Model` インスタンスを読み込んでいます
- @No__t_1 での `Model` インスタンスの描画

@No__t_0 コードファイル (**ゲーム**PCL にある) を次のコードに置き換えます。

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;

    // This is the model instance that we'll load
    // our XNB into:
    Model model;

    public Game1()
    {
        graphics = new GraphicsDeviceManager(this);
        graphics.IsFullScreen = true;

        Content.RootDirectory = "Content";
    }
    protected override void LoadContent()
    {
        // Notice that loading a model is very similar
        // to loading any other XNB (like a Texture2D).
        // The only difference is the generic type.
        model = Content.Load<Model> ("robot");
    }

    protected override void Update(GameTime gameTime)
    {
        base.Update(gameTime);
    }

    protected override void Draw(GameTime gameTime)
    {
        GraphicsDevice.Clear(Color.CornflowerBlue);

        // A model is composed of "Meshes" which are
        // parts of the model which can be positioned
        // independently, which can use different textures,
        // and which can have different rendering states
        // such as lighting applied.
        foreach (var mesh in model.Meshes)
        {
            // "Effect" refers to a shader. Each mesh may
            // have multiple shaders applied to it for more
            // advanced visuals. 
            foreach (BasicEffect effect in mesh.Effects)
            {
                // We could set up custom lights, but this
                // is the quickest way to get somethign on screen:
                effect.EnableDefaultLighting ();
                // This makes lighting look more realistic on
                // round surfaces, but at a slight performance cost:
                effect.PreferPerPixelLighting = true;

                // The world matrix can be used to position, rotate
                // or resize (scale) the model. Identity means that
                // the model is unrotated, drawn at the origin, and
                // its size is unchanged from the loaded content file.
                effect.World = Matrix.Identity;

                // Move the camera 8 units away from the origin:
                var cameraPosition = new Vector3 (0, 8, 0);
                // Tell the camera to look at the origin:
                var cameraLookAtVector = Vector3.Zero;
                // Tell the camera that positive Z is up
                var cameraUpVector = Vector3.UnitZ;

                effect.View = Matrix.CreateLookAt (
                    cameraPosition, cameraLookAtVector, cameraUpVector);

                // We want the aspect ratio of our display to match
                // the entire screen's aspect ratio:
                float aspectRatio = 
                    graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
                // Field of view measures how wide of a view our camera has.
                // Increasing this value means it has a wider view, making everything
                // on screen smaller. This is conceptually the same as "zooming out".
                // It also 
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                // Anything closer than this will not be drawn (will be clipped)
                float nearClipPlane = 1;
                // Anything further than this will not be drawn (will be clipped)
                float farClipPlane = 200;

                effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            }

            // Now that we've assigned our properties on the effects we can
            // draw the entire mesh
            mesh.Draw ();
        }
        base.Draw(gameTime);
    }
}
```

このコードを実行すると、モデルが画面上に表示されます。

![画面に表示されるモデル](part1-images/image8.png "このコードを実行すると、モデルは画面に表示されます。")

### <a name="model-class"></a>モデルクラス

@No__t_0 クラスは、コンテンツファイル (fbx ファイルなど) からの3D レンダリングを実行するためのコアクラスです。 これには、表示に必要なすべての情報が含まれています。これには、3D ジオメトリ、テクスチャ参照、および位置、照明、カメラの値を制御する `BasicEffect` インスタンスが含まれます。

@No__t_0 クラス自体には、このガイドの後半で説明するように、1つのモデルインスタンスを複数の場所にレンダリングできるため、配置用の変数は直接ありません。

各 `Model` は、`Meshes` プロパティを通じて公開される1つ以上の `ModelMesh` インスタンスで構成されます。 @No__t_0 は1つのゲームオブジェクト (ロボットや車など) として考えられるかもしれませんが、各 `ModelMesh` は異なる `BasicEffect` 値で描画できます。 たとえば、個々のメッシュパーツが、車のロボットまたは車輪の脚を表す場合があります。また、`BasicEffect` 値を割り当てて、車輪を回転させたり、脚を移動させたりすることがあります。 

### <a name="basiceffect-class"></a>BasicEffect クラス

@No__t_0 クラスには、表示オプションを制御するためのプロパティが用意されています。 @No__t_0 に対して行う最初の変更は、`EnableDefaultLighting` メソッドを呼び出すことです。 名前が示すように、これにより既定の照明が有効になります。これは、`Model` が意図したとおりにゲームに表示されることを確認するのに非常に便利です。 @No__t_0 の呼び出しをコメントアウトすると、テクスチャだけを使用してレンダリングされたモデルが表示されますが、網掛けや反射グローはありません。

```csharp
//effect.EnableDefaultLighting ();
```

![テクスチャだけでレンダリングされるモデルですが、網掛けや反射グローはありません。](part1-images/image9.png "テクスチャだけでレンダリングされるモデルですが、網掛けや反射グローはありません。")

@No__t_0 プロパティを使用して、モデルの位置、回転、およびスケールを調整できます。 上記のコードでは、`Matrix.Identity` 値を使用しています。これは、`Model` が fbx ファイルで指定されたとおりにゲーム内でレンダリングされることを意味します。 ここでは、[第3部](~/graphics-games/monogame/3d/part3.md)でマトリックスと3d 座標について詳しく説明しますが、例として、`World` プロパティを次のように変更して `Model` の位置を変更することができます。

```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

このコードを実行すると、オブジェクトが3つのワールド単位で上に移動します。

![このコードにより、オブジェクトが3つのワールド単位で上に移動します。](part1-images/image10.png "このコードにより、オブジェクトが3つのワールド単位で上に移動します。")

@No__t_0 に割り当てられた最後の2つのプロパティは `View` と `Projection` です。 [第3部](~/graphics-games/monogame/3d/part3.md)では3d カメラについて説明しますが、例として、ローカル `cameraPosition` 変数を変更して、カメラの位置を変更することができます。

```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

カメラがさらに後ろに移動したことがわかります。結果として、パースペクティブによって `Model` が小さくなります。

![カメラがさらに後ろに移動したため、パースペクティブによってモデルが小さくなります。](part1-images/image11.png "カメラがさらに後ろに移動したため、パースペクティブによってモデルが小さくなります。")

## <a name="rendering-multiple-models"></a>複数のモデルのレンダリング

前述のように、1つの `Model` を複数回描画できます。 これを簡単にするために、`Model` の描画コードを、目的の `Model` 位置をパラメーターとして受け取る独自のメソッドに移動します。 完了すると、`Draw` と `DrawModel` のメソッドは次のようになります。

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);
    DrawModel (new Vector3 (-4, 0, 0));
    DrawModel (new Vector3 ( 0, 0, 0));
    DrawModel (new Vector3 ( 4, 0, 0));
    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));
    base.Draw(gameTime);
}
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            effect.World = Matrix.CreateTranslation (modelPosition);
            var cameraPosition = new Vector3 (0, 10, 0);
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
        }
        // Now that we've assigned our properties on the effects we can
        // draw the entire mesh
        mesh.Draw ();
    }
}
```

この結果、ロボットモデルが6回描画されます。

![この結果、ロボットモデルが6回描画されます。](part1-images/image1.png "この結果、ロボットモデルが6回描画されます。")

## <a name="summary"></a>まとめ

このチュートリアルでは、モノゲームの `Model` クラスを導入しました。 この記事では、fbx ファイルを xnb に変換する方法について説明します。これは、`Model` クラスに読み込むことができます。 また、`BasicEffect` インスタンスに対する変更が `Model` 描画に与える影響についても示します。

## <a name="related-links"></a>関連リンク

- [モノゲームモデルリファレンス](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [コンテンツ .zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [完成したプロジェクト (サンプル)](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelrenderingmg/)
