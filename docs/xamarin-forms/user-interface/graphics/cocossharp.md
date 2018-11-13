---
title: Xamarin.Forms で CocosSharp を使用します。
description: CocosSharp は、正確な図形、イメージ、およびテキストのレンダリングを高度な視覚化用のアプリケーションに追加するために使用できます。
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2016
ms.openlocfilehash: 00c6b40e7611b0111d2ed2d0fabb3f60619d481a
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563551"
---
# <a name="using-cocossharp-in-xamarinforms"></a>Xamarin.Forms で CocosSharp を使用します。

_CocosSharp は、正確な図形、イメージ、およびテキストのレンダリングを高度な視覚化用のアプリケーションに追加するために使用できます。_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**Evolve 2016: Cocos # Xamarin.Forms で**

## <a name="overview"></a>概要

CocosSharp では、グラフィックスを表示する、タッチ入力を読み取り、オーディオ、および管理のコンテンツを再生するための柔軟で強力なテクノロジです。 このガイドでは、Xamarin.Forms アプリケーションに CocosSharp を追加する方法について説明します。 次について説明します。

* [CocosSharp とは何ですか。](#what)
* [CocosSharp の Nuget パッケージを追加します。](#nuget)
* [チュートリアル: CocosSharp を Xamarin.Forms アプリに追加します。](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>CocosSharp とは何ですか。

[CocosSharp](~/graphics-games/cocossharp/index.md)は、Xamarin プラットフォームで利用可能なオープン ソースのゲーム エンジンです。
CocosSharp では、次の機能が含まれたランタイムの効率的なライブラリを示します。

* イメージのレンダリングを使用して、 [CCSprite クラス](https://developer.xamarin.com/api/type/CocosSharp.CCSprite/)
* 図形のレンダリングを使用して、 [CCDrawNode クラス](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
* フレームごとのロジックを使用して、 [CCNode.Schedule メソッド](https://developer.xamarin.com/api/member/CocosSharp.CCNode.Schedule/p/System.Action%7BSystem.Single%7D/)
* コンテンツの管理 (読み込みとアンロードの .png ファイルなどのリソース) を使用して、 [CCTextureCache クラス](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
* アニメーションを使用して、 [CCAction クラス](https://developer.xamarin.com/api/type/CocosSharp.CCAction/)

CocosSharp の主な目的は、クロスプラット フォーム 2D ゲーム; の作成を簡略化ただし、Xamarin フォーム アプリケーションに優れた追加ことができます。 通常、ゲームには、効率的なレンダリングやビジュアルを正確に制御が必要があるために、ゲーム以外のアプリケーションに強力な視覚化と効果を追加する CocosSharp を使用できます。

Xamarin.Forms は、ネイティブなプラットフォーム固有の UI システムに基づいて構築されています。 たとえば、 [ `Button`s](xref:Xamarin.Forms.Button) iOS と Android での表示が異なると、オペレーティング システムのバージョンによっても異なる場合があります。 これに対し、すべてのビジュアル オブジェクトがすべてのプラットフォームで同じように見えるように、CocosSharp は任意のプラットフォームに固有のビジュアル オブジェクトを使用しません。 もちろん、デバイス、異なる解像度や縦横比と CocosSharp でそのビジュアルを表示する方法に影響する可能性です。 これらの詳細については、このガイドの後半で説明します。

詳細な情報が記載されて、 [CocosSharp セクション](~/graphics-games/cocossharp/index.md)します。

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>CocosSharp の Nuget パッケージを追加します。

CocosSharp を使用する前に、開発者は、Xamarin.Forms プロジェクトにいくつかの項目を追加する必要があります。
このガイドでは、Xamarin.Forms プロジェクトで、iOS、Android、および .NET Standard ライブラリ プロジェクト。
すべてのコードは、.NET Standard ライブラリ プロジェクトで出力されます。ただし、ライブラリは、iOS および Android のプロジェクトに追加する必要があります。

CocosSharp の Nuget パッケージには、すべての CocosSharp オブジェクトを作成するために必要なオブジェクトが含まれます。
CocosSharp.Forms の nuget パッケージに含まれる、`CocosSharpView`クラスは、Xamarin.Forms で CocosSharp をホストするために使用します。
追加、 **CocosSharp.Forms** NuGet と**CocosSharp**も自動的に追加されます。
これを行うを右クリックし、<span class="UIItem">パッケージ</span>フォルダーをクリックし、.NET Standard ライブラリ プロジェクトで<span class="UIItem">パッケージを追加しています.</span>.検索語句を入力します。 <span class="UIItem">CocosSharp.Forms</span>を選択します<span class="UIItem">Xamarin.Forms 用 CocosSharp</span>、順にクリックします<span class="UIItem">パッケージの追加</span>します。

![](cocossharp-images/image1.png "追加のパッケージ ダイアログ ボックス")

両方**CocosSharp**と**CocosSharp.Forms** NuGet パッケージをプロジェクトに追加されます。

![](cocossharp-images/image2.png "パッケージ フォルダー")

プラットフォーム固有プロジェクト (iOS と Android) などの上記の手順を繰り返します。

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>チュートリアル: CocosSharp を Xamarin.Forms アプリに追加します。

Xamarin.Forms アプリに単純な CocosSharp ビューを追加するこれらの手順に従います。

1. [フォーム ページ、Xamarin を作成します。](#1)
1. [CocosSharpView を追加します。](#2)
1. [GameScene を作成します。](#3)
1. [円を追加します。](#4)
1. [CocosSharp による操作](#5)

CocosSharp ビューは、Xamarin.Forms アプリに正常に追加したらを参照してください。、 [CocosSharp ドキュメント](~/graphics-games/cocossharp/index.md)を CocosSharp によるコンテンツの作成の詳細を参照してください。

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1.フォーム ページ、Xamarin を作成します。

CocosSharp は、すべての Xamarin.Forms コンテナーでホストできます。 このページには、このサンプルと呼ばれるページを使用して`HomePage`します。 `HomePage` 半分に分割されると、 `Grid` Xamarin.Forms と CocosSharp できますに表示する方法に同時に同じページを表示します。

最初に、ページの設定が含まれるように、`Grid`と 2 つ`Button`インスタンス。


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

Ios では、`HomePage`次の図のように表示されます。

![](cocossharp-images/image3.png "ホーム ページのスクリーン ショット")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2.CocosSharpView を追加します。

`CocosSharpView` CocosSharp を Xamarin.Forms アプリに埋め込むためにクラスを使用します。 `CocosSharpView`継承、 [Xamarin.Forms.View](xref:Xamarin.Forms.View)クラス、レイアウト、使い慣れたインターフェイスを提供しなどのレイアウト コンテナー内で使用できます[Xamarin.Forms.Grid](xref:Xamarin.Forms.Grid)します。 新しい追加`CocosSharpView`を実行してプロジェクトに、`CreateTopHalf`メソッド。


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

CocosSharp の初期化は即時ではないためときのイベントを登録する必要が、`CocosSharpView`その作成が完了します。 これで、`HandleViewCreated`メソッド。


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

`HandleViewCreated`メソッドにはお待ちで 2 つの重要な詳細情報。 1 つは、`GameScene`クラスは、次のセクションで作成されます。 までアプリはコンパイルされないことに注意してください、`GameScene`が作成されると、`gameScene`インスタンスの参照が解決されました。

2 番目の重要な詳細が、`DesignResolution`プロパティで、ゲームの可視領域 CocosSharp オブジェクトを定義します。 `DesignResolution`プロパティは、作成した後で検索が`GameScene`します。

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3.GameScene を作成します。

`GameScene` CocosSharp の継承クラス`CCScene`します。 `GameScene` CocosSharp による純粋処理最初のポイントです。 含まれるコード`GameScene`すべて CocosSharp のアプリで関数を Xamarin.Forms プロジェクト内で収容かどうか。

`CCScene`クラスはすべて CocosSharp レンダリングのビジュアルのルートです。 表示されている CocosSharp オブジェクトが含まれる必要があります、`CCScene`します。 具体的には、ビジュアル オブジェクトに追加する必要があります`CCLayer`インスタンス、およびその`CCLayer`にインスタンスを追加する必要があります、`CCScene`します。

次のグラフは、一般的な CocosSharp 階層を視覚化できます。

![](cocossharp-images/image4.png "CocosSharp の一般的な階層")

1 つだけ`CCScene`一度にアクティブにできます。 ほとんどのゲームを使用して、複数`CCLayer`コンテンツの並べ替えは、アプリケーションのインスタンスでは、1 つのみを使用します。 同様に、ほとんどのゲームを使用して、複数のビジュアル オブジェクトですが、アプリのいずれかを私たちだけ必要があります。 詳細については、CocosSharp でのビジュアル階層が見つかりません、 [BouncingGame チュートリアル](~/graphics-games/cocossharp/bouncing-game.md)します。

最初に、`GameScene`クラスにはほぼ何も – 内の参照を満たすために作成しますだけ`HomePage`です。 という名前の .NET Standard ライブラリ プロジェクトに新しいクラスを追加`GameScene`します。 継承する必要がありますが、`CCScene`クラスの次のようにします。


```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

これで`GameScene`を返すことができる、定義されている`HomePage`フィールドを追加して。


```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

プロジェクトをコンパイルしてを実行している CocosSharp を確認を実行しましたできますようになりました。 何も追加していない、`GameScene,`のため、ページの上半分は黒 – CocosSharp シーンの既定の色。

![](cocossharp-images/image5.png "空白 GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4.円を追加します。

アプリは現在、空の表示、CocosSharp エンジンのインスタンスが実行が`CCScene`します。 次に、ビジュアル オブジェクトを追加します。 円。 `CCDrawNode` 」の説明に従って、さまざまな幾何学図形を描画するためにクラスを使用できます、 [CCDrawNode ガイドに描画 Geometry](~/graphics-games/cocossharp/ccdrawnode.md)します。

追加する円を`GameScene`クラスし、次のコードに示すように、コンス トラクターでインスタンスします。


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

![](cocossharp-images/image6.png "GameScene の円")


#### <a name="understanding-designresolution"></a>Understanding DesignResolution

調査を行えるビジュアル CocosSharp オブジェクトを表示すると、これで、`DesignResolution`プロパティ。

`DesignResolution`を配置して、オブジェクトのサイズ変更の CocosSharp 領域の高さと幅を表します。 領域の実際の解像度の単位で*ピクセル*中に、`DesignResolution`は世界で測定されます*ユニット*します。 次の図は、iPhone 5 x 1136 640 ピクセルの解像度が画面に表示されるビューのさまざまな部分の解像度を示します。

![](cocossharp-images/image7.png "iPhone 5 s デザインの解決")

上記の図は、黒色の文字で、画面の外側のピクセル寸法を表示します。 単位は、白いテキストでダイアグラムの内側に表示されます。 上に表示される重要な詳細情報を次に示します。

* CocosSharp ディスプレイの原点は左下にあります。 X の値が増加、右に移動し、上に移動する Y 値が増加します。 確認の Y 値が逆になっていることと比較する他の 2D レイアウト エンジンと、(0, 0) は、キャンバスの左上。
* CocosSharp の既定の動作は、そのビューの縦横比を維持するためには。 グリッドの最初の行が高さよりも広いため、CocosSharp がピリオドで区切られた白い四角形が示すように、そのセルの幅全体を入力していません。 」の説明に従って、この動作を変更できる、 [CocosSharp での複数の解決を処理ガイド](~/graphics-games/cocossharp/resolutions.md)します。
* この例で CocosSharp 幅と高さのサイズに関係なく、100 単位の表示領域、またはそのデバイスの縦横比が維持されます。 これは、こと、CocosSharp の右端にバインドされている X = 100 はレイアウトを表示、管理することすべてのデバイスで一貫性のあるコードは想定できることを意味します。


#### <a name="ccdrawnode-details"></a>CCDrawNode の詳細

この単純なアプリは、`CCDrawNode`円を描画するクラス。 このクラスは、ベクトルに基づく geometry rendering – Xamarin.Forms から不足している機能を提供するためビジネス アプリの非常に役立ちます。 円、だけでなく、`CCDrawNode`クラスは、四角形、スプライン、線、およびカスタムの多角形を描画するために使用できます。 `CCDrawNode` イメージ ファイル (.png) などの使用必要としないためには使いやすいもします。 CCDrawNode のより詳細な説明が記載されて、 [CCDrawNode ガイドに描画 Geometry](~/graphics-games/cocossharp/ccdrawnode.md)します。

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5.CocosSharp による操作

CocosSharp のビジュアル要素 (など`CCDrawNode`) からの継承、`CCNode`クラス。 `CCNode` その親に対する相対的なオブジェクトを配置するために使用できる 2 つのプロパティ:`PositionX`と`PositionY`します。 コードに現在使用してこれら 2 つのプロパティ、円の中心にこのコード スニペットで示すよう。


```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

CocosSharp オブジェクトが Xamarin.Forms ビューのほとんどは、その親のレイアウト コントロールの動作に従って自動的に配置されるのではなく、明示的な位置の値が配置されていることに注意してください。 重要です。

10 ユニット (しないピクセル、CocosSharp ワールド単位で、円を描画するため)、円を左または右に移動する 2 つのボタンのいずれかをクリックするユーザーを許可するコードを追加します。 まず 2 つのパブリック メソッドを作成します、`GameScene`クラス。


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

次に、ハンドラーでは、2 つのボタンに追加します`HomePage`数回のクリックに応答します。 完了すると、`CreateBottomHalf`メソッドには、次のコードが含まれています。


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

![](cocossharp-images/image8.png "円の移動に GameScene")

## <a name="summary"></a>まとめ

このガイドは、CocosSharp を既存の Xamarin.Forms に追加する方法を示しています。 プロジェクトを Xamarin.Forms で CocosSharp との対話を作成する方法と CocosSharp でレイアウトを作成するときにさまざまな考慮事項について説明します。

CocosSharp のゲーム エンジンは、このガイドほんの CocosSharp で実行できるように、多くの機能と、深さを提供します。 CocosSharp の詳細の読み取り中に関心がある開発者に多くの記事を検索できます、 [CocosSharp セクション](~/graphics-games/cocossharp/index.md)します。



## <a name="related-links"></a>関連リンク

- [CocosSharp Api](https://developer.xamarin.com/api/root/CocosSharp/)
- [CocosSharpForms (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/CocosSharpForms/)
