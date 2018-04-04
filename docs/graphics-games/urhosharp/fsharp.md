---
title: F# によるプログラミング UrhoSharp
description: F# で Visual Studio の使用の Mac 簡単な UrhoSharp アプリケーションを作成する方法
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 1fff90056f39660695c1e6d9ed307fad575b5127
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="programming-urhosharp-with-f"></a>F# によるプログラミング UrhoSharp

_F# で Visual Studio の使用の Mac 簡単な UrhoSharp アプリケーションを作成する方法_

UrhoSharp は、f#、同じライブラリと c# プログラマが使用される概念を使用してプログラミングできます。 [を使用して UrhoSharp](~/graphics-games/urhosharp/using.md)記事 UrhoSharp エンジンの概要が示され、この記事の前に読み取る必要があります。

C++ の世界で発生した多くのライブラリと同様には、多くの UrhoSharp 関数は、ブール値または成功または失敗を示す整数を返します。 使用する必要があります`|> ignore`をこれらの値を無視します。

[サンプル プログラム](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)"Hello World"が、F# から UrhoSharp です。

## <a name="creating-an-empty-project"></a>空のプロジェクトを作成します。

F# UrhoSharp のテンプレートがないまだ UrhoSharp プロジェクトを作成するため、使用することができますを開始するか、[サンプル](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)またはこれらの手順に従います。

1. Mac 用 Visual Studio から新規作成**ソリューション**です。 選択**iOS > アプリ > 1 つのアプリの表示**選択**f#**実装の言語として。 
1. 削除、 **Main.storyboard**ファイル。 開く、 **Info.plist**ファイルし、[、 **iPhone/iPod 展開情報**] ウィンドウで、削除、`Main`内の文字列、 **Main インターフェイス**ドロップダウンします。
1. 削除、 **ViewController.fs**ファイルにもします。

## <a name="building-hello-world-in-urho"></a>Urho の建物 Hello World

ゲームのクラスの定義を開始する準備が整いました。 少なくとものサブクラスを定義する必要がある`Urho.Application`オーバーライドとその`Start`メソッドです。 このファイルを作成するには f# プロジェクトを右クリックし、**新しいファイルを追加しています.**し、プロジェクトに空の f# クラスを追加します。 新しいファイルは、プロジェクト内のファイルの一覧の末尾に追加されますが、表示されるようにドラッグする必要があります*する前に*で使用されている**AppDelegate.fs**です。

1. Urho NuGet パッケージへの参照を追加します。
1. 既存の Urho プロジェクトから (大) のディレクトリにコピー **CoreData/**と**データ/**プロジェクトの**リソース/**ディレクトリ。 F# のプロジェクトを右クリックし、**リソース**フォルダーと使用**追加/既存のフォルダーを追加**これらすべてのファイルをプロジェクトに追加します。

プロジェクトの構造のようになります。

![](fsharp-images/solutionpane.png "プロジェクトの構造のようになります")

サブタイプとして、新しく作成されたクラスを定義する`Urho.Application`オーバーライドとその`Start`メソッド。

```csharp
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

コードは、非常に簡単です。 使用して、`Urho.Gui.Text`特定のフォントと色のサイズの中央揃えの文字列を表示するクラス。 

このコードを実行する前に、UrhoSharp を初期化する必要があります。 

AppDelegate.fs ファイルを開き、変更、`FinishedLaunching`メソッドを次のようにします。

```csharp
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

`ApplicationOptions.Default`横モード アプリケーションの既定のオプションを提供します。 これらを渡す`ApplicationOptions`の既定のコンス トラクター、`Application`サブクラス (定義したときに注意してください、`HelloWorld`クラス、行`inherit Application(o)`基底クラス コンス トラクターを呼び出します)。 

`Run`のメソッド、`Application`プログラムを開始します。 返すとして定義されて、`int`がパイプする`ignore`です。 

生成されたプログラムは、ようになります。

![](fsharp-images/helloworldfsharp.png "生成されたプログラムのようになります")








## <a name="related-links"></a>関連リンク

- [GitHub (サンプル) の参照します。](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
