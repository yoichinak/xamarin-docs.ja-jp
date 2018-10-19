---
title: UrhoSharp の概要
description: このドキュメントでは、UrhoSharp のアプリケーションとさまざまなガイドと UrhoSharp を使用する方法を示すサンプル アプリケーションへのリンクの基本構造について説明します。
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: a3e14ebca961e828fc578035adaca5ba2a809438
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2018
ms.locfileid: "34783556"
---
# <a name="an-introduction-to-urhosharp"></a>UrhoSharp の概要

![UrhoSharp のロゴ](introduction-images/urhosharp-icon.png)

UrhoSharp は、Xamarin と .NET の開発者向けの強力な 3D ゲーム エンジンです。  Apple の SceneKit と SpriteKit を精神と類似していて、物理運動を含めるか、ナビゲーション、ネットワーク、およびその多くは、クロス プラットフォームが中により。

バインドを .NET では、 [Urho3D](http://urho3d.github.io/)エンジン、により、開発者は、Android、iOS を対象とするクロス プラットフォーム コードを記述して、Windows と Mac で、同じコードベースおよび OpenGL と Direct3D の両方のシステムにレンダリングできます。

UrhoSharp では、多くの機能をすぐにゲーム エンジンを示します。

- 強力な 3D グラフィックのレンダリング
- [シミュレーションの物理運動](https://developer.xamarin.com/api/namespace/Urho.Physics/)(行頭文字ライブラリを使用)
- [シーンの処理](https://developer.xamarin.com/api/type/Urho.Scene/)
- Await/非同期サポート
- [わかりやすい操作 API](https://developer.xamarin.com/api/namespace/Urho.Actions/)
- [3D シーンを 2D の統合](https://developer.xamarin.com/api/namespace/Urho.Urho2D/)
- [FreeType を使用したフォントのレンダリング](https://developer.xamarin.com/api/type/Urho.Gui.FontFaceFreeType/)
- [クライアントとサーバーのネットワーク機能](https://developer.xamarin.com/api/namespace/Urho.Network/)
- [さまざまなアセットをインポート](https://developer.xamarin.com/api/namespace/Urho.Resources/)(で開いているアセット ライブラリ)
- [ナビゲーションのメッシュと pathfinding](https://developer.xamarin.com/api/namespace/Urho.Navigation/) (再キャスト/迂回を使用)
- [衝突検出の凸包生成](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/)(StanHull を使用)
- [オーディオ再生](https://developer.xamarin.com/api/namespace/Urho.Audio/)(で**libvorbis**)

## <a name="getting-started"></a>作業の開始

UrhoSharp は便利なこととして配布されます、 [NuGet パッケージ](https://www.nuget.org/)それに追加できる、c# または f# プロジェクト Windows、Mac、Android または iOS を対象とします。  NuGet のエンジンによって使用される基本的な資産 (CoreData) だけでなく、プログラムの実行に必要なライブラリが付属します。

### <a name="urho-as-a-portable-class-library"></a>ポータブル クラス ライブラリとして Urho

Urho パッケージは、プラットフォーム固有プロジェクト、またはすべてのプラットフォームですべてのコードを再利用することができます、ポータブル クラス ライブラリ プロジェクトから使用できます。  つまり、プラットフォームの特定のエントリ ポイントを作成し、共有のゲームのコードに制御を転送はすべて、各プラットフォームで実行する必要があります。

### <a name="samples"></a>サンプル

Urho の機能を体験からサンプル ソリューションを Visual Studio for Mac または Visual Studio で開くことで取得できます。

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

既定のソリューションでは、プロジェクトを含む for Android、iOS、Windows とファルダ  私たちが構造化されてそのソリューション小さなプラットフォームの特定起動ツールがあり、ポータブル クラス ライブラリに在住のすべてのサンプル コードとゲーム コードようにすべてのプラットフォームでコードを再利用を最大化する方法を示します。

参照してください、 [Urho と Your プラットフォーム](~/graphics-games/urhosharp/platform/index.md)独自のソリューションを作成する方法の詳細ページ。

ユーザー インターフェイス要素の共通セットを共有すべてのサンプルのためのサンプルは、抽象化しています、基本的なセットアップでこのファイル。

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

いくつかの基本的なキーストロークを処理するサンプルの基本クラスを提供します。 これとタッチ イベント、設定、カメラ、基本的なユーザー インターフェイスの要素、およびこれにより、各サンプルが紹介されている特定の機能に重点を提供します。

次の例は、エンジンは何を実行できるかを示しています。

- [ゲームの samply](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) ShootySkies の単純なクローンします。

中に、その他のサンプルでは、各サンプルの個々 のプロパティを表示します。

## <a name="basic-structure"></a>基本構造

ゲームにサブクラス化する必要があります、 [`Application`](https://developer.xamarin.com/api/type/Urho.Application/)
クラスは、これは、ゲームがセットアップ (上、 [ `Setup` ](https://developer.xamarin.com/api/member/Urho.Application.Setup/)メソッド) ゲームを開始して (で、 [ `Start` ](https://developer.xamarin.com/api/member/Urho.Application.Start)メソッド)。  メイン ユーザー インターフェイスを構築します。  セットアップの 3D シーン、一部の UI 要素とすると、単純な動作をアタッチする Api を示す小規模なサンプルについて説明するでしょう。

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

このアプリケーションを実行する場合は、すぐにわかる、ランタイムから苦情がある資産がありません。  特殊なディレクトリ名"Data"で始まる、プロジェクトの階層を作成は行う必要があると、内で、これは、プログラムで参照されている資産を配置します。  設定する必要がありますし、各資産、「出力ディレクトリにコピー」を「新しい場合はコピー」のアイテムのプロパティでは確保できるように、データが存在します。

お知らせは、ここ何が起きてについて説明します。

アプリケーションを起動するには、次のように、アプリケーション クラスの新しいインスタンスを作成後に、エンジンの初期化関数を呼び出します。

```csharp
new MySample().Run();
```

ランタイムが呼び出す、`Setup`と`Start`メソッド。  オーバーライドする場合は`Setup`エンジン パラメーター (このサンプルでは表示しない) を構成することができます。

オーバーライドする必要があります`Start`ように、ゲームが起動します。  このメソッドは、資産を読み込む、イベント ハンドラーを接続、シーンをセットアップし、たい任意の操作を開始します。  サンプルでは、両者は、少し 3D シーンの構成設定に加えて、ユーザーに表示する UI を作成します。

次のコードでは、UI フレームワークを使用して、テキスト要素を作成し、アプリケーションに追加します。

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

UI フレームワークがある、非常に単純なゲーム内のユーザー インターフェイスを提供して、その機能に新しいノードを追加することで、 [ `UI.Root` ](https://developer.xamarin.com/api/property/Urho.Gui.UI.Root/)ノード。

2 番目の部分、セットアップのサンプルのメインのシーンです。  これには、複数シーンを作成する、3 D、画面で、3 D のボックスを作成、ライト、カメラ、および、ビューポートに追加する手順にはが含まれます。  セクションで詳しく解説しています[シーン、ノード、コンポーネント、およびカメラ](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras)します。

サンプルの 3 番目の部分では、いくつかのアクションをトリガーします。  操作は、特定の効果を記述し、それらを 1 回作成するためのレシピを呼び出すことによってオンデマンドでのノードによって実行されることができます、 [ `RunActionAsync` ](https://developer.xamarin.com/api/member/Urho.Node.RunActionsAsync)メソッドを`Node`します。

最初のアクションは、跳ね返り効果が適用されたボックスを拡大または縮小し、2 つ目は、ボックスを永久に回転します。

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

最初のアクションを作成するは、上記を示しています、 [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo/)アクションでは、これは、いずれかの値に対する 2 番目のノードのスケールのプロパティをスケールすることを示すレシピだけです。  このアクションは、イージング アクションにラップ順番される、 [ `EaseBounceOut` ](https://developer.xamarin.com/api/type/Urho.Actions.EaseBounceInOut/)アクション。  イージング アクション、アクションの実行に線形の変形し、効果を適用、この場合、アウト バウンド効果を提供します。
したがって、レシピもとしてを記述できます。

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

レシピが作成されると、レシピを実行します。

```csharp
await boxNode.RunActionsAsync (recipe)
```

Await には、ことを示しますは、アクションが完了すると、この行の後の実行を再開します。  アクションが完了すると、2 番目のアニメーションをトリガーします。

[を使用して UrhoSharp](~/graphics-games/urhosharp/using.md)ドキュメントで詳しく Urho およびゲームをビルドするコードを構成する方法の概念について説明します。

## <a name="copyrights"></a>著作権

このドキュメントは、Xamarin Inc から元のコンテンツが含まれていますが、Urho3D プロジェクトのオープン ソース ドキュメントから幅広くを描画し、Cocos2D プロジェクトのスクリーン ショットが含まれています。

