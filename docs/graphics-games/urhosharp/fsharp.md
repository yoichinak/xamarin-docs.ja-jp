---
title: F＃ を使った UrhoSharp のプログラミング
description: このドキュメントは、単純な hello world UrhoSharp を使用してアプリケーションを作成する方法を説明しますF#Visual studio for mac。
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 6269a7f2fa097136f492657d0ba7c6a1f056c38c
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832315"
---
# <a name="programming-urhosharp-with-f"></a>F＃ を使った UrhoSharp のプログラミング

UrhoSharp をプログラミングできるF#同じライブラリとによって使用される概念を使用してC#プログラマです。 [UrhoSharp を使用して](~/graphics-games/urhosharp/using.md)記事 UrhoSharp エンジンの概要を説明し、この記事の前に読む必要があります。

C++ の世界で発生した多くのライブラリと同様には、多くの UrhoSharp 関数は、ブール値または整数の成功または失敗を示す値を返します。 使用する必要があります`|> ignore`をこれらの値を無視します。

[サンプル プログラム](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)UrhoSharp からは、"Hello World"F#します。

## <a name="creating-an-empty-project"></a>空のプロジェクトを作成します。

あるないF#テンプレート UrhoSharp をまだ使用可能なで始めることができますか、独自の UrhoSharp プロジェクトを作成するには、ので、[サンプル](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)またはこれらの手順に従います。

1. Visual Studio for Mac では、作成、新しい**ソリューション**します。 選択**iOS > アプリ > 単一ビュー アプリ**選択**F#** 実装言語として。 
1. 削除、 **Main.storyboard**ファイル。 開く、 **Info.plist**ファイルし、 **iPhone/iPod 展開情報**ウィンドウで、削除、`Main`内の文字列、**メイン インターフェイス**ドロップダウンします。
1. 削除、 **ViewController.fs**ファイルにもします。

## <a name="building-hello-world-in-urho"></a>Urho の Hello World を構築

ゲームのクラスの定義を開始する準備が整いました。 少なくとものサブクラスを定義する必要があります`Urho.Application`オーバーライドとその`Start`メソッド。 このファイルを作成するを右クリックし、F#プロジェクトで、選択**新しいファイルを追加しています.** 、空の追加とF#クラスをプロジェクトにします。 新しいファイルは、プロジェクト内のファイルの一覧の末尾に追加されますが、表示されるようにドラッグする必要があります*する前に*で使用される**AppDelegate.fs**します。

1. Urho NuGet パッケージへの参照を追加します。
1. (大) のディレクトリをコピーすると、既存の Urho プロジェクトから**CoreData/** と**データ/** にプロジェクトの**リソース/** ディレクトリ。 F#プロジェクトを右クリックし、**リソース**フォルダー**追加/既存のフォルダーを追加**これらすべてのファイルをプロジェクトに追加します。

プロジェクトの構造は、以下のようになりますようになりました。

![](fsharp-images/solutionpane.png "プロジェクトの構造のようになります")

サブタイプとして、新しく作成されたクラスを定義する`Urho.Application`オーバーライドとその`Start`メソッド。

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

このコードは非常に簡単です。 使用して、`Urho.Gui.Text`特定のフォントと色のサイズの中央揃えの文字列を表示するクラス。 

このコードを実行する前に、UrhoSharp を初期化する必要があります。 

AppDelegate.fs ファイルを開き、変更、`FinishedLaunching`メソッドとして、次のとおりです。

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

`ApplicationOptions.Default`横モードのアプリケーションの既定のオプションを提供します。 これらを渡す`ApplicationOptions`の既定のコンス トラクター、`Application`サブクラス (定義したときに注意してください、`HelloWorld`クラス、行`inherit Application(o)`基底クラスのコンス トラクターを呼び出します)。

`Run`のメソッド、`Application`プログラムを開始します。 返すとして定義されて、`int`にパイプ処理する`ignore`します。

結果として得られるプログラムは、このスクリーン ショットのようになります。

![結果として得られるプログラムのスクリーン ショット](fsharp-images/helloworldfsharp.png)

## <a name="related-links"></a>関連リンク

- [GitHub (サンプル) で参照します。](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
