---
title: .xib ファイル
description: この記事では、Xcode のインターフェイスのビルダーを作成および維持 Xamarin.Mac アプリケーションのユーザー インターフェイスに作成された .xib ファイルと作業について説明します。
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: c1f575f5d3d5f0fbe82d5e0d08103b9261944602
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="xib-files"></a>.xib ファイル

_この記事では、Xcode のインターフェイスのビルダーを作成および維持 Xamarin.Mac アプリケーションのユーザー インターフェイスに作成された .xib ファイルと作業について説明します。_

> [!NOTE]
> ストーリー ボードでは、Xamarin.Mac アプリのユーザー インターフェイスを作成することをお勧めします。 このドキュメントには、履歴上の理由から、古い Xamarin.Mac プロジェクトを操作するため、インプレースが残っています。 詳細についてを参照してください、[ストーリー ボードの概要](~/mac/platform/storyboards/index.md)ドキュメント。

## <a name="overview"></a>概要

同じユーザー インターフェイス要素にアクセスし、ツールで作業する開発者 Xamarin.Mac アプリケーションでは、c# と .NET で作業するとき*Objective-C*と*Xcode*はします。 Xamarin.Mac は、Xcode と直接統合を使用すると Xcode の_インターフェイス ビルダー_を作成し、ユーザー インターフェイスを維持 (または必要に応じて c# コードで直接作成すること)。

Xcode のインターフェイスのビルダーで作成および管理される (メニュー、Windows、ビュー、ラベルのテキスト フィールドなど)、アプリケーションのユーザー インターフェイスの要素をグラフィカルに定義する、macOS によっては、.xib ファイルを使用します。

[![実行中のアプリの使用例](xib-images/intro01.png "実行中のアプリの例")](xib-images/intro01-large.png#lightbox)

この記事でしれません .xib ファイル Xamarin.Mac アプリケーションでの操作の基礎をについて説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)最初に、アーティクルのように主要な概念とこの記事で使用する手法について説明します。

