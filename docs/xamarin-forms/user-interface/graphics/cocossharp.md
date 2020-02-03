---
title: Xamarin.Forms で CocosSharp を使用します。
description: CocosSharp は、正確な図形、イメージ、およびテキストのレンダリングを高度な視覚化用のアプリケーションに追加するために使用できます。
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2016
ms.openlocfilehash: 8ba9e4b119384db401fc631f58c37a28cd2b8004
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724425"
---
# <a name="using-cocossharp-in-xamarinforms"></a>Xamarin.Forms で CocosSharp を使用します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)

_CocosSharp を使用して、高度な視覚化のためにアプリケーションに正確な形状、画像、テキストレンダリングを追加することができます。_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**進化 2016: Xamarin. Forms でのココス #**

## <a name="overview"></a>概要

CocosSharp では、グラフィックスを表示する、タッチ入力を読み取り、オーディオ、および管理のコンテンツを再生するための柔軟で強力なテクノロジです。 このガイドでは、Xamarin.Forms アプリケーションに CocosSharp を追加する方法について説明します。 次について説明します。

- [CocosSharp とは](#what)
- [CocosSharp NuGet パッケージの追加](#nuget)
- [チュートリアル: Xamarin. Forms アプリへの CocosSharp の追加](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>CocosSharp とは何ですか。

[CocosSharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)は、Xamarin プラットフォームで利用できるオープンソースのゲームエンジンです。
CocosSharp では、次の機能が含まれたランタイムの効率的なライブラリを示します。

- `CCSprite` クラスを使用したイメージのレンダリング
- `CCDrawNode` クラスを使用したシェイプレンダリング
- `CCNode.Schedule` クラスを使用するすべてのフレームロジック
- `CCTextureCache` を使用したコンテンツ管理 (.png ファイルなどのリソースの読み込みとアンロード)
- `CCAction` クラスを使用したアニメーション

CocosSharp の主な目的は、クロスプラット フォーム 2D ゲーム; の作成を簡略化ただし、Xamarin フォーム アプリケーションに優れた追加ことができます。 通常、ゲームには、効率的なレンダリングやビジュアルを正確に制御が必要があるために、ゲーム以外のアプリケーションに強力な視覚化と効果を追加する CocosSharp を使用できます。

Xamarin.Forms は、ネイティブなプラットフォーム固有の UI システムに基づいて構築されています。 たとえば、 [`Button`s](xref:Xamarin.Forms.Button)は IOS と Android では異なる方法で表示され、オペレーティングシステムのバージョンによっては異なる場合があります。 これに対し、すべてのビジュアル オブジェクトがすべてのプラットフォームで同じように見えるように、CocosSharp は任意のプラットフォームに固有のビジュアル オブジェクトを使用しません。 もちろん、デバイス、異なる解像度や縦横比と CocosSharp でそのビジュアルを表示する方法に影響する可能性です。 これらの詳細については、このガイドの後半で説明します。

詳細については、 [「CocosSharp」セクション](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)を参照してください。

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>CocosSharp NuGet パッケージの追加

CocosSharp を使用する前に、開発者は、Xamarin.Forms プロジェクトにいくつかの項目を追加する必要があります。
このガイドでは、Xamarin.Forms プロジェクトで、iOS、Android、および .NET Standard ライブラリ プロジェクト。
すべてのコードは、.NET Standard ライブラリ プロジェクトで出力されます。ただし、ライブラリは、iOS および Android のプロジェクトに追加する必要があります。

CocosSharp NuGet パッケージには、CocosSharp オブジェクトを作成するために必要なすべてのオブジェクトが含まれています。
CocosSharp NuGet パッケージには `CocosSharpView` クラスが含まれています。これは、CocosSharp でのホストに使用されます。
**CocosSharp** NuGet を追加すると、 **CocosSharp**も自動的に追加されます。
これを行うには、.NET Standard ライブラリ プロジェクトの **パッケージ** フォルダーを右クリックし、**パッケージの追加** を選択します。検索用語として「 **CocosSharp**」を入力し、 **CocosSharp に対し**て  を選択して、**パッケージの追加** をクリックします。

![](cocossharp-images/image1.png "Add Packages Dialog")

**CocosSharp**と**CocosSharp**の両方の NuGet パッケージがプロジェクトに追加されます。

![](cocossharp-images/image2.png "Packages Folder")

プラットフォーム固有プロジェクト (iOS と Android) などの上記の手順を繰り返します。

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>チュートリアル: CocosSharp を Xamarin.Forms アプリに追加します。

Xamarin.Forms アプリに単純な CocosSharp ビューを追加するこれらの手順に従います。

1. [Xamarin フォームページの作成](#1)
1. [CocosSharpView の追加](#2)
1. [GameScene の作成](#3)
1. [円を追加する](#4)
1. [CocosSharp との対話](#5)

CocosSharp ビューを Xamarin. Forms アプリに正常に追加したら、 [CocosSharp のドキュメント](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)を参照して、CocosSharp でコンテンツを作成する方法を確認してください。

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Xamarin フォームページを作成する

CocosSharp は、すべての Xamarin.Forms コンテナーでホストできます。 このページのこのサンプルでは、`HomePage`という名前のページを使用します。 `HomePage` は、`Grid` によって半分に分割され、同じページに CocosSharp とを同時に表示する方法を示します。

まず、`Grid` と2つの `Button` インスタンスが含まれるようにページを設定します。

```csharp
public class HomePage : ContentPage
{
public HomePage ()
    {
        // This is the top-level grid, which will split our page in half
        var grid = new Grid ();
        this.Content = grid;
        grid.RowDefinitions = new RowDefinitionCollection {
            // Each half will be the same size:
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
        };
        CreateTopHalf (grid);
        CreateBottomHalf (grid);
    }
    void CreateTopHalf(Grid grid)
    {
        // We'll be adding our CocosSharpView here:
    }
    void CreateBottomHalf(Grid grid)
    {
        // We'll use a StackLayout to organize our buttons
        var stackLayout = new StackLayout();
        // The first button will move the circle to the left when it is clicked:
        var moveLeftButton = new Button {
            Text = "Move Circle Left"
        };
        stackLayout.Children.Add (moveLeftButton);

        // The second button will move the circle to the right when clicked:
        var moveCircleRight = new Button {
            Text = "Move Circle Right"
        };
        stackLayout.Children.Add (moveCircleRight);
        // The stack layout will be in the bottom half (row 1):

        grid.Children.Add (stackLayout, 0, 1);
    }
}
```

IOS では、次の図に示すように `HomePage` が表示されます。

![](cocossharp-images/image3.png "HomePage Screenshot")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. CocosSharpView を追加する

`CocosSharpView` クラスは、CocosSharp を Xamarin. Forms アプリに埋め込むために使用されます。 `CocosSharpView` は、Xamarin. [forms. View](xref:Xamarin.Forms.View)クラスから継承されるため、レイアウト用の使い慣れたインターフェイスを提供し、 [xamarin](xref:Xamarin.Forms.Grid)などのレイアウトコンテナー内で使用できます。 `CreateTopHalf` メソッドを完了して、新しい `CocosSharpView` をプロジェクトに追加します。

```csharp
void CreateTopHalf(Grid grid)
{
    // This hosts our game view.
    var gameView = new CocosSharpView () {
        // Notice it has the same properties as other XamarinForms Views
        HorizontalOptions = LayoutOptions.FillAndExpand,
        VerticalOptions = LayoutOptions.FillAndExpand,
        // This gets called after CocosSharp starts up:
        ViewCreated = HandleViewCreated
    };
    // We'll add it to the top half (row 0)
    grid.Children.Add (gameView, 0, 0);
}
```

CocosSharp の初期化は即時ではないため、`CocosSharpView` が作成を完了したときにイベントを登録します。 `HandleViewCreated` メソッドで次の操作を行います。

```csharp
void HandleViewCreated (object sender, EventArgs e)
{
    var gameView = sender as CCGameView;
    if (gameView != null)
    {
        // This sets the game "world" resolution to 100x100:
        gameView.DesignResolution = new CCSizeI (100, 100);
        // GameScene is the root of the CocosSharp rendering hierarchy:
        gameScene = new GameScene (gameView);
        // Starts CocosSharp:
        gameView.RunWithScene (gameScene);
    }
}
```

`HandleViewCreated` メソッドには、注目すべき重要な2つの詳細情報があります。 1つ目は `GameScene` クラスです。このクラスは次のセクションで作成します。 `GameScene` が作成され、`gameScene` インスタンス参照が解決されるまで、アプリはコンパイルされないことに注意してください。

2番目の重要な詳細は `DesignResolution` プロパティです。これは、CocosSharp オブジェクトのゲームの可視領域を定義します。 `GameScene`を作成した後、`DesignResolution` プロパティが検索されます。

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. GameScene を作成する

`GameScene` クラスは、CocosSharp の `CCScene`から継承されます。 `GameScene` は、純粋に CocosSharp を処理する最初のポイントです。 `GameScene` に含まれているコードは、Xamarin. Forms プロジェクト内に格納されているかどうかにかかわらず、任意の CocosSharp アプリで機能します。

`CCScene` クラスは、すべての CocosSharp レンダリングのビジュアルルートです。 参照可能な CocosSharp オブジェクトは、`CCScene`内に含まれている必要があります。 具体的には、ビジュアルオブジェクトを `CCLayer` インスタンスに追加し、それらの `CCLayer` インスタンスを `CCScene`に追加する必要があります。

次のグラフは、一般的な CocosSharp 階層を視覚化できます。

![](cocossharp-images/image4.png "Typical CocosSharp Hierarchy")

一度にアクティブにできる `CCScene` は1つだけです。 ほとんどのゲームでは、複数の `CCLayer` インスタンスを使用してコンテンツを並べ替えますが、アプリケーションでは1つだけを使用します。 同様に、ほとんどのゲームを使用して、複数のビジュアル オブジェクトですが、アプリのいずれかを私たちだけ必要があります。 CocosSharp のビジュアル階層に関する詳細な説明については、 [BouncingGame のチュートリアル](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/bouncing-game.md)を参照してください。

`GameScene` クラスは、最初はほとんど空になります。 `HomePage`の参照を満たすように作成します。 `GameScene`という名前の新しいクラスを .NET Standard ライブラリプロジェクトに追加します。 次のように `CCScene` クラスから継承する必要があります。

```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

`GameScene` が定義されたので、`HomePage` に戻り、フィールドを追加できます。

```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

プロジェクトをコンパイルしてを実行している CocosSharp を確認を実行しましたできますようになりました。 `GameScene,` に何も追加していません。ページの上半分が黒になっています (CocosSharp シーンの既定の色)。

![](cocossharp-images/image5.png "Blank GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. 円を追加する

現在、アプリには CocosSharp エンジンの実行中のインスタンスがあり、空の `CCScene`が表示されています。 次に、ビジュアル オブジェクトを追加します。 円。 `CCDrawNode` クラスを使用すると、「 [CCDrawNode 使用したジオメトリの描画」ガイド](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md)で説明されているように、さまざまな幾何学図形を描画できます。

次のコードに示すように、`GameScene` クラスに円を追加し、コンストラクターでインスタンス化します。

```csharp
public class GameScene : CCScene
{
    CCDrawNode circle;
    public GameScene (CCGameView gameView) : base(gameView)
    {
        var layer = new CCLayer ();
        this.AddLayer (layer);
        circle = new CCDrawNode ();
        layer.AddChild (circle);
        circle.DrawCircle (
            // The center to use when drawing the circle,
            // relative to the CCDrawNode:
            new CCPoint (0, 0),
            radius:15,
            color:CCColor4B.White);
        circle.PositionX = 20;
        circle.PositionY = 50;
    }
}
```

今すぐアプリを実行すると、CocosSharp 表示領域の左側にある円が表示されます。

![](cocossharp-images/image6.png "Circle in GameScene")

#### <a name="understanding-designresolution"></a>Understanding DesignResolution

Visual CocosSharp オブジェクトが表示されたので、`DesignResolution` プロパティを調べることができます。

`DesignResolution` は、オブジェクトを配置およびサイズ設定するための CocosSharp 領域の幅と高さを表します。 領域の実際の解像度は*ピクセル*単位で計測され、`DesignResolution` はワールド*単位*で測定されます。 次の図は、iPhone 5 x 1136 640 ピクセルの解像度が画面に表示されるビューのさまざまな部分の解像度を示します。

![](cocossharp-images/image7.png "iPhone 5s Design Resolution")

上記の図は、黒色の文字で、画面の外側のピクセル寸法を表示します。 単位は、白いテキストでダイアグラムの内側に表示されます。 上に表示される重要な詳細情報を次に示します。

- CocosSharp ディスプレイの原点は左下にあります。 X の値が増加、右に移動し、上に移動する Y 値が増加します。 確認の Y 値が逆になっていることと比較する他の 2D レイアウト エンジンと、(0, 0) は、キャンバスの左上。
- CocosSharp の既定の動作は、そのビューの縦横比を維持するためには。 グリッドの最初の行が高さよりも広いため、CocosSharp がピリオドで区切られた白い四角形が示すように、そのセルの幅全体を入力していません。 この動作は、「 [CocosSharp での複数の解像度の処理」ガイド](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/resolutions.md)で説明されているように変更できます。
- この例で CocosSharp 幅と高さのサイズに関係なく、100 単位の表示領域、またはそのデバイスの縦横比が維持されます。 これは、こと、CocosSharp の右端にバインドされている X = 100 はレイアウトを表示、管理することすべてのデバイスで一貫性のあるコードは想定できることを意味します。

#### <a name="ccdrawnode-details"></a>CCDrawNode の詳細

この単純なアプリでは、`CCDrawNode` クラスを使用して円を描画します。 このクラスは、ベクトルに基づく geometry rendering – Xamarin.Forms から不足している機能を提供するためビジネス アプリの非常に役立ちます。 円に加えて、`CCDrawNode` クラスを使用して、四角形、スプライン、直線、およびカスタム多角形を描画できます。 画像ファイル (.png など) を使用する必要がないため、`CCDrawNode` も簡単に使用できます。 CCDrawNode の詳細については、「 [ccdrawnode を使用したジオメトリの描画」ガイド](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md)を参照してください。

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. CocosSharp との対話

CocosSharp ビジュアル要素 (`CCDrawNode`など) は、`CCNode` クラスから継承されます。 `CCNode` には、オブジェクトを親との相対位置に配置するために使用できる2つのプロパティが用意されています。 `PositionX` と `PositionY`です。 コードに現在使用してこれら 2 つのプロパティ、円の中心にこのコード スニペットで示すよう。

```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

CocosSharp オブジェクトが Xamarin.Forms ビューのほとんどは、その親のレイアウト コントロールの動作に従って自動的に配置されるのではなく、明示的な位置の値が配置されていることに注意してください。 重要です。

10 ユニット (しないピクセル、CocosSharp ワールド単位で、円を描画するため)、円を左または右に移動する 2 つのボタンのいずれかをクリックするユーザーを許可するコードを追加します。 まず、`GameScene` クラスに2つのパブリックメソッドを作成します。

```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

次に、`HomePage` の2つのボタンにハンドラーを追加してクリックに応答します。 完了すると、`CreateBottomHalf` メソッドには次のコードが含まれます。

```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

CocosSharp の円は、今すぐクリックに応答に移動します。 CocosSharp キャンバスの境界を左または右円を十分に移動することによっても明確に確認できます。

![](cocossharp-images/image8.png "GameScene with Moving Circle")

## <a name="summary"></a>まとめ

このガイドは、CocosSharp を既存の Xamarin.Forms に追加する方法を示しています。 プロジェクトを Xamarin.Forms で CocosSharp との対話を作成する方法と CocosSharp でレイアウトを作成するときにさまざまな考慮事項について説明します。

CocosSharp のゲーム エンジンは、このガイドほんの CocosSharp で実行できるように、多くの機能と、深さを提供します。 CocosSharp について詳しく知りたい開発者は、 [CocosSharp アーカイブ](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/)で多くの記事を見つけることができます。

## <a name="related-links"></a>関連リンク

- [CocosSharpForms (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)
