---
title: CocosSharp で複数の解像度の処理
description: このガイドでは、さまざまな解像度のデバイスに正しく表示するゲームを開発する CocosSharp を操作する方法を示します。
ms.prod: xamarin
ms.assetid: 859ABF98-2646-431A-A4A8-3E7E48DA5A43
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 6803dc2668b89ee2d037da8b34e202191dd5465d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61307818"
---
# <a name="handling-multiple-resolutions-in-cocossharp"></a>CocosSharp で複数の解像度の処理

_このガイドでは、さまざまな解像度のデバイスに正しく表示するゲームを開発する CocosSharp を操作する方法を示します。_

CocosSharp では、物理デバイスのディスプレイ上のピクセル数に関係なく、ゲーム内のオブジェクトのディメンションを標準化するためのメソッドを提供します。 これまでは、Pc、ゲーム機向けに開発されたゲームでは、1 つの解決をターゲットでした。 さまざまなデバイスに対応するためには、最新のゲーム – とモバイル デバイス用のゲームでは特に – をビルドする必要があります。 このガイドでは、物理的な画面の解像度の特性に関係なくゲーム CocosSharp を標準化する方法を示します。

CocosSharp の既定の解決の動作は、ゲーム内の座標と物理ピクセルを照合します。 次の表では、さまざまなデバイスは 368 x 240 の幅と高さでバック グラウンド環境スプライトをレンダリングを示します。 最初の行は、技術的にはありません、実際のデバイスがデバイスの解像度に関係なく、スプライトの予想されるレンダリングではなくには。


| **デバイス** | **画面の解像度** | **例のスクリーン ショット** |
|--- | --- |--- |
|必要な表示|368 x 240 (と縦横比の黒いバー)| ![368 x 240 (と縦横比の黒いバー)](resolutions-images/image1.png) |
|iPhone 4 s|960 x 640| ![iPhone 4s 960x640](resolutions-images/image2.png) |
|iPhone 6 Plus|1920 x 1080| ![iPhone 6 Plus 1920x1080](resolutions-images/image3.png) |

このドキュメントでは、CocosSharp を使用して、上記の表に示すように問題を解決する方法について説明します。 つまり、任意のデバイスの画面の解像度に関係なく – 最初の行に示すように表示する方法について説明します。


## <a name="working-with-setdesignresolutionsize"></a>SetDesignResolutionSize の操作

`CCScene`クラスがルート コンテナーとして、すべてのビジュアル オブジェクトの通常使用されますが、という静的メソッドも用意されています。`SetDesignResolutionSize`すべてシーンの既定のサイズを指定します。 つまり`SetDesignResolutionSize`メソッドにより、開発者はさまざまなハードウェアの解像度に正しく表示されるゲームを開発します。 CocosSharp プロジェクトのテンプレートでは、このメソッドを使用して、次のコードに示すように、1024 x 768、プロジェクトの既定のサイズを設定します。


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

変更することができます、`desiredWidth`と`desiredHeight`ゲームの目的の解像度に合わせて表示を変更する上記の変数。 たとえば、上記のコードをように変更を全画面表示として 368 x 240 背景を表示する可能性があります。


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


## <a name="ccsceneresolutionpolicy"></a>CCSceneResolutionPolicy

`SetDesignResolutionSize` ゲームのウィンドウが目的の解像度を調整する方法を指定できます。 次のセクションでは、異なる 500 x 500 イメージを表示する方法をデモンストレーションする`CCSceneResolutonPolicy`に渡される値、`SetDesignResolutionSize`メソッド。 次の値がによって提供される、`CCSceneResolutionPolicy`列挙型。

 - `ShowAll` -縦横比を維持、要求された解決全体を表示します。
 - `ExactFit` -要求された全体の解像度を表示しますが、縦横比を保持しません。
 - `FixedWidth` -縦横比を維持、要求された幅全体を表示します。
 - `FixedHeight` -縦横比を維持、要求された高さ全体を表示します。
 - `NoBorder` -全体の高さまたは幅全体、縦横比を維持のいずれかを表示します。 デバイスの縦横比が目的の解像度の縦横比よりも大きい場合は、全体の幅が表示されます。 デバイスの縦横比が目的の解像度の縦横比よりも小さい場合は、全体の高さが表示されます。
 - `Custom` – 表示によって異なります、`Viewport`プロパティ、現在の`CCScene`します。