確認することも、 [c# を公開するクラス/Objective-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性ネットワーク上での c# クラスを Objective-C オブジェクトと UI への要素に使用されます。


## <a name="introduction-to-xcode-and-interface-builder"></a>Xcode と Interface Builder の概要

Xcode の一環として、Apple がインターフェイスのビルダーは、デザイナーで、ユーザー インターフェイスを視覚的に作成することができますをというツールを作成します。 Xamarin.MacはInterface Builderとうまく統合されており、Objective-Cユーザーと同じツールでUIを作成することができます。


### <a name="components-of-xcode"></a>Xcode のコンポーネント

ドキュメントが開きます Mac を Visual Studio から Xcode で .xib ファイルを開くときに、**プロジェクト ナビゲーター** 、左側、**インターフェイス階層**と**インターフェイス エディター**の中間に、および**プロパティおよび公益事業**右側のセクション。

[![Xcode UI のコンポーネント](xib-images/xcode03.png "Xcode UI のコンポーネント")](xib-images/xcode03-large.png#lightbox)

見ていきましょうでこれらの各 Xcode のセクションがありを使用する方法に Xamarin.Mac アプリケーションのため、インターフェイスを作成します。


#### <a name="project-navigation"></a>プロジェクトのナビゲーション

Xcode で編集するための .xib ファイルを開くときに、Visual Studio for Mac はそれ自体と Xcode の間での変更を通信するためにバック グラウンドで Xcode プロジェクト ファイルを作成します。 後で、切り替えたときに Visual Studio に戻り for Mac Xcode から、このプロジェクトに加えられた変更はプロジェクトと同期 Xamarin.Mac によって Visual Studio for mac

**プロジェクト ナビゲーション**セクションでは、すべてこれを構成するファイルの間を移動することができます_shim_ Xcode プロジェクト。 通常、のみ対象となるようにこの一覧に .xib ファイルで**MainMenu.xib**と**MainWindow.xib**です。


#### <a name="interface-hierarchy"></a>インターフェイスの階層構造

**インターフェイス階層**セクションでは、簡単になど、ユーザー インターフェイスのいくつかのキー プロパティにアクセスすることができます、**プレース ホルダー**および main**ウィンドウ**します。 また、階層内でドラッグするだけで入れ子にする方法、ユーザー インターフェイスとの調整を構成する個々 の要素 (views) にアクセスするのにこのセクションでを使用することができます。


#### <a name="interface-editor"></a>インターフェイス エディター

**インターフェイス エディター**セクションを画面を提供する視覚的にレイアウト、ユーザー インターフェイスします。 要素をドラッグします、**ライブラリ**のセクションで、**プロパティおよび公益事業**セクションで、デザインを作成します。 それらに追加するユーザー インターフェイス要素 (views) をデザイン画面に追加すると、**インターフェイス階層**セクションに示されている順序で、**インターフェイス エディター**です。


#### <a name="properties--utilities"></a>プロパティおよび公益事業

**プロパティおよび公益事業**セクションでは、取り組んでは 2 つの主要なセクションに分かれています**プロパティ**(インスペクターとも呼ばれます) および**ライブラリ**:

![プロパティ インスペクター](xib-images/xcode04.png "プロパティ インスペクター")

最初にここではほとんど空ただし内の要素を選択した場合、**インターフェイス エディター**または**インターフェイス階層**、**プロパティ**セクションが設定されます指定した要素と調整できるプロパティに関する情報。

**プロパティ** セクションには、次の図に示すように 8 つの異なる*インスペクター タブ*があります。

[![すべてのインスペクターの概要](xib-images/xcode05.png "すべてのインスペクターの概要")](xib-images/xcode05-large.png#lightbox)

左から順に次のタブがあります。

-   **ファイル インスペクター** – ファイル インスペクターは編集中の Xib ファイルの場所やファイル名などのファイル情報を示します。
-   **クイック ヘルプ** – クイック ヘルプでは、Xcode での選択内容に基づいたヘルプが表示されます。
-   **ID インスペクター** – ID インスペクターでは、選択したコントロールまたはビューに関する情報が表示されます。
-   **属性のインスペクター** –、属性の検査では、選択したコントロール/ビューのさまざまな属性をカスタマイズすることができます。
-   **インスペクターをサイズ**–、サイズ Inspector では、サイズおよびサイズ変更、選択したコントロール/ビューの動作を制御することができます。
-   **接続のインスペクター** –「接続インスペクターは、選択したコントロールのコンセントとアクションの接続を示しています。 取り上げるコンセントとアクションもすぐにします。
-   **バインド インスペクター** –「バインド Inspector では、その値はデータ モデル自動的にバインドされるようコントロールを構成することができます。
-   **表示効果インスペクター** –「表示効果 Inspector では、アニメーションなどコントロールへの影響を指定することができます。

**ライブラリ** セクションで、コントロールを見つけることができ、デザイナーをグラフィカルに配置するオブジェクトが、ユーザー インターフェイスを構築します。

![ライブラリ インスペクターの例](xib-images/xcode06.png "ライブラリ インスペクターの例")

Xcode IDE とインターフェイスのビルダーに慣れて、これでは、ユーザー インターフェイスの作成に使用を見てみましょう。


## <a name="creating-and-maintaining-windows-in-xcode"></a>作成し、Xcode での windows の管理

Xamarin.Mac アプリのユーザー インターフェイスを作成するための推奨される方法はストーリー ボード (を参照してください、[ストーリー ボードの概要](~/mac/platform/storyboards/index.md)詳細についてはドキュメント) し、新しいプロジェクトが Xamarin.Mac はで起動された結果として、既定では、ストーリー ボードを使用します。

切り替えるには、.xib を使用してベースの UI は次の操作を行います。

1. Mac 用 Visual Studio を開き、新しい Xamarin.Mac プロジェクトを開始します。
2. **ソリューション パッド**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.**
3. 選択**Mac** > **Windows コント ローラー**:

    ![新しいウィンドウのコント ローラーを追加する](xib-images/setup00.png "新しいウィンドウ コント ローラーの追加")
4. 入力`MainWindow`クリックと名前の**新規**ボタン。

    ![新しいメイン ウィンドウの追加](xib-images/setup01.png "新しいメイン ウィンドウの追加")
5. プロジェクトをもう一度右クリックして**追加** > **新しいファイル.**
6. 選択**Mac** > **メイン メニュー**:

    ![新しいメイン メニューを追加する](xib-images/setup02.png "新しいメイン メニューの追加")
7. として名前をそのまま`MainMenu` をクリックし、**新規**ボタンをクリックします。
8. **ソリューション パッド**選択、 **Main.storyboard**ファイルを右クリックし、**削除**:

    ![メインのストーリー ボードを選択すると](xib-images/setup03.png "メインのストーリー ボードを選択します。")
9. [削除] ダイアログ ボックスで、**削除**ボタンをクリックします。

    ![削除を確認する](xib-images/setup04.png "削除を確認します。")
10. **ソリューション パッド**をダブルクリックして、 **Info.plist**ファイルを開いて編集するファイル。
11. 選択`MainMenu`から、 **Main インターフェイス**ドロップダウンします。

    [![メイン メニューを設定](xib-images/setup05.png "メイン メニューの設定")](xib-images/setup05-large.png#lightbox)
12. **ソリューション パッド**をダブルクリックして、 **MainMenu.xib**編集のため Xcode のインターフェイスのビルダーで開くファイル。
13. **ライブラリ インスペクター**、型`object`検索フィールドに、ドラッグして、新しい**オブジェクト**デザイン サーフェイスに。

    [![メイン メニューの編集](xib-images/setup06.png "メイン メニューの編集")](xib-images/setup06-large.png#lightbox)
14. **Identity インスペクター**、入力`AppDelegate`の**クラス**:

    [![アプリのデリゲートを選択すると](xib-images/setup07.png "アプリ デリゲートを選択します。")](xib-images/setup07-large.png#lightbox)
15. 選択**ファイルの所有者**から、**インターフェイス階層**に切り替え、**接続インスペクター**をデリゲートから線をドラッグし、 `AppDelegate` **オブジェクト**だけがプロジェクトに追加します。

    [![アプリのデリゲートを接続する](xib-images/setup08.png "アプリ デリゲートを接続します。")](xib-images/setup08-large.png#lightbox)
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

アプリのメイン ウィンドウが定義されているので、 **.xib**ファイル ウィンドウのコント ローラーを追加するときに、プロジェクトに自動的に追加します。 Windows のデザインを編集する、**ソリューション パッド**、ダブルクリックして、 **MainWindow.xib**ファイル。

![MainWindow.xib ファイルを選択する](xib-images/edit01.png "MainWindow.xib ファイルを選択します。")

Xcode のインターフェイスのビルダーで、ウィンドウのデザインが開きます。

[![編集、MainWindow.xib](xib-images/edit02.png "MainWindow.xib の編集")](xib-images/edit02-large.png#lightbox)


### <a name="standard-window-workflow"></a>標準的なウィンドウのワークフロー

Xamarin.Mac アプリケーションで使用して作成したウィンドウで、プロセスは基本的に同じです。

1. 新しいウィンドウをプロジェクトに自動的に追加の既定ではありませんが、プロジェクトに新しいウィンドウの定義を追加します。
2. Xcode のインターフェイスのビルダーで編集するためのウィンドウのデザインを開き、.xib ファイルをダブルクリックします。
3. 必要なウィンドウのプロパティを設定、**属性インスペクター**と**サイズ インスペクター**です。
4. インターフェイスを構築および構成に必要なコントロールでドラッグして、**属性インスペクター**です。
5. 使用して、**サイズ インスペクター** UI 要素のサイズ変更を処理します。
6. C# コンセントとアクションを使用してコード ウィンドウの UI 要素を公開します。
7. 変更を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。


### <a name="designing-a-window-layout"></a>ウィンドウ レイアウトのデザイン

インターフェイスのビルダーでのユーザー インターフェイス レイアウトに使用するプロセスは、基本的を追加するすべての要素に対して同じです。

1. 目的のコントロールを見つけ、**ライブラリ インスペクター**にドラッグし、**インターフェイス エディター**に配置します。
2. 必要なウィンドウのプロパティを設定、**属性インスペクター**です。
3. 使用して、**サイズ インスペクター** UI 要素のサイズ変更を処理します。
4. カスタム クラスを使用している場合は設定、 **Identity インスペクター**です。
5. C# コンセントとアクションを使用してコードを UI 要素を公開します。
6. 変更を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

たとえば、次のように入力します。

1. Xcode で、**ライブラリ** セクションから**ボタン**をドラッグします。

    [![ライブラリから、ボタンを選択すると](xib-images/xcode07.png "ライブラリから、ボタンを選択します。")](xib-images/xcode07-large.png#lightbox)
2. ドロップ ボタン上に、**ウィンドウ**で、**インターフェイス エディター**:

    [![ウィンドウにボタンの追加](xib-images/xcode08.png "ウィンドウにボタンの追加")](xib-images/xcode08-large.png#lightbox)
3. **属性インスペクター**の **Title** プロパティをクリックし、ボタンのタイトルを「`Click Me`」に変更します。

    ![ボタンの属性を設定](xib-images/xcode09.png "ボタン属性の設定")
4. **ライブラリ セクション**から**ラベル**をドラッグします。

    [![ライブラリ内のラベルを選択すると](xib-images/xcode10.png "ライブラリにラベルを選択します。")](xib-images/xcode10-large.png#lightbox)
5. **Interface Editor** のボタンの横にある**ウィンドウ**にラベルをドラッグします。

    [![ウィンドウにラベルを追加する](xib-images/xcode11.png "ウィンドウにラベルを追加します。")](xib-images/xcode11-large.png#lightbox)
6. ラベルの右ハンドルをつかみ、ウィンドウの端に近づくまでドラッグします。

    [![ラベルのサイズ変更](xib-images/xcode12.png "ラベルのサイズを変更します。")](xib-images/xcode12-large.png#lightbox)
7. 選択したまま、ラベル、**インターフェイス エディター**に切り替え、**サイズ インスペクター**:

    ![サイズ インスペクターを選択すると](xib-images/xcode13.png "サイズ インスペクターを選択します。")
8. **サイズの自動変更ボックス** をクリックして、 **Dim 赤の角かっこ**右側にある、 **Dim の赤の水平矢印**センター。

    ![自動プロパティを編集](xib-images/xcode14.png "自動プロパティの編集")
9. これにより、ラベルの拡大し、縮小のように実行中のアプリケーション ウィンドウのサイズを拡大または縮小されます。 **赤の角かっこ**の左側および上部、**自動ボックス**ボックスの場所の指定した X および Y に残っているようにラベルを指定します。
10. ユーザー インターフェイスに変更を保存します。

サイズを変更するでコントロールを移動した、する必要がありますにお気付きことインターフェイス ビルダーによってする便利なスナップ ヒントに基づく[OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)です。 次のガイドラインを使用すると、Mac のユーザーの使い慣れたルック アンド フィールを持っている高品質なアプリケーションを作成できます。

参照する場合、**インターフェイス階層**セクションで、レイアウトと、ユーザー インターフェイスを構成する要素の階層の表示方法に注意してください。

![インターフェイス階層内の要素の選択](xib-images/xcode15.png "インターフェイス階層内の要素の選択")

ここからは、編集、または UI 要素の順序を変更して必要な場合にドラッグする項目を選択できます。 たとえば、UI 要素は、別の要素で説明するが、ウィンドウの最上位の項目を一覧の一番下にはドラッグでした。

Xamarin.Mac アプリケーションでの Windows での作業の詳細についてを参照してください、 [Windows](~/mac/user-interface/window.md)ドキュメント。


## <a name="exposing-ui-elements-to-c-code"></a>C# コードに公開する UI 要素

インターフェイスのビルダーで、ユーザー インターフェイスのルック アンド フィールのレイアウトが終了したら、それらの c# コードからアクセスできるように、UI の要素を公開する必要があります。 これを行うが、アクション、およびコンセントを使用します。


### <a name="setting-a-custom-main-window-controller"></a>カスタムのメイン ウィンドウのコント ローラーを設定

C# コードを UI 要素を公開するには、コンセントとアクションを作成できるようにするには、Xamarin.Mac アプリは、カスタム ウィンドウ コント ローラーを使用する必要があります。

次の手順で行います。

1. Xcode のインターフェイスのビルダーで、アプリのストーリー ボードを開きます。
2. 選択、`NSWindowController`デザイン画面にします。
3. 切り替えて、 **Identity インスペクター**を表示および入力`WindowController`として、**クラス名**:

    [![クラス名を編集](xib-images/windowcontroller01.png "クラス名の編集")](xib-images/windowcontroller01-large.png#lightbox)
4. 変更内容を保存し、同期する Mac 用の Visual Studio に戻ります。
5. A **WindowController.cs**ファイルは、プロジェクトに追加する、**ソリューション パッド**Mac 用の Visual Studio で。

    ![Mac 用の Visual Studio で、新しいクラス名](xib-images/windowcontroller02.png "Mac 用の Visual Studio で、新しいクラス名")
6. Xcode のインターフェイスのビルダーでストーリー ボードを再度開きます。
7. **WindowController.h**ファイルが使用できるようになります。

    [![Xcode で対応する .h ファイル](xib-images/windowcontroller03.png "Xcode で対応する .h ファイル")](xib-images/windowcontroller03-large.png#lightbox)


### <a name="outlets-and-actions"></a>コンセントとアクション

これとはコンセントとアクション 従来の .NET ユーザー インターフェイス プログラミングでは、ユーザー インターフェイスのコントロールは追加時にプロパティとして自動的に公開されます。 Mac では事情が異なり、ビューにコントロールを追加しただけでは、コントロールはコードにアクセスできません。 開発者は、UI 要素を明示的にコードに公開する必要があります。 順序では、Apple により、2 つのオプション。

-  **Outlet**  – Outlet はプロパティに似ています。 プロパティを使用して、コードに公開されているコントロールをコンセントに接続する場合などのメソッドを呼び出すため、イベント ハンドラーをアタッチするなどの処理を行うことができます。
-  **アクション** – アクションは WPF のコマンド パターンに似ています。 たとえば、コントロールの操作が実行されると、ボタンがクリックすると、コントロールで、コード内のメソッドを呼び出すことが自動的には。 アクションは、同じアクションを多くのコントロールを接続するため、強力で便利です。

Xcode を使用してコードで直接コンセントとアクションを追加します。*コントロール ドラッグ*です。 具体的には、つまり、コンセントまたはアクションを作成するを押しながらコンセントまたはアクションを追加するコントロール要素を選択すること、**コントロール**キーボードのボタンをクリックし、コードに直接そのコントロールをドラッグします。

Xamarin.Mac 開発者コンセントまたはアクションを作成する (C#) ファイルに対応する Objective-C スタブ ファイルにドラッグすることを意味します。 Visual Studio for Mac という名前のファイルを作成した**MainWindow.h** shim Xcode プロジェクトの一部として、インターフェイスのビルダーを使用して生成します。

[![Xcode で .h ファイルの例を](xib-images/xcode16.png "Xcode で .h ファイルの例")](xib-images/xcode16-large.png#lightbox)

このスタブ .h ファイルがミラー化、 **MainWindow.designer.cs**新しい Xamarin.Mac プロジェクトに自動的に追加する`NSWindow`を作成します。 このファイルは、インターフェイスのビルダーによって行われた変更を同期するために使用されますは、私たちが作成されコンセントとアクション UI 要素の c# コードに公開されます。


#### <a name="adding-an-outlet"></a>コンセントを追加します。

コンセントとアクションとは何の基本的な知識では、c# コードの UI 要素を公開するコンセントを作成するを見てみましょう。

次の手順で行います。

1. Xcode の画面の右上の端にある**二連の輪**のボタンをクリックして、**アシスタント エディター**を開きます。

    [![アシスタント エディターを選択すると](xib-images/outlet01.png "アシスタント エディター を選択")](xib-images/outlet01-large.png#lightbox)
2. Xcode が分割ビュー モードに切り替わり、一方の側に**インターフェイス エディター**、もう一方の側に**コード エディター**が表示されます。
3. Xcode が自動的に取得される、 **MainWindowController.m**ファイルで、**コード エディター**が正しくありません。 コンセントとアクションは上記の説明から留意する必要がありますが、 **MainWindow.h**選択します。
4. 上部にある、**コード エディター**  をクリックして、**自動リンク**を選択し、 **MainWindow.h**ファイル。

    [![正しい .h ファイルを選択する](xib-images/outlet02.png "正しい .h ファイルを選択します。")](xib-images/outlet02-large.png#lightbox)
5. Xcode で正しいファイルが選択されます。

    [![選択された正しいファイル](xib-images/outlet03.png "正しいファイルの選択")](xib-images/outlet03-large.png#lightbox)
6. **最後のステップは非常に重要です!**  選択された正しいファイルをお持ちでない場合は、コンセントを作成することはできず、c# で間違ったクラスに公開される操作、またはこれら!
7. **インターフェイス エディター**を押しながら、**コントロール**キーボードのキーとクリック ドラッグ コード エディターに上で作成したラベルすぐ下、`@interface MainWindow : NSWindow { }`コード。

    [![新しいコンセントを作成するドラッグ](xib-images/outlet04.png "ドラッグ新しいコンセントを作成するには")](xib-images/outlet04-large.png#lightbox)
8. ダイアログ ボックスが表示されます。 ままにして、**接続**をコンセントに設定し、入力`ClickedLabel`の**名前**:

    [![コンセント プロパティを設定](xib-images/outlet05.png "コンセント プロパティの設定")](xib-images/outlet05-large.png#lightbox)
9. をクリックして、**接続**コンセントを作成するボタンをクリックします。

    ![完了したコンセント](xib-images/outlet06.png "完了コンセント")
10. 変更内容をファイルに保存します。


#### <a name="adding-an-action"></a>アクションを追加します。

次に、c# コードを UI 要素を持つユーザーの操作を公開するアクションの作成を見てみましょう。

次の手順で行います。

1. まだ確認してください、**アシスタント エディター**と**MainWindow.h**にファイルが表示、**コード エディター**です。
2. **インターフェイス エディター**を押しながら、**コントロール**キーボードのキーとクリック ドラッグ コード エディターに上で作成したボタンすぐ下、`@property (assign) IBOutlet NSTextField *ClickedLabel;`コード。

    [![ドラッグ操作を作成する](xib-images/action01.png "ドラッグ操作を作成するには")](xib-images/action01-large.png#lightbox)
3. 変更、**接続**アクションの種類。

    [![アクションの種類を選択して](xib-images/action02.png "アクションの種類の選択")](xib-images/action02-large.png#lightbox)
4. **[名前]** に「`ClickedButton`」を入力します。

    [![アクションの構成](xib-images/action03.png "アクションの構成")](xib-images/action03-large.png#lightbox)
5. をクリックして、**接続**アクションを作成するボタンをクリックします。

    ![完了したアクション](xib-images/action04.png "完了済みの操作")
6. 変更内容をファイルに保存します。

ワイヤード (有線) し、c# コードに公開されるは、ユーザー インターフェイス Mac を Visual Studio に切り替えるし、Xcode とインターフェイスのビルダーから変更を同期できるようにします。


### <a name="writing-the-code"></a>コードの記述

作成されたユーザー インターフェイスとコンセントとアクションを使用してコードに公開されている UI 要素の場合は、プログラムを現実に近づけるにコードを記述する準備ができたらです。 たとえば、開く、 **MainWindow.cs**ファイル内をダブルクリックして編集するため、**ソリューション パッド**:

[![MainWindow.cs ファイル](xib-images/code01.png "MainWindow.cs ファイル")](xib-images/code01-large.png#lightbox)

次のコードを追加し、`MainWindow`上記で作成したサンプル コンセントを使用するクラス。

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

なお、`NSLabel`は c# では名前でアクセス、直接ことを割り当てられている、Xcode でのプラグをコンセントを Xcode で作成したときと呼ばれるこの場合、`ClickedLabel`です。 C# の場合、通常クラスと同じ方法を任意のメソッドまたは公開されたオブジェクトのプロパティにアクセスすることができます。

> [!IMPORTANT]
> 使用する必要があります`AwakeFromNib`などの別の方法ではなく`Initialize`ので、`AwakeFromNib`が呼び出された_後_OS が読み込まれ、.xib ファイルからのユーザー インターフェイスをインスタンス化します。 表示される、しようとした場合に .xib ファイルの前に、ラベル コントロールのアクセスが完全に読み込まれるおよびインスタンス化、`NullReferenceException`エラー ラベル コントロールはまだ作成されないためです。

次に、次の部分クラスを追加、`MainWindow`クラス。

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

このコードは、Xcode とインターフェイスのビルダーを作成し、ユーザーがボタンをクリックしたいつでも呼び出されるアクションをアタッチします。

一部の UI 要素に自動的が組み込まれて操作、たとえば、既定のメニュー バー内の項目など、**オープンしています.**メニュー項目 (`openDocument:`)。 **ソリューション パッド**をダブルクリックして、 **<code>appdelegate.cs</code>**ファイルを開いて編集し、下の次のコードを追加するファイル、`DidFinishLaunching`メソッド。

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

重要な行`[Export ("openDocument:")]`、か`NSMenu`を**AppDelegate**メソッドを持つ`void OpenDialog (NSObject sender)`に応答する、`openDocument:`アクション。

メニューと操作の詳細についてを参照してください、[メニュー](~/mac/user-interface/menu.md)ドキュメント。


## <a name="synchronizing-changes-with-xcode"></a>Xcode での変更を同期します。

切り替えたときに Visual Studio に戻り for Mac Xcode から、Xcode に対して行った変更は、Xamarin.Mac プロジェクトで自動的に同期されます。

選択した場合、 **MainWindow.designer.cs**で、**ソリューション パッド**方法、マイクロソフトのコンセントとアクションがされたワイヤード (有線) の c# コードで表示することができます。

[![Xcode での変更を同期する](xib-images/sync01.png "Xcode での変更を同期します。")](xib-images/sync01-large.png#lightbox)

通知方法の 2 つの定義、 **MainWindow.designer.cs**ファイル。

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

内の定義に合わせて、 **MainWindow.h** Xcode でファイル。

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

わかるように、Visual Studio for Mac 待機 .h ファイルを変更して、それぞれにそれらの変更を自動的に同期**. designer.cs**ファイルをアプリケーションに公開することです。 場合もあります**MainWindow.designer.cs**部分クラスでは、Visual Studio for Mac は、変更する必要があるないように**MainWindow.cs**クラスに加え変更を上書きします。

通常必要はありませんを開くには、 **MainWindow.designer.cs** 、自分でこれがここに示す教育する目的でのみです。

> [!IMPORTANT]
> ほとんどの場合、Visual Studio for Mac は自動的に Xcode で行った変更を表示し、Xamarin.Mac プロジェクトに同期させるだけです。 同期が自動的に行われないときは Xcode に戻り、再び Visual Studio for Mac に戻ります。 こうすると通常、同期サイクルが開始します。


## <a name="adding-a-new-window-to-a-project"></a>新しいウィンドウをプロジェクトに追加します。

メイン ドキュメント ウィンドウとは別に Xamarin.Mac アプリケーションは、設定や検査パネルなどのユーザーにその他の種類のウィンドウを表示する必要があります。 プロジェクトに新しいウィンドウを追加するときに使用するようにして、**コント ローラーと Cocoa ウィンドウ**オプション、こうと簡単にファイルを .xib からウィンドウを読み込むプロセス。

新しいウィンドウを追加するには、次の操作を行います。

1. **ソリューション パッド**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.**.
2. 新しいファイル] ダイアログ ボックスで、[ **Xamarin.Mac** > **コント ローラーと Cocoa ウィンドウ**:

    ![新しいウィンドウのコント ローラーを追加する](xib-images/new01.png "新しいウィンドウのコント ローラーを追加します。")
3. **[名前]** に「`PreferencesWindow`」と入力し、**[新規]** ボタンをクリックします。
4. ダブルクリックして、 **PreferencesWindow.xib**編集のためインターフェイス ビルダーで開くファイル。

    [![Xcode でウィンドウを編集](xib-images/new02.png "Xcode でウィンドウの編集")](xib-images/new02-large.png#lightbox)
5. インターフェイスを設計するには。

    [![ウィンドウ レイアウトのデザイン](xib-images/new03.png "windows レイアウトのデザイン")](xib-images/new03-large.png#lightbox)
6. 変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

次のコードを追加**AppDelegate.cs**新しいウィンドウを表示します。

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

`var preferences = new PreferencesWindowController ();`行 .xib ファイルから、ウィンドウを読み込み、それを拡張するウィンドウ コント ローラーの新しいインスタンスを作成します。 `preferences.Window.MakeKeyAndOrderFront (this);`行をユーザーに新しいウィンドウが表示されます。

コードを実行しを選択する場合、**設定しています.**から、**アプリケーション メニュー**ウィンドウが表示されます。

![サンプル アプリを実行している](xib-images/new04.png "サンプル アプリの実行")

Xamarin.Mac アプリケーションでの Windows での作業の詳細についてを参照してください、 [Windows](~/mac/user-interface/window.md)ドキュメント。


## <a name="adding-a-new-view-to-a-project"></a>新しいビューをプロジェクトに追加します。

容易に分割ウィンドウのデザイン、いくつかのより管理しやすい .xib ファイルもあります。 ツールバー項目を選択するときに、メイン ウィンドウの内容を切り替えなどのように、**環境設定ウィンドウ**への応答のコンテンツを交換するか、**ソース リスト**選択します。

プロジェクトに新しいビューを追加するときに使用するようにして、**コント ローラーとビューを Cocoa**オプション、こうと簡単にファイルを .xib からビューを読み込むプロセス。

新しいビューを追加するには、次の操作を行います。

1. **ソリューション パッド**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.**.
2. 新しいファイル] ダイアログ ボックスで、[ **Xamarin.Mac** > **コント ローラーとビューを Cocoa**:

    ![新しいビューを追加する](xib-images/view01.png "新しいビューを追加します。")
3. **[名前]** に「`SubviewTable`」と入力し、**[新規]** ボタンをクリックします。
4. ダブルクリックして、 **SubviewTable.xib**ファイルを開いてインターフェイス ビルダーで編集し、ユーザー インターフェイスをデザインします。

    [![Xcode で新しいビューをデザインする](xib-images/view02.png "Xcode で新しいビューの設計")](xib-images/view02-large.png#lightbox)
5. 必要な操作とコンセントとを接続します。
6. 変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

次の編集、 **SubviewTable.cs**次のコードを追加し、 **AwakeFromNib**ファイルが読み込まれる場合に、新しいビューを設定します。

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

列挙型を表示するビューが表示されている追跡するために、プロジェクトに追加します。 たとえば、 **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

ビューを使用して表示されるウィンドウの .xib ファイルを編集します。 追加、**カスタム ビュー**として機能するコンテナーのビューの c# コードとというコンセントに公開してメモリに読み込まれる`ViewContainer`:

[![必要なコンセントを作成する](xib-images/view03.png "必要コンセントを作成します。")](xib-images/view03-large.png#lightbox)

変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

次に、新しいビューを表示するウィンドウの .cs ファイルを編集 (たとえば、 **MainWindow.cs**) し、次のコードを追加します。

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

ウィンドウのコンテナーで .xib ファイルから読み込まれたときに新しいビューを表示する必要があります (、**カスタム ビュー**の上に追加)、既存の任意のビューを削除して、新しいスワップ アウトしてこのコード ハンドル。 これを削除する場合、画面から、ビューを表示するが既にあることを表示するが検索されます。 渡されたビューを次回に (ビュー コント ローラーから読み込まれた) コンテンツ領域に合わせてサイズ変更し、コンテンツを表示するために追加します。

新しいビューを表示するには、次のコードを使用します。

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

これを表示するには、新しいビューのビュー コント ローラーの新しいインスタンスを作成、その型で指定されているプロジェクトに追加された列挙型) を設定およびを使用して、`DisplaySubview`メソッドを実際には、ビューを表示するウィンドウのクラスに追加します。 例えば:

[![サンプル アプリを実行している](xib-images/view04.png "サンプル アプリの実行")](xib-images/view04-large.png#lightbox)

Xamarin.Mac アプリケーションでの Windows での作業の詳細についてを参照してください、 [Windows](~/mac/user-interface/window.md)と[ダイアログ](~/mac/user-interface/dialog.md)ドキュメント。


## <a name="summary"></a>まとめ

この記事では、.xib ファイル Xamarin.Mac アプリケーションでの操作についての詳細を取得しました。 Xcode のでファイルを作成して保持 .xib 方法インターフェイス ビルダーと c# コードで .xib ファイルで作業する方法をアプリケーションのユーザー インターフェイスを作成するには、さまざまな種類と .xib ファイルの使用方法を説明しました。


## <a name="related-links"></a>関連リンク

- [MacImages (サンプル)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [メニュー](~/mac/user-interface/menu.md)
- [ダイアログ](~/mac/user-interface/dialog.md)
- [画像の操作](~/mac/app-fundamentals/image.md)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
