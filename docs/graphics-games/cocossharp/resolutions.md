---
title: "CocosSharp で複数の解像度の処理"
description: "このガイドでは、さまざまな解像度のデバイスに正しく表示するゲームを開発する CocosSharp を操作する方法を説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 859ABF98-2646-431A-A4A8-3E7E48DA5A43
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 9b76376bdbcf10bf35768cfdb79b6823388e303c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="handling-multiple-resolutions-in-cocossharp"></a>CocosSharp で複数の解像度の処理

_このガイドでは、さまざまな解像度のデバイスに正しく表示するゲームを開発する CocosSharp を操作する方法を説明します。_

CocosSharp は、物理デバイスのディスプレイ上のピクセル数に関係なく、ゲーム内のオブジェクトのディメンションを標準化するためのメソッドを提供します。 従来、ゲームの Pc やゲーム コンソール用に開発された 1 つの解決を対象にできます。 さまざまなデバイスに対応する最新のゲーム – およびモバイル デバイスのゲーム特に – を構築する必要があります。 このガイドでは、物理的な画面の解像度の特性に関係なくゲーム CocosSharp を標準化する方法を示します。

CocosSharp の既定の解像度の動作は、物理的なピクセルをゲームの座標と一致します。 次の表では、さまざまなデバイスが 368 x 240 の幅と高さでバック グラウンド環境スプライトをレンダリングとを示します。 最初の行があるが技術的にはありません、実際のデバイス、デバイスの解像度に関係なく、スプライトの予想されるレンダリングではなくです。


| **デバイス** | **ディスプレイの解像度** | **サンプルのスクリーン ショット** |
|--- | --- |--- |
|希望の表示|368 x 240 (の縦横比に黒のバー)| ![368 x 240 (の縦横比に黒のバー)](resolutions-images/image1.png) |
|iPhone 4s|960 x 640| ![iPhone 4s 960x640](resolutions-images/image2.png) |
|iPhone 6 Plus|1920 x 1080| ![iPhone 6 Plus 1920 x 1080](resolutions-images/image3.png) |

このドキュメントでは、CocosSharp を使用して、上記の表に示すように問題を解決する方法について説明します。 つまり、任意のデバイスの画面の解像度に関係なく – 最初の行に示すように表示する方法をしれませんについて説明します。


# <a name="working-with-setdesignresolutionsize"></a>SetDesignResolutionSize の操作

`CCScene`クラスがルート コンテナーとして、すべてのビジュアル オブジェクトの通常使用されますが、という静的メソッドも用意されています。`SetDesignResolutionSize`すべてのシーンの既定のサイズを指定するためです。 つまり`SetDesignResolutionSize`メソッドにより、開発者は、さまざまなハードウェアの解像度で正しく表示するゲームを開発します。 CocosSharp プロジェクト テンプレートでは、このメソッドを使用して、次のコードに示すようにプロジェクトの既定のサイズを 1024 x 768 に設定。


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");
    CCSize windowSize = mainWindow.WindowSizeInPixels;
    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```

変更することができます、`desiredWidth`と`desiredHeight`上、ゲームの目的の解像度に合わせて表示を変更する変数。 たとえば、上記のコードは、全画面表示として 368 x 240 背景を表示するとおり変更できませんでした。


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");
    CCSize windowSize = mainWindow.WindowSizeInPixels;
    float desiredWidth = 368.0f;
    float desiredHeight = 240.0f;
    
    // This will set the world bounds to(0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```


# <a name="ccsceneresolutionpolicy"></a>CCSceneResolutionPolicy

`SetDesignResolutionSize` ゲーム ウィンドウが目的の解像度を調整する方法を指定することができます。 次のセクションでは、異なる 500 x 500 イメージを表示する方法をデモンストレーション`CCSceneResolutonPolicy`に渡される値、`SetDesignResolutionSize`メソッドです。 次の値がによって提供される、`CCSceneResolutionPolicy`列挙型。

 - `ShowAll` -全体の要求された解決、縦横比の維持を表示します。
 - `ExactFit` – 要求全体の解像度が表示されますが、縦横比を保持しません。
 - `FixedWidth` -縦横比を維持、要求された幅全体を表示します。
 - `FixedHeight` -縦横比を維持、要求された高さ全体を表示します。
 - `NoBorder` -全体の高さまたは幅全体、縦横比の維持を表示します。 デバイスの縦横比が目的の解像度の縦横比よりも大きい場合は、全体の幅が表示されます。 デバイスの縦横比が目的の解像度の縦横比より小さい場合は、全体の高さが表示されます。
 - `Custom` – 表示異なります、 `Viewport` 、現在のプロパティ`CCScene`です。

