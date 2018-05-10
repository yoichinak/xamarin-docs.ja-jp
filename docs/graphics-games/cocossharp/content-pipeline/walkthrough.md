---
title: MonoGame PipelineTool を使用します。
description: MonoGame パイプライン ツールを使用してを作成および MonoGame コンテンツ プロジェクトを管理します。 コンテンツのプロジェクト内のファイルでは、Monogame パイプライン ツールによって処理され、CocosSharp および MonoGame アプリケーションで使用できる .xnb ファイルとして出力することができます。
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 50e6c611e285cde9184eed242353ad08b2a941ee
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="using-the-monogame-pipeline-tool"></a>MonoGame パイプライン ツールを使用します。

_MonoGame パイプライン ツールを使用してを作成および MonoGame コンテンツ プロジェクトを管理します。コンテンツのプロジェクト内のファイルでは、Monogame パイプライン ツールによって処理され、CocosSharp および MonoGame アプリケーションで使用できる .xnb ファイルとして出力することができます。_

MonoGame パイプライン ツールは、コンテンツ ファイルに変換するための使いやすい環境 **.xnb** CocosSharp および MonoGame アプリケーションで使用するファイル。 コンテンツのパイプラインとそれがゲームの開発に役立つ理由については、次を参照してください[コンテンツ パイプラインのこの概要。](~/graphics-games/cocossharp/content-pipeline/introduction.md)

このチュートリアルは、次について説明します。

 - MonoGame パイプライン ツールをインストールします。
 - CocosSharp プロジェクトを作成します。
 - コンテンツのプロジェクトを作成します。
 - MonoGame パイプライン ツールでファイルの処理
 - 実行時にファイルを使用します。

このチュートリアルでは、CocosSharp プロジェクトを使用を示すためにどのように **.xnb**ファイルを読み込むし、アプリケーションで使用します。 MonoGame のユーザーが CocosSharp と MonoGame の両方を使用して、同じように、このチュートリアルを参照することもあります **.xnb**コンテンツ ファイルです。

完成したアプリからのテクスチャを表示する 1 つスプライトが表示されます、 **.xnb**ファイルと 1 つのラベルからスプライト フォントを表示する、 **.xnb**ファイル。

![](walkthrough-images/image1.png "完成したアプリは .xnb ファイルからのテクスチャを表示する 1 つスプライトが表示されます。")


## <a name="monogame-pipeline-tool-discussion"></a>MonoGame パイプライン ツールについて

MonoGame パイプライン ツールは、Windows、OS X、Linux で使用できます。 このチュートリアルは、Windows では、ツールを実行するが、その後に指定できます Mac および Linux で同様です。 最大値または Linux のセットアップ ツールを取得する方法の詳細については、次を参照してください。[このページ](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/)です。

MonoGame パイプライン ツールは iOS アプリケーションも実行と Windows では、使用するための開発者には、コンテンツを作成することが[Xamarin Mac エージェント](~/ios/get-started/installation/windows/connecting-to-mac/index.md)Windows 上の開発を続けることができます。


## <a name="installing-the-monogame-pipeline-tool"></a>MonoGame パイプライン ツールをインストールします。

