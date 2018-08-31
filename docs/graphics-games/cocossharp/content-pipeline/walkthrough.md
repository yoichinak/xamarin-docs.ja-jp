---
title: MonoGame パイプライン ツールを使用します。
description: MonoGame のパイプライン ツールは、MonoGame コンテンツ プロジェクト作成および管理に使用されます。 コンテンツ プロジェクト内のファイルでは、MonoGame パイプライン ツールによって処理され、CocosSharp および MonoGame アプリケーションで使用するための .xnb ファイルとして出力することができます。
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 347cb7e9d417f97cb6e8d78e67b1c76a378cd188
ms.sourcegitcommit: 7ffbecf4a44c204a3fce2a7fb6a3f815ac6ffa21
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2018
ms.locfileid: "34783300"
---
# <a name="using-the-monogame-pipeline-tool"></a>MonoGame パイプライン ツールを使用します。

_MonoGame のパイプライン ツールは、MonoGame コンテンツ プロジェクト作成および管理に使用されます。コンテンツ プロジェクト内のファイルでは、MonoGame パイプライン ツールによって処理され、CocosSharp および MonoGame アプリケーションで使用するための .xnb ファイルとして出力することができます。_

MonoGame のパイプライン ツールは、コンテンツ ファイルに変換するための使いやすい環境を提供します。 **.xnb** CocosSharp および MonoGame アプリケーションで使用するファイル。 コンテンツ パイプラインとはゲーム開発に役立つ理由については、次を参照してください[コンテンツ パイプラインの概要。](~/graphics-games/cocossharp/content-pipeline/introduction.md)

このチュートリアルは、以下に取り上げます。

 - MonoGame パイプライン ツールをインストールします。
 - CocosSharp プロジェクトの作成
 - コンテンツのプロジェクトを作成します。
 - MonoGame のパイプライン ツールでファイルの処理
 - 実行時にファイルを使用します。

このチュートリアルでは、CocosSharp プロジェクトを使用を示すためにどの **.xnb**ファイルを読み込むし、アプリケーションで使用します。 MonoGame のユーザーは CocosSharp と MonoGame を使用して、同じように、このチュートリアルを参照できるようにすることも **.xnb**コンテンツ ファイル。

完成したアプリからのテクスチャを表示する 1 つのスプライトが表示されます、 **.xnb**ファイルと 1 つのラベルからスプライトのフォントを表示する、 **.xnb**ファイル。

![](walkthrough-images/image1.png "完成したアプリ .xnb ファイルからテクスチャを表示する 1 つのスプライトが表示されます。")


## <a name="monogame-pipeline-tool-discussion"></a>MonoGame パイプライン ツールについて

MonoGame のパイプライン ツールは、Windows、OS X、Linux で使用できます。 このチュートリアルは、Windows のツールを実行しますが、続きます Mac および Linux でも。 最大値または Linux で設定するツールを取得する方法の詳細については、次を参照してください。[このページ](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/)します。

