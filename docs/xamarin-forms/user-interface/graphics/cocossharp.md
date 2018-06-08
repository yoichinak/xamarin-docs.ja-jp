---
title: Xamarin.Forms で CocosSharp の使用
description: CocosSharp は、正確な形、画像、およびテキストのレンダリングを高度な視覚エフェクト用のアプリケーションに追加するために使用できます。
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/03/2016
ms.openlocfilehash: 4770076a0bf31ebd3cdf8f1b83da076a4dcc83ef
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847892"
---
# <a name="using-cocossharp-in-xamarinforms"></a>Xamarin.Forms で CocosSharp の使用

_CocosSharp は、正確な形、画像、およびテキストのレンダリングを高度な視覚エフェクト用のアプリケーションに追加するために使用できます。_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**2016 を発展: Cocos # xamarin.forms**

## <a name="overview"></a>概要

CocosSharp は、グラフィックスの表示、タッチ入力を読み取り、オーディオ、および管理のコンテンツの再生柔軟で強力なテクノロジです。 このガイドでは、Xamarin.Forms アプリケーションに CocosSharp を追加する方法について説明します。 以下について説明します。

* [CocosSharp とは何ですか。](#what)
* [CocosSharp Nuget パッケージを追加します。](#nuget)
* [チュートリアル: CocosSharp を Xamarin.Forms アプリに追加します。](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>CocosSharp とは何ですか。

[CocosSharp](~/graphics-games/cocossharp/index.md) Xamarin プラットフォームで利用可能なオープン ソースのゲーム エンジンです。
CocosSharp では、次の機能を含むランタイム効率的なライブラリです。

* イメージのレンダリングを使用して、 [CCSprite クラス](https://developer.xamarin.com/api/type/CocosSharp.CCSprite/)
* 図形のレンダリングを使用して、 [CCDrawNode クラス](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
* フレームごとのロジックを使用して、 [CCNode.Schedule メソッド](https://developer.xamarin.com/api/member/CocosSharp.CCNode.Schedule/p/System.Action%7BSystem.Single%7D/)
* コンテンツの管理 (の読み込みやアンロード .png ファイルなどのリソース) を使用して、 [CCTextureCache クラス](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
* アニメーションを使用して、 [CCAction クラス](https://developer.xamarin.com/api/type/CocosSharp.CCAction/)

CocosSharp の主な目的は、クロスプラット フォームの 2D ゲーム; の作成を簡略化するにはただし、Xamarin フォーム アプリケーションに優れた追加ことができます。 通常、ゲームには、効率的なレンダリングやビジュアルを正確に制御が必要があるために、ゲーム以外のアプリケーションに強力なビジュアル化および特殊効果を追加する CocosSharp を使用できます。

Xamarin.Forms は、プラットフォーム固有のネイティブ UI のシステムによって作成されています。 たとえば、 [ `Button`s](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) iOS および Android での表示が異なると、オペレーティング システムのバージョンによっても異なる場合があります。 これに対し、すべてのビジュアル オブジェクトが同じすべてのプラットフォームで表示されるように、CocosSharp はそのプラットフォームに固有のビジュアル オブジェクトを使用しません。 もちろん、解像度と縦横比は装置間で異なるし、CocosSharp がそのビジュアルを表示する方法に影響する可能性です。 これらの詳細は、このガイドの後半で説明されます。

詳細な情報は含まれて、 [CocosSharp セクション](~/graphics-games/cocossharp/index.md)です。

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>CocosSharp Nuget パッケージを追加します。

CocosSharp を使用する前に開発者は、Xamarin.Forms プロジェクトにいくつかの項目を追加する必要があります。
この前提として、iOS、Android、および .NET 標準と Xamarin.Forms プロジェクト ライブラリ プロジェクト。
すべてのコードは標準の .NET ライブラリ プロジェクトで書かれますただし、ライブラリは、iOS および Android のプロジェクトに追加する必要があります。

CocosSharp Nuget パッケージには、すべての CocosSharp オブジェクトを作成するために必要なオブジェクトが含まれています。
CocosSharp.Forms nuget パッケージに含まれる、 `CocosSharpView` xamarin.forms CocosSharp のホストに使用されるクラスです。
追加、 **CocosSharp.Forms** NuGet と**CocosSharp**も自動的に追加されます。
これを行うを右クリックし、<span class="UIItem">パッケージ</span>フォルダーをクリックし、標準的な .NET のライブラリ プロジェクトで<span class="UIItem">パッケージを追加しています.</span>.検索語句を入力<span class="UIItem">CocosSharp.Forms</span>を選択<span class="UIItem">Xamarin.Forms の CocosSharp</span>をクリックし、<span class="UIItem">パッケージの追加</span>です。

![](cocossharp-images/image1.png "追加のパッケージ ダイアログ ボックス")

両方**CocosSharp**と**CocosSharp.Forms** NuGet パッケージをプロジェクトに追加されます。

![](cocossharp-images/image2.png "パッケージ フォルダー")

(IOS および Android) などのプラットフォーム固有のプロジェクトを上記の手順を繰り返します。

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>チュートリアル: CocosSharp を Xamarin.Forms アプリに追加します。

Xamarin.Forms アプリに単純な CocosSharp ビューを追加する手順に従います。

1. [フォーム ページの Xamarin の作成](#1)
1. [CocosSharpView を追加します。](#2)
1. [GameScene を作成します。](#3)
1. [円の追加](#4)
1. [CocosSharp と対話します。](#5)

CocosSharp ビューは、Xamarin.Forms アプリに正常に追加したらを参照してください。、 [CocosSharp ドキュメント](~/graphics-games/cocossharp/index.md)CocosSharp によるコンテンツの作成に関する詳細についてはします。

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1.フォーム ページの Xamarin の作成

CocosSharp は、任意の Xamarin.Forms コンテナーでホストできます。 このページには、このサンプルと呼ばれるページを使用して`HomePage`です。 `HomePage` 半分に分割されると、 `Grid` Xamarin.Forms と CocosSharp できますの表示方法に同時に同じページに表示します。

最初に、ページの設定が含まれているように、`Grid`と 2 つ`Button`インスタンス。


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

Ios の場合、`HomePage`ように、次の図に示すように表示されます。

![](cocossharp-images/image3.png "ホーム ページのスクリーン ショット")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2.CocosSharpView を追加します。

`CocosSharpView` CocosSharp Xamarin.Forms アプリに埋め込むクラスを使用します。 `CocosSharpView`から継承、 [Xamarin.Forms.View](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)クラス、レイアウトの場合に使い慣れたインターフェイスを提供およびなどのレイアウト コンテナー内で使用できます[Xamarin.Forms.Grid](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)です。 新しい`CocosSharpView`を完了してプロジェクトに、`CreateTopHalf`メソッド。


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

CocosSharp 初期化がすぐに、ためときのイベントを登録、`CocosSharpView`の作成が完了します。 この操作で行う、`HandleViewCreated`メソッド。


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

`HandleViewCreated`メソッドが 2 つの重要な詳細に見ることです。 1 つは、`GameScene`クラスは、次のセクションで作成されます。 までアプリはコンパイルされないことに注意する必要が、`GameScene`が作成され、`gameScene`インスタンス参照は解決します。

2 つ目の重要な詳細は、 `DesignResolution` CocosSharp オブジェクト用のゲームの可視領域を定義するプロパティです。 `DesignResolution`プロパティが作成した後で検索される`GameScene`です。

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3.GameScene を作成します。

`GameScene` CocosSharp からクラスを継承`CCScene`です。 `GameScene` 最初 CocosSharp と純粋な処理です。 含まれるコード`GameScene`Xamarin.Forms プロジェクト内で収容かどうかどうか、CocosSharp アプリ内の関数をされます。

`CCScene`クラスはすべて CocosSharp レンダリングのビジュアルのルートです。 表示されている CocosSharp オブジェクトに含まれる必要があります、`CCScene`です。 ビジュアル オブジェクトを追加する必要がありますより具体的には、`CCLayer`インスタンス、およびそれら`CCLayer`にインスタンスを追加する必要があります、`CCScene`です。

次のグラフは、一般的な CocosSharp 階層の視覚化に役立つことができます。

![](cocossharp-images/image4.png "一般的な CocosSharp 階層")

1 つだけ`CCScene`一度にアクティブにできます。 ほとんどのゲーム複数回使用`CCLayer`並べ替えコンテンツは、アプリケーションのインスタンスを使用して 1 つだけです。 同様に、ほとんどのゲームは、複数のビジュアル オブジェクトを使用して、のみした、アプリケーション内で 1 つ。 詳細な階層構造は含まれて CocosSharp について、 [BouncingGame チュートリアル](~/graphics-games/cocossharp/bouncing-game.md)です。

最初に、`GameScene`クラスはほとんど空になります: を作成しましょう内の参照を満たすために、`HomePage`です。 名前付き .NET 標準ライブラリ プロジェクトに新しいクラスを追加`GameScene`です。 継承する必要がありますが、`CCScene`クラスの次のようにします。


```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

これで`GameScene`は私たちに戻ることが定義されている、`HomePage`し、フィールドを追加。


```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

プロジェクトをコンパイルようになりましたを実行すると、実行されている CocosSharp を参照してください。 何も追加していません、`GameScene,`のため、ページの上半分は黒 – CocosSharp シーンの既定の色。

![](cocossharp-images/image5.png "空白 GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4.円の追加

アプリは現在、空の表示、CocosSharp エンジンの実行中のインスタンスが`CCScene`です。 次に、ビジュアル オブジェクトを追加します。 円です。 `CCDrawNode`で説明したように、さまざまな幾何学図形を描画するクラスを使用できます、 [CCDrawNode ガイドを使用して図面 Geometry](~/graphics-games/cocossharp/ccdrawnode.md)です。

円を追加、`GameScene`クラスし、それをインスタンス化コンス トラクターで、次のコードに示すようにします。


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

今すぐアプリを実行している CocosSharp 表示領域の左側にある円を示しています。

![](cocossharp-images/image6.png "GameScene の円")


#### <a name="understanding-designresolution"></a>Understanding DesignResolution

これで visual CocosSharp オブジェクトが表示されたら、詳しく調査させていただきます、`DesignResolution`プロパティです。

`DesignResolution`を配置して、オブジェクトのサイズ変更の CocosSharp 領域の高さと幅を表します。 領域の実際の解像度の単位は*ピクセル*中に、`DesignResolution`は世界で測定されます*ユニット*です。 次の図は、640 x 1136 ピクセルの画面解像度で iPhone 5 に表示されるビューのさまざまな部分の解像度を示しています。

![](cocossharp-images/image7.png "iPhone 5 s デザインの解決")

上記の図は、黒色の文字で、画面の外側のピクセル数を表示します。 単位は、白いテキストでダイアグラムの内側に表示されます。 上に表示されるいくつかの重要な詳細を次に示します。

* CocosSharp ディスプレイの原点は左下にあります。 X の値を増加右に移動し、上に移動する Y 値を大きくします。 注意してください、Y 値が逆になっていることを他の 2D レイアウト エンジンと比較して、(0, 0) は、キャンバスの左上です。
* CocosSharp の既定の動作は、そのビューの縦横比を維持するためにです。 グリッドの最初の行が高さより広いため、CocosSharp が点線の白い四角形に示すようにそのセルの幅全体を入力してしません。 」の説明に従って、この動作を変更できる、[処理複数の解像度、CocosSharp ガイド](~/graphics-games/cocossharp/resolutions.md)です。
* この例では CocosSharp 幅と高さのサイズに関係なく 100 単位の表示領域、またはそのデバイスの縦横比が維持されます。 これは、ことコードと見なすことできること、CocosSharp の右端にバインドされている X = 100 表しますレイアウトを表示、保持しておくすべてのデバイスで一貫したことを意味します。


#### <a name="ccdrawnode-details"></a>CCDrawNode 詳細

この簡単なアプリは、`CCDrawNode`円を描画するクラス。 このクラスは、ジオメトリのベクトルに基づくレンダリング – Xamarin.Forms されない機能を提供するため for business アプリ非常に役立ちます。 円、に加えて、`CCDrawNode`クラスを使用して、四角形、スプライン、線、およびカスタムの多角形を描画することです。 `CCDrawNode` イメージ ファイル (.png) などの使用が必要ないためには使いやすいもです。 CCDrawNode の詳細については含まれて、 [CCDrawNode ガイドを使用して図面 Geometry](~/graphics-games/cocossharp/ccdrawnode.md)です。

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5.CocosSharp と対話します。

CocosSharp 視覚要素 (など`CCDrawNode`) から継承、`CCNode`クラスです。 `CCNode` その親オブジェクトを配置するために使用する 2 つのプロパティを提供します。`PositionX`と`PositionY`です。 コード現在使用してこれら 2 つのプロパティを円の中心を配置するこのコード スニペットに示すように。


```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

ことが重要 Xamarin.Forms ビューのほとんどは、その親のレイアウト コントロールの動作に従って自動的に配置されるのではなく、明示的な位置の値により CocosSharp オブジェクトが配置されることに注意してください。

ユーザーが 10 ユニット (いないピクセル、CocosSharp ワールド単位空間に、円を描画するため)、円を左側または右側に移動する 2 つのボタンのいずれかをクリックしてできるようにするコードを追加します。 最初の 2 つのパブリック メソッドを作成、`GameScene`クラス。


```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

次はハンドラーを追加で 2 つのボタン`HomePage`クリックに応答します。 完了したら、当社`CreateBottomHalf`メソッドには、次のコードが含まれています。


```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

CocosSharp 円移動するようにクリックに応答します。 CocosSharp キャンバスの境界を左または右に十分な円を移動することによっても明確に確認できます。

![](cocossharp-images/image8.png "円の移動に GameScene")

## <a name="summary"></a>まとめ

このガイドは、既存の xamarin.forms CocosSharp を追加する方法を示しますプロジェクト、Xamarin.Forms CocosSharp、間の相互作用を作成する方法と CocosSharp のレイアウトを作成するときに、さまざまな考慮事項を説明します。

CocosSharp のゲーム エンジンには、このガイドだけのほんの一部 CocosSharp 何ができるように、多くの機能との深さが用意されています。 CocosSharp に関する詳細の読み込み中に関心のある開発者は、多数の記事で見つけることができます、 [CocosSharp セクション](~/graphics-games/cocossharp/index.md)です。



## <a name="related-links"></a>関連リンク

- [CocosSharp Api](https://developer.xamarin.com/api/root/CocosSharp/)
- [CocosSharpForms (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/CocosSharpForms/)
