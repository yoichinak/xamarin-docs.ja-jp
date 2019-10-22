---
title: UrhoSharp の概要
description: このドキュメントでは、UrhoSharp アプリケーションの基本的な構造について説明し、UrhoSharp の使用方法を示すさまざまなガイドやサンプルアプリケーションへのリンクを示します。
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 441a3cc19b4246fb2bdea54508142a894af5c051
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "67832541"
---
# <a name="introduction-to-urhosharp"></a>UrhoSharp の概要

![UrhoSharp ロゴ](introduction-images/urhosharp-icon.png)

UrhoSharp は、Xamarin および .NET 開発者向けの強力な3D ゲームエンジンです。  これは、Apple の SceneKit と SpriteKit に似ています。また、クロスプラットフォームでも、物理、ナビゲーション、ネットワークなどが含まれています。

これは[Urho3D](http://urho3d.github.io/)エンジンへの .net バインドであり、開発者は、同じコードベースで Android、IOS、Windows、および Mac を対象とし、OpenGL システムと Direct3D システムの両方にレンダリングできるクロスプラットフォームコードを作成できます。

UrhoSharp は、さまざまな機能を備えたゲームエンジンです。

- 強力な3D グラフィックレンダリング
- 物理シミュレーション (箇条書きライブラリを使用)
- シーンの処理
- Await/Async のサポート
- フレンドリアクション API
- 3D シーンへの2D 統合
- FreeType を使用したフォントレンダリング
- クライアントとサーバーのネットワーク機能
- さまざまなアセットをインポートする (Open Assets ライブラリを含む)
- ナビゲーションメッシュと pathfinding (再キャスト/迂回を使用)
- 競合検出のための凸ハル生成 (StanHull を使用)
- オーディオ再生 ( **libvorbis**あり)

## <a name="get-started"></a>作業開始

UrhoSharp は、 [NuGet パッケージ](https://www.nuget.org/)として簡単に配布され、Windows C# 、 F# Mac、Android、または iOS を対象とするまたはプロジェクトに追加できます。  NuGet には、プログラムを実行するために必要なライブラリと、エンジンで使用される基本的なアセット (CoreData) の両方が付属しています。

### <a name="urho-as-a-portable-class-library"></a>ポータブルクラスライブラリとしての Urho

Urho パッケージは、プラットフォーム固有のプロジェクトまたはポータブルクラスライブラリプロジェクトから使用できます。これにより、すべてのプラットフォームですべてのコードを再利用することができます。  つまり、各プラットフォームに対して行う必要があるのは、プラットフォーム固有のエントリポイントを記述し、共有ゲームコードに制御を移すことだけです。

### <a name="samples"></a>サンプル

Urho の機能を確認するには、のサンプルソリューションで Visual Studio for Mac または Visual Studio のいずれかを開きます。

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

既定のソリューションには、Android、iOS、Windows、および Mac 用のプロジェクトが含まれています。  このソリューションは、プラットフォーム固有の起動ツールが用意されており、すべてのサンプルコードとゲームコードがポータブルクラスライブラリに存在するように構成されており、すべてのプラットフォームでコードの再利用を最大化する方法を示しています。

独自のソリューションを作成する方法の詳細については、 [Urho とプラットフォーム](~/graphics-games/urhosharp/platform/index.md)に関するページを参照してください。

すべてのサンプルはユーザーインターフェイス要素の共通セットを共有しているため、サンプルではこのファイルの基本的なセットアップを抽象化しています。

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

これには、いくつかの基本的なキーストロークとタッチイベントの処理、カメラの設定、基本的なユーザーインターフェイス要素の提供を行うサンプル基本クラスが用意されています。これにより、各サンプルでは、紹介されている特定の機能に焦点を当てることができます。

エンジンが実行できる操作を次の例に示します。

- [Samply Game](https://github.com/xamarin/urho-samples/tree/master/SamplyGame)は、ShootySkies の単純な複製です。

他のサンプルでは、各サンプルの個々のプロパティを示しています。

## <a name="basic-structure"></a>基本構造

ゲームでは `Application` クラスをサブクラス化する必要があります。これは、(`Setup` 方法で) ゲームを設定し、(`Start` 方法で) ゲームを開始する場所です。  次に、メインユーザーインターフェイスを構築します。  ここでは、3D シーン、いくつかの UI 要素を設定して、単純な動作をアタッチするための Api を示す小さなサンプルについて説明します。

```csharp
class MySample : Application {
    protected override void Start ()
    {
        CreateScene ();
        Input.KeyDown += (args) => {
            if (args.Key == Key.Esc) Exit ();
        };
    }

    async void CreateScene()
    {
        // UI text
        var helloText = new Text()
        {
            Value = "Hello World from MySample",
            HorizontalAlignment = HorizontalAlignment.Center,
            VerticalAlignment = VerticalAlignment.Center
        };
        helloText.SetColor(new Color(0f, 1f, 1f));
        helloText.SetFont(
            font: ResourceCache.GetFont("Fonts/Font.ttf"),
            size: 30);
        UI.Root.AddChild(helloText);

        // Create a top-level scene, must add the Octree
        // to visualize any 3D content.
        var scene = new Scene();
        scene.CreateComponent<Octree>();
        // Box
        Node boxNode = scene.CreateChild();
        boxNode.Position = new Vector3(0, 0, 5);
        boxNode.Rotation = new Quaternion(60, 0, 30);
        boxNode.SetScale(0f);
        StaticModel modelObject = boxNode.CreateComponent<StaticModel>();
        modelObject.Model = ResourceCache.GetModel("Models/Box.mdl");
        // Light
        Node lightNode = scene.CreateChild(name: "light");
        lightNode.SetDirection(new Vector3(0.6f, -1.0f, 0.8f));
        lightNode.CreateComponent<Light>();
        // Camera
        Node cameraNode = scene.CreateChild(name: "camera");
        Camera camera = cameraNode.CreateComponent<Camera>();
        // Viewport
        Renderer.SetViewport(0, new Viewport(scene, camera, null));
        // Perform some actions
        await boxNode.RunActionsAsync(
            new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
        await boxNode.RunActionsAsync(
            new RepeatForever(new RotateBy(duration: 1,
                deltaAngleX: 90, deltaAngleY: 0, deltaAngleZ: 0)));
     }
}
```

このアプリケーションを実行すると、自分の資産が存在しないことをランタイムが通知していることがすぐにわかります。  必要な作業は、プロジェクト内に、特別なディレクトリ名 "Data" で始まる階層を作成することです。この中には、プログラムで参照するアセットを配置します。  その後、[出力ディレクトリにコピー] を [新しい場合はコピーする] に設定して、各資産の項目のプロパティにを設定する必要があります。これにより、データが確実に作成されます。

ここで何が起こっているかを説明しましょう。

アプリケーションを起動するには、エンジン初期化関数を呼び出した後、次のようにアプリケーションクラスの新しいインスタンスを作成します。

```csharp
new MySample().Run();
```

ランタイムは、`Setup` と `Start` メソッドを呼び出します。  @No__t_0 をオーバーライドする場合は、エンジンパラメーターを構成できます (このサンプルでは示していません)。

これによってゲームが開始されるため、`Start` をオーバーライドする必要があります。  この方法では、アセットの読み込み、イベントハンドラーの接続、シーンの設定、必要なアクションの開始を行います。  このサンプルでは、2つの UI を作成して、ユーザーに表示したり、3D シーンを設定したりします。

次のコードでは、UI フレームワークを使用してテキスト要素を作成し、アプリケーションに追加しています。

```csharp
// UI text
var helloText = new Text()
{
    Value = "Hello World from UrhoSharp",
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center
};
helloText.SetColor(new Color(0f, 1f, 1f));
helloText.SetFont(
    font: ResourceCache.GetFont("Fonts/Font.ttf"),
    size: 30);
UI.Root.AddChild(helloText);
```

UI フレームワークは、非常に単純なゲーム内ユーザーインターフェイスを提供するためのものであり、`UI.Root` ノードに新しいノードを追加することによって機能します。

このサンプルの2番目の部分では、メインシーンを設定します。  これには、3D シーンの作成、画面での3D ボックスの作成、ライト、カメラ、ビューポートの追加など、さまざまな手順が含まれます。  これらの詳細については、「[シーン、ノード、コンポーネント、およびカメラ](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras)」を参照してください。

このサンプルの3番目の部分では、いくつかのアクションをトリガーします。  アクションは、特定の効果を記述するレシピであり、作成された後、`Node` で `RunActionAsync` メソッドを呼び出すことによって、必要に応じてノードによって実行できます。

最初のアクションでは、ボックスがバウンス効果で拡大縮小され、2番目のアクションでボックスが永久に回転します。

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

上の例では、最初に作成したアクションが `ScaleTo` アクションであることを示しています。これは単に、ノードの scale プロパティの値に対して1秒間にスケールを設定することを示すレシピです。  この操作は、イージングアクション (`EaseBounceOut` アクション) を中心にラップされます。  イージングアクションは、アクションの線形実行を歪曲し、効果を適用します。この場合、この例では、バウンス効果が提供されます。
このレシピは次のように記述できます。

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

レシピを作成したら、次のようにレシピを実行します。

```csharp
await boxNode.RunActionsAsync (recipe)
```

Await は、アクションが完了したときに、この行の後にが実行を再開することを示します。  アクションが完了すると、2番目のアニメーションがトリガーされます。

[UrhoSharp を使用](~/graphics-games/urhosharp/using.md)するドキュメントでは、Urho の背後にある概念と、ゲームを構築するためのコードを構築する方法について詳しく説明します。

## <a name="copyrights"></a>著作権

このドキュメントには、Xamarin Inc. の元のコンテンツが含まれていますが、Urho3D プロジェクトのオープンソースドキュメントから広く使用されており、Cocos2D プロジェクトのスクリーンショットが含まれています。
