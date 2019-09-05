---
title: Hello, Mac – チュートリアル
description: このドキュメントでは、Xamarin.Mac アプリを作成する方法、Visual Studio for Mac、Xcode、Interface Builder について紹介します。 アウトレットやアクションを経由してコードに UI コントロールを公開する方法、Xamarin.Mac アプリケーションをビルド、実行、テストする方法について説明します。
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 37D0E9E6-979B-7069-B3BE-C5F0AF99BA72
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 09/02/2018
ms.openlocfilehash: c017bd1a932847885f93c2df84b53887b184b538
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291136"
---
# <a name="hello-mac-walkthrough"></a>Hello, Mac - チュートリアル

Xamarin.Mac を使うと、*Objective-C* または *Swift* で開発するときに使用するのと同じ macOS API を使用して、C# と .NET で完全にネイティブな Mac アプリを開発できます。 Xamarin.Mac は直接 Xcode と統合できるため、開発者は Xcode の _Interface Builder_ を使用して、アプリのユーザー インターフェイスを作成できます (または、必要に応じて C# コードで直接作成することも可能です)。

さらに、Xamarin.Mac アプリケーションは C# と .NET で記述されているため、コードを Xamarin.iOS や Xamarin.Android モバイル アプリと共有し、なおかつ各プラットフォームでネイティブ エクスペリエンスを提供することができます。

この記事では、ボタンがクリックされた回数を数えるシンプルな **Hello Mac** アプリを構築するプロセスを示しながら、Xamarin.Mac、Visual Studio for Mac、Xcode の Interface Builder を使用して Mac アプリを作成するために必要な主要概念を紹介します。