MonoGame パイプライン ツールは、iOS アプリケーションでも実行とを使用するための開発者、Windows 上のコンテンツを作成することが[Xamarin Mac Agent](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Windows での開発を続けることができます。


## <a name="installing-the-monogame-pipeline-tool"></a>MonoGame パイプライン ツールをインストールします。

まず、MonoGame コンテンツ パイプラインを含む、MonoGame をインストールします。 MonoGame のコンテンツ パイプラインは、個別のダウンロード for mac。 MonoGame のインストーラーはすべてで確認できます、 [MonoGame のダウンロード ページ](http://www.monogame.net/downloads/)します。 私たちをダウンロードします MonoGame が開発者をインストールしたら、Visual Studio の使用 MonoGame Visual studio for Mac もできます。

![](walkthrough-images/image2.png "MonoGame が for Mac Visual Studio で、開発者が使用 MonoGame をもインストールされると、Visual Studio のダウンロードします。")

ダウンロードされるが、インストーラーで実行され、既定値のオプションを使用します。 インストーラーが完了すると、MonoGame パイプライン ツールがインストールされているし、スタート メニューの検索で見つかんだことができます。

![](walkthrough-images/image3.png "MonoGame のパイプライン ツールがインストールされているし、スタート メニューの検索で見つかるインストーラーが完了すると、")

MonoGame パイプライン ツールを起動します。

![](walkthrough-images/image4.png "MonoGame パイプライン ツールを起動します。")

MonoGame パイプライン ツールを実行すると、コンテンツとゲーム プロジェクトを始めることができます。


## <a name="creating-an-empty-cocossharp-project"></a>CocosSharp の空のプロジェクトを作成します。

次の手順では、CocosSharp プロジェクトを作成します。 CocosSharp プロジェクトによって作成されるフォルダー構造で、コンテンツのプロジェクトを保存できるようににまず、CocosSharp プロジェクトを作成することが重要です。 CocosSharp プロジェクトの構造を理解するを参照してください、 [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)、このガイドで使用します。 ただし、コンテンツを追加したい既存の CocosSharp プロジェクトがあれば、自由に BouncingGame の代わりにそのプロジェクトを使用します。

プロジェクトが作成されたら、ビルドを正しく設定するすべてのものがあることを確認して実行します。

![](walkthrough-images/image5.png "プロジェクトが作成されたら、実行にビルドされることを確認して、すべて正しくセットアップされています。")


## <a name="creating-a-content-project"></a>コンテンツのプロジェクトを作成します。

ゲームのプロジェクトが作成できた MonoGame パイプライン プロジェクトを作成できます。 MonoGame パイプライン ツールの選択で**ファイル > 新規.** プロジェクトのコンテンツのフォルダーに移動します。 Android では、フォルダーにある **[プロジェクトの root]\BouncingGame.Android\Assets\Content\** します。 Ios の場合、フォルダーにある **[プロジェクトの root]\BouncingGame.iOS\Content\** します。

変更、**ファイル名**に**ContentProject**  をクリックし、**保存**ボタン。

![](walkthrough-images/image8.png "ContentProject にファイル名を変更し、[保存] ボタンをクリックします。")

MonoGame のパイプライン ツールで、プロジェクトに関する情報が表示されます、プロジェクトが作成されると、ときに、ルート**ContentProject**項目が選択されています。

![](walkthrough-images/image9.png "ルート ContentProject 項目が選択されているときに、MonoGame パイプライン ツールがプロジェクトに関する情報を表示、プロジェクトが作成されると、")

コンテンツのプロジェクトの最も重要なオプションのいくつかを見てみましょう。


### <a name="output-folder"></a>出力フォルダー

これは、(コンテンツ プロジェクト自体) への相対フォルダーで出力 **.xnb**ファイルが保存されます。 簡単に、同じフォルダー、入力を保持して、出力ファイルを使用します。 つまりは変更し、**出力フォルダー**する **.\** :

![](walkthrough-images/image10.png "")


### <a name="platform"></a>プラットフォーム

これには、コンテンツのターゲット プラットフォームを定義します。 これは、通知**Windows**既定では、ありますようにこれは、ターゲット プラットフォームに変更**Android** (または iOS iOS プロジェクトと共に次の場合)。

![](walkthrough-images/image11.png "既定では Windows であることを確認しているため、この iOS プロジェクトと共に次の場合は、Android または iOS をターゲット プラットフォームに")


## <a name="processing-files-in-the-monogame-pipeline-tool"></a>MonoGame のパイプライン ツールでファイルの処理

次に追加するコンテンツを**ContentProject**します。 このプロジェクトでは、ファイル、プロジェクトのルートに追加しますが、大規模なプロジェクトがフォルダー内のコンテンツを整理して通常。

2 つのファイル プロジェクトに追加します。

 - A **.png**スプライトを描画するために使用されるファイル。 このファイルは[ここからダウンロード](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)します。
 - A **.spritefont**画面にテキストを描画するために使用されるファイル。 コンテンツ パイプライン ツールをダウンロードするファイルがないように .spritefont の新しいファイルの作成をサポートします。


### <a name="adding-a-png-file"></a>.Png ファイルを追加します。

追加する、 **.png**ファイルをプロジェクトにを最初にコピーを持つパイプライン プロジェクトと同じディレクトリに、 **.mgcb**拡張機能。

![](walkthrough-images/image12.png ".Png ファイルをプロジェクトに追加します。")

次に、パイプライン プロジェクトにファイルを追加します。 これを行う MonoGame パイプライン ツールで、次のように選択します**編集 > 項目の追...**、select、 **ball.png**ファイルし、をクリックして**開く**です。 ファイルは、コンテンツのプロジェクトの一部となるようになりましたし、選択すると、そのプロパティが表示されます。

![](walkthrough-images/image13.png "ファイルは、コンテンツのプロジェクトの一部となるようになりましたし、選択すると、そのプロパティが表示されます。")

CocosSharp で .xnb ファイルの読み込みに必要な変更がないと、既定の設定のすべての値を残しますされてがあるとします。 選択して、ファイルを構築できます、**ビルド > ビルド**メニュー オプションは、ビルドを開始し、ビルドの出力を表示します。 チェックして、ビルドが正しく機能したことを確認する、**コンテンツ**、新しいフォルダー **ball.xnb**ファイル。

![](walkthrough-images/image14.png "新しい ball.xnb ファイルのコンテンツ フォルダーをチェックして、ビルドが正しく機能したことを確認します。")


### <a name="adding-a-spritefont-file"></a>.Spritefont ファイルを追加します。

.Spritefont MonoGame パイプライン ツールを使用してファイルを作成できます。 CocosSharp がフォントである必要があります、**フォント**フォルダー、および CocosSharp テンプレートが自動的に、[フォント] フォルダーを自動的に作成します。 このフォルダーを選択して、MonoGame パイプライン ツールに追加できます**編集 > 追加 > 既存のフォルダー.**.参照、**コンテンツ**フォルダーと選択、**フォント**フォルダーをクリックします**OK**:

![](walkthrough-images/browsetofonts.png "コンテンツのフォルダーを参照し、フォント フォルダーを選択し、[ok] をクリックします。")

新しい .sprintefont ファイルを追加する [フォント] フォルダーを右クリックし、選択**追加 > [新しい項目.** を選択、 **SpriteFont 説明**オプションで、名前を入力します**arial ~ 36**、] をクリック**Ok**します。 CocosSharp では、– [FontType] の形式である必要があります - フォント ファイルの非常に特定の名前を付ける必要があります [FontSize]。 フォントがこの名前付け形式と一致しない場合に読み込まれません CocosSharp で実行時にします。

![](walkthrough-images/image15.png "フォントがこの名前付け形式と一致しない場合に読み込まれません CocosSharp で実行時に")

.Spritefont ファイルは、for mac。 Visual Studio を含む、任意のテキスト エディターで編集可能な XML ファイル実際には .Spritefont ファイルで編集する最も一般的な変数は、`FontName`と`Size`プロパティ。


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

任意のテキスト エディターで、ファイルを開くします。 として、 **arial 36.spritefont**名前からわかるように、そこで、`FontName`として`Arial`が、変更、`Size`値を`36`:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
## <a name="using-files-at-runtime"></a>実行時にファイルを使用します。

.Xnb ファイルは、今すぐ構築されておりにこのプロジェクトで利用できます。 追加されていくファイルを Visual Studio for Mac し、コードを追加します、`GameScene.cs`ファイルをこれらのファイルを読み込み、それらを表示します。


### <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Visual studio for Mac の .xnb ファイルの追加

まず、ファイル、プロジェクトに追加します。 Visual studio for Mac では、拡張します、 **BouncingGame.Android**プロジェクトで、展開、**資産**フォルダーを右クリックし、**コンテンツ**フォルダー、および選択**追加 > ファイルを追加しています.** 最初に、選択、 **ball.xnb**先ほど作成し、をクリックして**オープン**します。 上記の手順を繰り返しますが、追加し、 **arial 36.xnb**ファイル。 選択、**その現在のサブディレクトリにファイルを残して**Visual Studio for Mac は、ファイルを追加する方法を確認する場合は、オプションします。 完了したら両方のファイルは、プロジェクトの一部にする必要があります。

![](walkthrough-images/image20.png "完了したら両方のファイルがプロジェクトの一部にする必要があります")


### <a name="adding-gamescenecs"></a>追加**GameScene.cs**

という名前のクラスを作成します`GameScene,`これは、スプライトとテキスト オブジェクトが含まれます。 これを行うを右クリックし、 **BouncingGame** (いない BouncingGame.Android) 順に選択して**追加 > 新しいファイル.**.選択、**全般**カテゴリを選択、**空のクラス**オプション、および名前を入力し、 **GameScene**します。

作成後に変更します。、`GameScene.cs`ファイルに次のコードを含めます。


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

私たちは説明しません上記のコードのため CCSprite など CCLabelTtf CocosSharp ビジュアル オブジェクトの使用については、 [BouncingGame ガイド](~/graphics-games/cocossharp/bouncing-game.md)します。

また、新しく作成されたを読み込むためのコードを追加する必要があります`GameScene`します。 開いて、これを行う、`GameAppDelegate.cs`ファイル (である、 **BouncingGame** PCL) および変更、`ApplicationDidFinishLaunching`メソッドを次のように。


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

![](walkthrough-images/image1.png "実行すると、ゲームを外観します。")


## <a name="summary"></a>まとめ

このチュートリアルでは MonoGame パイプライン ツールを使用して、入力の .png ファイルから .xnb ファイルを作成する方法と新しく作成された .sprintefont ファイルから新しい .xnb ファイルを作成する方法を示しました。 .Xnb ファイルを使用する CocosSharp プロジェクトを構成する方法と実行時にこれらのファイルを読み込む方法についても説明します。

## <a name="related-links"></a>関連リンク

- [MonoGame のダウンロード](http://www.monogame.net/downloads/)
- [MonoGame パイプラインのドキュメント](http://www.monogame.net/documentation/?page=Pipeline)
- [Android (サンプル) の BouncingGame のプロジェクトの開始](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [ball.png グラフィック (サンプル)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
