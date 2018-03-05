---
title: "マルチプラット フォーム CocosSharp プロジェクトを作成します。"
description: "このチュートリアルでは、新しいマルチプラット フォーム CocosSharp ソリューションを作成する方法を示します。 このチュートリアルの結果は 3 つのプロジェクトを含む Mac ソリューション用の Visual Studio: 1 つのポータブル クラス ライブラリ プロジェクト、1 つの特定の Android プロジェクトおよび iOS に固有の 1 つのプロジェクトです。 作成されたプロジェクトの実行時に空の黒い画面が表示されます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 37C97693-B0A8-4064-97B6-A6FAB5BA4FB7
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 2906035ce9bd44d111b89ccfe7443896775315b7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-multi-platform-cocossharp-project"></a>マルチプラット フォーム CocosSharp プロジェクトを作成します。

_このチュートリアルでは、新しいマルチプラット フォーム CocosSharp ソリューションを作成する方法を示します。このチュートリアルの結果は 3 つのプロジェクトを含む Mac ソリューション用の Visual Studio: 1 つのポータブル クラス ライブラリ プロジェクト、1 つの特定の Android プロジェクトおよび iOS に固有の 1 つのプロジェクトです。作成されたプロジェクトの実行時に空の黒い画面が表示されます。_

CocosSharp 2D のゲーム エンジンは、コードと複数のプラットフォーム間で共有するコンテンツを許可します。 このチュートリアルでは、iOS と Android の開発の両方をサポートするプロジェクトを作成する方法を示します。 具体的には、次のトピックでは、このチュートリアルを取り上げます。

 - CocosSharp をインストールします。
 - 新しいソリューションの作成
 - `LoadGame` メソッド

# <a name="installing-cocossharp"></a>CocosSharp をインストールします。

最初に、追加 CocosSharp Visual studio for mac Mac でを実行する場合は、選択**Visual Studio for Mac** > **アドイン マネージャー.** . Windows で実行される場合は、選択**ツール** > **アドイン マネージャー.** . をクリックして、**ギャラリー**  タブで、展開、 **CocosSharp 項目**select、 **CocosSharp プロジェクト テンプレート**をクリックして、**をインストールしています.** .

![CocosSharp addin](part1-images/xamarinstudioaddinsmac.png "")

# <a name="creating-a-new-solution"></a>新しいソリューションの作成

CocosSharp をインストールすると、ソリューションを作成します。 Mac 用 Visual Studio で次のように選択します**ファイル** > **新規** > **ソリューションしています.。**.選択、**アプリ**オプションで 、**クロスプラット フォーム**セクションで、 **CocosSharp 空のプロジェクト**、順にクリック**次**:

![](part1-images/image1.png "クロス プラットフォーム セクションの下にあるアプリのオプションを選択、CocosSharp 空プロジェクトを選択し、[次へ] をクリックしてください")

名前を入力します**BouncingGame**の**プロジェクト名**をクリックし、**作成**:

![](part1-images/image2.png "プロジェクト名を BouncingGame の名前を入力してから作成 をクリックしてください")

プロジェクトが作成されていると、Mac を Visual Studio はコンパイルおよび実行できますグレーの背景を表示します。 

![](part1-images/image3.png "されたら、プロジェクトが作成され、Visual Studio for Mac をコンパイルしてグレーの背景を表示することを実行")


# <a name="loadgame-method"></a>LoadGame メソッド

既定の CocosSharp プロジェクトには、Android、iOS に固有のクラスには設定するためが含まれています、 `CCGameView`CocosSharp の開始に使用されます。 `CCGameView`プラットフォーム固有の方法でインスタンスを作成: iOS プロジェクトを作成、`CCGameView`で、`Main.storyboard`ファイル、Android の作成中、`CCGameView`で、`Main.axml`ファイル。 各プラットフォームで CCGameView インスタンスを使用して、`LoadGame`いくつかの基本的なセットアップを実行するメソッド。 このコードを変更することはありません、にもかかわらずいくつかの重要な詳細を見てをみましょう。

 - コード セット、 `gameView.DesignResolution` 1024 768 によってにします。 これは、現在のデバイスの縦横比、物理的な解像度、または印刷の向きに関係なくデバイス間での位置を標準化します。 
 - コードでは、いくつかの検索パスを追加します。 検索パスには、コンテンツ ディレクトリのプレフィックスなしロードすることもできます。 たとえば、以降、`"Sounds"`として検索パスでは、ディレクトリのファイル パスを追加`"Content/Sounds/mysound.xnb"`として読み込むように単純に`"mysound.xnb"`です。 検索パスと似ています`using`ステートメント コード – でコードを減らすことが、あいまいな可能性があります。
 - コードを構築、`GameLayer`インスタンス。 `GameLayer` `CCLayer`-場所を追加する予定のすべてのゲーム ロジックのクラスを継承します。 大規模なゲームは複数必要があります`CCLayer`インスタンス、または倍数`CCScene`インスタンス (複数を含めることができますが`CCLayer`インスタンス) が、1 つを`CCLayer`このゲームのです。

#  <a name="summary"></a>まとめ

このチュートリアルは、for mac Visual Studio を使用してクロスプラット フォーム CocosSharp プロジェクトを作成する方法を説明しました ゲーム、プロジェクトの開始点として使用できる空の画面になります。