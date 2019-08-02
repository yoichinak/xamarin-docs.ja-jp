---
title: xib ファイル (Xamarin. Mac)
description: この記事では、Xcode の Interface Builder で作成された xib ファイルを使用して、Xamarin アプリケーションのユーザーインターフェイスを作成および管理する方法について説明します。
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 4878f2516e764f3ea01ac89569ab4dfdbdbd37e6
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647574"
---
# <a name="xib-files-in-xamarinmac"></a>xib ファイル (Xamarin. Mac)

_この記事では、Xcode の Interface Builder で作成された xib ファイルを使用して、Xamarin アプリケーションのユーザーインターフェイスを作成および管理する方法について説明します。_

> [!NOTE]
> Xamarin. Mac アプリ用のユーザーインターフェイスを作成するには、ストーリーボードを使用することをお勧めします。 このドキュメントは、歴史的な理由や、古い Xamarin. Mac プロジェクトを操作するために残されています。 詳細については、[ストーリーボードの概要に](~/mac/platform/storyboards/index.md)関するドキュメントを参照してください。

## <a name="overview"></a>概要

同じユーザー インターフェイス要素にアクセスし、ツールで作業する開発者 Xamarin.Mac アプリケーションでは、C# と .NET で作業するとき*Objective-C*と*Xcode*はします。 Xcode は直接統合されているため、Xcode の_Interface Builder_を使用してユーザーインターフェイスを作成および管理できます (また、必要C#に応じて、コード内で直接作成することもできます)。

Xib ファイルは、Xcode の Interface Builder でグラフィカルに作成および維持される、アプリケーションのユーザーインターフェイス (メニュー、ウィンドウ、ビュー、ラベル、テキストフィールドなど) の要素を定義するために macOS によって使用されます。

