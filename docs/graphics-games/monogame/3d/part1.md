---
title: モデル クラスを使用します。
description: モデル クラスでは、3 D グラフィックスのレンダリングが従来の方法と比較した場合の複雑な 3D オブジェクトのレンダリングが大幅に簡略化します。 モデル オブジェクトは、カスタム コードがなくてもコンテンツを簡単に統合できるように、コンテンツ ファイルから作成されます。
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: d9c73dfcee6321ecb314ca229db407c6d0438977
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108589"
---
# <a name="using-the-model-class"></a>モデル クラスを使用します。

_モデル クラスでは、3 D グラフィックスのレンダリングが従来の方法と比較した場合の複雑な 3D オブジェクトのレンダリングが大幅に簡略化します。モデル オブジェクトは、カスタム コードがなくてもコンテンツを簡単に統合できるように、コンテンツ ファイルから作成されます。_

MonoGame API には、`Model`レンダリングを実行して、コンテンツ ファイルからデータを格納するために使用できるクラスが読み込まれます。 モデル ファイルは、純色の付いた三角形) など、非常に単純な場合があります。 または複雑なレンダリングでは、テクスチャや光のなどの情報を含めることができます。

このチュートリアルでは使用[ロボットの 3D モデルを](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)し、次について説明します。

- 新しいゲーム プロジェクトの開始
- モデルとそのテクスチャの XNBs の作成
- ゲームのプロジェクトで、XNBs を含む
- 3D モデルの描画
- 複数のモデルの描画

完了したら、このプロジェクトは次のように表示されます。

![6 つのロボットを示す最終的なサンプル](part1-images/image1.png)

## <a name="creating-an-empty-game-project"></a>ゲームの空のプロジェクトを作成します。

最初に呼び出された MonoGame3D ゲーム プロジェクトを設定する必要があります。 新しい MonoGame プロジェクトを作成する方法の詳細については、次を参照してください。[クロス プラットフォーム Monogame プロジェクトを作成するには、このチュートリアル](~/graphics-games/monogame/introduction/part1.md)します。

続行する前にする必要がありますのプロジェクトが開き、正しく展開を確認します。 展開後、空のブルー スクリーンが表示されます。

![青色空のゲーム画面](part1-images/image2.png)


## <a name="including-the-xnbs-in-the-game-project"></a>ゲームのプロジェクトで、XNBs を含む

