---
title: での CocosSharp の使用Xamarin.Forms
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cb2303eb91fe2aa332ed35131baa7f6dd3cfeff5
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84129520"
---
# <a name="using-cocossharp-in-xamarinforms"></a>での CocosSharp の使用Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)

_CocosSharp を使用して、高度な視覚化のためにアプリケーションに正確な形状、画像、テキストレンダリングを追加することができます。_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**進化 2016: ココス # inXamarin.Forms**

## <a name="overview"></a>概要

CocosSharp は、グラフィックスの表示、タッチ入力の読み取り、オーディオの再生、およびコンテンツの管理を行うための、柔軟で強力なテクノロジです。 このガイドでは、CocosSharp をアプリケーションに追加する方法について説明 Xamarin.Forms します。 次の内容について説明します。

- [CocosSharp とは](#what)
- [CocosSharp NuGet パッケージの追加](#nuget)
- [チュートリアル: アプリへの CocosSharp の追加 Xamarin.Forms](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>CocosSharp とは

[CocosSharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)は、Xamarin プラットフォームで利用できるオープンソースのゲームエンジンです。
CocosSharp は、次の機能を備えたランタイム効率の高いライブラリです。

- クラスを使用したイメージのレンダリング `CCSprite`
- クラスを使用したシェイプレンダリング `CCDrawNode`
- クラスを使用するすべての-フレームロジック `CCNode.Schedule`
- を使用したコンテンツ管理 (.png ファイルなどのリソースの読み込みとアンロード)`CCTextureCache`
- クラスを使用したアニメーション `CCAction`

CocosSharp の主な目的は、クロスプラットフォームの2D ゲームの作成を簡略化することです。ただし、Xamarin フォームアプリケーションを追加することもできます。 ゲームは通常、視覚エフェクトを効率的にレンダリングし、視覚エフェクトを正確に制御する必要があるため、CocosSharp を使用して、ゲーム以外のアプリケーションに強力な視覚化と効果を追加することができます。

Xamarin.Formsは、プラットフォーム固有のネイティブ UI システムに基づいて構築されています。 たとえば、 [ `Button` s](xref:Xamarin.Forms.Button)は iOS と Android では異なる方法で表示され、オペレーティングシステムのバージョンによっては異なる場合があります。 これに対し、CocosSharp ではプラットフォーム固有のビジュアルオブジェクトは使用されないため、すべてのビジュアルオブジェクトがすべてのプラットフォームで同じように表示されます。 もちろん、解像度と縦横比はデバイスによって異なります。これは、CocosSharp がどのようにビジュアルをレンダリングするかに影響する可能性があります。 これらの詳細については、このガイドの後半で説明します。

詳細については、 [「CocosSharp」セクション](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)を参照してください。

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>CocosSharp NuGet パッケージの追加

CocosSharp を使用する前に、開発者はプロジェクトにいくつかの追加を行う必要があり Xamarin.Forms ます。
このガイドでは、 Xamarin.Forms iOS、Android、および .NET Standard ライブラリプロジェクトを含むプロジェクトを想定しています。
すべてのコードは、.NET Standard library プロジェクトに記述されます。ただし、ライブラリは iOS および Android プロジェクトに追加する必要があります。

CocosSharp NuGet パッケージには、CocosSharp オブジェクトを作成するために必要なすべてのオブジェクトが含まれています。
CocosSharp NuGet パッケージには、 `CocosSharpView` で CocosSharp をホストするために使用されるクラスが含まれて Xamarin.Forms います。
**CocosSharp** NuGet を追加すると、 **CocosSharp**も自動的に追加されます。
これを行うには、[.NET Standard ライブラリ] プロジェクトの [**パッケージ**] フォルダーを右クリックし、[**パッケージの追加**] を選択します。検索用語として「 **CocosSharp**」を入力し、[CocosSharp] を選択して、[パッケージ**の Xamarin.Forms ****追加**] をクリックします。

![](cocossharp-images/image1.png "Add Packages Dialog")

**CocosSharp**と**CocosSharp**の両方の NuGet パッケージがプロジェクトに追加されます。

![](cocossharp-images/image2.png "Packages Folder")

プラットフォーム固有のプロジェクト (iOS や Android など) に対して上記の手順を繰り返します。

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>チュートリアル: アプリへの CocosSharp の追加 Xamarin.Forms

単純な CocosSharp ビューをアプリに追加するには、次の手順に従い Xamarin.Forms ます。

1. [Xamarin フォームページの作成](#1)
1. [CocosSharpView の追加](#2)
1. [GameScene の作成](#3)
1. [円を追加する](#4)
1. [CocosSharp との対話](#5)

CocosSharp view をアプリに正常に追加したら Xamarin.Forms 、 [CocosSharp のドキュメント](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)を参照して、CocosSharp でコンテンツを作成する方法を確認してください。

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Xamarin フォームページを作成する

CocosSharp は任意のコンテナーでホストでき Xamarin.Forms ます。 このページのこのサンプルでは、というページを使用 `HomePage` します。 `HomePage``Grid` Xamarin.Forms と CocosSharp を同じページに同時に表示する方法を示すために、によって半分に分割されます。

最初に、と2つのインスタンスが含まれるようにページを設定し `Grid` `Button` ます。

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

IOS では、は `HomePage` 次の図のように表示されます。

![](cocossharp-images/image3.png "HomePage Screenshot")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. CocosSharpView を追加する

クラスは、 `CocosSharpView` アプリに CocosSharp を埋め込むために使用され Xamarin.Forms ます。 `CocosSharpView`はを継承[ Xamarin.Forms します。ビュー](xref:Xamarin.Forms.View)クラスは、レイアウト用の使い慣れたインターフェイスを提供し、などのレイアウトコンテナー内で使用でき[ Xamarin.Forms ます。グリッド](xref:Xamarin.Forms.Grid)。 `CocosSharpView`メソッドを完了して、プロジェクトに新しいを追加し `CreateTopHalf` ます。

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

CocosSharp の初期化はすぐには行われないため、が作成を完了したときにイベントを登録し `CocosSharpView` ます。 メソッドで次の操作を行い `HandleViewCreated` ます。

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

メソッドには、注目すべき `HandleViewCreated` 重要な2つの詳細情報があります。 1つ目は `GameScene` クラスです。このクラスは次のセクションで作成します。 が作成され、 `GameScene` インスタンス参照が解決されるまで、アプリはコンパイルされないことに注意して `gameScene` ください。

2番目の重要な詳細は、 `DesignResolution` CocosSharp オブジェクトのゲームの可視領域を定義するプロパティです。 プロパティは、 `DesignResolution` 作成後に検索され `GameScene` ます。

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. GameScene を作成する

クラスは、 `GameScene` CocosSharp のから継承され `CCScene` ます。 `GameScene`は、純粋に CocosSharp を処理する最初のポイントです。 に含まれ `GameScene` ているコードは、プロジェクト内に格納されているかどうかにかかわらず、任意の CocosSharp アプリで機能 Xamarin.Forms します。

クラスは、 `CCScene` すべての CocosSharp レンダリングのビジュアルルートです。 すべての可視 CocosSharp オブジェクトは、内に含まれている必要があり `CCScene` ます。 具体的には、ビジュアルオブジェクトをインスタンスに追加 `CCLayer` し、それらのインスタンスをに追加する必要があり `CCLayer` `CCScene` ます。

次のグラフは、一般的な CocosSharp 階層を視覚化するのに役立ちます。

![](cocossharp-images/image4.png "Typical CocosSharp Hierarchy")

一度にアクティブにできるのは1つだけ `CCScene` です。 ほとんどのゲームでは `CCLayer` 、コンテンツを並べ替えるために複数のインスタンスを使用しますが、アプリケーションでは1つだけを使用します。 同様に、ほとんどのゲームは複数のビジュアルオブジェクトを使用しますが、アプリには1つしかありません。 CocosSharp のビジュアル階層に関する詳細な説明については、 [BouncingGame のチュートリアル](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/bouncing-game.md)を参照してください。

最初 `GameScene` に、クラスはほぼ空になります。このクラスは、の参照を満たすように作成するだけ `HomePage` です。 という名前の新しいクラスを .NET Standard ライブラリプロジェクトに追加 `GameScene` します。 次のようにクラスから継承する必要があり `CCScene` ます。

```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

これが定義されたので `GameScene` 、に戻り、 `HomePage` フィールドを追加できます。

```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

これで、プロジェクトをコンパイルして実行し、CocosSharp の実行を確認できるようになりました。 ここには何も追加 `GameScene,` していないので、ページの上半分は黒 (CocosSharp シーンの既定の色) になっています。

![](cocossharp-images/image5.png "Blank GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. 円を追加する

現在、アプリには CocosSharp エンジンの実行中のインスタンスがあり、空のが表示されてい `CCScene` ます。 次に、ビジュアルオブジェクト (円) を追加します。 この `CCDrawNode` クラスを使用すると、「 [ccdrawnode 使用したジオメトリの描画」ガイド](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md)で説明されているように、さまざまな幾何学図形を描画できます。

次のコードに示すように、クラスに円を追加 `GameScene` し、コンストラクターでインスタンス化します。

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

アプリを実行すると、CocosSharp 表示領域の左側に円が表示されるようになります。

![](cocossharp-images/image6.png "Circle in GameScene")

#### <a name="understanding-designresolution"></a>DesignResolution について

Visual CocosSharp オブジェクトが表示されたので、プロパティを調べることができ `DesignResolution` ます。

は、 `DesignResolution` オブジェクトを配置およびサイズ設定するための CocosSharp 領域の幅と高さを表します。 領域の実際の解像度は*ピクセル*単位で測定され、は `DesignResolution` ワールド*単位*で測定されます。 次の図は、iPhone 5 に表示されるビューのさまざまな部分の解像度を示しています。この解像度は、640x1136 ピクセルの画面解像度です。

![](cocossharp-images/image7.png "iPhone 5s Design Resolution")

上の図では、画面の外側のピクセル寸法が黒のテキストで表示されています。 単位は、図の内部に白いテキストで表示されます。 上に表示される重要な詳細を次に示します。

- CocosSharp 表示の原点は左下にあります。 右に移動すると X 値が増加し、上に移動すると Y 値が大きくなります。 Y 値は他の2D レイアウトエンジンと比較して逆になっていることに注意してください。ここで、(0, 0) はキャンバスの左上にあります。
- CocosSharp の既定の動作では、そのビューの縦横比が維持されます。 グリッドの最初の行は高さよりも幅が広いため、CocosSharp は、点線の白い四角形に示されているように、セルの幅全体を占めることはありません。 この動作は、「 [CocosSharp での複数の解像度の処理」ガイド](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/resolutions.md)で説明されているように変更できます。
- この例では、CocosSharp は、デバイスのサイズや縦横比に関係なく、100単位の表示領域を幅が広く、高さを維持します。 つまり、コードでは、X = 100 が CocosSharp ディスプレイの右端の境界を表し、すべてのデバイスでレイアウトの一貫性を維持していると想定できます。

#### <a name="ccdrawnode-details"></a>CCDrawNode の詳細

この単純なアプリでは、クラスを使用して `CCDrawNode` 円を描画します。 このクラスは、ベクターベースの geometry レンダリングを提供するため、ビジネスアプリケーションに非常に便利です。これは、にはない機能があり Xamarin.Forms ます。 円に加えて、クラスを使用して、 `CCDrawNode` 四角形、スプライン、直線、およびカスタム多角形を描画できます。 `CCDrawNode`は、イメージファイル (.png など) を使用する必要がないため、簡単に使用することもできます。 CCDrawNode の詳細については、「 [ccdrawnode を使用したジオメトリの描画」ガイド](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md)を参照してください。

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. CocosSharp との対話

CocosSharp のビジュアル要素 (など `CCDrawNode` ) は、クラスから継承さ `CCNode` れます。 `CCNode`には、親 (および) に対して相対的にオブジェクトを配置するために使用できる2つのプロパティが用意され `PositionX` て `PositionY` います。 現在、このコードでは、次のコードスニペットに示すように、これら2つのプロパティを使用して円の中心を配置しています。

```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

CocosSharp オブジェクトは、 Xamarin.Forms 親のレイアウトコントロールの動作に応じて自動的に配置されるほとんどのビューではなく、明示的な位置の値によって配置されることに注意してください。

ここでは、2つのボタンのいずれかをクリックして、円を左または右の10単位に移動するコードを追加します (円は CocosSharp ワールド単位の領域に描画されるため、ピクセルではありません)。 まず、クラスに2つのパブリックメソッドを作成し `GameScene` ます。

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

次に、の2つのボタンにハンドラーを追加して、 `HomePage` クリックに応答します。 完了すると、 `CreateBottomHalf` メソッドには次のコードが含まれます。

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

クリックすると、CocosSharp サークルが移動します。 また、CocosSharp キャンバスの境界をはっきりと見ることもできます。そのためには、左または右に十分な大きさの円を移動します。

![](cocossharp-images/image8.png "GameScene with Moving Circle")

## <a name="summary"></a>[概要]

このガイドでは、CocosSharp を既存のプロジェクトに追加する方法 Xamarin.Forms 、と CocosSharp 間の相互作用を作成する方法、 Xamarin.Forms CocosSharp でレイアウトを作成する際のさまざまな考慮事項について説明します。

CocosSharp ゲームエンジンには多数の機能と深さが用意されているため、このガイドでは、CocosSharp が実行できる操作のほんの一部のみを説明しています。 CocosSharp について詳しく知りたい開発者は、 [CocosSharp アーカイブ](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/)で多くの記事を見つけることができます。

## <a name="related-links"></a>関連リンク

- [CocosSharpForms (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)
