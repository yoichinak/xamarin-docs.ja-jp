---
title: F# を使用した UrhoSharp のプログラミング
description: 'このドキュメントでは、Visual Studio for Mac で F # を使用して単純な hello world UrhoSharp アプリケーションを作成する方法について説明します。'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: af9619ace957a47282cbf9fdefea4e81e7eace13
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86940011"
---
# <a name="programming-urhosharp-with-f"></a>F を使用した UrhoSharp のプログラミング\#

UrhoSharp は、C# プログラマが使用するのと同じライブラリおよび概念を使用して F # でプログラミングできます。 [UrhoSharp の使用](~/graphics-games/urhosharp/using.md)に関する記事では、urhosharp エンジンの概要を説明します。この記事の前に読む必要があります。

C++ で生成された多くのライブラリと同様、多くの UrhoSharp 関数は、ブール値または成功または失敗を示す整数を返します。 `|> ignore`これらの値を無視するには、を使用する必要があります。

この[サンプルプログラム](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)は、F # から UrhoSharp の "Hello World" です。

## <a name="creating-an-empty-project"></a>空のプロジェクトの作成

UrhoSharp の F # テンプレートはまだ使用できません。そのため、独自の UrhoSharp プロジェクトを作成するには、[サンプル](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)から始めるか、次の手順を実行します。

1. Visual Studio for Mac から、新しい**ソリューション**を作成します。 [ **IOS > アプリ > シングルビューアプリ**] を選択し、実装言語として**F #** を選択します。 
1. メインの**ストーリーボード**ファイルを削除します。 情報の**plist**ファイルを開き、 **IPhone/iPod Deployment 情報**ウィンドウで、 `Main` **メインインターフェイス**のドロップダウンから文字列を削除します。
1. **Viewcontroller**ファイルも削除します。

## <a name="building-hello-world-in-urho"></a>Urho での Hello World の構築

これで、ゲームのクラスの定義を開始する準備ができました。 少なくとも、のサブクラスを定義し、そのメソッドをオーバーライドする必要があり `Urho.Application` `Start` ます。 このファイルを作成するには、F # プロジェクトを右クリックし、[**新しいファイルの追加...** ] を選択して、プロジェクトに空の f # クラスを追加します。 新しいファイルはプロジェクト内のファイルの一覧の末尾に追加されますが、このファイルは、 **Appdelegate. fs**で使用される*前*に表示されるようにドラッグする必要があります。

1. Urho NuGet パッケージへの参照を追加します。
1. 既存の Urho プロジェクトから、(l) ディレクトリ**Coredata/** と**Data/** をプロジェクトの**リソース/** ディレクトリにコピーします。 F # プロジェクトで、 **Resources**フォルダーを右クリックし、[追加]、[**既存のフォルダーの追加**] の順にクリックして、これらのすべてのファイルをプロジェクトに追加します。

これで、プロジェクトの構造は次のようになります。

![プロジェクトの構造は次のようになります。](fsharp-images/solutionpane.png)

新しく作成したクラスをのサブタイプとして定義 `Urho.Application` し、そのメソッドをオーバーライドし `Start` ます。

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

コードは非常に単純です。 クラスを使用して、 `Urho.Gui.Text` 特定のフォントと色のサイズを持つ中央揃えの文字列を表示します。 

ただし、このコードを実行する前に、UrhoSharp を初期化する必要があります。 

AppDelegate の fs ファイルを開き、次のようにメソッドを変更し `FinishedLaunching` ます。

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

には、 `ApplicationOptions.Default` ランドスケープモードアプリケーションの既定のオプションが用意されています。 これら `ApplicationOptions` をサブクラスの既定のコンストラクターに渡し `Application` ます (クラスを定義したときに、 `HelloWorld` 行が基底クラスのコンストラクターを呼び出すことに注意して `inherit Application(o)` ください)。

`Run`のメソッドは、 `Application` プログラムを開始します。 これは、にパイプ処理できるを返すように定義されてい `int` `ignore` ます。

結果として得られるプログラムは、次のスクリーンショットのようになります。

![結果として得られるプログラムのスクリーンショット](fsharp-images/helloworldfsharp.png)

## <a name="related-links"></a>関連リンク

- [GitHub で参照 (サンプル)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
