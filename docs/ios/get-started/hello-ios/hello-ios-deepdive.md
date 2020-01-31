---
title: Hello, iOS – 深い分析
description: このドキュメントでは、"Hello, iOS" サンプル アプリケーションについて詳しく解説し、そのアーキテクチャ、ユーザー インターフェイス、コンテンツ ビュー階層、テスト、展開などを考察しています。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 61ba3a7e-fe11-4439-8bc8-9809512b8eff
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: 5fadd1ba556b15cb92134471f007e41f04fce69e
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724773"
---
# <a name="hello-ios--deep-dive"></a>Hello, iOS – 深い分析

クイックスタート チュートリアルでは、基本的な Xamarin.iOS アプリケーションのビルドと実行について説明しました。 次は、より複雑なプログラムをビルドできるように、iOS アプリケーションの仕組みの理解を深めます。 このガイドでは、iOS アプリケーション開発の基本的な概念についての理解できるように、Hello, iOS チュートリアルの手順について説明します。

このガイドでは、1 つの画面の iOS アプリケーションの構築に必要なスキルと知識を開発できます。 作業した後、Xamarin.iOS アプリケーションのさまざまな部分とそれらをまとめる方法を理解できます。

::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Visual Studio for Mac の概要

Visual Studio for Mac は、Visual Studio と Xcode から機能を結合した無料のオープンソース IDE です。 ビジュアル デザイナー、リファクタリング ツール付きのテキスト エディター、アセンブリ ブラウザー、ソース コード統合などの機能が完全に統合されています。 このガイドでは、Visual Studio for Mac の基本的な機能についていくつか説明しますが、Visual Studio for Mac を初めてご利用になる方は、[Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/) に関するドキュメントをご覧ください。

Visual Studio for Mac は、コードを*ソリューション*と*プロジェクト*に分けて整理するという Visual Studio の方法に従っています。 ソリューションとは、1 つまたは複数のプロジェクトを保持できるコンテナーです。 プロジェクトは、アプリケーション (iOS、Android など)、サポートするライブラリ、テスト アプリケーションなどの場合があります。 Phoneword アプリで、**単一ビュー アプリケーション** テンプレートが使用され、新しい iPhone プロジェクトが追加されています。 最初のソリューションは次のようになります。

![](hello-ios-deepdive-images/image30.png "A screenshot of the initial solution")

::: zone-end
::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Visual Studio の概要

Visual Studio は Microsoft 製の強力な IDE です。 ビジュアル デザイナー、リファクタリング ツール付きのテキスト エディター、アセンブリ ブラウザー、ソース コード統合などの機能が完全に統合されています。 このガイドでは、Visual Studio 用の Xamarin ツールを使用した Visual Studio のいくつかの基本的な機能について説明します。

Visual Studio は、コードをソリューションとプロジェクトに分けて整理しています。 ソリューションとは、1 つまたは複数のプロジェクトを保持できるコンテナーです。 プロジェクトは、アプリケーション (iOS、Android など)、サポートするライブラリ、テスト アプリケーションなどの場合があります。 Phoneword アプリで、**単一ビュー アプリケーション** テンプレートが使用され、新しい iPhone プロジェクトが追加されています。 最初のソリューションは次のようになります。

![](hello-ios-deepdive-images/vs-image30.png "A screenshot of the initial solution")

::: zone-end

## <a name="anatomy-of-a-xamarinios-application"></a>Xamarin.iOS アプリケーションの構造

::: zone pivot="macos"

左側は、ディレクトリ構造とソリューションに関連付けられているすべてのファイルを含む **Solution Pad** です。

![](hello-ios-deepdive-images/image31.png "The solution Pad, which contains the directory structure and all the files associated with the solution")

::: zone-end
::: zone pivot="windows"

右側は、ディレクトリ構造とソリューションに関連付けられているすべてのファイルを含む**ソリューション ウィンドウ**です。

![](hello-ios-deepdive-images/vs-image31.png "The solution Pane, which contains the directory structure and all the files associated with the solution")

::: zone-end

「[Hello, iOS](~/ios/get-started/hello-ios/hello-ios-quickstart.md)」チュートリアルでは、**Phoneword** というソリューションを作成し、iOS プロジェクト (**Phoneword_iOS**) をその中に配置しました。 プロジェクト内には次の項目があります。

