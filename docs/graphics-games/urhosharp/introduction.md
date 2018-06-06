---
title: UrhoSharp の概要
description: このドキュメントでは、UrhoSharp アプリケーションとさまざまなガイドや UrhoSharp を使用する方法を示すサンプル アプリケーションへのリンクの基本構造について説明します。
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: d39faabc0851e3e89b03ad58a7f1ead3894efc15
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783556"
---
# <a name="an-introduction-to-urhosharp"></a>UrhoSharp の概要

![UrhoSharp ロゴ](introduction-images/urhosharp-icon.png)

UrhoSharp は、Xamarin と .NET 開発者向けの強力な 3D ゲーム エンジンです。  Apple の SceneKit および SpriteKit を基本と似ていて、物理的な特性を含めるか、ナビゲーション、ネットワーク、および多くよりに、まだクロス プラットフォームです。

.NET バインディングを[Urho3D](http://urho3d.github.io/)エンジンにより、開発者は、Android、iOS を対象とするクロスプラット フォームのコードを記述すると、Windows と Mac を同じコードベースし OpenGL と Direct3D の両方のシステムを表示できます。

UrhoSharp は、多数の機能、ボックス外でのゲーム エンジンを示します。

- 強力な 3D グラフィックのレンダリング
- [物理シミュレーション](https://developer.xamarin.com/api/namespace/Urho.Physics/)(行頭文字ライブラリを使用)
- [シーンの処理](https://developer.xamarin.com/api/type/Urho.Scene/)
- Await/非同期のサポート
- [わかりやすい操作 API](https://developer.xamarin.com/api/namespace/Urho.Actions/)
- [3D シーンへの 2D の統合](https://developer.xamarin.com/api/namespace/Urho.Urho2D/)
- [FreeType とフォントの描画](https://developer.xamarin.com/api/type/Urho.Gui.FontFaceFreeType/)
- [クライアントとサーバーのネットワーク機能](https://developer.xamarin.com/api/namespace/Urho.Network/)
- [資産の幅の広い範囲のインポート](https://developer.xamarin.com/api/namespace/Urho.Resources/)(の資産のオープン ライブラリ)
- [ナビゲーション メッシュと経路](https://developer.xamarin.com/api/namespace/Urho.Navigation/)(再キャスト/迂回を使用)
- [競合の検出の凸包生成](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/)(StanHull を使用)
- [オーディオ再生](https://developer.xamarin.com/api/namespace/Urho.Audio/)(で**libvorbis**)

## <a name="getting-started"></a>作業の開始

UrhoSharp として配布される便利な[NuGet パッケージ](https://www.nuget.org/)追加する (C#) または f# プロジェクトをターゲットとする Windows、Mac、Android、iOS とします。  NuGet、プログラムの実行に必要なライブラリと、エンジンによって使用される基本的な資産 (CoreData) の両方が付属します。

### <a name="urho-as-a-portable-class-library"></a>ポータブル クラス ライブラリとして Urho

Urho パッケージを使用して、プラットフォーム固有のプロジェクトまたはポータブル クラス ライブラリ プロジェクトですべてのプラットフォームですべてのコードを再利用することができます。  つまり、各プラットフォームで作業を実行する必要がありますすべて、プラットフォームの特定のエントリ ポイントを作成し、共有、ゲーム コードに制御を転送します。

### <a name="samples"></a>サンプル

サンプル ソリューションを Visual Studio for Mac または Visual Studio で開くと、Urho の機能の体験を取得できます。

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

既定のソリューションでは、プロジェクトを含む for Android、iOS、Windows およびファルダ  おがような構造ソリューション小さなプラットフォームの特定ランチャーがあり、ポータブル クラス ライブラリに住んでいるすべてのサンプル コードとゲーム コードのすべてのプラットフォームでコードの再利用を最大化する方法を説明します。

参照してください、 [Urho と、プラットフォーム](~/graphics-games/urhosharp/platform/index.md)独自のソリューションを作成する方法の詳細ページ。

ユーザー インターフェイス要素の共通セットを共有すべてのサンプルからサンプルにはこのファイル内の基本的なセットアップが抽象化します。

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

これをいくつかの基本的なキー入力を処理するサンプルの基本クラスを提供し、タッチ イベントを設定する際、カメラを基本的なユーザー インターフェイス要素、およびこれにより、各サンプルが紹介されている特定の機能に重点を置くします。

次の例は、エンジンは何を実行できるかを示しています。

- [ゲームの samply](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) ShootySkies の単純な複製。

中に、他のサンプルでは、各サンプルの個々 のプロパティを表示します。

## <a name="basic-structure"></a>基本構造

ゲームにサブクラス化する必要があります、 [ `Application` ](https://developer.xamarin.com/api/type/Urho.Application/)クラス、これは、ゲームをセットアップする (上、 [ `Setup` ](https://developer.xamarin.com/api/member/Urho.Application.Setup/)メソッド)、ゲームを開始し、(で、 [ `Start` ](https://developer.xamarin.com/api/member/Urho.Application.Start)メソッド)。  主要なユーザー インターフェイスを構築します。  セットアップの 3D シーン、一部の UI 要素とすると、単純な動作をアタッチする Api を示す小さなサンプルを順を追ってしようとしています。

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

このアプリケーションを実行する場合はすぐにわかるのランタイムによって示されていること、資産がありません。  行う必要がある特殊なディレクトリ名を"Data"を開始、プロジェクトの階層を作成は、これには、内部、プログラムで参照されている資産を配置します。  設定する必要がありますし、各資産、「出力ディレクトリにコピー」に「新しい場合はコピー」の項目のプロパティでは確保できるように、データが存在します。

お知らせ何が起こってここで説明します。

アプリケーションを起動するには、次のように、アプリケーション クラスの新しいインスタンスを作成することでその後に、エンジン初期化関数を呼び出します。

```csharp
new MySample().Run();
```

ランタイムが呼び出す、`Setup`と`Start`するためのメソッドです。  オーバーライドする場合は`Setup`エンジン パラメーター (このサンプルでは表示されません) を構成することができます。

オーバーライドする必要があります`Start`ように、ゲームが起動します。  このメソッドは、あなたの資産を読み込むのイベント ハンドラー接続、シーンをセットアップし、たい任意の操作を開始します。  この例では両方は、少しユーザーだけでなく 3D シーンの設定を表示する UI を作成します。

次のコードは、テキスト要素を作成し、アプリケーションに追加する UI フレームワークを使用します。

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

非常に単純なゲーム内のユーザー インターフェイスを提供する UI フレームワークがあるし、する新しいノードを追加することによって動作、 [ `UI.Root` ](https://developer.xamarin.com/api/property/Urho.Gui.UI.Root/)ノード。

2 番目の部分をセットアップのサンプルのメインのシーンです。  これには、いくつかの 3D シーン、画面で、3 D ボックスの作成、ライト、カメラおよびビューポートの追加を作成する手順が含まれます。  これらのセクションで詳しくは、[シーン、ノード、コンポーネントおよびカメラ](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras)です。

サンプルの 3 番目の部分では、いくつかのアクションをトリガーします。  アクションは、呼び出すことによって、レシピを特定の効果を説明し、それらを 1 回作成を要求時にノードによって実行されることができます、 [ `RunActionAsync` ](https://developer.xamarin.com/api/member/Urho.Node.RunActionsAsync)メソッドを`Node`です。

最初のアクション跳ね返り効果のあるボックスのスケールを設定し、2 つ目は、ボックスを永久に回転します。

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

上記の方法を示しますを作成する最初のアクション、 [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo/)アクション、これは、ノードのスケールのプロパティのいずれかの値に対する 2 番目のスケールを設定することを示す、レシピだけを実行します。  このアクションには、ラップされたイージング アクションでは、 [ `EaseBounceOut` ](https://developer.xamarin.com/api/type/Urho.Actions.EaseBounceInOut/)アクション。  イージングのアクションが順番に実行するアクションが歪められるし、特殊効果を適用、ここでは、バウンス アウト効果を提供します。
レシピには、として記述できます。

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

レシピが作成されたら、レシピを実行します。

```csharp
await boxNode.RunActionsAsync (recipe)
```

Await には、ことを示します、アクションが完了すると、この行の後の実行を再開します。  操作が完了すると 2 番目のアニメーションがトリガーされます。

[を使用して UrhoSharp](~/graphics-games/urhosharp/using.md)ドキュメントがさらに詳しく Urho およびゲームを作成するコードを構成する方法の概念を説明します。

## <a name="copyrights"></a>著作権

このドキュメントは、Xamarin Inc から元のコンテンツが含まれていますが、オープン ソース プロジェクトのドキュメント、Urho3D から広範なを描画し、Cocos2D プロジェクトからスクリーン ショットが含まれています。

### <a name="related-links"></a>関連リンク

- [惑星地球ブック](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [座標のブックの表示](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