すべてのスクリーン ショットでは、横方向に iPhone 4 s 解像度 (960 x 640) で生成され、このイメージを使用します。

![](resolutions-images/image4.png "すべてのスクリーン ショットが iPhone 4 s 解像度 960 x 640 ランドス ケープ方向で生成され、このイメージを使用")


### <a name="ccsceneresolutionpolicyshowall"></a>CCSceneResolutionPolicy.ShowAll

`ShowAll` ゲーム全体の解像度は、画面上で表示されますが、表示可能性がありますを指定します*レター ボックス処理*異なる縦横比を調整する (黒のバー)。 このポリシーは、ゲーム ビュー全体は歪みせず画面に表示される表示されることを保証として通常使用されます。

コード例:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

レター ボックス処理は、左側と目的の解像度よりも太くなっている物理の縦横比に対応するイメージの右側に表示されます。

![](resolutions-images/image5.png "レター ボックス処理が左右される目的の解像度よりも広い物理の縦横比に対応するイメージの右側に表示されます。")


### <a name="ccsceneresolutionpolicyexactfit"></a>CCSceneResolutionPolicy.ExactFit

`ExactFit` ゲーム全体の解像度がないレター ボックス処理と画面上に表示されることを指定します。 表示可能な領域がゆがんで表示される可能性があります (縦横比を維持しない可能性があります) に従って、ハードウェアの縦横比。

コード例:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ExactFit);
```

レター ボックスが表示されていませんが、デバイスの解像度は四角形であるために、ゲームのビューがゆがめられます。

![](resolutions-images/image6.png "レター ボックスが表示されていませんが、デバイスの解像度は四角形であるために、ゲームのビューがゆがめられます")


### <a name="ccsceneresolutionpolicyfixedwidth"></a>CCSceneResolutionPolicy.FixedWidth

`FixedWidth` 渡される幅値をビューの幅に一致することを指定します。 `SetDesignResolutionSize`、が表示可能な高さは、物理デバイスの縦横比に依存します。 高さの値に渡される`SetDesignResolutionSize`は物理デバイスの縦横比に基づく実行時に計算するため、無視されます。 これは、計算された高さを必要な高さを (その結果オフスクリーンされるゲーム ビューの部分、)、または計算された高さが必要な高さを (そのゲーム表示されているビューの結果) を超える可能性がありますよりも小さいことを意味します。 表示されているゲームの方があります、ためしように思われる場合、レター ボックス処理が発生しました。ただし、余分なスペースは必ずしも任意のビジュアル オブジェクトが表示されている場合は黒です。 

コード例:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedWidth);
```

IPhone 4 秒では、計算された高さが約 333 ユニット: 2、3 の縦横比があります。

![](resolutions-images/image7.png "計算された高さが約 333 単位であるために、iPhone 4 s が、縦横比は 3:2")


### <a name="ccsceneresolutionpolicyfixedheight"></a>CCSceneResolutionPolicy.FixedHeight

概念的には、`FixedHeight`同じように動作`FixedWidth`– ゲームに渡される高さの値に適用されます`SetDesignResolutionSize,`が物理的な解像度に基づいて実行時に幅を計算します。 前述のように、表示される幅のことを意味ゲーム中の一部で、必要な幅より小さいか大きい画面またはそれぞれ、表示されているゲームの詳細はオフです。

コード例:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

次のスクリーン ショットは横モードで実行されているアプリを使用して作成されたために、計算された幅は 500 – 具体的には 750 を超えています。 このポリシーが 0 の X 値を維持する解像度は、画面の右側に表示できるように左揃え。

![](resolutions-images/image8.png "このポリシーは、0 の X 値左揃え、解像度は、画面の右側に表示できます。")


### <a name="ccsceneresolutionpolicynoborder"></a>CCSceneResolutionPolicy.NoBorder

