---
title: Xamarin.Mac で .xib ファイル
description: この記事では、.xib ファイルを作成して、Xamarin.Mac アプリケーションのユーザー インターフェイスを管理する Xcode の Interface Builder で作成した作業について説明します。
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 45eeee745b133646aef0f775bc879fa6a5d867c7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61062910"
---
# <a name="xib-files-in-xamarinmac"></a>Xamarin.Mac で .xib ファイル

_この記事では、.xib ファイルを作成して、Xamarin.Mac アプリケーションのユーザー インターフェイスを管理する Xcode の Interface Builder で作成した作業について説明します。_

> [!NOTE]
> Xamarin.Mac アプリのユーザー インターフェイスを作成することをお勧めは、ストーリー ボードの使用です。 このドキュメントには、歴史的な理由から、古い Xamarin.Mac プロジェクトを操作するため、インプレースが残っています。 詳細についてを参照してください、[ストーリー ボードの概要](~/mac/platform/storyboards/index.md)ドキュメント。

## <a name="overview"></a>概要

同じユーザー インターフェイス要素にアクセスし、ツールで作業する開発者 Xamarin.Mac アプリケーションでは、C# と .NET で作業するとき*Objective-C*と*Xcode*はします。 Xamarin.Mac は直接 Xcode と統合、ためには、Xcode を使用して_Interface Builder_を作成し、ユーザー インターフェイスを維持 (またはに応じて c# コードで直接作成する)。

.Xib ファイルは、Xcode の Interface Builder で作成および管理される (メニューの Windows、ビュー、ラベル、テキスト フィールド) など、アプリケーションのユーザー インターフェイスの要素をグラフィカルに定義する macOS によって使用されます。

[![実行中のアプリの例](xib-images/intro01.png "実行中のアプリの例")](xib-images/intro01-large.png#lightbox)

この記事では、Xamarin.Mac アプリケーションで .xib ファイルの操作の基礎を取り上げます。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)主な概念と、この記事で使用する方法については、最初に、記事。

