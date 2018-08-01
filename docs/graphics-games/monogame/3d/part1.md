---
title: モデル クラスを使用します。
description: モデル クラスでは、3 D グラフィックスの表示の従来の方法と比較した場合の複雑な 3D オブジェクトのレンダリングが大幅に簡略化します。 モデル オブジェクトは、カスタム コードがないのコンテンツを簡単に統合できるように、コンテンツ ファイルから作成されます。
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: e108bc6098d2c754428bf35e7312ebdc89459501
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783468"
---
# <a name="using-the-model-class"></a>モデル クラスを使用します。

_モデル クラスでは、3 D グラフィックスの表示の従来の方法と比較した場合の複雑な 3D オブジェクトのレンダリングが大幅に簡略化します。モデル オブジェクトは、カスタム コードがないのコンテンツを簡単に統合できるように、コンテンツ ファイルから作成されます。_

MonoGame API が含まれる、`Model`レンダリングを実行して、コンテンツ ファイルからデータを格納するために使用されるクラスが読み込まれています。 モデル ファイルは、(純色の付いた三角形) など、非常に単純な場合があります。 または複雑なレンダリングでは、テクスチャ、照明などの情報を含めることがあります。

このチュートリアルでは使用[ロボットの 3D モデル](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)次をカバーしています。

- 新しいゲーム プロジェクトを開始
- モデルおよびそのテクスチャの XNBs を作成します。
- ゲームのプロジェクトで、XNBs を含む
- 3D のモデルの描画
- 複数のモデルの描画

完了したら、プロジェクトが次のように表示されます。

![6 つロボットを示す完成したサンプル](part1-images/image1.png)

## <a name="creating-an-empty-game-project"></a>ゲームの空のプロジェクトを作成します。

最初に呼び出された MonoGame3D ゲーム プロジェクトを設定する必要があります。 新しい MonoGame プロジェクトを作成する方法については、次を参照してください。 [Cross Platform Monogame プロジェクトを作成するには、このチュートリアル](~/graphics-games/monogame/introduction/part1.md)です。

続行する前にする必要があります、プロジェクトが開くし、正しく展開を確認します。 1 回配置された空のブルー スクリーンいることを確認します。

![空の青いゲーム画面](part1-images/image2.png)


## <a name="including-the-xnbs-in-the-game-project"></a>ゲームのプロジェクトで、XNBs を含む