まず、MonoGame コンテンツ パイプラインを含む、MonoGame をインストールします。 MonoGame コンテンツ パイプラインは、for mac の個別のダウンロード MonoGame のすべてのインストーラーが見つかりません、 [MonoGame のダウンロード ページ](http://www.monogame.net/downloads/)です。 ダウンロードお MonoGame 取り付けたら、開発者が、Visual Studio の MonoGame Visual Studio での Mac にも使用できます。

![](walkthrough-images/image2.png "Mac 用 Visual Studio で、開発者による使用 MonoGame をすぎるインストールが、Visual Studio の MonoGame をダウンロードします。")

ダウンロードされると、おをインストーラーを実行し、既定の設定オプションを使用します。 インストーラーが完了したら、MonoGame パイプライン ツールがインストールされ、[スタート] メニューの検索で見つかんだことができます。

![](walkthrough-images/image3.png "インストーラーが完了したら、MonoGame パイプライン ツールがインストールされ、[スタート] メニューの検索で見つかんだことができます。")

MonoGame パイプライン ツールを起動します。

![](walkthrough-images/image4.png "MonoGame パイプライン ツールを起動します。")

MonoGame パイプライン ツールが実行されているコンテンツとゲーム プロジェクトを始めることができます。


## <a name="creating-an-empty-cocossharp-project"></a>CocosSharp の空のプロジェクトを作成します。

次の手順では、CocosSharp プロジェクトを作成します。 お CocosSharp プロジェクトによって作成されるフォルダー構造で、コンテンツのプロジェクトを保存できるようににまず CocosSharp プロジェクトを作成おことが重要です。 CocosSharp プロジェクトの構造を理解するを見て、 [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)、このガイドで使用します。 ただし、コンテンツを追加するには既存の CocosSharp プロジェクトがあれば、自由に BouncingGame の代わりにそのプロジェクトを使用してください。

プロジェクトが作成されたらがビルドされ、適切にセットアップされてすべてのものがあることが検証を実行します。

![](walkthrough-images/image5.png "プロジェクトを作成すると、実行にビルドされることを確認し、すべて正しくセットアップされています。")


## <a name="creating-a-content-project"></a>コンテンツのプロジェクトを作成します。

ゲームのプロジェクトができました MonoGame パイプライン プロジェクトを作成できます。 これを行う、MonoGame パイプライン ツールの選択で**ファイル > 新規しています.** し、プロジェクトのコンテンツ フォルダーに移動します。 Android では、フォルダーにある **[プロジェクトの root]\BouncingGame.Android\Assets\Content\** です。 Ios の場合、フォルダーにある **[プロジェクトの root]\BouncingGame.iOS\Content\** です。

変更、**ファイル名**に**ContentProject**  をクリックし、**保存**ボタン。

![](walkthrough-images/image8.png "ContentProject にファイル名を変更し、[保存] ボタンをクリックしてください")

MonoGame パイプライン ツールで、プロジェクトに関する情報が表示されます、プロジェクトが作成されると、ときに、ルート**ContentProject**項目が選択されています。

![](walkthrough-images/image9.png "プロジェクトが作成されると、MonoGame パイプライン ツール情報が表示されます、プロジェクトのルート ContentProject 項目が選択されています。")

一部のコンテンツのプロジェクトの最も重要なオプションを見てみましょう。


### <a name="output-folder"></a>出力フォルダー

ここでは、(コンテンツ プロジェクト自体) への相対フォルダー、出力 **.xnb**ファイルが保存されます。 煩雑にならないように、同じフォルダー、入力し、出力ファイルを使用します。 変更しましょう言い換えると、**出力フォルダー**する **.\** :

![](walkthrough-images/image10.png "")


### <a name="platform"></a>プラットフォーム

これは、コンテンツのターゲット プラットフォームを定義します。 これは、通知**Windows**既定では、のでおしますこれを変更するには、ターゲット プラットフォームに**Android** (または iOS iOS プロジェクトと共に次の場合)。

![](walkthrough-images/image11.png "既定では Windows であることを確認は、これは、iOS プロジェクトと共に次の場合に Android または iOS ターゲット プラットフォームを変更")


## <a name="processing-files-in-the-monogame-pipeline-tool"></a>MonoGame パイプライン ツールでファイルの処理

次に、追加されます。 コンテンツを、 **ContentProject**です。 このプロジェクトが追加されます。 ファイル、プロジェクトのルートにですが、大規模なプロジェクトがフォルダーでそのコンテンツを整理して通常。

2 つのファイル プロジェクトに追加されます。

 - A **.png**スプライトの描画に使用されるファイルです。 このファイルが[ここからダウンロード](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)です。
 - A **.spritefont**画面にテキストを描画するために使用するファイル。 コンテンツのパイプライン ツールは、ダウンロードするファイルがないように、新しい .spritefont ファイルを作成するをサポートします。


### <a name="adding-a-png-file"></a>ファイルは .png ファイルを追加します。

追加する、 **.png**ファイル、プロジェクトを最初にコピーしておくことを持つパイプライン プロジェクトと同じディレクトリに、 **.mgcb**拡張機能です。

![](walkthrough-images/image12.png "ファイルは .png ファイルをプロジェクトに追加します。")

次に、パイプライン プロジェクトにファイルを追加します。 これを行う MonoGame パイプライン ツールで、次のように選択します**編集 > 項目の追加.。**、select、 **ball.png**ファイルし、をクリックして**開く**です。 ファイルは、コンテンツのプロジェクトの一部となるようになりましたし、選択すると、そのプロパティが表示されます。

![](walkthrough-images/image13.png "ファイルは、コンテンツのプロジェクトの一部となるようになりましたし、選択すると、そのプロパティが表示されます。")

しましょうされているまま、すべての値が既定に CocosSharp で .xnb ファイルの読み込みに必要な変更はありません。 選択して、ファイルを構築できます、**ビルド > ビルド**メニュー オプションは、ビルドを開始され、ビルドの出力を表示します。 チェックして、ビルドが正しく動作を確認する、**コンテンツ**、新しいフォルダー **ball.xnb**ファイル。

![](walkthrough-images/image14.png "新しい ball.xnb ファイルのコンテンツ フォルダーをチェックして、ビルドが正しく動作を確認してください。")


### <a name="adding-a-spritefont-file"></a>.Spritefont ファイルを追加します。

MonoGame パイプライン ツールを通じて .spritefont ファイルを作成できます。 CocosSharp 内にあるフォントが必要です、**フォント**フォルダー、および CocosSharp テンプレートが自動的に、[フォント] フォルダーを自動的に作成します。 このフォルダーを選択して MonoGame パイプライン ツールに追加できる**編集 > 追加 > 既存のフォルダーしています.**.参照、**コンテンツ**フォルダーを選択、**フォント**フォルダーとクリック**OK**:

![](walkthrough-images/browsetofonts.png "コンテンツのフォルダーを参照、[フォント] フォルダーを選択し、[ok] をクリックしてください")

新しい .sprintefont ファイルを追加する フォント フォルダーを右クリックし、選択**追加 > 新しい項目の追加.** を選択、 **SpriteFont 説明**オプションで、名前を入力します**arial 36**、 をクリック**Ok**です。 CocosSharp では、– [FontType] の形式にする必要があります - フォント ファイルの具体的な名前を付ける必要があります [FontSize] です。 フォントがこの名前付け形式と一致しない場合に読み込まれません CocosSharp によって実行時にします。

![](walkthrough-images/image15.png "フォントがこの名前付け形式と一致しない場合に読み込まれません CocosSharp によって実行時に")

.Spritefont ファイルが実際には XML ファイルに、for mac Visual Studio を含む、任意のテキスト エディターで編集できます。 .Spritefont ファイルで編集する最も一般的な変数は、`FontName`と`Size`プロパティ。


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

任意のテキスト エディターでファイルを開くことがあります。 として、 **arial 36.spritefont**名前が示すように、`FontName`として`Arial`が、変更、`Size`値を`36`:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
## <a name="using-files-at-runtime"></a>実行時にファイルを使用します。

.Xnb ファイルが組み込まれています。 と、プロジェクトに使用する準備はできました。 追加されます。 ファイルを Visual Studio for Mac し、コードを追加、`GameScene.cs`ファイルをこれらのファイルをロードして、それらを表示します。


### <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Visual studio for Mac .xnb ファイルを追加します。

最初のファイル プロジェクトに追加されます。 展開して Mac を Visual Studio で、 **BouncingGame.Android**プロジェクトで、展開、**資産**フォルダーを右クリックし、**コンテンツ**フォルダー、および選択**追加 > ファイルを追加しています.** 最初に、ここを選択します、 **ball.xnb**先ほど作成し、をクリックして**開く**です。 上記の手順を繰り返しますが、追加し、 **arial 36.xnb**ファイル。 選択して、**その現在のサブディレクトリにファイルを残して**Mac 用の Visual Studio がファイルを追加する方法を確認する場合は、オプションです。 終了したら両方のファイルは、プロジェクトの一部にする必要があります。

![](walkthrough-images/image20.png "終了したら両方のファイルがプロジェクトの一部にする必要があります")


### <a name="adding-gamescenecs"></a>追加**GameScene.cs**

呼ばれるクラスを作成`GameScene,`スプライトとテキスト オブジェクトを含むです。 これを行うを右クリックし、 **BouncingGame** (いない BouncingGame.Android) プロジェクトし、選択**追加 > 新しいファイル.**.選択、**全般**カテゴリで、**空のクラス**オプション、および名前を入力し、 **GameScene**です。

私たちを変更して、作成した後、`GameScene.cs`を次のコードを含めるファイル。


```csharp
using System;
using CocosSharp;

namespace BouncingGame
{
    public class GameScene : CCScene
    {
        // All visual elements must be added to a CCLayer:
        CCLayer mainLayer;

        // The CCSprite is used to display the "ball" texture
        CCSprite sprite;
        // The CCLabelTtf is used to display the Arial36 sprite font
        CCLabelTtf label;

        public GameScene(CCWindow mainWindow) : base(mainWindow)
        {
            // Instantiate the CCLayer first:
            mainLayer = new CCLayer ();
            AddChild (mainLayer);

            // Now we can create the Sprite using the ball.xnb file:
            sprite = new CCSprite ("ball");
            sprite.PositionX = 200;
            sprite.PositionY = 200;
            mainLayer.AddChild (sprite);

            // The font name (arial) and size (36) need to match 
            // the .spritefont definition and file name.  
            label = new CCLabelTtf ("Using font 36", "arial", 36);
            label.PositionX = 200;
            label.PositionY = 300;
            mainLayer.AddChild (label);
        }
    }
} 
```

私たちは説明しません上記のコード CCSprite など CCLabelTtf CocosSharp visual オブジェクトによる操作は、「ので、 [BouncingGame ガイド](~/graphics-games/cocossharp/bouncing-game.md)です。

また、新しく作成されたを読み込むコードを追加する必要があります`GameScene`です。 開くことがこれを行うには、`GameAppDelegate.cs`ファイル (内に配置されていますが、 **BouncingGame** PCL) を変更し、`ApplicationDidFinishLaunching`メソッドを次のように。


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";

    // New code:
    GameScene gameScene = new GameScene (mainWindow);
    mainWindow.RunWithScene (gameScene);
} 
```

を実行しているときに、ゲームはようになります。

![](walkthrough-images/image1.png "を実行しているときに、ゲームを外観します。")


## <a name="summary"></a>まとめ

このチュートリアルでは MonoGame パイプライン ツールを使用して、入力の .png ファイルから .xnb ファイルを作成する方法と、新しく作成された .sprintefont ファイルから新しい .xnb ファイルを作成する方法を示しました。 また、.xnb ファイルを使用する CocosSharp プロジェクトを構成する方法と実行時にこれらのファイルを読み込む方法も説明します。

## <a name="related-links"></a>関連リンク

- [MonoGame ダウンロード](http://www.monogame.net/downloads/)
- [MonoGame パイプライン ドキュメント](http://www.monogame.net/documentation/?page=Pipeline)
- [For Android (サンプル) BouncingGame プロジェクトの開始](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [ball.png グラフィック (サンプル)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
