---
title: F＃ を使った UrhoSharp のプログラミング
description: このドキュメントでは、Visual Studio for Mac でを使用してF# 、簡単な Hello World UrhoSharp アプリケーションを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: d87749bd74cf2c478e96284060fed7386d10b853
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/19/2019
ms.locfileid: "69621004"
---
# <a name="programming-urhosharp-with-f"></a>F を使用した UrhoSharp のプログラミング\#

UrhoSharpは、C＃プログラマーによって使用されるのと同じライブラリーおよび概念を使用して F＃ でプログラムすることができます。 [UrhoSharp の使用](~/graphics-games/urhosharp/using.md) の記事は UrhoSharp エンジンの概要を説明しているので、この記事の前に読むことをお勧めします。

C++世界中の多くのライブラリと同様、多くの UrhoSharp 関数は、ブール値または成功または失敗を示す整数を返します。 これらの値`|> ignore`を無視するには、を使用する必要があります。

[サンプルプログラム](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)は、からF#の urhosharp の "Hello World" です。

## <a name="creating-an-empty-project"></a>空のプロジェクトの作成

UrhoSharp F#のテンプレートはまだ用意されていないため、独自の urhosharp プロジェクトを作成するには、[サンプル](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)から始めるか、次の手順を実行します。

1. Visual Studio for Mac から、新しい**ソリューション**を作成します。 **[IOS > アプリ > シングルビューアプリ]** を**F#** 選択し、実装言語としてを選択します。 
1. メインの**ストーリーボード**ファイルを削除します。 情報の**plist**ファイルを開き、 **iPhone/iPod Deployment 情報**ウィンドウで、**メインインターフェイス**の`Main`ドロップダウンから文字列を削除します。
1. **Viewcontroller**ファイルも削除します。

## <a name="building-hello-world-in-urho"></a>Urho での Hello World の構築

これで、ゲームのクラスの定義を開始する準備ができました。 少なくとも、の`Urho.Application`サブクラスを定義し、その`Start`メソッドをオーバーライドする必要があります。 このファイルを作成するには、 F#プロジェクトを右クリックし、 **[新しいファイルの追加...]** F#を選択して、プロジェクトに空のクラスを追加します。 新しいファイルはプロジェクト内のファイルの一覧の末尾に追加されますが、このファイルは、 **Appdelegate. fs**で使用される*前*に表示されるようにドラッグする必要があります。

1. Urho NuGet パッケージへの参照を追加します。
1. 既存の Urho プロジェクトから、(l) ディレクトリ**Coredata/** と**Data/** をプロジェクトの**リソース/** ディレクトリにコピーします。 プロジェクトで、**リソース** フォルダーを右クリックし、**追加**、既存のフォルダーの追加 の順にクリックして、これらのファイルをすべてプロジェクトに追加します。 F#

これで、プロジェクトの構造は次のようになります。

![](fsharp-images/solutionpane.png "プロジェクトの構造は次のようになります。")

新しく作成したクラスをの`Urho.Application`サブタイプとして定義し、その`Start`メソッドをオーバーライドします。

```fsharp
namespace HelloWorldUrho1

open Urho
open Urho.Gui
open Urho.iOS

type HelloWorld(o : ApplicationOptions) =
    inherit Urho.Application(o) 

override this.Start() = 
        let cache = this.ResourceCache
        let helloText = new Text()

        helloText.Value <- "Hello World from Urho3D, Mono, and F#"
        helloText.HorizontalAlignment <- HorizontalAlignment.Center
        helloText.VerticalAlignment <- VerticalAlignment.Center

        helloText.SetColor (new Color(0.f, 1.f, 0.f))
        let f = cache.GetFont("Fonts/Anonymous Pro.ttf")

        helloText.SetFont(f, 30) |> ignore
                  
        this.UI.Root.AddChild(helloText)
            
```

コードは非常に単純です。 `Urho.Gui.Text`クラスを使用して、特定のフォントと色のサイズを持つ中央揃えの文字列を表示します。 

ただし、このコードを実行する前に、UrhoSharp を初期化する必要があります。 

Appdelegate の fs ファイルを開き、次の`FinishedLaunching`ようにメソッドを変更します。

```fsharp
namespace HelloWorldUrho1

open System
open UIKit
open Foundation
open Urho
open Urho.iOS

[<Register ("AppDelegate")>]
type AppDelegate () =
    inherit UIApplicationDelegate ()

    override this.FinishedLaunching (app, options) =
        let o = ApplicationOptions.Default
     
        let g = new HelloWorld(o)
        g.Run() |> ignore
       
        true
```

に`ApplicationOptions.Default`は、ランドスケープモードアプリケーションの既定のオプションが用意されています。 これら`ApplicationOptions`を`Application`サブクラスの既定のコンストラクターに渡します ( `HelloWorld`クラスを定義したときに、 `inherit Application(o)`行が基底クラスのコンストラクターを呼び出すことに注意してください)。

のメソッドは`Run` 、プログラムを開始します。`Application` これは、にパイプ処理`int`できるを返すように`ignore`定義されています。

結果として得られるプログラムは、次のスクリーンショットのようになります。

![結果として得られるプログラムのスクリーンショット](fsharp-images/helloworldfsharp.png)

## <a name="related-links"></a>関連リンク

- [GitHub で参照 (サンプル)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