すべてスクリーン ショットは、横向きに iPhone 4 秒解像度 (960 x 640) で生成され、このイメージを使用します。

![](resolutions-images/image4.png "すべてのスクリーン ショットは、横方向に iPhone 4 秒解像度 960 x 640 で生成され、このイメージを使用")


## <a name="ccsceneresolutionpolicyshowall"></a>CCSceneResolutionPolicy.ShowAll

`ShowAll` ゲーム全体の解像度は、画面上で表示されますが、表示を示す*レター ボックス処理*縦横比が異なるを調整する (黒のバー)。 このポリシーは、ゲーム ビュー全体は任意歪みせず画面に表示される表示されることが保証に通常使用されます。

コードの例:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

レター ボックス処理は、左側と目的の解像度よりも太くなってされている物理縦横比に対応するイメージの右側に表示されます。

![](resolutions-images/image5.png "レター ボックス処理が左側と目的の解像度よりも太くなってされている物理縦横比に対応するイメージの右側に表示されます。")


## <a name="ccsceneresolutionpolicyexactfit"></a>CCSceneResolutionPolicy.ExactFit

`ExactFit` ゲーム全体の解像度がないレター ボックス処理で画面に表示される表示されることを指定します。 表示可能領域がゆがんで表示される可能性があります (縦横比が維持されない場合があります) に従ってハードウェア縦横比。

コードの例:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ExactFit);
```

レター ボックス処理は表示されませんが、ゲームのビューがゆがんで表示されるので、デバイスの解像度が長方形。

![](resolutions-images/image6.png "レター ボックス処理は表示されませんが、ゲームのビューがゆがんで表示されるので、デバイスの解像度が長方形")


## <a name="ccsceneresolutionpolicyfixedwidth"></a>CCSceneResolutionPolicy.FixedWidth

`FixedWidth` 渡される幅値をビューの幅に一致することを示す`SetDesignResolutionSize`が、表示可能な高さを物理デバイスの縦横比の対象になります。 高さの値に渡される`SetDesignResolutionSize`は物理デバイスの縦横比に基づいて実行時に計算するため、無視されます。 これは、計算された高さを必要な高さを (その結果画面から外れてされているゲームのビューの一部、)、または計算された高さが必要な高さを (その複数表示されているゲームのビューの結果) を超える可能性がありますよりも小さいする可能性があることを意味します。 これは、表示されているゲームをより詳細になり可能性があります、し、表示されるようレター ボックス処理が発生しました。ただし、余分なスペースする必要はありませんが表示されるビジュアル オブジェクトの場合は黒です。 

コードの例:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedWidth);
```

IPhone 4 秒では、計算された高さは約 333 単位数: 2、3 の縦横比があります。

![](resolutions-images/image7.png "IPhone 4 秒に 3:2 の縦横比があるため、計算された高さは約 333 ユニット")


## <a name="ccsceneresolutionpolicyfixedheight"></a>CCSceneResolutionPolicy.FixedHeight

概念的には、`FixedHeight`と同じように動作`FixedWidth`– ゲームが高さに渡された値に適用されます`SetDesignResolutionSize,`が物理的な解像度に基づいて実行時に幅を計算します。 前述のように、表示されている幅を指定することを意味のゲームが一部の結果として得られる、必要な幅より小さいか大きい画面またはそれぞれ、表示されているゲームの詳細をオフします。

コードの例:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

次のスクリーン ショットを横モードで実行されているアプリで作成されてから計算された幅が 500 – 具体的には 750 を超えています。 このポリシーは、0 の X 値左揃え、余分な解像度が画面の右側に表示します。

![](resolutions-images/image8.png "このポリシーは、0 の X 値左揃え、余分な解像度は、画面の右側にある表示")


## <a name="ccsceneresolutionpolicynoborder"></a>CCSceneResolutionPolicy.NoBorder