`NoBorder` 元の縦横比 (ゆがむことがなく) を維持しながらありませんレター ボックス処理でアプリケーションを表示しようとしています。 指定した解像度の縦横比には、デバイスの物理的な縦横比が一致すると、クリッピングは発生しません。 縦横比が一致しない場合、クリッピングが発生します。

コード例:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

次のスクリーン ショットでは、ディスプレイの表示幅 500 ピクセルのすべてが表示されている間は、クリップの上端と下端の一部を示します。

![](resolutions-images/image9.png "このスクリーン ショットには、ディスプレイの表示幅 500 ピクセルのすべてが表示されている間は、クリップの上部と下部のパーツが表示されます。")


### <a name="ccsceneresolutionpolicycustom"></a>CCSceneResolutionPolicy.Custom

`Custom` により、各`CCScene`解像度で指定された基準とした独自のカスタム ビューポートを指定する`SetDesignResolutionSize`します。

シーンの定義、`Viewport`プロパティを作成することにより、`CCViewport`でさらに構築された、`CCRect`します。 詳細については`CCViewport`と`CCRect`を参照してください、 [CocosSharp API ドキュメント](https://developer.xamarin.com/api/namespace/CocosSharp/)します。 作成のコンテキストで、 `CCViewport` 、 `CCRect` (順番に) コンス トラクターのパラメーターを指定します。

 - x – 各ユニットが 1 つの画面全体の幅を表す、ビューポートの水平方向にオフセットする量。 たとえば、画面の幅の半分の右側にシフト シーンで .5f 結果の値を使用します。
 - y: 各ユニットが 1 つの画面全体の高さを表す、ビューポートの垂直方向にオフセットする量。 たとえば、画面の高さの半分ずつ下に移動、シーン内 .5f 結果の値を使用します。
 - width: 水平方向の部分で、シーンを占有する必要があります。 たとえば、値 1 を使用して/3.0f 結果画面の 3 分の 1 を水平方向に占有しているシーンです。
 - height: 垂直方向の部分でシーンを占有する必要があります。 たとえば、値 1 を使用して/3.0f 結果画面の 3 分の 1 を垂直方向に占有しているシーンです。

コード例:


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

上記のコードは、次の結果します。

![](resolutions-images/image10.png "このスクリーン ショットで上記のコードを結果します。")


## <a name="defaulttexeltocontentsizeratio"></a>DefaultTexelToContentSizeRatio

`DefaultTexelToContentSizeRatio`より高い解像度スクリーンを備えたデバイスで高解像度のテクスチャを使いやすくなります。 具体的には、このプロパティは、サイズやビジュアル要素の位置を変更することがなく以上の解像度の資産を使用するゲームを使用できます。 

たとえば、ゲームはゲーム文字が、高さ 200 ピクセルと、ゲーム画面のアート アセットを含めることができます (で指定された`SetDesignResolutionSize`) 高さが 400 ピクセルにすることがあります。 この場合、文字、画面の半分は占有します。 ただし、400 ピクセル高さ資産、以上の解像度のデバイスに対して、文字の使用されていた場合、文字は表示の全体の高さを占有します。 意図をより高い解像度のデバイスを活用するために同じサイズの文字を保持する場合は、余分なコードは、画面の高さの半分にある文字のスプライトを維持するために必要です。

上で示した単純な例では、ゲームの開発 – より大きいサイズの資産を追加するデバイスの解像度に従って各ビジュアル要素のサイズ変更を実行する開発者を必要とせずに一般的な問題を表します。 `DefaultTexelToContentSizeRatio` ビジュアル要素のサイズを変更するために使用するグローバル プロパティ。 この値は、割り当て済みの値によって、特定の種類のすべてのビジュアル要素をサイズ変更します。

このプロパティは、次のクラス内に存在します。 

 - `CCApplication`
 - `CCSprite`
 - `CCLabelTtf`
 - `CCLabelBMFont`
 - `CCTMXLayer`

CocosSharp Visual Studio for Mac テンプレート セット`CCSprite.DefaultTexelToContentSizeRatio`に従って使用方法の例として、デバイスの幅。 次のコードが含まれている`CCApplicationDelegate.ApplicationDidFinishLaunching`:


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


### <a name="defaulttexeltocontentsizeratio-example"></a>DefaultTexelToContentSizeRatio 例

表示する方法`DefaultTexelToContentSizeRatio`ビジュアルのサイズに影響、要素上に示したコードを検討してください。


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

これは、500 x 500 テクスチャを使用して、次の図になります。

![](resolutions-images/image5.png "これは、結果、500 x 500 テクスチャを使用してこのイメージ")

IPad Retina は 2048 x 1536 の物理的な解像度で実行されます。 これは、上に表示されるように、ゲームが 1536 物理ピクセルで 500 ピクセルをストレッチはことを意味します。 ゲームを利用してこの余分な解決方法として指す何通常作成して*hd*資産 – 資産のより高い解像度画面上で実行するように設計されています。 たとえば、このゲームでは、このテクスチャのサイズは 1000 x 1000 の hd バージョンを使用できます。 ただし、大きいイメージを使用してになる、 `CCSprite` 1000 単位の高さと幅。 ゲームの 500 単位を常に表示するため (ために、`SetDesignResolutioSize`呼び出し)、1000 x 1000、スプライトが大きすぎます (その下部左側部分のみが表示されます) になります。

![](resolutions-images/image11.png "1000 x 1000 スプライトが大きすぎることの下部左側部分のみが表示されますなりますので、ゲームでは、500 単位 SetDesignResolutioSize 呼び出しによって常に表示、")

設定することが`CCSprite.DefaultTexelToContentSizeRatio`として全体に適用され、元の 500 x 500 テクスチャとして高さを 2 回される 1000 x 1000 テクスチャに対応します。


```csharp
CCSprite.DefaultTexelToContentSizeRatio = 2;
```

今すぐゲームを実行した場合、1000 x 1000 テクスチャに完全に表示のようになります。

![](resolutions-images/image12.png "今すぐゲームを実行した場合、1000 x 1000 テクスチャ完全が表示されます。")


### <a name="defaulttexeltocontentsizeratio-details"></a>DefaultTexelToContentSizeRatio の詳細

`DefaultTexelToContentSizeRatio`プロパティは`static,`同じ値を共有は、アプリケーション内のすべてのスプライトを意味します。 解決カテゴリごとの資産の完全なセットを含むさまざまな解像度の資産を使用したゲームの一般的なアプローチです。 CocosSharp Visual Studio for Mac テンプレートを既定で提供 **%ld**と**hd**というのゲームのテクスチャの 2 つのセットをサポートしている場合に便利ですが、資産のフォルダー。 サンプル コンテンツ フォルダーの内容は次のよう可能性があります。

![](resolutions-images/image13.png "次のようにコンテンツを含むコンテンツのサンプル フォルダー")

既定のテンプレートがアプリケーションの条件付きで追加するコードも含まれることに注意してください`ContentSearchPaths`:


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

これは、アプリケーションが高解像度または解像度を通常モードで実行されているかどうかに従ってパスのファイルの検索することを意味します。 たとえば場合、 **hd**と **%ld**という名前のファイルがフォルダーに含めることが**background.png**次のコードは実行され、解決する適切なファイルを選択します。


```csharp
backgroundSprite  = new CCSprite ("background");
```


## <a name="summary"></a>まとめ

この記事では、デバイスの解像度に関係なく正しく表示されるゲームを作成する方法について説明します。 表示の使用例を異なる`CCSceneResolutionPolicy`デバイスの解像度に従って、ゲームのサイズを変更する値。 方法の例も用意されています。`DefaultTexelToContentSizeRatio`個別にサイズを変更する視覚要素を必要とせずに複数のコンテンツのセットを対応するために使用できます。

## <a name="related-links"></a>関連リンク

- [CocosSharp の概要](~/graphics-games/cocossharp/index.md)
- [CocosSharp API ドキュメント](https://developer.xamarin.com/api/namespace/CocosSharp/)