.Xnb ファイル形式がビルドされたコンテンツの標準の拡張機能 (で作成されたコンテンツ、 [MonoGame パイプライン ツール](http://www.monogame.net/documentation/?page=Pipeline))。 組み込みのすべてのコンテンツは、ソース ファイル (ある場合は、モデル、.fbx ファイル) と、コピー先ファイル (.xnb ファイル) がします。 .Fbx 形式などのアプリケーションで作成できる一般的な 3D モデル形式[Maya](http://www.autodesk.com/products/maya/overview)と[Blender](http://www.blender.org/)です。 

`Model`ファイルを読み込んで .xnb を 3D geometry 型のデータを含むディスクからクラスを構築することができます。   コンテンツのプロジェクトからこの .xnb ファイルが作成されます。 自動的に、Monogame テンプレートには、コンテンツ フォルダーのコンテンツ (拡張子 .mgcp) のプロジェクトが含まれます。 MonoGame パイプライン ツールの詳細については、次を参照してください。、[コンテンツ パイプライン ガイド](~/graphics-games/cocossharp/content-pipeline/index.md)です。

このガイドの MonoGame パイプラインを使用してスキップしますツールし、使用、します。XNB ファイルがここで説明します。 なお、します。XNB ファイル プラットフォームごとに異なるため、必ず使用しているどのプラットフォームに正しい XNB ファイルのセットを使用します。

解凍しましたが、 [Content.zip ファイル](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)お包含 .xnb ファイルを使用して、ゲームにできるようにします。 右クリックし、Android プロジェクトで作業すると場合に、**資産**内のフォルダー、 **WalkingGame.Android**プロジェクト。 右クリックし、iOS プロジェクトで作業すると場合に、 **WalkingGame.iOS**プロジェクト。 選択**の追加]-> [ファイルを追加しています.** で作業しているプラットフォームのフォルダーに両方 .xnb ファイルを選択します。

2 つのファイルは、今すぐプロジェクトの一部をする必要があります。

![Xnb ファイルとソリューション エクスプ ローラーのコンテンツ フォルダー](part1-images/xnbsinxs.png)

Visual Studio for Mac 新しく追加された XNBs のビルド アクションは自動的に設定できません。 各ファイルと選択 を右クリックし、iOS の**ビルド アクション メニューの BundleResource**です。 Android では、右クリックし、ファイルの各**ビルド アクション] メニューの [AndroidAsset**です。

## <a name="rendering-a-3d-model"></a>3D のモデルの表示

画面に表示されるモデルを表示するために必要な最後の手順では、読み込みと描画コードを追加します。 具体的には、おある、次を手順します。

- 定義する、`Model`インスタンス、`Game1`クラス
- 読み込み、`Model`インスタンス `Game1.LoadContent`
- 描画、`Model`インスタンス `Game1.Draw`

置換、`Game1.cs`コード ファイル (内に配置されていますが、 **WalkingGame** PCL) に次の。

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;

    // This is the model instance that we'll load
    // our XNB into:
    Model model;

    public Game1()
    {
        graphics = new GraphicsDeviceManager(this);
        graphics.IsFullScreen = true;

        Content.RootDirectory = "Content";
    }
    protected override void LoadContent()
    {
        // Notice that loading a model is very similar
        // to loading any other XNB (like a Texture2D).
        // The only difference is the generic type.
        model = Content.Load<Model> ("robot");
    }

    protected override void Update(GameTime gameTime)
    {
        base.Update(gameTime);
    }

    protected override void Draw(GameTime gameTime)
    {
        GraphicsDevice.Clear(Color.CornflowerBlue);

        // A model is composed of "Meshes" which are
        // parts of the model which can be positioned
        // independently, which can use different textures,
        // and which can have different rendering states
        // such as lighting applied.
        foreach (var mesh in model.Meshes)
        {
            // "Effect" refers to a shader. Each mesh may
            // have multiple shaders applied to it for more
            // advanced visuals. 
            foreach (BasicEffect effect in mesh.Effects)
            {
                // We could set up custom lights, but this
                // is the quickest way to get somethign on screen:
                effect.EnableDefaultLighting ();
                // This makes lighting look more realistic on
                // round surfaces, but at a slight performance cost:
                effect.PreferPerPixelLighting = true;

                // The world matrix can be used to position, rotate
                // or resize (scale) the model. Identity means that
                // the model is unrotated, drawn at the origin, and
                // its size is unchanged from the loaded content file.
                effect.World = Matrix.Identity;

                // Move the camera 8 units away from the origin:
                var cameraPosition = new Vector3 (0, 8, 0);
                // Tell the camera to look at the origin:
                var cameraLookAtVector = Vector3.Zero;
                // Tell the camera that positive Z is up
                var cameraUpVector = Vector3.UnitZ;

                effect.View = Matrix.CreateLookAt (
                    cameraPosition, cameraLookAtVector, cameraUpVector);

                // We want the aspect ratio of our display to match
                // the entire screen's aspect ratio:
                float aspectRatio = 
                    graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
                // Field of view measures how wide of a view our camera has.
                // Increasing this value means it has a wider view, making everything
                // on screen smaller. This is conceptually the same as "zooming out".
                // It also 
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                // Anything closer than this will not be drawn (will be clipped)
                float nearClipPlane = 1;
                // Anything further than this will not be drawn (will be clipped)
                float farClipPlane = 200;

                effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            }

            // Now that we've assigned our properties on the effects we can
            // draw the entire mesh
            mesh.Draw ();
        }
        base.Draw(gameTime);
    }
}
```

このコードを実行している場合は、モデルを画面に表示されるお表示されます。

![画面に表示される表示されているモデル](part1-images/image8.png "モデルが画面に表示される表示する場合はこのコードを実行すると、")

### <a name="model-class"></a>モデル クラス

`Model`クラスは、コンテンツ ファイル (.fbx ファイルなど) から 3 D レンダリングを実行するためのコア クラスです。 すべてのレンダリングでは、3 D のジオメトリ、テクスチャの参照を含むために必要な情報が含まれていますと`BasicEffect`インスタンスを配置、明るさ、およびカメラの値を制御します。

`Model`クラス自体に直接いない変数の配置に複数の場所で 1 つのモデルのインスタンスを表示することができますのでにこのガイドの後半で説明します。

各`Model`は 1 つ以上で構成されて`ModelMesh`を通じて公開されているインスタンス、`Meshes`プロパティです。 検討できますが、 `Model` 1 つのゲームとしてオブジェクト (ロボットや車)、各`ModelMesh`別に描画できる`BasicEffect`値。 たとえば、メッシュの個別の部分は、ロボットまたは車のホイールの区間を表すことがあります、私たちを割り当てることがあります、 `BasicEffect` wheels スピンまたは、脚部が机の値を移動します。 

### <a name="basiceffect-class"></a>BasicEffect クラス

`BasicEffect`クラスは、レンダリング オプションを制御するためのプロパティを提供します。 最初の変更を行う場合に、`BasicEffect`が呼び出されて、`EnableDefaultLighting`メソッドです。 名前が示すように、これにより、既定の照明は、ことを確認する場合に便利です、`Model`ゲーム内期待どおりに表示されます。 コメント アウトする場合、`EnableDefaultLighting`呼び出すには、レンダリングしますが、ない網掛けや反射の効果の光彩のテクスチャでモデルを見てみましょう。

```csharp
//effect.EnableDefaultLighting ();
```

![ない網掛けや反射の効果の光彩のテクスチャで表示されるモデル](part1-images/image9.png "がない網掛けや反射の効果の光彩のテクスチャで表示されるモデル")

`World`位置、回転、およびモデルのスケールを調整するプロパティを使用できます。 使用して上記のコード、`Matrix.Identity`値、つまり、`Model`ゲーム内指定どおり .fbx ファイルを表示します。 テーマは、マトリックスのほか、3 D の座標で詳しく[パート 3](~/graphics-games/monogame/3d/part3.md)、例としては、位置を変更できますが、`Model`変更することで、`World`次のようにプロパティ。

```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

このコードは、3 つのワールド単位で移動対象のオブジェクトが得られます。

![このコードを実行して 3 つのワールド単位移動対象のオブジェクトで](part1-images/image10.png "3 のワールド単位で上へ移動オブジェクトでこのコードを実行")

割り当てられている最後の 2 つのプロパティ、`BasicEffect`は`View`と`Projection`です。 テーマは、3 D のカメラで[パート 3](~/graphics-games/monogame/3d/part3.md)、お例としては、ローカルを変更することでカメラの位置を変更することは`cameraPosition`変数。

```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

分かりますカメラがこれ以上の背面に移動その結果、`Model`パースペクティブにより小さく表示。

![カメラがさらに戻し、パースペクティブによって小さく表示されるモデルの結果として得られるを移動](part1-images/image11.png "カメラがさらに戻し、パースペクティブによって小さく表示されるモデルの結果として得られるを移動")

## <a name="rendering-multiple-models"></a>複数のモデルの表示

1 つ前に述べたよう`Model`複数回を描画することができます。 これが容易になる移動、`Model`目的は、独自のメソッドにコードを描画`Model`位置パラメーターとして。 1 回終了したら、当社`Draw`と`DrawModel`メソッドのようになります。


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

これは、結果、ロボット 6 回に描画されているモデルになります。

![これは、結果、ロボット 6 回に描画されているモデルで](part1-images/image1.png "ロボット 6 回に描画されているモデルでこの結果")

## <a name="summary"></a>まとめ

このチュートリアルの導入 MonoGame の`Model`クラスです。 .Xnb に .fbx ファイルへの変換に対応するさらに読み込める、`Model`クラスです。 示す方法への変更、`BasicEffect`インスタンスに影響を与えることができます`Model`描画します。

## <a name="related-links"></a>関連リンク

- [MonoGame モデルのリファレンス](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [完成したプロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)