`NoBorder` 元の縦横比 (ゆがむことがなく) を維持しながらありませんレター ボックス処理でアプリケーションを表示しようとしています。 指定した解像度の縦横比には、デバイスの物理的な縦横比が一致すると、領域は発生しません。 縦横比が一致しない場合、クリッピングが発生します。

コードの例:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

次のスクリーン ショットは、クリップ、ディスプレイの幅のすべての 500 ピクセルが表示されている間、画面の上部と下部の一部を示します。

![](resolutions-images/image9.png "このスクリーン ショットには、クリップ、ディスプレイの幅のすべての 500 ピクセルが表示されている間、画面の上部と下部のパーツが表示されます。")


## <a name="ccsceneresolutionpolicycustom"></a>CCSceneResolutionPolicy.Custom

`Custom` により、各`CCScene`解像度で指定された基準とした独自のカスタム ビューポートを指定する`SetDesignResolutionSize`です。

シーンを定義、`Viewport`プロパティを構築することによって、`CCViewport`でさらに構築された、`CCRect`です。 詳細については`CCViewport`と`CCRect`を参照してください、 [CocosSharp API ドキュメント](https://developer.xamarin.com/api/namespace/CocosSharp/)です。 作成のコンテキストで、 `CCViewport` 、 `CCRect` (注文) のコンス トラクターのパラメーターで指定します。

 - x-各ユニットが 1 つの画面全体の幅を表す、ビューポートの水平方向にオフセットする量。 たとえば、画面の幅の半分の右にシフト シーンで .5f 結果の値を使用します。
 - y – 各ユニットが 1 つの画面全体の高さを表す、ビューポートの垂直方向にオフセットする量。 たとえば、画面の高さの半分ずつ下に移動、シーン内 .5f 結果の値を使用します。
 - 幅 – 水平部分にシーンを占有する必要があります。 たとえば、値 1 を使用して 3.0f 結果画面の 1/3 を水平方向に占有シーン/です。
 - 高さ – 垂直部分にシーンを占有する必要があります。 たとえば、値 1 を使用して 3.0f 結果画面の 1/3 を垂直方向に占有シーン/です。

コードの例:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.Custom);
...
float horizontalDisplayPortion = 2 / 3.0f;
float verticalDisplayPortion = 1 / 2.0f;
float displayOffsetInScreenWidths = 0;
float displayOffsetInScreenHeights = .5f;
var rectangle = new CCRect (
    displayOffsetInScreenWidths, 
    displayOffsetInScreenHeights, 
    horizontalDisplayPortion, 
    verticalDisplayPortion);
scene.Viewport = new CCViewport (rectangle);
```

上記のコードは、次が得られます。

![](resolutions-images/image10.png "上記のコードは、このスクリーン ショットで結果します。")


# <a name="defaulttexeltocontentsizeratio"></a>DefaultTexelToContentSizeRatio

`DefaultTexelToContentSizeRatio`より高い解像度スクリーンを備えたデバイスで高解像度のテクスチャを使用して簡略化します。 具体的には、このプロパティは、サイズまたは視覚要素の位置を変更することがなく高解像度の資産を使用するゲームを使用できます。 

たとえば、ゲームはゲーム文字が 200 ピクセル、高さ、およびゲームの画面の画像資産を含めることができます (の指定に従って`SetDesignResolutionSize`) 高さが 400 ピクセル可能性があります。 この場合、文字、画面の半分は占有します。 ただし、400 ピクセル高さ資産、高解像度のデバイスの文字の使用されていた文字は表示の全体の高さを占有します。 目的が高解像度のデバイスを活用するために同じサイズの文字を保持する場合は、余分なコードは、画面の高さの半分に文字スプライトを維持する必要があります。

上記の簡単な例では、開発者がデバイスの解像度に従って各視覚的要素のサイズ変更は実行を必要とせず、資産のより大きいサイズを追加する – ゲームの開発で一般的な問題を表します。 `DefaultTexelToContentSizeRatio` ビジュアル要素のサイズを変更するために使用するグローバル プロパティ。 この値には、割り当てられた値によって、特定の種類のすべてのビジュアル要素がサイズ変更します。

このプロパティは、次のクラス内に存在します。 

 - `CCApplication`
 - `CCSprite`
 - `CCLabelTtf`
 - `CCLabelBMFont`
 - `CCTMXLayer`

Mac のテンプレートのセットの CocosSharp Visual Studio`CCSprite.DefaultTexelToContentSizeRatio`に従って使用方法の例として、デバイスの幅。 次のコードが含まれている`CCApplicationDelegate.ApplicationDidFinishLaunching`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
           
    ...
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }
    ...           
}
```