- **References** アプリケーションのビルドと実行に必要なアセンブリが含まれています。 ディレクトリを展開すると、[System](https://docs.microsoft.com/dotnet/api/system)、System.Core、[System.Xml](https://docs.microsoft.com/dotnet/api/system.xml) などの .NET アセンブリへの参照、および Xamarin.iOS アセンブリへの参照が表示されます。
- **Packages** - Packages ディレクトリには、あらかじめ用意されている NuGet パッケージが含まれています。
- **Resources** - Resources フォルダーには、その他のメディアを格納します。
- **Main.cs** – このファイルには、アプリケーションのメイン エントリ ポイントが含まれています。 アプリケーションを起動するには、メイン アプリケーション クラスの名前 `AppDelegate` が渡されます。
- **AppDelegate.cs** – このファイルは、メイン アプリケーション クラスを含み、ウィンドウの作成、ユーザーインターフェイスの構築、オペレーティング システムからのイベントのリッスンを行います。
- **Main.storyboard** - ストーリーボードは、アプリケーションのユーザー インターフェイスの視覚的なデザインを含みます。 ストーリーボード ファイルは、iOS Designer と呼ばれるグラフィカル エディターで開きます。
- **ViewController.cs** - ビュー コントローラーは、ユーザーに表示されタッチする画面 (ビュー) を稼働させます。 ビュー コントローラーは、ユーザーとビュー間の対話を処理します。
- **ViewController.designer.cs** - `designer.cs` は、ビュー内のコントロールとビュー コントローラーでのそれらのコード表現を結び付ける自動生成されたファイルです。 これは、内部の組み込みのファイルであるため、IDE は手動による変更を上書きします。多くの場合、このファイルは無視できます。 ビジュアル デザイナーと背後のコード間の関係の詳細については、「[iOS Designer Basics](~/ios/user-interface/designer/introduction.md)」(iOS Designer の基本) を参照してください。
- **Info.plist** - **Info.plist** では、アプリケーション名、アイコン、起動イメージなどのアプリケーションのプロパティが設定されます。 これは、強力なファイルで、完全な概要については、「[Working with Property Lists](~/ios/app-fundamentals/property-lists.md)」(プロパティ リストの操作) ガイドを参照してください。
- **Entitlements.plist** - 権利のプロパティ リストを使用して、iCloud、PassKit などのアプリケーションの*機能* (App Store テクノロジとも呼ばれます) を指定することができます。 **Entitlements.plist**の詳細については、「[Working with Property Lists](~/ios/app-fundamentals/property-lists.md)」(プロパティ リストの操作) ガイドを参照してください。 権利の一般的な概要については、「[Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)」(デバイスのプロビジョニング) ガイドを参照してください。

## <a name="architecture-and-app-fundamentals"></a>アーキテクチャとアプリケーションの基礎

iOS アプリケーションにユーザー インターフェイスを読み込む前に、次の 2 つを実行する必要があります。 最初に、アプリケーションで*エントリ ポイント*を定義する必要があります。エントリ ポイントは、アプリケーションのプロセスがメモリに読み込まれるときに実行される最初のコードです。 次に、アプリケーション全体のイベントを処理し、オペレーティング システムと対話するクラスを定義する必要があります。

このセクションでは、次の図に示す関係について学習します。

[![](hello-ios-deepdive-images/image32.png "The Architecture and App Fundamentals relationships are illustrated in this diagram")](hello-ios-deepdive-images/image32.png#lightbox)

### <a name="main-method"></a>Main メソッド

iOS アプリケーションのメイン エントリ ポイントは、`Application` クラスです。 `Application` クラスは **Main.cs** ファイルに定義されており、静的な `Main` メソッドを含んでいます。 これは、新しい Xamarin.iOS アプリケーション インスタンスを作成し、OS イベントを処理する *Application Delegate* クラスの名前を渡します。 静的 `Main` メソッドのテンプレート コードは以下のように表示されます。

```csharp
using System;
using UIKit;

namespace Phoneword_iOS
{
    public class Application
    {
        static void Main (string[] args)
        {
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="application-delegate"></a>アプリケーション デリゲート

iOS では、*Application Delegate* クラスがシステム イベントを処理します。このクラスは **AppDelegate.cs** 内にあります。 `AppDelegate` クラスは、アプリケーションの*ウィンドウ*を管理します。 ウィンドウはユーザー インターフェイスのコンテナーとして機能する `UIWindow` クラスの 1 つのインスタンスです。 既定では、アプリケーションがそのコンテンツを読み込む先の 1 つのウィンドウのみを取得し、ウィンドウは*画面* (単一 `UIScreen` インスタンス) に接続されます。画面は物理デバイス画面のサイズに一致する境界の四角形を提供します。

*AppDelegate* は、アプリの起動が終了したときや、メモリが少ないときなどの重要なアプリケーション イベントに関するシステム更新情報のサブスクライブも行います。

AppDelegate テンプレートのコードは次のとおりです。

```csharp
using System;
using Foundation;
using UIKit;

namespace Phoneword_iOS
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        public override UIWindow Window {
            get;
            set;
        }

        ...
    }
}
```

アプリケーションがウィンドウを定義したら、ユーザー インターフェイスの読み込みを開始できます。 次のセクションでは、UI の作成について説明します。

## <a name="user-interface"></a>ユーザー インターフェイス

iOS アプリのユーザー インターフェイスは店舗の正面のようなものです。アプリケーションは、通常、1 つのウィンドウを取得しますが、必要な数のオブジェクトをウィンドウいっぱいに表示し、表示するアプリの必要に応じてオブジェクトの配置を変更することもできます。 このシナリオのオブジェクト (ユーザーに表示される物事) はビューと呼ばれます。 アプリケーションで 1 つの画面をビルドするには、*コンテンツ ビュー階層*にビューが相互に積み重ねられ、階層が単一のビュー コントローラーによって管理されます。 複数の画面があるアプリケーションには、複数のコンテンツ ビュー階層、それぞれに独自のビュー コントローラー、およびウィンドウ内のアプリケーションの場所のビューがあり、ユーザーに表示される画面に基づいて異なるコンテンツ ビュー階層が作成されます。

このセクションでは、ビュー、コンテンツ ビュー階層、および iOS Designer を記述することで、ユーザー インターフェイスについて説明します。

### <a name="ios-designer-and-storyboards"></a>iOS Designer とストーリーボード

iOS Designer は、Xamarin 内のユーザー インターフェイスを構築するためのビジュアル ツールです。 任意のストーリーボード (.storyboard) ファイル上でダブルクリックしてデザイナーを起動すると、次のスクリーンショットのようなビューが開きます。

::: zone pivot="macos"

![](hello-ios-deepdive-images/image33.png "iOS Designer Interface")

*ストーリーボード*は、アプリケーションの画面のビジュアル デザインと、画面間の切り替え効果と関係を含むファイルです。 ストーリーボードでのアプリケーションの画面の表現は _シーン_ と呼ばれます。 各シーンは、ビュー コントローラーと、それが管理するビューのスタック (コンテンツ ビュー階層) を表します。 新しい**単一ビュー アプリケーション** プロジェクトがテンプレートから作成されると、下のスクリーンショットに示すように、Visual Studio for Mac が、`Main.storyboard` という名前のストーリーボード ファイルを自動的に生成し、1 つのシーンを追加します。

![](hello-ios-deepdive-images/image34.png "Visual Studio for Mac automatically generates a Storyboard file called Main.storyboard and populates it with a single Scene")

ストーリーボード画面の下部にある黒いバーを選択してシーンのビュー コントローラーを選択することができます。 ビュー コントローラーは、`UIViewController` インスタンスで、コンテンツ ビュー階層の背景のコードを含んでいます。 次のスクリーン ショットに示すように、このビュー コントローラーのプロパティを **Properties Pad** 内で表示および設定できます。

![](hello-ios-deepdive-images/image35.png "The Properties Pane")

::: zone-end
::: zone pivot="windows"

![](hello-ios-deepdive-images/vs-image33.png "iOS Designer Interface")

*ストーリーボード*は、アプリケーションの画面のビジュアル デザインと、画面間の切り替え効果と関係を含むファイルです。 ストーリーボードでのアプリケーションの画面の表現は _シーン_ と呼ばれます。 各シーンは、ビュー コントローラーと、それが管理するビューのスタック (コンテンツ ビュー階層) を表します。 新しい**単一ビュー アプリケーション** プロジェクトがテンプレートから作成されると、下のスクリーンショットに示すように、Visual Studio が、`Main.storyboard` という名前のストーリーボード ファイルを自動的に生成し、1 つのシーンを追加します。

![](hello-ios-deepdive-images/vs-image34.png "Visual Studio automatically generates a Storyboard file called Main.storyboard and populates it with a single Scene")

ストーリーボード画面の下部にあるバーを選択してシーンのビュー コントローラーを選択することができます。 ビュー コントローラーは、`UIViewController` インスタンスで、コンテンツ ビュー階層の背景のコードを含んでいます。 次のスクリーン ショットに示すように、このビュー コントローラーのプロパティを**プロパティ ウィンドウ**内で表示および設定できます。

![](hello-ios-deepdive-images/vs-image35.png "The Properties Pane")

::: zone-end

_ビュー_ を選択するには、シーンの白い部分の内部をクリックします。 ビューは、画面の領域を定義し、その領域内のコンテンツを操作するためのインターフェイスを提供する `UIView` クラスのインスタンスです。 既定のビューは、デバイス全体の画面を表示する 1 つの "*ルート ビュー*" です。

シーンの左側には、次のスクリーンショットに示すように、フラグ アイコン付きの灰色の矢印があります。

 [![](hello-ios-deepdive-images/image37.png "A gray arrow with a flag icon")](hello-ios-deepdive-images/image37.png#lightbox)

灰色の矢印は、*Segue* ("セグエ" と発音) と呼ばれるストーリーボードの切り替え効果を表しています。 このセグエには基がないので、*ソースレス セグエ*と呼ばれます。 ソースレス セグエは、アプリケーションの起動時にアプリケーションのウィンドウにビューが読み込まれる最初のシーンをポイントします。 シーンおよびその内部のビューは、アプリが読み込まれるときにユーザーに最初に表示されます。

ユーザー インターフェイスを構築するときに、次のスクリーンショットに示すように、**ツールボックス**から追加のビューを、デザイン サーフェイス上のメイン ビューにドラッグすることができます。

::: zone pivot="macos"

![](hello-ios-deepdive-images/image38.png "Additional Views can be dragged from the Toolbox onto the main View on the design surface")

::: zone-end
::: zone pivot="windows"

![](hello-ios-deepdive-images/vs-image38.png "Additional Views can be dragged from the Toolbox onto the main View on the design surface")

::: zone-end

これらの追加のビューを*サブビュー*と呼びます。 ルート ビューとサブビューはどちらも `ViewController` によって管理される*コンテンツ ビュー階層*の一部です。 シーン内のすべての要素のアウトラインは、**ドキュメント アウトライン** パッドを確認することで表示することができます。

::: zone pivot="macos"

![](hello-ios-deepdive-images/image39.png "The Document Outline pad")

::: zone-end
::: zone pivot="windows"

![](hello-ios-deepdive-images/vs-image39.png "The Document Outline pad")

::: zone-end

サブビューは、次の図で強調表示されます。

::: zone pivot="macos"

![](hello-ios-deepdive-images/image40.png "The Subviews are highlighted in the diagram")

::: zone-end
::: zone pivot="windows"

![](hello-ios-deepdive-images/vs-image40.png "The Subviews are highlighted in the diagram")

::: zone-end

次のセクションでは、このシーンで表されるコンテンツ ビュー階層について詳しく説明します。

## <a name="content-view-hierarchy"></a>コンテンツ ビュー階層

_コンテンツ ビュー階層_ は、次の図に示すように、1 つのビュー コントローラーによって管理されるビューとサブビューのスタックです。

 [![](hello-ios-deepdive-images/image41.png "The Content View Hierarchy")](hello-ios-deepdive-images/image41.png#lightbox)

下のスクリーンショットに示すように、**プロパティ パッド**のビュー セクションで、ルート ビューの背景色を一時的に黄色に変更することで、`ViewController` のコンテンツ ビューを見やすくすることができます。

::: zone pivot="macos"

![](hello-ios-deepdive-images/image42.png "Changing the background color of the root View to yellow in the View section of the Properties Pad")

::: zone-end
::: zone pivot="windows"

![](hello-ios-deepdive-images/vs-image42.png "Changing the background color of the root View to yellow in the View section of the Properties Pad")

::: zone-end

次の図は、デバイスの画面にユーザー インターフェイスを表示するウィンドウ、ビュー、サブビュー、およびビュー コントローラー間の関係を示しています。

[![](hello-ios-deepdive-images/image43.png "The relationships between the Window, Views, Subviews, and view controller")](hello-ios-deepdive-images/image43.png#lightbox)

次のセクションでは、コードでビューを操作する方法について説明し、ビュー コントローラーとビュー ライフサイクルを使用してユーザーの操作をプログラムする方法を説明します。

## <a name="view-controllers-and-the-view-lifecycle"></a>ビュー コントローラーとビュー ライフサイクル

すべてのコンテンツ ビュー階層には、ユーザー操作を反映するビュー コントローラーがあります。 ビュー コントローラーの役割は、コンテンツ ビュー階層のビューを管理することです。 ビュー コントローラーは、コンテンツ ビュー階層の一部ではなく、インターフェイスの要素ではありません。 代わりに、画面上のオブジェクトでのユーザーの操作を実行するコードを提供します。

### <a name="view-controllers-and-storyboards"></a>ビュー コントローラーとストーリーボード

::: zone pivot="macos"

ビュー コントローラーは、ストーリーボードで、シーンの下部にあるバーとして表されます。 ビュー コントローラーを選択すると、**プロパティ パッド** にそのプロパティが表示されます。

![](hello-ios-deepdive-images/image44.png "Selecting the view controller brings up its properties in the Properties Pane")

このシーンによって表されるコンテンツ ビュー階層のカスタム ビュー コントローラー クラスは、**プロパティ パッド** の **[ID]** セクションで **[クラス]** プロパティを編集することによって設定できます。 たとえば、私たちの **Phoneword** アプリケーションでは、下のスクリーンショットに示すように、最初の画面のビュー コントローラーとして `ViewController` が設定されます。

![](hello-ios-deepdive-images/image45new.png "The Phoneword application sets the ViewController as the view controller")

::: zone-end
::: zone pivot="windows"

ビュー コントローラーは、ストーリーボードで、シーンの下部にあるバーとして表されます。 ビュー コントローラーを選択すると、**プロパティ ウィンドウ**にそのプロパティが表示されます。

![](hello-ios-deepdive-images/vs-image44.png "Selecting the view controller brings up its properties in the Properties Pane")

このシーンによって表されるコンテンツ ビュー階層のカスタム ビュー コントローラー クラスは、**プロパティ ウィンドウ**の **[ID]** セクションで **[クラス]** プロパティを編集することによって設定できます。 たとえば、私たちの **Phoneword** アプリケーションでは、下のスクリーンショットに示すように、最初の画面のビュー コントローラーとして `ViewController` が設定されます。

![](hello-ios-deepdive-images/vs-image45.png "The Phoneword application sets the ViewController as the view controller")

::: zone-end

これにより、ビュー コントローラーのストーリーボード表現が `ViewController` C# クラスにリンクされます。 `ViewController.cs` ファイルを開くと、下のコードで示すように、ビュー コントローラーが `UIViewController` の*サブクラス*になっていることがわかります。

```csharp
public partial class ViewController : UIViewController
{
    public ViewController (IntPtr handle) : base (handle)
    {

    }
}
```

これで `ViewController` は、ストーリーボードでこのビュー コントローラーに関連付けられているコンテンツ ビュー階層のやり取りを実行します。 次に、ビュー ライフサイクルと呼ばれるプロセスを紹介することで、ビューの管理でのビュー コントローラーの役割について説明します。

> [!NOTE]
> ユーザーの介入を必要としない表示専用の画面の場合は、**プロパティ パッド** で **[Class]** プロパティを空白のままにすることができます。 これにより、`UIViewController` の既定の実装としてビュー コントローラーのバッキング クラスを設定します。これは、カスタム コードを追加する予定がない場合に適しています。

### <a name="view-lifecycle"></a>ビュー ライフサイクル

ビュー コントローラーはウィンドウからのコンテンツ ビュー階層の読み込みとアンロードを担当します。 コンテンツ ビュー階層のビューで何か重要なことが発生した場合、オペレーティング システムは、ビュー ライフサイクルのイベントを介してビュー コントローラーに通知します。 ビュー ライフサイクルでメソッドをオーバーライドすることで、画面上のオブジェクトと対話でき、応答性の高い、動的なユーザー インターフェイスを作成することができます。

次に、基本的なライフサイクル メソッドとその機能について説明します。

- **ViewDidLoad** - ビュー コントローラーがコンテンツビュー階層をメモリに読み込むときに *1 回*呼び出されます。 このときにサブビューが初めてコード内で利用可能になるので、これは初期セットアップを実行するために最適なタイミングです。
- **ViewWillAppear** - ビュー コントローラーのビューがコンテンツ ビュー階層に追加されて画面に表示されるたびに呼び出されます。
- **ViewWillDisappear** - ビュー コントローラーのビューがコンテンツ ビュー階層から削除されて画面から消える直前に毎回呼び出されます。 このライフサイクル イベントは、クリーンアップと状態の保存に使用されます。
- **ViewDidAppear** と **ViewDidDisappear** - それぞれビューがコンテンツ ビュー階層に追加されるときとコンテンツ ビュー階層から削除されるときに呼び出されます。

ライフサイクルのいずれかの段階にカスタム コードを追加するときは、そのライフサイクル メソッドの*基本実装*を*オーバーライド*する必要があります。 これは、既に一部のコードがアタッチされている既存のライフサイクル メソッドを利用し、追加のコードで拡張することによって実現されます。 新しいコードの前に確実に元のコードが実行されるようにベースの実装がメソッドの内部から呼び出されます。 次のセクションでこの例を示します。

ビュー コントローラーの操作の詳細については、Apple の「[View Controller Programming Guide for iOS](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457-CH2-SW1)」(iOS 用ビュー コントローラー プログラミング ガイド) と [UIViewController リファレンス](https://developer.apple.com/documentation/uikit/uiviewcontroller?language=objc)に関するページを参照してください。

### <a name="responding-to-user-interaction"></a>ユーザー操作に対する応答

ビュー コントローラーの最も重要な役割は、ボタンを押す操作、ナビゲーションなどのユーザーの操作に応答することです。 ユーザーの操作を処理する最も簡単な方法は、ユーザーの入力をリッスンするようにコントロールを配置し、入力に応答するためのイベント ハンドラーをアタッチすることです。 たとえば、Phoneword アプリで示したように、タッチ イベントに応答するようにボタンを配置することができます。

このしくみを見てみましょう。
`Phoneword_iOS` プロジェクトでは、`TranslateButton` というボタンを現在のビュー階層に追加しました。

[![](hello-ios-deepdive-images/image1.png "A button was added called TranslateButton to the Content View Hierarchy")](hello-ios-deepdive-images/image1.png#lightbox)

**プロパティ パッド**で **Name** が **Button** コントロールに割り当てられるときに、iOS Designer が **ViewController.designer.cs** でそれを自動的にコントロールにマッピングし、`ViewController` クラス内で `TranslateButton` を使用できるようにします。 コントロールは、ビュー ライフサイクルの `ViewDidLoad` ステージで初めて利用可能になるので、このライフサイクル メソッドを使用して、ユーザーのタッチに応答します。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // wire up TranslateButton here
}
```