確認することも、 [C# を公開するクラス/Objective-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性ネットワーク上での C# クラスを Objective-C オブジェクトと UI への要素に使用されます。


## <a name="introduction-to-xcode-and-interface-builder"></a>Xcode と Interface Builder の概要

Xcode の一部として、Apple は、デザイナーで、ユーザー インターフェイスを視覚的に作成することができます、Interface Builder というツールを作成しました。 Xamarin.MacはInterface Builderとうまく統合されており、Objective-Cユーザーと同じツールでUIを作成することができます。


### <a name="components-of-xcode"></a>Xcode のコンポーネント

開いた for Mac で Visual Studio から Xcode .xib ファイルを開くと、**プロジェクト ナビゲーター** 、左側の**インターフェイス階層**と**インターフェイス エディター**中央に、および**プロパティとユーティリティ**右側のセクション。

[![Xcode UI のコンポーネント](xib-images/xcode03.png "Xcode UI のコンポーネント")](xib-images/xcode03-large.png#lightbox)

見てとはそれらを使用方法、Xamarin.Mac アプリケーションのインターフェイスの作成にどのようなそれぞれの Xcode のセクションします。


#### <a name="project-navigation"></a>プロジェクトのナビゲーション

Xcode で編集するための .xib ファイルを開くときに Visual Studio for Mac と Xcode の間で変更が通知をバック グラウンドでの Xcode プロジェクトのファイルを作成します。 後でとに切り替えた Visual Studio for Mac Xcode からを、このプロジェクトに加えられた変更によって同期されます、Xamarin.Mac プロジェクトと Visual Studio for mac。

**プロジェクト ナビゲーション**セクションでは、すべてこれを構成するファイルの間を移動できます。 _shim_ Xcode プロジェクト。 通常、のみ対象となるように、この一覧に .xib ファイルで**MainMenu.xib**と**MainWindow.xib**します。


#### <a name="interface-hierarchy"></a>インターフェイス階層

**インターフェイス階層**セクションなどのユーザー インターフェイスのいくつかのキー プロパティを簡単にアクセスできます。 その**プレース ホルダー**および main**ウィンドウ**します。 このセクションでは、階層内でドラッグし、入れ子にする方法、ユーザー インターフェイスとの調整を構成する個々 の要素 (ビュー) にアクセスするのに使用することもできます。


#### <a name="interface-editor"></a>インターフェイス エディター

**インターフェイス エディター**セクションを画面を提供するグラフィカルにレイアウト、ユーザー インターフェイスします。 要素をドラッグします、**ライブラリ**のセクション、**プロパティとユーティリティ**設計を作成します。 デザイン画面にユーザー インターフェイス要素 (ビュー) を追加するときに追加されます、**インターフェイス階層**セクションに表示される順序で、**インターフェイス エディター**します。


#### <a name="properties--utilities"></a>プロパティとユーティリティ

**プロパティとユーティリティ**セクションで使用される 2 つの主要なセクションに分かれています**プロパティ**(インスペクターとも呼ばれます) および**ライブラリ**:

![プロパティ インスペクター](xib-images/xcode04.png "プロパティ インスペクター")

最初にこのセクションではほとんど空です、ただし内の要素を選択した場合、**インターフェイス エディター**または**インターフェイス階層**、**プロパティ**セクションが表示されます指定した要素と調整できるプロパティに関する情報。

**プロパティ** セクションには、次の図に示すように 8 つの異なる*インスペクター タブ*があります。

[![すべてのインスペクターの概要](xib-images/xcode05.png "すべてのインスペクターの概要")](xib-images/xcode05-large.png#lightbox)

左から順に次のタブがあります。

-   **ファイル インスペクター** – ファイル インスペクターは編集中の Xib ファイルの場所やファイル名などのファイル情報を示します。
-   **クイック ヘルプ** – クイック ヘルプでは、Xcode での選択内容に基づいたヘルプが表示されます。
-   **ID インスペクター** – ID インスペクターでは、選択したコントロールまたはビューに関する情報が表示されます。
-   **属性インスペクター** – 属性インスペクターでは、選択したコントロールまたはビューのさまざまな属性をカスタマイズできます。
-   **サイズ インスペクター** – サイズ インスペクターを使用して、サイズとサイズ変更は選択したコントロールまたはビューの動作を制御できます。
-   **接続インスペクター** – 接続インスペクターは、選択したコントロールのアウトレットとアクションの接続を示します。 Outlet と Action は、1 回しか現れないことについて説明します。
-   **バインド インスペクター** – バインド インスペクターを使用するデータ モデルにその値を自動的にバインドするためにコントロールを構成できます。
-   **表示効果インスペクター** – 表示効果インスペクターでは、アニメーションなど、コントロールの効果を指定できます。

**ライブラリ** セクションで、コントロールを見つけることができ、ユーザー インターフェイスをビルド、デザイナーをグラフィカルに配置するオブジェクト。

![ライブラリ インスペクターの例](xib-images/xcode06.png "ライブラリ インスペクターの例")

Xcode IDE と Interface Builder について理解したら、次のユーザー インターフェイスを作成するために使用を見てみましょう。


## <a name="creating-and-maintaining-windows-in-xcode"></a>作成して、Xcode での windows の保守

Xamarin.Mac アプリのユーザー インターフェイスを作成するための推奨される方法はストーリー ボード (を参照してください、[ストーリー ボードの概要](~/mac/platform/storyboards/index.md)詳細についてはドキュメント) および、新しいプロジェクトを Xamarin.Mac はで開始するため、既定では、ストーリー ボードを使用します。

切り替える、.xib を使用してベースの UI で、次の操作を行います。

1. Mac を Visual Studio を開き、新しい Xamarin.Mac プロジェクトを開始します。
2. **Solution Pad**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.**
3. 選択**Mac** > **Windows コント ローラー**:

    ![新しいウィンドウ コント ローラーの追加](xib-images/setup00.png "新しいウィンドウ コント ローラーの追加")
4. 入力`MainWindow` をクリックして、**新規**ボタン。

    ![新しいメイン ウィンドウの追加](xib-images/setup01.png "新しいメイン ウィンドウの追加")
5. もう一度、プロジェクトを右クリックし、選択**追加** > **新しいファイル.**
6. 選択**Mac** > **メイン メニュー**:

    ![新しいメイン メニューの追加](xib-images/setup02.png "新しいメイン メニューの追加")
7. として名前をそのまま`MainMenu` をクリックし、**新規**ボタンをクリックします。
8. **Solution Pad**選択、 **Main.storyboard**ファイルを右クリックし、**削除**:

    ![メインのストーリー ボードを選択する](xib-images/setup03.png "メイン ストーリー ボードを選択")
9. [削除] ダイアログ ボックスで、**削除**ボタンをクリックします。

    ![削除を確認する](xib-images/setup04.png "削除を確認します。")
10. **Solution Pad**、ダブルクリックして、 **Info.plist**ファイルを開き、編集します。
11. 選択`MainMenu`から、**メイン インターフェイス**ドロップダウン。

    [![メイン メニューの設定](xib-images/setup05.png "メイン メニューの設定")](xib-images/setup05-large.png#lightbox)
12. **Solution Pad**、ダブルクリックして、 **MainMenu.xib**ファイルを開き、Xcode の Interface Builder で編集します。
13. **ライブラリ インスペクター**、型`object`検索フィールドで、ドラッグ、新しい**オブジェクト**デザイン サーフェイスに。

    [![メイン メニューの編集](xib-images/setup06.png "メイン メニューの編集")](xib-images/setup06-large.png#lightbox)
14. **Identity Inspector**、入力`AppDelegate`の**クラス**:

    [![アプリ デリゲートを選択する](xib-images/setup07.png "アプリ デリゲートの選択")](xib-images/setup07-large.png#lightbox)
15. 選択**ファイルの所有者**から、**インターフェイス階層**に切り替え、**接続インスペクター**をデリゲートから線をドラッグし、 `AppDelegate` **オブジェクト**プロジェクトに追加します。

    [![接続アプリ デリゲート](xib-images/setup08.png "アプリ デリゲートの接続")](xib-images/setup08-large.png#lightbox)
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

アプリのメイン ウィンドウが定義されているので、 **.xib**ウィンドウ コント ローラーを追加するときに、プロジェクトに自動的に追加するファイル。 Windows のデザインを編集する、 **Solution Pad**、ダブルクリック、 **MainWindow.xib**ファイル。

![MainWindow.xib ファイルを選択する](xib-images/edit01.png "MainWindow.xib ファイルを選択します。")

これが、Xcode の Interface Builder でウィンドウのデザインを開きます。

[![編集、MainWindow.xib](xib-images/edit02.png "MainWindow.xib の編集")](xib-images/edit02-large.png#lightbox)


### <a name="standard-window-workflow"></a>標準的なウィンドウのワークフロー

作成して、Xamarin.Mac アプリケーションで使用したウィンドウで、プロセスは、基本的に同じ。

1. プロジェクトに自動的に追加の既定ではない新しい windows の場合は、プロジェクトに新しいウィンドウの定義を追加します。
2. Xcode の Interface Builder で編集するためのウィンドウのデザインを開く .xib ファイルをダブルクリックします。
3. ウィンドウの必要なプロパティを設定、**属性インスペクター**と**サイズ インスペクター**します。
4. インターフェイスを構築および構成に必要なコントロールでドラッグして、**属性インスペクター**します。
5. 使用して、**サイズ インスペクター** UI 要素のサイズ変更を処理します。
6. ウィンドウの UI 要素を公開C#outlet と action を使用してコード。
7. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。


### <a name="designing-a-window-layout"></a>ウィンドウ レイアウトのデザイン

インターフェイス ビルダーでのユーザー インターフェイスをレイアウトするためのプロセスは、基本的に追加するすべての要素に対して同じです。

1. 必要なコントロールを見つけて、**ライブラリ インスペクター**までドラッグし、**インターフェイス エディター**に置きます。
2. ウィンドウの必要なプロパティを設定、**属性インスペクター**します。
3. 使用して、**サイズ インスペクター** UI 要素のサイズ変更を処理します。
4. カスタム クラスを使用している場合は設定、 **Identity Inspector**します。
5. UI 要素を公開C#outlet と action を使用してコード。
6. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

たとえば、次のように入力します。

1. Xcode で、**ライブラリ** セクションから**ボタン**をドラッグします。

    [![ライブラリからボタンを選択する](xib-images/xcode07.png "ライブラリからボタンを選択します。")](xib-images/xcode07-large.png#lightbox)
2. 上にボタンのドロップ、**ウィンドウ**で、**インターフェイス エディター**:

    [![ウィンドウにボタンの追加](xib-images/xcode08.png "ウィンドウにボタンの追加")](xib-images/xcode08-large.png#lightbox)
3. **属性インスペクター**の **Title** プロパティをクリックし、ボタンのタイトルを「`Click Me`」に変更します。

    ![ボタンの属性を設定](xib-images/xcode09.png "ボタンの属性を設定")
4. **ライブラリ セクション**から**ラベル**をドラッグします。

    [![ライブラリ内のラベルを選択すると](xib-images/xcode10.png "ライブラリにラベルを選択します。")](xib-images/xcode10-large.png#lightbox)
5. **Interface Editor** のボタンの横にある**ウィンドウ**にラベルをドラッグします。

    [![ウィンドウにラベルを追加する](xib-images/xcode11.png "ウィンドウにラベルを追加します。")](xib-images/xcode11-large.png#lightbox)
6. ラベルの右ハンドルをつかみ、ウィンドウの端に近づくまでドラッグします。

    [![ラベルのサイズ変更](xib-images/xcode12.png "ラベルのサイズ変更")](xib-images/xcode12-large.png#lightbox)
7. 選択した状態でラベルを持つ、**インターフェイス エディター**に切り替え、**サイズ インスペクター**:

    ![サイズ インスペクターを選択する](xib-images/xcode13.png "サイズ インスペクターを選択します。")
8. **自動サイズ調整ボックス** をクリックして、 **Dim Red 角かっこ**右側にある、 **Dim の赤の水平矢印**センター。

    ![自動プロパティを編集](xib-images/xcode14.png "自動プロパティの編集")
9. これにより、ラベルは、拡大、縮小実行中のアプリケーション ウィンドウのサイズに拡張されます。 **赤の角かっこ**上部と左側の**自動サイズ調整ボックス**ボックスは、場所の指定した X および Y に残っているようにラベルを指示します。
10. ユーザー インターフェイスに変更を保存します。

サイズを変更するでコントロールが移動された、する必要がありますにお気付き、Interface Builder でに基づくに立つヒント[OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)します。 次のガイドラインを高品質なアプリケーションを Mac ユーザー向けに使い慣れたルック アンド フィールを作成できます。

検索する場合、**インターフェイス階層**セクションで、レイアウトと、ユーザー インターフェイスを構成する要素の階層を表示する方法に注意してください。

![インターフェイス階層内の要素の選択](xib-images/xcode15.png "インターフェイス階層内の要素の選択")

ここからは、編集、または UI 要素の順序を変更して必要な場合にドラッグする項目を選択できます。 たとえば、UI 要素は、別の要素で覆われているが、ウィンドウの最上位の項目を一覧の一番下にはドラッグでした。

Windows、Xamarin.Mac アプリケーションでの操作方法の詳細についてを参照してください、 [Windows](~/mac/user-interface/window.md)ドキュメント。


## <a name="exposing-ui-elements-to-c-code"></a>UI 要素を公開するC#コード

インターフェイス ビルダーで、ユーザー インターフェイスのルック アンド フィールのレイアウトが完了したら、UI の要素を公開してからアクセスできるようにする必要がありますC#コード。 これを行うをアクションとアウトレットを使用します。


### <a name="setting-a-custom-main-window-controller"></a>カスタムのメイン ウィンドウのコント ローラーを設定

C# コードに UI 要素を公開する Outlet と Action を作成できるようにするには、Xamarin.Mac アプリは、カスタム ウィンドウ コント ローラーを使用する必要があります。

次の手順で行います。

1. Xcode の Interface Builder では、アプリのストーリー ボードを開きます。
2. 選択、`NSWindowController`デザイン画面でします。
3. 切り替えて、 **Identity Inspector**を表示および入力`WindowController`として、**クラス名**:

    [![クラス名を編集](xib-images/windowcontroller01.png "クラス名の編集")](xib-images/windowcontroller01-large.png#lightbox)
4. 変更を保存し、Visual Studio for Mac を同期に戻ります。
5. A **WindowController.cs**ファイルは、プロジェクトに追加する、 **Solution Pad** Visual Studio for Mac で。

    ![新しいクラスの名前が Visual studio for Mac](xib-images/windowcontroller02.png "for Mac の Visual Studio で、新しいクラス名")
6. Xcode の Interface Builder のストーリー ボードを再度開きます。
7. **WindowController.h**ファイルが使用できるようにします。

    [![Xcode で対応する .h ファイル](xib-images/windowcontroller03.png "Xcode で対応する .h ファイル")](xib-images/windowcontroller03-large.png#lightbox)


### <a name="outlets-and-actions"></a>Outlet と action

したがって outlet と action はでしょうか。 従来の .NET ユーザー インターフェイス プログラミングでは、ユーザー インターフェイスのコントロールは追加時にプロパティとして自動的に公開されます。 Mac では事情が異なり、ビューにコントロールを追加しただけでは、コントロールはコードにアクセスできません。 開発者は、UI 要素を明示的にコードに公開する必要があります。 順序では、Apple により 2 つのオプション。

-  **Outlet**  – Outlet はプロパティに似ています。 プロパティを介してコードに公開される場合は、電源コンセントにコントロールを接続するなどのメソッドを呼び出すため、イベント ハンドラーをアタッチするなどの処理を行うことができます。
-  **アクション** – アクションは WPF のコマンド パターンに似ています。 たとえば、コントロールのアクションが実行されると、ボタンがクリックすると、コントロールで、コード内のメソッドを呼び出すことが自動的には。 アクションは、同じアクションに多くのコントロールを接続することができますので、強力で便利です。

Xcode を使用してコード内で直接 outlet と action を追加します。*コントロールのドラッグ*します。 具体的には、コンセントまたはアクションを作成するには、コンセントまたはアクションの追加、押しするコントロール要素を選択することは、**コントロール**キーボードのボタンをクリックし、コードに直接コントロールをドラッグします。

Xamarin.Mac 開発者コンセントまたはアクションを作成する (C#) ファイルに対応する Objective-C スタブ ファイルにドラッグすることを意味します。 Visual Studio for Mac という名前のファイルを作成する**MainWindow.h**インターフェイス ビルダーを使用して生成した shim Xcode プロジェクトの一部として。

[![Xcode で .h ファイルの例](xib-images/xcode16.png "Xcode で .h ファイルの例")](xib-images/xcode16-large.png#lightbox)

このスタブ .h ファイルがミラー化、 **MainWindow.designer.cs**新しい Xamarin.Mac プロジェクトに自動的に追加される`NSWindow`が作成されます。 このファイルの Interface Builder で行った変更を同期に使用して、私たちが作成され、outlet と action UI 要素に公開されますがC#コード。


#### <a name="adding-an-outlet"></a>アウトレットの追加

Outlet と action とは何の基本的な知識を見てみましょうに UI 要素を公開するアウトレットを作成、C#コード。

次の手順で行います。

1. Xcode の画面の右上の端にある**二連の輪**のボタンをクリックして、**アシスタント エディター**を開きます。

    [![アシスタント エディターを選択する](xib-images/outlet01.png "アシスタント エディターを選択します。")](xib-images/outlet01-large.png#lightbox)
2. Xcode が分割ビュー モードに切り替わり、一方の側に**インターフェイス エディター**、もう一方の側に**コード エディター**が表示されます。
3. Xcode が自動的に選択されていること、 **MainWindowController.m**ファイル、**コード エディター**が正しくありません。 Outlet と action は上記の説明からわかる場合が必要です、 **MainWindow.h**選択します。
4. 上部にある、**コード エディター**  をクリックして、**自動リンク**を選択し、 **MainWindow.h**ファイル。

    [![正しい .h ファイルを選択する](xib-images/outlet02.png "正しい .h ファイルを選択します。")](xib-images/outlet02-large.png#lightbox)
5. Xcode で正しいファイルが選択されます。

    [![正しいファイルを選択](xib-images/outlet03.png "正しいファイルを選択")](xib-images/outlet03-large.png#lightbox)
6. **最後のステップは非常に重要です!**  選択された正しいファイルを持っていない場合のアウトレットを作成することはできずで間違ったクラスに公開されるアクションもC#!
7. **インターフェイス エディター**を押しながら、**コントロール**キーボードのキーとクリックをコード エディターに上記で作成したラベルをドラッグすぐ下、`@interface MainWindow : NSWindow { }`コード。

    [![新しいアウトレットを作成するドラッグ](xib-images/outlet04.png "ドラッグ新しいアウトレットを作成するには")](xib-images/outlet04-large.png#lightbox)
8. ダイアログ ボックスが表示されます。 ままに、**接続**電源コンセントに設定し、入力`ClickedLabel`の**名前**:

    [![アウトレットのプロパティを設定](xib-images/outlet05.png "アウトレット プロパティの設定")](xib-images/outlet05-large.png#lightbox)
9. をクリックして、 **Connect**アウトレットを作成するボタンをクリックします。

    ![完了したアウトレット](xib-images/outlet06.png "完成した電源タップ")
10. 変更内容をファイルに保存します。


#### <a name="adding-an-action"></a>アクションの追加

次に、作成する UI 要素を持つユーザーの操作を公開するアクションを見て、C#コード。

次の手順で行います。

1. 中では必ず、**アシスタント エディター**と**MainWindow.h**にファイルが表示、**コード エディター**します。
2. **インターフェイス エディター**を押しながら、**コントロール**キーボードのキーとクリックをコード エディターに上記で作成したボタンをドラッグすぐ下、`@property (assign) IBOutlet NSTextField *ClickedLabel;`コード。

    [![ドラッグ操作を作成する](xib-images/action01.png "ドラッグ操作を作成するには")](xib-images/action01-large.png#lightbox)
3. 変更、**接続**アクションの種類。

    [![アクションの種類を選択します。](xib-images/action02.png "アクションの種類を選択します。")](xib-images/action02-large.png#lightbox)
4. **[名前]** に「`ClickedButton`」を入力します。

    [![アクションの構成](xib-images/action03.png "アクションの構成")](xib-images/action03-large.png#lightbox)
5. をクリックして、 **Connect**ボタン アクションを作成します。

    ![完了したアクション](xib-images/action04.png "完了済みの操作")
6. 変更内容をファイルに保存します。

ユーザー インターフェイスとワイヤード (有線) アップおよびに公開されるC#コード、Visual Studio に切り替えて Mac と Xcode と Interface Builder の変更を同期させます。


### <a name="writing-the-code"></a>コードの記述

ユーザー インターフェイスを作成、outlet と action を使用してコードに公開される UI 要素をプログラムを実現するコードを記述する準備が整ったらします。 たとえば、開く、 **MainWindow.cs**ファイルをダブルクリックして編集、 **Solution Pad**:

[![MainWindow.cs ファイル](xib-images/code01.png "MainWindow.cs ファイル")](xib-images/code01-large.png#lightbox)

次のコードを追加し、`MainWindow`クラスを作成したサンプル アウトレット操作します。

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

なお、`NSLabel`でアクセスC#のプラグをコンセントを Xcode で作成したときに Xcode で割り当てを直接の名前でこの例では、呼び出された`ClickedLabel`します。 アクセスできる任意のメソッドまたはプロパティが公開されたオブジェクトの通常の場合と同じ方法C#クラス。

> [!IMPORTANT]
> 使用する必要がある`AwakeFromNib`などの別のメソッドではなく`Initialize`ため、`AwakeFromNib`と呼びます_後_OS が読み込まれ、.xib ファイルからユーザー インターフェイスをインスタンス化します。 表示しようとした場合、.xib ファイルの前に、ラベル コントロールにアクセスするされ完全に読み込まれて、インスタンス化、`NullReferenceException`エラー ラベル コントロールはまだ作成されていないためです。

次に、次の部分クラスを追加、`MainWindow`クラス。

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

このコードは、Xcode と Interface Builder で作成して、ユーザーがボタンをクリックして、いつ呼び出されるアクションをアタッチします。

一部の UI 要素に自動的が組み込まれていますアクション、たとえば、既定のメニュー バー内の項目など、**オープンしています.** メニュー項目 (`openDocument:`)。 **Solution Pad**、ダブルクリックして、 **AppDelegate.cs**ファイルを開き、編集し、下の次のコードを追加、`DidFinishLaunching`メソッド。

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

ここで重要な行`[Export ("openDocument:")]`に通知`NSMenu`を**AppDelegate**メソッドを持つ`void OpenDialog (NSObject sender)`に応答する、`openDocument:`アクション。

メニューの操作方法の詳細についてを参照してください、[メニュー](~/mac/user-interface/menu.md)ドキュメント。


## <a name="synchronizing-changes-with-xcode"></a>Xcode と変更の同期

切り替えたときに Visual Studio for Mac Xcode、Xcode で行った変更に自動的に Xamarin.Mac プロジェクトと同期されています。

選択した場合、 **MainWindow.designer.cs**で、 **Solution Pad**方法、コンセントとアクションがされたワイヤード (有線) を表示することができます、C#コード。

[![Xcode と変更の同期](xib-images/sync01.png "Xcode と変更の同期")](xib-images/sync01-large.png#lightbox)

通知方法の 2 つの定義、 **MainWindow.designer.cs**ファイル。

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

内の定義と、 **MainWindow.h** Xcode でファイル。

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

ご覧のように、Visual Studio for Mac の変更、.h ファイルをリッスンし、それぞれでこれらの変更を自動的に同期 **. designer.cs**ファイル、アプリケーションに公開します。 場合もあります**MainWindow.designer.cs**部分クラスは、Visual Studio for Mac は、変更する必要があるないように**MainWindow.cs**クラスに加え変更を上書きします。

通常必要はありませんを開く、 **MainWindow.designer.cs** 、自分でこれがここで紹介教育目的のみ。

> [!IMPORTANT]
> ほとんどの場合、Visual Studio for Mac が自動的に Xcode で加えられた変更を参照してくださいし、し、Xamarin.Mac プロジェクトに同期されます。 同期が自動的に行われないときは Xcode に戻り、再び Visual Studio for Mac に戻ります。 こうすると通常、同期サイクルが開始します。


## <a name="adding-a-new-window-to-a-project"></a>新しいウィンドウをプロジェクトに追加します。

メイン ドキュメント ウィンドウとは別に、Xamarin.Mac アプリケーションは、基本設定や検査パネルなど、ユーザーに他の種類のウィンドウを表示する必要があります。 常に使用する必要がある、新しいウィンドウをプロジェクトに追加するときに、 **Cocoa ウィンドウ コント ローラーと**オプション、これにより、.xib から、ウィンドウの読み込みのプロセスを簡単にファイルします。

新しいウィンドウを追加するには、次の操作を行います。

1. **Solution Pad**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.**.
2. 新しいファイル] ダイアログ ボックスで、[ **Xamarin.Mac** > **Cocoa ウィンドウ コント ローラーと**:

    ![新しいウィンドウ コント ローラーを追加する](xib-images/new01.png "を新しいウィンドウ コント ローラーを追加します。")
3. **[名前]** に「`PreferencesWindow`」と入力し、**[新規]** ボタンをクリックします。
4. ダブルクリックして、 **PreferencesWindow.xib**ファイルを開き、Interface Builder での編集します。

    [![Xcode で、ウィンドウの編集](xib-images/new02.png "Xcode で、ウィンドウの編集")](xib-images/new02-large.png#lightbox)
5. インターフェイスを設計するには。

    [![ウィンドウ レイアウトのデザイン](xib-images/new03.png "windows レイアウトのデザイン")](xib-images/new03-large.png#lightbox)
6. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

次のコードを追加**AppDelegate.cs**新しいウィンドウを表示します。

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

`var preferences = new PreferencesWindowController ();`行 .xib ファイルから、ウィンドウが読み込まれ、それを拡張したウィンドウ コント ローラーの新しいインスタンスを作成します。 `preferences.Window.MakeKeyAndOrderFront (this);`行が、ユーザーに新しいウィンドウを表示します。

コードを実行し、選択した場合、**設定しています.** から、**アプリケーション メニュー**ウィンドウが表示されます。

![サンプル アプリを実行している](xib-images/new04.png "サンプル アプリを実行")

Windows、Xamarin.Mac アプリケーションでの操作方法の詳細についてを参照してください、 [Windows](~/mac/user-interface/window.md)ドキュメント。


## <a name="adding-a-new-view-to-a-project"></a>プロジェクトに新しいビューの追加

ウィンドウのデザインをいくつかより管理しやすい .xib ファイルに分割する簡単な場合もあります。 などのツール バー アイテムを選択するときに、メイン ウィンドウの内容を切り替えのように、**環境設定ウィンドウ**への応答のコンテンツを交換または、**ソース リスト**選択します。

常に使用する必要がありますをプロジェクトに新しいビューを追加するときに、 **Cocoa ビュー コント ローラーと**オプション、これにより、.xib からビューを読み込むプロセスを簡単にファイルします。

新しいビューを追加するには、次の操作を行います。

1. **Solution Pad**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.**.
2. 新しいファイル] ダイアログ ボックスで、[ **Xamarin.Mac** > **Cocoa ビュー コント ローラーと**:

    ![新しいビューを追加する](xib-images/view01.png "新しいビューを追加します。")
3. **[名前]** に「`SubviewTable`」と入力し、**[新規]** ボタンをクリックします。
4. ダブルクリックして、 **SubviewTable.xib**ファイルを開き、インターフェイス ビルダーで編集し、ユーザー インターフェイスをデザインします。

    [![Xcode で新しいビューをデザインする](xib-images/view02.png "Xcode で新しいビューの設計")](xib-images/view02-large.png#lightbox)
5. 任意の必要なアクションとアウトレットを結び付けます。
6. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

次の編集、 **SubviewTable.cs**次のコードを追加し、 **AwakeFromNib**ファイルが読み込まれるときに、新しいビューを設定します。

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

列挙型を追跡するために、ビューが表示されているプロジェクトに追加します。 たとえば、 **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

ビューを使用および表示されるウィンドウの .xib ファイルを編集します。 追加、**カスタム ビュー**として機能するコンテナーのビューによって、メモリに読み込まれるとC#コードをコンセントと呼ばれるに公開`ViewContainer`:

[![必要なアウトレットを作成する](xib-images/view03.png "必要アウトレットを作成します。")](xib-images/view03-large.png#lightbox)

変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

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

ウィンドウのコンテナー内の .xib ファイルから読み込まれたときに新しいビューを表示する必要があります (、**カスタム ビュー**上で追加)、既存の任意のビューを削除して、新しいスワップ アウトしてこのコード ハンドル。 画面から削除されるようにする場合、ビューを表示するが既にあることを確認します。 渡されたビューを次にかかるで (ビュー コント ローラーから読み込まれる) とコンテンツ領域に合わせてサイズを変更し、表示は、コンテンツを追加します。

新しいビューを表示するには、次のコードを使用します。

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

これは、表示するには、新しいビューのビュー コント ローラーの新しいインスタンスを作成、(としてプロジェクトに追加の列挙体によって指定される) には、その型を設定および使用、`DisplaySubview`メソッドが実際には、ビューを表示するウィンドウのクラスに追加します。 例えば:

[![サンプル アプリを実行している](xib-images/view04.png "サンプル アプリを実行")](xib-images/view04-large.png#lightbox)

Windows、Xamarin.Mac アプリケーションでの操作方法の詳細についてを参照してください、 [Windows](~/mac/user-interface/window.md)と[ダイアログ](~/mac/user-interface/dialog.md)ドキュメント。


## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションで .xib ファイルの使用方法について詳しく説明をしました。 アプリケーションのユーザー インターフェイスを作成するには、Xcode の ファイルを作成して .xib を管理する方法、インターフェイス ビルダーと .xib ファイルで使用する方法は、さまざまな種類と .xib ファイルの使用方法を説明しましたC#コード。


## <a name="related-links"></a>関連リンク

- [MacImages (サンプル)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [メニュー](~/mac/user-interface/menu.md)
- [ダイアログ](~/mac/user-interface/dialog.md)
- [画像の操作](~/mac/app-fundamentals/image.md)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