[![](hello-mac-images/run02-sml.png "Hello, Mac アプリの実行例")](hello-mac-images/run02.png#lightbox)

次の概念について説明します。

- **Visual Studio for Mac** – Visual Studio for Mac の概要と、Visual Studio for Mac を使って Xamarin.Mac アプリケーションを作成する方法。
- **Xamarin.Mac アプリケーションの構造** – Xamarin.Mac アプリケーションの構成。
- **Xcode の Interface Builder** – Xcode の Interface Builder を使用してアプリのユーザー インターフェイスを定義する方法。
- **Outlet と Action** – Outlet と Action を使用してユーザー インターフェイスのコントロールを接続する方法。
- **配置/テスト** – Xamarin.Mac アプリを実行してテストする方法。

## <a name="requirements"></a>必要条件

Xamarin.Mac アプリケーションの開発に必要な条件は次のとおりです。

- macOS High Sierra (10.13) 以上を実行している Mac コンピューター。
- [Xcode 9 以上](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).
- [Xamarin.Mac と Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/installation/) の最新バージョン。

Xamarin.Mac を使って構築されたアプリケーションを実行するには、次のものが必要です。

- macOS 10.7 以上を搭載している Mac コンピューター。

> [!WARNING]
> 今度の Xamarin.Mac 4.8 リリースでは、macOS 10.9 以降のみをサポートします。
> 以前のバージョンの Xamarin.Mac では macOS 10.7 以降をサポートしていましたが、これらの古い macOS バージョンは TLS 1.2 をサポートするための十分な TLS インフラストラクチャがありませんでした。 macOS 10.7 または macOS 10.8 をターゲットにするには、Xamarin.Mac 4.6 以前を使用してください。

## <a name="starting-a-new-xamarinmac-app-in-visual-studio-for-mac"></a>Visual Studio for Mac で新しい Xamarin.Mac アプリを起動する

前述のように、このガイドでは、`Hello_Mac` という Mac アプリを作成する手順を説明します。このアプリはメイン ウィンドウにボタンとラベルを 1 つずつ追加します。 ボタンがクリックされたとき、クリックされた回数がラベルに表示されます。

開始するには、次の手順を実行します。

1. Visual Studio for Mac を起動します。

    [![](hello-mac-images/setup01-sml.png "メインの Visual Studio for Mac インターフェイス")](hello-mac-images/setup01.png#lightbox)

2. **[新しいプロジェクト...]** ボタンをクリックして **[新しいプロジェクト]** ダイアログ ボックスを開き、 **[Mac]**  >  **[アプリ]**  >  **[Cocoa アプリ]** の順に選択して **[次へ]** ボタンをクリックします。

    [![](hello-mac-images/setup02-sml.png "Cocoa アプリの選択")](hello-mac-images/setup02.png#lightbox)

3. **[アプリ名]** に「`Hello_Mac`」と入力し、それ以外はすべて既定値のままにします。 **[次へ]** をクリックします。

    [![](hello-mac-images/setup03-sml.png "アプリの名前を設定")](hello-mac-images/setup03.png#lightbox)

4. コンピューター上の新しいプロジェクトの場所を確認します。

    [![](hello-mac-images/setup04-sml.png "新しいソリューションの詳細の確認")](hello-mac-images/setup04.png#lightbox)

5. **[作成]** ボタンをクリックします。

Visual Studio for Mac で新しい Xamarin.Mac アプリが作成され、アプリのソリューションに追加される既定のファイルが表示されます。

[![](hello-mac-images/project01-sml.png "ソリューションの新しい既定のビュー")](hello-mac-images/project01.png#lightbox)

Visual Studio for Mac では、Visual Studio 2019 と同じ **[ソリューション]** と **[プロジェクト]** の構造が使用されます。 ソリューションは 1 つまたは複数のプロジェクトを保持できるコンテナーです。プロジェクトには、アプリケーション、サポート ライブラリ、テスト アプリケーションなどを含めることができます。 **[ファイル] > [新しいプロジェクト]** テンプレートにより、ソリューションとアプリケーション プロジェクトが自動的に作成されます。

## <a name="anatomy-of-a-xamarinmac-application"></a>Xamarin.Mac アプリケーションの構造

Xamarin.Mac アプリケーションのプログラミングは、Xamarin.iOS を扱う場合とよく似ています。 iOS では、Mac で使われる Cocoa をスリムにしたバージョンである CocoaTouch フレームワークが使用されています。

プロジェクト内のファイルを見てください。

- **Main.cs** には、アプリのメイン エントリ ポイントが含まれています。 アプリが起動した時点では、`Main` クラスには実行される最初のメソッドが含まれます。
- **AppDelegate.cs** には、オペレーティング システムからのイベントをリッスンする `AppDelegate` クラスが含まれています。
- **Info.plist** には、アプリケーション名、アイコンなどのアプリのプロパティが含まれています。
- **Entitlements.plist** にはアプリの権利が含まれています。アプリの権利によってサンドボックスや iCloud のサポートなどにアクセスできます。
- **Main.storyboard** ではアプリのユーザー インターフェイス (ウィンドウとメニュー) が定義され、ウィンドウ間の相互接続が Segues 経由でレイアウトされます。 ストーリーボードは、ビュー (ユーザー インターフェイス要素) の定義を含む XML ファイルです。 このファイルは、Xcode 内の Interface Builder で作成、維持管理することができます。
- **ViewController.cs** はメイン ウィンドウのコントローラーです。 コントローラーについては別の記事で詳しく説明しますが、ここでは、コントローラーを特定のビューのメイン エンジンと考えることができます。
- **ViewController.designer.cs** には、メイン画面のユーザー インターフェイスとの統合のための組み込みコードが含まれています。

以下のセクションでは、これらのファイルのいくつかを簡単に見ていきます。 後で詳しく説明しますが、まず基本を理解することをお勧めします。

### <a name="maincs"></a>Main.cs

**Main.cs** ファイルはとてもシンプルです。 これには、新しい Xamarin.Mac アプリ インスタンスを作成し、OS イベントを処理するクラスの名前を渡す静的 `Main` メソッドが含まれています。ここでは、`AppDelegate` クラスが該当します。

```csharp
using System;
using System.Drawing;
using Foundation;
using AppKit;
using ObjCRuntime;

namespace Hello_Mac
{
    class MainClass
    {
        static void Main (string[] args)
        {
            NSApplication.Init ();
            NSApplication.Main (args);
        }
    }
}
```

### <a name="appdelegatecs"></a>AppDelegate.cs

`AppDelegate.cs` ファイルには `AppDelegate` クラスが含まれています。このクラスは、ウィンドウの作成と OS イベントのリッスンを行います。

```csharp
using AppKit;
using Foundation;

namespace Hello_Mac
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

このコードは、以前に iOS アプリを構築したことがある開発者以外にはおそらくなじみの薄いものですが、かなりシンプルです。

`DidFinishLaunching` メソッドは、アプリがインスタンス化された後に実行され、実際にアプリのウィンドウを作成して、その中にビューを表示するプロセスを開始します。

`WillTerminate` メソッドは、ユーザーまたはシステムがアプリのシャットダウンをインスタンス化したときに呼び出されます。 開発者はこのメソッドを使用して、アプリが終了する前に最終処理を行う必要があります (ユーザー設定やウィンドウのサイズと位置を保存するなど)。

### <a name="viewcontrollercs"></a>ViewController.cs

Cocoa (と派生の CocoaTouch) は、*モデル ビュー コントローラー* (MVC) パターンと呼ばれるパターンを使用します。 `ViewController` 宣言は、オブジェクトが実際のアプリ ウィンドウを制御することを表します。 一般に、作成されたすべてのウィンドウ (およびウィンドウ内の他の多くの要素) には、ウィンドウのライフサイクルに関する処理を行うコントローラーがあります。コントローラーは、ウィンドウを表示したり、ウィンドウに新しいビュー (コントロール) を追加したりします。

`ViewController` クラスはメイン ウィンドウのコントローラーです。 コントローラーによって、メイン ウィンドウのライフ サイクルに関する処理が行われます。 これについては後で詳しく説明します。ここでは簡単に見ていきます。

```csharp
using System;

using AppKit;
using Foundation;

namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }
    }
}
```

### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

メイン ウィンドウ クラスのデザイナー ファイルは最初は空ですが、Xcode Interface Builder を使用してユーザー インターフェイスを作成すると、Visual Studio for Mac によって自動的に作成されます。

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac to store outlets and
// actions made in the UI designer. If it is removed, they will be lost.
// Manual changes to this file may not be handled correctly.
//
using Foundation;

namespace Hello_Mac
{
    [Register ("ViewController")]
    partial class ViewController
    {
        void ReleaseDesignerOutlets ()
        {
        }
    }
}
```

デザイナー ファイルを直接編集しないでください。デザイナー ファイルは Visual Studio for Mac によって自動的に管理され、アプリ内のウィンドウまたはビューに追加されたコントロールへのアクセスに必要なコードを提供します。

Xamarin.Mac アプリ プロジェクトを作成し、そのコンポーネントの基本を理解したので、Xcode に切り替え、Interface Builder を使用してユーザー インターフェイスを作成します。

### <a name="infoplist"></a>Info.plist

`Info.plist` ファイルには、**名前**や**バンドル ID** などの Xamarin.Mac アプリに関する情報が含まれています。

[![](hello-mac-images/infoplist01.png "Visual Studio for Mac plist エディター")](hello-mac-images/infoplist01.png#lightbox)

また、**メイン インターフェイス** ドロップダウンの Xamarin.Mac アプリのユーザー インターフェイスを表示するために使用される_ストーリーボード_も定義します。 上記の例において、ドロップダウンの `Main` は**ソリューション エクスプローラー**のプロジェクト ツリーにある `Main.storyboard` に関連しています。 また、それらを含む*アセット カタログ* (この場合は **AppIcons**) を指定することで、アプリのアイコンが定義されます。

### <a name="entitlementsplist"></a>Entitlements.plist

アプリの `Entitlements.plist` ファイルは、Xamarin.Mac アプリの**サンドボックス**や **iCloud** などの権利を制御します。

[![](hello-mac-images/entitlements01.png "Visual Studio for Mac 権利エディター")](hello-mac-images/entitlements01.png#lightbox)

Hello World の例に権利は不要です。 次のセクションでは、Xcode の Interface Builder を使用して **Main.storyboard** ファイルを編集し、Xamarin.Mac アプリの UI を定義する方法を示します。

## <a name="introduction-to-xcode-and-interface-builder"></a>Xcode と Interface Builder の概要

Xcode の一部として、Apple は Interface Builder というツールを作成しました。このツールを使用すると、開発者はデザイナーでユーザー インターフェイスを視覚的に作成できます。 Xamarin.Mac は Interface Builder と完全に統合され、Objective-C ユーザーと同じツールで UI を作成できます。

作業を開始するには、**ソリューション エクスプローラー**で `Main.storyboard` ファイルをダブルクリックして、Xcode と Interface Builder での編集用に開きます。

[![](hello-mac-images/xcode01.png "ソリューション エクスプローラーの Main.storyboard ファイル")](hello-mac-images/xcode01.png#lightbox)

これで、次のスクリーンショットのように Xcode が起動します。

[![](hello-mac-images/xcode02.png "既定の Xcode Interface Builder ビュー")](hello-mac-images/xcode02.png#lightbox)

インターフェイスのデザインを始める前に、デザインに使用する主な機能を中心に Xcode の概要を簡単に説明します。

> [!NOTE]
> 開発者は、Xamarin.Mac アプリのユーザー インターフェイスを作成するのに必ずしも Xcode と Interface Builder を使用する必要はありません。C# コードから UI を直接作成できますが、それについてはこの記事では説明しません。 わかりやすくするために、このチュートリアルでは Interface Builder を使用してユーザー インターフェイスを作成します。

### <a name="components-of-xcode"></a>Xcode のコンポーネント

Visual Studio for Mac から Xcode で **.storyboard** ファイルを開くと、左側に**プロジェクト ナビゲーター**、中央に**インターフェイス階層**と**インターフェイス エディター**、右側に**プロパティとユーティリティ**のセクションが表示されます。

[![](hello-mac-images/xcode03.png "Xcode での Interface Builder のさまざまなセクション")](hello-mac-images/xcode03.png#lightbox)

以下のセクションでは、これらの Xcode の各機能と Xamarin.Mac アプリのインターフェイスを作成する方法について説明します。

### <a name="project-navigation"></a>プロジェクトのナビゲーション

Xcode で **.storyboard** ファイルを開くと、Visual Studio for Mac によって *Xcode プロジェクト ファイル*がバックグラウンドで作成され、Visual Studio for Mac と Xcode の間で変更が通知されます。 その後、開発者が Xcode から Visual Studio for Mac に戻ると、このプロジェクトに対する変更は Visual Studio for Mac によって Xamarin.Mac プロジェクトと同期されます。

**プロジェクト ナビゲーション** セクションでは、開発者はこの _shim_ Xcode プロジェクトを構成するすべてのファイル間を移動できます。 通常、開発者の関心の対象は、この一覧の `.storyboard` ファイル (`Main.storyboard`など) のみです。

### <a name="interface-hierarchy"></a>インターフェイス階層

**インターフェイス階層**セクションでは、開発者は**プレースホルダー**やメイン **ウィンドウ**などのユーザー インターフェイスの主要な複数のプロパティに簡単にアクセスできます。 このセクションを使用すると、ユーザー インターフェイスを構成する個々の要素 (ビュー) にアクセスしたり、階層内でそれらの要素をドラッグして、入れ子にする方法を調整したりできます。

### <a name="interface-editor"></a>インターフェイス エディター

**インターフェイス エディター**のセクションでは、ユーザー インターフェイスをグラフィカルにレイアウトする画面が提供されます。**プロパティとユーティリティ**のセクションにある**ライブラリ** セクションから要素をドラッグして、デザインを作成します。 ユーザー インターフェイス要素 (ビュー) をデザイン サーフェイスに追加すると、**インターフェイス エディター**に表示されている順序で**インターフェイス階層**セクションに追加されます。

### <a name="properties--utilities"></a>プロパティとユーティリティ

**プロパティとユーティリティ**のセクションは、**プロパティ** (インスペクターとも呼ばれる) と**ライブラリ**という 2 つの主要なセクションに分かれています。

[![](hello-mac-images/xcode04.png "プロパティ インスペクター")](hello-mac-images/xcode04.png#lightbox)

最初はこのセクションはほとんど空ですが、開発者が**インターフェイス エディター**または**インターフェイス階層**で要素を選択すると、**プロパティ** セクションには指定した要素と調整可能なプロパティに関する情報が設定されます。

**プロパティ** セクション内には、次の図に示すように 8 つの異なる*インスペクター タブ*があります。

[![](hello-mac-images/xcode05.png "すべてのインスペクターの概要")](hello-mac-images/xcode05.png#lightbox)

### <a name="properties--utility-types"></a>プロパティとユーティリティの種類

左から順に次のタブがあります。

- **ファイル インスペクター** – ファイル インスペクターは編集中の Xib ファイルの場所やファイル名などのファイル情報を示します。
- **クイック ヘルプ** – クイック ヘルプでは、Xcode での選択内容に基づいたヘルプが表示されます。
- **ID インスペクター** – ID インスペクターでは、選択したコントロールまたはビューに関する情報が表示されます。
- **属性インスペクター** – 属性インスペクターを使用すると、開発者は選択したコントロールまたはビューのさまざまな属性をカスタマイズできます。
- **サイズ インスペクター** – サイズ インスペクターを使用すると、開発者は選択したコントロールまたはビューのサイズとサイズ変更の動作を制御できます。
- **接続インスペクター** – 接続インスペクターには、選択したコントロールの**アウトレット**と**アクション**の接続が表示されます。 Outlet と Action については、以下で詳しく説明します。
- **バインド インスペクター** – バインド インスペクターを使用すると、開発者は値が自動的にデータ モデルにバインドされるようにコントロールを構成できます。
- **表示効果インスペクター** – 表示効果インスペクターを使用すると、開発者はコントロールにアニメーションなどの効果を指定できます。

**ライブラリ** セクションでは、コントロールとオブジェクトを探してデザイナーに配置し、グラフィカルにユーザー インターフェイスを構築します。

[![](hello-mac-images/xcode06.png "Xcode ライブラリ インスペクター")](hello-mac-images/xcode06.png#lightbox)

## <a name="creating-the-interface"></a>インターフェイスの作成

Xcode IDE と Interface Builder の基本を理解したら、開発者はメイン ビューのユーザー インターフェイスを作成できます。

次の手順に従って Interface Builder を使用します。

1. Xcode で、**ライブラリ** セクションから**ボタン**をドラッグします。

    [![](hello-mac-images/xcode07.png "ライブラリ インスペクターから NSButton を選択する")](hello-mac-images/xcode07.png#lightbox)

2. **Interface Editor** の**ビュー** (**ウィンドウ コントローラー**の下) にボタンをドロップします。

    [![](hello-mac-images/xcode08.png "インターフェイスのデザインにボタンを追加する")](hello-mac-images/xcode08.png#lightbox)

3. **属性インスペクター**の **Title** プロパティをクリックし、ボタンのタイトルを「**クリックしてください**」に変更します。

    [![](hello-mac-images/xcode09.png "ボタンのプロパティの設定")](hello-mac-images/xcode09.png#lightbox)

4. **ライブラリ セクション**から**ラベル**をドラッグします。

    [![](hello-mac-images/xcode10.png "ライブラリ インスペクターからラベルを選択する")](hello-mac-images/xcode10.png#lightbox)

5. **Interface Editor** のボタンの横にある**ウィンドウ**にラベルをドラッグします。

    [![](hello-mac-images/xcode11.png "インターフェイスのデザインにラベルを追加する")](hello-mac-images/xcode11.png#lightbox)

6. ラベルの右ハンドルをつかみ、ウィンドウの端に近づくまでドラッグします。

    [![](hello-mac-images/xcode12.png "ラベルのサイズ変更")](hello-mac-images/xcode12.png#lightbox)

7. 追加したボタンを**インターフェイス エディター**で選択し、ウィンドウ下部の**制約エディター** アイコンをクリックします。

    [![](hello-mac-images/xcode13.png "ボタンに制約を追加する")](hello-mac-images/xcode13.png#lightbox)

8. エディターの上部で、左上の**赤い I ビーム**をクリックします。 これにより、ウィンドウのサイズが変更されても、ボタンが画面の左上隅の同じ位置に維持されます。

9. 次に、 **[高さ]** と **[幅]** のボックスをクリックし、既定のサイズをそのまま使用します。 こうすると、ウィンドウのサイズが変更されても、ボタンが同じサイズに維持されます。

10. **[Add 4 Constraints]\(4 制約の追加\)** ボタンをクリックして制約を追加し、エディターを閉じます。

11. ラベルを選択し、**制約エディター** アイコンをもう一度クリックします。

    [![](hello-mac-images/xcode14.png "ラベルに制約を追加する")](hello-mac-images/xcode14.png#lightbox)

12. **制約エディター**の上、右、左にある**赤い I ビーム**をクリックすると、指定された X と Y の位置にラベルが固定され、実行中のアプリケーションでウィンドウのサイズが変更されたときに拡大、縮小するように指定されます。

13. もう一度 **[高さ]** ボックスをクリックし、既定のサイズをそのまま使用します。次に、 **[Add 4 Constraints]\(4 制約の追加\)** ボタンをクリックして制約を追加し、エディターを閉じます。

14. ユーザー インターフェイスに対する変更を保存します。

コントロールのサイズ変更と移動中、[macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)に基づく便利なヒントが Interface Builder に表示されます。 これらのガイドラインは、開発者が Mac ユーザーにとって使い慣れた外観と操作感を持つ高品質なアプリを作成するのに役立ちます。

**インターフェイス階層**セクションを参照して、ユーザー インターフェイスを構成する要素のレイアウトと階層がどのように表示されるかを確認します。

[![](hello-mac-images/xcode15.png "インターフェイス階層内の要素の選択")](hello-mac-images/xcode15.png#lightbox)

ここから、開発者は必要に応じて項目を選択して編集したり、UI 要素をドラッグして順序を変更したりできます。 たとえば、UI 要素が別の要素で覆われている場合、その要素をリストの一番下にドラッグすると、その要素をウィンドウの一番上の項目にすることができます。

ユーザー インターフェイスを作成したら、開発者は UI 項目を公開して、Xamarin.Mac が C# コードでそれらの項目にアクセスして対話できるようにする必要があります。 次の「**Outlet と Action**」のセクションで、その方法を説明します。

### <a name="outlets-and-actions"></a>Outlet と Action

**Outlet** と **Action** とは何でしょうか? 従来の .NET ユーザー インターフェイス プログラミングでは、ユーザー インターフェイスのコントロールは追加時にプロパティとして自動的に公開されます。 Mac では事情が異なり、ビューにコントロールを追加しただけでは、コントロールはコードにアクセスできません。 開発者は、UI 要素を明示的にコードに公開する必要があります。 これを行うために、Apple は 2 つのオプションを提供しています。

- **Outlet**  – Outlet はプロパティに似ています。 開発者がコントロールをアウトレットに接続すると、プロパティを介してコードに公開されるため、イベント ハンドラーをアタッチしたり、メソッドを呼び出したりする操作を行うことができます。
- **アクション** – アクションは WPF のコマンド パターンに似ています。 たとえば、コントロールに対してボタンのクリックなどのアクションが実行されると、コントロールはコード内でメソッドを自動的に呼び出します。 開発者は多くのコントロールを同じアクションに接続できるため、アクションは強力で便利です。

Xcode では、**Outlet** と **Action** は*コントロールのドラッグ*で直接コードに追加されます。 具体的には、**アウトレット**または**アクション**を作成するために、開発者は**アウトレット**または**アクション**を追加するコントロール要素を選択し、キーボードの **Control** キーを押したまま、コントロールをコードに直接ドラッグします。

Xamarin.Mac 開発者にとって、これは開発者が**アウトレット**または**アクション**を作成する Objective-C スタブ ファイル (C# ファイルに対応) にドラッグすることを意味します。 Visual Studio for Mac は、Interface Builder を使用するために生成した shim Xcode プロジェクトの一部として `ViewController.h` というファイルを作成しました。

[![](hello-mac-images/xcode16-sml.png "Xcode でのソースの表示")](hello-mac-images/xcode16.png#lightbox)

このスタブ `.h` ファイルは、新しい `NSWindow` が作成されたときに自動的に Xamarin.Mac プロジェクトに追加される `ViewController.designer.cs` を反映しています。 このファイルは、Interface Builder によって行われた変更を同期するためにも使用されます。そしてこのファイルで **Outlet** と **Action** が作成され、UI 要素が C# コードに公開されます。

#### <a name="adding-an-outlet"></a>アウトレットの追加

**Outlet** と **Action** が何であるかの基本を理解したので、作成したラベルを C# コードに公開する**Outlet** を作成します。

次の手順で行います。

1. Xcode の画面の右上の端にある**二連の輪**のボタンをクリックして、**アシスタント エディター**を開きます。

    [![](hello-mac-images/outlet01.png "アシスタント エディターを表示する")](hello-mac-images/outlet01.png#lightbox)

2. Xcode が分割ビュー モードに切り替わり、一方の側に**インターフェイス エディター**、もう一方の側に**コード エディター**が表示されます。

3. Xcode により**コード エディター**で自動的に **ViewController.m** ファイルが選択されたことに注意してください。これは間違っています。 前述の **Outlet** と **Action** とは何かについての説明のように、開発者は **ViewController.h** を選択する必要があります。

4. **コード エディター**の上部で、 **[自動リンク]** をクリックし、`ViewController.h` ファイルを選択します。

    [![](hello-mac-images/outlet02.png "正しいファイルを選択する")](hello-mac-images/outlet02.png#lightbox)

5. Xcode で正しいファイルが選択されます。

    [![](hello-mac-images/outlet03.png "ViewController.h ファイルを表示する")](hello-mac-images/outlet03.png#lightbox)

6. **最後のステップは非常に重要です。** 正しいファイルを選択しなかった場合、**Outlet** と **Action** を作成することができなくなるか、またはそれらが C# の間違ったクラスに公開されます。

7. **インターフェイス エディター**で、キーボードの **Control** キーを押しながら、前述の手順で作成したラベルをクリックしてコード エディターの `@interface ViewController : NSViewController {}` コードのすぐ下にドラッグします。

    [![](hello-mac-images/outlet04.png "ドラッグしてアウトレットを作成する")](hello-mac-images/outlet04.png#lightbox)

8. ダイアログ ボックスが表示されます。 **[接続]** を **[アウトレット]** に設定したままにして、 **[名前]** に「`ClickedLabel`」と入力します。

    [![](hello-mac-images/outlet05.png "アウトレットを定義する")](hello-mac-images/outlet05.png#lightbox)

9. **[接続]** ボタンをクリックして**アウトレット**を作成します。

    [![](hello-mac-images/outlet06.png "最終アウトレットを表示する")](hello-mac-images/outlet06.png#lightbox)

10. 変更内容をファイルに保存します。

#### <a name="adding-an-action"></a>アクションの追加

次に、ボタンを C# コードに公開します。 上記のラベルと同じように、開発者はボタンを**アウトレット**に接続できます。 クリックされたボタンにのみ応答するようにしたいので、代わりに**アクション**を使用します。

次の手順で行います。

1. Xcode で**アシスタント エディター**がまだ開いていて、**ViewController.h** ファイルが**コード エディター**に表示されていることを確認します。

2. **インターフェイス エディター**で、キーボードの **Control** キーを押しながら、前述の手順で作成したボタンをクリックしてコード エディターの `@property (assign) IBOutlet NSTextField *ClickedLabel;` コードのすぐ下にドラッグします。

    [![](hello-mac-images/action01.png "ドラッグしてアクションを作成する")](hello-mac-images/action01.png#lightbox)

3. **[接続]** の種類を **[アクション]** に変更します。

    [![](hello-mac-images/action02.png "アクションを定義する")](hello-mac-images/action02.png#lightbox)

4. **[名前]** に「`ClickedButton`」を入力します。

    [![](hello-mac-images/action03.png "新しいアクションの名前を付ける")](hello-mac-images/action03.png#lightbox)

5. **[接続]** ボタンをクリックして**アクション**を作成します。

    [![](hello-mac-images/action04.png "最終アクションを表示する")](hello-mac-images/action04.png#lightbox)

6. 変更内容をファイルに保存します。

ユーザー インターフェイスを接続して C# コードに公開したので、Visual Studio for Mac に戻り、Xcode と Interface Builder で行った変更を同期させます。

> [!NOTE]
> この最初のアプリのユーザー インターフェイスおよび **Outlet** と **Action** を作成するには時間がかかったことでしょう。作業量が多いように思われるかもしれませんが、多くの新しい概念が導入されているため、時間をかけて新しい作業方法を紹介しました。 Interface Builder を使用してしばらく練習すれば、このインターフェイスおよびすべての **Outlet** と **Action** をわずか 1、2 分で作成できるようになります。

### <a name="synchronizing-changes-with-xcode"></a>Xcode との変更の同期

開発者が Xcode から Visual Studio for Mac に戻ると、Xcode で行った変更は自動的に Xamarin.Mac プロジェクトと同期されます。

**ソリューション エクスプローラー**で **ViewController.designer.cs** を選択して、**Outlet** と **Action** が C# コードでどのように接続されているかを確認します。

[![](hello-mac-images/sync01-sml.png "Xcode との変更の同期")](hello-mac-images/sync01.png#lightbox)

**ViewController.designer.cs** ファイル内の 2 つの定義がどのようになっているかに注目してください。

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Xcode の `ViewController.h` ファイルの定義と比べてみましょう。

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Visual Studio for Mac は **.h** ファイルの変更をリッスンし、それぞれの **.designer.cs** ファイルでこれらの変更を自動的に同期して、アプリに公開します。 **ViewController.designer.cs** が部分クラスであるため、Visual Studio for Mac は、開発者がクラスに対して行った変更を上書きする **ViewController.cs** を変更する必要がないことに注意してください。

通常、開発者は **ViewController.designer.cs** を開く必要はありませんが、ここでは学習の目的のためだけに示しています。

> [!NOTE]
> ほとんどの場合、Visual Studio for Mac は Xcode で行われた変更を自動的に確認し、Xamarin.Mac プロジェクトに同期します。 同期が自動的に行われないときは Xcode に戻り、再び Visual Studio for Mac に戻ります。 こうすると通常、同期サイクルが開始します。

## <a name="writing-the-code"></a>コードの記述

ユーザー インターフェイスが作成され、UI 要素が **Outlet** と **Action** を介してコードに公開されたので、最終的にプログラムを実現するためのコードを書く準備が整いました。

このサンプル アプリでは、最初のボタンをクリックするたびにラベルが更新され、ボタンがクリックされた回数が表示されます。 これを行うには、**ソリューション エクスプローラー**で `ViewController.cs` ファイルをダブルクリックして編集用に開きます。

[![](hello-mac-images/code01-sml.png "Visual Studio for Mac で ViewController.cs ファイルを表示する")](hello-mac-images/code01.png#lightbox)

まず、`ViewController` クラスにクラスレベル変数を作成して、発生したクリックの回数を記録します。 クラス定義を編集し、次のようにします。

```csharp
namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

次に、同じクラス (`ViewController`) で、`ViewDidLoad` メソッドをオーバーライドし、ラベルの初期メッセージを設定するコードを追加します。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

`ViewDidLoad` (`Initialize` などの別のメソッドではなく) を使用します。`ViewDidLoad` は、OS が読み込まれ、 **.storyboard** ファイルからユーザー インターフェイスがインスタンス化された*後*に呼び出されるためです。 **.storyboard** ファイルが完全に読み込まれてインスタンス化される前に開発者がラベル コントロールにアクセスしようとすると、ラベル コントロールがまだ存在しないため `NullReferenceException` エラーが発生します。

次に、ボタンをクリックしたユーザーに応答するコードを追加します。 次の部分メソッドを `ViewController` クラスに追加します。

```csharp
partial void ClickedButton (Foundation.NSObject sender) {
    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

このコードは、Xcode と Interface Builder で作成された**アクション**にアタッチされ、ユーザーがボタンをクリックするたびに呼び出されます。

## <a name="testing-the-application"></a>アプリケーションのテスト

アプリをビルドして実行し、期待どおりに動作することを確認します。 開発者は 1 つのステップですべてをビルドして実行することも、実行せずビルドすることもできます。

アプリのビルド時、開発者はビルドの種類を選択できます。

- **デバッグ** – デバッグ ビルドは、アプリの実行中に何が起きているのかを開発者がデバッグすることを可能にする追加のメタデータを含む **.app** (アプリケーション) ファイルにコンパイルされます。
- **リリース** – リリース ビルドでは、 **.app** ファイルも作成されますが、デバッグ情報は含まれていないためサイズが小さく、高速に実行されます。

開発者は、Visual Studio for Mac 画面の左上隅にある**構成セレクター**からビルドの種類を選択できます。

[![](hello-mac-images/run01-sml.png "デバッグ ビルドを選択する")](hello-mac-images/run01.png#lightbox)

## <a name="building-the-application"></a>アプリケーションのビルド

この例ではデバッグ ビルドで十分なので、 **[デバッグ]** が選択されていることを確認してください。 **⌘B** を押すか、 **[ビルド]** メニューの **[すべてビルド]** を選択して、最初にアプリをビルドします。

エラーがなければ、Visual Studio for Mac のステータス バーに "**ビルドが成功しました**" というメッセージが表示されます。 エラーがあった場合はプロジェクトを見直し、上記の手順に正しく従っていることを確認してください。 まず、コード (Xcode と Visual Studio for Mac の両方) がチュートリアルのコードと一致していることを確認します。

## <a name="running-the-application"></a>アプリケーションの実行

アプリを実行するには次の 3 つの方法があります。

- **⌘+Enter** を押します。
- **[実行]** メニューの **[デバッグ]** を選択します。
- Visual Studio for Mac ツール バー (**ソリューション エクスプローラー**のすぐ上) の **[再生]** ボタンをクリックします。

アプリはビルド (まだビルドされていない場合)、デバッグ モードで起動し、メイン インターフェイス ウィンドウを表示します。

[![](hello-mac-images/run02-sml.png "アプリケーションの実行")](hello-mac-images/run02.png#lightbox)

ボタンを何回かクリックすると、ラベルのカウントが更新されます。

[![](hello-mac-images/run03-sml.png "ボタンをクリックした結果の表示")](hello-mac-images/run03.png#lightbox)

## <a name="where-to-next"></a>次の場所

Xamarin.Mac アプリケーションの操作の基礎をより深く理解するために、次のドキュメントを参照してください。

- [Introduction to Storyboards (ストーリーボードの概要)](~/mac/platform/storyboards/index.md) - この記事では、Xamarin.Mac アプリでのストーリーボードの使用について紹介しています。 ストーリーボードと Xcode の Interface Builder を使用したアプリの UI の作成と維持管理に関する内容が含まれています。
- [Window (ウィンドウ)](~/mac/user-interface/window.md) - この記事では、Xamarin.Mac アプリケーションでのウィンドウとパネルの使用について説明しています。 Xcode とインターフェイス ビルダーでのウィンドウとパネルの作成と維持管理、.xib ファイルからのウィンドウとパネルの読み込み、C# コードでのウィンドウの使用とウィンドウへの応答に関する内容が含まれています。
- [Dialogs (ダイアログ)](~/mac/user-interface/dialog.md) - この記事では、Xamarin.Mac アプリケーションでのダイアログとモーダル ウィンドウの使用について説明しています。 Xcode とインターフェイス ビルダーでのモーダル ウィンドウの作成と維持管理、標準ダイアログの使用、C# コードでのウィンドウの表示とウィンドウへの応答に関する内容が含まれています。
- [Alerts (アラート)](~/mac/user-interface/alert.md) - この記事では、Xamarin.Mac アプリケーションでのアラートの使用について説明しています。 C# コードからのアラートの作成と表示、アラートへの応答に関する内容が含まれています。
- [Menus (メニュー)](~/mac/user-interface/menu.md) - メニューは、Mac アプリケーションのユーザー インターフェイスのさまざまな部分で使用されます (画面上部にあるアプリケーションのメイン メニューから、ウィンドウ内のどこにでも表示されるポップアップ メニューとコンテキスト メニューまで)。 メニューは、Mac アプリケーションのユーザー エクスペリエンスにとって不可欠な部分です。 この記事では、Xamarin.Mac アプリケーションでの Cocoa メニューの使用について説明しています。
- [Toolbars (ツール バー)](~/mac/user-interface/toolbar.md) - この記事では、Xamarin.Mac アプリケーションでのツール バーの使用について説明しています。 ここでは作成と維持について説明します。 Xcode と Interface Builder でのツール バー、Outlet と Action を使用してツール バー項目をコードに公開する方法、ツール バー項目の有効化と無効化、最後に C# コードでのツール バー項目への応答に関する内容が含まれています。
- [Table Views (テーブル ビュー)](~/mac/user-interface/table-view.md) - この記事では、Xamarin.Mac アプリケーションでのテーブル ビューの使用について説明しています。 Xcode とインターフェイス ビルダーでのテーブル ビューの作成と維持管理、Outlet と Action を使用してテーブル ビュー項目をコードに公開する方法、テーブル ビュー項目の設定、最後に C# コードでのテーブル ビュー項目への応答に関する内容が含まれています。
- [Outline View (アウトライン ビュー)](~/mac/user-interface/outline-view.md) - この記事では、Xamarin.Mac アプリケーションでのアウトライン ビューの使用について説明しています。 Xcode とインターフェイス ビルダーでのアウトライン ビューの作成と維持管理、Outlet と Action を使用してアウトライン ビュー項目をコードに公開する方法、アウトライン ビュー項目の設定、最後に C# コードでのアウトライン ビュー項目への応答に関する内容が含まれています。
- [Source Lists (ソース リスト)](~/mac/user-interface/source-list.md) - この記事では、Xamarin.Mac アプリケーションでのソース リストの使用について説明しています。 Xcode とインターフェイス ビルダーでのソース リストの作成と維持管理、Outlet と Action を使用してソース リスト項目をコードに公開する方法、ソース リスト項目の設定、最後に C# コードでのソース リスト項目への応答に関する内容が含まれています。
- [Collection Views (コレクション ビュー)](~/mac/user-interface/collection-view.md) - この記事では、Xamarin.Mac アプリケーションでのコレクション ビューの使用について説明しています。 Xcode とインターフェイス ビルダーでのコレクション ビューの作成と維持管理、Outlet と Action を使用してコレクション ビュー項目をコードに公開する方法、コレクション ビューの設定、最後に C# コードでのコレクション ビューへの応答に関する内容が含まれています。
- [イメージの処理](~/mac/app-fundamentals/image.md) - この記事では、Xamarin.Mac アプリケーションでの画像の使用について説明しています。 アプリのアイコンを作成するために必要な画像の作成と維持管理、C# コードと Xcode の Interface Builder の両方での画像の使用に関する内容が含まれています。

[Mac サンプル ギャラリー](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)には、Xamarin.Mac の学習に役立つすぐに使用できるコード例が含まれています。

ユーザーが一般的な Mac アプリケーションに期待する機能を多数含んでいる完全な Xamarin.Mac アプリの 1 つが、[SourceWriter サンプル アプリ](https://docs.microsoft.com/samples/xamarin/mac-samples/sourcewriter)です。 SourceWriter は、コードの完了とシンプルな構文の強調表示をサポートするシンプルなソース コード エディターです。

SourceWriter のコード全体に詳細なコメントが付いており、また利用可能な場合には、重要な技術やメソッドから Xamarin.Mac ドキュメントの関連情報まで、さまざまなリンクが用意されています。

## <a name="summary"></a>まとめ

この記事では、標準的な Xamarin.Mac アプリの基本について説明しています。 Visual Studio for Mac での新しいアプリの作成、Xcode と Interface Builder でのユーザー インターフェイスのデザイン、**Outlet** と **Action** を使用して UI 要素を C# コードに公開する方法、UI 要素と連携するようにコードを追加する方法、最後に Xamarin.Mac アプリのビルドとテストに関する内容が含まれます。

## <a name="related-links"></a>関連リンク

- [Hello Mac (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/hello-mac)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