Phoneword アプリでは、`TouchUpInside` というタッチ イベントを使用してユーザーのタッチを待ち受けるようにします。 `TouchUpInside` は、コントロールのボタン内でのタッチ ダウン (指が画面に触れる) に続くタッチ アップ イベント (指が画面から離れる) を待ち受けます。 `TouchUpInside` の逆は、ユーザーがコントロールを下に押したときに発生する `TouchDown` イベントです。 `TouchDown` イベントは、多数のノイズをキャプチャし、ユーザーはコントロールから指をスライドさせて離してタッチをキャンセルするオプションを与えられません。 `TouchUpInside` は、**ボタン** タッチに応答するための最も一般的の方法であり、ユーザーがボタンを押すときに予想するエクスペリエンスを作成します。 これの詳細については、Apple の「[iOS Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/MobileHIG/index.html)」(iOS ヒューマン インターフェイス ガイドライン) を参照してください。

このアプリでは、`TouchUpInside` イベントがラムダで処理されますが、デリゲートまたは名前付きイベント ハンドラーを使用することもできます。 最後のボタン コードは次のようになります。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
      translatedNumber = Core.PhonewordTranslator.ToNumber(PhoneNumberText.Text);
      PhoneNumberText.ResignFirstResponder ();

      if (translatedNumber == "") {
        CallButton.SetTitle ("Call", UIControlState.Normal);
        CallButton.Enabled = false;
      } else {
        CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
        CallButton.Enabled = true;
      }
  };
}
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword で導入されているその他の概念