[![実行中のアプリの例](xib-images/intro01.png "実行中のアプリの例")](xib-images/intro01-large.png#lightbox)

この記事では、xib ファイルを Xamarin. Mac アプリケーションで操作するための基本について説明します。 この記事で使用する主要な概念と手法について説明しているため、最初に[Hello, Mac](~/mac/get-started/hello-mac.md)の記事を通じて作業することを強くお勧めします。

確認することも、 [C# を公開するクラス/Objective-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性ネットワーク上での C# クラスを Objective-C オブジェクトと UI への要素に使用されます。


## <a name="introduction-to-xcode-and-interface-builder"></a>Xcode と Interface Builder の概要

Xcode の一部として、Apple は Interface Builder というツールを作成しました。これにより、デザイナーでユーザーインターフェイスを視覚的に作成できます。 Xamarin.MacはInterface Builderとうまく統合されており、Objective-Cユーザーと同じツールでUIを作成することができます。


### <a name="components-of-xcode"></a>Xcode のコンポーネント

Visual Studio for Mac から Xcode で xib ファイルを開くと、左側に**プロジェクトナビゲーター** 、中央に**インターフェイス階層**と**インターフェイスエディター** 、右側に **[プロパティ & ユーティリティ]** セクションが表示されます。

[![XCODE UI のコンポーネント](xib-images/xcode03.png "XCODE UI のコンポーネント")](xib-images/xcode03-large.png#lightbox)

これらの Xcode の各セクションの内容と、それらを使用して Xamarin. Mac アプリケーションのインターフェイスを作成する方法を見てみましょう。


#### <a name="project-navigation"></a>プロジェクトのナビゲーション

Xcode で編集するために xib ファイルを開くと、Visual Studio for Mac は、自身と Xcode の間で変更を通知するために、バックグラウンドで Xcode プロジェクトファイルを作成します。 その後、Xcode から Visual Studio for Mac に戻ると、このプロジェクトに加えられたすべての変更が Visual Studio for Mac によって Xamarin. Mac プロジェクトと同期されます。

**[プロジェクトのナビゲーション]** セクションでは、この_shim_ Xcode プロジェクトを構成するすべてのファイル間を移動できます。 通常、必要なのは、 **xib**や**mainwindow.xaml**など、この一覧の xib ファイルのみです。


#### <a name="interface-hierarchy"></a>インターフェイス階層

**[インターフェイス階層]** セクションでは、**プレースホルダー**やメイン**ウィンドウ**など、ユーザーインターフェイスのいくつかの主要なプロパティに簡単にアクセスできます。 また、このセクションを使用して、ユーザーインターフェイスを構成する個々の要素 (ビュー) にアクセスしたり、階層内でドラッグして入れ子にする方法を調整したりすることもできます。


#### <a name="interface-editor"></a>インターフェイスエディター

**インターフェイスエディター**のセクションには、ユーザーインターフェイスをグラフィカルにレイアウトするためのサーフェイスが用意されています。 **[プロパティ & ユーティリティ]** セクションの **[ライブラリ]** セクションから要素をドラッグして、デザインを作成します。 ユーザーインターフェイス要素 (ビュー) をデザインサーフェイスに追加すると、インターフェイス**エディター**に表示される順序で**インターフェイス階層**セクションに追加されます。


#### <a name="properties--utilities"></a>ユーティリティ & プロパティ

**[プロパティ & ユーティリティ]** セクションは、作業する2つの主要なセクション、**プロパティ**(インスペクターとも呼ばれます)、および**ライブラリ**に分かれています。

![プロパティインスペクター](xib-images/xcode04.png "プロパティインスペクター")

初期状態では、このセクションはほとんど空ですが、**インターフェイスエディター**または**インターフェイス階層**で要素を選択すると、 **[プロパティ]** セクションに、指定された要素とプロパティに関する情報が設定されます。補正.

**プロパティ** セクションには、次の図に示すように 8 つの異なる*インスペクター タブ*があります。

[![すべてのインスペクターの概要](xib-images/xcode05.png "すべてのインスペクターの概要")](xib-images/xcode05-large.png#lightbox)

左から順に次のタブがあります。

-   **ファイル インスペクター** – ファイル インスペクターは編集中の Xib ファイルの場所やファイル名などのファイル情報を示します。
-   **クイック ヘルプ** – クイック ヘルプでは、Xcode での選択内容に基づいたヘルプが表示されます。
-   **ID インスペクター** – ID インスペクターでは、選択したコントロールまたはビューに関する情報が表示されます。
-   **属性インスペクター** –属性インスペクターを使用すると、選択したコントロール/ビューのさまざまな属性をカスタマイズできます。
-   **サイズインスペクター** –サイズインスペクターを使用すると、選択したコントロール/ビューのサイズとサイズ変更動作を制御できます。
-   **接続インスペクター** –接続インスペクターには、選択したコントロールのアウトレットとアクションの接続が表示されます。 ここでは、すぐにアウトレットとアクションを確認します。
-   **バインドインスペクター** –バインドインスペクターを使用すると、コントロールの値がデータモデルに自動的にバインドされるようにコントロールを構成できます。
-   **ビュー効果インスペクター** – [ビュー効果] インスペクターを使用すると、アニメーションなどのコントロールに効果を指定できます。

**[ライブラリ]** セクションでは、デザイナーに配置するコントロールとオブジェクトを見つけて、ユーザーインターフェイスをグラフィカルに作成できます。

![ライブラリインスペクターの例](xib-images/xcode06.png "ライブラリインスペクターの例")

Xcode IDE と Interface Builder について理解したところで、それを使用してユーザーインターフェイスを作成する方法を見てみましょう。


## <a name="creating-and-maintaining-windows-in-xcode"></a>Xcode でのウィンドウの作成と保守

Xamarin アプリのユーザーインターフェイスを作成するには、ストーリーボードを使用することをお勧めします (詳細については、[ストーリーボードの概要に](~/mac/platform/storyboards/index.md)関するドキュメントを参照してください)。その結果、xamarin で開始された新しいプロジェクトは、ストーリーボードを使用します。標準.

Xib ベースの UI を使用するように切り替えるには、次の手順を実行します。

1. Visual Studio for Mac を開き、新しい Xamarin. Mac プロジェクトを開始します。
2. **Solution Pad**で、プロジェクトを右クリックし、[新しいファイルの**追加** >  **...** ] を選択します。
3. **Mac** > **Windows コントローラー**の選択:

    ![新しいウィンドウコントローラーの追加](xib-images/setup00.png "新しいウィンドウコントローラーの追加")
4. 名前`MainWindow`として「」と入力し、 **[新規]** ボタンをクリックします。

    ![新しいメインウィンドウの追加](xib-images/setup01.png "新しいメインウィンドウの追加")
5. プロジェクトをもう一度右クリックし、[**新しいファイル**の**追加** > ] を選択します。
6. **Mac** > **メインメニュー**の選択:

    ![新しいメインメニューの追加](xib-images/setup02.png "新しいメインメニューの追加")
7. 名前はそのまま`MainMenu`にして、 **[新規作成]** ボタンをクリックします。
8. **Solution Pad** **メインのストーリーボード**ファイルを選択し、右クリックして **[削除]** を選択します。

    ![メインストーリーボードの選択](xib-images/setup03.png "メインストーリーボードの選択")
9. 削除 ダイアログボックスで、**削除** ボタンをクリックします。

    ![削除の確認](xib-images/setup04.png "削除の確認")
10. **Solution Pad**で **、ファイルを**ダブルクリックして編集用に開きます。
11. `MainMenu` **メインインターフェイス**のドロップダウンから、次のように選択します。

    [![メインメニューの設定](xib-images/setup05.png "メインメニューの設定")](xib-images/setup05-large.png#lightbox)
12. **Solution Pad**で、 **xib**ファイルをダブルクリックして、Xcode の Interface Builder で編集するために開きます。
13. **ライブラリインスペクター**で、検索フィールド`object`に「」と入力し、新しい**オブジェクト**をデザイン画面にドラッグします。

    [![メインメニューの編集](xib-images/setup06.png "メインメニューの編集")](xib-images/setup06-large.png#lightbox)
14. **Id インスペクター**で、**クラス**と`AppDelegate`して「」と入力します。

    [![アプリのデリゲートを選択する](xib-images/setup07.png "アプリのデリゲートを選択する")](xib-images/setup07-large.png#lightbox)
15. **インターフェイス階層**から**ファイルの所有者**を選択し、**接続インスペクター**に切り替えて、プロジェクトに追加したばかり`AppDelegate`の**オブジェクト**にデリゲートの行をドラッグします。

    [![アプリデリゲートの接続](xib-images/setup08.png "アプリデリゲートの接続")](xib-images/setup08-large.png#lightbox)
16. 変更を保存し、for Mac を Visual Studio に戻る

これらすべての変更にでは、編集、 **AppDelegate.cs**ファイルし、次のように表示します。

```csharp
using AppKit;
using Foundation;

namespace MacXib
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public MainWindowController mainWindowController { get; set; }

        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
            mainWindowController = new MainWindowController ();
            mainWindowController.Window.MakeKeyAndOrderFront (this);
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

これで、アプリのメインウィンドウは、ウィンドウコントローラーを追加するときにプロジェクトに自動的に含まれる xib ファイルで定義され**ます**。 Windows のデザインを編集するには、 **Solution Pad**で、 **mainwindow.xaml xib**ファイルをダブルクリックします。

![Mainwindow.xaml xib ファイルの選択](xib-images/edit01.png "Mainwindow.xaml xib ファイルの選択")

これにより、Xcode の Interface Builder でウィンドウのデザインが開きます。

[![Mainwindow.xaml を編集しています。 xib](xib-images/edit02.png "Mainwindow.xaml を編集しています。 xib")](xib-images/edit02-large.png#lightbox)


### <a name="standard-window-workflow"></a>標準ウィンドウワークフロー

Xamarin. Mac アプリケーションで作成および操作するウィンドウについては、基本的にプロセスは同じです。

1. プロジェクトに自動的に追加されない新しいウィンドウの場合は、新しいウィンドウ定義をプロジェクトに追加します。
2. Xib ファイルをダブルクリックして、Xcode の Interface Builder で編集するためのウィンドウデザインを開きます。
3. **属性インスペクター**と**サイズインスペクター**で必要なウィンドウプロパティを設定します。
4. インターフェイスを構築するために必要なコントロールをドラッグして、**属性インスペクター**で構成します。
5. **サイズインスペクター**を使用して、UI 要素のサイズ変更を処理します。
6. ウィンドウの UI 要素を、コンセントC#とアクションを使用してコードに公開します。
7. 変更を保存し、Visual Studio for Mac に戻って Xcode と同期します。


### <a name="designing-a-window-layout"></a>ウィンドウレイアウトのデザイン

インターフェイスビルダーでユーザーインターフェイスをレイアウトするプロセスは、基本的に、追加するすべての要素に対して同じです。

1. **ライブラリインスペクター**で目的のコントロールを検索し、それを**インターフェイスエディター**にドラッグして配置します。
2. **属性インスペクター**で必要なウィンドウプロパティを設定します。
3. **サイズインスペクター**を使用して、UI 要素のサイズ変更を処理します。
4. カスタムクラスを使用している場合は、 **Id インスペクター**で設定します。
5. UI 要素を、コンセントC#とアクションを使用してコードに公開します。
6. 変更を保存し、Visual Studio for Mac に戻って Xcode と同期します。

たとえば、次のように入力します。

1. Xcode で、**ライブラリ** セクションから**ボタン**をドラッグします。

    [![ライブラリからのボタンの選択](xib-images/xcode07.png "ライブラリからのボタンの選択")](xib-images/xcode07-large.png#lightbox)
2. **インターフェイスエディター**で、ボタンを**ウィンドウ**にドロップします。

    [![ウィンドウへのボタンの追加](xib-images/xcode08.png "ウィンドウへのボタンの追加")](xib-images/xcode08-large.png#lightbox)
3. **属性インスペクター**の **Title** プロパティをクリックし、ボタンのタイトルを「`Click Me`」に変更します。

    ![ボタン属性の設定](xib-images/xcode09.png "ボタン属性の設定")
4. **ライブラリ セクション**から**ラベル**をドラッグします。

    [![ライブラリ内のラベルの選択](xib-images/xcode10.png "ライブラリ内のラベルの選択")](xib-images/xcode10-large.png#lightbox)
5. **Interface Editor** のボタンの横にある**ウィンドウ**にラベルをドラッグします。

    [![ウィンドウへのラベルの追加](xib-images/xcode11.png "ウィンドウへのラベルの追加")](xib-images/xcode11-large.png#lightbox)
6. ラベルの右ハンドルをつかみ、ウィンドウの端に近づくまでドラッグします。

    [![ラベルのサイズを変更する](xib-images/xcode12.png "ラベルのサイズを変更する")](xib-images/xcode12-large.png#lightbox)
7. **インターフェイスエディター**でラベルを選択した状態で、**サイズインスペクター**に切り替えます。

    ![サイズインスペクターを選択する](xib-images/xcode13.png "サイズインスペクターを選択する")
8. [**自動サイズ調整] ボックス**で、右側にある**Dim 赤色の角かっこ**をクリックし、中央の**暗い横線の矢印**をクリックします。

    ![自動サイズ調整のプロパティの編集](xib-images/xcode14.png "自動サイズ調整のプロパティの編集")
9. これにより、実行中のアプリケーションでウィンドウのサイズが変更されたときに、ラベルが拡大したり縮小したりすることが保証されます。 **赤色の角かっこ**と **[自動サイズ調整 box]** ボックスの左上には、ラベルが指定された X と Y の場所にスタックされるように指示します。
10. ユーザーインターフェイスへの変更を保存します。

コントロールのサイズ変更や移動を行っているときに、 [OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)に基づいた便利なスナップヒントが Interface Builder に提供されていることに注意してください。 これらのガイドラインは、Mac ユーザーの使い慣れたルックアンドフィールを備えた高品質のアプリケーションを作成するのに役立ちます。

**[インターフェイス階層]** セクションを確認すると、ユーザーインターフェイスを構成する要素のレイアウトと階層がどのように表示されるかがわかります。

![インターフェイス階層内の要素の選択](xib-images/xcode15.png "インターフェイス階層内の要素の選択")

ここから、必要に応じて、編集またはドラッグする項目を選択して、UI 要素を並べ替えることができます。 たとえば、UI 要素が別の要素によってカバーされていた場合は、それをリストの一番下にドラッグして、ウィンドウ上の一番上の項目にすることができます。

Xamarin. Mac アプリケーションで Windows を操作する方法の詳細については、 [windows](~/mac/user-interface/window.md)のドキュメントを参照してください。


## <a name="exposing-ui-elements-to-c-code"></a>公開 (UI 要素C#をコードに)

Interface Builder でユーザーインターフェイスのルックアンドフィールをレイアウトしたら、UI の要素を公開して、コードからC#アクセスできるようにする必要があります。 これを行うには、アクションとアウトレットを使用します。


### <a name="setting-a-custom-main-window-controller"></a>カスタムメインウィンドウコントローラーの設定

UI 要素をコードにC#公開するためのアウトレットとアクションを作成できるようにするには、Xamarin. Mac アプリでカスタムウィンドウコントローラーを使用する必要があります。

次の手順で行います。

1. Xcode の Interface Builder でアプリのストーリーボードを開きます。
2. `NSWindowController`デザインサーフェイスでを選択します。
3. **[Identity Inspector]** ビューに切り替え、 `WindowController` **クラス名**として「」と入力します。

    [![クラス名の編集](xib-images/windowcontroller01.png "クラス名の編集")](xib-images/windowcontroller01-large.png#lightbox)
4. 変更を保存し、Visual Studio for Mac に戻って同期します。
5. Visual Studio for Mac の**Solution Pad**に、 **WindowController.cs**ファイルがプロジェクトに追加されます。

    ![の新しいクラス名 Visual Studio for Mac](xib-images/windowcontroller02.png "の新しいクラス名 Visual Studio for Mac")
6. Xcode の Interface Builder でストーリーボードを再度開きます。
7. **Windowcontroller .h**ファイルは、次のように使用できます。

    [![Xcode の対応する .h ファイル](xib-images/windowcontroller03.png "Xcode の対応する .h ファイル")](xib-images/windowcontroller03-large.png#lightbox)


### <a name="outlets-and-actions"></a>アウトレットとアクション

では、アウトレットとアクションとは何ですか。 従来の .NET ユーザー インターフェイス プログラミングでは、ユーザー インターフェイスのコントロールは追加時にプロパティとして自動的に公開されます。 Mac では事情が異なり、ビューにコントロールを追加しただけでは、コントロールはコードにアクセスできません。 開発者は、UI 要素を明示的にコードに公開する必要があります。 これを行うために、Apple は2つのオプションを提供しています。

-  **Outlet**  – Outlet はプロパティに似ています。 コントロールをコンセントに接続すると、プロパティを使用してコードに公開されます。これにより、イベントハンドラーのアタッチ、メソッドの呼び出しなどの操作を実行できます。
-  **アクション** – アクションは WPF のコマンド パターンに似ています。 たとえば、ボタンのクリックなど、コントロールに対してアクションが実行されると、コントロールは自動的にコード内のメソッドを呼び出します。 アクションは、多くのコントロールを同じアクションに接続できるため、強力で便利です。

Xcode では、*コントロールのドラッグ*によって、コンセントとアクションがコードに直接追加されます。 具体的には、アウトレットまたはアクションを作成するには、アウトレットまたはアクションを追加するコントロール要素を選択し、キーボードの**コントロール**ボタンを押したまま、コントロールをコードに直接ドラッグします。

Xamarin.Mac 開発者コンセントまたはアクションを作成する (C#) ファイルに対応する Objective-C スタブ ファイルにドラッグすることを意味します。 Visual Studio for Mac Interface Builder を使用するために生成された shim Xcode プロジェクトの一部として、 **mainwindow.xaml**という名前のファイルを作成しました。

[![Xcode の .h ファイルの例](xib-images/xcode16.png "Xcode の .h ファイルの例")](xib-images/xcode16-large.png#lightbox)

この**MainWindow.designer.cs**ファイルは、新しい`NSWindow`が作成されたときに Xamarin. Mac プロジェクトに自動的に追加されるを反映しています。 このファイルは、Interface Builder によって行われた変更を同期するために使用されます。また、UI 要素がコードにC#公開されるように、アウトレットとアクションを作成します。


#### <a name="adding-an-outlet"></a>アウトレットの追加

アウトレットとアクションについての基本的な理解を深めるために、 C#コードに UI 要素を公開するアウトレットの作成について見てみましょう。

次の手順で行います。

1. Xcode の画面の右上の端にある**二連の輪**のボタンをクリックして、**アシスタント エディター**を開きます。

    [![アシスタントエディターの選択](xib-images/outlet01.png "アシスタントエディターの選択")](xib-images/outlet01-large.png#lightbox)
2. Xcode が分割ビュー モードに切り替わり、一方の側に**インターフェイス エディター**、もう一方の側に**コード エディター**が表示されます。
3. **コードエディター**で**Xcode ファイルが**自動的に選択されたことに注意してください。これは正しくありません。 上記のアウトレットとアクションについての説明を忘れた場合は、 **mainwindow.xaml**を選択する必要があります。
4. **コードエディター**の上部で、[**自動] リンク**をクリックし、 **mainwindow.xaml**ファイルを選択します。

    [![正しい .h ファイルを選択する](xib-images/outlet02.png "正しい .h ファイルを選択する")](xib-images/outlet02-large.png#lightbox)
5. Xcode で正しいファイルが選択されます。

    [![正しいファイルが選択されまし]た(xib-images/outlet03.png "正しいファイルが選択されまし")た](xib-images/outlet03-large.png#lightbox)
6. **最後のステップは非常に重要です!** 正しいファイルが選択されていない場合、コンセントやアクションを作成することはできません。また、でC#間違ったクラスに公開されることもありません。
7. **インターフェイスエディター**で、キーボードの**ctrl**キーを押したまま、前の手順で作成したラベルをコードエディターのコードの`@interface MainWindow : NSWindow { }`すぐ下にドラッグします。

    [![ドラッグして新しいアウトレットを作成する](xib-images/outlet04.png "ドラッグして新しいアウトレットを作成する")](xib-images/outlet04-large.png#lightbox)
8. ダイアログ ボックスが表示されます。 **接続** を アウトレット の`ClickedLabel`ままにして、**名前**として「」と入力します。

    [![アウトレットのプロパティの設定](xib-images/outlet05.png "アウトレットのプロパティの設定")](xib-images/outlet05-large.png#lightbox)
9. **[接続]** ボタンをクリックして、アウトレットを作成します。

    ![完了]したアウトレット(xib-images/outlet06.png "完了")したアウトレット
10. 変更内容をファイルに保存します。


#### <a name="adding-an-action"></a>アクションの追加

次に、UI 要素とのユーザー操作をコードに公開するアクションを作成する方法C#を見てみましょう。

次の手順で行います。

1. まだ**アシスタントエディター**で、 **Mainwindow.xaml**ファイルが**コードエディター**に表示されていることを確認します。
2. **インターフェイスエディター**で、キーボードの**ctrl**キーを押したまま、前の手順で作成したボタンをコードエディターのコードの`@property (assign) IBOutlet NSTextField *ClickedLabel;`すぐ下にドラッグします。

    [![ドラッグしてアクションを作成する](xib-images/action01.png "ドラッグしてアクションを作成する")](xib-images/action01-large.png#lightbox)
3. **接続**の種類を action に変更します。

    [![アクションの種類を選択し]ます(xib-images/action02.png "アクションの種類を選択し")ます](xib-images/action02-large.png#lightbox)
4. **[名前]** に「`ClickedButton`」を入力します。

    [![アクションの構成](xib-images/action03.png "アクションの構成")](xib-images/action03-large.png#lightbox)
5. アクションを作成するには、 **[接続]** ボタンをクリックします。

    ![完了したアクション](xib-images/action04.png "完了したアクション")
6. 変更内容をファイルに保存します。

ユーザーインターフェイスをワイヤードに接続し、コードにC#公開したら Visual Studio for Mac に戻り、Xcode と Interface Builder の変更を同期させます。


### <a name="writing-the-code"></a>コードの記述

ユーザーインターフェイスが作成され、その UI 要素がコンセントとアクションを使用してコードに公開されたので、コードを記述してプログラムを完成させることができます。 たとえば、 **Solution Pad**で**MainWindow.cs**ファイルをダブルクリックして編集するには、次のように開きます。

[![MainWindow.cs ファイル](xib-images/code01.png "MainWindow.cs ファイル")](xib-images/code01-large.png#lightbox)

次のコードを`MainWindow`クラスに追加して、上で作成したサンプルアウトレットを操作します。

```csharp
private int numberOfTimesClicked = 0;
...

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

は、 `NSLabel` Xcode でアウトレットをC#作成したときに Xcode で割り当てたダイレクト名によってにアクセスされることに注意してください。 `ClickedLabel`この場合は、という名前になります。 公開されたオブジェクトのメソッドまたはプロパティには、通常C#のクラスと同じ方法でアクセスできます。

> [!IMPORTANT]
> は、OS が`AwakeFromNib`読み込まれ、xib ファイルからユーザーインターフェイス`AwakeFromNib`がインスタンス化され_た後_に呼び出されるため、などの別のメソッド`Initialize`ではなくを使用する必要があります。 Xib ファイルが完全に読み込まれてインスタンス化される前にラベルコントロールにアクセスしようとすると、ラベル`NullReferenceException`コントロールがまだ作成されていないため、エラーが発生します。

次に、次の部分クラスを`MainWindow`クラスに追加します。

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

このコードは、Xcode および Interface Builder で作成したアクションにアタッチし、ユーザーがボタンをクリックするたびに呼び出されます。

一部の UI 要素には、組み込みのアクションが自動的に組み込まれています。たとえば、 **[開く]** メニュー項目 (`openDocument:`) などの既定のメニューバーの項目があります。 **Solution Pad**で、 **AppDelegate.cs**ファイルをダブルクリックして編集用に開き、メソッドの`DidFinishLaunching`下に次のコードを追加します。

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

ここでの重要な`[Export ("openDocument:")]`のは、 `NSMenu` **appdelegate**が`openDocument:`アクションに応答する`void OpenDialog (NSObject sender)`メソッドを持っていることを示しています。

メニューの操作の詳細については、[メニュー](~/mac/user-interface/menu.md)のドキュメントを参照してください。


## <a name="synchronizing-changes-with-xcode"></a>Xcode との変更の同期

Xcode から Visual Studio for Mac に戻ると、Xcode で行ったすべての変更が自動的に Xamarin. Mac プロジェクトと同期されます。

Solution Pad で**MainWindow.designer.cs**を選択すると 、次のC#コードでアウトレットとアクションがどのようにつながっているかを確認できます。

[![Xcode との変更の同期](xib-images/sync01.png "Xcode との変更の同期")](xib-images/sync01-large.png#lightbox)

**MainWindow.designer.cs**ファイル内の2つの定義があることに注意してください。

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Xcode の**mainwindow.xaml**ファイルの定義で行アップします。

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

ご覧のように、Visual Studio for Mac によって .h ファイルの変更がリッスンされ、 **designer.cs**ファイルでこれらの変更が自動的に同期され、アプリケーションに公開されます。 また、 **MainWindow.designer.cs**が部分クラスであるため、クラスに対して行った変更が上書きされるようにするために、Visual Studio for Mac では**MainWindow.cs**を変更する必要がないことにも注意してください。

通常は、 **MainWindow.designer.cs**を自分で開く必要はありません。ここでは、教育目的でのみ提供されています。

> [!IMPORTANT]
> ほとんどの場合、Visual Studio for Mac は Xcode で行われた変更を自動的に確認し、それらを Xamarin. Mac プロジェクトに同期します。 同期が自動的に行われないときは Xcode に戻り、再び Visual Studio for Mac に戻ります。 こうすると通常、同期サイクルが開始します。


## <a name="adding-a-new-window-to-a-project"></a>プロジェクトへの新しいウィンドウの追加

メインのドキュメントウィンドウとは別に、Xamarin. Mac アプリケーションでは、他の種類のウィンドウをユーザーに表示する必要がある場合があります (ユーザー設定やインスペクターパネルなど)。 新しいウィンドウをプロジェクトに追加するときは、常に**Cocoa ウィンドウとコントローラー**オプションを使用する必要があります。これにより、xib ファイルからウィンドウが読みやすくなります。

新しいウィンドウを追加するには、次の手順を実行します。

1. **Solution Pad**で、プロジェクトを右クリックし、[新しいファイルの**追加** >  **..** .] を選択します。
2. [新しいファイル] ダイアログボックスで **、次の**ように選択し**ます。**  > 

    ![新しいウィンドウコントローラーの追加](xib-images/new01.png "新しいウィンドウコントローラーの追加")
3. **[名前]** に「`PreferencesWindow`」と入力し、 **[新規]** ボタンをクリックします。
4. PreferencesWindow ファイルをダブルクリックして、Interface Builder で編集するために開き**ます。**

    [![Xcode でのウィンドウの編集](xib-images/new02.png "Xcode でのウィンドウの編集")](xib-images/new02-large.png#lightbox)
5. インターフェイスを設計します。

    [![Windows レイアウトのデザイン](xib-images/new03.png "Windows レイアウトのデザイン")](xib-images/new03-large.png#lightbox)
6. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

次のコードを追加**AppDelegate.cs**新しいウィンドウを表示します。

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

この`var preferences = new PreferencesWindowController ();`行は、ウィンドウを xib ファイルから読み込み、増えする、ウィンドウコントローラーの新しいインスタンスを作成します。 線`preferences.Window.MakeKeyAndOrderFront (this);`は、ユーザーに新しいウィンドウを表示します。

コードを実行し、[**アプリケーション] メニュー**の **[ユーザー設定...]** を選択すると、ウィンドウが表示されます。

![サンプルアプリの実行](xib-images/new04.png "サンプルアプリの実行")

Xamarin. Mac アプリケーションで Windows を操作する方法の詳細については、 [windows](~/mac/user-interface/window.md)のドキュメントを参照してください。


## <a name="adding-a-new-view-to-a-project"></a>新しいビューをプロジェクトに追加する

ウィンドウのデザインを、管理しやすい複数の xib ファイルに分割する方が簡単な場合もあります。 たとえば、[**基本設定] ウィンドウ**でツールバー項目を選択したり、**ソースリスト**の選択に応じてコンテンツを交換したりするときに、メインウィンドウの内容を切り替えるとします。

新しいビューをプロジェクトに追加する場合は、常に**Cocoa View With Controller**オプションを使用する必要があります。これにより、xib ファイルからビューを読み込むプロセスが簡単になります。

新しいビューを追加するには、次の手順を実行します。

1. **Solution Pad**で、プロジェクトを右クリックし、[新しいファイルの**追加** >  **..** .] を選択します。
2. [新しいファイル] ダイアログボックスで、[ **Xamarin. Mac** > **cocoa View with Controller**] を選択します。

    ![新しいビューの追加](xib-images/view01.png "新しいビューの追加")
3. **[名前]** に「`SubviewTable`」と入力し、 **[新規]** ボタンをクリックします。
4. **Xib**ファイルをダブルクリックして開き、Interface Builder で編集して、ユーザーインターフェイスをデザインします。

    [![Xcode での新しいビューのデザイン](xib-images/view02.png "Xcode での新しいビューのデザイン")](xib-images/view02-large.png#lightbox)
5. 必要なアクションとアウトレットを接続します。
6. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

次に、 **SubviewTable.cs**を編集し、次のコードを**AwakeFromNib**ファイルに追加して、新しいビューが読み込まれたときに設定します。

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));
    DataSource.Sort ("Title", true);

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);

    // Auto select the first row
    ProductTable.SelectRow (0, false);
}
```

現在表示されているビューを追跡するために、プロジェクトに列挙型を追加します。 たとえば、 **SubviewType.cs**のようになります。

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

ビューを使用して表示するウィンドウの xib ファイルを編集します。 コードによってC#メモリに読み込まれた後、ビューのコンテナーとして機能する`ViewContainer`**カスタムビュー**を追加し、というアウトレットに公開します。

[![必要なアウトレットの作成](xib-images/view03.png "必要なアウトレットの作成")](xib-images/view03-large.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

次に、新しいビューを表示するウィンドウの .cs ファイル (たとえば、 **MainWindow.cs**) を編集し、次のコードを追加します。

```csharp
private SubviewType ViewType = SubviewType.None;
private NSViewController SubviewController = null;
private NSView Subview = null;
...

private void DisplaySubview(NSViewController controller, SubviewType type) {

    // Is this view already displayed?
    if (ViewType == type) return;

    // Is there a view already being displayed?
    if (Subview != null) {
        // Yes, remove it from the view
        Subview.RemoveFromSuperview ();

        // Release memory
        Subview = null;
        SubviewController = null;
    }

    // Save values
    ViewType = type;
    SubviewController = controller;
    Subview = controller.View;

    // Define frame and display
    Subview.Frame = new CGRect (0, 0, ViewContainer.Frame.Width, ViewContainer.Frame.Height);
    ViewContainer.AddSubview (Subview);
}
```

ウィンドウのコンテナー (上に追加した**カスタムビュー** ) の xib ファイルから読み込まれた新しいビューを表示する必要がある場合、このコードは既存のビューの削除を処理し、新しいビューを交換します。 ビューが表示されていることがわかります。表示されている場合は、画面から削除します。 次に、(ビューコントローラーから読み込まれるように) 渡されたビューのサイズを変更して、コンテンツ領域に合わせてサイズを変更し、表示するコンテンツに追加します。

新しいビューを表示するには、次のコードを使用します。

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

これにより、表示する新しいビューのビューコントローラーの新しいインスタンスが作成され、その型 (プロジェクトに追加された列挙型によって指定`DisplaySubview` ) が設定され、ウィンドウのクラスに追加されたメソッドを使用してビューが実際に表示されます。 例えば:

[![サンプルアプリの実行](xib-images/view04.png "サンプルアプリの実行")](xib-images/view04-large.png#lightbox)

Xamarin. Mac アプリケーションで Windows を操作する方法の詳細については、 [windows](~/mac/user-interface/window.md)と[ダイアログ](~/mac/user-interface/dialog.md)のドキュメントを参照してください。


## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションでの xib ファイルの使用について詳しく説明しました。 ここでは、アプリケーションのユーザーインターフェイスを作成するためのさまざまな型と xib ファイルの使用方法、Xcode の Interface Builder で xib ファイルを作成および管理する方法、およびコードでC# xib ファイルを操作する方法について説明しました。


## <a name="related-links"></a>関連リンク

- [MacImages (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [メニュー](~/mac/user-interface/menu.md)
- [ダイアログ](~/mac/user-interface/dialog.md)
- [イメージの操作](~/mac/app-fundamentals/image.md)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