## <a name="defaulttexeltocontentsizeratio-example"></a>DefaultTexelToContentSizeRatio 例

表示する方法`DefaultTexelToContentSizeRatio`visual のサイズに影響、要素上に示したコードを検討してください。


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

これにより、500 x 500 テクスチャを使用して次の図が発生します。

![](resolutions-images/image5.png "これは、結果、500 x 500 テクスチャを使用してこのイメージ")

IPad Retina は 2048 x 1536 の物理的な解像度で実行されます。 これは、上に表示されるように、ゲームが 1536 物理的なピクセルを 500 ピクセルをストレッチはことを意味します。 ゲームを利用この余分な解像度の新機能は通常と呼ばれるを作成することで*hd*資産 – 資産をより高い解像度画面上で実行するよう設計されています。 たとえば、このゲームはこのテクスチャのサイズは 1000 x 1000 の hd バージョンを使用する可能性があります。 ただし、大きいイメージを使用することになります、`CCSprite`幅と 1000 単位の高さ。 ゲームの 500 ユニットを常に表示するため (ために、`SetDesignResolutioSize`呼び出し)、1000 x 1000 スプライトが大きすぎます (だけ下の左側の部分に表示されます)。

![](resolutions-images/image11.png "1000 x 1000 スプライトにのみ、左側の下部に表示が大きすぎますなりますゲームでは、500 ユニット SetDesignResolutioSize 呼び出しによって常に表示、ので")

設定することが`CCSprite.DefaultTexelToContentSizeRatio`1000 x 1000 テクスチャの幅と高さ元 500 x 500 テクスチャが 2 回されているアカウントにします。


```csharp
CCSprite.DefaultTexelToContentSizeRatio = 2;
```

今すぐゲームを実行して場合 1000 x 1000 テクスチャは完全に表示のようになります。

![](resolutions-images/image12.png "今すぐ場合、ゲームを実行して 1000 x 1000 テクスチャ完全に表示されます。")


## <a name="defaulttexeltocontentsizeratio-details"></a>DefaultTexelToContentSizeRatio 詳細

`DefaultTexelToContentSizeRatio`プロパティは`static,`アプリケーション内のすべてのスプライト意味は同じ値を共有します。 異なる解像度に対して行われたアセット ゲーム用の一般的な方法では、解決カテゴリごとの資産の完全なセットを含むです。 既定の Mac テンプレート CocosSharp Visual Studio で提供**%ld**と**hd**テクスチャの 2 つのセットをサポートするゲーム用役に立つ、資産のフォルダーです。 コンテンツを含むサンプル コンテンツ フォルダーのようになります。

![](resolutions-images/image13.png "コンテンツを含むサンプル コンテンツ フォルダーのようになります")

既定のテンプレートも条件付きで、アプリケーションに追加するコードが含まれることに注意してください`ContentSearchPaths`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }
    ...           
}
```

これは、アプリケーションは、高解像度または標準解像度モードで実行されているかどうかに従ってパスのファイルの検索されることを意味します。 たとえば場合、 **hd**と**%ld**フォルダーという名前のファイルに含める**background.png**次に、次のコードは実行し、正しいファイルの解像度を選択。


```csharp
backgroundSprite  = new CCSprite ("background");
```


# <a name="summary"></a>まとめ

この記事では、デバイスの解像度に関係なく、正しく表示するゲームを作成する方法について説明します。 表示の使用例を異なる`CCSceneResolutionPolicy`デバイスの解像度に従ってゲームをサイズ変更するための値。 方法の例も用意されています。`DefaultTexelToContentSizeRatio`視覚要素に個別にサイズ変更を必要とせず複数のコンテンツのセットに合わせて使用できます。

## <a name="related-links"></a>関連リンク

- [CocosSharp の概要](~/graphics-games/cocossharp/first-game/index.md)
- [CocosSharp API ドキュメント](https://developer.xamarin.com/api/namespace/CocosSharp/)