Phoneword アプリケーションでは、このガイドでは説明していない概念がいくつか導入されています。 たとえば、次のような概念です。

- **ボタンのテキストの変更** – Phoneword アプリでは、**ボタン**上で `SetTitle` を呼び出して、新しいテキストと**ボタンの** _コントロールの状態_を渡すことによって**ボタン**のと変更する方法を紹介しています。 たとえば、次のコードは、CallButton のテキストを "Call" に変更します。

    ```csharp
    CallButton.SetTitle ("Call", UIControlState.Normal);
    ```

- **ボタンの有効化と無効化** - **ボタン**は、`Enabled` または `Disabled` 状態になります。 無効になっている**ボタン**はユーザー入力に応答しません。 たとえば、次のコード例は `CallButton` を無効にしています。

    ```csharp
    CallButton.Enabled = false;
    ```

    ボタンの詳細については、[ボタン](~/ios/user-interface/controls/buttons.md)に関するガイドを参照してください。

- **キーボードを消去** – ユーザーがテキスト フィールドをタップすると、ユーザーが値を入力できるようにするキーボードが iOS によって表示されます。 残念ながら、キーボードを消去する組み込みの機能はありません。 ユーザーが `TranslateButton` を押したときにキーボードを消去する次のコードが `TranslateButton` に追加されます。

    ```csharp
    PhoneNumberText.ResignFirstResponder ();
    ```

    キーボードを消去する他の例については、「[Dismiss the Keyboard](https://github.com/xamarin/recipes/tree/master/Recipes/ios/input/keyboards/dismiss_the_keyboard)」 (キーボードの消去) のレシピを参照してください。

- **URL を使用して電話をかける** – Phoneword アプリで Apple URL スキームを使用してシステム電話アプリを起動します。 次のコードに示すように、カスタムの URL スキームは、"tel:" プレフィックスと変換された電話番号で構成されます。

    ```csharp
    var url = new NSUrl ("tel:" + translatedNumber);
    if (!UIApplication.SharedApplication.OpenUrl (url))
    {
        // show alert Controller
    }
    ```

- **アラートを表示** – シミュレーターや iPod Touch などの呼び出しをサポートしないデバイスでユーザーが電話をかけようとした場合、電話をかけることができないことを知らせる警告ダイアログが表示されます。 次のコードでは、警告コントローラーを作成して入力します。

    ```csharp
    if (!UIApplication.SharedApplication.OpenUrl (url)) {
                    var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                    alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                    PresentViewController (alert, true, null);
                }
    ```

    iOS の警告ビューの詳細については、[Alert Controller のレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)を参照してください。

## <a name="testing-deployment-and-finishing-touches"></a>テスト、展開、および最後の仕上げ

Visual Studio for Mac と Visual Studio のいずれも、アプリケーションをテストおよび展開するためのオプションを多数用意しています。 このセクションでは、デバッグ オプションについて説明し、デバイスでのアプリケーションのテストのデモンストレーションを示し、カスタム アプリ アイコンと起動イメージを作成するためのツールを紹介します。

### <a name="debugging-tools"></a>デバッグ ツール

アプリケーション コード内の問題の診断は困難なことがあります。 複雑なコードの問題の診断に役立てるために、[ブレークポイントを設定する](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)、[コードのステップを実行する](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)、または[ログ ウィンドウに情報を出力する](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)ことができます。

### <a name="deploy-to-a-device"></a>デバイスに展開する

iOS シミュレーターは、アプリケーションをテストする簡単な方法です。 シミュレーターには、場所の偽装、[移動のシミュレーション](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/test_location_changes_in_simulator)などのいくつかの役に立つテストのための最適化が用意されています。 ただし、ユーザーは、最終的なアプリをシミュレーターで使用しません。 すべてのアプリケーションは、早い段階で頻繁に実際のデバイスでテストしてください。

デバイスは、プロビジョニングに時間がかかり、Apple 開発者アカウントが必要です。 [デバイス プロビジョニング](~/ios/get-started/installation/device-provisioning/index.md) ガイドでは、開発用デバイスを準備する詳細な手順を説明しています。

> [!NOTE]
> 現時点では、Apple の求めにより、物理デバイスまたはシミュレーターのコードをビルドするために、開発証明書または_署名 ID_ を用意する必要があります。 [デバイス プロビジョニング ガイド](~/ios/get-started/installation/device-provisioning/manual-provisioning.md)の手順に従ってこれを設定します。

デバイスがプロビジョニングされたら、プラグインすることで展開し、次のスクリーンショットに示すようにビルド ツールバーでターゲットを iOS デバイスに変更して、 **[Start]** (**Play**) を押します。

::: zone pivot="macos"

![](hello-ios-deepdive-images/image46new.png "Pressing Start/Play")

::: zone-end
::: zone pivot="windows"

![](hello-ios-deepdive-images/vs-image46.png "Pressing Start/Play")

::: zone-end

iOS デバイスにアプリが展開されます。

[![](hello-ios-deepdive-images/image1.png "The app will deploy to the iOS device and run")](hello-ios-deepdive-images/image1.png#lightbox)

### <a name="generate-custom-icons-and-launch-images"></a>カスタム アイコンを生成し、イメージを起動する

誰もが、デザイナーを使用してカスタム アイコンを作成し、アプリで目立つようにする必要がある画像を起動できるわけではありません。カスタム アプリのアートワークを生成するためのいくつかの代替の方法を次に示します。

::: zone pivot="macos"

- [**Pixelmator**](https://www.pixelmator.com/) – 約 30 ドルの Mac 用の多様な画像編集アプリです。
- [**Fiverr**](https://www.fiverr.com/) – 5 ドルから利用でき、さまざまなデザイナーから選択してアイコンのセットを作成できます。 見つかる場合も見つからない場合もありますが、アイコンをすぐにデザインする必要がある場合は有効なリソースです。

::: zone-end
::: zone pivot="windows"

- Visual Studio - これを使用して、アプリ用の単純なアイコン セットを IDE で直接作成できます。
- [**Fiverr**](https://www.fiverr.com/) – 5 ドルから利用でき、さまざまなデザイナーから選択してアイコンのセットを作成できます。 見つかる場合も見つからない場合もありますが、アイコンをすぐにデザインする必要がある場合は有効なリソースです。

::: zone-end

アイコンと起動イメージのサイズと要件に関する詳細については、「[イメージの処理](~/ios/app-fundamentals/images-icons/index.md)」ガイドを参照してください。

## <a name="summary"></a>まとめ

おめでとうございます! これで、Xamarin.iOS アプリケーションのコンポーネント、およびそれを作成するためのツールを確実に理解できました。
[作業の開始シリーズの次のチュートリアル](~/ios/get-started/hello-ios-multiscreen/index.md)では、複数の画面を処理するようにアプリケーションを拡張します。 複数の画面を処理するようにアプリケーションを拡張する途中でナビゲーション コントローラーを実装して、ストーリーボードの Segue について学習し、モデル、ビュー、コントローラー (MVC) のパターンを紹介します。

## <a name="related-links"></a>関連リンク

- [Hello, iOS (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/hello-ios)
- [iOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/design/human-interface-guidelines/ios/overview/themes/)
- [iOS プロビジョニング ポータル](https://developer.apple.com/account/#/overview)