.Xnb ファイルの形式がビルドされたコンテンツを標準の拡張機能 (で作成されたコンテンツ、 [MonoGame パイプライン ツール](http://www.monogame.net/documentation/?page=Pipeline))。 組み込みのすべてのコンテンツは、ソース ファイル (つまり、モデルの場合、.fbx ファイル) および変換先のファイル (.xnb ファイル) を持ちます。 .Fbx 形式などのアプリケーションで作成できる一般的な 3D モデル形式[Maya](http://www.autodesk.com/products/maya/overview)と[Blender](http://www.blender.org/)します。 

`Model`ファイルを読み込んで .xnb 3D ジオメトリ データが含まれているディスクからクラスを作成できます。   コンテンツ プロジェクトを通じてこの .xnb ファイルが作成されます。 自動的に、Monogame テンプレートには、当社の Content フォルダのコンテンツ (拡張子 .mgcp) を含むプロジェクトが含まれます。 MonoGame パイプライン ツールの詳細については、次を参照してください。、[コンテンツ パイプライン ガイド](~/graphics-games/cocossharp/content-pipeline/index.md)します。

MonoGame パイプラインを使用して経由では省略しますこのガイドは、ツールを使用します。ここに含まれる XNB ファイル。 なお、します。XNB ファイル プラットフォームごとに異なるため、XNB ファイルの正しいセットを使用しているいずれのプラットフォームを使用してください。

解凍しましたが、 [Content.zip ファイル](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)包含 .xnb ファイル、ゲームで使用できるようにします。 Android プロジェクトに取り組んでいる場合を右クリックし、**資産**フォルダーで、 **WalkingGame.Android**プロジェクト。 IOS プロジェクトに取り組んでいる場合を右クリックし、 **WalkingGame.iOS**プロジェクト。 選択**の追加]-> [ファイルを追加しています.** で作業しているプラットフォームのフォルダーに両方の .xnb ファイルを選択します。

2 つのファイルは、プロジェクトの一部を今すぐする必要があります。

![ソリューション エクスプ ローラーのコンテンツ フォルダーは xnb ファイル](part1-images/xnbsinxs.png)

Visual Studio for Mac は、新しく追加された XNBs のビルド アクションを自動的に設定しない可能性があります。 IOS の各ファイルと選択を右クリックして**ビルド アクション BundleResource]-> [** します。 Android では、ファイル選択の各上を右クリックして**ビルド アクション AndroidAsset]-> [** します。

## <a name="rendering-a-3d-model"></a>3D モデルを表示

画面に表示される、モデルを表示するために必要な最後の手順では、読み込みと描画コードを追加します。 具体的には、私たちがする、次を手順します。

- 定義する、`Model`インスタンス、`Game1`クラス
- 読み込み、`Model`インスタンス `Game1.LoadContent`
- 描画、`Model`インスタンス `Game1.Draw`

置換、`Game1.cs`コード ファイル (である、 **WalkingGame** PCL) に次の。

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

このコードを実行した場合は、モデルを画面に表示される表示されます。

![画面上に表示されるモデル](part1-images/image8.png "モデルが画面に表示される表示する場合、このコードを実行すると、")

### <a name="model-class"></a>モデル クラス

`Model`クラスは (.fbx ファイル) などのコンテンツ ファイルから 3D レンダリングを実行するための中核となるクラスです。 すべてのレンダリングでは、3 D ジオメトリ、テクスチャの参照を含むために必要な情報が含まれていますと`BasicEffect`インスタンス、配置、光源の方向、およびカメラの値を制御します。

`Model`クラス自体に直接にこのガイドの後半で説明しますので、1 つのモデルのインスタンスは、複数の場所で表示できますを配置用の変数がありません。

各`Model`は 1 つまたは複数で構成されます`ModelMesh`を介して公開される、インスタンス、`Meshes`プロパティ。 検討できますが、`Model`として 1 つのゲーム オブジェクト (ロボットや車)、各`ModelMesh`別で描画できる`BasicEffect`値。 たとえば、メッシュの個別のパーツがロボットまたは車の車輪の区間を表すことができます、私たちを割り当てることがあります、`BasicEffect`車輪スピンまたは区間の値に移動します。 

### <a name="basiceffect-class"></a>BasicEffect クラス

`BasicEffect`クラスのレンダリング オプションを制御するためのプロパティを提供します。 最初の変更を行った、`BasicEffect`を呼び出すには、`EnableDefaultLighting`メソッド。 名前のとおり、これによりを検証するために非常に便利ですが、既定の照明を`Model`ゲーム期待どおりに表示されます。 コメント アウトする場合、`EnableDefaultLighting`呼び出す、のテクスチャがない掛けや反射の光彩のレンダリング モデルを見てみましょう。

```csharp
//effect.EnableDefaultLighting ();
```

![テクスチャがない掛けや反射の光彩のレンダリング モデル](part1-images/image9.png "のテクスチャがない掛けや反射の光彩のレンダリング モデル")

`World`位置、回転、およびモデルのスケールを調整するプロパティを使用できます。 使用上のコード、`Matrix.Identity`値、つまり、`Model`ゲーム指定どおり正確に .fbx ファイル内にレンダリングされます。 テーマは、マトリックス、および 3D 座標について詳しく[パート 3](~/graphics-games/monogame/3d/part3.md)、例としては、位置を変更できますが、`Model`変更することで、`World`次のようにプロパティ。

```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

このコードは、3 つのワールド単位で上へ移動するオブジェクトが得られます。

![このコードの 3 つのワールド単位で上へ移動オブジェクトで結果](part1-images/image10.png "3 のワールド単位で上へ移動オブジェクトでこのコードの結果")

割り当てられている最後の 2 つのプロパティ、`BasicEffect`は`View`と`Projection`します。 テーマは、3 D カメラで撮影[パート 3](~/graphics-games/monogame/3d/part3.md)、例として、ローカルに変更することで、カメラの位置を変更しますできますが、`cameraPosition`変数。

```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

わかりますカメラがそれ以上のバックアップを移動した結果として、`Model`パースペクティブにより小さく表示されます。

![パースペクティブにより小さく表示されるモデルの結果として、さらに戻る移動カメラ](part1-images/image11.png "カメラがパースペクティブにより小さく表示されるモデルの結果として、さらにバックアップを移動")

## <a name="rendering-multiple-models"></a>複数のモデルの表示

1 つ前に述べたよう`Model`複数回を描画することができます。 簡単に確認する移動弊社はこと、`Model`目的は、独自のメソッドにコードを描画`Model`をパラメーターとしての位置。 完了後、この`Draw`と`DrawModel`メソッドのようになります。


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

これは、ロボット 6 回に描画されているモデルが得られます。

![これは、結果、6 回に描画されているモデルのロボット](part1-images/image1.png "ロボット 6 回に描画されているモデルでこの結果")

## <a name="summary"></a>まとめ

このチュートリアルに導入された MonoGame の`Model`クラス。 、、.Xnb に .fbx ファイルの変換について説明しますが読み込むことがさらに、`Model`クラス。 表示方法に変更を`BasicEffect`インスタンスに影響を与えることができます`Model`描画します。

## <a name="related-links"></a>関連リンク

- [MonoGame モデルのリファレンス](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [完成したプロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)
